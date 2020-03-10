---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Uso de un CAPTCHA para impedir que los bots usen el sitio de ASP.NET Web Razor) | Microsoft Docs
author: microsoft
description: En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realicen tareas en una ASP.NET Web Pages (Razor)...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2647a3155893a3dfb3214795a5f9cf1e8931fa91
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440197"
---
# <a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Uso de un CAPTCHA para impedir que los bots usen el sitio de ASP.NET Web Razor)

por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar ReCaptcha (una medida de seguridad) para evitar que programas automatizados (bots) realicen tareas en un sitio web de ASP.NET Web Pages (Razor).
> 
> **Lo que aprenderá:** 
> 
> - Cómo agregar una prueba de CAPTCHA a su sitio.
> 
> Estas son las características de ASP.NET presentadas en el artículo:
> 
> - Aplicación auxiliar de `ReCaptcha`.
> 
> > [!NOTE]
> > La información de este artículo se aplica a ASP.NET Web Pages 1,0 y páginas web 2.

## <a name="about-captchas"></a>Acerca de CAPTCHAs

Cada vez que permita que los usuarios se registren en su sitio, o que solo escriban un nombre y una dirección URL (por ejemplo, para un Comentario de blog), podría obtener una avalancha de nombres falsos. A menudo se les dejan programas automatizados (bots) que intentan dejar las direcciones URL en todos los sitios web que pueden encontrar. (Una motivación común es publicar las direcciones URL de los productos para su venta).

Puede ayudar a asegurarse de que un usuario es una persona real y no un programa informático mediante el uso de un *CAPTCHA* para validar a los usuarios cuando se registran o escriben su nombre y sitio. CAPTCHA es una prueba de convertir pública completamente automatizada para indicar a los equipos y a los seres humanos que están separados. Un CAPTCHA es una prueba de *desafío-respuesta* en la que se pide al usuario que haga algo que sea fácil de realizar para una persona, pero que sea difícil de hacer con un programa automatizado. El tipo más común de CAPTCHA es aquel en el que se ven algunas letras distorsionadas y se les pide que las escriba. (Se supone que la distorsión dificulta que los bots descifran las letras).

## <a name="adding-a-recaptcha-test"></a>Agregar una prueba de ReCaptcha

En las páginas de ASP.NET, puede usar la aplicación auxiliar de `ReCaptcha` para representar una prueba de CAPTCHA basada en el servicio ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). La aplicación auxiliar de `ReCaptcha` muestra una imagen de dos palabras distorsionadas que los usuarios tienen que escribir correctamente antes de que se valide la página. El servicio ReCaptcha.Net valida la respuesta del usuario.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registre el sitio web en ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Cuando haya completado el registro, obtendrá una clave pública y una clave privada.
2. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
3. Si aún no tiene un archivo *\_AppStart. cshtml* , en la carpeta raíz de un sitio web, cree un archivo denominado *\_AppStart. cshtml*.
4. Agregue la siguiente configuración auxiliar de `Recaptcha` en el archivo *\_AppStart. cshtml* : 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Establezca las propiedades `PublicKey` y `PrivateKey` con sus propias claves públicas y privadas.
6. Guarde el archivo *\_AppStart. cshtml* y ciérrelo.
7. En la carpeta raíz de un sitio web, cree una nueva página denominada *reCAPTCHA. cshtml*.
8. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Ejecute la página *reCAPTCHA. cshtml* en un explorador. Si el valor de `PrivateKey` es válido, la página muestra el control ReCaptcha y un botón. Si no ha establecido las claves globalmente en *\_AppStart. html*, la página mostrará un error. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Escriba las palabras de la prueba. Si pasa la prueba de ReCaptcha, verá un mensaje para ese efecto. De lo contrario, verá un mensaje de error y se vuelve a mostrar el control ReCaptcha.

> [!NOTE]
> Si el equipo se encuentra en un dominio que utiliza el servidor proxy, es posible que deba configurar el elemento `defaultproxy` del archivo *Web. config* . En el ejemplo siguiente se muestra un archivo *Web. config* con el elemento `defaultproxy` configurado para permitir que el servicio ReCaptcha funcione.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Personalizar el comportamiento de todo el sitio para sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sitio de ReCaptcha](https://www.google.com/recaptcha)
