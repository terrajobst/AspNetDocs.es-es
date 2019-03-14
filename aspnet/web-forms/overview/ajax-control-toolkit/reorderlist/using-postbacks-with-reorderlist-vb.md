---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usar Postbacks con ReorderList (VB) | Microsoft Docs
author: wenz
description: El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Cada vez que se puede reordenar la lista, un pedido de compra...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e2124327a36c94db8bafbdf91f767068ac7e834
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025432"
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="d0507-104">Usar postbacks con ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="d0507-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="d0507-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d0507-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d0507-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d0507-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="d0507-107">El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="d0507-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="d0507-108">Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="d0507-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="d0507-109">Información general</span><span class="sxs-lookup"><span data-stu-id="d0507-109">Overview</span></span>

<span data-ttu-id="d0507-110">El `ReorderList` control de AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="d0507-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="d0507-111">Cada vez que se puede reordenar la lista, una devolución informará al servidor del cambio.</span><span class="sxs-lookup"><span data-stu-id="d0507-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="d0507-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="d0507-112">Steps</span></span>

<span data-ttu-id="d0507-113">Hay varios orígenes de datos posibles para el `ReorderList` control.</span><span class="sxs-lookup"><span data-stu-id="d0507-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="d0507-114">Una consiste en usar un `XmlDataSource` control:</span><span class="sxs-lookup"><span data-stu-id="d0507-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="d0507-115">Para enlazar este XML a un `ReorderList` se deben establecer el control y habilitar las devoluciones de datos, los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="d0507-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="d0507-116">`DataSourceID`: El identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="d0507-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="d0507-117">`SortOrderField`: La propiedad para ordenar por</span><span class="sxs-lookup"><span data-stu-id="d0507-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="d0507-118">`AllowReorder`: Si se permite al usuario reordenar los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="d0507-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="d0507-119">`PostBackOnReorder`: Si desea crear una devolución de datos cada vez que se reorganiza la lista</span><span class="sxs-lookup"><span data-stu-id="d0507-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="d0507-120">Este es el formato adecuado para el control:</span><span class="sxs-lookup"><span data-stu-id="d0507-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="d0507-121">Dentro de la `ReorderList` control, los datos específicos del origen de datos puede estar enlazado con el `Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="d0507-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="d0507-122">En una posición arbitraria en la página, una etiqueta contendrá la información cuando ocurrió la última reordenación:</span><span class="sxs-lookup"><span data-stu-id="d0507-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="d0507-123">Esta etiqueta se rellena con texto en el código de servidor, que controla la devolución de datos:</span><span class="sxs-lookup"><span data-stu-id="d0507-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="d0507-124">Por último, para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en la página:</span><span class="sxs-lookup"><span data-stu-id="d0507-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="d0507-125">[![Cada reordenación desencadena una devolución de datos](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0507-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="d0507-126">Cada reordenación desencadena una devolución de datos ([haga clic aquí para ver imagen en tamaño completo](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d0507-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d0507-127">[Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Siguiente](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d0507-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>