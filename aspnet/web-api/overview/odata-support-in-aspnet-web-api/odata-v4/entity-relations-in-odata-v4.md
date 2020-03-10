---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relaciones de entidad en OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: 'La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores. Con OData, los clientes pueden navegar por...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fbafb2b2346689271905db5790cdddeeb809b070
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484465"
---
# <a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="6399f-104">Relaciones de entidad en OData V4 con ASP.NET Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="6399f-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="6399f-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6399f-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6399f-106">La mayoría de los conjuntos de datos definen las relaciones entre las entidades: los clientes tienen pedidos; los libros tienen autores; los productos tienen proveedores.</span><span class="sxs-lookup"><span data-stu-id="6399f-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="6399f-107">Con OData, los clientes pueden navegar por las relaciones de entidad.</span><span class="sxs-lookup"><span data-stu-id="6399f-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="6399f-108">Dado un producto, puede encontrar el proveedor.</span><span class="sxs-lookup"><span data-stu-id="6399f-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="6399f-109">También puede crear o quitar relaciones.</span><span class="sxs-lookup"><span data-stu-id="6399f-109">You can also create or remove relationships.</span></span> <span data-ttu-id="6399f-110">Por ejemplo, puede establecer el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="6399f-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="6399f-111">En este tutorial se muestra cómo admitir estas operaciones en OData V4 mediante ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="6399f-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="6399f-112">El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6399f-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6399f-113">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="6399f-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="6399f-114">API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="6399f-114">Web API 2.1</span></span>
> - <span data-ttu-id="6399f-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="6399f-115">OData v4</span></span>
> - <span data-ttu-id="6399f-116">Visual Studio 2013 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="6399f-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="6399f-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6399f-117">Entity Framework 6</span></span>
> - <span data-ttu-id="6399f-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6399f-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="6399f-119">Versiones del tutorial</span><span class="sxs-lookup"><span data-stu-id="6399f-119">Tutorial versions</span></span>
>
> <span data-ttu-id="6399f-120">Para la versión 3 de OData, consulte [compatibilidad con las relaciones de entidad en OData V3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="6399f-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="6399f-121">Agregar una entidad de proveedor</span><span class="sxs-lookup"><span data-stu-id="6399f-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="6399f-122">El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6399f-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="6399f-123">En primer lugar, necesitamos una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="6399f-123">First, we need a related entity.</span></span> <span data-ttu-id="6399f-124">Agregue una clase denominada `Supplier` en la carpeta models.</span><span class="sxs-lookup"><span data-stu-id="6399f-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="6399f-125">Agregue una propiedad de navegación a la clase `Product`:</span><span class="sxs-lookup"><span data-stu-id="6399f-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="6399f-126">Agregue un nuevo **DbSet** a la clase `ProductsContext`, de modo que Entity Framework incluirá la tabla de proveedores en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6399f-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="6399f-127">En WebApiConfig.cs, agregue un conjunto de entidades&quot; de &quot;Suppliers al Entity Data Model:</span><span class="sxs-lookup"><span data-stu-id="6399f-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="6399f-128">Agregar un controlador de proveedores</span><span class="sxs-lookup"><span data-stu-id="6399f-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="6399f-129">Agregue una clase `SuppliersController` a la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="6399f-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="6399f-130">No se muestra cómo agregar operaciones CRUD para este controlador.</span><span class="sxs-lookup"><span data-stu-id="6399f-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="6399f-131">Los pasos son los mismos que para el controlador de productos (consulte [creación de un punto de conexión de oData V4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="6399f-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="6399f-132">Obtener entidades relacionadas</span><span class="sxs-lookup"><span data-stu-id="6399f-132">Getting Related Entities</span></span>

<span data-ttu-id="6399f-133">Para obtener el proveedor de un producto, el cliente envía una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="6399f-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="6399f-134">Para admitir esta solicitud, agregue el método siguiente a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="6399f-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="6399f-135">Este método usa una Convención de nomenclatura predeterminada</span><span class="sxs-lookup"><span data-stu-id="6399f-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="6399f-136">Nombre del método: GetX, donde X es la propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="6399f-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="6399f-137">Nombre del parámetro: *clave*</span><span class="sxs-lookup"><span data-stu-id="6399f-137">Parameter name: *key*</span></span>

<span data-ttu-id="6399f-138">Si sigue esta Convención de nomenclatura, la API Web asigna automáticamente la solicitud HTTP al método de controlador.</span><span class="sxs-lookup"><span data-stu-id="6399f-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="6399f-139">Solicitud HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6399f-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="6399f-140">Respuesta HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6399f-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="6399f-141">Obtener una colección relacionada</span><span class="sxs-lookup"><span data-stu-id="6399f-141">Getting a related collection</span></span>

<span data-ttu-id="6399f-142">En el ejemplo anterior, un producto tiene un proveedor.</span><span class="sxs-lookup"><span data-stu-id="6399f-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="6399f-143">Una propiedad de navegación también puede devolver una colección.</span><span class="sxs-lookup"><span data-stu-id="6399f-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="6399f-144">El código siguiente obtiene los productos de un proveedor:</span><span class="sxs-lookup"><span data-stu-id="6399f-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="6399f-145">En este caso, el método devuelve un **IQueryable** en lugar de un **SingleResult&lt;t&gt;**</span><span class="sxs-lookup"><span data-stu-id="6399f-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="6399f-146">Solicitud HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6399f-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="6399f-147">Respuesta HTTP de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6399f-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="6399f-148">Crear una relación entre entidades</span><span class="sxs-lookup"><span data-stu-id="6399f-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="6399f-149">OData admite la creación o eliminación de relaciones entre dos entidades existentes.</span><span class="sxs-lookup"><span data-stu-id="6399f-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="6399f-150">En la terminología de OData V4, la relación es una referencia &quot;&quot;.</span><span class="sxs-lookup"><span data-stu-id="6399f-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="6399f-151">(En OData V3, la relación se llamaba un *vínculo*.</span><span class="sxs-lookup"><span data-stu-id="6399f-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="6399f-152">Las diferencias de Protocolo no importan para este tutorial).</span><span class="sxs-lookup"><span data-stu-id="6399f-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="6399f-153">Una referencia tiene su propio URI, con el formato `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="6399f-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="6399f-154">Por ejemplo, este es el URI para tratar la referencia entre un producto y su proveedor:</span><span class="sxs-lookup"><span data-stu-id="6399f-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="6399f-155">Para agregar una relación, el cliente envía una solicitud POST o PUT a esta dirección.</span><span class="sxs-lookup"><span data-stu-id="6399f-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="6399f-156">PUT si la propiedad de navegación es una sola entidad, como `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="6399f-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="6399f-157">POST si la propiedad de navegación es una colección, como `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="6399f-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="6399f-158">El cuerpo de la solicitud contiene el URI de la otra entidad de la relación.</span><span class="sxs-lookup"><span data-stu-id="6399f-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="6399f-159">A continuación se muestra una solicitud de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6399f-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="6399f-160">En este ejemplo, el cliente envía una solicitud PUT a `/Products(6)/Supplier/$ref`, que es el URI $ref para la `Supplier` del producto con el identificador 6.</span><span class="sxs-lookup"><span data-stu-id="6399f-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="6399f-161">Si la solicitud se realiza correctamente, el servidor envía una respuesta 204 (sin contenido):</span><span class="sxs-lookup"><span data-stu-id="6399f-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="6399f-162">Este es el método de controlador para agregar una relación a un `Product`:</span><span class="sxs-lookup"><span data-stu-id="6399f-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="6399f-163">El parámetro *navigationProperty* especifica la relación que se va a establecer.</span><span class="sxs-lookup"><span data-stu-id="6399f-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="6399f-164">(Si hay más de una propiedad de navegación en la entidad, puede agregar más instrucciones `case`).</span><span class="sxs-lookup"><span data-stu-id="6399f-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="6399f-165">El parámetro de *vínculo* contiene el URI del proveedor.</span><span class="sxs-lookup"><span data-stu-id="6399f-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="6399f-166">Web API analiza automáticamente el cuerpo de la solicitud para obtener el valor de este parámetro.</span><span class="sxs-lookup"><span data-stu-id="6399f-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="6399f-167">Para buscar el proveedor, necesitamos el identificador (o clave), que forma parte del parámetro de *vínculo* .</span><span class="sxs-lookup"><span data-stu-id="6399f-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="6399f-168">Para ello, utilice el siguiente método auxiliar:</span><span class="sxs-lookup"><span data-stu-id="6399f-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="6399f-169">Básicamente, este método usa la biblioteca de OData para dividir la ruta de acceso del URI en segmentos, buscar el segmento que contiene la clave y convertir la clave en el tipo correcto.</span><span class="sxs-lookup"><span data-stu-id="6399f-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="6399f-170">Eliminar una relación entre entidades</span><span class="sxs-lookup"><span data-stu-id="6399f-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="6399f-171">Para eliminar una relación, el cliente envía una solicitud HTTP DELETE al URI $ref:</span><span class="sxs-lookup"><span data-stu-id="6399f-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="6399f-172">Este es el método de controlador para eliminar la relación entre un producto y un proveedor:</span><span class="sxs-lookup"><span data-stu-id="6399f-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="6399f-173">En este caso, `Product.Supplier` es el &quot;1&quot; final de una relación de uno a varios, por lo que puede quitar la relación simplemente estableciendo `Product.Supplier` en `null`.</span><span class="sxs-lookup"><span data-stu-id="6399f-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="6399f-174">En el &quot;muchos&quot; extremo de una relación, el cliente debe especificar la entidad relacionada que se va a quitar.</span><span class="sxs-lookup"><span data-stu-id="6399f-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="6399f-175">Para ello, el cliente envía el URI de la entidad relacionada en la cadena de consulta de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6399f-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="6399f-176">Por ejemplo, para quitar "producto 1" de "proveedor 1":</span><span class="sxs-lookup"><span data-stu-id="6399f-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="6399f-177">Para admitir esto en Web API, es necesario incluir un parámetro adicional en el método `DeleteRef`.</span><span class="sxs-lookup"><span data-stu-id="6399f-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="6399f-178">Este es el método de controlador para quitar un producto de la relación de `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="6399f-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="6399f-179">El parámetro *key* es la clave del proveedor y el parámetro *relatedKey* es la clave del producto que se va a quitar de la relación `Products`.</span><span class="sxs-lookup"><span data-stu-id="6399f-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="6399f-180">Tenga en cuenta que la API Web obtiene automáticamente la clave de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="6399f-180">Note that Web API automatically gets the key from the query string.</span></span>
