---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504703"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Control de excepciones en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe el control de errores y excepciones en ASP.NET Web API.

- [HttpResponseException](#httpresponserexception)
- [Filtros de excepciones](#exception_filters)
- [Registrar filtros de excepciones](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

¿Qué ocurre si un controlador de API Web produce una excepción no detectada? De forma predeterminada, la mayoría de las excepciones se traducen en una respuesta HTTP con el código de estado 500, error interno del servidor.

El tipo **HttpResponseException** es un caso especial. Esta excepción devuelve cualquier código de Estado HTTP que especifique en el constructor de la excepción. Por ejemplo, el método siguiente devuelve 404, no encontrado, si el parámetro *ID* no es válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para obtener más control sobre la respuesta, también puede crear el mensaje de respuesta completo e incluirlo con el **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de excepciones

Puede personalizar el modo en que la API Web controla las excepciones escribiendo un *filtro de excepciones*. Se ejecuta un filtro de excepción cuando un método de controlador produce una excepción no controlada que *no* es una excepción **HttpResponseException** . El tipo **HttpResponseException** es un caso especial, ya que está diseñado específicamente para devolver una respuesta http.

Los filtros de excepción implementan la interfaz **System. Web. http. filters. IExceptionFilter** . La manera más sencilla de escribir un filtro de excepción es derivar de la clase **System. Web. http. filters. ExceptionFilterAttribute** e invalidar **el método de excepción.**

> [!NOTE]
> Los filtros de excepción en ASP.NET Web API son similares a los de ASP.NET MVC. Sin embargo, se declaran en un espacio de nombres independiente y una función por separado. En concreto, la clase **HandleErrorAttribute** que se usa en MVC no controla las excepciones iniciadas por los controladores de API Web.

Este es un filtro que convierte las excepciones **NotImplementedException** en el código de estado http 501, no implementado:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

La propiedad **Response** del objeto **HttpActionExecutedContext** contiene el mensaje de respuesta HTTP que se enviará al cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrar filtros de excepciones

Hay varias formas de registrar un filtro de excepción API web:

- Por acción
- Por controlador
- Globalmente

Para aplicar el filtro a una acción concreta, agregue el filtro como un atributo a la acción:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar el filtro a todas las acciones de un controlador, agregue el filtro como un atributo a la clase de controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar el filtro globalmente a todos los controladores de la API Web, agregue una instancia del filtro a la colección **GlobalConfiguration. Configuration. filters** . Los filtros de excepción de esta colección se aplican a cualquier acción de controlador de API web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Si usa la plantilla de proyecto "aplicación web MVC 4 de ASP.NET" para crear el proyecto, coloque el código de configuración de la API Web dentro de la clase `WebApiConfig`, que se encuentra en la carpeta app\_Inicio:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

El objeto **HttpError** proporciona una manera coherente de devolver información de error en el cuerpo de la respuesta. En el ejemplo siguiente se muestra cómo devolver el código de Estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de la respuesta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** es un método de extensión definido en la clase **System .net. http. HttpRequestMessageExtensions** . Internamente, **CreateErrorResponse** crea una instancia de **HttpError** y, a continuación, crea un **HttpResponseMessage** que contiene **HttpError**.

En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP. Pero si no se encuentra el producto solicitado, la respuesta HTTP contiene un **HttpError** en el cuerpo de la solicitud. La respuesta podría ser similar a la siguiente:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Observe que **HttpError** se serializó a JSON en este ejemplo. Una ventaja de usar **HttpError** es que pasa por el mismo proceso [de](../formats-and-model-binding/content-negotiation.md) serialización y negociación de contenido que cualquier otro modelo con establecimiento inflexible de tipos.

### <a name="httperror-and-model-validation"></a>HttpError y validación del modelo

Para la validación del modelo, puede pasar el estado del modelo a **CreateErrorResponse**para incluir los errores de validación en la respuesta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Este ejemplo puede devolver la siguiente respuesta:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obtener más información sobre la validación de modelos, vea [validación de modelos en ASP.net web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Uso de HttpError con HttpResponseException

En los ejemplos anteriores se devuelve un mensaje **HttpResponseMessage** desde la acción del controlador, pero también puede usar **HttpResponseException** para devolver una **HttpError**. Esto le permite devolver un modelo fuertemente tipado en el caso de éxito normal y, al mismo tiempo, devolver **HttpError** si se produce un error:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
