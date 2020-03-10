---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Inicio de sesión con sitios externos en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Google, Twitter, Yahoo y otros sitios, es decir, Cómo admitir...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518791"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Inicio de sesión con sitios externos en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo iniciar sesión en el sitio de ASP.NET Web Pages (Razor) con Facebook, Google, Twitter, Yahoo y otros sitios, es decir, Cómo admitir OAuth y OpenID en su sitio.
> 
> Temas que se abordarán:
> 
> - Habilitación del inicio de sesión desde otros sitios cuando se usa la plantilla del sitio de inicio de WebMatrix.
> 
> Esta es la característica ASP.NET presentada en el artículo:
> 
> - Aplicación auxiliar de `OAuthWebSecurity`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web Pages incluye compatibilidad con proveedores de [OAuth](http://oauth.net/) y [OpenID](http://openid.net/) . Con estos proveedores, puede permitir que los usuarios inicien sesión en su sitio con sus credenciales existentes de Facebook, Twitter, Microsoft y Google. Por ejemplo, para iniciar sesión con una cuenta de Facebook, los usuarios solo pueden elegir un icono de Facebook, que los redirige a la página de inicio de sesión de Facebook donde escriben su información de usuario. Después, pueden asociar el inicio de sesión de Facebook a su cuenta en el sitio. Una mejora relacionada con las características de pertenencia de las páginas web es que los usuarios pueden asociar varios inicios de sesión (incluidos los inicios de sesión de sitios de redes sociales) con una sola cuenta en el sitio Web.

En esta imagen se muestra la página de inicio de sesión de la plantilla del **sitio de inicio** , donde un usuario puede elegir un icono de Facebook, Twitter, Google o Microsoft para habilitar el inicio de sesión con una cuenta externa:

![proveedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Puede habilitar la pertenencia a OAuth y OpenID si quita las marcas de comentario de algunas líneas de código en la plantilla del **sitio de inicio** . Los métodos y las propiedades que se usan para trabajar con los proveedores OAuth y OpenID se encuentran en la clase `WebMatrix.Security.OAuthWebSecurity`. La plantilla del **sitio de inicio** incluye una infraestructura de pertenencia completa, completa con una página de inicio de sesión, una base de datos de pertenencia y todo el código que necesita para que los usuarios inicien sesión en su sitio con credenciales locales o con las de otro sitio.

En esta sección se proporciona un ejemplo de cómo permitir a los usuarios iniciar sesión desde sitios externos a un sitio basado en la plantilla del **sitio de inicio** . Después de crear un sitio de inicio, haga esto (los detalles siguen):

- En el caso de los sitios que usan un proveedor de OAuth (Facebook, Twitter y Microsoft), se crea una aplicación en el sitio externo. Esto le proporciona las claves de aplicación que necesitará para invocar la característica de inicio de sesión para esos sitios.
- En el caso de sitios que usan un proveedor de OpenID (Google), no es necesario crear una aplicación. Para todos estos sitios, debe tener una cuenta para iniciar sesión y crear aplicaciones para desarrolladores.

    > [!NOTE]
    > Las aplicaciones de Microsoft solo aceptan una dirección URL activa para un sitio web de trabajo, por lo que no puede usar una dirección URL de sitio Web local para probar los inicios de sesión.
- Edite algunos archivos en el sitio web con el fin de especificar el proveedor de autenticación adecuado y enviar un inicio de sesión al sitio que desea usar.

En este artículo se proporcionan instrucciones independientes para las tareas siguientes:

- [Habilitación de inicios de sesión de Google](#To_enable_Google_logins)
- [Habilitación de inicios de sesión de Facebook](#To_enable_Facebook_logins)
- [Habilitación de inicios de sesión de Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitación de inicios de sesión de Google

1. Cree o abra un sitio ASP.NET Web Pages basado en la plantilla del sitio de inicio de WebMatrix.
2. Abra la página *\_AppStart. cshtml* y quite la marca de comentario de la siguiente línea de código. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Probar el inicio de sesión de Google

1. Ejecute la página *default. cshtml* del sitio y elija el botón **iniciar sesión** .
2. En la página de *Inicio de sesión* , en la sección **usar otro servicio para iniciar sesión** , elija el botón Enviar de **Google** o **Yahoo** . En este ejemplo se usa el inicio de sesión de Google. 

    La página web redirige la solicitud a la página de inicio de sesión de Google.

    ![Inicio de sesión en Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Escriba las credenciales de una cuenta de Google existente.
4. Si Google le pregunta si desea permitir que *localhost* use información de la cuenta, haga clic en **permitir**.

    El código usa el token de Google para autenticar al usuario y, después, vuelve a esta página en el sitio Web. Esta página permite a los usuarios asociar el inicio de sesión de Google con una cuenta existente en el sitio web o pueden registrar una cuenta nueva en el sitio para asociar el inicio de sesión externo con.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Elija el botón **asociar** . El explorador vuelve a la Página principal de la aplicación.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitación de inicios de sesión de Facebook

1. Vaya al [sitio de desarrolladores de Facebook](https://developers.facebook.com/apps) (inicie sesión si aún no ha iniciado sesión).
2. Elija el botón **crear nueva aplicación** y, a continuación, siga las indicaciones para asignar un nombre y crear la nueva aplicación.
3. En la sección **seleccionar cómo se integrará la aplicación con Facebook**, elija la sección **sitio web** .
4. Rellene el campo **dirección URL del sitio** con la dirección URL del sitio (por ejemplo, `http://www.example.com`). El campo **dominio** es opcional. puede usarlo para proporcionar autenticación para un dominio completo (por ejemplo, *example.com*). 

    > [!NOTE]
    > Si está ejecutando un sitio en el equipo local con una dirección URL como `http://localhost:12345` (donde el número es un número de puerto local), puede Agregar este valor al campo **dirección URL del sitio** para probar el sitio. Sin embargo, cada vez que cambie el número de puerto del sitio local, tendrá que actualizar el campo de **dirección URL del sitio** de la aplicación.
5. Elija el botón **Guardar cambios** .
6. Vuelva a elegir la pestaña **aplicaciones** y, a continuación, vea la página de inicio de la aplicación.
7. Copie los valores de ID. de **aplicación** y **secreto de aplicación** de la aplicación y péguelos en un archivo de texto temporal. Pasará estos valores al proveedor de Facebook en el código del sitio Web.
8. Salga del sitio para desarrolladores de Facebook.

Ahora puede realizar cambios en dos páginas del sitio web para que los usuarios puedan iniciar sesión en el sitio con sus cuentas de Facebook.

1. Cree o abra un sitio ASP.NET Web Pages basado en la plantilla del sitio de inicio de WebMatrix.
2. Abra la página *\_AppStart. cshtml* y quite el comentario del código del proveedor de OAuth de Facebook. El bloque de código sin comentarios tiene un aspecto similar al siguiente: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copie el valor de ID. de **aplicación** de la aplicación de Facebook como el valor del parámetro `appId` (dentro de las comillas).
4. Copie el valor del **secreto** de la aplicación de la aplicación de Facebook como el valor del parámetro `appSecret`.
5. Guarde y cierre el archivo.

### <a name="testing-facebook-login"></a>Prueba del inicio de sesión de Facebook

1. Ejecute la página *default. cshtml* del sitio y elija el botón de **Inicio de sesión** .
2. En la página de *Inicio de sesión* , en la sección **usar otro servicio para iniciar sesión** , elija el icono de **Facebook** . 

    La página web redirige la solicitud a la página de inicio de sesión de Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Inicie sesión en una cuenta de Facebook. 

    El código usa el token de Facebook para autenticarse y, a continuación, vuelve a una página donde puede asociar el inicio de sesión de Facebook al inicio de sesión de su sitio. El nombre de usuario o la dirección de correo electrónico se rellenan en el campo **correo electrónico** del formulario.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Elija el botón **asociar** . 

    El explorador vuelve a la Página principal y ha iniciado sesión.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitación de inicios de sesión de Twitter

1. Vaya al [sitio de desarrolladores de Twitter](https://dev.twitter.com/).
2. Elija el vínculo **crear una aplicación** y, a continuación, inicie sesión en el sitio.
3. En el formulario **crear una aplicación** , rellene los campos **nombre** y **Descripción** .
4. En el campo **sitio web** , escriba la dirección URL del sitio (por ejemplo, `http://www.example.com`). 

    > [!NOTE]
    > Si va a probar el sitio localmente (mediante una dirección URL como `http://localhost:12345`), es posible que Twitter no acepte la dirección URL. Sin embargo, es posible que pueda usar la dirección IP de bucle invertido local (por ejemplo `http://127.0.0.1:12345`). Esto simplifica el proceso de prueba de la aplicación localmente. Sin embargo, cada vez que cambie el número de puerto del sitio local, deberá actualizar el campo **sitio web** de la aplicación.
5. En el campo **dirección URL de devolución de llamada** , escriba una dirección URL para la página en el sitio web a la que desea que los usuarios vuelvan después de iniciar sesión en Twitter. Por ejemplo, para enviar usuarios a la Página principal del sitio de inicio (que reconocerá su estado de inicio de sesión), escriba la misma dirección URL que especificó en el campo **sitio web** .
6. Acepte los términos y elija el botón **crear la aplicación de Twitter** .
7. En la página de aterrizaje **mis aplicaciones** , elija la aplicación que ha creado.
8. En la pestaña **detalles** , desplácese hasta la parte inferior y elija el botón **crear mi token de acceso** .
9. En la pestaña **detalles** , copie los valores de **clave de consumidor** y secreto de **consumidor** de la aplicación y péguelos en un archivo de texto temporal. Pasará estos valores al proveedor de Twitter en el código del sitio Web.
10. Salga del sitio de Twitter.

Ahora puede realizar cambios en dos páginas del sitio web para que los usuarios puedan iniciar sesión en el sitio con sus cuentas de Twitter.

1. Cree o abra un sitio ASP.NET Web Pages basado en la plantilla del sitio de inicio de WebMatrix.
2. Abra la página *\_AppStart. cshtml* y quite el comentario del código del proveedor de OAuth de Twitter. El bloque de código sin comentarios tiene el siguiente aspecto: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copie el valor de **clave de consumidor** de la aplicación de Twitter como el valor del parámetro `consumerKey` (dentro de las comillas).
4. Copie el valor de **secreto de consumidor** de la aplicación de Twitter como el valor del parámetro `consumerSecret`.
5. Guarde y cierre el archivo.

### <a name="testing-twitter-login"></a>Prueba del inicio de sesión de Twitter

1. Ejecute la página *default. cshtml* del sitio y elija el botón de **Inicio de sesión** .
2. En la página de *Inicio de sesión* , en la sección **usar otro servicio para iniciar sesión** , elija el icono de **Twitter** . 

    La página web redirige la solicitud a una página de inicio de sesión de Twitter para la aplicación que ha creado.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Inicie sesión en una cuenta de Twitter.
4. El código usa el token de Twitter para autenticar al usuario y después le devuelve a una página donde puede asociar el inicio de sesión a su cuenta de sitio Web. Su nombre o dirección de correo electrónico se rellena en el campo **correo electrónico** del formulario.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Elija el botón **asociar** . 

    El explorador vuelve a la Página principal y ha iniciado sesión.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Personalizar el comportamiento de todo el sitio](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Agregar seguridad y pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202904)
