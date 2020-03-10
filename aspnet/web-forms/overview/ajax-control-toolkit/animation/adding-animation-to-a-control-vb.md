---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Agregar animación a un control (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. En este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484129"
---
# <a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="a4f34-104">Agregar animación a un control (VB)</span><span class="sxs-lookup"><span data-stu-id="a4f34-104">Adding Animation to a Control (VB)</span></span>

<span data-ttu-id="a4f34-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a4f34-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a4f34-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a4f34-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="a4f34-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a4f34-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a4f34-108">En este tutorial se muestra cómo configurar este tipo de animación.</span><span class="sxs-lookup"><span data-stu-id="a4f34-108">This tutorial shows how to set up such an animation.</span></span>

## <a name="overview"></a><span data-ttu-id="a4f34-109">Información general</span><span class="sxs-lookup"><span data-stu-id="a4f34-109">Overview</span></span>

<span data-ttu-id="a4f34-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a4f34-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a4f34-111">En este tutorial se muestra cómo configurar este tipo de animación.</span><span class="sxs-lookup"><span data-stu-id="a4f34-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="a4f34-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="a4f34-112">Steps</span></span>

<span data-ttu-id="a4f34-113">El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="a4f34-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="a4f34-114">La animación de este escenario se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a4f34-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="a4f34-115">La clase CSS asociada para el panel define un color de fondo y un ancho:</span><span class="sxs-lookup"><span data-stu-id="a4f34-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="a4f34-116">A continuación, necesitamos el `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="a4f34-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="a4f34-117">Después de proporcionar un `ID` y el `runat="server"`habitual, el atributo `TargetControlID` debe establecerse en el control para animar en nuestro caso, el panel:</span><span class="sxs-lookup"><span data-stu-id="a4f34-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="a4f34-118">La animación completa se aplica mediante declaración, mediante una sintaxis XML, lamentablemente no es totalmente compatible con IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4f34-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="a4f34-119">El nodo raíz se `<Animations>;` dentro de este nodo; se permiten varios eventos que determinan cuándo se realizan las animaciones:</span><span class="sxs-lookup"><span data-stu-id="a4f34-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="a4f34-120">`OnClick` (clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="a4f34-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="a4f34-121">`OnHoverOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="a4f34-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="a4f34-122">`OnHoverOver` (cuando se mantiene el mouse sobre un control y se detiene la animación `OnHoverOut`)</span><span class="sxs-lookup"><span data-stu-id="a4f34-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="a4f34-123">`OnLoad` (cuando se carga la página)</span><span class="sxs-lookup"><span data-stu-id="a4f34-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="a4f34-124">`OnMouseOut` (cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="a4f34-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="a4f34-125">`OnMouseOver` (cuando se mantiene el mouse sobre un control, sin detener la animación de `OnMouseOut`)</span><span class="sxs-lookup"><span data-stu-id="a4f34-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="a4f34-126">El marco de trabajo incluye un conjunto de animaciones, cada una representada por su propio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="a4f34-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="a4f34-127">Esta es una selección:</span><span class="sxs-lookup"><span data-stu-id="a4f34-127">Here is a selection:</span></span>

- <span data-ttu-id="a4f34-128">`<Color>` (cambio de un color)</span><span class="sxs-lookup"><span data-stu-id="a4f34-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="a4f34-129">`<FadeIn>` (fundido en)</span><span class="sxs-lookup"><span data-stu-id="a4f34-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="a4f34-130">`<FadeOut>` (fundido)</span><span class="sxs-lookup"><span data-stu-id="a4f34-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="a4f34-131">`<Property>` (cambiar la propiedad de un control)</span><span class="sxs-lookup"><span data-stu-id="a4f34-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="a4f34-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="a4f34-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="a4f34-133">`<Resize>` (cambiar el tamaño)</span><span class="sxs-lookup"><span data-stu-id="a4f34-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="a4f34-134">`<Scale>` (cambiar proporcionalmente el tamaño)</span><span class="sxs-lookup"><span data-stu-id="a4f34-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="a4f34-135">En este ejemplo, el panel se atenúa. La animación debe tomar 1,5 segundos (`Duration` atributo), mostrando 24 fotogramas (pasos de animación) por segundo (`Fps` atributo).</span><span class="sxs-lookup"><span data-stu-id="a4f34-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="a4f34-136">Este es el marcado completo del control `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="a4f34-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="a4f34-137">Al ejecutar este script, el panel se muestra y se atenúa en unos segundos.</span><span class="sxs-lookup"><span data-stu-id="a4f34-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>

<span data-ttu-id="a4f34-138">[![se atenúa el panel](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a4f34-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="a4f34-139">El panel se atenúa ([haga clic para ver la imagen de tamaño completo](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a4f34-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4f34-140">[Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Siguiente](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a4f34-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
