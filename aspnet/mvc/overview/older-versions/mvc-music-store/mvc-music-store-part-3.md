---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: vistas y ViewModels | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 3 cubre las vistas y ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451135"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="629a1-104">Parte 3: vistas y ViewModels</span><span class="sxs-lookup"><span data-stu-id="629a1-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="629a1-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="629a1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="629a1-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="629a1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="629a1-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="629a1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="629a1-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="629a1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="629a1-109">La parte 3 cubre las vistas y ViewModels.</span><span class="sxs-lookup"><span data-stu-id="629a1-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="629a1-110">Hasta ahora hemos devuelto cadenas desde las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="629a1-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="629a1-111">Esta es una buena manera de hacerse una idea de cómo funcionan los controladores, pero no es cómo desea crear una aplicación web real.</span><span class="sxs-lookup"><span data-stu-id="629a1-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="629a1-112">Vamos a querer una manera mejor de volver a generar HTML en los exploradores que visitan nuestro sitio: uno en el que podemos usar archivos de plantilla para personalizar más fácilmente el contenido HTML devuelto.</span><span class="sxs-lookup"><span data-stu-id="629a1-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="629a1-113">Eso es exactamente lo que hacen las vistas.</span><span class="sxs-lookup"><span data-stu-id="629a1-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="629a1-114">Agregar una plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="629a1-114">Adding a View template</span></span>

