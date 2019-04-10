---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Usar el Control deslizante con Postback automático (C#) | Microsoft Docs
author: wenz
description: El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante Autocontab...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: a5b858a05470caa244902afbb404adbb2e4761b7
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59402731"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Usar el Control deslizante con Postback automático (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.


## <a name="overview"></a>Información general

El control deslizante en AJAX Control Toolkit proporciona un control deslizante gráfico que se puede controlar con el mouse. Es posible hacer que el control deslizante autopostback una vez que los cambios en su valor.

## <a name="steps"></a>Pasos

Para hacer que el control deslizante postback automáticamente tras un cambio, dos cuadros de texto necesitan el atributo `AutoPostBack="true"`: El cuadro de texto que se convertirá en el control deslizante propio y el cuadro de texto que contiene la posición del control deslizante. Este es el marcado necesario para:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

El `SliderExtender` control de ASP.NET AJAX Control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Un elemento de etiqueta adicional se usará después para informar al usuario de una devolución de datos:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Por último, el `ScriptManager` control de AJAX de ASP.NET carga el código JavaScript necesario para el Kit de herramientas de Control para que funcione:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Ahora el control deslizante está publicando. en el lado servidor, este evento se puede detectar y actúa sobre ella:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Mel control deslizante oving desencadena una devolución de datos](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Moviendo el control deslizante activa una devolución ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Afterwards, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Después, la fecha de este cambio se escribe en la etiqueta ([haga clic aquí para ver imagen en tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Siguiente](databinding-the-slider-control-cs.md)
