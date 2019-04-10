---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviar datos de formulario HTML en ASP.NET Web API: Carga de archivos y MIME de varias partes - ASP.NET 4.x'
author: MikeWasson
description: Este tutorial muestra cómo cargar archivos a una API web. También se describe cómo procesar los datos de varias partes MIME.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 70e150a32f208cf75086f959d484d86e8501c6bd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59419930"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="35f09-104">Enviar datos de formulario HTML en ASP.NET Web API: Carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="35f09-104">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>

<span data-ttu-id="35f09-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="35f09-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="35f09-106">Parte 2: Carga de archivos y MIME de varias partes</span><span class="sxs-lookup"><span data-stu-id="35f09-106">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="35f09-107">Este tutorial muestra cómo cargar archivos a una API web.</span><span class="sxs-lookup"><span data-stu-id="35f09-107">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="35f09-108">También se describe cómo procesar los datos de varias partes MIME.</span><span class="sxs-lookup"><span data-stu-id="35f09-108">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="35f09-109">[Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="35f09-109">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="35f09-110">Este es un ejemplo de un formulario HTML para cargar un archivo:</span><span class="sxs-lookup"><span data-stu-id="35f09-110">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="35f09-111">Este formulario contiene un control de entrada de texto y un control de entrada de archivo.</span><span class="sxs-lookup"><span data-stu-id="35f09-111">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="35f09-112">Cuando un formulario contiene un control de entrada de archivo, el **enctype** atributo debe ser siempre &quot;multipart/datos de formulario&quot;, que especifica que el formulario se enviará como un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="35f09-112">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="35f09-113">El formato de un mensaje MIME de varias partes es más fácil comprender echando un vistazo a una solicitud de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="35f09-113">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="35f09-114">Este mensaje se divide en dos *partes*, uno para cada control de formulario.</span><span class="sxs-lookup"><span data-stu-id="35f09-114">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="35f09-115">Los límites de parte se indican mediante las líneas que comienzan con guiones.</span><span class="sxs-lookup"><span data-stu-id="35f09-115">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="35f09-116">El límite de la parte incluye un componente aleatorio (&quot;41184676334&quot;) para asegurarse de que la cadena delimitadora no accidentalmente aparecen dentro de una parte de mensaje.</span><span class="sxs-lookup"><span data-stu-id="35f09-116">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="35f09-117">Cada parte del mensaje contiene uno o varios encabezados, seguidos por el contenido del elemento.</span><span class="sxs-lookup"><span data-stu-id="35f09-117">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="35f09-118">El encabezado Content-Disposition incluye el nombre del control.</span><span class="sxs-lookup"><span data-stu-id="35f09-118">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="35f09-119">Para los archivos, también contiene el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="35f09-119">For files, it also contains the file name.</span></span>
- <span data-ttu-id="35f09-120">El encabezado Content-Type describe los datos en la parte.</span><span class="sxs-lookup"><span data-stu-id="35f09-120">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="35f09-121">Si se omite este encabezado, el valor predeterminado es texto/sin formato.</span><span class="sxs-lookup"><span data-stu-id="35f09-121">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="35f09-122">En el ejemplo anterior, el usuario ha cargado un archivo llamado GrandCanyon.jpg, con el tipo de contenido de imagen/jpeg; y el valor de la entrada de texto era &quot;vacaciones de verano&quot;.</span><span class="sxs-lookup"><span data-stu-id="35f09-122">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="35f09-123">Carga de archivos</span><span class="sxs-lookup"><span data-stu-id="35f09-123">File Upload</span></span>

<span data-ttu-id="35f09-124">Ahora Echemos un vistazo a un controlador de Web API que lee los archivos de un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="35f09-124">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="35f09-125">El controlador lee los archivos de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="35f09-125">The controller will read the files asynchronously.</span></span> <span data-ttu-id="35f09-126">Web API es compatible con las acciones asincrónicas mediante el [modelo de programación basado en tareas](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="35f09-126">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="35f09-127">En primer lugar, este es el código si tiene como destino .NET Framework 4.5, que es compatible con la **async** y **await** palabras clave.</span><span class="sxs-lookup"><span data-stu-id="35f09-127">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="35f09-128">Tenga en cuenta que la acción de controlador no toma ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="35f09-128">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="35f09-129">Esto es debido a que se procesa dentro de la acción, el cuerpo de solicitud sin invocar a un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="35f09-129">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="35f09-130">El **IsMultipartContent** método comprueba si la solicitud contiene un mensaje MIME de varias partes.</span><span class="sxs-lookup"><span data-stu-id="35f09-130">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="35f09-131">Si no es así, el controlador devuelve el código de estado HTTP 415 (tipo de medio no compatible).</span><span class="sxs-lookup"><span data-stu-id="35f09-131">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="35f09-132">El **MultipartFormDataStreamProvider** clase es un objeto auxiliar que asigna las secuencias de archivo para archivos cargados.</span><span class="sxs-lookup"><span data-stu-id="35f09-132">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="35f09-133">Para leer el mensaje MIME de varias partes, llame a la **ReadAsMultipartAsync** método.</span><span class="sxs-lookup"><span data-stu-id="35f09-133">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="35f09-134">Este método extrae todas las partes del mensaje y los escribe en las secuencias proporcionadas por el **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="35f09-134">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="35f09-135">Cuando se completa el método, puede obtener información acerca de los archivos desde el **FileData** propiedad, que es una colección de **MultipartFileData** objetos.</span><span class="sxs-lookup"><span data-stu-id="35f09-135">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="35f09-136">**MultipartFileData.FileName** es el nombre de archivo local en el servidor, donde se guardó el archivo.</span><span class="sxs-lookup"><span data-stu-id="35f09-136">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="35f09-137">**MultipartFileData.Headers** contiene el encabezado de elemento (*no* el encabezado de solicitud).</span><span class="sxs-lookup"><span data-stu-id="35f09-137">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="35f09-138">Se puede usar para tener acceso al contenido\_encabezados Disposition y Content-Type.</span><span class="sxs-lookup"><span data-stu-id="35f09-138">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="35f09-139">Como sugiere su nombre, **ReadAsMultipartAsync** es un método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="35f09-139">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="35f09-140">Para realizar el trabajo después de completarse el método, use un [tarea de continuación](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o el **await** palabra clave (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="35f09-140">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="35f09-141">Esta es la versión de .NET Framework 4.0 del código anterior:</span><span class="sxs-lookup"><span data-stu-id="35f09-141">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="35f09-142">Leer datos de Control de formulario</span><span class="sxs-lookup"><span data-stu-id="35f09-142">Reading Form Control Data</span></span>

<span data-ttu-id="35f09-143">El formulario HTML que mostré anteriormente tenía un control de entrada de texto.</span><span class="sxs-lookup"><span data-stu-id="35f09-143">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="35f09-144">Puede obtener el valor del control desde el **FormData** propiedad de la **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="35f09-144">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="35f09-145">**FormData** es un **NameValueCollection** que contiene pares nombre/valor para los controles del formulario.</span><span class="sxs-lookup"><span data-stu-id="35f09-145">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="35f09-146">La colección puede contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="35f09-146">The collection can contain duplicate keys.</span></span> <span data-ttu-id="35f09-147">Considere este formulario:</span><span class="sxs-lookup"><span data-stu-id="35f09-147">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="35f09-148">El cuerpo de solicitud podría parecerse a esto:</span><span class="sxs-lookup"><span data-stu-id="35f09-148">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="35f09-149">En ese caso, el **FormData** colección contendría los siguientes pares de clave/valor:</span><span class="sxs-lookup"><span data-stu-id="35f09-149">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="35f09-150">ida y vuelta: ida y vuelta</span><span class="sxs-lookup"><span data-stu-id="35f09-150">trip: round-trip</span></span>
- <span data-ttu-id="35f09-151">opciones: la protección sin interrupciones</span><span class="sxs-lookup"><span data-stu-id="35f09-151">options: nonstop</span></span>
- <span data-ttu-id="35f09-152">opciones: las fechas</span><span class="sxs-lookup"><span data-stu-id="35f09-152">options: dates</span></span>
- <span data-ttu-id="35f09-153">asiento: ventana</span><span class="sxs-lookup"><span data-stu-id="35f09-153">seat: window</span></span>
