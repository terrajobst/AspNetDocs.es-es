---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET los modelos y el acceso a los datos de MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: en este laboratorio práctico se supone que tiene conocimientos básicos de ASP.NET MVC. Si no ha usado ASP.NET MVC antes, le recomendamos que pase a ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451471"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="9bead-104">Acceso a datos y modelos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="9bead-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="9bead-105">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9bead-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9bead-106">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="9bead-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9bead-107">Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="9bead-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="9bead-108">Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="9bead-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="9bead-109">Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.</span><span class="sxs-lookup"><span data-stu-id="9bead-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-110">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="9bead-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9bead-111">El proyecto específico de este laboratorio está disponible en los [modelos y el acceso a datos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="9bead-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="9bead-112">En el laboratorio práctico de **conceptos básicos de ASP.NET MVC** , ha estado pasando datos codificados de forma rígida desde los controladores a las plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="9bead-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="9bead-113">Sin embargo, para compilar una aplicación web real, puede que desee usar una base de datos real.</span><span class="sxs-lookup"><span data-stu-id="9bead-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="9bead-114">Este laboratorio práctico le mostrará cómo usar un motor de base de datos para almacenar y recuperar los datos necesarios para la aplicación Music Store.</span><span class="sxs-lookup"><span data-stu-id="9bead-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="9bead-115">Para ello, comenzará con una base de datos existente y creará la Entity Data Model a partir de ella.</span><span class="sxs-lookup"><span data-stu-id="9bead-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="9bead-116">A lo largo de este laboratorio, cumplirá el enfoque **Database First** , así como el enfoque **code First** .</span><span class="sxs-lookup"><span data-stu-id="9bead-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="9bead-117">Sin embargo, también puede usar el enfoque de **Model First** , crear el mismo modelo con las herramientas de y, a continuación, generar la base de datos a partir de él.</span><span class="sxs-lookup"><span data-stu-id="9bead-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="9bead-118">![Database First frente a Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First frente a Model First")</span><span class="sxs-lookup"><span data-stu-id="9bead-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="9bead-119">*Database First frente a Model First*</span><span class="sxs-lookup"><span data-stu-id="9bead-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="9bead-120">Después de generar el modelo, realizará los ajustes adecuados en el StoreController para proporcionar las vistas de almacén con los datos tomados de la base de datos, en lugar de usar datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="9bead-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="9bead-121">No será necesario realizar ningún cambio en las plantillas de vista porque el StoreController devolverá el mismo ViewModels a las plantillas de vista, aunque esta vez los datos procederán de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="9bead-122">**El enfoque Code First**</span><span class="sxs-lookup"><span data-stu-id="9bead-122">**The Code First Approach**</span></span>

<span data-ttu-id="9bead-123">El enfoque Code First nos permite definir el modelo a partir del código sin generar clases que generalmente están acopladas al marco.</span><span class="sxs-lookup"><span data-stu-id="9bead-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="9bead-124">En Code First, los objetos de modelo se definen con POCO, &quot;objetos CLR antiguos sin formato&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bead-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="9bead-125">Los POCO son clases sin formato simples que no tienen herencia y no implementan interfaces.</span><span class="sxs-lookup"><span data-stu-id="9bead-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="9bead-126">Podemos generar automáticamente la base de datos a partir de ellas, o bien usar una base de datos existente y generar la asignación de clase a partir del código.</span><span class="sxs-lookup"><span data-stu-id="9bead-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="9bead-127">Las ventajas de usar este enfoque es que el modelo sigue siendo independiente del marco de persistencia (en este caso, Entity Framework), ya que las clases POCO no se acoplan con el marco de asignación.</span><span class="sxs-lookup"><span data-stu-id="9bead-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-128">Este laboratorio se basa en ASP.NET MVC 4 y en una versión de la aplicación de ejemplo de Music Store personalizada y minimizada para ajustarse solo a las características que se muestran en este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="9bead-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="9bead-129">Si desea explorar toda la aplicación de tutorial de **Music Store** , puede encontrarla en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="9bead-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9bead-130">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9bead-130">Prerequisites</span></span>

<span data-ttu-id="9bead-131">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="9bead-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9bead-132">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="9bead-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9bead-133">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="9bead-133">Setup</span></span>

<span data-ttu-id="9bead-134">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="9bead-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="9bead-135">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bead-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9bead-136">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="9bead-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9bead-137">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bead-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9bead-138">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="9bead-138">Exercises</span></span>

<span data-ttu-id="9bead-139">Este laboratorio práctico se compone de los siguientes ejercicios:</span><span class="sxs-lookup"><span data-stu-id="9bead-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="9bead-140">Ejercicio 1: agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="9bead-141">Ejercicio 2: crear una base de datos con Code First</span><span class="sxs-lookup"><span data-stu-id="9bead-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="9bead-142">Ejercicio 3: consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="9bead-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="9bead-143">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="9bead-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9bead-144">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="9bead-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="9bead-145">Tiempo estimado para completar este laboratorio: **35 minutos**.</span><span class="sxs-lookup"><span data-stu-id="9bead-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="9bead-146">Ejercicio 1: agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="9bead-147">En este ejercicio, obtendrá información sobre cómo agregar una base de datos con las tablas de la aplicación MusicStore a la solución con el fin de consumir sus datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="9bead-148">Una vez que la base de datos se genera con el modelo y se agrega a la solución, modificará la clase StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="9bead-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="9bead-149">Tarea 1: agregar una base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="9bead-150">En esta tarea, agregará una base de datos ya creada con las tablas principales de la aplicación MusicStore a la solución.</span><span class="sxs-lookup"><span data-stu-id="9bead-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="9bead-151">Abra la carpeta **Begin** Solution ubicada en **source/EX1-AddingADatabaseDBFirst/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="9bead-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="9bead-152">Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9bead-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9bead-153">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9bead-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9bead-154">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="9bead-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9bead-155">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="9bead-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9bead-156">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9bead-157">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9bead-158">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="9bead-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9bead-159">Agregue el archivo de base de datos **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="9bead-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="9bead-160">En este laboratorio práctico, usará una base de datos ya creada denominada **MvcMusicStore. MDF**.</span><span class="sxs-lookup"><span data-stu-id="9bead-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="9bead-161">Para ello, haga clic con el botón derecho en **App\_** carpeta de datos, seleccione **Agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="9bead-162">Vaya a **\Source\Assets** y seleccione el archivo **MvcMusicStore. MDF** .</span><span class="sxs-lookup"><span data-stu-id="9bead-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="9bead-163">![Agregar un elemento existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Agregar un elemento existente")</span><span class="sxs-lookup"><span data-stu-id="9bead-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="9bead-164">*Agregar un elemento existente*</span><span class="sxs-lookup"><span data-stu-id="9bead-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="9bead-165">![Archivo de base de datos MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "Archivo de base de datos MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="9bead-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="9bead-166">*Archivo de base de datos MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="9bead-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="9bead-167">La base de datos se ha agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-167">The database has been added to the project.</span></span> <span data-ttu-id="9bead-168">Incluso cuando la base de datos se encuentra dentro de la solución, puede consultarla y actualizarla tal y como se hospedaba en un servidor de base de datos diferente.</span><span class="sxs-lookup"><span data-stu-id="9bead-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="9bead-169">![Base de datos MvcMusicStore en Explorador de soluciones](aspnet-mvc-4-models-and-data-access/_static/image4.png "Base de datos MvcMusicStore en Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="9bead-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="9bead-170">*Base de datos MvcMusicStore en Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="9bead-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="9bead-171">Compruebe la conexión a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-171">Verify the connection to the database.</span></span> <span data-ttu-id="9bead-172">Para ello, haga doble clic en **MvcMusicStore. MDF** para establecer una conexión.</span><span class="sxs-lookup"><span data-stu-id="9bead-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="9bead-173">![Conexión a MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Conexión a MvcMusicStore. MDF")</span><span class="sxs-lookup"><span data-stu-id="9bead-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="9bead-174">*Conexión a MvcMusicStore. MDF*</span><span class="sxs-lookup"><span data-stu-id="9bead-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="9bead-175">Tarea 2: crear un modelo de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="9bead-176">En esta tarea, creará un modelo de datos para interactuar con la base de datos agregada en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="9bead-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="9bead-177">Cree un modelo de datos que represente la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="9bead-178">Para ello, en Explorador de soluciones haga clic con el botón secundario en la carpeta **modelos** , seleccione **Agregar** y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9bead-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="9bead-179">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la plantilla de **datos** y, a continuación, el elemento **Entity Data Model ADO.net** .</span><span class="sxs-lookup"><span data-stu-id="9bead-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="9bead-180">Cambie el nombre del modelo de datos a **StoreDB. edmx** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="9bead-181">![Agregar el StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Agregar el StoreDB ADO.NET Entity Data Model")</span><span class="sxs-lookup"><span data-stu-id="9bead-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="9bead-182">*Agregar el StoreDB ADO.NET Entity Data Model*</span><span class="sxs-lookup"><span data-stu-id="9bead-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="9bead-183">Aparecerá el **Asistente para Entity Data Model** .</span><span class="sxs-lookup"><span data-stu-id="9bead-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="9bead-184">Este asistente le guiará a través de la creación de la capa de modelo.</span><span class="sxs-lookup"><span data-stu-id="9bead-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="9bead-185">Dado que el modelo debe crearse según la base de datos existente agregada recientemente, seleccione **generar desde la base de datos** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-185">Since the model should be created based on the existing database recently added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="9bead-186">![Elección del contenido del modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "Elección del contenido del modelo")</span><span class="sxs-lookup"><span data-stu-id="9bead-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="9bead-187">*Elección del contenido del modelo*</span><span class="sxs-lookup"><span data-stu-id="9bead-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="9bead-188">Dado que va a generar un modelo a partir de una base de datos, deberá especificar la conexión que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="9bead-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="9bead-189">Haga clic en **nueva conexión**.</span><span class="sxs-lookup"><span data-stu-id="9bead-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="9bead-190">Seleccione **Microsoft SQL Server archivo de base de datos** y haga clic en **continuar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="9bead-191">![Elegir origen de datos](aspnet-mvc-4-models-and-data-access/_static/image8.png "Elegir origen de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="9bead-192">*Cuadro de diálogo elegir origen de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="9bead-193">Haga clic en **examinar** y seleccione la base de datos **MvcMusicStore. MDF** que se encuentra en la carpeta **App\_Data** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="9bead-194">![Propiedades de conexión](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propiedades de la conexión")</span><span class="sxs-lookup"><span data-stu-id="9bead-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="9bead-195">*Propiedades de conexión*</span><span class="sxs-lookup"><span data-stu-id="9bead-195">*Connection properties*</span></span>
6. <span data-ttu-id="9bead-196">La clase generada debe tener el mismo nombre que la cadena de conexión de entidad, así que cambie su nombre a **MusicStoreEntities** y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="9bead-197">![Elegir la conexión de datos](aspnet-mvc-4-models-and-data-access/_static/image10.png "Elegir la conexión de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="9bead-198">*Elegir la conexión de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="9bead-199">Elija los objetos de base de datos que desea usar.</span><span class="sxs-lookup"><span data-stu-id="9bead-199">Choose the database objects to use.</span></span> <span data-ttu-id="9bead-200">Dado que el modelo de entidad usará solo las tablas de la base de datos, seleccione la opción **tablas** y asegúrese de que las opciones **incluir columnas de clave externa en el modelo** y poner en **plural o en singular los nombres de objeto generados** también están seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="9bead-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="9bead-201">Cambie el espacio de nombres del modelo a **MvcMusicStore. Model** y haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="9bead-202">![Elección de los objetos de base de datos](aspnet-mvc-4-models-and-data-access/_static/image11.png "Elección de los objetos de base de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="9bead-203">*Elección de los objetos de base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-204">Si se muestra un cuadro de diálogo de advertencia de seguridad, haga clic en **Aceptar** para ejecutar la plantilla y generar las clases para las entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="9bead-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="9bead-205">Aparecerá un diagrama de entidades para la base de datos, mientras que se creará una clase independiente que asigna cada tabla a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="9bead-206">Por ejemplo, la tabla de **álbumes** se representará mediante una clase **album** , donde cada columna de la tabla se asignará a una propiedad de clase.</span><span class="sxs-lookup"><span data-stu-id="9bead-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="9bead-207">Esto le permitirá consultar y trabajar con objetos que representan las filas de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="9bead-208">![Diagrama de entidades](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagrama de entidades")</span><span class="sxs-lookup"><span data-stu-id="9bead-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="9bead-209">*Diagrama de entidades*</span><span class="sxs-lookup"><span data-stu-id="9bead-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-210">Las plantillas T4 (. TT) ejecutan código para generar las clases de entidades y sobrescribirán las clases existentes con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="9bead-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="9bead-211">En este ejemplo, las clases &quot;álbum&quot;, &quot;género&quot; y &quot;artista&quot; se han sobrescrito con el código generado.</span><span class="sxs-lookup"><span data-stu-id="9bead-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="9bead-212">Tarea 3: compilar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9bead-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="9bead-213">En esta tarea, comprobará que, aunque la generación del modelo haya quitado las clases de modelo **album**, **género** y **artista** , el proyecto se compilará correctamente con las nuevas clases de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="9bead-214">Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="9bead-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="9bead-215">![Compilar el proyecto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="9bead-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="9bead-216">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="9bead-216">*Building the project*</span></span>
2. <span data-ttu-id="9bead-217">El proyecto se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="9bead-217">The project builds successfully.</span></span> <span data-ttu-id="9bead-218">¿Por qué sigue funcionando?</span><span class="sxs-lookup"><span data-stu-id="9bead-218">Why does it still work?</span></span> <span data-ttu-id="9bead-219">Funciona porque las tablas de base de datos tienen campos que incluyen las propiedades que estaba usando en el **álbum** de clases quitadas y el **género**.</span><span class="sxs-lookup"><span data-stu-id="9bead-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="9bead-220">![Compilaciones correctas](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilaciones correctas")</span><span class="sxs-lookup"><span data-stu-id="9bead-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="9bead-221">*Compilaciones correctas*</span><span class="sxs-lookup"><span data-stu-id="9bead-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="9bead-222">Aunque el diseñador muestra las entidades en un formato de diagrama, son realmente C# clases.</span><span class="sxs-lookup"><span data-stu-id="9bead-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="9bead-223">Expanda el nodo **StoreDB. edmx** en el explorador de soluciones y, a continuación, **StoreDB.TT**, verá las nuevas entidades generadas.</span><span class="sxs-lookup"><span data-stu-id="9bead-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="9bead-224">![Archivos generados](aspnet-mvc-4-models-and-data-access/_static/image15.png "Archivos generados")</span><span class="sxs-lookup"><span data-stu-id="9bead-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="9bead-225">*Archivos generados*</span><span class="sxs-lookup"><span data-stu-id="9bead-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="9bead-226">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="9bead-227">En esta tarea, actualizará la clase StoreController para que, en lugar de usar datos codificados, consulte la base de datos para recuperar la información.</span><span class="sxs-lookup"><span data-stu-id="9bead-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="9bead-228">Abra **Controllers\StoreController.CS** y agregue el siguiente campo a la clase para que contenga una instancia de la clase **MusicStoreEntities** , denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="9bead-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="9bead-229">(Fragmentos de código- *modelos y acceso a datos-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="9bead-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="9bead-230">La clase **MusicStoreEntities** expone una propiedad de colección para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="9bead-231">Actualice el método de acción de **exploración** para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="9bead-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="9bead-232">(Fragmentos de código- *modelos y acceso a datos; examen del almacén EX1*)</span><span class="sxs-lookup"><span data-stu-id="9bead-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="9bead-233">Está usando una funcionalidad de .NET llamada **LINQ** (Language-Integrated Query) para escribir expresiones de consulta fuertemente tipadas en estas colecciones, que ejecutan código en la base de datos y devuelven objetos que se pueden programar.</span><span class="sxs-lookup"><span data-stu-id="9bead-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="9bead-234">Para obtener más información acerca de LINQ, visite el [sitio de MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="9bead-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="9bead-235">Actualice el método de acción de **Índice** para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="9bead-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="9bead-236">(Fragmento de código- *modelos y acceso a datos-índice del almacén EX1*)</span><span class="sxs-lookup"><span data-stu-id="9bead-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="9bead-237">Actualice el método de acción de **Índice** para recuperar todos los géneros y transformar la colección en una lista.</span><span class="sxs-lookup"><span data-stu-id="9bead-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="9bead-238">(Fragmentos de código- *modelos y acceso a datos-EX1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="9bead-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9bead-239">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9bead-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="9bead-240">En esta tarea, comprobará que la página de índice de la tienda mostrará ahora los géneros almacenados en la base de datos en lugar de los codificados.</span><span class="sxs-lookup"><span data-stu-id="9bead-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="9bead-241">No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve las mismas entidades que antes, pero esta vez los datos proceden de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="9bead-242">Vuelva a compilar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bead-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9bead-243">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="9bead-243">The project starts in the Home page.</span></span> <span data-ttu-id="9bead-244">Compruebe que el menú de **géneros** ya no es una lista codificada y que los datos se recuperan directamente de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="9bead-246">*Examinar géneros de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="9bead-247">Ahora, vaya a cualquier género y compruebe que los álbumes se han rellenado desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="9bead-248">![Examinar álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image17.png "Examinar álbumes de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="9bead-249">*Examinar álbumes de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="9bead-250">Ejercicio 2: crear una base de datos con Code First</span><span class="sxs-lookup"><span data-stu-id="9bead-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="9bead-251">En este ejercicio, obtendrá información sobre cómo usar el enfoque Code First para crear una base de datos con las tablas de la aplicación MusicStore y cómo obtener acceso a sus datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="9bead-252">Una vez generado el modelo, modificará el StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="9bead-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-253">Si ha completado el ejercicio 1 y ya ha trabajado con el enfoque Database First, ahora aprenderá a obtener los mismos resultados con un proceso diferente.</span><span class="sxs-lookup"><span data-stu-id="9bead-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="9bead-254">Las tareas comunes con el ejercicio 1 se han marcado para facilitar la lectura.</span><span class="sxs-lookup"><span data-stu-id="9bead-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="9bead-255">Si no ha completado el ejercicio 1 pero le gustaría aprender el enfoque de Code First, puede empezar a partir de este ejercicio y obtener una cobertura completa del tema.</span><span class="sxs-lookup"><span data-stu-id="9bead-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="9bead-256">Tarea 1: rellenar datos de ejemplo</span><span class="sxs-lookup"><span data-stu-id="9bead-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="9bead-257">En esta tarea, rellenará la base de datos con datos de ejemplo cuando se cree inicialmente con Code-First.</span><span class="sxs-lookup"><span data-stu-id="9bead-257">In this task, you will populate the database with sample data when it is initially created using Code-First.</span></span>

1. <span data-ttu-id="9bead-258">Abra la carpeta **Begin** Solution ubicada en **source/Ex2-CreatingADatabaseCodeFirst/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="9bead-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="9bead-259">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="9bead-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9bead-260">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9bead-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9bead-261">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9bead-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9bead-262">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="9bead-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9bead-263">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="9bead-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9bead-264">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9bead-265">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9bead-266">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="9bead-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9bead-267">Agregue el archivo **SampleData.CS** a la carpeta **Models** .</span><span class="sxs-lookup"><span data-stu-id="9bead-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="9bead-268">Para ello, haga clic con el botón secundario en la carpeta **Models** , seleccione **Agregar** y, a continuación, haga clic en **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="9bead-269">Vaya a **\Source\Assets** y seleccione el archivo **SampleData.CS** .</span><span class="sxs-lookup"><span data-stu-id="9bead-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="9bead-270">![Código de rellenado de datos de ejemplo](aspnet-mvc-4-models-and-data-access/_static/image18.png "Código de rellenado de datos de ejemplo")</span><span class="sxs-lookup"><span data-stu-id="9bead-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="9bead-271">*Código de rellenado de datos de ejemplo*</span><span class="sxs-lookup"><span data-stu-id="9bead-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="9bead-272">Abra el archivo **global.asax.CS** y agregue las siguientes instrucciones *using* .</span><span class="sxs-lookup"><span data-stu-id="9bead-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="9bead-273">(Fragmentos de código- *modelos y acceso a datos-Ex2 variables de asax globales mediante*)</span><span class="sxs-lookup"><span data-stu-id="9bead-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="9bead-274">En el método **Start () de Application\_,** agregue la siguiente línea para establecer el inicializador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="9bead-275">(Fragmentos de código- *modelos y acceso a datos-Ex2 global asax SetInitializer*)</span><span class="sxs-lookup"><span data-stu-id="9bead-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="9bead-276">Tarea 2: configurar la conexión a la base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="9bead-277">Ahora que ya ha agregado una base de datos a nuestro proyecto, escribirá en el archivo **Web. config** la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="9bead-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="9bead-278">Agregue una cadena de conexión en **Web. config**. Para ello, abra el **archivo Web. config** en la raíz del proyecto y reemplace la cadena de conexión denominada DefaultConnection por esta línea en la sección **&lt;connectionStrings&gt;** :</span><span class="sxs-lookup"><span data-stu-id="9bead-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="9bead-279">![Ubicación del archivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Ubicación del archivo Web. config")</span><span class="sxs-lookup"><span data-stu-id="9bead-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="9bead-280">*Ubicación del archivo Web. config*</span><span class="sxs-lookup"><span data-stu-id="9bead-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="9bead-281">Tarea 3: trabajar con el modelo</span><span class="sxs-lookup"><span data-stu-id="9bead-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="9bead-282">Ahora que ya ha configurado la conexión a la base de datos, vinculará el modelo con las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="9bead-283">En esta tarea, creará una clase que se vinculará a la base de datos con Code First.</span><span class="sxs-lookup"><span data-stu-id="9bead-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="9bead-284">Recuerde que hay una clase de modelo POCO existente que debe modificarse.</span><span class="sxs-lookup"><span data-stu-id="9bead-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-285">Si ha completado el ejercicio 1, observará que este paso lo realizó un asistente.</span><span class="sxs-lookup"><span data-stu-id="9bead-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="9bead-286">Al realizar Code First, creará manualmente las clases que se vincularán a las entidades de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="9bead-287">Abra la carpeta del proyecto de clase POCO modelo **género** desde **modelos** e incluya un identificador.</span><span class="sxs-lookup"><span data-stu-id="9bead-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="9bead-288">Use una propiedad int con el nombre **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="9bead-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="9bead-289">(Fragmentos de código- *modelos y acceso a datos-Ex2 Code First género*)</span><span class="sxs-lookup"><span data-stu-id="9bead-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="9bead-290">Para trabajar con convenciones de Code First, el género de la clase debe tener una propiedad de clave principal que se detectará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="9bead-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="9bead-291">Puede leer más información sobre las convenciones de Code First en este [artículo de MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="9bead-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="9bead-292">Ahora, abra la carpeta de proyecto de clase POCO modelo **album** de **modelos** e incluya las claves externas, cree propiedades con los nombres **GenreId** y **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="9bead-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="9bead-293">Esta clase ya tiene **GenreId** para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="9bead-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="9bead-294">(Fragmento de código- *modelos y acceso a datos-Ex2 Code First album*)</span><span class="sxs-lookup"><span data-stu-id="9bead-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="9bead-295">Abra el **intérprete** de clase poco modelo y incluya la propiedad **ArtistId** .</span><span class="sxs-lookup"><span data-stu-id="9bead-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="9bead-296">(Fragmentos de código- *modelos y acceso a datos-Ex2 Code First artista*)</span><span class="sxs-lookup"><span data-stu-id="9bead-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="9bead-297">Haga clic con el botón derecho en la carpeta del proyecto **modelos** y seleccione **Agregar | Clase**.</span><span class="sxs-lookup"><span data-stu-id="9bead-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="9bead-298">Asigne al archivo el nombre **MusicStoreEntities.CS**.</span><span class="sxs-lookup"><span data-stu-id="9bead-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="9bead-299">A continuación, haga clic en **Agregar.**</span><span class="sxs-lookup"><span data-stu-id="9bead-299">Then, click **Add.**</span></span>

    <span data-ttu-id="9bead-300">![Agregar una clase](aspnet-mvc-4-models-and-data-access/_static/image20.png "Agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="9bead-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="9bead-301">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="9bead-301">*Adding a new item*</span></span>

    <span data-ttu-id="9bead-302">![Agregar una clase2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Agregar una clase2")</span><span class="sxs-lookup"><span data-stu-id="9bead-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="9bead-303">*Agregar una clase*</span><span class="sxs-lookup"><span data-stu-id="9bead-303">*Adding a class*</span></span>
5. <span data-ttu-id="9bead-304">Abra la clase que acaba de crear, **MusicStoreEntities.CS**e incluya los espacios de nombres **System. Data. Entity** y **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="9bead-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="9bead-305">Reemplace la declaración de clase para extender la clase **DbContext** : declare un método público **DBSet** e invalide **OnModelCreating** .</span><span class="sxs-lookup"><span data-stu-id="9bead-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="9bead-306">Después de este paso, obtendrá una clase de dominio que vinculará el modelo con el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9bead-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="9bead-307">Para ello, reemplace el código de clase por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bead-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="9bead-308">(Fragmentos de código- *modelos y acceso a datos-Ex2 Code First MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="9bead-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="9bead-309">Con Entity Framework **DbContext** y **DBSet** , podrá consultar el género de clase poco.</span><span class="sxs-lookup"><span data-stu-id="9bead-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="9bead-310">Al extender el método **OnModelCreating** , se especifica en el **código** cómo se asignará el género a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="9bead-311">Puede encontrar más información sobre DBContext y DBSet en este artículo de MSDN: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="9bead-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="9bead-312">Tarea 4: consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="9bead-313">En esta tarea, actualizará la clase StoreController para que, en lugar de usar datos codificados, la recupere de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-314">Esta tarea es común con el ejercicio 1.</span><span class="sxs-lookup"><span data-stu-id="9bead-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="9bead-315">Si ha completado el ejercicio 1, tendrá en cuenta que estos pasos son los mismos en ambos enfoques (primero la base de datos o el código primero).</span><span class="sxs-lookup"><span data-stu-id="9bead-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="9bead-316">Son diferentes en la forma en que se vinculan los datos al modelo, pero el acceso a las entidades de datos todavía es transparente desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="9bead-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>

1. <span data-ttu-id="9bead-317">Abra **Controllers\StoreController.CS** y agregue el siguiente campo a la clase para que contenga una instancia de la clase **MusicStoreEntities** , denominada **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="9bead-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="9bead-318">(Fragmentos de código- *modelos y acceso a datos-EX1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="9bead-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="9bead-319">La clase **MusicStoreEntities** expone una propiedad de colección para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="9bead-320">Actualice el método de acción de **exploración** para recuperar un género con todos los **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="9bead-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="9bead-321">(Fragmentos de código- *modelos y acceso a datos-Ex2 Store Browse*)</span><span class="sxs-lookup"><span data-stu-id="9bead-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="9bead-322">Está usando una funcionalidad de .NET llamada **LINQ** (Language-Integrated Query) para escribir expresiones de consulta fuertemente tipadas en estas colecciones, que ejecutan código en la base de datos y devuelven objetos que se pueden programar.</span><span class="sxs-lookup"><span data-stu-id="9bead-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="9bead-323">Para obtener más información acerca de LINQ, visite el [sitio de MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="9bead-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="9bead-324">Actualice el método de acción de **Índice** para recuperar todos los géneros.</span><span class="sxs-lookup"><span data-stu-id="9bead-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="9bead-325">(Fragmento de código- *modelos y acceso a datos-índice de almacén Ex2*)</span><span class="sxs-lookup"><span data-stu-id="9bead-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="9bead-326">Actualice el método de acción de **Índice** para recuperar todos los géneros y transformar la colección en una lista.</span><span class="sxs-lookup"><span data-stu-id="9bead-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="9bead-327">(Fragmentos de código- *modelos y acceso a datos-Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="9bead-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9bead-328">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9bead-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="9bead-329">En esta tarea, comprobará que la página de índice de la tienda mostrará ahora los géneros almacenados en la base de datos en lugar de los codificados.</span><span class="sxs-lookup"><span data-stu-id="9bead-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="9bead-330">No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve el mismo **StoreIndexViewModel** que antes, pero esta vez los datos proceden de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="9bead-331">Vuelva a compilar la solución y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bead-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9bead-332">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="9bead-332">The project starts in the Home page.</span></span> <span data-ttu-id="9bead-333">Compruebe que el menú de **géneros** ya no es una lista codificada y que los datos se recuperan directamente de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="9bead-335">*Examinar géneros de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="9bead-336">Ahora, vaya a cualquier género y compruebe que los álbumes se han rellenado desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="9bead-337">![Examinar álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image23.png "Examinar álbumes de la base de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="9bead-338">*Examinar álbumes de la base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="9bead-339">Ejercicio 3: consultar la base de datos con parámetros</span><span class="sxs-lookup"><span data-stu-id="9bead-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="9bead-340">En este ejercicio, obtendrá información sobre cómo consultar la base de datos mediante parámetros y cómo usar el modelado de resultados de consultas, una característica que reduce el número de accesos de base de datos a la recuperación de datos de una manera más eficaz.</span><span class="sxs-lookup"><span data-stu-id="9bead-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-341">Para obtener más información sobre el modelado de resultados de consultas, visite el siguiente [artículo de MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="9bead-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="9bead-342">Tarea 1: modificación de StoreController para recuperar álbumes de la base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="9bead-343">En esta tarea, cambiará la clase **StoreController** para tener acceso a la base de datos con el fin de recuperar álbumes de un género específico.</span><span class="sxs-lookup"><span data-stu-id="9bead-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="9bead-344">Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** si quiere usar el enfoque de primer código o la carpeta **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** si desea usar el enfoque de primera base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="9bead-345">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="9bead-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9bead-346">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9bead-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9bead-347">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9bead-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9bead-348">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="9bead-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9bead-349">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="9bead-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9bead-350">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9bead-351">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9bead-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9bead-352">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="9bead-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9bead-353">Abra la clase **StoreController** para cambiar el método de acción de **exploración** .</span><span class="sxs-lookup"><span data-stu-id="9bead-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="9bead-354">Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.</span><span class="sxs-lookup"><span data-stu-id="9bead-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="9bead-355">Cambie el método de acción de **exploración** para recuperar los álbumes de un género específico.</span><span class="sxs-lookup"><span data-stu-id="9bead-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="9bead-356">Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bead-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="9bead-357">(Fragmentos de código- *modelos y acceso a datos-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="9bead-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="9bead-358">Para rellenar una colección de la entidad, debe utilizar el método **include** para especificar que desea recuperar también los álbumes.</span><span class="sxs-lookup"><span data-stu-id="9bead-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="9bead-359">Puede usar el. Extensión **Single ()** en LINQ porque, en este caso, solo se espera un género para un álbum.</span><span class="sxs-lookup"><span data-stu-id="9bead-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="9bead-360">El método **Single ()** toma una expresión lambda como parámetro, que en este caso especifica un objeto de género único, de modo que su nombre coincide con el valor definido.</span><span class="sxs-lookup"><span data-stu-id="9bead-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="9bead-361">Aprovechará una característica que le permitirá indicar otras entidades relacionadas que desea cargar también cuando se recupere el objeto Genre.</span><span class="sxs-lookup"><span data-stu-id="9bead-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="9bead-362">Esta característica se denomina **modelado de resultados de consultas**y permite reducir el número de veces que es necesario tener acceso a la base de datos para recuperar información.</span><span class="sxs-lookup"><span data-stu-id="9bead-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="9bead-363">En este escenario, querrá capturar previamente los álbumes del género que recupere.</span><span class="sxs-lookup"><span data-stu-id="9bead-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="9bead-364">La consulta incluye **géneros. incluya (&quot;álbumes&quot;)** para indicar que desea también álbumes relacionados.</span><span class="sxs-lookup"><span data-stu-id="9bead-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="9bead-365">Esto producirá una aplicación más eficaz, ya que recuperará los datos de género y de álbum en una única solicitud de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="9bead-366">Tarea 2: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="9bead-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="9bead-367">En esta tarea, ejecutará la aplicación y recuperará álbumes de un género específico de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="9bead-368">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bead-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9bead-369">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="9bead-369">The project starts in the Home page.</span></span> <span data-ttu-id="9bead-370">Cambie la dirección URL a **/Store/Browse? Genre = pop** para comprobar que los resultados se recuperan de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="9bead-371">![Examinar por género](aspnet-mvc-4-models-and-data-access/_static/image24.png "Examinar por género")</span><span class="sxs-lookup"><span data-stu-id="9bead-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="9bead-372">*Examinar/Store/Browse? género = pop*</span><span class="sxs-lookup"><span data-stu-id="9bead-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="9bead-373">Tarea 3: acceso a álbumes por identificador</span><span class="sxs-lookup"><span data-stu-id="9bead-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="9bead-374">En esta tarea, repetirá el procedimiento anterior para obtener álbumes por su identificador.</span><span class="sxs-lookup"><span data-stu-id="9bead-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="9bead-375">Cierre el explorador si es necesario para volver a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bead-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="9bead-376">Abra la clase **StoreController** para cambiar el método de acción **Details** .</span><span class="sxs-lookup"><span data-stu-id="9bead-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="9bead-377">Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.</span><span class="sxs-lookup"><span data-stu-id="9bead-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="9bead-378">Cambie el método de acción de **detalles** para recuperar los detalles de los álbumes según su **identificador**. Para ello, reemplace el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bead-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="9bead-379">(Fragmentos de código- *modelos y acceso a datos-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="9bead-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9bead-380">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="9bead-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="9bead-381">En esta tarea, ejecutará la aplicación en un explorador Web y obtendrá los detalles del álbum por su identificador.</span><span class="sxs-lookup"><span data-stu-id="9bead-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="9bead-382">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bead-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9bead-383">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="9bead-383">The project starts in the Home page.</span></span> <span data-ttu-id="9bead-384">Cambie la dirección URL a **/Store/details/51** o examine los géneros y seleccione un álbum para comprobar que los resultados se recuperan de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="9bead-385">![Detalles de la exploración](aspnet-mvc-4-models-and-data-access/_static/image25.png "Detalles de la exploración")</span><span class="sxs-lookup"><span data-stu-id="9bead-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="9bead-386">*Examinar/Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="9bead-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="9bead-387">Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="9bead-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9bead-388">Resumen</span><span class="sxs-lookup"><span data-stu-id="9bead-388">Summary</span></span>

<span data-ttu-id="9bead-389">Al completar este laboratorio práctico, ha aprendido los aspectos básicos de los modelos y el acceso a los datos de ASP.NET MVC, mediante el enfoque de **Database First** , así como el enfoque **code First** :</span><span class="sxs-lookup"><span data-stu-id="9bead-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="9bead-390">Cómo agregar una base de datos a la solución para consumir sus datos</span><span class="sxs-lookup"><span data-stu-id="9bead-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="9bead-391">Cómo actualizar controladores para proporcionar plantillas de vista con los datos tomados de la base de datos en lugar de codificarlos de forma rígida</span><span class="sxs-lookup"><span data-stu-id="9bead-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="9bead-392">Cómo consultar la base de datos mediante parámetros</span><span class="sxs-lookup"><span data-stu-id="9bead-392">How to query the database using parameters</span></span>
- <span data-ttu-id="9bead-393">Cómo usar la forma de los resultados de la consulta, una característica que reduce el número de accesos a bases de datos, la recuperación de datos de una manera más eficaz</span><span class="sxs-lookup"><span data-stu-id="9bead-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="9bead-394">Cómo usar los enfoques de Database First y Code First en Microsoft Entity Framework para vincular la base de datos con el modelo</span><span class="sxs-lookup"><span data-stu-id="9bead-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9bead-395">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="9bead-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9bead-396">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="9bead-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9bead-397">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="9bead-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9bead-398">Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9bead-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9bead-399">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9bead-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9bead-400">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="9bead-400">Click on **Install Now**.</span></span> <span data-ttu-id="9bead-401">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="9bead-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9bead-402">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="9bead-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9bead-403">![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9bead-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9bead-404">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="9bead-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9bead-405">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="9bead-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="9bead-407">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="9bead-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9bead-408">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="9bead-408">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="9bead-410">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="9bead-410">*Installation progress*</span></span>
6. <span data-ttu-id="9bead-411">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-411">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="9bead-413">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="9bead-413">*Installation completed*</span></span>
7. <span data-ttu-id="9bead-414">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="9bead-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9bead-415">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="9bead-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="9bead-417">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="9bead-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9bead-418">Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="9bead-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9bead-419">Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="9bead-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="9bead-420">Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure</span><span class="sxs-lookup"><span data-stu-id="9bead-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="9bead-421">Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.</span><span class="sxs-lookup"><span data-stu-id="9bead-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-422">Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico.</span><span class="sxs-lookup"><span data-stu-id="9bead-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9bead-423">Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="9bead-423">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9bead-424">![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Iniciar sesión en Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="9bead-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9bead-425">*Iniciar sesión en Windows Azure Portal de administración*</span><span class="sxs-lookup"><span data-stu-id="9bead-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="9bead-426">Haga clic en **nuevo** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="9bead-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9bead-427">![Crear un nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Crear un nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="9bead-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9bead-428">*Crear un nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="9bead-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9bead-429">Haga clic en **compute** | **sitio web**.</span><span class="sxs-lookup"><span data-stu-id="9bead-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9bead-430">A continuación, seleccione la opción **creación rápida** .</span><span class="sxs-lookup"><span data-stu-id="9bead-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="9bead-431">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.</span><span class="sxs-lookup"><span data-stu-id="9bead-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-432">Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="9bead-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9bead-433">La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="9bead-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="9bead-434">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9bead-435">![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Crear un nuevo sitio web mediante creación rápida")</span><span class="sxs-lookup"><span data-stu-id="9bead-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9bead-436">*Crear un nuevo sitio web mediante creación rápida*</span><span class="sxs-lookup"><span data-stu-id="9bead-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9bead-437">Espere hasta que se cree el nuevo **sitio web** .</span><span class="sxs-lookup"><span data-stu-id="9bead-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9bead-438">Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** .</span><span class="sxs-lookup"><span data-stu-id="9bead-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9bead-439">Compruebe que el nuevo sitio web funciona.</span><span class="sxs-lookup"><span data-stu-id="9bead-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9bead-440">![Ir al nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Ir al nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="9bead-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9bead-441">*Ir al nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="9bead-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9bead-442">![Sitio web en ejecución](aspnet-mvc-4-models-and-data-access/_static/image35.png "Sitio web en ejecución")</span><span class="sxs-lookup"><span data-stu-id="9bead-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="9bead-443">*Sitio web en ejecución*</span><span class="sxs-lookup"><span data-stu-id="9bead-443">*Web site running*</span></span>
6. <span data-ttu-id="9bead-444">Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="9bead-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9bead-445">![Abrir las páginas de administración de sitios web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Abrir las páginas de administración de sitios web")</span><span class="sxs-lookup"><span data-stu-id="9bead-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9bead-446">*Abrir las páginas de administración de sitios web*</span><span class="sxs-lookup"><span data-stu-id="9bead-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9bead-447">En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .</span><span class="sxs-lookup"><span data-stu-id="9bead-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-448">El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="9bead-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="9bead-449">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos.</span><span class="sxs-lookup"><span data-stu-id="9bead-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9bead-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="9bead-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="9bead-451">![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Descargar el perfil de publicación del sitio web")</span><span class="sxs-lookup"><span data-stu-id="9bead-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9bead-452">*Descargar el perfil de publicación del sitio web*</span><span class="sxs-lookup"><span data-stu-id="9bead-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9bead-453">Descargue el archivo de Perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="9bead-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9bead-454">Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9bead-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="9bead-455">![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image38.png "Guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="9bead-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9bead-456">*Guardar el archivo de Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="9bead-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9bead-457">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="9bead-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9bead-458">Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="9bead-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9bead-459">Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="9bead-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9bead-460">Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bead-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9bead-461">Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**.</span><span class="sxs-lookup"><span data-stu-id="9bead-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9bead-462">Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="9bead-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9bead-463">Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="9bead-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9bead-464">No cree todavía la base de datos, ya que se creará en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="9bead-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9bead-465">![Panel de SQL Database Server](aspnet-mvc-4-models-and-data-access/_static/image39.png "Panel de SQL Database Server")</span><span class="sxs-lookup"><span data-stu-id="9bead-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9bead-466">*Panel de SQL Database Server*</span><span class="sxs-lookup"><span data-stu-id="9bead-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9bead-467">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor.</span><span class="sxs-lookup"><span data-stu-id="9bead-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9bead-468">Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-models-and-data-access/_static/image40.png).</span><span class="sxs-lookup"><span data-stu-id="9bead-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="9bead-470">*Agregando la dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="9bead-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9bead-471">Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="9bead-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="9bead-473">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="9bead-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9bead-474">Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="9bead-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9bead-475">Vuelva a la solución ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9bead-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9bead-476">En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9bead-477">![Publicar la aplicación](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="9bead-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="9bead-478">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="9bead-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="9bead-479">Importe el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="9bead-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9bead-480">![Importar el perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="9bead-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9bead-481">*Importando Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="9bead-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="9bead-482">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="9bead-482">Click **Validate Connection**.</span></span> <span data-ttu-id="9bead-483">Una vez finalizada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9bead-484">La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.</span><span class="sxs-lookup"><span data-stu-id="9bead-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9bead-485">![Validando conexión](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validando conexión")</span><span class="sxs-lookup"><span data-stu-id="9bead-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="9bead-486">*Validando conexión*</span><span class="sxs-lookup"><span data-stu-id="9bead-486">*Validating connection*</span></span>
4. <span data-ttu-id="9bead-487">En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="9bead-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9bead-488">![Configuración de web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuración de web deploy")</span><span class="sxs-lookup"><span data-stu-id="9bead-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9bead-489">*Configuración de web deploy*</span><span class="sxs-lookup"><span data-stu-id="9bead-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9bead-490">Configure la conexión de base de datos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="9bead-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="9bead-491">En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="9bead-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="9bead-492">En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="9bead-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="9bead-493">En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="9bead-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="9bead-494">Escriba un nuevo nombre de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9bead-494">Type a new database name.</span></span>

     <span data-ttu-id="9bead-495">![Configuración de la cadena de conexión de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="9bead-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="9bead-496">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="9bead-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9bead-497">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-497">Then click **OK**.</span></span> <span data-ttu-id="9bead-498">Cuando se le pida que cree la base de datos, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="9bead-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9bead-499">![Crear la base de datos](aspnet-mvc-4-models-and-data-access/_static/image48.png "Crear la cadena de base de datos")</span><span class="sxs-lookup"><span data-stu-id="9bead-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="9bead-500">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="9bead-500">*Creating the database*</span></span>
7. <span data-ttu-id="9bead-501">La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9bead-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9bead-502">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="9bead-502">Then click **Next**.</span></span>

    <span data-ttu-id="9bead-503">![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Cadena de conexión que apunta a SQL Database")</span><span class="sxs-lookup"><span data-stu-id="9bead-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9bead-504">*Cadena de conexión que apunta a SQL Database*</span><span class="sxs-lookup"><span data-stu-id="9bead-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9bead-505">En la página **vista previa** , haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="9bead-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9bead-506">![Publicación de la aplicación Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publicación de la aplicación Web")</span><span class="sxs-lookup"><span data-stu-id="9bead-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="9bead-507">*Publicación de la aplicación Web*</span><span class="sxs-lookup"><span data-stu-id="9bead-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="9bead-508">Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.</span><span class="sxs-lookup"><span data-stu-id="9bead-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="9bead-509">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="9bead-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="9bead-510">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="9bead-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9bead-511">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="9bead-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9bead-512">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="9bead-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9bead-513">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="9bead-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9bead-514">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="9bead-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9bead-515">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="9bead-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9bead-516">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="9bead-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9bead-517">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="9bead-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9bead-518">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="9bead-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9bead-519">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="9bead-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9bead-520">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-models-and-data-access/_static/image52.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="9bead-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9bead-521">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="9bead-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="9bead-522">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-models-and-data-access/_static/image53.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="9bead-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9bead-523">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="9bead-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9bead-524">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-models-and-data-access/_static/image54.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="9bead-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9bead-525">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="9bead-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9bead-526">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="9bead-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="9bead-527">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="9bead-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="9bead-528">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="9bead-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="9bead-529">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="9bead-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9bead-530">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-models-and-data-access/_static/image55.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="9bead-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9bead-531">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="9bead-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9bead-532">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-models-and-data-access/_static/image56.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="9bead-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9bead-533">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="9bead-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
