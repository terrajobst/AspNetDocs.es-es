---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Usar Signalr con Web Apps en Azure App Service | Microsoft Docs
author: bradygaster
description: En este documento se describe cómo configurar una aplicación Signalr que se ejecuta en Microsoft Azure. Versiones de software usadas en el tutorial Visual Studio 2013 o vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450187"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a>Usar SignalR con Web Apps en Azure App Service

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se describe cómo configurar una aplicación Signalr que se ejecuta en Microsoft Azure.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) o Visual Studio 2012
> - .NET 4.5
> - Signalr versión 2
> - Azure SDK 2,3 para Visual Studio 2013 o 2012
>
>
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), en [stackoverflow.com](http://stackoverflow.com/)o en los [foros de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).

## <a name="table-of-contents"></a>Tabla de contenido

- [Introducción](#introduction)
- [Implementación de una aplicación Web de Signalr en Azure App Service](#deploying)
- [Habilitación de WebSockets en Azure App Service](#websocket)
- [Usar el backplane Azure Redis Cache](#backplane)
- [Pasos siguientes](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a>Introducción

ASP.NET Signalr se puede usar para incorporar un nuevo nivel de interactividad entre los servidores y los clientes Web o de .NET. Cuando se hospeda en Azure, las aplicaciones de Signalr pueden aprovechar el entorno de alta disponibilidad, escalable y de rendimiento que proporciona en la nube.

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a>Implementación de una aplicación Web de Signalr en Azure App Service

Signalr no agrega complicaciones particulares a la implementación de una aplicación en Azure frente a la implementación en un servidor local. Una aplicación que usa Signalr se puede hospedar en Azure sin realizar cambios en la configuración ni en otras opciones (aunque para la compatibilidad con WebSockets, consulte [habilitación de WebSockets en Azure App Service](#websocket) a continuación). En este tutorial, implementará la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) en Azure.

**Requisitos previos**

- Visual Studio 2013. Si no tiene Visual Studio, Visual Studio 2013 Express para web se incluye en la instalación del SDK de Azure.
- [Azure sdk 2,3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) o [Azure SDK 2,3 para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).
- Para completar este tutorial, deberá tener una suscripción de Azure. Puede [activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)o [registrarse para obtener una suscripción de prueba](https://azure.microsoft.com/pricing/free-trial/).

### <a name="deploying-a-signalr-web-app-to-azure"></a>Implementación de una aplicación Web de Signalr en Azure

1. Complete el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md)o descargue el proyecto terminado de la [Galería de código](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
2. En Visual Studio, seleccione **compilar**, **publicar chat de signalr**.
3. En el cuadro de diálogo "publicar web", seleccione "sitios web de Windows Azure".

    ![Seleccionar sitios web de Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. Si no ha iniciado sesión en el cuenta de Microsoft, haga clic en **iniciar sesión...** en el cuadro de diálogo "seleccionar sitio web existente" e inicie sesión.

    ![Seleccionar sitio web existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Iniciar sesión en Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. En el cuadro de diálogo "seleccionar sitio web existente", haga clic en **nuevo**.

    ![Nuevo sitio web](using-signalr-with-azure-web-sites/_static/image4.png)
6. En el cuadro de diálogo "crear sitio en Windows Azure", escriba un nombre de aplicación único. Seleccione la región más cercana a usted en la lista desplegable región. Haga clic en **Crear**.

    ![Creación de sitio en Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. En el cuadro de diálogo "publicar web", haga clic en **publicar**.

    ![Publicar sitio](using-signalr-with-azure-web-sites/_static/image6.png)
8. Cuando la aplicación haya completado la publicación, la aplicación Signalr chat hospedada en Azure App Service Web Apps se abrirá en un explorador.

    ![Abrir el sitio en un explorador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a>Habilitación de WebSockets en Azure App Service Web Apps

WebSockets debe habilitarse explícitamente en la aplicación web para usarse en una aplicación de Signalr. de lo contrario, se usarán otros protocolos (consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener más detalles).

Para usar WebSockets en Azure App Service Web Apps, habilítelo en la sección configuración de la aplicación Web. Para ello, abra la aplicación web en [Azure portal de administración](https://manage.windowsazure.com/)y seleccione Configurar.

![Pestaña Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

En la parte superior de la página Configuración, asegúrese de que se usa .NET 4,5 para la aplicación Web.

![Configuración de la versión 4,5 de .NET Framework](using-signalr-with-azure-web-sites/_static/image9.png)

En la página Configuración, en la configuración de **WebSockets** , seleccione **activado**.

![Configuración de WebSockets: activado](using-signalr-with-azure-web-sites/_static/image10.png)

En la parte inferior de la página Configuración, seleccione **Guardar** para guardar los cambios.

![Guardar configuración](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a>Usar el backplane Azure Redis Cache

Si usa varias instancias para la aplicación web y los usuarios de esas instancias necesitan interactuar entre sí (por ejemplo, los mensajes de chat creados en una instancia pueden llegar a los usuarios conectados a otras instancias), el [Azure Redis cache backplane](../performance/scaleout-with-redis.md) debe implementarse en la aplicación.

<a id="nextsteps"></a>
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Web Apps en Azure App Service, consulte [información general sobre Web Apps](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).
