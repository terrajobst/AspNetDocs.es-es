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
# <a name="aspnet-webhooks-debugging"></a>Depuración de webhooks de ASP.NET  

## <a name="debugging-in-azure"></a>Depuración en Azure

Para depurar la aplicación web mientras se ejecuta en Azure, consulte el tutorial [solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depurar con código fuente y símbolos

Además de depurar su propio código, es posible depurar directamente en Microsoft ASP.NET webhooks y, de hecho, en todo .NET. Esto funciona independientemente de si se depura de forma local o remota. En primer lugar, configure Visual Studio para buscar el código fuente y los símbolos; para ello, vaya a **depurar** y, a continuación, **Opciones y configuración**. Establezca las opciones de la siguiente manera:

![Opciones y configuración](_static/SourceSymbols.png)

Después, agregue un vínculo a [symbolsource.org](http://symbolsource.org) para descargar el código fuente y los símbolos. Vaya a la pestaña **símbolos** del menú anterior y agregue lo siguiente como una ubicación de símbolos:

```
http://srv.symbolsource.org/pdb/Public
```

Además, asegúrese de que el directorio de caché tenga un nombre corto; de lo contrario, los nombres de archivo pueden llegar a ser demasiado largos, lo que hará que no se carguen los símbolos. Una ruta de acceso de ejemplo es:

```
C:\SymCache
```

La configuración debe ser similar a la siguiente:

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
