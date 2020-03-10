---
title: Ejemplo de cookie de SameSite para ASP.NET 4.7.2 VB MVC
author: blowdart
description: Ejemplo de cookie de SameSite para ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440851"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a>Ejemplo de cookie de SameSite para ASP.NET 4.7.2 VB MVC

.NET Framework 4,7 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se ajusta al estándar original.
El comportamiento revisado cambió el significado de `SameSite.None` para emitir el atributo con un valor de `None`, en lugar de no emitir el valor. Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.

## <a name="sampleCode"></a>Escribir el atributo SameSite

A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie;

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
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

La autenticación basada en cookies de ASP.net OWIN usa un administrador de cookies para habilitar el cambio de atributos de cookies. [SameSiteCookieManager. VB](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) es una implementación de este tipo de clase que se puede copiar en sus propios proyectos. 

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

Los componentes de autenticación deben estar configurados para usar CookieManager en la clase startup.

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

Se debe establecer un administrador de cookies en *cada* componente que lo admita, esto incluye CookieAuthentication y OpenIdConnectAuthentication.

SystemWebCookieManager se utiliza para evitar [problemas conocidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) con la integración de cookies de respuesta.

### <a name="running-the-sample"></a>Ejecutar el ejemplo

Si ejecuta el proyecto de ejemplo, cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio.
Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.

![Lista de cookies del depurador del explorador](sample/img/BrowserDebugger.png)

Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookie de SameSite" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).

## <a name="interception"></a>Interceptar cookies que no controla

.NET 4.5.2 presentó un nuevo evento para interceptar la escritura de encabezados `Response.AddOnSendingHeaders`. Se puede usar para interceptar cookies antes de que se devuelvan al equipo cliente. En el ejemplo se conecta el evento a un método estático que comprueba si el explorador admite los nuevos cambios de sameSite y, si no es así, cambia las cookies para que no emita el atributo si se ha establecido el nuevo valor de `None`.

Vea [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) para obtener un ejemplo de cómo enlazar el evento y [SameSiteCookieRewriter. VB](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) para obtener un ejemplo de cómo controlar el evento y ajustar la cookie `sameSite` atributo que se puede copiar en su propio código.

```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

Puede cambiar el comportamiento específico de cookies con nombre de forma muy similar; en el ejemplo siguiente se ajusta la cookie de autenticación predeterminada de `Lax` a `None` en los exploradores que admiten el valor de `None` o se quita el atributo sameSite en los exploradores que no admiten `None`.

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a>Más información
 
[Actualizaciones de Chrome](https://www.chromium.org/updates/same-site)

[Documentación de OWIN SameSite](/aspnet/samesite/owin-samesite)

[Documentación de ASP.NET](/aspnet/samesite/system-web-samesite)

[Revisiones de .NET SameSite](/aspnet/samesite/kbs-samesite)