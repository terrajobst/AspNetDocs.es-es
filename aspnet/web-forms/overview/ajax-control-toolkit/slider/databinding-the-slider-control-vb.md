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
# <a name="databinding-the-slider-control-vb"></a><span data-ttu-id="69cd1-104">Enlace de datos del control deslizante (VB)</span><span class="sxs-lookup"><span data-stu-id="69cd1-104">Databinding the Slider Control (VB)</span></span>

<span data-ttu-id="69cd1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="69cd1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="69cd1-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="69cd1-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="69cd1-107">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="69cd1-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="69cd1-108">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="69cd1-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="69cd1-109">Información general</span><span class="sxs-lookup"><span data-stu-id="69cd1-109">Overview</span></span>

<span data-ttu-id="69cd1-110">El control deslizante en el kit de herramientas de control de AJAX proporciona un control deslizante gráfico que se puede controlar mediante el mouse.</span><span class="sxs-lookup"><span data-stu-id="69cd1-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="69cd1-111">Es posible enlazar la posición actual del control deslizante a otro control ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="69cd1-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="69cd1-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="69cd1-112">Steps</span></span>

<span data-ttu-id="69cd1-113">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="69cd1-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="69cd1-114">A continuación, agregue dos controles `TextBox` a la página.</span><span class="sxs-lookup"><span data-stu-id="69cd1-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="69cd1-115">Uno se transformará en un control deslizante gráfico y el otro contendrá la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="69cd1-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="69cd1-116">El paso siguiente ya es el paso final.</span><span class="sxs-lookup"><span data-stu-id="69cd1-116">The next step is already the final step.</span></span> <span data-ttu-id="69cd1-117">El control de `SliderExtender` de ASP.NET AJAX control Toolkit hace que un control deslizante salga del primer cuadro de texto y actualiza automáticamente el segundo cuadro de texto cuando cambia la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="69cd1-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="69cd1-118">Para que funcione, el atributo `TargetControlID` del `SliderExtender`debe establecerse en el identificador del primer cuadro de texto. el atributo `BoundControlID` debe establecerse en el identificador del segundo cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="69cd1-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="69cd1-119">Como puede ver en el explorador, el enlace de datos funciona en ambas direcciones: Si escribe un nuevo valor en el cuadro de texto, se actualiza la posición del control deslizante.</span><span class="sxs-lookup"><span data-stu-id="69cd1-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="69cd1-120">Si hace que el segundo cuadro de texto sea de solo lectura, puede Agregar una protección débil al campo de texto para que sea más difícil que el usuario actualice manualmente el valor en él.</span><span class="sxs-lookup"><span data-stu-id="69cd1-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="69cd1-121">[![control deslizante y cuadro de texto están sincronizados](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="69cd1-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="69cd1-122">El control deslizante y el cuadro de texto están sincronizados ([haga clic para ver la imagen de tamaño completo](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="69cd1-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="69cd1-123">Anterior</span><span class="sxs-lookup"><span data-stu-id="69cd1-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
