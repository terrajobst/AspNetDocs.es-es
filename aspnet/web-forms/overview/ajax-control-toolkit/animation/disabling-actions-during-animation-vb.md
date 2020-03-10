---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deshabilitar acciones durante la animación (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También admite la acción...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430933"
---
# <a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="ca286-104">Deshabilitar las acciones durante la animación (VB)</span><span class="sxs-lookup"><span data-stu-id="ca286-104">Disabling Actions during Animation (VB)</span></span>

<span data-ttu-id="ca286-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ca286-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ca286-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ca286-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="ca286-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ca286-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ca286-108">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="ca286-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="ca286-109">Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="ca286-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="overview"></a><span data-ttu-id="ca286-110">Información general</span><span class="sxs-lookup"><span data-stu-id="ca286-110">Overview</span></span>

<span data-ttu-id="ca286-111">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="ca286-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ca286-112">También admite acciones, como clics del mouse.</span><span class="sxs-lookup"><span data-stu-id="ca286-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="ca286-113">Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.</span><span class="sxs-lookup"><span data-stu-id="ca286-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ca286-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="ca286-114">Steps</span></span>

<span data-ttu-id="ca286-115">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="ca286-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="ca286-116">La animación se aplicará a un botón HTML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ca286-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="ca286-117">Tenga en cuenta que se usa un control HTML en lugar de un control Web, ya que no se desea que el botón cree un postback; solo se iniciará la animación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="ca286-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="ca286-118">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="ca286-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="ca286-119">Dentro del nodo `<Animations>`, `<OnClick>` es el elemento adecuado para controlar el clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="ca286-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="ca286-120">Sin embargo, también se puede hacer clic en el botón durante la animación.</span><span class="sxs-lookup"><span data-stu-id="ca286-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="ca286-121">El elemento `<EnableAction>` puede encargarse de ello.</span><span class="sxs-lookup"><span data-stu-id="ca286-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="ca286-122">Al establecer `Enabled="false"`, se deshabilita el botón como parte de la animación.</span><span class="sxs-lookup"><span data-stu-id="ca286-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="ca286-123">Dado que usamos varias animaciones individuales (deshabilitando el botón y las animaciones reales), el elemento `<Parallel>` es necesario para pegar las animaciones individuales en una sola.</span><span class="sxs-lookup"><span data-stu-id="ca286-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="ca286-124">Este es el marcado completo de `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="ca286-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="ca286-125">También sería posible volver a habilitar para el botón después de la animación, utilizando el siguiente elemento XML al final de la lista:</span><span class="sxs-lookup"><span data-stu-id="ca286-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="ca286-126">Sin embargo, en el escenario dado esto sería inútil, ya que el botón se atenúa y no está visible al final de la animación.</span><span class="sxs-lookup"><span data-stu-id="ca286-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>

<span data-ttu-id="ca286-127">[![el botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ca286-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="ca286-128">El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic para ver la imagen de tamaño completo](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ca286-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca286-129">[Anterior](animating-in-response-to-user-interaction-vb.md)
> [Siguiente](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="ca286-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
