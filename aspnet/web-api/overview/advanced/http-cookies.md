---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe cómo enviar y recibir cookies HTTP en Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449317"
---
# <a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe cómo enviar y recibir cookies HTTP en Web API.

## <a name="background-on-http-cookies"></a>Información general sobre cookies HTTP

En esta sección se proporciona una breve descripción de cómo se implementan las cookies en el nivel HTTP. Para obtener más información, consulte [RFC 6265](http://tools.ietf.org/html/rfc6265).

Una cookie es un fragmento de datos que un servidor envía en la respuesta HTTP. El cliente (opcionalmente) almacena la cookie y la devuelve en solicitudes posteriores. Esto permite que el cliente y el servidor compartan el estado. Para establecer una cookie, el servidor incluye un encabezado Set-Cookie en la respuesta. El formato de una cookie es un par nombre-valor, con atributos opcionales. Por ejemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

A continuación se muestra un ejemplo con atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para devolver una cookie al servidor, el cliente incluye un encabezado de cookie en las solicitudes posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Una respuesta HTTP puede incluir varios encabezados Set-Cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

El cliente devuelve varias cookies mediante un encabezado de cookie único.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

El ámbito y la duración de una cookie se controlan mediante los siguientes atributos del encabezado Set-Cookie:

- **Dominio**: indica al cliente qué dominio debe recibir la cookie. Por ejemplo, si el dominio es "example.com", el cliente devuelve la cookie a todos los subdominios de example.com. Si no se especifica, el dominio es el servidor de origen.
- **Ruta de acceso**: restringe la cookie a la ruta de acceso especificada dentro del dominio. Si no se especifica, se usa la ruta de acceso del URI de solicitud.
- **Expires**: establece una fecha de expiración para la cookie. El cliente elimina la cookie cuando expira.
- **Max-Age**: establece la antigüedad máxima de la cookie. El cliente elimina la cookie cuando llega a la antigüedad máxima.

Si se establecen `Expires` y `Max-Age`, `Max-Age` tiene prioridad. Si no se establece ninguno, el cliente elimina la cookie cuando finaliza la sesión actual. (El significado exacto de "session" viene determinado por el agente de usuario).

Sin embargo, tenga en cuenta que los clientes pueden omitir las cookies. Por ejemplo, un usuario podría deshabilitar las cookies por motivos de privacidad. Los clientes pueden eliminar cookies antes de que expiren, o bien limitar el número de cookies almacenadas. Por motivos de privacidad, los clientes a menudo rechazan las cookies de "terceros", donde el dominio no coincide con el servidor de origen. En Resumen, el servidor no debe confiar en obtener las cookies que establece.

## <a name="cookies-in-web-api"></a>Cookies en Web API

Para agregar una cookie a una respuesta HTTP, cree una instancia de **CookieHeaderValue** que represente la cookie. A continuación, llame al método de extensión **AddCookies** , que se define en **System .net. http. Clase HttpResponseHeadersExtensions** , para agregar la cookie.

Por ejemplo, el código siguiente agrega una cookie dentro de una acción del controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Tenga en cuenta que **AddCookies** toma una matriz de instancias de **CookieHeaderValue** .

Para extraer las cookies de una solicitud de cliente, llame al método **GetCookies** :

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Un **CookieHeaderValue** contiene una colección de instancias de **CookieState** . Cada **CookieState** representa una cookie. Use el método de indexador para obtener un **CookieState** por nombre, como se muestra.

## <a name="structured-cookie-data"></a>Datos de cookies estructurados

Muchos exploradores limitan cuántas cookies almacenarán&#8212;tanto el número total como el número por dominio. Por lo tanto, puede ser útil poner los datos estructurados en una cookie única, en lugar de establecer varias cookies.

> [!NOTE]
> RFC 6265 no define la estructura de los datos de cookies.

Con la clase **CookieHeaderValue** , puede pasar una lista de pares nombre-valor para los datos de cookies. Estos pares de nombre y valor se codifican como datos de formulario con codificación URL en el encabezado Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

El código anterior genera el siguiente encabezado Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

La clase **CookieState** proporciona un método de indexador para leer los subvalores de una cookie en el mensaje de solicitud:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Ejemplo: establecimiento y recuperación de cookies en un controlador de mensajes

En los ejemplos anteriores se mostró cómo usar las cookies desde un controlador de API Web. Otra opción consiste en usar [controladores de mensajes](http-message-handlers.md). Los controladores de mensajes se invocan anteriormente en la canalización que los controladores. Un controlador de mensajes puede leer cookies de la solicitud antes de que la solicitud llegue al controlador o agregar cookies a la respuesta después de que el controlador genere la respuesta.

![](http-cookies/_static/image2.png)

En el código siguiente se muestra un controlador de mensajes para crear identificadores de sesión. El ID. de sesión se almacena en una cookie. El controlador comprueba la solicitud de la cookie de sesión. Si la solicitud no incluye la cookie, el controlador genera un nuevo identificador de sesión. En cualquier caso, el controlador almacena el identificador de sesión en el contenedor de propiedades **HttpRequestMessage. Properties** . También agrega la cookie de sesión a la respuesta HTTP.

Esta implementación no valida que el servidor haya emitido realmente el identificador de sesión del cliente. No lo use como forma de autenticación. El punto del ejemplo es mostrar la administración de cookies HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Un controlador puede obtener el identificador de sesión del contenedor de propiedades **HttpRequestMessage. Properties** .

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
