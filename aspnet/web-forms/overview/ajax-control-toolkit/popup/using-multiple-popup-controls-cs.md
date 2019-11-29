---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Usar varios controles Popup (C#) | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. También es posible usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8700fe89af591e8b481e853580b0efa0cddbf1bc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611647"
---
# <a name="using-multiple-popup-controls-c"></a><span data-ttu-id="b6f0b-104">Usar varios controles emergentes (C#)</span><span class="sxs-lookup"><span data-stu-id="b6f0b-104">Using Multiple Popup Controls (C#)</span></span>

<span data-ttu-id="b6f0b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b6f0b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b6f0b-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b6f0b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="b6f0b-107">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b6f0b-108">También es posible usar más de un control popup en una página.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="b6f0b-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="b6f0b-109">Overview</span></span>

<span data-ttu-id="b6f0b-110">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b6f0b-111">También es posible usar más de un control popup en una página.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="b6f0b-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="b6f0b-112">Steps</span></span>

<span data-ttu-id="b6f0b-113">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="b6f0b-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="b6f0b-114">A continuación, agregue un panel que actúe como el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="b6f0b-115">En el escenario actual, el panel contiene un control de `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="b6f0b-116">Con el fin de evitar que se actualice la página causada por las devoluciones del calendario, el panel se coloca dentro de un control de `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="b6f0b-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="b6f0b-117">La página también contiene dos cuadros de texto.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-117">The page also contains two text boxes.</span></span> <span data-ttu-id="b6f0b-118">Para cada cuadro de texto, el cuadro emergente del calendario aparecerá una vez que se active el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="b6f0b-119">Ahora, extienda cada uno de los dos cuadros de texto con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="b6f0b-120">El atributo `TargetControlID` proporciona el identificador del control asociado al objeto extender.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="b6f0b-121">El atributo `PopupControlID` contiene el identificador del panel emergente.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="b6f0b-122">En este caso, ambos objetos extender muestran el mismo panel, pero también se pueden aplicar paneles diferentes.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="b6f0b-123">Ahora, cada vez que haga clic en un campo de texto, aparecerá un calendario debajo del campo, lo que le permitirá seleccionar una fecha.</span><span class="sxs-lookup"><span data-stu-id="b6f0b-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="b6f0b-124">(La obtención de la fecha seleccionada en los cuadros de texto se tratará en un tutorial diferente).</span><span class="sxs-lookup"><span data-stu-id="b6f0b-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="b6f0b-125">[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b6f0b-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="b6f0b-126">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b6f0b-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b6f0b-127">Siguiente</span><span class="sxs-lookup"><span data-stu-id="b6f0b-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
