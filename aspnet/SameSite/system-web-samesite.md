---
title: Trabajar con cookies de SameSite en ASP.NET
author: rick-anderson
description: Aprenda a usar las cookies de SameSite en ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c81ca38648609aa5347d2a8cc11889fc85d81711
ms.sourcegitcommit: 4d439e01c82c7c95b19216fedaf5b1a11a1deb06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76826619"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Trabajar con cookies de SameSite en ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite es un estándar de borrador de [IETF](https://ietf.org/about/) diseñado para proporcionar protección contra los ataques de falsificación de solicitud entre sitios (CSRF). Originalmente elaborado en [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), el borrador estándar se actualizó en [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). El estándar actualizado no es compatible con versiones anteriores del estándar anterior, y las diferencias más destacables son las siguientes:

* Las cookies sin encabezado SameSite se tratan de forma predeterminada como `SameSite=Lax`.
* `SameSite=None` debe usarse para permitir el uso de cookies entre sitios.
* Las cookies que validan `SameSite=None` también se deben marcar como `Secure`.
* El valor SameSite = None no lo permite el [estándar 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) y hace que algunas implementaciones traten estas cookies como SameSite = STRICT. Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

La configuración de `SameSite=Lax` funciona en la mayoría de las cookies de aplicación. Algunas formas de autenticación como [OpenID Connect](https://openid.net/connect/) (OIDC) y [WS-Federation](https://auth0.com/docs/protocols/ws-fed) tienen como valor predeterminado el envío de redirecciones basadas en post. Las redirecciones basadas en POST desencadenan las protecciones del explorador de SameSite, por lo que SameSite está deshabilitado para estos componentes. La mayoría de los inicios de sesión de [OAuth](https://oauth.net/) no se ven afectados debido a las diferencias en el modo en que fluye la solicitud.

Las aplicaciones que usan `iframe` pueden experimentar problemas con cookies de `SameSite=Lax` o `SameSite=Strict` porque los iframe se tratan como escenarios entre sitios.

Cada componente de ASP.NET que emite cookies debe decidir si SameSite es adecuado.

Vea [problemas conocidos](#known) de problemas con las aplicaciones después de instalar las actualizaciones de SameSite .net de 2019.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Uso de SameSite en ASP.NET 4.7.2 y 4,8

.Net 4.7.2 y 4,8 admiten el [borrador estándar 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) de SameSite desde la publicación de actualizaciones en diciembre de 2019. Los desarrolladores pueden controlar mediante programación el valor del encabezado SameSite mediante la [propiedad HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite). Al establecer la propiedad `SameSite` en `Strict`, `Lax`o `None`, los valores se escriben en la red con la cookie. Si se establece en `(SameSiteMode)(-1)`, indica que no debe incluirse ningún encabezado SameSite en la red con la cookie. La [propiedad HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure)o ' requireSSL ' en los archivos de configuración se pueden usar para marcar la cookie como `Secure` o no.

Las nuevas instancias de `HttpCookie` tendrán como valor predeterminado `SameSite=(SameSiteMode)(-1)` y `Secure=false`. Estos valores predeterminados se pueden invalidar en la sección configuración de `system.web/httpCookies`, donde la cadena `"Unspecified"` es una sintaxis sencilla de solo configuración para `(SameSiteMode)(-1)`:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

ASP.Net también emite cuatro cookies específicas propias para estas características: autenticación anónima, autenticación de formularios, estado de sesión y administración de roles. Las instancias de estas cookies obtenidas en tiempo de ejecución se pueden manipular mediante las propiedades `SameSite` y `Secure` como cualquier otra instancia de HttpCookie. Sin embargo, debido a la aparición de revisiones del estándar SameSite, las opciones de configuración de estas cuatro cookies de características son incoherentes. A continuación se muestran las secciones de configuración y los atributos pertinentes, con valores predeterminados. Si no hay ningún atributo relacionado con `SameSite` o `Secure` para una característica, la característica revertirá a los valores predeterminados configurados en la sección `system.web/httpCookies` descrita anteriormente.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequiresSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Nota**: ' sin especificar ' solo está disponible para `system.web/httpCookies@sameSite` en este momento. Esperamos agregar una sintaxis similar a los atributos de cookieSameSite mostrados anteriormente en futuras actualizaciones. La configuración de `(SameSiteMode)(-1)` en el código sigue funcionando en las instancias de estas cookies. *

## <a name="history-and-changes"></a>Historial y cambios

La compatibilidad con SameSite se implementó por primera vez en .NET 4.7.2 con el [estándar de borrador 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Las actualizaciones del 19 de noviembre de 2019 de Windows actualizaron .NET 4.7.2 + del estándar 2016 al estándar 2019. Hay actualizaciones adicionales disponibles para otras versiones de Windows. Para obtener más información, vea <xref:samesite/kbs-samesite>.

 El borrador 2019 de la especificación SameSite:

* **No** es compatible con las versiones anteriores del borrador 2016. Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.
* Especifica que las cookies se tratan como `SameSite=Lax` de forma predeterminada.
* Especifica las cookies que validan explícitamente `SameSite=None` para habilitar la entrega entre sitios también se debe marcar como `Secure`.
* Es compatible con las revisiones emitidas tal y como se describe en la lista anterior de KB.
* Está programado para que [Chrome](https://chromestatus.com/feature/5088147346030592) lo habilite de forma predeterminada en [febrero de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Los exploradores empezaron a pasar a este estándar en 2019.

<a name="known"><a/>

## <a name="known-issues"></a>Problemas conocidos

Dado que las especificaciones de borrador 2016 y 2019 no son compatibles, la actualización de .NET Framework de la versión de noviembre del 2019 presenta algunos cambios que pueden ser problemáticos.

* Las cookies de estado de sesión y autenticación de formularios ahora se escriben en la red como `Lax` en lugar de sin especificar.
  * Aunque la mayoría de las aplicaciones funcionan con cookies `SameSite=Lax`, las aplicaciones que se PUBLICAn en sitios o aplicaciones que hacen uso de `iframe` pueden encontrar que el estado de sesión o las cookies de autorización de formularios no se usan según lo esperado. Para solucionarlo, cambie el valor de `cookieSameSite` en la sección de configuración correspondiente, tal y como se explicó anteriormente.
* HttpCookies (que establecen explícitamente `SameSite=None` en el código o en la configuración ahora tienen ese valor escrito con la cookie, mientras que antes se omitió. Esto puede producir problemas con exploradores más antiguos que solo admiten el estándar de borrador 2016.
  * Cuando los exploradores de destino admiten el estándar de borrador de 2019 con cookies `SameSite=None`, recuerde también marcarlos `Secure` o es posible que no se reconozcan.
  * Para revertir al comportamiento 2016 de no escribir `SameSite=None`, use la configuración de la aplicación `aspnet:SupressSameSiteNone=true`. Tenga en cuenta que esto se aplicará a todos los httpCookies (de la aplicación.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Azure App Service: control de cookies de SameSite

Consulte [Azure App Service: control de cookies de SameSite y .NET Framework revisión de 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) para obtener información sobre cómo Azure App Service configura los comportamientos de SameSite en las aplicaciones de .net 4.7.2.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Compatibilidad con exploradores más antiguos

El estándar 2016 SameSite impediba que los valores desconocidos se deben tratar como valores `SameSite=Strict`. Las aplicaciones a las que se accede desde exploradores más antiguos que admiten el estándar 2016 SameSite se pueden interrumpir cuando obtienen una propiedad SameSite con un valor de `None`. Las aplicaciones web deben implementar la detección del explorador si pretenden admitir exploradores más antiguos. ASP.NET no implementa la detección del explorador porque los valores de los agentes de usuario son muy volátiles y cambian con frecuencia. Se puede llamar al siguiente código en el sitio de llamada de <xref:HTTP.HttpCookie>:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

En el ejemplo anterior, `MyUserAgentDetectionLib.DisallowsSameSiteNone` es una biblioteca suministrada por el usuario que detecta si el agente de usuario no es compatible con SameSite `None`. En el código siguiente se muestra un método `DisallowsSameSiteNone` de ejemplo:

> [!WARNING]
> El código siguiente es solo para la demostración:
> * No debe considerarse como completa.
> * No se mantiene ni se admite.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Probar aplicaciones para problemas de SameSite

Las aplicaciones que interactúan con sitios remotos, como a través de un inicio de sesión de terceros, deben:

* Pruebe la interacción en varios exploradores.
* Aplique la [detección y la mitigación del explorador](#sob) que se describen en este documento.

Pruebe las aplicaciones web con una versión de cliente que pueda participar en el nuevo comportamiento de SameSite. Chrome, Firefox y el borde de cromo tienen nuevas marcas de características opcionales que se pueden usar para las pruebas. Una vez que la aplicación aplica las revisiones de SameSite, probarla con versiones de cliente anteriores, especialmente Safari. Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

### <a name="test-with-chrome"></a>Prueba con Chrome

Chrome 78 + ofrece resultados engañosos porque tiene una mitigación temporal en contexto. La mitigación temporal de Chrome 78 + permite cookies con menos de dos minutos de antigüedad. Chrome 76 o 77 con las marcas de prueba adecuadas habilitadas proporcionan resultados más precisos. Para probar el nuevo comportamiento de SameSite, cambie `chrome://flags/#same-site-by-default-cookies` a **habilitado**. Las versiones anteriores de Chrome (75 e inferiores) se muestran como erróneas con el nuevo valor de `None`. Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

Google no hace que las versiones anteriores de Chrome estén disponibles. Siga las instrucciones de [Descargar cromo](https://www.chromium.org/getting-involved/download-chromium) para probar versiones anteriores de Chrome. **No** Descargue Chrome desde los vínculos que se proporcionan al buscar versiones anteriores de Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Prueba con Safari

Safari 12 implementó estrictamente el borrador anterior y produce un error cuando el nuevo valor de `None` está en una cookie. `None` se evita a través del código de detección del explorador que [admite exploradores anteriores](#sob) en este documento. Pruebe los inicios de sesión de estilo de SO basados en Safari 12, Safari 13 y WebKit con MSAL, ADAL o cualquier biblioteca que use. El problema depende de la versión del sistema operativo subyacente. OSX Mojave (10,14) e iOS 12 se sabe que tienen problemas de compatibilidad con el nuevo comportamiento de SameSite. Al actualizar el sistema operativo a OSX Catalina (10,15) o iOS 13 se corrige el problema. Safari no tiene actualmente una marca de participación para probar el nuevo comportamiento de la especificación.

### <a name="test-with-firefox"></a>Prueba con Firefox

La compatibilidad con Firefox para el nuevo estándar se puede probar en la versión 68 + al optar por la `about:config` página con la marca de características `network.cookie.sameSite.laxByDefault`. No ha habido informes de problemas de compatibilidad con versiones anteriores de Firefox.

### <a name="test-with-edge-browser"></a>Probar con el explorador Edge

Edge es compatible con el estándar SameSite antiguo. La versión perimetral 44 no tiene ningún problema de compatibilidad conocido con el nuevo estándar.

### <a name="test-with-edge-chromium"></a>Prueba con borde (cromo)

Las marcas SameSite se establecen en la página `edge://flags/#same-site-by-default-cookies`. No se detectaron problemas de compatibilidad con el cromo perimetral.

### <a name="test-with-electron"></a>Prueba con electrones

Las versiones de Electron incluyen versiones anteriores de Chromium. Por ejemplo, la versión de electrones utilizada por los equipos es cromo 66, que exhibe el comportamiento anterior. Debe realizar sus propias pruebas de compatibilidad con la versión de electrones que usa el producto. Consulte [compatibilidad con exploradores anteriores](#sob) en la sección siguiente.

## <a name="additional-resources"></a>Recursos adicionales

* [Próximos cambios en la cookie de SameSite en ASP.NET y ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog de cromo: desarrolladores: Prepárese para New SameSite = None; Configuración de cookies seguras](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explicación de las cookies de SameSite](https://web.dev/samesite-cookies-explained/)
