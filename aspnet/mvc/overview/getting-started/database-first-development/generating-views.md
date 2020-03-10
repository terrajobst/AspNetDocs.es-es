---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: generación de vistas para EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en el uso de la técnica scaffolding de ASP.NET para generar los controladores y las vistas.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499477"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="8513b-103">Tutorial: generación de vistas para EF Database First con la aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8513b-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="8513b-104">Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="8513b-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8513b-105">En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8513b-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8513b-107">Este tutorial se centra en el uso de la técnica scaffolding de ASP.NET para generar los controladores y las vistas.</span><span class="sxs-lookup"><span data-stu-id="8513b-107">This tutorial focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="8513b-108">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="8513b-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8513b-109">Agregar scaffold</span><span class="sxs-lookup"><span data-stu-id="8513b-109">Add scaffold</span></span>
> * <span data-ttu-id="8513b-110">Agregar vínculos a nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="8513b-110">Add links to new views</span></span>
> * <span data-ttu-id="8513b-111">Mostrar vistas de estudiante</span><span class="sxs-lookup"><span data-stu-id="8513b-111">Display student views</span></span>
> * <span data-ttu-id="8513b-112">Mostrar vistas de inscripción</span><span class="sxs-lookup"><span data-stu-id="8513b-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="8513b-113">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="8513b-113">Prerequisite</span></span>

