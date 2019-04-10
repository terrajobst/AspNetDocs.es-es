---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animar un Control UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Para el contenido de un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a7c40ebe359e21602d9f1de8205e1a7c808acc85
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59384258"
---
# <a name="animating-an-updatepanel-control-vb"></a><span data-ttu-id="7f566-104">Animar un control UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="7f566-104">Animating an UpdatePanel Control (VB)</span></span>

<span data-ttu-id="7f566-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7f566-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7f566-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7f566-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)</span></span>

> <span data-ttu-id="7f566-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="7f566-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7f566-108">Para el contenido de un UpdatePanel, un extensor especial existe que se basa principalmente en el marco de animación: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="7f566-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="7f566-109">Este tutorial muestra cómo configurar este tipo de una animación de un UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="7f566-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="7f566-110">Información general</span><span class="sxs-lookup"><span data-stu-id="7f566-110">Overview</span></span>

<span data-ttu-id="7f566-111">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="7f566-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7f566-112">Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa principalmente en el marco de animación: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="7f566-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="7f566-113">Este tutorial muestra cómo configurar este tipo de una animación para un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7f566-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="7f566-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="7f566-114">Steps</span></span>

<span data-ttu-id="7f566-115">El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="7f566-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

<span data-ttu-id="7f566-116">La animación en este escenario se aplicarán a ASP.NET `Wizard` control web que residen en un `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="7f566-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="7f566-117">Tres pasos (arbitrarios) proporcionan suficiente opciones para desencadenar devoluciones de datos:</span><span class="sxs-lookup"><span data-stu-id="7f566-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

<span data-ttu-id="7f566-118">El marcado necesario para la `UpdatePanelAnimationExtender` control es bastante semejante al marcado que se usa para el `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7f566-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="7f566-119">En el `TargetControlID` atributo proporcionamos el `ID` de la `UpdatePanel` se va a animar; dentro de la `UpdatePanelAnimationExtender` (control), el `<Animations>` elemento contiene el marcado XML de la animación.</span><span class="sxs-lookup"><span data-stu-id="7f566-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="7f566-120">Sin embargo, hay una diferencia: La cantidad de eventos y controladores de eventos está limitada en comparación con `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7f566-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="7f566-121">Para `UpdatePanels`, sólo dos de ellas existen:</span><span class="sxs-lookup"><span data-stu-id="7f566-121">For `UpdatePanels`, only two of them exist:</span></span>

- `<OnUpdated>` <span data-ttu-id="7f566-122">Cuando se ha actualizado el UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="7f566-122">when the UpdatePanel has been updated</span></span>
- `<OnUpdating>` <span data-ttu-id="7f566-123">Cuando inicia la actualización de UpdatePanel</span><span class="sxs-lookup"><span data-stu-id="7f566-123">when the UpdatePanel starts updating</span></span>

<span data-ttu-id="7f566-124">En este escenario, el nuevo contenido de la `UpdatePanel` (después de la devolución de datos) serán fundido de entrada.</span><span class="sxs-lookup"><span data-stu-id="7f566-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="7f566-125">Este es el marcado necesario para:</span><span class="sxs-lookup"><span data-stu-id="7f566-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

<span data-ttu-id="7f566-126">Ahora cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel de fundido de entrada sin problemas.</span><span class="sxs-lookup"><span data-stu-id="7f566-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


[![T<span data-ttu-id="7f566-127">siguiente paso del asistente se desvanece]</span><span class="sxs-lookup"><span data-stu-id="7f566-127">he next wizard step is fading in]</span></span>(animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

<span data-ttu-id="7f566-128">El siguiente paso del asistente se desvanece ([haga clic aquí para ver imagen en tamaño completo](animating-an-updatepanel-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7f566-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7f566-129">[Anterior](changing-an-animation-using-client-side-code-vb.md)
> [Siguiente](dynamically-controlling-updatepanel-animations-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7f566-129">[Previous](changing-an-animation-using-client-side-code-vb.md)
[Next](dynamically-controlling-updatepanel-animations-vb.md)</span></span>
