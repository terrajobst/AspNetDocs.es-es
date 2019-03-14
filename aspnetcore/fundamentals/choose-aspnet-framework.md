---
title: Elección entre ASP.NET 4.x y ASP.NET Core
author: rick-anderson
description: Se explican las diferencias entre ASP.NET Core. y ASP.NET 4.x, y cómo elegir entre ellos.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: eb216bdac7dd029c3d985f2edd9e70eb91f42883
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040992"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="5f701-103">Elección entre ASP.NET 4.x y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="5f701-104">ASP.NET Core es un rediseño de ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="5f701-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="5f701-105">En este artículo se enumeran las diferencias entre ellos.</span><span class="sxs-lookup"><span data-stu-id="5f701-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="5f701-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-106">ASP.NET Core</span></span>

<span data-ttu-id="5f701-107">ASP.NET Core es un marco multiplataforma de código abierto que tiene como finalidad compilar modernas aplicaciones web basadas en la nube en Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="5f701-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="5f701-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="5f701-108">ASP.NET 4.x</span></span>

<span data-ttu-id="5f701-109">ASP.NET 4.x es un marco consolidado que proporciona los servicios necesarios para compilar aplicaciones web de nivel empresarial basadas en servidor en Windows.</span><span class="sxs-lookup"><span data-stu-id="5f701-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="5f701-110">Selección del marco</span><span class="sxs-lookup"><span data-stu-id="5f701-110">Framework selection</span></span>

<span data-ttu-id="5f701-111">En la tabla siguiente se compara ASP.NET Core en ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="5f701-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="5f701-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-112">ASP.NET Core</span></span> | <span data-ttu-id="5f701-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="5f701-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="5f701-114">Compilación para Windows, macOS o Linux</span><span class="sxs-lookup"><span data-stu-id="5f701-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="5f701-115">Compilación para Windows</span><span class="sxs-lookup"><span data-stu-id="5f701-115">Build for Windows</span></span>|
|<span data-ttu-id="5f701-116">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="5f701-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="5f701-117">Vea también [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api) y [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="5f701-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="5f701-118">Use [formularios Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/) o [Web Pages](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="5f701-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="5f701-119">Varias versiones por equipo</span><span class="sxs-lookup"><span data-stu-id="5f701-119">Multiple versions per machine</span></span>|<span data-ttu-id="5f701-120">Una versión por equipo</span><span class="sxs-lookup"><span data-stu-id="5f701-120">One version per machine</span></span>|
|<span data-ttu-id="5f701-121">Desarrollo con Visual Studio, [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/) o [Visual Studio Code](https://code.visualstudio.com/) con C# o F#</span><span class="sxs-lookup"><span data-stu-id="5f701-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="5f701-122">Desarrollo con Visual Studio con C#, VB o F#</span><span class="sxs-lookup"><span data-stu-id="5f701-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="5f701-123">Mayor rendimiento que ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="5f701-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="5f701-124">Buen rendimiento</span><span class="sxs-lookup"><span data-stu-id="5f701-124">Good performance</span></span>|
|[<span data-ttu-id="5f701-125">Elegir .NET Framework o .NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="5f701-126">Usar el tiempo de ejecución de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="5f701-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="5f701-127">Vea [ASP.NET Core con .NET Framework como destino](xref:index#target-framework) para obtener información sobre la compatibilidad de ASP.NET Core 2.x en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5f701-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="5f701-128">Escenarios de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="5f701-129">[Las páginas de Razor](xref:razor-pages/index) son el método recomendado para crear una interfaz de usuario web desde la aparición de ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="5f701-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="5f701-130">Sitios web</span><span class="sxs-lookup"><span data-stu-id="5f701-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="5f701-131">API</span><span class="sxs-lookup"><span data-stu-id="5f701-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="5f701-132">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="5f701-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="5f701-133">Implementación de una aplicación ASP.NET Core en Azure</span><span class="sxs-lookup"><span data-stu-id="5f701-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="5f701-134">Escenarios de ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="5f701-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="5f701-135">Sitios web</span><span class="sxs-lookup"><span data-stu-id="5f701-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="5f701-136">API</span><span class="sxs-lookup"><span data-stu-id="5f701-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="5f701-137">En tiempo real</span><span class="sxs-lookup"><span data-stu-id="5f701-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="5f701-138">Creación de una aplicación web ASP.NET 4.x en Azure</span><span class="sxs-lookup"><span data-stu-id="5f701-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="5f701-139">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5f701-139">Additional resources</span></span>

* [<span data-ttu-id="5f701-140">Introducción a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5f701-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="5f701-141">Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f701-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>
