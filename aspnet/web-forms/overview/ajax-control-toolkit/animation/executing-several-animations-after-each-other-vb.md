---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ejecutar varias animaciones una tras otra (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Permite ejecutar Servera...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483985"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="64ac8-104">Ejecutar varias animaciones una tras otra (VB)</span><span class="sxs-lookup"><span data-stu-id="64ac8-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="64ac8-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="64ac8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="64ac8-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="64ac8-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="64ac8-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="64ac8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="64ac8-108">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="64ac8-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="64ac8-109">Información general</span><span class="sxs-lookup"><span data-stu-id="64ac8-109">Overview</span></span>

<span data-ttu-id="64ac8-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="64ac8-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="64ac8-111">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="64ac8-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="64ac8-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="64ac8-112">Steps</span></span>

<span data-ttu-id="64ac8-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="64ac8-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="64ac8-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="64ac8-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="64ac8-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="64ac8-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="64ac8-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="64ac8-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="64ac8-117">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="64ac8-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="64ac8-118">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="64ac8-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="64ac8-119">El marco de animación permite combinar varias animaciones en una mediante el elemento `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="64ac8-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="64ac8-120">Todas las animaciones dentro de `<Sequence>` se ejecutan una tras otra.</span><span class="sxs-lookup"><span data-stu-id="64ac8-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="64ac8-121">A continuación se muestra un posible marcado para el control de `AnimationExtender`, lo que primero es hacer que el panel sea más ancho y, a continuación, reducir su altura:</span><span class="sxs-lookup"><span data-stu-id="64ac8-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="64ac8-122">Al ejecutar este script, el panel se amplía primero y, a continuación, se reduce.</span><span class="sxs-lookup"><span data-stu-id="64ac8-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="64ac8-123">[![primero se aumenta el ancho](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="64ac8-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="64ac8-124">En primer lugar, el ancho aumenta ([haga clic para ver la imagen de tamaño completo](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="64ac8-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="64ac8-125">[![se reduce el alto](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="64ac8-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="64ac8-126">A continuación, se reduce el alto ([haga clic para ver la imagen de tamaño completo](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="64ac8-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="64ac8-127">[Anterior](executing-several-animations-at-the-same-time-vb.md)
> [Siguiente](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="64ac8-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
