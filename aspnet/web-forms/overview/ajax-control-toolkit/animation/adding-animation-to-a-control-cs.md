---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Agregar animación a un Control (C#) | Microsoft Docs
author: wenz
description: El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control. Este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e4c6bfe1884d3e066c7b27e07e3a069943793bdd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392292"
---
# <a name="adding-animation-to-a-control-c"></a><span data-ttu-id="de6cc-104">Agregar animación a un control (C#)</span><span class="sxs-lookup"><span data-stu-id="de6cc-104">Adding Animation to a Control (C#)</span></span>

<span data-ttu-id="de6cc-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="de6cc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="de6cc-106">[Descargar código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="de6cc-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="de6cc-107">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="de6cc-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="de6cc-108">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="de6cc-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="de6cc-109">Información general</span><span class="sxs-lookup"><span data-stu-id="de6cc-109">Overview</span></span>

<span data-ttu-id="de6cc-110">El control de animación en ASP.NET AJAX Control Toolkit no es simplemente un control, pero un marco completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="de6cc-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="de6cc-111">Este tutorial muestra cómo configurar este tipo de una animación.</span><span class="sxs-lookup"><span data-stu-id="de6cc-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="de6cc-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="de6cc-112">Steps</span></span>

<span data-ttu-id="de6cc-113">El primer paso es como es habitual incluir el `ScriptManager` en la página para que se carga la biblioteca AJAX de ASP.NET y se puede usar el Kit de herramientas de Control:</span><span class="sxs-lookup"><span data-stu-id="de6cc-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="de6cc-114">La animación en este escenario se aplicará a un panel de texto que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="de6cc-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="de6cc-115">La clase CSS asociada para el panel define un ancho y un color de fondo:</span><span class="sxs-lookup"><span data-stu-id="de6cc-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="de6cc-116">A continuación, necesitamos el `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="de6cc-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="de6cc-117">Después de proporcionar un `ID` y el habitual `runat="server"`, el `TargetControlID` atributo debe establecerse para el control se va a animar en nuestro caso, el panel:</span><span class="sxs-lookup"><span data-stu-id="de6cc-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="de6cc-118">Mediante declaración, se aplica la animación completa mediante una sintaxis XML, lamentablemente actualmente no admitida totalmente IntelliSense de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="de6cc-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="de6cc-119">El nodo raíz es `<Animations>;` dentro de este nodo, se permiten varios eventos que determinan cuándo la animación adopten lugar:</span><span class="sxs-lookup"><span data-stu-id="de6cc-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- `OnClick` <span data-ttu-id="de6cc-120">(clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="de6cc-120">(mouse click)</span></span>
- `OnHoverOut` <span data-ttu-id="de6cc-121">(cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="de6cc-121">(when the mouse leaves a control)</span></span>
- `OnHoverOver` <span data-ttu-id="de6cc-122">(al mantener el mouse sobre un control, deteniendo el `OnHoverOut` animación)</span><span class="sxs-lookup"><span data-stu-id="de6cc-122">(when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- `OnLoad` <span data-ttu-id="de6cc-123">(cuando se ha cargado la página)</span><span class="sxs-lookup"><span data-stu-id="de6cc-123">(when the page has been loaded)</span></span>
- `OnMouseOut` <span data-ttu-id="de6cc-124">(cuando el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="de6cc-124">(when the mouse leaves a control)</span></span>
- `OnMouseOver` <span data-ttu-id="de6cc-125">(al mantener el mouse sobre un control, no Deteniendo el `OnMouseOut` animación)</span><span class="sxs-lookup"><span data-stu-id="de6cc-125">(when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="de6cc-126">El marco de trabajo incluye un conjunto de animaciones, cada uno representado por su propio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="de6cc-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="de6cc-127">Aquí es una selección:</span><span class="sxs-lookup"><span data-stu-id="de6cc-127">Here is a selection:</span></span>

- `<Color>` <span data-ttu-id="de6cc-128">(cambiar un color)</span><span class="sxs-lookup"><span data-stu-id="de6cc-128">(changing a color)</span></span>
- `<FadeIn>` <span data-ttu-id="de6cc-129">(resaltar)</span><span class="sxs-lookup"><span data-stu-id="de6cc-129">(fading in)</span></span>
- `<FadeOut>` <span data-ttu-id="de6cc-130">(difuminado)</span><span class="sxs-lookup"><span data-stu-id="de6cc-130">(fading out)</span></span>
- `<Property>` <span data-ttu-id="de6cc-131">(cambiar la propiedad de un control)</span><span class="sxs-lookup"><span data-stu-id="de6cc-131">(changing a control's property)</span></span>
- `<Pulse>` <span data-ttu-id="de6cc-132">(en alza)</span><span class="sxs-lookup"><span data-stu-id="de6cc-132">(pulsating)</span></span>
- `<Resize>` <span data-ttu-id="de6cc-133">(cambiar el tamaño)</span><span class="sxs-lookup"><span data-stu-id="de6cc-133">(changing the size)</span></span>
- `<Scale>` <span data-ttu-id="de6cc-134">(cambiar proporcionalmente el tamaño)</span><span class="sxs-lookup"><span data-stu-id="de6cc-134">(proportionally changing the size)</span></span>

<span data-ttu-id="de6cc-135">En este ejemplo, el panel será fundido de salida. La animación adoptarán 1,5 segundos (`Duration` atributo), mostrar (pasos de animación) de 24 fotogramas por segundo (`Fps` atributo).</span><span class="sxs-lookup"><span data-stu-id="de6cc-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="de6cc-136">Aquí está el marcado completo para el `AnimationExtender` control:</span><span class="sxs-lookup"><span data-stu-id="de6cc-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="de6cc-137">Al ejecutar este script, el panel se muestra y atenúa en segundos de uno y medio.</span><span class="sxs-lookup"><span data-stu-id="de6cc-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


[![T<span data-ttu-id="de6cc-138">panel se atenúa]</span><span class="sxs-lookup"><span data-stu-id="de6cc-138">he panel is fading out]</span></span>(adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

<span data-ttu-id="de6cc-139">El panel se atenúa ([haga clic aquí para ver imagen en tamaño completo](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="de6cc-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="de6cc-140">Siguiente</span><span class="sxs-lookup"><span data-stu-id="de6cc-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
