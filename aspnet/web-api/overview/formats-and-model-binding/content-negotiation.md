---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: En ASP.NET Web API la negociación de contenido | Microsoft Docs
author: MikeWasson
description: Describe cómo ASP.NET Web API implementa la negociación de contenido HTTP.
ms.author: riande
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: e936bdfa52f786ec86d3e84eac3cd644225b6f92
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039252"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="59c97-103">Negociación de contenido en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="59c97-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="59c97-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59c97-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="59c97-105">En este artículo se describe cómo ASP.NET Web API implementa la negociación de contenido.</span><span class="sxs-lookup"><span data-stu-id="59c97-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="59c97-106">La especificación de HTTP (RFC 2616) define la negociación de contenido como "el proceso de seleccionar la mejor representación para una respuesta determinada cuando hay varias representaciones".</span><span class="sxs-lookup"><span data-stu-id="59c97-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="59c97-107">El mecanismo principal para la negociación de contenido en HTTP son estos encabezados de solicitud:</span><span class="sxs-lookup"><span data-stu-id="59c97-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="59c97-108">**Aceptar:** Los tipos de medios que son aceptables para la respuesta, por ejemplo, "application/json", "application/xml" o un tipo de medio personalizado como &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="59c97-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="59c97-109">**Accept-Charset:** Los juegos de caracteres son aceptables, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="59c97-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="59c97-110">**Codificación de Aceptar:** Las codificaciones de contenido son aceptables, como gzip.</span><span class="sxs-lookup"><span data-stu-id="59c97-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="59c97-111">**Aceptar-lenguaje:** El idioma preferido natural, como "en-us".</span><span class="sxs-lookup"><span data-stu-id="59c97-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="59c97-112">El servidor también puede mirar otras partes de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="59c97-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="59c97-113">Por ejemplo, si la solicitud contiene un encabezado X-Requested-With, que indica una solicitud AJAX, el servidor podría predeterminado para JSON si no hay ningún encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="59c97-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="59c97-114">En este artículo, daremos un vistazo a cómo la API Web utiliza los encabezados Accept y Accept-Charset.</span><span class="sxs-lookup"><span data-stu-id="59c97-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="59c97-115">(En este momento, no hay ninguna compatibilidad integrada para Accept-Language o Accept-Encoding.)</span><span class="sxs-lookup"><span data-stu-id="59c97-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="59c97-116">Serialización</span><span class="sxs-lookup"><span data-stu-id="59c97-116">Serialization</span></span>

