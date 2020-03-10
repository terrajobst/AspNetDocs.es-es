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
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Enviar datos de formulario HTML en ASP.NET Web API: carga de archivos y MIME de varias partes

por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: carga de archivos y MIME de varias partes

En este tutorial se muestra cómo cargar archivos en una API Web. También se describe cómo procesar datos MIME de varias partes.

> [!NOTE]
> [Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

A continuación se muestra un ejemplo de un formulario HTML para cargar un archivo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Este formulario contiene un control entrada de texto y un control entrada de archivo. Cuando un formulario contiene un control de entrada de archivo, el atributo **enctype** siempre debe ser &quot;&quot;de varias partes/datos de formulario, lo que especifica que el formulario se enviará como un mensaje MIME de varias partes.

El formato de un mensaje MIME de varias partes es más fácil de entender si se examina una solicitud de ejemplo:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Este mensaje se divide en dos *partes*, una para cada control de formulario. Los límites de las partes se indican mediante las líneas que comienzan con guiones.

> [!NOTE]
> El límite del elemento incluye un componente aleatorio (&quot;41184676334&quot;) para asegurarse de que la cadena de límite no aparece accidentalmente dentro de una parte del mensaje.

Cada parte del mensaje contiene uno o varios encabezados, seguidos del contenido del elemento.

- El encabezado Content-Disposition incluye el nombre del control. En el caso de los archivos, también contiene el nombre de archivo.
- El encabezado Content-Type describe los datos del elemento. Si se omite este encabezado, el valor predeterminado es texto/sin formato.

En el ejemplo anterior, el usuario cargó un archivo llamado GrandCanyon. jpg, con el tipo de contenido image/JPEG; y el valor de la entrada de texto se &quot;&quot;de vacaciones de verano.

## <a name="file-upload"></a>Carga de archivos

Veamos ahora un controlador de API Web que lee archivos de un mensaje MIME de varias partes. El controlador leerá los archivos de forma asincrónica. La API Web admite acciones asincrónicas mediante el [modelo de programación basado en tareas](https://msdn.microsoft.com/library/dd460693.aspx). En primer lugar, este es el código si tiene como destino .NET Framework 4,5, que admite las palabras clave **Async** y **Await** .

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Tenga en cuenta que la acción del controlador no toma ningún parámetro. Esto se debe a que el cuerpo de la solicitud se procesa dentro de la acción, sin invocar un formateador de tipo de medio.

El método **IsMultipartContent** comprueba si la solicitud contiene un mensaje MIME de varias partes. En caso contrario, el controlador devuelve el código de Estado HTTP 415 (tipo de medio no admitido).

La clase **MultipartFormDataStreamProvider** es un objeto auxiliar que asigna secuencias de archivos para los archivos cargados. Para leer el mensaje MIME de varias partes, llame al método **ReadAsMultipartAsync** . Este método extrae todas las partes del mensaje y las escribe en las secuencias proporcionadas por **MultipartFormDataStreamProvider**.

Cuando el método se completa, puede obtener información sobre los archivos de la propiedad **Filedata** , que es una colección de objetos **MultipartFileData** .

- **MultipartFileData. FileName** es el nombre de archivo local en el servidor donde se guardó el archivo.
- **MultipartFileData. Headers** contiene el encabezado de elemento (*no* el encabezado de solicitud). Puede utilizar esto para tener acceso al contenido\_encabezados de tipo de contenido y disposición.

Como sugiere el nombre, **ReadAsMultipartAsync** es un método asincrónico. Para realizar el trabajo una vez finalizado el método, use una [tarea de continuación](https://msdn.microsoft.com/library/ee372288.aspx) (.net 4,0) o la palabra clave **await** (.net 4,5).

Esta es la versión .NET Framework 4,0 del código anterior:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Leer datos de control de formulario

El formulario HTML que mostré anteriormente tenía un control entrada de texto.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Puede obtener el valor del control de la propiedad **FormData** de **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** es un **NameValueCollection** que contiene pares de nombre/valor para los controles de formulario. La colección puede contener claves duplicadas. Tenga en cuenta este formato:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

El cuerpo de la solicitud podría ser similar al siguiente:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

En ese caso, la colección **FormData** contiene los siguientes pares clave-valor:

- viaje: ida y vuelta
- opciones: nonstop
- opciones: fechas
- asiento: window
