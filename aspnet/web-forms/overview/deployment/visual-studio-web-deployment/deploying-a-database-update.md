---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implementación web de ASP.NET con Visual Studio: implementación de una actualización de base de datos | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440791"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="5e34a-103">Implementación web de ASP.NET con Visual Studio: implementar una actualización de base de datos</span><span class="sxs-lookup"><span data-stu-id="5e34a-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="5e34a-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5e34a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="5e34a-105">Descargar el proyecto de inicio</span><span class="sxs-lookup"><span data-stu-id="5e34a-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="5e34a-106">En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="5e34a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="5e34a-107">Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5e34a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="5e34a-108">Información general</span><span class="sxs-lookup"><span data-stu-id="5e34a-108">Overview</span></span>

<span data-ttu-id="5e34a-109">En este tutorial, realizará un cambio en la base de datos y los cambios en el código relacionado, probará los cambios en Visual Studio y, a continuación, implementará la actualización en los entornos de prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="5e34a-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="5e34a-110">En primer lugar, el tutorial muestra cómo actualizar una base de datos administrada por Migraciones de Code First y, posteriormente, muestra cómo actualizar una base de datos mediante el proveedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="5e34a-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="5e34a-111">Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5e34a-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="5e34a-112">Implementar una actualización de base de datos mediante Migraciones de Code First</span><span class="sxs-lookup"><span data-stu-id="5e34a-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="5e34a-113">En esta sección, agregará una columna de fecha de nacimiento a la clase base `Person` para las entidades `Student` y `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5e34a-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="5e34a-114">A continuación, actualice la página que muestra los datos del instructor para que muestren la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="5e34a-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="5e34a-115">Por último, implemente los cambios de prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="5e34a-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="5e34a-116">Agregar una columna a una tabla en la base de datos de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5e34a-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="5e34a-117">En el proyecto *ContosoUniversity. Dal* , Abra *Person.CS* y agregue la siguiente propiedad al final de la clase `Person` (debe haber dos llaves de cierre después):</span><span class="sxs-lookup"><span data-stu-id="5e34a-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="5e34a-118">A continuación, actualice el método de `Seed` para que proporcione un valor para la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="5e34a-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="5e34a-119">Abra *Migrations\Configuration.CS* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` por el siguiente bloque de código que incluye la información de fecha de nacimiento:</span><span class="sxs-lookup"><span data-stu-id="5e34a-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="5e34a-120">Compile la solución y, a continuación, abra la ventana **consola del administrador de paquetes** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="5e34a-121">Asegúrese de que ContosoUniversity. DAL sigue seleccionado como **proyecto predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="5e34a-122">En la ventana de la **consola del administrador de paquetes** , seleccione **ContosoUniversity. Dal** como **proyecto predeterminado**y, a continuación, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="5e34a-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="5e34a-123">Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva clase de `DbMigration` y, en el método `Up`, puede ver el código que crea la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="5e34a-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="5e34a-124">El método `Up` crea la columna cuando se está implementando el cambio y el método `Down` elimina la columna cuando se revierte el cambio.</span><span class="sxs-lookup"><span data-stu-id="5e34a-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="5e34a-126">Compile la solución y, a continuación, escriba el siguiente comando en la ventana de la **consola del administrador de paquetes** (Asegúrese de que el proyecto CONTOSOUNIVERSITY. Dal todavía está seleccionado):</span><span class="sxs-lookup"><span data-stu-id="5e34a-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="5e34a-127">El Entity Framework ejecuta el método `Up` y, a continuación, ejecuta el método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="5e34a-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="5e34a-128">Mostrar la nueva columna en la página de instructores</span><span class="sxs-lookup"><span data-stu-id="5e34a-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="5e34a-129">En el proyecto ContosoUniversity, Abra *instructors. aspx* y agregue un nuevo campo plantilla para mostrar la fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="5e34a-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="5e34a-130">Agréguelo entre los de la fecha de contratación y la asignación de la oficina:</span><span class="sxs-lookup"><span data-stu-id="5e34a-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="5e34a-131">(Si la sangría de código no está sincronizada, puede presionar CTRL-K y, a continuación, CTRL + D para cambiar el formato del archivo automáticamente).</span><span class="sxs-lookup"><span data-stu-id="5e34a-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="5e34a-132">Ejecute la aplicación y haga clic en el vínculo **Instructors** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="5e34a-133">Cuando se cargue la página, verá que tiene el nuevo campo de fecha de nacimiento.</span><span class="sxs-lookup"><span data-stu-id="5e34a-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Página de instructores con FechaNacimiento](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="5e34a-135">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="5e34a-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="5e34a-136">Implementar la actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="5e34a-136">Deploy the database update</span></span>

1. <span data-ttu-id="5e34a-137">En **Explorador de soluciones** Seleccione el proyecto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="5e34a-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="5e34a-138">En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="5e34a-139">(Si la barra de herramientas está deshabilitada, seleccione el proyecto ContosoUniversity en **Explorador de soluciones**).</span><span class="sxs-lookup"><span data-stu-id="5e34a-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="5e34a-140">Visual Studio implementa la aplicación actualizada y el explorador se abre en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="5e34a-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="5e34a-141">Ejecute la página **instructores** para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="5e34a-142">Cuando la aplicación intenta obtener acceso a la base de datos de esta página, Code First actualiza el esquema de la base de datos y ejecuta el método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="5e34a-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="5e34a-143">Cuando se muestre la página, verá la columna **fecha de nacimiento** esperada con fechas.</span><span class="sxs-lookup"><span data-stu-id="5e34a-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="5e34a-144">En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **ensayo** y, a continuación, haga clic en **publicar web**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="5e34a-145">Ejecute la página de **instructores** en el almacenamiento provisional para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="5e34a-146">En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **producción** y, a continuación, haga clic en **publicar web**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="5e34a-147">Ejecute la página de **instructores** en producción para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="5e34a-148">En el caso de una actualización de la aplicación de producción real que incluye un cambio en la base de datos, normalmente la aplicación también se desconecta durante la implementación mediante *app\_offline. htm*, como se vio en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="5e34a-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="5e34a-149">Implementar una actualización de base de datos mediante el proveedor dbDacFx</span><span class="sxs-lookup"><span data-stu-id="5e34a-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="5e34a-150">En esta sección, agregará una columna de *comentarios* a la tabla de *usuario* en la base de datos de pertenencia y creará una página que le permitirá mostrar y editar los comentarios de cada usuario.</span><span class="sxs-lookup"><span data-stu-id="5e34a-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="5e34a-151">Después, implemente los cambios de prueba, ensayo y producción.</span><span class="sxs-lookup"><span data-stu-id="5e34a-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="5e34a-152">Agregar una columna a una tabla en la base de datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="5e34a-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="5e34a-153">En Visual Studio, Abra **Explorador de objetos de SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="5e34a-154">Expanda **(LocalDB) \v11.0**, expanda **bases de datos**, expanda **ASPNET-ContosoUniversity** (no **ASPNET-ContosoUniversity-Prod**) y, a continuación, expanda **tablas**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="5e34a-155">Si no ve **(LocalDB) \v11.0** en el nodo **SQL Server** , haga clic con el botón secundario en el nodo **SQL Server** y haga clic en **Agregar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="5e34a-156">En el cuadro de diálogo **conectar al servidor** , escriba *(LocalDB) \v11.0* como el **nombre del servidor**y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="5e34a-157">Si no ve **ASPNET-ContosoUniversity**, ejecute el proyecto e inicie sesión con las credenciales de *Administrador* (la contraseña es *devpwd*) y, a continuación, actualice la ventana **Explorador de objetos de SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="5e34a-158">Haga clic con el botón secundario en la tabla **usuarios** y, a continuación, haga clic en **Diseñador de vistas**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Diseñador de vistas SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="5e34a-160">En el diseñador, agregue una columna *comentarios* y escríbala en *nvarchar (128)* y acepte valores NULL y, a continuación, haga clic en **Actualizar**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Agregar una columna de comentarios](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="5e34a-162">En el cuadro **vista previa de actualizaciones de base de datos** , haga clic en **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Vista previa de actualizaciones de base de datos](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="5e34a-164">Crear una página para mostrar y editar la nueva columna</span><span class="sxs-lookup"><span data-stu-id="5e34a-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="5e34a-165">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta **account** del proyecto ContosoUniversity, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="5e34a-166">Cree un nuevo **formulario Web Forms con la página maestra** y asígnele el nombre *UserInfo. aspx*.</span><span class="sxs-lookup"><span data-stu-id="5e34a-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="5e34a-167">Acepte el archivo *site. Master* predeterminado como la página maestra.</span><span class="sxs-lookup"><span data-stu-id="5e34a-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="5e34a-168">Copie el marcado siguiente en el elemento `MainContent` `Content` (el último de los tres elementos `Content`):</span><span class="sxs-lookup"><span data-stu-id="5e34a-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="5e34a-169">Haga clic con el botón secundario en la página *UserInfo. aspx* y haga clic en **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="5e34a-170">Inicie sesión con sus credenciales de usuario de *Administrador* (la contraseña es *devpwd*) y agregue algunos comentarios a un usuario para comprobar que la página funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="5e34a-172">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="5e34a-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="5e34a-173">Implementar la actualización de la base de datos</span><span class="sxs-lookup"><span data-stu-id="5e34a-173">Deploy the database update</span></span>

<span data-ttu-id="5e34a-174">Para realizar la implementación mediante el proveedor dbDacFx, solo tiene que seleccionar la opción **Actualizar base de datos** en el perfil de publicación.</span><span class="sxs-lookup"><span data-stu-id="5e34a-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="5e34a-175">Sin embargo, para la implementación inicial, cuando se usa esta opción, también se configuraron algunos scripts SQL adicionales para ejecutarlos: todavía están en el perfil y tendrá que evitar que se ejecuten de nuevo.</span><span class="sxs-lookup"><span data-stu-id="5e34a-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="5e34a-176">Para abrir el Asistente para **publicación web** , haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="5e34a-177">Seleccione el perfil de **prueba** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="5e34a-178">Haga clic en la pestaña **Configuración** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="5e34a-179">En **DefaultConnection**, seleccione **Actualizar base de datos**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="5e34a-180">Deshabilite los scripts adicionales que configuró para ejecutarse para la implementación inicial:</span><span class="sxs-lookup"><span data-stu-id="5e34a-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="5e34a-181">Haga clic en **configurar actualizaciones de base de datos**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="5e34a-182">En el cuadro de diálogo **configurar actualizaciones de base de datos** , desactive las casillas situadas junto a *Grant. SQL* y *ASPNET-Data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="5e34a-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="5e34a-183">Haga clic en **Cerrar**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-183">Click **Close**.</span></span>
6. <span data-ttu-id="5e34a-184">Haga clic en la pestaña **Vista previa** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="5e34a-185">En **bases** de datos y a la derecha de **DefaultConnection**, haga clic en el vínculo **vista previa de base de datos** .</span><span class="sxs-lookup"><span data-stu-id="5e34a-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Vista previa de base de datos](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="5e34a-187">La ventana de vista previa muestra el script que se ejecutará en la base de datos de destino para que el esquema de la base de datos coincida con el esquema de la base de datos de origen.</span><span class="sxs-lookup"><span data-stu-id="5e34a-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="5e34a-188">El script incluye un comando ALTER TABLE que agrega la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="5e34a-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="5e34a-189">Cierre el cuadro de diálogo **vista previa de base de datos** y, a continuación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="5e34a-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="5e34a-190">Visual Studio implementa la aplicación actualizada y el explorador se abre en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="5e34a-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="5e34a-191">Ejecute la página UserInfo (agregue *account/UserInfo. aspx* a la dirección URL de la Página principal) para comprobar que la actualización se ha implementado correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="5e34a-192">Tendrá que iniciar sesión especificando *admin* y *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="5e34a-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="5e34a-193">Los datos de las tablas no se implementan de forma predeterminada y no se configuró un script de implementación de datos para ejecutarse, por lo que no encontrará el comentario que agregó en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="5e34a-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="5e34a-194">Puede Agregar un nuevo comentario ahora en ensayo para comprobar que el cambio se implementó en la base de datos y que la página funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="5e34a-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="5e34a-195">Siga el mismo procedimiento para realizar la implementación en el entorno de ensayo y la producción.</span><span class="sxs-lookup"><span data-stu-id="5e34a-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="5e34a-196">No olvide deshabilitar los scripts adicionales.</span><span class="sxs-lookup"><span data-stu-id="5e34a-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="5e34a-197">La única diferencia en comparación con el perfil de prueba es que solo se deshabilitará un script en los perfiles de ensayo y de producción, ya que se configuraron para ejecutar solo *ASPNET-Prod-Data. SQL*.</span><span class="sxs-lookup"><span data-stu-id="5e34a-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="5e34a-198">Las credenciales para el almacenamiento provisional y la producción son admin y prodpwd.</span><span class="sxs-lookup"><span data-stu-id="5e34a-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="5e34a-199">En el caso de una actualización de la aplicación de producción real que incluye un cambio en la base de datos, normalmente también se desconecta la aplicación durante la implementación mediante la carga de *app\_offline. htm* antes de publicarla y eliminarla después, como se vio en [el tutorial anterior](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="5e34a-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="5e34a-200">Resumen</span><span class="sxs-lookup"><span data-stu-id="5e34a-200">Summary</span></span>

<span data-ttu-id="5e34a-201">Ahora ha implementado una actualización de la aplicación que incluye un cambio en la base de datos mediante Migraciones de Code First y el proveedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="5e34a-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Página de instructores con FechaNacimiento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="5e34a-204">En el siguiente tutorial se muestra cómo ejecutar implementaciones mediante la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="5e34a-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e34a-205">[Anterior](deploying-a-code-update.md)
> [Siguiente](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="5e34a-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
