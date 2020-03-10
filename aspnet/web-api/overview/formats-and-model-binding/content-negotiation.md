---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Negociación de contenido en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe cómo ASP.NET Web API implementa la negociación de contenido HTTP para ASP.NET 4. x.
ms.author: riande
ms.date: 05/20/2012
ms.custom: seoapril2019
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: cb6668ff6de276d3778ce11f27ce597d8bf1f9c7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504661"
---
# <a name="content-negotiation-in-aspnet-web-api"></a>Negociación de contenido en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API implementa la negociación de contenido para ASP.NET 4. x.

La especificación HTTP (RFC 2616) define la negociación de contenido como "el proceso de selección de la mejor representación para una respuesta determinada cuando hay varias representaciones disponibles". El mecanismo principal para la negociación de contenido en HTTP son estos encabezados de solicitud:

- **Aceptar:** Los tipos de medios que son aceptables para la respuesta, como "Application/JSON", "Application/XML" o un tipo de medio personalizado como &quot;Application/vnd. example + XML&quot;
- **Accept-charset:** Los juegos de caracteres que son aceptables, como UTF-8 o ISO 8859-1.
- **Codificación de aceptación:** Las codificaciones de contenido que son aceptables, como gzip.
- **Accept-Language:** El lenguaje natural preferido, como "en-US".

El servidor también puede examinar otras partes de la solicitud HTTP. Por ejemplo, si la solicitud contiene un encabezado X-requested-with, que indica una solicitud AJAX, el servidor puede tener como valor predeterminado JSON si no hay ningún encabezado Accept.

En este artículo, veremos cómo Web API usa los encabezados Accept y Accept-charset. (En este momento, no hay compatibilidad integrada para aceptar-codificación o Accept-Language).

## <a name="serialization"></a>Serialización

Si un controlador de API Web devuelve un recurso como tipo CLR, la canalización serializa el valor devuelto y lo escribe en el cuerpo de respuesta HTTP.

Por ejemplo, considere la siguiente acción del controlador:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Un cliente podría enviar esta solicitud HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

En respuesta, el servidor puede enviar:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

En este ejemplo, el cliente solicitó JSON, JavaScript o "cualquiera" (\*/\*). El servidor respondió con una representación JSON del objeto `Product`. Observe que el encabezado Content-Type de la respuesta se establece en &quot;&quot;Application/JSON.

Un controlador también puede devolver un objeto **HttpResponseMessage** . Para especificar un objeto CLR para el cuerpo de la respuesta, llame al método de extensión **CreateResponse** :

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Esta opción le proporciona más control sobre los detalles de la respuesta. Puede establecer el código de estado, agregar encabezados HTTP, etc.

El objeto que serializa el recurso se denomina *formateador multimedia*. Los formateadores multimedia derivan de la clase **elemento mediatypeformatter** . Web API proporciona formateadores multimedia para XML y JSON, y puede crear formateadores personalizados para admitir otros tipos de medios. Para obtener información sobre cómo escribir un formateador personalizado, vea [formateadores de medios](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Cómo funciona la negociación de contenido

En primer lugar, la canalización obtiene el servicio **IContentNegotiator** del objeto **HttpConfiguration** . También obtiene la lista de formateadores multimedia de la colección **HttpConfiguration. Formatters** .

A continuación, la canalización llama a **IContentNegotiator. Negotiate**, pasando:

- Tipo de objeto que se va a serializar.
- Colección de formateadores multimedia
- La solicitud HTTP

El método **Negotiate** devuelve dos fragmentos de información:

- Qué formateador usar
- El tipo de medio para la respuesta.

Si no se encuentra ningún formateador, el método **Negotiate** devuelve **null**y el cliente recibe el error http 406 (no aceptable).

En el código siguiente se muestra cómo un controlador puede invocar directamente la negociación de contenido:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Este código es equivalente a lo que hace la canalización automáticamente.

## <a name="default-content-negotiator"></a>Negociador de contenido predeterminado

La clase **DefaultContentNegotiator** proporciona la implementación predeterminada de **IContentNegotiator**. Usa varios criterios para seleccionar un formateador.

En primer lugar, el formateador debe ser capaz de serializar el tipo. Esto se comprueba mediante una llamada a **elemento mediatypeformatter. CanWriteType**.

Después, el negociador de contenido examina cada formateador y evalúa lo bien que coincide con la solicitud HTTP. Para evaluar la coincidencia, el negociador de contenido examina dos cosas en el formateador:

- La colección **SupportedMediaTypes** , que contiene una lista de los tipos de medios admitidos. El negociador de contenido intenta coincidir con esta lista con el encabezado Accept de la solicitud. Tenga en cuenta que el encabezado Accept puede incluir intervalos. Por ejemplo, "text/plain" es una coincidencia para Text/\* o \*/\*.
- La colección **MediaTypeMappings** , que contiene una lista de objetos **MediaTypeMapping** . La clase **MediaTypeMapping** proporciona una manera genérica de hacer coincidir las solicitudes HTTP con los tipos de medios. Por ejemplo, podría asignar un encabezado HTTP personalizado a un tipo de medio determinado.

Si hay varias coincidencias, gana la coincidencia con el factor de calidad más alto. Por ejemplo:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

En este ejemplo, Application/JSON tiene un factor de calidad implícito de 1,0, por lo que es preferible usar Application/XML.

Si no se encuentran coincidencias, el negociador de contenido intenta coincidir con el tipo de medio del cuerpo de la solicitud, si existe. Por ejemplo, si la solicitud contiene datos JSON, el negociador de contenido busca un formateador JSON.

Si todavía no hay ninguna coincidencia, el negociador de contenido simplemente selecciona el primer formateador que puede serializar el tipo.

## <a name="selecting-a-character-encoding"></a>Seleccionar una codificación de caracteres

Una vez seleccionado un formateador, el negociador de contenido elige la mejor codificación de caracteres examinando la propiedad **SupportedEncodings** en el formateador y cotejando con el encabezado Accept-charset de la solicitud (si existe).
