---
uid: webhooks/diagnostics/debugging
title: Depuración de webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Cómo depurar webhooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520603"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="4154c-103">Depuración de webhooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4154c-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="4154c-104">Depuración en Azure</span><span class="sxs-lookup"><span data-stu-id="4154c-104">Debugging in Azure</span></span>

<span data-ttu-id="4154c-105">Para depurar la aplicación web mientras se ejecuta en Azure, consulte el tutorial [solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="4154c-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="4154c-106">Depurar con código fuente y símbolos</span><span class="sxs-lookup"><span data-stu-id="4154c-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="4154c-107">Además de depurar su propio código, es posible depurar directamente en Microsoft ASP.NET webhooks y, de hecho, en todo .NET.</span><span class="sxs-lookup"><span data-stu-id="4154c-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="4154c-108">Esto funciona independientemente de si se depura de forma local o remota.</span><span class="sxs-lookup"><span data-stu-id="4154c-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="4154c-109">En primer lugar, configure Visual Studio para buscar el código fuente y los símbolos; para ello, vaya a **depurar** y, a continuación, **Opciones y configuración**.</span><span class="sxs-lookup"><span data-stu-id="4154c-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="4154c-110">Establezca las opciones de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="4154c-110">Set the options like this:</span></span>

![Opciones y configuración](_static/SourceSymbols.png)

<span data-ttu-id="4154c-112">Después, agregue un vínculo a [symbolsource.org](http://symbolsource.org) para descargar el código fuente y los símbolos.</span><span class="sxs-lookup"><span data-stu-id="4154c-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="4154c-113">Vaya a la pestaña **símbolos** del menú anterior y agregue lo siguiente como una ubicación de símbolos:</span><span class="sxs-lookup"><span data-stu-id="4154c-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="4154c-114">Además, asegúrese de que el directorio de caché tenga un nombre corto; de lo contrario, los nombres de archivo pueden llegar a ser demasiado largos, lo que hará que no se carguen los símbolos.</span><span class="sxs-lookup"><span data-stu-id="4154c-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="4154c-115">Una ruta de acceso de ejemplo es:</span><span class="sxs-lookup"><span data-stu-id="4154c-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="4154c-116">La configuración debe ser similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="4154c-116">The settings should look similar to this:</span></span>

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
