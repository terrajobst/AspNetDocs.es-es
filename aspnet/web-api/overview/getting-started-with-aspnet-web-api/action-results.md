---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Resultados de la acción en Web API 2-ASP.NET 4. x
author: MikeWasson
description: Describe cómo ASP.NET Web API convierte el valor devuelto de una acción del controlador en un mensaje de respuesta HTTP en ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448777"
---
# <a name="action-results-in-web-api-2"></a>Resultados de la acción en Web API 2

[!INCLUDE[](~/includes/coreWebAPI.md)]

En este tema se describe cómo ASP.NET Web API convierte el valor devuelto de una acción del controlador en un mensaje de respuesta HTTP.

Una acción del controlador de API Web puede devolver cualquiera de los siguientes elementos:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Otro tipo

Dependiendo de cuál de estos se devuelva, la API Web utiliza un mecanismo diferente para crear la respuesta HTTP.

| Tipo devuelto | Cómo crea la API Web la respuesta |
| --- | --- |
| void | Devolver 204 vacío (sin contenido) |
| **HttpResponseMessage** | Convertir directamente en un mensaje de respuesta HTTP. |
| **IHttpActionResult** | Llame a **ExecuteAsync** para crear un **HttpResponseMessage**y, a continuación, convierta en un mensaje de respuesta http. |
| Otro tipo | Escribe el valor devuelto serializado en el cuerpo de la respuesta; Devuelve 200 (correcto). |

En el resto de este tema se describe cada opción con más detalle.

## <a name="void"></a>void

Si el tipo de valor devuelto es `void`, Web API simplemente devuelve una respuesta HTTP vacía con el código de estado 204 (sin contenido).

Controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Respuesta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>httpResponseMessage

Si la acción devuelve un [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), la API Web convierte el valor devuelto directamente en un mensaje de respuesta http, utilizando las propiedades del objeto **HttpResponseMessage** para rellenar la respuesta.

Esta opción proporciona un gran control sobre el mensaje de respuesta. Por ejemplo, la siguiente acción del controlador establece el encabezado Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Si pasa un modelo de dominio al método **CreateResponse** , la API Web utiliza un [formateador de medios](../formats-and-model-binding/media-formatters.md) para escribir el modelo serializado en el cuerpo de la respuesta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web API usa el encabezado Accept en la solicitud para elegir el formateador. Para obtener más información, vea [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

La interfaz **IHttpActionResult** se presentó en Web API 2. Esencialmente, define un generador **HttpResponseMessage** . Estas son algunas ventajas del uso de la interfaz **IHttpActionResult** :

- Simplifica las [pruebas unitarias](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de los controladores.
- Mueve la lógica común para crear respuestas HTTP en clases independientes.
- Hace que la intención de la acción del controlador sea más clara ocultando los detalles de bajo nivel de la construcción de la respuesta.

**IHttpActionResult** contiene un único método, **ExecuteAsync**, que crea de forma asincrónica una instancia de **HttpResponseMessage** .

[!code-csharp[Main](action-results/samples/sample6.cs)]

Si una acción de controlador devuelve un **IHttpActionResult**, la API Web llama al método **ExecuteAsync** para crear un **HttpResponseMessage**. A continuación, convierte **HttpResponseMessage** en un mensaje de respuesta http.

A continuación se muestra una implementación sencilla de **IHttpActionResult** que crea una respuesta de texto sin formato:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Acción de controlador de ejemplo:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Respuesta:

[!code-console[Main](action-results/samples/sample9.cmd)]

Con mayor frecuencia, se usan las implementaciones de **IHttpActionResult** definidas en el espacio de nombres **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** . La clase **ApiController** define métodos auxiliares que devuelven estos resultados de acción integrados.

En el ejemplo siguiente, si la solicitud no coincide con un ID. de producto existente, el controlador llama a [ApiController. notfound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para crear una respuesta 404 (no encontrada). De lo contrario, el controlador llama a [ApiController. OK](https://msdn.microsoft.com/library/dn314591.aspx), que crea una respuesta 200 (OK) que contiene el producto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Otros tipos de valor devueltos

Para todos los demás tipos de valor devueltos, la API Web utiliza un [formateador de medios](../formats-and-model-binding/media-formatters.md) para serializar el valor devuelto. La API Web escribe el valor serializado en el cuerpo de la respuesta. El código de estado de la respuesta es 200 (correcto).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Un inconveniente de este enfoque es que no se puede devolver directamente un código de error, como 404. Sin embargo, puede iniciar una **HttpResponseException** para los códigos de error. Para obtener más información, vea [control de excepciones en ASP.net web API](../error-handling/exception-handling.md).

Web API usa el encabezado Accept en la solicitud para elegir el formateador. Para obtener más información, vea [negociación de contenido](../formats-and-model-binding/content-negotiation.md).

Solicitud de ejemplo

[!code-console[Main](action-results/samples/sample12.cmd)]

Respuesta de ejemplo

[!code-console[Main](action-results/samples/sample13.cmd)]
