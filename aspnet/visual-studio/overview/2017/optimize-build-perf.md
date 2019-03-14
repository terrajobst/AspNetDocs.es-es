---
uid: visual-studio/overview/2017/optimize-build-perf
title: Optimización del rendimiento de compilación de una solución
author: AngelosP
description: Optimización del rendimiento de compilación de una solución
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050512"
---
# <a name="optimize-build-performance-for-solution"></a>Optimización del rendimiento de compilación de una solución

15,8 de Visual Studio 2017 o versiones posteriores incluyen un elemento de menú: **Compilar** > **compilación de ASP.NET** > **optimizar el rendimiento de la compilación para la solución**.

![captura de pantalla del nuevo elemento de menú](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compila sus vistas en tiempo de ejecución, lo que significa que un proyecto de ASP.NET conlleva una copia del compilador. Sin embargo en una máquina de desarrollo cuando la copia del compilador no coincide con la copia de Visual Studio, rendimiento de la compilación se ve afectado del orden de 1 a 3 segundos por compilación incremental. Esta característica actualiza la copia del proyecto del compilador para que coincida con Visual Studio, lo que normalmente se acelera las compilaciones incrementales.

**Esto es aplicable al marco de ASP.NET 4.7.1 o posterior solo para proyectos, no se aplica a ASP.NET Core.**
