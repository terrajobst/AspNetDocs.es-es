---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Usar el control deslizante con postback automáticoC#() | Microsoft Docs
author: wenz
description: El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible hacer que el control deslizante AUTOPOST...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445831"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a>Usar el control deslizante con el PostBackC#automático ()

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible hacer que el control deslizante sea automático una vez que su valor cambie.

## <a name="overview"></a>Información general

El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible hacer que el control deslizante sea automático una vez que su valor cambie.

## <a name="steps"></a>Pasos

Para que el control deslizante se devierta automáticamente tras un cambio, ambos cuadros de texto necesitan el atributo `AutoPostBack="true"`: el cuadro de texto que se convertirá en el control deslizante y el cuadro de texto que contiene la posición del control deslizante. Este es el marcado necesario para eso:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

El control `SliderExtender` de ASP.NET AJAX control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Más adelante se usará un elemento de etiqueta adicional para informar al usuario de un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Por último, el `ScriptManager` control de ASP.NET AJAX carga el JavaScript necesario para que el kit de herramientas de control funcione:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Ahora el control deslizante se vuelve a enviar; en el lado del servidor, este evento se puede detectar y actuar sobre:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

[![mover el control deslizante desencadena un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Al mover el control deslizante, se desencadena un postback ([hacer clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image3.png))

[![después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Posteriormente, la fecha de este cambio se escribe en la etiqueta ([haga clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Siguiente](databinding-the-slider-control-cs.md)
