---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Agregar modelos y controladores | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449191"
---
# <a name="add-models-and-controllers"></a><span data-ttu-id="959be-102">Agregar modelos y controladores</span><span class="sxs-lookup"><span data-stu-id="959be-102">Add Models and Controllers</span></span>

<span data-ttu-id="959be-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="959be-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="959be-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="959be-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="959be-105">En esta sección, agregará clases de modelo que definen las entidades de base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="959be-106">A continuación, agregará controladores de API Web que realizan operaciones CRUD en esas entidades.</span><span class="sxs-lookup"><span data-stu-id="959be-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="959be-107">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="959be-107">Add Model Classes</span></span>

<span data-ttu-id="959be-108">En este tutorial, crearemos la base de datos con el enfoque "Code First" para Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="959be-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="959be-109">Con Code First, escribe C# las clases que corresponden a las tablas de base de datos y EF crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="959be-110">(Para obtener más información, vea [métodos de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)).</span><span class="sxs-lookup"><span data-stu-id="959be-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="959be-111">Comenzaremos definiendo nuestros objetos de dominio como POCO (objetos CLR antiguos sin formato).</span><span class="sxs-lookup"><span data-stu-id="959be-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="959be-112">Se crearán los siguientes POCO:</span><span class="sxs-lookup"><span data-stu-id="959be-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="959be-113">Autor</span><span class="sxs-lookup"><span data-stu-id="959be-113">Author</span></span>
- <span data-ttu-id="959be-114">Book</span><span class="sxs-lookup"><span data-stu-id="959be-114">Book</span></span>

<span data-ttu-id="959be-115">En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos.</span><span class="sxs-lookup"><span data-stu-id="959be-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="959be-116">Seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="959be-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="959be-117">Asigne a la clase el nombre `Author`.</span><span class="sxs-lookup"><span data-stu-id="959be-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="959be-118">Reemplace todo el código reutilizable de Author.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="959be-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="959be-119">Agregue otra clase denominada `Book`, con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="959be-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="959be-120">Entity Framework usará estos modelos para crear tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="959be-121">Para cada modelo, la propiedad `Id` se convertirá en la columna de clave principal de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="959be-122">En la clase Book, el `AuthorId` define una clave externa en la tabla `Author`.</span><span class="sxs-lookup"><span data-stu-id="959be-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="959be-123">(Por motivos de simplicidad, supongo que cada libro tiene un solo autor). La clase Book también contiene una propiedad de navegación a la `Author`relacionada.</span><span class="sxs-lookup"><span data-stu-id="959be-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="959be-124">Puede usar la propiedad de navegación para tener acceso al `Author` relacionado en el código.</span><span class="sxs-lookup"><span data-stu-id="959be-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="959be-125">Me digo más sobre las propiedades de navegación en la parte 4, el control de las [relaciones de entidad](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="959be-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="959be-126">Agregar controladores de API Web</span><span class="sxs-lookup"><span data-stu-id="959be-126">Add Web API Controllers</span></span>

<span data-ttu-id="959be-127">En esta sección, agregaremos controladores de API Web que admitan las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="959be-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="959be-128">Los controladores utilizarán Entity Framework para comunicarse con el nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="959be-129">En primer lugar, puede eliminar los controladores de archivos/clase valuescontroller. cs.</span><span class="sxs-lookup"><span data-stu-id="959be-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="959be-130">Este archivo contiene un controlador de API Web de ejemplo, pero no lo necesita para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="959be-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="959be-131">A continuación, compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="959be-131">Next, build the project.</span></span> <span data-ttu-id="959be-132">El scaffolding de API Web utiliza la reflexión para buscar las clases de modelo, por lo que necesita el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="959be-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="959be-133">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="959be-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="959be-134">Seleccione **Agregar**y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="959be-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="959be-135">En el cuadro de diálogo **Agregar scaffold** , seleccione "controlador Web API 2 con acciones, usando Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="959be-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="959be-136">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="959be-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="959be-137">En el cuadro de diálogo **Agregar controlador** , haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="959be-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="959be-138">En el menú desplegable **clase de modelo** , seleccione la clase `Author`.</span><span class="sxs-lookup"><span data-stu-id="959be-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="959be-139">(Si no aparece en la lista desplegable, asegúrese de que ha compilado el proyecto).</span><span class="sxs-lookup"><span data-stu-id="959be-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="959be-140">Active "usar acciones de controlador Async".</span><span class="sxs-lookup"><span data-stu-id="959be-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="959be-141">Deje el nombre del controlador como &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="959be-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="959be-142">Haga clic en el botón más (+) situado junto a **clase de contexto de datos**.</span><span class="sxs-lookup"><span data-stu-id="959be-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="959be-143">En el cuadro de diálogo **nuevo contexto de datos** , deje el nombre predeterminado y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="959be-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="959be-144">Haga clic en **Agregar** para completar el cuadro de diálogo **Agregar controlador** .</span><span class="sxs-lookup"><span data-stu-id="959be-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="959be-145">El cuadro de diálogo agrega dos clases al proyecto:</span><span class="sxs-lookup"><span data-stu-id="959be-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="959be-146">`AuthorsController` define un controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="959be-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="959be-147">El controlador implementa la API de REST que usan los clientes para realizar operaciones CRUD en la lista de autores.</span><span class="sxs-lookup"><span data-stu-id="959be-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="959be-148">`BookServiceContext` administra objetos entidad durante el tiempo de ejecución, lo que incluye llenar objetos con datos de una base de datos, realizar un seguimiento de los cambios y conservar los datos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="959be-149">Hereda de `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="959be-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="959be-150">En este punto, vuelva a compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="959be-150">At this point, build the project again.</span></span> <span data-ttu-id="959be-151">Ahora siga los mismos pasos para agregar un controlador de API para `Book` entidades.</span><span class="sxs-lookup"><span data-stu-id="959be-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="959be-152">Esta vez, seleccione `Book` para la clase de modelo y seleccione la clase de `BookServiceContext` existente para la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="959be-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="959be-153">(No cree un nuevo contexto de datos). Haga clic en **Agregar** para agregar el controlador.</span><span class="sxs-lookup"><span data-stu-id="959be-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="959be-154">[Anterior](part-1.md)
> [Siguiente](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="959be-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
