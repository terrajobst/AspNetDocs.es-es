---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Iniciar una ventana emergente modal desde el código del servidor (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, algunos escenarios requieren que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496963"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="97480-104">Iniciar una ventana emergente modal desde el código del servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="97480-104">Launching a Modal Popup Window from Server Code (VB)</span></span>

<span data-ttu-id="97480-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="97480-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="97480-106">[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="97480-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="97480-107">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="97480-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="97480-108">Sin embargo, algunos escenarios requieren que la apertura del elemento emergente modal se desencadene en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="97480-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="97480-109">Información general</span><span class="sxs-lookup"><span data-stu-id="97480-109">Overview</span></span>

<span data-ttu-id="97480-110">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="97480-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="97480-111">Sin embargo, algunos escenarios requieren que la apertura del elemento emergente modal se desencadene en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="97480-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="97480-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="97480-112">Steps</span></span>

<span data-ttu-id="97480-113">En primer lugar, se requiere un control Web del botón ASP.NET para demostrar cómo funciona el control ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="97480-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="97480-114">Agregue este tipo de botón en el &lt;formulario&gt; elemento en una nueva página:</span><span class="sxs-lookup"><span data-stu-id="97480-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="97480-115">A continuación, necesitará el marcado para el elemento emergente que desea crear.</span><span class="sxs-lookup"><span data-stu-id="97480-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="97480-116">Defina como un control de `<asp:Panel>` y asegúrese de que incluye un control de botón.</span><span class="sxs-lookup"><span data-stu-id="97480-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="97480-117">El control ModalPopup ofrece la funcionalidad de hacer que un botón de este tipo cierre el elemento emergente. de lo contrario, no hay ninguna manera fácil de dejar de desaparecer.</span><span class="sxs-lookup"><span data-stu-id="97480-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="97480-118">A continuación, agregue el control ModalPopup desde el kit de herramientas de ASP.NET AJAX a la página.</span><span class="sxs-lookup"><span data-stu-id="97480-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="97480-119">Establezca las propiedades del botón que carga el control, el botón que hace desaparecer y el identificador del elemento emergente real.</span><span class="sxs-lookup"><span data-stu-id="97480-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="97480-120">Como con todas las páginas web basadas en ASP.NET AJAX; el administrador de scripts es necesario para cargar las bibliotecas de JavaScript necesarias para los diferentes exploradores de destino:</span><span class="sxs-lookup"><span data-stu-id="97480-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="97480-121">Ejecute el ejemplo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="97480-121">Run the example in the browser.</span></span> <span data-ttu-id="97480-122">Al hacer clic en el botón, aparece la ventana emergente modal.</span><span class="sxs-lookup"><span data-stu-id="97480-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="97480-123">Para lograr el mismo efecto con el código del lado servidor, se requiere un botón nuevo:</span><span class="sxs-lookup"><span data-stu-id="97480-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="97480-124">Como puede ver, un clic en el botón genera un postback y ejecuta el método `ServerButton_Click()` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="97480-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="97480-125">En este método, se ejecuta una función de JavaScript llamada `launchModal()` para ser exacta; la función de JavaScript se ejecutará una vez que se haya cargado la página:</span><span class="sxs-lookup"><span data-stu-id="97480-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="97480-126">El trabajo de `launchModal()` es mostrar el ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="97480-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="97480-127">La función `launchModal()` se ejecuta una vez cargada la página HTML completa.</span><span class="sxs-lookup"><span data-stu-id="97480-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="97480-128">En ese momento, sin embargo, el marco de AJAX de ASP.NET aún no se ha cargado completamente.</span><span class="sxs-lookup"><span data-stu-id="97480-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="97480-129">Por lo tanto, la función `launchModal()` simplemente establece una variable que el control ModalPopup se debe mostrar más adelante:</span><span class="sxs-lookup"><span data-stu-id="97480-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="97480-130">La función `pageLoad()` JavaScript es una función especial que se ejecuta una vez que ASP.NET AJAX se ha cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="97480-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="97480-131">Por lo tanto, agregamos código a esta función para mostrar el control ModalPopup, pero solo si se ha llamado a `launchModal()` antes de:</span><span class="sxs-lookup"><span data-stu-id="97480-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="97480-132">La función `$find()` busca un elemento con nombre en la página y espera el identificador del lado servidor como parámetro.</span><span class="sxs-lookup"><span data-stu-id="97480-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="97480-133">Por consiguiente, `$find("mpe")` devuelve la representación del cliente del control ModalPopup; su método `show()` permite que aparezca la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="97480-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="97480-134">[![aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="97480-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="97480-135">La ventana emergente modal aparece cuando se hace clic en cualquiera de los botones ([haga clic para ver la imagen de tamaño completo](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="97480-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97480-136">[Anterior](positioning-a-modalpopup-cs.md)
> [Siguiente](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="97480-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
