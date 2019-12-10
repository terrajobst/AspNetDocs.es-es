---
title: Trabajo con cookies de SameSite y la interfaz web abierta para .NET (OWIN)
author: rick-anderson
description: Trabajo con cookies de SameSite y la interfaz web abierta para .NET (OWIN)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: ac5ae24eeb9e8e1cc6296667a4bebef72c3eb62c
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993080"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>Cookies de SameSite y la interfaz web abierta para .NET (OWIN)

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` es un borrador de [IETF](https://ietf.org/about/) diseñado para proporcionar protección contra los ataques de falsificación de solicitud entre sitios (CSRF). El [borrador de SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Trata las cookies como `SameSite=Lax` de forma predeterminada.
* Indica que las cookies que imponen explícitamente `SameSite=None` para habilitar la entrega entre sitios se deben marcar como `Secure`.

`Lax` funciona para la mayoría de las cookies de aplicación. Algunas formas de autenticación como [OpenID Connect](https://openid.net/connect/) (OIDC) y [WS-Federation](https://auth0.com/docs/protocols/ws-fed) tienen como valor predeterminado el envío de redirecciones basadas en post. Las redirecciones basadas en POST desencadenan las protecciones del explorador `SameSite`, por lo que `SameSite` está deshabilitado para estos componentes. La mayoría de los inicios de sesión de [OAuth](https://oauth.net/) no se ven afectados debido a las diferencias en el modo en que fluye la solicitud Todos los demás componentes **no** establecen `SameSite` de forma predeterminada y usan el comportamiento predeterminado de los clientes (anterior o nuevo).

El parámetro `None` causa problemas de compatibilidad con los clientes que implementaron el [estándar de borrador anterior 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (por ejemplo, iOS 12). Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

Cada componente OWIN que emite cookies debe decidir si `SameSite` es adecuado.

Para obtener la versión ASP.NET 4. x de este artículo, consulte <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>Uso de la API con SameSite

`Microsoft.Owin` tiene su propia implementación de `SameSite`:

* Que no depende directamente del de `System.Web`.
* `SameSite` funciona en todas las versiones que tienen como destino los paquetes de `Microsoft.Owin`, .NET 4,5 y versiones posteriores.
* Solo el componente [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) interactúa directamente con la clase `HttpCookie` de `System.Web`.

`SystemWebCookieManager` depende de las API de `System.Web` de .NET 4.7.2 para habilitar la compatibilidad con `SameSite` y las revisiones para cambiar el comportamiento.

Los motivos para usar `SystemWebCookieManager` se describen en los [problemas de integración de cookies de respuesta de OWIN y System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues). se recomienda `SystemWebCookieManager` cuando se ejecuta en `System.Web`.

El código siguiente establece `SameSite` en `Lax`:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

Las siguientes API usan `SameSite`:

* [Microsoft. Owin. SameSiteMode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [CookieOptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Clase CookieAuthenticationOptions](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [CookieAuthenticationOptions. CookieSameSite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [SystemWebChunkingCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [CookieAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [OpenIdConnectAuthenticationOptions. CookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Historial y cambios

[Microsoft. Owin](https://www.nuget.org/packages/Microsoft.Owin/) nunca admitía el [`SameSite` borrador estándar de 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

La compatibilidad con el [borrador de SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) solo está disponible en `Microsoft.Owin` 4.1.0 y versiones posteriores. No hay revisiones para versiones anteriores.

El borrador 2019 de la especificación de `SameSite`:

* **No** es compatible con las versiones anteriores del borrador 2016. Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.
* Especifica que las cookies se tratan como `SameSite=Lax` de forma predeterminada.
* Especifica las cookies que validan explícitamente `SameSite=None` para habilitar la entrega entre sitios debe marcarse como `Secure`. `None` es una nueva entrada para rechazarla.
* Está programado para que [Chrome](https://chromestatus.com/feature/5088147346030592) lo habilite de forma predeterminada en [febrero de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Los exploradores empezaron a pasar a este estándar en 2019.
* Es compatible con las revisiones emitidas tal y como se describe en los artículos de Knowledge base. Para obtener más información, vea <xref:samesite/kbs-samesite>.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Compatibilidad con exploradores más antiguos

2016 `SameSite` estándar asigna que los valores desconocidos se deben tratar como valores `SameSite=Strict`. Las aplicaciones a las que se accede desde exploradores más antiguos que admiten el estándar 2016 `SameSite` se pueden interrumpir cuando obtienen una propiedad `SameSite` con un valor de `None`. Las aplicaciones web deben implementar la detección del explorador si pretenden admitir exploradores más antiguos. ASP.NET no implementa la detección del explorador porque los valores de los agentes de usuario son muy volátiles y cambian con frecuencia. Un punto de extensión de [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) permite conectar lógica específica del agente de usuario.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

En `Startup.Configuration`, agregue código similar al siguiente:

[!code-csharp[](sample/Startup1.cs?name=snippet)]

El código anterior requiere la revisión de .NET 4.7.2 o posterior `SameSite`.

En el código siguiente se muestra un ejemplo de implementación de `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

