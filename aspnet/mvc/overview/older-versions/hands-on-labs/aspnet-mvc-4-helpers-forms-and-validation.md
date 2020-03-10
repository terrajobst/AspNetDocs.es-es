---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Aplicaciones auxiliares, formularios y validación de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: En los modelos de ASP.NET MVC 4 y el laboratorio práctico de acceso a datos, ha estado cargando y mostrando datos de la base de datos. En este laboratorio práctico, agregará a...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433795"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="7c649-104">Validación, formularios y asistentes de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="7c649-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="7c649-105">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7c649-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7c649-106">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="7c649-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7c649-107">En los **modelos de ASP.NET MVC 4 y** el laboratorio práctico de acceso a datos, ha estado cargando y mostrando datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="7c649-108">En este laboratorio práctico, agregará a la aplicación **Music Store** la capacidad de editar los datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="7c649-109">Teniendo en cuenta ese objetivo, primero creará el controlador que admitirá las acciones de creación, lectura, actualización y eliminación (CRUD) de los álbumes.</span><span class="sxs-lookup"><span data-stu-id="7c649-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="7c649-110">Se generará una plantilla de vista de índice que aprovecha la característica de scaffolding de ASP.NET MVC para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="7c649-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="7c649-111">Para mejorar la vista, agregará una aplicación auxiliar HTML personalizada que truncará las descripciones largas.</span><span class="sxs-lookup"><span data-stu-id="7c649-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="7c649-112">Después, agregará las vistas Editar y crear que le permitirá modificar los álbumes en la base de datos, con la ayuda de elementos de formulario como listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="7c649-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="7c649-113">Por último, permitirá que los usuarios eliminen un álbum y también les impedirá que escriban datos incorrectos mediante la validación de su entrada.</span><span class="sxs-lookup"><span data-stu-id="7c649-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="7c649-114">Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="7c649-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="7c649-115">Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC** .</span><span class="sxs-lookup"><span data-stu-id="7c649-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="7c649-116">Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="7c649-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="7c649-117">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7c649-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7c649-118">El proyecto específico de este laboratorio está disponible en las [aplicaciones auxiliares, formularios y validación de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="7c649-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7c649-119">Objetivos</span><span class="sxs-lookup"><span data-stu-id="7c649-119">Objectives</span></span>

<span data-ttu-id="7c649-120">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="7c649-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7c649-121">Creación de un controlador para admitir operaciones CRUD</span><span class="sxs-lookup"><span data-stu-id="7c649-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="7c649-122">Generar una vista de índice para mostrar las propiedades de la entidad en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="7c649-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="7c649-123">Agregar una aplicación auxiliar HTML personalizada</span><span class="sxs-lookup"><span data-stu-id="7c649-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="7c649-124">Crear y personalizar una vista de edición</span><span class="sxs-lookup"><span data-stu-id="7c649-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="7c649-125">Diferenciar entre los métodos de acción que reaccionan a las llamadas HTTP-GET o HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="7c649-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="7c649-126">Adición y personalización de una vista de creación</span><span class="sxs-lookup"><span data-stu-id="7c649-126">Add and customize a Create View</span></span>
- <span data-ttu-id="7c649-127">Controlar la eliminación de una entidad</span><span class="sxs-lookup"><span data-stu-id="7c649-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="7c649-128">Validación de entradas de usuario</span><span class="sxs-lookup"><span data-stu-id="7c649-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7c649-129">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7c649-129">Prerequisites</span></span>

<span data-ttu-id="7c649-130">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="7c649-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7c649-131">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="7c649-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7c649-132">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="7c649-132">Setup</span></span>

<span data-ttu-id="7c649-133">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="7c649-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="7c649-134">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c649-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7c649-135">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="7c649-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7c649-136">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice B: usar fragmentos de código](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7c649-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7c649-137">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="7c649-137">Exercises</span></span>

<span data-ttu-id="7c649-138">Los ejercicios siguientes constituyen este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="7c649-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="7c649-139">Creación del controlador del administrador de la tienda y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="7c649-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="7c649-140">Agregar una aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="7c649-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="7c649-141">Crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="7c649-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="7c649-142">Agregar una vista de creación</span><span class="sxs-lookup"><span data-stu-id="7c649-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="7c649-143">Controlar la eliminación</span><span class="sxs-lookup"><span data-stu-id="7c649-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="7c649-144">Agregar una validación</span><span class="sxs-lookup"><span data-stu-id="7c649-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="7c649-145">Usar jQuery discreto en el lado cliente</span><span class="sxs-lookup"><span data-stu-id="7c649-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="7c649-146">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="7c649-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="7c649-147">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="7c649-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="7c649-148">Tiempo estimado para completar este laboratorio: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="7c649-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="7c649-149">Ejercicio 1: creación del controlador del administrador de la tienda y su vista de índice</span><span class="sxs-lookup"><span data-stu-id="7c649-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="7c649-150">En este ejercicio, obtendrá información sobre cómo crear un controlador nuevo para admitir operaciones CRUD, personalizar su método de acción de índice para que devuelva una lista de álbumes de la base de datos y, por último, generar una plantilla de vista de índice que aproveche el scaffolding de ASP.NET MVC. característica para mostrar las propiedades de los álbumes en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="7c649-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="7c649-151">Tarea 1: creación de StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="7c649-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="7c649-152">En esta tarea, creará un nuevo controlador denominado **StoreManagerController** para admitir las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="7c649-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="7c649-153">Abra la carpeta **Begin** Solution ubicada en **source/EX1-CreatingTheStoreManagerController/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="7c649-154">Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-155">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-156">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-157">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-158">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-159">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-160">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-161">Agregue un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="7c649-161">Add a new controller.</span></span> <span data-ttu-id="7c649-162">Para ello, haga clic con el botón secundario en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, el comando **controlador** .</span><span class="sxs-lookup"><span data-stu-id="7c649-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="7c649-163">Cambie el **nombre** del controlador a **StoreManagerController** y asegúrese de que está seleccionada la opción **controlador de MVC con acciones de lectura/escritura vacías** .</span><span class="sxs-lookup"><span data-stu-id="7c649-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="7c649-164">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-164">Click **Add**.</span></span>

    <span data-ttu-id="7c649-165">![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Cuadro de diálogo Agregar controlador")</span><span class="sxs-lookup"><span data-stu-id="7c649-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="7c649-166">*Cuadro de diálogo Agregar controlador*</span><span class="sxs-lookup"><span data-stu-id="7c649-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="7c649-167">Se genera una nueva clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="7c649-167">A new Controller class is generated.</span></span> <span data-ttu-id="7c649-168">Dado que ha indicado agregar acciones para la lectura y escritura, los métodos de código auxiliar para estas, las acciones CRUD comunes se crean con los comentarios TODO rellenados, lo que solicita incluir la lógica específica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="7c649-169">Tarea 2: personalización del índice de StoreManager</span><span class="sxs-lookup"><span data-stu-id="7c649-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="7c649-170">En esta tarea, personalizará el método de acción de índice StoreManager para que devuelva una vista con la lista de álbumes de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="7c649-171">En la clase StoreManagerController, agregue las siguientes directivas *using* .</span><span class="sxs-lookup"><span data-stu-id="7c649-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="7c649-172">(Fragmento de código-ASP.NET de las *aplicaciones auxiliares y formularios y validación-EX1 con MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="7c649-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="7c649-173">Agregue un campo a **StoreManagerController** para que contenga una instancia de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="7c649-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="7c649-174">(Fragmento de código- *ASP.NET MVC 4 helpers y formularios y validación-EX1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="7c649-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="7c649-175">Implemente la acción de índice StoreManagerController para devolver una vista con la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="7c649-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="7c649-176">La lógica de acción del controlador será muy similar a la acción de índice de StoreController escrita anteriormente.</span><span class="sxs-lookup"><span data-stu-id="7c649-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="7c649-177">Use LINQ para recuperar todos los álbumes, incluida la información del género y el artista para su presentación.</span><span class="sxs-lookup"><span data-stu-id="7c649-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="7c649-178">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-EX1 StoreManagerController index*)</span><span class="sxs-lookup"><span data-stu-id="7c649-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="7c649-179">Tarea 3: crear la vista de índice</span><span class="sxs-lookup"><span data-stu-id="7c649-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="7c649-180">En esta tarea, creará la plantilla de vista de índice para mostrar la lista de álbumes devueltos por el controlador **StoreManager** .</span><span class="sxs-lookup"><span data-stu-id="7c649-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="7c649-181">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **cuadro de diálogo Agregar vista** Conozca la clase **album** que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="7c649-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="7c649-182">Seleccionar **compilación | Cree MvcMusicStore** para compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="7c649-183">Haga clic con el botón derecho en el método de acción de **Índice** y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="7c649-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="7c649-184">Se abrirá el cuadro de diálogo **Agregar vista** .</span><span class="sxs-lookup"><span data-stu-id="7c649-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="7c649-185">![Agregar vista](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="7c649-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="7c649-186">*Agregar una vista desde el método index*</span><span class="sxs-lookup"><span data-stu-id="7c649-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="7c649-187">En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Índice**.</span><span class="sxs-lookup"><span data-stu-id="7c649-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="7c649-188">Seleccione la opción **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** .</span><span class="sxs-lookup"><span data-stu-id="7c649-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="7c649-189">Seleccione **lista** en el menú desplegable **plantilla scaffolding** .</span><span class="sxs-lookup"><span data-stu-id="7c649-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="7c649-190">Deje el **motor de vista** en **Razor** y los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="7c649-191">![Agregar una vista de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Agregar una vista de índice")</span><span class="sxs-lookup"><span data-stu-id="7c649-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="7c649-192">*Agregar una vista de índice*</span><span class="sxs-lookup"><span data-stu-id="7c649-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="7c649-193">Tarea 4: personalizar el scaffolding de la vista de índice</span><span class="sxs-lookup"><span data-stu-id="7c649-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="7c649-194">En esta tarea, ajustará la plantilla de vista simple creada con la característica de scaffolding de ASP.NET MVC para que muestre los campos que desee.</span><span class="sxs-lookup"><span data-stu-id="7c649-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="7c649-195">La compatibilidad con **scaffolding** en ASP.NET MVC genera una plantilla de vista simple que muestra todos los campos del modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="7c649-196">La **técnica scaffolding** proporciona una forma rápida de empezar a trabajar en una vista fuertemente tipada: en lugar de tener que escribir la plantilla de vista manualmente, la técnica de scaffolding genera rápidamente una plantilla predeterminada y, a continuación, puede modificar el código generado.</span><span class="sxs-lookup"><span data-stu-id="7c649-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="7c649-197">Revise el código creado.</span><span class="sxs-lookup"><span data-stu-id="7c649-197">Review the code created.</span></span> <span data-ttu-id="7c649-198">La lista de campos generada formará parte de la siguiente tabla HTML que usa la **técnica scaffolding** para mostrar los datos tabulares.</span><span class="sxs-lookup"><span data-stu-id="7c649-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="7c649-199">Reemplace la **&lt;tabla&gt;** código por el código siguiente para mostrar solo los campos **género**, **intérprete**, **título del álbum**y **precio** .</span><span class="sxs-lookup"><span data-stu-id="7c649-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="7c649-200">Esto elimina las columnas **AlbumId** y **dirección URL** de carátula de álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="7c649-201">Además, cambia las columnas GenreId y ArtistId para mostrar las propiedades de la clase vinculada de **Artist.Name** y **Genre.Name**, y quita el vínculo **Details** .</span><span class="sxs-lookup"><span data-stu-id="7c649-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="7c649-202">Cambie las descripciones siguientes.</span><span class="sxs-lookup"><span data-stu-id="7c649-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7c649-203">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="7c649-204">En esta tarea, comprobará que la plantilla de vista de **Índice** **StoreManager** muestra una lista de álbumes según el diseño de los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="7c649-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="7c649-205">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-206">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-206">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-207">Cambie la dirección URL a **/StoreManager** para comprobar que se muestra una lista de álbumes, mostrando su **título**, **intérprete** y **género**.</span><span class="sxs-lookup"><span data-stu-id="7c649-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="7c649-208">![Examinar la lista de álbumes](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Examinar la lista de álbumes")</span><span class="sxs-lookup"><span data-stu-id="7c649-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="7c649-209">*Examinar la lista de álbumes*</span><span class="sxs-lookup"><span data-stu-id="7c649-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="7c649-210">Ejercicio 2: agregar una aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="7c649-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="7c649-211">La página de índice de StoreManager tiene un posible problema: las propiedades de título y nombre de intérprete pueden ser lo suficientemente largas para salir del formato de tabla.</span><span class="sxs-lookup"><span data-stu-id="7c649-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="7c649-212">En este ejercicio aprenderá a agregar una aplicación auxiliar HTML personalizada para truncar el texto.</span><span class="sxs-lookup"><span data-stu-id="7c649-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="7c649-213">En la ilustración siguiente, puede ver cómo se modifica el formato debido a la longitud del texto cuando se usa un tamaño pequeño del explorador.</span><span class="sxs-lookup"><span data-stu-id="7c649-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="7c649-214">![Examinar la lista de álbumes con texto no truncado](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Examinar la lista de álbumes con texto no truncado")</span><span class="sxs-lookup"><span data-stu-id="7c649-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="7c649-215">*Examinar la lista de álbumes con texto no truncado*</span><span class="sxs-lookup"><span data-stu-id="7c649-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="7c649-216">Tarea 1: extensión de la aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="7c649-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="7c649-217">En esta tarea, agregará un nuevo método que se **truncará** en el objeto **HTML** expuesto en las vistas de MVC de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="7c649-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="7c649-218">Para ello, implementará un método de **extensión** en la clase integrada **System. Web. Mvc. HtmlHelper** proporcionada por ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7c649-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="7c649-219">Para obtener más información acerca de **los métodos de extensión**, visite este artículo de MSDN.</span><span class="sxs-lookup"><span data-stu-id="7c649-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="7c649-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c649-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="7c649-221">Abra la carpeta **Begin** Solution ubicada en **source/Ex2-AddingAnHTMLHelper/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="7c649-222">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-223">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-224">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-225">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-226">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-227">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-228">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-229">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-230">Abra la vista de índice de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="7c649-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="7c649-231">Para ello, en el Explorador de soluciones expanda la carpeta **views** , **StoreManager** y abra el archivo **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="7c649-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="7c649-232">Agregue el siguiente código debajo de la directiva <strong>@model</strong> para definir el método de aplicación auxiliar <strong>TRUNCATE</strong> .</span><span class="sxs-lookup"><span data-stu-id="7c649-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="7c649-233">Tarea 2: truncamiento de texto en la página</span><span class="sxs-lookup"><span data-stu-id="7c649-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="7c649-234">En esta tarea, usará el método **TRUNCATE** para truncar el texto de la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="7c649-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="7c649-235">Abra la vista de índice de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="7c649-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="7c649-236">Para ello, en el Explorador de soluciones expanda la carpeta **views** , **StoreManager** y abra el archivo **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="7c649-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="7c649-237">Reemplace las líneas que muestran el **nombre del intérprete** y el **título**del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="7c649-238">Para ello, reemplace las líneas siguientes.</span><span class="sxs-lookup"><span data-stu-id="7c649-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7c649-239">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="7c649-240">En esta tarea, comprobará que la plantilla de vista de **Índice** **StoreManager** trunca el título del álbum y el nombre del intérprete.</span><span class="sxs-lookup"><span data-stu-id="7c649-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="7c649-241">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-242">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-242">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-243">Cambie la dirección URL a **/StoreManager** para comprobar que se truncan los textos largos en el **título** y la columna de **intérprete** .</span><span class="sxs-lookup"><span data-stu-id="7c649-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="7c649-244">![Nombres de los títulos y artistas truncados](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nombres de los títulos y artistas truncados")</span><span class="sxs-lookup"><span data-stu-id="7c649-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="7c649-245">*Nombres de intérpretes y títulos truncados*</span><span class="sxs-lookup"><span data-stu-id="7c649-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="7c649-246">Ejercicio 3: crear la vista de edición</span><span class="sxs-lookup"><span data-stu-id="7c649-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="7c649-247">En este ejercicio, obtendrá información sobre cómo crear un formulario para permitir que los administradores de la tienda editen un álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="7c649-248">Examinarán la dirección URL de **/StoreManager/Edit/ID** (**identificador** que es el identificador único del álbum que se va a editar), con lo que se realiza una llamada HTTP-GET al servidor.</span><span class="sxs-lookup"><span data-stu-id="7c649-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="7c649-249">El método de acción de edición del controlador recuperará el álbum adecuado de la base de datos, creará un objeto **StoreManagerViewModel** para encapsularlo (junto con una lista de artistas y géneros) y, a continuación, lo pasará a una plantilla de vista para volver a presentar la página HTML al usuario.</span><span class="sxs-lookup"><span data-stu-id="7c649-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="7c649-250">Esta página contendrá un **formulario&lt;&gt;** elemento con cuadros de texto y listas desplegables para editar las propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="7c649-251">Una vez que el usuario actualiza los valores del formulario de álbum y hace clic en el botón **Guardar** , los cambios se envían a través de una llamada http-post a **/StoreManager/Edit/ID**. Aunque la dirección URL sigue siendo la misma que en la última llamada, ASP.NET MVC identifica que esta vez es HTTP-POST y, por tanto, ejecuta un método de acción de edición diferente (uno decorado con **[HttpPost]** ).</span><span class="sxs-lookup"><span data-stu-id="7c649-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="7c649-252">Tarea 1: implementar el método de acción de edición HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="7c649-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="7c649-253">En esta tarea, implementará la versión HTTP-GET del método de acción Edit para recuperar el álbum adecuado de la base de datos, así como una lista de todos los géneros y artistas.</span><span class="sxs-lookup"><span data-stu-id="7c649-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="7c649-254">Empaquetará estos datos en el objeto **StoreManagerViewModel** definido en el último paso, que se pasará después a una plantilla de vista para representar la respuesta con.</span><span class="sxs-lookup"><span data-stu-id="7c649-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="7c649-255">Abra la carpeta **Begin** Solution ubicada en **source/Ex3-CreatingTheEditView/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="7c649-256">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-257">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-258">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-259">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-260">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-261">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-262">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-263">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-264">Abra la clase **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="7c649-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="7c649-265">Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.</span><span class="sxs-lookup"><span data-stu-id="7c649-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="7c649-266">Reemplace el método **de acción de edición HTTP-GET** con el siguiente código para recuperar el **álbum** adecuado, así como las listas **géneros** y **intérpretes** .</span><span class="sxs-lookup"><span data-stu-id="7c649-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="7c649-267">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex3 STOREMANAGERCONTROLLER HTTP-GET Edit Action*)</span><span class="sxs-lookup"><span data-stu-id="7c649-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c649-268">Está usando **System. Web. Mvc** **SelectList** para artistas y géneros en lugar de la lista **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="7c649-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="7c649-269">**SelectList** es una forma más limpia de rellenar las listas desplegables HTML y administrar elementos como la selección actual.</span><span class="sxs-lookup"><span data-stu-id="7c649-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="7c649-270">La creación de instancias de estos objetos ViewModel y versiones posteriores en la acción del controlador hará que el limpiador del escenario del formulario de edición se limpie.</span><span class="sxs-lookup"><span data-stu-id="7c649-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="7c649-271">Tarea 2: creación de la vista de edición</span><span class="sxs-lookup"><span data-stu-id="7c649-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="7c649-272">En esta tarea, creará una plantilla de vista de edición que mostrará posteriormente las propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="7c649-273">Cree la vista de edición.</span><span class="sxs-lookup"><span data-stu-id="7c649-273">Create the Edit View.</span></span> <span data-ttu-id="7c649-274">Para ello, haga clic con el botón derecho en el método **Editar** acción y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="7c649-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="7c649-275">En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Editar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="7c649-276">Active la casilla **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **Ver clase de datos** .</span><span class="sxs-lookup"><span data-stu-id="7c649-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="7c649-277">Seleccione **Editar** en la lista desplegable **plantilla scaffolding** .</span><span class="sxs-lookup"><span data-stu-id="7c649-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="7c649-278">Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="7c649-279">![Agregar una vista de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Agregar una vista de edición")</span><span class="sxs-lookup"><span data-stu-id="7c649-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="7c649-280">*Agregar una vista de edición*</span><span class="sxs-lookup"><span data-stu-id="7c649-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7c649-281">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="7c649-282">En esta tarea, probará que en la página **Editar** vista de StoreManager se muestran los valores de las propiedades del álbum pasado como parámetro.</span><span class="sxs-lookup"><span data-stu-id="7c649-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="7c649-283">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-284">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-284">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-285">Cambie la dirección URL a **/StoreManager/Edit/1** para comprobar que se muestran los valores de las propiedades del álbum que se ha pasado.</span><span class="sxs-lookup"><span data-stu-id="7c649-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="7c649-286">![Examinar la vista de edición del álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Examinar la vista de edición del álbum")</span><span class="sxs-lookup"><span data-stu-id="7c649-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="7c649-287">*Examinar la vista de edición del álbum*</span><span class="sxs-lookup"><span data-stu-id="7c649-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="7c649-288">Tarea 4: implementar listas desplegables en la plantilla del editor de álbumes</span><span class="sxs-lookup"><span data-stu-id="7c649-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="7c649-289">En esta tarea, agregará listas desplegables a la plantilla de vista creada en la última tarea, de modo que el usuario pueda seleccionar en una lista de artistas y géneros.</span><span class="sxs-lookup"><span data-stu-id="7c649-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="7c649-290">Reemplace todo el código de fieldset del **álbum** por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="7c649-291">Se ha agregado una aplicación auxiliar **HTML. DropDownList** para representar las listas desplegables para elegir artistas y géneros.</span><span class="sxs-lookup"><span data-stu-id="7c649-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="7c649-292">Los parámetros que se pasan a **HTML. DropDownList** son:</span><span class="sxs-lookup"><span data-stu-id="7c649-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="7c649-293">Nombre del campo de formulario ( **&quot;ArtistId&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="7c649-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="7c649-294">**SelectList** de los valores de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="7c649-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7c649-295">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="7c649-296">En esta tarea, probará que la página **Editar** vista de **StoreManager** muestra las listas desplegables en lugar de los campos de texto ID. de intérprete y género.</span><span class="sxs-lookup"><span data-stu-id="7c649-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="7c649-297">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-298">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-298">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-299">Cambie la dirección URL a **/StoreManager/Edit/1** para comprobar que muestra las listas desplegables en lugar de los campos de texto de ID. de intérprete y género.</span><span class="sxs-lookup"><span data-stu-id="7c649-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="7c649-300">![Examinar la vista de edición del álbum con listas desplegables](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Examinar la vista de edición del álbum con listas desplegables")</span><span class="sxs-lookup"><span data-stu-id="7c649-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="7c649-301">*Examinar la vista de edición del álbum, esta vez con listas desplegables*</span><span class="sxs-lookup"><span data-stu-id="7c649-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="7c649-302">Tarea 6: implementar el método de acción de edición HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="7c649-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="7c649-303">Ahora que la vista de edición se muestra como se esperaba, debe implementar el método de acción de edición HTTP-POST para guardar los cambios realizados en el álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="7c649-304">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c649-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7c649-305">Abra **StoreManagerController** desde la carpeta **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="7c649-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="7c649-306">Reemplace el código del método de acción **de edición http-post** con lo siguiente (tenga en cuenta que el método que se debe reemplazar es una versión sobrecargada que recibe dos parámetros):</span><span class="sxs-lookup"><span data-stu-id="7c649-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="7c649-307">(Fragmento de código-ASP.NET de las *aplicaciones auxiliares y formularios y validación-Ex3 STOREMANAGERCONTROLLER http-post*)</span><span class="sxs-lookup"><span data-stu-id="7c649-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c649-308">Este método se ejecutará cuando el usuario haga clic en el botón **Guardar** de la vista y realice una post http de los valores del formulario en el servidor para que los conserve en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="7c649-309">El elemento Decorator **[HttpPost]** indica que el método se debe usar para esos escenarios http-post.</span><span class="sxs-lookup"><span data-stu-id="7c649-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="7c649-310">El método toma un objeto **album** .</span><span class="sxs-lookup"><span data-stu-id="7c649-310">The method takes an **Album** object.</span></span> <span data-ttu-id="7c649-311">ASP.NET MVC creará automáticamente el objeto de álbum a partir del formulario de &lt;publicado&gt; valores.</span><span class="sxs-lookup"><span data-stu-id="7c649-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="7c649-312">El método realizará estos pasos:</span><span class="sxs-lookup"><span data-stu-id="7c649-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="7c649-313">Si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="7c649-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="7c649-314">Actualice la entrada del álbum en el contexto para marcarla como un objeto modificado.</span><span class="sxs-lookup"><span data-stu-id="7c649-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="7c649-315">Guarde los cambios y redirija a la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="7c649-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="7c649-316">Si el modelo no es válido, rellenará ViewBag con **GenreId** y **ArtistId**y, a continuación, devolverá la vista con el objeto album recibido para permitir que el usuario realice las actualizaciones necesarias.</span><span class="sxs-lookup"><span data-stu-id="7c649-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="7c649-317">Tarea 7: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="7c649-318">En esta tarea, probará que la página de vista de **edición de StoreManager** guarda realmente los datos de álbumes actualizados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="7c649-319">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-320">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-320">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-321">Cambie la dirección URL a **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="7c649-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="7c649-322">Cambie el título del álbum a **cargar** y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="7c649-323">Compruebe que el título del álbum ha cambiado realmente en la lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="7c649-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="7c649-324">![Actualizar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Actualizar un álbum")</span><span class="sxs-lookup"><span data-stu-id="7c649-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="7c649-325">*Actualizar un álbum*</span><span class="sxs-lookup"><span data-stu-id="7c649-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="7c649-326">Ejercicio 4: agregar una vista de creación</span><span class="sxs-lookup"><span data-stu-id="7c649-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="7c649-327">Ahora que **StoreManagerController** admite la capacidad de **edición** , en este ejercicio aprenderá a agregar una plantilla de creación de vistas para permitir que los administradores de la tienda agreguen nuevos álbumes a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="7c649-328">Como hizo con la funcionalidad de edición, implementará el escenario de creación con dos métodos independientes dentro de la clase **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="7c649-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="7c649-329">Un método de acción mostrará un formulario vacío cuando los administradores de almacenes visiten primero la dirección URL de **/StoreManager/Create** .</span><span class="sxs-lookup"><span data-stu-id="7c649-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="7c649-330">Un segundo método de acción controlará el escenario en el que el administrador de la tienda hace clic en el botón **Guardar** del formulario y envía los valores de nuevo a la dirección URL de **/StoreManager/Create** como http-post.</span><span class="sxs-lookup"><span data-stu-id="7c649-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="7c649-331">Tarea 1: implementar el método de acción HTTP-GET Create</span><span class="sxs-lookup"><span data-stu-id="7c649-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="7c649-332">En esta tarea, implementará la versión HTTP-GET del método Create Action para recuperar una lista de todos los géneros y artistas, empaquetará estos datos en un objeto **StoreManagerViewModel** , que se pasará a continuación a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="7c649-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="7c649-333">Abra la carpeta **Begin** Solution ubicada en **source/Ex4-AddingACreateView/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="7c649-334">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-335">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-336">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-337">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-338">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-339">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-340">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-341">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-342">Abra la clase **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="7c649-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="7c649-343">Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.</span><span class="sxs-lookup"><span data-stu-id="7c649-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="7c649-344">Reemplace el código del método **Create** Action por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="7c649-345">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex4 STOREMANAGERCONTROLLER HTTP-GET Create Action*)</span><span class="sxs-lookup"><span data-stu-id="7c649-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="7c649-346">Tarea 2: adición de la vista de creación</span><span class="sxs-lookup"><span data-stu-id="7c649-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="7c649-347">En esta tarea, agregará la plantilla crear vista que mostrará un nuevo formulario de álbum (vacío).</span><span class="sxs-lookup"><span data-stu-id="7c649-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="7c649-348">Haga clic con el botón derecho en el método **Create** Action y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="7c649-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="7c649-349">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="7c649-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="7c649-350">En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista sea **crear**.</span><span class="sxs-lookup"><span data-stu-id="7c649-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="7c649-351">Seleccione la opción **crear una vista fuertemente tipada** y seleccione **album (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** y **cree** a partir de la lista desplegable **plantilla scaffolding** .</span><span class="sxs-lookup"><span data-stu-id="7c649-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="7c649-352">Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="7c649-353">![Agregar una vista de creación](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")</span><span class="sxs-lookup"><span data-stu-id="7c649-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="7c649-354">*Agregar la vista de creación*</span><span class="sxs-lookup"><span data-stu-id="7c649-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="7c649-355">Actualice los campos **GenreId** y **ArtistId** para utilizar una lista desplegable, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="7c649-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="7c649-356">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="7c649-357">En esta tarea, comprobará que la página **crear** vista de StoreManager muestra un formulario de álbum vacío.</span><span class="sxs-lookup"><span data-stu-id="7c649-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="7c649-358">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-359">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-359">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-360">Cambie la dirección URL a **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="7c649-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="7c649-361">Compruebe que se muestra un formulario vacío para rellenar las nuevas propiedades del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="7c649-362">![Crear vista con un formulario vacío](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Crear vista con un formulario vacío")</span><span class="sxs-lookup"><span data-stu-id="7c649-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="7c649-363">*Crear vista con un formulario vacío*</span><span class="sxs-lookup"><span data-stu-id="7c649-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="7c649-364">Tarea 4: implementar el método de acción HTTP-POST Create</span><span class="sxs-lookup"><span data-stu-id="7c649-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="7c649-365">En esta tarea, implementará la versión HTTP-POST del método de acción Create que se invocará cuando un usuario haga clic en el botón **Guardar** .</span><span class="sxs-lookup"><span data-stu-id="7c649-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="7c649-366">El método debe guardar el nuevo álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="7c649-367">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c649-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7c649-368">Abra la clase **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="7c649-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="7c649-369">Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.</span><span class="sxs-lookup"><span data-stu-id="7c649-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="7c649-370">Reemplace **http-post crear** código de método de acción con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="7c649-371">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex4 STOREMANAGERCONTROLLER http-post Create Action*)</span><span class="sxs-lookup"><span data-stu-id="7c649-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c649-372">La acción Create es muy similar al método de acción de edición anterior, pero en lugar de establecer el objeto como modificado, se agrega al contexto.</span><span class="sxs-lookup"><span data-stu-id="7c649-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="7c649-373">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="7c649-374">En esta tarea, comprobará que la página **crear** vista de StoreManager le permite crear un nuevo álbum y, a continuación, redirige a la vista de índice StoreManager.</span><span class="sxs-lookup"><span data-stu-id="7c649-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="7c649-375">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-376">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-376">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-377">Cambie la dirección URL a **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="7c649-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="7c649-378">Rellene todos los campos de formulario con datos para un nuevo álbum, como el de la ilustración siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="7c649-379">![Crear un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Crear un álbum")</span><span class="sxs-lookup"><span data-stu-id="7c649-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="7c649-380">*Crear un álbum*</span><span class="sxs-lookup"><span data-stu-id="7c649-380">*Creating an Album*</span></span>
3. <span data-ttu-id="7c649-381">Compruebe que se le redirige a la vista de índice StoreManager que incluye el nuevo álbum que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="7c649-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="7c649-382">![Nuevo álbum creado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuevo álbum creado")</span><span class="sxs-lookup"><span data-stu-id="7c649-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="7c649-383">*Nuevo álbum creado*</span><span class="sxs-lookup"><span data-stu-id="7c649-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="7c649-384">Ejercicio 5: control de la eliminación</span><span class="sxs-lookup"><span data-stu-id="7c649-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="7c649-385">La capacidad de eliminar álbumes todavía no está implementada.</span><span class="sxs-lookup"><span data-stu-id="7c649-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="7c649-386">Este es el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="7c649-386">This is what this exercise will be about.</span></span> <span data-ttu-id="7c649-387">Al igual que antes, se implementará el escenario de eliminación con dos métodos independientes dentro de la clase **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="7c649-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="7c649-388">Un método de acción mostrará un formulario de confirmación</span><span class="sxs-lookup"><span data-stu-id="7c649-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="7c649-389">Un segundo método de acción controlará el envío del formulario</span><span class="sxs-lookup"><span data-stu-id="7c649-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="7c649-390">Tarea 1: implementar el método de acción de eliminación HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="7c649-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="7c649-391">En esta tarea, implementará la versión HTTP-GET del método de acción delete para recuperar la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="7c649-392">Abra la carpeta **Begin** Solution ubicada en **source/Ex5-HandlingDeletion/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="7c649-393">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-394">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-395">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-396">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-397">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-398">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-399">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-400">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-401">Abra la clase **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="7c649-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="7c649-402">Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.</span><span class="sxs-lookup"><span data-stu-id="7c649-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="7c649-403">La acción eliminar controlador es exactamente la misma que la acción anterior del controlador de detalles de la tienda: consulta el objeto de **álbum** desde la base de datos con el **identificador** proporcionado en la dirección URL y devuelve la **vista**adecuada.</span><span class="sxs-lookup"><span data-stu-id="7c649-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="7c649-404">Para ello, reemplace el código del método de acción HTTP-GET **Delete** por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="7c649-405">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex5 Handling Deleting HTTP-GET Delete Action*)</span><span class="sxs-lookup"><span data-stu-id="7c649-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="7c649-406">Haga clic con el botón derecho en el método de acción de **eliminación** y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="7c649-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="7c649-407">Se abrirá el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="7c649-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="7c649-408">En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Delete**.</span><span class="sxs-lookup"><span data-stu-id="7c649-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="7c649-409">Seleccione la opción **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** .</span><span class="sxs-lookup"><span data-stu-id="7c649-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="7c649-410">Seleccione **eliminar** en la lista desplegable **plantilla scaffolding** .</span><span class="sxs-lookup"><span data-stu-id="7c649-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="7c649-411">Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="7c649-412">![Agregar una vista de eliminación](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Agregar una vista de eliminación")</span><span class="sxs-lookup"><span data-stu-id="7c649-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="7c649-413">*Agregar una vista de eliminación*</span><span class="sxs-lookup"><span data-stu-id="7c649-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="7c649-414">La plantilla de eliminación muestra todos los campos del modelo.</span><span class="sxs-lookup"><span data-stu-id="7c649-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="7c649-415">Solo mostrará el título del álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-415">You will show only the album's title.</span></span> <span data-ttu-id="7c649-416">Para ello, reemplace el contenido de la vista por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="7c649-417">Tarea 2: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="7c649-418">En esta tarea, comprobará que la página de vista de **eliminación** de **StoreManager** muestra un formulario de eliminación de confirmación.</span><span class="sxs-lookup"><span data-stu-id="7c649-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="7c649-419">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-420">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-420">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-421">Cambie la dirección URL a **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="7c649-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="7c649-422">Seleccione el álbum que desea eliminar haciendo clic en **eliminar** y compruebe que la nueva vista se ha cargado.</span><span class="sxs-lookup"><span data-stu-id="7c649-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="7c649-423">![Eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="7c649-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="7c649-424">*Eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="7c649-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="7c649-425">Tarea 3: implementar el método de acción de eliminación HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="7c649-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="7c649-426">En esta tarea, implementará la versión HTTP-POST del método de acción Delete que se invocará cuando un usuario haga clic en el botón **eliminar** .</span><span class="sxs-lookup"><span data-stu-id="7c649-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="7c649-427">El método debe eliminar el álbum en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="7c649-428">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c649-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="7c649-429">Abra la clase **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="7c649-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="7c649-430">Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.</span><span class="sxs-lookup"><span data-stu-id="7c649-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="7c649-431">Reemplace el código del método de acción **de eliminación http-post** por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="7c649-432">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex5 Handling Deleting http-post Delete Action*)</span><span class="sxs-lookup"><span data-stu-id="7c649-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="7c649-433">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="7c649-434">En esta tarea, probará que la página de vista de **eliminación de StoreManager** le permite eliminar un álbum y, a continuación, redirige a la vista de índice de StoreManager.</span><span class="sxs-lookup"><span data-stu-id="7c649-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="7c649-435">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-436">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-436">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-437">Cambie la dirección URL a **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="7c649-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="7c649-438">Seleccione el álbum que desea eliminar haciendo clic en **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="7c649-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="7c649-439">Para confirmar la eliminación, haga clic en el botón **eliminar** :</span><span class="sxs-lookup"><span data-stu-id="7c649-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="7c649-440">![Eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Eliminar un álbum")</span><span class="sxs-lookup"><span data-stu-id="7c649-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="7c649-441">*Eliminar un álbum*</span><span class="sxs-lookup"><span data-stu-id="7c649-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="7c649-442">Compruebe que el álbum se ha eliminado porque no aparece en la página de **Índice** .</span><span class="sxs-lookup"><span data-stu-id="7c649-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="7c649-443">Ejercicio 6: agregar validación</span><span class="sxs-lookup"><span data-stu-id="7c649-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="7c649-444">Actualmente, los formularios de creación y edición que tiene en su lugar no realizan ningún tipo de validación.</span><span class="sxs-lookup"><span data-stu-id="7c649-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="7c649-445">Si el usuario deja un campo obligatorio en blanco o escribe letras en el campo Precio, el primer error que obtendrá será de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="7c649-446">Puede Agregar la validación a la aplicación agregando anotaciones de datos a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="7c649-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="7c649-447">Las anotaciones de datos permiten describir las reglas que desea aplicar a las propiedades del modelo y ASP.NET MVC se encargará de aplicar y mostrar el mensaje adecuado a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="7c649-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="7c649-448">Tarea 1: agregar anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="7c649-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="7c649-449">En esta tarea, agregará anotaciones de datos al modelo de álbum que hará que la página crear y editar muestre los mensajes de validación cuando corresponda.</span><span class="sxs-lookup"><span data-stu-id="7c649-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="7c649-450">En el caso de una clase de modelo simple, agregar una anotación de datos se controla simplemente agregando una instrucción **using** para **System. ComponentModel. DataAnnotations**y colocando un atributo **[required]** en las propiedades adecuadas.</span><span class="sxs-lookup"><span data-stu-id="7c649-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="7c649-451">En el ejemplo siguiente, la propiedad de **nombre** se convierte en un campo obligatorio en la vista.</span><span class="sxs-lookup"><span data-stu-id="7c649-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="7c649-452">Este es un poco más complejo en casos como esta aplicación donde se genera el Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="7c649-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="7c649-453">Si agregó anotaciones de datos directamente a las clases del modelo, se sobrescribirán si actualiza el modelo desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="7c649-454">En su lugar, puede hacer uso de las clases parciales de metadatos que existirán para contener las anotaciones y se asocian a las clases de modelo mediante el atributo **[MetadataType]** .</span><span class="sxs-lookup"><span data-stu-id="7c649-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="7c649-455">Abra la carpeta **Begin** Solution ubicada en **source/EX6-AddingValidation/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="7c649-456">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-457">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-458">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-459">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-460">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-461">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-462">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-463">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-464">Abra **Album.CS** desde la carpeta **Models** .</span><span class="sxs-lookup"><span data-stu-id="7c649-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="7c649-465">Reemplace el contenido de **Album.CS** por el código resaltado para que tenga el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c649-466">La línea **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica que las cadenas vacías del modelo no se convertirán en NULL cuando se actualice el campo de datos en el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="7c649-467">Esta configuración evitará una excepción cuando el Entity Framework asigna valores NULL al modelo antes de que la anotación de datos valide los campos.</span><span class="sxs-lookup"><span data-stu-id="7c649-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="7c649-468">(Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-EX6 album Metadata clase parcial*)</span><span class="sxs-lookup"><span data-stu-id="7c649-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="7c649-469">Esta clase parcial de **álbum** tiene un atributo **MetadataType** que apunta a la clase **AlbumMetaData** para las anotaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="7c649-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="7c649-470">Estos son algunos de los atributos de anotación de datos que se usan para anotar el modelo de álbum:</span><span class="sxs-lookup"><span data-stu-id="7c649-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="7c649-471">Requerido: indica que la propiedad es un campo obligatorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="7c649-472">DisplayName: define el texto que se va a usar en los campos de formulario y en los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="7c649-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="7c649-473">DisplayFormat: especifica cómo se muestran los campos de datos y cómo se les da formato.</span><span class="sxs-lookup"><span data-stu-id="7c649-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="7c649-474">StringLength: define una longitud máxima para un campo de cadena.</span><span class="sxs-lookup"><span data-stu-id="7c649-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="7c649-475">Range: proporciona un valor máximo y mínimo para un campo numérico.</span><span class="sxs-lookup"><span data-stu-id="7c649-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="7c649-476">ScaffoldColumn: permite ocultar campos de los formularios del editor</span><span class="sxs-lookup"><span data-stu-id="7c649-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="7c649-477">Tarea 2: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7c649-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="7c649-478">En esta tarea, probará que los campos crear y editar páginas validan, con los nombres para mostrar elegidos en la última tarea.</span><span class="sxs-lookup"><span data-stu-id="7c649-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="7c649-479">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="7c649-480">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-480">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-481">Cambie la dirección URL a **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="7c649-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="7c649-482">Compruebe que los nombres para mostrar coinciden con los de la clase parcial (como la **dirección URL** de carátula de álbum en lugar de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="7c649-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="7c649-483">Haga clic en **crear**sin rellenar el formulario.</span><span class="sxs-lookup"><span data-stu-id="7c649-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="7c649-484">Compruebe que obtiene los mensajes de validación correspondientes.</span><span class="sxs-lookup"><span data-stu-id="7c649-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="7c649-485">![Campos validados en la página crear](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campos validados en la página crear")</span><span class="sxs-lookup"><span data-stu-id="7c649-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="7c649-486">*Campos validados en la página crear*</span><span class="sxs-lookup"><span data-stu-id="7c649-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="7c649-487">Puede comprobar que ocurre lo mismo con la página **Editar** .</span><span class="sxs-lookup"><span data-stu-id="7c649-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="7c649-488">Cambie la dirección URL a **/StoreManager/Edit/1** y compruebe que los nombres para mostrar coinciden con los de la clase parcial (como la **dirección URL** de la carátula de álbum en lugar de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="7c649-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="7c649-489">Vacíe los campos **título** y **precio** y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="7c649-490">Compruebe que obtiene los mensajes de validación correspondientes.</span><span class="sxs-lookup"><span data-stu-id="7c649-490">Verify that you get the corresponding validation messages.</span></span>

    ![Campos validados en la página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="7c649-492">*Campos validados en la página Editar*</span><span class="sxs-lookup"><span data-stu-id="7c649-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="7c649-493">Ejercicio 7: usar jQuery discreto en el lado cliente</span><span class="sxs-lookup"><span data-stu-id="7c649-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="7c649-494">En este ejercicio, obtendrá información sobre cómo habilitar la validación de jQuery discreta de MVC 4 en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7c649-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="7c649-495">La jQuery discreta usa JavaScript de prefijo de AJAX de datos para invocar métodos de acción en el servidor en lugar de emitir indiscretamente scripts de cliente insertados.</span><span class="sxs-lookup"><span data-stu-id="7c649-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="7c649-496">Tarea 1: ejecutar la aplicación antes de habilitar jQuery discreto</span><span class="sxs-lookup"><span data-stu-id="7c649-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="7c649-497">En esta tarea, ejecutará la aplicación antes de incluir jQuery para comparar ambos modelos de validación.</span><span class="sxs-lookup"><span data-stu-id="7c649-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="7c649-498">Abra la carpeta **Begin** Solution ubicada en **source/EX7-UnobtrusivejQueryValidation/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="7c649-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="7c649-499">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="7c649-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="7c649-500">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="7c649-501">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="7c649-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="7c649-502">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="7c649-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="7c649-503">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="7c649-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c649-504">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="7c649-505">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="7c649-506">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="7c649-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="7c649-507">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="7c649-508">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-508">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-509">Vaya a **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para comprobar que obtiene los mensajes de validación:</span><span class="sxs-lookup"><span data-stu-id="7c649-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="7c649-510">![Validación de cliente deshabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validación de cliente deshabilitada")</span><span class="sxs-lookup"><span data-stu-id="7c649-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="7c649-511">*Validación de cliente deshabilitada*</span><span class="sxs-lookup"><span data-stu-id="7c649-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="7c649-512">En el explorador, abra el código fuente HTML:</span><span class="sxs-lookup"><span data-stu-id="7c649-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="7c649-513">Tarea 2: habilitar la validación de cliente discreta</span><span class="sxs-lookup"><span data-stu-id="7c649-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="7c649-514">En esta tarea, habilitará la **validación de cliente discreta** de jQuery desde el archivo **Web. config** , que está establecido de forma predeterminada en false en todos los proyectos nuevos de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7c649-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="7c649-515">Además, agregará las referencias de script necesarias para hacer que el trabajo de validación de clientes discretos de jQuery no funcione.</span><span class="sxs-lookup"><span data-stu-id="7c649-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="7c649-516">Abra el archivo **Web. config** en la raíz del proyecto y asegúrese de que los valores de las claves **ClientValidationEnabled** y **UnobtrusiveJavaScriptEnabled** están establecidos en **true**.</span><span class="sxs-lookup"><span data-stu-id="7c649-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="7c649-517">También puede habilitar la validación del cliente por código en Global.asax.cs para obtener los mismos resultados:</span><span class="sxs-lookup"><span data-stu-id="7c649-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="7c649-518">**HtmlHelper. ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="7c649-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="7c649-519">Además, puede asignar el atributo ClientValidationEnabled en cualquier controlador para que tenga un comportamiento personalizado.</span><span class="sxs-lookup"><span data-stu-id="7c649-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="7c649-520">Abra **Create. cshtml** en **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="7c649-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="7c649-521">Asegúrese de que se hace referencia a los siguientes archivos de script, **jQuery. Validate** y **jQuery. Validate. discreta**, en la vista a través del conjunto de &quot; **~/bundles/jqueryval**&quot;.</span><span class="sxs-lookup"><span data-stu-id="7c649-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="7c649-522">Todas estas bibliotecas de jQuery se incluyen en los nuevos proyectos de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7c649-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="7c649-523">Puede encontrar más bibliotecas en la carpeta **/scripts** del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7c649-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="7c649-524">Para que estas bibliotecas de validación funcionen, debe agregar una referencia a la biblioteca de marcos de jQuery.</span><span class="sxs-lookup"><span data-stu-id="7c649-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="7c649-525">Puesto que esta referencia ya se ha agregado en el archivo **\_layout. cshtml** , no es necesario agregarla en esta vista concreta.</span><span class="sxs-lookup"><span data-stu-id="7c649-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="7c649-526">Tarea 3: ejecución de la aplicación con la validación de jQuery discreta</span><span class="sxs-lookup"><span data-stu-id="7c649-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="7c649-527">En esta tarea, comprobará que la plantilla de creación de vista de **StoreManager** realiza la validación del lado cliente mediante bibliotecas de jQuery cuando el usuario crea un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="7c649-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="7c649-528">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7c649-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7c649-529">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="7c649-529">The project starts in the Home page.</span></span> <span data-ttu-id="7c649-530">Vaya a **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para comprobar que obtiene los mensajes de validación:</span><span class="sxs-lookup"><span data-stu-id="7c649-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="7c649-531">![Validación de cliente con jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validación de cliente con jQuery habilitado")</span><span class="sxs-lookup"><span data-stu-id="7c649-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="7c649-532">*Validación de cliente con jQuery habilitado*</span><span class="sxs-lookup"><span data-stu-id="7c649-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="7c649-533">En el explorador, abra el código fuente para crear vista:</span><span class="sxs-lookup"><span data-stu-id="7c649-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="7c649-534">Para cada regla de validación de cliente, jQuery discreto agrega un atributo con Data-Val-*rulename*=&quot;&quot;de *mensaje* .</span><span class="sxs-lookup"><span data-stu-id="7c649-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="7c649-535">A continuación se muestra una lista de etiquetas que no molestan las inserciones de jQuery en el campo de entrada HTML para realizar la validación del cliente:</span><span class="sxs-lookup"><span data-stu-id="7c649-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="7c649-536">Datos: Val</span><span class="sxs-lookup"><span data-stu-id="7c649-536">Data-val</span></span>
   > - <span data-ttu-id="7c649-537">Data-Val-número</span><span class="sxs-lookup"><span data-stu-id="7c649-537">Data-val-number</span></span>
   > - <span data-ttu-id="7c649-538">Data-Val-Range</span><span class="sxs-lookup"><span data-stu-id="7c649-538">Data-val-range</span></span>
   > - <span data-ttu-id="7c649-539">Data-Val-Range-min/Data-Val-Range-Max</span><span class="sxs-lookup"><span data-stu-id="7c649-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="7c649-540">Data-Val-required</span><span class="sxs-lookup"><span data-stu-id="7c649-540">Data-val-required</span></span>
   > - <span data-ttu-id="7c649-541">Data-Val-longitud</span><span class="sxs-lookup"><span data-stu-id="7c649-541">Data-val-length</span></span>
   > - <span data-ttu-id="7c649-542">Data-Val-Length-Max/Data-Val-length-min</span><span class="sxs-lookup"><span data-stu-id="7c649-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="7c649-543">Todos los valores de datos se rellenan con la anotación del modelo de **datos**.</span><span class="sxs-lookup"><span data-stu-id="7c649-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="7c649-544">Después, toda la lógica que funciona en el lado servidor se puede ejecutar en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="7c649-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="7c649-545">Por ejemplo, el atributo Price tiene la siguiente anotación de datos en el modelo:</span><span class="sxs-lookup"><span data-stu-id="7c649-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="7c649-546">Después de usar jQuery discreto, el código generado es:</span><span class="sxs-lookup"><span data-stu-id="7c649-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7c649-547">Resumen</span><span class="sxs-lookup"><span data-stu-id="7c649-547">Summary</span></span>

<span data-ttu-id="7c649-548">Al completar este laboratorio práctico, ha aprendido a permitir que los usuarios cambien los datos almacenados en la base de datos por el uso de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7c649-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="7c649-549">Acciones de controlador como index, Create, Edit, Delete</span><span class="sxs-lookup"><span data-stu-id="7c649-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="7c649-550">Característica de scaffolding de ASP.NET MVC para mostrar propiedades en una tabla HTML</span><span class="sxs-lookup"><span data-stu-id="7c649-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="7c649-551">Aplicaciones auxiliares HTML personalizadas para mejorar la experiencia del usuario</span><span class="sxs-lookup"><span data-stu-id="7c649-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="7c649-552">Métodos de acción que reaccionan a llamadas HTTP-GET o HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="7c649-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="7c649-553">Una plantilla de editor compartida para plantillas de vista similares, como crear y editar</span><span class="sxs-lookup"><span data-stu-id="7c649-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="7c649-554">Elementos de formulario como listas desplegables</span><span class="sxs-lookup"><span data-stu-id="7c649-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="7c649-555">Anotaciones de datos para la validación del modelo</span><span class="sxs-lookup"><span data-stu-id="7c649-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="7c649-556">Validación del lado cliente con la biblioteca discreta de jQuery</span><span class="sxs-lookup"><span data-stu-id="7c649-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7c649-557">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="7c649-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7c649-558">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="7c649-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7c649-559">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="7c649-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7c649-560">Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7c649-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7c649-561">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7c649-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7c649-562">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="7c649-562">Click on **Install Now**.</span></span> <span data-ttu-id="7c649-563">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="7c649-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7c649-564">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="7c649-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7c649-565">![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7c649-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7c649-566">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7c649-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7c649-567">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="7c649-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="7c649-569">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="7c649-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7c649-570">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="7c649-570">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="7c649-572">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="7c649-572">*Installation progress*</span></span>
6. <span data-ttu-id="7c649-573">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="7c649-573">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="7c649-575">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="7c649-575">*Installation completed*</span></span>
7. <span data-ttu-id="7c649-576">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="7c649-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7c649-577">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="7c649-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="7c649-579">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="7c649-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="7c649-580">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="7c649-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="7c649-581">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="7c649-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7c649-582">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="7c649-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7c649-583">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="7c649-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7c649-584">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="7c649-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7c649-585">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="7c649-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7c649-586">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="7c649-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7c649-587">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="7c649-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7c649-588">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="7c649-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7c649-589">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="7c649-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7c649-590">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="7c649-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7c649-591">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="7c649-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7c649-592">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="7c649-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="7c649-593">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="7c649-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7c649-594">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="7c649-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7c649-595">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="7c649-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7c649-596">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="7c649-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7c649-597">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="7c649-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7c649-598">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="7c649-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7c649-599">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="7c649-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7c649-600">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="7c649-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7c649-601">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="7c649-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7c649-602">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="7c649-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7c649-603">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="7c649-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7c649-604">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="7c649-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
