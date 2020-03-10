---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviar datos de formulario HTML en ASP.NET Web API: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: En este artículo se muestra cómo publicar datos de form-urlencoded en un controlador de API Web con ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449245"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="61fa7-103">Enviar datos de formulario HTML en ASP.NET Web API: datos form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="61fa7-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="61fa7-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="61fa7-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="61fa7-105">Parte 1: datos form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="61fa7-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="61fa7-106">En este artículo se muestra cómo publicar datos de form-urlencoded en un controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="61fa7-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="61fa7-107">Información general de formularios HTML</span><span class="sxs-lookup"><span data-stu-id="61fa7-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="61fa7-108">Envío de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="61fa7-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="61fa7-109">Envío de datos de formulario a través de AJAX</span><span class="sxs-lookup"><span data-stu-id="61fa7-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="61fa7-110">Enviar tipos simples</span><span class="sxs-lookup"><span data-stu-id="61fa7-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="61fa7-111">[Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="61fa7-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="61fa7-112">Información general de formularios HTML</span><span class="sxs-lookup"><span data-stu-id="61fa7-112">Overview of HTML Forms</span></span>

<span data-ttu-id="61fa7-113">Los formularios HTML utilizan GET o POST para enviar datos al servidor.</span><span class="sxs-lookup"><span data-stu-id="61fa7-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="61fa7-114">El atributo **Method** del elemento **Form** proporciona el método http:</span><span class="sxs-lookup"><span data-stu-id="61fa7-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="61fa7-115">El método predeterminado es GET.</span><span class="sxs-lookup"><span data-stu-id="61fa7-115">The default method is GET.</span></span> <span data-ttu-id="61fa7-116">Si el formulario usa GET, los datos del formulario se codifican en el URI como una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="61fa7-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="61fa7-117">Si el formulario usa POST, los datos del formulario se colocan en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61fa7-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="61fa7-118">Para los datos enviados, el atributo **enctype** especifica el formato del cuerpo de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="61fa7-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="61fa7-119">enctype</span><span class="sxs-lookup"><span data-stu-id="61fa7-119">enctype</span></span> | <span data-ttu-id="61fa7-120">Description</span><span class="sxs-lookup"><span data-stu-id="61fa7-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="61fa7-121">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="61fa7-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="61fa7-122">Los datos de formulario se codifican como pares de nombre/valor, de forma similar a una cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="61fa7-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="61fa7-123">Este es el formato predeterminado para POST.</span><span class="sxs-lookup"><span data-stu-id="61fa7-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="61fa7-124">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="61fa7-124">multipart/form-data</span></span> | <span data-ttu-id="61fa7-125">Los datos de formulario se codifican como un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="61fa7-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="61fa7-126">Use este formato si va a cargar un archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="61fa7-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="61fa7-127">En la primera parte de este artículo se examina el formato x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="61fa7-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="61fa7-128">La [parte 2](sending-html-form-data-part-2.md) describe MIME con varias partes.</span><span class="sxs-lookup"><span data-stu-id="61fa7-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="61fa7-129">Envío de tipos complejos</span><span class="sxs-lookup"><span data-stu-id="61fa7-129">Sending Complex Types</span></span>

<span data-ttu-id="61fa7-130">Normalmente, enviará un tipo complejo, compuesto por valores tomados de varios controles de formulario.</span><span class="sxs-lookup"><span data-stu-id="61fa7-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="61fa7-131">Considere el siguiente modelo que representa una actualización de estado:</span><span class="sxs-lookup"><span data-stu-id="61fa7-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="61fa7-132">Este es un controlador de API Web que acepta un objeto `Update` a través de POST.</span><span class="sxs-lookup"><span data-stu-id="61fa7-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="61fa7-133">Este controlador usa el [enrutamiento basado en acciones](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), por lo que la plantilla de ruta es &quot;API/{Controller}/{Action}/{id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="61fa7-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="61fa7-134">El cliente publicará los datos en &quot;&quot;/API/updates/Complex.</span><span class="sxs-lookup"><span data-stu-id="61fa7-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="61fa7-135">Ahora vamos a escribir un formulario HTML para que los usuarios envíen una actualización de estado.</span><span class="sxs-lookup"><span data-stu-id="61fa7-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="61fa7-136">Observe que el atributo **Action** del formulario es el URI de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="61fa7-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="61fa7-137">Este es el formulario con algunos valores introducidos en:</span><span class="sxs-lookup"><span data-stu-id="61fa7-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="61fa7-138">Cuando el usuario hace clic en enviar, el explorador envía una solicitud HTTP similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="61fa7-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="61fa7-139">Observe que el cuerpo de la solicitud contiene los datos del formulario, con formato de pares nombre-valor.</span><span class="sxs-lookup"><span data-stu-id="61fa7-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="61fa7-140">Web API convierte automáticamente los pares de nombre y valor en una instancia de la clase `Update`.</span><span class="sxs-lookup"><span data-stu-id="61fa7-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="61fa7-141">Envío de datos de formulario a través de AJAX</span><span class="sxs-lookup"><span data-stu-id="61fa7-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="61fa7-142">Cuando un usuario envía un formulario, el explorador sale de la página actual y representa el cuerpo del mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="61fa7-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="61fa7-143">Es correcto cuando la respuesta es una página HTML.</span><span class="sxs-lookup"><span data-stu-id="61fa7-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="61fa7-144">Sin embargo, con una API Web, el cuerpo de la respuesta suele estar vacío o contener datos estructurados, como JSON.</span><span class="sxs-lookup"><span data-stu-id="61fa7-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="61fa7-145">En ese caso, tiene más sentido enviar los datos del formulario mediante una solicitud AJAX, de modo que la página pueda procesar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="61fa7-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="61fa7-146">En el código siguiente se muestra cómo publicar datos de formulario mediante jQuery.</span><span class="sxs-lookup"><span data-stu-id="61fa7-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="61fa7-147">La función de **envío** de jQuery reemplaza la acción de formulario por una nueva función.</span><span class="sxs-lookup"><span data-stu-id="61fa7-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="61fa7-148">Esto invalida el comportamiento predeterminado del botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="61fa7-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="61fa7-149">La función **Serialize** serializa los datos del formulario en pares de nombre/valor.</span><span class="sxs-lookup"><span data-stu-id="61fa7-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="61fa7-150">Para enviar los datos del formulario al servidor, llame a `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="61fa7-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="61fa7-151">Cuando se completa la solicitud, el controlador `.success()` o `.error()` muestra al usuario un mensaje adecuado.</span><span class="sxs-lookup"><span data-stu-id="61fa7-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="61fa7-152">Enviar tipos simples</span><span class="sxs-lookup"><span data-stu-id="61fa7-152">Sending Simple Types</span></span>

<span data-ttu-id="61fa7-153">En las secciones anteriores, enviamos un tipo complejo, que API Web deserializaba a una instancia de una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="61fa7-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="61fa7-154">También puede enviar tipos simples, como una cadena.</span><span class="sxs-lookup"><span data-stu-id="61fa7-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="61fa7-155">Antes de enviar un tipo simple, considere la posibilidad de ajustar el valor en un tipo complejo en su lugar.</span><span class="sxs-lookup"><span data-stu-id="61fa7-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="61fa7-156">Esto le ofrece las ventajas de la validación de modelos en el lado servidor y facilita la ampliación del modelo si es necesario.</span><span class="sxs-lookup"><span data-stu-id="61fa7-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="61fa7-157">Los pasos básicos para enviar un tipo simple son los mismos, pero hay dos diferencias sutiles.</span><span class="sxs-lookup"><span data-stu-id="61fa7-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="61fa7-158">En primer lugar, en el controlador, debe decorar el nombre del parámetro con el atributo **FromBody** .</span><span class="sxs-lookup"><span data-stu-id="61fa7-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="61fa7-159">De forma predeterminada, Web API intenta obtener tipos simples del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="61fa7-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="61fa7-160">El atributo **FromBody** indica a la API Web que lea el valor del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61fa7-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="61fa7-161">Web API lee el cuerpo de la respuesta una vez como máximo, por lo que solo un parámetro de una acción puede proviene del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="61fa7-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="61fa7-162">Si necesita obtener varios valores del cuerpo de la solicitud, defina un tipo complejo.</span><span class="sxs-lookup"><span data-stu-id="61fa7-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="61fa7-163">En segundo lugar, el cliente debe enviar el valor con el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="61fa7-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="61fa7-164">En concreto, la parte del nombre del par nombre-valor debe estar vacía para un tipo simple.</span><span class="sxs-lookup"><span data-stu-id="61fa7-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="61fa7-165">No todos los exploradores son compatibles con los formularios HTML, pero se crea este formato en el script de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="61fa7-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="61fa7-166">Este es un formulario de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="61fa7-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="61fa7-167">Y este es el script para enviar el valor del formulario.</span><span class="sxs-lookup"><span data-stu-id="61fa7-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="61fa7-168">La única diferencia del script anterior es el argumento pasado a la función **post** .</span><span class="sxs-lookup"><span data-stu-id="61fa7-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="61fa7-169">Puede usar el mismo enfoque para enviar una matriz de tipos simples:</span><span class="sxs-lookup"><span data-stu-id="61fa7-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="61fa7-170">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="61fa7-170">Additional Resources</span></span>

[<span data-ttu-id="61fa7-171">Parte 2: carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="61fa7-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
