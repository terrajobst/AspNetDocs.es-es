---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ejecutar varias animaciones una tras otra (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Permite ejecutar Servera...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575446"
---
# <a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="18588-104">Ejecutar varias animaciones una tras otra (C#)</span><span class="sxs-lookup"><span data-stu-id="18588-104">Executing Several Animations after Each Other (C#)</span></span>

<span data-ttu-id="18588-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="18588-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="18588-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="18588-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="18588-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="18588-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="18588-108">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="18588-108">It allows to run several animations one after the other.</span></span>

<span data-ttu-id="18588-109">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="18588-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="18588-110">Permite ejecutar varias animaciones una tras otra.</span><span class="sxs-lookup"><span data-stu-id="18588-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="18588-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="18588-111">Steps</span></span>

<span data-ttu-id="18588-112">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="18588-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="18588-113">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="18588-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="18588-114">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="18588-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="18588-115">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="18588-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="18588-116">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="18588-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="18588-117">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="18588-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="18588-118">El marco de animación permite combinar varias animaciones en una mediante el elemento `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="18588-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="18588-119">Todas las animaciones dentro de `<Sequence>` se ejecutan una tras otra.</span><span class="sxs-lookup"><span data-stu-id="18588-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="18588-120">A continuación se muestra un posible marcado para el control de `AnimationExtender`, lo que primero es hacer que el panel sea más ancho y, a continuación, reducir su altura:</span><span class="sxs-lookup"><span data-stu-id="18588-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="18588-121">Al ejecutar este script, el panel se amplía primero y, a continuación, se reduce.</span><span class="sxs-lookup"><span data-stu-id="18588-121">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="18588-122">[![primero se aumenta el ancho](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="18588-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="18588-123">En primer lugar, el ancho aumenta ([haga clic para ver la imagen de tamaño completo](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="18588-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>

<span data-ttu-id="18588-124">[![se reduce el alto](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="18588-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="18588-125">A continuación, se reduce el alto ([haga clic para ver la imagen de tamaño completo](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="18588-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="18588-126">[Anterior](executing-several-animations-at-the-same-time-cs.md)
> [Siguiente](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="18588-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
