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
# <a name="razor-components-routing"></a>Enrutamiento de componentes de Razor

Por [Luke Latham](https://github.com/guardrex)

Aprenda a enrutar las solicitudes en las aplicaciones y sobre el componente NavLink.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.

## <a name="route-templates"></a>Plantillas de ruta

El `<Router>` componente habilita el enrutamiento, y se proporciona una plantilla de ruta para cada componente puede tener acceso. El `<Router>` componente aparecerá en el *App.cshtml* archivo:

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

Cuando un  *\*.cshtml* de archivos con un `@page` directiva se compila, se proporciona la clase generada una [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) especificar la plantilla de ruta. En tiempo de ejecución, el enrutador busca las clases de componente con un `RouteAttribute` y representa el componente tiene una plantilla de ruta que coincida con la dirección URL solicitada.

Varias plantillas de ruta se pueden aplicar a un componente. En el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), el siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`.

*Pages/BlazorRoute.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

`<Router>` admite la configuración de un componente de reserva para el procesamiento cuando una ruta solicitada no se resuelve. Habilitar este escenario de participación en estableciendo el `FallbackComponent` parámetro al tipo de la clase de componente de reserva.

En el ejemplo siguiente se establece un componente definido en *Pages/MyFallbackRazorComponent.cshtml* como el componente de reserva para un `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Para generar rutas correctamente, la aplicación debe incluir un `<base>` etiqueta en su *wwwroot/index.HTML* archivo con la ruta de la base de aplicación especificada en el `href` atributo (`<base href="/" />`). Para obtener más información, consulte [Host e implementar: Ruta de acceso de base de aplicación](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="route-parameters"></a>Parámetros de ruta

El enrutador usa los parámetros de ruta para rellenar los parámetros correspondientes del componente con el mismo nombre (no distingue mayúsculas de minúsculas).

*Pages/RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

Los parámetros opcionales no se admiten aún, por lo que dos `@page` directivas se aplican en el ejemplo anterior. La primera permite la navegación al componente sin un parámetro. El segundo `@page` directiva toma el `{text}` parámetro de ruta y asigna el valor para el `Text` propiedad.

## <a name="route-constraints"></a>Restricciones de ruta

Una restricción de ruta exige la búsqueda de coincidencias en un segmento de ruta a un componente de tipo.

En el ejemplo siguiente, la ruta para el componente de los usuarios coincide solo si:

* Un `Id` segmento de ruta está presente en la dirección URL de solicitud.
* El `Id` segmento es un entero (`int`).

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

Las restricciones de ruta que se muestra en la tabla siguiente están disponibles para su uso. Para las restricciones de ruta que coinciden con la referencia cultural invariable, vea la advertencia debajo de la tabla para obtener más información.

| Restricción | Ejemplo           | Coincidencias de ejemplo                                                                  | Invariable<br>referencia cultural<br>coincidencia |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | No                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sí                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sí                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Sí                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | No                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sí                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sí                              |

> [!WARNING]
> Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable. Estas restricciones dan por supuesto que la dirección URL no es localizable.

## <a name="navlink-component"></a>Componente NavLink

Usar un componente NavLink en lugar de HTML  **\<un >** elementos al crear vínculos de navegación. Un componente NavLink se comporta como un  **\<un >** elemento, excepto que alterna un `active` clase CSS en función de si su `href` coincide con la dirección URL actual. La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación que se muestran.

El componente NavMenu en el [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) crea un [Bootstrap](https://getbootstrap.com/docs/) barra de navegación que se muestra cómo utilizar componentes NavLink. El marcado siguiente muestra los dos primeros NavLinks en el *Shared/NavMenu.cshtml* archivo.

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

Hay dos `NavLinkMatch` opciones:

* `NavLinkMatch.All` &ndash; Especifica que el NavLink debe estar activo cuando coincide con la dirección URL actual completa.
* `NavLinkMatch.Prefix` &ndash; Especifica que el NavLink debe estar activo cuando coincide con cualquier prefijo de la dirección URL actual.

En el ejemplo anterior, el inicio NavLink (`href=""`) coincide con todas las direcciones URL y siempre recibe la `active` clase CSS. El segundo NavLink solo recibe la `active` clase cuando el usuario visita el componente BlazorRoute (`href="BlazorRoute"`).
