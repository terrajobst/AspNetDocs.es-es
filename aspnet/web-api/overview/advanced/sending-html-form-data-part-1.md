---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviar datos de formulario HTML en ASP.NET Web API: form-urlencoded Data-ASP.NET 4. x'
author: MikeWasson
description: En este artículo se muestra cómo publicar datos de form-urlencoded en un controlador de API Web con ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449245"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Enviar datos de formulario HTML en ASP.NET Web API: datos form-urlencoded

por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Parte 1: datos form-urlencoded

En este artículo se muestra cómo publicar datos de form-urlencoded en un controlador de API Web.

- [Información general de formularios HTML](#overview_of_html_forms)
- [Envío de tipos complejos](#sending_complex_types)
- [Envío de datos de formulario a través de AJAX](#sending_form_data_via_ajax)
- [Enviar tipos simples](#sending_simple_types)

> [!NOTE]
> [Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Información general de formularios HTML

Los formularios HTML utilizan GET o POST para enviar datos al servidor. El atributo **Method** del elemento **Form** proporciona el método http:

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

El método predeterminado es GET. Si el formulario usa GET, los datos del formulario se codifican en el URI como una cadena de consulta. Si el formulario usa POST, los datos del formulario se colocan en el cuerpo de la solicitud. Para los datos enviados, el atributo **enctype** especifica el formato del cuerpo de la solicitud:

| enctype | Description |
| --- | --- |
| application/x-www-form-urlencoded | Los datos de formulario se codifican como pares de nombre/valor, de forma similar a una cadena de consulta de URI. Este es el formato predeterminado para POST. |
| multipart/form-data | Los datos de formulario se codifican como un mensaje MIME de varias partes. Use este formato si va a cargar un archivo en el servidor. |

En la primera parte de este artículo se examina el formato x-www-form-urlencoded. La [parte 2](sending-html-form-data-part-2.md) describe MIME con varias partes.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Envío de tipos complejos

Normalmente, enviará un tipo complejo, compuesto por valores tomados de varios controles de formulario. Considere el siguiente modelo que representa una actualización de estado:

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Este es un controlador de API Web que acepta un objeto `Update` a través de POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Este controlador usa el [enrutamiento basado en acciones](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), por lo que la plantilla de ruta es &quot;API/{Controller}/{Action}/{id}&quot;. El cliente publicará los datos en &quot;&quot;/API/updates/Complex.

Ahora vamos a escribir un formulario HTML para que los usuarios envíen una actualización de estado.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Observe que el atributo **Action** del formulario es el URI de la acción del controlador. Este es el formulario con algunos valores introducidos en:

![](sending-html-form-data-part-1/_static/image1.png)

Cuando el usuario hace clic en enviar, el explorador envía una solicitud HTTP similar a la siguiente:

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Observe que el cuerpo de la solicitud contiene los datos del formulario, con formato de pares nombre-valor. Web API convierte automáticamente los pares de nombre y valor en una instancia de la clase `Update`.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Envío de datos de formulario a través de AJAX

Cuando un usuario envía un formulario, el explorador sale de la página actual y representa el cuerpo del mensaje de respuesta. Es correcto cuando la respuesta es una página HTML. Sin embargo, con una API Web, el cuerpo de la respuesta suele estar vacío o contener datos estructurados, como JSON. En ese caso, tiene más sentido enviar los datos del formulario mediante una solicitud AJAX, de modo que la página pueda procesar la respuesta.

En el código siguiente se muestra cómo publicar datos de formulario mediante jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

La función de **envío** de jQuery reemplaza la acción de formulario por una nueva función. Esto invalida el comportamiento predeterminado del botón Enviar. La función **Serialize** serializa los datos del formulario en pares de nombre/valor. Para enviar los datos del formulario al servidor, llame a `$.post()`.

Cuando se completa la solicitud, el controlador `.success()` o `.error()` muestra al usuario un mensaje adecuado.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Enviar tipos simples

En las secciones anteriores, enviamos un tipo complejo, que API Web deserializaba a una instancia de una clase de modelo. También puede enviar tipos simples, como una cadena.

> [!NOTE]
> Antes de enviar un tipo simple, considere la posibilidad de ajustar el valor en un tipo complejo en su lugar. Esto le ofrece las ventajas de la validación de modelos en el lado servidor y facilita la ampliación del modelo si es necesario.

Los pasos básicos para enviar un tipo simple son los mismos, pero hay dos diferencias sutiles. En primer lugar, en el controlador, debe decorar el nombre del parámetro con el atributo **FromBody** .

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

De forma predeterminada, Web API intenta obtener tipos simples del URI de solicitud. El atributo **FromBody** indica a la API Web que lea el valor del cuerpo de la solicitud.

> [!NOTE]
> Web API lee el cuerpo de la respuesta una vez como máximo, por lo que solo un parámetro de una acción puede proviene del cuerpo de la solicitud. Si necesita obtener varios valores del cuerpo de la solicitud, defina un tipo complejo.

En segundo lugar, el cliente debe enviar el valor con el siguiente formato:

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

En concreto, la parte del nombre del par nombre-valor debe estar vacía para un tipo simple. No todos los exploradores son compatibles con los formularios HTML, pero se crea este formato en el script de la siguiente manera:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Este es un formulario de ejemplo:

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Y este es el script para enviar el valor del formulario. La única diferencia del script anterior es el argumento pasado a la función **post** .

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Puede usar el mismo enfoque para enviar una matriz de tipos simples:

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Recursos adicionales

[Parte 2: carga de archivos y MIME de varias partes](sending-html-form-data-part-2.md)
