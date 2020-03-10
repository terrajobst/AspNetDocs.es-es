---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Compatibilidad con las relaciones de entidad en OData V3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 726a7d51123805e05f6831ef9cd7eaa84b6c44bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484507"
---
# <a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="713c6-104">Compatibilidad de las relaciones de entidad en OData V3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="713c6-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>

<span data-ttu-id="713c6-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="713c6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="713c6-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="713c6-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="713c6-107">La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores.</span><span class="sxs-lookup"><span data-stu-id="713c6-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="713c6-108">Con OData, los clientes pueden navegar por las relaciones de entidad.</span><span class="sxs-lookup"><span data-stu-id="713c6-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="713c6-109">Dado un producto, puede encontrar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="713c6-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="713c6-110">También puede crear o quitar relaciones.</span><span class="sxs-lookup"><span data-stu-id="713c6-110">You can also create or remove relationships.</span></span> <span data-ttu-id="713c6-111">Por ejemplo, puede establecer el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="713c6-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="713c6-112">En este tutorial se muestra cómo admitir estas operaciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="713c6-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="713c6-113">El tutorial se basa en el tutorial [creación de un punto de conexión de oData V3 con Web API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="713c6-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="713c6-114">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="713c6-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="713c6-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="713c6-115">Web API 2</span></span>
> - <span data-ttu-id="713c6-116">Versión 3 de OData</span><span class="sxs-lookup"><span data-stu-id="713c6-116">OData Version 3</span></span>
> - <span data-ttu-id="713c6-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="713c6-117">Entity Framework 6</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="713c6-118">Agregar una entidad de proveedor</span><span class="sxs-lookup"><span data-stu-id="713c6-118">Add a Supplier Entity</span></span>

<span data-ttu-id="713c6-119">En primer lugar, es necesario agregar un nuevo tipo de entidad a la fuente de OData.</span><span class="sxs-lookup"><span data-stu-id="713c6-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="713c6-120">Vamos a agregar una clase `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="713c6-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="713c6-121">Esta clase usa una cadena para la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="713c6-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="713c6-122">En la práctica, esto podría ser menos común que usar una clave entera.</span><span class="sxs-lookup"><span data-stu-id="713c6-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="713c6-123">Pero merece la pena ver cómo OData controla otros tipos de clave además de enteros.</span><span class="sxs-lookup"><span data-stu-id="713c6-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="713c6-124">A continuación, vamos a crear una relación agregando una propiedad `Supplier` a la clase `Product`:</span><span class="sxs-lookup"><span data-stu-id="713c6-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="713c6-125">Agregue un nuevo **DbSet** a la clase `ProductServiceContext`, de modo que Entity Framework incluirá la tabla `Supplier` en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="713c6-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="713c6-126">En WebApiConfig.cs, agregue una entidad "Suppliers" al modelo EDM:</span><span class="sxs-lookup"><span data-stu-id="713c6-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="713c6-127">Propiedades de navegación</span><span class="sxs-lookup"><span data-stu-id="713c6-127">Navigation Properties</span></span>

<span data-ttu-id="713c6-128">Para obtener el proveedor de un producto, el cliente envía una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="713c6-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="713c6-129">Aquí "proveedor" es una propiedad de navegación en el tipo de `Product`.</span><span class="sxs-lookup"><span data-stu-id="713c6-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="713c6-130">En este caso, `Supplier` hace referencia a un único elemento, pero una propiedad de navegación también puede devolver una colección (relación de uno a varios o de varios a varios).</span><span class="sxs-lookup"><span data-stu-id="713c6-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="713c6-131">Para admitir esta solicitud, agregue el método siguiente a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="713c6-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="713c6-132">El parámetro *key* es la clave del producto.</span><span class="sxs-lookup"><span data-stu-id="713c6-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="713c6-133">El método devuelve la entidad&#8212;relacionada en este caso, una instancia de `Supplier`.</span><span class="sxs-lookup"><span data-stu-id="713c6-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="713c6-134">El nombre del método y el nombre del parámetro son importantes.</span><span class="sxs-lookup"><span data-stu-id="713c6-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="713c6-135">En general, si la propiedad de navegación se denomina "X", debe agregar un método denominado "GetX".</span><span class="sxs-lookup"><span data-stu-id="713c6-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="713c6-136">El método debe tomar un parámetro denominado "*key*" que coincida con el tipo de datos de la clave del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="713c6-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="713c6-137">También es importante incluir el atributo **[FromOdataUri]** en el parámetro *clave* .</span><span class="sxs-lookup"><span data-stu-id="713c6-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="713c6-138">Este atributo indica a la API Web que use las reglas de sintaxis de OData cuando analiza la clave del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="713c6-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="713c6-139">Crear y eliminar vínculos</span><span class="sxs-lookup"><span data-stu-id="713c6-139">Creating and Deleting Links</span></span>

