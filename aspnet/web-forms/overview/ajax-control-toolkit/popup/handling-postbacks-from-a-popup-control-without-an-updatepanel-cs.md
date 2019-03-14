---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Controlar los Postbacks desde un Control emergente sin un UpdatePanel (C#) | Microsoft Docs
author: wenz
description: El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control. Cuando se produce un postback en unidades de búsqueda...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 3ad041b3df270e65c1dd39cf0cb6adb28b57880e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57036852"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="a0eea-104">Controlar los postbacks desde un control emergente sin un UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="a0eea-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="a0eea-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a0eea-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a0eea-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a0eea-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="a0eea-107">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="a0eea-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="a0eea-108">Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.</span><span class="sxs-lookup"><span data-stu-id="a0eea-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="a0eea-109">Información general</span><span class="sxs-lookup"><span data-stu-id="a0eea-109">Overview</span></span>

<span data-ttu-id="a0eea-110">El extensor PopupControl en AJAX Control Toolkit ofrece una manera sencilla de desencadenar una ventana emergente cuando se activa cualquier otro control.</span><span class="sxs-lookup"><span data-stu-id="a0eea-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="a0eea-111">Cuando se produce un postback en ese panel y hay varios paneles en la página es difícil determinar qué panel se ha hecho clic.</span><span class="sxs-lookup"><span data-stu-id="a0eea-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="a0eea-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="a0eea-112">Steps</span></span>

<span data-ttu-id="a0eea-113">Cuando se usa un `PopupControl` con una devolución de datos, pero sin tener un `UpdatePanel` en la página, el Kit de herramientas de Control no ofrece una manera de determinar qué elemento de cliente ha desencadenado la ventana emergente que a su vez la originaron.</span><span class="sxs-lookup"><span data-stu-id="a0eea-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="a0eea-114">Sin embargo, un pequeño truco proporciona una solución alternativa para este escenario.</span><span class="sxs-lookup"><span data-stu-id="a0eea-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="a0eea-115">En primer lugar, aquí es la configuración básica: dos cuadros de texto que desencadenen la mismo ventana emergente, un calendario.</span><span class="sxs-lookup"><span data-stu-id="a0eea-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="a0eea-116">Dos `PopupControlExtenders` reunir los cuadros de texto y el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="a0eea-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="a0eea-117">La idea básica consiste en Agregar un campo de formulario oculto en el &lt; `form` &gt; elemento que contiene el cuadro de texto que se inicia la ventana emergente:</span><span class="sxs-lookup"><span data-stu-id="a0eea-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="a0eea-118">Cuando se carga la página, el código de JavaScript agrega un controlador de eventos en ambos cuadros de texto: Cada vez que el usuario hace clic en un cuadro de texto, se escribe su nombre en el campo de formulario oculto:</span><span class="sxs-lookup"><span data-stu-id="a0eea-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="a0eea-119">En el código del lado servidor, se debe leer el valor del campo oculto.</span><span class="sxs-lookup"><span data-stu-id="a0eea-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="a0eea-120">Puesto que son triviales para manipular campos ocultos de formulario, se requiere un enfoque de lista de permitidos para validar el valor hidden.</span><span class="sxs-lookup"><span data-stu-id="a0eea-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="a0eea-121">Una vez identificado el cuadro de texto correcto, la fecha del calendario se escribe en él.</span><span class="sxs-lookup"><span data-stu-id="a0eea-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="a0eea-122">[![El calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a0eea-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="a0eea-123">El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a0eea-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="a0eea-124">[![Al hacer clic en una fecha, coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="a0eea-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="a0eea-125">Al hacer clic en una fecha, coloca en el cuadro de texto ([haga clic aquí para ver imagen en tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a0eea-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0eea-126">[Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Siguiente](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a0eea-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
