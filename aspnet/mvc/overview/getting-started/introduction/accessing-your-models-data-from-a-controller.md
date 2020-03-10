---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499231"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="51a71-102">Obtener acceso a los datos del modelo desde un controlador</span><span class="sxs-lookup"><span data-stu-id="51a71-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="51a71-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51a71-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="51a71-104">En esta sección, creará una nueva clase de `MoviesController` y escribirá código que recupera los datos de la película y los muestra en el explorador con una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="51a71-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="51a71-105">**Compile la aplicación** antes de pasar al paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="51a71-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="51a71-106">Si no compila la aplicación, obtendrá un error al agregar un controlador.</span><span class="sxs-lookup"><span data-stu-id="51a71-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="51a71-107">En Explorador de soluciones, haga clic con el botón secundario en la carpeta *Controladores* y, a continuación, haga clic en **Agregar**y luego en **controlador**.</span><span class="sxs-lookup"><span data-stu-id="51a71-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="51a71-108">En el cuadro de diálogo **Agregar scaffold** , haga clic en **controlador de MVC 5 con vistas, utilizando Entity Framework**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="51a71-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="51a71-109">Seleccione **película (MvcMovie. Models)** para la clase modelo.</span><span class="sxs-lookup"><span data-stu-id="51a71-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="51a71-110">Seleccione **MovieDBContext (MvcMovie. Models)** para la clase de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="51a71-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="51a71-111">En el nombre del controlador, escriba **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="51a71-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="51a71-112">En la imagen siguiente se muestra el cuadro de diálogo completado.</span><span class="sxs-lookup"><span data-stu-id="51a71-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="51a71-113">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="51a71-113">Click **Add**.</span></span> <span data-ttu-id="51a71-114">(Si recibe un error, probablemente no haya compilado la aplicación antes de empezar a agregar el controlador). Visual Studio crea los siguientes archivos y carpetas:</span><span class="sxs-lookup"><span data-stu-id="51a71-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="51a71-115">*Un archivo MoviesController.CS* en la carpeta *Controllers* .</span><span class="sxs-lookup"><span data-stu-id="51a71-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="51a71-116">Una carpeta *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="51a71-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="51a71-117">*Cree. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* en la nueva carpeta *Views\Movies*</span><span class="sxs-lookup"><span data-stu-id="51a71-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="51a71-118">Visual Studio crea automáticamente las vistas y los métodos de acción [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) automáticamente (la creación automática de métodos y vistas de acción CRUD se conoce como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="51a71-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="51a71-119">Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar entradas de películas.</span><span class="sxs-lookup"><span data-stu-id="51a71-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="51a71-120">Ejecute la aplicación y haga clic en el vínculo de **película de MVC** (o vaya al controlador de `Movies` anexando */Movies* a la dirección URL en la barra de direcciones del explorador).</span><span class="sxs-lookup"><span data-stu-id="51a71-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="51a71-121">Dado que la aplicación se basa en el enrutamiento predeterminado (definido en el archivo Start\RouteConfig.cs de la *aplicación\_* ), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta al método de acción de `Index` predeterminado del controlador de `Movies`.</span><span class="sxs-lookup"><span data-stu-id="51a71-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="51a71-122">En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es realmente la misma que la solicitud del explorador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="51a71-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="51a71-123">El resultado es una lista vacía de películas, ya que aún no ha agregado ninguna.</span><span class="sxs-lookup"><span data-stu-id="51a71-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="51a71-124">Crear una película</span><span class="sxs-lookup"><span data-stu-id="51a71-124">Creating a Movie</span></span>

<span data-ttu-id="51a71-125">Seleccione el vínculo **Crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="51a71-125">Select the **Create New** link.</span></span> <span data-ttu-id="51a71-126">Escriba algunos detalles sobre una película y, a continuación, haga clic en el botón **crear** .</span><span class="sxs-lookup"><span data-stu-id="51a71-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="51a71-127">Es posible que no pueda escribir comas o puntos decimales en el campo precio.</span><span class="sxs-lookup"><span data-stu-id="51a71-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="51a71-128">para admitir la validación de jQuery para configuraciones regionales distintas del inglés que usan una coma (&quot;,&quot;) para un separador decimal y formatos de fecha que no son de Inglés de EE. UU., debe incluir *Globalization. js* y el `Globalize.parseFloat`[https://github.com/jquery/globalize](https://github.com/jquery/globalize) archivo *culturas. culturas. js* .</span><span class="sxs-lookup"><span data-stu-id="51a71-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="51a71-129">Mostraré cómo hacerlo en el siguiente tutorial.</span><span class="sxs-lookup"><span data-stu-id="51a71-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="51a71-130">Por ahora, tan solo debe escribir números enteros como 10.</span><span class="sxs-lookup"><span data-stu-id="51a71-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="51a71-131">Al hacer clic en el botón **crear** , el formulario se envía al servidor, donde se guarda la información de la película en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="51a71-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="51a71-132">A continuación, se le redirigirá a la dirección URL de */Movies* , donde podrá ver la película recién creada en la lista.</span><span class="sxs-lookup"><span data-stu-id="51a71-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="51a71-133">Cree un par de entradas más de película.</span><span class="sxs-lookup"><span data-stu-id="51a71-133">Create a couple more movie entries.</span></span> <span data-ttu-id="51a71-134">Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.</span><span class="sxs-lookup"><span data-stu-id="51a71-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="51a71-135">Examinar el código generado</span><span class="sxs-lookup"><span data-stu-id="51a71-135">Examining the Generated Code</span></span>

<span data-ttu-id="51a71-136">Abra el archivo *Controllers\MoviesController.CS* y examine el método de `Index` generado.</span><span class="sxs-lookup"><span data-stu-id="51a71-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="51a71-137">A continuación se muestra una parte del controlador de película con el método `Index`.</span><span class="sxs-lookup"><span data-stu-id="51a71-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="51a71-138">Una solicitud al controlador de `Movies` devuelve todas las entradas de la tabla `Movies` y, a continuación, pasa los resultados a la vista `Index`.</span><span class="sxs-lookup"><span data-stu-id="51a71-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="51a71-139">La siguiente línea de la clase `MoviesController` crea una instancia de un contexto de la base de datos Movie, tal como se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="51a71-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="51a71-140">Puede usar el contexto de la base de datos de películas para consultar, editar y eliminar películas.</span><span class="sxs-lookup"><span data-stu-id="51a71-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="51a71-141">Modelos fuertemente tipados y la palabra clave @model</span><span class="sxs-lookup"><span data-stu-id="51a71-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="51a71-142">Anteriormente en este tutorial, vio cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante el objeto `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="51a71-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="51a71-143">La `ViewBag` es un objeto dinámico que proporciona una cómoda manera de enlazar información a una vista.</span><span class="sxs-lookup"><span data-stu-id="51a71-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="51a71-144">MVC también proporciona la capacidad de pasar objetos *fuertemente tipados* a una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="51a71-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="51a71-145">Este enfoque fuertemente tipado permite una mejor comprobación en tiempo de compilación del código y [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) más enriquecida en el editor de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51a71-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="51a71-146">El mecanismo de scaffolding de Visual Studio usó este enfoque (es decir, pasando un modelo *fuertemente tipado* ) con la clase `MoviesController` y las plantillas de vista cuando creó los métodos y las vistas.</span><span class="sxs-lookup"><span data-stu-id="51a71-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="51a71-147">En el archivo *Controllers\MoviesController.CS* , examine el método de `Details` generado.</span><span class="sxs-lookup"><span data-stu-id="51a71-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="51a71-148">A continuación se muestra el método `Details`.</span><span class="sxs-lookup"><span data-stu-id="51a71-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="51a71-149">Normalmente, el parámetro `id` se pasa como datos de ruta, por ejemplo `http://localhost:1234/movies/details/1` establecerá el controlador en el controlador de película, la acción que se va a `details` y la `id` en 1.</span><span class="sxs-lookup"><span data-stu-id="51a71-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="51a71-150">También puede pasar el identificador con una cadena de consulta como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="51a71-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="51a71-151">Si se encuentra un `Movie`, se pasa una instancia del modelo de `Movie` a la vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="51a71-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="51a71-152">Examine el contenido del archivo *Views\Movies\Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="51a71-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="51a71-153">Al incluir una instrucción `@model` en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera la vista.</span><span class="sxs-lookup"><span data-stu-id="51a71-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="51a71-154">Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="51a71-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="51a71-155">Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="51a71-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="51a71-156">Por ejemplo, en la plantilla *details. cshtml* , el código pasa cada campo de película a las aplicaciones auxiliares HTML `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con el objeto `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="51a71-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="51a71-157">Los métodos `Create` y `Edit` y las plantillas de vista también pasan un objeto de modelo de película.</span><span class="sxs-lookup"><span data-stu-id="51a71-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="51a71-158">Examine la plantilla de vista *index. cshtml* y el método `Index` en el archivo *MoviesController.CS* .</span><span class="sxs-lookup"><span data-stu-id="51a71-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="51a71-159">Observe cómo el código crea un objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) cuando llama al método auxiliar `View` en el método de acción `Index`.</span><span class="sxs-lookup"><span data-stu-id="51a71-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="51a71-160">A continuación, el código pasa esta lista de `Movies` desde el método de acción `Index` a la vista:</span><span class="sxs-lookup"><span data-stu-id="51a71-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="51a71-161">Al crear el controlador de película, Visual Studio incluye automáticamente la siguiente instrucción `@model` en la parte superior del archivo *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="51a71-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="51a71-162">Esta directiva de `@model` permite tener acceso a la lista de películas que el controlador pasó a la vista mediante un objeto de `Model` fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="51a71-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="51a71-163">Por ejemplo, en la plantilla *index. cshtml* , el código recorre en bucle las películas mediante una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:</span><span class="sxs-lookup"><span data-stu-id="51a71-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="51a71-164">Dado que el objeto `Model` está fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada `item` objeto del bucle se escribe como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="51a71-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="51a71-165">Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y la compatibilidad completa con IntelliSense en el editor de código:</span><span class="sxs-lookup"><span data-stu-id="51a71-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="51a71-167">Trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="51a71-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="51a71-168">Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó apuntaba a una base de datos de `Movies` que todavía no existía, por lo que Code First creó la base de datos automáticamente.</span><span class="sxs-lookup"><span data-stu-id="51a71-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="51a71-169">Para comprobar que se ha creado, busque en la carpeta *App\_Data* .</span><span class="sxs-lookup"><span data-stu-id="51a71-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="51a71-170">Si no ve el archivo *movies. MDF* , haga clic en el botón **Mostrar todos los archivos** en la barra de herramientas **Explorador de soluciones** , haga clic en el botón **Actualizar** y, a continuación, expanda la carpeta *App\_Data* .</span><span class="sxs-lookup"><span data-stu-id="51a71-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="51a71-171">Haga doble clic en *movies. MDF* para abrir el **Explorador de servidores**y, a continuación, expanda la carpeta **tablas** para ver la tabla películas.</span><span class="sxs-lookup"><span data-stu-id="51a71-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="51a71-172">Observe el icono de llave junto a ID.</span><span class="sxs-lookup"><span data-stu-id="51a71-172">Note the key icon next to ID.</span></span> <span data-ttu-id="51a71-173">De forma predeterminada, EF convertirá una propiedad denominada ID en la clave principal.</span><span class="sxs-lookup"><span data-stu-id="51a71-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="51a71-174">Para obtener más información sobre EF y MVC, consulte el excelente tutorial de Tom Dykstra sobre [MVC y EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="51a71-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="51a71-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="51a71-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="51a71-176">Haga clic con el botón secundario en la tabla `Movies` y seleccione **Mostrar datos de tabla** para ver los datos que ha creado.</span><span class="sxs-lookup"><span data-stu-id="51a71-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="51a71-177">Haga clic con el botón secundario en la tabla `Movies` y seleccione **abrir definición de tabla** para ver la estructura de la tabla que Entity Framework Code First crear automáticamente.</span><span class="sxs-lookup"><span data-stu-id="51a71-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="51a71-178">Observe cómo se asigna el esquema de la tabla `Movies` a la clase `Movie` que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="51a71-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="51a71-179">Entity Framework Code First crea automáticamente este esquema en función de la clase `Movie`.</span><span class="sxs-lookup"><span data-stu-id="51a71-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="51a71-180">Cuando haya terminado, cierre la conexión haciendo clic con el botón secundario en *MovieDBContext* y seleccionando **cerrar conexión**.</span><span class="sxs-lookup"><span data-stu-id="51a71-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="51a71-181">(Si no cierra la conexión, es posible que reciba un error la próxima vez que ejecute el proyecto).</span><span class="sxs-lookup"><span data-stu-id="51a71-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="51a71-182">Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos.</span><span class="sxs-lookup"><span data-stu-id="51a71-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="51a71-183">En el siguiente tutorial, examinaremos el resto del código con scaffolding y agregaremos un método `SearchIndex` y una vista `SearchIndex` que le permita buscar películas en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="51a71-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="51a71-184">Para obtener más información sobre el uso de Entity Framework con MVC, vea [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="51a71-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51a71-185">[Anterior](creating-a-connection-string.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="51a71-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
