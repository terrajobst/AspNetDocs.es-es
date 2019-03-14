---
title: Enrutamiento de componentes de Razor
author: guardrex
description: Aprenda a enrutar las solicitudes en las aplicaciones y sobre el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031612"
---
# <a name="razor-components-routing"></a><span data-ttu-id="b43c4-103">Enrutamiento de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="b43c4-103">Razor Components routing</span></span>

<span data-ttu-id="b43c4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b43c4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b43c4-105">Aprenda a enrutar las solicitudes en las aplicaciones y sobre el componente NavLink.</span><span class="sxs-lookup"><span data-stu-id="b43c4-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="b43c4-106">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b43c4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b43c4-107">Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.</span><span class="sxs-lookup"><span data-stu-id="b43c4-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="b43c4-108">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="b43c4-108">Route templates</span></span>

<span data-ttu-id="b43c4-109">El `<Router>` componente habilita el enrutamiento, y se proporciona una plantilla de ruta para cada componente puede tener acceso.</span><span class="sxs-lookup"><span data-stu-id="b43c4-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="b43c4-110">El `<Router>` componente aparecerá en el *App.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="b43c4-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="b43c4-111">Cuando un  *\*.cshtml* de archivos con un `@page` directiva se compila, se proporciona la clase generada una [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) especificar la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="b43c4-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="b43c4-112">En tiempo de ejecución, el enrutador busca las clases de componente con un `RouteAttribute` y representa el componente tiene una plantilla de ruta que coincida con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="b43c4-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="b43c4-113">Varias plantillas de ruta se pueden aplicar a un componente.</span><span class="sxs-lookup"><span data-stu-id="b43c4-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="b43c4-114">En el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), el siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="b43c4-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="b43c4-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b43c4-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="b43c4-116">`<Router>` admite la configuración de un componente de reserva para el procesamiento cuando una ruta solicitada no se resuelve.</span><span class="sxs-lookup"><span data-stu-id="b43c4-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="b43c4-117">Habilitar este escenario de participación en estableciendo el `FallbackComponent` parámetro al tipo de la clase de componente de reserva.</span><span class="sxs-lookup"><span data-stu-id="b43c4-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="b43c4-118">En el ejemplo siguiente se establece un componente definido en *Pages/MyFallbackRazorComponent.cshtml* como el componente de reserva para un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="b43c4-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="b43c4-119">Para generar rutas correctamente, la aplicación debe incluir un `<base>` etiqueta en su *wwwroot/index.HTML* archivo con la ruta de la base de aplicación especificada en el `href` atributo (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="b43c4-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="b43c4-120">Para obtener más información, consulte [Host e implementar: Ruta de acceso de base de aplicación](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="b43c4-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="b43c4-121">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="b43c4-121">Route parameters</span></span>

<span data-ttu-id="b43c4-122">El enrutador usa los parámetros de ruta para rellenar los parámetros correspondientes del componente con el mismo nombre (no distingue mayúsculas de minúsculas).</span><span class="sxs-lookup"><span data-stu-id="b43c4-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="b43c4-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b43c4-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="b43c4-124">Los parámetros opcionales no se admiten aún, por lo que dos `@page` directivas se aplican en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="b43c4-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="b43c4-125">La primera permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="b43c4-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="b43c4-126">El segundo `@page` directiva toma el `{text}` parámetro de ruta y asigna el valor para el `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="b43c4-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="b43c4-127">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="b43c4-127">Route constraints</span></span>

<span data-ttu-id="b43c4-128">Una restricción de ruta exige la búsqueda de coincidencias en un segmento de ruta a un componente de tipo.</span><span class="sxs-lookup"><span data-stu-id="b43c4-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="b43c4-129">En el ejemplo siguiente, la ruta para el componente de los usuarios coincide solo si:</span><span class="sxs-lookup"><span data-stu-id="b43c4-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="b43c4-130">Un `Id` segmento de ruta está presente en la dirección URL de solicitud.</span><span class="sxs-lookup"><span data-stu-id="b43c4-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="b43c4-131">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="b43c4-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="b43c4-132">Las restricciones de ruta que se muestra en la tabla siguiente están disponibles para su uso.</span><span class="sxs-lookup"><span data-stu-id="b43c4-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="b43c4-133">Para las restricciones de ruta que coinciden con la referencia cultural invariable, vea la advertencia debajo de la tabla para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b43c4-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="b43c4-134">Restricción</span><span class="sxs-lookup"><span data-stu-id="b43c4-134">Constraint</span></span> | <span data-ttu-id="b43c4-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b43c4-135">Example</span></span>           | <span data-ttu-id="b43c4-136">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="b43c4-136">Example Matches</span></span>                                                                  | <span data-ttu-id="b43c4-137">Invariable</span><span class="sxs-lookup"><span data-stu-id="b43c4-137">Invariant</span></span><br><span data-ttu-id="b43c4-138">referencia cultural</span><span class="sxs-lookup"><span data-stu-id="b43c4-138">culture</span></span><br><span data-ttu-id="b43c4-139">coincidencia</span><span class="sxs-lookup"><span data-stu-id="b43c4-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="b43c4-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="b43c4-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="b43c4-141">No</span><span class="sxs-lookup"><span data-stu-id="b43c4-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="b43c4-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="b43c4-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="b43c4-143">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="b43c4-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="b43c4-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="b43c4-145">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="b43c4-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="b43c4-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="b43c4-147">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="b43c4-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="b43c4-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="b43c4-149">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="b43c4-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="b43c4-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="b43c4-151">No</span><span class="sxs-lookup"><span data-stu-id="b43c4-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="b43c4-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="b43c4-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="b43c4-153">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="b43c4-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="b43c4-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="b43c4-155">Sí</span><span class="sxs-lookup"><span data-stu-id="b43c4-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="b43c4-156">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="b43c4-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="b43c4-157">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="b43c4-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="b43c4-158">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="b43c4-158">NavLink component</span></span>

<span data-ttu-id="b43c4-159">Usar un componente NavLink en lugar de HTML  **\<un >** elementos al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="b43c4-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="b43c4-160">Un componente NavLink se comporta como un  **\<un >** elemento, excepto que alterna un `active` clase CSS en función de si su `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="b43c4-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="b43c4-161">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación que se muestran.</span><span class="sxs-lookup"><span data-stu-id="b43c4-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="b43c4-162">El componente NavMenu en el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) crea un [Bootstrap](https://getbootstrap.com/docs/) barra de navegación que se muestra cómo utilizar componentes NavLink.</span><span class="sxs-lookup"><span data-stu-id="b43c4-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="b43c4-163">El marcado siguiente muestra los dos primeros NavLinks en el *Shared/NavMenu.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="b43c4-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="b43c4-164">Hay dos `NavLinkMatch` opciones:</span><span class="sxs-lookup"><span data-stu-id="b43c4-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="b43c4-165">`NavLinkMatch.All` &ndash; Especifica que el NavLink debe estar activo cuando coincide con la dirección URL actual completa.</span><span class="sxs-lookup"><span data-stu-id="b43c4-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="b43c4-166">`NavLinkMatch.Prefix` &ndash; Especifica que el NavLink debe estar activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="b43c4-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="b43c4-167">En el ejemplo anterior, el inicio NavLink (`href=""`) coincide con todas las direcciones URL y siempre recibe la `active` clase CSS.</span><span class="sxs-lookup"><span data-stu-id="b43c4-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="b43c4-168">El segundo NavLink solo recibe la `active` clase cuando el usuario visita el componente BlazorRoute (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="b43c4-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
