---
title: SDK de Razor de ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: 0e6cfeb1863ed14ffe670cf082e99f28b26718dd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054742"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="efaae-103">SDK de Razor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efaae-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="efaae-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="efaae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efaae-105">El [!INCLUDE[](~/includes/2.1-SDK.md)] incluye la `Microsoft.NET.Sdk.Razor` MSBuild SDK (SDK de Razor).</span><span class="sxs-lookup"><span data-stu-id="efaae-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="efaae-106">El SDK de Razor:</span><span class="sxs-lookup"><span data-stu-id="efaae-106">The Razor SDK:</span></span>

* <span data-ttu-id="efaae-107">Normaliza la experiencia de creación, empaquetado y publicación de proyectos que contienen archivos de [Razor](xref:mvc/views/razor) relativos a proyectos basados en ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="efaae-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="efaae-108">Incluye un conjunto de destinos, propiedades y elementos predefinidos que permiten personalizar la compilación de archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efaae-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="efaae-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="efaae-110">Uso del SDK de Razor</span><span class="sxs-lookup"><span data-stu-id="efaae-110">Using the Razor SDK</span></span>

<span data-ttu-id="efaae-111">La mayoría de las aplicaciones web no están necesario hacer referencia explícitamente el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="efaae-112">Para usar el SDK de Razor para crear bibliotecas de clases que contengan vistas de Razor o páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="efaae-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="efaae-113">Use `Microsoft.NET.Sdk.Razor` en lugar de `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="efaae-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="efaae-114">Normalmente, una referencia de paquete para `Microsoft.AspNetCore.Mvc` se requiere para recibir las dependencias adicionales que son necesarios para generar y compilar las páginas de Razor y las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="efaae-115">Como mínimo, el proyecto debe agregar referencias de paquete para:</span><span class="sxs-lookup"><span data-stu-id="efaae-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
  <span data-ttu-id="efaae-116">El `Microsoft.AspNetCore.Razor.Design` paquete proporciona las tareas de compilación de Razor y destinos para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="efaae-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="efaae-117">Los paquetes anteriores están incluidos en `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="efaae-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="efaae-118">El marcado siguiente muestra un archivo de proyecto que usa el SDK de Razor para generar archivos de Razor para una aplicación de páginas de Razor de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="efaae-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="efaae-119">El `Microsoft.AspNetCore.Razor.Design` y `Microsoft.AspNetCore.Mvc.Razor.Extensions` paquetes se incluyen en el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="efaae-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="efaae-120">Sin embargo, la versión de menor `Microsoft.AspNetCore.App` referencia de paquete proporciona un metapaquete a la aplicación que no incluye la versión más reciente de `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="efaae-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="efaae-121">Deben hacer referencia a una versión coherente de los proyectos `Microsoft.AspNetCore.Razor.Design` (o `Microsoft.AspNetCore.Mvc`) para que se incluyen las correcciones más recientes de tiempo de compilación para Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="efaae-122">Para obtener más información, consulte [este problema de GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="efaae-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="efaae-123">Propiedades</span><span class="sxs-lookup"><span data-stu-id="efaae-123">Properties</span></span>

<span data-ttu-id="efaae-124">Las siguientes propiedades controlan el comportamiento del SDK de Razor como parte de la generación de un proyecto:</span><span class="sxs-lookup"><span data-stu-id="efaae-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="efaae-125">`RazorCompileOnBuild` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la creación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="efaae-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="efaae-126">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="efaae-126">Defaults to `true`.</span></span>
* <span data-ttu-id="efaae-127">`RazorCompileOnPublish` &ndash; Cuando `true`, compila y se emite el ensamblado de Razor como parte de la publicación del proyecto.</span><span class="sxs-lookup"><span data-stu-id="efaae-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="efaae-128">Tiene como valor predeterminado `true`.</span><span class="sxs-lookup"><span data-stu-id="efaae-128">Defaults to `true`.</span></span>

<span data-ttu-id="efaae-129">Las propiedades y elementos en la tabla siguiente se utilizan para configurar las entradas y salida para el SDK de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="efaae-130">Elementos</span><span class="sxs-lookup"><span data-stu-id="efaae-130">Items</span></span> | <span data-ttu-id="efaae-131">Descripción</span><span class="sxs-lookup"><span data-stu-id="efaae-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="efaae-132">Elementos (archivos *.cshtml*) que son entradas para los destinos de generación de código.</span><span class="sxs-lookup"><span data-stu-id="efaae-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="efaae-133">Elementos de elemento (*.cs* archivos) que son entradas para los destinos de compilación de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="efaae-134">Use este ItemGroup para especificar otros archivos que se compilen en el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="efaae-135">Elementos que se usan para generar atributos de código para el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="efaae-136">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="efaae-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="efaae-137">Elementos que se agregan como recursos incrustados en el ensamblado generado de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="efaae-138">Property</span><span class="sxs-lookup"><span data-stu-id="efaae-138">Property</span></span> | <span data-ttu-id="efaae-139">Descripción</span><span class="sxs-lookup"><span data-stu-id="efaae-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="efaae-140">Nombre de archivo (sin extensión) del ensamblado generado por Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="efaae-141">Directorio de salida de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="efaae-142">Sirve para determinar el conjunto de herramientas que se va a usar para compilar el ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="efaae-143">Valores válidos son `Implicit`, `RazorSDK` y `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="efaae-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="efaae-144">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="efaae-144">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="efaae-145">El valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="efaae-145">Default is `true`.</span></span> <span data-ttu-id="efaae-146">Cuando `true`, incluye *web.config*, *.json*, y *.cshtml* archivos como contenido en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="efaae-146">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="efaae-147">Cuando se hace referencia a través de `Microsoft.NET.Sdk.Web`, los archivos en *wwwroot* y también se incluyen los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="efaae-147">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="efaae-148">Si es `true`, incluye los archivos *.cshtml* de los elementos `Content` en elementos `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="efaae-148">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="efaae-149">Cuando `true`, genera un *.cs* archivo que contiene los atributos especificados por `RazorAssemblyAttribute` e incluye el archivo en la salida de compilación.</span><span class="sxs-lookup"><span data-stu-id="efaae-149">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="efaae-150">Si es `true`, agrega a `RazorAssemblyAttribute` un conjunto predeterminado de atributos de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="efaae-150">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="efaae-151">Cuando `true`, copias `RazorGenerate` elementos (*.cshtml*) archivos en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="efaae-151">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="efaae-152">Normalmente, los archivos de Razor no son necesarios para una aplicación publicada si participa en la compilación en tiempo de compilación o tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="efaae-152">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="efaae-153">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="efaae-153">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="efaae-154">Si es `true`, copia los elementos de referencia del ensamblado en el directorio de publicación.</span><span class="sxs-lookup"><span data-stu-id="efaae-154">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="efaae-155">Normalmente, los ensamblados de referencia no son necesarios para una aplicación publicada si la compilación de Razor se produce en tiempo de compilación o tiempo de publicación.</span><span class="sxs-lookup"><span data-stu-id="efaae-155">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="efaae-156">Establecido en `true` si la aplicación publicada requiere compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="efaae-156">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="efaae-157">Por ejemplo, establezca el valor en `true` si modifica la aplicación *.cshtml* archivos en tiempo de ejecución o usa vistas incrustadas.</span><span class="sxs-lookup"><span data-stu-id="efaae-157">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="efaae-158">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="efaae-158">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="efaae-159">Cuando `true`, todos los elementos de contenido de Razor (*.cshtml* archivos) se marcan para su inclusión en el paquete NuGet generado.</span><span class="sxs-lookup"><span data-stu-id="efaae-159">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="efaae-160">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="efaae-160">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="efaae-161">Si es `true`, agrega los elementos de RazorGenerate (*.cshtml*) como archivos incrustados al ensamblado de Razor generado.</span><span class="sxs-lookup"><span data-stu-id="efaae-161">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="efaae-162">Tiene como valor predeterminado `false`.</span><span class="sxs-lookup"><span data-stu-id="efaae-162">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="efaae-163">Si es `true`, usa un proceso de servidor de compilación persistente para descargar el trabajo de generación de código.</span><span class="sxs-lookup"><span data-stu-id="efaae-163">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="efaae-164">El valor predeterminado es `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="efaae-164">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="efaae-165">Para más información sobre las propiedades, consulte [Propiedades de MSBuild](/visualstudio/msbuild/msbuild-properties).</span><span class="sxs-lookup"><span data-stu-id="efaae-165">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="efaae-166">Destinos</span><span class="sxs-lookup"><span data-stu-id="efaae-166">Targets</span></span>

