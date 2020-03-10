---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Abrir tipos en OData V4 con ASP.NET Web API | Microsoft Docs
author: microsoft
description: En OData V4, un tipo abierto es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo. Abrir...
ms.author: riande
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 950442c071bf50d2c8c1588971f13f85c4891436
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504583"
---
# <a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="7a75a-104">Tipos abiertos en OData V4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7a75a-104">Open Types in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="7a75a-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7a75a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7a75a-106">En OData V4, un *tipo abierto* es un tipo estructurado que contiene propiedades dinámicas, además de las propiedades que se declaran en la definición de tipo.</span><span class="sxs-lookup"><span data-stu-id="7a75a-106">In OData v4, an *open type* is a structured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="7a75a-107">Los tipos abiertos permiten agregar flexibilidad a los modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="7a75a-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="7a75a-108">En este tutorial se muestra cómo usar tipos abiertos en ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="7a75a-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="7a75a-109">En este tutorial se supone que ya sabe cómo crear un punto de conexión de OData en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="7a75a-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="7a75a-110">En caso contrario, empiece por leer primero [crear un punto de conexión de oData V4](create-an-odata-v4-endpoint.md) .</span><span class="sxs-lookup"><span data-stu-id="7a75a-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7a75a-111">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="7a75a-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7a75a-112">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="7a75a-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="7a75a-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="7a75a-113">OData v4</span></span>

<span data-ttu-id="7a75a-114">En primer lugar, algunas terminología de OData:</span><span class="sxs-lookup"><span data-stu-id="7a75a-114">First, some OData terminology:</span></span>

- <span data-ttu-id="7a75a-115">Tipo de entidad: un tipo estructurado con una clave.</span><span class="sxs-lookup"><span data-stu-id="7a75a-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="7a75a-116">Tipo complejo: tipo estructurado sin clave.</span><span class="sxs-lookup"><span data-stu-id="7a75a-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="7a75a-117">Open type: un tipo con propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="7a75a-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="7a75a-118">Los tipos de entidad y los tipos complejos pueden estar abiertos.</span><span class="sxs-lookup"><span data-stu-id="7a75a-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="7a75a-119">El valor de una propiedad dinámica puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos.</span><span class="sxs-lookup"><span data-stu-id="7a75a-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="7a75a-120">Para obtener más información sobre los tipos abiertos, vea la [especificación OData V4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="7a75a-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="7a75a-121">Instalación de las bibliotecas de web OData</span><span class="sxs-lookup"><span data-stu-id="7a75a-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="7a75a-122">Use el administrador de paquetes NuGet para instalar las bibliotecas de OData API Web más recientes.</span><span class="sxs-lookup"><span data-stu-id="7a75a-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="7a75a-123">Desde la ventana de la consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="7a75a-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="7a75a-124">Definir los tipos CLR</span><span class="sxs-lookup"><span data-stu-id="7a75a-124">Define the CLR Types</span></span>

<span data-ttu-id="7a75a-125">Empiece por definir los modelos de EDM como tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="7a75a-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="7a75a-126">Cuando se crea el Entity Data Model (EDM),</span><span class="sxs-lookup"><span data-stu-id="7a75a-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="7a75a-127">`Category` es un tipo de enumeración.</span><span class="sxs-lookup"><span data-stu-id="7a75a-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="7a75a-128">`Address` es un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="7a75a-128">`Address` is a complex type.</span></span> <span data-ttu-id="7a75a-129">(No tiene una clave, por lo que no es un tipo de entidad).</span><span class="sxs-lookup"><span data-stu-id="7a75a-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="7a75a-130">`Customer` es un tipo de entidad.</span><span class="sxs-lookup"><span data-stu-id="7a75a-130">`Customer` is an entity type.</span></span> <span data-ttu-id="7a75a-131">(Tiene una clave).</span><span class="sxs-lookup"><span data-stu-id="7a75a-131">(It has a key.)</span></span>
- <span data-ttu-id="7a75a-132">`Press` es un tipo complejo abierto.</span><span class="sxs-lookup"><span data-stu-id="7a75a-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="7a75a-133">`Book` es un tipo de entidad abierto.</span><span class="sxs-lookup"><span data-stu-id="7a75a-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="7a75a-134">Para crear un tipo abierto, el tipo CLR debe tener una propiedad de tipo `IDictionary<string, object>`, que contiene las propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="7a75a-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="7a75a-135">Compilación del modelo EDM</span><span class="sxs-lookup"><span data-stu-id="7a75a-135">Build the EDM Model</span></span>

<span data-ttu-id="7a75a-136">Si usa **ODataConventionModelBuilder** para crear el EDM, `Press` y `Book` se agregan automáticamente como tipos abiertos, en función de la presencia de una propiedad `IDictionary<string, object>`.</span><span class="sxs-lookup"><span data-stu-id="7a75a-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="7a75a-137">También puede compilar el EDM explícitamente mediante **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="7a75a-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="7a75a-138">Agregar un controlador OData</span><span class="sxs-lookup"><span data-stu-id="7a75a-138">Add an OData Controller</span></span>

