---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Enlazar el control deslizante (VB) | Microsoft Docs
author: wenz
description: El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar el Positio actual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c14373bdfdead9916950b8a1cf61f427f7ba50b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508903"
---
# <a name="databinding-the-slider-control-vb"></a>Enlace de datos del control deslizante (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="overview"></a>Información general

El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

A continuación, agregue dos controles `TextBox` a la página. Uno se transformará en un control deslizante gráfico y el otro contendrá la posición del control deslizante.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

El paso siguiente ya es el paso final. El control de `SliderExtender` de ASP.NET AJAX control Toolkit hace que un control deslizante salga del primer cuadro de texto y actualiza automáticamente el segundo cuadro de texto cuando cambia la posición del control deslizante. Para que funcione, el atributo `TargetControlID` del `SliderExtender`debe establecerse en el identificador del primer cuadro de texto. el atributo `BoundControlID` debe establecerse en el identificador del segundo cuadro de texto.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: Si escribe un nuevo valor en el cuadro de texto, se actualiza la posición del control deslizante. Si hace que el segundo cuadro de texto sea de solo lectura, puede Agregar una protección débil al campo de texto para que sea más difícil que el usuario actualice manualmente el valor en él.

[![control deslizante y cuadro de texto están sincronizados](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

El control deslizante y el cuadro de texto están sincronizados ([haga clic para ver la imagen de tamaño completo](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-the-slider-control-with-auto-postback-vb.md)
