---
uid: web-api/overview/security/basic-authentication
title: Autenticación básica en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Describe el uso de la autenticación básica en ASP.NET Web API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447637"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Autenticación básica en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

La autenticación básica se define en [RFC 2617, autenticación http: autenticación básica y de acceso implícita](http://www.ietf.org/rfc/rfc2617.txt).

Desventajas

- Las credenciales de usuario se envían en la solicitud.
- Las credenciales se envían como texto sin formato.
- Las credenciales se envían con cada solicitud.
- No es posible cerrar sesión, excepto al finalizar la sesión del explorador.
- Vulnerable a la falsificación de solicitudes entre sitios (CSRF); requiere medidas anti-CSRF.

Ventajas

- Estándar de Internet.
- Compatible con todos los exploradores principales.
- Protocolo relativamente simple.

La autenticación básica funciona de la siguiente manera:

1. Si una solicitud requiere autenticación, el servidor devuelve 401 (no autorizado). La respuesta incluye un encabezado WWW-Authenticate, que indica que el servidor admite la autenticación básica.
2. El cliente envía otra solicitud, con las credenciales del cliente en el encabezado de autorización. Las credenciales tienen el formato de cadena "nombre: contraseña", codificada en Base64. Las credenciales no se cifran.

La autenticación básica se realiza en el contexto de un "dominio". El servidor incluye el nombre del dominio Kerberos en el encabezado WWW-Authenticate. Las credenciales del usuario son válidas dentro de ese territorio. El servidor define el ámbito exacto de un dominio Kerberos. Por ejemplo, puede definir varios dominios para particionar los recursos.

![](basic-authentication/_static/image1.png)

Dado que las credenciales se envían sin cifrar, la autenticación básica solo es segura a través de HTTPS. Consulte [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).

La autenticación básica también es vulnerable a los ataques CSRF. Después de que el usuario escriba las credenciales, el explorador las enviará automáticamente en las solicitudes posteriores al mismo dominio, mientras dure la sesión. Esto incluye las solicitudes AJAX. Consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticación básica con IIS

IIS admite la autenticación básica, pero hay una advertencia: el usuario se autentica con sus credenciales de Windows. Esto significa que el usuario debe tener una cuenta en el dominio del servidor. Para un sitio web de acceso público, normalmente desea autenticarse en un proveedor de pertenencia a ASP.NET.

Para habilitar la autenticación básica mediante IIS, establezca el modo de autenticación en "Windows" en el archivo Web. config del proyecto ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

En este modo, IIS usa las credenciales de Windows para la autenticación. Además, debe habilitar la autenticación básica en IIS. En el administrador de IIS, vaya a la vista características, seleccione autenticación y habilite la autenticación básica.

![](basic-authentication/_static/image2.png)

En el proyecto de API Web, agregue el atributo `[Authorize]` para las acciones de controlador que necesiten autenticación.

Un cliente se autentica a sí mismo estableciendo el encabezado de autorización en la solicitud. Los clientes del explorador realizan este paso automáticamente. Los clientes que no son de explorador deberán establecer el encabezado.

## <a name="basic-authentication-with-custom-membership"></a>Autenticación básica con pertenencia personalizada

Como se mencionó, la autenticación básica integrada en IIS utiliza las credenciales de Windows. Esto significa que necesita crear cuentas para los usuarios en el servidor de hospedaje. Pero para una aplicación de Internet, las cuentas de usuario suelen almacenarse en una base de datos externa.

En el código siguiente se describe cómo un módulo HTTP que realiza la autenticación básica. Puede conectar fácilmente un proveedor de pertenencia a ASP.NET reemplazando el método `CheckPassword`, que es un método ficticio en este ejemplo.

En Web API 2, debería considerar la posibilidad de escribir un [filtro de autenticación](authentication-filters.md) o [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), en lugar de un módulo http.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar el módulo HTTP, agregue lo siguiente al archivo Web. config en la sección **System. WebServer** :

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Reemplace "YourAssemblyName" por el nombre del ensamblado (sin incluir la extensión "dll").

Debe deshabilitar otros esquemas de autenticación, como formularios o la autenticación de Windows.
