---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Usar postback automático con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574453"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="54ee9-103">Usar el postback automático con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="54ee9-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>

<span data-ttu-id="54ee9-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="54ee9-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="54ee9-105">[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="54ee9-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="54ee9-106">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="54ee9-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="54ee9-107">Sin embargo, cuando se usa el control CascadingDropDown, ASP. La característica de postback del control DropDownList de la red no funciona, puesto que la carga de datos de forma asincrónica en la lista genera un postback (innecesario).</span><span class="sxs-lookup"><span data-stu-id="54ee9-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="54ee9-108">Con algún código JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="54ee9-108">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="overview"></a><span data-ttu-id="54ee9-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="54ee9-109">Overview</span></span>

<span data-ttu-id="54ee9-110">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="54ee9-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="54ee9-111">(Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). Sin embargo, cuando se usa el control CascadingDropDown, ASP. La característica de postback del control DropDownList de la red no funciona, puesto que la carga de datos de forma asincrónica en la lista genera un postback (innecesario).</span><span class="sxs-lookup"><span data-stu-id="54ee9-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="54ee9-112">Con algún código JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="54ee9-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="54ee9-113">Pasos</span><span class="sxs-lookup"><span data-stu-id="54ee9-113">Steps</span></span>

<span data-ttu-id="54ee9-114">Para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del &lt;`form`elemento &gt;):</span><span class="sxs-lookup"><span data-stu-id="54ee9-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="54ee9-115">A continuación, se requiere un control DropDownList:</span><span class="sxs-lookup"><span data-stu-id="54ee9-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="54ee9-116">En esta lista, se agrega un extensor CascadingDropDown, que proporciona la dirección URL del servicio Web y la información del método:</span><span class="sxs-lookup"><span data-stu-id="54ee9-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="54ee9-117">A continuación, el extensor CascadingDropDown llama de forma asincrónica a un servicio Web con la siguiente firma de método:</span><span class="sxs-lookup"><span data-stu-id="54ee9-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="54ee9-118">El método devuelve una matriz de tipo CascadingDropDown Value.</span><span class="sxs-lookup"><span data-stu-id="54ee9-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="54ee9-119">El constructor del tipo espera primero el título de la entrada de la lista y, a continuación, el valor (atributo de `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="54ee9-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="54ee9-120">La carga de la página en el explorador rellenará la lista desplegable con tres proveedores y la segunda se preseleccionará.</span><span class="sxs-lookup"><span data-stu-id="54ee9-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="54ee9-121">Además, ASP.NET define el método JavaScript `__doPostBack()`.</span><span class="sxs-lookup"><span data-stu-id="54ee9-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="54ee9-122">Una vez cargada la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella.</span><span class="sxs-lookup"><span data-stu-id="54ee9-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="54ee9-123">Si no hay ningún elemento en la lista, el kit de herramientas de control lo está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y vuelve a intentarlo en un medio segundo.</span><span class="sxs-lookup"><span data-stu-id="54ee9-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="54ee9-124">De este modo, solo se ejecuta un postback cuando realmente hay elementos en la lista y el usuario selecciona una entrada.</span><span class="sxs-lookup"><span data-stu-id="54ee9-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>

<span data-ttu-id="54ee9-125">[![seleccionar un elemento de lista provoca un postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="54ee9-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="54ee9-126">Al seleccionar un elemento de lista, se produce un postback ([hacer clic para ver la imagen de tamaño completo](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="54ee9-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="54ee9-127">Anterior</span><span class="sxs-lookup"><span data-stu-id="54ee9-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
