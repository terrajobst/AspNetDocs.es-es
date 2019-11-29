---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Crear un control numérico de arriba y abajo con un back-endC#de servicio Web () | Microsoft Docs
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría demostrar como más c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598905"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Crear un control NumericUpDown con un back-end de servicio web (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo. De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.

## <a name="overview"></a>Información general del

En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo. De forma predeterminada, el control `NumericUpDown` siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.

## <a name="steps"></a>Pasos

El kit de herramientas de control de ASP.NET AJAX contiene el extensor `NumericUpDown`, que agrega automáticamente dos botones a un cuadro de texto: uno para aumentar su valor, uno para reducirlo. Sin embargo, el control también admite una llamada de servicio Web (o una llamada al método de página). Cada vez que se hace clic en el botón arriba o abajo, el código JavaScript se conecta al servidor Web y ejecuta un método allí. La firma del método es la siguiente:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

El `current` argumento es el valor actual del cuadro de texto; el atributo `tag` es datos de contexto adicionales que se pueden establecer como una propiedad del extensor `NumericUpDown` (pero no es necesario).

En este ejemplo, el control numérico de arriba y abajo solo permitirá valores que sean potencias de dos: 1, 2, 4, 8, 16, 32, 64, etc. Por lo tanto, el método ejecutado cuando el usuario desea aumentar el valor debe doblar el valor anterior; el otro método debe dividir el valor por dos. Este es el servicio web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Por último, cree una nueva página de ASP.NET. Como de costumbre, necesita un control `ScriptManager`, un control `TextBox` y un control `NumericUpDownExtender`. En este último caso, tendrá que proporcionar la información del servicio Web:

- `ServiceDownMethod` nombre del método Web o del método de página inactivo
- `ServiceDownPath` ruta de acceso al servicio Web con el método de servicio inactivo; omitir si se usa un método de página
- `ServiceUpMethod` nombre del método Web o del método de página up
- `ServiceUpPath` ruta de acceso al servicio Web con el método de servicio up; omitir si se usa un método de página

Este es el marcado completo de la página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Si ejecuta la página, observe que el valor del cuadro de texto siempre se duplica al hacer clic en el botón superior y se reduce al hacer clic en el botón inferior.

[![solo aparecen los números que son una potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Solo aparecen los números que son una potencia de 2 ([haga clic para ver la imagen de tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
