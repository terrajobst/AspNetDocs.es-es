---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ejemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472351"
---
# <a name="katana-samples"></a><span data-ttu-id="a9096-102">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="a9096-102">Katana Samples</span></span>

<span data-ttu-id="a9096-103">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9096-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="a9096-104">Ejemplos de Katana</span><span class="sxs-lookup"><span data-stu-id="a9096-104">Katana Samples</span></span>

<span data-ttu-id="a9096-105">[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes) de | de **rutas de ASP.net de ejemplo**</span><span class="sxs-lookup"><span data-stu-id="a9096-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="a9096-106">En algunas aplicaciones, querrá enlazar componentes OWIN en la tabla de rutas Asp.Net en paralelo con los componentes no OWIN.</span><span class="sxs-lookup"><span data-stu-id="a9096-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="a9096-107">En este ejemplo se muestra cómo usar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionados por Microsoft. Owin. host. SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="a9096-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="a9096-108">**Ejemplo de canalizaciones de bifurcación** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="a9096-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="a9096-109">Las canalizaciones de procesamiento de solicitudes OWIN no necesitan ser lineales, sino que se pueden bifurcar para procesar solicitudes de diferentes maneras.</span><span class="sxs-lookup"><span data-stu-id="a9096-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="a9096-110">En este ejemplo se muestra cómo construir una canalización de bifurcación basada en rutas de acceso de solicitud u otros datos de solicitud, como encabezados.</span><span class="sxs-lookup"><span data-stu-id="a9096-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="a9096-111">Estos componentes están disponibles en el paquete Nuget Microsoft. Owin. Mapping.</span><span class="sxs-lookup"><span data-stu-id="a9096-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="a9096-112">**Ejemplo de servidor personalizado** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="a9096-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="a9096-113">Muestra cómo usar un servidor OWIN personalizado cuando se autohospeda OWIN.</span><span class="sxs-lookup"><span data-stu-id="a9096-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="a9096-114">[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de **ejemplo | incrustado**</span><span class="sxs-lookup"><span data-stu-id="a9096-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="a9096-115">Algunos servidores OWIN pueden ejecutarse dentro de su propio proceso (&quot;autohospedado&quot;).</span><span class="sxs-lookup"><span data-stu-id="a9096-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="a9096-116">En este ejemplo se muestra cómo iniciar una aplicación OWIN con las herramientas proporcionadas por el paquete Nuget Microsoft. Owin. Hosting.</span><span class="sxs-lookup"><span data-stu-id="a9096-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="a9096-117">**Ejemplo HelloWorld** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span><span class="sxs-lookup"><span data-stu-id="a9096-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="a9096-118">OWIN es una abstracción de API de servidor HTTP que permite la portabilidad de aplicaciones entre varios servidores.</span><span class="sxs-lookup"><span data-stu-id="a9096-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="a9096-119">En este ejemplo se muestra cómo escribir una aplicación Hola mundo mediante algunos **contenedores simples** en torno a la abstracción OWIN sin procesar y ejecutarla en un servidor web como ASP.net.</span><span class="sxs-lookup"><span data-stu-id="a9096-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="a9096-120">Hola mundo el [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) del **ejemplo OWIN sin procesar** | </span><span class="sxs-lookup"><span data-stu-id="a9096-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="a9096-121">En este ejemplo se muestra cómo escribir una aplicación Hola mundo con la abstracción OWIN **sin procesar** y ejecutarla en un servidor web como ASP.net.</span><span class="sxs-lookup"><span data-stu-id="a9096-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="a9096-122">[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de **ejemplo de signalr** | </span><span class="sxs-lookup"><span data-stu-id="a9096-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="a9096-123">Muestra cómo autohospedar Signalr mediante OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="a9096-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="a9096-124">Para obtener más información sobre Signalr autohospedado, consulte [Tutorial: Autohospedaje de signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="a9096-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="a9096-125">**Ejemplo de archivos estáticos** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="a9096-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="a9096-126">Muestra cómo admitir solicitudes HTTP para archivos estáticos mediante OWIN/Katana.</span><span class="sxs-lookup"><span data-stu-id="a9096-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="a9096-127">[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) de | de **API Web** </span><span class="sxs-lookup"><span data-stu-id="a9096-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="a9096-128">En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="a9096-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="a9096-129">**Ejemplo de socket Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="a9096-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="a9096-130">Muestra cómo admitir Sockets Web en OWIN mediante la clase [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="a9096-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
