---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Agregar un nuevo campo a la tabla y al modelo de películas | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457705"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="f5edc-104">Agregar un nuevo campo a la tabla y modelo de películas</span><span class="sxs-lookup"><span data-stu-id="f5edc-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="f5edc-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f5edc-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f5edc-106">[Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f5edc-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="f5edc-107">Es más seguro, mucho más fácil de seguir y demuestra más características.</span><span class="sxs-lookup"><span data-stu-id="f5edc-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="f5edc-108">En esta sección, usará Migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo para que el cambio se aplique en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="f5edc-109">De forma predeterminada, cuando se utiliza Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla a la base de datos para realizar un seguimiento de si el esquema de la base de datos está sincronizado con las clases de modelo desde las que se generó.</span><span class="sxs-lookup"><span data-stu-id="f5edc-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="f5edc-110">Si no están sincronizados, el Entity Framework produce un error.</span><span class="sxs-lookup"><span data-stu-id="f5edc-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="f5edc-111">Esto facilita el seguimiento de los problemas en el tiempo de desarrollo que, de otra forma, podría encontrar (mediante errores ocultos) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f5edc-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="f5edc-112">Configuración de Migraciones de Code First para los cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="f5edc-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="f5edc-113">Si usa Visual Studio 2012, haga doble clic en el archivo *movies. MDF* desde explorador de soluciones para abrir la herramienta de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="f5edc-114">Visual Studio Express para web mostrará Explorador de bases de datos, Visual Studio 2012 mostrará Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="f5edc-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="f5edc-115">Si usa Visual Studio 2010, use Explorador de objetos de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f5edc-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="f5edc-116">En la herramienta de base de datos (Explorador de bases de datos, Explorador de servidores o Explorador de objetos de SQL Server), haga clic con el botón derecho en `MovieDBContext` y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="f5edc-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="f5edc-117">Vuelva a Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="f5edc-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="f5edc-118">Haga clic con el botón derecho en el archivo *movies. MDF* y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="f5edc-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="f5edc-119">Compile la aplicación para asegurarse de que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="f5edc-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="f5edc-120">En el menú **Tools** (Herramientas), haga clic en **Administrador de paquetes NuGet** y luego en **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f5edc-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Agregar el paquete Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="f5edc-122">En la ventana de la **consola del administrador de paquetes** , en el símbolo del sistema de `PM>`, escriba "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="f5edc-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="f5edc-123">El comando **enable-Migrations** (mostrado anteriormente) crea un archivo *Configuration.CS* en una nueva carpeta *Migrations* .</span><span class="sxs-lookup"><span data-stu-id="f5edc-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="f5edc-124">Visual Studio abre el archivo *Configuration.CS* .</span><span class="sxs-lookup"><span data-stu-id="f5edc-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="f5edc-125">Reemplace el método `Seed` del archivo *Configuration.CS* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f5edc-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="f5edc-126">Haga clic con el botón derecho en la línea ondulada roja en `Movie` y seleccione **resolver** y, a continuación, **use** **MvcMovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="f5edc-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="f5edc-127">Al hacerlo, se agrega la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="f5edc-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="f5edc-128">Migraciones de Code First llama al método `Seed` después de cada migración (es decir, al llamar a **Update-Database** en la consola del administrador de paquetes) y este método actualiza las filas que ya se han insertado o las inserta si aún no existen.</span><span class="sxs-lookup"><span data-stu-id="f5edc-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="f5edc-129">**Presione Ctrl-Mayús-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si no se compila en este momento).</span><span class="sxs-lookup"><span data-stu-id="f5edc-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="f5edc-130">El siguiente paso consiste en crear una clase `DbMigration` para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f5edc-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="f5edc-131">Esta migración a crea una nueva base de datos, por eso se ha eliminado el archivo *Movie. MDF* en un paso anterior.</span><span class="sxs-lookup"><span data-stu-id="f5edc-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="f5edc-132">En la ventana de la **consola del administrador de paquetes** , escriba el comando "agregar-migración inicial" para crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="f5edc-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="f5edc-133">El nombre "Initial" es arbitrario y se utiliza para asignar un nombre al archivo de migración creado.</span><span class="sxs-lookup"><span data-stu-id="f5edc-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="f5edc-134">Migraciones de Code First crea otro archivo de clase en la carpeta *Migrations* (con el nombre *{fecha}\_Initial.CS* ) y esta clase contiene código que crea el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="f5edc-135">El nombre de archivo de la migración se ha fijado previamente con una marca de tiempo para ayudar con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="f5edc-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="f5edc-136">Examine el archivo *{fecha}\_Initial.CS* , que contiene las instrucciones para crear la tabla de películas para la base de la película.</span><span class="sxs-lookup"><span data-stu-id="f5edc-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="f5edc-137">Al actualizar la base de datos en las instrucciones siguientes, se ejecutará este archivo *{fecha}\_Initial.CS* y se creará el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="f5edc-138">A continuación, el método de **inicialización** se ejecutará para rellenar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="f5edc-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="f5edc-139">En la **consola del administrador de paquetes**, escriba el comando "Update-Database" para crear la base de datos y ejecutar el método de **inicialización** .</span><span class="sxs-lookup"><span data-stu-id="f5edc-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="f5edc-140">Si recibe un error que indica que una tabla ya existe y no se puede crear, es probable que haya ejecutado la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="f5edc-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="f5edc-141">En ese caso, elimine el archivo *movies. MDF* de nuevo y vuelva a intentar el comando `update-database`.</span><span class="sxs-lookup"><span data-stu-id="f5edc-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="f5edc-142">Si sigue apareciendo un error, elimine la carpeta migraciones y el contenido y, a continuación, comience con las instrucciones que se indican en la parte superior de esta página (es decir, elimine el archivo *movies. MDF* y continúe con habilitar-migraciones).</span><span class="sxs-lookup"><span data-stu-id="f5edc-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="f5edc-143">Ejecute la aplicación y vaya a la dirección URL de */Movies* .</span><span class="sxs-lookup"><span data-stu-id="f5edc-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5edc-144">Se muestran los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="f5edc-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="f5edc-145">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="f5edc-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="f5edc-146">Comience agregando una nueva propiedad `Rating` a la clase `Movie` existente.</span><span class="sxs-lookup"><span data-stu-id="f5edc-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="f5edc-147">Abra el archivo *Models\Movie.CS* y agregue la propiedad `Rating` como esta:</span><span class="sxs-lookup"><span data-stu-id="f5edc-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="f5edc-148">La clase `Movie` completa ahora es similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="f5edc-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="f5edc-149">Compile la aplicación con el comando de menú **Compilar** &gt;**compilar película** o presionando Ctrl-Mayús-B.</span><span class="sxs-lookup"><span data-stu-id="f5edc-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="f5edc-150">Ahora que ha actualizado la clase `Model`, también debe actualizar las plantillas de vista *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* para mostrar la nueva propiedad `Rating` en la vista de explorador.</span><span class="sxs-lookup"><span data-stu-id="f5edc-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="f5edc-151">Abra el archivo<em>\Views\Movies\Index.cshtml</em> y agregue un encabezado de columna `<th>Rating</th>` justo después de la columna <strong>Price</strong> .</span><span class="sxs-lookup"><span data-stu-id="f5edc-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="f5edc-152">A continuación, agregue una `<td>` columna cerca del final de la plantilla para representar el valor `@item.Rating`.</span><span class="sxs-lookup"><span data-stu-id="f5edc-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="f5edc-153">A continuación se muestra el aspecto de la plantilla de vista <em>index. cshtml</em> actualizada:</span><span class="sxs-lookup"><span data-stu-id="f5edc-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="f5edc-154">A continuación, abra el archivo *\Views\Movies\Create.cshtml* y agregue el siguiente marcado cerca del final del formulario.</span><span class="sxs-lookup"><span data-stu-id="f5edc-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="f5edc-155">Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.</span><span class="sxs-lookup"><span data-stu-id="f5edc-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="f5edc-156">Ahora ha actualizado el código de aplicación para admitir la nueva propiedad `Rating`.</span><span class="sxs-lookup"><span data-stu-id="f5edc-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="f5edc-157">Ahora ejecute la aplicación y vaya a la dirección URL de */Movies* .</span><span class="sxs-lookup"><span data-stu-id="f5edc-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5edc-158">Sin embargo, cuando lo haga, verá uno de los siguientes errores:</span><span class="sxs-lookup"><span data-stu-id="f5edc-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="f5edc-159">Está viendo este error porque la clase del modelo de `Movie` actualizada en la aplicación es ahora diferente del esquema de la tabla de `Movie` de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="f5edc-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="f5edc-160">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="f5edc-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="f5edc-161">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="f5edc-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="f5edc-162">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="f5edc-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="f5edc-163">Este enfoque es muy práctico cuando se realiza el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el modelo y el esquema de la base de datos juntos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="f5edc-164">Sin embargo, el inconveniente es que se pierden los datos existentes en la base de datos, por lo que *no* se desea usar este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="f5edc-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="f5edc-165">El uso de un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una forma productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5edc-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="f5edc-166">Para obtener más información sobre Entity Framework inicializadores de base de datos, vea el [tutorial de ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="f5edc-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="f5edc-167">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="f5edc-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="f5edc-168">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="f5edc-169">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="f5edc-170">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f5edc-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="f5edc-171">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="f5edc-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="f5edc-172">Actualice el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="f5edc-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="f5edc-173">Abra el archivo Migrations\Configuration.cs y agregue un campo rating a cada objeto Movie.</span><span class="sxs-lookup"><span data-stu-id="f5edc-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="f5edc-174">Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f5edc-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="f5edc-175">El comando `add-migration` indica al marco de migración que examine el modelo de película actual con el esquema de la base de la película actual y cree el código necesario para migrar la base de BD al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="f5edc-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="f5edc-176">AddRatingMig es arbitrario y se utiliza para asignar un nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="f5edc-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="f5edc-177">Resulta útil usar un nombre descriptivo para el paso de migración.</span><span class="sxs-lookup"><span data-stu-id="f5edc-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="f5edc-178">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada y, en el método `Up`, puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="f5edc-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="f5edc-179">Compile la solución y, a continuación, escriba el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** .</span><span class="sxs-lookup"><span data-stu-id="f5edc-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="f5edc-180">En la imagen siguiente se muestra el resultado en la ventana de la **consola del administrador de paquetes** (la marca de fecha que preAddRatingMig es diferente).</span><span class="sxs-lookup"><span data-stu-id="f5edc-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="f5edc-181">Vuelva a ejecutar la aplicación y vaya a la dirección URL de/movies.</span><span class="sxs-lookup"><span data-stu-id="f5edc-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="f5edc-182">Puede ver el nuevo campo de clasificación.</span><span class="sxs-lookup"><span data-stu-id="f5edc-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="f5edc-183">Haga clic en el vínculo **crear nuevo** para agregar una nueva película.</span><span class="sxs-lookup"><span data-stu-id="f5edc-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="f5edc-184">Tenga en cuenta que puede Agregar una clasificación.</span><span class="sxs-lookup"><span data-stu-id="f5edc-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="f5edc-186">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="f5edc-186">Click **Create**.</span></span> <span data-ttu-id="f5edc-187">La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:</span><span class="sxs-lookup"><span data-stu-id="f5edc-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="f5edc-189">También debe agregar el campo `Rating` a las plantillas de vista Edit, details y SearchIndex.</span><span class="sxs-lookup"><span data-stu-id="f5edc-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="f5edc-190">Puede volver a escribir el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** y no se realizarán cambios, ya que el esquema coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="f5edc-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="f5edc-191">En esta sección vio cómo puede modificar objetos de modelo y mantener la base de datos sincronizada con los cambios.</span><span class="sxs-lookup"><span data-stu-id="f5edc-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="f5edc-192">También aprendió una manera de rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios.</span><span class="sxs-lookup"><span data-stu-id="f5edc-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="f5edc-193">A continuación, echemos un vistazo a cómo puede Agregar una lógica de validación más enriquecida a las clases de modelo y permitir que se apliquen algunas reglas de negocios.</span><span class="sxs-lookup"><span data-stu-id="f5edc-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5edc-194">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="f5edc-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
