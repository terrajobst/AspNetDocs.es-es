---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Signalr ampliación con SQL Server (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431131"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a>Escalabilidad horizontal de SignalR con SQL Server (SignalR 1.x)

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En este tutorial, usará SQL Server para distribuir los mensajes a través de una aplicación Signalr implementada en dos instancias de IIS independientes. También puede ejecutar este tutorial en un único equipo de pruebas, pero para obtener el efecto completo, debe implementar la aplicación Signalr en dos o más servidores. También debe instalar SQL Server en uno de los servidores o en un servidor dedicado independiente. Otra opción consiste en ejecutar el tutorial con máquinas virtuales en Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Requisitos previos

Microsoft SQL Server 2005 o posterior. El backplane es compatible con las ediciones de escritorio y servidor de SQL Server. No admite SQL Server Compact Edition o Azure SQL Database. (Si la aplicación está hospedada en Azure, tenga en cuenta el Service Bus backplane en su lugar).

## <a name="overview"></a>Información general

Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.

1. Cree una nueva base de datos vacía. El backplane creará las tablas necesarias en esta base de datos.
2. Agregue estos paquetes de NuGet a la aplicación: 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Cree una aplicación Signalr.
4. Agregue el código siguiente a global. asax para configurar el backplane: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a>Configurar la base de datos

Decida si la aplicación usará la autenticación de Windows o la autenticación de SQL Server para tener acceso a la base de datos. Sin importar, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.

Cree una nueva base de datos para que la use el backplane. Puede asignar cualquier nombre a la base de datos. No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Habilitar Service Broker

Se recomienda habilitar Service Broker para la base de datos de backplane. Service Broker proporciona compatibilidad nativa para mensajería y puesta en cola en SQL Server, lo que permite que el backplane reciba las actualizaciones de forma más eficaz. (Sin embargo, el backplane también funciona sin Service Broker).

Para comprobar si Service Broker está habilitado, consulte la columna **is\_broker\_Enabled** en la vista de catálogo **Sys. Databases** .

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Para habilitar Service Broker, use la siguiente consulta SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Si esta consulta aparece en interbloqueo, asegúrese de que no haya ninguna aplicación conectada a la base de BD.

Si ha habilitado el seguimiento, los seguimientos también mostrarán si Service Broker está habilitado.

## <a name="create-a-signalr-application"></a>Creación de una aplicación Signalr

Cree una aplicación de Signalr siguiendo uno de estos tutoriales:

- [Introducción con Signalr](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introducción con Signalr y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md)

A continuación, modificaremos la aplicación de chat para admitir ampliación con SQL Server. En primer lugar, agregue el paquete NuGet Signalr. SqlServer al proyecto. En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

A continuación, abra el archivo global. asax. Agregue el código siguiente al método de **Inicio de\_** de la aplicación:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Implementación y ejecución de la aplicación

Preparar las instancias de Windows Server para implementar la aplicación Signalr.

Agregue el rol de IIS. Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Incluya también el servicio de administración de (que aparece en "herramientas de administración").

![](scaleout-with-sql-server/_static/image5.png)

**Instale Web Deploy 3,0.** Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386). En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0

![](scaleout-with-sql-server/_static/image6.png)

Compruebe que el servicio de administración web se está ejecutando. Si no es así, inicie el servicio. (Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).

Por último, abra el puerto 8172 para TCP. Este es el puerto que utiliza la herramienta de Web Deploy.

Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor. En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.

Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro. (Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).

![](scaleout-with-sql-server/_static/image7.png)

Después de ejecutar la aplicación, puede ver que Signalr ha creado tablas automáticamente en la base de datos:

![](scaleout-with-sql-server/_static/image8.png)

Signalr administra las tablas. Siempre que se implemente la aplicación, no elimine filas, modifique la tabla, etc.
