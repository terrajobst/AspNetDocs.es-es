---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Controlar los postbacks desde un ModalPopupC#() | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Se debe tener especial cuidado cuando un PDV...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599094"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="4c2c7-104">Controlar postbacks desde un ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="4c2c7-104">Handling Postbacks from a ModalPopup (C#)</span></span>

<span data-ttu-id="4c2c7-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4c2c7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4c2c7-106">[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4c2c7-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="4c2c7-107">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4c2c7-108">Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="4c2c7-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="4c2c7-109">Overview</span></span>

<span data-ttu-id="4c2c7-110">El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="4c2c7-111">Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="4c2c7-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="4c2c7-112">Steps</span></span>

<span data-ttu-id="4c2c7-113">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="4c2c7-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="4c2c7-114">A continuación, agregue un panel que actúe como elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="4c2c7-115">Allí, el usuario puede escribir un nombre y una dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="4c2c7-116">Se usa un botón para cerrar el elemento emergente y guardar la información.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="4c2c7-117">Tenga en cuenta que el atributo `OnClick` está establecido para que se produzca un postback al hacer clic en este botón:</span><span class="sxs-lookup"><span data-stu-id="4c2c7-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="4c2c7-118">La propia página consta de dos etiquetas para exactamente la misma información: nombre y dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="4c2c7-119">Se usa un botón para desencadenar el elemento emergente modal:</span><span class="sxs-lookup"><span data-stu-id="4c2c7-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="4c2c7-120">Para que aparezca el elemento emergente, agregue el control `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="4c2c7-121">Establezca el atributo `PopupControlID` en el identificador del panel y `TargetControlID` en el identificador del botón:</span><span class="sxs-lookup"><span data-stu-id="4c2c7-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="4c2c7-122">Ahora, siempre que se haga clic en el botón `Save` en el menú emergente modal, se ejecutará el método de `SaveData()` del servidor.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="4c2c7-123">Allí, puede guardar los datos especificados en un almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="4c2c7-124">Por simplicidad, los nuevos datos se generan en la etiqueta:</span><span class="sxs-lookup"><span data-stu-id="4c2c7-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="4c2c7-125">Además, los controles de cuadro de texto dentro del elemento emergente modal deben rellenarse con el nombre y el correo electrónico actuales.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="4c2c7-126">Sin embargo, esto solo es necesario cuando no se produce un postback.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="4c2c7-127">Si hay un postback, la característica ASP.NET ViewState rellenará automáticamente los cuadros de texto con los valores adecuados.</span><span class="sxs-lookup"><span data-stu-id="4c2c7-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

<span data-ttu-id="4c2c7-128">[![el elemento emergente modal produce un postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4c2c7-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="4c2c7-129">El elemento emergente modal produce un postback ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4c2c7-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4c2c7-130">[Anterior](using-modalpopup-with-a-repeater-control-cs.md)
> [Siguiente](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4c2c7-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
