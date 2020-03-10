---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: personalización de la vista para EF Database First con la aplicación ASP.NET MVC'
description: Este tutorial se centra en cambiar las vistas generadas automáticamente para mejorar la presentación.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471523"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="52bd3-103">Tutorial: personalización de la vista para EF Database First con la aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="52bd3-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="52bd3-104">Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="52bd3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="52bd3-105">En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="52bd3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="52bd3-106">El código generado corresponde a las columnas de la tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="52bd3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="52bd3-107">Este tutorial se centra en cambiar las vistas generadas automáticamente para mejorar la presentación.</span><span class="sxs-lookup"><span data-stu-id="52bd3-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="52bd3-108">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="52bd3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52bd3-109">Agregar cursos a la página de detalles de estudiante</span><span class="sxs-lookup"><span data-stu-id="52bd3-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="52bd3-110">Confirmar que los cursos se agregan a la página</span><span class="sxs-lookup"><span data-stu-id="52bd3-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52bd3-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="52bd3-111">Prerequisites</span></span>

* [<span data-ttu-id="52bd3-112">Cambio de la base de datos</span><span class="sxs-lookup"><span data-stu-id="52bd3-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="52bd3-113">Agregar cursos a los detalles de estudiante</span><span class="sxs-lookup"><span data-stu-id="52bd3-113">Add courses to student detail</span></span>

<span data-ttu-id="52bd3-114">El código generado proporciona un buen punto de partida para la aplicación, pero no proporciona necesariamente toda la funcionalidad que necesita en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52bd3-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="52bd3-115">Puede personalizar el código para satisfacer los requisitos específicos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52bd3-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="52bd3-116">Actualmente, la aplicación no muestra los cursos inscritos del estudiante seleccionado.</span><span class="sxs-lookup"><span data-stu-id="52bd3-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="52bd3-117">En esta sección, agregará los cursos inscritos de cada estudiante a la vista de **detalles** del alumno.</span><span class="sxs-lookup"><span data-stu-id="52bd3-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="52bd3-118">Abra **vistas** > **estudiantes** > *details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="52bd3-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="52bd3-119">Debajo de la última &lt;etiqueta de&gt;/DL, pero antes de la etiqueta de cierre &lt;/div&gt;, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="52bd3-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="52bd3-120">Este código crea una tabla que muestra una fila para cada registro de la tabla de inscripción del alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="52bd3-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="52bd3-121">El método de **visualización** representa el código HTML para el objeto (modelItem) que representa la expresión.</span><span class="sxs-lookup"><span data-stu-id="52bd3-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="52bd3-122">Use el método display (en lugar de simplemente incrustar el valor de propiedad en el código) para asegurarse de que el valor tiene el formato correcto según su tipo y la plantilla de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="52bd3-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="52bd3-123">En este ejemplo, cada expresión devuelve una única propiedad del registro actual del bucle y los valores son tipos primitivos que se representan como texto.</span><span class="sxs-lookup"><span data-stu-id="52bd3-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="52bd3-124">Confirmar que se han agregado cursos</span><span class="sxs-lookup"><span data-stu-id="52bd3-124">Confirm courses are added</span></span>

<span data-ttu-id="52bd3-125">Ejecute la solución.</span><span class="sxs-lookup"><span data-stu-id="52bd3-125">Run the solution.</span></span> <span data-ttu-id="52bd3-126">Haga clic en **lista de estudiantes** y seleccione **detalles** para uno de los alumnos.</span><span class="sxs-lookup"><span data-stu-id="52bd3-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="52bd3-127">Verá que los cursos inscritos se han incluido en la vista.</span><span class="sxs-lookup"><span data-stu-id="52bd3-127">You will see the enrolled courses have been included in the view.</span></span>

![estudiante con inscripción](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="52bd3-129">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="52bd3-129">Next steps</span></span>
<span data-ttu-id="52bd3-130">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="52bd3-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52bd3-131">Cursos agregados a la página de detalles de estudiante</span><span class="sxs-lookup"><span data-stu-id="52bd3-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="52bd3-132">Confirmado que los cursos se han agregado a la página</span><span class="sxs-lookup"><span data-stu-id="52bd3-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="52bd3-133">Avance al siguiente tutorial para aprender a agregar anotaciones de datos para especificar los requisitos de validación y el formato de presentación.</span><span class="sxs-lookup"><span data-stu-id="52bd3-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="52bd3-134">Mejorar la validación de datos</span><span class="sxs-lookup"><span data-stu-id="52bd3-134">Enhance data validation</span></span>](enhancing-data-validation.md)
