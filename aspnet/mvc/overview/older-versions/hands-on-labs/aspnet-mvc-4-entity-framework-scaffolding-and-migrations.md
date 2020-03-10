---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework scaffolding y migraciones | Microsoft Docs
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484645"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="52f5b-103">Scaffolding y migraciones de Entity Framework de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="52f5b-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="52f5b-104">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="52f5b-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="52f5b-105">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="52f5b-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="52f5b-106">Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y validación&quot; laboratorio práctico, debe tener en cuenta que muchas de las lógicas para crear, actualizar, enumerar y quitar cualquier entidad de datos se repiten entre la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="52f5b-107">No se debe mencionar que, si el modelo tiene varias clases que manipular, es probable que se dedique un tiempo considerable a escribir los métodos de acción POST y GET para cada operación de entidad, así como a cada una de las vistas.</span><span class="sxs-lookup"><span data-stu-id="52f5b-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="52f5b-108">En este laboratorio aprenderá a usar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea base del CRUD de la aplicación (creación, lectura, actualización y eliminación).</span><span class="sxs-lookup"><span data-stu-id="52f5b-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="52f5b-109">A partir de una clase de modelo simple y, sin escribir una sola línea de código, creará un controlador que contendrá todas las operaciones CRUD, así como todas las vistas necesarias.</span><span class="sxs-lookup"><span data-stu-id="52f5b-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="52f5b-110">Después de compilar y ejecutar la solución simple, tendrá la base de datos de la aplicación generada, junto con la lógica y las vistas de MVC para la manipulación de datos.</span><span class="sxs-lookup"><span data-stu-id="52f5b-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="52f5b-111">Además, aprenderá lo fácil que es usar las migraciones de Entity Framework para realizar actualizaciones de modelos en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="52f5b-112">Entity Framework migraciones le permitirán modificar la base de datos una vez que el modelo haya cambiado con pasos sencillos.</span><span class="sxs-lookup"><span data-stu-id="52f5b-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="52f5b-113">Teniendo en cuenta todos estos elementos, podrá compilar y mantener las aplicaciones Web de forma más eficaz, aprovechando las características más recientes de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="52f5b-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="52f5b-114">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="52f5b-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="52f5b-115">El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Entity Framework scaffolding y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="52f5b-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="52f5b-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="52f5b-116">Objectives</span></span>

<span data-ttu-id="52f5b-117">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="52f5b-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="52f5b-118">Use el scaffolding de ASP.NET para las operaciones CRUD en los controladores.</span><span class="sxs-lookup"><span data-stu-id="52f5b-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="52f5b-119">Cambiar el modelo de base de datos mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52f5b-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="52f5b-120">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="52f5b-120">Prerequisites</span></span>

<span data-ttu-id="52f5b-121">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="52f5b-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="52f5b-122">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="52f5b-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="52f5b-123">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="52f5b-123">Setup</span></span>

