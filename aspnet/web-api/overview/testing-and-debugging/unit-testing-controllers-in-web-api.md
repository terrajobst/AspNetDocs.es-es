---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Controladores de pruebas unitarias en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: En este tema se describen algunas técnicas específicas para los controladores de pruebas unitarias en Web API 2. Antes de leer este tema, puede que desee leer la unidad del tutorial...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447007"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a><span data-ttu-id="7ce08-104">Controladores de pruebas unitarias en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="7ce08-104">Unit Testing Controllers in ASP.NET Web API 2</span></span>

<span data-ttu-id="7ce08-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7ce08-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="7ce08-106">En este tema se describen algunas técnicas específicas para los controladores de pruebas unitarias en Web API 2.</span><span class="sxs-lookup"><span data-stu-id="7ce08-106">This topic describes some specific techniques for unit testing controllers in Web API 2.</span></span> <span data-ttu-id="7ce08-107">Antes de leer este tema, puede que desee leer las [pruebas unitarias del tutorial ASP.net web API 2](unit-testing-with-aspnet-web-api.md), que muestra cómo agregar un proyecto de prueba unitaria a la solución.</span><span class="sxs-lookup"><span data-stu-id="7ce08-107">Before reading this topic, you might want to read the tutorial [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), which shows how to add a unit-test project to your solution.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7ce08-108">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="7ce08-108">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="7ce08-109">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7ce08-109">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="7ce08-110">API Web 2</span><span class="sxs-lookup"><span data-stu-id="7ce08-110">Web API 2</span></span>
> - <span data-ttu-id="7ce08-111">[MOQ](https://github.com/Moq) 4.5.30</span><span class="sxs-lookup"><span data-stu-id="7ce08-111">[Moq](https://github.com/Moq) 4.5.30</span></span>

> [!NOTE]
> <span data-ttu-id="7ce08-112">He usado MOQ, pero la misma idea se aplica a cualquier marco ficticio.</span><span class="sxs-lookup"><span data-stu-id="7ce08-112">I used Moq, but the same idea applies to any mocking framework.</span></span> <span data-ttu-id="7ce08-113">MOQ 4.5.30 (y versiones posteriores) es compatible con Visual Studio 2017, Roslyn y .NET 4,5 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="7ce08-113">Moq 4.5.30 (and later) supports Visual Studio 2017, Roslyn and .NET 4.5 and later versions.</span></span>

<span data-ttu-id="7ce08-114">Un patrón común en las pruebas unitarias es &quot;Arrange-Act-Assert&quot;:</span><span class="sxs-lookup"><span data-stu-id="7ce08-114">A common pattern in unit tests is &quot;arrange-act-assert&quot;:</span></span>

- <span data-ttu-id="7ce08-115">Organizar: Configure los requisitos previos para que se ejecute la prueba.</span><span class="sxs-lookup"><span data-stu-id="7ce08-115">Arrange: Set up any prerequisites for the test to run.</span></span>
- <span data-ttu-id="7ce08-116">Act: realice la prueba.</span><span class="sxs-lookup"><span data-stu-id="7ce08-116">Act: Perform the test.</span></span>
- <span data-ttu-id="7ce08-117">Assert: Compruebe que la prueba se ha realizado correctamente.</span><span class="sxs-lookup"><span data-stu-id="7ce08-117">Assert: Verify that the test succeeded.</span></span>

<span data-ttu-id="7ce08-118">En el paso de organización, a menudo usará objetos ficticios o de código auxiliar.</span><span class="sxs-lookup"><span data-stu-id="7ce08-118">In the arrange step, you will often use mock or stub objects.</span></span> <span data-ttu-id="7ce08-119">Esto minimiza el número de dependencias, por lo que la prueba se centra en probar una cosa.</span><span class="sxs-lookup"><span data-stu-id="7ce08-119">That minimizes the number of dependencies, so the test is focused on testing one thing.</span></span>

<span data-ttu-id="7ce08-120">Estas son algunas de las cosas que debe realizar en la prueba unitaria en los controladores de la API Web:</span><span class="sxs-lookup"><span data-stu-id="7ce08-120">Here are some things that you should unit test in your Web API controllers:</span></span>

- <span data-ttu-id="7ce08-121">La acción devuelve el tipo de respuesta correcto.</span><span class="sxs-lookup"><span data-stu-id="7ce08-121">The action returns the correct type of response.</span></span>
- <span data-ttu-id="7ce08-122">Los parámetros no válidos devuelven la respuesta de error correcta.</span><span class="sxs-lookup"><span data-stu-id="7ce08-122">Invalid parameters return the correct error response.</span></span>
- <span data-ttu-id="7ce08-123">La acción llama al método correcto en el repositorio o el nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="7ce08-123">The action calls the correct method on the repository or service layer.</span></span>
- <span data-ttu-id="7ce08-124">Si la respuesta incluye un modelo de dominio, compruebe el tipo de modelo.</span><span class="sxs-lookup"><span data-stu-id="7ce08-124">If the response includes a domain model, verify the model type.</span></span>

<span data-ttu-id="7ce08-125">Estos son algunos de los aspectos generales para probar, pero los detalles dependen de la implementación del controlador.</span><span class="sxs-lookup"><span data-stu-id="7ce08-125">These are some of the general things to test, but the specifics depend on your controller implementation.</span></span> <span data-ttu-id="7ce08-126">En concreto, hace una gran diferencia si las acciones del controlador devuelven **HttpResponseMessage** o **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-126">In particular, it makes a big difference whether your controller actions return **HttpResponseMessage** or **IHttpActionResult**.</span></span> <span data-ttu-id="7ce08-127">Para obtener más información sobre estos tipos de resultado, consulte [resultados de la acción en Web API 2](../getting-started-with-aspnet-web-api/action-results.md).</span><span class="sxs-lookup"><span data-stu-id="7ce08-127">For more information about these result types, see [Action Results in Web Api 2](../getting-started-with-aspnet-web-api/action-results.md).</span></span>

## <a name="testing-actions-that-return-httpresponsemessage"></a><span data-ttu-id="7ce08-128">Acciones de prueba que devuelven HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="7ce08-128">Testing Actions that Return HttpResponseMessage</span></span>

<span data-ttu-id="7ce08-129">A continuación se muestra un ejemplo de un controlador cuyas acciones devuelven **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-129">Here is an example of a controller whose actions return **HttpResponseMessage**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

<span data-ttu-id="7ce08-130">Observe que el controlador utiliza la inserción de dependencias para insertar un `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="7ce08-130">Notice the controller uses dependency injection to inject an `IProductRepository`.</span></span> <span data-ttu-id="7ce08-131">Esto hace que el controlador sea más comprobable, ya que puede insertar un repositorio ficticio.</span><span class="sxs-lookup"><span data-stu-id="7ce08-131">That makes the controller more testable, because you can inject a mock repository.</span></span> <span data-ttu-id="7ce08-132">La prueba unitaria siguiente comprueba que el método `Get` escribe un `Product` en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7ce08-132">The following unit test verifies that the `Get` method writes a `Product` to the response body.</span></span> <span data-ttu-id="7ce08-133">Supongamos que `repository` es un `IProductRepository`ficticio.</span><span class="sxs-lookup"><span data-stu-id="7ce08-133">Assume that `repository` is a mock `IProductRepository`.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

<span data-ttu-id="7ce08-134">Es importante establecer la **solicitud** y la **configuración** en el controlador.</span><span class="sxs-lookup"><span data-stu-id="7ce08-134">It's important to set **Request** and **Configuration** on the controller.</span></span> <span data-ttu-id="7ce08-135">De lo contrario, se producirá un error en la prueba con una **excepción ArgumentNullException** o **InvalidOperationException**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-135">Otherwise, the test will fail with an **ArgumentNullException** or **InvalidOperationException**.</span></span>

## <a name="testing-link-generation"></a><span data-ttu-id="7ce08-136">Probar la generación de vínculos</span><span class="sxs-lookup"><span data-stu-id="7ce08-136">Testing Link Generation</span></span>

<span data-ttu-id="7ce08-137">El método `Post` llama a **UrlHelper. Link** para crear vínculos en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7ce08-137">The `Post` method calls **UrlHelper.Link** to create links in the response.</span></span> <span data-ttu-id="7ce08-138">Esto requiere un poco más de configuración en la prueba unitaria:</span><span class="sxs-lookup"><span data-stu-id="7ce08-138">This requires a little more setup in the unit test:</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

<span data-ttu-id="7ce08-139">La clase **UrlHelper** necesita la dirección URL de la solicitud y los datos de ruta, por lo que la prueba tiene que establecer valores para ellos.</span><span class="sxs-lookup"><span data-stu-id="7ce08-139">The **UrlHelper** class needs the request URL and route data, so the test has to set values for these.</span></span> <span data-ttu-id="7ce08-140">Otra opción es Mock o stub **UrlHelper**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-140">Another option is mock or stub **UrlHelper**.</span></span> <span data-ttu-id="7ce08-141">Con este enfoque, reemplazará el valor predeterminado de [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) por una versión de código auxiliar o ficticio que devuelva un valor fijo.</span><span class="sxs-lookup"><span data-stu-id="7ce08-141">With this approach, you replace the default value of [ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) with a mock or stub version that returns a fixed value.</span></span>

<span data-ttu-id="7ce08-142">Vamos a escribir la prueba con el marco [MOQ](https://github.com/Moq) .</span><span class="sxs-lookup"><span data-stu-id="7ce08-142">Let's rewrite the test using the [Moq](https://github.com/Moq) framework.</span></span> <span data-ttu-id="7ce08-143">Instale el paquete NuGet de `Moq` en el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="7ce08-143">Install the `Moq` NuGet package in the test project.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

<span data-ttu-id="7ce08-144">En esta versión, no es necesario configurar ningún dato de ruta, porque el **UrlHelper** ficticio devuelve una cadena de constante.</span><span class="sxs-lookup"><span data-stu-id="7ce08-144">In this version, you don't need to set up any route data, because the mock **UrlHelper** returns a constant string.</span></span>

## <a name="testing-actions-that-return-ihttpactionresult"></a><span data-ttu-id="7ce08-145">Acciones de prueba que devuelven IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="7ce08-145">Testing Actions that Return IHttpActionResult</span></span>

<span data-ttu-id="7ce08-146">En Web API 2, una acción de controlador puede devolver **IHttpActionResult**, que es análogo a **ACTIONRESULT** en ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7ce08-146">In Web API 2, a controller action can return **IHttpActionResult**, which is analogous to **ActionResult** in ASP.NET MVC.</span></span> <span data-ttu-id="7ce08-147">La interfaz **IHttpActionResult** define un patrón de comando para crear respuestas http.</span><span class="sxs-lookup"><span data-stu-id="7ce08-147">The **IHttpActionResult** interface defines a command pattern for creating HTTP responses.</span></span> <span data-ttu-id="7ce08-148">En lugar de crear la respuesta directamente, el controlador devuelve un **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-148">Instead of creating the response directly, the controller returns an **IHttpActionResult**.</span></span> <span data-ttu-id="7ce08-149">Más adelante, la canalización invoca el **IHttpActionResult** para crear la respuesta.</span><span class="sxs-lookup"><span data-stu-id="7ce08-149">Later, the pipeline invokes the **IHttpActionResult** to create the response.</span></span> <span data-ttu-id="7ce08-150">Este enfoque facilita la escritura de pruebas unitarias, ya que se puede omitir una gran cantidad de configuración que se necesita para **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-150">This approach makes it easier to write unit tests, because you can skip a lot of the setup that is needed for **HttpResponseMessage**.</span></span>

<span data-ttu-id="7ce08-151">A continuación se muestra un controlador de ejemplo cuyas acciones devuelven **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-151">Here is an example controller whose actions return **IHttpActionResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

<span data-ttu-id="7ce08-152">En este ejemplo se muestran algunos patrones comunes con **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-152">This example shows some common patterns using **IHttpActionResult**.</span></span> <span data-ttu-id="7ce08-153">Veamos cómo hacer pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="7ce08-153">Let's see how to unit test them.</span></span>

### <a name="action-returns-200-ok-with-a-response-body"></a><span data-ttu-id="7ce08-154">La acción devuelve 200 (correcto) con un cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="7ce08-154">Action returns 200 (OK) with a response body</span></span>

<span data-ttu-id="7ce08-155">El método `Get` llama a `Ok(product)` si se encuentra el producto.</span><span class="sxs-lookup"><span data-stu-id="7ce08-155">The `Get` method calls `Ok(product)` if the product is found.</span></span> <span data-ttu-id="7ce08-156">En la prueba unitaria, asegúrese de que el tipo de valor devuelto es **OkNegotiatedContentResult** y que el producto devuelto tiene el identificador correcto.</span><span class="sxs-lookup"><span data-stu-id="7ce08-156">In the unit test, make sure the return type is **OkNegotiatedContentResult** and the returned product has the right ID.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

<span data-ttu-id="7ce08-157">Tenga en cuenta que la prueba unitaria no ejecuta el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="7ce08-157">Notice that the unit test doesn't execute the action result.</span></span> <span data-ttu-id="7ce08-158">Puede suponer que el resultado de la acción crea la respuesta HTTP correctamente.</span><span class="sxs-lookup"><span data-stu-id="7ce08-158">You can assume the action result creates the HTTP response correctly.</span></span> <span data-ttu-id="7ce08-159">(Este es el motivo por el que el marco de trabajo de la API Web tiene sus propias pruebas unitarias).</span><span class="sxs-lookup"><span data-stu-id="7ce08-159">(That's why the Web API framework has its own unit tests!)</span></span>

### <a name="action-returns-404-not-found"></a><span data-ttu-id="7ce08-160">La acción devuelve 404 (no encontrado)</span><span class="sxs-lookup"><span data-stu-id="7ce08-160">Action returns 404 (Not Found)</span></span>

<span data-ttu-id="7ce08-161">El método `Get` llama a `NotFound()` si no se encuentra el producto.</span><span class="sxs-lookup"><span data-stu-id="7ce08-161">The `Get` method calls `NotFound()` if the product is not found.</span></span> <span data-ttu-id="7ce08-162">En este caso, la prueba unitaria solo comprueba si el tipo de valor devuelto es **NotFoundResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-162">For this case, the unit test just checks if the return type is **NotFoundResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a><span data-ttu-id="7ce08-163">La acción devuelve 200 (correcto) sin cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="7ce08-163">Action returns 200 (OK) with no response body</span></span>

<span data-ttu-id="7ce08-164">El método `Delete` llama a `Ok()` para devolver una respuesta HTTP 200 vacía.</span><span class="sxs-lookup"><span data-stu-id="7ce08-164">The `Delete` method calls `Ok()` to return an empty HTTP 200 response.</span></span> <span data-ttu-id="7ce08-165">Al igual que en el ejemplo anterior, la prueba unitaria comprueba el tipo de valor devuelto, en este caso **OkResult**.</span><span class="sxs-lookup"><span data-stu-id="7ce08-165">Like the previous example, the unit test checks the return type, in this case **OkResult**.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a><span data-ttu-id="7ce08-166">La acción devuelve 201 (creado) con un encabezado de ubicación</span><span class="sxs-lookup"><span data-stu-id="7ce08-166">Action returns 201 (Created) with a Location header</span></span>

<span data-ttu-id="7ce08-167">El método `Post` llama a `CreatedAtRoute` para devolver una respuesta HTTP 201 con un URI en el encabezado Location.</span><span class="sxs-lookup"><span data-stu-id="7ce08-167">The `Post` method calls `CreatedAtRoute` to return an HTTP 201 response with a URI in the Location header.</span></span> <span data-ttu-id="7ce08-168">En la prueba unitaria, compruebe que la acción establece los valores de enrutamiento correctos.</span><span class="sxs-lookup"><span data-stu-id="7ce08-168">In the unit test, verify that the action sets the correct routing values.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a><span data-ttu-id="7ce08-169">La acción devuelve otro 2xx con un cuerpo de respuesta</span><span class="sxs-lookup"><span data-stu-id="7ce08-169">Action returns another 2xx with a response body</span></span>

<span data-ttu-id="7ce08-170">El método `Put` llama a `Content` para devolver una respuesta HTTP 202 (aceptado) con un cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="7ce08-170">The `Put` method calls `Content` to return an HTTP 202 (Accepted) response with a response body.</span></span> <span data-ttu-id="7ce08-171">Este caso es similar a devolver 200 (correcto), pero la prueba unitaria también debe comprobar el código de estado.</span><span class="sxs-lookup"><span data-stu-id="7ce08-171">This case is similar to returning 200 (OK), but the unit test should also check the status code.</span></span>

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a><span data-ttu-id="7ce08-172">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7ce08-172">Additional Resources</span></span>

- [<span data-ttu-id="7ce08-173">Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="7ce08-173">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- <span data-ttu-id="7ce08-174">[Escritura de pruebas para un servicio de ASP.net web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (entrada de blog de Youssef Moussaoui).</span><span class="sxs-lookup"><span data-stu-id="7ce08-174">[Writing tests for an ASP.NET Web API service](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (blog post by Youssef Moussaoui).</span></span>
- [<span data-ttu-id="7ce08-175">Depurar ASP.NET Web API con Route Debugger</span><span class="sxs-lookup"><span data-stu-id="7ce08-175">Debugging ASP.NET Web API with Route Debugger</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
