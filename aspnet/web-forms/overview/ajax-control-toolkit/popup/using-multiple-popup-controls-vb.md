---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usar varios controles Popup (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. También es posible usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611591"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="499a6-104">Usar varios controles emergentes (VB)</span><span class="sxs-lookup"><span data-stu-id="499a6-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="499a6-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="499a6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="499a6-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="499a6-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="499a6-107">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="499a6-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="499a6-108">También es posible usar más de un control popup en una página.</span><span class="sxs-lookup"><span data-stu-id="499a6-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="499a6-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="499a6-109">Overview</span></span>

<span data-ttu-id="499a6-110">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="499a6-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="499a6-111">También es posible usar más de un control popup en una página.</span><span class="sxs-lookup"><span data-stu-id="499a6-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="499a6-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="499a6-112">Steps</span></span>

<span data-ttu-id="499a6-113">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="499a6-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="499a6-114">A continuación, agregue un panel que actúe como el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="499a6-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="499a6-115">En el escenario actual, el panel contiene un control de `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="499a6-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="499a6-116">Con el fin de evitar que se actualice la página causada por las devoluciones del calendario, el panel se coloca dentro de un control de `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="499a6-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="499a6-117">La página también contiene dos cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="499a6-117">The page also contains two text boxes.</span></span> <span data-ttu-id="499a6-118">Para cada cuadro de texto, el cuadro emergente del calendario aparecerá una vez que se active el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="499a6-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="499a6-119">Ahora, extienda cada uno de los dos cuadros de texto con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="499a6-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="499a6-120">El atributo `TargetControlID` proporciona el identificador del control asociado al objeto extender.</span><span class="sxs-lookup"><span data-stu-id="499a6-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="499a6-121">El atributo `PopupControlID` contiene el identificador del panel emergente.</span><span class="sxs-lookup"><span data-stu-id="499a6-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="499a6-122">En este caso, ambos objetos extender muestran el mismo panel, pero también se pueden aplicar paneles diferentes.</span><span class="sxs-lookup"><span data-stu-id="499a6-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="499a6-123">Ahora, cada vez que haga clic en un campo de texto, aparecerá un calendario debajo del campo, lo que le permitirá seleccionar una fecha.</span><span class="sxs-lookup"><span data-stu-id="499a6-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="499a6-124">(La obtención de la fecha seleccionada en los cuadros de texto se tratará en un tutorial diferente).</span><span class="sxs-lookup"><span data-stu-id="499a6-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="499a6-125">[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="499a6-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="499a6-126">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="499a6-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="499a6-127">[Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="499a6-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
