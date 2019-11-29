---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Usar el control deslizante con postback automático (VB) | Microsoft Docs
author: wenz
description: El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse. Es posible hacer que el control deslizante AUTOPOST...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: e7a3286bcf7ca844f5dcfa4848c15e0bd4767c0f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598549"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="a6172-104">Usar el control deslizante con el PostBack automático (VB)</span><span class="sxs-lookup"><span data-stu-id="a6172-104">Using the Slider Control With Auto-Postback (VB)</span></span>

<span data-ttu-id="a6172-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a6172-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a6172-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a6172-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="a6172-107">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="a6172-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a6172-108">Es posible hacer que el control deslizante sea automático una vez que su valor cambie.</span><span class="sxs-lookup"><span data-stu-id="a6172-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="a6172-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="a6172-109">Overview</span></span>

<span data-ttu-id="a6172-110">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="a6172-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a6172-111">Es posible hacer que el control deslizante sea automático una vez que su valor cambie.</span><span class="sxs-lookup"><span data-stu-id="a6172-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="a6172-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="a6172-112">Steps</span></span>

<span data-ttu-id="a6172-113">Para que el control deslizante se devierta automáticamente tras un cambio, ambos cuadros de texto necesitan el atributo `AutoPostBack="true"`: el cuadro de texto que se convertirá en el control deslizante y el cuadro de texto que contiene la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="a6172-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="a6172-114">Este es el marcado necesario para eso:</span><span class="sxs-lookup"><span data-stu-id="a6172-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="a6172-115">El control `SliderExtender` de ASP.NET AJAX control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:</span><span class="sxs-lookup"><span data-stu-id="a6172-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="a6172-116">Más adelante se usará un elemento de etiqueta adicional para informar al usuario de un postback:</span><span class="sxs-lookup"><span data-stu-id="a6172-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="a6172-117">Por último, el `ScriptManager` control de ASP.NET AJAX carga el JavaScript necesario para que el kit de herramientas de control funcione:</span><span class="sxs-lookup"><span data-stu-id="a6172-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="a6172-118">Ahora el control deslizante se vuelve a enviar; en el lado del servidor, este evento se puede detectar y actuar sobre:</span><span class="sxs-lookup"><span data-stu-id="a6172-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

<span data-ttu-id="a6172-119">[![mover el control deslizante desencadena un postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a6172-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="a6172-120">Al mover el control deslizante, se desencadena un postback ([hacer clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a6172-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>

<span data-ttu-id="a6172-121">[![después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a6172-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="a6172-122">Posteriormente, la fecha de este cambio se escribe en la etiqueta ([haga clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a6172-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6172-123">[Anterior](databinding-the-slider-control-cs.md)
> [Siguiente](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a6172-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