<span data-ttu-id="efaae-167">El SDK de Razor establece dos objetivos principales:</span><span class="sxs-lookup"><span data-stu-id="efaae-167">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="efaae-168">`RazorGenerate` &ndash; Genera código *.cs* archivos desde `RazorGenerate` elementos de elemento.</span><span class="sxs-lookup"><span data-stu-id="efaae-168">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="efaae-169">Use la propiedad `RazorGenerateDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="efaae-169">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="efaae-170">`RazorCompile` &ndash; Compila genera *.cs* archivos en un ensamblado de Razor.</span><span class="sxs-lookup"><span data-stu-id="efaae-170">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="efaae-171">Use `RazorCompileDependsOn` para especificar más destinos que puedan ejecutarse antes o después de este destino.</span><span class="sxs-lookup"><span data-stu-id="efaae-171">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="efaae-172">Compilación en tiempo de ejecución de vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="efaae-172">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="efaae-173">El SDK de Razor no publica de forma predeterminada los ensamblados de referencia necesarios para realizar una compilación en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="efaae-173">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="efaae-174">Como consecuencia, se producen errores de compilación cuando el modelo de aplicación se basa en la compilación en tiempo de ejecución; por ejemplo, la aplicación usa vistas incrustadas o cambia vistas tras haber sido publicada.</span><span class="sxs-lookup"><span data-stu-id="efaae-174">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="efaae-175">Establezca `CopyRefAssembliesToPublishDirectory` en `true` para seguir publicando ensamblados de referencia.</span><span class="sxs-lookup"><span data-stu-id="efaae-175">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="efaae-176">Para una aplicación web, asegúrese de que su aplicación tiene como destino el `Microsoft.NET.Sdk.Web` SDK.</span><span class="sxs-lookup"><span data-stu-id="efaae-176">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>