<span data-ttu-id="629a1-115">Para usar una plantilla de vista, cambiaremos el método de índice de HomeController para devolver un ActionResult y hacer que devuelva la vista (), como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="629a1-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="629a1-116">El cambio anterior indica que en lugar de devolver una cadena, queremos usar una "vista" para volver a generar un resultado.</span><span class="sxs-lookup"><span data-stu-id="629a1-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="629a1-117">Ahora vamos a agregar una plantilla de vista adecuada a nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="629a1-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="629a1-118">Para ello, colocaremos el cursor de texto en el método de acción de índice, luego haga clic con el botón derecho y seleccione "agregar vista".</span><span class="sxs-lookup"><span data-stu-id="629a1-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="629a1-119">Se abrirá el cuadro de diálogo Agregar vista:</span><span class="sxs-lookup"><span data-stu-id="629a1-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="629a1-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="629a1-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="629a1-121">El cuadro de diálogo "agregar vista" nos permite generar archivos de plantilla de vista de forma rápida y sencilla.</span><span class="sxs-lookup"><span data-stu-id="629a1-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="629a1-122">De forma predeterminada, el cuadro de diálogo "agregar vista" rellena previamente el nombre de la plantilla de vista que se va a crear para que coincida con el método de acción que la usará.</span><span class="sxs-lookup"><span data-stu-id="629a1-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="629a1-123">Dado que usamos el menú contextual "agregar vista" dentro del método de acción index () de nuestro HomeController, el cuadro de diálogo "agregar vista" anterior tiene "index" como el nombre de la vista que se ha rellenado previamente de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="629a1-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="629a1-124">No es necesario cambiar ninguna de las opciones de este cuadro de diálogo, así que haga clic en el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="629a1-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="629a1-125">Al hacer clic en el botón Agregar, Visual Web Developer creará una nueva plantilla de vista index. cshtml para nosotros en el directorio \Views\Home y creará la carpeta si aún no existe.</span><span class="sxs-lookup"><span data-stu-id="629a1-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="629a1-126">El nombre y la ubicación de la carpeta del archivo "index. cshtml" es importante y sigue las convenciones de nomenclatura predeterminadas de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="629a1-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="629a1-127">El nombre del directorio, \Views\Home, coincide con el controlador, que se denomina HomeController.</span><span class="sxs-lookup"><span data-stu-id="629a1-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="629a1-128">El nombre de la plantilla de vista, índice, coincide con el método de acción del controlador que mostrará la vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="629a1-129">ASP.NET MVC nos permite evitar tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando usamos esta Convención de nomenclatura para devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="629a1-130">De forma predeterminada, la plantilla de vista \Views\Home\Index.cshtml se representa cuando se escribe código como el siguiente en el HomeController:</span><span class="sxs-lookup"><span data-stu-id="629a1-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="629a1-131">Visual Web Developer creó y abrió la plantilla de vista "index. cshtml" después de hacer clic en el botón "agregar" del cuadro de diálogo "agregar vista".</span><span class="sxs-lookup"><span data-stu-id="629a1-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="629a1-132">A continuación se muestra el contenido de index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="629a1-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="629a1-133">Esta vista usa el sintaxis Razor, que es más conciso que el motor de vista de formularios Web Forms usado en los formularios Web Forms de ASP.NET y versiones anteriores de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="629a1-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="629a1-134">El motor de vista de formularios Web Forms sigue estando disponible en ASP.NET MVC 3, pero muchos desarrolladores descubren que el motor de vistas de Razor se adapta al desarrollo de ASP.NET MVC realmente bien.</span><span class="sxs-lookup"><span data-stu-id="629a1-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="629a1-135">Las tres primeras líneas establecen el título de la página mediante ViewBag. title.</span><span class="sxs-lookup"><span data-stu-id="629a1-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="629a1-136">Veremos cómo funciona con más detalle pronto, pero primero vamos a actualizar el texto del encabezado de texto y ver la página.</span><span class="sxs-lookup"><span data-stu-id="629a1-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="629a1-137">Actualice la etiqueta &lt;H2&gt; para decir "esta es la Página principal", como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="629a1-138">La ejecución de la aplicación muestra que el nuevo texto está visible en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="629a1-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="629a1-139">Usar un diseño para los elementos comunes del sitio</span><span class="sxs-lookup"><span data-stu-id="629a1-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="629a1-140">La mayoría de los sitios web tienen contenido que se comparte entre muchas páginas: navegación, pies de página, imágenes de logotipo, referencias de hoja de estilos, etc. El motor de vistas de Razor facilita la administración mediante el uso de una página denominada \_layout. cshtml que se ha creado automáticamente en la carpeta/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="629a1-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="629a1-141">Haga doble clic en esta carpeta para ver el contenido, que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="629a1-142">El contenido de nuestras vistas individuales se mostrará con el comando @RenderBody() y cualquier contenido común que desee que aparezca fuera de, se puede Agregar al marcado \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="629a1-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="629a1-143">Queremos que nuestra tienda de música de MVC tenga un encabezado común con vínculos a nuestra página principal y área de almacenamiento en todas las páginas del sitio, por lo que agregaremos a la plantilla directamente encima de la instrucción @RenderBody().</span><span class="sxs-lookup"><span data-stu-id="629a1-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="629a1-144">Actualizar la hoja de estilos</span><span class="sxs-lookup"><span data-stu-id="629a1-144">Updating the StyleSheet</span></span>

