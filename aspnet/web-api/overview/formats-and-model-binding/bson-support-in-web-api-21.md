---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2,1-ASP.NET 4. x
author: MikeWasson
description: muestra cómo usar BSON en un controlador de API Web (servidor) y en una aplicación cliente de .NET para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519340"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="5912a-103">Compatibilidad con BSON en ASP.NET Web API 2,1</span><span class="sxs-lookup"><span data-stu-id="5912a-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="5912a-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5912a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5912a-105">En este tema se muestra cómo usar BSON en el controlador de API Web (lado servidor) y en una aplicación cliente .NET.</span><span class="sxs-lookup"><span data-stu-id="5912a-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="5912a-106">Web API 2,1 presenta compatibilidad con BSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="5912a-107">¿Qué es BSON?</span><span class="sxs-lookup"><span data-stu-id="5912a-107">What is BSON?</span></span>

<span data-ttu-id="5912a-108">[Bson](http://bsonspec.org/) es un formato de serialización binaria.</span><span class="sxs-lookup"><span data-stu-id="5912a-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="5912a-109">"BSON" representa "JSON binario", pero BSON y JSON se serializan de forma muy diferente.</span><span class="sxs-lookup"><span data-stu-id="5912a-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="5912a-110">BSON es "JSON-like", porque los objetos se representan como pares de nombre y valor, similares a JSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="5912a-111">A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, no como cadenas</span><span class="sxs-lookup"><span data-stu-id="5912a-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="5912a-112">BSON se diseñó para ser ligero, fácil de examinar y rápido para codificar y descodificar.</span><span class="sxs-lookup"><span data-stu-id="5912a-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="5912a-113">BSON tiene un tamaño comparable en JSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="5912a-114">En función de los datos, una carga BSON puede ser menor o mayor que una carga JSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="5912a-115">Para serializar datos binarios, como un archivo de imagen, BSON es menor que JSON, ya que los datos binarios no están codificados en Base64.</span><span class="sxs-lookup"><span data-stu-id="5912a-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="5912a-116">Los documentos BSON son fáciles de examinar porque los elementos tienen el prefijo de un campo de longitud, por lo que un analizador puede omitir elementos sin descodificarlos.</span><span class="sxs-lookup"><span data-stu-id="5912a-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="5912a-117">La codificación y la descodificación son eficaces, ya que los tipos de datos numéricos se almacenan como números, no como cadenas.</span><span class="sxs-lookup"><span data-stu-id="5912a-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="5912a-118">Los clientes nativos, como las aplicaciones cliente de .NET, pueden beneficiarse del uso de BSON en lugar de formatos basados en texto como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="5912a-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="5912a-119">En el caso de los clientes del explorador, probablemente querrá usar JSON, ya que JavaScript puede convertir directamente la carga de JSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="5912a-120">Afortunadamente, Web API usa la [negociación de contenido](content-negotiation.md), por lo que la API puede admitir ambos formatos y permitir que el cliente elija.</span><span class="sxs-lookup"><span data-stu-id="5912a-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="5912a-121">Habilitación de BSON en el servidor</span><span class="sxs-lookup"><span data-stu-id="5912a-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="5912a-122">En la configuración de la API Web, agregue **BsonMediaTypeFormatter** a la colección de formateadores.</span><span class="sxs-lookup"><span data-stu-id="5912a-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="5912a-123">Ahora, si el cliente solicita "Application/bson", la API Web usará el formateador BSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="5912a-124">Para asociar BSON a otros tipos de medios, agréguelos a la colección SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="5912a-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="5912a-125">El código siguiente agrega "Application/vnd. contoso" a los tipos de medios admitidos:</span><span class="sxs-lookup"><span data-stu-id="5912a-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="5912a-126">Sesión HTTP de ejemplo</span><span class="sxs-lookup"><span data-stu-id="5912a-126">Example HTTP Session</span></span>

<span data-ttu-id="5912a-127">En este ejemplo, usaremos la siguiente clase de modelo junto con un controlador de API web simple:</span><span class="sxs-lookup"><span data-stu-id="5912a-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="5912a-128">Un cliente puede enviar la siguiente solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="5912a-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="5912a-129">Esta es la respuesta:</span><span class="sxs-lookup"><span data-stu-id="5912a-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="5912a-130">Aquí he reemplazado los datos binarios por &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="5912a-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="5912a-131">En la siguiente captura de pantalla de Fiddler se muestran los valores hexadecimales sin formato.</span><span class="sxs-lookup"><span data-stu-id="5912a-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="5912a-132">Uso de BSON con HttpClient</span><span class="sxs-lookup"><span data-stu-id="5912a-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="5912a-133">Las aplicaciones de clientes de .NET pueden usar el formateador BSON con **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="5912a-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="5912a-134">Para obtener más información sobre **HttpClient**, consulte [llamada a una API Web desde un cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="5912a-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="5912a-135">El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5912a-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="5912a-136">Para solicitar BSON desde el servidor, establezca el encabezado Accept en "Application/bson":</span><span class="sxs-lookup"><span data-stu-id="5912a-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="5912a-137">Para deserializar el cuerpo de la respuesta, use **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="5912a-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="5912a-138">Este formateador no está en la colección de formateadores predeterminados, por lo que tiene que especificarlo al leer el cuerpo de la respuesta:</span><span class="sxs-lookup"><span data-stu-id="5912a-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="5912a-139">En el ejemplo siguiente se muestra cómo enviar una solicitud POST que contiene BSON.</span><span class="sxs-lookup"><span data-stu-id="5912a-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="5912a-140">Gran parte de este código es igual que el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="5912a-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="5912a-141">Pero en el método **PostAsync** , especifique **BsonMediaTypeFormatter** como el formateador:</span><span class="sxs-lookup"><span data-stu-id="5912a-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="5912a-142">Serializar tipos primitivos de nivel superior</span><span class="sxs-lookup"><span data-stu-id="5912a-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="5912a-143">Cada documento de BSON es una lista de pares clave-valor. La especificación BSON no define una sintaxis para serializar un solo valor sin formato, como un entero o una cadena.</span><span class="sxs-lookup"><span data-stu-id="5912a-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="5912a-144">Para evitar esta limitación, el **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial.</span><span class="sxs-lookup"><span data-stu-id="5912a-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="5912a-145">Antes de la serialización, convierte el valor en un par clave-valor con la clave "Value".</span><span class="sxs-lookup"><span data-stu-id="5912a-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="5912a-146">Por ejemplo, supongamos que el controlador de API devuelve un entero:</span><span class="sxs-lookup"><span data-stu-id="5912a-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="5912a-147">Antes de la serialización, el formateador BSON lo convierte al siguiente par clave-valor:</span><span class="sxs-lookup"><span data-stu-id="5912a-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="5912a-148">Al deserializar, el formateador vuelve a convertir los datos al valor original.</span><span class="sxs-lookup"><span data-stu-id="5912a-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="5912a-149">Sin embargo, los clientes que usen un analizador de BSON diferente deberán controlar este caso, si la API Web devuelve valores sin formato.</span><span class="sxs-lookup"><span data-stu-id="5912a-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="5912a-150">En general, debe considerar la posibilidad de devolver datos estructurados, en lugar de valores sin formato.</span><span class="sxs-lookup"><span data-stu-id="5912a-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5912a-151">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5912a-151">Additional Resources</span></span>

[<span data-ttu-id="5912a-152">Ejemplo de BSON de API Web</span><span class="sxs-lookup"><span data-stu-id="5912a-152">Web API BSON Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[<span data-ttu-id="5912a-153">Formateadores multimedia</span><span class="sxs-lookup"><span data-stu-id="5912a-153">Media Formatters</span></span>](media-formatters.md)
