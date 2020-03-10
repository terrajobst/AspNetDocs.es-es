---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Control de las relaciones de entidad | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449125"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="d920a-102">Controlar las relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="d920a-102">Handling Entity Relations</span></span>

<span data-ttu-id="d920a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d920a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d920a-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d920a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d920a-105">En esta sección se describen algunos detalles de cómo EF carga las entidades relacionadas y cómo controlar las propiedades de navegación circulares en las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="d920a-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="d920a-106">(En esta sección se proporciona información básica y no es necesario para completar el tutorial.</span><span class="sxs-lookup"><span data-stu-id="d920a-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="d920a-107">Si lo prefiere, vaya a la [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="d920a-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="d920a-108">Carga diligente frente a carga diferida</span><span class="sxs-lookup"><span data-stu-id="d920a-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="d920a-109">Al usar EF con una base de datos relacional, es importante comprender cómo EF carga los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="d920a-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="d920a-110">También resulta útil ver las consultas SQL que genera EF.</span><span class="sxs-lookup"><span data-stu-id="d920a-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="d920a-111">Para realizar el seguimiento de SQL, agregue la siguiente línea de código al constructor `BookServiceContext`:</span><span class="sxs-lookup"><span data-stu-id="d920a-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="d920a-112">Si envía una solicitud GET a/API/Books, devuelve JSON similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d920a-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="d920a-113">Puede ver que la propiedad Author es null, aunque el libro contenga una AuthorId válida.</span><span class="sxs-lookup"><span data-stu-id="d920a-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="d920a-114">Esto se debe a que EF no está cargando las entidades de autor relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d920a-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="d920a-115">El registro de seguimiento de la consulta SQL confirma lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d920a-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="d920a-116">La instrucción SELECT toma de la tabla Books y no hace referencia a la tabla Author.</span><span class="sxs-lookup"><span data-stu-id="d920a-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="d920a-117">Como referencia, este es el método de la clase `BooksController` que devuelve la lista de libros.</span><span class="sxs-lookup"><span data-stu-id="d920a-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="d920a-118">Veamos cómo podemos devolver el autor como parte de los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="d920a-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="d920a-119">Hay tres maneras de cargar datos relacionados en Entity Framework: carga diligente, carga diferida y carga explícita.</span><span class="sxs-lookup"><span data-stu-id="d920a-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="d920a-120">Existen ventajas e inconvenientes con cada técnica, por lo que es importante comprender cómo funcionan.</span><span class="sxs-lookup"><span data-stu-id="d920a-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="d920a-121">Carga diligente</span><span class="sxs-lookup"><span data-stu-id="d920a-121">Eager Loading</span></span>

<span data-ttu-id="d920a-122">Con la *carga diligente*, EF carga las entidades relacionadas como parte de la consulta de base de datos inicial.</span><span class="sxs-lookup"><span data-stu-id="d920a-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="d920a-123">Para realizar la carga diligente, use el método de extensión **System. Data. Entity. include** .</span><span class="sxs-lookup"><span data-stu-id="d920a-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="d920a-124">Esto indica a EF que incluya los datos de autor en la consulta.</span><span class="sxs-lookup"><span data-stu-id="d920a-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="d920a-125">Si realiza este cambio y ejecuta la aplicación, ahora los datos JSON tienen el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="d920a-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="d920a-126">El registro de seguimiento muestra que EF realizó una combinación en las tablas Book y Author.</span><span class="sxs-lookup"><span data-stu-id="d920a-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="d920a-127">Carga diferida</span><span class="sxs-lookup"><span data-stu-id="d920a-127">Lazy Loading</span></span>

<span data-ttu-id="d920a-128">Con la carga diferida, EF carga automáticamente una entidad relacionada cuando se desreferencia la propiedad de navegación de esa entidad.</span><span class="sxs-lookup"><span data-stu-id="d920a-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="d920a-129">Para habilitar la carga diferida, haga que la propiedad de navegación sea virtual.</span><span class="sxs-lookup"><span data-stu-id="d920a-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="d920a-130">Por ejemplo, en la clase Book:</span><span class="sxs-lookup"><span data-stu-id="d920a-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="d920a-131">Ahora, considere el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="d920a-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="d920a-132">Cuando se habilita la carga diferida, el acceso a la propiedad `Author` en `books[0]` hace que EF consulte en la base de datos el autor.</span><span class="sxs-lookup"><span data-stu-id="d920a-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="d920a-133">La carga diferida requiere varios recorridos de base de datos, porque EF envía una consulta cada vez que recupera una entidad relacionada.</span><span class="sxs-lookup"><span data-stu-id="d920a-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="d920a-134">Por lo general, desea deshabilitar la carga diferida para los objetos que se serializan.</span><span class="sxs-lookup"><span data-stu-id="d920a-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="d920a-135">El serializador tiene que leer todas las propiedades del modelo, lo que desencadena la carga de las entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d920a-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="d920a-136">Por ejemplo, estas son las consultas SQL cuando EF serializa la lista de libros con la carga diferida habilitada.</span><span class="sxs-lookup"><span data-stu-id="d920a-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="d920a-137">Puede ver que EF realiza tres consultas independientes para los tres autores.</span><span class="sxs-lookup"><span data-stu-id="d920a-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="d920a-138">Todavía hay ocasiones en las que podría querer usar la carga diferida.</span><span class="sxs-lookup"><span data-stu-id="d920a-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="d920a-139">La carga diligente puede hacer que EF genere una combinación muy compleja.</span><span class="sxs-lookup"><span data-stu-id="d920a-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="d920a-140">O bien, puede que necesite entidades relacionadas para un pequeño subconjunto de los datos y la carga diferida sería más eficaz.</span><span class="sxs-lookup"><span data-stu-id="d920a-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="d920a-141">Una manera de evitar problemas de serialización es serializar los objetos de transferencia de datos (DTO) en lugar de los objetos de entidad.</span><span class="sxs-lookup"><span data-stu-id="d920a-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="d920a-142">Mostraré este enfoque más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="d920a-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="d920a-143">Carga explícita</span><span class="sxs-lookup"><span data-stu-id="d920a-143">Explicit Loading</span></span>

<span data-ttu-id="d920a-144">La carga explícita es similar a la carga diferida, salvo que se obtienen explícitamente los datos relacionados en el código. no se produce automáticamente cuando se obtiene acceso a una propiedad de navegación.</span><span class="sxs-lookup"><span data-stu-id="d920a-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="d920a-145">La carga explícita le proporciona más control sobre cuándo cargar los datos relacionados, pero requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="d920a-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="d920a-146">Para obtener más información sobre la carga explícita, vea [cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="d920a-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="d920a-147">Propiedades de navegación y referencias circulares</span><span class="sxs-lookup"><span data-stu-id="d920a-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="d920a-148">Al definir los modelos de libro y de autor, se define una propiedad de navegación en la clase `Book` para la relación de autor del libro, pero no se definió una propiedad de navegación en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="d920a-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="d920a-149">¿Qué ocurre si agrega la propiedad de navegación correspondiente a la clase `Author`?</span><span class="sxs-lookup"><span data-stu-id="d920a-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="d920a-150">Desafortunadamente, esto crea un problema al serializar los modelos.</span><span class="sxs-lookup"><span data-stu-id="d920a-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="d920a-151">Si carga los datos relacionados, se crea un gráfico de objetos circulares.</span><span class="sxs-lookup"><span data-stu-id="d920a-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="d920a-152">Cuando el formateador JSON o XML intenta serializar el gráfico, producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="d920a-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="d920a-153">Los dos formateadores producen mensajes de excepción diferentes.</span><span class="sxs-lookup"><span data-stu-id="d920a-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="d920a-154">Este es un ejemplo del formateador JSON:</span><span class="sxs-lookup"><span data-stu-id="d920a-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="d920a-155">Este es el formateador XML:</span><span class="sxs-lookup"><span data-stu-id="d920a-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="d920a-156">Una solución es usar dto, que se describe en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="d920a-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="d920a-157">Como alternativa, puede configurar los formateadores JSON y XML para controlar los ciclos del gráfico.</span><span class="sxs-lookup"><span data-stu-id="d920a-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="d920a-158">Para obtener más información, vea [controlar referencias circulares de objetos](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="d920a-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="d920a-159">En este tutorial, no necesita la propiedad de navegación `Author.Book`, por lo que puede dejarla fuera.</span><span class="sxs-lookup"><span data-stu-id="d920a-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d920a-160">[Anterior](part-3.md)
> [Siguiente](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d920a-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
