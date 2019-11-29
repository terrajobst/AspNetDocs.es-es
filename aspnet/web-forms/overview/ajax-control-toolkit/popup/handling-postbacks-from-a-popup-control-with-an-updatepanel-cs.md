---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Controlar los postbacks desde un control popup con un UpdatePanelC#() | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Se debe tener especial cuidado...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606336"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="6e152-104">Controlar los postbacks desde un control emergente con un UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="6e152-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="6e152-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6e152-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6e152-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6e152-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="6e152-107">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="6e152-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6e152-108">Se debe tener especial cuidado cuando se produce un postback dentro de este tipo de elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="6e152-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="6e152-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="6e152-109">Overview</span></span>

<span data-ttu-id="6e152-110">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="6e152-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6e152-111">Se debe tener especial cuidado cuando se produce un postback dentro de este tipo de elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="6e152-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="6e152-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="6e152-112">Steps</span></span>

<span data-ttu-id="6e152-113">Cuando se usa un `PopupControl` con un postback, una `UpdatePanel` puede evitar la actualización de la página causada por el PostBack.</span><span class="sxs-lookup"><span data-stu-id="6e152-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="6e152-114">El marcado siguiente define un par de elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="6e152-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="6e152-115">Un control `ScriptManager` para que el kit de herramientas de control de AJAX de ASP.NET funcione</span><span class="sxs-lookup"><span data-stu-id="6e152-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="6e152-116">Dos controles `TextBox` que desencadenarán un elemento popup</span><span class="sxs-lookup"><span data-stu-id="6e152-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="6e152-117">Control `Panel` que servirá como elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="6e152-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="6e152-118">Dentro del panel, se inserta un control `Calendar` dentro de un control `UpdatePanel`</span><span class="sxs-lookup"><span data-stu-id="6e152-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="6e152-119">Dos `PopupControlExtender` controles que asignan el panel a los cuadros de texto</span><span class="sxs-lookup"><span data-stu-id="6e152-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="6e152-120">Tenga en cuenta que el atributo `OnSelectionChanged` del control `Calendar` está establecido.</span><span class="sxs-lookup"><span data-stu-id="6e152-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="6e152-121">Por tanto, cuando el usuario selecciona una fecha en el calendario, se produce un postback y se ejecuta el método del servidor `c1_SelectionChanged()`.</span><span class="sxs-lookup"><span data-stu-id="6e152-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="6e152-122">Dentro de ese método, la fecha actual se debe recuperar y volver a escribir en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="6e152-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="6e152-123">La sintaxis de es la siguiente: en primer lugar, se debe generar un objeto de proxy para el `PopupControlExtender` en la página.</span><span class="sxs-lookup"><span data-stu-id="6e152-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="6e152-124">ASP.NET AJAX control Toolkit ofrece el método `GetProxyForCurrentPopup()`.</span><span class="sxs-lookup"><span data-stu-id="6e152-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="6e152-125">El objeto que devuelve este método admite el método `Commit()` que envía un valor al control que desencadenó el elemento emergente (no el control que desencadenó la llamada al método).</span><span class="sxs-lookup"><span data-stu-id="6e152-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="6e152-126">El código siguiente proporciona la fecha seleccionada como argumento para el método `Commit()`, lo que hace que el código vuelva a escribir la fecha seleccionada en el cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="6e152-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="6e152-127">Ahora, cada vez que haga clic en una fecha de calendario, la fecha seleccionada aparecerá en el cuadro de texto asociado y creará un control selector de fecha que se puede encontrar actualmente en muchos sitios Web.</span><span class="sxs-lookup"><span data-stu-id="6e152-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="6e152-128">[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6e152-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="6e152-129">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6e152-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="6e152-130">[![al hacer clic en una fecha, la coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6e152-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="6e152-131">Al hacer clic en una fecha, se coloca en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6e152-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6e152-132">[Anterior](using-multiple-popup-controls-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6e152-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
