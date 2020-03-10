---
uid: signalr/overview/older-versions/signalr-performance
title: Rendimiento de signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Rendimiento de SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468109"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="86b22-103">Rendimiento de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="86b22-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="86b22-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="86b22-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="86b22-105">En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="86b22-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="86b22-106">Para obtener una presentación reciente sobre el rendimiento y el escalado de Signalr, consulte [escalado del Web en tiempo real con ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="86b22-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="86b22-107">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="86b22-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="86b22-108">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="86b22-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="86b22-109">Ajuste del rendimiento del servidor de Signalr</span><span class="sxs-lookup"><span data-stu-id="86b22-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="86b22-110">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="86b22-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="86b22-111">Usar contadores de rendimiento de Signalr</span><span class="sxs-lookup"><span data-stu-id="86b22-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="86b22-112">Usar otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="86b22-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="86b22-113">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="86b22-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="86b22-114">Consideraciones de diseño</span><span class="sxs-lookup"><span data-stu-id="86b22-114">Design considerations</span></span>

<span data-ttu-id="86b22-115">En esta sección se describen los patrones que se pueden implementar durante el diseño de una aplicación Signalr, para asegurarse de que el rendimiento no se obstaculiza generando tráfico de red innecesario.</span><span class="sxs-lookup"><span data-stu-id="86b22-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="86b22-116">Frecuencia del mensaje de limitación</span><span class="sxs-lookup"><span data-stu-id="86b22-116">Throttling message frequency</span></span>

