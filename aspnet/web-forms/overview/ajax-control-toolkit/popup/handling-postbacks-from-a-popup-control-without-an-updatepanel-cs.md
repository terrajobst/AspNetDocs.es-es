---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Controlar los postbacks desde un control emergente sin un UpdatePanelC#() | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598712"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="46b13-104">Controlar los postbacks desde un control emergente sin un UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="46b13-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>

<span data-ttu-id="46b13-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="46b13-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="46b13-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="46b13-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="46b13-107">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="46b13-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="46b13-108">Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.</span><span class="sxs-lookup"><span data-stu-id="46b13-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="46b13-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="46b13-109">Overview</span></span>

<span data-ttu-id="46b13-110">El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="46b13-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="46b13-111">Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.</span><span class="sxs-lookup"><span data-stu-id="46b13-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="46b13-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="46b13-112">Steps</span></span>

<span data-ttu-id="46b13-113">Cuando se usa un `PopupControl` con un postback, pero sin tener un `UpdatePanel` en la página, el kit de herramientas de control no ofrece una manera de determinar qué elemento de cliente ha desencadenado el elemento emergente que, a su vez, causó el PostBack.</span><span class="sxs-lookup"><span data-stu-id="46b13-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="46b13-114">Sin embargo, un truco pequeño proporciona una solución alternativa para este escenario.</span><span class="sxs-lookup"><span data-stu-id="46b13-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="46b13-115">En primer lugar, esta es la configuración básica: dos cuadros de texto que desencadenan el mismo elemento emergente, un calendario.</span><span class="sxs-lookup"><span data-stu-id="46b13-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="46b13-116">Dos `PopupControlExtenders` incorporar cuadros de texto y ventanas emergentes.</span><span class="sxs-lookup"><span data-stu-id="46b13-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="46b13-117">La idea básica es agregar un campo de formulario oculto en el &lt;`form`elemento de &gt; que contiene el cuadro de texto que inició la ventana emergente:</span><span class="sxs-lookup"><span data-stu-id="46b13-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="46b13-118">Cuando se carga la página, el código JavaScript agrega un controlador de eventos a ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, su nombre se escribe en el campo de formulario oculto:</span><span class="sxs-lookup"><span data-stu-id="46b13-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="46b13-119">En el código del lado servidor, se debe leer el valor del campo oculto.</span><span class="sxs-lookup"><span data-stu-id="46b13-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="46b13-120">Dado que los campos de formulario ocultos son triviales de manipular, es necesario un enfoque de lista blanca para validar el valor oculto.</span><span class="sxs-lookup"><span data-stu-id="46b13-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="46b13-121">Una vez identificado el cuadro de texto correcto, se escribe en él la fecha del calendario.</span><span class="sxs-lookup"><span data-stu-id="46b13-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

<span data-ttu-id="46b13-122">[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="46b13-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="46b13-123">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="46b13-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="46b13-124">[![al hacer clic en una fecha, la coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="46b13-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="46b13-125">Al hacer clic en una fecha, se coloca en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="46b13-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="46b13-126">[Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Siguiente](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="46b13-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
