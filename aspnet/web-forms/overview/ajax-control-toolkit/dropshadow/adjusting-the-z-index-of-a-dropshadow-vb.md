---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustar el índice Z de una sombra (VB) | Microsoft Docs
author: wenz
description: El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Sin embargo, a veces esta sombra entra en conflicto con otros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 10495a9590ce1f25e9e3fa218ac5144268f50711
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497515"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="11a67-104">Ajustar el índice Z de un control DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="11a67-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>

<span data-ttu-id="11a67-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11a67-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11a67-106">[Descargar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="11a67-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="11a67-107">El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="11a67-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="11a67-108">Sin embargo, a veces esta sombra entra en conflicto con otros controles, por ejemplo, el control de menú ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11a67-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="11a67-109">Cuando se muestra una entrada de menú, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="11a67-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="11a67-110">Información general</span><span class="sxs-lookup"><span data-stu-id="11a67-110">Overview</span></span>

<span data-ttu-id="11a67-111">El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="11a67-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="11a67-112">Sin embargo, a veces esta sombra entra en conflicto con otros controles, por ejemplo, el control de menú ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="11a67-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="11a67-113">Cuando se muestra una entrada de menú, aparece detrás de la sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="11a67-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="11a67-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="11a67-114">Steps</span></span>

<span data-ttu-id="11a67-115">El código comienza con el panel en sí, con un texto suficiente para que el panel contenga texto suficiente para que el efecto sea visible:</span><span class="sxs-lookup"><span data-stu-id="11a67-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="11a67-116">Otro panel se coloca directamente antes del panel de `panelShadow`.</span><span class="sxs-lookup"><span data-stu-id="11a67-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="11a67-117">Contiene un menú con orientación horizontal para que las entradas de menú aparezcan encima (o en lugar de hacerlo en) en el panel de `dropShadow`):</span><span class="sxs-lookup"><span data-stu-id="11a67-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="11a67-118">A continuación, se agrega el `DropShadowExtender` para ampliar el panel de `panelShadow` con un efecto de sombra paralela:</span><span class="sxs-lookup"><span data-stu-id="11a67-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="11a67-119">Por último, el control ASP.NET AJAX `ScriptManager` permite que el kit de herramientas de control funcione:</span><span class="sxs-lookup"><span data-stu-id="11a67-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="11a67-120">Al ejecutar este script, las entradas de menú aparecen debajo del panel.</span><span class="sxs-lookup"><span data-stu-id="11a67-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="11a67-121">Sin embargo, el menú usa la clase CSS `panel` donde solo tiene que definir dos cosas para hacer que los elementos aparezcan delante del otro panel:</span><span class="sxs-lookup"><span data-stu-id="11a67-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="11a67-122">Posición relativa</span><span class="sxs-lookup"><span data-stu-id="11a67-122">Relative positioning</span></span>
- <span data-ttu-id="11a67-123">Un índice z positivo</span><span class="sxs-lookup"><span data-stu-id="11a67-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="11a67-124">Después, el control de `DropShadowExtender` no entra en conflicto con el control de menú.</span><span class="sxs-lookup"><span data-stu-id="11a67-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="11a67-125">[![antes de: la entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11a67-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="11a67-126">Antes: la entrada de menú no está visible ([haga clic para ver la imagen de tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="11a67-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>

<span data-ttu-id="11a67-127">[![después: aparece la entrada de menú](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="11a67-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="11a67-128">Después: aparece la entrada de menú ([haga clic para ver la imagen de tamaño completo](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="11a67-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11a67-129">[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Siguiente](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="11a67-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
