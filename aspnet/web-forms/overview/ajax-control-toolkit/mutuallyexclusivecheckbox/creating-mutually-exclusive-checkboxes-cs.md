---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Crear casillas de verificación mutuamente excluyentes (C#) | Microsoft Docs
author: wenz
description: 'Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio. Sin embargo, hay un inconveniente: cuando se selecciona un botón de radio en un grupo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496789"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="613e6-104">Crear casillas de verificación mutuamente excluyentes (C#)</span><span class="sxs-lookup"><span data-stu-id="613e6-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>

<span data-ttu-id="613e6-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="613e6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="613e6-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="613e6-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="613e6-107">Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="613e6-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="613e6-108">Sin embargo, hay un inconveniente: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="613e6-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="613e6-109">Las casillas pueden desactivarse en cualquier momento; sin embargo, no se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="613e6-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="613e6-110">En este tutorial se ofrece lo mejor de ambos enfoques: las casillas que son mutuamente excluyentes.</span><span class="sxs-lookup"><span data-stu-id="613e6-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="613e6-111">Información general</span><span class="sxs-lookup"><span data-stu-id="613e6-111">Overview</span></span>

<span data-ttu-id="613e6-112">Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="613e6-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="613e6-113">Sin embargo, hay un inconveniente: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio.</span><span class="sxs-lookup"><span data-stu-id="613e6-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="613e6-114">Las casillas pueden desactivarse en cualquier momento; sin embargo, no se excluyen mutuamente.</span><span class="sxs-lookup"><span data-stu-id="613e6-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="613e6-115">En este tutorial se ofrece lo mejor de ambos enfoques: las casillas que son mutuamente excluyentes.</span><span class="sxs-lookup"><span data-stu-id="613e6-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="613e6-116">Pasos</span><span class="sxs-lookup"><span data-stu-id="613e6-116">Steps</span></span>

<span data-ttu-id="613e6-117">El kit de herramientas de control de AJAX de ASP.NET contiene el extensor MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="613e6-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="613e6-118">Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="613e6-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="613e6-119">En todas las casillas del mismo grupo, solo se puede seleccionar una al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="613e6-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="613e6-120">Comencemos con la colocación de dos casillas en una nueva página de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="613e6-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="613e6-121">Puede haber más, pero dos de ellos bastan para demostrar el principio:</span><span class="sxs-lookup"><span data-stu-id="613e6-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="613e6-122">En ambas casillas, se debe colocar un control MutuallyExclusiveCheckBoxExtender en la página.</span><span class="sxs-lookup"><span data-stu-id="613e6-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="613e6-123">Ambos atributos de clave deben tener el mismo valor, al igual que los atributos de valor de los elementos de botón de radio HTML deben ser idénticos para indicar el grupo al que pertenecen.</span><span class="sxs-lookup"><span data-stu-id="613e6-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="613e6-124">La propiedad TargetControlID del objeto extender apunta al identificador de la casilla.</span><span class="sxs-lookup"><span data-stu-id="613e6-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="613e6-125">Por último, incluya el `ScriptManager` de AJAX de ASP.NET que requieran todos los elementos de ASP.NET AJAX control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="613e6-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="613e6-126">Guardar y ejecutar la página: puede activar y desactivar ambas casillas; sin embargo, en ningún momento puede activar ambas casillas.</span><span class="sxs-lookup"><span data-stu-id="613e6-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="613e6-127">[![solo se puede comprobar una casilla a la vez](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="613e6-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="613e6-128">Solo se puede comprobar una casilla a la vez ([haga clic para ver la imagen de tamaño completo](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="613e6-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="613e6-129">Siguiente</span><span class="sxs-lookup"><span data-stu-id="613e6-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
