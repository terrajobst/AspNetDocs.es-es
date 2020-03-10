---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: 'Guía de la API de ASP.NET Signalr hubs: cliente JavaScript | Microsoft Docs'
author: bradygaster
description: En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de JavaScript, como los exploradores y la aplicación de la tienda Windows (WinJS)...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 8befe133c3627dac1f7d011959c68e2054d345da
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431293"
---
# <a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="3b303-103">Guía de la API de ASP.NET Signalr hubs: cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b303-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="3b303-104">En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de JavaScript, como aplicaciones de la tienda Windows (WinJS) y exploradores.</span><span class="sxs-lookup"><span data-stu-id="3b303-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="3b303-105">Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="3b303-106">En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3b303-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="3b303-107">En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="3b303-108">Signalr se encarga de todas las conexiones de cliente a servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="3b303-109">Signalr también ofrece una API de nivel inferior denominada conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="3b303-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="3b303-110">Para ver una introducción a Signalr, hubs y conexiones persistentes, consulte [Introducción a signalr](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3b303-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3b303-111">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="3b303-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="3b303-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3b303-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="3b303-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3b303-113">.NET 4.5</span></span>
> - <span data-ttu-id="3b303-114">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="3b303-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3b303-115">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="3b303-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="3b303-116">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3b303-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3b303-117">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="3b303-117">Questions and comments</span></span>
>
> <span data-ttu-id="3b303-118">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="3b303-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3b303-119">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3b303-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="3b303-120">Información general</span><span class="sxs-lookup"><span data-stu-id="3b303-120">Overview</span></span>

<span data-ttu-id="3b303-121">Este documento contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="3b303-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="3b303-122">El proxy generado y lo que hace para usted</span><span class="sxs-lookup"><span data-stu-id="3b303-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="3b303-123">Cuándo usar el proxy generado</span><span class="sxs-lookup"><span data-stu-id="3b303-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="3b303-124">Configuración de cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="3b303-125">Cómo hacer referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="3b303-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="3b303-126">Cómo crear un archivo físico para el proxy generado por Signalr</span><span class="sxs-lookup"><span data-stu-id="3b303-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="3b303-127">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="3b303-128">$. Connection. Hub es el mismo objeto que $. hubConnection () crea</span><span class="sxs-lookup"><span data-stu-id="3b303-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="3b303-129">Ejecución asincrónica del método Start</span><span class="sxs-lookup"><span data-stu-id="3b303-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="3b303-130">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="3b303-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="3b303-131">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="3b303-132">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="3b303-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="3b303-133">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="3b303-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="3b303-134">Cómo obtener un proxy para una clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="3b303-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="3b303-135">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="3b303-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="3b303-136">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="3b303-137">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="3b303-138">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="3b303-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="3b303-139">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="3b303-140">Para obtener documentación sobre cómo programar el servidor o los clientes de .NET, vea los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="3b303-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="3b303-141">Guía de API de signalr hubs-servidor</span><span class="sxs-lookup"><span data-stu-id="3b303-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="3b303-142">Guía de la API de signalr hubs: cliente .NET</span><span class="sxs-lookup"><span data-stu-id="3b303-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="3b303-143">El componente Signalr 2 Server solo está disponible en .NET 4,5 (aunque hay un cliente .NET para Signalr 2 en .NET 4,0).</span><span class="sxs-lookup"><span data-stu-id="3b303-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="3b303-144">El proxy generado y lo que hace para usted</span><span class="sxs-lookup"><span data-stu-id="3b303-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="3b303-145">Puede programar un cliente de JavaScript para que se comunique con un servicio de Signalr con o sin un proxy que Signalr genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3b303-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="3b303-146">Lo que hace el proxy es simplificar la sintaxis del código que se utiliza para conectarse, escribir métodos a los que llama el servidor y llamar a métodos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="3b303-147">Al escribir código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece que estaba ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="3b303-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="3b303-148">La sintaxis de proxy generada también permite un error de lado cliente inmediato e inteligible si se escribe un nombre de método de servidor invariable.</span><span class="sxs-lookup"><span data-stu-id="3b303-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="3b303-149">Y si crea manualmente el archivo que define los proxies, también puede obtener compatibilidad con IntelliSense para escribir código que llame a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="3b303-150">Por ejemplo, supongamos que tiene la siguiente clase de concentrador en el servidor:</span><span class="sxs-lookup"><span data-stu-id="3b303-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="3b303-151">En los siguientes ejemplos de código se muestra el aspecto del código JavaScript para invocar el método `NewContosoChatMessage` en el servidor y recibir invocaciones del método `addContosoChatMessageToPage` desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="3b303-152">**Con el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="3b303-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="3b303-153">**Sin el proxy generado**</span><span class="sxs-lookup"><span data-stu-id="3b303-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="3b303-154">Cuándo usar el proxy generado</span><span class="sxs-lookup"><span data-stu-id="3b303-154">When to use the generated proxy</span></span>

