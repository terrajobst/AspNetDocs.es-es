---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formateadores de medios en ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: Muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: da0c566dad302054d7d0a6435e4c6df178c64772
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448945"
---
# <a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="59a10-103">Formateadores de medios en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="59a10-103">Media Formatters in ASP.NET Web API 2</span></span>

<span data-ttu-id="59a10-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59a10-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="59a10-105">En este tutorial se muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="59a10-105">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="59a10-106">Tipos de medios de Internet</span><span class="sxs-lookup"><span data-stu-id="59a10-106">Internet Media Types</span></span>

<span data-ttu-id="59a10-107">Un tipo de medio, también denominado tipo MIME, identifica el formato de un fragmento de datos.</span><span class="sxs-lookup"><span data-stu-id="59a10-107">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="59a10-108">En HTTP, los tipos de medios describen el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="59a10-108">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="59a10-109">Un tipo de medio consta de dos cadenas, un tipo y un subtipo.</span><span class="sxs-lookup"><span data-stu-id="59a10-109">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="59a10-110">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59a10-110">For example:</span></span>

- <span data-ttu-id="59a10-111">text/html</span><span class="sxs-lookup"><span data-stu-id="59a10-111">text/html</span></span>
- <span data-ttu-id="59a10-112">image/png</span><span class="sxs-lookup"><span data-stu-id="59a10-112">image/png</span></span>
- <span data-ttu-id="59a10-113">application/json</span><span class="sxs-lookup"><span data-stu-id="59a10-113">application/json</span></span>

<span data-ttu-id="59a10-114">Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type Especifica el formato del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="59a10-114">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="59a10-115">Esto indica al receptor cómo analizar el contenido del cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="59a10-115">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="59a10-116">Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los encabezados siguientes.</span><span class="sxs-lookup"><span data-stu-id="59a10-116">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="59a10-117">Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado Accept.</span><span class="sxs-lookup"><span data-stu-id="59a10-117">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="59a10-118">El encabezado Accept indica al servidor los tipos de medios que el cliente desea obtener del servidor.</span><span class="sxs-lookup"><span data-stu-id="59a10-118">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="59a10-119">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="59a10-119">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="59a10-120">Este encabezado indica al servidor que el cliente desea HTML, XHTML o XML.</span><span class="sxs-lookup"><span data-stu-id="59a10-120">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="59a10-121">El tipo de medio determina cómo API Web serializa y deserializa el cuerpo del mensaje HTTP.</span><span class="sxs-lookup"><span data-stu-id="59a10-121">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="59a10-122">Web API tiene compatibilidad integrada con los datos XML, JSON, BSON y form-urlencoded, y puede admitir otros tipos de medios si escribe un *formateador multimedia*.</span><span class="sxs-lookup"><span data-stu-id="59a10-122">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="59a10-123">Para crear un formateador de medios, derive de una de estas clases:</span><span class="sxs-lookup"><span data-stu-id="59a10-123">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="59a10-124">[Elemento mediatypeformatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a10-124">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="59a10-125">Esta clase usa métodos asincrónicos de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="59a10-125">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="59a10-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="59a10-126">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="59a10-127">Esta clase se deriva de **elemento mediatypeformatter** pero utiliza métodos sincrónicos de lectura y escritura.</span><span class="sxs-lookup"><span data-stu-id="59a10-127">This class derives from **MediaTypeFormatter** but uses synchronous read/write methods.</span></span>

<span data-ttu-id="59a10-128">La derivación de **BufferedMediaTypeFormatter** es más sencilla, ya que no hay ningún código asincrónico, sino que también significa que el subproceso de llamada puede bloquearse durante la e/s.</span><span class="sxs-lookup"><span data-stu-id="59a10-128">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="59a10-129">Ejemplo: creación de un formateador de medios CSV</span><span class="sxs-lookup"><span data-stu-id="59a10-129">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="59a10-130">En el ejemplo siguiente se muestra un formateador de tipo de medio que puede serializar un objeto de producto en un formato de valores separados por comas (CSV).</span><span class="sxs-lookup"><span data-stu-id="59a10-130">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="59a10-131">En este ejemplo se usa el tipo de producto definido en el tutorial [creación de una API Web que admita operaciones CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="59a10-131">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="59a10-132">Esta es la definición del objeto Product:</span><span class="sxs-lookup"><span data-stu-id="59a10-132">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="59a10-133">Para implementar un formateador de CSV, defina una clase que derive de **BufferedMediaTypeFormatter**:</span><span class="sxs-lookup"><span data-stu-id="59a10-133">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormatter**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="59a10-134">En el constructor, agregue los tipos de medios que admite el formateador.</span><span class="sxs-lookup"><span data-stu-id="59a10-134">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="59a10-135">En este ejemplo, el formateador admite un solo tipo de medio, &quot;Text/CSV&quot;:</span><span class="sxs-lookup"><span data-stu-id="59a10-135">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="59a10-136">Invalide el método **CanWriteType** para indicar los tipos que el formateador puede serializar:</span><span class="sxs-lookup"><span data-stu-id="59a10-136">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="59a10-137">En este ejemplo, el formateador puede serializar objetos de `Product` única, así como colecciones de objetos de `Product`.</span><span class="sxs-lookup"><span data-stu-id="59a10-137">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="59a10-138">Del mismo modo, invalide el método **CanReadType** para indicar los tipos que el formateador puede deserializar.</span><span class="sxs-lookup"><span data-stu-id="59a10-138">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="59a10-139">En este ejemplo, el formateador no admite la deserialización, por lo que el método simplemente devuelve **false**.</span><span class="sxs-lookup"><span data-stu-id="59a10-139">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="59a10-140">Por último, invalide el método **WriteToStream** .</span><span class="sxs-lookup"><span data-stu-id="59a10-140">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="59a10-141">Este método serializa un tipo escribiéndolo en una secuencia.</span><span class="sxs-lookup"><span data-stu-id="59a10-141">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="59a10-142">Si el formateador admite la deserialización, también invalide el método **ReadFromStream** .</span><span class="sxs-lookup"><span data-stu-id="59a10-142">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="59a10-143">Adición de un formateador de medios a la canalización de API Web</span><span class="sxs-lookup"><span data-stu-id="59a10-143">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="59a10-144">Para agregar un formateador de tipo de medio a la canalización de la API Web, use la propiedad **Formatters** en el objeto **HttpConfiguration** .</span><span class="sxs-lookup"><span data-stu-id="59a10-144">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="59a10-145">Codificaciones de caracteres</span><span class="sxs-lookup"><span data-stu-id="59a10-145">Character Encodings</span></span>

<span data-ttu-id="59a10-146">Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="59a10-146">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="59a10-147">En el constructor, agregue uno o varios tipos [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) a la colección **SupportedEncodings** .</span><span class="sxs-lookup"><span data-stu-id="59a10-147">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="59a10-148">Coloque la codificación predeterminada en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="59a10-148">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="59a10-149">En los métodos **WriteToStream** y **ReadFromStream** , llame a [elemento mediatypeformatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida.</span><span class="sxs-lookup"><span data-stu-id="59a10-149">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="59a10-150">Este método coincide con los encabezados de solicitud con la lista de codificaciones admitidas.</span><span class="sxs-lookup"><span data-stu-id="59a10-150">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="59a10-151">Use la **codificación** devuelta cuando lea o escriba desde el flujo:</span><span class="sxs-lookup"><span data-stu-id="59a10-151">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
