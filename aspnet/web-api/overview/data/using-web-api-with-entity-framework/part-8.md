---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Mostrar detalles del elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449005"
---
# <a name="display-item-details"></a><span data-ttu-id="a031a-102">Mostrar detalles del elemento</span><span class="sxs-lookup"><span data-stu-id="a031a-102">Display Item Details</span></span>

<span data-ttu-id="a031a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a031a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a031a-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="a031a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a031a-105">En esta sección, agregará la capacidad de ver los detalles de cada libro.</span><span class="sxs-lookup"><span data-stu-id="a031a-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="a031a-106">En App. js, agregue al siguiente código al modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="a031a-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="a031a-107">En views/home/index. cshtml, agregue un elemento enlazado a datos al vínculo Details:</span><span class="sxs-lookup"><span data-stu-id="a031a-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="a031a-108">Esto enlaza el controlador de clic para el &lt;un elemento de&gt; a la función `getBookDetail` en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="a031a-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="a031a-109">En el mismo archivo, reemplace la siguiente marca:</span><span class="sxs-lookup"><span data-stu-id="a031a-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="a031a-110">por este:</span><span class="sxs-lookup"><span data-stu-id="a031a-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="a031a-111">Este marcado crea una tabla que está enlazada a datos con las propiedades del `detail` observable en el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="a031a-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="a031a-112">La sintaxis "&lt;!--Ko--&gt;&quot; permite incluir un enlace de cobertura fuera de un elemento DOM.</span><span class="sxs-lookup"><span data-stu-id="a031a-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="a031a-113">En este caso, el enlace de `if` hace que esta sección de marcado solo se muestre cuando `details` no es NULL.</span><span class="sxs-lookup"><span data-stu-id="a031a-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="a031a-114">Ahora, si ejecuta la aplicación y hace clic en uno de los vínculos de&quot; de detalles de &quot;, la aplicación mostrará los detalles del libro.</span><span class="sxs-lookup"><span data-stu-id="a031a-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="a031a-115">[Anterior](part-7.md)
> [Siguiente](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="a031a-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
