---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: creación de una interfaz de usuario dinámica con Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447865"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="10858-102">Parte 5: creación de una interfaz de usuario dinámica con Knockout. js</span><span class="sxs-lookup"><span data-stu-id="10858-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="10858-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="10858-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="10858-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="10858-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="10858-105">Crear una IU dinámica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="10858-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="10858-106">En esta sección, usaremos Knockout. js para agregar funcionalidad a la vista de administración.</span><span class="sxs-lookup"><span data-stu-id="10858-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="10858-107">[Knockout. js](http://knockoutjs.com/) es una biblioteca de JavaScript que facilita el enlace de controles HTML a los datos.</span><span class="sxs-lookup"><span data-stu-id="10858-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="10858-108">Knockout. js usa el patrón Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="10858-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="10858-109">El *modelo* es la representación del lado servidor de los datos en el dominio empresarial (en nuestro caso, productos y pedidos).</span><span class="sxs-lookup"><span data-stu-id="10858-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="10858-110">La *vista* es el nivel de presentación (html).</span><span class="sxs-lookup"><span data-stu-id="10858-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="10858-111">El *modelo de vista* es un objeto de JavaScript que contiene los datos del modelo.</span><span class="sxs-lookup"><span data-stu-id="10858-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="10858-112">El modelo de vista es una abstracción de código de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="10858-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="10858-113">No tiene ningún conocimiento de la representación en HTML.</span><span class="sxs-lookup"><span data-stu-id="10858-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="10858-114">En su lugar, representa características abstractas de la vista, como "una lista de elementos".</span><span class="sxs-lookup"><span data-stu-id="10858-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="10858-115">La vista está enlazada a datos con el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="10858-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="10858-116">Las actualizaciones del modelo de vista se reflejan automáticamente en la vista.</span><span class="sxs-lookup"><span data-stu-id="10858-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="10858-117">El modelo de vista también obtiene los eventos de la vista, como los clics de botón, y realiza operaciones en el modelo, como la creación de un pedido.</span><span class="sxs-lookup"><span data-stu-id="10858-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="10858-118">En primer lugar, vamos a definir el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="10858-118">First we'll define the view-model.</span></span> <span data-ttu-id="10858-119">Después, enlazaremos el marcado HTML al modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="10858-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="10858-120">Agregue la siguiente sección de Razor a admin. cshtml:</span><span class="sxs-lookup"><span data-stu-id="10858-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="10858-121">Puede Agregar esta sección en cualquier parte del archivo.</span><span class="sxs-lookup"><span data-stu-id="10858-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="10858-122">Cuando se representa la vista, la sección aparece en la parte inferior de la página HTML, justo antes de la etiqueta de cierre &lt;/Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="10858-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="10858-123">Todo el script de esta página irá dentro de la etiqueta de script indicada por el comentario:</span><span class="sxs-lookup"><span data-stu-id="10858-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="10858-124">En primer lugar, defina una clase de modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="10858-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="10858-125">**Ko. observableArray** es un tipo especial de objeto en Knockout, denominado *observable*.</span><span class="sxs-lookup"><span data-stu-id="10858-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="10858-126">En la [documentación de Knockout. js](http://knockoutjs.com/documentation/observables.html): un objeto observable es un "objeto de JavaScript que puede notificar los cambios a los suscriptores".</span><span class="sxs-lookup"><span data-stu-id="10858-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="10858-127">Cuando el contenido de un objeto observable cambia, la vista se actualiza automáticamente para que coincida.</span><span class="sxs-lookup"><span data-stu-id="10858-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="10858-128">Para rellenar la matriz de `products`, realice una solicitud AJAX a la API Web.</span><span class="sxs-lookup"><span data-stu-id="10858-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="10858-129">Recuerde que hemos almacenado el URI base para la API en el contenedor de vistas (consulte la [parte 4](using-web-api-with-entity-framework-part-4.md) del tutorial).</span><span class="sxs-lookup"><span data-stu-id="10858-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="10858-130">A continuación, agregue funciones al modelo de vista para crear, actualizar y eliminar productos.</span><span class="sxs-lookup"><span data-stu-id="10858-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="10858-131">Estas funciones envían llamadas AJAX a la API Web y usan los resultados para actualizar el modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="10858-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="10858-132">Ahora la parte más importante: cuando el DOM está lleno, llame a la función **Ko. applyBindings** y pase una nueva instancia de la `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="10858-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="10858-133">El método **Ko. applyBindings** activa la cobertura y conecta el modelo de vista a la vista.</span><span class="sxs-lookup"><span data-stu-id="10858-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="10858-134">Ahora que tenemos un modelo de vista, podemos crear los enlaces.</span><span class="sxs-lookup"><span data-stu-id="10858-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="10858-135">En Knockout. js, esto se hace agregando `data-bind` atributos a los elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="10858-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="10858-136">Por ejemplo, para enlazar una lista HTML a una matriz, use el enlace de `foreach`:</span><span class="sxs-lookup"><span data-stu-id="10858-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="10858-137">El enlace de `foreach` recorre en iteración la matriz y crea elementos secundarios para cada objeto de la matriz.</span><span class="sxs-lookup"><span data-stu-id="10858-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="10858-138">Los enlaces de los elementos secundarios pueden hacer referencia a las propiedades de los objetos de matriz.</span><span class="sxs-lookup"><span data-stu-id="10858-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="10858-139">Agregue los siguientes enlaces a la lista "Update-Products":</span><span class="sxs-lookup"><span data-stu-id="10858-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="10858-140">El elemento `<li>` aparece dentro del ámbito del enlace **foreach** .</span><span class="sxs-lookup"><span data-stu-id="10858-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="10858-141">Esto significa que la cobertura representará el elemento una vez por cada producto de la matriz de `products`.</span><span class="sxs-lookup"><span data-stu-id="10858-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="10858-142">Todos los enlaces dentro del elemento `<li>` hacen referencia a esa instancia del producto.</span><span class="sxs-lookup"><span data-stu-id="10858-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="10858-143">Por ejemplo, `$data.Name` hace referencia a la propiedad `Name` del producto.</span><span class="sxs-lookup"><span data-stu-id="10858-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="10858-144">Para establecer los valores de las entradas de texto, utilice el enlace de `value`.</span><span class="sxs-lookup"><span data-stu-id="10858-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="10858-145">Los botones se enlazan a las funciones de la vista modelo, mediante el enlace de `click`.</span><span class="sxs-lookup"><span data-stu-id="10858-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="10858-146">La instancia del producto se pasa como un parámetro a cada función.</span><span class="sxs-lookup"><span data-stu-id="10858-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="10858-147">Para obtener más información, la [documentación de Knockout. js](http://knockoutjs.com/documentation/observables.html) contiene buenas descripciones de los distintos enlaces.</span><span class="sxs-lookup"><span data-stu-id="10858-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="10858-148">A continuación, agregue un enlace para el evento de **envío** en el formulario agregar producto:</span><span class="sxs-lookup"><span data-stu-id="10858-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="10858-149">Este enlace llama a la función `create` en el modelo de vista para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="10858-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="10858-150">Este es el código completo de la vista de administración:</span><span class="sxs-lookup"><span data-stu-id="10858-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="10858-151">Ejecute la aplicación, inicie sesión con la cuenta de administrador y haga clic en el vínculo "admin".</span><span class="sxs-lookup"><span data-stu-id="10858-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="10858-152">Debería ver la lista de productos y poder crear, actualizar o eliminar productos.</span><span class="sxs-lookup"><span data-stu-id="10858-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10858-153">[Anterior](using-web-api-with-entity-framework-part-4.md)
> [Siguiente](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="10858-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
