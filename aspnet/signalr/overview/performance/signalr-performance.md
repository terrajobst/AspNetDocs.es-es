---
uid: signalr/overview/performance/signalr-performance
title: Rendimiento de signalr | Microsoft Docs
author: bradygaster
description: Rendimiento de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449863"
---
# <a name="signalr-performance"></a><span data-ttu-id="d41ed-103">Rendimiento de SignalR</span><span class="sxs-lookup"><span data-stu-id="d41ed-103">SignalR Performance</span></span>

<span data-ttu-id="d41ed-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d41ed-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="d41ed-105">En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="d41ed-106">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="d41ed-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="d41ed-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d41ed-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="d41ed-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d41ed-108">.NET 4.5</span></span>
> - <span data-ttu-id="d41ed-109">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="d41ed-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="d41ed-110">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="d41ed-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="d41ed-111">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="d41ed-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="d41ed-112">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="d41ed-112">Questions and comments</span></span>
>
> <span data-ttu-id="d41ed-113">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="d41ed-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="d41ed-114">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="d41ed-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="d41ed-115">Para obtener una presentación reciente sobre el rendimiento y el escalado de Signalr, consulte [escalado del Web en tiempo real con ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="d41ed-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="d41ed-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="d41ed-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="d41ed-117">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="d41ed-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="d41ed-118">Ajuste del rendimiento del servidor de Signalr</span><span class="sxs-lookup"><span data-stu-id="d41ed-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="d41ed-119">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="d41ed-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="d41ed-120">Usar contadores de rendimiento de Signalr</span><span class="sxs-lookup"><span data-stu-id="d41ed-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="d41ed-121">Usar otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="d41ed-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="d41ed-122">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="d41ed-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="d41ed-123">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="d41ed-123">Design considerations</span></span>

<span data-ttu-id="d41ed-124">En esta sección se describen los patrones que se pueden implementar durante el diseño de una aplicación Signalr, para asegurarse de que el rendimiento no se obstaculiza generando tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="d41ed-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="d41ed-125">Frecuencia del mensaje de limitación</span><span class="sxs-lookup"><span data-stu-id="d41ed-125">Throttling message frequency</span></span>

<span data-ttu-id="d41ed-126">Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no necesitan enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="d41ed-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="d41ed-127">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que pone en cola y envía mensajes con una frecuencia superior a una velocidad fija (es decir, se enviará un número determinado de mensajes cada segundo, si hay mensajes en ese momento en tervalo que se va a enviar).</span><span class="sxs-lookup"><span data-stu-id="d41ed-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="d41ed-128">Para obtener una aplicación de ejemplo que limita los mensajes a una determinada tasa (desde el cliente y el servidor), consulte [la alta frecuencia en tiempo real con signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="d41ed-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="d41ed-129">Reducir el tamaño de los mensajes</span><span class="sxs-lookup"><span data-stu-id="d41ed-129">Reducing message size</span></span>

<span data-ttu-id="d41ed-130">Puede reducir el tamaño de un mensaje de Signalr reduciendo el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="d41ed-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="d41ed-131">En el código del servidor, si va a enviar un objeto que contiene propiedades que no es necesario transmitir, evite que esas propiedades se serialicen mediante el `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="d41ed-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="d41ed-132">Los nombres de las propiedades también se almacenan en el mensaje; los nombres de las propiedades se pueden acortar mediante el atributo `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="d41ed-133">En el ejemplo de código siguiente se muestra cómo excluir una propiedad de la que se va a enviar al cliente y cómo acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="d41ed-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="d41ed-134">**Código de servidor .NET que muestra el atributo JsonIgnore para excluir los datos que se envían al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="d41ed-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="d41ed-135">Con el fin de conservar la legibilidad y el mantenimiento en el código de cliente, los nombres de propiedad abreviados se pueden reasignar a nombres descriptivos después de recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="d41ed-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="d41ed-136">En el ejemplo de código siguiente se muestra una posible manera de volver a asignar nombres abreviados a más largos, mediante la definición de un contrato de mensaje (asignación) y el uso de la función `reMap` para aplicar el contrato a la clase de mensaje optimizada:</span><span class="sxs-lookup"><span data-stu-id="d41ed-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="d41ed-137">**Código JavaScript del lado cliente que reasigna nombres de propiedad abreviados a nombres legibles para el usuario.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="d41ed-138">Los nombres también se pueden acortar en los mensajes del cliente al servidor, mediante el mismo método.</span><span class="sxs-lookup"><span data-stu-id="d41ed-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="d41ed-139">Reducir la superficie de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del objeto de mensaje también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d41ed-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="d41ed-140">Por ejemplo, si el intervalo completo de un `int` no es necesario, se puede usar en su lugar un `short` o `byte`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="d41ed-141">Puesto que los mensajes se almacenan en el bus de mensajes en la memoria del servidor, la reducción del tamaño de los mensajes también puede solucionar problemas de memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="d41ed-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="d41ed-142">Ajuste del rendimiento del servidor de Signalr</span><span class="sxs-lookup"><span data-stu-id="d41ed-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="d41ed-143">Se pueden usar las siguientes opciones de configuración para optimizar el servidor y obtener un mejor rendimiento en una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="d41ed-144">Para obtener información general sobre cómo mejorar el rendimiento en una aplicación de ASP.NET, vea [mejorar el rendimiento de ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="d41ed-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="d41ed-145">**Opciones de configuración de signalr**</span><span class="sxs-lookup"><span data-stu-id="d41ed-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="d41ed-146">**DefaultMessageBufferSize**: de forma predeterminada, signalr conserva 1000 mensajes en memoria por cada concentrador por conexión.</span><span class="sxs-lookup"><span data-stu-id="d41ed-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="d41ed-147">Si se usan mensajes grandes, se pueden crear problemas de memoria que se pueden mitigar reduciendo este valor.</span><span class="sxs-lookup"><span data-stu-id="d41ed-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="d41ed-148">Esta configuración se puede establecer en el controlador de eventos `Application_Start` en una aplicación ASP.NET o en el método `Configuration` de una clase de inicio OWIN en una aplicación autohospedada.</span><span class="sxs-lookup"><span data-stu-id="d41ed-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="d41ed-149">En el ejemplo siguiente se muestra cómo reducir este valor para reducir la superficie de memoria de la aplicación con el fin de reducir la cantidad de memoria del servidor usada:</span><span class="sxs-lookup"><span data-stu-id="d41ed-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="d41ed-150">**Código de servidor .NET en Startup.cs para reducir el tamaño del búfer de mensajes predeterminado**</span><span class="sxs-lookup"><span data-stu-id="d41ed-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="d41ed-151">**Opciones de configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="d41ed-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="d41ed-152">Número **máximo de solicitudes simultáneas por aplicación**: el aumento del número de solicitudes de IIS simultáneas aumentará los recursos del servidor disponibles para atender las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d41ed-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="d41ed-153">El valor predeterminado es 5000; para aumentar este valor, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="d41ed-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="d41ed-154">**ApplicationPool QueueLength**: este es el número máximo de solicitudes que http. sys pone en cola para el grupo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="d41ed-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="d41ed-155">Cuando la cola está llena, las nuevas solicitudes reciben una respuesta 503 "servicio no disponible".</span><span class="sxs-lookup"><span data-stu-id="d41ed-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="d41ed-156">El valor predeterminado es 1000.</span><span class="sxs-lookup"><span data-stu-id="d41ed-156">The default value is 1000.</span></span>

    <span data-ttu-id="d41ed-157">Acortar la longitud de la cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación conservará los recursos de memoria.</span><span class="sxs-lookup"><span data-stu-id="d41ed-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="d41ed-158">Para obtener más información, vea [Administración, ajuste y configuración de grupos de aplicaciones](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="d41ed-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="d41ed-159">**Opciones de configuración de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="d41ed-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="d41ed-160">En esta sección se incluyen las opciones de configuración que se pueden establecer en el archivo de `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="d41ed-161">Este archivo se encuentra en una de las dos ubicaciones, en función de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="d41ed-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="d41ed-162">Entre las opciones de ASP.NET que pueden mejorar el rendimiento de Signalr se incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="d41ed-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="d41ed-163">**Número máximo de solicitudes simultáneas por CPU**: el aumento de este valor puede aliviar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d41ed-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="d41ed-164">Para aumentar este valor, agregue la siguiente opción de configuración al archivo `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="d41ed-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="d41ed-165">**Límite**de la cola de solicitudes: cuando el número total de conexiones supera el valor de `maxConcurrentRequestsPerCPU`, ASP.net iniciará las solicitudes de limitación mediante una cola.</span><span class="sxs-lookup"><span data-stu-id="d41ed-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="d41ed-166">Para aumentar el tamaño de la cola, puede aumentar el valor de `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="d41ed-167">Para ello, agregue la siguiente opción de configuración al nodo `processModel` en `config/machine.config` (en lugar de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="d41ed-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="d41ed-168">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="d41ed-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="d41ed-169">En esta sección se describen las formas de encontrar cuellos de botella de rendimiento en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d41ed-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="d41ed-170">Comprobando que se está usando WebSocket</span><span class="sxs-lookup"><span data-stu-id="d41ed-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="d41ed-171">Aunque Signalr puede utilizar una variedad de transportes para la comunicación entre el cliente y el servidor, WebSocket ofrece una ventaja significativa en el rendimiento y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="d41ed-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="d41ed-172">Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="d41ed-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="d41ed-173">Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollo del explorador y examinar los registros para ver qué transporte se utiliza para la conexión.</span><span class="sxs-lookup"><span data-stu-id="d41ed-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="d41ed-174">Para obtener información sobre el uso de las herramientas de desarrollo del explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="d41ed-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="d41ed-175">Usar contadores de rendimiento de Signalr</span><span class="sxs-lookup"><span data-stu-id="d41ed-175">Using SignalR performance counters</span></span>

<span data-ttu-id="d41ed-176">En esta sección se describe cómo habilitar y usar los contadores de rendimiento de Signalr, que se encuentran en el paquete de `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="d41ed-177">Instalando signalr. exe</span><span class="sxs-lookup"><span data-stu-id="d41ed-177">Installing signalr.exe</span></span>

<span data-ttu-id="d41ed-178">Los contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada Signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="d41ed-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="d41ed-179">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="d41ed-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="d41ed-180">En Visual Studio, seleccione **herramientas** > **Administrador de paquetes Nuget** > **administrar paquetes Nuget para la solución** .</span><span class="sxs-lookup"><span data-stu-id="d41ed-180">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="d41ed-181">Busque **signalr. utils**y seleccione instalar.</span><span class="sxs-lookup"><span data-stu-id="d41ed-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="d41ed-182">Acepte el contrato de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="d41ed-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="d41ed-183">Signalr. exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="d41ed-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="d41ed-184">Instalación de contadores de rendimiento con Signalr. exe</span><span class="sxs-lookup"><span data-stu-id="d41ed-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="d41ed-185">Para instalar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="d41ed-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="d41ed-186">Para quitar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="d41ed-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="d41ed-187">Contadores de rendimiento de signalr</span><span class="sxs-lookup"><span data-stu-id="d41ed-187">SignalR Performance counters</span></span>

<span data-ttu-id="d41ed-188">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="d41ed-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="d41ed-189">Los contadores "total" miden el número de eventos desde el último reinicio del grupo de aplicaciones o del servidor.</span><span class="sxs-lookup"><span data-stu-id="d41ed-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="d41ed-190">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="d41ed-190">**Connection metrics**</span></span>

<span data-ttu-id="d41ed-191">Las métricas siguientes miden los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="d41ed-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="d41ed-192">Para obtener más información, vea [Descripción y control de eventos de duración](../guide-to-the-api/handling-connection-lifetime-events.md)de la conexión.</span><span class="sxs-lookup"><span data-stu-id="d41ed-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="d41ed-193">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="d41ed-193">**Connections Connected**</span></span>
- <span data-ttu-id="d41ed-194">**Conexiones reconectadas**</span><span class="sxs-lookup"><span data-stu-id="d41ed-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="d41ed-195">**Conexiones desconectadas**</span><span class="sxs-lookup"><span data-stu-id="d41ed-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="d41ed-196">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="d41ed-196">**Connections Current**</span></span>

<span data-ttu-id="d41ed-197">**Métricas de mensajes**</span><span class="sxs-lookup"><span data-stu-id="d41ed-197">**Message metrics**</span></span>

<span data-ttu-id="d41ed-198">Las métricas siguientes miden el tráfico de mensajes generado por Signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="d41ed-199">**Total de mensajes de conexión recibidos**</span><span class="sxs-lookup"><span data-stu-id="d41ed-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="d41ed-200">**Total de mensajes de conexión enviados**</span><span class="sxs-lookup"><span data-stu-id="d41ed-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="d41ed-201">**Mensajes de conexión recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="d41ed-202">**Mensajes de conexión enviados/seg.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="d41ed-203">**Métricas del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="d41ed-203">**Message bus metrics**</span></span>

<span data-ttu-id="d41ed-204">Las métricas siguientes miden el tráfico a través del bus de mensajes interno de Signalr, la cola en la que se colocan todos los mensajes entrantes y salientes de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="d41ed-205">Un mensaje se **publica** cuando se envía o se emite.</span><span class="sxs-lookup"><span data-stu-id="d41ed-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="d41ed-206">Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; debe ser igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="d41ed-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="d41ed-207">Un **trabajador asignado** es un componente que envía datos a las conexiones activas. un **trabajador ocupado** es aquel que está enviando un mensaje de forma activa.</span><span class="sxs-lookup"><span data-stu-id="d41ed-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="d41ed-208">**Total de mensajes de bus de mensajes recibidos**</span><span class="sxs-lookup"><span data-stu-id="d41ed-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="d41ed-209">**Mensajes de bus de mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="d41ed-210">**Total de mensajes de bus de mensajes publicados**</span><span class="sxs-lookup"><span data-stu-id="d41ed-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="d41ed-211">**Mensajes de bus de mensajes publicados/s**</span><span class="sxs-lookup"><span data-stu-id="d41ed-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="d41ed-212">**Suscriptores de bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="d41ed-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="d41ed-213">**Total de suscriptores de Message bus**</span><span class="sxs-lookup"><span data-stu-id="d41ed-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="d41ed-214">**Suscriptores de Message bus/s**</span><span class="sxs-lookup"><span data-stu-id="d41ed-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="d41ed-215">**Trabajos asignados del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="d41ed-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="d41ed-216">**Trabajadores ocupados del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="d41ed-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="d41ed-217">**Temas del bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="d41ed-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="d41ed-218">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="d41ed-218">**Error metrics**</span></span>

<span data-ttu-id="d41ed-219">Las métricas siguientes miden los errores generados por el tráfico de mensajes de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="d41ed-220">Los errores de **resolución de concentrador** se producen cuando no se puede resolver un método de concentrador o concentrador.</span><span class="sxs-lookup"><span data-stu-id="d41ed-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="d41ed-221">Los errores de **invocación de concentrador** son excepciones que se producen al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="d41ed-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="d41ed-222">Los errores de **transporte** son errores de conexión producidos durante una solicitud o respuesta http.</span><span class="sxs-lookup"><span data-stu-id="d41ed-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="d41ed-223">**Errores: todos los totales**</span><span class="sxs-lookup"><span data-stu-id="d41ed-223">**Errors: All Total**</span></span>
- <span data-ttu-id="d41ed-224">**Errores: todos/seg.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="d41ed-225">**Errores: total de resolución de concentrador**</span><span class="sxs-lookup"><span data-stu-id="d41ed-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="d41ed-226">**Errores: resolución de concentrador/s**</span><span class="sxs-lookup"><span data-stu-id="d41ed-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="d41ed-227">**Errores: total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="d41ed-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="d41ed-228">**Errores: invocación de concentrador/s**</span><span class="sxs-lookup"><span data-stu-id="d41ed-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="d41ed-229">**Errores: total de transporte**</span><span class="sxs-lookup"><span data-stu-id="d41ed-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="d41ed-230">**Errores: transporte/s**</span><span class="sxs-lookup"><span data-stu-id="d41ed-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="d41ed-231">**Métricas de ampliación**</span><span class="sxs-lookup"><span data-stu-id="d41ed-231">**Scaleout metrics**</span></span>

<span data-ttu-id="d41ed-232">Las métricas siguientes miden el tráfico y los errores generados por el proveedor de ampliación.</span><span class="sxs-lookup"><span data-stu-id="d41ed-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="d41ed-233">Un **flujo** en este contexto es una unidad de escalado que usa el proveedor ampliación. se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="d41ed-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="d41ed-234">Cada flujo garantiza operaciones de lectura y escritura ordenadas; un flujo único es un cuello de botella de escala posible, por lo que se puede aumentar el número de flujos para ayudar a reducir el cuello de botella.</span><span class="sxs-lookup"><span data-stu-id="d41ed-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="d41ed-235">Si se usan varias secuencias, Signalr distribuirá automáticamente los mensajes (de partición) a través de estos flujos de forma que se asegure de que los mensajes enviados desde cualquier conexión determinada estén en orden.</span><span class="sxs-lookup"><span data-stu-id="d41ed-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="d41ed-236">La configuración [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla la longitud de la cola de envío de ampliación mantenida por signalr.</span><span class="sxs-lookup"><span data-stu-id="d41ed-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="d41ed-237">Si se establece en un valor mayor que 0, todos los mensajes de una cola de envío se enviarán de uno en uno al backplane de mensajería configurado.</span><span class="sxs-lookup"><span data-stu-id="d41ed-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="d41ed-238">Si el tamaño de la cola supera la longitud configurada, las llamadas subsiguientes a SEND producirán un error inmediatamente con una [excepción InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes de la cola sea menor que el valor de de nuevo.</span><span class="sxs-lookup"><span data-stu-id="d41ed-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="d41ed-239">La puesta en cola está deshabilitada de forma predeterminada porque los Backplanes implementados generalmente tienen su propia cola o control de flujo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="d41ed-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="d41ed-240">En el caso de SQL Server, la agrupación de conexiones limita de forma eficaz el número de envíos que se están realizando en un momento dado.</span><span class="sxs-lookup"><span data-stu-id="d41ed-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="d41ed-241">De forma predeterminada, solo se usa una secuencia para SQL Server y Redis, se usan cinco flujos para Service Bus y la puesta en cola está deshabilitada, pero esta configuración se puede cambiar a través de la configuración en SQL Server y Service Bus:</span><span class="sxs-lookup"><span data-stu-id="d41ed-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="d41ed-242">**Código de servidor .NET para configurar el recuento de tablas y la longitud de cola para SQL Server backplane**</span><span class="sxs-lookup"><span data-stu-id="d41ed-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="d41ed-243">**Código de servidor .NET para configurar el número de temas y la longitud de la cola para Service Bus backplane**</span><span class="sxs-lookup"><span data-stu-id="d41ed-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="d41ed-244">Una secuencia de **almacenamiento en búfer** es aquella que ha entrado en un estado de error. Cuando la secuencia se encuentra en el estado FAULTED, se producirá un error en todos los mensajes enviados al plano posterior hasta que la secuencia deje de producir errores.</span><span class="sxs-lookup"><span data-stu-id="d41ed-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="d41ed-245">La **longitud** de la cola de envío es el número de mensajes que se han expuesto pero que todavía no se han enviado.</span><span class="sxs-lookup"><span data-stu-id="d41ed-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="d41ed-246">**Ampliación mensajes de bus de mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="d41ed-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="d41ed-247">**Total de secuencias ampliación**</span><span class="sxs-lookup"><span data-stu-id="d41ed-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="d41ed-248">**Secuencias de ampliación abiertas**</span><span class="sxs-lookup"><span data-stu-id="d41ed-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="d41ed-249">**Almacenamiento en búfer de secuencias ampliación**</span><span class="sxs-lookup"><span data-stu-id="d41ed-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="d41ed-250">**Total de errores de ampliación**</span><span class="sxs-lookup"><span data-stu-id="d41ed-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="d41ed-251">**Errores de ampliación por segundo**</span><span class="sxs-lookup"><span data-stu-id="d41ed-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="d41ed-252">**Longitud de cola de envío de ampliación**</span><span class="sxs-lookup"><span data-stu-id="d41ed-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="d41ed-253">Para obtener más información sobre lo que están midiendo estos contadores, consulte [signalr ampliación con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="d41ed-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="d41ed-254">Usar otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="d41ed-254">Using other performance counters</span></span>

<span data-ttu-id="d41ed-255">Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d41ed-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="d41ed-256">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="d41ed-256">**Memory**</span></span>

- <span data-ttu-id="d41ed-257">Memoria de .NET CLR\\# bytes en todos los montones (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="d41ed-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="d41ed-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="d41ed-258">**ASP.NET**</span></span>

- <span data-ttu-id="d41ed-259">ASP. Net\solicitudes actual</span><span class="sxs-lookup"><span data-stu-id="d41ed-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="d41ed-260">ASP. NET\Queued</span><span class="sxs-lookup"><span data-stu-id="d41ed-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="d41ed-261">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="d41ed-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="d41ed-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="d41ed-262">**CPU**</span></span>

- <span data-ttu-id="d41ed-263">Tiempo de Information\Processor de procesador</span><span class="sxs-lookup"><span data-stu-id="d41ed-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="d41ed-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="d41ed-264">**TCP/IP**</span></span>

- <span data-ttu-id="d41ed-265">TCPv6/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="d41ed-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="d41ed-266">TCPv4/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="d41ed-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="d41ed-267">**Servicio Web**</span><span class="sxs-lookup"><span data-stu-id="d41ed-267">**Web Service**</span></span>

- <span data-ttu-id="d41ed-268">Conexiones web Ftp\conexiones</span><span class="sxs-lookup"><span data-stu-id="d41ed-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="d41ed-269">Conexiones web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="d41ed-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="d41ed-270">**Subprocesamiento**</span><span class="sxs-lookup"><span data-stu-id="d41ed-270">**Threading**</span></span>

- <span data-ttu-id="d41ed-271">Bloqueos y subprocesos de CLR de .NET\\n.º de subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="d41ed-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="d41ed-272">Bloqueos y subprocesos de CLR de .NET\\n.º de subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="d41ed-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="d41ed-273">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="d41ed-273">Other Resources</span></span>

<span data-ttu-id="d41ed-274">Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d41ed-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="d41ed-275">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="d41ed-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="d41ed-276">Uso de subprocesos de ASP.NET en IIS 7,5, IIS 7,0 e IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="d41ed-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="d41ed-277">Elemento &lt;applicationPool&gt; (configuración Web)</span><span class="sxs-lookup"><span data-stu-id="d41ed-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
