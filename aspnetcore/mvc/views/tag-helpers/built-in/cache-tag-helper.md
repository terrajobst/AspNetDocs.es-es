---
title: Asistente de etiquetas de caché en ASP.NET Core MVC
author: pkellner
description: Obtenga información sobre cómo usar el asistente de etiquetas de caché.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57060102"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="fd044-103">Asistente de etiquetas de caché en ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="fd044-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="fd044-104">Por [Peter Kellner](http://peterkellner.net) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fd044-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span> 

<span data-ttu-id="fd044-105">El asistente de etiquetas de caché proporciona la capacidad para mejorar el rendimiento de la aplicación de ASP.NET Core al permitir almacenar en memoria caché su contenido en el proveedor de caché interno de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fd044-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="fd044-106">Para obtener información general sobre asistentes de etiquetas, vea <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="fd044-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="fd044-107">Este marcado de Razor almacena en caché la fecha actual:</span><span class="sxs-lookup"><span data-stu-id="fd044-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="fd044-108">La primera solicitud a la página que contiene el asistente de etiquetas muestra la fecha actual.</span><span class="sxs-lookup"><span data-stu-id="fd044-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="fd044-109">Las solicitudes adicionales muestran el valor almacenado en caché hasta que la memoria caché expira (el valor predeterminado es 20 minutos) o hasta que la fecha almacenada en caché se expulsa de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="fd044-110">Atributos del asistente de etiqueta de caché</span><span class="sxs-lookup"><span data-stu-id="fd044-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="fd044-111">enabled</span><span class="sxs-lookup"><span data-stu-id="fd044-111">enabled</span></span>

| <span data-ttu-id="fd044-112">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-112">Attribute Type</span></span>  | <span data-ttu-id="fd044-113">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-113">Examples</span></span>        | <span data-ttu-id="fd044-114">Default</span><span class="sxs-lookup"><span data-stu-id="fd044-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="fd044-115">Booleano</span><span class="sxs-lookup"><span data-stu-id="fd044-115">Boolean</span></span>         | <span data-ttu-id="fd044-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="fd044-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="fd044-117">`enabled` determina si se almacena en caché el contenido incluido por el asistente de etiquetas de caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="fd044-118">De manera predeterminada, es `true`.</span><span class="sxs-lookup"><span data-stu-id="fd044-118">The default is `true`.</span></span> <span data-ttu-id="fd044-119">Si establece en `false`, el resultado representado **no** se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="fd044-120">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd044-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="fd044-121">expires-on</span><span class="sxs-lookup"><span data-stu-id="fd044-121">expires-on</span></span>

| <span data-ttu-id="fd044-122">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-122">Attribute Type</span></span>   | <span data-ttu-id="fd044-123">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd044-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="fd044-124">`expires-on` establece una fecha de expiración absoluta para el elemento almacenado en caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="fd044-125">En este ejemplo se almacena en caché el contenido del asistente de etiquetas de caché hasta las 17:02 del 29 de enero de 2025.</span><span class="sxs-lookup"><span data-stu-id="fd044-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="fd044-126">expires-after</span><span class="sxs-lookup"><span data-stu-id="fd044-126">expires-after</span></span>

| <span data-ttu-id="fd044-127">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-127">Attribute Type</span></span> | <span data-ttu-id="fd044-128">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd044-128">Example</span></span>                      | <span data-ttu-id="fd044-129">Default</span><span class="sxs-lookup"><span data-stu-id="fd044-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="fd044-130">20 minutos</span><span class="sxs-lookup"><span data-stu-id="fd044-130">20 minutes</span></span> |

<span data-ttu-id="fd044-131">`expires-after` establece el período de tiempo desde el momento de la primera solicitud para almacenar en caché el contenido.</span><span class="sxs-lookup"><span data-stu-id="fd044-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="fd044-132">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd044-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="fd044-133">El motor de vistas de Razor establece el valor predeterminado `expires-after` en veinte minutos.</span><span class="sxs-lookup"><span data-stu-id="fd044-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="fd044-134">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="fd044-134">expires-sliding</span></span>

| <span data-ttu-id="fd044-135">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-135">Attribute Type</span></span> | <span data-ttu-id="fd044-136">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd044-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="fd044-137">Establece la hora en que se debe expulsar una entrada de caché si no se ha accedido a su valor.</span><span class="sxs-lookup"><span data-stu-id="fd044-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="fd044-138">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd044-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="fd044-139">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="fd044-139">vary-by-header</span></span>

| <span data-ttu-id="fd044-140">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-140">Attribute Type</span></span> | <span data-ttu-id="fd044-141">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="fd044-142">String</span><span class="sxs-lookup"><span data-stu-id="fd044-142">String</span></span>         | <span data-ttu-id="fd044-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="fd044-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="fd044-144">`vary-by-header` acepta una lista de los valores de encabezado separados por comas que desencadenan una actualización de la caché cuando cambian.</span><span class="sxs-lookup"><span data-stu-id="fd044-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="fd044-145">En el ejemplo siguiente se supervisa el valor del encabezado `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="fd044-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="fd044-146">En el ejemplo se almacena en caché el contenido de cada `User-Agent` diferente que se presenta al servidor web:</span><span class="sxs-lookup"><span data-stu-id="fd044-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="fd044-147">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="fd044-147">vary-by-query</span></span>

| <span data-ttu-id="fd044-148">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-148">Attribute Type</span></span> | <span data-ttu-id="fd044-149">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="fd044-150">String</span><span class="sxs-lookup"><span data-stu-id="fd044-150">String</span></span>         | <span data-ttu-id="fd044-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="fd044-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="fd044-152">`vary-by-query` acepta una lista de valores separados por comas de <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> en una cadena de consulta (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) que desencadenan una actualización de la caché cuando cambia el valor de cualquiera de las claves.</span><span class="sxs-lookup"><span data-stu-id="fd044-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="fd044-153">En este ejemplo se supervisan los valores de `Make` y `Model`.</span><span class="sxs-lookup"><span data-stu-id="fd044-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="fd044-154">En el ejemplo se almacena en caché el contenido de cada `Make` y `Model`diferente que se presenta al servidor web:</span><span class="sxs-lookup"><span data-stu-id="fd044-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="fd044-155">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="fd044-155">vary-by-route</span></span>

| <span data-ttu-id="fd044-156">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-156">Attribute Type</span></span> | <span data-ttu-id="fd044-157">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="fd044-158">String</span><span class="sxs-lookup"><span data-stu-id="fd044-158">String</span></span>         | <span data-ttu-id="fd044-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="fd044-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="fd044-160">`vary-by-route` acepta una lista delimitada por comas de nombres de parámetros de ruta que desencadenan una actualización de la caché cuando el valor del parámetro de datos de ruta cambia.</span><span class="sxs-lookup"><span data-stu-id="fd044-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="fd044-161">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd044-161">Example:</span></span>

<span data-ttu-id="fd044-162">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="fd044-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="fd044-163">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fd044-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="fd044-164">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="fd044-164">vary-by-cookie</span></span>

| <span data-ttu-id="fd044-165">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-165">Attribute Type</span></span> | <span data-ttu-id="fd044-166">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="fd044-167">String</span><span class="sxs-lookup"><span data-stu-id="fd044-167">String</span></span>         | <span data-ttu-id="fd044-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="fd044-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="fd044-169">`vary-by-cookie` acepta una lista delimitada por comas de nombres de cookies que desencadenan una actualización de la caché cuando los valores de las cookies cambian.</span><span class="sxs-lookup"><span data-stu-id="fd044-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="fd044-170">En este ejemplo se supervisa la cookie asociada con ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="fd044-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="fd044-171">Cuando se autentica un usuario, un cambio en la cookie de identidad desencadena una actualización de caché:</span><span class="sxs-lookup"><span data-stu-id="fd044-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="fd044-172">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="fd044-172">vary-by-user</span></span>

| <span data-ttu-id="fd044-173">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-173">Attribute Type</span></span>  | <span data-ttu-id="fd044-174">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-174">Examples</span></span>        | <span data-ttu-id="fd044-175">Default</span><span class="sxs-lookup"><span data-stu-id="fd044-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="fd044-176">Booleano</span><span class="sxs-lookup"><span data-stu-id="fd044-176">Boolean</span></span>         | <span data-ttu-id="fd044-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="fd044-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="fd044-178">`vary-by-user` especifica la memoria caché se restablece o no cuando cambia el usuario que ha iniciado la sesión (o la entidad de seguridad del contexto).</span><span class="sxs-lookup"><span data-stu-id="fd044-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="fd044-179">El usuario actual también se conoce como entidad de seguridad del contexto de solicitud y puede verse en una vista Razor mediante una referencia a `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="fd044-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="fd044-180">En este ejemplo se supervisa el usuario actual que ha iniciado sesión para desencadenar una actualización de caché:</span><span class="sxs-lookup"><span data-stu-id="fd044-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="fd044-181">Con este atributo, se mantiene el contenido en caché a través de un ciclo de inicio y cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="fd044-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="fd044-182">Cuando el valor se establece en `true`, un ciclo de autenticación invalida la memoria caché para el usuario autenticado.</span><span class="sxs-lookup"><span data-stu-id="fd044-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="fd044-183">Se invalida la memoria caché porque se genera un nuevo valor único de cookie cuando se autentica un usuario.</span><span class="sxs-lookup"><span data-stu-id="fd044-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="fd044-184">Se mantiene la memoria caché para el estado anónimo cuando no se presenta ninguna cookie o la cookie ha expirado.</span><span class="sxs-lookup"><span data-stu-id="fd044-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="fd044-185">Si **no** se autentica el usuario, se mantiene la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="fd044-186">vary-by</span><span class="sxs-lookup"><span data-stu-id="fd044-186">vary-by</span></span>

| <span data-ttu-id="fd044-187">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-187">Attribute Type</span></span> | <span data-ttu-id="fd044-188">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="fd044-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="fd044-189">String</span><span class="sxs-lookup"><span data-stu-id="fd044-189">String</span></span>         | `@Model` |

<span data-ttu-id="fd044-190">`vary-by` permite personalizar qué datos se almacenan en caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="fd044-191">Cuando el objeto al que hace referencia el valor de cadena del atributo cambia, el contenido del asistente de etiqueta de caché se actualiza.</span><span class="sxs-lookup"><span data-stu-id="fd044-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="fd044-192">A menudo se asignan a este atributo una concatenación de cadenas de valores del modelo.</span><span class="sxs-lookup"><span data-stu-id="fd044-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="fd044-193">De hecho, esto provoca una situación en la que una actualización de cualquiera de los valores concatenados invalida la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="fd044-194">En este ejemplo se supone que el método de controlador que representa la vista suma el valor del entero de los dos parámetros de ruta, `myParam1` y `myParam2`, y devuelve el resultado de la suma como la propiedad de modelo simple.</span><span class="sxs-lookup"><span data-stu-id="fd044-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="fd044-195">Cuando se cambia esta suma, el contenido del asistente de etiqueta de caché se representa y almacena en caché de nuevo.</span><span class="sxs-lookup"><span data-stu-id="fd044-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="fd044-196">Acción:</span><span class="sxs-lookup"><span data-stu-id="fd044-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="fd044-197">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="fd044-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="fd044-198">priority</span><span class="sxs-lookup"><span data-stu-id="fd044-198">priority</span></span>

| <span data-ttu-id="fd044-199">Tipo de atributo</span><span class="sxs-lookup"><span data-stu-id="fd044-199">Attribute Type</span></span>      | <span data-ttu-id="fd044-200">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="fd044-200">Examples</span></span>                               | <span data-ttu-id="fd044-201">Default</span><span class="sxs-lookup"><span data-stu-id="fd044-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="fd044-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="fd044-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="fd044-203">`priority` proporciona instrucciones de expulsión de caché para el proveedor de caché integrado.</span><span class="sxs-lookup"><span data-stu-id="fd044-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="fd044-204">El servidor web expulsa primero las entradas de caché `Low` cuando está bajo presión de memoria.</span><span class="sxs-lookup"><span data-stu-id="fd044-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="fd044-205">Ejemplo:</span><span class="sxs-lookup"><span data-stu-id="fd044-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="fd044-206">El atributo `priority` no garantiza un nivel específico de retención de la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="fd044-207">`CacheItemPriority` es solo una sugerencia.</span><span class="sxs-lookup"><span data-stu-id="fd044-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="fd044-208">Establecer este atributo en `NeverRemove` no garantiza que siempre se conserven los elementos en la memoria caché.</span><span class="sxs-lookup"><span data-stu-id="fd044-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="fd044-209">Para más información, eche un vistazo a los temas de la sección [Recursos adicionales](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="fd044-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="fd044-210">El asistente de etiquetas de caché es dependiente del [servicio de caché de memoria](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="fd044-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="fd044-211">El asistente de etiquetas de caché agrega el servicio si no se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="fd044-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fd044-212">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fd044-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
