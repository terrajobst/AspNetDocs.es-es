---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: crear la Página principal | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484453"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="ef328-102">Parte 7: creación de la Página principal</span><span class="sxs-lookup"><span data-stu-id="ef328-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="ef328-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef328-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ef328-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="ef328-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="ef328-105">Crear la página principal</span><span class="sxs-lookup"><span data-stu-id="ef328-105">Creating the Main Page</span></span>

<span data-ttu-id="ef328-106">En esta sección, creará la Página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef328-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="ef328-107">Esta página será más compleja que la página Administrador, por lo que nos centraremos en varios pasos.</span><span class="sxs-lookup"><span data-stu-id="ef328-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="ef328-108">A lo largo del proceso, verá algunas técnicas más avanzadas de Knockout. js.</span><span class="sxs-lookup"><span data-stu-id="ef328-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="ef328-109">Este es el diseño básico de la página:</span><span class="sxs-lookup"><span data-stu-id="ef328-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="ef328-110">"Productos" contiene una matriz de productos.</span><span class="sxs-lookup"><span data-stu-id="ef328-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="ef328-111">"Cart" contiene una matriz de productos con cantidades.</span><span class="sxs-lookup"><span data-stu-id="ef328-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="ef328-112">Al hacer clic en "agregar al carro", se actualiza el carro.</span><span class="sxs-lookup"><span data-stu-id="ef328-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="ef328-113">"Orders" contiene una matriz de identificadores de pedido.</span><span class="sxs-lookup"><span data-stu-id="ef328-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="ef328-114">"Detalles" contiene un detalle del pedido, que es una matriz de elementos (productos con cantidades)</span><span class="sxs-lookup"><span data-stu-id="ef328-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="ef328-115">Comenzaremos definiendo un diseño básico en HTML, sin enlace de datos ni scripts.</span><span class="sxs-lookup"><span data-stu-id="ef328-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="ef328-116">Abra el archivo views/home/index. cshtml y reemplace todo el contenido por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef328-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="ef328-117">A continuación, agregue una sección scripts y cree un modelo de vista vacía:</span><span class="sxs-lookup"><span data-stu-id="ef328-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="ef328-118">Según el diseño esbozado anteriormente, nuestro modelo de vista necesita observables para productos, carros, pedidos y detalles.</span><span class="sxs-lookup"><span data-stu-id="ef328-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="ef328-119">Agregue las siguientes variables al objeto `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="ef328-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="ef328-120">Los usuarios pueden agregar elementos de la lista de productos al carro y quitar elementos del carro.</span><span class="sxs-lookup"><span data-stu-id="ef328-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="ef328-121">Para encapsular estas funciones, vamos a crear otra clase de modelo de vista que representa un producto.</span><span class="sxs-lookup"><span data-stu-id="ef328-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="ef328-122">Agrega el código siguiente a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="ef328-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="ef328-123">La clase `ProductViewModel` contiene dos funciones que se usan para trasladar el producto hacia y desde el carro: `addItemToCart` agrega una unidad del producto al carro y `removeAllFromCart` quita todas las cantidades del producto.</span><span class="sxs-lookup"><span data-stu-id="ef328-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="ef328-124">Los usuarios pueden seleccionar un pedido existente y obtener los detalles del pedido.</span><span class="sxs-lookup"><span data-stu-id="ef328-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="ef328-125">Encapsularemos esta funcionalidad en otro modelo de vista:</span><span class="sxs-lookup"><span data-stu-id="ef328-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="ef328-126">El `OrderDetailsViewModel` se inicializa con un pedido y captura los detalles del pedido mediante el envío de una solicitud AJAX al servidor.</span><span class="sxs-lookup"><span data-stu-id="ef328-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="ef328-127">Observe también la propiedad `total` en el `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ef328-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="ef328-128">Esta propiedad es una clase especial de observable denominada [observable calculada](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="ef328-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="ef328-129">Como implica el nombre, un objeto observable calculado permite enlazar datos a un valor&#8212;calculado en este caso, el costo total del pedido.</span><span class="sxs-lookup"><span data-stu-id="ef328-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="ef328-130">A continuación, agregue estas funciones a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="ef328-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="ef328-131">`resetCart` quita todos los elementos del carro.</span><span class="sxs-lookup"><span data-stu-id="ef328-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="ef328-132">`getDetails` obtiene los detalles de un pedido (insertando un nuevo `OrderDetailsViewModel` en la lista de `details`).</span><span class="sxs-lookup"><span data-stu-id="ef328-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="ef328-133">`createOrder` crea un nuevo pedido y vacía el carro.</span><span class="sxs-lookup"><span data-stu-id="ef328-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="ef328-134">Por último, inicialice el modelo de vista realizando solicitudes AJAX para los productos y pedidos:</span><span class="sxs-lookup"><span data-stu-id="ef328-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="ef328-135">Bien, eso es una gran cantidad de código, pero lo compilamos paso a paso, por lo que espero que el diseño esté claro.</span><span class="sxs-lookup"><span data-stu-id="ef328-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="ef328-136">Ahora podemos agregar algunos enlaces Knockout. js al código HTML.</span><span class="sxs-lookup"><span data-stu-id="ef328-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="ef328-137">**Productos**</span><span class="sxs-lookup"><span data-stu-id="ef328-137">**Products**</span></span>