<span data-ttu-id="59c97-117">Si un controlador de API Web devuelve un recurso como tipo de CLR, la canalización serializa el valor devuelto y lo escribe en el cuerpo de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="59c97-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="59c97-118">Por ejemplo, considere la siguiente acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="59c97-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="59c97-119">Un cliente puede enviar esta solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="59c97-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="59c97-120">En respuesta, el servidor podría enviar:</span><span class="sxs-lookup"><span data-stu-id="59c97-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="59c97-121">En este ejemplo, el cliente solicitó JSON, Javascript o "cualquier cosa" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="59c97-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="59c97-122">El servidor responde con una representación JSON de la `Product` objeto.</span><span class="sxs-lookup"><span data-stu-id="59c97-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="59c97-123">Tenga en cuenta que el encabezado Content-Type en la respuesta se establece en &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="59c97-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="59c97-124">También puede devolver un controlador de un **HttpResponseMessage** objeto.</span><span class="sxs-lookup"><span data-stu-id="59c97-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="59c97-125">Para especificar un objeto CLR para el cuerpo de respuesta, llame a la **CreateResponse** método de extensión:</span><span class="sxs-lookup"><span data-stu-id="59c97-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="59c97-126">Esta opción le ofrece más control sobre los detalles de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="59c97-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="59c97-127">Puede establecer el código de estado, agregue los encabezados HTTP y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="59c97-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="59c97-128">El objeto que serializa el recurso se denomina un *formateador media*.</span><span class="sxs-lookup"><span data-stu-id="59c97-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="59c97-129">Formateadores de contenido multimedia que se derivan de la **elemento MediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="59c97-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="59c97-130">API Web proporciona los formateadores de contenido multimedia para XML y JSON, y puede crear formateadores personalizados para admitir otros tipos de medios.</span><span class="sxs-lookup"><span data-stu-id="59c97-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="59c97-131">Para obtener información sobre cómo escribir un formateador personalizado, vea [formateadores de contenido multimedia](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="59c97-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="59c97-132">Contenido cómo funciona la negociación</span><span class="sxs-lookup"><span data-stu-id="59c97-132">How Content Negotiation Works</span></span>

<span data-ttu-id="59c97-133">En primer lugar, la canalización Obtiene el **IContentNegotiator** desde el **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="59c97-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="59c97-134">También obtiene la lista de formateadores de contenido multimedia desde el **HttpConfiguration.Formatters** colección.</span><span class="sxs-lookup"><span data-stu-id="59c97-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="59c97-135">A continuación, llama la canalización **IContentNegotiatior.Negotiate**, pasando:</span><span class="sxs-lookup"><span data-stu-id="59c97-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="59c97-136">El tipo de objeto que se va a serializar</span><span class="sxs-lookup"><span data-stu-id="59c97-136">The type of object to serialize</span></span>
- <span data-ttu-id="59c97-137">La colección de formateadores de contenido multimedia</span><span class="sxs-lookup"><span data-stu-id="59c97-137">The collection of media formatters</span></span>
- <span data-ttu-id="59c97-138">La solicitud HTTP</span><span class="sxs-lookup"><span data-stu-id="59c97-138">The HTTP request</span></span>

<span data-ttu-id="59c97-139">El **Negotiate** método devuelve dos fragmentos de información:</span><span class="sxs-lookup"><span data-stu-id="59c97-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="59c97-140">El formateador que se va a utilizar</span><span class="sxs-lookup"><span data-stu-id="59c97-140">Which formatter to use</span></span>
- <span data-ttu-id="59c97-141">El tipo de medio para la respuesta</span><span class="sxs-lookup"><span data-stu-id="59c97-141">The media type for the response</span></span>

<span data-ttu-id="59c97-142">Si no se encuentra ningún formateador, el **Negotiate** devuelve del método **null**y el cliente recibe HTTP 406 (no aceptable).</span><span class="sxs-lookup"><span data-stu-id="59c97-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="59c97-143">El código siguiente muestra cómo un controlador puede invocar directamente la negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="59c97-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="59c97-144">Este código es equivalente a lo que hace automáticamente la canalización.</span><span class="sxs-lookup"><span data-stu-id="59c97-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="59c97-145">Negociador de contenido predeterminado</span><span class="sxs-lookup"><span data-stu-id="59c97-145">Default Content Negotiator</span></span>

<span data-ttu-id="59c97-146">El **DefaultContentNegotiator** clase proporciona la implementación predeterminada de **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="59c97-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="59c97-147">Usa varios criterios para seleccionar a un formateador.</span><span class="sxs-lookup"><span data-stu-id="59c97-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="59c97-148">En primer lugar, el formateador debe ser capaz de serializar el tipo.</span><span class="sxs-lookup"><span data-stu-id="59c97-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="59c97-149">Esto se comprueba mediante una llamada a **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="59c97-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="59c97-150">A continuación, el negociador de contenido examina cada formateador y evalúa coincide con la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="59c97-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="59c97-151">Para evaluar a la coincidencia, el negociador de contenido examina dos cosas en el formateador:</span><span class="sxs-lookup"><span data-stu-id="59c97-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="59c97-152">El **SupportedMediaTypes** colección, que contiene una lista de tipos de medios admitidos.</span><span class="sxs-lookup"><span data-stu-id="59c97-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="59c97-153">El negociador de contenido que se intenta hacer coincidir esta lista en el encabezado Accept de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="59c97-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="59c97-154">Tenga en cuenta que el encabezado Accept puede incluir intervalos.</span><span class="sxs-lookup"><span data-stu-id="59c97-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="59c97-155">Por ejemplo, "text/plain" es una coincidencia de texto /\* o \* / \*.</span><span class="sxs-lookup"><span data-stu-id="59c97-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="59c97-156">El **MediaTypeMappings** colección, que contiene una lista de **MediaTypeMapping** objetos.</span><span class="sxs-lookup"><span data-stu-id="59c97-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="59c97-157">El **MediaTypeMapping** clase proporciona una forma genérica para que coincida con las solicitudes HTTP con tipos de medios.</span><span class="sxs-lookup"><span data-stu-id="59c97-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="59c97-158">Por ejemplo, podría asignar un encabezado HTTP personalizado para un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="59c97-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="59c97-159">Si hay varios coincide, la coincidencia con el más alto gana factor de calidad.</span><span class="sxs-lookup"><span data-stu-id="59c97-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="59c97-160">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59c97-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="59c97-161">En este ejemplo, application/json tiene un factor de calidad implícita de 1.0, por lo que es preferible a la aplicación/xml.</span><span class="sxs-lookup"><span data-stu-id="59c97-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="59c97-162">Si se encuentra ninguna coincidencia, el negociador de contenido intenta hacer coincidir en el tipo de medio del cuerpo de la solicitud, si existe.</span><span class="sxs-lookup"><span data-stu-id="59c97-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="59c97-163">Por ejemplo, si la solicitud contiene datos JSON, busca el negociador de contenido para un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="59c97-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="59c97-164">Si aún no hay ninguna coincidencia, el negociador de contenido simplemente toma al primer formateador que puede serializar el tipo.</span><span class="sxs-lookup"><span data-stu-id="59c97-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="59c97-165">Seleccionar una codificación de caracteres</span><span class="sxs-lookup"><span data-stu-id="59c97-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="59c97-166">Una vez seleccionado un formateador, el negociador de contenido elige la mejor codificación de caracteres examinando el **SupportedEncodings** propiedad en el formateador y hacerla coincidir con el encabezado Accept-Charset en la solicitud (si existe).</span><span class="sxs-lookup"><span data-stu-id="59c97-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
