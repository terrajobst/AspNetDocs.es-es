---
title: Trabajar con cookies de SameSite en ASP.NET
author: rick-anderson
description: Aprenda a usar las cookies de SameSite en ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: edb368910b24be2d042afe3c19ffa1fb23245443
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455715"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Trabajar con cookies de SameSite en ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite es un estándar de borrador de [IETF](https://ietf.org/about/) diseñado para proporcionar protección contra los ataques de falsificación de solicitud entre sitios (CSRF). Originalmente elaborado en [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), el borrador estándar se actualizó en [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). El estándar actualizado no es compatible con versiones anteriores del estándar anterior, y las diferencias más destacables son las siguientes:

* Las cookies sin encabezado SameSite se tratan de forma predeterminada como `SameSite=Lax`.
* `SameSite=None` debe usarse para permitir el uso de cookies entre sitios.
* Las cookies que validan `SameSite=None` también se deben marcar como `Secure`.
* Las aplicaciones que usan [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) pueden experimentar problemas con cookies de `sameSite=Lax` o `sameSite=Strict` porque `<iframe>` se trata como escenarios entre sitios.
* El valor `SameSite=None` no está permitido por el [estándar 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) y hace que algunas implementaciones traten estas cookies como `SameSite=Strict`. Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

La configuración de `SameSite=Lax` funciona en la mayoría de las cookies de aplicación. Algunas formas de autenticación como [OpenID Connect](https://openid.net/connect/) (OIDC) y [WS-Federation](https://auth0.com/docs/protocols/ws-fed) tienen como valor predeterminado el envío de redirecciones basadas en post. Las redirecciones basadas en POST desencadenan las protecciones del explorador de SameSite, por lo que SameSite está deshabilitado para estos componentes. La mayoría de los inicios de sesión de [OAuth](https://oauth.net/) no se ven afectados debido a las diferencias en el modo en que fluye la solicitud.

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
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Nota**: ' sin especificar ' solo está disponible para `system.web/httpCookies@sameSite` en este momento. Esperamos agregar una sintaxis similar a los atributos de cookieSameSite mostrados anteriormente en futuras actualizaciones. La configuración de `(SameSiteMode)(-1)` en el código sigue funcionando en las instancias de estas cookies. *

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a>Redestinar aplicaciones .NET

Para tener como destino .NET 4.7.2 o posterior:

* Asegúrese de que el *archivo Web. config* contiene lo siguiente:  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  La [Guía de migración de .net](/dotnet/framework/migration-guide/) contiene más detalles.

* Compruebe que los paquetes NuGet del proyecto tengan como destino la versión correcta de Framework. Puede comprobar la versión correcta del marco examinando el archivo *packages. config* , por ejemplo:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  En el archivo *packages. config* anterior, el paquete de `Microsoft.ApplicationInsights`:
    * Está destinado a .NET 4.5.1.
    * Debe tener el atributo `targetFramework` actualizado a `net472` si existe un paquete actualizado destinado al destino de .NET Framework.

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a>Versiones de .NET anteriores a 4.7.2

Microsoft no admite versiones de .NET inferiores a las 4.7.2 para escribir el atributo de cookie del mismo sitio. No hemos encontrado una manera confiable de:

* Asegúrese de que el atributo se escribe correctamente según la versión del explorador.
* Interceptar y ajustar la autenticación y las cookies de sesión en versiones anteriores de .NET Framework.

### <a name="december-patch-behavior-changes"></a>Cambios de comportamiento de revisión de diciembre

El cambio de comportamiento específico para .NET Framework es el modo en que la propiedad `SameSite` interpreta el valor `None`:

* Antes de que la revisión sea un valor de `None` significaba:
  * No emita el atributo.
* Después de la revisión:
  * Un valor de `None`significa "emitir el atributo con un valor de `None`".
  * Un valor `SameSite` de `(SameSiteMode)(-1)` hace que no se emita el atributo.

Se ha cambiado el valor predeterminado de SameSite para la autenticación de formularios y las cookies de estado de sesión de `None` a `Lax`.

### <a name="summary-of-change-impact-on-browsers"></a>Resumen del impacto de los cambios en los exploradores

Si instala la revisión y emite una cookie con `SameSite.None`, se producirá una de las dos acciones siguientes:
* Chrome V80 tratará esta cookie según la nueva implementación y no aplicará las mismas restricciones de sitio en la cookie.
* Cualquier explorador que no se haya actualizado para admitir la nueva implementación seguirá la implementación anterior. La implementación anterior indica:
  * Si ve un valor que no entiende, omítalo y cambie a las mismas restricciones de sitio estrictas.

Por lo tanto, la aplicación se interrumpe en Chrome o se interrumpe en muchos otros lugares.

## <a name="history-and-changes"></a>Historial y cambios

La compatibilidad con SameSite se implementó por primera vez en .NET 4.7.2 con el [estándar de borrador 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

Las actualizaciones del 19 de noviembre de 2019 de Windows actualizaron .NET 4.7.2 + del estándar 2016 al estándar 2019. Hay actualizaciones adicionales disponibles para otras versiones de Windows. Para más información, consulte <xref:samesite/kbs-samesite>.

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

El estándar 2016 SameSite impediba que los valores desconocidos se deben tratar como valores `SameSite=Strict`. Las aplicaciones a las que se accede desde exploradores más antiguos que admiten el estándar 2016 SameSite se pueden interrumpir cuando obtienen una propiedad SameSite con un valor de `None`. Las aplicaciones web deben implementar la detección del explorador si pretenden admitir exploradores más antiguos. ASP.NET no implementa la detección del explorador porque los valores de los agentes de usuario son muy volátiles y cambian con frecuencia.

El enfoque de Microsoft para solucionar el problema es ayudarle a implementar los componentes de detección del explorador para quitar el `sameSite=None` atributo de las cookies si se sabe que un explorador no lo admite. El Consejo de Google fue emitir cookies dobles, una con el nuevo atributo y otra sin el atributo. Sin embargo, consideramos que el Consejo de Google está limitado. Algunos exploradores, especialmente los exploradores móviles, tienen límites muy pequeños en el número de cookies de un sitio o un nombre de dominio que puede enviar. El envío de varias cookies, especialmente las cookies de gran tamaño, como las cookies de autenticación, puede llegar rápidamente al límite del explorador móvil, lo que provoca errores en la aplicación que son difíciles de diagnosticar y corregir. Además, como un marco de trabajo hay un gran ecosistema de código y componentes de terceros que no se pueden actualizar para utilizar un enfoque de doble cookie.

El código de detección de explorador usado en los proyectos de ejemplo de [este repositorio de github]() se encuentra en dos archivos.

* [C#SameSiteSupport.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [VB SameSiteSupport. VB](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

Estas detecciones son los agentes de explorador más comunes que se han detectado y admiten el estándar 2016 y para los que es necesario quitar por completo el atributo. No se ha diseñado como una implementación completa:

* Es posible que la aplicación vea exploradores que no son de su sitio de prueba.
* Debe estar preparado para agregar detecciones según sea necesario para su entorno.

La forma de conectar la detección varía en función de la versión de .NET y el marco web que esté usando. Se puede llamar al siguiente código en el sitio de llamada de <xref:HTTP.HttpCookie>:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

Vea los siguientes temas de ASP.NET 4.7.2 SameSite cookie:

* [C#MVC](xref:samesite/csMVC)
* [C#WebForms](xref:samesite/CSharpWebForms)
* [WebForms de VB](xref:samesite/vbWF)
* [MVC DE VB](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a>Asegurarse de que el sitio se redirige a HTTPS

En el caso de ASP.NET 4. x, WebForms y MVC, [la característica de reescritura de direcciones URL de IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) se puede usar para redirigir todas las solicitudes a https. El siguiente código XML muestra una regla de ejemplo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Redirect to https" stopProcessing="true">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTPS}" pattern="Off"/>
            <add input="{REQUEST_METHOD}" pattern="^get$|^head$" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent"/>
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

En las instalaciones locales de [reescritura de direcciones URL de IIS](https://www.iis.net/downloads/microsoft/url-rewrite) es una característica opcional que puede necesitar instalar.

## <a name="test-apps-for-samesite-problems"></a>Probar aplicaciones para problemas de SameSite

Debe probar la aplicación con los exploradores que admite y revisar los escenarios que implican cookies. Normalmente, los escenarios de cookies implican

* Formularios de inicio de sesión
* Mecanismos de inicio de sesión externos como Facebook, Azure AD, OAuth y OIDC
* Páginas que aceptan solicitudes de otros sitios
* Páginas de la aplicación diseñadas para insertarse en iframes

Debe comprobar que las cookies se crean, se conservan y se eliminan correctamente en la aplicación.

Las aplicaciones que interactúan con sitios remotos, como a través de un inicio de sesión de terceros, deben:

* Pruebe la interacción en varios exploradores.
* Aplique la [detección y la mitigación del explorador](#sob) que se describen en este documento.

Pruebe las aplicaciones web con una versión de cliente que pueda participar en el nuevo comportamiento de SameSite. Chrome, Firefox y el borde de cromo tienen nuevas marcas de características opcionales que se pueden usar para las pruebas. Una vez que la aplicación aplica las revisiones de SameSite, probarla con versiones de cliente anteriores, especialmente Safari. Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

### <a name="test-with-chrome"></a>Prueba con Chrome

Chrome 78 + ofrece resultados engañosos porque tiene una mitigación temporal en contexto. La mitigación temporal de Chrome 78 + permite cookies con menos de dos minutos de antigüedad. Chrome 76 o 77 con las marcas de prueba adecuadas habilitadas proporcionan resultados más precisos. Para probar el nuevo comportamiento de SameSite, cambie `chrome://flags/#same-site-by-default-cookies` a **habilitado**. Las versiones anteriores de Chrome (75 e inferiores) se muestran como erróneas con el nuevo valor de `None`. Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.

Google no hace que las versiones anteriores de Chrome estén disponibles. Siga las instrucciones de [Descargar cromo](https://www.chromium.org/getting-involved/download-chromium) para probar versiones anteriores de Chrome. **No** Descargue Chrome desde los vínculos que se proporcionan al buscar versiones anteriores de Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* Si no está usando una versión de 64 bits de Windows, puede usar el [visor de OmahaProxy](https://omahaproxy.appspot.com/) para buscar la rama de cromo que corresponde a Chrome 74 (v 74.0.3729.108) mediante las [instrucciones proporcionadas por cromo](https://www.chromium.org/getting-involved/download-chromium).

#### <a name="test-with-chrome-80"></a>Prueba con Chrome 80 +

[Descargue](https://www.google.com/chrome/) una versión de Chrome que admita su nuevo atributo. En el momento de escribir este documento, la versión actual es Chrome 80. Chrome 80 necesita que la marca `chrome://flags/#same-site-by-default-cookies` habilitada para usar el nuevo comportamiento. También debe habilitar (`chrome://flags/#cookies-without-same-site-must-be-secure`) para probar el próximo comportamiento de las cookies que no tienen ningún atributo sameSite habilitado. Chrome 80 está en el destino para que el modificador pueda tratar las cookies sin el atributo como `SameSite=Lax`, aunque con un período de gracia temporal para ciertas solicitudes. Para deshabilitar el período de gracia tiempo, se puede iniciar Chrome 80 con el siguiente argumento de línea de comandos:

`--enable-features=SameSiteDefaultChecksMethodRigorously`

Chrome 80 tiene mensajes de advertencia en la consola del explorador sobre los atributos sameSite que faltan. Use F12 para abrir la consola del explorador.

### <a name="test-with-safari"></a>Prueba con Safari

Safari 12 implementó estrictamente el borrador anterior y produce un error cuando el nuevo valor de `None` está en una cookie. `None` se evita a través del código de detección del explorador que [admite exploradores anteriores](#sob) en este documento. Pruebe los inicios de sesión de estilo de SO basados en Safari 12, Safari 13 y WebKit con MSAL, ADAL o cualquier biblioteca que use. El problema depende de la versión del sistema operativo subyacente. OSX Mojave (10,14) e iOS 12 se sabe que tienen problemas de compatibilidad con el nuevo comportamiento de SameSite. Al actualizar el sistema operativo a OSX Catalina (10,15) o iOS 13 se corrige el problema. Safari no tiene actualmente una marca de participación para probar el nuevo comportamiento de la especificación.

### <a name="test-with-firefox"></a>Prueba con Firefox

La compatibilidad con Firefox para el nuevo estándar se puede probar en la versión 68 + al optar por la `about:config` página con la marca de características `network.cookie.sameSite.laxByDefault`. No ha habido informes de problemas de compatibilidad con versiones anteriores de Firefox.

### <a name="test-with-edge-legacy-browser"></a>Prueba con el explorador perimetral (heredado)

Edge es compatible con el estándar SameSite antiguo. La versión de Edge 44 + no tiene ningún problema de compatibilidad conocido con el nuevo estándar.

### <a name="test-with-edge-chromium"></a>Prueba con borde (cromo)

Las marcas SameSite se establecen en la página `edge://flags/#same-site-by-default-cookies`. No se detectaron problemas de compatibilidad con el cromo perimetral.

### <a name="test-with-electron"></a>Prueba con electrones

Las versiones de Electron incluyen versiones anteriores de Chromium. Por ejemplo, la versión de electrones utilizada por los equipos es cromo 66, que exhibe el comportamiento anterior. Debe realizar sus propias pruebas de compatibilidad con la versión de electrones que usa el producto. Consulte [compatibilidad con exploradores más antiguos](#sob).

## <a name="reverting-samesite-patches"></a>Revertir revisiones de SameSite

Puede revertir el comportamiento actualizado de sameSite en .NET Framework aplicaciones a su comportamiento anterior, donde no se emite el atributo sameSite para un valor de `None`y revertir las cookies de autenticación y sesión para que no emitan el valor. Esto se debe ver como una *solución extremadamente temporal*, ya que los cambios de cromo interrumpirán cualquier solicitud post externa o la autenticación para los usuarios que usen exploradores que admitan los cambios en el estándar.

### <a name="reverting-net-472-behavior"></a>Reversión del comportamiento de 4.7.2 de .NET

Actualice el *archivo Web. config* para incluir los siguientes valores de configuración:

```xml
<configuration> 
  <appSettings>
    <add key="aspnet:SuppressSameSiteNone" value="true" />
  </appSettings>
 
  <system.web> 
    <authentication> 
      <forms cookieSameSite="None" /> 
    </authentication> 
    <sessionState cookieSameSite="None" /> 
  </system.web> 
</configuration>
```

## <a name="additional-resources"></a>Recursos adicionales

* [Próximos cambios en la cookie de SameSite en ASP.NET y ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog de cromo: desarrolladores: Prepárese para New SameSite = None; Configuración de cookies seguras](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [Explicación de las cookies de SameSite](https://web.dev/samesite-cookies-explained/)
* [Actualizaciones de Chrome](https://www.chromium.org/updates/same-site)
* [Revisiones de .NET SameSite](/aspnet/samesite/kbs-samesite)
* [Información del mismo sitio de aplicaciones Web de Azure](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [Información del mismo sitio de Azure Active Directory](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
