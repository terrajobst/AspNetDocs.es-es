---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimización del rendimiento de compilación de una solución
author: AngelosP
description: Optimización del rendimiento de compilación de una solución
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504979"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="9f465-103">Optimización del rendimiento de compilación de una solución</span><span class="sxs-lookup"><span data-stu-id="9f465-103">Optimize build performance for solution</span></span>

<span data-ttu-id="9f465-104">Visual Studio 2017 15,8 o posterior incluyen un elemento de menú: **compilación** > **compilación de ASP.net** > optimizar el **rendimiento de la compilación para la solución**.</span><span class="sxs-lookup"><span data-stu-id="9f465-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="9f465-106">ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que un proyecto de ASP.NET incluye una copia del compilador.</span><span class="sxs-lookup"><span data-stu-id="9f465-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="9f465-107">Sin embargo, en un equipo de desarrollador cuando la copia del compilador no coincide con la copia de Visual Studio, el rendimiento de la compilación se ve afectado en el orden de 1-3 segundos por cada compilación incremental.</span><span class="sxs-lookup"><span data-stu-id="9f465-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="9f465-108">Esta característica actualiza la copia del compilador del proyecto para que coincida con la de Visual Studio, lo que normalmente acelera las compilaciones incrementales.</span><span class="sxs-lookup"><span data-stu-id="9f465-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="9f465-109">**Esto solo es aplicable a proyectos de ASP.NET Framework 4.7.1 o versiones posteriores, no se aplica a ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="9f465-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
