---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Enlazar el control deslizanteC#() | Microsoft Docs
author: wenz
description: El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar el Positio actual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598608"
---
# <a name="databinding-the-slider-control-c"></a>Enlace de datos del control deslizante (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="overview"></a>Información general del

El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

A continuación, agregue dos controles `TextBox` a la página. Uno se transformará en un control deslizante gráfico y el otro contendrá la posición del control deslizante.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

El paso siguiente ya es el paso final. El control de `SliderExtender` de ASP.NET AJAX control Toolkit hace que un control deslizante salga del primer cuadro de texto y actualiza automáticamente el segundo cuadro de texto cuando cambia la posición del control deslizante. Para que funcione, el atributo `TargetControlID` del `SliderExtender`debe establecerse en el identificador del primer cuadro de texto. el atributo `BoundControlID` debe establecerse en el identificador del segundo cuadro de texto.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: Si escribe un nuevo valor en el cuadro de texto, se actualiza la posición del control deslizante. Si hace que el segundo cuadro de texto sea de solo lectura, puede Agregar una protección débil al campo de texto para que sea más difícil que el usuario actualice manualmente el valor en él.

[![control deslizante y cuadro de texto están sincronizados](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

El control deslizante y el cuadro de texto están sincronizados ([haga clic para ver la imagen de tamaño completo](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-the-slider-control-with-auto-postback-cs.md)
> [Siguiente](using-the-slider-control-with-auto-postback-vb.md)