<span data-ttu-id="3b303-155">Si desea registrar varios controladores de eventos para un método de cliente al que el servidor llama, no puede usar el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="3b303-156">De lo contrario, puede elegir usar el proxy generado o no según sus preferencias de codificación.</span><span class="sxs-lookup"><span data-stu-id="3b303-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="3b303-157">Si decide no utilizarlo, no es necesario que haga referencia a la dirección URL de "signalr/hubs" en un elemento de `script` en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="3b303-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="3b303-158">Configuración del cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-158">Client setup</span></span>

<span data-ttu-id="3b303-159">Un cliente de JavaScript requiere referencias a jQuery y al archivo JavaScript de Signalr Core.</span><span class="sxs-lookup"><span data-stu-id="3b303-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="3b303-160">La versión de jQuery debe ser 1.6.4 o versiones posteriores, como 1.7.2, 1.8.2 o 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="3b303-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="3b303-161">Si decide usar el proxy generado, también necesitará una referencia al archivo JavaScript del proxy generado por Signalr.</span><span class="sxs-lookup"><span data-stu-id="3b303-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="3b303-162">En el ejemplo siguiente se muestra el aspecto de las referencias en una página HTML que usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="3b303-163">Estas referencias se deben incluir en este orden: primero jQuery, Signalr Core después de eso y los proxies de Signalr en último lugar.</span><span class="sxs-lookup"><span data-stu-id="3b303-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="3b303-164">Cómo hacer referencia al proxy generado dinámicamente</span><span class="sxs-lookup"><span data-stu-id="3b303-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="3b303-165">En el ejemplo anterior, la referencia al proxy generado por Signalr es el código de JavaScript generado dinámicamente, no un archivo físico.</span><span class="sxs-lookup"><span data-stu-id="3b303-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="3b303-166">Signalr crea el código JavaScript del proxy sobre la marcha y lo atiende al cliente en respuesta a la dirección URL "/signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="3b303-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="3b303-167">Si especificó una dirección URL base diferente para las conexiones de Signalr en el servidor en el método de `MapSignalR`, la dirección URL del archivo de proxy generado dinámicamente es su dirección URL personalizada con "/hubs" anexado.</span><span class="sxs-lookup"><span data-stu-id="3b303-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="3b303-168">En el caso de los clientes JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar de uno generado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="3b303-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="3b303-169">Para obtener más información, vea [Cómo crear un archivo físico para el proxy generado por signalr](#manualproxy) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="3b303-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>

<span data-ttu-id="3b303-170">En una vista de Razor de ASP.NET MVC 4 o 5, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo proxy:</span><span class="sxs-lookup"><span data-stu-id="3b303-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="3b303-171">Para obtener más información sobre el uso de Signalr en MVC 5, vea [Introducción con signalr y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="3b303-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="3b303-172">En una vista de Razor de ASP.NET MVC 3, use `Url.Content` para la referencia del archivo proxy:</span><span class="sxs-lookup"><span data-stu-id="3b303-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="3b303-173">En una aplicación de formularios Web Forms ASP.NET, use `ResolveClientUrl` para la referencia de archivo de servidores proxy o regístrela mediante el ScriptManager mediante la ruta de acceso relativa de la raíz de la aplicación (empezando por una tilde):</span><span class="sxs-lookup"><span data-stu-id="3b303-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="3b303-174">Como norma general, use el mismo método para especificar la dirección URL "/signalr/hubs" que se usa para los archivos CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b303-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="3b303-175">Si especifica una dirección URL sin usar una tilde, en algunos escenarios la aplicación funcionará correctamente al realizar la prueba en Visual Studio con IIS Express pero se producirá un error 404 al implementar en el IIS completo.</span><span class="sxs-lookup"><span data-stu-id="3b303-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="3b303-176">Para obtener más información, vea **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web de ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio de MSDN.</span><span class="sxs-lookup"><span data-stu-id="3b303-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="3b303-177">Al ejecutar un proyecto web en Visual Studio 2017 en modo de depuración, y si usa Internet Explorer como explorador, puede ver el archivo de proxy en **Explorador de soluciones** en **scripts**.</span><span class="sxs-lookup"><span data-stu-id="3b303-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="3b303-178">Para ver el contenido del archivo, haga doble clic en **hubs**.</span><span class="sxs-lookup"><span data-stu-id="3b303-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="3b303-179">Si no usa Visual Studio 2012 o 2013 e Internet Explorer, o si no está en modo de depuración, también puede obtener el contenido del archivo si va a la dirección URL "/signalR/hubs".</span><span class="sxs-lookup"><span data-stu-id="3b303-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="3b303-180">Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.</span><span class="sxs-lookup"><span data-stu-id="3b303-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="3b303-181">Cómo crear un archivo físico para el proxy generado por Signalr</span><span class="sxs-lookup"><span data-stu-id="3b303-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="3b303-182">Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código proxy y hacer referencia a ese archivo.</span><span class="sxs-lookup"><span data-stu-id="3b303-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="3b303-183">Tal vez desee hacerlo para controlar el comportamiento de almacenamiento en caché o de agrupación, o para obtener IntelliSense cuando se codifican llamadas a métodos de servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="3b303-184">Para crear un archivo proxy, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3b303-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="3b303-185">Instale el paquete NuGet [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .</span><span class="sxs-lookup"><span data-stu-id="3b303-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="3b303-186">Abra un símbolo del sistema y vaya a la carpeta *Tools* que contiene el archivo signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="3b303-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="3b303-187">La carpeta Tools se encuentra en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="3b303-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="3b303-188">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b303-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="3b303-189">La ruta de acceso al *archivo. dll* es normalmente la carpeta *bin* en la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="3b303-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="3b303-190">Este comando crea un archivo denominado *Server. js* en la misma carpeta que *signalr. exe*.</span><span class="sxs-lookup"><span data-stu-id="3b303-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="3b303-191">Coloque el archivo *Server. js* en una carpeta adecuada del proyecto, cambie el nombre según corresponda a la aplicación y agregue una referencia a él en lugar de la referencia "signalr/hubs".</span><span class="sxs-lookup"><span data-stu-id="3b303-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="3b303-192">Cómo establecer una conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-192">How to establish a connection</span></span>

<span data-ttu-id="3b303-193">Antes de establecer una conexión, tiene que crear un objeto de conexión, crear un proxy y registrar controladores de eventos para los métodos a los que se puede llamar desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="3b303-194">Cuando el proxy y los controladores de eventos estén configurados, establezca la conexión llamando al método `start`.</span><span class="sxs-lookup"><span data-stu-id="3b303-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="3b303-195">Si usa el proxy generado, no tiene que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.</span><span class="sxs-lookup"><span data-stu-id="3b303-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="3b303-196">**Establecer una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="3b303-197">**Establecer una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="3b303-198">El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr.</span><span class="sxs-lookup"><span data-stu-id="3b303-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="3b303-199">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="3b303-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="3b303-200">De forma predeterminada, la ubicación del concentrador es el servidor actual; Si se va a conectar a un servidor diferente, especifique la dirección URL antes de llamar al método `start`, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b303-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="3b303-201">Normalmente se registran los controladores de eventos antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="3b303-202">Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los controladores de eventos antes de llamar al método `start`.</span><span class="sxs-lookup"><span data-stu-id="3b303-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="3b303-203">Una razón para esto es que puede haber muchos concentradores en una aplicación, pero no querrá desencadenar el evento de `OnConnected` en cada concentrador si solo va a usar uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="3b303-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="3b303-204">Cuando se establece la conexión, la presencia de un método de cliente en el proxy de un concentrador es lo que indica a Signalr que desencadene el evento `OnConnected`.</span><span class="sxs-lookup"><span data-stu-id="3b303-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="3b303-205">Si no registra ningún controlador de eventos antes de llamar al método `start`, podrá invocar métodos en el concentrador, pero no se llamará al método `OnConnected` del concentrador y no se invocará ningún método de cliente desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="3b303-206">$. Connection. Hub es el mismo objeto que $. hubConnection () crea</span><span class="sxs-lookup"><span data-stu-id="3b303-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="3b303-207">Como puede ver en los ejemplos, al utilizar el proxy generado, `$.connection.hub` hace referencia al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="3b303-208">Este es el mismo objeto que se obtiene al llamar a `$.hubConnection()` cuando no se usa el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="3b303-209">El código proxy generado crea la conexión mediante la ejecución de la siguiente instrucción:</span><span class="sxs-lookup"><span data-stu-id="3b303-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Crear una conexión en el archivo de proxy generado](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="3b303-211">Cuando use el proxy generado, puede hacer algo con `$.connection.hub` que puede hacer con un objeto de conexión cuando no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="3b303-212">Ejecución asincrónica del método Start</span><span class="sxs-lookup"><span data-stu-id="3b303-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="3b303-213">El método `start` se ejecuta de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3b303-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="3b303-214">Devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada llamando a métodos como `pipe`, `done`y `fail`.</span><span class="sxs-lookup"><span data-stu-id="3b303-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="3b303-215">Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, coloque ese código en una función de devolución de llamada o llámelo desde una función de devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="3b303-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="3b303-216">El método de devolución de llamada de `.done` se ejecuta una vez establecida la conexión y después de que se complete la ejecución de cualquier código que tenga en el método de control de eventos `OnConnected` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="3b303-217">Si coloca la instrucción "ahora conectada" del ejemplo anterior como la siguiente línea de código después de la llamada al método `start` (no en una devolución de llamada `.done`), la línea de `console.log` se ejecutará antes de establecer la conexión, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3b303-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Manera incorrecta de escribir código que se ejecute una vez establecida la conexión](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="3b303-219">Cómo establecer una conexión entre dominios</span><span class="sxs-lookup"><span data-stu-id="3b303-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="3b303-220">Normalmente, si el explorador carga una página desde `http://contoso.com`, la conexión de Signalr está en el mismo dominio, en `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="3b303-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="3b303-221">Si la página de `http://contoso.com` establece una conexión con `http://fabrikam.com/signalr`, es una conexión entre dominios.</span><span class="sxs-lookup"><span data-stu-id="3b303-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="3b303-222">Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3b303-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="3b303-223">En Signalr 1. x, las solicitudes entre dominios se controlaban mediante una sola marca EnableCrossDomain.</span><span class="sxs-lookup"><span data-stu-id="3b303-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="3b303-224">Esta marca controla las solicitudes JSONP y CORS.</span><span class="sxs-lookup"><span data-stu-id="3b303-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="3b303-225">Para mayor flexibilidad, se ha quitado toda la compatibilidad con CORS del componente de servidor de Signalr (los clientes de JavaScript todavía usan CORS normalmente si se detecta que el explorador lo admite) y se ha puesto a disposición un nuevo middleware OWIN para admitir estos escenarios.</span><span class="sxs-lookup"><span data-stu-id="3b303-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="3b303-226">Si se requiere JSONP en el cliente (para admitir solicitudes entre dominios en exploradores anteriores), deberá habilitarse explícitamente estableciendo `EnableJSONP` en el objeto `HubConfiguration` en `true`, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3b303-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="3b303-227">JSONP está deshabilitado de forma predeterminada, ya que es menos seguro que CORS.</span><span class="sxs-lookup"><span data-stu-id="3b303-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="3b303-228">**Agregando Microsoft. Owin. CORS al proyecto:** Para instalar esta biblioteca, ejecute el siguiente comando en la consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="3b303-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="3b303-229">Este comando agregará la versión de 2.1.0 del paquete al proyecto.</span><span class="sxs-lookup"><span data-stu-id="3b303-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="3b303-230">Llamando a UseCors</span><span class="sxs-lookup"><span data-stu-id="3b303-230">Calling UseCors</span></span>

 <span data-ttu-id="3b303-231">En el fragmento de código siguiente se muestra cómo implementar conexiones entre dominios en Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="3b303-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="3b303-232">**Implementación de solicitudes entre dominios en Signalr 2**</span><span class="sxs-lookup"><span data-stu-id="3b303-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="3b303-233">En el código siguiente se muestra cómo habilitar CORS o JSONP en un proyecto Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="3b303-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="3b303-234">En este ejemplo de código se usa `Map` y `RunSignalR` en lugar de `MapSignalR`, de modo que el middleware CORS solo se ejecute para las solicitudes Signalr que requieran compatibilidad con CORS (en lugar de para todo el tráfico de la ruta de acceso especificada en `MapSignalR`). Map también se puede usar para cualquier otro middleware que deba ejecutarse para un prefijo de dirección URL específico, en lugar de para toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3b303-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="3b303-235">No establezca `jQuery.support.cors` en true en el código.</span><span class="sxs-lookup"><span data-stu-id="3b303-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![No establezca jQuery. support. CORS en true](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="3b303-237">Signalr controla el uso de CORS.</span><span class="sxs-lookup"><span data-stu-id="3b303-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="3b303-238">Si se establece `jQuery.support.cors` en true, se deshabilita JSONP porque hace que Signalr asuma que el explorador es compatible con CORS.</span><span class="sxs-lookup"><span data-stu-id="3b303-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="3b303-239">Cuando se conecta a una dirección URL de localhost, Internet Explorer 10 no la considerará una conexión entre dominios, por lo que la aplicación funcionará localmente con IE 10 aunque no haya habilitado las conexiones entre dominios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="3b303-240">Para obtener información sobre el uso de conexiones entre dominios con Internet Explorer 9, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="3b303-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="3b303-241">Para obtener información sobre el uso de conexiones entre dominios con Chrome, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="3b303-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="3b303-242">El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr.</span><span class="sxs-lookup"><span data-stu-id="3b303-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="3b303-243">Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="3b303-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="3b303-244">Cómo configurar la conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-244">How to configure the connection</span></span>

<span data-ttu-id="3b303-245">Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el método de transporte.</span><span class="sxs-lookup"><span data-stu-id="3b303-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="3b303-246">Cómo especificar parámetros de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="3b303-246">How to specify query string parameters</span></span>

<span data-ttu-id="3b303-247">Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="3b303-248">En los siguientes ejemplos se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="3b303-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="3b303-249">**Establecer un valor de cadena de consulta antes de llamar al método Start (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="3b303-250">**Establecer un valor de cadena de consulta antes de llamar al método Start (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="3b303-251">En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="3b303-252">Cómo especificar el método de transporte</span><span class="sxs-lookup"><span data-stu-id="3b303-252">How to specify the transport method</span></span>

<span data-ttu-id="3b303-253">Como parte del proceso de conexión, un cliente de Signalr normalmente negocia con el servidor para determinar el mejor transporte compatible con el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="3b303-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="3b303-254">Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación especificando el método de transporte al llamar al método `start`.</span><span class="sxs-lookup"><span data-stu-id="3b303-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="3b303-255">**Código de cliente que especifica el método de transporte (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="3b303-256">**Código de cliente que especifica el método de transporte (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="3b303-257">Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea que Signalr los pruebe:</span><span class="sxs-lookup"><span data-stu-id="3b303-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="3b303-258">**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="3b303-259">**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="3b303-260">Puede usar los siguientes valores para especificar el método de transporte:</span><span class="sxs-lookup"><span data-stu-id="3b303-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="3b303-261">WebSockets</span><span class="sxs-lookup"><span data-stu-id="3b303-261">"webSockets"</span></span>
- <span data-ttu-id="3b303-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="3b303-262">"foreverFrame"</span></span>
- <span data-ttu-id="3b303-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="3b303-263">"serverSentEvents"</span></span>
- <span data-ttu-id="3b303-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="3b303-264">"longPolling"</span></span>

<span data-ttu-id="3b303-265">En los siguientes ejemplos se muestra cómo averiguar qué método de transporte está utilizando una conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="3b303-266">**Código de cliente que muestra el método de transporte utilizado por una conexión (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="3b303-267">**Código de cliente que muestra el método de transporte utilizado por una conexión (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="3b303-268">Para obtener información sobre cómo comprobar el método de transporte en el código del servidor, consulte [ASP.net signalr hubs API Guide-Server: How to get Information about the Client from the Context Property](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="3b303-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="3b303-269">Para obtener más información sobre los transportes y los respaldos, vea [Introducción a signalr-transportes y reservas](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="3b303-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="3b303-270">Cómo obtener un proxy para una clase de concentrador</span><span class="sxs-lookup"><span data-stu-id="3b303-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="3b303-271">Cada objeto de conexión que se crea encapsula información sobre una conexión a un servicio de Signalr que contiene una o más clases de concentrador.</span><span class="sxs-lookup"><span data-stu-id="3b303-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="3b303-272">Para comunicarse con una clase de concentrador, se usa un objeto proxy que se crea (si no se usa el proxy generado) o que se genera automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3b303-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="3b303-273">En el cliente, el nombre del proxy es una versión con mayúsculas y minúsculas Camel del nombre de la clase de concentrador.</span><span class="sxs-lookup"><span data-stu-id="3b303-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="3b303-274">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b303-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="3b303-275">**Clase hub en el servidor**</span><span class="sxs-lookup"><span data-stu-id="3b303-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="3b303-276">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="3b303-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="3b303-277">**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="3b303-278">Si decora la clase hub con un atributo `HubName`, use el nombre exacto sin cambiar mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3b303-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="3b303-279">**Clase hub en el servidor con el atributo HubName**</span><span class="sxs-lookup"><span data-stu-id="3b303-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="3b303-280">**Obtener una referencia al proxy de cliente generado para el concentrador**</span><span class="sxs-lookup"><span data-stu-id="3b303-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="3b303-281">**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="3b303-282">Cómo definir métodos en el cliente a los que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="3b303-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="3b303-283">Para definir un método al que el servidor pueda llamar desde un concentrador, agregue un controlador de eventos al proxy del concentrador mediante la propiedad `client` del proxy generado o llame al método `on` si no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="3b303-284">Los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="3b303-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="3b303-285">Agregue el controlador de eventos antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="3b303-286">(Si desea agregar controladores de eventos después de llamar al método `start`, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y use la sintaxis que se muestra para definir un método sin usar el proxy generado).</span><span class="sxs-lookup"><span data-stu-id="3b303-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="3b303-287">La coincidencia de nombres de método no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3b303-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="3b303-288">Por ejemplo, `Clients.All.addContosoChatMessageToPage` en el servidor ejecutará `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` en el cliente.</span><span class="sxs-lookup"><span data-stu-id="3b303-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="3b303-289">**Define el método en el cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="3b303-290">**Manera alternativa de definir el método en el cliente (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="3b303-291">**Define el método en el cliente (sin el proxy generado o al agregar después de llamar al método Start)**</span><span class="sxs-lookup"><span data-stu-id="3b303-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="3b303-292">**Código de servidor que llama al método de cliente**</span><span class="sxs-lookup"><span data-stu-id="3b303-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="3b303-293">En los ejemplos siguientes se incluye un objeto complejo como parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="3b303-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="3b303-294">**Define el método en el cliente que toma un objeto complejo (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="3b303-295">**Define el método en el cliente que toma un objeto complejo (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="3b303-296">**Código de servidor que define el objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="3b303-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="3b303-297">**Código de servidor que llama al método de cliente mediante un objeto complejo**</span><span class="sxs-lookup"><span data-stu-id="3b303-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="3b303-298">Cómo llamar a métodos de servidor desde el cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-298">How to call server methods from the client</span></span>

<span data-ttu-id="3b303-299">Para llamar a un método de servidor desde el cliente, use la propiedad `server` del proxy generado o el método `invoke` en el proxy de concentrador si no está utilizando el proxy generado.</span><span class="sxs-lookup"><span data-stu-id="3b303-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="3b303-300">El valor devuelto o los parámetros pueden ser objetos complejos.</span><span class="sxs-lookup"><span data-stu-id="3b303-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="3b303-301">Pase una versión en mayúsculas y minúsculas Camel del nombre del método en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="3b303-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="3b303-302">Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3b303-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="3b303-303">En los siguientes ejemplos se muestra cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="3b303-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="3b303-304">**Método de servidor sin atributo HubMethodName**</span><span class="sxs-lookup"><span data-stu-id="3b303-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="3b303-305">**Código de servidor que define el objeto complejo pasado en un parámetro**</span><span class="sxs-lookup"><span data-stu-id="3b303-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="3b303-306">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="3b303-307">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="3b303-308">Si decora el método de concentrador con un atributo `HubMethodName`, use ese nombre sin cambiar mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="3b303-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="3b303-309">**Método de servidor** con un atributo HubMethodName</span><span class="sxs-lookup"><span data-stu-id="3b303-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="3b303-310">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="3b303-311">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="3b303-312">En los ejemplos anteriores se muestra cómo llamar a un método de servidor que no tiene ningún valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="3b303-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="3b303-313">En los siguientes ejemplos se muestra cómo llamar a un método de servidor que tiene un valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="3b303-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="3b303-314">**Código de servidor para un método que tiene un valor devuelto**</span><span class="sxs-lookup"><span data-stu-id="3b303-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="3b303-315">**La clase stock utilizada para el** valor devuelto</span><span class="sxs-lookup"><span data-stu-id="3b303-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="3b303-316">**Código de cliente que invoca el método de servidor (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="3b303-317">**Código de cliente que invoca el método de servidor (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="3b303-318">Cómo controlar los eventos de duración de la conexión</span><span class="sxs-lookup"><span data-stu-id="3b303-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="3b303-319">Signalr proporciona los siguientes eventos de duración de conexión que puede controlar:</span><span class="sxs-lookup"><span data-stu-id="3b303-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="3b303-320">`starting`: se genera antes de que se envíen datos a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="3b303-321">`received`: se genera cuando se reciben datos en la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="3b303-322">Proporciona los datos recibidos.</span><span class="sxs-lookup"><span data-stu-id="3b303-322">Provides the received data.</span></span>
- <span data-ttu-id="3b303-323">`connectionSlow`: se genera cuando el cliente detecta una conexión de eliminación lenta o con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="3b303-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="3b303-324">`reconnecting`: se genera cuando se inicia la reconexión del transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="3b303-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="3b303-325">`reconnected`: se genera cuando se vuelve a conectar el transporte subyacente.</span><span class="sxs-lookup"><span data-stu-id="3b303-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="3b303-326">`stateChanged`: se genera cuando cambia el estado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="3b303-327">Proporciona el estado anterior y el nuevo estado (conexión, conexión, reconexión o desconexión).</span><span class="sxs-lookup"><span data-stu-id="3b303-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="3b303-328">`disconnected`: se genera cuando se desconecta la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="3b303-329">Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden provocar retrasos perceptibles, controle el evento `connectionSlow`.</span><span class="sxs-lookup"><span data-stu-id="3b303-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="3b303-330">**Controlar el evento connectionSlow (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="3b303-331">**Controlar el evento connectionSlow (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="3b303-332">Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="3b303-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="3b303-333">Cómo controlar errores</span><span class="sxs-lookup"><span data-stu-id="3b303-333">How to handle errors</span></span>

<span data-ttu-id="3b303-334">El cliente de Signalr JavaScript proporciona un evento `error` al que se puede Agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="3b303-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="3b303-335">También puede utilizar el método FAIL para agregar un controlador para los errores resultantes de una invocación de método de servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="3b303-336">Si no se habilitan explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que Signalr devuelve después de un error contiene información mínima sobre el error.</span><span class="sxs-lookup"><span data-stu-id="3b303-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="3b303-337">Por ejemplo, si se produce un error en una llamada a `newContosoChatMessage`, el mensaje de error del objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`". no se recomienda el envío de mensajes de error detallados a los clientes en producción por motivos de seguridad, pero si desea habilitar mensajes de error detallados para la solución de problemas, use el siguiente código en el servidor.</span><span class="sxs-lookup"><span data-stu-id="3b303-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="3b303-338">En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.</span><span class="sxs-lookup"><span data-stu-id="3b303-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="3b303-339">**Agregar un controlador de errores (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="3b303-340">**Agregar un controlador de errores (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="3b303-341">En el ejemplo siguiente se muestra cómo controlar un error desde una invocación de método.</span><span class="sxs-lookup"><span data-stu-id="3b303-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="3b303-342">**Controlar un error desde una invocación de método (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="3b303-343">**Controlar un error desde una invocación de método (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="3b303-344">Si se produce un error en la invocación de un método, también se produce el evento `error`, por lo que se ejecutaría el código en el controlador del método `error` y en la devolución de llamada del método `.fail`.</span><span class="sxs-lookup"><span data-stu-id="3b303-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="3b303-345">Cómo habilitar el registro del lado cliente</span><span class="sxs-lookup"><span data-stu-id="3b303-345">How to enable client-side logging</span></span>

<span data-ttu-id="3b303-346">Para habilitar el registro del lado cliente en una conexión, establezca la propiedad `logging` en el objeto de conexión antes de llamar al método `start` para establecer la conexión.</span><span class="sxs-lookup"><span data-stu-id="3b303-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="3b303-347">**Habilitar el registro (con el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="3b303-348">**Habilitar el registro (sin el proxy generado)**</span><span class="sxs-lookup"><span data-stu-id="3b303-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="3b303-349">Para ver los registros, abra las herramientas de desarrollo del explorador y vaya a la pestaña consola. Para ver un tutorial en el que se muestran instrucciones paso a paso y capturas de pantalla que muestran cómo hacerlo, consulte [difusión del servidor con ASP.net signalr-habilitar el registro](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="3b303-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
