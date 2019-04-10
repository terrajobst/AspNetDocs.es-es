---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de contenido multimedia en ASP.NET Web API 2 - ASP.NET 4.x
author: MikeWasson
description: Muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API para ASP.NET 4.x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418773"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="8c02a-103">Formateadores de contenido multimedia en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="8c02a-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="8c02a-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8c02a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8c02a-105">Este tutorial muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="8c02a-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="8c02a-106">Tipos de medios de Internet</span><span class="sxs-lookup"><span data-stu-id="8c02a-106">Internet Media Types</span></span>

<span data-ttu-id="8c02a-107">Un tipo de medio, también denominado tipo MIME, identifica el formato de un elemento de datos.</span><span class="sxs-lookup"><span data-stu-id="8c02a-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="8c02a-108">Tipos de medios en HTTP, describen el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="8c02a-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="8c02a-109">Un tipo de medio consta de dos cadenas, un tipo y un subtipo.</span><span class="sxs-lookup"><span data-stu-id="8c02a-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="8c02a-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8c02a-110">For example:</span></span>

- <span data-ttu-id="8c02a-111">text/html</span><span class="sxs-lookup"><span data-stu-id="8c02a-111">text/html</span></span>
- <span data-ttu-id="8c02a-112">image/png</span><span class="sxs-lookup"><span data-stu-id="8c02a-112">image/png</span></span>
- <span data-ttu-id="8c02a-113">application/json</span><span class="sxs-lookup"><span data-stu-id="8c02a-113">application/json</span></span>

<span data-ttu-id="8c02a-114">Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type especifica el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="8c02a-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="8c02a-115">Esto indica que el receptor a cómo analizar el contenido del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="8c02a-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="8c02a-116">Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los siguientes encabezados.</span><span class="sxs-lookup"><span data-stu-id="8c02a-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="8c02a-117">Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="8c02a-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="8c02a-118">El encabezado Accept indica que el servidor de tipos de medios que el cliente desea desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="8c02a-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="8c02a-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8c02a-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="8c02a-120">Este encabezado indica al servidor que desea que el cliente HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="8c02a-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="8c02a-121">El tipo de medio determina cómo la API Web serializa y deserializa el cuerpo del mensaje HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c02a-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="8c02a-122">API Web tiene compatibilidad integrada para XML, JSON, BSON y datos de formato form-urlencoded y puede admitir tipos de medios adicionales escribiendo un *formateador media*.</span><span class="sxs-lookup"><span data-stu-id="8c02a-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="8c02a-123">Para crear a un formateador de medios, que se derivan de una de estas clases:</span><span class="sxs-lookup"><span data-stu-id="8c02a-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="8c02a-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c02a-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="8c02a-125">Esta lectura asincrónica de usos de clases y métodos de escritura.</span><span class="sxs-lookup"><span data-stu-id="8c02a-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="8c02a-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c02a-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="8c02a-127">Esta clase se deriva de **elemento MediaTypeFormatter** pero utiliza los métodos de lectura y escritura sincrónica.</span><span class="sxs-lookup"><span data-stu-id="8c02a-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="8c02a-128">Derivar de **BufferedMediaTypeFormatter** es más sencillo, porque no hay ningún código asincrónico, pero también significa que puede bloquear el subproceso de llamada durante la E/S.</span><span class="sxs-lookup"><span data-stu-id="8c02a-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="8c02a-129">Ejemplo: Creación de un formateador de medios CSV</span><span class="sxs-lookup"><span data-stu-id="8c02a-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="8c02a-130">El ejemplo siguiente muestra un formateador de tipo de medios que puede serializar un objeto de producto a un formato de valores separados por comas (CSV).</span><span class="sxs-lookup"><span data-stu-id="8c02a-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="8c02a-131">Este ejemplo utiliza el tipo de producto definido en el tutorial [crear una API Web que admite operaciones de CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="8c02a-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="8c02a-132">Esta es la definición del objeto de producto:</span><span class="sxs-lookup"><span data-stu-id="8c02a-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="8c02a-133">Para implementar un formateador CSV, defina una clase que deriva de **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="8c02a-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="8c02a-134">En el constructor, agregue los tipos de medios que admite el formateador.</span><span class="sxs-lookup"><span data-stu-id="8c02a-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="8c02a-135">En este ejemplo, el formateador admite un único tipo de medio, &quot;texto o csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="8c02a-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="8c02a-136">Invalidar el **CanWriteType** método para indicar que escribe el formateador puede serializar:</span><span class="sxs-lookup"><span data-stu-id="8c02a-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="8c02a-137">En este ejemplo, el formateador puede serializar solo `Product` objetos, así como las colecciones de `Product` objetos.</span><span class="sxs-lookup"><span data-stu-id="8c02a-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="8c02a-138">De forma similar, reemplace el **CanReadType** método para indicar que escribe el formateador puede deserializar.</span><span class="sxs-lookup"><span data-stu-id="8c02a-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="8c02a-139">En este ejemplo, el formateador no admite la deserialización, por lo que simplemente devuelve el método **false**.</span><span class="sxs-lookup"><span data-stu-id="8c02a-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="8c02a-140">Por último, reemplace el **WriteToStream** método.</span><span class="sxs-lookup"><span data-stu-id="8c02a-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="8c02a-141">Este método serializa un tipo escribiendo en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="8c02a-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="8c02a-142">Si el formateador admite la deserialización, también invalidar el **ReadFromStream** método.</span><span class="sxs-lookup"><span data-stu-id="8c02a-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="8c02a-143">Agregar a un formateador de medios a la canalización de API Web</span><span class="sxs-lookup"><span data-stu-id="8c02a-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="8c02a-144">Para agregar un tipo de medio formateador a la canalización de Web API, use el **formateadores** propiedad en el **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="8c02a-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="8c02a-145">Codificaciones de caracteres</span><span class="sxs-lookup"><span data-stu-id="8c02a-145">Character Encodings</span></span>

<span data-ttu-id="8c02a-146">Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="8c02a-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="8c02a-147">En el constructor, agregue uno o varios [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) tipos a la **SupportedEncodings** colección.</span><span class="sxs-lookup"><span data-stu-id="8c02a-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="8c02a-148">Coloque el valor predeterminado de codificación primero.</span><span class="sxs-lookup"><span data-stu-id="8c02a-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="8c02a-149">En el **WriteToStream** y **ReadFromStream** llamar métodos, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida.</span><span class="sxs-lookup"><span data-stu-id="8c02a-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="8c02a-150">Este método coincide con los encabezados de solicitud con la lista de codificaciones compatibles.</span><span class="sxs-lookup"><span data-stu-id="8c02a-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="8c02a-151">Utilice el valor devuelto **Encoding** al leer o escribir desde la secuencia:</span><span class="sxs-lookup"><span data-stu-id="8c02a-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
