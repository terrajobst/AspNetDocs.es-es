---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Prevención de ataques de falsificación de solicitud entre sitios (CSRF) en ASP.NET MVC
author: MikeWasson
description: Describe el ataque de falsificación de solicitud entre sitios (CSRF) y cómo implementar las medidas anti-CSRF en ASP.NET Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447115"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Prevención de ataques de falsificación de solicitud entre sitios (CSRF) en la aplicación ASP.NET MVC

por [Mike Wasson](https://github.com/MikeWasson)

La falsificación de solicitudes entre sitios (CSRF) es un ataque en el que un sitio malintencionado envía una solicitud a un sitio vulnerable en el que el usuario ha iniciado sesión actualmente.

A continuación se muestra un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en `www.example.com` mediante la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio Web malintencionado. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Tenga en cuenta que la acción del formulario se publica en el sitio vulnerable, no en el sitio malintencionado. Esta es la parte "entre sitios" de CSRF.
4. El usuario hace clic en el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor con el contexto de autenticación del usuario y puede hacer todo lo que se permite a un usuario autenticado.

Aunque en este ejemplo se requiere que el usuario haga clic en el botón de formulario, la página malintencionada podría ejecutar fácilmente un script que envíe el formulario automáticamente. Además, el uso de SSL no evita un ataque CSRF, ya que el sitio malintencionado puede enviar una solicitud "https://".

Normalmente, los ataques CSRF son posibles en sitios web que usan cookies para la autenticación, ya que los exploradores envían todas las cookies pertinentes al sitio web de destino. Sin embargo, los ataques CSRF no se limitan a aprovechar las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Después de que un usuario inicia sesión con la autenticación básica o implícita. el explorador enviará automáticamente las credenciales hasta que finalice la sesión.

## <a name="anti-forgery-tokens"></a>Tokens antifalsificación

Para ayudar a evitar ataques CSRF, ASP.NET MVC usa tokens antifalsificación, también denominados *tokens de comprobación de solicitudes*.

1. El cliente solicita una página HTML que contiene un formulario.
2. El servidor incluye dos tokens en la respuesta. Un token se envía como una cookie. El otro se coloca en un campo de formulario oculto. Los tokens se generan de forma aleatoria para que un adversario no pueda adivinar los valores.
3. Cuando el cliente envía el formulario, debe enviar ambos tokens de vuelta al servidor. El cliente envía el token de la cookie como una cookie y envía el token del formulario dentro de los datos del formulario. (Un cliente del explorador realiza automáticamente esto cuando el usuario envía el formulario).
4. Si una solicitud no incluye ambos tokens, el servidor no permite la solicitud.

Este es un ejemplo de un formulario HTML con un token de formulario oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Los tokens antifalsificación funcionan porque la página malintencionada no puede leer los tokens del usuario debido a las directivas del mismo origen. ([Las directivas del mismo origen](http://www.w3.org/Security/wiki/Same_Origin_Policy) impiden que los documentos hospedados en dos sitios diferentes tengan acceso al contenido de los demás. Por lo tanto, en el ejemplo anterior, la página malintencionada puede enviar solicitudes a example.com, pero no puede leer la respuesta).

Para evitar ataques CSRF, utilice tokens antifalsificación con cualquier protocolo de autenticación en el que el explorador envíe las credenciales de forma silenciosa una vez que el usuario inicie sesión. Esto incluye protocolos de autenticación basados en cookies, como la autenticación de formularios, así como protocolos como la autenticación básica e implícita.

Debe requerir tokens antifalsificación para cualquier método no seguro (POST, PUT, DELETE). Además, asegúrese de que los métodos seguros (GET, HEAD) no tienen efectos secundarios. Además, si habilita la compatibilidad entre dominios, como CORS o JSONP, incluso los métodos seguros como GET son potencialmente vulnerables a los ataques CSRF, lo que permite al atacante leer datos potencialmente confidenciales.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens antifalsificación en ASP.NET MVC

Para agregar los tokens antifalsificación a una página de Razor, use el método auxiliar **HtmlHelper. AntiForgeryToken** :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Este método agrega el campo de formulario oculto y también establece el token de cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF y AJAX

el token de formulario puede ser un problema para las solicitudes AJAX, porque estas podrían enviar datos JSON en lugar de datos de formulario HTML. Una solución consiste en enviar los tokens en un encabezado HTTP personalizado. En el código siguiente, se utiliza sintaxis de Razor para generar los tokens, que después se agregan a una solicitud AJAX. Los tokens se generan en el servidor mediante una llamada a **Antifalsificate. GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Cuando procese la solicitud, extraiga los tokens del encabezado de la solicitud. A continuación, llame al método **Antifalsification. Validate** para validar los tokens. El método **Validate** produce una excepción si los tokens no son válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
