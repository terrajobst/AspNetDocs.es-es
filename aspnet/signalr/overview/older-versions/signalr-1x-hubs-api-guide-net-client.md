---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'Guía de la API de ASP.NET Signalr hubs: cliente .NET (Signalr 1. x) | Microsoft Docs'
author: bradygaster
description: En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de .NET, como la tienda Windows (WinRT), WPF, Silverlight y los inconvenientes...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505975"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="1dc19-103">Guía de la API de ASP.NET Signalr hubs: cliente .NET (Signalr 1. x)</span><span class="sxs-lookup"><span data-stu-id="1dc19-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>

<span data-ttu-id="1dc19-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1dc19-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="1dc19-105">En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de .NET, como la tienda Windows (WinRT), WPF, Silverlight y aplicaciones de consola.</span><span class="sxs-lookup"><span data-stu-id="1dc19-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="1dc19-106">Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="1dc19-107">En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="1dc19-108">En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="1dc19-109">Signalr se encarga de todas las conexiones de cliente a servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="1dc19-110">Signalr también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="1dc19-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="1dc19-111">Para obtener una introducción a Signalr, hubs y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación Signalr completa, consulte [signalr-introducción](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="1dc19-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="1dc19-112">Información general</span><span class="sxs-lookup"><span data-stu-id="1dc19-112">Overview</span></span>

