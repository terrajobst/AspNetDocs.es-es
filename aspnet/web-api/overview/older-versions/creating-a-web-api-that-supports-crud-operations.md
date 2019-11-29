---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar las operaciones CRUD en ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: En el tutorial se muestra cómo admitir las operaciones CRUD en un servicio HTTP mediante ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600337"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="9033a-103">Habilitar operaciones CRUD en ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="9033a-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="9033a-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9033a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9033a-105">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="9033a-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="9033a-106">En este tutorial se muestra cómo admitir las operaciones CRUD en un servicio HTTP mediante ASP.NET Web API para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="9033a-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9033a-107">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="9033a-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9033a-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9033a-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="9033a-109">API Web 1 (también funciona con Web API 2)</span><span class="sxs-lookup"><span data-stu-id="9033a-109">Web API 1 (also works with Web API 2)</span></span>

<span data-ttu-id="9033a-110">CRUD significa &quot;crear, leer, actualizar y eliminar&quot; que son las cuatro operaciones básicas de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9033a-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="9033a-111">Muchos servicios HTTP también modelan las operaciones CRUD a través de las API de REST o del tipo REST.</span><span class="sxs-lookup"><span data-stu-id="9033a-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="9033a-112">En este tutorial, creará una API Web muy sencilla para administrar una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="9033a-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="9033a-113">Cada producto contendrá un nombre, un precio y una categoría (como &quot;juguetes&quot; o &quot;&quot;de hardware), además de un identificador de producto.</span><span class="sxs-lookup"><span data-stu-id="9033a-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="9033a-114">La API Products expondrá los siguientes métodos.</span><span class="sxs-lookup"><span data-stu-id="9033a-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="9033a-115">Acción de</span><span class="sxs-lookup"><span data-stu-id="9033a-115">Action</span></span> | <span data-ttu-id="9033a-116">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="9033a-116">HTTP method</span></span> | <span data-ttu-id="9033a-117">URI relativo</span><span class="sxs-lookup"><span data-stu-id="9033a-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9033a-118">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="9033a-118">Get a list of all products</span></span> | <span data-ttu-id="9033a-119">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-119">GET</span></span> | <span data-ttu-id="9033a-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="9033a-120">/api/products</span></span> |
| <span data-ttu-id="9033a-121">Obtener un producto por identificador</span><span class="sxs-lookup"><span data-stu-id="9033a-121">Get a product by ID</span></span> | <span data-ttu-id="9033a-122">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-122">GET</span></span> | <span data-ttu-id="9033a-123">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="9033a-123">/api/products/*id*</span></span> |
| <span data-ttu-id="9033a-124">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="9033a-124">Get a product by category</span></span> | <span data-ttu-id="9033a-125">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-125">GET</span></span> | <span data-ttu-id="9033a-126">/API/Products? categoría =*categoría*</span><span class="sxs-lookup"><span data-stu-id="9033a-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="9033a-127">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="9033a-127">Create a new product</span></span> | <span data-ttu-id="9033a-128">Exponer</span><span class="sxs-lookup"><span data-stu-id="9033a-128">POST</span></span> | <span data-ttu-id="9033a-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="9033a-129">/api/products</span></span> |
| <span data-ttu-id="9033a-130">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="9033a-130">Update a product</span></span> | <span data-ttu-id="9033a-131">PUT</span><span class="sxs-lookup"><span data-stu-id="9033a-131">PUT</span></span> | <span data-ttu-id="9033a-132">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="9033a-132">/api/products/*id*</span></span> |
| <span data-ttu-id="9033a-133">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="9033a-133">Delete a product</span></span> | <span data-ttu-id="9033a-134">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="9033a-134">DELETE</span></span> | <span data-ttu-id="9033a-135">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="9033a-135">/api/products/*id*</span></span> |

<span data-ttu-id="9033a-136">Observe que algunos de los URI incluyen el ID. de producto en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="9033a-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="9033a-137">Por ejemplo, para obtener el producto cuyo identificador es 28, el cliente envía una solicitud GET para `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="9033a-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="9033a-138">Recursos</span><span class="sxs-lookup"><span data-stu-id="9033a-138">Resources</span></span>

<span data-ttu-id="9033a-139">La API Products define los URI para dos tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="9033a-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="9033a-140">Recurso</span><span class="sxs-lookup"><span data-stu-id="9033a-140">Resource</span></span> | <span data-ttu-id="9033a-141">URI</span><span class="sxs-lookup"><span data-stu-id="9033a-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="9033a-142">Lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="9033a-142">The list of all the products.</span></span> | <span data-ttu-id="9033a-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="9033a-143">/api/products</span></span> |
| <span data-ttu-id="9033a-144">Un producto individual.</span><span class="sxs-lookup"><span data-stu-id="9033a-144">An individual product.</span></span> | <span data-ttu-id="9033a-145">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="9033a-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="9033a-146">Métodos</span><span class="sxs-lookup"><span data-stu-id="9033a-146">Methods</span></span>

<span data-ttu-id="9033a-147">Los cuatro métodos HTTP principales (GET, PUT, POST y DELETE) pueden asignarse a las operaciones CRUD como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="9033a-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="9033a-148">GET recupera la representación del recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="9033a-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="9033a-149">GET no debe tener efectos secundarios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="9033a-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="9033a-150">PUT actualiza un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="9033a-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="9033a-151">PUT también se puede usar para crear un nuevo recurso en un URI especificado, si el servidor permite a los clientes especificar nuevos URI.</span><span class="sxs-lookup"><span data-stu-id="9033a-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="9033a-152">En este tutorial, la API no admitirá la creación a través de PUT.</span><span class="sxs-lookup"><span data-stu-id="9033a-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="9033a-153">POST crea un nuevo recurso.</span><span class="sxs-lookup"><span data-stu-id="9033a-153">POST creates a new resource.</span></span> <span data-ttu-id="9033a-154">El servidor asigna el URI para el nuevo objeto y devuelve este URI como parte del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="9033a-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="9033a-155">DELETE elimina un recurso en un URI especificado.</span><span class="sxs-lookup"><span data-stu-id="9033a-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="9033a-156">Nota: el método PUT reemplaza toda la entidad product.</span><span class="sxs-lookup"><span data-stu-id="9033a-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="9033a-157">Es decir, se espera que el cliente envíe una representación completa del producto actualizado.</span><span class="sxs-lookup"><span data-stu-id="9033a-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="9033a-158">Si desea admitir actualizaciones parciales, se prefiere el método PATCH.</span><span class="sxs-lookup"><span data-stu-id="9033a-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="9033a-159">En este tutorial no se implementa PATCH.</span><span class="sxs-lookup"><span data-stu-id="9033a-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="9033a-160">Creación de un nuevo proyecto de API Web</span><span class="sxs-lookup"><span data-stu-id="9033a-160">Create a New Web API Project</span></span>

<span data-ttu-id="9033a-161">Para empezar, ejecute Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="9033a-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="9033a-162">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="9033a-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="9033a-163">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="9033a-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="9033a-164">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="9033a-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="9033a-165">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="9033a-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="9033a-166">Asigne al proyecto el nombre &quot;ProductStore&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9033a-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="9033a-167">En el cuadro de diálogo **nuevo ASP.NET MVC 4** , seleccione **Web API** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="9033a-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="9033a-168">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="9033a-168">Adding a Model</span></span>

<span data-ttu-id="9033a-169">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9033a-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="9033a-170">En ASP.NET Web API, puede usar objetos CLR fuertemente tipados como modelos y se serializarán automáticamente a XML o JSON para el cliente.</span><span class="sxs-lookup"><span data-stu-id="9033a-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="9033a-171">En el caso de la API de ProductStore, nuestros datos se componen de productos, por lo que vamos a crear una nueva clase llamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="9033a-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="9033a-172">Si Explorador de soluciones todavía no está visible, haga clic en el menú **Ver** y seleccione **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="9033a-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="9033a-173">En Explorador de soluciones, haga clic con el botón secundario en la carpeta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="9033a-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="9033a-174">En el menú contextual, seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="9033a-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="9033a-175">Asigne a la clase el nombre &quot;&quot;del producto.</span><span class="sxs-lookup"><span data-stu-id="9033a-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="9033a-176">Agregue las siguientes propiedades a la clase `Product`.</span><span class="sxs-lookup"><span data-stu-id="9033a-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="9033a-177">Agregar un repositorio</span><span class="sxs-lookup"><span data-stu-id="9033a-177">Adding a Repository</span></span>

<span data-ttu-id="9033a-178">Necesitamos almacenar una colección de productos.</span><span class="sxs-lookup"><span data-stu-id="9033a-178">We need to store a collection of products.</span></span> <span data-ttu-id="9033a-179">Es una buena idea separar la colección de nuestra implementación del servicio.</span><span class="sxs-lookup"><span data-stu-id="9033a-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="9033a-180">De este modo, podemos cambiar la memoria auxiliar sin volver a escribir la clase de servicio.</span><span class="sxs-lookup"><span data-stu-id="9033a-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="9033a-181">Este tipo de diseño se denomina patrón de *repositorio* .</span><span class="sxs-lookup"><span data-stu-id="9033a-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="9033a-182">Empiece por definir una interfaz genérica para el repositorio.</span><span class="sxs-lookup"><span data-stu-id="9033a-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="9033a-183">En Explorador de soluciones, haga clic con el botón secundario en la carpeta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="9033a-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="9033a-184">Seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9033a-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="9033a-185">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el C# nodo.</span><span class="sxs-lookup"><span data-stu-id="9033a-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="9033a-186">En C#, seleccione **código**.</span><span class="sxs-lookup"><span data-stu-id="9033a-186">Under C#, select **Code**.</span></span> <span data-ttu-id="9033a-187">En la lista de plantillas de código, seleccione **interfaz**.</span><span class="sxs-lookup"><span data-stu-id="9033a-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="9033a-188">Asigne a la interfaz el nombre &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="9033a-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="9033a-189">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="9033a-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="9033a-190">Ahora agregue otra clase a la carpeta models, denominada &quot;ProductRepository.&quot; esta clase implementará la interfaz `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="9033a-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="9033a-191">Agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="9033a-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="9033a-192">El repositorio mantiene la lista en la memoria local.</span><span class="sxs-lookup"><span data-stu-id="9033a-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="9033a-193">Esto es correcto para un tutorial, pero en una aplicación real, almacenaría los datos externamente, ya sea una base de datos o un almacenamiento en la nube.</span><span class="sxs-lookup"><span data-stu-id="9033a-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="9033a-194">El patrón de repositorio facilitará el cambio de la implementación más adelante.</span><span class="sxs-lookup"><span data-stu-id="9033a-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="9033a-195">Adición de un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="9033a-195">Adding a Web API Controller</span></span>

<span data-ttu-id="9033a-196">Si ha trabajado con ASP.NET MVC, ya está familiarizado con los controladores.</span><span class="sxs-lookup"><span data-stu-id="9033a-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="9033a-197">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP desde el cliente.</span><span class="sxs-lookup"><span data-stu-id="9033a-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="9033a-198">El Asistente para nuevo proyecto creó dos controladores automáticamente al crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9033a-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="9033a-199">Para verlos, expanda la carpeta Controllers en Explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="9033a-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="9033a-200">HomeController es un controlador ASP.NET MVC tradicional.</span><span class="sxs-lookup"><span data-stu-id="9033a-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="9033a-201">Es responsable de proporcionar páginas HTML para el sitio y no está directamente relacionado con nuestra API Web.</span><span class="sxs-lookup"><span data-stu-id="9033a-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="9033a-202">Clase valuescontroller es un controlador de WebAPI de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9033a-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="9033a-203">Continúe y elimine clase valuescontroller; para ello, haga clic con el botón derecho en el archivo en Explorador de soluciones y seleccione **eliminar.**</span><span class="sxs-lookup"><span data-stu-id="9033a-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="9033a-204">Ahora agregue un nuevo controlador, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="9033a-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="9033a-205">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="9033a-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="9033a-206">Seleccione **Agregar** y, a continuación, seleccione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="9033a-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="9033a-207">En el Asistente para **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="9033a-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="9033a-208">En la lista desplegable **plantilla** , seleccione **controlador de API vacío**.</span><span class="sxs-lookup"><span data-stu-id="9033a-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="9033a-209">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="9033a-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="9033a-210">No es necesario colocar los controladores en una carpeta denominada Controllers.</span><span class="sxs-lookup"><span data-stu-id="9033a-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="9033a-211">El nombre de la carpeta no es importante; es simplemente una manera cómoda de organizar los archivos de código fuente.</span><span class="sxs-lookup"><span data-stu-id="9033a-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>

<span data-ttu-id="9033a-212">El Asistente para **agregar controladores** creará un archivo denominado ProductsController.CS en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="9033a-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="9033a-213">Si este archivo no está abierto, haga doble clic en el archivo para abrirlo.</span><span class="sxs-lookup"><span data-stu-id="9033a-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="9033a-214">Agregue la siguiente instrucción **using** :</span><span class="sxs-lookup"><span data-stu-id="9033a-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="9033a-215">Agregue un campo que contenga una instancia de **IProductRepository** .</span><span class="sxs-lookup"><span data-stu-id="9033a-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="9033a-216">Llamar a `new ProductRepository()` en el controlador no es el mejor diseño, ya que une el controlador a una implementación determinada de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="9033a-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="9033a-217">Para obtener un mejor enfoque, consulte [uso de la resolución de dependencia de la API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="9033a-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>

## <a name="getting-a-resource"></a><span data-ttu-id="9033a-218">Obtención de un recurso</span><span class="sxs-lookup"><span data-stu-id="9033a-218">Getting a Resource</span></span>

<span data-ttu-id="9033a-219">La API de ProductStore expondrá varios &quot;leer&quot; acciones como métodos GET de HTTP.</span><span class="sxs-lookup"><span data-stu-id="9033a-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="9033a-220">Cada acción se corresponderá con un método de la clase `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="9033a-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="9033a-221">Acción de</span><span class="sxs-lookup"><span data-stu-id="9033a-221">Action</span></span> | <span data-ttu-id="9033a-222">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="9033a-222">HTTP method</span></span> | <span data-ttu-id="9033a-223">URI relativo</span><span class="sxs-lookup"><span data-stu-id="9033a-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9033a-224">Obtener una lista de todos los productos</span><span class="sxs-lookup"><span data-stu-id="9033a-224">Get a list of all products</span></span> | <span data-ttu-id="9033a-225">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-225">GET</span></span> | <span data-ttu-id="9033a-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="9033a-226">/api/products</span></span> |
| <span data-ttu-id="9033a-227">Obtener un producto por identificador</span><span class="sxs-lookup"><span data-stu-id="9033a-227">Get a product by ID</span></span> | <span data-ttu-id="9033a-228">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-228">GET</span></span> | <span data-ttu-id="9033a-229">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="9033a-229">/api/products/*id*</span></span> |
| <span data-ttu-id="9033a-230">Obtener un producto por categoría</span><span class="sxs-lookup"><span data-stu-id="9033a-230">Get a product by category</span></span> | <span data-ttu-id="9033a-231">GET</span><span class="sxs-lookup"><span data-stu-id="9033a-231">GET</span></span> | <span data-ttu-id="9033a-232">/API/Products? categoría =*categoría*</span><span class="sxs-lookup"><span data-stu-id="9033a-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="9033a-233">Para obtener la lista de todos los productos, agregue este método a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="9033a-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="9033a-234">El nombre del método comienza con &quot;obtener&quot;, por lo que, por Convención, se asigna a las solicitudes GET.</span><span class="sxs-lookup"><span data-stu-id="9033a-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="9033a-235">Además, dado que el método no tiene parámetros, se asigna a un URI que no contiene un *identificador de&quot;&quot;* segmento de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="9033a-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="9033a-236">Para obtener un producto por identificador, agregue este método a la clase `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="9033a-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="9033a-237">Este nombre de método también empieza por &quot;obtener&quot;, pero el método tiene un parámetro denominado *ID*. Este parámetro se asigna al &quot;ID&quot; segmento de la ruta de acceso del identificador URI.</span><span class="sxs-lookup"><span data-stu-id="9033a-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="9033a-238">El marco de trabajo de ASP.NET Web API convierte automáticamente el identificador en el tipo de datos correcto (**int**) del parámetro.</span><span class="sxs-lookup"><span data-stu-id="9033a-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="9033a-239">El método GetProduct produce una excepción de tipo **HttpResponseException** si el *identificador* no es válido.</span><span class="sxs-lookup"><span data-stu-id="9033a-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="9033a-240">El marco de trabajo traducirá esta excepción a un error 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="9033a-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="9033a-241">Por último, agregue un método para buscar productos por Categoría:</span><span class="sxs-lookup"><span data-stu-id="9033a-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="9033a-242">Si el URI de solicitud tiene una cadena de consulta, la API Web intenta coincidir los parámetros de consulta con los parámetros del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="9033a-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="9033a-243">Por lo tanto, un URI con el formato "API/Products? Category =*Category*" se asignará a este método.</span><span class="sxs-lookup"><span data-stu-id="9033a-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="9033a-244">Creación de un recurso</span><span class="sxs-lookup"><span data-stu-id="9033a-244">Creating a Resource</span></span>

<span data-ttu-id="9033a-245">A continuación, agregaremos un método a la clase `ProductsController` para crear un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="9033a-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="9033a-246">A continuación se muestra una implementación sencilla del método:</span><span class="sxs-lookup"><span data-stu-id="9033a-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="9033a-247">Tenga en cuenta dos cosas sobre este método:</span><span class="sxs-lookup"><span data-stu-id="9033a-247">Note two things about this method:</span></span>

- <span data-ttu-id="9033a-248">El nombre del método comienza con &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="9033a-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="9033a-249">Para crear un nuevo producto, el cliente envía una solicitud HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="9033a-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="9033a-250">El método toma un parámetro de tipo product.</span><span class="sxs-lookup"><span data-stu-id="9033a-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="9033a-251">En Web API, los parámetros con tipos complejos se deserializan del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9033a-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="9033a-252">Por lo tanto, esperamos que el cliente envíe una representación serializada de un objeto product, en formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="9033a-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="9033a-253">Esta implementación funcionará, pero no es bastante completa.</span><span class="sxs-lookup"><span data-stu-id="9033a-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="9033a-254">Idealmente, nos gustaría que la respuesta HTTP incluira lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9033a-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="9033a-255">**Código de respuesta:** De forma predeterminada, el marco de la API Web establece el código de estado de la respuesta en 200 (correcto).</span><span class="sxs-lookup"><span data-stu-id="9033a-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="9033a-256">Pero según el protocolo HTTP/1.1, cuando una solicitud POST tiene como resultado la creación de un recurso, el servidor debe responder con el estado 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="9033a-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="9033a-257">**Ubicación:** Cuando el servidor crea un recurso, debe incluir el URI del nuevo recurso en el encabezado de ubicación de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9033a-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="9033a-258">ASP.NET Web API facilita la manipulación del mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="9033a-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="9033a-259">Esta es la implementación mejorada:</span><span class="sxs-lookup"><span data-stu-id="9033a-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="9033a-260">Observe que el tipo de valor devuelto del método es ahora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="9033a-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="9033a-261">Al devolver un **HttpResponseMessage** en lugar de un producto, se pueden controlar los detalles del mensaje de respuesta http, incluido el código de estado y el encabezado de ubicación.</span><span class="sxs-lookup"><span data-stu-id="9033a-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="9033a-262">El método **CreateResponse** crea una **HttpResponseMessage** y escribe automáticamente una representación serializada del objeto Product en el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="9033a-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="9033a-263">En este ejemplo no se valida el `Product`.</span><span class="sxs-lookup"><span data-stu-id="9033a-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="9033a-264">Para obtener información sobre la validación de modelos, vea [validación de modelos en ASP.net web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="9033a-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

## <a name="updating-a-resource"></a><span data-ttu-id="9033a-265">Actualización de un recurso</span><span class="sxs-lookup"><span data-stu-id="9033a-265">Updating a Resource</span></span>

<span data-ttu-id="9033a-266">Actualizar un producto con PUT es sencillo:</span><span class="sxs-lookup"><span data-stu-id="9033a-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="9033a-267">El nombre del método comienza con &quot;Put...&quot;, por lo que la API Web lo hace coincidir con las solicitudes PUT.</span><span class="sxs-lookup"><span data-stu-id="9033a-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="9033a-268">El método toma dos parámetros, el identificador de producto y el producto actualizado.</span><span class="sxs-lookup"><span data-stu-id="9033a-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="9033a-269">El parámetro *ID* se toma de la ruta de acceso del URI y el parámetro *Product* se deserializa del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9033a-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="9033a-270">De forma predeterminada, el marco de ASP.NET Web API toma tipos de parámetros simples de la ruta y los tipos complejos del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="9033a-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="9033a-271">Eliminar un recurso</span><span class="sxs-lookup"><span data-stu-id="9033a-271">Deleting a Resource</span></span>

<span data-ttu-id="9033a-272">Para eliminar un recurso, defina una "eliminar..." forma.</span><span class="sxs-lookup"><span data-stu-id="9033a-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="9033a-273">Si una solicitud de eliminación se realiza correctamente, puede devolver el estado 200 (correcto) con un cuerpo de entidad que describe el estado. Estado 202 (aceptado) si la eliminación todavía está pendiente; o el estado 204 (sin contenido) sin cuerpo de entidad.</span><span class="sxs-lookup"><span data-stu-id="9033a-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="9033a-274">En este caso, el método `DeleteProduct` tiene un tipo de valor devuelto `void`, por lo que ASP.NET Web API traduce automáticamente en el código de estado 204 (sin contenido).</span><span class="sxs-lookup"><span data-stu-id="9033a-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
