---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Colocar un ModalPopup (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, el control no ofrece...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8034f5aeb5a1a80f1ea8cbc9d638f3dfb1a38706
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599009"
---
# <a name="positioning-a-modalpopup-c"></a><span data-ttu-id="276ec-104">Colocar un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="276ec-104">Positioning a ModalPopup (C#)</span></span>

<span data-ttu-id="276ec-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="276ec-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="276ec-106">[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="276ec-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="276ec-107">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="276ec-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="276ec-108">Sin embargo, el control no ofrece una funcionalidad integrada para colocar el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="276ec-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="276ec-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="276ec-109">Overview</span></span>

<span data-ttu-id="276ec-110">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="276ec-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="276ec-111">Sin embargo, el control no ofrece una funcionalidad integrada para colocar el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="276ec-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="276ec-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="276ec-112">Steps</span></span>

<span data-ttu-id="276ec-113">Para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, el `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="276ec-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="276ec-114">el control debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="276ec-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="276ec-115">A continuación, agregue un panel que actúe como elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="276ec-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="276ec-116">Se usa un botón para cerrar el elemento emergente:</span><span class="sxs-lookup"><span data-stu-id="276ec-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="276ec-117">Siempre que se muestre el elemento emergente, se colocará en un lugar determinado de la página.</span><span class="sxs-lookup"><span data-stu-id="276ec-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="276ec-118">Para esta tarea, se crea una función de JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="276ec-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="276ec-119">Primero intenta acceder al panel.</span><span class="sxs-lookup"><span data-stu-id="276ec-119">It first tries to access the panel.</span></span> <span data-ttu-id="276ec-120">Si se realiza correctamente, la posición del panel se establece mediante CSS y JavaScript (cambie la posición del menú emergente en).</span><span class="sxs-lookup"><span data-stu-id="276ec-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="276ec-121">Sin embargo, el control `ModalPopupExtender` también intenta colocar el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="276ec-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="276ec-122">Por lo tanto, el código JavaScript coloca la ventana emergente repetidamente, cada décima de segundo.</span><span class="sxs-lookup"><span data-stu-id="276ec-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="276ec-123">Como puede ver, el valor devuelto del método `setTimeout()` JavaScript se guarda en una variable global.</span><span class="sxs-lookup"><span data-stu-id="276ec-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="276ec-124">Esto permite detener el posicionamiento repetido de la ventana emergente a petición, mediante el método `clearTimeout()`:</span><span class="sxs-lookup"><span data-stu-id="276ec-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="276ec-125">Ahora todo lo que queda por hacer es hacer que el explorador llame a estas funciones siempre que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="276ec-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="276ec-126">Se debe llamar a la función de JavaScript `movePanel()` cuando se hace clic en el botón que desencadena el panel:</span><span class="sxs-lookup"><span data-stu-id="276ec-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="276ec-127">Y la función `stopMoving()` entra en juego cuando se cierra el elemento emergente que se puede desencadenar en el control `ModalPopupExtender`:</span><span class="sxs-lookup"><span data-stu-id="276ec-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]

<span data-ttu-id="276ec-128">[![aparece la ventana emergente modal en la posición designada](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="276ec-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="276ec-129">La ventana emergente modal aparece en la posición designada ([haga clic para ver la imagen de tamaño completo](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="276ec-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="276ec-130">[Anterior](handling-postbacks-from-a-modalpopup-cs.md)
> [Siguiente](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="276ec-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
