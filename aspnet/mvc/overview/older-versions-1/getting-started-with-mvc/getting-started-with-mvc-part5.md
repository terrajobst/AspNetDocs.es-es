---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437371"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="9f2d4-104">Obtener acceso a los datos del modelo desde un controlador</span><span class="sxs-lookup"><span data-stu-id="9f2d4-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="9f2d4-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="9f2d4-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="9f2d4-107">Creará una aplicación web simple que lee y escribe en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="9f2d4-108">Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="9f2d4-109">En esta sección vamos a crear una nueva clase MoviesController y escribiremos código que recupere los datos de la película y los mostrará de nuevo en el explorador con una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="9f2d4-110">Haga clic con el botón derecho en la carpeta Controllers y cree un nuevo MoviesController.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="9f2d4-111">[![agregar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="9f2d4-112">Se creará un nuevo archivo "MoviesController.cs" debajo de nuestra carpeta \Controllers dentro de nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="9f2d4-113">Vamos a actualizar MovieController para recuperar la lista de películas de nuestra base de datos recién rellenada.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="9f2d4-114">Vamos a realizar una consulta LINQ para que solo se recuperen las películas publicadas después del verano de 1984.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="9f2d4-115">Necesitaremos una plantilla de vista para volver a presentar esta lista de películas, así que haga clic con el botón derecho en el método y seleccione Agregar vista para crearla.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="9f2d4-116">En el cuadro de diálogo Agregar vista, se indicará que se está pasando una lista&lt;películas. Models. Movie&gt; a nuestra plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="9f2d4-117">A diferencia de las horas anteriores usamos el cuadro de diálogo Agregar vista y eligió crear una plantilla "vacía", esta vez indicaremos que queremos que Visual Studio "scaffolding" automáticamente sea una plantilla de vista para nosotros con algún contenido predeterminado.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="9f2d4-118">Haremos esto seleccionando el elemento "list" en el menú desplegable de vista de contenido.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="9f2d4-119">Recuerde que si ha creado una nueva clase, deberá compilar la aplicación para que se muestre en el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Agregar vista](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="9f2d4-121">Haga clic en agregar y el sistema generará automáticamente el código de una vista que muestra nuestra lista de películas.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="9f2d4-122">Es un buen momento para cambiar el encabezado &lt;H2&gt; a algo parecido a "mi lista de películas", como hicimos anteriormente con la vista Hola mundo.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="9f2d4-123">[Películas de ![: Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="9f2d4-124">Ejecute la aplicación y visite/Movies en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="9f2d4-125">Ahora se han recuperado los datos de la base de datos mediante una consulta básica en el controlador y se han devuelto los datos a una vista que conoce las películas.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="9f2d4-126">A continuación, esta vista gira a través de la lista de películas y crea una tabla de datos para nosotros.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="9f2d4-127">[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="9f2d4-128">No vamos a implementar la funcionalidad de edición, detalles y eliminación con esta aplicación, por lo que no necesitamos los vínculos predeterminados creados por nosotros.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="9f2d4-129">Abra el archivo/Movies/Index.aspx y quítelo.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="9f2d4-130">Este es el código fuente del aspecto que debería tener nuestra plantilla de vista actualizada una vez que realizamos estos cambios:</span><span class="sxs-lookup"><span data-stu-id="9f2d4-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="9f2d4-131">Está creando vínculos que no necesitamos, por lo que los eliminaremos en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="9f2d4-132">Sin embargo, conservaremos el vínculo crear nuevo, tal y como eso es próximo.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="9f2d4-133">Este es el aspecto de la aplicación con esa columna quitada.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="9f2d4-134">[![lista de películas: Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="9f2d4-135">Ahora tenemos una lista simple de los datos de la película.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="9f2d4-136">Sin embargo, si hacemos clic en el vínculo "crear nuevo", se producirá un error porque no está enlazado.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="9f2d4-137">Vamos a implementar un método de acción Create y permitir que un usuario escriba nuevas películas en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="9f2d4-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9f2d4-138">[Anterior](getting-started-with-mvc-part4.md)
> [Siguiente](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="9f2d4-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