En el ejemplo anterior, se llama a `DisallowsSameSiteNone` en el método `CheckSameSite`. `DisallowsSameSiteNone` es un método de usuario que detecta si el agente de usuario no es compatible con `SameSite` `None`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

En el código siguiente se muestra un método `DisallowsSameSiteNone` de ejemplo:

> [!WARNING]
> El código siguiente es solo para la demostración:
> * No debe considerarse como completa.
> * No se mantiene ni se admite.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Probar aplicaciones para problemas de SameSite

Las aplicaciones que interactúan con sitios remotos, como a través de un inicio de sesión de terceros, deben:

* Pruebe la interacción en varios exploradores.
* Aplique la [detección y la mitigación del explorador](#sob) que se describen en este documento.

Pruebe las aplicaciones web con una versión de cliente que pueda participar en el nuevo comportamiento de `SameSite`. Chrome, Firefox y el borde de cromo tienen nuevas marcas de características opcionales que se pueden usar para las pruebas. Una vez que la aplicación aplica las revisiones de `SameSite`, probarla con versiones de cliente anteriores, especialmente Safari. Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

### <a name="test-with-chrome"></a>Prueba con Chrome

Chrome 78 + ofrece resultados engañosos porque tiene una mitigación temporal en contexto. La mitigación temporal de Chrome 78 + permite cookies con menos de dos minutos de antigüedad. Chrome 76 o 77 con las marcas de prueba adecuadas habilitadas proporcionan resultados más precisos. Para probar el comportamiento del nuevo `SameSite`, cambie `chrome://flags/#same-site-by-default-cookies` a **habilitado**. Las versiones anteriores de Chrome (75 e inferiores) se muestran como erróneas con el nuevo valor de `None`. Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

Google no hace que las versiones anteriores de Chrome estén disponibles. Siga las instrucciones de [Descargar cromo](https://www.chromium.org/getting-involved/download-chromium) para probar versiones anteriores de Chrome. **No** Descargue Chrome desde los vínculos que se proporcionan al buscar versiones anteriores de Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Prueba con Safari

Safari 12 implementó estrictamente el borrador anterior y produce un error cuando el nuevo valor de `None` está en una cookie. `None` se evita a través del código de detección del explorador que [admite exploradores anteriores](#sob) en este documento. Pruebe los inicios de sesión de estilo de SO basados en Safari 12, Safari 13 y WebKit con MSAL, ADAL o cualquier biblioteca que use. El problema depende de la versión del sistema operativo subyacente. OSX Mojave (10,14) e iOS 12 se sabe que tienen problemas de compatibilidad con el nuevo comportamiento de `SameSite`. Al actualizar el sistema operativo a OSX Catalina (10,15) o iOS 13 se corrige el problema. Safari no tiene actualmente una marca de participación para probar el nuevo comportamiento de la especificación.

### <a name="test-with-firefox"></a>Prueba con Firefox

La compatibilidad con Firefox para el nuevo estándar se puede probar en la versión 68 + al optar por la `about:config` página con la marca de características `network.cookie.sameSite.laxByDefault`. No ha habido informes de problemas de compatibilidad con versiones anteriores de Firefox.

### <a name="test-with-edge-browser"></a>Probar con el explorador Edge

Edge admite el estándar de `SameSite` anterior. La versión perimetral 44 no tiene ningún problema de compatibilidad conocido con el nuevo estándar.

### <a name="test-with-edge-chromium"></a>Prueba con borde (cromo)

`SameSite` marcas se establecen en la página `edge://flags/#same-site-by-default-cookies`. No se detectaron problemas de compatibilidad con el cromo perimetral.

### <a name="test-with-electron"></a>Prueba con electrones

Las versiones de Electron incluyen versiones anteriores de Chromium. Por ejemplo, la versión de electrones utilizada por los equipos es cromo 66, que exhibe el comportamiento anterior. Debe realizar sus propias pruebas de compatibilidad con la versión de electrones que usa el producto. Consulte [compatibilidad con exploradores anteriores](#sob) en la sección siguiente.

## <a name="additional-resources"></a>Recursos adicionales

* [Blog de cromo: desarrolladores: Prepárese para New SameSite = None; Configuración de cookies seguras](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explicación de las cookies de SameSite](https://web.dev/samesite-cookies-explained/)
* [Problemas de integración de cookies de respuesta de OWIN y System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)
