---
title: Bibliotecas de clases de componentes de Razor
author: guardrex
description: Descubra cómo los componentes se pueden incluir en las aplicaciones de componentes de Razor de una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 0e644627178bae2b8880760335860b3e0ebef156
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037412"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="95344-103">Bibliotecas de clases de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="95344-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="95344-104">Por [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="95344-104">By [Simon Timms](https://github.com/stimms)</span></span>

> [!NOTE]
> <span data-ttu-id="95344-105">El SDK de .NET Core 3.0 Preview 2 no incluye una plantilla de proyecto para las bibliotecas de clases de componente de Razor, pero esperamos poder agregar una plantilla en una vista previa futura.</span><span class="sxs-lookup"><span data-stu-id="95344-105">The .NET Core 3.0 Preview 2 SDK doesn't include a project template for Razor Component Class Libraries, but we expect to add a template in a future preview.</span></span> <span data-ttu-id="95344-106">En mientras tanto, puede usar la plantilla de biblioteca de clases de componente Blazor explicada en este tema.</span><span class="sxs-lookup"><span data-stu-id="95344-106">In meantime, you can use the Blazor Component Class Library template explained in this topic.</span></span>

<span data-ttu-id="95344-107">Los componentes se pueden compartir en las bibliotecas de componentes entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="95344-107">Components can be shared in component libraries across projects.</span></span> <span data-ttu-id="95344-108">Los componentes se pueden incluidos desde:</span><span class="sxs-lookup"><span data-stu-id="95344-108">Components can be included from:</span></span>

* <span data-ttu-id="95344-109">Otro proyecto en la solución.</span><span class="sxs-lookup"><span data-stu-id="95344-109">Another project in the solution.</span></span>
* <span data-ttu-id="95344-110">Un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="95344-110">A NuGet package.</span></span>
* <span data-ttu-id="95344-111">Biblioteca de .NET que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="95344-111">A referenced .NET library.</span></span>

<span data-ttu-id="95344-112">Como los componentes son tipos de .NET normales, las bibliotecas de componentes son ensamblados de .NET normales.</span><span class="sxs-lookup"><span data-stu-id="95344-112">Just as components are regular .NET types, component libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="95344-113">Para crear una nueva biblioteca de componentes, use el `blazorlib` plantilla con el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando.</span><span class="sxs-lookup"><span data-stu-id="95344-113">To create a new component library, use the `blazorlib` template with the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="95344-114">La plantilla forma parte de las plantillas instaladas cuando [configurar los componentes de Razor](xref:razor-components/get-started).</span><span class="sxs-lookup"><span data-stu-id="95344-114">The template is part of the templates installed when [setting up Razor Components](xref:razor-components/get-started).</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="95344-115">Para agregar la biblioteca a un proyecto existente, use el [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:</span><span class="sxs-lookup"><span data-stu-id="95344-115">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

```console
dotnet sln add .\MyComponentLib1
```

<span data-ttu-id="95344-116">Las bibliotecas de componentes pueden contener los archivos estáticos, como imágenes, JavaScript y hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="95344-116">Component libraries may contain static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="95344-117">En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado (*.dll*), que permite el consumo de los componentes sin tener que preocuparse sobre cómo incluir sus recursos.</span><span class="sxs-lookup"><span data-stu-id="95344-117">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="95344-118">Los archivos incluidos en el `content` directory se marcan como recurso incrustado.</span><span class="sxs-lookup"><span data-stu-id="95344-118">Any files included in the `content` directory are marked as an embedded resource.</span></span> 

## <a name="consume-a-library-component"></a><span data-ttu-id="95344-119">Consumir un componente de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="95344-119">Consume a library component</span></span>

<span data-ttu-id="95344-120">Para consumir los componentes definidos en una biblioteca en otro proyecto, el [ @addTagHelper ](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) se debe usar la directiva.</span><span class="sxs-lookup"><span data-stu-id="95344-120">In order to consume components defined in a library in another project, the [@addTagHelper](/aspnet/core/mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="95344-121">Los componentes individuales se pueden agregar por nombre.</span><span class="sxs-lookup"><span data-stu-id="95344-121">Individual components may be added by name.</span></span> <span data-ttu-id="95344-122">Por ejemplo, se agrega la siguiente directiva `Component1` de `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="95344-122">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="95344-123">El formato general de la directiva es:</span><span class="sxs-lookup"><span data-stu-id="95344-123">The general format of the directive is:</span></span>

```cshtml
@addTagHelper <namespaced component name>, <assembly name>
```

<span data-ttu-id="95344-124">Sin embargo, es común para incluir todos los componentes de un ensamblado con un carácter comodín:</span><span class="sxs-lookup"><span data-stu-id="95344-124">However, it's common to include all of the components from an assembly using a wildcard:</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="95344-125">El `@addTagHelper` directiva puede incluirse en *_ViewImport.cshtml* para que los componentes disponibles para un proyecto completo o aplicada a una sola página o un conjunto de páginas dentro de una carpeta.</span><span class="sxs-lookup"><span data-stu-id="95344-125">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="95344-126">Con el `@addTagHelper` la directiva en su lugar, los componentes de la biblioteca de componentes pueden utilizarse como si estuvieran en el mismo ensamblado que la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95344-126">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span> 

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="95344-127">Compilación, pack y envío de NuGet</span><span class="sxs-lookup"><span data-stu-id="95344-127">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="95344-128">Dado que las bibliotecas de componentes son bibliotecas de .NET standard, empaquetado y envío a NuGet no es diferente de empaquetado y envío de cualquier biblioteca en NuGet.</span><span class="sxs-lookup"><span data-stu-id="95344-128">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="95344-129">Empaquetado se realiza mediante el [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:</span><span class="sxs-lookup"><span data-stu-id="95344-129">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="95344-130">Cargar el paquete en NuGet mediante la [publicar dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) comando:</span><span class="sxs-lookup"><span data-stu-id="95344-130">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="95344-131">Cualquier incluye recursos estáticos se incluyen en el paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="95344-131">Any included static resources are included in the NuGet package.</span></span> <span data-ttu-id="95344-132">Los consumidores de la biblioteca reciben automáticamente los scripts y hojas de estilos, por lo que no están necesarios instalar manualmente los recursos a los consumidores.</span><span class="sxs-lookup"><span data-stu-id="95344-132">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
