---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Llamar a una API Web desde un cliente .NET (C#)-ASP.NET 4.x
author: MikeWasson
description: Este tutorial muestra cómo llamar a una API web desde una aplicación de .NET 4.x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ca3b9424f30f48c7b8c71b850ffeca64244b123b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112844"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="bead7-103">Llamar a una API Web desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="bead7-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="bead7-104">por [Mike Wasson](https://github.com/MikeWasson) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bead7-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bead7-105">[Descargue el proyecto completado](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="bead7-105">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="bead7-106">[Instrucciones de descarga](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bead7-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="bead7-107">Este tutorial muestra cómo llamar a una API web desde una aplicación de .NET mediante [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="bead7-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="bead7-108">En este tutorial, una aplicación cliente se escribe que consume la API web siguiente:</span><span class="sxs-lookup"><span data-stu-id="bead7-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="bead7-109">Acción</span><span class="sxs-lookup"><span data-stu-id="bead7-109">Action</span></span> | <span data-ttu-id="bead7-110">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="bead7-110">HTTP method</span></span> | <span data-ttu-id="bead7-111">URI relativo</span><span class="sxs-lookup"><span data-stu-id="bead7-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bead7-112">Obtener un producto por Id.</span><span class="sxs-lookup"><span data-stu-id="bead7-112">Get a product by ID</span></span> | <span data-ttu-id="bead7-113">GET</span><span class="sxs-lookup"><span data-stu-id="bead7-113">GET</span></span> | <span data-ttu-id="bead7-114">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bead7-114">/api/products/*id*</span></span> |
| <span data-ttu-id="bead7-115">Crear un nuevo producto</span><span class="sxs-lookup"><span data-stu-id="bead7-115">Create a new product</span></span> | <span data-ttu-id="bead7-116">EXPONER</span><span class="sxs-lookup"><span data-stu-id="bead7-116">POST</span></span> | <span data-ttu-id="bead7-117">productos/api /</span><span class="sxs-lookup"><span data-stu-id="bead7-117">/api/products</span></span> |
| <span data-ttu-id="bead7-118">Actualizar un producto</span><span class="sxs-lookup"><span data-stu-id="bead7-118">Update a product</span></span> | <span data-ttu-id="bead7-119">PUT</span><span class="sxs-lookup"><span data-stu-id="bead7-119">PUT</span></span> | <span data-ttu-id="bead7-120">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bead7-120">/api/products/*id*</span></span> |
| <span data-ttu-id="bead7-121">Eliminar un producto</span><span class="sxs-lookup"><span data-stu-id="bead7-121">Delete a product</span></span> | <span data-ttu-id="bead7-122">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="bead7-122">DELETE</span></span> | <span data-ttu-id="bead7-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="bead7-123">/api/products/*id*</span></span> |

<span data-ttu-id="bead7-124">Para obtener información sobre cómo implementar esta API con ASP.NET Web API, consulte [crear una API Web que admite operaciones de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="bead7-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="bead7-125">Por motivos de simplicidad, la aplicación cliente en este tutorial es una aplicación de consola de Windows.</span><span class="sxs-lookup"><span data-stu-id="bead7-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="bead7-126">**HttpClient** también se admite para las aplicaciones de Windows Phone y Windows Store.</span><span class="sxs-lookup"><span data-stu-id="bead7-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="bead7-127">Para obtener más información, consulte [escribir el código de cliente de API de Web para varias plataformas utilizando las bibliotecas portables](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="bead7-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="bead7-128">Crear la aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="bead7-128">Create the Console Application</span></span>

<span data-ttu-id="bead7-129">En Visual Studio, cree una nueva aplicación de consola de Windows denominada **HttpClientSample** y pegue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="bead7-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="bead7-130">El código anterior es la aplicación cliente completa.</span><span class="sxs-lookup"><span data-stu-id="bead7-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="bead7-131">`RunAsync` ejecuciones y bloques hasta que se complete.</span><span class="sxs-lookup"><span data-stu-id="bead7-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="bead7-132">La mayoría **HttpClient** métodos son asincrónicos, ya que realizan operaciones de E/S de red.</span><span class="sxs-lookup"><span data-stu-id="bead7-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="bead7-133">Todas las tareas asincrónicas se realizan dentro de `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="bead7-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="bead7-134">Normalmente, una aplicación no bloquea el subproceso principal, pero esta aplicación no permite ninguna interacción.</span><span class="sxs-lookup"><span data-stu-id="bead7-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="bead7-135">Instalar las bibliotecas de cliente de API Web</span><span class="sxs-lookup"><span data-stu-id="bead7-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="bead7-136">Use el Administrador de paquetes de NuGet para instalar el paquete de bibliotecas de cliente de API Web.</span><span class="sxs-lookup"><span data-stu-id="bead7-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="bead7-137">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="bead7-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="bead7-138">En la consola de administrador de paquetes (PMC), escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bead7-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="bead7-139">El comando anterior agrega los siguientes paquetes NuGet al proyecto:</span><span class="sxs-lookup"><span data-stu-id="bead7-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="bead7-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="bead7-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="bead7-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="bead7-141">Newtonsoft.Json</span></span>

<span data-ttu-id="bead7-142">Json.NET es un popular marco JSON de alto rendimiento para. NET.</span><span class="sxs-lookup"><span data-stu-id="bead7-142">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="bead7-143">Agregar una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="bead7-143">Add a Model Class</span></span>

<span data-ttu-id="bead7-144">Examine la clase `Product`:</span><span class="sxs-lookup"><span data-stu-id="bead7-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="bead7-145">Esta clase coincide con el modelo de datos utilizado por la API web.</span><span class="sxs-lookup"><span data-stu-id="bead7-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="bead7-146">Puede usar una aplicación **HttpClient** para leer un `Product` instancia de una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="bead7-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="bead7-147">La aplicación no tiene que escribir ningún código de deserialización.</span><span class="sxs-lookup"><span data-stu-id="bead7-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="bead7-148">Crear e inicializar HttpClient</span><span class="sxs-lookup"><span data-stu-id="bead7-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="bead7-149">Examinar estático **HttpClient** propiedad:</span><span class="sxs-lookup"><span data-stu-id="bead7-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="bead7-150">**HttpClient** está diseñado para ejecutarse una vez y volver a usar durante la vida de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="bead7-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="bead7-151">Las condiciones siguientes pueden producir en **SocketException** errores:</span><span class="sxs-lookup"><span data-stu-id="bead7-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="bead7-152">Crear un nuevo **HttpClient** instancia por solicitud.</span><span class="sxs-lookup"><span data-stu-id="bead7-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="bead7-153">Servidor con mucha carga.</span><span class="sxs-lookup"><span data-stu-id="bead7-153">Server under heavy load.</span></span>

<span data-ttu-id="bead7-154">Crear un nuevo **HttpClient** instancia por solicitud puede agotar los sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="bead7-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="bead7-155">El siguiente código inicializa el **HttpClient** instancia:</span><span class="sxs-lookup"><span data-stu-id="bead7-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="bead7-156">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="bead7-156">The preceding code:</span></span>

* <span data-ttu-id="bead7-157">Establece el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bead7-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="bead7-158">Cambiar el número de puerto para el puerto usado en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="bead7-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="bead7-159">La aplicación no funcionará a menos que se usa el puerto para la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="bead7-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="bead7-160">Establece el encabezado Accept en "application/json".</span><span class="sxs-lookup"><span data-stu-id="bead7-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="bead7-161">Establecer este encabezado indica al servidor para enviar datos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="bead7-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="bead7-162">Envía una solicitud GET para recuperar un recurso</span><span class="sxs-lookup"><span data-stu-id="bead7-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="bead7-163">El código siguiente envía una solicitud GET para un producto:</span><span class="sxs-lookup"><span data-stu-id="bead7-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="bead7-164">El **GetAsync** método envía la solicitud HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="bead7-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="bead7-165">Cuando finalice el método, devuelve un **HttpResponseMessage** que contiene la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="bead7-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="bead7-166">Si el código de estado en la respuesta es un código correcto, el cuerpo de respuesta contiene la representación JSON de un producto.</span><span class="sxs-lookup"><span data-stu-id="bead7-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="bead7-167">Llame a **ReadAsAsync** para deserializar la carga de JSON para un `Product` instancia.</span><span class="sxs-lookup"><span data-stu-id="bead7-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="bead7-168">El **ReadAsAsync** método es asincrónico porque el cuerpo de respuesta puede ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="bead7-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="bead7-169">**HttpClient** no produce una excepción cuando la respuesta HTTP contiene un código de error.</span><span class="sxs-lookup"><span data-stu-id="bead7-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="bead7-170">En su lugar, el **IsSuccessStatusCode** propiedad es **false** si el estado es un código de error.</span><span class="sxs-lookup"><span data-stu-id="bead7-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="bead7-171">Si lo prefiere tratar los códigos de error HTTP como excepciones, llame a [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) en el objeto de respuesta.</span><span class="sxs-lookup"><span data-stu-id="bead7-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="bead7-172">`EnsureSuccessStatusCode` produce una excepción si el código de estado está fuera del intervalo 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="bead7-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="bead7-173">Tenga en cuenta que **HttpClient** puede producir excepciones por otras razones &mdash; por ejemplo, si la solicitud agota el tiempo.</span><span class="sxs-lookup"><span data-stu-id="bead7-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="bead7-174">Formateadores de tipo de medio para deserializar</span><span class="sxs-lookup"><span data-stu-id="bead7-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="bead7-175">Cuando **ReadAsAsync** se llama sin parámetros, usa un conjunto predeterminado de *formateadores de contenido multimedia* para leer el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="bead7-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="bead7-176">Los formateadores predeterminados admiten JSON, XML y datos codificados de dirección url de formulario.</span><span class="sxs-lookup"><span data-stu-id="bead7-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="bead7-177">En lugar de utilizar los formateadores de forma predeterminada, puede proporcionar una lista de formateadores para la **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="bead7-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="bead7-178">Con una lista de formateadores es útil si tiene un formateador de tipo de medio personalizado:</span><span class="sxs-lookup"><span data-stu-id="bead7-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="bead7-179">Para obtener más información, consulte [formateadores de contenido multimedia en ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="bead7-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="bead7-180">Enviar una solicitud POST para crear un recurso</span><span class="sxs-lookup"><span data-stu-id="bead7-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="bead7-181">El código siguiente envía una solicitud POST que contiene un `Product` instancia en formato JSON:</span><span class="sxs-lookup"><span data-stu-id="bead7-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="bead7-182">El **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="bead7-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="bead7-183">Serializa un objeto a JSON.</span><span class="sxs-lookup"><span data-stu-id="bead7-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="bead7-184">Envía la carga de JSON en una solicitud POST.</span><span class="sxs-lookup"><span data-stu-id="bead7-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="bead7-185">Si la solicitud se realiza correctamente:</span><span class="sxs-lookup"><span data-stu-id="bead7-185">If the request succeeds:</span></span>

* <span data-ttu-id="bead7-186">Debe devolver una respuesta de 201 (creado).</span><span class="sxs-lookup"><span data-stu-id="bead7-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="bead7-187">La respuesta debe incluir la dirección URL de los recursos creados en el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="bead7-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="bead7-188">Enviar una solicitud PUT para actualizar un recurso</span><span class="sxs-lookup"><span data-stu-id="bead7-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="bead7-189">El código siguiente envía una solicitud PUT para actualizar un producto:</span><span class="sxs-lookup"><span data-stu-id="bead7-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="bead7-190">El **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, excepto en que envía una solicitud PUT en lugar de POST.</span><span class="sxs-lookup"><span data-stu-id="bead7-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="bead7-191">Enviar una solicitud DELETE para eliminar un recurso</span><span class="sxs-lookup"><span data-stu-id="bead7-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="bead7-192">El código siguiente envía una solicitud DELETE para eliminar un producto:</span><span class="sxs-lookup"><span data-stu-id="bead7-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="bead7-193">Una solicitud de eliminación, como GET, no tiene un cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="bead7-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="bead7-194">No es necesario especificar el formato JSON o XML con la eliminación.</span><span class="sxs-lookup"><span data-stu-id="bead7-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="bead7-195">Probar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="bead7-195">Test the sample</span></span>

<span data-ttu-id="bead7-196">Para probar la aplicación cliente:</span><span class="sxs-lookup"><span data-stu-id="bead7-196">To test the client app:</span></span>

1. <span data-ttu-id="bead7-197">[Descargar](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) y ejecutar la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="bead7-197">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="bead7-198">[Instrucciones de descarga](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bead7-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="bead7-199">Comprobar que funciona la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="bead7-199">Verify the server app is working.</span></span> <span data-ttu-id="bead7-200">Por ejemplo, `http://localhost:64195/api/products` debe devolver una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="bead7-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="bead7-201">Establecer el URI base para las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="bead7-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="bead7-202">Cambiar el número de puerto para el puerto usado en la aplicación de servidor.</span><span class="sxs-lookup"><span data-stu-id="bead7-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="bead7-203">Ejecute la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="bead7-203">Run the client app.</span></span> <span data-ttu-id="bead7-204">Se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="bead7-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
