---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviar datos de formulario HTML en ASP.NET Web API: carga de archivos y multipart MIME-ASP.NET 4. x'
author: MikeWasson
description: En este tutorial se muestra cómo cargar archivos en una API Web. También se describe cómo procesar datos MIME de varias partes.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449215"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="ac8c9-104">Enviar datos de formulario HTML en ASP.NET Web API: carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="ac8c9-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="ac8c9-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ac8c9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="ac8c9-106">Parte 2: carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="ac8c9-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="ac8c9-107">En este tutorial se muestra cómo cargar archivos en una API Web.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="ac8c9-108">También se describe cómo procesar datos MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="ac8c9-109">[Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="ac8c9-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>

<span data-ttu-id="ac8c9-110">A continuación se muestra un ejemplo de un formulario HTML para cargar un archivo:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="ac8c9-111">Este formulario contiene un control entrada de texto y un control entrada de archivo.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="ac8c9-112">Cuando un formulario contiene un control de entrada de archivo, el atributo **enctype** siempre debe ser &quot;&quot;de varias partes/datos de formulario, lo que especifica que el formulario se enviará como un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="ac8c9-113">El formato de un mensaje MIME de varias partes es más fácil de entender si se examina una solicitud de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="ac8c9-114">Este mensaje se divide en dos *partes*, una para cada control de formulario.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="ac8c9-115">Los límites de las partes se indican mediante las líneas que comienzan con guiones.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="ac8c9-116">El límite del elemento incluye un componente aleatorio (&quot;41184676334&quot;) para asegurarse de que la cadena de límite no aparece accidentalmente dentro de una parte del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>

<span data-ttu-id="ac8c9-117">Cada parte del mensaje contiene uno o varios encabezados, seguidos del contenido del elemento.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="ac8c9-118">El encabezado Content-Disposition incluye el nombre del control.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="ac8c9-119">En el caso de los archivos, también contiene el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="ac8c9-120">El encabezado Content-Type describe los datos del elemento.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="ac8c9-121">Si se omite este encabezado, el valor predeterminado es texto/sin formato.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="ac8c9-122">En el ejemplo anterior, el usuario cargó un archivo llamado GrandCanyon. jpg, con el tipo de contenido image/JPEG; y el valor de la entrada de texto se &quot;&quot;de vacaciones de verano.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="ac8c9-123">Carga de archivos</span><span class="sxs-lookup"><span data-stu-id="ac8c9-123">File Upload</span></span>

<span data-ttu-id="ac8c9-124">Veamos ahora un controlador de API Web que lee archivos de un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="ac8c9-125">El controlador leerá los archivos de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="ac8c9-126">La API Web admite acciones asincrónicas mediante el [modelo de programación basado en tareas](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="ac8c9-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="ac8c9-127">En primer lugar, este es el código si tiene como destino .NET Framework 4,5, que admite las palabras clave **Async** y **Await** .</span><span class="sxs-lookup"><span data-stu-id="ac8c9-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="ac8c9-128">Tenga en cuenta que la acción del controlador no toma ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="ac8c9-129">Esto se debe a que el cuerpo de la solicitud se procesa dentro de la acción, sin invocar un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="ac8c9-130">El método **IsMultipartContent** comprueba si la solicitud contiene un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="ac8c9-131">En caso contrario, el controlador devuelve el código de Estado HTTP 415 (tipo de medio no admitido).</span><span class="sxs-lookup"><span data-stu-id="ac8c9-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="ac8c9-132">La clase **MultipartFormDataStreamProvider** es un objeto auxiliar que asigna secuencias de archivos para los archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="ac8c9-133">Para leer el mensaje MIME de varias partes, llame al método **ReadAsMultipartAsync** .</span><span class="sxs-lookup"><span data-stu-id="ac8c9-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="ac8c9-134">Este método extrae todas las partes del mensaje y las escribe en las secuencias proporcionadas por **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="ac8c9-135">Cuando el método se completa, puede obtener información sobre los archivos de la propiedad **Filedata** , que es una colección de objetos **MultipartFileData** .</span><span class="sxs-lookup"><span data-stu-id="ac8c9-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="ac8c9-136">**MultipartFileData. FileName** es el nombre de archivo local en el servidor donde se guardó el archivo.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="ac8c9-137">**MultipartFileData. Headers** contiene el encabezado de elemento (*no* el encabezado de solicitud).</span><span class="sxs-lookup"><span data-stu-id="ac8c9-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="ac8c9-138">Puede utilizar esto para tener acceso al contenido\_encabezados de tipo de contenido y disposición.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="ac8c9-139">Como sugiere el nombre, **ReadAsMultipartAsync** es un método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="ac8c9-140">Para realizar el trabajo una vez finalizado el método, use una [tarea de continuación](https://msdn.microsoft.com/library/ee372288.aspx) (.net 4,0) o la palabra clave **await** (.net 4,5).</span><span class="sxs-lookup"><span data-stu-id="ac8c9-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="ac8c9-141">Esta es la versión .NET Framework 4,0 del código anterior:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="ac8c9-142">Leer datos de control de formulario</span><span class="sxs-lookup"><span data-stu-id="ac8c9-142">Reading Form Control Data</span></span>

<span data-ttu-id="ac8c9-143">El formulario HTML que mostré anteriormente tenía un control entrada de texto.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="ac8c9-144">Puede obtener el valor del control de la propiedad **FormData** de **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="ac8c9-145">**FormData** es un **NameValueCollection** que contiene pares de nombre/valor para los controles de formulario.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="ac8c9-146">La colección puede contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="ac8c9-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="ac8c9-147">Tenga en cuenta este formato:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="ac8c9-148">El cuerpo de la solicitud podría ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="ac8c9-149">En ese caso, la colección **FormData** contiene los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="ac8c9-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="ac8c9-150">viaje: ida y vuelta</span><span class="sxs-lookup"><span data-stu-id="ac8c9-150">trip: round-trip</span></span>
- <span data-ttu-id="ac8c9-151">opciones: nonstop</span><span class="sxs-lookup"><span data-stu-id="ac8c9-151">options: nonstop</span></span>
- <span data-ttu-id="ac8c9-152">opciones: fechas</span><span class="sxs-lookup"><span data-stu-id="ac8c9-152">options: dates</span></span>
- <span data-ttu-id="ac8c9-153">asiento: window</span><span class="sxs-lookup"><span data-stu-id="ac8c9-153">seat: window</span></span>
