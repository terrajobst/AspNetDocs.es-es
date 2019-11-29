---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Usar postbacks con ReorderList (C#) | Microsoft Docs
author: wenz
description: El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordena la lista, se muestra un pedido...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611394"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="e892f-104">Usar postbacks con ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="e892f-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="e892f-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e892f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e892f-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e892f-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="e892f-107">El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="e892f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="e892f-108">Cada vez que se reordene la lista, un postback informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="e892f-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="e892f-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="e892f-109">Overview</span></span>

<span data-ttu-id="e892f-110">El control `ReorderList` en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="e892f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="e892f-111">Cada vez que se reordene la lista, un postback informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="e892f-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="e892f-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="e892f-112">Steps</span></span>

<span data-ttu-id="e892f-113">Hay varios orígenes de datos posibles para el control `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="e892f-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="e892f-114">Uno es usar un control `XmlDataSource`:</span><span class="sxs-lookup"><span data-stu-id="e892f-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="e892f-115">Para enlazar este XML a un `ReorderList` control y habilitar los postbacks, se deben establecer los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="e892f-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="e892f-116">`DataSourceID`: el identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="e892f-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="e892f-117">`SortOrderField`: propiedad por la que se va a ordenar</span><span class="sxs-lookup"><span data-stu-id="e892f-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="e892f-118">`AllowReorder`: si se permite al usuario cambiar el orden de los elementos de la lista</span><span class="sxs-lookup"><span data-stu-id="e892f-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="e892f-119">`PostBackOnReorder`: si se va a crear un postback cada vez que se reorganice la lista</span><span class="sxs-lookup"><span data-stu-id="e892f-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="e892f-120">Este es el marcado adecuado para el control:</span><span class="sxs-lookup"><span data-stu-id="e892f-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="e892f-121">En el control `ReorderList`, los datos específicos del origen de datos se pueden enlazar mediante el método `Eval()`:</span><span class="sxs-lookup"><span data-stu-id="e892f-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="e892f-122">En una posición arbitraria de la página, una etiqueta contendrá la información cuando se haya producido la última reordenación:</span><span class="sxs-lookup"><span data-stu-id="e892f-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="e892f-123">Esta etiqueta se rellena con texto en el código del lado servidor, controlando el PostBack:</span><span class="sxs-lookup"><span data-stu-id="e892f-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="e892f-124">Por último, para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, se debe colocar el control `ScriptManager` en la página:</span><span class="sxs-lookup"><span data-stu-id="e892f-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

<span data-ttu-id="e892f-125">[![cada reordenación desencadena un postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e892f-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="e892f-126">Cada reordenación desencadena un postback ([haga clic para ver la imagen de tamaño completo](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e892f-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e892f-127">Siguiente</span><span class="sxs-lookup"><span data-stu-id="e892f-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
