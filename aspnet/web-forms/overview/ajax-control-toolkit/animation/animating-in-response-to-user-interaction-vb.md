---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animar en respuesta a la interacción del usuario (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Las animaciones pueden estrella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497929"
---
# <a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="3af0a-104">Animaciones en respuesta a la interacción del usuario (VB)</span><span class="sxs-lookup"><span data-stu-id="3af0a-104">Animating in Response To User Interaction (VB)</span></span>

<span data-ttu-id="3af0a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3af0a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3af0a-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="3af0a-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="3af0a-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="3af0a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3af0a-108">Las animaciones pueden iniciarse automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="3af0a-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="overview"></a><span data-ttu-id="3af0a-109">Información general</span><span class="sxs-lookup"><span data-stu-id="3af0a-109">Overview</span></span>

<span data-ttu-id="3af0a-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="3af0a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="3af0a-111">Las animaciones pueden iniciarse automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.</span><span class="sxs-lookup"><span data-stu-id="3af0a-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="3af0a-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="3af0a-112">Steps</span></span>

<span data-ttu-id="3af0a-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="3af0a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="3af0a-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="3af0a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="3af0a-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="3af0a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="3af0a-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="3af0a-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="3af0a-117">Dentro del nodo `<Animations>`, hay cinco maneras de iniciar la animación a través de la interacción del usuario (el elemento que falta se `<OnLoad>` que se ejecuta una vez que la página completa se ha cargado completamente):</span><span class="sxs-lookup"><span data-stu-id="3af0a-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="3af0a-118">`<OnClick>` (hacer clic con el mouse en el control)</span><span class="sxs-lookup"><span data-stu-id="3af0a-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="3af0a-119">`<OnHoverOut>` (el mouse sale del control)</span><span class="sxs-lookup"><span data-stu-id="3af0a-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="3af0a-120">`<OnHoverOver>` (se mantiene el mouse sobre un control y se detiene la animación `<OnHoverOut>`)</span><span class="sxs-lookup"><span data-stu-id="3af0a-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="3af0a-121">`<OnMouseOut>` (el mouse sale de un control)</span><span class="sxs-lookup"><span data-stu-id="3af0a-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="3af0a-122">`<OnMouseOver>` (el mouse se mantiene sobre un control, sin detener la animación `<OnMouseOut>`)</span><span class="sxs-lookup"><span data-stu-id="3af0a-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="3af0a-123">En este escenario, se utiliza `<OnClick>`.</span><span class="sxs-lookup"><span data-stu-id="3af0a-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="3af0a-124">Cuando el usuario hace clic en el panel, se cambia de tamaño y se atenúa al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="3af0a-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

<span data-ttu-id="3af0a-125">[![un clic del mouse inicia la animación](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3af0a-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="3af0a-126">Un clic del mouse inicia la animación ([haga clic para ver la imagen de tamaño completo](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3af0a-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3af0a-127">[Anterior](picking-one-animation-out-of-a-list-vb.md)
> [Siguiente](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3af0a-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
