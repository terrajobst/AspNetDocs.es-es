---
title: Diseños de los componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear componentes reutilizables de diseño para aplicaciones Blazor y componentes de Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039042"
---
# <a name="razor-components-layouts"></a>Diseños de los componentes de Razor

Por [Rainer Stropek](https://www.timecockpit.com)

Normalmente, las aplicaciones contienen más de una página. Elementos de diseño, como menús, los mensajes de copyright y logotipos, deben estar presentes en todas las páginas. Copiando el código de estos elementos de diseño en todas las páginas de una aplicación no es una solución eficaz. Esta duplicación es difícil de mantener y probablemente se lleva a contenido incoherente con el tiempo. *Los diseños* solucionar este problema.

Técnicamente, un diseño es simplemente otro componente. Un diseño que se define en una plantilla de Razor o en C# de código y puede contener el enlace de datos, inserción de dependencias y otras características de los componentes normales. Dos aspectos adicionales activar un *componente* en un *diseño*:

* El componente de diseño debe heredar de `BlazorLayoutComponent`. `BlazorLayoutComponent` define un `Body` propiedad que contiene el contenido que se representará dentro del diseño.
* Usa el componente de diseño la `Body` propiedad para especificar que el contenido del cuerpo deben representar mediante la sintaxis de Razor `@Body`. Durante la representación, `@Body` se sustituye por el contenido del diseño.

Ejemplo de código siguiente muestra la plantilla de Razor de un componente de diseño. Tenga en cuenta el uso de `BlazorLayoutComponent` y `@Body`:

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a>Usar un diseño en un componente

Utilice la directiva de Razor `@layout` para aplicar un diseño a un componente. El compilador convierte esta directiva en un `LayoutAttribute`, que se aplica a la clase de componente.

Ejemplo de código siguiente muestra el concepto. El contenido de este componente se inserta en la *MasterLayout* en la posición del `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Selección del diseño centralizada

Todas las carpetas de un una aplicación puede contener opcionalmente un archivo de plantilla denominado *_ViewImports.cshtml*. El compilador incluye las directivas especificadas en el archivo de importaciones de vista en todas las plantillas de Razor en la misma carpeta y de forma recursiva en todas sus subcarpetas. Por lo tanto, un *_ViewImports.cshtml* que contiene el archivo `@layout MainLayout` garantiza que todos los componentes en una carpeta, use el *MainLayout* diseño. No es necesario para agregar varias veces `@layout` a todos los  *\*.cshtml* archivos.

Tenga en cuenta que la plantilla predeterminada usa la *_ViewImports.cshtml* mecanismo para la selección de diseño. Contiene una aplicación recién creada el *_ViewImports.cshtml* de archivos en el *páginas* carpeta.

## <a name="nested-layouts"></a>Diseños anidados

Las aplicaciones pueden constar de los diseños anidados. Un componente puede hacer referencia a un diseño que a su vez hace referencia a otra distribución. Por ejemplo, los diseños de anidamiento pueden usarse para reflejar una estructura de varios niveles de menú.

Ejemplos de código siguientes muestran cómo utilizar los diseños anidados. El *CustomersComponent.cshtml* archivo es el componente para mostrar. Tenga en cuenta que el componente hace referencia el diseño `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

El *MasterDataLayout.cshtml* archivo proporciona el `MasterDataLayout`. El diseño hace referencia a otro diseño `MainLayout`, donde se va a insertarse.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Por último, `MainLayout` contiene los elementos de diseño de nivel superior, como el encabezado, pie de página y el menú principal.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
