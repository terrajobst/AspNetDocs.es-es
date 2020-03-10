---
title: Ejemplo de cookie SameSite para WebForms de ASP.NET 4.7.2 VB
author: blowdart
description: Ejemplo de cookie SameSite para WebForms de ASP.NET 4.7.2 VB
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbWF
ms.openlocfilehash: 8979edecc5acf7dac81b9f53d31af00389f4727c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438439"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a><span data-ttu-id="0b83f-103">Ejemplo de cookie SameSite para WebForms de ASP.NET 4.7.2 VB</span><span class="sxs-lookup"><span data-stu-id="0b83f-103">SameSite cookie sample for ASP.NET 4.7.2 VB WebForms</span></span>
<span data-ttu-id="0b83f-104">.NET Framework 4,7 tiene compatibilidad integrada con el atributo [SameSite](https://www.owasp.org/index.php/SameSite) , pero se ajusta al estándar original.</span><span class="sxs-lookup"><span data-stu-id="0b83f-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="0b83f-105">El comportamiento revisado cambió el significado de `SameSite.None` para emitir el atributo con un valor de `None`, en lugar de no emitir el valor.</span><span class="sxs-lookup"><span data-stu-id="0b83f-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="0b83f-106">Si no desea emitir el valor, puede establecer la propiedad `SameSite` de una cookie en-1.</span><span class="sxs-lookup"><span data-stu-id="0b83f-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="0b83f-107">Escribir el atributo SameSite</span><span class="sxs-lookup"><span data-stu-id="0b83f-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="0b83f-108">A continuación se describe un ejemplo de cómo escribir un atributo SameSite en una cookie;</span><span class="sxs-lookup"><span data-stu-id="0b83f-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

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

<span data-ttu-id="0b83f-109">El atributo sameSite predeterminado para una cookie de autenticación de formularios se establece en el parámetro `cookieSameSite` de la configuración de autenticación de formularios en `web.config`</span><span class="sxs-lookup"><span data-stu-id="0b83f-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="0b83f-110">El atributo sameSite predeterminado para el estado de sesión también se establece en el parámetro ' cookieSameSite ' de la configuración de sesión en `web.config`</span><span class="sxs-lookup"><span data-stu-id="0b83f-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="0b83f-111">La actualización de noviembre de 2019 de .NET cambió la configuración predeterminada para la autenticación de formularios y la sesión en `lax` como la configuración más compatible. sin embargo, si inserta páginas en iframe, es posible que necesite revertir esta configuración a ninguna y, a continuación, agregar el código de [intercepción](#interception) que se muestra a continuación para ajustar el comportamiento de `none` en función del explorador.</span><span class="sxs-lookup"><span data-stu-id="0b83f-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="0b83f-112">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="0b83f-112">Running the sample</span></span>

<span data-ttu-id="0b83f-113">Si ejecuta el proyecto de ejemplo, cargue el depurador del explorador en la página inicial y úselo para ver la colección de cookies para el sitio.</span><span class="sxs-lookup"><span data-stu-id="0b83f-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="0b83f-114">Para ello, en Edge y Chrome, presione `F12`, después, seleccione la pestaña `Application` y haga clic en la dirección URL del sitio en la opción `Cookies` de la sección `Storage`.</span><span class="sxs-lookup"><span data-stu-id="0b83f-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista de cookies del depurador del explorador](sample/img/BrowserDebugger.png)

<span data-ttu-id="0b83f-116">Puede ver en la imagen anterior que la cookie creada por el ejemplo al hacer clic en el botón "crear cookies" tiene un valor de atributo SameSite de `Lax`, que coincide con el valor establecido en el [código de ejemplo](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="0b83f-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="0b83f-117">Interceptar cookies que no controla</span><span class="sxs-lookup"><span data-stu-id="0b83f-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="0b83f-118">.NET 4.5.2 presentó un nuevo evento para interceptar la escritura de encabezados `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="0b83f-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="0b83f-119">Se puede usar para interceptar cookies antes de que se devuelvan al equipo cliente.</span><span class="sxs-lookup"><span data-stu-id="0b83f-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="0b83f-120">En el ejemplo se conecta el evento a un método estático que comprueba si el explorador admite los nuevos cambios de sameSite y, si no es así, cambia las cookies para que no emita el atributo si se ha establecido el nuevo valor de `None`.</span><span class="sxs-lookup"><span data-stu-id="0b83f-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="0b83f-121">Vea [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) para obtener un ejemplo de cómo enlazar el evento y [SameSiteCookieRewriter.CS](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) para obtener un ejemplo de cómo controlar el evento y ajustar la cookie `sameSite` atributo.</span><span class="sxs-lookup"><span data-stu-id="0b83f-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>


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

<span data-ttu-id="0b83f-122">Puede cambiar el comportamiento específico de cookies con nombre de forma muy similar; en el ejemplo siguiente se ajusta la cookie de autenticación predeterminada de `Lax` a `None` en los exploradores que admiten el valor de `None` o se quita el atributo sameSite en los exploradores que no admiten `None`.</span><span class="sxs-lookup"><span data-stu-id="0b83f-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="0b83f-123">Más información</span><span class="sxs-lookup"><span data-stu-id="0b83f-123">More Information</span></span>

[<span data-ttu-id="0b83f-124">Actualizaciones de Chrome</span><span class="sxs-lookup"><span data-stu-id="0b83f-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="0b83f-125">Documentación de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0b83f-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="0b83f-126">Revisiones de .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="0b83f-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)