---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Crear la vista (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448987"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="db116-102">Crear la vista (IU)</span><span class="sxs-lookup"><span data-stu-id="db116-102">Create the View (UI)</span></span>

<span data-ttu-id="db116-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="db116-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="db116-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="db116-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="db116-105">En esta sección, comenzará a definir el código HTML de la aplicación y agregará el enlace de datos entre el código HTML y el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="db116-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="db116-106">Abra el archivo views/home/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="db116-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="db116-107">Reemplace todo el contenido de ese archivo por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="db116-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="db116-108">La mayoría de los elementos de `div` están ahí para el estilo [bootstrap](http://getbootstrap.com/) .</span><span class="sxs-lookup"><span data-stu-id="db116-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="db116-109">Los elementos importantes son los que tienen atributos `data-bind`.</span><span class="sxs-lookup"><span data-stu-id="db116-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="db116-110">Este atributo vincula el código HTML al modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="db116-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="db116-111">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="db116-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="db116-112">En este ejemplo, la &quot;`text`&quot; enlace hace que el elemento `<p>` muestre el valor de la propiedad `error` del modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="db116-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="db116-113">Recuerde que `error` se declaró como `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="db116-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="db116-114">Siempre que se asigna un nuevo valor a `error`, el knockout actualiza el texto del elemento `<p>`.</span><span class="sxs-lookup"><span data-stu-id="db116-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="db116-115">En el enlace de `foreach` se indica la cobertura para recorrer el contenido de la matriz de `books`.</span><span class="sxs-lookup"><span data-stu-id="db116-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="db116-116">Para cada elemento de la matriz, el knockout crea un nuevo elemento &lt;Li&gt;.</span><span class="sxs-lookup"><span data-stu-id="db116-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="db116-117">Los enlaces dentro del contexto de la `foreach` hacen referencia a las propiedades del elemento de la matriz.</span><span class="sxs-lookup"><span data-stu-id="db116-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="db116-118">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="db116-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="db116-119">Aquí, el enlace de `text` lee la propiedad Author de cada libro.</span><span class="sxs-lookup"><span data-stu-id="db116-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="db116-120">Si ejecuta la aplicación ahora, debería tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="db116-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="db116-121">La lista de libros se carga de forma asincrónica, después de que se cargue la página.</span><span class="sxs-lookup"><span data-stu-id="db116-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="db116-122">En este momento, los &quot;detalles&quot; vínculos no son funcionales.</span><span class="sxs-lookup"><span data-stu-id="db116-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="db116-123">Agregaremos esta funcionalidad en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="db116-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="db116-124">[Anterior](part-6.md)
> [Siguiente](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="db116-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