<span data-ttu-id="1dc19-113">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="1dc19-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="1dc19-114">Configuración de cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="1dc19-115">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="1dc19-116">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="1dc19-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="1dc19-117">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="1dc19-118">Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPF</span><span class="sxs-lookup"><span data-stu-id="1dc19-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="1dc19-119">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="1dc19-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="1dc19-120">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="1dc19-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="1dc19-121">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="1dc19-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="1dc19-122">Cómo especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="1dc19-123">Cómo crear el proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="1dc19-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="1dc19-124">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="1dc19-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="1dc19-125">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="1dc19-126">Métodos con parámetros, especificar tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="1dc19-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="1dc19-127">Métodos con parámetros, especificar objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="1dc19-128">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="1dc19-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="1dc19-129">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="1dc19-130">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="1dc19-131">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="1dc19-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="1dc19-132">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="1dc19-133">Ejemplos de código de la aplicación de consola, Silverlight y WPF para los métodos de cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="1dc19-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="1dc19-134">Para obtener un ejemplo de proyectos de cliente .NET, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="1dc19-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="1dc19-135">[Gustavo-Armenta/signalr-samples](https://github.com/gustavo-armenta/SignalR-Samples) en github.com (WinRT, Silverlight, ejemplos de aplicación de consola).</span><span class="sxs-lookup"><span data-stu-id="1dc19-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="1dc19-136">[DamianEdwards/signalr-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en github.com (ejemplo de WPF).</span><span class="sxs-lookup"><span data-stu-id="1dc19-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="1dc19-137">[Signalr/Microsoft. Aspnet. signalr. Client. samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en github.com (ejemplo de aplicación de consola).</span><span class="sxs-lookup"><span data-stu-id="1dc19-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="1dc19-138">Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="1dc19-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="1dc19-139">Guía de API de signalr hubs-servidor</span><span class="sxs-lookup"><span data-stu-id="1dc19-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="1dc19-140">Guía de API de signalr hubs: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="1dc19-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="1dc19-141">Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API.</span><span class="sxs-lookup"><span data-stu-id="1dc19-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="1dc19-142">Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="1dc19-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="1dc19-143">Configuración del cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-143">Client setup</span></span>

<span data-ttu-id="1dc19-144">Instale el paquete NuGet [Microsoft. Aspnet. signalr. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (no el paquete [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ).</span><span class="sxs-lookup"><span data-stu-id="1dc19-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="1dc19-145">Este paquete es compatible con los clientes de WinRT, Silverlight, WPF, aplicación de consola y Windows Phone, tanto para .NET 4 como para .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="1dc19-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="1dc19-146">Si la versión de Signalr que tiene en el cliente es diferente de la versión que tiene en el servidor, Signalr suele ser capaz de adaptarse a la diferencia.</span><span class="sxs-lookup"><span data-stu-id="1dc19-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="1dc19-147">Por ejemplo, si se lanza la versión 2,0 de Signalr y se instala en el servidor, el servidor admitirá clientes que tengan 1.1. x instalado, así como clientes que tengan instalado 2,0.</span><span class="sxs-lookup"><span data-stu-id="1dc19-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="1dc19-148">Si la diferencia entre la versión del servidor y la versión del cliente es demasiado grande, Signalr inicia una excepción `InvalidOperationException` cuando el cliente intenta establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="1dc19-149">El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="1dc19-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="1dc19-150">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-150">How to establish a connection</span></span>

<span data-ttu-id="1dc19-151">Para poder establecer una conexión, tiene que crear un objeto de `HubConnection` y crear un proxy.</span><span class="sxs-lookup"><span data-stu-id="1dc19-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="1dc19-152">Para establecer la conexión, llame al método `Start` en el objeto `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="1dc19-153">En el caso de los clientes de JavaScript, debe registrar al menos un controlador de eventos antes de llamar al método `Start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="1dc19-154">Esto no es necesario para los clientes de .NET.</span><span class="sxs-lookup"><span data-stu-id="1dc19-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="1dc19-155">En el caso de los clientes de JavaScript, el código de proxy generado crea automáticamente servidores proxy para todos los concentradores que existen en el servidor, y el registro de un controlador es la forma en que se indican los concentradores que el cliente intenta usar.</span><span class="sxs-lookup"><span data-stu-id="1dc19-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="1dc19-156">Pero para un cliente .NET se crean proxies de concentrador de forma manual, por lo que Signalr supone que va a usar cualquier centro para el que cree un proxy.</span><span class="sxs-lookup"><span data-stu-id="1dc19-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>

<span data-ttu-id="1dc19-157">El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr.</span><span class="sxs-lookup"><span data-stu-id="1dc19-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="1dc19-158">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="1dc19-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="1dc19-159">El método `Start` se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="1dc19-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="1dc19-160">Para asegurarse de que las líneas de código posteriores no se ejecutan hasta que se establece la conexión, use `await` en un método asincrónico ASP.NET 4,5 o `.Wait()` en un método sincrónico.</span><span class="sxs-lookup"><span data-stu-id="1dc19-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="1dc19-161">No use `.Wait()` en un cliente de WinRT.</span><span class="sxs-lookup"><span data-stu-id="1dc19-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="1dc19-162">La clase `HubConnection` es segura para la ejecución de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="1dc19-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="1dc19-163">Conexiones entre dominios desde clientes de Silverlight</span><span class="sxs-lookup"><span data-stu-id="1dc19-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="1dc19-164">Para obtener información sobre cómo habilitar las conexiones entre dominios desde clientes de Silverlight, consulte [hacer que un servicio esté disponible a través](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)de los límites del dominio.</span><span class="sxs-lookup"><span data-stu-id="1dc19-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="1dc19-165">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-165">How to configure the connection</span></span>

<span data-ttu-id="1dc19-166">Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="1dc19-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="1dc19-167">Límite de conexiones simultáneas.</span><span class="sxs-lookup"><span data-stu-id="1dc19-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="1dc19-168">Parámetros de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="1dc19-168">Query string parameters.</span></span>
- <span data-ttu-id="1dc19-169">Método de transporte.</span><span class="sxs-lookup"><span data-stu-id="1dc19-169">The transport method.</span></span>
- <span data-ttu-id="1dc19-170">Encabezados HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dc19-170">HTTP headers.</span></span>
- <span data-ttu-id="1dc19-171">Certificados de cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="1dc19-172">Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPF</span><span class="sxs-lookup"><span data-stu-id="1dc19-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="1dc19-173">En los clientes de WPF, es posible que tenga que aumentar el número máximo de conexiones simultáneas de su valor predeterminado de 2.</span><span class="sxs-lookup"><span data-stu-id="1dc19-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="1dc19-174">El valor recomendado es 10.</span><span class="sxs-lookup"><span data-stu-id="1dc19-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="1dc19-175">Para obtener más información, consulte [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dc19-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="1dc19-176">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="1dc19-176">How to specify query string parameters</span></span>

<span data-ttu-id="1dc19-177">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="1dc19-178">En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="1dc19-179">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="1dc19-180">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="1dc19-180">How to specify the transport method</span></span>

<span data-ttu-id="1dc19-181">Como parte del proceso de conexión, un cliente de Signalr normalmente negocia con el servidor para determinar el mejor transporte compatible con el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="1dc19-182">Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación.</span><span class="sxs-lookup"><span data-stu-id="1dc19-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="1dc19-183">Para especificar el método de transporte, pase un objeto de transporte al método de inicio.</span><span class="sxs-lookup"><span data-stu-id="1dc19-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="1dc19-184">En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="1dc19-185">El espacio de nombres [Microsoft. Aspnet. signalr. Client. transportes](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) incluye las siguientes clases que se pueden utilizar para especificar el transporte.</span><span class="sxs-lookup"><span data-stu-id="1dc19-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="1dc19-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="1dc19-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="1dc19-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="1dc19-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="1dc19-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y el cliente usan .net 4,5).</span><span class="sxs-lookup"><span data-stu-id="1dc19-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="1dc19-189">[Transporte](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) automático (elige automáticamente el mejor transporte que admiten tanto el cliente como el servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="1dc19-190">Este es el transporte predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1dc19-190">This is the default transport.</span></span> <span data-ttu-id="1dc19-191">Pasarlo al método `Start` tiene el mismo efecto que no pasar nada.)</span><span class="sxs-lookup"><span data-stu-id="1dc19-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="1dc19-192">El transporte ForeverFrame no se incluye en esta lista porque solo lo usan los exploradores.</span><span class="sxs-lookup"><span data-stu-id="1dc19-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="1dc19-193">Para obtener información sobre cómo comprobar el método de transporte en el código del servidor, consulte [ASP.net signalr hubs API Guide-Server: How to get Information about the Client from the Context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="1dc19-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="1dc19-194">Para obtener más información sobre los transportes y los respaldos, vea [Introducción a signalr-transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="1dc19-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="1dc19-195">Cómo especificar encabezados HTTP</span><span class="sxs-lookup"><span data-stu-id="1dc19-195">How to specify HTTP headers</span></span>

<span data-ttu-id="1dc19-196">Para establecer los encabezados HTTP, utilice la propiedad `Headers` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="1dc19-197">En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="1dc19-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="1dc19-198">Cómo especificar certificados de cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-198">How to specify client certificates</span></span>

<span data-ttu-id="1dc19-199">Para agregar certificados de cliente, use el método `AddClientCertificate` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="1dc19-200">Cómo crear el proxy de concentrador</span><span class="sxs-lookup"><span data-stu-id="1dc19-200">How to create the Hub proxy</span></span>

<span data-ttu-id="1dc19-201">Para definir métodos en el cliente a los que puede llamar un concentrador desde el servidor e invocar métodos en un concentrador en el servidor, cree un proxy para el concentrador llamando a `CreateHubProxy` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="1dc19-202">La cadena que se pasa a `CreateHubProxy` es el nombre de la clase del concentrador o el nombre especificado por el atributo `HubName` si se usó uno en el servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="1dc19-203">La coincidencia de nombres no distingue mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1dc19-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="1dc19-204">**Clase hub en el servidor**</span><span class="sxs-lookup"><span data-stu-id="1dc19-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="1dc19-205">**Crear proxy de cliente para la clase de concentrador**</span><span class="sxs-lookup"><span data-stu-id="1dc19-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="1dc19-206">Si decora la clase hub con un atributo `HubName`, use ese nombre.</span><span class="sxs-lookup"><span data-stu-id="1dc19-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="1dc19-207">**Clase hub en el servidor**</span><span class="sxs-lookup"><span data-stu-id="1dc19-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="1dc19-208">**Crear proxy de cliente para la clase de concentrador**</span><span class="sxs-lookup"><span data-stu-id="1dc19-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="1dc19-209">El objeto proxy es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="1dc19-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="1dc19-210">De hecho, si llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtiene el mismo objeto de `IHubProxy` almacenado en memoria caché.</span><span class="sxs-lookup"><span data-stu-id="1dc19-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="1dc19-211">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="1dc19-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="1dc19-212">Para definir un método al que pueda llamar el servidor, use el método `On` del proxy para registrar un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="1dc19-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="1dc19-213">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1dc19-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="1dc19-214">Por ejemplo, `Clients.All.UpdateStockPrice` en el servidor ejecutará `updateStockPrice`, `updatestockprice`o `UpdateStockPrice` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="1dc19-215">Las distintas plataformas de cliente tienen requisitos diferentes para escribir el código de método para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="1dc19-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="1dc19-216">Los ejemplos que se muestran son para clientes de WinRT (Windows Store .NET).</span><span class="sxs-lookup"><span data-stu-id="1dc19-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="1dc19-217">Los ejemplos de WPF, Silverlight y aplicación de consola se proporcionan en [una sección independiente, más adelante en este tema](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="1dc19-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="1dc19-218">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-218">Methods without parameters</span></span>

<span data-ttu-id="1dc19-219">Si el método que está controlando no tiene parámetros, use la sobrecarga no genérica del método `On`:</span><span class="sxs-lookup"><span data-stu-id="1dc19-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="1dc19-220">**Método de cliente de llamada de código de servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="1dc19-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="1dc19-221">**Código de cliente de WinRT para el método al que se llama desde el servidor sin parámetros ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="1dc19-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="1dc19-222">Métodos con parámetros que especifican los tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="1dc19-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="1dc19-223">Si el método que está controlando tiene parámetros, especifique los tipos de los parámetros como tipos genéricos del método `On`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="1dc19-224">Existen sobrecargas genéricas del método `On` para que pueda especificar hasta 8 parámetros (4 en Windows Phone 7).</span><span class="sxs-lookup"><span data-stu-id="1dc19-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="1dc19-225">En el ejemplo siguiente, se envía un parámetro al método `UpdateStockPrice`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="1dc19-226">**Método de cliente de llamada de código de servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="1dc19-227">**La clase stock utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="1dc19-228">**Código de cliente de WinRT para un método llamado desde el servidor con un parámetro ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="1dc19-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="1dc19-229">Métodos con parámetros, especificar objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="1dc19-230">Como alternativa a especificar parámetros como tipos genéricos del método `On`, puede especificar parámetros como objetos dinámicos:</span><span class="sxs-lookup"><span data-stu-id="1dc19-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="1dc19-231">**Método de cliente de llamada de código de servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="1dc19-232">**La clase stock utilizada para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="1dc19-233">**Código de cliente de WinRT para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="1dc19-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="1dc19-234">Cómo quitar un controlador</span><span class="sxs-lookup"><span data-stu-id="1dc19-234">How to remove a handler</span></span>

<span data-ttu-id="1dc19-235">Para quitar un controlador, llame a su método `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="1dc19-236">**Código de cliente para un método llamado desde el servidor**</span><span class="sxs-lookup"><span data-stu-id="1dc19-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="1dc19-237">**Código de cliente para quitar el controlador**</span><span class="sxs-lookup"><span data-stu-id="1dc19-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="1dc19-238">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-238">How to call server methods from the client</span></span>

<span data-ttu-id="1dc19-239">Para llamar a un método en el servidor, use el método `Invoke` en el proxy de concentrador.</span><span class="sxs-lookup"><span data-stu-id="1dc19-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="1dc19-240">Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica del método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="1dc19-241">**Código de servidor para un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="1dc19-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="1dc19-242">**Código de cliente que llama a un método que no tiene ningún valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="1dc19-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="1dc19-243">Si el método de servidor tiene un valor devuelto, especifique el tipo de valor devuelto como tipo genérico del método `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="1dc19-244">**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo.**</span><span class="sxs-lookup"><span data-stu-id="1dc19-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="1dc19-245">**La clase stock utilizada para el parámetro y el valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="1dc19-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="1dc19-246">**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico ASP.NET 4,5**</span><span class="sxs-lookup"><span data-stu-id="1dc19-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="1dc19-247">**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**</span><span class="sxs-lookup"><span data-stu-id="1dc19-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="1dc19-248">El método `Invoke` se ejecuta de forma asincrónica y devuelve un objeto `Task`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="1dc19-249">Si no especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que finalice la ejecución del método invocado.</span><span class="sxs-lookup"><span data-stu-id="1dc19-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="1dc19-250">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="1dc19-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="1dc19-251">Signalr proporciona los siguientes eventos de duración de conexión que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="1dc19-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="1dc19-252">`Received`: se genera cuando se reciben datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="1dc19-253">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="1dc19-253">Provides the received data.</span></span>
- <span data-ttu-id="1dc19-254">`ConnectionSlow`: se genera cuando el cliente detecta una conexión de eliminación lenta o con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="1dc19-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="1dc19-255">`Reconnecting`: se genera cuando se inicia la reconexión del transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="1dc19-256">`Reconnected`: se genera cuando se vuelve a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="1dc19-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="1dc19-257">`StateChanged`: se genera cuando cambia el estado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="1dc19-258">Proporciona el estado anterior y el nuevo estado.</span><span class="sxs-lookup"><span data-stu-id="1dc19-258">Provides the old state and the new state.</span></span> <span data-ttu-id="1dc19-259">Para obtener información sobre los valores de estado de la conexión, consulte [enumeración ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="1dc19-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="1dc19-260">`Closed`: se genera cuando se desconecta la conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="1dc19-261">Por ejemplo, si desea mostrar mensajes de advertencia sobre errores que no son graves pero causan problemas de conexión intermitentes, como la lentitud o la caída frecuente de la conexión, controle el evento `ConnectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="1dc19-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="1dc19-262">Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="1dc19-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="1dc19-263">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="1dc19-263">How to handle errors</span></span>

<span data-ttu-id="1dc19-264">Si no se habilitan explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que Signalr devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="1dc19-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="1dc19-265">Por ejemplo, si se produce un error en una llamada a `newContosoChatMessage`, el mensaje de error del objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`". no se recomienda el envío de mensajes de error detallados a los clientes en producción por motivos de seguridad, pero si desea habilitar mensajes de error detallados para la solución de problemas, use el siguiente código en el servidor.</span><span class="sxs-lookup"><span data-stu-id="1dc19-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="1dc19-266">Para controlar los errores generados por Signalr, puede Agregar un controlador para el evento `Error` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="1dc19-267">Para controlar los errores de las invocaciones de método, ajuste el código en un bloque try-catch.</span><span class="sxs-lookup"><span data-stu-id="1dc19-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="1dc19-268">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="1dc19-268">How to enable client-side logging</span></span>

<span data-ttu-id="1dc19-269">Para habilitar el registro del lado cliente, establezca las propiedades `TraceLevel` y `TraceWriter` en el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="1dc19-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="1dc19-270">Ejemplos de código de la aplicación de consola, Silverlight y WPF para los métodos de cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="1dc19-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="1dc19-271">Los ejemplos de código mostrados anteriormente para definir métodos de cliente a los que puede llamar el servidor se aplican a los clientes de WinRT.</span><span class="sxs-lookup"><span data-stu-id="1dc19-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="1dc19-272">En los siguientes ejemplos se muestra el código equivalente para los clientes de la aplicación de consola, Silverlight y WPF.</span><span class="sxs-lookup"><span data-stu-id="1dc19-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="1dc19-273">Métodos sin parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-273">Methods without parameters</span></span>

<span data-ttu-id="1dc19-274">**Código de cliente de WPF para el método al que se llama desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="1dc19-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="1dc19-275">**Código de cliente de Silverlight para el método al que se llama desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="1dc19-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="1dc19-276">**Código de cliente de aplicación de consola para el método al que se llama desde el servidor sin parámetros**</span><span class="sxs-lookup"><span data-stu-id="1dc19-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="1dc19-277">Métodos con parámetros que especifican los tipos de parámetro</span><span class="sxs-lookup"><span data-stu-id="1dc19-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="1dc19-278">**Código de cliente de WPF para un método llamado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="1dc19-279">**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="1dc19-280">**Código de cliente de aplicación de consola para un método llamado desde el servidor con un parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="1dc19-281">Métodos con parámetros, especificar objetos dinámicos para los parámetros</span><span class="sxs-lookup"><span data-stu-id="1dc19-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="1dc19-282">**Código de cliente de WPF para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="1dc19-283">**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="1dc19-284">**Código de cliente de aplicación de consola para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**</span><span class="sxs-lookup"><span data-stu-id="1dc19-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
