---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: 'Guía de la API signalr 1. x hubs: cliente JavaScript | Microsoft Docs'
author: bradygaster
description: En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 1,1 en clientes de JavaScript, como exploradores y la tienda Windows (WinJS) aplic...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431107"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="38170-103">Guía de la API SignalR 1.x Hubs - Cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="38170-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>

<span data-ttu-id="38170-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="38170-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="38170-105">En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 1,1 en los clientes de JavaScript, como los exploradores y las aplicaciones de la tienda Windows (WinJS).</span><span class="sxs-lookup"><span data-stu-id="38170-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="38170-106">Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="38170-107">En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="38170-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="38170-108">En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="38170-109">Signalr se encarga de todas las conexiones de cliente a servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="38170-110">Signalr también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="38170-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="38170-111">Para obtener una introducción a Signalr, hubs y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación Signalr completa, consulte [signalr-introducción](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="38170-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>

## <a name="overview"></a><span data-ttu-id="38170-112">Información general</span><span class="sxs-lookup"><span data-stu-id="38170-112">Overview</span></span>

<span data-ttu-id="38170-113">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="38170-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="38170-114">El proxy generado y lo que hace para usted</span><span class="sxs-lookup"><span data-stu-id="38170-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="38170-115">Cuándo usar el proxy generado</span><span class="sxs-lookup"><span data-stu-id="38170-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="38170-116">Configuración de cliente</span><span class="sxs-lookup"><span data-stu-id="38170-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="38170-117">Cómo hacer referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="38170-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="38170-118">Cómo crear un archivo físico para el proxy generado por Signalr</span><span class="sxs-lookup"><span data-stu-id="38170-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="38170-119">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="38170-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="38170-120">$. Connection. Hub es el mismo objeto que $. hubConnection () crea</span><span class="sxs-lookup"><span data-stu-id="38170-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="38170-121">Ejecución asincrónica del método Start</span><span class="sxs-lookup"><span data-stu-id="38170-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="38170-122">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="38170-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="38170-123">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="38170-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="38170-124">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="38170-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="38170-125">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="38170-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="38170-126">Cómo obtener un proxy para una clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="38170-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="38170-127">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="38170-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="38170-128">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="38170-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="38170-129">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="38170-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="38170-130">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="38170-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="38170-131">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="38170-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="38170-132">Para obtener documentación sobre cómo programar el servidor o los clientes de .NET, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="38170-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="38170-133">Guía de API de signalr hubs-servidor</span><span class="sxs-lookup"><span data-stu-id="38170-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="38170-134">Guía de la API de signalr hubs: cliente .NET</span><span class="sxs-lookup"><span data-stu-id="38170-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="38170-135">Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API.</span><span class="sxs-lookup"><span data-stu-id="38170-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="38170-136">Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="38170-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="38170-137">El proxy generado y lo que hace para usted</span><span class="sxs-lookup"><span data-stu-id="38170-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="38170-138">Puede programar un cliente de JavaScript para que se comunique con un servicio de Signalr con o sin un proxy que Signalr genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="38170-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="38170-139">Lo que hace el proxy es simplificar la sintaxis del código que se utiliza para conectarse, escribir métodos a los que llama el servidor y llamar a métodos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="38170-140">Al escribir código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece que estaba ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="38170-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="38170-141">La sintaxis de proxy generada también permite un error de lado cliente inmediato e inteligible si se escribe un nombre de método de servidor invariable.</span><span class="sxs-lookup"><span data-stu-id="38170-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="38170-142">Y si crea manualmente el archivo que define los proxies, también puede obtener compatibilidad con IntelliSense para escribir código que llame a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="38170-143">Por ejemplo, supongamos que tiene la siguiente clase de concentrador en el servidor:</span><span class="sxs-lookup"><span data-stu-id="38170-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="38170-144">En los siguientes ejemplos de código se muestra el aspecto del código JavaScript para invocar el método `NewContosoChatMessage` en el servidor y recibir invocaciones del método `addContosoChatMessageToPage` desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="38170-145">**Con el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="38170-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="38170-146">**Sin el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="38170-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="38170-147">Cuándo usar el proxy generado</span><span class="sxs-lookup"><span data-stu-id="38170-147">When to use the generated proxy</span></span>