<span data-ttu-id="629a1-145">La plantilla de proyecto vacía incluye un archivo CSS muy simplificado que solo incluye estilos que se usan para mostrar los mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="629a1-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="629a1-146">Nuestro diseñador ha proporcionado algunos CSS e imágenes adicionales para definir la apariencia y el funcionamiento de nuestro sitio, por lo que los agregaremos en este momento.</span><span class="sxs-lookup"><span data-stu-id="629a1-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="629a1-147">El archivo CSS actualizado y las imágenes se incluyen en el directorio de contenido de MvcMusicStore-Assets. zip, que está disponible en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="629a1-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="629a1-148">Seleccionaremos ambos en el explorador de Windows y los colocaremos en la carpeta de contenido de la solución en Visual Web Developer, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="629a1-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="629a1-149">Se le pedirá que confirme si desea sobrescribir el archivo site. CSS existente.</span><span class="sxs-lookup"><span data-stu-id="629a1-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="629a1-150">Haga clic en Sí.</span><span class="sxs-lookup"><span data-stu-id="629a1-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="629a1-151">La carpeta de contenido de la aplicación aparecerá ahora de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="629a1-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="629a1-152">Ahora vamos a ejecutar la aplicación y ver cómo se ven los cambios en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="629a1-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="629a1-153">Vamos a revisar lo que ha cambiado: el método de acción de índice de HomeController encontró y mostraba la plantilla \Views\Home\Index.cshtmlView, aunque nuestro código llamara "Return View ()", ya que nuestra plantilla de vista siguió la Convención de nomenclatura estándar.</span><span class="sxs-lookup"><span data-stu-id="629a1-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="629a1-154">La Página principal muestra un mensaje de bienvenida sencillo que se define dentro de la plantilla de vista \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="629a1-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="629a1-155">La Página principal usa nuestra plantilla \_layout. cshtml y, por tanto, el mensaje de bienvenida está incluido en el diseño HTML del sitio estándar.</span><span class="sxs-lookup"><span data-stu-id="629a1-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="629a1-156">Usar un modelo para pasar información a nuestra vista</span><span class="sxs-lookup"><span data-stu-id="629a1-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="629a1-157">Una plantilla de vista que solo muestra HTML codificado no va a crear un sitio web muy interesante.</span><span class="sxs-lookup"><span data-stu-id="629a1-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="629a1-158">Para crear un sitio web dinámico, en su lugar desearemos pasar información de nuestras acciones del controlador a nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="629a1-159">En el modelo modelo-vista-controlador, el término modelo hace referencia a los objetos que representan los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="629a1-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="629a1-160">A menudo, los objetos de modelo se corresponden con las tablas de la base de datos, pero no tienen que hacerlo.</span><span class="sxs-lookup"><span data-stu-id="629a1-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="629a1-161">Los métodos de acción del controlador que devuelven un ActionResult pueden pasar un objeto de modelo a la vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="629a1-162">Esto permite a un controlador empaquetar correctamente toda la información necesaria para generar una respuesta y, a continuación, pasar esta información a una plantilla de vista para usar para generar la respuesta HTML adecuada.</span><span class="sxs-lookup"><span data-stu-id="629a1-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="629a1-163">Esto es más fácil de entender si se ve en acción, así que vamos a empezar.</span><span class="sxs-lookup"><span data-stu-id="629a1-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="629a1-164">En primer lugar, crearemos algunas clases de modelo para representar los géneros y los álbumes de nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="629a1-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="629a1-165">Comencemos por crear una clase Genre.</span><span class="sxs-lookup"><span data-stu-id="629a1-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="629a1-166">Haga clic con el botón secundario en la carpeta "models" del proyecto, elija la opción "Agregar clase" y asigne al archivo el nombre "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="629a1-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="629a1-167">A continuación, agregue una propiedad de nombre de cadena pública a la clase que se creó:</span><span class="sxs-lookup"><span data-stu-id="629a1-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="629a1-168">*Nota: en caso de que se pregunte, la notación {Get; Set;} está haciendo uso C#de la característica de propiedades implementadas automáticamente. Esto nos ofrece las ventajas de una propiedad sin necesidad de declarar un campo de respaldo.*</span><span class="sxs-lookup"><span data-stu-id="629a1-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="629a1-169">A continuación, siga los mismos pasos para crear una clase album (denominada Album.cs) que tenga un título y una propiedad Genre:</span><span class="sxs-lookup"><span data-stu-id="629a1-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="629a1-170">Ahora podemos modificar StoreController para usar vistas que muestren información dinámica de nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="629a1-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="629a1-171">Si, en este momento, se llamara a nuestros álbumes en función del identificador de solicitud, podríamos Mostrar esa información como en la siguiente vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="629a1-172">Comenzaremos cambiando la acción Store details para que muestre la información de un solo álbum.</span><span class="sxs-lookup"><span data-stu-id="629a1-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="629a1-173">Agregue una instrucción "Using" a la parte superior de la clase **StoreControllers** para incluir el espacio de nombres MvcMusicStore. Models, por lo que no es necesario escribir MvcMusicStore. Models. album cada vez que queremos usar la clase Album.</span><span class="sxs-lookup"><span data-stu-id="629a1-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="629a1-174">La sección "Using" de esa clase debe aparecer ahora como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="629a1-175">A continuación, actualizaremos la acción del controlador de detalles para que devuelva un ActionResult en lugar de una cadena, como hicimos con el método index de HomeController.</span><span class="sxs-lookup"><span data-stu-id="629a1-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="629a1-176">Ahora podemos modificar la lógica para devolver un objeto album a la vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="629a1-177">Más adelante en este tutorial se van a recuperar los datos de una base de datos de, pero, de momento, vamos a usar "datos ficticios" para comenzar.</span><span class="sxs-lookup"><span data-stu-id="629a1-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="629a1-178">*Nota: Si no está familiarizado con C#, puede suponer que el uso de var significa que la variable de álbum está enlazada en tiempo de ejecución. Esto no es correcto: el C# compilador usa la inferencia de tipos en función de lo que estamos asignando a la variable para determinar que el álbum es de tipo album y compilando la variable de álbum local como un tipo de álbum, por lo que obtenemos la comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="629a1-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="629a1-179">Ahora vamos a crear una plantilla de vista que use nuestro álbum para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="629a1-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="629a1-180">Antes de hacerlo, debemos compilar el proyecto para que el cuadro de diálogo Agregar vista Conozca nuestra clase de álbum recién creada.</span><span class="sxs-lookup"><span data-stu-id="629a1-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="629a1-181">Puede compilar el proyecto seleccionando el elemento de menú Depurar ⇨ compilación MvcMusicStore (para crédito adicional, puede usar el método abreviado CTRL-SHIFT-B para compilar el proyecto).</span><span class="sxs-lookup"><span data-stu-id="629a1-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="629a1-182">Ahora que hemos configurado nuestras clases de soporte técnico, estamos preparados para crear nuestra plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="629a1-183">Haga clic con el botón derecho en el método de detalles y seleccione "agregar vista..." en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="629a1-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="629a1-184">Vamos a crear una nueva plantilla de vista como hicimos antes con el HomeController.</span><span class="sxs-lookup"><span data-stu-id="629a1-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="629a1-185">Dado que se crea a partir de StoreController, se generará de forma predeterminada en un archivo \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="629a1-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="629a1-186">A diferencia de antes, vamos a activar la casilla "crear una vista fuertemente tipada".</span><span class="sxs-lookup"><span data-stu-id="629a1-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="629a1-187">A continuación, vamos a seleccionar la clase "album" en el Drop-downlist de "View Data-Class".</span><span class="sxs-lookup"><span data-stu-id="629a1-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="629a1-188">Esto hará que el cuadro de diálogo "agregar vista" cree una plantilla de vista que espera que se le pase un objeto de álbum para usar.</span><span class="sxs-lookup"><span data-stu-id="629a1-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="629a1-189">Cuando hacemos clic en el botón "agregar", se creará la plantilla de vista \Views\Store\Details.cshtml, que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="629a1-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="629a1-190">Observe la primera línea, que indica que esta vista está fuertemente tipada a nuestra clase Album.</span><span class="sxs-lookup"><span data-stu-id="629a1-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="629a1-191">El motor de vistas de Razor entiende que se ha pasado un objeto de álbum, por lo que podemos acceder fácilmente a las propiedades del modelo e incluso tener la ventaja de IntelliSense en el editor de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="629a1-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="629a1-192">Actualice la etiqueta del&gt; de &lt;H2 para que muestre la propiedad title del álbum modificando esa línea para que aparezca de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="629a1-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="629a1-193">Observe que IntelliSense se desencadena cuando se escribe el punto después de la palabra clave @Model, que muestra las propiedades y los métodos que admite la clase Album.</span><span class="sxs-lookup"><span data-stu-id="629a1-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="629a1-194">Ahora vamos a ejecutar el proyecto y a visitar la dirección URL de/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="629a1-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="629a1-195">Veremos los detalles de un álbum como el que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="629a1-196">Ahora crearemos una actualización similar para el método de acción de búsqueda en el almacén.</span><span class="sxs-lookup"><span data-stu-id="629a1-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="629a1-197">Actualice el método para que devuelva un ActionResult y modifique la lógica del método para que cree un nuevo objeto Genre y lo devuelva a la vista.</span><span class="sxs-lookup"><span data-stu-id="629a1-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="629a1-198">Haga clic con el botón derecho en el método de exploración y seleccione "agregar vista..." en el menú contextual, agregue una vista fuertemente tipada agregue una fuertemente tipada a la clase Genre.</span><span class="sxs-lookup"><span data-stu-id="629a1-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="629a1-199">Actualice el elemento&gt; de &lt;H2 en el código de vista (en/Views/Store/Browse.cshtml) para mostrar la información del género.</span><span class="sxs-lookup"><span data-stu-id="629a1-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="629a1-200">Ahora vamos a volver a ejecutar nuestro proyecto y a buscar el/Store/Browse? Genre = dirección URL de disco.</span><span class="sxs-lookup"><span data-stu-id="629a1-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="629a1-201">Veremos la página examinar como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="629a1-202">Por último, vamos a crear una actualización ligeramente más compleja en el método de acción de índice de la **tienda** y la vista para mostrar una lista de todos los géneros de nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="629a1-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="629a1-203">Lo haremos mediante una lista de géneros como objeto del modelo, en lugar de un solo género.</span><span class="sxs-lookup"><span data-stu-id="629a1-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="629a1-204">Haga clic con el botón derecho en el método de acción almacenar índice y seleccione Agregar vista como antes, seleccione género como clase de modelo y presione el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="629a1-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="629a1-205">En primer lugar, cambiaremos la declaración de @model para indicar que la vista esperará varios objetos de género en lugar de uno solo.</span><span class="sxs-lookup"><span data-stu-id="629a1-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="629a1-206">Cambie la primera línea de/Store/Index.cshtml para que quede como sigue:</span><span class="sxs-lookup"><span data-stu-id="629a1-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="629a1-207">Esto indica al motor de vistas de Razor que va a trabajar con un objeto de modelo que puede contener varios objetos de género.</span><span class="sxs-lookup"><span data-stu-id="629a1-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="629a1-208">Estamos usando un género IEnumerable&lt;&gt; en lugar de una lista&lt;género&gt; ya que es más genérico, lo que nos permite cambiar el tipo de modelo más adelante a cualquier tipo de objeto que admita la interfaz IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="629a1-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="629a1-209">A continuación, vamos a recorrer los objetos de género en el modelo, tal como se muestra en el código de vista completado a continuación.</span><span class="sxs-lookup"><span data-stu-id="629a1-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="629a1-210">Tenga en cuenta que tenemos compatibilidad completa con IntelliSense a medida que especificamos este código, de modo que cuando escribamos "@Model".</span><span class="sxs-lookup"><span data-stu-id="629a1-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="629a1-211">vemos todos los métodos y las propiedades que admite un IEnumerable de tipo Genre.</span><span class="sxs-lookup"><span data-stu-id="629a1-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="629a1-212">En nuestro bucle "foreach", Visual Web Developer sabe que cada elemento es de tipo Genre, por lo que vemos IntelliSense para cada tipo de género.</span><span class="sxs-lookup"><span data-stu-id="629a1-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="629a1-213">A continuación, la característica de scaffolding examinó el objeto Genre y determinó que cada uno tendrá una propiedad Name, de modo que recorra y escriba en bucle. También genera vínculos de edición, detalles y eliminación a cada elemento individual.</span><span class="sxs-lookup"><span data-stu-id="629a1-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="629a1-214">Nos beneficiaremos de esto más adelante en nuestro administrador de tienda, pero por ahora nos gustaría tener una lista simple en su lugar.</span><span class="sxs-lookup"><span data-stu-id="629a1-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="629a1-215">Cuando ejecutamos la aplicación y desplazamos hasta/Store, vemos que se muestra el recuento y la lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="629a1-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="629a1-216">Agregar vínculos entre páginas</span><span class="sxs-lookup"><span data-stu-id="629a1-216">Adding Links between pages</span></span>

