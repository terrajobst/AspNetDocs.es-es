---
uid: web-api/overview/security/forms-authentication
title: Autenticación de formularios en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Describe el uso de la autenticación de formularios en ASP.NET Web API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484381"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Autenticación mediante formularios en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

La autenticación mediante formularios usa un formulario HTML para enviar las credenciales del usuario al servidor. No es un estándar de Internet. La autenticación de formularios solo es adecuada para las API Web a las que se llama desde una aplicación Web, de modo que el usuario pueda interactuar con el formulario HTML.

| Ventajas | Desventajas |
| --- | --- |
| -Fácil de implementar: integrado en ASP.NET. -Usa el proveedor de pertenencia a ASP.NET, que facilita la administración de cuentas de usuario. | -No es un mecanismo de autenticación HTTP estándar; utiliza cookies HTTP en lugar del encabezado de autorización estándar. -Requiere un cliente de explorador. -Las credenciales se envían como texto sin formato. -Es vulnerable a la falsificación de solicitudes entre sitios (CSRF); requiere medidas anti-CSRF. -Difícil de usar desde clientes que no son de explorador. Login requiere un explorador. -Las credenciales de usuario se envían en la solicitud. -Algunos usuarios deshabilitan las cookies. |

En Resumen, la autenticación de formularios en ASP.NET funciona de la siguiente manera:

1. El cliente solicita un recurso que requiere autenticación.
2. Si el usuario no está autenticado, el servidor devuelve HTTP 302 (encontrado) y redirige a una página de inicio de sesión.
3. El usuario escribe las credenciales y envía el formulario.
4. El servidor devuelve otro HTTP 302 que redirige de nuevo al URI original. Esta respuesta incluye una cookie de autenticación.
5. El cliente vuelve a solicitar el recurso. La solicitud incluye la cookie de autenticación, por lo que el servidor concede la solicitud.

![](forms-authentication/_static/image1.png)

Para obtener más información, vea [información general sobre la autenticación de formularios.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Uso de la autenticación de formularios con Web API

Para crear una aplicación que use la autenticación de formularios, seleccione la plantilla "aplicación de Internet" en el Asistente para proyectos de MVC 4. Esta plantilla crea controladores MVC para la administración de cuentas. También puede usar la plantilla "aplicación de una sola página", disponible en la actualización ASP.NET 2012.

En los controladores de la API Web, puede restringir el acceso mediante el `[Authorize]` atributo, como se describe en [usar el atributo [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

La autenticación de formularios usa una cookie de sesión para autenticar las solicitudes. Los exploradores envían automáticamente todas las cookies pertinentes al sitio web de destino. Esta característica hace que la autenticación de formularios sea vulnerable a ataques de falsificación de solicitud entre sitios (CSRF). consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

La autenticación de formularios no cifra las credenciales del usuario. Por lo tanto, la autenticación de formularios no es segura a menos que se use con SSL. Consulte [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).
