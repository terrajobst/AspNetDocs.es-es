---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animar un control UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d66dda923940a328c0757049c9d8bfa3b2d2b9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431017"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="34335-104">Animar un control UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="34335-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="34335-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="34335-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="34335-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="34335-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="34335-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="34335-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="34335-108">Para el contenido de un UpdatePanel, existe un extensor especial que se basa en gran medida en el marco de animación: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="34335-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="34335-109">En este tutorial se muestra cómo configurar este tipo de animación para un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="34335-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>

## <a name="overview"></a><span data-ttu-id="34335-110">Información general</span><span class="sxs-lookup"><span data-stu-id="34335-110">Overview</span></span>

<span data-ttu-id="34335-111">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="34335-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="34335-112">Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa en gran medida en el marco de animación: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="34335-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="34335-113">En este tutorial se muestra cómo configurar este tipo de animación para un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="34335-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="34335-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="34335-114">Steps</span></span>

<span data-ttu-id="34335-115">El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="34335-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="34335-116">La animación de este escenario se aplicará a un control Web de ASP.NET `Wizard` que resida en un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="34335-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="34335-117">Tres pasos (arbitrarios) proporcionan suficientes opciones para desencadenar postbacks:</span><span class="sxs-lookup"><span data-stu-id="34335-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="34335-118">El marcado necesario para el control `UpdatePanelAnimationExtender` es bastante similar al marcado que se usa para la `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="34335-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="34335-119">En el atributo `TargetControlID` se proporciona el `ID` de la `UpdatePanel` que se va a animar; en el control `UpdatePanelAnimationExtender`, el elemento `<Animations>` contiene el marcado XML para las animaciones.</span><span class="sxs-lookup"><span data-stu-id="34335-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="34335-120">Sin embargo, hay una diferencia: la cantidad de eventos y controladores de eventos se limita en comparación con `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="34335-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="34335-121">Por `UpdatePanels`, solo existen dos de ellos:</span><span class="sxs-lookup"><span data-stu-id="34335-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="34335-122">`<OnUpdated>` cuando se ha actualizado el UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="34335-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="34335-123">`<OnUpdating>` cuando se inicia la actualización de UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="34335-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="34335-124">En este escenario, el nuevo contenido del `UpdatePanel` (después del postback) debe atenuarse.</span><span class="sxs-lookup"><span data-stu-id="34335-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="34335-125">Este es el marcado necesario para ello:</span><span class="sxs-lookup"><span data-stu-id="34335-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="34335-126">Ahora, cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel se desvanece sin problemas.</span><span class="sxs-lookup"><span data-stu-id="34335-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>

<span data-ttu-id="34335-127">[![se atenúa el siguiente paso del asistente](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="34335-127">[![The next wizard step is fading in](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="34335-128">Se difumina el siguiente paso del asistente ([haga clic para ver la imagen de tamaño completo](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="34335-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="34335-129">[Anterior](changing-an-animation-using-client-side-code-vb.md)
> [Siguiente](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="34335-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
