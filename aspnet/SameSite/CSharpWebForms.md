---
title: Ejemplo de cookies de SameSite C# para WebForms de 4.7.2 de ASP.net
author: blowdart
description: Ejemplo de cookies de SameSite C# para WebForms de 4.7.2 de ASP.net
ms.author: riande
ms.date: 2/15/2019
uid: samesite/CSharpWebForms
ms.openlocfilehash: 50d4745eca5954275abaa59dab726e7cf7ea193f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458424"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-webforms"></a>Ejemplo de cookies de SameSite C# para WebForms de 4.7.2 de ASP.net

.NET Framework 4,7 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se ajusta al estándar original.
El comportamiento revisado cambió el significado de `SameSite.None` para emitir el atributo con un valor de `None`, en lugar de no emitir el valor. Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.

## <a name="sampleCode"></a>Escribir el atributo SameSite

A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie;

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookie
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for SameSite none.
// Note this will also require you to be running on HTTPS
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

El atributo sameSite predeterminado para una cookie de autenticación de formularios se establece en el parámetro `cookieSameSite` de la configuración de autenticación de formularios en `web.config` 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

El atributo sameSite predeterminado para el estado de sesión también se establece en el parámetro ' cookieSameSite ' de la configuración de sesión en `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

La actualización de noviembre de 2019 de .NET cambió la configuración predeterminada para la autenticación de formularios y la sesión en `lax` como la configuración más compatible. sin embargo, si inserta páginas en iframe, es posible que necesite revertir esta configuración a ninguna y, a continuación, agregar el código de [intercepción](#interception) que se muestra a continuación para ajustar el comportamiento de `none` en función del explorador.

### <a name="running-the-sample"></a>Ejecución del ejemplo

Si ejecuta el proyecto de ejemplo, cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio.
Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.

![Lista de cookies del depurador del explorador](sample/img/BrowserDebugger.png)

Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookies" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).

## <a name="interception"></a>Interceptar cookies que no controla

.NET 4.5.2 presentó un nuevo evento para interceptar la escritura de encabezados `Response.AddOnSendingHeaders`. Se puede usar para interceptar cookies antes de que se devuelvan al equipo cliente. En el ejemplo se conecta el evento a un método estático que comprueba si el explorador admite los nuevos cambios de sameSite y, si no es así, cambia las cookies para que no emita el atributo si se ha establecido el nuevo valor de `None`.

Vea [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/Global.asax.cs) para obtener un ejemplo de cómo enlazar el evento y [SameSiteCookieRewriter.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpWebForms/SameSiteCookieRewriter.cs) para obtener un ejemplo de cómo controlar el evento y ajustar la cookie `sameSite` atributo.

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

## <a name="more-information"></a>Más información

[Actualizaciones de Chrome](https://www.chromium.org/updates/same-site)

[Documentación de ASP.NET](/aspnet/samesite/system-web-samesite)

[Revisiones de .NET SameSite](/aspnet/samesite/kbs-samesite)