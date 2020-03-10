---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guía de seguridad para ASP.NET Web API 2 OData-ASP.NET 4. x
author: MikeWasson
description: Describe los problemas de seguridad que se deben tener en cuenta al exponer un conjunto de DataSet a través de OData para ASP.NET Web API 2 en ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448297"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="8ec04-103">Guía de seguridad para ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="8ec04-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="8ec04-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8ec04-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8ec04-105">En este tema se describen algunos de los problemas de seguridad que se deben tener en cuenta al exponer un conjunto de DataSet a través de OData para ASP.NET Web API 2 en ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="8ec04-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="8ec04-106">Seguridad de EDM</span><span class="sxs-lookup"><span data-stu-id="8ec04-106">EDM Security</span></span>

<span data-ttu-id="8ec04-107">La semántica de la consulta se basa en el Entity Data Model (EDM), no en los tipos de modelo subyacentes.</span><span class="sxs-lookup"><span data-stu-id="8ec04-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="8ec04-108">Puede excluir una propiedad del EDM y no será visible para la consulta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="8ec04-109">Por ejemplo, supongamos que el modelo incluye un tipo de empleado con una propiedad salary.</span><span class="sxs-lookup"><span data-stu-id="8ec04-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="8ec04-110">Es posible que desee excluir esta propiedad del EDM para ocultarla de los clientes.</span><span class="sxs-lookup"><span data-stu-id="8ec04-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="8ec04-111">Hay dos maneras de excluir una propiedad del EDM.</span><span class="sxs-lookup"><span data-stu-id="8ec04-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="8ec04-112">Puede establecer el atributo **[IgnoreDataMember]** en la propiedad en la clase de modelo:</span><span class="sxs-lookup"><span data-stu-id="8ec04-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="8ec04-113">También puede quitar la propiedad del EDM mediante programación:</span><span class="sxs-lookup"><span data-stu-id="8ec04-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="8ec04-114">Seguridad de las consultas</span><span class="sxs-lookup"><span data-stu-id="8ec04-114">Query Security</span></span>

