---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Resultados de la acción en Web API 2-ASP.NET 4. x
author: MikeWasson
description: Describe cómo ASP.NET Web API convierte el valor devuelto de una acción del controlador en un mensaje de respuesta HTTP en ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985840"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="0ca5e-103">Resultados de la acción en Web API 2</span><span class="sxs-lookup"><span data-stu-id="0ca5e-103">Action Results in Web API 2</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="0ca5e-104">En este tema se describe cómo ASP.NET Web API convierte el valor devuelto de una acción del controlador en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="0ca5e-105">Una acción del controlador de API Web puede devolver cualquiera de los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="0ca5e-106">void</span><span class="sxs-lookup"><span data-stu-id="0ca5e-106">void</span></span>
2. <span data-ttu-id="0ca5e-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="0ca5e-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="0ca5e-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="0ca5e-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="0ca5e-109">Otro tipo</span><span class="sxs-lookup"><span data-stu-id="0ca5e-109">Some other type</span></span>

<span data-ttu-id="0ca5e-110">Dependiendo de cuál de estos se devuelva, la API Web utiliza un mecanismo diferente para crear la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="0ca5e-111">Tipo devuelto</span><span class="sxs-lookup"><span data-stu-id="0ca5e-111">Return type</span></span> | <span data-ttu-id="0ca5e-112">Cómo crea la API Web la respuesta</span><span class="sxs-lookup"><span data-stu-id="0ca5e-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="0ca5e-113">void</span><span class="sxs-lookup"><span data-stu-id="0ca5e-113">void</span></span> | <span data-ttu-id="0ca5e-114">Devolver 204 vacío (sin contenido)</span><span class="sxs-lookup"><span data-stu-id="0ca5e-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="0ca5e-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="0ca5e-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="0ca5e-116">Convertir directamente en un mensaje de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="0ca5e-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="0ca5e-117">**IHttpActionResult**</span></span> | <span data-ttu-id="0ca5e-118">Llame a **ExecuteAsync** para crear un **HttpResponseMessage**y, a continuación, convierta en un mensaje de respuesta http.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="0ca5e-119">Otro tipo</span><span class="sxs-lookup"><span data-stu-id="0ca5e-119">Other type</span></span> | <span data-ttu-id="0ca5e-120">Escribe el valor devuelto serializado en el cuerpo de la respuesta; Devuelve 200 (correcto).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="0ca5e-121">En el resto de este tema se describe cada opción con más detalle.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="0ca5e-122">void</span><span class="sxs-lookup"><span data-stu-id="0ca5e-122">void</span></span>

<span data-ttu-id="0ca5e-123">Si el tipo de valor `void`devuelto es, la API Web simplemente devuelve una respuesta http vacía con el código de estado 204 (sin contenido).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="0ca5e-124">Controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="0ca5e-125">Respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="0ca5e-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="0ca5e-126">HttpResponseMessage</span></span>

<span data-ttu-id="0ca5e-127">Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), la API Web convierte el valor devuelto directamente en un mensaje de respuesta http, utilizando las propiedades del objeto **HttpResponseMessage** para rellenar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="0ca5e-128">Esta opción proporciona un gran control sobre el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="0ca5e-129">Por ejemplo, la siguiente acción del controlador establece el encabezado Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="0ca5e-130">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="0ca5e-131">Si pasa un modelo de dominio al método **CreateResponse** , la API Web utiliza un [formateador de medios](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="0ca5e-132">Web API usa el encabezado Accept en la solicitud para elegir el formateador.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="0ca5e-133">Para obtener más información, vea [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="0ca5e-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="0ca5e-134">IHttpActionResult</span></span>

<span data-ttu-id="0ca5e-135">La interfaz **IHttpActionResult** se presentó en Web API 2.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="0ca5e-136">Esencialmente, define un generador **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="0ca5e-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="0ca5e-137">Estas son algunas ventajas del uso de la interfaz **IHttpActionResult** :</span><span class="sxs-lookup"><span data-stu-id="0ca5e-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="0ca5e-138">Simplifica las [pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de los controladores.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="0ca5e-139">Mueve la lógica común para crear respuestas HTTP en clases independientes.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="0ca5e-140">Hace que la intención de la acción del controlador sea más clara ocultando los detalles de bajo nivel de la construcción de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="0ca5e-141">**IHttpActionResult** contiene un único método, **ExecuteAsync**, que crea de forma asincrónica una instancia de **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="0ca5e-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="0ca5e-142">Si una acción de controlador devuelve un **IHttpActionResult**, la API Web llama al método **ExecuteAsync** para crear un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="0ca5e-143">A continuación, convierte **HttpResponseMessage** en un mensaje de respuesta http.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="0ca5e-144">A continuación se muestra una implementación sencilla de **IHttpActionResult** que crea una respuesta de texto sin formato:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="0ca5e-145">Acción de controlador de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="0ca5e-146">Respuesta:</span><span class="sxs-lookup"><span data-stu-id="0ca5e-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="0ca5e-147">Con mayor frecuencia, se usan las implementaciones de **IHttpActionResult** definidas en el espacio de nombres **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="0ca5e-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="0ca5e-148">La clase **ApiController** define métodos auxiliares que devuelven estos resultados de acción integrados.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="0ca5e-149">En el ejemplo siguiente, si la solicitud no coincide con un ID. de producto existente, el controlador llama a [ApiController. notfound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrada).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="0ca5e-150">De lo contrario, el controlador llama a [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="0ca5e-151">Otros tipos de valor devueltos</span><span class="sxs-lookup"><span data-stu-id="0ca5e-151">Other Return Types</span></span>

<span data-ttu-id="0ca5e-152">Para todos los demás tipos de valor devueltos, la API Web utiliza un [formateador de medios](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="0ca5e-153">La API Web escribe el valor serializado en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="0ca5e-154">El código de estado de la respuesta es 200 (correcto).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="0ca5e-155">Un inconveniente de este enfoque es que no se puede devolver directamente un código de error, como 404.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="0ca5e-156">Sin embargo, puede iniciar una **HttpResponseException** para los códigos de error.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="0ca5e-157">Para obtener más información, vea [control de excepciones en ASP.net web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="0ca5e-158">Web API usa el encabezado Accept en la solicitud para elegir el formateador.</span><span class="sxs-lookup"><span data-stu-id="0ca5e-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="0ca5e-159">Para obtener más información, vea [negociación de contenido](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="0ca5e-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="0ca5e-160">Solicitud de ejemplo</span><span class="sxs-lookup"><span data-stu-id="0ca5e-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="0ca5e-161">Respuesta de ejemplo</span><span class="sxs-lookup"><span data-stu-id="0ca5e-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
