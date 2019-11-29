---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Desencadenar una animación en otro control (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575016"
---
# <a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="28ec5-104">Desencadenar una animación en otro control (VB)</span><span class="sxs-lookup"><span data-stu-id="28ec5-104">Triggering an Animation in another Control (VB)</span></span>

<span data-ttu-id="28ec5-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="28ec5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="28ec5-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="28ec5-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="28ec5-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28ec5-108">Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="28ec5-109">Sin embargo, también es posible interactuar con un control y después animar otro control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="28ec5-110">Información general del</span><span class="sxs-lookup"><span data-stu-id="28ec5-110">Overview</span></span>

<span data-ttu-id="28ec5-111">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28ec5-112">Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="28ec5-113">Sin embargo, también es posible interactuar con un control y después animar otro control.</span><span class="sxs-lookup"><span data-stu-id="28ec5-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="28ec5-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="28ec5-114">Steps</span></span>

<span data-ttu-id="28ec5-115">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="28ec5-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="28ec5-116">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="28ec5-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="28ec5-117">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="28ec5-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="28ec5-118">Para iniciar la animación del panel, se usa un botón HTML.</span><span class="sxs-lookup"><span data-stu-id="28ec5-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="28ec5-119">Tenga en cuenta que `<input type="button" />` es favorable a `<asp:Button />` ya que no deseamos un postback cuando el usuario hace clic en ese botón.</span><span class="sxs-lookup"><span data-stu-id="28ec5-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="28ec5-120">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio.</span><span class="sxs-lookup"><span data-stu-id="28ec5-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="28ec5-121">Es importante establecer `TargetControlID` en el identificador del botón (el elemento que desencadena la animación), no en el identificador del panel (el elemento que se anima)</span><span class="sxs-lookup"><span data-stu-id="28ec5-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="28ec5-122">Dentro del nodo `<Animations>`, coloque las animaciones como de costumbre.</span><span class="sxs-lookup"><span data-stu-id="28ec5-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="28ec5-123">Para que cambien el panel, no el botón, establezca el `AnimationTarget` atributo para cada elemento de animación dentro de `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="28ec5-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="28ec5-124">El valor de `AnimationTarget` es el identificador del panel, por supuesto.</span><span class="sxs-lookup"><span data-stu-id="28ec5-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="28ec5-125">De este modo, las animaciones se producen con el panel, no con el botón desencadenador.</span><span class="sxs-lookup"><span data-stu-id="28ec5-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="28ec5-126">Este es el marcado de `AnimationExtender` para este escenario:</span><span class="sxs-lookup"><span data-stu-id="28ec5-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="28ec5-127">Observe el orden especial en el que aparecen las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="28ec5-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="28ec5-128">En primer lugar, el botón se desactiva una vez que se ejecuta la animación.</span><span class="sxs-lookup"><span data-stu-id="28ec5-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="28ec5-129">Puesto que no hay ningún atributo `AnimationTarget` en el elemento `<EnableAction>`, esta animación se aplica al control de origen: el botón.</span><span class="sxs-lookup"><span data-stu-id="28ec5-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="28ec5-130">Los dos pasos siguientes de animación deben realizarse en paralelo (elemento`<Parallel>`).</span><span class="sxs-lookup"><span data-stu-id="28ec5-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="28ec5-131">Ambos tienen los atributos `AnimationTarget` establecidos en `"Panel1"`, lo que anima el panel, no el botón.</span><span class="sxs-lookup"><span data-stu-id="28ec5-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="28ec5-132">[![un clic del mouse en el botón inicia la animación del panel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28ec5-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="28ec5-133">Al hacer clic con el mouse en el botón, se inicia la animación del panel ([haga clic para ver la imagen de tamaño completo](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="28ec5-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28ec5-134">[Anterior](disabling-actions-during-animation-vb.md)
> [Siguiente](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="28ec5-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
