---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 2 cubre los controladores.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451195"
---
# <a name="part-2-controllers"></a><span data-ttu-id="6cea3-104">Parte 2: Controladores</span><span class="sxs-lookup"><span data-stu-id="6cea3-104">Part 2: Controllers</span></span>

<span data-ttu-id="6cea3-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6cea3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6cea3-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="6cea3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6cea3-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="6cea3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6cea3-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="6cea3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6cea3-109">La parte 2 cubre los controladores.</span><span class="sxs-lookup"><span data-stu-id="6cea3-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="6cea3-110">Con los marcos web tradicionales, las direcciones URL de entrada se asignan normalmente a archivos en disco.</span><span class="sxs-lookup"><span data-stu-id="6cea3-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="6cea3-111">Por ejemplo: una solicitud para una dirección URL como "/Products.aspx" o "/Products.php" puede ser procesada por un archivo "Products. aspx" o "Products. php".</span><span class="sxs-lookup"><span data-stu-id="6cea3-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="6cea3-112">Los marcos MVC basados en Web asignan direcciones URL al código del servidor de una manera ligeramente diferente.</span><span class="sxs-lookup"><span data-stu-id="6cea3-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="6cea3-113">En lugar de asignar las direcciones URL entrantes a los archivos, asignan direcciones URL a métodos en las clases.</span><span class="sxs-lookup"><span data-stu-id="6cea3-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="6cea3-114">Estas clases se denominan "controladores" y son responsables del procesamiento de solicitudes HTTP entrantes, el control de la entrada del usuario, la recuperación y el almacenamiento de datos, y la determinación de la respuesta que se devuelve al cliente (mostrar HTML, descargar un archivo, redirigir a otro URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="6cea3-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="6cea3-115">Agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="6cea3-115">Adding a HomeController</span></span>

<span data-ttu-id="6cea3-116">Comenzaremos nuestra aplicación MVC Music Store agregando una clase de controlador que controlará las direcciones URL de la Página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="6cea3-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="6cea3-117">Seguiremos las convenciones de nomenclatura predeterminadas de ASP.NET MVC y lo llamarán HomeController.</span><span class="sxs-lookup"><span data-stu-id="6cea3-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="6cea3-118">Haga clic con el botón secundario en la carpeta "Controllers" dentro del Explorador de soluciones, seleccione "agregar" y, a continuación, el "controlador..." Command</span><span class="sxs-lookup"><span data-stu-id="6cea3-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="6cea3-119">Se abrirá el cuadro de diálogo "agregar controlador".</span><span class="sxs-lookup"><span data-stu-id="6cea3-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="6cea3-120">Asigne al controlador el nombre "HomeController" y presione el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="6cea3-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="6cea3-121">Se creará un nuevo archivo, HomeController.cs, con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6cea3-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="6cea3-122">Para empezar lo más fácilmente posible, reemplace el método index por un método simple que simplemente devuelva una cadena.</span><span class="sxs-lookup"><span data-stu-id="6cea3-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="6cea3-123">Realizaremos dos cambios:</span><span class="sxs-lookup"><span data-stu-id="6cea3-123">We'll make two changes:</span></span>

- <span data-ttu-id="6cea3-124">Cambie el método para que devuelva una cadena en lugar de un ActionResult</span><span class="sxs-lookup"><span data-stu-id="6cea3-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="6cea3-125">Cambie la instrucción return para que devuelva "Hello From Home"</span><span class="sxs-lookup"><span data-stu-id="6cea3-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="6cea3-126">El método debería tener ahora el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="6cea3-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="6cea3-127">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="6cea3-127">Running the Application</span></span>

<span data-ttu-id="6cea3-128">Ahora vamos a ejecutar el sitio.</span><span class="sxs-lookup"><span data-stu-id="6cea3-128">Now let's run the site.</span></span> <span data-ttu-id="6cea3-129">Podemos iniciar nuestro Web-Server y probar el sitio con cualquiera de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="6cea3-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="6cea3-130">Elija el elemento de menú Depurar ⇨ iniciar depuración</span><span class="sxs-lookup"><span data-stu-id="6cea3-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="6cea3-131">Haga clic en el botón de flecha verde de la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="6cea3-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="6cea3-132">Use el método abreviado de teclado, F5.</span><span class="sxs-lookup"><span data-stu-id="6cea3-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="6cea3-133">Con cualquiera de los pasos anteriores se compilará el proyecto y, a continuación, se iniciará el Servidor de desarrollo de ASP.NET integrado en Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="6cea3-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="6cea3-134">Aparecerá una notificación en la esquina inferior de la pantalla para indicar que el Servidor de desarrollo de ASP.NET se ha iniciado y mostrará el número de puerto en el que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="6cea3-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="6cea3-135">Visual Web Developer abrirá automáticamente una ventana del explorador cuya dirección URL señala a nuestro servidor Web.</span><span class="sxs-lookup"><span data-stu-id="6cea3-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="6cea3-136">Esto nos permitirá probar rápidamente nuestra aplicación web:</span><span class="sxs-lookup"><span data-stu-id="6cea3-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="6cea3-137">Bien, eso era bastante rápido: hemos creado un nuevo sitio web, hemos agregado una función de tres líneas y hemos incluido texto en un explorador.</span><span class="sxs-lookup"><span data-stu-id="6cea3-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="6cea3-138">No se trata de la ciencia, pero es un principio.</span><span class="sxs-lookup"><span data-stu-id="6cea3-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="6cea3-139">*Nota: Visual Web Developer incluye el Servidor de desarrollo de ASP.NET, que ejecutará el sitio web en un número de "puerto" gratuito aleatorio. En la captura de pantalla anterior, el sitio se ejecuta en `http://localhost:26641/`, por lo que usa el puerto 26641. El número de puerto será diferente. Cuando hablemos de la dirección URL como/Store/Browse en este tutorial, irá después del número de puerto. Suponiendo un número de puerto de 26641, la exploración a/Store/Browse significará que se va a `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="6cea3-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="6cea3-140">Adición de un StoreController</span><span class="sxs-lookup"><span data-stu-id="6cea3-140">Adding a StoreController</span></span>

<span data-ttu-id="6cea3-141">Hemos agregado un HomeController sencillo que implementa la Página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="6cea3-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="6cea3-142">Ahora vamos a agregar otro controlador que usaremos para implementar la funcionalidad de exploración de nuestra tienda de música.</span><span class="sxs-lookup"><span data-stu-id="6cea3-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="6cea3-143">Nuestro controlador de tienda será compatible con tres escenarios:</span><span class="sxs-lookup"><span data-stu-id="6cea3-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="6cea3-144">Página de lista de los géneros musicales de nuestra tienda de música</span><span class="sxs-lookup"><span data-stu-id="6cea3-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="6cea3-145">Página de exploración en la que se muestran todos los álbumes de música de un género determinado.</span><span class="sxs-lookup"><span data-stu-id="6cea3-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="6cea3-146">Una página de detalles que muestra información sobre un álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="6cea3-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="6cea3-147">Comenzaremos agregando una nueva clase StoreController..</span><span class="sxs-lookup"><span data-stu-id="6cea3-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="6cea3-148">Si todavía no lo ha hecho, detenga la ejecución de la aplicación; para ello, cierre el explorador o seleccione el elemento de menú Depurar ⇨ detener depuración.</span><span class="sxs-lookup"><span data-stu-id="6cea3-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="6cea3-149">Ahora, agregue un nuevo StoreController.</span><span class="sxs-lookup"><span data-stu-id="6cea3-149">Now add a new StoreController.</span></span> <span data-ttu-id="6cea3-150">Al igual que hicimos con HomeController, haremos esto haciendo clic con el botón derecho en la carpeta "Controllers" en el Explorador de soluciones y eligiendo el elemento de menú controlador de Add-&gt;.</span><span class="sxs-lookup"><span data-stu-id="6cea3-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="6cea3-151">Nuestro nuevo StoreController ya tiene un método "índice".</span><span class="sxs-lookup"><span data-stu-id="6cea3-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="6cea3-152">Usaremos este método de "índice" para implementar la página de anuncios en la que se enumeran todos los géneros de nuestra tienda de música.</span><span class="sxs-lookup"><span data-stu-id="6cea3-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="6cea3-153">También agregaremos dos métodos adicionales para implementar los otros dos escenarios que queremos que StoreController administre: examinar y detalles.</span><span class="sxs-lookup"><span data-stu-id="6cea3-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="6cea3-154">Estos métodos (índice, exploración y detalles) en nuestro controlador se denominan "acciones del controlador" y, como ya se ha visto con el método de acción HomeController. index (), su trabajo consiste en responder a las solicitudes URL y, por lo general, determinar qué contenido debe volver a enviarse al explorador o al usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="6cea3-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="6cea3-155">Comenzaremos nuestra implementación de StoreController cambiando el método index () para que devuelva la cadena "Hello from Store. index ()" y agregaremos métodos similares para Browse () y Details ():</span><span class="sxs-lookup"><span data-stu-id="6cea3-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="6cea3-156">Vuelva a ejecutar el proyecto y examine las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="6cea3-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="6cea3-157">/Store</span><span class="sxs-lookup"><span data-stu-id="6cea3-157">/Store</span></span>
- <span data-ttu-id="6cea3-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="6cea3-158">/Store/Browse</span></span>
- <span data-ttu-id="6cea3-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="6cea3-159">/Store/Details</span></span>

<span data-ttu-id="6cea3-160">El acceso a estas direcciones URL invocará los métodos de acción dentro de nuestro controlador y devolverá respuestas de cadena:</span><span class="sxs-lookup"><span data-stu-id="6cea3-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="6cea3-161">Eso es excelente, pero se trata simplemente de cadenas constantes.</span><span class="sxs-lookup"><span data-stu-id="6cea3-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="6cea3-162">Vamos a hacerlos dinámicos, de modo que tomen información de la dirección URL y la muestren en el resultado de la página.</span><span class="sxs-lookup"><span data-stu-id="6cea3-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="6cea3-163">En primer lugar, vamos a cambiar el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="6cea3-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="6cea3-164">Podemos hacer esto agregando un parámetro "género" a nuestro método de acción.</span><span class="sxs-lookup"><span data-stu-id="6cea3-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="6cea3-165">Cuando hacemos esto, ASP.NET MVC pasará automáticamente los parámetros QueryString o post de formulario denominados "género" a nuestro método de acción cuando se invoque.</span><span class="sxs-lookup"><span data-stu-id="6cea3-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="6cea3-166">*Nota: estamos usando el método de la utilidad HttpUtility. HtmlEncode para corregir la entrada del usuario. Esto impide que los usuarios inserten JavaScript en nuestra vista con un vínculo como/Store/Browse? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="6cea3-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="6cea3-167">Ahora vamos a examinar/Store/Browse? Género = disco</span><span class="sxs-lookup"><span data-stu-id="6cea3-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="6cea3-168">A continuación, vamos a cambiar la acción de detalles para leer y mostrar un parámetro de entrada denominado ID.</span><span class="sxs-lookup"><span data-stu-id="6cea3-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="6cea3-169">A diferencia de nuestro método anterior, no vamos a insertar el valor de identificador como parámetro QueryString.</span><span class="sxs-lookup"><span data-stu-id="6cea3-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="6cea3-170">En su lugar, lo insertaremos directamente dentro de la propia dirección URL.</span><span class="sxs-lookup"><span data-stu-id="6cea3-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="6cea3-171">Por ejemplo:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="6cea3-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="6cea3-172">ASP.NET MVC nos permite hacer esto fácilmente sin tener que configurar nada.</span><span class="sxs-lookup"><span data-stu-id="6cea3-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="6cea3-173">La Convención de enrutamiento predeterminada de MVC de ASP.NET consiste en tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado "ID".</span><span class="sxs-lookup"><span data-stu-id="6cea3-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="6cea3-174">Si el método de acción tiene un parámetro denominado ID, ASP.NET MVC le pasará automáticamente el segmento de dirección URL como parámetro.</span><span class="sxs-lookup"><span data-stu-id="6cea3-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="6cea3-175">Ejecute la aplicación y vaya a/Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="6cea3-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="6cea3-176">Vamos a resumir lo que hemos hecho hasta ahora:</span><span class="sxs-lookup"><span data-stu-id="6cea3-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="6cea3-177">Hemos creado un nuevo proyecto de ASP.NET MVC en Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="6cea3-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="6cea3-178">Hemos analizado la estructura de carpetas básica de una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6cea3-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="6cea3-179">Hemos aprendido a ejecutar nuestro sitio web mediante el Servidor de desarrollo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6cea3-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="6cea3-180">Hemos creado dos clases de controlador: HomeController y StoreController</span><span class="sxs-lookup"><span data-stu-id="6cea3-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="6cea3-181">Hemos agregado métodos de acción a los controladores que responden a las solicitudes URL y devuelven texto al explorador.</span><span class="sxs-lookup"><span data-stu-id="6cea3-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6cea3-182">[Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="6cea3-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
