---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Seleccionar una animación de una lista (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también per...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430825"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="a4556-104">Seleccionar una animación de una lista (VB)</span><span class="sxs-lookup"><span data-stu-id="a4556-104">Picking One Animation Out Of a List (VB)</span></span>

<span data-ttu-id="a4556-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a4556-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a4556-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a4556-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="a4556-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a4556-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a4556-108">El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a4556-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="a4556-109">Información general</span><span class="sxs-lookup"><span data-stu-id="a4556-109">Overview</span></span>

<span data-ttu-id="a4556-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="a4556-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a4556-111">El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a4556-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a4556-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="a4556-112">Steps</span></span>

<span data-ttu-id="a4556-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="a4556-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="a4556-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a4556-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="a4556-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="a4556-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="a4556-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="a4556-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="a4556-117">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="a4556-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="a4556-118">En lugar de una de las animaciones normales, el elemento `<Case>` entra en juego.</span><span class="sxs-lookup"><span data-stu-id="a4556-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="a4556-119">Se evalúa el valor de su atributo SelectScript; el valor devuelto debe ser numérico.</span><span class="sxs-lookup"><span data-stu-id="a4556-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="a4556-120">Dependiendo de este número, se ejecuta una de las subanimaciones en &lt;caso&gt;.</span><span class="sxs-lookup"><span data-stu-id="a4556-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="a4556-121">Por ejemplo, si SelectScript se evalúa como 2, el kit de herramientas de control ejecuta la tercera animación en &lt;caso&gt; (el recuento comienza en 0).</span><span class="sxs-lookup"><span data-stu-id="a4556-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="a4556-122">El marcado siguiente define tres Subanimaciones: cambiar el tamaño del ancho, cambiar el tamaño del alto y atenuarla. A continuación, el código JavaScript (`Math.floor(3 * Math.random())`) selecciona un número entre 0 y 2 para que se ejecute una de las tres animaciones:</span><span class="sxs-lookup"><span data-stu-id="a4556-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

<span data-ttu-id="a4556-123">[![una de las tres animaciones posibles: el panel se amplía](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a4556-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="a4556-124">Una de las tres posibles animaciones: el panel es más amplio ([haga clic para ver la imagen de tamaño completo](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a4556-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a4556-125">[Anterior](animation-depending-on-a-condition-vb.md)
> [Siguiente](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a4556-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
