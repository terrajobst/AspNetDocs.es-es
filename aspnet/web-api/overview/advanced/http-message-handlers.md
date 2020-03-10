---
uid: web-api/overview/advanced/http-message-handlers
title: Controladores de mensajes HTTP en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Información general de los controladores de mensajes HTTP en ASP.NET Web API para ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504931"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a>Controladores de mensajes HTTP en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta http. Los controladores de mensajes derivan de la clase **HttpMessageHandler** abstracta.

Normalmente, una serie de controladores de mensajes se encadenan entre sí. El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador. En algún momento, se crea la respuesta y se realiza una copia de seguridad de la cadena. Este patrón se denomina controlador de *delegación* .

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Controladores de mensajes del servidor

En el lado del servidor, la canalización de la API Web usa algunos controladores de mensajes integrados:

- **HttpServer** obtiene la solicitud del host.
- **HttpRoutingDispatcher** envía la solicitud en función de la ruta.
- **HttpControllerDispatcher** envía la solicitud a un controlador de API Web.

Puede agregar controladores personalizados a la canalización. Los controladores de mensajes son buenos para cuestiones transversales que operan en el nivel de mensajes HTTP (en lugar de acciones de controlador). Por ejemplo, un controlador de mensajes podría:

- Leer o modificar los encabezados de solicitud.
- Agregue un encabezado de respuesta a las respuestas.
- Valide las solicitudes antes de que lleguen al controlador.

En este diagrama se muestran dos controladores personalizados insertados en la canalización:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> En el lado del cliente, HttpClient también usa controladores de mensajes. Para obtener más información, consulte [controladores de mensajes HttpClient](httpclient-message-handlers.md).

## <a name="custom-message-handlers"></a>Controladores de mensajes personalizados

Para escribir un controlador de mensajes personalizado, derive de **System .net. http. DelegatingHandler** e invalide el método **SendAsync** . Este método tiene la siguiente firma:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**. Una implementación típica hace lo siguiente:

1. Procesar el mensaje de solicitud.
2. Llame a `base.SendAsync` para enviar la solicitud al controlador interno.
3. El controlador interno devuelve un mensaje de respuesta. (Este paso es asincrónico).
4. Procesa la respuesta y la devuelve al autor de la llamada.

Este es un ejemplo trivial:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> La llamada a `base.SendAsync` es asincrónica. Si el controlador realiza cualquier trabajo después de esta llamada, use la palabra clave **Await** , como se muestra.

Un controlador de delegación también puede omitir el controlador interno y crear directamente la respuesta:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Si un controlador de delegación crea la respuesta sin llamar a `base.SendAsync`, la solicitud omite el resto de la canalización. Esto puede ser útil para un controlador que valida la solicitud (creando una respuesta de error).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Agregar un controlador a la canalización

Para agregar un controlador de mensajes en el lado servidor, agregue el controlador a la colección **HttpConfiguration. MessageHandlers** . Si usó la plantilla "aplicación Web de ASP.NET MVC 4" para crear el proyecto, puede hacerlo dentro de la clase **WebApiConfig** :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Se llama a los controladores de mensajes en el mismo orden en que aparecen en la colección **MessageHandlers** . Dado que están anidados, el mensaje de respuesta se desplaza en la otra dirección. Es decir, el último controlador es el primero en obtener el mensaje de respuesta.

Tenga en cuenta que no es necesario establecer los controladores internos; el marco de trabajo de la API Web conecta automáticamente los controladores de mensajes.

Si está [autohospedado](../older-versions/self-host-a-web-api.md), cree una instancia de la clase **HttpSelfHostConfiguration** y agregue los controladores a la colección **MessageHandlers** .

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Ahora veamos algunos ejemplos de controladores de mensajes personalizados.

## <a name="example-x-http-method-override"></a>Ejemplo: X-HTTP-Method-override

