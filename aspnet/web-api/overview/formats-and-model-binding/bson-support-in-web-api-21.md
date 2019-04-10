---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2.1 - ASP.NET 4.x
author: MikeWasson
description: se muestra cómo usar BSON en un controlador de Web API (servidor) y en una aplicación de cliente de .NET para ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 911e2abcfd277075b3cba71e624ec6390b99a15e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59382239"
---
# <a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="86687-103">Compatibilidad con BSON en ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="86687-103">BSON Support in ASP.NET Web API 2.1</span></span>

<span data-ttu-id="86687-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="86687-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="86687-105">Este tema muestra cómo usar BSON en el controlador de API de Web (servidor) y en una aplicación cliente. NET.</span><span class="sxs-lookup"><span data-stu-id="86687-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span> <span data-ttu-id="86687-106">Web API 2.1 incluye compatibilidad con BSON.</span><span class="sxs-lookup"><span data-stu-id="86687-106">Web API 2.1 introduces support for BSON.</span></span> 

## <a name="what-is-bson"></a><span data-ttu-id="86687-107">¿Qué es BSON?</span><span class="sxs-lookup"><span data-stu-id="86687-107">What is BSON?</span></span>

<span data-ttu-id="86687-108">[BSON](http://bsonspec.org/) es un formato de serialización binaria.</span><span class="sxs-lookup"><span data-stu-id="86687-108">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="86687-109">"BSON" es el acrónimo "Binario"JSON", pero BSON y JSON se serializan de forma muy diferente.</span><span class="sxs-lookup"><span data-stu-id="86687-109">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="86687-110">Como BSON solo es "JSON similar a", porque los objetos se representan como pares de nombre-valor, similares a JSON.</span><span class="sxs-lookup"><span data-stu-id="86687-110">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="86687-111">A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, cadenas no</span><span class="sxs-lookup"><span data-stu-id="86687-111">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="86687-112">BSON se diseñó para ser ligero, fácil de explorar y rápido para la codificación y descodificación.</span><span class="sxs-lookup"><span data-stu-id="86687-112">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="86687-113">BSON es comparable en tamaño a JSON.</span><span class="sxs-lookup"><span data-stu-id="86687-113">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="86687-114">Dependiendo de los datos, una carga BSON puede ser menor o mayor que una carga JSON.</span><span class="sxs-lookup"><span data-stu-id="86687-114">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="86687-115">Para serializar los datos binarios, como un archivo de imagen BSON es menor que JSON, porque los datos binarios no están codificado en base64.</span><span class="sxs-lookup"><span data-stu-id="86687-115">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="86687-116">Son fáciles de analizar porque los elementos van precedidos de un campo de longitud, por lo que un analizador puede omitir los elementos sin decodificarlos documentos BSON.</span><span class="sxs-lookup"><span data-stu-id="86687-116">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="86687-117">Codificación y descodificación son eficaces, porque los tipos de datos numéricos se almacenan como números, cadenas no.</span><span class="sxs-lookup"><span data-stu-id="86687-117">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="86687-118">Los clientes nativos, como las aplicaciones cliente. NET, pueden beneficiarse del uso BSON en lugar de los formatos basados en texto como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="86687-118">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="86687-119">Para los clientes de explorador, probablemente deseará quedarse con JSON, porque JavaScript puede convertir directamente la carga de JSON.</span><span class="sxs-lookup"><span data-stu-id="86687-119">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="86687-120">Afortunadamente, la API Web usa [negociación de contenido](content-negotiation.md), por lo que puede admitir ambos formatos y permitir que el cliente elija su API.</span><span class="sxs-lookup"><span data-stu-id="86687-120">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="86687-121">Habilitar BSON en el servidor</span><span class="sxs-lookup"><span data-stu-id="86687-121">Enabling BSON on the Server</span></span>

<span data-ttu-id="86687-122">En la configuración de la API Web, agregue el **BsonMediaTypeFormatter** a la colección de formateadores.</span><span class="sxs-lookup"><span data-stu-id="86687-122">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="86687-123">Ahora, si el cliente solicita "application/bson", API Web usará al formateador BSON.</span><span class="sxs-lookup"><span data-stu-id="86687-123">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="86687-124">Para asociar BSON con otros tipos de medios, agregarlos a la colección SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="86687-124">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="86687-125">El código siguiente agrega "application/vnd.contoso" para los tipos de medios admitidos:</span><span class="sxs-lookup"><span data-stu-id="86687-125">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="86687-126">Sesión de HTTP de ejemplo</span><span class="sxs-lookup"><span data-stu-id="86687-126">Example HTTP Session</span></span>

<span data-ttu-id="86687-127">En este ejemplo, vamos a usar un controlador de API Web simple además de la clase del modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="86687-127">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="86687-128">Un cliente puede enviar la solicitud HTTP siguiente:</span><span class="sxs-lookup"><span data-stu-id="86687-128">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="86687-129">Esta es la respuesta:</span><span class="sxs-lookup"><span data-stu-id="86687-129">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="86687-130">Aquí he reemplazado los datos binarios con &quot;.&quot; caracteres.</span><span class="sxs-lookup"><span data-stu-id="86687-130">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="86687-131">Captura de pantalla siguiente muestra Fiddler los valores sin formato hexadecimales.</span><span class="sxs-lookup"><span data-stu-id="86687-131">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="86687-132">Uso de BSON con HttpClient</span><span class="sxs-lookup"><span data-stu-id="86687-132">Using BSON with HttpClient</span></span>

<span data-ttu-id="86687-133">Las aplicaciones de los clientes de .NET pueden utilizar el formateador BSON con **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="86687-133">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="86687-134">Para obtener más información acerca de **HttpClient**, consulte [una llamada a una Web API desde un cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="86687-134">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="86687-135">El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="86687-135">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="86687-136">Para solicitar BSON desde el servidor, establezca el encabezado Accept a "application/bson":</span><span class="sxs-lookup"><span data-stu-id="86687-136">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="86687-137">Para deserializar el cuerpo de respuesta, utilice el **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="86687-137">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="86687-138">Este formateador no está en la colección de formateadores de forma predeterminada, por lo que debe especificarla al leer el cuerpo de respuesta:</span><span class="sxs-lookup"><span data-stu-id="86687-138">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="86687-139">El ejemplo siguiente muestra cómo enviar una solicitud POST que contiene BSON.</span><span class="sxs-lookup"><span data-stu-id="86687-139">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="86687-140">Gran parte de este código es el mismo que el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="86687-140">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="86687-141">Sin embargo, en el **PostAsync** método, especifique **BsonMediaTypeFormatter** como el formateador:</span><span class="sxs-lookup"><span data-stu-id="86687-141">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="86687-142">Serializar tipos primitivos de nivel superior</span><span class="sxs-lookup"><span data-stu-id="86687-142">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="86687-143">Todos los documentos BSON es una lista de pares clave/valor. La especificación de BSON no define una sintaxis para serializar un único valor sin formato, como un entero o cadena.</span><span class="sxs-lookup"><span data-stu-id="86687-143">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="86687-144">Para evitar esta limitación, la **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial.</span><span class="sxs-lookup"><span data-stu-id="86687-144">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="86687-145">Antes de serializar, convierte el valor en un par clave/valor con la clave "Value".</span><span class="sxs-lookup"><span data-stu-id="86687-145">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="86687-146">Por ejemplo, suponga que el controlador de API devuelve un entero:</span><span class="sxs-lookup"><span data-stu-id="86687-146">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="86687-147">Antes de serializar, el formateador BSON lo convierte en el siguiente par clave-valor:</span><span class="sxs-lookup"><span data-stu-id="86687-147">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="86687-148">Al deserializar, el formateador convierte los datos al valor original.</span><span class="sxs-lookup"><span data-stu-id="86687-148">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="86687-149">Sin embargo, los clientes que usan un analizador BSON diferente necesitará controlar este caso, si su API web devuelve los valores sin formato.</span><span class="sxs-lookup"><span data-stu-id="86687-149">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="86687-150">En general, debería considerar devolver datos estructurados, en lugar de valores sin procesar.</span><span class="sxs-lookup"><span data-stu-id="86687-150">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86687-151">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="86687-151">Additional Resources</span></span>

[<span data-ttu-id="86687-152">Ejemplo de la API BSON de Web</span><span class="sxs-lookup"><span data-stu-id="86687-152">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="86687-153">Formateadores de contenido multimedia</span><span class="sxs-lookup"><span data-stu-id="86687-153">Media Formatters</span></span>](media-formatters.md)
