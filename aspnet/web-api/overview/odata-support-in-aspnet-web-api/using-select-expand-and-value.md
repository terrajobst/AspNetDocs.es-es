---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Usar $select, $expand y $value en ASP.NET Web API 2 OData - ASP.NET 4.x
author: MikeWasson
description: Información general y ejemplos de código para el $expandir, $select, y opciones de $value de OData Web API 2 de ASP.NET 4.x.
ms.author: riande
ms.date: 10/11/2013
ms.custom: seoapril2019
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: 8b5d3e87c679a31f1908aa648219ae5c6b701a1f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400703"
---
# <a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="02caa-103">Usar $select, $expand y $value en ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="02caa-103">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="02caa-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="02caa-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="02caa-105">Información general y ejemplos de código para el $expandir, $select, y opciones de $value de OData Web API 2 de ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="02caa-105">Overview and code samples for the $expand, $select, and $value options in OData Web API 2 for ASP.NET 4.x.</span></span> <span data-ttu-id="02caa-106">Estas opciones permiten que un cliente controlar la representación que recibe desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="02caa-106">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="02caa-107">**$expand** hace que las entidades relacionadas ser incluido en línea en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="02caa-107">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="02caa-108">**$select** selecciona un subconjunto de propiedades para incluir en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="02caa-108">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="02caa-109">**$value** Obtiene el valor de una propiedad sin formato.</span><span class="sxs-lookup"><span data-stu-id="02caa-109">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="02caa-110">Esquema de ejemplo</span><span class="sxs-lookup"><span data-stu-id="02caa-110">Example Schema</span></span>

<span data-ttu-id="02caa-111">En este artículo, usaré un servicio de OData que define tres entidades: Categoría, proveedor y producto.</span><span class="sxs-lookup"><span data-stu-id="02caa-111">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="02caa-112">Cada producto tiene una categoría y un proveedor.</span><span class="sxs-lookup"><span data-stu-id="02caa-112">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="02caa-113">Estas son las clases de C# que definen los modelos de entidad:</span><span class="sxs-lookup"><span data-stu-id="02caa-113">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="02caa-114">Tenga en cuenta que el `Product` clase define las propiedades de navegación para el `Supplier` y `Category`.</span><span class="sxs-lookup"><span data-stu-id="02caa-114">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="02caa-115">La `Category` clase define una propiedad de navegación para los productos de cada categoría.</span><span class="sxs-lookup"><span data-stu-id="02caa-115">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="02caa-116">Para crear un extremo de OData para este esquema, usar el scaffolding de Visual Studio 2013, como se describe en [creación de un extremo de OData en ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="02caa-116">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="02caa-117">Agregar controladores independientes para el proveedor, categoría y producto.</span><span class="sxs-lookup"><span data-stu-id="02caa-117">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="02caa-118">Habilitación de $expanda y $select</span><span class="sxs-lookup"><span data-stu-id="02caa-118">Enabling $expand and $select</span></span>