* [<span data-ttu-id="8513b-114">Crear la aplicación web y los modelos de datos</span><span class="sxs-lookup"><span data-stu-id="8513b-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="8513b-115">Agregar scaffold</span><span class="sxs-lookup"><span data-stu-id="8513b-115">Add scaffold</span></span>

<span data-ttu-id="8513b-116">Está listo para generar código que proporcionará las operaciones de datos estándar para las clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="8513b-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="8513b-117">Agregue el código agregando un elemento scaffold.</span><span class="sxs-lookup"><span data-stu-id="8513b-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="8513b-118">Hay muchas opciones para el tipo de scaffolding que puede Agregar; en este tutorial, el scaffolding incluirá un controlador y vistas que corresponden a los modelos de estudiantes e inscripción que creó en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="8513b-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="8513b-119">Para mantener la coherencia en el proyecto, agregará el nuevo controlador a la carpeta de **Controladores** existentes.</span><span class="sxs-lookup"><span data-stu-id="8513b-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="8513b-120">Haga clic con el botón derecho en la carpeta **Controllers** y seleccione **Agregar** > **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="8513b-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="8513b-121">Seleccione el **controlador de MVC 5 con vistas con Entity Framework** opción.</span><span class="sxs-lookup"><span data-stu-id="8513b-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="8513b-122">Esta opción generará el controlador y las vistas para actualizar, eliminar, crear y mostrar los datos en el modelo.</span><span class="sxs-lookup"><span data-stu-id="8513b-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Agregar controlador de MVC](generating-views/_static/image2.png)

<span data-ttu-id="8513b-124">Seleccione **Student (ContosoSite. Models)** para la clase Model y seleccione **ContosoUniversityDataEntities (ContosoSite. Models)** para la clase context.</span><span class="sxs-lookup"><span data-stu-id="8513b-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="8513b-125">Mantenga el nombre del controlador como **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="8513b-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="8513b-126">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8513b-126">Click **Add**.</span></span>

<span data-ttu-id="8513b-127">Si recibe un error, puede deberse a que no ha compilado el proyecto en la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="8513b-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="8513b-128">Si es así, intente compilar el proyecto y, a continuación, vuelva a agregar el elemento con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8513b-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="8513b-129">Una vez completado el proceso de generación de código, verá un nuevo controlador y vistas en los **Controladores** y **vistas** de proyecto > carpetas de **estudiantes** .</span><span class="sxs-lookup"><span data-stu-id="8513b-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>

<span data-ttu-id="8513b-130">Vuelva a realizar los mismos pasos, pero agregue un scaffolding a la clase **Enrollment** .</span><span class="sxs-lookup"><span data-stu-id="8513b-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="8513b-131">Cuando termine, tendrá un archivo **EnrollmentsController.CS** y una carpeta en **views** denominado **inscripciones** con las vistas crear, eliminar, detalles, editar e índice.</span><span class="sxs-lookup"><span data-stu-id="8513b-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="8513b-132">Agregar vínculos a nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="8513b-132">Add links to new views</span></span>

<span data-ttu-id="8513b-133">Para facilitar la navegación a las nuevas vistas, puede Agregar un par de hipervínculos a las vistas de índice para estudiantes e inscripciones.</span><span class="sxs-lookup"><span data-stu-id="8513b-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="8513b-134">Abra el archivo en **views** > **Home** > *index. cshtml*, que es la Página principal de su sitio.</span><span class="sxs-lookup"><span data-stu-id="8513b-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="8513b-135">Agregue el código siguiente debajo de Jumbotron.</span><span class="sxs-lookup"><span data-stu-id="8513b-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="8513b-136">En el caso del método ActionLink, el primer parámetro es el texto que se va a mostrar en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="8513b-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="8513b-137">El segundo parámetro es la acción y el tercer parámetro es el nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="8513b-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="8513b-138">Por ejemplo, el primer vínculo apunta a la acción de índice en StudentsController.</span><span class="sxs-lookup"><span data-stu-id="8513b-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="8513b-139">El hipervínculo real se construye a partir de estos valores.</span><span class="sxs-lookup"><span data-stu-id="8513b-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="8513b-140">En última instancia, el primer vínculo lleva a los usuarios al archivo **index. cshtml** dentro de la carpeta **views/Students** .</span><span class="sxs-lookup"><span data-stu-id="8513b-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="8513b-141">Mostrar vistas de estudiante</span><span class="sxs-lookup"><span data-stu-id="8513b-141">Display student views</span></span>

<span data-ttu-id="8513b-142">Comprobará que el código agregado al proyecto muestra correctamente una lista de los alumnos y permite a los usuarios editar, crear o eliminar los registros de estudiante en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="8513b-143">Haga clic con el botón derecho en el archivo **views** > **Home** > *index. cshtml* y seleccione **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="8513b-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="8513b-144">En la Página principal de la aplicación, seleccione **lista de alumnos**.</span><span class="sxs-lookup"><span data-stu-id="8513b-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="8513b-145">En la página **Índice** , observe la lista de estudiantes y vínculos para modificar estos datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="8513b-146">Seleccione el vínculo **crear nuevo** y proporcione algunos valores para un estudiante nuevo.</span><span class="sxs-lookup"><span data-stu-id="8513b-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="8513b-147">Haga clic en **crear**y observe que el nuevo estudiante se agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="8513b-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="8513b-148">De nuevo en la página de **Índice** , seleccione el vínculo **Editar** y cambie algunos de los valores de un estudiante.</span><span class="sxs-lookup"><span data-stu-id="8513b-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="8513b-149">Haga clic en **Guardar**y observe que se ha cambiado el registro del alumno.</span><span class="sxs-lookup"><span data-stu-id="8513b-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="8513b-150">Por último, seleccione el vínculo **eliminar** y confirme que desea eliminar el registro haciendo clic en el botón **eliminar** .</span><span class="sxs-lookup"><span data-stu-id="8513b-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="8513b-151">Sin escribir código, se han agregado vistas que realizan operaciones comunes en los datos de la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="8513b-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="8513b-152">Es posible que haya observado que la etiqueta de texto de un campo se basa en la propiedad de base de datos (como **LastName**), que no es necesariamente lo que desea mostrar en la Página Web.</span><span class="sxs-lookup"><span data-stu-id="8513b-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="8513b-153">Por ejemplo, puede que prefiera que la etiqueta sea **Last Name**.</span><span class="sxs-lookup"><span data-stu-id="8513b-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="8513b-154">Corregirá este problema de presentación más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="8513b-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="8513b-155">Mostrar vistas de inscripción</span><span class="sxs-lookup"><span data-stu-id="8513b-155">Display enrollment views</span></span>

<span data-ttu-id="8513b-156">La base de datos incluye una relación de uno a varios entre las tablas Student e Enrollment y una relación uno a varios entre las tablas Course e Enrollment.</span><span class="sxs-lookup"><span data-stu-id="8513b-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="8513b-157">Las vistas para la inscripción controlan correctamente estas relaciones.</span><span class="sxs-lookup"><span data-stu-id="8513b-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="8513b-158">Vaya a la Página principal del sitio y seleccione el vínculo **lista de inscripciones** y, a continuación, el vínculo **crear nuevo** .</span><span class="sxs-lookup"><span data-stu-id="8513b-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="8513b-159">La vista muestra un formulario para crear un nuevo registro de inscripción.</span><span class="sxs-lookup"><span data-stu-id="8513b-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="8513b-160">En concreto, tenga en cuenta que el formulario contiene una lista desplegable **CourseID** y una lista desplegable **StudentID** .</span><span class="sxs-lookup"><span data-stu-id="8513b-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="8513b-161">Ambos se rellenan con valores de las tablas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="8513b-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="8513b-162">Además, la validación de los valores proporcionados se aplica automáticamente en función del tipo de datos del campo.</span><span class="sxs-lookup"><span data-stu-id="8513b-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="8513b-163">La **calificación** requiere un número, por lo que se muestra un mensaje de error si se intenta proporcionar un valor incompatible: *el campo grado debe ser un número.*</span><span class="sxs-lookup"><span data-stu-id="8513b-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="8513b-164">Ha comprobado que las vistas generadas automáticamente permiten a los usuarios trabajar con los datos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="8513b-165">En el siguiente tutorial de esta serie, actualizará la base de datos y realizará los cambios correspondientes en la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="8513b-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8513b-166">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="8513b-166">Next steps</span></span>

<span data-ttu-id="8513b-167">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="8513b-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8513b-168">Scaffolding agregado</span><span class="sxs-lookup"><span data-stu-id="8513b-168">Added scaffold</span></span>
> * <span data-ttu-id="8513b-169">Vínculos agregados a nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="8513b-169">Added links to new views</span></span>
> * <span data-ttu-id="8513b-170">Vistas de estudiante mostradas</span><span class="sxs-lookup"><span data-stu-id="8513b-170">Displayed student views</span></span>
> * <span data-ttu-id="8513b-171">Vistas de inscripción mostradas</span><span class="sxs-lookup"><span data-stu-id="8513b-171">Displayed enrollment views</span></span>

<span data-ttu-id="8513b-172">Avance al siguiente tutorial para obtener información sobre cómo cambiar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8513b-172">Advance to the next tutorial to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8513b-173">Cambio de la base de datos</span><span class="sxs-lookup"><span data-stu-id="8513b-173">Change the database</span></span>](changing-the-database.md)