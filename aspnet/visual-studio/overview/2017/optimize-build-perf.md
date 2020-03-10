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
# <a name="optimize-build-performance-for-solution"></a>Optimización del rendimiento de compilación de una solución

Visual Studio 2017 15,8 o posterior incluyen un elemento de menú: **compilación** > **compilación de ASP.net** > optimizar el **rendimiento de la compilación para la solución**.

![Captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que un proyecto de ASP.NET incluye una copia del compilador. Sin embargo, en un equipo de desarrollador cuando la copia del compilador no coincide con la copia de Visual Studio, el rendimiento de la compilación se ve afectado en el orden de 1-3 segundos por cada compilación incremental. Esta característica actualiza la copia del compilador del proyecto para que coincida con la de Visual Studio, lo que normalmente acelera las compilaciones incrementales.

**Esto solo es aplicable a proyectos de ASP.NET Framework 4.7.1 o versiones posteriores, no se aplica a ASP.NET Core.**