<span data-ttu-id="02caa-119">En Visual Studio 2013, el scaffolding de Web API OData crea un controlador que admite automáticamente la $expand y $select.</span><span class="sxs-lookup"><span data-stu-id="02caa-119">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="02caa-120">Como referencia, estos son los requisitos para admitir $expanda y $select en un controlador.</span><span class="sxs-lookup"><span data-stu-id="02caa-120">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="02caa-121">Para de las colecciones, el controlador `Get` método debe devolver un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="02caa-121">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="02caa-122">Para entidades únicas, devolver un **SingleResult&lt;T&gt;**, donde T es un **IQueryable** que contiene cero entidades o una entidad.</span><span class="sxs-lookup"><span data-stu-id="02caa-122">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="02caa-123">Además, decore el `Get` métodos con el **[Queryable]** de atributo, como se muestra en los fragmentos de código anterior.</span><span class="sxs-lookup"><span data-stu-id="02caa-123">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="02caa-124">Como alternativa, llame a **EnableQuerySupport** en el **HttpConfiguration** objeto en el inicio.</span><span class="sxs-lookup"><span data-stu-id="02caa-124">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="02caa-125">(Para obtener más información, consulte [habilitar opciones de consulta de OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="02caa-125">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="02caa-126">$Expanda</span><span class="sxs-lookup"><span data-stu-id="02caa-126">Using $expand</span></span>

<span data-ttu-id="02caa-127">Al consultar una entidad de OData o una colección, la respuesta predeterminada no incluye las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="02caa-127">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="02caa-128">Por ejemplo, aquí está la respuesta predeterminada para el conjunto de entidades de categorías:</span><span class="sxs-lookup"><span data-stu-id="02caa-128">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="02caa-129">Como puede ver, la respuesta no incluye ningún producto, aunque la entidad Category tiene un vínculo de navegación de productos.</span><span class="sxs-lookup"><span data-stu-id="02caa-129">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="02caa-130">Sin embargo, el cliente puede utilizar $expanda esta opción para obtener la lista de productos para cada categoría.</span><span class="sxs-lookup"><span data-stu-id="02caa-130">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="02caa-131">Opción de expansión de los $ se incluye en la cadena de consulta de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="02caa-131">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="02caa-132">Ahora el servidor incluirá los productos para cada categoría, en línea con las categorías.</span><span class="sxs-lookup"><span data-stu-id="02caa-132">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="02caa-133">Esta es la carga de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-133">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="02caa-134">Tenga en cuenta que cada entrada de la matriz de "value" contiene una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="02caa-134">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="02caa-135">El $expandir la lista de opciones toma separados por comas de las propiedades de navegación para expandir.</span><span class="sxs-lookup"><span data-stu-id="02caa-135">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="02caa-136">La solicitud siguiente amplía la categoría y el proveedor de un producto.</span><span class="sxs-lookup"><span data-stu-id="02caa-136">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="02caa-137">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-137">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="02caa-138">Puede expandir más de un nivel de propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="02caa-138">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="02caa-139">El ejemplo siguiente incluye todos los productos para una categoría y también el proveedor de cada producto.</span><span class="sxs-lookup"><span data-stu-id="02caa-139">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="02caa-140">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-140">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="02caa-141">De forma predeterminada, Web API limita la profundidad de expansión máxima a 2.</span><span class="sxs-lookup"><span data-stu-id="02caa-141">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="02caa-142">Que evita que el cliente envíe solicitudes complejas, como `$expand=Orders/OrderDetails/Product/Supplier/Region`, lo que podría resultar ineficaz para consultar y crear las respuestas de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="02caa-142">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="02caa-143">Para reemplazar el valor predeterminado, establezca el **MaxExpansionDepth** propiedad en el **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="02caa-143">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="02caa-144">Para obtener más información acerca de los $ opción $expand, consulte [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) en la documentación oficial de OData.</span><span class="sxs-lookup"><span data-stu-id="02caa-144">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="02caa-145">Usar $select</span><span class="sxs-lookup"><span data-stu-id="02caa-145">Using $select</span></span>

<span data-ttu-id="02caa-146">La opción $select especifica un subconjunto de propiedades para incluir en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="02caa-146">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="02caa-147">Por ejemplo, para obtener solamente el nombre y el precio de cada producto, use la siguiente consulta:</span><span class="sxs-lookup"><span data-stu-id="02caa-147">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="02caa-148">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-148">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="02caa-149">Puede combinar $select y $expand en la misma consulta.</span><span class="sxs-lookup"><span data-stu-id="02caa-149">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="02caa-150">Asegúrese de incluir la propiedad expandida en la opción $select.</span><span class="sxs-lookup"><span data-stu-id="02caa-150">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="02caa-151">Por ejemplo, la siguiente solicitud obtiene el nombre de producto y el proveedor.</span><span class="sxs-lookup"><span data-stu-id="02caa-151">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="02caa-152">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-152">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="02caa-153">También puede seleccionar las propiedades dentro de una propiedad expandida.</span><span class="sxs-lookup"><span data-stu-id="02caa-153">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="02caa-154">La solicitud siguiente amplía los productos y selecciona el nombre de la categoría junto con el nombre de producto.</span><span class="sxs-lookup"><span data-stu-id="02caa-154">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="02caa-155">Este es el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="02caa-155">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="02caa-156">Para obtener más información acerca de la opción $select, consulte [seleccione System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) en la documentación oficial de OData.</span><span class="sxs-lookup"><span data-stu-id="02caa-156">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="02caa-157">Introducción a las propiedades individuales de una entidad ($value)</span><span class="sxs-lookup"><span data-stu-id="02caa-157">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="02caa-158">Hay dos formas para que un cliente de OData obtener una propiedad individual de una entidad.</span><span class="sxs-lookup"><span data-stu-id="02caa-158">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="02caa-159">El cliente puede obtener el valor en formato de OData u obtener el valor sin formato de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="02caa-159">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="02caa-160">La siguiente solicitud obtiene una propiedad en formato de OData.</span><span class="sxs-lookup"><span data-stu-id="02caa-160">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="02caa-161">Este es un ejemplo de respuesta en formato JSON:</span><span class="sxs-lookup"><span data-stu-id="02caa-161">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="02caa-162">Para obtener el valor de la propiedad sin formato, anexe $value al URI:</span><span class="sxs-lookup"><span data-stu-id="02caa-162">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="02caa-163">Esta es la respuesta.</span><span class="sxs-lookup"><span data-stu-id="02caa-163">Here is the response.</span></span> <span data-ttu-id="02caa-164">Tenga en cuenta que el tipo de contenido "text/plain", no es JSON.</span><span class="sxs-lookup"><span data-stu-id="02caa-164">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="02caa-165">Para admitir estas consultas en el controlador de OData, agregue un método denominado `GetProperty`, donde `Property` es el nombre de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="02caa-165">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="02caa-166">Por ejemplo, el método para obtener la propiedad Name se denominará `GetName`.</span><span class="sxs-lookup"><span data-stu-id="02caa-166">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="02caa-167">El método debe devolver el valor de esa propiedad:</span><span class="sxs-lookup"><span data-stu-id="02caa-167">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