<span data-ttu-id="86b22-117">Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no necesitan enviar más de unos pocos mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="86b22-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="86b22-118">Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que pone en cola y envía mensajes con una frecuencia superior a una velocidad fija (es decir, se enviará un número determinado de mensajes cada segundo, si hay mensajes en ese momento en tervalo que se va a enviar).</span><span class="sxs-lookup"><span data-stu-id="86b22-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="86b22-119">Para obtener una aplicación de ejemplo que limita los mensajes a una determinada tasa (desde el cliente y el servidor), consulte [la alta frecuencia en tiempo real con signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="86b22-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="86b22-120">Reducir el tamaño de los mensajes</span><span class="sxs-lookup"><span data-stu-id="86b22-120">Reducing message size</span></span>

<span data-ttu-id="86b22-121">Puede reducir el tamaño de un mensaje de Signalr reduciendo el tamaño de los objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="86b22-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="86b22-122">En el código del servidor, si va a enviar un objeto que contiene propiedades que no es necesario transmitir, evite que esas propiedades se serialicen mediante el `JsonIgnore` atributo.</span><span class="sxs-lookup"><span data-stu-id="86b22-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="86b22-123">Los nombres de las propiedades también se almacenan en el mensaje; los nombres de las propiedades se pueden acortar mediante el atributo `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="86b22-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="86b22-124">En el ejemplo de código siguiente se muestra cómo excluir una propiedad de la que se va a enviar al cliente y cómo acortar los nombres de propiedad:</span><span class="sxs-lookup"><span data-stu-id="86b22-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="86b22-125">**Código de servidor .NET que muestra el atributo JsonIgnore para excluir los datos que se envían al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**</span><span class="sxs-lookup"><span data-stu-id="86b22-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="86b22-126">Con el fin de conservar la legibilidad y el mantenimiento en el código de cliente, los nombres de propiedad abreviados se pueden reasignar a nombres descriptivos después de recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="86b22-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="86b22-127">En el ejemplo de código siguiente se muestra una posible manera de volver a asignar nombres abreviados a más largos, mediante la definición de un contrato de mensaje (asignación) y el uso de la función `reMap` para aplicar el contrato a la clase de mensaje optimizada:</span><span class="sxs-lookup"><span data-stu-id="86b22-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="86b22-128">**Código JavaScript del lado cliente que reasigna nombres de propiedad abreviados a nombres legibles para el usuario.**</span><span class="sxs-lookup"><span data-stu-id="86b22-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="86b22-129">Los nombres también se pueden acortar en los mensajes del cliente al servidor, mediante el mismo método.</span><span class="sxs-lookup"><span data-stu-id="86b22-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="86b22-130">Reducir la superficie de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del objeto de mensaje también puede mejorar el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="86b22-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="86b22-131">Por ejemplo, si el intervalo completo de un `int` no es necesario, se puede usar en su lugar un `short` o `byte`.</span><span class="sxs-lookup"><span data-stu-id="86b22-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="86b22-132">Puesto que los mensajes se almacenan en el bus de mensajes en la memoria del servidor, la reducción del tamaño de los mensajes también puede solucionar problemas de memoria del servidor.</span><span class="sxs-lookup"><span data-stu-id="86b22-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="86b22-133">Ajuste del rendimiento del servidor de Signalr</span><span class="sxs-lookup"><span data-stu-id="86b22-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="86b22-134">Se pueden usar las siguientes opciones de configuración para optimizar el servidor y obtener un mejor rendimiento en una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="86b22-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="86b22-135">Para obtener información general sobre cómo mejorar el rendimiento en una aplicación de ASP.NET, vea [mejorar el rendimiento de ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="86b22-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="86b22-136">**Opciones de configuración de signalr**</span><span class="sxs-lookup"><span data-stu-id="86b22-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="86b22-137">**DefaultMessageBufferSize**: de forma predeterminada, signalr conserva 1000 mensajes en memoria por cada concentrador por conexión.</span><span class="sxs-lookup"><span data-stu-id="86b22-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="86b22-138">Si se usan mensajes grandes, se pueden crear problemas de memoria que se pueden mitigar reduciendo este valor.</span><span class="sxs-lookup"><span data-stu-id="86b22-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="86b22-139">Esta configuración se puede establecer en el controlador de eventos `Application_Start` en una aplicación ASP.NET o en el método `Configuration` de una clase de inicio OWIN en una aplicación autohospedada.</span><span class="sxs-lookup"><span data-stu-id="86b22-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="86b22-140">En el ejemplo siguiente se muestra cómo reducir este valor para reducir la superficie de memoria de la aplicación con el fin de reducir la cantidad de memoria del servidor usada:</span><span class="sxs-lookup"><span data-stu-id="86b22-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="86b22-141">**Código de servidor .NET en global. asax para reducir el tamaño del búfer de mensajes predeterminado**</span><span class="sxs-lookup"><span data-stu-id="86b22-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="86b22-142">**Opciones de configuración de IIS**</span><span class="sxs-lookup"><span data-stu-id="86b22-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="86b22-143">Número **máximo de solicitudes simultáneas por aplicación**: el aumento del número de solicitudes de IIS simultáneas aumentará los recursos del servidor disponibles para atender las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="86b22-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="86b22-144">El valor predeterminado es 5000; para aumentar este valor, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:</span><span class="sxs-lookup"><span data-stu-id="86b22-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="86b22-145">**Opciones de configuración de ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="86b22-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="86b22-146">En esta sección se incluyen las opciones de configuración que se pueden establecer en el archivo de `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="86b22-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="86b22-147">Este archivo se encuentra en una de las dos ubicaciones, en función de la plataforma:</span><span class="sxs-lookup"><span data-stu-id="86b22-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="86b22-148">Entre las opciones de ASP.NET que pueden mejorar el rendimiento de Signalr se incluyen las siguientes:</span><span class="sxs-lookup"><span data-stu-id="86b22-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="86b22-149">**Número máximo de solicitudes simultáneas por CPU**: el aumento de este valor puede aliviar los cuellos de botella de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="86b22-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="86b22-150">Para aumentar este valor, agregue la siguiente opción de configuración al archivo `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="86b22-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="86b22-151">**Límite**de la cola de solicitudes: cuando el número total de conexiones supera el valor de `maxConcurrentRequestsPerCPU`, ASP.net iniciará las solicitudes de limitación mediante una cola.</span><span class="sxs-lookup"><span data-stu-id="86b22-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="86b22-152">Para aumentar el tamaño de la cola, puede aumentar el valor de `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="86b22-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="86b22-153">Para ello, agregue la siguiente opción de configuración al nodo `processModel` en `config/machine.config` (en lugar de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="86b22-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="86b22-154">Solución de problemas de rendimiento</span><span class="sxs-lookup"><span data-stu-id="86b22-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="86b22-155">En esta sección se describen las formas de encontrar cuellos de botella de rendimiento en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86b22-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="86b22-156">Comprobando que se está usando WebSocket</span><span class="sxs-lookup"><span data-stu-id="86b22-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="86b22-157">Aunque Signalr puede utilizar una variedad de transportes para la comunicación entre el cliente y el servidor, WebSocket ofrece una ventaja significativa en el rendimiento y debe usarse si el cliente y el servidor lo admiten.</span><span class="sxs-lookup"><span data-stu-id="86b22-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="86b22-158">Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="86b22-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="86b22-159">Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollo del explorador y examinar los registros para ver qué transporte se utiliza para la conexión.</span><span class="sxs-lookup"><span data-stu-id="86b22-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="86b22-160">Para obtener información sobre el uso de las herramientas de desarrollo del explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="86b22-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="86b22-161">Usar contadores de rendimiento de Signalr</span><span class="sxs-lookup"><span data-stu-id="86b22-161">Using SignalR performance counters</span></span>

<span data-ttu-id="86b22-162">En esta sección se describe cómo habilitar y usar los contadores de rendimiento de Signalr, que se encuentran en el paquete de `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="86b22-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="86b22-163">Instalando signalr. exe</span><span class="sxs-lookup"><span data-stu-id="86b22-163">Installing signalr.exe</span></span>

<span data-ttu-id="86b22-164">Los contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada Signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="86b22-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="86b22-165">Para instalar esta utilidad, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="86b22-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="86b22-166">En Visual Studio, seleccione **herramientas** > **Administrador de paquetes Nuget** > **administrar paquetes Nuget para la solución** .</span><span class="sxs-lookup"><span data-stu-id="86b22-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="86b22-167">Busque **signalr. utils**y seleccione instalar.</span><span class="sxs-lookup"><span data-stu-id="86b22-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="86b22-168">Acepte el contrato de licencia para instalar el paquete.</span><span class="sxs-lookup"><span data-stu-id="86b22-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="86b22-169">Signalr. exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="86b22-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="86b22-170">Instalación de contadores de rendimiento con Signalr. exe</span><span class="sxs-lookup"><span data-stu-id="86b22-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="86b22-171">Para instalar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="86b22-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="86b22-172">Para quitar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="86b22-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="86b22-173">Contadores de rendimiento de signalr</span><span class="sxs-lookup"><span data-stu-id="86b22-173">SignalR Performance counters</span></span>

<span data-ttu-id="86b22-174">El paquete de utilidades instala los siguientes contadores de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="86b22-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="86b22-175">Los contadores "total" miden el número de eventos desde el último reinicio del grupo de aplicaciones o del servidor.</span><span class="sxs-lookup"><span data-stu-id="86b22-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="86b22-176">**Métricas de conexión**</span><span class="sxs-lookup"><span data-stu-id="86b22-176">**Connection metrics**</span></span>

<span data-ttu-id="86b22-177">Las métricas siguientes miden los eventos de duración de la conexión que se producen.</span><span class="sxs-lookup"><span data-stu-id="86b22-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="86b22-178">Para obtener más información, vea [Descripción y control de eventos de duración](../guide-to-the-api/handling-connection-lifetime-events.md)de la conexión.</span><span class="sxs-lookup"><span data-stu-id="86b22-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="86b22-179">**Conexiones conectadas**</span><span class="sxs-lookup"><span data-stu-id="86b22-179">**Connections Connected**</span></span>
- <span data-ttu-id="86b22-180">**Conexiones reconectadas**</span><span class="sxs-lookup"><span data-stu-id="86b22-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="86b22-181">**Conexiones desconectadas**</span><span class="sxs-lookup"><span data-stu-id="86b22-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="86b22-182">**Conexiones actuales**</span><span class="sxs-lookup"><span data-stu-id="86b22-182">**Connections Current**</span></span>

<span data-ttu-id="86b22-183">**Métricas de mensajes**</span><span class="sxs-lookup"><span data-stu-id="86b22-183">**Message metrics**</span></span>

<span data-ttu-id="86b22-184">Las métricas siguientes miden el tráfico de mensajes generado por Signalr.</span><span class="sxs-lookup"><span data-stu-id="86b22-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="86b22-185">**Total de mensajes de conexión recibidos**</span><span class="sxs-lookup"><span data-stu-id="86b22-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="86b22-186">**Total de mensajes de conexión enviados**</span><span class="sxs-lookup"><span data-stu-id="86b22-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="86b22-187">**Mensajes de conexión recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="86b22-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="86b22-188">**Mensajes de conexión enviados/seg.**</span><span class="sxs-lookup"><span data-stu-id="86b22-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="86b22-189">**Métricas del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="86b22-189">**Message bus metrics**</span></span>

<span data-ttu-id="86b22-190">Las métricas siguientes miden el tráfico a través del bus de mensajes interno de Signalr, la cola en la que se colocan todos los mensajes entrantes y salientes de Signalr.</span><span class="sxs-lookup"><span data-stu-id="86b22-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="86b22-191">Un mensaje se **publica** cuando se envía o se emite.</span><span class="sxs-lookup"><span data-stu-id="86b22-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="86b22-192">Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; debe ser igual al número de clientes más el propio servidor.</span><span class="sxs-lookup"><span data-stu-id="86b22-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="86b22-193">Un **trabajador asignado** es un componente que envía datos a las conexiones activas. un **trabajador ocupado** es aquel que está enviando un mensaje de forma activa.</span><span class="sxs-lookup"><span data-stu-id="86b22-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="86b22-194">**Total de mensajes de bus de mensajes recibidos**</span><span class="sxs-lookup"><span data-stu-id="86b22-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="86b22-195">**Mensajes de bus de mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="86b22-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="86b22-196">**Total de mensajes de bus de mensajes publicados**</span><span class="sxs-lookup"><span data-stu-id="86b22-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="86b22-197">**Mensajes de bus de mensajes publicados/s**</span><span class="sxs-lookup"><span data-stu-id="86b22-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="86b22-198">**Suscriptores de bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="86b22-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="86b22-199">**Total de suscriptores de Message bus**</span><span class="sxs-lookup"><span data-stu-id="86b22-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="86b22-200">**Suscriptores de Message bus/s**</span><span class="sxs-lookup"><span data-stu-id="86b22-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="86b22-201">**Trabajos asignados del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="86b22-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="86b22-202">**Trabajadores ocupados del bus de mensajes**</span><span class="sxs-lookup"><span data-stu-id="86b22-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="86b22-203">**Temas del bus de mensajes actuales**</span><span class="sxs-lookup"><span data-stu-id="86b22-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="86b22-204">**Métricas de error**</span><span class="sxs-lookup"><span data-stu-id="86b22-204">**Error metrics**</span></span>

<span data-ttu-id="86b22-205">Las métricas siguientes miden los errores generados por el tráfico de mensajes de Signalr.</span><span class="sxs-lookup"><span data-stu-id="86b22-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="86b22-206">Los errores de **resolución de concentrador** se producen cuando no se puede resolver un método de concentrador o concentrador.</span><span class="sxs-lookup"><span data-stu-id="86b22-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="86b22-207">Los errores de **invocación de concentrador** son excepciones que se producen al invocar un método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="86b22-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="86b22-208">Los errores de **transporte** son errores de conexión producidos durante una solicitud o respuesta http.</span><span class="sxs-lookup"><span data-stu-id="86b22-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="86b22-209">**Errores: todos los totales**</span><span class="sxs-lookup"><span data-stu-id="86b22-209">**Errors: All Total**</span></span>
- <span data-ttu-id="86b22-210">**Errores: todos/seg.**</span><span class="sxs-lookup"><span data-stu-id="86b22-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="86b22-211">**Errores: total de resolución de concentrador**</span><span class="sxs-lookup"><span data-stu-id="86b22-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="86b22-212">**Errores: resolución de concentrador/s**</span><span class="sxs-lookup"><span data-stu-id="86b22-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="86b22-213">**Errores: total de invocación de concentrador**</span><span class="sxs-lookup"><span data-stu-id="86b22-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="86b22-214">**Errores: invocación de concentrador/s**</span><span class="sxs-lookup"><span data-stu-id="86b22-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="86b22-215">**Errores: total de transporte**</span><span class="sxs-lookup"><span data-stu-id="86b22-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="86b22-216">**Errores: transporte/s**</span><span class="sxs-lookup"><span data-stu-id="86b22-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="86b22-217">**Métricas de ampliación**</span><span class="sxs-lookup"><span data-stu-id="86b22-217">**Scaleout metrics**</span></span>

<span data-ttu-id="86b22-218">Las métricas siguientes miden el tráfico y los errores generados por el proveedor de ampliación.</span><span class="sxs-lookup"><span data-stu-id="86b22-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="86b22-219">Un **flujo** en este contexto es una unidad de escalado que usa el proveedor ampliación. se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis.</span><span class="sxs-lookup"><span data-stu-id="86b22-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="86b22-220">De forma predeterminada, solo se usa una secuencia, pero se puede aumentar a través de la configuración en SQL Server y Service Bus.</span><span class="sxs-lookup"><span data-stu-id="86b22-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="86b22-221">Una secuencia de **almacenamiento en búfer** es aquella que ha entrado en un estado de error. Cuando la secuencia se encuentra en el estado FAULTED, se producirá un error en todos los mensajes enviados al plano posterior hasta que la secuencia deje de producir errores.</span><span class="sxs-lookup"><span data-stu-id="86b22-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="86b22-222">La **longitud** de la cola de envío es el número de mensajes que se han expuesto pero que todavía no se han enviado.</span><span class="sxs-lookup"><span data-stu-id="86b22-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="86b22-223">**Ampliación mensajes de bus de mensajes recibidos/seg.**</span><span class="sxs-lookup"><span data-stu-id="86b22-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="86b22-224">**Total de secuencias ampliación**</span><span class="sxs-lookup"><span data-stu-id="86b22-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="86b22-225">**Secuencias de ampliación abiertas**</span><span class="sxs-lookup"><span data-stu-id="86b22-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="86b22-226">**Almacenamiento en búfer de secuencias ampliación**</span><span class="sxs-lookup"><span data-stu-id="86b22-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="86b22-227">**Total de errores de ampliación**</span><span class="sxs-lookup"><span data-stu-id="86b22-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="86b22-228">**Errores de ampliación por segundo**</span><span class="sxs-lookup"><span data-stu-id="86b22-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="86b22-229">**Longitud de cola de envío de ampliación**</span><span class="sxs-lookup"><span data-stu-id="86b22-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="86b22-230">Para obtener más información sobre lo que están midiendo estos contadores, consulte [signalr ampliación con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="86b22-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="86b22-231">Usar otros contadores de rendimiento</span><span class="sxs-lookup"><span data-stu-id="86b22-231">Using other performance counters</span></span>

<span data-ttu-id="86b22-232">Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86b22-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="86b22-233">**Memoria**</span><span class="sxs-lookup"><span data-stu-id="86b22-233">**Memory**</span></span>

- <span data-ttu-id="86b22-234">Memoria de .NET CLR n.º de bytes en todos los montones (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="86b22-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="86b22-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="86b22-235">**ASP.NET**</span></span>

- <span data-ttu-id="86b22-236">ASP. Net\solicitudes actual</span><span class="sxs-lookup"><span data-stu-id="86b22-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="86b22-237">ASP. NET\Queued</span><span class="sxs-lookup"><span data-stu-id="86b22-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="86b22-238">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="86b22-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="86b22-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="86b22-239">**CPU**</span></span>

- <span data-ttu-id="86b22-240">Tiempo de Information\Processor de procesador</span><span class="sxs-lookup"><span data-stu-id="86b22-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="86b22-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="86b22-241">**TCP/IP**</span></span>

- <span data-ttu-id="86b22-242">TCPv6/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="86b22-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="86b22-243">TCPv4/conexiones establecidas</span><span class="sxs-lookup"><span data-stu-id="86b22-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="86b22-244">**Servicio Web**</span><span class="sxs-lookup"><span data-stu-id="86b22-244">**Web Service**</span></span>

- <span data-ttu-id="86b22-245">Conexiones web Ftp\conexiones</span><span class="sxs-lookup"><span data-stu-id="86b22-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="86b22-246">Conexiones web Service\Maximum</span><span class="sxs-lookup"><span data-stu-id="86b22-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="86b22-247">**Subprocesamiento**</span><span class="sxs-lookup"><span data-stu-id="86b22-247">**Threading**</span></span>

- <span data-ttu-id="86b22-248">\# de .NET CLR LocksAndThreads de subprocesos lógicos actuales</span><span class="sxs-lookup"><span data-stu-id="86b22-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="86b22-249">.NET CLR LocksAnd threads\# de subprocesos físicos actuales</span><span class="sxs-lookup"><span data-stu-id="86b22-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="86b22-250">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="86b22-250">Other Resources</span></span>

<span data-ttu-id="86b22-251">Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="86b22-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="86b22-252">[Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="86b22-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="86b22-253">Uso de subprocesos de ASP.NET en IIS 7,5, IIS 7,0 e IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="86b22-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="86b22-254">Elemento &lt;applicationPool&gt; (configuración Web)</span><span class="sxs-lookup"><span data-stu-id="86b22-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