<span data-ttu-id="8ec04-115">Un cliente malintencionado o Naive puede crear una consulta que tarda mucho tiempo en ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="8ec04-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="8ec04-116">En el peor de los casos, esto puede interrumpir el acceso a su servicio.</span><span class="sxs-lookup"><span data-stu-id="8ec04-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="8ec04-117">El atributo **[Queryable]** es un filtro de acción que analiza, valida y aplica la consulta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="8ec04-118">El filtro convierte las opciones de consulta en una expresión LINQ.</span><span class="sxs-lookup"><span data-stu-id="8ec04-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="8ec04-119">Cuando el controlador OData devuelve un tipo **IQueryable** , el proveedor LINQ de **IQueryable** convierte la expresión LINQ en una consulta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="8ec04-120">Por lo tanto, el rendimiento depende del proveedor LINQ que se utiliza, y también de las características concretas del conjunto de datos o del esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ec04-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="8ec04-121">Para obtener más información sobre el uso de las opciones de consulta de OData en ASP.NET Web API, consulte [compatibilidad con las opciones de consulta de oData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="8ec04-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="8ec04-122">Si sabe que todos los clientes son de confianza (por ejemplo, en un entorno empresarial) o si el conjunto de información es pequeño, el rendimiento de las consultas podría no ser un problema.</span><span class="sxs-lookup"><span data-stu-id="8ec04-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="8ec04-123">De lo contrario, debe tener en cuenta las siguientes recomendaciones.</span><span class="sxs-lookup"><span data-stu-id="8ec04-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="8ec04-124">Pruebe el servicio con varias consultas y realice el perfil de la base de BD.</span><span class="sxs-lookup"><span data-stu-id="8ec04-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="8ec04-125">Habilitar la paginación controlada por el servidor, para evitar que se devuelva un conjunto de datos grande en una consulta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="8ec04-126">Para obtener más información, consulte [paginación controlada por el servidor](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="8ec04-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="8ec04-127">¿Necesita $filter y $orderby?</span><span class="sxs-lookup"><span data-stu-id="8ec04-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="8ec04-128">Algunas aplicaciones pueden permitir la paginación del cliente mediante $top y $skip, pero deshabilitar las demás opciones de consulta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="8ec04-129">Considere la posibilidad de restringir $orderby a las propiedades de un índice clúster.</span><span class="sxs-lookup"><span data-stu-id="8ec04-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="8ec04-130">La ordenación de datos grandes sin un índice clúster es lenta.</span><span class="sxs-lookup"><span data-stu-id="8ec04-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="8ec04-131">Número máximo de nodos: la propiedad **MaxNodeCount** en **[Queryable]** establece el número máximo de nodos permitido en el árbol de sintaxis $Filter.</span><span class="sxs-lookup"><span data-stu-id="8ec04-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="8ec04-132">El valor predeterminado es 100, pero puede que desee establecer un valor inferior, ya que un gran número de nodos puede ser lento de la compilación.</span><span class="sxs-lookup"><span data-stu-id="8ec04-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="8ec04-133">Esto es especialmente cierto si usa LINQ to Objects (es decir, consultas LINQ en una colección en memoria, sin usar un proveedor LINQ intermedio).</span><span class="sxs-lookup"><span data-stu-id="8ec04-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="8ec04-134">Considere la posibilidad de deshabilitar las funciones any () y All (), ya que pueden ser lentas.</span><span class="sxs-lookup"><span data-stu-id="8ec04-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="8ec04-135">Si alguna de las propiedades de cadena&#8212;contiene cadenas grandes, por ejemplo, una descripción del&#8212;producto o una entrada del blog considere la posibilidad de deshabilitar las funciones de cadena.</span><span class="sxs-lookup"><span data-stu-id="8ec04-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="8ec04-136">Considere la posibilidad de no permitir el filtrado en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="8ec04-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="8ec04-137">El filtrado de las propiedades de navegación puede dar lugar a una combinación, que podría ser lenta, dependiendo del esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ec04-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="8ec04-138">En el código siguiente se muestra un validador de consulta que impide el filtrado en las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="8ec04-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="8ec04-139">Para obtener más información sobre los validadores de consultas, vea [validación de consultas](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="8ec04-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="8ec04-140">Considere la posibilidad de restringir las consultas de $filter escribiendo un validador personalizado para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8ec04-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="8ec04-141">Por ejemplo, considere estas dos consultas:</span><span class="sxs-lookup"><span data-stu-id="8ec04-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="8ec04-142">Todas las películas con actores cuyo apellido empieza por ' A '.</span><span class="sxs-lookup"><span data-stu-id="8ec04-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="8ec04-143">Todas las películas publicadas en 1994.</span><span class="sxs-lookup"><span data-stu-id="8ec04-143">All movies released in 1994.</span></span>

    <span data-ttu-id="8ec04-144">A menos que los actores indexen las películas, es posible que la primera consulta requiera que el motor de base de BD examine toda la lista de películas.</span><span class="sxs-lookup"><span data-stu-id="8ec04-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="8ec04-145">Mientras que la segunda consulta podría ser aceptable, suponiendo que las películas se indexan por año de la versión.</span><span class="sxs-lookup"><span data-stu-id="8ec04-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="8ec04-146">En el código siguiente se muestra un validador que permite filtrar en las propiedades "ReleaseYear" y "title", pero no en otras propiedades.</span><span class="sxs-lookup"><span data-stu-id="8ec04-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="8ec04-147">En general, tenga en cuenta qué $filter funciones necesita.</span><span class="sxs-lookup"><span data-stu-id="8ec04-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="8ec04-148">Si los clientes no necesitan la expresividad total de $filter, puede limitar las funciones permitidas.</span><span class="sxs-lookup"><span data-stu-id="8ec04-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