<span data-ttu-id="629a1-217">En la dirección URL de/Store que muestra géneros se enumeran actualmente los nombres de género simplemente como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="629a1-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="629a1-218">Vamos a cambiar esto para que, en lugar de texto sin formato, los nombres de los géneros se vinculen a la dirección URL de/Store/Browse adecuada, de modo que al hacer clic en un género musical como "disco" vaya a la dirección URL de/Store/Browse? Genre = disco.</span><span class="sxs-lookup"><span data-stu-id="629a1-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="629a1-219">Podríamos actualizar nuestra plantilla de vista de \Views\Store\Index.cshtml para generar estos vínculos con código como **el siguiente (no lo escribas en)** :</span><span class="sxs-lookup"><span data-stu-id="629a1-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="629a1-220">Eso funciona, pero podría provocar problemas más adelante, ya que se basa en una cadena codificada.</span><span class="sxs-lookup"><span data-stu-id="629a1-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="629a1-221">Por ejemplo, si deseáramos cambiar el nombre del controlador, es necesario buscar en nuestro código los vínculos que deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="629a1-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="629a1-222">Un enfoque alternativo que se puede utilizar es aprovechar un método auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="629a1-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="629a1-223">ASP.NET MVC incluye métodos auxiliares HTML que están disponibles en nuestro código de plantilla de vista para realizar diversas tareas comunes de la misma manera.</span><span class="sxs-lookup"><span data-stu-id="629a1-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="629a1-224">El método auxiliar HTML. ActionLink () es un método especialmente útil y facilita la compilación de HTML &lt;un&gt; vínculos y se encarga de los detalles molestos, como asegurarse de que las rutas de acceso URL están codificadas correctamente como dirección URL.</span><span class="sxs-lookup"><span data-stu-id="629a1-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="629a1-225">HTML. ActionLink () tiene varias sobrecargas diferentes para permitir especificar toda la información que necesita para los vínculos.</span><span class="sxs-lookup"><span data-stu-id="629a1-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="629a1-226">En el caso más simple, proporcionará solo el texto del vínculo y el método de acción que se va a usar cuando se haga clic en el hipervínculo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="629a1-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="629a1-227">Por ejemplo, podemos crear un vínculo al método index () de "/Store/" en la página de detalles de la tienda con el texto del vínculo "ir al índice de la tienda" mediante la siguiente llamada:</span><span class="sxs-lookup"><span data-stu-id="629a1-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="629a1-228">*Nota: en este caso, no es necesario especificar el nombre del controlador porque simplemente estamos vinculando a otra acción dentro del mismo controlador que representa la vista actual.*</span><span class="sxs-lookup"><span data-stu-id="629a1-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="629a1-229">Sin embargo, los vínculos a la página examinar deberán pasar un parámetro, por lo que usaremos otra sobrecarga del método html. ActionLink que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="629a1-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="629a1-230">Texto del vínculo, que mostrará el nombre del género</span><span class="sxs-lookup"><span data-stu-id="629a1-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="629a1-231">Nombre de acción del controlador (examinar)</span><span class="sxs-lookup"><span data-stu-id="629a1-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="629a1-232">Valores de parámetro de la ruta, que especifican el nombre (género) y el valor (nombre de género)</span><span class="sxs-lookup"><span data-stu-id="629a1-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="629a1-233">En conjunto, aquí se muestra cómo escribiremos esos vínculos en la vista de índice de tienda:</span><span class="sxs-lookup"><span data-stu-id="629a1-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="629a1-234">Ahora, al ejecutar el proyecto de nuevo y acceder a la dirección URL de/Store/, se verá una lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="629a1-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="629a1-235">Cada género es un hipervínculo, cuando se hace clic en él nos llevará a nuestra dirección URL de/Store/Browse? Genre = *[Genre]* .</span><span class="sxs-lookup"><span data-stu-id="629a1-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="629a1-236">El código HTML de la lista género tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="629a1-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="629a1-237">[Anterior](mvc-music-store-part-2.md)
> [Siguiente](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="629a1-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
