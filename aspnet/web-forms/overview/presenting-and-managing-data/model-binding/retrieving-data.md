---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520027"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="a10bb-104">Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="a10bb-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="a10bb-105">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a10bb-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="a10bb-106">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="a10bb-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="a10bb-107">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="a10bb-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="a10bb-108">El patrón de enlace de modelos funciona con cualquier tecnología de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="a10bb-109">En este tutorial, usará Entity Framework, pero puede usar la tecnología de acceso a datos que le resulte más familiar.</span><span class="sxs-lookup"><span data-stu-id="a10bb-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="a10bb-110">En un control de servidor enlazado a datos, como un control GridView, ListView, DetailsView o FormView, se especifican los nombres de los métodos que se van a usar para seleccionar, actualizar, eliminar y crear datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="a10bb-111">En este tutorial, va a especificar un valor para SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="a10bb-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="a10bb-112">Dentro de ese método, se proporciona la lógica para recuperar los datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="a10bb-113">En el siguiente tutorial, establecerá los valores de UpdateMethod, DeleteMethod y InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="a10bb-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="a10bb-114">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a10bb-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="a10bb-115">El código descargable funciona con Visual Studio 2012 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a10bb-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="a10bb-116">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2017 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a10bb-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="a10bb-117">En el tutorial, ejecute la aplicación en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a10bb-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="a10bb-118">También puede implementar la aplicación en un proveedor de hospedaje y hacer que esté disponible a través de Internet.</span><span class="sxs-lookup"><span data-stu-id="a10bb-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="a10bb-119">Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un</span><span class="sxs-lookup"><span data-stu-id="a10bb-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="a10bb-120">[cuenta de evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="a10bb-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="a10bb-121">Para obtener información sobre cómo implementar un proyecto Web de Visual Studio en Azure App Service Web Apps, consulte [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span><span class="sxs-lookup"><span data-stu-id="a10bb-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="a10bb-122">En este tutorial también se muestra cómo usar Migraciones de Entity Framework Code First para implementar la base de datos SQL Server en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a10bb-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a10bb-123">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a10bb-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="a10bb-124">Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="a10bb-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="a10bb-125">Este tutorial también funciona con Visual Studio 2012 y Visual Studio 2013, pero hay algunas diferencias en la interfaz de usuario y la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="a10bb-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="a10bb-126">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="a10bb-126">What you'll build</span></span>

<span data-ttu-id="a10bb-127">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a10bb-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="a10bb-128">Cree objetos de datos que reflejen una Universidad con estudiantes inscritos en cursos</span><span class="sxs-lookup"><span data-stu-id="a10bb-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="a10bb-129">Crear tablas de base de datos a partir de objetos</span><span class="sxs-lookup"><span data-stu-id="a10bb-129">Build database tables from the objects</span></span>
* <span data-ttu-id="a10bb-130">Rellenar la base de datos con datos de prueba</span><span class="sxs-lookup"><span data-stu-id="a10bb-130">Populate the database with test data</span></span>
* <span data-ttu-id="a10bb-131">Mostrar datos en un formulario Web Forms</span><span class="sxs-lookup"><span data-stu-id="a10bb-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a10bb-132">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="a10bb-132">Create the project</span></span>

1. <span data-ttu-id="a10bb-133">En Visual Studio 2017, cree un proyecto de **aplicación Web de ASP.net (.NET Framework)** denominado **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![creación de proyecto](retrieving-data/_static/image19.png)

2. <span data-ttu-id="a10bb-135">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-135">Select **OK**.</span></span> <span data-ttu-id="a10bb-136">Aparece el cuadro de diálogo para seleccionar una plantilla.</span><span class="sxs-lookup"><span data-stu-id="a10bb-136">The dialog box to select a template appears.</span></span>

   ![seleccionar formularios Web Forms](retrieving-data/_static/image3.png)

3. <span data-ttu-id="a10bb-138">Seleccione la plantilla de **formularios Web Forms** .</span><span class="sxs-lookup"><span data-stu-id="a10bb-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="a10bb-139">Si es necesario, cambie la autenticación a **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="a10bb-140">Seleccione **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a10bb-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="a10bb-141">Modificar la apariencia del sitio</span><span class="sxs-lookup"><span data-stu-id="a10bb-141">Modify site appearance</span></span>

   <span data-ttu-id="a10bb-142">Realice algunos cambios para personalizar la apariencia del sitio.</span><span class="sxs-lookup"><span data-stu-id="a10bb-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="a10bb-143">Abra el archivo site. Master.</span><span class="sxs-lookup"><span data-stu-id="a10bb-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="a10bb-144">Cambie el título para que muestre **contoso University** y no **la aplicación ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="a10bb-145">Cambie el texto del encabezado del nombre de la **aplicación** a **contoso University**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="a10bb-146">Cambie los vínculos del encabezado de navegación a los apropiados para el sitio.</span><span class="sxs-lookup"><span data-stu-id="a10bb-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="a10bb-147">Quite los vínculos de **acerca** de y **póngase en contacto** con y, en su lugar, vincule a una página de **estudiantes** , que creará.</span><span class="sxs-lookup"><span data-stu-id="a10bb-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="a10bb-148">Guarde site. Master.</span><span class="sxs-lookup"><span data-stu-id="a10bb-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="a10bb-149">Agregar un formulario web para mostrar los datos de los estudiantes</span><span class="sxs-lookup"><span data-stu-id="a10bb-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="a10bb-150">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar** y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="a10bb-151">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione el **formulario web con la plantilla de página maestra** y asígnele el nombre **Students. aspx**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![Crear página](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="a10bb-153">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="a10bb-154">En la página maestra del formulario web, seleccione **site. Master**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="a10bb-155">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="a10bb-156">Agregar el modelo de datos</span><span class="sxs-lookup"><span data-stu-id="a10bb-156">Add the data model</span></span>

<span data-ttu-id="a10bb-157">En la carpeta **Models** , agregue una clase denominada **UniversityModels.CS**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="a10bb-158">Haga clic con el botón derecho en **modelos**, seleccione **Agregar**y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="a10bb-159">Aparecerá el cuadro de diálogo **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="a10bb-160">En el menú de navegación izquierdo, seleccione **código**y, a continuación, **clase**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![crear clase de modelo](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="a10bb-162">Asigne a la clase el nombre **UniversityModels.CS** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="a10bb-163">En este archivo, defina las clases `SchoolContext`, `Student`, `Enrollment`y `Course` de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="a10bb-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="a10bb-164">La clase `SchoolContext` deriva de `DbContext`, que administra la conexión a la base de datos y los cambios en los datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="a10bb-165">En la clase `Student`, observe los atributos que se aplican a las propiedades `FirstName`, `LastName`y `Year`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="a10bb-166">En este tutorial se usan estos atributos para la validación de datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="a10bb-167">Para simplificar el código, solo estas propiedades se marcan con los atributos de validación de datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="a10bb-168">En un proyecto real, aplicaría atributos de validación a todas las propiedades que necesitan validación.</span><span class="sxs-lookup"><span data-stu-id="a10bb-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="a10bb-169">Guarde UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="a10bb-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="a10bb-170">Configurar la base de datos basada en clases</span><span class="sxs-lookup"><span data-stu-id="a10bb-170">Set up the database based on classes</span></span>

<span data-ttu-id="a10bb-171">En este tutorial se usa [migraciones de Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) para crear objetos y tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="a10bb-172">Estas tablas almacenan información acerca de los estudiantes y sus cursos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="a10bb-173">Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="a10bb-174">En la **consola del administrador de paquetes**, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="a10bb-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="a10bb-175">Si el comando se completa correctamente, aparece un mensaje que indica que se han habilitado las migraciones.</span><span class="sxs-lookup"><span data-stu-id="a10bb-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![habilitar migraciones](retrieving-data/_static/image8.png)

      <span data-ttu-id="a10bb-177">Observe que se ha creado un archivo denominado *Configuration.CS* .</span><span class="sxs-lookup"><span data-stu-id="a10bb-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="a10bb-178">La clase `Configuration` tiene un método `Seed`, que puede rellenar previamente las tablas de base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a10bb-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="a10bb-179">Rellenar previamente la base de datos</span><span class="sxs-lookup"><span data-stu-id="a10bb-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="a10bb-180">Abra Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="a10bb-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="a10bb-181">Agregue el código siguiente al método `Seed` .</span><span class="sxs-lookup"><span data-stu-id="a10bb-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="a10bb-182">Además, agregue una instrucción `using` para el espacio de nombres `ContosoUniversityModelBinding. Models`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="a10bb-183">Guarde Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="a10bb-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="a10bb-184">En la consola del administrador de paquetes, ejecute el comando **Add-Migration Initial**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="a10bb-185">Ejecute el comando **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="a10bb-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="a10bb-186">Si recibe una excepción al ejecutar este comando, los valores `StudentID` y `CourseID` pueden ser diferentes de los valores del método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="a10bb-187">Abra esas tablas de base de datos y busque los valores existentes para `StudentID` y `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="a10bb-188">Agregue esos valores al código para inicializar la tabla `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="a10bb-189">Agregar un control GridView</span><span class="sxs-lookup"><span data-stu-id="a10bb-189">Add a GridView control</span></span>

<span data-ttu-id="a10bb-190">Con datos de base de datos rellenados, ahora está listo para recuperar los datos y mostrarlos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="a10bb-191">Abra Students. aspx.</span><span class="sxs-lookup"><span data-stu-id="a10bb-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="a10bb-192">Busque el marcador de posición `MainContent`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="a10bb-193">Dentro de ese marcador de posición, agregue un control **GridView** que incluya este código.</span><span class="sxs-lookup"><span data-stu-id="a10bb-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="a10bb-194">Cosas que hay que tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="a10bb-194">Things to note:</span></span>
   * <span data-ttu-id="a10bb-195">Observe el valor establecido para la propiedad `SelectMethod` en el elemento GridView.</span><span class="sxs-lookup"><span data-stu-id="a10bb-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="a10bb-196">Este valor especifica el método usado para recuperar los datos de GridView, que se crean en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="a10bb-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="a10bb-197">La propiedad `ItemType` se establece en la clase `Student` creada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a10bb-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="a10bb-198">Esta configuración le permite hacer referencia a las propiedades de clase en el marcado.</span><span class="sxs-lookup"><span data-stu-id="a10bb-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="a10bb-199">Por ejemplo, la clase `Student` tiene una colección denominada `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="a10bb-200">Puede usar `Item.Enrollments` para recuperar esa colección y, a continuación, utilizar la [Sintaxis de LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) para recuperar la suma de los créditos inscritos de cada estudiante.</span><span class="sxs-lookup"><span data-stu-id="a10bb-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="a10bb-201">Guarde Students. aspx.</span><span class="sxs-lookup"><span data-stu-id="a10bb-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="a10bb-202">Agregar código para recuperar datos</span><span class="sxs-lookup"><span data-stu-id="a10bb-202">Add code to retrieve data</span></span>

   <span data-ttu-id="a10bb-203">En el archivo de código subyacente Students. aspx, agregue el método especificado para el valor `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="a10bb-204">Abra Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="a10bb-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="a10bb-205">Agregue `using` instrucciones para los espacios de nombres `ContosoUniversityModelBinding. Models` y `System.Data.Entity`.</span><span class="sxs-lookup"><span data-stu-id="a10bb-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="a10bb-206">Agregue el método especificado para `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="a10bb-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="a10bb-207">La cláusula `Include` mejora el rendimiento de las consultas, pero no es necesario.</span><span class="sxs-lookup"><span data-stu-id="a10bb-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="a10bb-208">Sin la cláusula `Include`, los datos se recuperan mediante la [*carga diferida*](https://en.wikipedia.org/wiki/Lazy_loading), lo que implica el envío de una consulta independiente a la base de datos cada vez que se recuperan los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="a10bb-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="a10bb-209">Con la cláusula `Include`, los datos se recuperan mediante la *carga diligente*, lo que significa que una consulta de base de datos única recupera todos los datos relacionados.</span><span class="sxs-lookup"><span data-stu-id="a10bb-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="a10bb-210">Si no se usan datos relacionados, la carga diligente es menos eficaz porque se recuperan más datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="a10bb-211">Sin embargo, en este caso, la carga diligente le ofrece el mejor rendimiento, ya que los datos relacionados se muestran para cada registro.</span><span class="sxs-lookup"><span data-stu-id="a10bb-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="a10bb-212">Para obtener más información sobre las consideraciones de rendimiento al cargar datos relacionados, consulte la sección sobre la **carga diferida, diligente y explícita de datos relacionados** en el artículo sobre la [lectura de datos relacionados con el Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="a10bb-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="a10bb-213">De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave.</span><span class="sxs-lookup"><span data-stu-id="a10bb-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="a10bb-214">Puede Agregar una cláusula `OrderBy` para especificar un valor de ordenación diferente.</span><span class="sxs-lookup"><span data-stu-id="a10bb-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="a10bb-215">En este ejemplo, la propiedad `StudentID` predeterminada se usa para la ordenación.</span><span class="sxs-lookup"><span data-stu-id="a10bb-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="a10bb-216">En el artículo [ordenación, paginación y filtrado de datos](sorting-paging-and-filtering-data.md) , el usuario está habilitado para seleccionar una columna para la ordenación.</span><span class="sxs-lookup"><span data-stu-id="a10bb-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="a10bb-217">Guarde Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="a10bb-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="a10bb-218">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="a10bb-218">Run your application</span></span> 

<span data-ttu-id="a10bb-219">Ejecute la aplicación web (**F5**) y vaya a la página **Students** , que muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a10bb-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="a10bb-221">Generación automática de métodos de enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="a10bb-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="a10bb-222">Al trabajar en esta serie de tutoriales, simplemente puede copiar el código del tutorial al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a10bb-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="a10bb-223">Sin embargo, una desventaja de este enfoque es que es posible que no sea consciente de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="a10bb-224">Cuando trabaje en sus propios proyectos, la generación automática de código puede ahorrarle tiempo y ayudarle a tener una idea de cómo implementar una operación.</span><span class="sxs-lookup"><span data-stu-id="a10bb-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="a10bb-225">En esta sección se describe la característica de generación automática de código.</span><span class="sxs-lookup"><span data-stu-id="a10bb-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="a10bb-226">Esta sección solo es informativa y no contiene ningún código que deba implementar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a10bb-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="a10bb-227">Al establecer un valor para las propiedades `SelectMethod`, `UpdateMethod`, `InsertMethod`o `DeleteMethod` en el código de marcado, puede seleccionar la opción **crear nuevo método** .</span><span class="sxs-lookup"><span data-stu-id="a10bb-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![crear un método](retrieving-data/_static/image18.png)

<span data-ttu-id="a10bb-229">Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación para realizar la operación.</span><span class="sxs-lookup"><span data-stu-id="a10bb-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="a10bb-230">Si primero establece la propiedad `ItemType` antes de utilizar la característica de generación automática de código, el código generado utiliza ese tipo para las operaciones.</span><span class="sxs-lookup"><span data-stu-id="a10bb-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="a10bb-231">Por ejemplo, al establecer la propiedad `UpdateMethod`, se genera automáticamente el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a10bb-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="a10bb-232">De nuevo, no es necesario agregar este código al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a10bb-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="a10bb-233">En el siguiente tutorial, implementará métodos para actualizar, eliminar y agregar nuevos datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="a10bb-234">Resumen</span><span class="sxs-lookup"><span data-stu-id="a10bb-234">Summary</span></span>

<span data-ttu-id="a10bb-235">En este tutorial, ha creado clases de modelo de datos y ha generado una base de datos a partir de esas clases.</span><span class="sxs-lookup"><span data-stu-id="a10bb-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="a10bb-236">Ha rellenado las tablas de base de datos con datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="a10bb-236">You filled the database tables with test data.</span></span> <span data-ttu-id="a10bb-237">Usó el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.</span><span class="sxs-lookup"><span data-stu-id="a10bb-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="a10bb-238">En el siguiente [tutorial](updating-deleting-and-creating-data.md) de esta serie, habilitará la actualización, la eliminación y la creación de datos.</span><span class="sxs-lookup"><span data-stu-id="a10bb-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a10bb-239">Siguiente</span><span class="sxs-lookup"><span data-stu-id="a10bb-239">Next</span></span>](updating-deleting-and-creating-data.md)
