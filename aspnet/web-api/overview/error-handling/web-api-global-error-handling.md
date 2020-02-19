---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Control de errores global en ASP.NET Web API 2-ASP.NET 4. x
author: davidmatson
description: Información general sobre el control de errores global en ASP.NET Web API 2 para ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457744"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Control de errores global en ASP.NET Web API 2

por [David paspartúson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tema se proporciona información general sobre el control de errores global en ASP.NET Web API 2 para ASP.NET 4. x. Actualmente no hay ninguna manera fácil en la API Web de registrar o controlar los errores de forma global. Algunas excepciones no controladas se pueden procesar a través de [filtros de excepción](exception-handling.md), pero hay varios casos en los que los filtros de excepción no pueden controlar. Por ejemplo:

1. Excepciones iniciadas por constructores del controlador.
2. Excepciones iniciadas por controladores de mensajes.
3. Excepciones iniciadas durante el enrutamiento.
4. Excepciones producidas durante la serialización del contenido de la respuesta.

Queremos proporcionar una manera sencilla y coherente de registrar y controlar (siempre que sea posible) estas excepciones. 

Hay dos casos principales para controlar las excepciones, el caso en el que podemos enviar una respuesta de error y el caso en el que todo lo que podemos hacer es registrar la excepción. Un ejemplo de este último caso es cuando se produce una excepción en medio del contenido de respuesta de streaming; en ese caso, es demasiado tarde para enviar un nuevo mensaje de respuesta, ya que el código de estado, los encabezados y el contenido parcial ya han ido a través de la conexión, por lo que simplemente anulamos la conexión. Aunque no se puede controlar la excepción para producir un nuevo mensaje de respuesta, todavía se admite el registro de la excepción. En los casos en los que se puede detectar un error, se puede devolver una respuesta de error adecuada como se muestra a continuación:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opciones existentes

Además de los [filtros de excepciones](exception-handling.md), se pueden usar [controladores de mensajes](../advanced/http-message-handlers.md) hoy mismo para observar todas las respuestas de nivel 500, pero actuar en esas respuestas es difícil, ya que carecen de contexto sobre el error original. Los controladores de mensajes también tienen algunas de las mismas limitaciones que los filtros de excepciones en lo que respecta a los casos que pueden controlar. Mientras que la API Web tiene una infraestructura de seguimiento que captura condiciones de error, la infraestructura de seguimiento es para fines de diagnóstico y no está diseñada ni adecuada para ejecutarse en entornos de producción. El control de excepciones y el registro globales deben ser servicios que pueden ejecutarse durante la producción y estar conectados a soluciones de supervisión existentes (por ejemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Información general de la solución

 Proporcionamos dos nuevos servicios reemplazables por el usuario, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) y IExceptionHandler, para registrar y controlar las excepciones no controladas. Los servicios son muy similares, con dos diferencias principales:

1. Se admite el registro de varios registradores de excepciones, pero solo un controlador de excepciones.
2. Siempre se llama a los registradores de excepciones, incluso si vamos a anular la conexión. Solo se llama a los controladores de excepciones cuando todavía es posible elegir el mensaje de respuesta que se va a enviar.

Ambos servicios proporcionan acceso a un contexto de excepción que contiene información pertinente desde el punto en el que se detectó la excepción, especialmente en [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), la excepción producida y el origen de la excepción (más abajo).

### <a name="design-principles"></a>Principios de diseño

1. **No hay cambios importantes** Dado que esta funcionalidad se agrega en una versión secundaria, una restricción importante que afecta a la solución es que no hay cambios importantes, ya sea para el tipo de contratos o para el comportamiento. Esta restricción descartaba parte de la limpieza que nos gustaría hacer en cuanto a los bloques Catch existentes que convierten las excepciones en respuestas 500. Esta limpieza adicional es algo que podríamos considerar para una versión principal posterior. Si esto es importante para usted, vote en [ASP.net web API Voice User](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Mantener la coherencia con las construcciones de la API Web** La canalización de filtros de la API Web es una excelente manera de controlar las cuestiones transversales con la flexibilidad de aplicar la lógica en un ámbito específico de la acción, específico del controlador o global. Los filtros, incluidos los filtros de excepciones, siempre tienen contextos de acción y controlador, incluso cuando se registran en el ámbito global. Dicho contrato tiene sentido para los filtros, pero significa que los filtros de excepciones, incluso los de ámbito global, no son una buena opción para algunos casos de control de excepciones, como las excepciones de los controladores de mensajes, donde no existe ningún contexto de acción o controlador. Si queremos usar el ámbito flexible que ofrecen los filtros para el control de excepciones, todavía necesitamos filtros de excepciones. Pero si es necesario controlar la excepción fuera de un contexto del controlador, también se necesita una construcción independiente para el control de errores global completo (algo sin el contexto del controlador y las restricciones de contexto de acción).

### <a name="when-to-use"></a>Cuándo usar

- Los registradores de excepciones son la solución para ver todas las excepciones no controladas detectadas por la API Web.
- Los controladores de excepciones son la solución para personalizar todas las respuestas posibles a las excepciones no controladas detectadas por Web API.
- Los filtros de excepciones son la solución más sencilla para procesar las excepciones no controladas del subconjunto relacionadas con una acción o un controlador específicos.

### <a name="service-details"></a>Detalles del servicio

 Las interfaces del registrador de excepciones y del servicio de controlador son métodos asincrónicos simples que toman los contextos respectivos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 También proporcionamos clases base para ambas interfaces. Reemplazar los métodos principales (Sync o Async) es todo lo que se necesita para registrar o controlar en los momentos recomendados. Para el registro, la clase base `ExceptionLogger` garantizará que solo se llame una vez al método de registro principal para cada excepción (incluso si posteriormente se propaga más adelante en la pila de llamadas y se vuelve a capturar). La clase base `ExceptionHandler` llamará al método Core Handling solo para las excepciones en la parte superior de la pila de llamadas, omitiendo los bloques Catch anidados heredados. (Las versiones simplificadas de estas clases base se encuentran en el apéndice siguiente). Tanto `IExceptionLogger` como `IExceptionHandler` reciben información sobre la excepción a través de un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Cuando el marco de trabajo llama a un registrador de excepciones o a un controlador de excepciones, siempre proporcionará un `Exception` y un `Request`. A excepción de las pruebas unitarias, también proporcionará siempre una `RequestContext`. Rara vez proporcionará un `ControllerContext` y `ActionContext` (solo cuando se llame a desde el bloque catch para los filtros de excepción). Normalmente, proporcionará un `Response`(solo en ciertos casos de IIS en medio de intentar escribir la respuesta). Tenga en cuenta que, dado que algunas de estas propiedades se pueden `null` es el consumidor el que debe buscar `null` antes de tener acceso a los miembros de la clase de excepción.`CatchBlock` es una cadena que indica el bloque catch que vio la excepción. Las cadenas de bloques Catch son las siguientes:

- HttpServer (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (procesamiento de ApiController de la canalización de filtro de excepciones en ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter. BufferResponseContentAsync (para almacenar en búfer los resultados)
    - HttpMessageHandlerAdapter. CopyResponseContentAsync (para la salida de streaming)
- Host Web:

    - HttpControllerHandler. WriteBufferedResponseContentAsync (para almacenar en búfer los resultados)
    - HttpControllerHandler. WriteStreamedResponseContentAsync (para la salida de streaming)
    - HttpControllerHandler. WriteErrorResponseContentAsync (para errores en la recuperación de errores en el modo de salida almacenado en búfer)

La lista de cadenas de bloques Catch también está disponible a través de propiedades de solo lectura estáticas. (La cadena del bloque catch principal está en el ExceptionCatchBlocks estático; el resto aparece en una clase estática para OWIN y el host Web).`IsTopLevelCatchBlock` resulta útil para seguir el modelo recomendado de control de excepciones solo en la parte superior de la pila de llamadas. En lugar de convertir excepciones en respuestas 500 en cualquier lugar en que se produzca un bloque catch anidado, un controlador de excepciones puede permitir que las excepciones se propaguen hasta que estén a punto de ser visibles por el host.

Además del `ExceptionContext`, un registrador obtiene una parte más de la información a través de la `ExceptionLoggerContext`completa:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La segunda propiedad, `CanBeHandled`, permite a un registrador identificar una excepción que no se puede controlar. Cuando la conexión está a punto de anularse y no se puede enviar un nuevo mensaje de respuesta, se llamará a los registradores, pero ***no*** se llamará al controlador y los registradores podrán identificar este escenario a partir de esta propiedad.

Además del `ExceptionContext`, un controlador obtiene una propiedad más que puede establecer en el `ExceptionHandlerContext` completo para controlar la excepción:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un controlador de excepciones indica que ha controlado una excepción estableciendo la propiedad `Result` en un resultado de acción (por ejemplo, [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)o un resultado personalizado). Si la propiedad `Result` es null, la excepción no se controla y se vuelve a producir la excepción original.

En el caso de las excepciones en la parte superior de la pila de llamadas, hemos realizado un paso adicional para asegurarse de que la respuesta sea adecuada para los autores de llamadas de API. Si la excepción se propaga hasta el host, el autor de la llamada verá la pantalla amarilla de muerte o cualquier otra respuesta proporcionada por el host, que suele ser HTML y no suele ser una respuesta de error de API adecuada. En estos casos, el resultado comienza en un valor distinto de NULL y solo si un controlador de excepciones personalizado lo vuelve a establecer explícitamente en `null` (no controlado), la excepción se propagará al host. Establecer `Result` en `null` en estos casos puede ser útil para dos escenarios:

1. API Web hospedada por OWIN con middleware de control de excepciones personalizado registrado antes o fuera de la API Web.
2. Depuración local a través de un explorador, donde la pantalla amarilla de muerte es realmente una respuesta útil para una excepción no controlada.

En el caso de los registradores de excepciones y los controladores de excepciones, no hacemos nada para recuperar si el registrador o el propio controlador inicia una excepción. (Aparte de permitir que la excepción se propague, deje comentarios en la parte inferior de esta página si tiene un enfoque mejor). El contrato para los registradores y controladores de excepciones es que no deben permitir que las excepciones se propaguen a sus llamadores. de lo contrario, la excepción se propagará, a menudo hasta el host, con lo que se producirá un error HTML (como el ASP). Pantalla amarilla de la red) que se envía al cliente (que normalmente no es la opción preferida para los llamadores de API que esperan JSON o XML).

## <a name="examples"></a>Ejemplos

### <a name="tracing-exception-logger"></a>Registrador de excepciones de seguimiento

El registrador de excepciones siguiente envía datos de excepción a los orígenes de seguimiento configurados (incluida la ventana de salida de depuración en Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Controlador de excepciones de mensajes de error personalizados

A continuación se genera una respuesta de error personalizada a los clientes, incluida una dirección de correo electrónico para ponerse en contacto con el soporte técnico.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrar filtros de excepciones

Si usa la plantilla de proyecto "aplicación web MVC 4 ASP.NET" para crear el proyecto, coloque el código de configuración de la API Web dentro de la clase `WebApiConfig`, en la carpeta *App/_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apéndice: detalles de la clase base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
