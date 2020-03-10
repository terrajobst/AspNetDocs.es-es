---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitación del seguimiento de Signalr | Microsoft Docs
author: bradygaster
description: En este documento se describe cómo habilitar y configurar el seguimiento de los clientes y servidores de Signalr. La traza le permite ver información de diagnóstico sobre eventos...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467443"
---
# <a name="enabling-signalr-tracing"></a>Habilitar el seguimiento de SignalR

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se describe cómo habilitar y configurar el seguimiento de los clientes y servidores de Signalr. La traza le permite ver información de diagnóstico sobre los eventos de la aplicación Signalr.
>
> Este tema fue escrito originalmente por Patrick Fletcher.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - Signalr versión 2
>
>
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

Cuando el seguimiento está habilitado, una aplicación Signalr crea entradas de registro para los eventos. Puede registrar eventos tanto del cliente como del servidor. Seguimiento en los registros de servidor conexión, proveedor de ampliación y eventos del bus de mensajes. El seguimiento en el cliente registra los eventos de conexión. En Signalr 2,1 y versiones posteriores, el seguimiento en el cliente registra todo el contenido de los mensajes de invocación de concentrador.

## <a name="contents"></a>Contenido

- [Habilitar el seguimiento en el servidor](#server)

    - [Registrar eventos de servidor en archivos de texto](#server_text)
    - [Registrar eventos de servidor en el registro de eventos](#server_eventlog)
- [Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)](#net_client)

    - [Registro de eventos de cliente de escritorio en la consola](#desktop_console)
    - [Registro de eventos de cliente de escritorio en un archivo de texto](#desktop_text)
- [Habilitación del seguimiento en Windows Phone 8 clientes](#phone)

    - [Registro de eventos de Windows Phone Client en la interfaz de usuario](#phone_ui)
    - [Registro de eventos de Windows Phone Client en la consola de depuración](#phone_debug)
- [Habilitar el seguimiento en el cliente de JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Habilitar el seguimiento en el servidor

Habilite el seguimiento en el servidor en el archivo de configuración de la aplicación (App. config o Web. config, en función del tipo de proyecto). Especifique qué categorías de eventos desea registrar. En el archivo de configuración, también se especifica si se deben registrar los eventos en un archivo de texto, en el registro de eventos de Windows o en un registro personalizado mediante una implementación de [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

Las categorías de eventos de servidor incluyen los siguientes tipos de mensajes:

| Origen | Mensajes |
| --- | --- |
| SignalR.SqlMessageBus | Configuración del proveedor de ampliación de bus de mensajes de SQL, operación de base de datos, error y eventos de tiempo de espera |
| SignalR.ServiceBusMessageBus | Creación de temas de proveedor de ampliación de Service Bus y eventos de suscripción, error y mensajería |
| SignalR.RedisMessageBus | Conexión del proveedor de Redis ampliación, desconexión y eventos de error |
| SignalR.ScaleoutMessageBus | Eventos de mensajería de ampliación |
| SignalR.Transports.WebSocketTransport | Eventos de conexión, desconexión, mensajería y errores de transporte de WebSocket |
| SignalR.Transports.ServerSentEventsTransport | ServerSentEvents de conexión de transporte, desconexión, mensajería y eventos de error |
| SignalR.Transports.ForeverFrameTransport | ForeverFrame de conexión de transporte, desconexión, mensajería y eventos de error |
| SignalR.Transports.LongPollingTransport | LongPolling de conexión de transporte, desconexión, mensajería y eventos de error |
| SignalR.Transports.TransportHeartBeat | Eventos de conexión, desconexión y keepalive de transporte |
| Signalr. ReflectedHubDescriptorProvider | Eventos de detección de concentradores |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registrar eventos de servidor en archivos de texto

En el código siguiente se muestra cómo habilitar el seguimiento para cada categoría de evento. En este ejemplo se configura la aplicación para registrar eventos en archivos de texto.

**Código de servidor XML para habilitar el seguimiento**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

En el código anterior, la entrada `SignalRSwitch` especifica el [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) que se usa para los eventos enviados al registro especificado. En este caso, se establece en `Verbose`, lo que significa que se registran todos los mensajes de depuración y seguimiento.

La siguiente salida muestra entradas del archivo de `transports.log.txt` para una aplicación que usa el archivo de configuración anterior. Muestra una nueva conexión, una conexión eliminada y eventos de latido de transporte.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registrar eventos de servidor en el registro de eventos

Para registrar los eventos en el registro de eventos en lugar de un archivo de texto, cambie los valores de las entradas del nodo `sharedListeners`. En el código siguiente se muestra cómo registrar eventos de servidor en el registro de eventos:

**Código de servidor XML para registrar eventos en el registro de eventos**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Los eventos se registran en el registro de aplicaciones y están disponibles a través del Visor de eventos, como se muestra a continuación:

![Visor de eventos que muestra los registros de Signalr](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Cuando use el registro de eventos, establezca **TraceLevel** en **error** para mantener el número de mensajes que se deben administrar.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Habilitar el seguimiento en el cliente de .NET (aplicaciones de escritorio de Windows)

El cliente .NET puede registrar eventos en la consola, un archivo de texto o un registro personalizado mediante una implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Para habilitar el registro en el cliente .NET, establezca la propiedad `TraceLevel` de la conexión en un valor [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) y la propiedad `TraceWriter` en una instancia de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) válida.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registro de eventos de cliente de escritorio en la consola

En el C# código siguiente se muestra cómo registrar eventos en el cliente de .net en la consola de:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registro de eventos de cliente de escritorio en un archivo de texto

En el C# código siguiente se muestra cómo registrar eventos en el cliente de .net en un archivo de texto:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

La siguiente salida muestra entradas del archivo de `ClientLog.txt` para una aplicación que usa el archivo de configuración anterior. Muestra el cliente que se conecta al servidor y el concentrador que invoca un método de cliente llamado `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Habilitación del seguimiento en Windows Phone 8 clientes

Las aplicaciones de signalr para Windows Phone aplicaciones usan el mismo cliente .NET que las aplicaciones de escritorio, pero [Console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) y escribir en un archivo con [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) no están disponibles. En su lugar, debe crear una implementación personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para el seguimiento.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registro de eventos de Windows Phone Client en la interfaz de usuario

El [código base de signalr](https://github.com/SignalR/SignalR/archive/master.zip) incluye un ejemplo Windows Phone que escribe la salida del seguimiento en un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mediante una implementación personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) denominada `TextBlockWriter`. Esta clase se puede encontrar en el proyecto **Samples/Microsoft. Aspnet. signalr. Client. WP8. samples** . Al crear una instancia de `TextBlockWriter`, pase el [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)actual y un [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) donde creará un [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) que se usará para los resultados del seguimiento:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

El resultado del seguimiento se escribirá en un nuevo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) creado en el [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que pasó:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registro de eventos de Windows Phone Client en la consola de depuración

Para enviar la salida a la consola de depuración en lugar de a la interfaz de usuario, cree una implementación de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que escriba en la ventana de depuración y asígnela a la propiedad [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) de la conexión:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

La información de seguimiento se escribirá en la ventana de depuración en Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Habilitar el seguimiento en el cliente de JavaScript

Para habilitar el registro del lado cliente en una conexión, establezca la propiedad `logging` en el objeto de conexión antes de llamar al método `start` para establecer la conexión.

**Código JavaScript del cliente para habilitar el seguimiento en la consola del explorador (con el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Código JavaScript del cliente para habilitar el seguimiento en la consola del explorador (sin el proxy generado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Cuando el seguimiento está habilitado, el cliente JavaScript registra los eventos en la consola del explorador. Para obtener acceso a la consola del explorador, consulte [supervisión de transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).

En la captura de pantalla siguiente se muestra un cliente de Signalr JavaScript con seguimiento habilitado. Muestra eventos de invocación de conexión y de concentrador en la consola del explorador:

![Eventos de seguimiento de signalr en la consola del explorador](enabling-signalr-tracing/_static/image4.png)
