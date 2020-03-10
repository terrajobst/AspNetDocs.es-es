---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Signalr ampliación con Azure Service Bus | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr version 2 de este tema para la versión Signalr 1. x de este tema,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467737"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a>Escalabilidad horizontal de SignalR con Azure Service Bus

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

En este tutorial, implementará una aplicación Signalr en un rol Web de Windows Azure, mediante el Service Bus backplane para distribuir los mensajes a cada instancia de rol. (También puede usar el backplane Service Bus con [Web Apps en Azure App Service](https://docs.microsoft.com/azure/app-service-web/)).

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

Requisitos previos:

- Una cuenta de Windows Azure.
- El [SDK de Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).
- Visual Studio 2012 o 2013

El backplane de Service Bus también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1,1. Sin embargo, no es compatible con la versión 1,0 de Service Bus para Windows Server.

## <a name="pricing"></a>Precio

El backplane Service Bus usa temas para enviar mensajes. Para obtener la información más reciente sobre los precios, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/). En el momento de redactar este documento, puede enviar 1 millón mensajes al mes de menos de $1. El backplane envía un mensaje de Service Bus para cada invocación de un método de concentrador de Signalr. También hay algunos mensajes de control para las conexiones, las desconexiones, la Unión o el abandono de grupos, etc. En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.

## <a name="overview"></a>Información general

Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.

1. Use el Azure Portal de Windows para crear un nuevo espacio de nombres de Service Bus.
2. Agregue estos paquetes de NuGet a la aplicación: 

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) o [Microsoft. Aspnet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)
3. Cree una aplicación Signalr.
4. Agregue el código siguiente a Startup.cs para configurar el backplane: 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

Este código configura el Backplane con los valores predeterminados para [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Para obtener información sobre cómo cambiar estos valores, vea [rendimiento de signalr: métricas de ampliación](signalr-performance.md#scaleout_metrics).

Para cada aplicación, elija un valor diferente para "YourAppName". No use el mismo valor en varias aplicaciones.

## <a name="create-the-azure-services"></a>Creación de los servicios de Azure

Cree un servicio en la nube, como se describe en [creación e implementación de un servicio en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy). Siga los pasos de la sección "Cómo: crear un servicio en la nube mediante creación rápida". En este tutorial, no es necesario cargar un certificado.

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

Cree un nuevo espacio de nombres de Service Bus, como se describe en [uso de Service Bus temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions). Siga los pasos de la sección "crear un espacio de nombres de servicio".

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres Service Bus.

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Inicie Visual Studio. En el menú **Archivo**, haga clic en **Nuevo proyecto**.

En el cuadro de diálogo **nuevo proyecto** , expanda **Visual C#** . En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio en la nube de Windows Azure**. Mantenga el valor predeterminado .NET Framework 4,5. Asigne a la aplicación el nombre ChatService y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , seleccione rol Web ASP.net. Haga clic en el botón de flecha derecha ( **&gt;** ) para agregar el rol a la solución.

Mantenga el mouse sobre el nuevo rol, de modo que el icono de lápiz esté visible. Haga clic en este icono para cambiar el nombre del rol. Asigne el nombre "SignalRChat" al rol y haga clic en **Aceptar**.

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione **MVC**y haga clic en Aceptar.

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

El Asistente para proyectos crea dos proyectos:

- ChatService: este proyecto es la aplicación de Windows Azure. Define los roles de Azure y otras opciones de configuración.
- SignalRChat: este proyecto es el proyecto de ASP.NET MVC 5.

## <a name="create-the-signalr-chat-application"></a>Creación de la aplicación Signalr chat

Para crear la aplicación de chat, siga los pasos del tutorial [Introducción con signalr y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

Use NuGet para instalar las bibliotecas necesarias. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la **consola del administrador de paquetes** , escriba los siguientes comandos:

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

Use la opción `-ProjectName` para instalar los paquetes en el proyecto ASP.NET MVC, en lugar del proyecto de Windows Azure.

## <a name="configure-the-backplane"></a>Configurar el backplane

En el archivo Startup.cs de la aplicación, agregue el código siguiente:

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

Ahora debe obtener la cadena de conexión de Service Bus. En el Azure Portal, seleccione el espacio de nombres de Service Bus que ha creado y haga clic en el icono clave de acceso.

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

Copie la cadena de conexión en el portapapeles y, a continuación, péguela en la variable *ConnectionString* .

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a>Implementar en Azure

En Explorador de soluciones, expanda la carpeta **roles** dentro del proyecto ChatService.

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

Haga clic con el botón secundario en el rol SignalRChat y seleccione **propiedades**. Seleccione la pestaña **configuración** . En **instancias** , seleccione 2. También puede establecer el tamaño de la máquina virtual en **extra pequeño**.

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

Guarde los cambios.

En Explorador de soluciones, haga clic con el botón secundario en el proyecto ChatService. Seleccione **Publicar**.

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

Si esta es la primera vez que publica en Windows Azure, debe descargar las credenciales. En el Asistente para **publicación** , haga clic en "iniciar sesión para descargar credenciales". Esto le pedirá que inicie sesión en el Azure Portal de Windows y descargue un archivo de configuración de publicación.

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

Haga clic en **importar** y seleccione el archivo de configuración de publicación que ha descargado.

Haga clic en **Siguiente**. En el cuadro de diálogo **configuración de publicación** , en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

Haga clic en **Publicar**. La implementación de la aplicación puede tardar unos minutos e iniciar las máquinas virtuales.

Ahora, al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, mediante un tema Service Bus. Un tema es una cola de mensajes que permite varios suscriptores.

El backplane crea automáticamente el tema y las suscripciones. Para ver la actividad de las suscripciones y los mensajes, abra el Azure Portal, seleccione el espacio de nombres Service Bus y haga clic en "temas".

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

La actividad del mensaje tarda unos minutos en aparecer en el panel.

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

Signalr administra la duración del tema. Siempre que se implemente la aplicación, no intente eliminar manualmente los temas ni cambiar la configuración del tema.

## <a name="troubleshooting"></a>Solución de problemas

**System. InvalidOperationException "el único IsolationLevel admitido es ' IsolationLevel. serializable '".**

Este error puede producirse si el nivel de transacción de una operación se establece en un valor distinto de `Serializable`. Compruebe que no se realiza ninguna operación con otros niveles de transacción.
