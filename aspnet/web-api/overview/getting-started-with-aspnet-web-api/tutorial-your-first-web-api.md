---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introducción a ASP.NET Web API 2 (C#)-ASP.net 4. x
author: MikeWasson
description: Tutorial con código. Use ASP.NET Web API para crear una API Web que devuelva una lista de productos.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084058"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="d4765-104">Introducción a ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="d4765-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="d4765-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d4765-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d4765-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d4765-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="d4765-107">En este tutorial, usará ASP.NET Web API para crear una API Web que devuelva una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="d4765-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="d4765-108">HTTP no es solo para servir páginas Web.</span><span class="sxs-lookup"><span data-stu-id="d4765-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="d4765-109">HTTP es también una plataforma eficaz para la creación de API que exponen servicios y datos.</span><span class="sxs-lookup"><span data-stu-id="d4765-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="d4765-110">HTTP es simple, flexible y omnipresente.</span><span class="sxs-lookup"><span data-stu-id="d4765-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="d4765-111">Casi cualquier plataforma que se pueda considerar tiene una biblioteca HTTP, por lo que los servicios HTTP pueden llegar a una amplia gama de clientes, incluidos exploradores, dispositivos móviles y aplicaciones de escritorio tradicionales.</span><span class="sxs-lookup"><span data-stu-id="d4765-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="d4765-112">ASP.NET Web API es un marco para crear API Web sobre la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d4765-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d4765-113">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="d4765-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="d4765-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="d4765-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="d4765-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="d4765-115">Web API 2</span></span>

<span data-ttu-id="d4765-116">Consulte [creación de una API Web con ASP.net Core y Visual Studio para Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para obtener una versión más reciente de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="d4765-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="d4765-117">Creación de un proyecto de API Web</span><span class="sxs-lookup"><span data-stu-id="d4765-117">Create a Web API Project</span></span>

<span data-ttu-id="d4765-118">En este tutorial, usará ASP.NET Web API para crear una API Web que devuelva una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="d4765-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="d4765-119">La Página Web de front-end utiliza jQuery para mostrar los resultados.</span><span class="sxs-lookup"><span data-stu-id="d4765-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="d4765-120">Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="d4765-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="d4765-121">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d4765-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="d4765-122">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="d4765-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="d4765-123">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="d4765-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="d4765-124">En la lista de plantillas de proyecto, seleccione **aplicación Web ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="d4765-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="d4765-125">Asigne al proyecto el nombre "ProductsApp" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d4765-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="d4765-126">En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** .</span><span class="sxs-lookup"><span data-stu-id="d4765-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="d4765-127">En &quot;agregar carpetas y referencias principales para&quot;, consulte **API Web**.</span><span class="sxs-lookup"><span data-stu-id="d4765-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="d4765-128">Haga clic en **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4765-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="d4765-129">También puede crear un proyecto de API Web con la plantilla de&quot; Web API de &quot;.</span><span class="sxs-lookup"><span data-stu-id="d4765-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="d4765-130">La plantilla de API Web usa ASP.NET MVC para proporcionar páginas de ayuda de la API.</span><span class="sxs-lookup"><span data-stu-id="d4765-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="d4765-131">Estoy usando la plantilla vacía para este tutorial porque quiero mostrar la API Web sin MVC.</span><span class="sxs-lookup"><span data-stu-id="d4765-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="d4765-132">En general, no es necesario conocer ASP.NET MVC para usar Web API.</span><span class="sxs-lookup"><span data-stu-id="d4765-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="d4765-133">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="d4765-133">Adding a Model</span></span>

<span data-ttu-id="d4765-134">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d4765-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="d4765-135">ASP.NET Web API puede serializar automáticamente el modelo a JSON, XML o a algún otro formato y, a continuación, escribir los datos serializados en el cuerpo del mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4765-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="d4765-136">Siempre que un cliente pueda leer el formato de serialización, puede deserializar el objeto.</span><span class="sxs-lookup"><span data-stu-id="d4765-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="d4765-137">La mayoría de los clientes pueden analizar XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="d4765-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="d4765-138">Además, el cliente puede indicar el formato que desea estableciendo el encabezado Accept en el mensaje de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4765-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="d4765-139">Comencemos por crear un modelo simple que representa un producto.</span><span class="sxs-lookup"><span data-stu-id="d4765-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="d4765-140">Si el Explorador de soluciones no está visible, haga clic en el menú **Ver** y seleccione **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="d4765-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="d4765-141">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="d4765-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d4765-142">En el menú contextual, seleccione **Agregar** y, después, haga clic en **Clase**.</span><span class="sxs-lookup"><span data-stu-id="d4765-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="d4765-143">Asigne a la clase el nombre &quot;&quot;del producto.</span><span class="sxs-lookup"><span data-stu-id="d4765-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="d4765-144">Agregue las siguientes propiedades a la clase `Product`.</span><span class="sxs-lookup"><span data-stu-id="d4765-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="d4765-145">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="d4765-145">Adding a Controller</span></span>

<span data-ttu-id="d4765-146">En la API Web, un *controlador* es un objeto que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4765-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="d4765-147">Vamos a agregar un controlador que puede devolver una lista de productos o un único producto especificado por identificador.</span><span class="sxs-lookup"><span data-stu-id="d4765-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="d4765-148">Si ha usado ASP.NET MVC, ya está familiarizado con los controladores.</span><span class="sxs-lookup"><span data-stu-id="d4765-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="d4765-149">Los controladores de API Web son similares a los controladores de MVC, pero heredan la clase **ApiController** en lugar de la clase de **controlador** .</span><span class="sxs-lookup"><span data-stu-id="d4765-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="d4765-150">En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="d4765-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="d4765-151">Seleccione **Agregar** y, luego, **Controlador**.</span><span class="sxs-lookup"><span data-stu-id="d4765-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="d4765-152">En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de API Web: en blanco**.</span><span class="sxs-lookup"><span data-stu-id="d4765-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="d4765-153">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="d4765-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="d4765-154">En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="d4765-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="d4765-155">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="d4765-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="d4765-156">El scaffolding crea un archivo denominado ProductsController.cs en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="d4765-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="d4765-157">No es necesario colocar los controladores en una carpeta denominada Controllers.</span><span class="sxs-lookup"><span data-stu-id="d4765-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="d4765-158">El nombre de la carpeta es simplemente una manera cómoda de organizar los archivos de código fuente.</span><span class="sxs-lookup"><span data-stu-id="d4765-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="d4765-159">Si este archivo no está abierto, haga doble clic en el archivo para abrirlo.</span><span class="sxs-lookup"><span data-stu-id="d4765-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="d4765-160">Reemplace el código de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d4765-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="d4765-161">Para que el ejemplo sea sencillo, los productos se almacenan en una matriz fija dentro de la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="d4765-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="d4765-162">Por supuesto, en una aplicación real, realizaría una consulta en una base de datos o usaría algún otro origen de datos externo.</span><span class="sxs-lookup"><span data-stu-id="d4765-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="d4765-163">El controlador define dos métodos que devuelven productos:</span><span class="sxs-lookup"><span data-stu-id="d4765-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="d4765-164">El método `GetAllProducts` devuelve la lista completa de productos como un tipo de **&gt;de producto IEnumerable&lt;** .</span><span class="sxs-lookup"><span data-stu-id="d4765-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="d4765-165">El método `GetProduct` busca un único producto por su identificador.</span><span class="sxs-lookup"><span data-stu-id="d4765-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="d4765-166">Eso es todo.</span><span class="sxs-lookup"><span data-stu-id="d4765-166">That's it!</span></span> <span data-ttu-id="d4765-167">Tiene una API Web en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="d4765-167">You have a working web API.</span></span> <span data-ttu-id="d4765-168">Cada método en el controlador corresponde a uno o varios URI:</span><span class="sxs-lookup"><span data-stu-id="d4765-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="d4765-169">Método de controlador</span><span class="sxs-lookup"><span data-stu-id="d4765-169">Controller Method</span></span> | <span data-ttu-id="d4765-170">URI</span><span class="sxs-lookup"><span data-stu-id="d4765-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="d4765-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="d4765-171">GetAllProducts</span></span> | <span data-ttu-id="d4765-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="d4765-172">/api/products</span></span> |
| <span data-ttu-id="d4765-173">GetProduct</span><span class="sxs-lookup"><span data-stu-id="d4765-173">GetProduct</span></span> | <span data-ttu-id="d4765-174">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="d4765-174">/api/products/*id*</span></span> |

<span data-ttu-id="d4765-175">En el caso del método `GetProduct`, el *identificador* del identificador URI es un marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="d4765-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="d4765-176">Por ejemplo, para obtener el producto con el identificador 5, el URI es `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="d4765-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="d4765-177">Para obtener más información sobre cómo la API Web enruta las solicitudes HTTP a los métodos de controlador, consulte [enrutamiento en ASP.net web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="d4765-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="d4765-178">Llamar a la API Web con JavaScript y jQuery</span><span class="sxs-lookup"><span data-stu-id="d4765-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="d4765-179">En esta sección, agregaremos una página HTML que usa AJAX para llamar a la API Web.</span><span class="sxs-lookup"><span data-stu-id="d4765-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="d4765-180">Usaremos jQuery para realizar las llamadas AJAX y también para actualizar la página con los resultados.</span><span class="sxs-lookup"><span data-stu-id="d4765-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="d4765-181">En Explorador de soluciones, haga clic con el botón derecho en el proyecto, seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d4765-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="d4765-182">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione el nodo **Web** en **C#visual**y, a continuación, seleccione el elemento de **página HTML** .</span><span class="sxs-lookup"><span data-stu-id="d4765-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="d4765-183">Asigne a la página el nombre &quot;&quot;index. html.</span><span class="sxs-lookup"><span data-stu-id="d4765-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="d4765-184">Reemplace todo el código de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d4765-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="d4765-185">Existen varias formas de obtener jQuery.</span><span class="sxs-lookup"><span data-stu-id="d4765-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="d4765-186">En este ejemplo, usé la [red CDN de Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4765-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="d4765-187">También puede descargarlo desde [http://jquery.com/](http://jquery.com/)y la plantilla de proyecto ASP.net "Web API" también incluye jQuery.</span><span class="sxs-lookup"><span data-stu-id="d4765-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="d4765-188">Obtención de una lista de productos</span><span class="sxs-lookup"><span data-stu-id="d4765-188">Getting a List of Products</span></span>

<span data-ttu-id="d4765-189">Para obtener una lista de productos, envíe una solicitud HTTP GET a &quot;&quot;/API/Products.</span><span class="sxs-lookup"><span data-stu-id="d4765-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="d4765-190">La función [getJSON](http://api.jquery.com/jQuery.getJSON/) de jQuery envía una solicitud Ajax.</span><span class="sxs-lookup"><span data-stu-id="d4765-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="d4765-191">La respuesta contiene una matriz de objetos JSON.</span><span class="sxs-lookup"><span data-stu-id="d4765-191">The response contains array of JSON objects.</span></span> <span data-ttu-id="d4765-192">La función `done` especifica una devolución de llamada a la que se llama si la solicitud se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="d4765-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="d4765-193">En la devolución de llamada, actualizamos el DOM con la información del producto.</span><span class="sxs-lookup"><span data-stu-id="d4765-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="d4765-194">Obtención de un producto por identificador</span><span class="sxs-lookup"><span data-stu-id="d4765-194">Getting a Product By ID</span></span>

<span data-ttu-id="d4765-195">Para obtener un producto por identificador, envíe una solicitud HTTP GET a &quot;*ID* . de/API/Products/&quot;, donde *ID* es el identificador del producto.</span><span class="sxs-lookup"><span data-stu-id="d4765-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="d4765-196">Seguimos llamando `getJSON` para enviar la solicitud de AJAX, pero esta vez colocamos el identificador en el URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="d4765-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="d4765-197">La respuesta de esta solicitud es una representación JSON de un único producto.</span><span class="sxs-lookup"><span data-stu-id="d4765-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="d4765-198">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d4765-198">Running the Application</span></span>

<span data-ttu-id="d4765-199">Presione F5 para iniciar la depuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d4765-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="d4765-200">La página web debería tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d4765-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="d4765-201">Para obtener un producto por identificador, escriba el identificador y haga clic en buscar:</span><span class="sxs-lookup"><span data-stu-id="d4765-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="d4765-202">Si escribe un identificador no válido, el servidor devuelve un error HTTP:</span><span class="sxs-lookup"><span data-stu-id="d4765-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="d4765-203">Usar F12 para ver la solicitud y la respuesta HTTP</span><span class="sxs-lookup"><span data-stu-id="d4765-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="d4765-204">Cuando se trabaja con un servicio HTTP, puede ser muy útil ver los mensajes de solicitud y respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4765-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and response messages.</span></span> <span data-ttu-id="d4765-205">Puede hacerlo con las herramientas de desarrollo F12 de Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="d4765-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="d4765-206">En Internet Explorer 9, presione **F12** para abrir las herramientas.</span><span class="sxs-lookup"><span data-stu-id="d4765-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="d4765-207">Haga clic en la pestaña **red** y presione **Iniciar captura**.</span><span class="sxs-lookup"><span data-stu-id="d4765-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="d4765-208">Ahora vuelva a la página web y presione **F5** para volver a cargar la Página Web.</span><span class="sxs-lookup"><span data-stu-id="d4765-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="d4765-209">Internet Explorer capturará el tráfico HTTP entre el explorador y el servidor Web.</span><span class="sxs-lookup"><span data-stu-id="d4765-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="d4765-210">La vista resumen muestra todo el tráfico de red de una página:</span><span class="sxs-lookup"><span data-stu-id="d4765-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="d4765-211">Busque la entrada del URI relativo "API/Products/".</span><span class="sxs-lookup"><span data-stu-id="d4765-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="d4765-212">Seleccione esta entrada y haga clic en **ir a la vista detallada**.</span><span class="sxs-lookup"><span data-stu-id="d4765-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="d4765-213">En la vista de detalle, hay pestañas para ver los encabezados y cuerpos de solicitud y respuesta.</span><span class="sxs-lookup"><span data-stu-id="d4765-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="d4765-214">Por ejemplo, si hace clic en la pestaña **encabezados de solicitud** , puede ver que el cliente solicitó &quot;&quot; Application/JSON en el encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="d4765-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="d4765-215">Si hace clic en la pestaña cuerpo de respuesta, puede ver cómo se serializó la lista de productos en JSON.</span><span class="sxs-lookup"><span data-stu-id="d4765-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="d4765-216">Otros exploradores tienen una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="d4765-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="d4765-217">Otra herramienta útil es [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy de depuración web.</span><span class="sxs-lookup"><span data-stu-id="d4765-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="d4765-218">Puede usar Fiddler para ver el tráfico HTTP y también para crear solicitudes HTTP, lo que le proporciona un control total sobre los encabezados HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="d4765-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="d4765-219">Vea esta aplicación en ejecución en Azure</span><span class="sxs-lookup"><span data-stu-id="d4765-219">See this App Running on Azure</span></span>

<span data-ttu-id="d4765-220">¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa?</span><span class="sxs-lookup"><span data-stu-id="d4765-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="d4765-221">Puede implementar una versión completa de la aplicación en su cuenta de Azure simplemente haciendo clic en el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="d4765-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="d4765-222">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="d4765-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="d4765-223">Si aún no tiene una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="d4765-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="d4765-224">[Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="d4765-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="d4765-225">[Activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : su suscripción a MSDN le proporciona créditos todos los meses que puede usar para los servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="d4765-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4765-226">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d4765-226">Next Steps</span></span>

- <span data-ttu-id="d4765-227">Para obtener un ejemplo más completo de un servicio HTTP que admite acciones POST, PUT y DELETE y escrituras en una base de datos, consulte [uso de Web API 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="d4765-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="d4765-228">Para obtener más información sobre cómo crear aplicaciones web fluidas y receptivas sobre un servicio HTTP, vea aplicación de una [sola página de ASP.net](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="d4765-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="d4765-229">Para obtener información sobre cómo implementar un proyecto Web de Visual Studio en Azure App Service, consulte [creación de una aplicación Web de ASP.net en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="d4765-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
