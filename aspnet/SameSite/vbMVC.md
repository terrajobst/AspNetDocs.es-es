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
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a><span data-ttu-id="d194a-103">Ejemplo de cookie de SameSite para ASP.NET 4.7.2 VB MVC</span><span class="sxs-lookup"><span data-stu-id="d194a-103">SameSite cookie sample for ASP.NET 4.7.2 VB MVC</span></span>

<span data-ttu-id="d194a-104">.NET Framework 4,7 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se ajusta al estándar original.</span><span class="sxs-lookup"><span data-stu-id="d194a-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="d194a-105">El comportamiento revisado cambió el significado de `SameSite.None` para emitir el atributo con un valor de `None`, en lugar de no emitir el valor.</span><span class="sxs-lookup"><span data-stu-id="d194a-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="d194a-106">Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.</span><span class="sxs-lookup"><span data-stu-id="d194a-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="d194a-107">Escribir el atributo SameSite</span><span class="sxs-lookup"><span data-stu-id="d194a-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="d194a-108">A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie;</span><span class="sxs-lookup"><span data-stu-id="d194a-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

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

<span data-ttu-id="d194a-109">El atributo sameSite predeterminado para el estado de sesión se establece en el parámetro ' cookieSameSite ' de la configuración de sesión en `web.config`</span><span class="sxs-lookup"><span data-stu-id="d194a-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="d194a-110">Autenticación MVC</span><span class="sxs-lookup"><span data-stu-id="d194a-110">MVC Authentication</span></span>

<span data-ttu-id="d194a-111">La autenticación basada en cookies de ASP.net OWIN usa un administrador de cookies para habilitar el cambio de atributos de cookies.</span><span class="sxs-lookup"><span data-stu-id="d194a-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="d194a-112">[SameSiteCookieManager. VB](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) es una implementación de este tipo de clase que se puede copiar en sus propios proyectos.</span><span class="sxs-lookup"><span data-stu-id="d194a-112">The [SameSiteCookieManager.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="d194a-113">Debe asegurarse de que los componentes de Microsoft. Owin se han actualizado a la versión 4.1.0 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d194a-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="d194a-114">Compruebe el archivo de `packages.config` para asegurarse de que todos los números de versión coinciden, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d194a-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

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

<span data-ttu-id="d194a-115">Los componentes de autenticación deben estar configurados para usar CookieManager en la clase startup.</span><span class="sxs-lookup"><span data-stu-id="d194a-115">The authentication components must be configured to use the CookieManager in your startup class;</span></span>

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

<span data-ttu-id="d194a-116">Se debe establecer un administrador de cookies en *cada* componente que lo admita, esto incluye CookieAuthentication y OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="d194a-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="d194a-117">SystemWebCookieManager se utiliza para evitar [problemas conocidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) con la integración de cookies de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d194a-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="d194a-118">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d194a-118">Running the sample</span></span>

<span data-ttu-id="d194a-119">Si ejecuta el proyecto de ejemplo, cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio.</span><span class="sxs-lookup"><span data-stu-id="d194a-119">If you run the sample project please load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="d194a-120">Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.</span><span class="sxs-lookup"><span data-stu-id="d194a-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista de cookies del depurador del explorador](sample/img/BrowserDebugger.png)

<span data-ttu-id="d194a-122">Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookie de SameSite" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="d194a-122">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="d194a-123">Interceptar cookies que no controla</span><span class="sxs-lookup"><span data-stu-id="d194a-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="d194a-124">.NET 4.5.2 presentó un nuevo evento para interceptar la escritura de encabezados `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="d194a-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="d194a-125">Se puede usar para interceptar cookies antes de que se devuelvan al equipo cliente.</span><span class="sxs-lookup"><span data-stu-id="d194a-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="d194a-126">En el ejemplo se conecta el evento a un método estático que comprueba si el explorador admite los nuevos cambios de sameSite y, si no es así, cambia las cookies para que no emita el atributo si se ha establecido el nuevo valor de `None`.</span><span class="sxs-lookup"><span data-stu-id="d194a-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="d194a-127">Vea [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) para obtener un ejemplo de cómo enlazar el evento y [SameSiteCookieRewriter. VB](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) para obtener un ejemplo de cómo controlar el evento y ajustar la cookie `sameSite` atributo que se puede copiar en su propio código.</span><span class="sxs-lookup"><span data-stu-id="d194a-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

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

<span data-ttu-id="d194a-128">Puede cambiar el comportamiento específico de cookies con nombre de forma muy similar; en el ejemplo siguiente se ajusta la cookie de autenticación predeterminada de `Lax` a `None` en los exploradores que admiten el valor de `None` o se quita el atributo sameSite en los exploradores que no admiten `None`.</span><span class="sxs-lookup"><span data-stu-id="d194a-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="d194a-129">Más información</span><span class="sxs-lookup"><span data-stu-id="d194a-129">More Information</span></span>
 
[<span data-ttu-id="d194a-130">Actualizaciones de Chrome</span><span class="sxs-lookup"><span data-stu-id="d194a-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="d194a-131">Documentación de OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="d194a-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="d194a-132">Documentación de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d194a-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="d194a-133">Revisiones de .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="d194a-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)