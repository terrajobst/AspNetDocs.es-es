---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes HttpClient en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Cree controladores de mensajes personalizados para ASP.NET Web API en ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449281"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>Controladores de mensajes HttpClient en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta http.

Normalmente, una serie de controladores de mensajes se encadenan entre sí. El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador. En algún momento, se crea la respuesta y se realiza una copia de seguridad de la cadena. Este patrón se denomina controlador de *delegación* .

![](httpclient-message-handlers/_static/image1.png)

En el lado del cliente, la clase **HttpClient** usa un controlador de mensajes para procesar solicitudes. El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y obtiene la respuesta del servidor. Puede insertar controladores de mensajes personalizados en la canalización de cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API también utiliza controladores de mensajes en el lado servidor. Para obtener más información, consulte [controladores de mensajes http](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Controladores de mensajes personalizados

Para escribir un controlador de mensajes personalizado, derive de **System .net. http. DelegatingHandler** e invalide el método **SendAsync** . Esta es la firma del método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**. Una implementación típica hace lo siguiente:

1. Procesar el mensaje de solicitud.
2. Llame a `base.SendAsync` para enviar la solicitud al controlador interno.
3. El controlador interno devuelve un mensaje de respuesta. (Este paso es asincrónico).
4. Procesa la respuesta y la devuelve al autor de la llamada.

En el ejemplo siguiente se muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud saliente:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

La llamada a `base.SendAsync` es asincrónica. Si el controlador realiza cualquier trabajo después de esta llamada, use la palabra clave **Await** para reanudar la ejecución después de que se complete el método. En el ejemplo siguiente se muestra un controlador que registra códigos de error. El registro en sí no es muy interesante, pero en el ejemplo se muestra cómo obtener la respuesta dentro del controlador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Agregar controladores de mensajes a la canalización de cliente

Para agregar controladores personalizados a **HttpClient**, use el método **HttpClientFactory. Create** :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Se llama a los controladores de mensajes en el orden en que se pasan al método **Create** . Dado que los controladores están anidados, el mensaje de respuesta se desplaza en la otra dirección. Es decir, el último controlador es el primero en obtener el mensaje de respuesta.
