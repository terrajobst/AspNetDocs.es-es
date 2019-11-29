---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Crear un control numérico de arriba y abajo con un back-end de servicio Web (VB) | Microsoft Docs
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría demostrar como más c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: 2bf6e1b27180589d39e308de62b5be1f47fa8fe2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606364"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a>Crear un control NumericUpDown con un back-end de servicio web (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)

> En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo. De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.

## <a name="overview"></a>Información general del

En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo. De forma predeterminada, el control `NumericUpDown` siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.

## <a name="steps"></a>Pasos

El kit de herramientas de control de ASP.NET AJAX contiene el extensor `NumericUpDown`, que agrega automáticamente dos botones a un cuadro de texto: uno para aumentar su valor, uno para reducirlo. Sin embargo, el control también admite una llamada de servicio Web (o una llamada al método de página). Cada vez que se hace clic en el botón arriba o abajo, el código JavaScript se conecta al servidor Web y ejecuta un método allí. La firma del método es la siguiente:

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

El `current` argumento es el valor actual del cuadro de texto; el atributo `tag` es datos de contexto adicionales que se pueden establecer como una propiedad del extensor `NumericUpDown` (pero no es necesario).

En este ejemplo, el control numérico de arriba y abajo solo permitirá valores que sean potencias de dos: 1, 2, 4, 8, 16, 32, 64, etc. Por lo tanto, el método ejecutado cuando el usuario desea aumentar el valor debe doblar el valor anterior; el otro método debe dividir el valor por dos. Este es el servicio web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

Por último, cree una nueva página de ASP.NET. Como de costumbre, necesita un control `ScriptManager`, un control `TextBox` y un control `NumericUpDownExtender`. En este último caso, tendrá que proporcionar la información del servicio Web:

- `ServiceDownMethod` nombre del método Web o del método de página inactivo
- `ServiceDownPath` ruta de acceso al servicio Web con el método de servicio inactivo; omitir si se usa un método de página
- `ServiceUpMethod` nombre del método Web o del método de página up
- `ServiceUpPath` ruta de acceso al servicio Web con el método de servicio up; omitir si se usa un método de página

Este es el marcado completo de la página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

Si ejecuta la página, observe que el valor del cuadro de texto siempre se duplica al hacer clic en el botón superior y se reduce al hacer clic en el botón inferior.

[![solo aparecen los números que son una potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)

Solo aparecen los números que son una potencia de 2 ([haga clic para ver la imagen de tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
