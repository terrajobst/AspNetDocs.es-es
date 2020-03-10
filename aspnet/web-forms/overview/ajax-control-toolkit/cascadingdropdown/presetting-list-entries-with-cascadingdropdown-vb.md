---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Preconfigurar entradas de lista con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483811"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="1b3b8-103">Preconfigurar entradas de lista con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="1b3b8-103">Presetting List Entries with CascadingDropDown (VB)</span></span>

<span data-ttu-id="1b3b8-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1b3b8-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1b3b8-105">[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1b3b8-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="1b3b8-106">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1b3b8-107">Con un poco de código es posible que un elemento de lista esté preseleccionado una vez que los datos se hayan cargado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="overview"></a><span data-ttu-id="1b3b8-108">Información general</span><span class="sxs-lookup"><span data-stu-id="1b3b8-108">Overview</span></span>

<span data-ttu-id="1b3b8-109">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="1b3b8-110">(Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). Con un poco de código es posible que un elemento de lista esté preseleccionado una vez que los datos se hayan cargado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="1b3b8-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="1b3b8-111">Steps</span></span>

<span data-ttu-id="1b3b8-112">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="1b3b8-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="1b3b8-113">A continuación, se requiere un control DropDownList:</span><span class="sxs-lookup"><span data-stu-id="1b3b8-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="1b3b8-114">En esta lista, se agrega un extensor CascadingDropDown, que proporciona la dirección URL del servicio Web y la información del método:</span><span class="sxs-lookup"><span data-stu-id="1b3b8-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="1b3b8-115">A continuación, el extensor CascadingDropDown llama de forma asincrónica a un servicio Web con la siguiente firma de método:</span><span class="sxs-lookup"><span data-stu-id="1b3b8-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="1b3b8-116">El método devuelve una matriz de tipo CascadingDropDown Value.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="1b3b8-117">El constructor del tipo espera primero el título de la entrada de la lista y, a continuación, el valor (atributo de `value` HTML).</span><span class="sxs-lookup"><span data-stu-id="1b3b8-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="1b3b8-118">Si el tercer argumento se establece en true, el elemento de lista se selecciona automáticamente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="1b3b8-119">La carga de la página en el explorador rellenará la lista desplegable con tres proveedores y la segunda se preseleccionará.</span><span class="sxs-lookup"><span data-stu-id="1b3b8-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>

<span data-ttu-id="1b3b8-120">[![la lista se rellena y preselecciona automáticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b3b8-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="1b3b8-121">La lista se rellena y se selecciona automáticamente ([haga clic para ver la imagen de tamaño completo](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1b3b8-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1b3b8-122">[Anterior](using-cascadingdropdown-with-a-database-vb.md)
> [Siguiente](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="1b3b8-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
