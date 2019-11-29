---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Crear un control de clasificación (VB) | Microsoft Docs
author: wenz
description: Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos. Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611513"
---
# <a name="creating-a-rating-control-vb"></a><span data-ttu-id="2c157-104">Crear un control de clasificación (VB)</span><span class="sxs-lookup"><span data-stu-id="2c157-104">Creating a Rating Control (VB)</span></span>

<span data-ttu-id="2c157-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2c157-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2c157-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2c157-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="2c157-107">Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos.</span><span class="sxs-lookup"><span data-stu-id="2c157-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="2c157-108">Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos el kit de herramientas de control para nuestra eliminación.</span><span class="sxs-lookup"><span data-stu-id="2c157-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="2c157-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="2c157-109">Overview</span></span>

<span data-ttu-id="2c157-110">Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos.</span><span class="sxs-lookup"><span data-stu-id="2c157-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="2c157-111">Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos el kit de herramientas de control para nuestra eliminación.</span><span class="sxs-lookup"><span data-stu-id="2c157-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="2c157-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="2c157-112">Steps</span></span>

<span data-ttu-id="2c157-113">En primer lugar, necesita (al menos) dos tipos de imágenes: una para un elemento de calificación rellenado y otra para un elemento de clasificación vacío.</span><span class="sxs-lookup"><span data-stu-id="2c157-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="2c157-114">Un elemento de clasificación suele ser una estrella o un smiley.</span><span class="sxs-lookup"><span data-stu-id="2c157-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="2c157-115">En este escenario, encontrará tres archivos: Smiley. png y Empty. png y Smiley-done. png como parte de las descargas de código fuente de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="2c157-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="2c157-116">A continuación, cree un nuevo archivo ASP.NET y empiece por agregarle un `ScriptManager` control:</span><span class="sxs-lookup"><span data-stu-id="2c157-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2c157-117">A continuación, agregue el control `Rating` de ASP.NET AJAX control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="2c157-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="2c157-118">Se deben establecer los siguientes atributos para este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2c157-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="2c157-119">`CurrentRating` la clasificación inicial que se va a usar</span><span class="sxs-lookup"><span data-stu-id="2c157-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="2c157-120">`MaxRating` la clasificación máxima</span><span class="sxs-lookup"><span data-stu-id="2c157-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="2c157-121">`EmptyStarCssClass` la clase CSS que se va a usar cuando un elemento de clasificación (Star) esté vacío</span><span class="sxs-lookup"><span data-stu-id="2c157-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="2c157-122">`FilledStarCssClass` la clase CSS que se va a usar cuando se rellena un elemento de clasificación (Star)</span><span class="sxs-lookup"><span data-stu-id="2c157-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="2c157-123">`StarCssClass` la clase CSS que se va a usar para una estadística visible</span><span class="sxs-lookup"><span data-stu-id="2c157-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="2c157-124">`WaitingStarCssClass` la clase CSS que se va a usar mientras se envía una clasificación por estrellas al servidor</span><span class="sxs-lookup"><span data-stu-id="2c157-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="2c157-125">Y este es el marcado que crea un control de clasificación con cinco elementos (sonrisais) de los que no se rellena inicialmente ninguno:</span><span class="sxs-lookup"><span data-stu-id="2c157-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2c157-126">Las tres clases CSS a las que se hace referencia ahora deben mostrar los archivos de imagen adecuados, lo que es fácil de usar con CSS:</span><span class="sxs-lookup"><span data-stu-id="2c157-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="2c157-127">Asegúrese de proporcionar el ancho y el alto de las tres imágenes; de lo contrario, la pantalla puede parecer un poco desordenado.</span><span class="sxs-lookup"><span data-stu-id="2c157-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="2c157-128">Por último, el resultado de la clasificación debe mostrarse al usuario (o, al menos, guardarse en una base de datos).</span><span class="sxs-lookup"><span data-stu-id="2c157-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="2c157-129">Por tanto, agregue una etiqueta para la salida de un mensaje de texto y un botón de envío para devolver el formulario de clasificación al servidor:</span><span class="sxs-lookup"><span data-stu-id="2c157-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="2c157-130">En el código del lado servidor, acceda al control clasificación a través de su `ID` y, a continuación, acceda a su propiedad `CurrentRating`, que es el número de los elementos de clasificación seleccionados, en el ejemplo un valor entre 0 y 5.</span><span class="sxs-lookup"><span data-stu-id="2c157-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="2c157-131">Guarde la página y cargarla en el explorador.</span><span class="sxs-lookup"><span data-stu-id="2c157-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="2c157-132">Al mantener el mouse sobre los elementos de clasificación (inicialmente vacíos), se produce un efecto de JavaScript: la clasificación cambia.</span><span class="sxs-lookup"><span data-stu-id="2c157-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="2c157-133">Al hacer clic en el conjunto de estrellas, se conserva la clasificación actual.</span><span class="sxs-lookup"><span data-stu-id="2c157-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="2c157-134">Por último, al enviar el formulario, el código del lado servidor genera la clasificación seleccionada.</span><span class="sxs-lookup"><span data-stu-id="2c157-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="2c157-135">[![crear un sistema de clasificación con código mínimo](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2c157-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2c157-136">Creación de un sistema de clasificación con código mínimo ([haga clic para ver la imagen de tamaño completo](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2c157-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2c157-137">Anterior</span><span class="sxs-lookup"><span data-stu-id="2c157-137">Previous</span></span>](creating-a-rating-control-cs.md)
