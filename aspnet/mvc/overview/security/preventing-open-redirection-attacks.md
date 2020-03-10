---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Prevención de ataques de redireccionamiento abiertos (C#) | Microsoft Docs
author: jongalloway
description: En este tutorial se explica cómo puede evitar ataques de redireccionamiento abiertos en sus aplicaciones ASP.NET MVC. En este tutorial se describen los cambios que se han realizado...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: cfa635d4fd14d031993c5b452325cbe334f82dc2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432589"
---
# <a name="preventing-open-redirection-attacks-c"></a>Prevenir los ataques de redireccionamiento abierto (C#)

por [Jon Galloway](https://github.com/jongalloway)

> En este tutorial se explica cómo puede evitar ataques de redireccionamiento abiertos en sus aplicaciones ASP.NET MVC. En este tutorial se describen los cambios que se han realizado en AccountController en ASP.NET MVC 3 y se muestra cómo se pueden aplicar estos cambios en las aplicaciones existentes de ASP.NET MVC 1,0 y 2.

## <a name="what-is-an-open-redirection-attack"></a>¿Qué es un ataque de redireccionamiento abierto?

Cualquier aplicación web que redirija a una dirección URL que se especifica a través de la solicitud, como la cadena de consulta o los datos del formulario, se pueden alterar para redirigir a los usuarios a una dirección URL externa y malintencionada. Esta manipulación se denomina ataque de redireccionamiento abierto.

Siempre que la lógica de la aplicación redirija a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha alterado. El inicio de sesión usado en el AccountController predeterminado para ASP.NET MVC 1,0 y ASP.NET MVC 2 es vulnerable a ataques de redireccionamiento abiertos. Afortunadamente, es fácil actualizar las aplicaciones existentes para usar las correcciones de ASP.NET MVC 3 Preview.

Para comprender la vulnerabilidad, echemos un vistazo a cómo funciona la redirección de inicio de sesión en un proyecto de aplicación Web ASP.NET MVC 2 predeterminado. En esta aplicación, si intenta visitar una acción de controlador que tenga el atributo [Authorize], se redirigirá a los usuarios no autorizados a la vista/Account/LogOn. Esta redirección a/Account/LogOn incluirá un parámetro de cadena de error returnUrl para que el usuario pueda volver a la dirección URL solicitada originalmente después de haber iniciado sesión correctamente.

En la captura de pantalla siguiente, podemos ver que un intento de obtener acceso a la vista de/Account/ChangePassword cuando no se ha iniciado sesión produce un redireccionamiento a/Account/LogOn? ReturnUrl =% 2fAccount% 2fChangePassword% 2F.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Figura 01**: Página de inicio de sesión con un redireccionamiento abierto

Puesto que no se valida el parámetro de cadena de QueryString ReturnUrl, un atacante puede modificarlo para insertar cualquier dirección URL en el parámetro para realizar un ataque de redirección abierto. Para demostrar esto, podemos modificar el parámetro ReturnUrl a [http://bing.com](http://bing.com), por lo que la dirección URL de inicio de sesión resultante será/Account/Logon? ReturnUrl =<http://www.bing.com/>. Tras iniciar sesión correctamente en el sitio, se le redirigirá a [http://bing.com](http://bing.com). Puesto que no se valida esta redirección, podría apuntar a un sitio malintencionado que intenta engañar al usuario.

### <a name="a-more-complex-open-redirection-attack"></a>Un ataque de redireccionamiento abierto más complejo

Los ataques de redireccionamiento abiertos son especialmente peligrosos porque un atacante sabe que estamos intentando iniciar sesión en un sitio web específico, lo que nos hace más vulnerable a un [ataque de suplantación de identidad](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Por ejemplo, un atacante podría enviar correos electrónicos malintencionados a los usuarios del sitio web en un intento de capturar sus contraseñas. Veamos cómo funcionaría en el sitio de NerdDinner. (Tenga en cuenta que el sitio Live NerdDinner se ha actualizado para protegerse frente a ataques de redireccionamiento abiertos).

En primer lugar, un atacante nos envía un vínculo a la página de inicio de sesión en NerdDinner que incluye una redirección a su página falsificada:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Tenga en cuenta que la dirección URL de retorno apunta a nerddiner.com, que falta una "n" de la palabra cena. En este ejemplo, se trata de un dominio que controla el atacante. Al acceder al vínculo anterior, se le dirigirá a la página de inicio de sesión de NerdDinner.com legítima.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Figura 02**: Página de inicio de sesión de NerdDinner con un redireccionamiento abierto

Cuando se inicia sesión correctamente, la acción de inicio de sesión de ASP.NET MVC AccountController redirige a la dirección URL especificada en el parámetro de cadena de QueryString returnUrl. En este caso, es la dirección URL que ha escrito el atacante, que es [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). A menos que estamos muy atentos, es muy probable que no observemos esto, especialmente porque el atacante se ha asegurado de que su página falsificada tiene el mismo aspecto que la página de inicio de sesión legítima. Esta página de inicio de sesión incluye un mensaje de error que le solicita que iniciemos sesión de nuevo. En lo que nos sentimos, debemos no escribir la contraseña.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Figura 03**: pantalla de inicio de sesión de NerdDinner falsificado

Cuando se vuelva a escribir el nombre de usuario y la contraseña, la página de inicio de sesión falsificada guardará la información y nos enviará de nuevo al sitio legítimo de NerdDinner.com. En este momento, el sitio de NerdDinner.com ya se autenticó, por lo que la página de inicio de sesión falsificada puede redirigirse directamente a esa página. El resultado final es que el atacante tiene el nombre de usuario y la contraseña, y no es consciente de que se les ha proporcionado.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Examinar el código vulnerable en la acción de inicio de sesión de AccountController

A continuación se muestra el código para la acción de inicio de sesión en una aplicación ASP.NET MVC 2. Tenga en cuenta que, tras un inicio de sesión correcto, el controlador devuelve una redirección a la returnUrl. Puede ver que no se realiza ninguna validación en el parámetro returnUrl.

**Lista 1: acción de inicio de sesión de ASP.NET MVC 2 en `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Ahora, echemos un vistazo a los cambios en la acción de inicio de sesión de ASP.NET MVC 3. Este código se ha cambiado para validar el parámetro returnUrl mediante una llamada a un nuevo método en la clase auxiliar System. Web. Mvc. URL denominada `IsLocalUrl()`.

**Lista 2: acción de inicio de sesión de ASP.NET MVC 3 en `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Esto se ha cambiado para validar el parámetro de la dirección URL de retorno llamando a un nuevo método en la clase auxiliar System. Web. Mvc. URL, `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Protección de las aplicaciones ASP.NET MVC 1,0 y MVC 2

Podemos aprovechar las ventajas de los cambios de ASP.NET MVC 3 en nuestras aplicaciones ASP.NET MVC 1,0 y 2 existentes agregando el método auxiliar IsLocalUrl () y actualizando la acción LogOn para validar el parámetro returnUrl.

El método UrlHelper IsLocalUrl () en realidad llama a un método en System. Web. Webpages, ya que la validación también la usan las aplicaciones ASP.NET Web Pages.

**Lista 3: el método IsLocalUrl () de ASP.NET MVC 3 UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

El método IsUrlLocalToHost contiene la lógica de validación real, tal como se muestra en la lista 4.

**Lista 4: método IsUrlLocalToHost () de la clase System. Web. Webpages RequestExtensions**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

En nuestra aplicación ASP.NET MVC 1,0 o 2, agregaremos un método IsLocalUrl () a AccountController, pero le recomendamos que lo agregue a una clase auxiliar independiente si es posible. Realizaremos dos pequeños cambios en la versión ASP.NET MVC 3 de IsLocalUrl () para que funcione dentro de AccountController. En primer lugar, cambiaremos de un método público a un método privado, ya que se puede tener acceso a los métodos públicos de los controladores como acciones de controlador. En segundo lugar, modificaremos la llamada que comprueba el host de la dirección URL en el host de la aplicación. Esa llamada hace uso de un campo RequestContext local en la clase UrlHelper. En lugar de utilizar este. RequestContext. HttpContext. request. URL. host, lo usaremos. Request. URL. host. En el código siguiente se muestra el método IsLocalUrl () modificado para su uso con una clase de controlador en aplicaciones ASP.NET MVC 1,0 y 2.

**Enumeración 5: método IsLocalUrl (), que se modifica para su uso con una clase de controlador de MVC**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Ahora que el método IsLocalUrl () está en su lugar, podemos llamarlo desde nuestra acción de inicio de sesión para validar el parámetro returnUrl, tal y como se muestra en el código siguiente.

**Listado 6: método de inicio de sesión actualizado que valida el parámetro returnUrl**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Ahora podemos probar un ataque de redireccionamiento abierto intentando iniciar sesión con una dirección URL de retorno externa. Vamos a usar/Account/LogOn? ReturnUrl =<http://www.bing.com/> de nuevo.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Figura 04**: prueba de la acción de inicio de sesión actualizada

Después de iniciar sesión correctamente, se le redirigirá a la acción del controlador de índice o de inicio en lugar de la dirección URL externa.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Figura 05**: ataque de redireccionamiento abierto derrotado

## <a name="summary"></a>Resumen

Pueden producirse ataques de redireccionamiento abiertos cuando las direcciones URL de redireccionamiento se pasan como parámetros en la dirección URL de una aplicación. La plantilla ASP.NET MVC 3 incluye código para protegerse frente a ataques de redireccionamiento abiertos. Puede Agregar este código con alguna modificación a las aplicaciones ASP.NET MVC 1,0 y 2. Para protegerse frente a ataques de redireccionamiento abiertos al iniciar sesión en aplicaciones ASP.NET 1,0 y 2, agregue un método IsLocalUrl () y valide el parámetro returnUrl en la acción LogOn.