<span data-ttu-id="38170-148">Si desea registrar varios controladores de eventos para un método de cliente al que el servidor llama, no puede usar el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="38170-149">De lo contrario, puede elegir usar el proxy generado o no según sus preferencias de codificación.</span><span class="sxs-lookup"><span data-stu-id="38170-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="38170-150">Si decide no utilizarlo, no es necesario que haga referencia a la dirección URL de "signalr/hubs" en un elemento de `script` en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="38170-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="38170-151">Configuración del cliente</span><span class="sxs-lookup"><span data-stu-id="38170-151">Client setup</span></span>

<span data-ttu-id="38170-152">Un cliente de JavaScript requiere referencias a jQuery y al archivo JavaScript de Signalr Core.</span><span class="sxs-lookup"><span data-stu-id="38170-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="38170-153">La versión de jQuery debe ser 1.6.4 o versiones posteriores, como 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="38170-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="38170-154">Si decide usar el proxy generado, también necesitará una referencia al archivo JavaScript del proxy generado por Signalr.</span><span class="sxs-lookup"><span data-stu-id="38170-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="38170-155">En el ejemplo siguiente se muestra el aspecto de las referencias en una página HTML que usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="38170-156">Estas referencias se deben incluir en este orden: primero jQuery, Signalr Core después de eso y los proxies de Signalr en último lugar.</span><span class="sxs-lookup"><span data-stu-id="38170-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="38170-157">Cómo hacer referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="38170-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="38170-158">En el ejemplo anterior, la referencia al proxy generado por Signalr es el código de JavaScript generado dinámicamente, no un archivo físico.</span><span class="sxs-lookup"><span data-stu-id="38170-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="38170-159">Signalr crea el código JavaScript del proxy sobre la marcha y lo atiende al cliente en respuesta a la dirección URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="38170-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="38170-160">Si especificó una dirección URL base diferente para las conexiones de Signalr en el servidor en el método de `MapHubs`, la dirección URL del archivo de proxy generado dinámicamente es su dirección URL personalizada con "/hubs" anexado.</span><span class="sxs-lookup"><span data-stu-id="38170-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="38170-161">En el caso de los clientes JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar de uno generado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="38170-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="38170-162">Para obtener más información, vea [Cómo crear un archivo físico para el proxy generado por signalr](#manualproxy) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="38170-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="38170-163">En una vista de Razor de ASP.NET MVC 4, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo proxy:</span><span class="sxs-lookup"><span data-stu-id="38170-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="38170-164">Para obtener más información sobre el uso de Signalr en MVC 4, vea [Introducción con signalr y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="38170-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="38170-165">En una vista de Razor de ASP.NET MVC 3, use `Url.Content` para la referencia del archivo proxy:</span><span class="sxs-lookup"><span data-stu-id="38170-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="38170-166">En una aplicación de formularios Web Forms ASP.NET, use `ResolveClientUrl` para la referencia de archivo de servidores proxy o regístrela mediante el ScriptManager mediante la ruta de acceso relativa de la raíz de la aplicación (empezando por una tilde):</span><span class="sxs-lookup"><span data-stu-id="38170-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="38170-167">Como norma general, use el mismo método para especificar la dirección URL "/signalr/hubs" que se usa para los archivos CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="38170-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="38170-168">Si especifica una dirección URL sin usar una tilde, en algunos escenarios la aplicación funcionará correctamente al realizar la prueba en Visual Studio con IIS Express pero se producirá un error 404 al implementar en el IIS completo.</span><span class="sxs-lookup"><span data-stu-id="38170-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="38170-169">Para obtener más información, vea **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web de ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio de MSDN.</span><span class="sxs-lookup"><span data-stu-id="38170-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="38170-170">Al ejecutar un proyecto web en Visual Studio 2012 en modo de depuración, y si usa Internet Explorer como explorador, puede ver el archivo de proxy en **Explorador de soluciones** en **documentos de script**, como se muestra en la siguiente ilustración.</span><span class="sxs-lookup"><span data-stu-id="38170-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Archivo proxy generado por JavaScript en Explorador de soluciones](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="38170-172">Para ver el contenido del archivo, haga doble clic en **hubs**.</span><span class="sxs-lookup"><span data-stu-id="38170-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="38170-173">Si no usa Visual Studio 2012 e Internet Explorer, o si no está en modo de depuración, también puede obtener el contenido del archivo si va a la dirección URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="38170-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="38170-174">Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="38170-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="38170-175">Cómo crear un archivo físico para el proxy generado por Signalr</span><span class="sxs-lookup"><span data-stu-id="38170-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="38170-176">Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código proxy y hacer referencia a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="38170-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="38170-177">Tal vez desee hacerlo para controlar el comportamiento de almacenamiento en caché o de agrupación, o para obtener IntelliSense cuando se codifican llamadas a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="38170-178">Para crear un archivo proxy, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="38170-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="38170-179">Instale el paquete NuGet [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="38170-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="38170-180">Abra un símbolo del sistema y vaya a la carpeta *Tools* que contiene el archivo signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="38170-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="38170-181">La carpeta Tools se encuentra en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="38170-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="38170-182">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="38170-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="38170-183">La ruta de acceso al *archivo. dll* es normalmente la carpeta *bin* en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="38170-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="38170-184">Este comando crea un archivo denominado *Server. js* en la misma carpeta que *signalr. exe*.</span><span class="sxs-lookup"><span data-stu-id="38170-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="38170-185">Coloque el archivo *Server. js* en una carpeta adecuada del proyecto, cambie el nombre según corresponda a la aplicación y agregue una referencia a él en lugar de la referencia "signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="38170-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="38170-186">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="38170-186">How to establish a connection</span></span>

<span data-ttu-id="38170-187">Antes de establecer una conexión, tiene que crear un objeto de conexión, crear un proxy y registrar controladores de eventos para los métodos a los que se puede llamar desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="38170-188">Cuando el proxy y los controladores de eventos estén configurados, establezca la conexión llamando al método `start`.</span><span class="sxs-lookup"><span data-stu-id="38170-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="38170-189">Si usa el proxy generado, no tiene que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.</span><span class="sxs-lookup"><span data-stu-id="38170-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="38170-190">**Establecer una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="38170-191">**Establecer una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="38170-192">El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr.</span><span class="sxs-lookup"><span data-stu-id="38170-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="38170-193">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="38170-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="38170-194">Normalmente se registran los controladores de eventos antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="38170-195">Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los controladores de eventos antes de llamar al método `start`.</span><span class="sxs-lookup"><span data-stu-id="38170-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="38170-196">Una razón para esto es que puede haber muchos concentradores en una aplicación, pero no querrá desencadenar el evento de `OnConnected` en cada concentrador si solo va a usar uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="38170-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="38170-197">Cuando se establece la conexión, la presencia de un método de cliente en el proxy de un concentrador es lo que indica a Signalr que desencadene el evento `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="38170-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="38170-198">Si no registra ningún controlador de eventos antes de llamar al método `start`, podrá invocar métodos en el concentrador, pero no se llamará al método `OnConnected` del concentrador y no se invocará ningún método de cliente desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="38170-199">$. Connection. Hub es el mismo objeto que $. hubConnection () crea</span><span class="sxs-lookup"><span data-stu-id="38170-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="38170-200">Como puede ver en los ejemplos, al utilizar el proxy generado, `$.connection.hub` hace referencia al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="38170-201">Este es el mismo objeto que se obtiene al llamar a `$.hubConnection()` cuando no se usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="38170-202">El código proxy generado crea la conexión mediante la ejecución de la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="38170-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Crear una conexión en el archivo de proxy generado](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="38170-204">Cuando use el proxy generado, puede hacer algo con `$.connection.hub` que puede hacer con un objeto de conexión cuando no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="38170-205">Ejecución asincrónica del método Start</span><span class="sxs-lookup"><span data-stu-id="38170-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="38170-206">El método `start` se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="38170-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="38170-207">Devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada llamando a métodos como `pipe`, `done`y `fail`.</span><span class="sxs-lookup"><span data-stu-id="38170-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="38170-208">Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, coloque ese código en una función de devolución de llamada o llámelo desde una función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="38170-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="38170-209">El método de devolución de llamada de `.done` se ejecuta una vez establecida la conexión y después de que se complete la ejecución de cualquier código que tenga en el método de control de eventos `OnConnected` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="38170-210">Si coloca la instrucción "ahora conectada" del ejemplo anterior como la siguiente línea de código después de la llamada al método `start` (no en una devolución de llamada `.done`), la línea de `console.log` se ejecutará antes de establecer la conexión, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="38170-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Manera incorrecta de escribir código que se ejecute una vez establecida la conexión](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="38170-212">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="38170-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="38170-213">Normalmente, si el explorador carga una página desde `http://contoso.com`, la conexión de Signalr está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="38170-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="38170-214">Si la página de `http://contoso.com` establece una conexión con `http://fabrikam.com/signalr`, es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="38170-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="38170-215">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="38170-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="38170-216">Para establecer una conexión entre dominios, asegúrese de que las conexiones entre dominios estén habilitadas en el servidor y especifique la dirección URL de conexión cuando cree el objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="38170-217">Signalr usará la tecnología adecuada para las conexiones entre dominios, como [JSONP](http://en.wikipedia.org/wiki/JSONP) o [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="38170-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="38170-218">En el servidor, habilite las conexiones entre dominios seleccionando esa opción al llamar al método `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="38170-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="38170-219">En el cliente, especifique la dirección URL cuando cree el objeto de conexión (sin el proxy generado) o antes de llamar al método Start (con el proxy generado).</span><span class="sxs-lookup"><span data-stu-id="38170-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="38170-220">**Código de cliente que especifica una conexión entre dominios (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="38170-221">**Código de cliente que especifica una conexión entre dominios (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="38170-222">Al usar el constructor `$.hubConnection`, no tiene que incluir `signalr` en la dirección URL porque se agrega automáticamente (a menos que especifique `useDefaultUrl` como `false`).</span><span class="sxs-lookup"><span data-stu-id="38170-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="38170-223">Puede crear varias conexiones a distintos puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="38170-224">No establezca `jQuery.support.cors` en true en el código.</span><span class="sxs-lookup"><span data-stu-id="38170-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![No establezca jQuery. support. CORS en true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="38170-226">Signalr controla el uso de JSONP o CORS.</span><span class="sxs-lookup"><span data-stu-id="38170-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="38170-227">Si se establece `jQuery.support.cors` en true, se deshabilita JSONP porque hace que Signalr asuma que el explorador es compatible con CORS.</span><span class="sxs-lookup"><span data-stu-id="38170-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="38170-228">Cuando se conecta a una dirección URL de localhost, Internet Explorer 10 no la considerará una conexión entre dominios, por lo que la aplicación funcionará localmente con IE 10 aunque no haya habilitado las conexiones entre dominios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="38170-229">Para obtener información sobre el uso de conexiones entre dominios con Internet Explorer 9, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="38170-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="38170-230">Para obtener información sobre el uso de conexiones entre dominios con Chrome, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="38170-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="38170-231">El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr.</span><span class="sxs-lookup"><span data-stu-id="38170-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="38170-232">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="38170-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="38170-233">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="38170-233">How to configure the connection</span></span>

<span data-ttu-id="38170-234">Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el método de transporte.</span><span class="sxs-lookup"><span data-stu-id="38170-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="38170-235">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="38170-235">How to specify query string parameters</span></span>

<span data-ttu-id="38170-236">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="38170-237">En los siguientes ejemplos se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="38170-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="38170-238">**Establecer un valor de cadena de consulta antes de llamar al método Start (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="38170-239">**Establecer un valor de cadena de consulta antes de llamar al método Start (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="38170-240">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="38170-241">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="38170-241">How to specify the transport method</span></span>

<span data-ttu-id="38170-242">Como parte del proceso de conexión, un cliente de Signalr normalmente negocia con el servidor para determinar el mejor transporte compatible con el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="38170-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="38170-243">Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación especificando el método de transporte al llamar al método `start`.</span><span class="sxs-lookup"><span data-stu-id="38170-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="38170-244">**Código de cliente que especifica el método de transporte (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="38170-245">**Código de cliente que especifica el método de transporte (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="38170-246">Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea que Signalr los pruebe:</span><span class="sxs-lookup"><span data-stu-id="38170-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="38170-247">**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="38170-248">**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="38170-249">Puede usar los siguientes valores para especificar el método de transporte:</span><span class="sxs-lookup"><span data-stu-id="38170-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="38170-250">WebSockets</span><span class="sxs-lookup"><span data-stu-id="38170-250">"webSockets"</span></span>
- <span data-ttu-id="38170-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="38170-251">"foreverFrame"</span></span>
- <span data-ttu-id="38170-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="38170-252">"serverSentEvents"</span></span>
- <span data-ttu-id="38170-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="38170-253">"longPolling"</span></span>

<span data-ttu-id="38170-254">En los siguientes ejemplos se muestra cómo averiguar qué método de transporte está utilizando una conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="38170-255">**Código de cliente que muestra el método de transporte utilizado por una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="38170-256">**Código de cliente que muestra el método de transporte utilizado por una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="38170-257">Para obtener información sobre cómo comprobar el método de transporte en el código del servidor, consulte [ASP.net signalr hubs API Guide-Server: How to get Information about the Client from the Context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="38170-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="38170-258">Para obtener más información sobre los transportes y los respaldos, vea [Introducción a signalr-transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="38170-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="38170-259">Cómo obtener un proxy para una clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="38170-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="38170-260">Cada objeto de conexión que se crea encapsula información sobre una conexión a un servicio de Signalr que contiene una o más clases de concentrador.</span><span class="sxs-lookup"><span data-stu-id="38170-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="38170-261">Para comunicarse con una clase de concentrador, se usa un objeto proxy que se crea (si no se usa el proxy generado) o que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="38170-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="38170-262">En el cliente, el nombre del proxy es una versión con mayúsculas y minúsculas Camel del nombre de la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="38170-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="38170-263">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="38170-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="38170-264">**Clase hub en el servidor**</span><span class="sxs-lookup"><span data-stu-id="38170-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="38170-265">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="38170-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="38170-266">**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="38170-267">Si decora la clase hub con un atributo `HubName`, use el nombre exacto sin cambiar mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="38170-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="38170-268">**Clase hub en el servidor con el atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="38170-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="38170-269">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="38170-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="38170-270">**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="38170-271">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="38170-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="38170-272">Para definir un método al que el servidor pueda llamar desde un concentrador, agregue un controlador de eventos al proxy del concentrador mediante la propiedad `client` del proxy generado o llame al método `on` si no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="38170-273">Los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="38170-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="38170-274">Agregue el controlador de eventos antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="38170-275">(Si desea agregar controladores de eventos después de llamar al método `start`, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y use la sintaxis que se muestra para definir un método sin usar el proxy generado).</span><span class="sxs-lookup"><span data-stu-id="38170-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="38170-276">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="38170-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="38170-277">Por ejemplo, `Clients.All.addContosoChatMessageToPage` en el servidor ejecutará `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="38170-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="38170-278">**Define el método en el cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="38170-279">**Manera alternativa de definir el método en el cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="38170-280">**Define el método en el cliente (sin el proxy generado o al agregar después de llamar al método Start)**</span><span class="sxs-lookup"><span data-stu-id="38170-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="38170-281">**Código de servidor que llama al método de cliente**</span><span class="sxs-lookup"><span data-stu-id="38170-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="38170-282">En los ejemplos siguientes se incluye un objeto complejo como parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="38170-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="38170-283">**Define el método en el cliente que toma un objeto complejo (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="38170-284">**Define el método en el cliente que toma un objeto complejo (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="38170-285">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="38170-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="38170-286">**Código de servidor que llama al método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="38170-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="38170-287">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="38170-287">How to call server methods from the client</span></span>

<span data-ttu-id="38170-288">Para llamar a un método de servidor desde el cliente, use la propiedad `server` del proxy generado o el método `invoke` en el proxy de concentrador si no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="38170-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="38170-289">El valor devuelto o los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="38170-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="38170-290">Pase una versión en mayúsculas y minúsculas Camel del nombre del método en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="38170-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="38170-291">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="38170-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="38170-292">En los siguientes ejemplos se muestra cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="38170-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="38170-293">**Método de servidor sin atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="38170-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="38170-294">**Código de servidor que define el objeto complejo pasado en un parámetro**</span><span class="sxs-lookup"><span data-stu-id="38170-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="38170-295">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="38170-296">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="38170-297">Si decora el método de concentrador con un atributo `HubMethodName`, use ese nombre sin cambiar mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="38170-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="38170-298">**Método de servidor** con un atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="38170-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="38170-299">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="38170-300">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="38170-301">En los ejemplos anteriores se muestra cómo llamar a un método de servidor que no tiene ningún valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="38170-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="38170-302">En los siguientes ejemplos se muestra cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="38170-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="38170-303">**Código de servidor para un método que tiene un valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="38170-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="38170-304">**La clase stock utilizada para el** valor devuelto</span><span class="sxs-lookup"><span data-stu-id="38170-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="38170-305">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="38170-306">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="38170-307">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="38170-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="38170-308">Signalr proporciona los siguientes eventos de duración de conexión que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="38170-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="38170-309">`starting`: se genera antes de que se envíen datos a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="38170-310">`received`: se genera cuando se reciben datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="38170-311">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="38170-311">Provides the received data.</span></span>
- <span data-ttu-id="38170-312">`connectionSlow`: se genera cuando el cliente detecta una conexión de eliminación lenta o con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="38170-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="38170-313">`reconnecting`: se genera cuando se inicia la reconexión del transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="38170-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="38170-314">`reconnected`: se genera cuando se vuelve a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="38170-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="38170-315">`stateChanged`: se genera cuando cambia el estado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="38170-316">Proporciona el estado anterior y el nuevo estado (conexión, conexión, reconexión o desconexión).</span><span class="sxs-lookup"><span data-stu-id="38170-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="38170-317">`disconnected`: se genera cuando se desconecta la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="38170-318">Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden provocar retrasos perceptibles, controle el evento `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="38170-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="38170-319">**Controlar el evento connectionSlow (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="38170-320">**Controlar el evento connectionSlow (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="38170-321">Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="38170-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="38170-322">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="38170-322">How to handle errors</span></span>

<span data-ttu-id="38170-323">El cliente de Signalr JavaScript proporciona un evento `error` al que se puede Agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="38170-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="38170-324">También puede utilizar el método FAIL para agregar un controlador para los errores resultantes de una invocación de método de servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="38170-325">Si no se habilitan explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que Signalr devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="38170-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="38170-326">Por ejemplo, si se produce un error en una llamada a `newContosoChatMessage`, el mensaje de error del objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`". no se recomienda el envío de mensajes de error detallados a los clientes en producción por motivos de seguridad, pero si desea habilitar mensajes de error detallados para la solución de problemas, use el siguiente código en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38170-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="38170-327">En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.</span><span class="sxs-lookup"><span data-stu-id="38170-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="38170-328">**Agregar un controlador de errores (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="38170-329">**Agregar un controlador de errores (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="38170-330">En el ejemplo siguiente se muestra cómo controlar un error desde una invocación de método.</span><span class="sxs-lookup"><span data-stu-id="38170-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="38170-331">**Controlar un error desde una invocación de método (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="38170-332">**Controlar un error desde una invocación de método (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="38170-333">Si se produce un error en la invocación de un método, también se produce el evento `error`, por lo que se ejecutaría el código en el controlador del método `error` y en la devolución de llamada del método `.fail`.</span><span class="sxs-lookup"><span data-stu-id="38170-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="38170-334">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="38170-334">How to enable client-side logging</span></span>

<span data-ttu-id="38170-335">Para habilitar el registro del lado cliente en una conexión, establezca la propiedad `logging` en el objeto de conexión antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="38170-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="38170-336">**Habilitar el registro (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="38170-337">**Habilitar el registro (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="38170-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="38170-338">Para ver los registros, abra las herramientas de desarrollo del explorador y vaya a la pestaña consola. Para ver un tutorial en el que se muestran instrucciones paso a paso y capturas de pantalla que muestran cómo hacerlo, consulte [difusión del servidor con ASP.net signalr-habilitar el registro](index.md).</span><span class="sxs-lookup"><span data-stu-id="38170-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
