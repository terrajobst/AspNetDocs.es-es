---
title: Ejemplo de cookie de SameSite para C# ASP.net 4.7.2 MVC
author: blowdart
description: Ejemplo de cookie de SameSite para C# ASP.net 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438205"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a>Ejemplo de cookie de SameSite para C# ASP.net 4.7.2 MVC

.NET Framework 4,7 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se ajusta al estándar original.
El comportamiento revisado cambió el significado de `SameSite.None` para emitir el atributo con un valor de `None`, en lugar de no emitir el valor. Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.

## <a name="sampleCode"></a>Escribir el atributo SameSite

A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie;

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

El atributo sameSite predeterminado para el estado de sesión se establece en el parámetro ' cookieSameSite ' de la configuración de sesión en `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Autenticación MVC

La autenticación basada en cookies de ASP.net OWIN usa un administrador de cookies para habilitar el cambio de atributos de cookies. [SameSiteCookieManager.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) es una implementación de este tipo de clase que se puede copiar en sus propios proyectos. 

Debe asegurarse de que los componentes de Microsoft. Owin se han actualizado a la versión 4.1.0 o posterior. Compruebe el archivo de `packages.config` para asegurarse de que todos los números de versión coinciden, por ejemplo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

Los componentes de autenticación se deben configurar para usar CookieManager en la clase startup.

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

Se debe establecer un administrador de cookies en *cada* componente que lo admita, esto incluye CookieAuthentication y OpenIdConnectAuthentication.

SystemWebCookieManager se utiliza para evitar [problemas conocidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) con la integración de cookies de respuesta.

### <a name="running-the-sample"></a>Ejecutar el ejemplo

Si ejecuta el proyecto de ejemplo, cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio.
Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.

![Lista de cookies del depurador del explorador](sample/img/BrowserDebugger.png)

Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookies" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).

## <a name="interception"></a>Interceptar cookies que no controla

.NET 4.5.2 presentó un nuevo evento para interceptar la escritura de encabezados `Response.AddOnSendingHeaders`. Se puede usar para interceptar cookies antes de que se devuelvan al equipo cliente. En el ejemplo se conecta el evento a un método estático que comprueba si el explorador admite los nuevos cambios de sameSite y, si no es así, cambia las cookies para que no emita el atributo si se ha establecido el nuevo valor de `None`.

Vea [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) para obtener un ejemplo de cómo enlazar el evento y [SameSiteCookieRewriter.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) para obtener un ejemplo de cómo controlar el evento y ajustar la cookie `sameSite` atributo que se puede copiar en su propio código.

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

Puede cambiar el comportamiento específico de cookies con nombre de forma muy similar; en el ejemplo siguiente se ajusta la cookie de autenticación predeterminada de `Lax` a `None` en los exploradores que admiten el valor de `None` o se quita el atributo sameSite en los exploradores que no admiten `None`.

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a>Más información
 
[Actualizaciones de Chrome](https://www.chromium.org/updates/same-site)

[Documentación de OWIN SameSite](/aspnet/samesite/owin-samesite)

[Documentación de ASP.NET](/aspnet/samesite/system-web-samesite)

[Revisiones de .NET SameSite](/aspnet/samesite/kbs-samesite)