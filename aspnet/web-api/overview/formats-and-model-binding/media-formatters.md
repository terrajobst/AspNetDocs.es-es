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
# <a name="media-formatters-in-aspnet-web-api-2"></a>Formateadores de medios en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

En este tutorial se muestra cómo admitir formatos multimedia adicionales en ASP.NET Web API.

## <a name="internet-media-types"></a>Tipos de medios de Internet

Un tipo de medio, también denominado tipo MIME, identifica el formato de un fragmento de datos. En HTTP, los tipos de medios describen el formato del cuerpo del mensaje. Un tipo de medio consta de dos cadenas, un tipo y un subtipo. Por ejemplo:

- text/html
- image/png
- application/json

Cuando un mensaje HTTP contiene un cuerpo de entidad, el encabezado Content-Type Especifica el formato del cuerpo del mensaje. Esto indica al receptor cómo analizar el contenido del cuerpo del mensaje.

Por ejemplo, si una respuesta HTTP contiene una imagen PNG, la respuesta podría tener los encabezados siguientes.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Cuando el cliente envía un mensaje de solicitud, puede incluir un encabezado Accept. El encabezado Accept indica al servidor los tipos de medios que el cliente desea obtener del servidor. Por ejemplo:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Este encabezado indica al servidor que el cliente desea HTML, XHTML o XML.

El tipo de medio determina cómo API Web serializa y deserializa el cuerpo del mensaje HTTP. Web API tiene compatibilidad integrada con los datos XML, JSON, BSON y form-urlencoded, y puede admitir otros tipos de medios si escribe un *formateador multimedia*.

Para crear un formateador de medios, derive de una de estas clases:

- [Elemento mediatypeformatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Esta clase usa métodos asincrónicos de lectura y escritura.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Esta clase se deriva de **elemento mediatypeformatter** pero utiliza métodos sincrónicos de lectura y escritura.

La derivación de **BufferedMediaTypeFormatter** es más sencilla, ya que no hay ningún código asincrónico, sino que también significa que el subproceso de llamada puede bloquearse durante la e/s.

## <a name="example-creating-a-csv-media-formatter"></a>Ejemplo: creación de un formateador de medios CSV

En el ejemplo siguiente se muestra un formateador de tipo de medio que puede serializar un objeto de producto en un formato de valores separados por comas (CSV). En este ejemplo se usa el tipo de producto definido en el tutorial [creación de una API Web que admita operaciones CRUD](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Esta es la definición del objeto Product:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Para implementar un formateador de CSV, defina una clase que derive de **BufferedMediaTypeFormatter**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

En el constructor, agregue los tipos de medios que admite el formateador. En este ejemplo, el formateador admite un solo tipo de medio, &quot;Text/CSV&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Invalide el método **CanWriteType** para indicar los tipos que el formateador puede serializar:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

En este ejemplo, el formateador puede serializar objetos de `Product` única, así como colecciones de objetos de `Product`.

Del mismo modo, invalide el método **CanReadType** para indicar los tipos que el formateador puede deserializar. En este ejemplo, el formateador no admite la deserialización, por lo que el método simplemente devuelve **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Por último, invalide el método **WriteToStream** . Este método serializa un tipo escribiéndolo en una secuencia. Si el formateador admite la deserialización, también invalide el método **ReadFromStream** .

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Adición de un formateador de medios a la canalización de API Web

Para agregar un formateador de tipo de medio a la canalización de la API Web, use la propiedad **Formatters** en el objeto **HttpConfiguration** .

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Codificaciones de caracteres

Opcionalmente, un formateador de medios puede admitir varias codificaciones de caracteres, como UTF-8 o ISO 8859-1.

En el constructor, agregue uno o varios tipos [System. Text. Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) a la colección **SupportedEncodings** . Coloque la codificación predeterminada en primer lugar.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

En los métodos **WriteToStream** y **ReadFromStream** , llame a [elemento mediatypeformatter. SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) para seleccionar la codificación de caracteres preferida. Este método coincide con los encabezados de solicitud con la lista de codificaciones admitidas. Use la **codificación** devuelta cuando lea o escriba desde el flujo:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
