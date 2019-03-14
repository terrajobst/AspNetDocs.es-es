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
# <a name="razor-components-layouts"></a><span data-ttu-id="5cb12-103">Diseños de los componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="5cb12-103">Razor Components layouts</span></span>

<span data-ttu-id="5cb12-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="5cb12-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="5cb12-105">Normalmente, las aplicaciones contienen más de una página.</span><span class="sxs-lookup"><span data-stu-id="5cb12-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="5cb12-106">Elementos de diseño, como menús, los mensajes de copyright y logotipos, deben estar presentes en todas las páginas.</span><span class="sxs-lookup"><span data-stu-id="5cb12-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="5cb12-107">Copiando el código de estos elementos de diseño en todas las páginas de una aplicación no es una solución eficaz.</span><span class="sxs-lookup"><span data-stu-id="5cb12-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="5cb12-108">Esta duplicación es difícil de mantener y probablemente se lleva a contenido incoherente con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="5cb12-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="5cb12-109">*Los diseños* solucionar este problema.</span><span class="sxs-lookup"><span data-stu-id="5cb12-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="5cb12-110">Técnicamente, un diseño es simplemente otro componente.</span><span class="sxs-lookup"><span data-stu-id="5cb12-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="5cb12-111">Un diseño que se define en una plantilla de Razor o en C# de código y puede contener el enlace de datos, inserción de dependencias y otras características de los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="5cb12-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="5cb12-112">Dos aspectos adicionales activar un *componente* en un *diseño*:</span><span class="sxs-lookup"><span data-stu-id="5cb12-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="5cb12-113">El componente de diseño debe heredar de `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="5cb12-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="5cb12-114">`BlazorLayoutComponent` define un `Body` propiedad que contiene el contenido que se representará dentro del diseño.</span><span class="sxs-lookup"><span data-stu-id="5cb12-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="5cb12-115">Usa el componente de diseño la `Body` propiedad para especificar que el contenido del cuerpo deben representar mediante la sintaxis de Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="5cb12-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="5cb12-116">Durante la representación, `@Body` se sustituye por el contenido del diseño.</span><span class="sxs-lookup"><span data-stu-id="5cb12-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="5cb12-117">Ejemplo de código siguiente muestra la plantilla de Razor de un componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="5cb12-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="5cb12-118">Tenga en cuenta el uso de `BlazorLayoutComponent` y `@Body`:</span><span class="sxs-lookup"><span data-stu-id="5cb12-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

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

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="5cb12-119">Usar un diseño en un componente</span><span class="sxs-lookup"><span data-stu-id="5cb12-119">Use a layout in a component</span></span>

<span data-ttu-id="5cb12-120">Utilice la directiva de Razor `@layout` para aplicar un diseño a un componente.</span><span class="sxs-lookup"><span data-stu-id="5cb12-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="5cb12-121">El compilador convierte esta directiva en un `LayoutAttribute`, que se aplica a la clase de componente.</span><span class="sxs-lookup"><span data-stu-id="5cb12-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="5cb12-122">Ejemplo de código siguiente muestra el concepto.</span><span class="sxs-lookup"><span data-stu-id="5cb12-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="5cb12-123">El contenido de este componente se inserta en la *MasterLayout* en la posición del `@Body`:</span><span class="sxs-lookup"><span data-stu-id="5cb12-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="5cb12-124">Selección del diseño centralizada</span><span class="sxs-lookup"><span data-stu-id="5cb12-124">Centralized layout selection</span></span>

<span data-ttu-id="5cb12-125">Todas las carpetas de un una aplicación puede contener opcionalmente un archivo de plantilla denominado *_ViewImports.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5cb12-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="5cb12-126">El compilador incluye las directivas especificadas en el archivo de importaciones de vista en todas las plantillas de Razor en la misma carpeta y de forma recursiva en todas sus subcarpetas.</span><span class="sxs-lookup"><span data-stu-id="5cb12-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="5cb12-127">Por lo tanto, un *_ViewImports.cshtml* que contiene el archivo `@layout MainLayout` garantiza que todos los componentes en una carpeta, use el *MainLayout* diseño.</span><span class="sxs-lookup"><span data-stu-id="5cb12-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="5cb12-128">No es necesario para agregar varias veces `@layout` a todos los  *\*.cshtml* archivos.</span><span class="sxs-lookup"><span data-stu-id="5cb12-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="5cb12-129">Tenga en cuenta que la plantilla predeterminada usa la *_ViewImports.cshtml* mecanismo para la selección de diseño.</span><span class="sxs-lookup"><span data-stu-id="5cb12-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="5cb12-130">Contiene una aplicación recién creada el *_ViewImports.cshtml* de archivos en el *páginas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="5cb12-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="5cb12-131">Diseños anidados</span><span class="sxs-lookup"><span data-stu-id="5cb12-131">Nested layouts</span></span>

<span data-ttu-id="5cb12-132">Las aplicaciones pueden constar de los diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="5cb12-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="5cb12-133">Un componente puede hacer referencia a un diseño que a su vez hace referencia a otra distribución.</span><span class="sxs-lookup"><span data-stu-id="5cb12-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="5cb12-134">Por ejemplo, los diseños de anidamiento pueden usarse para reflejar una estructura de varios niveles de menú.</span><span class="sxs-lookup"><span data-stu-id="5cb12-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="5cb12-135">Ejemplos de código siguientes muestran cómo utilizar los diseños anidados.</span><span class="sxs-lookup"><span data-stu-id="5cb12-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="5cb12-136">El *CustomersComponent.cshtml* archivo es el componente para mostrar.</span><span class="sxs-lookup"><span data-stu-id="5cb12-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="5cb12-137">Tenga en cuenta que el componente hace referencia el diseño `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="5cb12-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="5cb12-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5cb12-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="5cb12-139">El *MasterDataLayout.cshtml* archivo proporciona el `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="5cb12-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="5cb12-140">El diseño hace referencia a otro diseño `MainLayout`, donde se va a insertarse.</span><span class="sxs-lookup"><span data-stu-id="5cb12-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="5cb12-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5cb12-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="5cb12-142">Por último, `MainLayout` contiene los elementos de diseño de nivel superior, como el encabezado, pie de página y el menú principal.</span><span class="sxs-lookup"><span data-stu-id="5cb12-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="5cb12-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5cb12-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
