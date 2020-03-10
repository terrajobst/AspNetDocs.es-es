---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 1cf092c3db3959d6f47006f1be2ba82833c5dc06
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437227"
---
# <a name="adding-a-column-to-the-model"></a><span data-ttu-id="f2f69-104">Agregar una columna al modelo</span><span class="sxs-lookup"><span data-stu-id="f2f69-104">Adding a Column to the Model</span></span>

<span data-ttu-id="f2f69-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f2f69-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="f2f69-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2f69-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f2f69-107">Creará una aplicación web simple que lee y escribe en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f69-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f2f69-108">Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2f69-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="f2f69-109">En esta sección, vamos a examinar cómo podemos realizar cambios en el esquema de nuestra base de datos y controlar los cambios en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="f2f69-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="f2f69-110">Vamos a agregar una columna "rating" a la tabla Movie.</span><span class="sxs-lookup"><span data-stu-id="f2f69-110">Let's add a "Rating" Column to the Movie table.</span></span> <span data-ttu-id="f2f69-111">Vuelva al IDE y haga clic en el Explorador de bases de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f69-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="f2f69-112">Haga clic con el botón derecho en la tabla de películas y seleccione Abrir definición de tabla.</span><span class="sxs-lookup"><span data-stu-id="f2f69-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="f2f69-113">Agregue una columna "rating" como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="f2f69-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="f2f69-114">Dado que ahora no tenemos ninguna clasificación, la columna puede permitir valores NULL.</span><span class="sxs-lookup"><span data-stu-id="f2f69-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="f2f69-115">Haga clic en Guardar.</span><span class="sxs-lookup"><span data-stu-id="f2f69-115">Click Save.</span></span>

<span data-ttu-id="f2f69-116">[![tabla de películas de edición](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2f69-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="f2f69-117">A continuación, vuelva al Explorador de soluciones y abra el archivo movies. edmx (que se encuentra en la carpeta \Models).</span><span class="sxs-lookup"><span data-stu-id="f2f69-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="f2f69-118">Haga clic con el botón derecho en la superficie de diseño (el área en blanco) y seleccione Actualizar modelo desde base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f69-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="f2f69-119">[Películas de ![: Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f2f69-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="f2f69-120">Se iniciará el "Asistente para actualización".</span><span class="sxs-lookup"><span data-stu-id="f2f69-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="f2f69-121">Haga clic en la pestaña actualizar dentro de ella y haga clic en finalizar.</span><span class="sxs-lookup"><span data-stu-id="f2f69-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="f2f69-122">A continuación, nuestra clase de modelo de película se actualizará con la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="f2f69-122">Our Movie model class will then be updated with the new column.</span></span>

![Asistente para actualización (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="f2f69-124">Después de hacer clic en finalizar, puede ver que se ha agregado la nueva columna de clasificación a la entidad de película en nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="f2f69-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="f2f69-125">[![entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="f2f69-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="f2f69-126">Hemos agregado una columna en el modelo de base de datos, pero las vistas no lo saben.</span><span class="sxs-lookup"><span data-stu-id="f2f69-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="f2f69-127">Actualizar vistas con cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="f2f69-127">Update Views with Model Changes</span></span>

<span data-ttu-id="f2f69-128">Hay varias maneras de actualizar las plantillas de vista para que reflejen la nueva columna de clasificación.</span><span class="sxs-lookup"><span data-stu-id="f2f69-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="f2f69-129">Dado que creamos estas vistas mediante su generación mediante el cuadro de diálogo Agregar vista, podríamos eliminarlas y volver a crearlas.</span><span class="sxs-lookup"><span data-stu-id="f2f69-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="f2f69-130">Sin embargo, normalmente los usuarios ya habrán realizado modificaciones en sus plantillas de vista a partir de la generación con scaffolding inicial y querrán agregar o eliminar campos manualmente, tal y como hicimos con el campo de ID. para crear.</span><span class="sxs-lookup"><span data-stu-id="f2f69-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="f2f69-131">Abra la plantilla \Views\Movies\Index.aspx y agregue un &lt;&gt;clasificación&lt;/TH&gt; al encabezado de la tabla Movie.</span><span class="sxs-lookup"><span data-stu-id="f2f69-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="f2f69-132">Agregué el mío después del género.</span><span class="sxs-lookup"><span data-stu-id="f2f69-132">I added mine after Genre.</span></span> <span data-ttu-id="f2f69-133">A continuación, en la misma posición de la columna pero abajo, agregue una línea para generar la nueva clasificación.</span><span class="sxs-lookup"><span data-stu-id="f2f69-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="f2f69-134">Nuestra plantilla index. aspx final tendrá el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="f2f69-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="f2f69-135">A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregaremos una etiqueta y un cuadro de texto para la nueva propiedad Rating:</span><span class="sxs-lookup"><span data-stu-id="f2f69-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="f2f69-136">Nuestra plantilla final Create. aspx tendrá el siguiente aspecto y cambiaremos el título del explorador y el &lt;de la versión H2 del&gt; del título a algo parecido a "crear una película" mientras estamos aquí.</span><span class="sxs-lookup"><span data-stu-id="f2f69-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="f2f69-137">Ejecute la aplicación y ahora tiene un nuevo campo en la base de datos que se ha agregado a la página crear.</span><span class="sxs-lookup"><span data-stu-id="f2f69-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="f2f69-138">Agregue una nueva película (esta vez con una clasificación) y haga clic en crear.</span><span class="sxs-lookup"><span data-stu-id="f2f69-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="f2f69-139">[![crear una película (Windows Internet Explorer)](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="f2f69-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="f2f69-140">Después de hacer clic en crear, se le enviará a la página de índice donde se muestra la nueva película con la nueva columna clasificación en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f2f69-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="f2f69-141">[![lista de películas: Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f2f69-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="f2f69-142">En este tutorial básico se empezó a crear controladores y a asociarlos con vistas y pasar los datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="f2f69-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="f2f69-143">Después creamos y diseñamos una base de datos y colocamos algunos datos en ella.</span><span class="sxs-lookup"><span data-stu-id="f2f69-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="f2f69-144">Recuperamos los datos de la base de datos y los mostramos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="f2f69-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="f2f69-145">A continuación, se ha agregado un formulario de creación que permite al usuario agregar datos a la base de datos desde dentro de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="f2f69-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="f2f69-146">Hemos agregado la validación y, a continuación, hemos realizado la validación para usar JavaScript en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="f2f69-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="f2f69-147">Por último, hemos cambiado la base de datos para incluir una nueva columna de datos y, a continuación, hemos actualizado nuestras dos páginas para crear y mostrar estos datos nuevos.</span><span class="sxs-lookup"><span data-stu-id="f2f69-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="f2f69-148">Ahora le animamos a pasar a nuestro tutorial de nivel intermedio "[almacén de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como a muchos vídeos y recursos de [https://asp.net/mvc](https://asp.net/mvc) para obtener más información sobre ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="f2f69-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="f2f69-149">Disfrútelo.</span><span class="sxs-lookup"><span data-stu-id="f2f69-149">Enjoy!</span></span>

- <span data-ttu-id="f2f69-150">Scott Hanselman- [http://hanselman.com](http://hanselman.com) y [@shanselman](http://twitter.com/shanselman) en Twitter.</span><span class="sxs-lookup"><span data-stu-id="f2f69-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f2f69-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="f2f69-151">Previous</span></span>](getting-started-with-mvc-part7.md)
