---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: cambio de la base de datos de EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en hacer una actualización de la estructura de la base de datos y propagar ese cambio en toda la aplicación Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499525"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="be2a0-103">Tutorial: cambio de la base de datos de EF Database First con la aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="be2a0-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="be2a0-104">Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="be2a0-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="be2a0-105">En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="be2a0-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="be2a0-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="be2a0-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="be2a0-107">Este tutorial se centra en hacer una actualización de la estructura de la base de datos y propagar ese cambio en toda la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="be2a0-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="be2a0-108">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="be2a0-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be2a0-109">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="be2a0-109">Add a column</span></span>
> * <span data-ttu-id="be2a0-110">Agregar la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="be2a0-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be2a0-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="be2a0-111">Prerequisites</span></span>

* [<span data-ttu-id="be2a0-112">Generar vistas</span><span class="sxs-lookup"><span data-stu-id="be2a0-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="be2a0-113">Agregar una columna</span><span class="sxs-lookup"><span data-stu-id="be2a0-113">Add a column</span></span>

<span data-ttu-id="be2a0-114">Si actualiza la estructura de una tabla en la base de datos, debe asegurarse de que el cambio se propague al modelo de datos, las vistas y el controlador.</span><span class="sxs-lookup"><span data-stu-id="be2a0-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="be2a0-115">En este tutorial, agregará una nueva columna a la tabla Student para registrar el segundo nombre del estudiante.</span><span class="sxs-lookup"><span data-stu-id="be2a0-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="be2a0-116">Para agregar esta columna, abra el proyecto de base de datos y abra el archivo Student. SQL.</span><span class="sxs-lookup"><span data-stu-id="be2a0-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="be2a0-117">A través del diseñador o del código T-SQL, agregue una columna denominada **MiddleName** que sea un nvarchar (50) y admita valores NULL.</span><span class="sxs-lookup"><span data-stu-id="be2a0-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="be2a0-118">Implemente este cambio en la base de datos local. para ello, inicie el proyecto de base de datos (o F5).</span><span class="sxs-lookup"><span data-stu-id="be2a0-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="be2a0-119">El nuevo campo se agrega a la tabla.</span><span class="sxs-lookup"><span data-stu-id="be2a0-119">The new field is added to the table.</span></span> <span data-ttu-id="be2a0-120">Si no lo ve en el Explorador de objetos de SQL Server, haga clic en el botón actualizar en el panel.</span><span class="sxs-lookup"><span data-stu-id="be2a0-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar nueva columna](changing-the-database/_static/image2.png)

<span data-ttu-id="be2a0-122">La nueva columna existe en la tabla de base de datos, pero no existe actualmente en la clase de modelo de datos.</span><span class="sxs-lookup"><span data-stu-id="be2a0-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="be2a0-123">Debe actualizar el modelo para incluir la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="be2a0-123">You must update the model to include your new column.</span></span> <span data-ttu-id="be2a0-124">En la carpeta **Models** , abra el archivo **ContosoModel. edmx** para mostrar el diagrama del modelo.</span><span class="sxs-lookup"><span data-stu-id="be2a0-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="be2a0-125">Observe que el modelo Student no contiene la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="be2a0-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="be2a0-126">Haga clic con el botón secundario en cualquier parte de la superficie de diseño y seleccione **Actualizar modelo desde base de datos**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="be2a0-127">En el Asistente para actualización, seleccione la pestaña **Actualizar** y, a continuación, seleccione **tablas** > **DBO** > **Student**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="be2a0-128">Haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-128">Click **Finish**.</span></span>

<span data-ttu-id="be2a0-129">Una vez finalizado el proceso de actualización, el diagrama de base de datos incluye la nueva propiedad **MiddleName** .</span><span class="sxs-lookup"><span data-stu-id="be2a0-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="be2a0-130">Guarde el archivo **ContosoModel. edmx** .</span><span class="sxs-lookup"><span data-stu-id="be2a0-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="be2a0-131">Debe guardar este archivo para que la nueva propiedad se propague a la clase **Student.CS** .</span><span class="sxs-lookup"><span data-stu-id="be2a0-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="be2a0-132">Ahora ha actualizado la base de datos y el modelo.</span><span class="sxs-lookup"><span data-stu-id="be2a0-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="be2a0-133">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="be2a0-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="be2a0-134">Agregar la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="be2a0-134">Add the property to the views</span></span>

<span data-ttu-id="be2a0-135">Desafortunadamente, las vistas todavía no contienen la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="be2a0-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="be2a0-136">Para actualizar las vistas tiene dos opciones: puede volver a generar las vistas mediante la adición de scaffolding para la clase Student, o bien puede Agregar manualmente la nueva propiedad a las vistas existentes.</span><span class="sxs-lookup"><span data-stu-id="be2a0-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="be2a0-137">En este tutorial, agregará el scaffolding de nuevo porque no ha realizado ningún cambio personalizado en las vistas generadas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="be2a0-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="be2a0-138">Puede considerar la posibilidad de agregar manualmente la propiedad cuando haya realizado cambios en las vistas y no desee perder los cambios.</span><span class="sxs-lookup"><span data-stu-id="be2a0-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="be2a0-139">Para asegurarse de que se vuelven a crear las vistas, elimine la carpeta **Students** en **views**y elimine **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="be2a0-140">A continuación, haga clic con el botón secundario en la carpeta **Controllers** y agregue scaffolding para el modelo **Student** .</span><span class="sxs-lookup"><span data-stu-id="be2a0-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="be2a0-141">De nuevo, asigne al controlador el nombre **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="be2a0-142">Seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="be2a0-142">Select **Add**.</span></span>

<span data-ttu-id="be2a0-143">Vuelva a compilar la solución.</span><span class="sxs-lookup"><span data-stu-id="be2a0-143">Build the solution again.</span></span> <span data-ttu-id="be2a0-144">Las vistas contienen ahora la propiedad MiddleName.</span><span class="sxs-lookup"><span data-stu-id="be2a0-144">The views now contain the MiddleName property.</span></span>

![Mostrar segundo nombre](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="be2a0-146">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="be2a0-146">Next steps</span></span>

<span data-ttu-id="be2a0-147">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="be2a0-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="be2a0-148">Se ha agregado una columna</span><span class="sxs-lookup"><span data-stu-id="be2a0-148">Added a column</span></span>
> * <span data-ttu-id="be2a0-149">Se ha agregado la propiedad a las vistas</span><span class="sxs-lookup"><span data-stu-id="be2a0-149">Added the property to the views</span></span>

<span data-ttu-id="be2a0-150">Avance al siguiente tutorial para aprender a personalizar la vista para mostrar los detalles de un registro de estudiante.</span><span class="sxs-lookup"><span data-stu-id="be2a0-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="be2a0-151">Personalización de una vista</span><span class="sxs-lookup"><span data-stu-id="be2a0-151">Customize a view</span></span>](customizing-a-view.md)