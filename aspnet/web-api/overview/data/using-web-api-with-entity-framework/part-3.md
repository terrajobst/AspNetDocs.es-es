---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar migraciones de Code First para inicializar la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065502"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="3be60-102">Usar migraciones de Code First para inicializar la base de datos</span><span class="sxs-lookup"><span data-stu-id="3be60-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="3be60-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3be60-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3be60-104">Descargue el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3be60-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3be60-105">En esta sección, usará [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) en EF para inicializar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="3be60-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="3be60-106">Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet**, a continuación, seleccione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="3be60-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3be60-107">En la ventana de consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3be60-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="3be60-108">Este comando agrega una carpeta denominada migraciones a su proyecto, además de un archivo de código denominado Configuration.cs en la carpeta de migraciones.</span><span class="sxs-lookup"><span data-stu-id="3be60-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="3be60-109">Abra el archivo Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="3be60-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="3be60-110">Agregue el siguiente **mediante** instrucción.</span><span class="sxs-lookup"><span data-stu-id="3be60-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="3be60-111">A continuación, agregue el código siguiente a la **Configuration.Seed** método:</span><span class="sxs-lookup"><span data-stu-id="3be60-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="3be60-112">En la ventana de consola de administrador de paquetes, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="3be60-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="3be60-113">El primer comando genera código que crea la base de datos y el segundo comando ejecuta ese código.</span><span class="sxs-lookup"><span data-stu-id="3be60-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="3be60-114">La base de datos se crea localmente, mediante [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="3be60-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="3be60-115">Explorar la API (opcional)</span><span class="sxs-lookup"><span data-stu-id="3be60-115">Explore the API (Optional)</span></span>

<span data-ttu-id="3be60-116">Presione F5 para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="3be60-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="3be60-117">Visual Studio inicia IIS Express y ejecuta la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3be60-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="3be60-118">A continuación, Visual Studio inicia un explorador y abre la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3be60-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="3be60-119">Cuando Visual Studio se ejecuta un proyecto web, asigna a un número de puerto.</span><span class="sxs-lookup"><span data-stu-id="3be60-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="3be60-120">En la imagen siguiente, el número de puerto es 50524.</span><span class="sxs-lookup"><span data-stu-id="3be60-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="3be60-121">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="3be60-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="3be60-122">La página principal se implementa mediante ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3be60-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="3be60-123">En la parte superior de la página, hay un vínculo que dice "API".</span><span class="sxs-lookup"><span data-stu-id="3be60-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="3be60-124">Este vínculo lleva a una página de ayuda generada automáticamente para la API web.</span><span class="sxs-lookup"><span data-stu-id="3be60-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="3be60-125">(Para obtener información sobre cómo se genera esta página de ayuda, y cómo puede agregar su propia documentación a la página, vea [crear páginas de ayuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Puede hacer clic en la Ayuda de vínculos de página para ver detalles acerca de la API, incluido el formato de solicitud y respuesta.</span><span class="sxs-lookup"><span data-stu-id="3be60-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="3be60-126">La API permite operaciones CRUD en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3be60-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="3be60-127">A continuación resume la API.</span><span class="sxs-lookup"><span data-stu-id="3be60-127">The following summarizes the API.</span></span>

| <span data-ttu-id="3be60-128">Authors</span><span class="sxs-lookup"><span data-stu-id="3be60-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="3be60-129">OBTENER api/authors</span><span class="sxs-lookup"><span data-stu-id="3be60-129">GET api/authors</span></span> | <span data-ttu-id="3be60-130">Obtener a todos los autores.</span><span class="sxs-lookup"><span data-stu-id="3be60-130">Get all authors.</span></span> |
| <span data-ttu-id="3be60-131">GET api/authors / {id}</span><span class="sxs-lookup"><span data-stu-id="3be60-131">GET api/authors/{id}</span></span> | <span data-ttu-id="3be60-132">Obtener a un autor por Id.</span><span class="sxs-lookup"><span data-stu-id="3be60-132">Get an author by ID.</span></span> |
| <span data-ttu-id="3be60-133">Autores/api/POST</span><span class="sxs-lookup"><span data-stu-id="3be60-133">POST /api/authors</span></span> | <span data-ttu-id="3be60-134">Crear a un nuevo autor.</span><span class="sxs-lookup"><span data-stu-id="3be60-134">Create a new author.</span></span> |
| <span data-ttu-id="3be60-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="3be60-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="3be60-136">Actualizar a un autor existente.</span><span class="sxs-lookup"><span data-stu-id="3be60-136">Update an existing author.</span></span> |
| <span data-ttu-id="3be60-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="3be60-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="3be60-138">Eliminar a un autor.</span><span class="sxs-lookup"><span data-stu-id="3be60-138">Delete an author.</span></span> |

| <span data-ttu-id="3be60-139">Libros</span><span class="sxs-lookup"><span data-stu-id="3be60-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="3be60-140">OBTENER /api/books</span><span class="sxs-lookup"><span data-stu-id="3be60-140">GET /api/books</span></span> | <span data-ttu-id="3be60-141">Obtener todos los libros.</span><span class="sxs-lookup"><span data-stu-id="3be60-141">Get all books.</span></span> |
| <span data-ttu-id="3be60-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3be60-142">GET /api/books/{id}</span></span> | <span data-ttu-id="3be60-143">Obtenga un libro por identificador.</span><span class="sxs-lookup"><span data-stu-id="3be60-143">Get a book by ID.</span></span> |
| <span data-ttu-id="3be60-144">PUBLICAR libros / api /</span><span class="sxs-lookup"><span data-stu-id="3be60-144">POST /api/books</span></span> | <span data-ttu-id="3be60-145">Cree un nuevo libro.</span><span class="sxs-lookup"><span data-stu-id="3be60-145">Create a new book.</span></span> |
| <span data-ttu-id="3be60-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3be60-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="3be60-147">Actualizar un libro existente.</span><span class="sxs-lookup"><span data-stu-id="3be60-147">Update an existing book.</span></span> |
| <span data-ttu-id="3be60-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3be60-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="3be60-149">Eliminar un libro.</span><span class="sxs-lookup"><span data-stu-id="3be60-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="3be60-150">Vista de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="3be60-150">View the Database (Optional)</span></span>

<span data-ttu-id="3be60-151">Cuando ejecutó el comando Update-Database, EF crea la base de datos y se llama el `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="3be60-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="3be60-152">Cuando se ejecuta la aplicación localmente, EF usa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="3be60-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="3be60-153">Puede ver la base de datos en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3be60-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="3be60-154">Desde el **vista** menú, seleccione **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3be60-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="3be60-155">En el **conectar al servidor** cuadro de diálogo, en el **nombre del servidor** cuadro de edición, escriba "(localdb) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="3be60-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="3be60-156">Deje el **autenticación** opción como "Autenticación de Windows".</span><span class="sxs-lookup"><span data-stu-id="3be60-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="3be60-157">Haga clic en **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="3be60-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="3be60-158">Visual Studio se conecta a LocalDB y muestra las bases de datos existentes en la ventana del explorador de objetos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3be60-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="3be60-159">Puede expandir los nodos para ver las tablas que EF creado.</span><span class="sxs-lookup"><span data-stu-id="3be60-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="3be60-160">Para ver los datos, haga clic en una tabla y seleccione **ver datos**.</span><span class="sxs-lookup"><span data-stu-id="3be60-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="3be60-161">Captura de pantalla siguiente muestra los resultados de la tabla de los libros en pantalla.</span><span class="sxs-lookup"><span data-stu-id="3be60-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="3be60-162">Tenga en cuenta que EF rellena la base de datos con los datos de inicialización y la tabla contiene la clave externa a la tabla Authors.</span><span class="sxs-lookup"><span data-stu-id="3be60-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="3be60-163">[Anterior](part-2.md)
> [Siguiente](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="3be60-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>