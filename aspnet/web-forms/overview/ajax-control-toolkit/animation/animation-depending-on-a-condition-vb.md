---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animación según una condición (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Si una animación es...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497881"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="96679-104">Animación según una condición (VB)</span><span class="sxs-lookup"><span data-stu-id="96679-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="96679-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="96679-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="96679-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="96679-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="96679-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="96679-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="96679-108">El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="96679-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="96679-109">Información general</span><span class="sxs-lookup"><span data-stu-id="96679-109">Overview</span></span>

<span data-ttu-id="96679-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="96679-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="96679-111">El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="96679-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="96679-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="96679-112">Steps</span></span>

<span data-ttu-id="96679-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="96679-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="96679-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="96679-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="96679-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="96679-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="96679-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="96679-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="96679-117">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="96679-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="96679-118">En lugar de una de las animaciones normales, el elemento `<Condition>` entra en juego.</span><span class="sxs-lookup"><span data-stu-id="96679-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="96679-119">El código JavaScript proporcionado como el valor del atributo `ConditionScript` se ejecuta en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="96679-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="96679-120">Si se evalúa como true, se ejecuta la animación; de lo contrario, no.</span><span class="sxs-lookup"><span data-stu-id="96679-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="96679-121">El marcado siguiente proporciona dos animaciones, cada una de las cuales se ejecuta en el 50% de los casos de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="96679-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="96679-122">Dado que solo puede haber una animación dentro de `<OnLoad>`, las dos animaciones `<Condition>` se unen juntas mediante el elemento `<Sequence>`:</span><span class="sxs-lookup"><span data-stu-id="96679-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="96679-123">Tenga en cuenta que el signo menor que (`<`) en el atributo `ConditionScript` debe ser un carácter de escape ().</span><span class="sxs-lookup"><span data-stu-id="96679-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="96679-124">Al ejecutar este script, no se ejecuta ninguna animación, o bien uno de los dos, o ambos.</span><span class="sxs-lookup"><span data-stu-id="96679-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="96679-125">[![el panel se atenúa sin cambiar de tamaño, por lo que la segunda animación se ejecuta, la primera no](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="96679-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="96679-126">El panel se atenúa sin cambiar el tamaño, por lo que la segunda animación se ejecuta, pero la primera no ([hacer clic para ver la imagen de tamaño completo](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="96679-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="96679-127">[Anterior](executing-several-animations-after-each-other-vb.md)
> [Siguiente](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="96679-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
