---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449251"
---
# <a name="routing-in-aspnet-web-api"></a>Enrutar en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describe cómo ASP.NET Web API enruta las solicitudes HTTP a los controladores.

> [!NOTE]
> Si está familiarizado con ASP.NET MVC, el enrutamiento de API Web es muy similar al enrutamiento de MVC. La principal diferencia es que la API Web usa el verbo HTTP, no la ruta de acceso del URI, para seleccionar la acción. También puede usar el enrutamiento de estilo MVC en Web API. En este artículo no se asume ningún conocimiento de ASP.NET MVC.

## <a name="routing-tables"></a>Tablas de enrutamiento

En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP. Los métodos públicos del controlador se denominan *métodos de acción* o simplemente *acciones*. Cuando el marco de la API Web recibe una solicitud, enruta la solicitud a una acción.

Para determinar la acción que se va a invocar, el marco de trabajo utiliza una *tabla de enrutamiento*. La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Esta ruta se define en el archivo *WebApiConfig.CS* , que se coloca en el directorio de inicio de la *aplicación\_* :

![](routing-in-aspnet-web-api/_static/image1.png)

Para obtener más información sobre la clase `WebApiConfig`, vea [configurar ASP.net web API](../advanced/configuring-aspnet-web-api.md).

Si Self-host Web API, debe establecer la tabla de enrutamiento directamente en el objeto `HttpSelfHostConfiguration`. Para obtener más información, consulte [Autohospedar una API Web](../older-versions/self-host-a-web-api.md).

Cada entrada de la tabla de enrutamiento contiene una *plantilla de ruta*. La plantilla de ruta predeterminada para Web API es &quot;API/{Controller}/{ID}&quot;. En esta plantilla, &quot;API&quot; es un segmento de ruta de acceso literal, y {Controller} y {ID} son variables de marcador de posición.

Cuando el marco de la API Web recibe una solicitud HTTP, intenta hacer coincidir el URI con una de las plantillas de ruta de la tabla de enrutamiento. Si no coincide ninguna ruta, el cliente recibe un error 404. Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:

- /api/contacts
- /api/contacts/1
- /api/products/gizmo1

Sin embargo, el URI siguiente no coincide porque carece del segmento&quot; de &quot;API:

- /contacts/1

> [!NOTE]
> La razón para usar "API" en la ruta es evitar colisiones con el enrutamiento de MVC de ASP.NET. De este modo, puede tener &quot;/Contacts&quot; ir a un controlador MVC y &quot;/API/Contacts&quot; ir a un controlador de API Web. Por supuesto, si no le gusta esta Convención, puede cambiar la tabla de ruta predeterminada.

Una vez que se encuentra una ruta coincidente, la API Web selecciona el controlador y la acción:

- Para buscar el controlador, la API Web agrega &quot;controlador&quot; al valor de la variable *{Controller}* .
- Para encontrar la acción, la API Web examina el verbo HTTP y busca una acción cuyo nombre comienza con ese nombre de verbo HTTP. Por ejemplo, con una solicitud GET, la API Web busca una acción con el prefijo &quot;Get&quot;, como &quot;GetContact&quot; o &quot;GetAllContacts&quot;. Esta Convención solo se aplica a los verbos GET, POST, PUT, DELETE, HEAD, OPTIONs y PATCH. Puede habilitar otros verbos HTTP mediante el uso de atributos en el controlador. Veremos un ejemplo más adelante.
- Otras variables de marcador de posición de la plantilla de ruta, como *{ID},* se asignan a los parámetros de acción.

Veamos un ejemplo. Supongamos que define el siguiente controlador:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

A continuación se indican algunas posibles solicitudes HTTP, junto con la acción que se invoca para cada una:

| Verbo HTTP | Ruta de acceso de URI | Acción | Parámetro |
| --- | --- | --- | --- |
| GET | API/productos | GetAllProducts | *ninguna* |
| GET | API/productos/4 | GetProductById | 4 |
| SUPRIMIR | API/productos/4 | DeleteProduct | 4 |
| EXPONER | API/productos | *(sin coincidencia)* |  |

Observe que el segmento *{ID}* del URI, si está presente, se asigna al parámetro *ID* de la acción. En este ejemplo, el controlador define dos métodos GET, uno con un parámetro *ID* y otro sin parámetros.

Además, tenga en cuenta que se producirá un error en la solicitud POST porque el controlador no define un método &quot;post...&quot;.

## <a name="routing-variations"></a>Variaciones de enrutamiento

En la sección anterior se describe el mecanismo de enrutamiento básico para ASP.NET Web API. En esta sección se describen algunas variaciones.

### <a name="http-verbs"></a>verbos HTTP

En lugar de usar la Convención de nomenclatura para los verbos HTTP, se puede especificar explícitamente el verbo HTTP para una acción si se decora el método de acción con uno de los siguientes atributos:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

En el ejemplo siguiente, el método `FindProduct` se asigna a las solicitudes GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Para permitir varios verbos HTTP para una acción, o para permitir verbos HTTP distintos de GET, PUT, POST, DELETE, HEAD, OPTIONs y PATCH, utilice el atributo `[AcceptVerbs]`, que toma una lista de verbos HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Enrutamiento por nombre de acción

Con la plantilla de enrutamiento predeterminada, la API Web usa el verbo HTTP para seleccionar la acción. Sin embargo, también puede crear una ruta en la que el nombre de la acción se incluya en el URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

En esta plantilla de ruta, el parámetro *{Action}* nombra el método de acción en el controlador. Con este estilo de enrutamiento, use atributos para especificar los verbos HTTP permitidos. Por ejemplo, suponga que el controlador tiene el método siguiente:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

En este caso, una solicitud GET de "API/products/details/1" se asignaría al método `Details`. Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.

Puede invalidar el nombre de la acción mediante el atributo `[ActionName]`. En el ejemplo siguiente, hay dos acciones que se asignan a &quot;API/Products/thumbnail/*ID*. Uno admite GET y el otro admite POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>No acciones

Para evitar que un método se invoque como una acción, use el atributo `[NonAction]`. Esto indica al marco de trabajo que el método no es una acción, aunque de otro modo coincidiría con las reglas de enrutamiento.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Información adicional

En este tema se proporciona una vista de alto nivel del enrutamiento. Para obtener más información, consulte [enrutamiento y selección de acciones](routing-and-action-selection.md), que describe exactamente cómo el marco de trabajo coincide con un URI de una ruta, selecciona un controlador y, a continuación, selecciona la acción que se va a invocar.
