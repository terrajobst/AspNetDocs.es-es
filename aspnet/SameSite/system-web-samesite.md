---
title: Trabajar con cookies de SameSite en ASP.NET
author: rick-anderson
description: Aprenda a usar las cookies de SameSite en ASP.NET
ms.author: riande
ms.date: 2/15/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 7987a5d6c9b3a82679d42a2d381d471d56f495c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439939"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a><span data-ttu-id="aa365-103">Trabajar con cookies de SameSite en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="aa365-103">Work with SameSite cookies in ASP.NET</span></span>

<span data-ttu-id="aa365-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa365-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa365-105">SameSite es un estándar de borrador de [IETF](https://ietf.org/about/) diseñado para proporcionar protección contra los ataques de falsificación de solicitud entre sitios (CSRF).</span><span class="sxs-lookup"><span data-stu-id="aa365-105">SameSite is an [IETF](https://ietf.org/about/) draft standard designed to provide some protection against cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="aa365-106">Originalmente elaborado en [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), el borrador estándar se actualizó en [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span><span class="sxs-lookup"><span data-stu-id="aa365-106">Originally drafted in [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), the draft standard was updated in [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00).</span></span> <span data-ttu-id="aa365-107">El estándar actualizado no es compatible con versiones anteriores del estándar anterior, y las diferencias más destacables son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="aa365-107">The updated standard is not backward compatible with the previous standard, with the following being the most noticeable differences:</span></span>

* <span data-ttu-id="aa365-108">Las cookies sin encabezado SameSite se tratan de forma predeterminada como `SameSite=Lax`.</span><span class="sxs-lookup"><span data-stu-id="aa365-108">Cookies without SameSite header are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="aa365-109">`SameSite=None` debe usarse para permitir el uso de cookies entre sitios.</span><span class="sxs-lookup"><span data-stu-id="aa365-109">`SameSite=None` must be used to allow cross-site cookie use.</span></span>
* <span data-ttu-id="aa365-110">Las cookies que validan `SameSite=None` también se deben marcar como `Secure`.</span><span class="sxs-lookup"><span data-stu-id="aa365-110">Cookies that assert `SameSite=None` must also be marked as `Secure`.</span></span>
* <span data-ttu-id="aa365-111">Las aplicaciones que usan [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) pueden experimentar problemas con cookies de `sameSite=Lax` o `sameSite=Strict` porque `<iframe>` se trata como escenarios entre sitios.</span><span class="sxs-lookup"><span data-stu-id="aa365-111">Applications that use [`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe) may experience issues with `sameSite=Lax` or `sameSite=Strict` cookies because `<iframe>` is treated as cross-site scenarios.</span></span>
* <span data-ttu-id="aa365-112">El valor `SameSite=None` no está permitido por el [estándar 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) y hace que algunas implementaciones traten estas cookies como `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="aa365-112">The value `SameSite=None` is not allowed by the [2016 standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07) and causes some implementations to treat such cookies as `SameSite=Strict`.</span></span> <span data-ttu-id="aa365-113">Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-113">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="aa365-114">La configuración de `SameSite=Lax` funciona en la mayoría de las cookies de aplicación.</span><span class="sxs-lookup"><span data-stu-id="aa365-114">The `SameSite=Lax` setting works for most application cookies.</span></span> <span data-ttu-id="aa365-115">Algunas formas de autenticación como [OpenID Connect](https://openid.net/connect/) (OIDC) y [WS-Federation](https://auth0.com/docs/protocols/ws-fed) tienen como valor predeterminado el envío de redirecciones basadas en post.</span><span class="sxs-lookup"><span data-stu-id="aa365-115">Some forms of authentication like [OpenID Connect](https://openid.net/connect/) (OIDC) and [WS-Federation](https://auth0.com/docs/protocols/ws-fed) default to POST based redirects.</span></span> <span data-ttu-id="aa365-116">Las redirecciones basadas en POST desencadenan las protecciones del explorador de SameSite, por lo que SameSite está deshabilitado para estos componentes.</span><span class="sxs-lookup"><span data-stu-id="aa365-116">The POST based redirects trigger the SameSite browser protections, so SameSite is disabled for these components.</span></span> <span data-ttu-id="aa365-117">La mayoría de los inicios de sesión de [OAuth](https://oauth.net/) no se ven afectados debido a las diferencias en el modo en que fluye la solicitud.</span><span class="sxs-lookup"><span data-stu-id="aa365-117">Most [OAuth](https://oauth.net/) logins are not affected due to differences in how the request flows.</span></span>

<span data-ttu-id="aa365-118">Cada componente de ASP.NET que emite cookies debe decidir si SameSite es adecuado.</span><span class="sxs-lookup"><span data-stu-id="aa365-118">Each ASP.NET component that emits cookies needs to decide if SameSite is appropriate.</span></span>

<span data-ttu-id="aa365-119">Vea [problemas conocidos](#known) de problemas con las aplicaciones después de instalar las actualizaciones de SameSite .net de 2019.</span><span class="sxs-lookup"><span data-stu-id="aa365-119">See [Known Issues](#known) for problems with applications after installing the 2019 .Net SameSite updates.</span></span>

## <a name="using-samesite-in-aspnet-472-and-48"></a><span data-ttu-id="aa365-120">Uso de SameSite en ASP.NET 4.7.2 y 4,8</span><span class="sxs-lookup"><span data-stu-id="aa365-120">Using SameSite in ASP.NET 4.7.2 and 4.8</span></span>

<span data-ttu-id="aa365-121">.Net 4.7.2 y 4,8 admiten el [borrador estándar 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) de SameSite desde la publicación de actualizaciones en diciembre de 2019.</span><span class="sxs-lookup"><span data-stu-id="aa365-121">.Net 4.7.2 and 4.8 supports the [2019 draft standard](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) for SameSite since the release of updates in December 2019.</span></span> <span data-ttu-id="aa365-122">Los desarrolladores pueden controlar mediante programación el valor del encabezado SameSite mediante la [propiedad HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span><span class="sxs-lookup"><span data-stu-id="aa365-122">Developers are able to programmatically control the value of the SameSite header using the [HttpCookie.SameSite property](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite).</span></span> <span data-ttu-id="aa365-123">Al establecer la propiedad `SameSite` en `Strict`, `Lax`o `None`, los valores se escriben en la red con la cookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-123">Setting the `SameSite` property to `Strict`, `Lax`, or `None` results in those values being written on the network with the cookie.</span></span> <span data-ttu-id="aa365-124">Si se establece en `(SameSiteMode)(-1)`, indica que no debe incluirse ningún encabezado SameSite en la red con la cookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-124">Setting it equal to `(SameSiteMode)(-1)` indicates that no SameSite header should be included on the network with the cookie.</span></span> <span data-ttu-id="aa365-125">La [propiedad HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure)o ' requireSSL ' en los archivos de configuración se pueden usar para marcar la cookie como `Secure` o no.</span><span class="sxs-lookup"><span data-stu-id="aa365-125">The [HttpCookie.Secure Property](/dotnet/api/system.web.httpcookie.secure), or 'requireSSL' in config files, can be used to mark the cookie as `Secure` or not.</span></span>

<span data-ttu-id="aa365-126">Las nuevas instancias de `HttpCookie` tendrán como valor predeterminado `SameSite=(SameSiteMode)(-1)` y `Secure=false`.</span><span class="sxs-lookup"><span data-stu-id="aa365-126">New `HttpCookie` instances will default to `SameSite=(SameSiteMode)(-1)` and `Secure=false`.</span></span> <span data-ttu-id="aa365-127">Estos valores predeterminados se pueden invalidar en la sección configuración de `system.web/httpCookies`, donde la cadena `"Unspecified"` es una sintaxis sencilla de solo configuración para `(SameSiteMode)(-1)`:</span><span class="sxs-lookup"><span data-stu-id="aa365-127">These defaults can be overridden in the `system.web/httpCookies` configuration section, where the string `"Unspecified"` is a friendly configuration-only syntax for `(SameSiteMode)(-1)`:</span></span>

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

<span data-ttu-id="aa365-128">ASP.Net también emite cuatro cookies específicas propias para estas características: autenticación anónima, autenticación de formularios, estado de sesión y administración de roles.</span><span class="sxs-lookup"><span data-stu-id="aa365-128">ASP.Net also issues four specific cookies of its own for these features: Anonymous Authentication, Forms Authentication, Session State, and Role Management.</span></span> <span data-ttu-id="aa365-129">Las instancias de estas cookies obtenidas en tiempo de ejecución se pueden manipular mediante las propiedades `SameSite` y `Secure` como cualquier otra instancia de HttpCookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-129">Instances of these cookies obtained in runtime can be manipulated using the `SameSite` and `Secure` properties just like any other HttpCookie instance.</span></span> <span data-ttu-id="aa365-130">Sin embargo, debido a la aparición de revisiones del estándar SameSite, las opciones de configuración de estas cuatro cookies de características son incoherentes.</span><span class="sxs-lookup"><span data-stu-id="aa365-130">However, due to the patchwork emergence of the SameSite standard, configuration options for these four features cookies is inconsistent.</span></span> <span data-ttu-id="aa365-131">A continuación se muestran las secciones de configuración y los atributos pertinentes, con valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="aa365-131">The relevant configuration sections and attributes, with defaults, are shown below.</span></span> <span data-ttu-id="aa365-132">Si no hay ningún atributo relacionado con `SameSite` o `Secure` para una característica, la característica revertirá a los valores predeterminados configurados en la sección `system.web/httpCookies` descrita anteriormente.</span><span class="sxs-lookup"><span data-stu-id="aa365-132">If there is no `SameSite` or `Secure` related attribute for a feature, then the feature will fall back on the defaults configured in the `system.web/httpCookies` section discussed above.</span></span>

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

<span data-ttu-id="aa365-133">**Nota**: ' sin especificar ' solo está disponible para `system.web/httpCookies@sameSite` en este momento.</span><span class="sxs-lookup"><span data-stu-id="aa365-133">**Note**: 'Unspecified' is only available to `system.web/httpCookies@sameSite` at the moment.</span></span> <span data-ttu-id="aa365-134">Esperamos agregar una sintaxis similar a los atributos de cookieSameSite mostrados anteriormente en futuras actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="aa365-134">We hope to add similar syntax to the previously shown cookieSameSite attributes in future updates.</span></span> <span data-ttu-id="aa365-135">La configuración de `(SameSiteMode)(-1)` en el código sigue funcionando en las instancias de estas cookies. \*</span><span class="sxs-lookup"><span data-stu-id="aa365-135">Setting `(SameSiteMode)(-1)` in code still works on instances of these cookies.\*</span></span>

[!INCLUDE[](~/includes/MTcomments.md)]

<a name="retargeting"></a>

### <a name="retarget-net-apps"></a><span data-ttu-id="aa365-136">Redestinar aplicaciones .NET</span><span class="sxs-lookup"><span data-stu-id="aa365-136">Retarget .NET apps</span></span>

<span data-ttu-id="aa365-137">Para tener como destino .NET 4.7.2 o posterior:</span><span class="sxs-lookup"><span data-stu-id="aa365-137">To target .NET 4.7.2 or later:</span></span>

* <span data-ttu-id="aa365-138">Asegúrese de que el *archivo Web. config* contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="aa365-138">Ensure *web.config* contains the following:</span></span>  <!-- review, I removed `debug="true"` -->

  ```xml
  <system.web>
    <compilation targetFramework="4.7.2"/>
    <httpRuntime targetFramework="4.7.2"/>
  </system.web>

* Verify the project file contains the correct [TargetFrameworkVersion](/visualstudio/msbuild/msbuild-target-framework-and-target-platform?view=vs-2019):

  ```xml
  <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
  ```

  <span data-ttu-id="aa365-139">La [Guía de migración de .net](/dotnet/framework/migration-guide/) contiene más detalles.</span><span class="sxs-lookup"><span data-stu-id="aa365-139">The [.NET Migration Guide](/dotnet/framework/migration-guide/) has more details.</span></span>

* <span data-ttu-id="aa365-140">Compruebe que los paquetes NuGet del proyecto tengan como destino la versión correcta de Framework.</span><span class="sxs-lookup"><span data-stu-id="aa365-140">Verify NuGet packages in the project are targeted at the correct framework version.</span></span> <span data-ttu-id="aa365-141">Puede comprobar la versión correcta del marco examinando el archivo *packages. config* , por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="aa365-141">You can verify the correct framework version by examining the *packages.config* file, for example:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <packages>
    <package id="Microsoft.AspNet.Mvc" version="5.2.7" targetFramework="net472" />
    <package id="Microsoft.ApplicationInsights" version="2.4.0" targetFramework="net451" />
  </packages>
  ```

  <span data-ttu-id="aa365-142">En el archivo *packages. config* anterior, el paquete de `Microsoft.ApplicationInsights`:</span><span class="sxs-lookup"><span data-stu-id="aa365-142">In the preceding *packages.config* file, the `Microsoft.ApplicationInsights` package:</span></span>
    * <span data-ttu-id="aa365-143">Está destinado a .NET 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="aa365-143">Is  targeted against .NET 4.5.1.</span></span>
    * <span data-ttu-id="aa365-144">Debe tener el atributo `targetFramework` actualizado a `net472` si existe un paquete actualizado destinado al destino de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="aa365-144">Should have its `targetFramework` attribute updated to `net472` if an updated package targeting your framework target exists.</span></span>

<a name="nope"></a>

### <a name="net-versions-earlier-than-472"></a><span data-ttu-id="aa365-145">Versiones de .NET anteriores a 4.7.2</span><span class="sxs-lookup"><span data-stu-id="aa365-145">.NET versions earlier than 4.7.2</span></span>

<span data-ttu-id="aa365-146">Microsoft no admite versiones de .NET inferiores a las 4.7.2 para escribir el atributo de cookie del mismo sitio.</span><span class="sxs-lookup"><span data-stu-id="aa365-146">Microsoft does not support .NET versions lower that 4.7.2 for writing the same-site cookie attribute.</span></span> <span data-ttu-id="aa365-147">No hemos encontrado una manera confiable de:</span><span class="sxs-lookup"><span data-stu-id="aa365-147">We have not found a reliable way to:</span></span>

* <span data-ttu-id="aa365-148">Asegúrese de que el atributo se escribe correctamente según la versión del explorador.</span><span class="sxs-lookup"><span data-stu-id="aa365-148">Ensure the attribute is written correctly based on browser version.</span></span>
* <span data-ttu-id="aa365-149">Interceptar y ajustar la autenticación y las cookies de sesión en versiones anteriores de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="aa365-149">Intercept and adjust authentication and session cookies on older framework versions.</span></span>

### <a name="december-patch-behavior-changes"></a><span data-ttu-id="aa365-150">Cambios de comportamiento de revisión de diciembre</span><span class="sxs-lookup"><span data-stu-id="aa365-150">December patch behavior changes</span></span>

<span data-ttu-id="aa365-151">El cambio de comportamiento específico para .NET Framework es el modo en que la propiedad `SameSite` interpreta el valor `None`:</span><span class="sxs-lookup"><span data-stu-id="aa365-151">The specific behavior change for .NET Framework is how the `SameSite` property interprets the `None` value:</span></span>

* <span data-ttu-id="aa365-152">Antes de que la revisión sea un valor de `None` significaba:</span><span class="sxs-lookup"><span data-stu-id="aa365-152">Before the patch a value of `None` meant:</span></span>
  * <span data-ttu-id="aa365-153">No emita el atributo.</span><span class="sxs-lookup"><span data-stu-id="aa365-153">Do not emit the attribute at all.</span></span>
* <span data-ttu-id="aa365-154">Después de la revisión:</span><span class="sxs-lookup"><span data-stu-id="aa365-154">After the patch:</span></span>
  * <span data-ttu-id="aa365-155">Un valor de `None`significa "emitir el atributo con un valor de `None`".</span><span class="sxs-lookup"><span data-stu-id="aa365-155">A value of `None`it means "Emit the attribute with a value of `None`".</span></span>
  * <span data-ttu-id="aa365-156">Un valor `SameSite` de `(SameSiteMode)(-1)` hace que no se emita el atributo.</span><span class="sxs-lookup"><span data-stu-id="aa365-156">A `SameSite` value of `(SameSiteMode)(-1)` causes the attribute not to be emitted.</span></span>

<span data-ttu-id="aa365-157">Se ha cambiado el valor predeterminado de SameSite para la autenticación de formularios y las cookies de estado de sesión de `None` a `Lax`.</span><span class="sxs-lookup"><span data-stu-id="aa365-157">The default SameSite value for forms authentication and session state cookies was changed from `None` to `Lax`.</span></span>

### <a name="summary-of-change-impact-on-browsers"></a><span data-ttu-id="aa365-158">Resumen del impacto de los cambios en los exploradores</span><span class="sxs-lookup"><span data-stu-id="aa365-158">Summary of change impact on browsers</span></span>

<span data-ttu-id="aa365-159">Si instala la revisión y emite una cookie con `SameSite.None`, se producirá una de las dos acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="aa365-159">If you install the patch and issue a cookie with `SameSite.None`, one of two things will happen:</span></span>
* <span data-ttu-id="aa365-160">Chrome V80 tratará esta cookie según la nueva implementación y no aplicará las mismas restricciones de sitio en la cookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-160">Chrome v80 will treat this cookie according to the new implementation, and not enforce same site restrictions on the cookie.</span></span>
* <span data-ttu-id="aa365-161">Cualquier explorador que no se haya actualizado para admitir la nueva implementación seguirá la implementación anterior.</span><span class="sxs-lookup"><span data-stu-id="aa365-161">Any browser that has not been updated to support the new implementation will follow the old implementation.</span></span> <span data-ttu-id="aa365-162">La implementación anterior indica:</span><span class="sxs-lookup"><span data-stu-id="aa365-162">The old implementation says:</span></span>
  * <span data-ttu-id="aa365-163">Si ve un valor que no entiende, omítalo y cambie a las mismas restricciones de sitio estrictas.</span><span class="sxs-lookup"><span data-stu-id="aa365-163">If you see a value you don't understand, ignore it and switch to strict same site restrictions.</span></span>

<span data-ttu-id="aa365-164">Por lo tanto, la aplicación se interrumpe en Chrome o se interrumpe en muchos otros lugares.</span><span class="sxs-lookup"><span data-stu-id="aa365-164">So either the app breaks in Chrome, or you break in numerous other places.</span></span>

## <a name="history-and-changes"></a><span data-ttu-id="aa365-165">Historial y cambios</span><span class="sxs-lookup"><span data-stu-id="aa365-165">History and changes</span></span>

<span data-ttu-id="aa365-166">La compatibilidad con SameSite se implementó por primera vez en .NET 4.7.2 con el [estándar de borrador 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span><span class="sxs-lookup"><span data-stu-id="aa365-166">SameSite support was first implemented in .NET 4.7.2 using the [2016 draft standard](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).</span></span>

<span data-ttu-id="aa365-167">Las actualizaciones del 19 de noviembre de 2019 de Windows actualizaron .NET 4.7.2 + del estándar 2016 al estándar 2019.</span><span class="sxs-lookup"><span data-stu-id="aa365-167">The November 19, 2019 updates for Windows updated .NET 4.7.2+ from the 2016 standard to the 2019 standard.</span></span> <span data-ttu-id="aa365-168">Hay actualizaciones adicionales disponibles para otras versiones de Windows.</span><span class="sxs-lookup"><span data-stu-id="aa365-168">Additional updates are forthcoming for other versions of Windows.</span></span> <span data-ttu-id="aa365-169">Para obtener más información, consulta <xref:samesite/kbs-samesite>.</span><span class="sxs-lookup"><span data-stu-id="aa365-169">For more information, see <xref:samesite/kbs-samesite>.</span></span>

 <span data-ttu-id="aa365-170">El borrador 2019 de la especificación SameSite:</span><span class="sxs-lookup"><span data-stu-id="aa365-170">The 2019 draft of the SameSite specification:</span></span>

* <span data-ttu-id="aa365-171">**No** es compatible con las versiones anteriores del borrador 2016.</span><span class="sxs-lookup"><span data-stu-id="aa365-171">Is **not** backwards compatible with the 2016 draft.</span></span> <span data-ttu-id="aa365-172">Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-172">For more information, see [Supporting older browsers](#sob) in this document.</span></span>
* <span data-ttu-id="aa365-173">Especifica que las cookies se tratan como `SameSite=Lax` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="aa365-173">Specifies cookies are treated as `SameSite=Lax` by default.</span></span>
* <span data-ttu-id="aa365-174">Especifica las cookies que validan explícitamente `SameSite=None` para habilitar la entrega entre sitios también se debe marcar como `Secure`.</span><span class="sxs-lookup"><span data-stu-id="aa365-174">Specifies cookies that explicitly assert `SameSite=None` in order to enable cross-site delivery should also be marked as `Secure`.</span></span>
* <span data-ttu-id="aa365-175">Es compatible con las revisiones emitidas tal y como se describe en la lista anterior de KB.</span><span class="sxs-lookup"><span data-stu-id="aa365-175">Is supported by patches issued as described in the KB's listed above.</span></span>
* <span data-ttu-id="aa365-176">Está programado para que [Chrome](https://chromestatus.com/feature/5088147346030592) lo habilite de forma predeterminada en [febrero de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span><span class="sxs-lookup"><span data-stu-id="aa365-176">Is scheduled to be enabled by [Chrome](https://chromestatus.com/feature/5088147346030592) by default in [Feb 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html).</span></span> <span data-ttu-id="aa365-177">Los exploradores empezaron a pasar a este estándar en 2019.</span><span class="sxs-lookup"><span data-stu-id="aa365-177">Browsers started moving to this standard in 2019.</span></span>

<a name="known"><a/>

## <a name="known-issues"></a><span data-ttu-id="aa365-178">Problemas conocidos</span><span class="sxs-lookup"><span data-stu-id="aa365-178">Known Issues</span></span>

<span data-ttu-id="aa365-179">Dado que las especificaciones de borrador 2016 y 2019 no son compatibles, la actualización de .NET Framework de la versión de noviembre del 2019 presenta algunos cambios que pueden ser problemáticos.</span><span class="sxs-lookup"><span data-stu-id="aa365-179">Because the 2016 and 2019 draft specifications are not compatible, the November 2019 .Net Framework update introduces some changes that may be breaking.</span></span>

* <span data-ttu-id="aa365-180">Las cookies de estado de sesión y autenticación de formularios ahora se escriben en la red como `Lax` en lugar de sin especificar.</span><span class="sxs-lookup"><span data-stu-id="aa365-180">Session State and Forms Authentication cookies are now written to the network as `Lax` instead of unspecified.</span></span>
  * <span data-ttu-id="aa365-181">Aunque la mayoría de las aplicaciones funcionan con cookies `SameSite=Lax`, las aplicaciones que se PUBLICAn en sitios o aplicaciones que hacen uso de `iframe` pueden encontrar que el estado de sesión o las cookies de autorización de formularios no se usan según lo esperado.</span><span class="sxs-lookup"><span data-stu-id="aa365-181">While most apps work with `SameSite=Lax` cookies, apps that POST across sites or applications that make use of `iframe` may find that their session state or forms authorization cookies aren't being used as expected.</span></span> <span data-ttu-id="aa365-182">Para solucionarlo, cambie el valor de `cookieSameSite` en la sección de configuración correspondiente, tal y como se explicó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="aa365-182">To remedy this, change the `cookieSameSite` value in the appropriate configuration section as discussed previously.</span></span>
* <span data-ttu-id="aa365-183">HttpCookies (que establecen explícitamente `SameSite=None` en el código o en la configuración ahora tienen ese valor escrito con la cookie, mientras que antes se omitió.</span><span class="sxs-lookup"><span data-stu-id="aa365-183">HttpCookies that explicitly set `SameSite=None` in code or configuration now have that value written with the cookie, whereas it was previously omitted.</span></span> <span data-ttu-id="aa365-184">Esto puede producir problemas con exploradores más antiguos que solo admiten el estándar de borrador 2016.</span><span class="sxs-lookup"><span data-stu-id="aa365-184">This may cause issues with older browsers that only support the 2016 draft standard.</span></span>
  * <span data-ttu-id="aa365-185">Cuando los exploradores de destino admiten el estándar de borrador de 2019 con cookies `SameSite=None`, recuerde también marcarlos `Secure` o es posible que no se reconozcan.</span><span class="sxs-lookup"><span data-stu-id="aa365-185">When targeting browsers supporting the 2019 draft standard with `SameSite=None` cookies, remember to also mark them `Secure` or they may not be recognized.</span></span>
  * <span data-ttu-id="aa365-186">Para revertir al comportamiento 2016 de no escribir `SameSite=None`, use la configuración de la aplicación `aspnet:SupressSameSiteNone=true`.</span><span class="sxs-lookup"><span data-stu-id="aa365-186">To revert to the 2016 behavior of not writing `SameSite=None`, use the app setting `aspnet:SupressSameSiteNone=true`.</span></span> <span data-ttu-id="aa365-187">Tenga en cuenta que esto se aplicará a todos los httpCookies (de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aa365-187">Note that this will apply to all HttpCookies in the app.</span></span>

### <a name="azure-app-servicesamesite-cookie-handling"></a><span data-ttu-id="aa365-188">Azure App Service: control de cookies de SameSite</span><span class="sxs-lookup"><span data-stu-id="aa365-188">Azure App Service—SameSite cookie handling</span></span>

<span data-ttu-id="aa365-189">Consulte [Azure App Service: control de cookies de SameSite y .NET Framework revisión de 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) para obtener información sobre cómo Azure App Service configura los comportamientos de SameSite en las aplicaciones de .net 4.7.2.</span><span class="sxs-lookup"><span data-stu-id="aa365-189">See [Azure App Service—SameSite cookie handling and .NET Framework 4.7.2 patch](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) for information about how Azure App Service is configuring SameSite behaviors in .Net 4.7.2 apps.</span></span>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a><span data-ttu-id="aa365-190">Compatibilidad con exploradores más antiguos</span><span class="sxs-lookup"><span data-stu-id="aa365-190">Supporting older browsers</span></span>

<span data-ttu-id="aa365-191">El estándar 2016 SameSite impediba que los valores desconocidos se deben tratar como valores `SameSite=Strict`.</span><span class="sxs-lookup"><span data-stu-id="aa365-191">The 2016 SameSite standard mandated that unknown values must be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="aa365-192">Las aplicaciones a las que se accede desde exploradores más antiguos que admiten el estándar 2016 SameSite se pueden interrumpir cuando obtienen una propiedad SameSite con un valor de `None`.</span><span class="sxs-lookup"><span data-stu-id="aa365-192">Apps accessed from older browsers which support the 2016 SameSite standard may break when they get a SameSite property with a value of `None`.</span></span> <span data-ttu-id="aa365-193">Las aplicaciones web deben implementar la detección del explorador si pretenden admitir exploradores más antiguos.</span><span class="sxs-lookup"><span data-stu-id="aa365-193">Web apps must implement browser detection if they intend to support older browsers.</span></span> <span data-ttu-id="aa365-194">ASP.NET no implementa la detección del explorador porque los valores de los agentes de usuario son muy volátiles y cambian con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="aa365-194">ASP.NET doesn't implement browser detection because User-Agents values are highly volatile and change frequently.</span></span>

<span data-ttu-id="aa365-195">El enfoque de Microsoft para solucionar el problema es ayudarle a implementar los componentes de detección del explorador para quitar el `sameSite=None` atributo de las cookies si se sabe que un explorador no lo admite.</span><span class="sxs-lookup"><span data-stu-id="aa365-195">Microsoft's approach to fixing the problem is to help you implement browser detection components to strip the `sameSite=None` attribute from cookies if a browser is known to not support it.</span></span> <span data-ttu-id="aa365-196">El Consejo de Google fue emitir cookies dobles, una con el nuevo atributo y otra sin el atributo.</span><span class="sxs-lookup"><span data-stu-id="aa365-196">Google's advice was to issue double cookies, one with the new attribute, and one without the attribute at all.</span></span> <span data-ttu-id="aa365-197">Sin embargo, consideramos que el Consejo de Google está limitado.</span><span class="sxs-lookup"><span data-stu-id="aa365-197">However we consider Google's advice limited.</span></span> <span data-ttu-id="aa365-198">Algunos exploradores, especialmente los exploradores móviles, tienen límites muy pequeños en el número de cookies de un sitio o un nombre de dominio que puede enviar.</span><span class="sxs-lookup"><span data-stu-id="aa365-198">Some browsers, especially mobile browsers have very small limits on the number of cookies a site, or a domain name can send.</span></span> <span data-ttu-id="aa365-199">El envío de varias cookies, especialmente las cookies de gran tamaño, como las cookies de autenticación, puede llegar rápidamente al límite del explorador móvil, lo que provoca errores en la aplicación que son difíciles de diagnosticar y corregir.</span><span class="sxs-lookup"><span data-stu-id="aa365-199">Sending multiple cookies, especially large cookies like authentication cookies can reach the mobile browser limit very quickly, causing app failures that are hard to diagnose and fix.</span></span> <span data-ttu-id="aa365-200">Además, como un marco de trabajo hay un gran ecosistema de código y componentes de terceros que no se pueden actualizar para utilizar un enfoque de doble cookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-200">Furthermore as a framework there is a large ecosystem of third party code and components that may not be updated to use a double cookie approach.</span></span>

<span data-ttu-id="aa365-201">El código de detección de explorador usado en los proyectos de ejemplo de [este repositorio de github]() se encuentra en dos archivos.</span><span class="sxs-lookup"><span data-stu-id="aa365-201">The browser detection code used in the sample projects in [this GitHub repository]() is contained in two files</span></span>

* [<span data-ttu-id="aa365-202">C#SameSiteSupport.cs</span><span class="sxs-lookup"><span data-stu-id="aa365-202">C# SameSiteSupport.cs</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.cs)
* [<span data-ttu-id="aa365-203">VB SameSiteSupport. VB</span><span class="sxs-lookup"><span data-stu-id="aa365-203">VB SameSiteSupport.vb</span></span>](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/SameSiteSupport.vb)

<span data-ttu-id="aa365-204">Estas detecciones son los agentes de explorador más comunes que se han detectado y admiten el estándar 2016 y para los que es necesario quitar por completo el atributo.</span><span class="sxs-lookup"><span data-stu-id="aa365-204">These detections are the most common browser agents we have seen that support the 2016 standard and for which the attribute needs to be completely removed.</span></span> <span data-ttu-id="aa365-205">No se ha diseñado como una implementación completa:</span><span class="sxs-lookup"><span data-stu-id="aa365-205">It isn't meant as a complete implementation:</span></span>

* <span data-ttu-id="aa365-206">Es posible que la aplicación vea exploradores que no son de su sitio de prueba.</span><span class="sxs-lookup"><span data-stu-id="aa365-206">Your app may see browsers that our test sites do not.</span></span>
* <span data-ttu-id="aa365-207">Debe estar preparado para agregar detecciones según sea necesario para su entorno.</span><span class="sxs-lookup"><span data-stu-id="aa365-207">You should be prepared to add detections as necessary for your environment.</span></span>

<span data-ttu-id="aa365-208">La forma de conectar la detección varía en función de la versión de .NET y el marco web que esté usando.</span><span class="sxs-lookup"><span data-stu-id="aa365-208">How you wire up the detection varies according the version of .NET and the web framework that you are using.</span></span> <span data-ttu-id="aa365-209">Se puede llamar al siguiente código en el sitio de la llamada a [HttpCookie](/dotnet/api/system.web.httpcookie) :</span><span class="sxs-lookup"><span data-stu-id="aa365-209">The following code can be called at the [HttpCookie](/dotnet/api/system.web.httpcookie) call site:</span></span>

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

<span data-ttu-id="aa365-210">Vea los siguientes temas de ASP.NET 4.7.2 SameSite cookie:</span><span class="sxs-lookup"><span data-stu-id="aa365-210">See the following ASP.NET 4.7.2 SameSite cookie topics:</span></span>

* [<span data-ttu-id="aa365-211">C#MVC</span><span class="sxs-lookup"><span data-stu-id="aa365-211">C# MVC</span></span>](xref:samesite/csMVC)
* [<span data-ttu-id="aa365-212">C#WebForms</span><span class="sxs-lookup"><span data-stu-id="aa365-212">C# WebForms</span></span>](xref:samesite/CSharpWebForms)
* [<span data-ttu-id="aa365-213">WebForms de VB</span><span class="sxs-lookup"><span data-stu-id="aa365-213">VB WebForms</span></span>](xref:samesite/vbWF)
* [<span data-ttu-id="aa365-214">MVC DE VB</span><span class="sxs-lookup"><span data-stu-id="aa365-214">VB MVC</span></span>](xref:samesite/vbMVC)
<!--
* <xref:samesite/csMVC>
* <xref:samesite/CSharpWebForms>
* <xref:samesite/vbWF>
* <xref:samesite/vbMVC>
-->

### <a name="ensuring-your-site-redirects-to-https"></a><span data-ttu-id="aa365-215">Asegurarse de que el sitio se redirige a HTTPS</span><span class="sxs-lookup"><span data-stu-id="aa365-215">Ensuring your site redirects to HTTPS</span></span>

<span data-ttu-id="aa365-216">En el caso de ASP.NET 4. x, WebForms y MVC, [la característica de reescritura de direcciones URL de IIS](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) se puede usar para redirigir todas las solicitudes a https.</span><span class="sxs-lookup"><span data-stu-id="aa365-216">For ASP.NET 4.x, WebForms and MVC, [IIS's URL Rewrite](/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module) feature can be used to redirect all requests to HTTPS.</span></span> <span data-ttu-id="aa365-217">El siguiente código XML muestra una regla de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="aa365-217">The following XML shows a sample rule:</span></span>

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

<span data-ttu-id="aa365-218">En las instalaciones locales de [reescritura de direcciones URL de IIS](https://www.iis.net/downloads/microsoft/url-rewrite) es una característica opcional que puede necesitar instalar.</span><span class="sxs-lookup"><span data-stu-id="aa365-218">In on-premises installations of [IIS URL Rewrite](https://www.iis.net/downloads/microsoft/url-rewrite) is an optional feature that may need installing.</span></span>

## <a name="test-apps-for-samesite-problems"></a><span data-ttu-id="aa365-219">Probar aplicaciones para problemas de SameSite</span><span class="sxs-lookup"><span data-stu-id="aa365-219">Test apps for SameSite problems</span></span>

<span data-ttu-id="aa365-220">Debe probar la aplicación con los exploradores que admite y revisar los escenarios que implican cookies.</span><span class="sxs-lookup"><span data-stu-id="aa365-220">You must test your app with the browsers you support and go through your scenarios that involve cookies.</span></span> <span data-ttu-id="aa365-221">Normalmente, los escenarios de cookies implican</span><span class="sxs-lookup"><span data-stu-id="aa365-221">Cookie scenarios typically involve</span></span>

* <span data-ttu-id="aa365-222">Formularios de inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="aa365-222">Login forms</span></span>
* <span data-ttu-id="aa365-223">Mecanismos de inicio de sesión externos como Facebook, Azure AD, OAuth y OIDC</span><span class="sxs-lookup"><span data-stu-id="aa365-223">External login mechanisms such as Facebook, Azure AD, OAuth and OIDC</span></span>
* <span data-ttu-id="aa365-224">Páginas que aceptan solicitudes de otros sitios</span><span class="sxs-lookup"><span data-stu-id="aa365-224">Pages that accept requests from other sites</span></span>
* <span data-ttu-id="aa365-225">Páginas de la aplicación diseñadas para insertarse en iframes</span><span class="sxs-lookup"><span data-stu-id="aa365-225">Pages in your app designed to be embedded in iframes</span></span>

<span data-ttu-id="aa365-226">Debe comprobar que las cookies se crean, se conservan y se eliminan correctamente en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="aa365-226">You should check that cookies are created, persisted and deleted correctly in your app.</span></span>

<span data-ttu-id="aa365-227">Las aplicaciones que interactúan con sitios remotos, como a través de un inicio de sesión de terceros, deben:</span><span class="sxs-lookup"><span data-stu-id="aa365-227">Apps that interact with remote sites such as through third-party login need to:</span></span>

* <span data-ttu-id="aa365-228">Pruebe la interacción en varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="aa365-228">Test the interaction on multiple browsers.</span></span>
* <span data-ttu-id="aa365-229">Aplique la [detección y la mitigación del explorador](#sob) que se describen en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-229">Apply the [browser detection and mitigation](#sob) discussed in this document.</span></span>

<span data-ttu-id="aa365-230">Pruebe las aplicaciones web con una versión de cliente que pueda participar en el nuevo comportamiento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="aa365-230">Test web apps using a client version that can opt-in to the new SameSite behavior.</span></span> <span data-ttu-id="aa365-231">Chrome, Firefox y el borde de cromo tienen nuevas marcas de características opcionales que se pueden usar para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="aa365-231">Chrome, Firefox, and Chromium Edge all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="aa365-232">Una vez que la aplicación aplica las revisiones de SameSite, probarla con versiones de cliente anteriores, especialmente Safari.</span><span class="sxs-lookup"><span data-stu-id="aa365-232">After your app applies the SameSite patches, test it with older client versions, especially Safari.</span></span> <span data-ttu-id="aa365-233">Para obtener más información, consulte [compatibilidad con exploradores anteriores](#sob) en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-233">For more information, see [Supporting older browsers](#sob) in this document.</span></span>

### <a name="test-with-chrome"></a><span data-ttu-id="aa365-234">Prueba con Chrome</span><span class="sxs-lookup"><span data-stu-id="aa365-234">Test with Chrome</span></span>

<span data-ttu-id="aa365-235">Chrome 78 + ofrece resultados engañosos porque tiene una mitigación temporal en contexto.</span><span class="sxs-lookup"><span data-stu-id="aa365-235">Chrome 78+ gives misleading results because it has a temporary mitigation in place.</span></span> <span data-ttu-id="aa365-236">La mitigación temporal de Chrome 78 + permite cookies con menos de dos minutos de antigüedad.</span><span class="sxs-lookup"><span data-stu-id="aa365-236">The Chrome 78+ temporary mitigation allows cookies less than two minutes old.</span></span> <span data-ttu-id="aa365-237">Chrome 76 o 77 con las marcas de prueba adecuadas habilitadas proporcionan resultados más precisos.</span><span class="sxs-lookup"><span data-stu-id="aa365-237">Chrome 76 or 77 with the appropriate test flags enabled provides more accurate results.</span></span> <span data-ttu-id="aa365-238">Para probar el nuevo comportamiento de SameSite, cambie `chrome://flags/#same-site-by-default-cookies` a **habilitado**.</span><span class="sxs-lookup"><span data-stu-id="aa365-238">To test the new SameSite behavior toggle `chrome://flags/#same-site-by-default-cookies` to **Enabled**.</span></span> <span data-ttu-id="aa365-239">Las versiones anteriores de Chrome (75 e inferiores) se muestran como erróneas con el nuevo valor de `None`.</span><span class="sxs-lookup"><span data-stu-id="aa365-239">Older versions of Chrome (75 and below) are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="aa365-240">Consulte [compatibilidad con exploradores anteriores](#sob) en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-240">See [Supporting older browsers](#sob) in this document.</span></span>

<span data-ttu-id="aa365-241">Google no hace que las versiones anteriores de Chrome estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="aa365-241">Google does not make older chrome versions available.</span></span> <span data-ttu-id="aa365-242">Siga las instrucciones de [Descargar cromo](https://www.chromium.org/getting-involved/download-chromium) para probar versiones anteriores de Chrome.</span><span class="sxs-lookup"><span data-stu-id="aa365-242">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium) to test older versions of Chrome.</span></span> <span data-ttu-id="aa365-243">**No** Descargue Chrome desde los vínculos que se proporcionan al buscar versiones anteriores de Chrome.</span><span class="sxs-lookup"><span data-stu-id="aa365-243">Do **not** download Chrome from links provided by searching for older versions of chrome.</span></span>

* [<span data-ttu-id="aa365-244">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="aa365-244">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="aa365-245">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="aa365-245">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)
* <span data-ttu-id="aa365-246">Si no está usando una versión de 64 bits de Windows, puede usar el [visor de OmahaProxy](https://omahaproxy.appspot.com/) para buscar la rama de cromo que corresponde a Chrome 74 (v 74.0.3729.108) mediante las [instrucciones proporcionadas por cromo](https://www.chromium.org/getting-involved/download-chromium).</span><span class="sxs-lookup"><span data-stu-id="aa365-246">If you're not using a 64bit version of Windows you can use the [OmahaProxy viewer](https://omahaproxy.appspot.com/) to look up which Chromium branch corresponds to Chrome 74 (v74.0.3729.108) using the [instructions provided by Chromium](https://www.chromium.org/getting-involved/download-chromium).</span></span>

<span data-ttu-id="aa365-247">A partir de la versión Canarias `80.0.3975.0`, se puede deshabilitar la mitigación temporal de LAX + POST con el fin de realizar pruebas mediante la nueva marca `--enable-features=SameSiteDefaultChecksMethodRigorously` para permitir la realización de pruebas de sitios y servicios en el estado final eventual de la característica en la que se ha quitado la mitigación.</span><span class="sxs-lookup"><span data-stu-id="aa365-247">Starting in Canary version `80.0.3975.0`, the Lax+POST temporary mitigation can be disabled for testing purposes using the new flag `--enable-features=SameSiteDefaultChecksMethodRigorously` to allow testing of sites and services in the eventual end state of the feature where the mitigation has been removed.</span></span> <span data-ttu-id="aa365-248">Para obtener más información, consulte las actualizaciones de los proyectos de cromo [SameSite](https://www.chromium.org/updates/same-site)</span><span class="sxs-lookup"><span data-stu-id="aa365-248">For more information, see The Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)</span></span>

#### <a name="test-with-chrome-80"></a><span data-ttu-id="aa365-249">Prueba con Chrome 80 +</span><span class="sxs-lookup"><span data-stu-id="aa365-249">Test with Chrome 80+</span></span>

<span data-ttu-id="aa365-250">[Descargue](https://www.google.com/chrome/) una versión de Chrome que admita su nuevo atributo.</span><span class="sxs-lookup"><span data-stu-id="aa365-250">[Download](https://www.google.com/chrome/) a version of Chrome that supports their new attribute.</span></span> <span data-ttu-id="aa365-251">En el momento de escribir este documento, la versión actual es Chrome 80.</span><span class="sxs-lookup"><span data-stu-id="aa365-251">At the time of writing, the current version is Chrome 80.</span></span> <span data-ttu-id="aa365-252">Chrome 80 necesita que la marca `chrome://flags/#same-site-by-default-cookies` habilitada para usar el nuevo comportamiento.</span><span class="sxs-lookup"><span data-stu-id="aa365-252">Chrome 80 needs the flag `chrome://flags/#same-site-by-default-cookies` enabled to use the new behavior.</span></span> <span data-ttu-id="aa365-253">También debe habilitar (`chrome://flags/#cookies-without-same-site-must-be-secure`) para probar el próximo comportamiento de las cookies que no tienen ningún atributo sameSite habilitado.</span><span class="sxs-lookup"><span data-stu-id="aa365-253">You should also enable (`chrome://flags/#cookies-without-same-site-must-be-secure`) to test the upcoming behavior for cookies which have no sameSite attribute enabled.</span></span> <span data-ttu-id="aa365-254">Chrome 80 está en el destino para que el modificador pueda tratar las cookies sin el atributo como `SameSite=Lax`, aunque con un período de gracia temporal para ciertas solicitudes.</span><span class="sxs-lookup"><span data-stu-id="aa365-254">Chrome 80 is on target to make the switch to treat cookies without the attribute as `SameSite=Lax`, albeit with a timed grace period for certain requests.</span></span> <span data-ttu-id="aa365-255">Para deshabilitar el período de gracia tiempo, se puede iniciar Chrome 80 con el siguiente argumento de línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="aa365-255">To disable the timed grace period Chrome 80 can be launched with the following command line argument:</span></span>

`--enable-features=SameSiteDefaultChecksMethodRigorously`

<span data-ttu-id="aa365-256">Chrome 80 tiene mensajes de advertencia en la consola del explorador sobre los atributos sameSite que faltan.</span><span class="sxs-lookup"><span data-stu-id="aa365-256">Chrome 80 has warning messages in the browser console about missing sameSite attributes.</span></span> <span data-ttu-id="aa365-257">Use F12 para abrir la consola del explorador.</span><span class="sxs-lookup"><span data-stu-id="aa365-257">Use F12 to open the browser console.</span></span>

### <a name="test-with-safari"></a><span data-ttu-id="aa365-258">Prueba con Safari</span><span class="sxs-lookup"><span data-stu-id="aa365-258">Test with Safari</span></span>

<span data-ttu-id="aa365-259">Safari 12 implementó estrictamente el borrador anterior y produce un error cuando el nuevo valor de `None` está en una cookie.</span><span class="sxs-lookup"><span data-stu-id="aa365-259">Safari 12 strictly implemented the prior draft and fails when the new `None` value is in a cookie.</span></span> <span data-ttu-id="aa365-260">`None` se evita a través del código de detección del explorador que [admite exploradores anteriores](#sob) en este documento.</span><span class="sxs-lookup"><span data-stu-id="aa365-260">`None` is avoided via the browser detection code [Supporting older browsers](#sob) in this document.</span></span> <span data-ttu-id="aa365-261">Pruebe los inicios de sesión de estilo de SO basados en Safari 12, Safari 13 y WebKit con MSAL, ADAL o cualquier biblioteca que use.</span><span class="sxs-lookup"><span data-stu-id="aa365-261">Test Safari 12, Safari 13, and WebKit based OS style logins using MSAL, ADAL or whatever library you are using.</span></span> <span data-ttu-id="aa365-262">El problema depende de la versión del sistema operativo subyacente.</span><span class="sxs-lookup"><span data-stu-id="aa365-262">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="aa365-263">OSX Mojave (10,14) e iOS 12 se sabe que tienen problemas de compatibilidad con el nuevo comportamiento de SameSite.</span><span class="sxs-lookup"><span data-stu-id="aa365-263">OSX Mojave (10.14) and iOS 12 are known to have compatibility problems with the new SameSite behavior.</span></span> <span data-ttu-id="aa365-264">Al actualizar el sistema operativo a OSX Catalina (10,15) o iOS 13 se corrige el problema.</span><span class="sxs-lookup"><span data-stu-id="aa365-264">Upgrading the OS to OSX Catalina (10.15) or iOS 13 fixes the problem.</span></span> <span data-ttu-id="aa365-265">Safari no tiene actualmente una marca de participación para probar el nuevo comportamiento de la especificación.</span><span class="sxs-lookup"><span data-stu-id="aa365-265">Safari does not currently have an opt-in flag for testing the new spec behavior.</span></span>

### <a name="test-with-firefox"></a><span data-ttu-id="aa365-266">Prueba con Firefox</span><span class="sxs-lookup"><span data-stu-id="aa365-266">Test with Firefox</span></span>

<span data-ttu-id="aa365-267">La compatibilidad con Firefox para el nuevo estándar se puede probar en la versión 68 + al optar por la `about:config` página con la marca de características `network.cookie.sameSite.laxByDefault`.</span><span class="sxs-lookup"><span data-stu-id="aa365-267">Firefox support for the new standard can be tested on version 68+ by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="aa365-268">No ha habido informes de problemas de compatibilidad con versiones anteriores de Firefox.</span><span class="sxs-lookup"><span data-stu-id="aa365-268">There haven't been reports of compatibility issues with older versions of Firefox.</span></span>

### <a name="test-with-edge-legacy-browser"></a><span data-ttu-id="aa365-269">Prueba con el explorador perimetral (heredado)</span><span class="sxs-lookup"><span data-stu-id="aa365-269">Test with Edge (Legacy) browser</span></span>

<span data-ttu-id="aa365-270">Edge es compatible con el estándar SameSite antiguo.</span><span class="sxs-lookup"><span data-stu-id="aa365-270">Edge supports the old SameSite standard.</span></span> <span data-ttu-id="aa365-271">La versión de Edge 44 + no tiene ningún problema de compatibilidad conocido con el nuevo estándar.</span><span class="sxs-lookup"><span data-stu-id="aa365-271">Edge version 44+ doesn't have any known compatibility problems with the new standard.</span></span>

### <a name="test-with-edge-chromium"></a><span data-ttu-id="aa365-272">Prueba con borde (cromo)</span><span class="sxs-lookup"><span data-stu-id="aa365-272">Test with Edge (Chromium)</span></span>

<span data-ttu-id="aa365-273">Las marcas SameSite se establecen en la página `edge://flags/#same-site-by-default-cookies`.</span><span class="sxs-lookup"><span data-stu-id="aa365-273">SameSite flags are set on the `edge://flags/#same-site-by-default-cookies` page.</span></span> <span data-ttu-id="aa365-274">No se detectaron problemas de compatibilidad con el cromo perimetral.</span><span class="sxs-lookup"><span data-stu-id="aa365-274">No compatibility issues were discovered with Edge Chromium.</span></span>

### <a name="test-with-electron"></a><span data-ttu-id="aa365-275">Prueba con electrones</span><span class="sxs-lookup"><span data-stu-id="aa365-275">Test with Electron</span></span>

<span data-ttu-id="aa365-276">Las versiones de Electron incluyen versiones anteriores de Chromium.</span><span class="sxs-lookup"><span data-stu-id="aa365-276">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="aa365-277">Por ejemplo, la versión de electrones utilizada por los equipos es cromo 66, que exhibe el comportamiento anterior.</span><span class="sxs-lookup"><span data-stu-id="aa365-277">For example, the version of Electron used by Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="aa365-278">Debe realizar sus propias pruebas de compatibilidad con la versión de electrones que usa el producto.</span><span class="sxs-lookup"><span data-stu-id="aa365-278">You must perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="aa365-279">Consulte [compatibilidad con exploradores más antiguos](#sob).</span><span class="sxs-lookup"><span data-stu-id="aa365-279">See [Supporting older browsers](#sob).</span></span>

## <a name="reverting-samesite-patches"></a><span data-ttu-id="aa365-280">Revertir revisiones de SameSite</span><span class="sxs-lookup"><span data-stu-id="aa365-280">Reverting SameSite patches</span></span>

<span data-ttu-id="aa365-281">Puede revertir el comportamiento actualizado de sameSite en .NET Framework aplicaciones a su comportamiento anterior, donde no se emite el atributo sameSite para un valor de `None`y revertir las cookies de autenticación y sesión para que no emitan el valor.</span><span class="sxs-lookup"><span data-stu-id="aa365-281">You can revert the updated sameSite behavior in .NET Framework apps to its previous behavior where the sameSite attribute is not emitted for a value of `None`, and revert the authentication and session cookies to not emit the value.</span></span> <span data-ttu-id="aa365-282">Esto se debe ver como una *solución extremadamente temporal*, ya que los cambios de cromo interrumpirán cualquier solicitud post externa o la autenticación para los usuarios que usen exploradores que admitan los cambios en el estándar.</span><span class="sxs-lookup"><span data-stu-id="aa365-282">This should be viewed as an *extremely temporary fix*, as the Chrome changes will break any external POST requests or authentication for users using browsers which support the changes to the standard.</span></span>

### <a name="reverting-net-472-behavior"></a><span data-ttu-id="aa365-283">Reversión del comportamiento de 4.7.2 de .NET</span><span class="sxs-lookup"><span data-stu-id="aa365-283">Reverting .NET 4.7.2 behavior</span></span>

<span data-ttu-id="aa365-284">Actualice el *archivo Web. config* para incluir los siguientes valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="aa365-284">Update *web.config* to include the following configuration settings:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="aa365-285">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="aa365-285">Additional resources</span></span>

* [<span data-ttu-id="aa365-286">Próximos cambios en la cookie de SameSite en ASP.NET y ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa365-286">Upcoming SameSite Cookie Changes in ASP.NET and ASP.NET Core</span></span>](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [<span data-ttu-id="aa365-287">Sugerencias para probar y depurar SameSite-de forma predeterminada y "SameSite = None; Proteger las cookies</span><span class="sxs-lookup"><span data-stu-id="aa365-287">Tips for testing and debugging SameSite-by-default and “SameSite=None; Secure” cookies</span></span>](https://www.chromium.org/updates/same-site/test-debug)
* [<span data-ttu-id="aa365-288">Blog de cromo: desarrolladores: Prepárese para New SameSite = None; Configuración de cookies seguras</span><span class="sxs-lookup"><span data-stu-id="aa365-288">Chromium Blog:Developers: Get Ready for New SameSite=None; Secure Cookie Settings</span></span>](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [<span data-ttu-id="aa365-289">Explicación de las cookies de SameSite</span><span class="sxs-lookup"><span data-stu-id="aa365-289">SameSite cookies explained</span></span>](https://web.dev/samesite-cookies-explained/)
* [<span data-ttu-id="aa365-290">Actualizaciones de Chrome</span><span class="sxs-lookup"><span data-stu-id="aa365-290">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)
* [<span data-ttu-id="aa365-291">Revisiones de .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="aa365-291">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)
* [<span data-ttu-id="aa365-292">Información del mismo sitio de aplicaciones Web de Azure</span><span class="sxs-lookup"><span data-stu-id="aa365-292">Azure Web Applications Same Site Information</span></span>](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/)
* [<span data-ttu-id="aa365-293">Información del mismo sitio de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa365-293">Azure ActiveDirectory Same Site Information</span></span>](/azure/active-directory/develop/howto-handle-samesite-cookie-changes-chrome-browser)
