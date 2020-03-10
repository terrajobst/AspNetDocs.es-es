---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animación según una condición (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Si una animación es...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484171"
---
# <a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="68a29-104">Animación según una condición (C#)</span><span class="sxs-lookup"><span data-stu-id="68a29-104">Animation Depending On a Condition (C#)</span></span>

<span data-ttu-id="68a29-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68a29-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68a29-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="68a29-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="68a29-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="68a29-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="68a29-108">El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="68a29-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="68a29-109">Información general</span><span class="sxs-lookup"><span data-stu-id="68a29-109">Overview</span></span>

<span data-ttu-id="68a29-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="68a29-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="68a29-111">El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="68a29-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="68a29-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="68a29-112">Steps</span></span>

<span data-ttu-id="68a29-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="68a29-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="68a29-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="68a29-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="68a29-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="68a29-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="68a29-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="68a29-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="68a29-117">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="68a29-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="68a29-118">En lugar de una de las animaciones normales, el elemento `<Condition>` entra en juego.</span><span class="sxs-lookup"><span data-stu-id="68a29-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="68a29-119">El código JavaScript proporcionado como el valor del atributo `ConditionScript` se ejecuta en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="68a29-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="68a29-120">Si se evalúa como true, se ejecuta la animación; de lo contrario, no.</span><span class="sxs-lookup"><span data-stu-id="68a29-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="68a29-121">El marcado siguiente proporciona dos animaciones, cada una de las cuales se ejecuta en el 50% de los casos de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="68a29-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="68a29-122">Dado que solo puede haber una animación dentro de `<OnLoad>`, las dos animaciones `<Condition>` se unen juntas mediante el elemento `<Sequence>`:</span><span class="sxs-lookup"><span data-stu-id="68a29-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="68a29-123">Tenga en cuenta que el signo menor que (`<`) en el atributo `ConditionScript` debe ser un carácter de escape ().</span><span class="sxs-lookup"><span data-stu-id="68a29-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="68a29-124">Al ejecutar este script, no se ejecuta ninguna animación, o bien uno de los dos, o ambos.</span><span class="sxs-lookup"><span data-stu-id="68a29-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="68a29-125">[![el panel se atenúa sin cambiar de tamaño, por lo que la segunda animación se ejecuta, la primera no](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68a29-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="68a29-126">El panel se atenúa sin cambiar el tamaño, por lo que la segunda animación se ejecuta, pero la primera no ([hacer clic para ver la imagen de tamaño completo](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68a29-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="68a29-127">[Anterior](executing-several-animations-after-each-other-cs.md)
> [Siguiente](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="68a29-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
