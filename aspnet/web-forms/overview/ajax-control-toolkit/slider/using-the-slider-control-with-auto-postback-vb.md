---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Usar el Control deslizante con Postback automático (VB) | Microsoft Docs
author: wenz
description: El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante Autocontab...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: c4ee6642726b4209d09907f615ee3286ca00caa3
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65124617"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a>Usar el Control deslizante con Postback automático (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.

## <a name="overview"></a>Información general

El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.

## <a name="steps"></a>Pasos

Para hacer que el control deslizante postback automáticamente tras un cambio, dos cuadros de texto necesitan el atributo `AutoPostBack="true"`: El cuadro de texto que se convertirá en el control deslizante propio y el cuadro de texto que contiene la posición del control deslizante. Este es el marcado necesario para:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

El `SliderExtender` control de ASP.NET AJAX Control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Un elemento de etiqueta adicional se usará después para informar al usuario de una devolución de datos:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Por último, el `ScriptManager` control de AJAX de ASP.NET carga el código JavaScript necesario para el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Ahora el control deslizante está publicando. en el lado servidor, este evento se puede detectar y actúa sobre ella:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

[![Moviendo el control deslizante desencadena una devolución de datos](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Moviendo el control deslizante activa una devolución ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image3.png))

[![Después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Después, la fecha de este cambio se escribe en la etiqueta ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](databinding-the-slider-control-cs.md)
> [Siguiente](databinding-the-slider-control-vb.md)