<span data-ttu-id="52f5b-124">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="52f5b-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="52f5b-125">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52f5b-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="52f5b-126">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="52f5b-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="52f5b-127">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice B: usar fragmentos de código](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="52f5b-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="52f5b-128">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="52f5b-128">Exercises</span></span>

<span data-ttu-id="52f5b-129">El siguiente ejercicio constituye este laboratorio práctico:</span><span class="sxs-lookup"><span data-stu-id="52f5b-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="52f5b-130">Uso de la técnica scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="52f5b-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="52f5b-131">Este ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="52f5b-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="52f5b-132">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="52f5b-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>

<span data-ttu-id="52f5b-133">Tiempo estimado para completar este laboratorio: **30 minutos**</span><span class="sxs-lookup"><span data-stu-id="52f5b-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="52f5b-134">Ejercicio 1: uso de la técnica scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="52f5b-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="52f5b-135">La técnica scaffolding de ASP.NET MVC proporciona una forma rápida de generar las operaciones CRUD de forma estandarizada, creando la lógica necesaria que permite a la aplicación interactuar con el nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="52f5b-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="52f5b-136">En este ejercicio, obtendrá información sobre cómo usar la técnica de scaffolding de ASP.NET MVC 4 con Code First para crear los métodos CRUD.</span><span class="sxs-lookup"><span data-stu-id="52f5b-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="52f5b-137">A continuación, obtendrá información sobre cómo actualizar el modelo aplicando los cambios en la base de datos mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52f5b-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="52f5b-138">Tarea 1: creación de un nuevo proyecto de ASP.NET MVC 4 con scaffolding</span><span class="sxs-lookup"><span data-stu-id="52f5b-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="52f5b-139">Si aún no está abierto, inicie **Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="52f5b-140">Seleccionar **archivo | Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-140">Select **File | New Project**.</span></span> <span data-ttu-id="52f5b-141">En el cuadro de diálogo nuevo proyecto, en el **objeto visual C# |** En la sección Web, seleccione **aplicación web MVC 4 ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="52f5b-142">Asigne al proyecto el nombre **MVC4andEFMigrations** y establezca la ubicación en la carpeta **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="52f5b-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="52f5b-143">Establezca el **nombre** de la solución en **Inicio** y asegúrese de que la opción **Crear directorio para la solución** está activada.</span><span class="sxs-lookup"><span data-stu-id="52f5b-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="52f5b-144">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-144">Click **OK**.</span></span>

    <span data-ttu-id="52f5b-145">![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="52f5b-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="52f5b-146">*Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="52f5b-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="52f5b-147">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla **aplicación de Internet** y asegúrese de que **Razor** es el **motor de vista**seleccionado.</span><span class="sxs-lookup"><span data-stu-id="52f5b-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="52f5b-148">Haga clic en **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="52f5b-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="52f5b-149">![Nueva aplicación de Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nueva aplicación de Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="52f5b-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="52f5b-150">*Nueva aplicación de Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="52f5b-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="52f5b-151">En el Explorador de soluciones, haga clic con el botón derecho en **modelos** y seleccione **Agregar | Clase** para crear una clase simple person (poco).</span><span class="sxs-lookup"><span data-stu-id="52f5b-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="52f5b-152">Asígnele el nombre **Person** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="52f5b-153">Abra la clase Person e inserte las siguientes propiedades.</span><span class="sxs-lookup"><span data-stu-id="52f5b-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="52f5b-154">(Fragmentos de código- *ASP.NET MVC 4 y migraciones Entity Framework-propiedades de la persona EX1*)</span><span class="sxs-lookup"><span data-stu-id="52f5b-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="52f5b-155">Haga clic en **generar | Compile la solución** para guardar los cambios y compilar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="52f5b-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="52f5b-156">![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilación de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="52f5b-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="52f5b-157">*Compilación de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="52f5b-157">*Building the Application*</span></span>
7. <span data-ttu-id="52f5b-158">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Controllers y seleccione **Agregar | Controlador**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="52f5b-159">Asigne al controlador el nombre *PersonController* y complete las **Opciones de scaffolding** con los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="52f5b-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="52f5b-160">En la lista desplegable **plantilla** , seleccione el **controlador de MVC con acciones y vistas de lectura/escritura con Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="52f5b-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="52f5b-161">En la lista desplegable **clase de modelo** , seleccione la clase **Person** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="52f5b-162">En la lista **clase de contexto de datos** , seleccione **&lt;nuevo contexto de datos...&gt;** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="52f5b-163">Elija cualquier nombre y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="52f5b-164">En la lista desplegable **vistas** , asegúrese de que **Razor** está seleccionado.</span><span class="sxs-lookup"><span data-stu-id="52f5b-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="52f5b-165">![Adición del controlador person con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adición del controlador person con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="52f5b-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="52f5b-166">*Adición del controlador person con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="52f5b-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="52f5b-167">Haga clic en **Agregar** para crear el nuevo controlador para la persona con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="52f5b-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="52f5b-168">Ahora ha generado las acciones del controlador, así como las vistas.</span><span class="sxs-lookup"><span data-stu-id="52f5b-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="52f5b-169">![Después de crear el controlador person con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Después de crear el controlador person con scaffolding")</span><span class="sxs-lookup"><span data-stu-id="52f5b-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="52f5b-170">*Después de crear el controlador person con scaffolding*</span><span class="sxs-lookup"><span data-stu-id="52f5b-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="52f5b-171">Abra la clase **PersonController** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-171">Open **PersonController** class.</span></span> <span data-ttu-id="52f5b-172">Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="52f5b-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="52f5b-173">![Dentro del controlador Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Dentro del controlador Person")</span><span class="sxs-lookup"><span data-stu-id="52f5b-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="52f5b-174">*Dentro del controlador Person*</span><span class="sxs-lookup"><span data-stu-id="52f5b-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="52f5b-175">Tarea 2: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="52f5b-175">Task 2- Running the application</span></span>

<span data-ttu-id="52f5b-176">En este momento, la base de datos aún no se ha creado.</span><span class="sxs-lookup"><span data-stu-id="52f5b-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="52f5b-177">En esta tarea, ejecutará la aplicación por primera vez y probará las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="52f5b-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="52f5b-178">La base de datos se creará sobre la marcha con Code First.</span><span class="sxs-lookup"><span data-stu-id="52f5b-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="52f5b-179">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="52f5b-180">En el explorador, agregue **/Person** a la dirección URL para abrir la página person.</span><span class="sxs-lookup"><span data-stu-id="52f5b-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="52f5b-181">![Primera ejecución de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Primera ejecución de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="52f5b-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="52f5b-182">*Aplicación: primera ejecución*</span><span class="sxs-lookup"><span data-stu-id="52f5b-182">*Application: first run*</span></span>
3. <span data-ttu-id="52f5b-183">Ahora explorará las páginas person y probará las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="52f5b-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="52f5b-184">Haga clic en **crear nuevo** para agregar una nueva persona.</span><span class="sxs-lookup"><span data-stu-id="52f5b-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="52f5b-185">Escriba un nombre y un apellido y haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="52f5b-186">![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Agregar una nueva persona")</span><span class="sxs-lookup"><span data-stu-id="52f5b-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="52f5b-187">*Agregar una nueva persona*</span><span class="sxs-lookup"><span data-stu-id="52f5b-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="52f5b-188">En la lista de personas, puede eliminar, editar o agregar elementos.</span><span class="sxs-lookup"><span data-stu-id="52f5b-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="52f5b-189">![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")</span><span class="sxs-lookup"><span data-stu-id="52f5b-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="52f5b-190">*Lista de personas*</span><span class="sxs-lookup"><span data-stu-id="52f5b-190">*Person list*</span></span>
    3. <span data-ttu-id="52f5b-191">Haga clic en **detalles** para abrir los detalles de la persona.</span><span class="sxs-lookup"><span data-stu-id="52f5b-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="52f5b-192">![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Detalles de la persona")</span><span class="sxs-lookup"><span data-stu-id="52f5b-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="52f5b-193">*Detalles de la persona*</span><span class="sxs-lookup"><span data-stu-id="52f5b-193">*Person's details*</span></span>
4. <span data-ttu-id="52f5b-194">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52f5b-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="52f5b-195">Tenga en cuenta que ha creado el CRUD completo para la entidad Person en toda la aplicación: desde el modelo hasta las vistas, sin tener que escribir una sola línea de código.</span><span class="sxs-lookup"><span data-stu-id="52f5b-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="52f5b-196">Tarea 3: actualización de la base de datos mediante migraciones de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="52f5b-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="52f5b-197">En esta tarea, actualizará la base de datos mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52f5b-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="52f5b-198">Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52f5b-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="52f5b-199">Abra la consola del administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="52f5b-199">Open the Package Manager Console.</span></span> <span data-ttu-id="52f5b-200">Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="52f5b-201">En la Consola del Administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="52f5b-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="52f5b-202">PMC</span><span class="sxs-lookup"><span data-stu-id="52f5b-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="52f5b-203">![Habilitación de migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Habilitación de migraciones")</span><span class="sxs-lookup"><span data-stu-id="52f5b-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="52f5b-204">*Habilitación de migraciones*</span><span class="sxs-lookup"><span data-stu-id="52f5b-204">*Enabling migrations*</span></span>

    <span data-ttu-id="52f5b-205">El comando enable-Migration crea la carpeta **Migrations** , que contiene un script para inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="52f5b-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="52f5b-206">![Carpeta migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Carpeta migraciones")</span><span class="sxs-lookup"><span data-stu-id="52f5b-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="52f5b-207">*Carpeta migraciones*</span><span class="sxs-lookup"><span data-stu-id="52f5b-207">*Migrations folder*</span></span>
3. <span data-ttu-id="52f5b-208">Abra el archivo **Configuration.CS** en la carpeta Migrations.</span><span class="sxs-lookup"><span data-stu-id="52f5b-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="52f5b-209">Busque el constructor de clase y cambie el valor de **AutomaticMigrationsEnabled** a *true*.</span><span class="sxs-lookup"><span data-stu-id="52f5b-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="52f5b-210">Abra la clase person y agregue un atributo para el segundo nombre de la persona.</span><span class="sxs-lookup"><span data-stu-id="52f5b-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="52f5b-211">Con este nuevo atributo, está cambiando el modelo.</span><span class="sxs-lookup"><span data-stu-id="52f5b-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="52f5b-212">Seleccionar **compilación | Compilar solución** en el menú para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="52f5b-213">![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilación de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="52f5b-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="52f5b-214">*Compilar la aplicación*</span><span class="sxs-lookup"><span data-stu-id="52f5b-214">*Building the application*</span></span>
6. <span data-ttu-id="52f5b-215">En la Consola del Administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="52f5b-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="52f5b-216">PMC</span><span class="sxs-lookup"><span data-stu-id="52f5b-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="52f5b-217">Este comando buscará cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos según corresponda.</span><span class="sxs-lookup"><span data-stu-id="52f5b-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="52f5b-218">![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Agregar un segundo nombre")</span><span class="sxs-lookup"><span data-stu-id="52f5b-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="52f5b-219">*Agregar un segundo nombre*</span><span class="sxs-lookup"><span data-stu-id="52f5b-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="52f5b-220">Opta Puede ejecutar el siguiente comando para generar un script SQL con la actualización diferencial.</span><span class="sxs-lookup"><span data-stu-id="52f5b-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="52f5b-221">Esto le permitirá actualizar la base de datos manualmente (en este caso, no es necesario) o aplicar los cambios en otras bases de datos:</span><span class="sxs-lookup"><span data-stu-id="52f5b-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="52f5b-222">PMC</span><span class="sxs-lookup"><span data-stu-id="52f5b-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="52f5b-223">![Generar un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generación de un script SQL")</span><span class="sxs-lookup"><span data-stu-id="52f5b-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="52f5b-224">*Generar un script SQL*</span><span class="sxs-lookup"><span data-stu-id="52f5b-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="52f5b-225">![Actualización de scripts SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Actualización de scripts SQL")</span><span class="sxs-lookup"><span data-stu-id="52f5b-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="52f5b-226">*Actualización de scripts SQL*</span><span class="sxs-lookup"><span data-stu-id="52f5b-226">*SQL Script update*</span></span>
8. <span data-ttu-id="52f5b-227">En la consola del administrador de paquetes, escriba el comando siguiente para actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="52f5b-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="52f5b-228">PMC</span><span class="sxs-lookup"><span data-stu-id="52f5b-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="52f5b-229">![Actualizar la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Actualizar la base de datos")</span><span class="sxs-lookup"><span data-stu-id="52f5b-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="52f5b-230">*Actualizar la base de datos*</span><span class="sxs-lookup"><span data-stu-id="52f5b-230">*Updating the Database*</span></span>

    <span data-ttu-id="52f5b-231">Esto agregará la columna **MiddleName** de la tabla **People** para que coincida con la definición actual de la clase **Person** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="52f5b-232">Una vez actualizada la base de datos, haga clic con el botón derecho en la carpeta Controller y seleccione **Agregar | Controlador** para volver a agregar el controlador de persona (complete con los mismos valores).</span><span class="sxs-lookup"><span data-stu-id="52f5b-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="52f5b-233">Esto actualizará los métodos y vistas existentes agregando el nuevo atributo.</span><span class="sxs-lookup"><span data-stu-id="52f5b-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="52f5b-234">![Agregar una actualización de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Agregar una actualización de controlador")</span><span class="sxs-lookup"><span data-stu-id="52f5b-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="52f5b-235">*Actualización del controlador*</span><span class="sxs-lookup"><span data-stu-id="52f5b-235">*Updating the controller*</span></span>
10. <span data-ttu-id="52f5b-236">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-236">Click **Add**.</span></span> <span data-ttu-id="52f5b-237">A continuación, seleccione los valores **sobrescribir PersonController.CS** y **sobrescribir vistas asociadas** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="52f5b-239">*Actualización del controlador*</span><span class="sxs-lookup"><span data-stu-id="52f5b-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="52f5b-240">Task4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="52f5b-240">Task4- Running the application</span></span>

1. <span data-ttu-id="52f5b-241">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="52f5b-242">Abra **/Person**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-242">Open **/Person**.</span></span> <span data-ttu-id="52f5b-243">Tenga en cuenta que se han conservado los datos, mientras que se ha agregado la columna Middle Name.</span><span class="sxs-lookup"><span data-stu-id="52f5b-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="52f5b-244">![Segundo nombre agregado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Segundo nombre agregado")</span><span class="sxs-lookup"><span data-stu-id="52f5b-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="52f5b-245">*Segundo nombre agregado*</span><span class="sxs-lookup"><span data-stu-id="52f5b-245">*Middle Name added*</span></span>
3. <span data-ttu-id="52f5b-246">Si hace clic en **Editar**, podrá agregar un segundo nombre a la persona actual.</span><span class="sxs-lookup"><span data-stu-id="52f5b-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="52f5b-247">![Edición de segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edición de segundo nombre")</span><span class="sxs-lookup"><span data-stu-id="52f5b-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="52f5b-248">Resumen</span><span class="sxs-lookup"><span data-stu-id="52f5b-248">Summary</span></span>

<span data-ttu-id="52f5b-249">En este laboratorio práctico, ha aprendido pasos sencillos para crear operaciones CRUD con scaffolding de ASP.NET MVC 4 mediante cualquier clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="52f5b-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="52f5b-250">Después, ha aprendido a realizar una actualización de un extremo a otro en la aplicación: desde la base de datos hasta las vistas, mediante migraciones de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52f5b-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="52f5b-251">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="52f5b-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="52f5b-252">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="52f5b-253">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="52f5b-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="52f5b-254">Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="52f5b-254">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="52f5b-255">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="52f5b-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="52f5b-256">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-256">Click on **Install Now**.</span></span> <span data-ttu-id="52f5b-257">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="52f5b-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="52f5b-258">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="52f5b-259">![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="52f5b-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="52f5b-260">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="52f5b-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="52f5b-261">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="52f5b-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="52f5b-263">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="52f5b-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="52f5b-264">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="52f5b-264">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="52f5b-266">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="52f5b-266">*Installation progress*</span></span>
6. <span data-ttu-id="52f5b-267">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-267">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="52f5b-269">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="52f5b-269">*Installation completed*</span></span>
7. <span data-ttu-id="52f5b-270">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="52f5b-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="52f5b-271">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="52f5b-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="52f5b-273">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="52f5b-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="52f5b-274">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="52f5b-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="52f5b-275">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="52f5b-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="52f5b-276">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="52f5b-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="52f5b-277">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="52f5b-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="52f5b-278">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="52f5b-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="52f5b-279">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="52f5b-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="52f5b-280">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="52f5b-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="52f5b-281">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="52f5b-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="52f5b-282">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="52f5b-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="52f5b-283">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="52f5b-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="52f5b-284">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="52f5b-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="52f5b-285">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="52f5b-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="52f5b-286">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="52f5b-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="52f5b-287">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="52f5b-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="52f5b-288">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="52f5b-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="52f5b-289">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="52f5b-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="52f5b-290">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="52f5b-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="52f5b-291">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="52f5b-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="52f5b-292">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="52f5b-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="52f5b-293">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="52f5b-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="52f5b-294">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="52f5b-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="52f5b-295">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="52f5b-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="52f5b-296">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="52f5b-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="52f5b-297">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="52f5b-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="52f5b-298">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="52f5b-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