<span data-ttu-id="713c6-140">OData admite la creación o eliminación de relaciones entre dos entidades.</span><span class="sxs-lookup"><span data-stu-id="713c6-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="713c6-141">En la terminología de OData, la relación es un "vínculo".</span><span class="sxs-lookup"><span data-stu-id="713c6-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="713c6-142">Cada vínculo tiene un URI con el formato *entidad*/$links/*entidad*.</span><span class="sxs-lookup"><span data-stu-id="713c6-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="713c6-143">Por ejemplo, el vínculo de producto a proveedor tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="713c6-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="713c6-144">Para crear un vínculo nuevo, el cliente envía una solicitud POST al URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="713c6-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="713c6-145">El cuerpo de la solicitud es el URI de la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="713c6-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="713c6-146">Por ejemplo, supongamos que hay un proveedor con la clave "CTSO".</span><span class="sxs-lookup"><span data-stu-id="713c6-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="713c6-147">Para crear un vínculo de "Product (1)" a "Supplier (' CTSO ')", el cliente envía una solicitud como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="713c6-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="713c6-148">Para eliminar un vínculo, el cliente envía una solicitud DELETE al URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="713c6-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="713c6-149">**Crear vínculos**</span><span class="sxs-lookup"><span data-stu-id="713c6-149">**Creating Links**</span></span>

<span data-ttu-id="713c6-150">Para permitir que un cliente cree vínculos de proveedor de productos, agregue el código siguiente a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="713c6-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="713c6-151">Este método usa tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="713c6-151">This method takes three parameters:</span></span>

- <span data-ttu-id="713c6-152">*key*: la clave de la entidad primaria (el producto)</span><span class="sxs-lookup"><span data-stu-id="713c6-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="713c6-153">*navigationProperty*: el nombre de la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="713c6-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="713c6-154">En este ejemplo, la única propiedad de navegación válida es "proveedor".</span><span class="sxs-lookup"><span data-stu-id="713c6-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="713c6-155">*Link*: el URI de oData de la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="713c6-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="713c6-156">Este valor se toma del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="713c6-156">This value is taken from the request body.</span></span> <span data-ttu-id="713c6-157">Por ejemplo, el URI del vínculo podría ser "`http://localhost/odata/Suppliers('CTSO')`, lo que significa que el proveedor con ID. = ' CTSO '.</span><span class="sxs-lookup"><span data-stu-id="713c6-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="713c6-158">El método utiliza el vínculo para buscar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="713c6-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="713c6-159">Si se encuentra el proveedor coincidente, el método establece la propiedad `Product.Supplier` y guarda el resultado en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="713c6-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="713c6-160">La parte más difícil es analizar el URI del vínculo.</span><span class="sxs-lookup"><span data-stu-id="713c6-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="713c6-161">Básicamente, debe simular el resultado de enviar una solicitud GET a ese URI.</span><span class="sxs-lookup"><span data-stu-id="713c6-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="713c6-162">El siguiente método auxiliar muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="713c6-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="713c6-163">El método invoca el proceso de enrutamiento de la API Web y devuelve una instancia de **ODataPath** que representa la ruta de acceso de oData analizada.</span><span class="sxs-lookup"><span data-stu-id="713c6-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="713c6-164">Para un URI de vínculo, uno de los segmentos debe ser la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="713c6-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="713c6-165">(Si no es así, el cliente envió un URI incorrecto).</span><span class="sxs-lookup"><span data-stu-id="713c6-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="713c6-166">**Eliminar vínculos**</span><span class="sxs-lookup"><span data-stu-id="713c6-166">**Deleting Links**</span></span>

<span data-ttu-id="713c6-167">Para eliminar un vínculo, agregue el código siguiente a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="713c6-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="713c6-168">En este ejemplo, la propiedad de navegación es una entidad de `Supplier` única.</span><span class="sxs-lookup"><span data-stu-id="713c6-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="713c6-169">Si la propiedad de navegación es una colección, el URI para eliminar un vínculo debe incluir una clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="713c6-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="713c6-170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="713c6-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="713c6-171">Esta solicitud quita el pedido 1 del cliente 1.</span><span class="sxs-lookup"><span data-stu-id="713c6-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="713c6-172">En este caso, el método DeleteLink tendrá la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="713c6-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="713c6-173">El parámetro *relatedKey* proporciona la clave para la entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="713c6-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="713c6-174">Por lo tanto, en el método de `DeleteLink`, busque la entidad principal mediante el parámetro de *clave* , busque la entidad relacionada mediante el parámetro *relatedKey* y, a continuación, quite la asociación.</span><span class="sxs-lookup"><span data-stu-id="713c6-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="713c6-175">Dependiendo del modelo de datos, puede que tenga que implementar ambas versiones de `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="713c6-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="713c6-176">La API Web llamará a la versión correcta en función del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="713c6-176">Web API will call the correct version based on the request URI.</span></span>
