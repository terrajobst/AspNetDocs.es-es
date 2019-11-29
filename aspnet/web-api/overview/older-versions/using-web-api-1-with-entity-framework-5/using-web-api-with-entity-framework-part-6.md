---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: creación de controladores de productos y pedidos | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600025"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="9bdb1-102">Parte 6: creación de controladores de productos y pedidos</span><span class="sxs-lookup"><span data-stu-id="9bdb1-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="9bdb1-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9bdb1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9bdb1-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="9bdb1-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="9bdb1-105">Agregar un controlador de productos</span><span class="sxs-lookup"><span data-stu-id="9bdb1-105">Add a Products Controller</span></span>

<span data-ttu-id="9bdb1-106">El controlador de administración es para los usuarios que tienen privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="9bdb1-107">Por otra parte, los clientes pueden ver los productos pero no pueden crearlos, actualizarlos ni eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="9bdb1-108">Se puede restringir fácilmente el acceso a los métodos post, Put y DELETE, a la vez que se dejan los métodos Get abiertos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="9bdb1-109">Pero fíjese en los datos que se devuelven para un producto:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="9bdb1-110">La propiedad `ActualCost` no debe estar visible para los clientes.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="9bdb1-111">La solución consiste en definir un *objeto de transferencia de datos* (DTO) que incluya un subconjunto de propiedades que deben ser visibles para los clientes.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="9bdb1-112">Usaremos LINQ to Project `Product` instancias para `ProductDTO` instancias.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="9bdb1-113">Agregue una clase denominada `ProductDTO` a la carpeta models.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="9bdb1-114">Ahora, agregue el controlador.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-114">Now add the controller.</span></span> <span data-ttu-id="9bdb1-115">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9bdb1-116">Seleccione **Agregar**y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="9bdb1-117">En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="9bdb1-118">En **plantilla**, seleccione **controlador de API vacío**.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="9bdb1-119">Reemplace todo en el archivo de código fuente por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="9bdb1-120">El controlador sigue utilizando el `OrdersContext` para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="9bdb1-121">Pero en lugar de devolver instancias de `Product` directamente, llamamos a `MapProducts` para proyectarlas en `ProductDTO` instancias:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="9bdb1-122">El método `MapProducts` devuelve un **IQueryable**, por lo que podemos crear el resultado con otros parámetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="9bdb1-123">Puede verlo en el método `GetProduct`, que agrega una cláusula **Where** a la consulta:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="9bdb1-124">Agregar un controlador de pedidos</span><span class="sxs-lookup"><span data-stu-id="9bdb1-124">Add an Orders Controller</span></span>

<span data-ttu-id="9bdb1-125">A continuación, agregue un controlador que permita a los usuarios crear y ver los pedidos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="9bdb1-126">Comenzaremos con otro DTO.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-126">We'll start with another DTO.</span></span> <span data-ttu-id="9bdb1-127">En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos y agregue una clase denominada `OrderDTO` use la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="9bdb1-128">Ahora, agregue el controlador.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-128">Now add the controller.</span></span> <span data-ttu-id="9bdb1-129">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="9bdb1-130">Seleccione **Agregar**y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="9bdb1-131">En el cuadro de diálogo **Agregar controlador** , establezca las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="9bdb1-132">En **nombre del controlador**, escriba "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="9bdb1-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="9bdb1-133">En **plantilla**, seleccione "controlador de API con acciones de lectura y escritura, con Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="9bdb1-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="9bdb1-134">En **clase de modelo**, seleccione &quot;orden (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="9bdb1-135">En **clase de contexto de datos**, seleccione &quot;OrdersContext (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="9bdb1-136">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-136">Click **Add**.</span></span> <span data-ttu-id="9bdb1-137">Esto agrega un archivo denominado OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="9bdb1-138">A continuación, es necesario modificar la implementación predeterminada del controlador.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="9bdb1-139">En primer lugar, elimine los métodos `PutOrder` y `DeleteOrder`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="9bdb1-140">En este ejemplo, los clientes no pueden modificar ni eliminar los pedidos existentes.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="9bdb1-141">En una aplicación real, se necesitaría una gran cantidad de lógica de back-end para controlar estos casos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="9bdb1-142">(Por ejemplo, ¿el pedido ya se ha enviado?)</span><span class="sxs-lookup"><span data-stu-id="9bdb1-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="9bdb1-143">Cambie el método `GetOrders` para devolver solo los pedidos que pertenecen al usuario:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="9bdb1-144">Cambie el método `GetOrder` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="9bdb1-145">Estos son los cambios que hemos realizado en el método:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="9bdb1-146">El valor devuelto es una instancia de `OrderDTO`, en lugar de una `Order`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="9bdb1-147">Cuando se consulta la base de datos para el pedido, usamos el método [DbQuery. include](https://msdn.microsoft.com/library/gg696395) para capturar las entidades `OrderDetail` y `Product` relacionadas.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="9bdb1-148">El resultado se acopla mediante una proyección.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="9bdb1-149">La respuesta HTTP contendrá una matriz de productos con cantidades:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="9bdb1-150">Este formato es más fácil para que los clientes consuman el gráfico de objetos original, que contiene entidades anidadas (Order, details y Products).</span><span class="sxs-lookup"><span data-stu-id="9bdb1-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="9bdb1-151">Último método que se va a considerar `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="9bdb1-152">En este momento, este método toma una instancia de `Order`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="9bdb1-153">Pero tenga en cuenta lo que sucede si un cliente envía un cuerpo de solicitud de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="9bdb1-154">Este es un orden bien estructurado y Entity Framework se insertará en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="9bdb1-155">Pero contiene una entidad de producto que no existía anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="9bdb1-156">El cliente acaba de crear un nuevo producto en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-156">The client just created a new product in our database!</span></span> <span data-ttu-id="9bdb1-157">Esto supondrá una sorpresa en el Departamento de realización de pedidos, cuando vea un pedido de Koala.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="9bdb1-158">La moral es, en realidad, tenga cuidado con los datos que acepta en una solicitud POST o PUT.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="9bdb1-159">Para evitar este problema, cambie el método `PostOrder` para tomar una instancia `OrderDTO`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="9bdb1-160">Utilice la `OrderDTO` para crear el `Order`.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="9bdb1-161">Tenga en cuenta que se usan las propiedades `ProductID` y `Quantity`, y se omiten los valores que el cliente envió por nombre de producto o precio.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="9bdb1-162">Si el ID. de producto no es válido, infringirá la restricción FOREIGN KEY en la base de datos y se producirá un error en la inserción, como debería.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="9bdb1-163">Este es el método de `PostOrder` completo:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="9bdb1-164">Por último, agregue el atributo **Authorize** al controlador:</span><span class="sxs-lookup"><span data-stu-id="9bdb1-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="9bdb1-165">Ahora solo los usuarios registrados pueden crear o ver los pedidos.</span><span class="sxs-lookup"><span data-stu-id="9bdb1-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9bdb1-166">[Anterior](using-web-api-with-entity-framework-part-5.md)
> [Siguiente](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="9bdb1-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
