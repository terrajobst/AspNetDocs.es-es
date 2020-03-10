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
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="fb450-104">Usar el control deslizante con el PostBackC#automático ()</span><span class="sxs-lookup"><span data-stu-id="fb450-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="fb450-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fb450-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fb450-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fb450-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="fb450-107">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="fb450-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="fb450-108">Es posible hacer que el control deslizante sea automático una vez que su valor cambie.</span><span class="sxs-lookup"><span data-stu-id="fb450-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="fb450-109">Información general</span><span class="sxs-lookup"><span data-stu-id="fb450-109">Overview</span></span>

<span data-ttu-id="fb450-110">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="fb450-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="fb450-111">Es posible hacer que el control deslizante sea automático una vez que su valor cambie.</span><span class="sxs-lookup"><span data-stu-id="fb450-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="fb450-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="fb450-112">Steps</span></span>

<span data-ttu-id="fb450-113">Para que el control deslizante se devierta automáticamente tras un cambio, ambos cuadros de texto necesitan el atributo `AutoPostBack="true"`: el cuadro de texto que se convertirá en el control deslizante y el cuadro de texto que contiene la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="fb450-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="fb450-114">Este es el marcado necesario para eso:</span><span class="sxs-lookup"><span data-stu-id="fb450-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="fb450-115">El control `SliderExtender` de ASP.NET AJAX control Toolkit asigna la funcionalidad del control deslizante a los dos cuadros de texto:</span><span class="sxs-lookup"><span data-stu-id="fb450-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="fb450-116">Más adelante se usará un elemento de etiqueta adicional para informar al usuario de un postback:</span><span class="sxs-lookup"><span data-stu-id="fb450-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="fb450-117">Por último, el `ScriptManager` control de ASP.NET AJAX carga el JavaScript necesario para que el kit de herramientas de control funcione:</span><span class="sxs-lookup"><span data-stu-id="fb450-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="fb450-118">Ahora el control deslizante se vuelve a enviar; en el lado del servidor, este evento se puede detectar y actuar sobre:</span><span class="sxs-lookup"><span data-stu-id="fb450-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

<span data-ttu-id="fb450-119">[![mover el control deslizante desencadena un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fb450-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="fb450-120">Al mover el control deslizante, se desencadena un postback ([hacer clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fb450-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>

<span data-ttu-id="fb450-121">[![después, la fecha de este cambio se escribe en la etiqueta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fb450-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="fb450-122">Posteriormente, la fecha de este cambio se escribe en la etiqueta ([haga clic para ver la imagen de tamaño completo](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="fb450-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fb450-123">Siguiente</span><span class="sxs-lookup"><span data-stu-id="fb450-123">Next</span></span>](databinding-the-slider-control-cs.md)
