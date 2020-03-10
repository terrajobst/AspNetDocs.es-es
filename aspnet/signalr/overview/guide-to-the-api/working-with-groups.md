---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Trabajar con grupos en Signalr | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo conservar la información de pertenencia a grupos con la API de Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450085"
---
# <a name="working-with-groups-in-signalr"></a>Trabajar con grupos en SignalR

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo agregar usuarios a grupos y conservar la información de pertenencia a grupos.
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

## <a name="overview"></a>Información general

Los grupos de Signalr proporcionan un método para difundir mensajes a subconjuntos específicos de clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos. No tiene que crear grupos explícitamente. En efecto, se crea automáticamente un grupo la primera vez que se especifica su nombre en una llamada a groups. Add, y se elimina cuando se quita la última conexión de la pertenencia a él. Para obtener una introducción al uso de grupos, consulte [Administración de la pertenencia a grupos desde la clase hub](hubs-api-guide-server.md#groupsfromhub) en la guía de hubs API-Server.

No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos. Signalr envía mensajes a clientes y grupos basados en un modelo pub/sub, y el servidor no mantiene listas de grupos o pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, ya que cada vez que se agrega un nodo a una granja de servidores Web, cualquier Estado que mantenga Signalr debe propagarse al nuevo nodo.

Cuando se agrega un usuario a un grupo mediante el método `Groups.Add`, el usuario recibe los mensajes dirigidos a ese grupo mientras dure la conexión actual, pero la pertenencia del usuario a ese grupo no se conserva más allá de la conexión actual. Si desea conservar permanentemente información acerca de los grupos y la pertenencia a grupos, debe almacenar los datos en un repositorio, como una base de datos o Azure Table Storage. Después, cada vez que un usuario se conecta a la aplicación, recupera del repositorio al que pertenece el usuario y agrega manualmente el usuario a esos grupos.

Al volver a conectarse después de una interrupción temporal, el usuario vuelve a unirse automáticamente a los grupos asignados previamente. La Unión automática de un grupo solo se aplica cuando se vuelve a conectar, no al establecer una nueva conexión. Se pasa un token firmado digitalmente desde el cliente que contiene la lista de grupos asignados previamente. Si desea comprobar si el usuario pertenece a los grupos solicitados, puede invalidar el comportamiento predeterminado.

En este tema, se incluyen las siguientes secciones:

- [Adición y eliminación de usuarios](#add)
- [Llamar a miembros de un grupo](#call)
- [Almacenar la pertenencia a un grupo en una base de datos](#storedatabase)
- [Almacenamiento de la pertenencia a grupos en Azure Table Storage](#storeazuretable)
- [Comprobando la pertenencia al grupo al volver a conectar](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Adición y eliminación de usuarios

Para agregar o quitar usuarios de un grupo, debe llamar a los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) y pasar el identificador de conexión del usuario y el nombre del grupo como parámetros. No es necesario quitar manualmente un usuario de un grupo cuando finaliza la conexión.

En el ejemplo siguiente se muestran los métodos `Groups.Add` y `Groups.Remove` usados en métodos de concentrador.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Los métodos `Groups.Add` y `Groups.Remove` se ejecutan de forma asincrónica.

Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, debe asegurarse de que el método groups. Add finaliza primero. En los siguientes ejemplos de código se muestra cómo hacerlo.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

En general, no debe incluir `await` al llamar al método `Groups.Remove` porque es posible que el identificador de conexión que está intentando quitar ya no esté disponible. En ese caso, se produce `TaskCanceledException` una vez que se agota el tiempo de espera de la solicitud. Si la aplicación debe asegurarse de que el usuario se ha quitado del grupo antes de enviar un mensaje al grupo, puede Agregar `await` antes de `Groups.Remove`y, a continuación, capturar la excepción `TaskCanceledException` que se puede producir.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Llamar a miembros de un grupo

Puede enviar mensajes a todos los miembros de un grupo o solo a los miembros especificados del grupo, tal y como se muestra en los ejemplos siguientes.

- **Todos** los clientes conectados en un grupo especificado.

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Todos los clientes conectados en un grupo especificado, **excepto los clientes especificados**, identificados por el identificador de conexión.

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Todos los clientes conectados en un grupo especificado, **excepto el cliente que llama**.

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Almacenar la pertenencia a un grupo en una base de datos

En los siguientes ejemplos se muestra cómo conservar la información del grupo y del usuario en una base de datos. Puede utilizar cualquier tecnología de acceso a datos; sin embargo, en el ejemplo siguiente se muestra cómo definir modelos mediante Entity Framework. Estos modelos de entidad corresponden a tablas y campos de base de datos. La estructura de los datos puede variar considerablemente en función de los requisitos de la aplicación. En este ejemplo se incluye una clase denominada `ConversationRoom` que sería única para una aplicación que permite a los usuarios unir conversaciones sobre diferentes asuntos, como deportes o jardinería. En este ejemplo también se incluye una clase para las conexiones. La clase de conexión no es absolutamente necesaria para realizar el seguimiento de la pertenencia a grupos pero suele formar parte de una solución sólida para el seguimiento de los usuarios.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

A continuación, en el concentrador, puede recuperar la información de grupo y de usuario de la base de datos y agregar manualmente el usuario a los grupos adecuados. En el ejemplo no se incluye código para realizar el seguimiento de las conexiones de usuario. En este ejemplo, la palabra clave `await` no se aplica antes de `Groups.Add` porque no se envía inmediatamente un mensaje a los miembros del grupo. Si desea enviar un mensaje a todos los miembros del grupo inmediatamente después de agregar el nuevo miembro, querrá aplicar la palabra clave `await` para asegurarse de que se ha completado la operación asincrónica.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Almacenamiento de la pertenencia a grupos en Azure Table Storage

El uso de Azure Table Storage para almacenar información de grupos y usuarios es similar al uso de una base de datos. En el ejemplo siguiente se muestra una entidad de tabla que almacena el nombre de usuario y el nombre de grupo.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

En el concentrador, se recuperan los grupos asignados cuando el usuario se conecta.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Comprobando la pertenencia al grupo al volver a conectar

De forma predeterminada, Signalr vuelve a asignar automáticamente un usuario a los grupos adecuados cuando se vuelve a conectar desde una interrupción temporal, como cuando una conexión se quita y se vuelve a establecer antes de que se agote el tiempo de espera de la conexión. La información del grupo del usuario se pasa en un token cuando se vuelve a conectar y ese token se comprueba en el servidor. Para obtener información sobre el proceso de comprobación para volver a unirse a los usuarios a grupos, consulte [reunir grupos cuando se vuelve a conectar](../security/introduction-to-security.md#rejoingroup).

En general, debe usar el comportamiento predeterminado de volver a unirse automáticamente a los grupos al reconectarse. Los grupos de signalr no están diseñados como un mecanismo de seguridad para restringir el acceso a los datos confidenciales. Sin embargo, si la aplicación debe comprobar la pertenencia a grupos de un usuario al volver a conectarlo, puede invalidar el comportamiento predeterminado. Cambiar el comportamiento predeterminado puede Agregar una carga a la base de datos porque se debe recuperar la pertenencia a grupos de un usuario para cada reconexión, en lugar de solo cuando el usuario se conecta.

Si debe comprobar la pertenencia a grupos al volver a conectar, cree un nuevo módulo de canalización de concentrador que devuelva una lista de grupos asignados, como se muestra a continuación.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

A continuación, agregue ese módulo a la canalización del concentrador, tal y como se resalta a continuación.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
