---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Rellenar una lista conC#CascadingDropDown () | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: b5e9874fb5b6d3e55c8af5b85d12bf1ffacc116b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574842"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="32c24-103">Rellenar una lista con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="32c24-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="32c24-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="32c24-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="32c24-105">[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="32c24-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="32c24-106">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="32c24-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="32c24-107">(Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). El primer desafío que resolver es realmente rellenar una lista desplegable con este control.</span><span class="sxs-lookup"><span data-stu-id="32c24-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="32c24-108">Información general del</span><span class="sxs-lookup"><span data-stu-id="32c24-108">Overview</span></span>

<span data-ttu-id="32c24-109">El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList.</span><span class="sxs-lookup"><span data-stu-id="32c24-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="32c24-110">(Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). El primer desafío que resolver es realmente rellenar una lista desplegable con este control.</span><span class="sxs-lookup"><span data-stu-id="32c24-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="32c24-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="32c24-111">Steps</span></span>

<span data-ttu-id="32c24-112">Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="32c24-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="32c24-113">A continuación, se requiere un control DropDownList:</span><span class="sxs-lookup"><span data-stu-id="32c24-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="32c24-114">En esta lista, se agrega un extensor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="32c24-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="32c24-115">Enviará una solicitud asincrónica a un servicio Web que, a continuación, devolverá una lista de entradas que se mostrarán en la lista.</span><span class="sxs-lookup"><span data-stu-id="32c24-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="32c24-116">Para que esto funcione, es necesario establecer los siguientes atributos de CascadingDropDown:</span><span class="sxs-lookup"><span data-stu-id="32c24-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="32c24-117">`ServicePath`: dirección URL de un servicio Web que entrega las entradas de la lista</span><span class="sxs-lookup"><span data-stu-id="32c24-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="32c24-118">`ServiceMethod`: método Web que entrega las entradas de la lista</span><span class="sxs-lookup"><span data-stu-id="32c24-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="32c24-119">`TargetControlID`: ID. de la lista desplegable</span><span class="sxs-lookup"><span data-stu-id="32c24-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="32c24-120">`Category`: información de categoría que se envía al método Web cuando se llama</span><span class="sxs-lookup"><span data-stu-id="32c24-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="32c24-121">`PromptText`: texto que se muestra cuando se cargan datos de lista de forma asincrónica desde el servidor</span><span class="sxs-lookup"><span data-stu-id="32c24-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="32c24-122">Este es el marcado del elemento `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="32c24-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="32c24-123">La única diferencia entre C# y VB es el nombre del servicio web asociado:</span><span class="sxs-lookup"><span data-stu-id="32c24-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="32c24-124">El código JavaScript procedente del extensor `CascadingDropDown` llama a un método de servicio Web con la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="32c24-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="32c24-125">Por lo tanto, el aspecto importante es que el método debe devolver una matriz de tipo `CascadingDropDownNameValue` (definida por el kit de herramientas de control de AJAX de ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="32c24-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="32c24-126">En el constructor `CascadingDropDownNameValue`, primero se debe proporcionar el texto de la entrada de la lista y, a continuación, su valor, tal como lo haría `<option value="VALUE">NAME</option>` en HTML.</span><span class="sxs-lookup"><span data-stu-id="32c24-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="32c24-127">Estos son algunos datos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32c24-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="32c24-128">Si carga la página en el explorador, la lista se rellenará con tres proveedores.</span><span class="sxs-lookup"><span data-stu-id="32c24-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="32c24-129">[![la lista se rellena automáticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="32c24-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="32c24-130">La lista se rellena automáticamente ([haga clic para ver la imagen de tamaño completo](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="32c24-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="32c24-131">Siguiente</span><span class="sxs-lookup"><span data-stu-id="32c24-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