X-HTTP-Method-override es un encabezado HTTP no estándar. Está diseñado para los clientes que no pueden enviar determinados tipos de solicitud HTTP, como PUT o DELETE. En su lugar, el cliente envía una solicitud POST y establece el encabezado X-HTTP-Method-override en el método deseado. Por ejemplo:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Este es un controlador de mensajes que agrega compatibilidad con X-HTTP-Method-override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

En el método **SendAsync** , el controlador comprueba si el mensaje de solicitud es una solicitud post y si contiene el encabezado X-http-Method-override. Si es así, valida el valor de encabezado y, a continuación, modifica el método de solicitud. Por último, el controlador llama a `base.SendAsync` para pasar el mensaje al siguiente controlador.

Cuando la solicitud llega a la clase **HttpControllerDispatcher** , **HttpControllerDispatcher** enrutará la solicitud según el método de solicitud actualizado.

## <a name="example-adding-a-custom-response-header"></a>Ejemplo: agregar un encabezado de respuesta personalizado

Este es un controlador de mensajes que agrega un encabezado personalizado a cada mensaje de respuesta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

En primer lugar, el controlador llama a `base.SendAsync` para pasar la solicitud al controlador de mensajes interno. El controlador interno devuelve un mensaje de respuesta, pero lo hace de forma asincrónica mediante una **tarea&lt;t&gt;** objeto. El mensaje de respuesta no está disponible hasta que `base.SendAsync` se completa de forma asincrónica.

En este ejemplo se usa la palabra clave **Await** para realizar el trabajo de forma asincrónica después de que se complete `SendAsync`. Si tiene como destino .NET Framework 4,0, use la **tarea**&lt;t&gt; **. Método ContinueWith** :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Ejemplo: comprobar una clave de API

Algunos servicios Web requieren que los clientes incluyan una clave de API en su solicitud. En el ejemplo siguiente se muestra cómo un controlador de mensajes puede comprobar las solicitudes de una clave de API válida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Este controlador busca la clave de API en la cadena de consulta de URI. (En este ejemplo, se supone que la clave es una cadena estática. Una implementación real probablemente utilizará una validación más compleja). Si la cadena de consulta contiene la clave, el controlador pasa la solicitud al controlador interno.

Si la solicitud no tiene una clave válida, el controlador crea un mensaje de respuesta con el estado 403, prohibido. En este caso, el controlador no llama a `base.SendAsync`, por lo que el controlador interno nunca recibe la solicitud ni el controlador. Por lo tanto, el controlador puede suponer que todas las solicitudes entrantes tienen una clave de API válida.

> [!NOTE]
> Si la clave de API solo se aplica a determinadas acciones del controlador, considere la posibilidad de usar un filtro de acción en lugar de un controlador de mensajes. Los filtros de acción se ejecutan después de realizar el enrutamiento de URI.

## <a name="per-route-message-handlers"></a>Controladores de mensajes por ruta

Los controladores de la colección **HttpConfiguration. MessageHandlers** se aplican globalmente.

Como alternativa, puede Agregar un controlador de mensajes a una ruta específica al definir la ruta:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

En este ejemplo, si el URI de solicitud coincide con "Route2", la solicitud se envía a `MessageHandler2`. En el diagrama siguiente se muestra la canalización para estas dos rutas:

![](http-message-handlers/_static/image4.png)

Observe que `MessageHandler2` reemplaza el valor predeterminado de **HttpControllerDispatcher**. En este ejemplo, `MessageHandler2` crea la respuesta y las solicitudes que coinciden con "Route2" nunca van a un controlador. Esto le permite reemplazar todo el mecanismo del controlador de API Web por su propio punto de conexión personalizado.

Como alternativa, un controlador de mensajes por ruta puede delegar a **HttpControllerDispatcher**, que después envía a un controlador.

![](http-message-handlers/_static/image5.png)

En el código siguiente se muestra cómo configurar esta ruta:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
