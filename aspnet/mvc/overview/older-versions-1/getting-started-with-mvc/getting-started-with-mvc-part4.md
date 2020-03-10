---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Crear una base de datos | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469675"
---
# <a name="creating-a-database"></a><span data-ttu-id="2135d-104">Crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="2135d-104">Creating a Database</span></span>

<span data-ttu-id="2135d-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="2135d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="2135d-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2135d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="2135d-107">Creará una aplicación web simple que lee y escribe en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="2135d-108">Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2135d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="2135d-109">En esta sección vamos a crear una nueva base de datos de SQL Express que usaremos para almacenar y recuperar los datos de la película.</span><span class="sxs-lookup"><span data-stu-id="2135d-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="2135d-110">En el IDE de Visual Web Developer, seleccione Vista | Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="2135d-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="2135d-111">Haga clic con el botón derecho en conexiones de datos y haga clic en agregar conexión...</span><span class="sxs-lookup"><span data-stu-id="2135d-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="2135d-113">En el cuadro de diálogo elegir origen de datos, seleccione Microsoft SQL Server y seleccione continuar.</span><span class="sxs-lookup"><span data-stu-id="2135d-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="2135d-114">En el cuadro de diálogo Agregar conexión, escriba ".\SQLEXPRESS" como nombre del servidor y escriba "Movies" como nombre de la nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="2135d-115">[![cuadro de diálogo Agregar conexión](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="2135d-116">Haga clic en aceptar y se le preguntará si desea crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="2135d-117">Seleccione Sí.</span><span class="sxs-lookup"><span data-stu-id="2135d-117">Select yes.</span></span>

<span data-ttu-id="2135d-118">[¿![crear películas?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="2135d-119">Ahora tiene una base de datos vacía en Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="2135d-119">Now you've got an empty database in Server Explorer.</span></span>

![Agregar nueva tabla](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="2135d-121">Haga clic con el botón derecho en tablas y haga clic en agregar tabla.</span><span class="sxs-lookup"><span data-stu-id="2135d-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="2135d-122">Aparecerá el Diseñador de tablas.</span><span class="sxs-lookup"><span data-stu-id="2135d-122">The Table Designer will appear.</span></span> <span data-ttu-id="2135d-123">Agregue columnas para ID, title, ReleaseDate, Genre y price.</span><span class="sxs-lookup"><span data-stu-id="2135d-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="2135d-124">Haga clic con el botón derecho en la columna ID y haga clic en establecer clave principal.</span><span class="sxs-lookup"><span data-stu-id="2135d-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="2135d-125">Este es el aspecto de mis áreas de diseño.</span><span class="sxs-lookup"><span data-stu-id="2135d-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="2135d-126">[Editor de tablas de ![base de datos](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="2135d-127">Además, seleccione la columna ID. y, en propiedades de columna, cambie "Especificación de identidad" a "sí".</span><span class="sxs-lookup"><span data-stu-id="2135d-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="2135d-128">[![IsIdentity-propiedades de la columna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="2135d-129">Cuando haya terminado, haga clic en el icono guardar en la barra de herramientas o seleccione Archivo | Guarde en el menú y asigne un nombre a la tabla "**Movie**" (singular).</span><span class="sxs-lookup"><span data-stu-id="2135d-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="2135d-130">Tenemos una base de datos y una tabla.</span><span class="sxs-lookup"><span data-stu-id="2135d-130">We've got a database and a table!</span></span>

<span data-ttu-id="2135d-131">[![elegir nombre](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="2135d-132">Vuelva a Explorador de servidores, haga clic con el botón secundario en la tabla de películas y seleccione "Mostrar datos de tabla".</span><span class="sxs-lookup"><span data-stu-id="2135d-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="2135d-133">Escriba algunas películas para que nuestra base de datos tenga algunos datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="2135d-134">[![edición de tabla de base de datos](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="2135d-135">Creación de un modelo</span><span class="sxs-lookup"><span data-stu-id="2135d-135">Creating a Model</span></span>

<span data-ttu-id="2135d-136">Ahora, vuelva a la Explorador de soluciones en el lado derecho del IDE y haga clic con el botón derecho en la carpeta modelos y seleccione Agregar | Nuevo elemento.</span><span class="sxs-lookup"><span data-stu-id="2135d-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="2135d-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="2135d-138">Vamos a crear un modelo de entidad a partir de la nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="2135d-139">Esto agregará un conjunto de clases a nuestro proyecto que facilitan la consulta y manipulación de los datos de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="2135d-140">Seleccione el nodo de datos en el lado izquierdo del cuadro de diálogo y, a continuación, seleccione la plantilla ADO.NET Entity Data Model Item.</span><span class="sxs-lookup"><span data-stu-id="2135d-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="2135d-141">Asígnele el nombre movies. edmx.</span><span class="sxs-lookup"><span data-stu-id="2135d-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="2135d-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="2135d-143">Haga clic en el botón "Agregar".</span><span class="sxs-lookup"><span data-stu-id="2135d-143">Click the "Add" button.</span></span> <span data-ttu-id="2135d-144">A continuación, se iniciará el "Asistente para Entity Data Model".</span><span class="sxs-lookup"><span data-stu-id="2135d-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="2135d-145">En el nuevo cuadro de diálogo que aparece, seleccione generar desde la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2135d-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="2135d-146">Dado que se acaba de crear una base de datos, solo hay que indicar al Entity Framework sobre la nueva base de datos y su tabla.</span><span class="sxs-lookup"><span data-stu-id="2135d-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="2135d-147">Haga clic en siguiente para guardar la conexión de la base de datos en la configuración de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="2135d-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="2135d-148">Ahora, active la casilla tablas y películas y haga clic en finalizar.</span><span class="sxs-lookup"><span data-stu-id="2135d-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="2135d-149">[Asistente para Entity Data Model de ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="2135d-150">Ahora podemos ver la nueva tabla de películas en el Entity Framework Designer y acceder a ella desde el código.</span><span class="sxs-lookup"><span data-stu-id="2135d-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="2135d-151">[Películas de ![: Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="2135d-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="2135d-152">En la superficie de diseño, puede ver una clase "Movie".</span><span class="sxs-lookup"><span data-stu-id="2135d-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="2135d-153">Esta clase se asigna a la tabla "Movie" de nuestra base de datos y cada propiedad dentro de ella se asigna a una columna con la tabla.</span><span class="sxs-lookup"><span data-stu-id="2135d-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="2135d-154">Cada instancia de una clase "Movie" se corresponderá con una fila de la tabla "Movie".</span><span class="sxs-lookup"><span data-stu-id="2135d-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="2135d-155">Si no le gustan las convenciones de nomenclatura y asignación predeterminadas utilizadas por el Entity Framework, puede utilizar el diseñador de Entity Framework para cambiarlos o personalizarlos.</span><span class="sxs-lookup"><span data-stu-id="2135d-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="2135d-156">En esta aplicación se usarán los valores predeterminados y solo se guardará el archivo tal cual.</span><span class="sxs-lookup"><span data-stu-id="2135d-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="2135d-157">Ahora, vamos a trabajar con algunos datos reales.</span><span class="sxs-lookup"><span data-stu-id="2135d-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2135d-158">[Anterior](getting-started-with-mvc-part3.md)
> [Siguiente](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="2135d-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
