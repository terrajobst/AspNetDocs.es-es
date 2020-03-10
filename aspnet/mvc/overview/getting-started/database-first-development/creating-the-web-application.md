---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: creación de la aplicación web y los modelos de datos para EF Database First con ASP.NET MVC'
description: Este tutorial se centra en la creación de la aplicación web y la generación de los modelos de datos basados en las tablas de base de datos.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499531"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="8d84f-103">Tutorial: creación de la aplicación web y los modelos de datos para EF Database First con ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8d84f-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="8d84f-104">Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="8d84f-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8d84f-105">En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8d84f-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8d84f-107">Este tutorial se centra en la creación de la aplicación web y la generación de los modelos de datos basados en las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="8d84f-108">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="8d84f-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d84f-109">Crear una aplicación web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d84f-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="8d84f-110">Generar los modelos</span><span class="sxs-lookup"><span data-stu-id="8d84f-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8d84f-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8d84f-111">Prerequisites</span></span>

* [<span data-ttu-id="8d84f-112">Introducción a Entity Framework 6 Database First con MVC 5</span><span class="sxs-lookup"><span data-stu-id="8d84f-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="8d84f-113">Crear una aplicación web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d84f-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="8d84f-114">En una solución nueva o en la misma solución que el proyecto de base de datos, cree un nuevo proyecto en Visual Studio y seleccione la plantilla **aplicación Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="8d84f-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="8d84f-115">Asigne al proyecto el nombre **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-115">Name the project **ContosoSite**.</span></span>

![creación de proyecto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="8d84f-117">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-117">Click **OK**.</span></span>

<span data-ttu-id="8d84f-118">En la ventana nuevo proyecto de ASP.NET, seleccione la plantilla **MVC** .</span><span class="sxs-lookup"><span data-stu-id="8d84f-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="8d84f-119">Puede borrar la opción **host en la nube** por ahora porque implementará la aplicación en la nube más adelante.</span><span class="sxs-lookup"><span data-stu-id="8d84f-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="8d84f-120">Haga clic en **Aceptar** para crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d84f-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="8d84f-121">El proyecto se crea con los archivos y carpetas predeterminados.</span><span class="sxs-lookup"><span data-stu-id="8d84f-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="8d84f-122">En este tutorial, usará Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8d84f-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="8d84f-123">Puede comprobar la versión de Entity Framework del proyecto a través de la ventana administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="8d84f-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="8d84f-124">Si es necesario, actualice su versión de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8d84f-124">If necessary, update your version of Entity Framework.</span></span>

![Mostrar versión](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="8d84f-126">Generar los modelos</span><span class="sxs-lookup"><span data-stu-id="8d84f-126">Generate the models</span></span>

<span data-ttu-id="8d84f-127">Ahora creará Entity Framework modelos a partir de las tablas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="8d84f-128">Estos modelos son clases que usará para trabajar con los datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="8d84f-129">Cada modelo refleja una tabla en la base de datos y contiene propiedades que corresponden a las columnas de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8d84f-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="8d84f-130">Haga clic con el botón secundario en la carpeta **modelos** y seleccione **Agregar** y **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="8d84f-131">En la ventana Agregar nuevo elemento, seleccione **datos** en el panel izquierdo y **ADO.NET Entity Data Model** en las opciones del panel central.</span><span class="sxs-lookup"><span data-stu-id="8d84f-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="8d84f-132">Asigne al nuevo archivo de modelo el nombre **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="8d84f-133">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-133">Click **Add**.</span></span>

<span data-ttu-id="8d84f-134">En el Asistente para Entity Data Model, seleccione **EF Designer en la base de datos**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="8d84f-135">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-135">Click **Next**.</span></span>

<span data-ttu-id="8d84f-136">Si tiene conexiones de base de datos definidas en el entorno de desarrollo, es posible que vea una de estas conexiones previamente seleccionada.</span><span class="sxs-lookup"><span data-stu-id="8d84f-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="8d84f-137">Sin embargo, desea crear una nueva conexión a la base de datos creada en la primera parte de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="8d84f-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="8d84f-138">Haga clic en el botón **nueva conexión** .</span><span class="sxs-lookup"><span data-stu-id="8d84f-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="8d84f-139">En el ventana Propiedades de conexión, proporcione el nombre del servidor local en el que se creó la base de datos (en este caso, **LocalDB) \ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="8d84f-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="8d84f-140">Después de proporcionar el nombre del servidor, seleccione el ContosoUniversityData de las bases de datos disponibles.</span><span class="sxs-lookup"><span data-stu-id="8d84f-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![establecer propiedades de conexión](creating-the-web-application/_static/image8.png)

<span data-ttu-id="8d84f-142">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-142">Click **OK**.</span></span>

<span data-ttu-id="8d84f-143">Ahora se muestran las propiedades de conexión correctas.</span><span class="sxs-lookup"><span data-stu-id="8d84f-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="8d84f-144">Puede usar el nombre predeterminado para la conexión en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="8d84f-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="8d84f-145">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-145">Click **Next**.</span></span>

<span data-ttu-id="8d84f-146">Seleccione la versión más reciente de Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8d84f-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="8d84f-147">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-147">Click **Next**.</span></span>

<span data-ttu-id="8d84f-148">Seleccione **tablas** para generar modelos para las tres tablas.</span><span class="sxs-lookup"><span data-stu-id="8d84f-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="8d84f-149">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="8d84f-149">Click **Finish**.</span></span>

<span data-ttu-id="8d84f-150">Si recibe una advertencia de seguridad, seleccione **Aceptar** para continuar con la ejecución de la plantilla.</span><span class="sxs-lookup"><span data-stu-id="8d84f-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="8d84f-151">Los modelos se generan a partir de las tablas de base de datos y se muestra un diagrama que muestra las propiedades y las relaciones entre las tablas.</span><span class="sxs-lookup"><span data-stu-id="8d84f-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagrama del modelo](creating-the-web-application/_static/image11.png)

<span data-ttu-id="8d84f-153">La carpeta modelos ahora incluye muchos archivos nuevos relacionados con los modelos generados a partir de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="8d84f-154">El archivo **ContosoModel.Context.CS** contiene una clase que se deriva de la clase **DbContext** y proporciona una propiedad para cada clase de modelo que corresponde a una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="8d84f-155">Los archivos **Course.CS**, **Enrollment.CS**y **Student.CS** contienen las clases de modelo que representan las tablas de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="8d84f-156">Usará tanto la clase de contexto como las clases de modelo al trabajar con la técnica scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8d84f-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="8d84f-157">Antes de continuar con este tutorial, compile el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8d84f-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="8d84f-158">En la siguiente sección, generará código basado en los modelos de datos, pero esa sección no funcionará si el proyecto no se ha compilado.</span><span class="sxs-lookup"><span data-stu-id="8d84f-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d84f-159">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="8d84f-159">Next steps</span></span>

<span data-ttu-id="8d84f-160">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="8d84f-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8d84f-161">Se creó una aplicación Web de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d84f-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="8d84f-162">Modelos generados</span><span class="sxs-lookup"><span data-stu-id="8d84f-162">Generated the models</span></span>

<span data-ttu-id="8d84f-163">Avance al siguiente tutorial para aprender a crear código generado basado en los modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="8d84f-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8d84f-164">Generar vistas</span><span class="sxs-lookup"><span data-stu-id="8d84f-164">Generating views</span></span>](generating-views.md)
