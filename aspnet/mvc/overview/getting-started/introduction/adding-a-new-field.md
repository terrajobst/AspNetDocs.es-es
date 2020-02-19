---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Agregando un nuevo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5974e53e4610dccc7812df261dc97a9b0327de85
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456691"
---
# <a name="adding-a-new-field"></a><span data-ttu-id="40795-102">Adición de un nuevo campo</span><span class="sxs-lookup"><span data-stu-id="40795-102">Adding a New Field</span></span>

<span data-ttu-id="40795-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="40795-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="40795-104">En esta sección, usará Migraciones de Entity Framework Code First para migrar algunos cambios a las clases del modelo para que el cambio se aplique en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="40795-105">De forma predeterminada, cuando se utiliza Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla a la base de datos para realizar un seguimiento de si el esquema de la base de datos está sincronizado con las clases de modelo desde las que se generó.</span><span class="sxs-lookup"><span data-stu-id="40795-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="40795-106">Si no están sincronizados, el Entity Framework produce un error.</span><span class="sxs-lookup"><span data-stu-id="40795-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="40795-107">Esto facilita el seguimiento de los problemas en el tiempo de desarrollo que, de otra forma, podría encontrar (mediante errores ocultos) en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="40795-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="40795-108">Configuración de Migraciones de Code First para los cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="40795-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="40795-109">Vaya a Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="40795-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="40795-110">Haga clic con el botón derecho en el archivo *movies. MDF* y seleccione **eliminar** para quitar la base de datos de películas.</span><span class="sxs-lookup"><span data-stu-id="40795-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="40795-111">Si no ve el archivo *movies. MDF* , haga clic en el icono **Mostrar todos los archivos** que se muestra a continuación en el contorno rojo.</span><span class="sxs-lookup"><span data-stu-id="40795-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="40795-112">Compile la aplicación para asegurarse de que no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="40795-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="40795-113">En el menú **Tools** (Herramientas), haga clic en **Administrador de paquetes NuGet** y luego en **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="40795-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Agregar el paquete Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="40795-115">En la ventana de la **consola del administrador de paquetes** , en el símbolo del sistema `PM>` escriba</span><span class="sxs-lookup"><span data-stu-id="40795-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="40795-116">Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="40795-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="40795-117">El comando **enable-Migrations** (mostrado anteriormente) crea un archivo *Configuration.CS* en una nueva carpeta *Migrations* .</span><span class="sxs-lookup"><span data-stu-id="40795-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="40795-118">Visual Studio abre el archivo *Configuration.CS* .</span><span class="sxs-lookup"><span data-stu-id="40795-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="40795-119">Reemplace el método `Seed` del archivo *Configuration.CS* por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="40795-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="40795-120">Mantenga el mouse sobre la línea ondulada roja en `Movie`, haga clic en `Show Potential Fixes` y, a continuación, haga clic en **usar** **MvcMovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="40795-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="40795-121">Al hacerlo, se agrega la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="40795-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="40795-122">Migraciones de Code First llama al método `Seed` después de cada migración (es decir, al llamar a **Update-Database** en la consola del administrador de paquetes) y este método actualiza las filas que ya se han insertado o las inserta si aún no existen.</span><span class="sxs-lookup"><span data-stu-id="40795-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="40795-123">El método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) del código siguiente realiza una operación "Upsert":</span><span class="sxs-lookup"><span data-stu-id="40795-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="40795-124">Dado que el método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) se ejecuta con cada migración, no se pueden insertar datos, ya que las filas que está intentando agregar ya estarán allí después de la primera migración que crea la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="40795-125">La operación "[Upsert](http://en.wikipedia.org/wiki/Upsert)" evita errores que podrían producirse si se intenta insertar una fila que ya existe, pero reemplaza cualquier cambio en los datos que haya realizado al probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="40795-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="40795-126">En el caso de los datos de prueba de algunas tablas, es posible que no desee que suceda: en algunos casos, cuando cambie los datos mientras realiza las pruebas desea que los cambios permanezcan después de las actualizaciones de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="40795-127">En ese caso, desea realizar una operación de inserción condicional: Inserte una fila solo si aún no existe.</span><span class="sxs-lookup"><span data-stu-id="40795-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="40795-128">El primer parámetro que se pasa al método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica la propiedad que se va a usar para comprobar si ya existe una fila.</span><span class="sxs-lookup"><span data-stu-id="40795-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="40795-129">En el caso de los datos de película de prueba que se proporcionan, se puede usar la propiedad `Title` para este propósito, ya que cada título de la lista es único:</span><span class="sxs-lookup"><span data-stu-id="40795-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="40795-130">Este código supone que los títulos son únicos.</span><span class="sxs-lookup"><span data-stu-id="40795-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="40795-131">Si agrega manualmente un título duplicado, recibirá la siguiente excepción la próxima vez que realice una migración.</span><span class="sxs-lookup"><span data-stu-id="40795-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
> <span data-ttu-id="40795-132">*La secuencia contiene más de un elemento*</span><span class="sxs-lookup"><span data-stu-id="40795-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="40795-133">Para obtener más información acerca del método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) , consulte el método de encargarse [con EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).</span><span class="sxs-lookup"><span data-stu-id="40795-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>

