---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Asignación de usuarios de Signalr a las conexiones | Microsoft Docs
author: bradygaster
description: En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones. Patrick Fletcher ayudó a escribir este tema. Versiones de software usadas en este tema...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431395"
---
# <a name="mapping-signalr-users-to-connections"></a>Asignar usuarios de SignalR a las conexiones

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones.
>
> Patrick Fletcher ayudó a escribir este tema.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

## <a name="introduction"></a>Introducción

Cada cliente que se conecta a un concentrador pasa un identificador de conexión único. Puede recuperar este valor en la propiedad `Context.ConnectionId` del contexto del concentrador. Si la aplicación necesita asignar un usuario al identificador de conexión y conservar esa asignación, puede utilizar una de las siguientes opciones:

- [El proveedor de ID. de usuario (Signalr 2)](#IUserIdProvider)
- [Almacenamiento en memoria](#inmemory), como un diccionario
- [Grupo de signalr para cada usuario](#groups)
- [Almacenamiento externo permanente](#database), como una tabla de base de datos o Azure Table Storage

En este tema se muestra cada una de estas implementaciones. Use los métodos `OnConnected`, `OnDisconnected`y `OnReconnected` de la clase `Hub` para realizar un seguimiento del estado de conexión del usuario.

El mejor enfoque para la aplicación depende de:

- El número de servidores web que hospedan la aplicación.
- Si necesita obtener una lista de los usuarios conectados actualmente.
- Si necesita conservar la información del grupo y del usuario cuando se reinicia la aplicación o el servidor.
- Si la latencia de llamar a un servidor externo es un problema.

En la tabla siguiente se muestra qué enfoque funciona en estas consideraciones.

|  | Más de un servidor | Obtiene una lista de usuarios conectados actualmente | Conservar la información después de los reinicios | Rendimiento óptimo |
| --- | --- | --- | --- | --- |
| Proveedor UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupos de usuario único | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externa | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Proveedor IUserID

Esta característica permite a los usuarios especificar qué identificador de usuario se basa en un objeto irequest a través de una nueva interfaz IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

De forma predeterminada, habrá una implementación que utiliza el `IPrincipal.Identity.Name` del usuario como nombre de usuario. Para cambiar esto, registre la implementación de `IUserIdProvider` con el host global cuando se inicie la aplicación:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Desde dentro de un concentrador, podrá enviar mensajes a estos usuarios a través de la siguiente API:

**Enviar un mensaje a un usuario específico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Almacenamiento en memoria

En los siguientes ejemplos se muestra cómo conservar la información de conexión y de usuario en un diccionario almacenado en memoria. El Diccionario usa un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación Signalr. Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.

Si la aplicación se cierra, se perderá toda la información, pero se volverá a rellenar cuando los usuarios vuelvan a establecer sus conexiones. El almacenamiento en memoria no funciona si su entorno incluye más de un servidor Web, ya que cada servidor tendría una colección independiente de conexiones.

En el primer ejemplo se muestra una clase que administra la asignación de usuarios a las conexiones. La clave de HashSet será el nombre del usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador. La instancia de la clase se almacena en un nombre de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de usuario único

Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando quiera llegar solo a ese usuario. El nombre de cada grupo es el nombre del usuario. Si un usuario tiene más de una conexión, cada identificador de conexión se agrega al grupo del usuario.

No debe quitar manualmente el usuario del grupo cuando el usuario se desconecta. Esta acción la realiza automáticamente Signalr Framework.

En el ejemplo siguiente se muestra cómo implementar grupos de usuario único.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Almacenamiento externo permanente

En este tema se muestra cómo usar una base de datos o Azure Table Storage para almacenar información de conexión. Este enfoque funciona cuando hay varios servidores Web, ya que cada servidor web puede interactuar con el mismo repositorio de datos. Si los servidores web dejan de funcionar o se reinicia la aplicación, no se llama al método `OnDisconnected`. Por lo tanto, es posible que el repositorio de datos tenga registros para los identificadores de conexión que ya no son válidos. Para limpiar estos registros huérfanos, es posible que desee invalidar cualquier conexión que se haya creado fuera de un período de tiempo relevante para su aplicación. En los ejemplos de esta sección se incluye un valor para realizar un seguimiento de Cuándo se creó la conexión, pero no se muestra cómo limpiar los registros antiguos porque tal vez desee hacerlo como proceso en segundo plano.

### <a name="database"></a>Base de datos

En los siguientes ejemplos se muestra cómo conservar la información de conexión y de usuario en una base de datos. Puede utilizar cualquier tecnología de acceso a datos; sin embargo, en el ejemplo siguiente se muestra cómo definir modelos mediante Entity Framework. Estos modelos de entidad corresponden a tablas y campos de base de datos. La estructura de los datos puede variar considerablemente en función de los requisitos de la aplicación.

En el primer ejemplo se muestra cómo definir una entidad de usuario que se puede asociar a muchas entidades de conexión.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Después, desde el concentrador, puede realizar un seguimiento del estado de cada conexión con el código que se muestra a continuación.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

El siguiente ejemplo de Azure Table Storage es similar al ejemplo de base de datos. No incluye toda la información necesaria para empezar a trabajar con el servicio Azure Table Storage. Para obtener más información, consulte [uso de Table Storage desde .net](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

En el ejemplo siguiente se muestra una entidad de tabla para almacenar información de conexión. Divide los datos por nombre de usuario e identifica cada entidad por el identificador de conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

En el concentrador, se realiza un seguimiento del estado de la conexión de cada usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