<span data-ttu-id="ef328-138">Estos son los enlaces para la lista de productos:</span><span class="sxs-lookup"><span data-stu-id="ef328-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="ef328-139">Esto recorre en iteración la matriz de productos y muestra el nombre y el precio.</span><span class="sxs-lookup"><span data-stu-id="ef328-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="ef328-140">El botón "Agregar a pedido" solo está visible cuando el usuario ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="ef328-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="ef328-141">El botón "Agregar a pedido" llama `addItemToCart` en la instancia de `ProductViewModel` para el producto.</span><span class="sxs-lookup"><span data-stu-id="ef328-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="ef328-142">Esto muestra una interesante característica de Knockout. js: cuando un modelo de vista contiene otros modelos de vista, puede aplicar los enlaces al modelo interno.</span><span class="sxs-lookup"><span data-stu-id="ef328-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="ef328-143">En este ejemplo, los enlaces dentro del `foreach` se aplican a cada una de las instancias de `ProductViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ef328-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="ef328-144">Este enfoque es mucho más limpio que poner toda la funcionalidad en un único modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="ef328-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="ef328-145">**Maletín**</span><span class="sxs-lookup"><span data-stu-id="ef328-145">**Cart**</span></span>

<span data-ttu-id="ef328-146">Estos son los enlaces para el carro:</span><span class="sxs-lookup"><span data-stu-id="ef328-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="ef328-147">Esto recorre en iteración la matriz del carro y muestra el nombre, el precio y la cantidad.</span><span class="sxs-lookup"><span data-stu-id="ef328-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="ef328-148">Tenga en cuenta que el vínculo "quitar" y el botón "crear pedido" están enlazados a las funciones de modelo de vista.</span><span class="sxs-lookup"><span data-stu-id="ef328-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="ef328-149">**Pedidos**</span><span class="sxs-lookup"><span data-stu-id="ef328-149">**Orders**</span></span>

<span data-ttu-id="ef328-150">Estos son los enlaces para la lista de pedidos:</span><span class="sxs-lookup"><span data-stu-id="ef328-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="ef328-151">Esto recorre en iteración los pedidos y muestra el ID. de pedido.</span><span class="sxs-lookup"><span data-stu-id="ef328-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="ef328-152">El evento click del vínculo se enlaza a la función `getDetails`.</span><span class="sxs-lookup"><span data-stu-id="ef328-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="ef328-153">**Detalles del pedido**</span><span class="sxs-lookup"><span data-stu-id="ef328-153">**Order Details**</span></span>

<span data-ttu-id="ef328-154">Estos son los enlaces de los detalles del pedido:</span><span class="sxs-lookup"><span data-stu-id="ef328-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="ef328-155">Esto recorre en iteración los elementos en el orden y muestra el producto, el precio y la cantidad.</span><span class="sxs-lookup"><span data-stu-id="ef328-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="ef328-156">El div circundante solo es visible si la matriz de detalles contiene uno o varios elementos.</span><span class="sxs-lookup"><span data-stu-id="ef328-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ef328-157">Conclusión</span><span class="sxs-lookup"><span data-stu-id="ef328-157">Conclusion</span></span>

<span data-ttu-id="ef328-158">En este tutorial, ha creado una aplicación que usa Entity Framework para comunicarse con la base de datos y ASP.NET Web API para proporcionar una interfaz de acceso público en la parte superior de la capa de datos.</span><span class="sxs-lookup"><span data-stu-id="ef328-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="ef328-159">Usamos ASP.NET MVC 4 para presentar las páginas HTML y Knockout. js más jQuery para proporcionar interacciones dinámicas sin recargas de páginas.</span><span class="sxs-lookup"><span data-stu-id="ef328-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="ef328-160">Recursos adicionales:</span><span class="sxs-lookup"><span data-stu-id="ef328-160">Additional resources:</span></span>

- [<span data-ttu-id="ef328-161">Mapa de contenido de ASP.NET Data Access</span><span class="sxs-lookup"><span data-stu-id="ef328-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="ef328-162">Centro para desarrolladores de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ef328-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="ef328-163">Anterior</span><span class="sxs-lookup"><span data-stu-id="ef328-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