<span data-ttu-id="40795-134">**Presione Ctrl-Mayús-B para compilar el proyecto.** (Se producirá un error en los pasos siguientes si no se compila en este momento).</span><span class="sxs-lookup"><span data-stu-id="40795-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="40795-135">El siguiente paso consiste en crear una clase `DbMigration` para la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="40795-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="40795-136">Esta migración crea una nueva base de datos, por eso se ha eliminado el archivo *Movie. MDF* en un paso anterior.</span><span class="sxs-lookup"><span data-stu-id="40795-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="40795-137">En la ventana de la **consola del administrador de paquetes** , escriba el comando `add-migration Initial` para crear la migración inicial.</span><span class="sxs-lookup"><span data-stu-id="40795-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="40795-138">El nombre "Initial" es arbitrario y se utiliza para asignar un nombre al archivo de migración creado.</span><span class="sxs-lookup"><span data-stu-id="40795-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="40795-139">Migraciones de Code First crea otro archivo de clase en la carpeta *Migrations* (con el nombre *{fecha}\_Initial.CS* ) y esta clase contiene código que crea el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="40795-140">El nombre de archivo de la migración se ha fijado previamente con una marca de tiempo para ayudar con la ordenación.</span><span class="sxs-lookup"><span data-stu-id="40795-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="40795-141">Examine el archivo *{fecha}\_Initial.CS* , que contiene las instrucciones para crear la tabla de `Movies` de la base de la película.</span><span class="sxs-lookup"><span data-stu-id="40795-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="40795-142">Al actualizar la base de datos en las instrucciones siguientes, se ejecutará este archivo *{fecha}\_Initial.CS* y se creará el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="40795-143">A continuación, el método de **inicialización** se ejecutará para rellenar la base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="40795-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="40795-144">En la **consola del administrador de paquetes**, escriba el comando `update-database` para crear la base de datos y ejecutar el método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="40795-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="40795-145">Si recibe un error que indica que una tabla ya existe y no se puede crear, es probable que haya ejecutado la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="40795-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="40795-146">En ese caso, elimine el archivo *movies. MDF* de nuevo y vuelva a intentar el comando `update-database`.</span><span class="sxs-lookup"><span data-stu-id="40795-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="40795-147">Si sigue apareciendo un error, elimine la carpeta migraciones y el contenido y, a continuación, comience con las instrucciones que se indican en la parte superior de esta página (es decir, elimine el archivo *movies. MDF* y continúe con habilitar-migraciones).</span><span class="sxs-lookup"><span data-stu-id="40795-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="40795-148">Si sigue apareciendo un error, abra Explorador de objetos de SQL Server y quite la base de datos de la lista.</span><span class="sxs-lookup"><span data-stu-id="40795-148">If you still get an error, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="40795-149">Ejecute la aplicación y vaya a la dirección URL de */Movies* .</span><span class="sxs-lookup"><span data-stu-id="40795-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="40795-150">Se muestran los datos de inicialización.</span><span class="sxs-lookup"><span data-stu-id="40795-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="40795-151">Adición de una propiedad de clasificación al modelo Movie</span><span class="sxs-lookup"><span data-stu-id="40795-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="40795-152">Comience agregando una nueva propiedad `Rating` a la clase `Movie` existente.</span><span class="sxs-lookup"><span data-stu-id="40795-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="40795-153">Abra el archivo *Models\Movie.CS* y agregue la propiedad `Rating` como esta:</span><span class="sxs-lookup"><span data-stu-id="40795-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="40795-154">La clase `Movie` completa ahora es similar al código siguiente:</span><span class="sxs-lookup"><span data-stu-id="40795-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="40795-155">Compile la aplicación (Ctrl + Mayús + B).</span><span class="sxs-lookup"><span data-stu-id="40795-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="40795-156">Dado que ha agregado un nuevo campo a la clase `Movie`, también debe actualizar la *lista blanca* de enlaces para que se incluya esta nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="40795-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="40795-157">Actualice el atributo `bind` de `Create` y `Edit` métodos de acción para incluir la propiedad `Rating`:</span><span class="sxs-lookup"><span data-stu-id="40795-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="40795-158">También debe actualizar las plantillas de vista para mostrar, crear y editar la nueva propiedad `Rating` en la vista del explorador.</span><span class="sxs-lookup"><span data-stu-id="40795-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="40795-159">Abra el archivo *\Views\Movies\Index.cshtml* y agregue un encabezado de columna `<th>Rating</th>` justo después de la columna **Price** .</span><span class="sxs-lookup"><span data-stu-id="40795-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="40795-160">A continuación, agregue una `<td>` columna cerca del final de la plantilla para representar el valor `@item.Rating`.</span><span class="sxs-lookup"><span data-stu-id="40795-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="40795-161">A continuación se muestra el aspecto de la plantilla de vista *index. cshtml* actualizada:</span><span class="sxs-lookup"><span data-stu-id="40795-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="40795-162">A continuación, abra el archivo *\Views\Movies\Create.cshtml* y agregue el campo `Rating` con el siguiente marcado resaltado.</span><span class="sxs-lookup"><span data-stu-id="40795-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighted markup.</span></span> <span data-ttu-id="40795-163">Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.</span><span class="sxs-lookup"><span data-stu-id="40795-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="40795-164">Ahora ha actualizado el código de aplicación para admitir la nueva propiedad `Rating`.</span><span class="sxs-lookup"><span data-stu-id="40795-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="40795-165">Ejecute la aplicación y vaya a la dirección URL de */Movies* .</span><span class="sxs-lookup"><span data-stu-id="40795-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="40795-166">Sin embargo, cuando lo haga, verá uno de los siguientes errores:</span><span class="sxs-lookup"><span data-stu-id="40795-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="40795-167">El modelo de respaldo del contexto ' MovieDBContext ' ha cambiado desde que se creó la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="40795-168">Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="40795-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="40795-169">Está viendo este error porque la clase del modelo de `Movie` actualizada en la aplicación es ahora diferente del esquema de la tabla de `Movie` de la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="40795-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="40795-170">(No hay ninguna columna `Rating` en la tabla de la base de datos).</span><span class="sxs-lookup"><span data-stu-id="40795-170">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="40795-171">Este error se puede resolver de varias maneras:</span><span class="sxs-lookup"><span data-stu-id="40795-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="40795-172">Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo.</span><span class="sxs-lookup"><span data-stu-id="40795-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="40795-173">Este enfoque resulta muy conveniente al principio del ciclo de desarrollo cuando se está realizando el desarrollo activo en una base de datos de prueba; permite desarrollar rápidamente el esquema del modelo y la base de datos juntos.</span><span class="sxs-lookup"><span data-stu-id="40795-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="40795-174">Sin embargo, el inconveniente es que se pierden los datos existentes en la base de datos, por lo que *no* se desea usar este enfoque en una base de datos de producción.</span><span class="sxs-lookup"><span data-stu-id="40795-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="40795-175">Usar un inicializador para inicializar automáticamente una base de datos con datos de prueba suele ser una manera productiva de desarrollar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="40795-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="40795-176">Para obtener más información sobre Entity Framework inicializadores de base de datos, vea el [tutorial de ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="40795-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="40795-177">Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="40795-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="40795-178">La ventaja de este enfoque es que se conservan los datos.</span><span class="sxs-lookup"><span data-stu-id="40795-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="40795-179">Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="40795-180">Use Migraciones de Code First para actualizar el esquema de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-180">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="40795-181">Para este tutorial se usa Migraciones de Code First.</span><span class="sxs-lookup"><span data-stu-id="40795-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="40795-182">Actualice el método de inicialización para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="40795-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="40795-183">Abra el archivo Migrations\Configuration.cs y agregue un campo rating a cada objeto Movie.</span><span class="sxs-lookup"><span data-stu-id="40795-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="40795-184">Compile la solución y, a continuación, abra la ventana de la **consola del administrador de paquetes** y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="40795-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="40795-185">El comando `add-migration` indica al marco de migración que examine el modelo de película actual con el esquema de la base de la película actual y cree el código necesario para migrar la base de BD al nuevo modelo.</span><span class="sxs-lookup"><span data-stu-id="40795-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="40795-186">La *clasificación* del nombre es arbitraria y se usa para asignar un nombre al archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="40795-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="40795-187">Resulta útil usar un nombre descriptivo para el paso de migración.</span><span class="sxs-lookup"><span data-stu-id="40795-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="40795-188">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define el nuevo `DbMigration` clase derivada y, en el método `Up`, puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="40795-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="40795-189">Compile la solución y, a continuación, escriba el comando `update-database` en la ventana de la **consola del administrador de paquetes** .</span><span class="sxs-lookup"><span data-stu-id="40795-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="40795-190">La siguiente imagen muestra la salida en la ventana de la **consola del administrador de paquetes** (la *clasificación* de fecha pendiente de la marca de fecha será diferente).</span><span class="sxs-lookup"><span data-stu-id="40795-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="40795-191">Vuelva a ejecutar la aplicación y vaya a la dirección URL de/movies.</span><span class="sxs-lookup"><span data-stu-id="40795-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="40795-192">Puede ver el nuevo campo de clasificación.</span><span class="sxs-lookup"><span data-stu-id="40795-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="40795-193">Haga clic en el vínculo **crear nuevo** para agregar una nueva película.</span><span class="sxs-lookup"><span data-stu-id="40795-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="40795-194">Tenga en cuenta que puede Agregar una clasificación.</span><span class="sxs-lookup"><span data-stu-id="40795-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="40795-196">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="40795-196">Click **Create**.</span></span> <span data-ttu-id="40795-197">La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:</span><span class="sxs-lookup"><span data-stu-id="40795-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="40795-199">Ahora que el proyecto está usando migraciones, no tendrá que quitar la base de datos al agregar un nuevo campo o actualizar el esquema.</span><span class="sxs-lookup"><span data-stu-id="40795-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="40795-200">En la siguiente sección, realizaremos más cambios en el esquema y usaremos migraciones para actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="40795-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="40795-201">También debe agregar el campo `Rating` a las plantillas de vista Edit, details y DELETE.</span><span class="sxs-lookup"><span data-stu-id="40795-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="40795-202">Podría escribir de nuevo el comando "Update-Database" en la ventana de la **consola del administrador de paquetes** y no se ejecutaría ningún código de migración, porque el esquema coincide con el modelo.</span><span class="sxs-lookup"><span data-stu-id="40795-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="40795-203">Sin embargo, la ejecución de "Update-Database" volverá a ejecutar el método `Seed` y, si ha cambiado alguno de los datos de inicialización, se perderán los cambios porque el método de `Seed` upserts datos.</span><span class="sxs-lookup"><span data-stu-id="40795-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="40795-204">Puede leer más sobre el método `Seed` en el [tutorial popular ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="40795-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="40795-205">En esta sección vio cómo puede modificar objetos de modelo y mantener la base de datos sincronizada con los cambios.</span><span class="sxs-lookup"><span data-stu-id="40795-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="40795-206">También aprendió una manera de rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios.</span><span class="sxs-lookup"><span data-stu-id="40795-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="40795-207">Esta es solo una introducción rápida a Code First, vea [creación de un modelo de datos Entity Framework para una aplicación ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para obtener un tutorial más completo sobre el tema.</span><span class="sxs-lookup"><span data-stu-id="40795-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="40795-208">A continuación, echemos un vistazo a cómo puede Agregar una lógica de validación más enriquecida a las clases de modelo y permitir que se apliquen algunas reglas de negocios.</span><span class="sxs-lookup"><span data-stu-id="40795-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40795-209">[Anterior](adding-search.md)
> [Siguiente](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="40795-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