<span data-ttu-id="7a75a-139">A continuación, agregue un controlador OData.</span><span class="sxs-lookup"><span data-stu-id="7a75a-139">Next, add an OData controller.</span></span> <span data-ttu-id="7a75a-140">En este tutorial, usaremos un controlador simplificado que solo admita solicitudes GET y POST, y usa una lista en memoria para almacenar entidades.</span><span class="sxs-lookup"><span data-stu-id="7a75a-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="7a75a-141">Observe que la primera instancia de `Book` no tiene propiedades dinámicas.</span><span class="sxs-lookup"><span data-stu-id="7a75a-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="7a75a-142">La segunda instancia de `Book` tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="7a75a-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="7a75a-143">"Publicado": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="7a75a-143">"Published": Primitive type</span></span>
- <span data-ttu-id="7a75a-144">"Authors": colección de tipos primitivos</span><span class="sxs-lookup"><span data-stu-id="7a75a-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="7a75a-145">"OtherCategories": colección de tipos de enumeración.</span><span class="sxs-lookup"><span data-stu-id="7a75a-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="7a75a-146">Además, la propiedad `Press` de esa instancia de `Book` tiene las siguientes propiedades dinámicas:</span><span class="sxs-lookup"><span data-stu-id="7a75a-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="7a75a-147">"Blog": tipo primitivo</span><span class="sxs-lookup"><span data-stu-id="7a75a-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="7a75a-148">"Address": tipo complejo</span><span class="sxs-lookup"><span data-stu-id="7a75a-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="7a75a-149">Consultar los metadatos</span><span class="sxs-lookup"><span data-stu-id="7a75a-149">Query the Metadata</span></span>

<span data-ttu-id="7a75a-150">Para obtener el documento de metadatos de OData, envíe una solicitud GET a `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="7a75a-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="7a75a-151">El cuerpo de la respuesta debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7a75a-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="7a75a-152">En el documento de metadatos, puede ver lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7a75a-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="7a75a-153">En el caso de los tipos `Book` y `Press`, el valor del atributo `OpenType` es true.</span><span class="sxs-lookup"><span data-stu-id="7a75a-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="7a75a-154">Los tipos `Customer` y `Address` no tienen este atributo.</span><span class="sxs-lookup"><span data-stu-id="7a75a-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="7a75a-155">El tipo de entidad `Book` tiene tres propiedades declaradas: ISBN, title y Press.</span><span class="sxs-lookup"><span data-stu-id="7a75a-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="7a75a-156">Los metadatos de OData no incluyen la propiedad `Book.Properties` de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="7a75a-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="7a75a-157">Del mismo modo, el tipo complejo de `Press` solo tiene dos propiedades declaradas: nombre y categoría.</span><span class="sxs-lookup"><span data-stu-id="7a75a-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="7a75a-158">Los metadatos no incluyen la propiedad `Press.DynamicProperties` de la clase CLR.</span><span class="sxs-lookup"><span data-stu-id="7a75a-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="7a75a-159">Consultar una entidad</span><span class="sxs-lookup"><span data-stu-id="7a75a-159">Query an Entity</span></span>

<span data-ttu-id="7a75a-160">Para que el libro con ISBN sea igual a "978-0-7356-7942-9", envíe una solicitud GET a `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="7a75a-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="7a75a-161">El cuerpo de la respuesta debe ser similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="7a75a-161">The response body should look similar to the following.</span></span> <span data-ttu-id="7a75a-162">(Con sangría para que sea más legible).</span><span class="sxs-lookup"><span data-stu-id="7a75a-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="7a75a-163">Observe que las propiedades dinámicas se incluyen alineadas con las propiedades declaradas.</span><span class="sxs-lookup"><span data-stu-id="7a75a-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="7a75a-164">PUBLICAR una entidad</span><span class="sxs-lookup"><span data-stu-id="7a75a-164">POST an Entity</span></span>

<span data-ttu-id="7a75a-165">Para agregar una entidad de libro, envíe una solicitud POST a `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="7a75a-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="7a75a-166">El cliente puede establecer propiedades dinámicas en la carga de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7a75a-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="7a75a-167">A continuación se muestra una solicitud de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="7a75a-167">Here is an example request.</span></span> <span data-ttu-id="7a75a-168">Tenga en cuenta las propiedades "Price" y "Published".</span><span class="sxs-lookup"><span data-stu-id="7a75a-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="7a75a-169">Si establece un punto de interrupción en el método de controlador, puede ver que la API Web agregó estas propiedades al Diccionario de `Properties`.</span><span class="sxs-lookup"><span data-stu-id="7a75a-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="7a75a-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7a75a-170">Additional Resources</span></span>

[<span data-ttu-id="7a75a-171">Ejemplo de Open type de OData</span><span class="sxs-lookup"><span data-stu-id="7a75a-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
