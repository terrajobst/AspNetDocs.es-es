---
title: Proveedores de archivo en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 5d0d46ba82cd84e48e5a9b23d6d330d8888beb41
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042722"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="7334e-103">Proveedores de archivo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7334e-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="7334e-104">Por [Steve Smith](https://ardalis.com/) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7334e-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7334e-105">ASP.NET Core abstrae el acceso al sistema de archivos mediante el uso de proveedores de archivos.</span><span class="sxs-lookup"><span data-stu-id="7334e-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="7334e-106">Los proveedores de archivos se usan en el marco de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="7334e-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="7334e-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) expone la raíz del contenido de la aplicación y la raíz web como tipos `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7334e-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="7334e-108">El [middleware de archivos estáticos](xref:fundamentals/static-files) usa proveedores de archivos para buscar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="7334e-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="7334e-109">[Razor](xref:mvc/views/razor) usa proveedores de archivos para localizar páginas y vistas.</span><span class="sxs-lookup"><span data-stu-id="7334e-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="7334e-110">Las herramientas de .NET Core usan proveedores de archivos y patrones globales para especificar los archivos que deben publicarse.</span><span class="sxs-lookup"><span data-stu-id="7334e-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="7334e-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7334e-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="7334e-112">Interfaces de proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7334e-112">File Provider interfaces</span></span>

<span data-ttu-id="7334e-113">La interfaz principal es [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="7334e-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="7334e-114">`IFileProvider` expone métodos para:</span><span class="sxs-lookup"><span data-stu-id="7334e-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="7334e-115">Obtener información de archivo ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="7334e-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="7334e-116">Obtener información de directorio ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="7334e-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="7334e-117">Configurar las notificaciones de cambio (mediante [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="7334e-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="7334e-118">`IFileInfo` proporciona métodos y propiedades para trabajar con archivos:</span><span class="sxs-lookup"><span data-stu-id="7334e-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="7334e-119">Exists</span><span class="sxs-lookup"><span data-stu-id="7334e-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="7334e-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="7334e-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="7334e-121">Name</span><span class="sxs-lookup"><span data-stu-id="7334e-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="7334e-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (en bytes)</span><span class="sxs-lookup"><span data-stu-id="7334e-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="7334e-123">Fecha [LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)</span><span class="sxs-lookup"><span data-stu-id="7334e-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="7334e-124">Puede leer del archivo mediante el método [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream).</span><span class="sxs-lookup"><span data-stu-id="7334e-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="7334e-125">La aplicación de ejemplo muestra cómo configurar un proveedor de archivos en `Startup.ConfigureServices` para su uso en toda la aplicación a través de la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7334e-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7334e-126">Implementaciones del proveedor de archivos</span><span class="sxs-lookup"><span data-stu-id="7334e-126">File Provider implementations</span></span>

<span data-ttu-id="7334e-127">Hay tres implementaciones de `IFileProvider` disponibles.</span><span class="sxs-lookup"><span data-stu-id="7334e-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="7334e-128">Implementación</span><span class="sxs-lookup"><span data-stu-id="7334e-128">Implementation</span></span> | <span data-ttu-id="7334e-129">Descripción</span><span class="sxs-lookup"><span data-stu-id="7334e-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7334e-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7334e-131">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="7334e-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7334e-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="7334e-133">El proveedor insertado de manifiestos se utiliza para tener acceder a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7334e-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7334e-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7334e-135">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="7334e-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="7334e-136">Implementación</span><span class="sxs-lookup"><span data-stu-id="7334e-136">Implementation</span></span> | <span data-ttu-id="7334e-137">Descripción</span><span class="sxs-lookup"><span data-stu-id="7334e-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7334e-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7334e-139">El proveedor físico se utiliza para acceder a los archivos físicos del sistema.</span><span class="sxs-lookup"><span data-stu-id="7334e-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7334e-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="7334e-141">El proveedor insertado se utiliza para tener acceso a archivos insertados en ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7334e-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7334e-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7334e-143">El proveedor compuesto se utiliza para proporcionar acceso combinado a archivos y directorios de uno o más proveedores.</span><span class="sxs-lookup"><span data-stu-id="7334e-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="7334e-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-144">PhysicalFileProvider</span></span>

<span data-ttu-id="7334e-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) proporciona acceso al sistema de archivos físico.</span><span class="sxs-lookup"><span data-stu-id="7334e-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="7334e-146">`PhysicalFileProvider` usa el tipo [System.IO.File](/dotnet/api/system.io.file) (para el proveedor físico) y define el ámbito de todas las rutas de acceso a un directorio y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7334e-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="7334e-147">Esta definición de ámbito impide el acceso al sistema de archivos fuera del directorio especificado y sus elementos secundarios.</span><span class="sxs-lookup"><span data-stu-id="7334e-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="7334e-148">Al crear una instancia de este proveedor, se requiere una ruta de acceso del directorio, que actúa como la ruta de acceso base para todas las solicitudes realizadas usando el proveedor.</span><span class="sxs-lookup"><span data-stu-id="7334e-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="7334e-149">Puede crear instancias de un proveedor `PhysicalFileProvider` directamente o puede solicitar `IFileProvider` en un constructor a través de [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7334e-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="7334e-150">**Tipos estáticos**</span><span class="sxs-lookup"><span data-stu-id="7334e-150">**Static types**</span></span>

<span data-ttu-id="7334e-151">El código siguiente muestra cómo crear `PhysicalFileProvider` y usarlo para obtener el contenido del directorio y la información del archivo:</span><span class="sxs-lookup"><span data-stu-id="7334e-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="7334e-152">Tipos del ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="7334e-152">Types in the preceding example:</span></span>

* <span data-ttu-id="7334e-153">`provider` es `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="7334e-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="7334e-154">`contents` es `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="7334e-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="7334e-155">`fileInfo` es `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="7334e-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="7334e-156">El proveedor de archivos puede usarse para iterar por el directorio especificado por `applicationRoot` o llamar a `GetFileInfo` para obtener información de un archivo.</span><span class="sxs-lookup"><span data-stu-id="7334e-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="7334e-157">El proveedor de archivos no tiene acceso fuera del directorio `applicationRoot`.</span><span class="sxs-lookup"><span data-stu-id="7334e-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="7334e-158">La aplicación de ejemplo crea el proveedor en la clase `Startup.ConfigureServices` de la aplicación mediante [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="7334e-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="7334e-159">**Obtención de tipos de proveedor de archivos con inserción de dependencias**</span><span class="sxs-lookup"><span data-stu-id="7334e-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="7334e-160">Inserte el proveedor en cualquier constructor de clase y asígnelo a un campo local.</span><span class="sxs-lookup"><span data-stu-id="7334e-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="7334e-161">Utilice el campo en todos los métodos de la clase para acceder a archivos.</span><span class="sxs-lookup"><span data-stu-id="7334e-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7334e-162">En la aplicación de ejemplo, la clase `IndexModel` recibe una instancia `IFileProvider` para obtener el contenido del directorio para la ruta de acceso base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7334e-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="7334e-163">*Páginas/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="7334e-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="7334e-164">`IDirectoryContents` se iteran en la página.</span><span class="sxs-lookup"><span data-stu-id="7334e-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="7334e-165">*Páginas/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7334e-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7334e-166">En la aplicación de ejemplo, la clase `HomeController` recibe una instancia `IFileProvider` para obtener el contenido del directorio para la ruta de acceso base de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="7334e-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="7334e-167">*Controladores/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="7334e-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="7334e-168">`IDirectoryContents` se iteran en la vista.</span><span class="sxs-lookup"><span data-stu-id="7334e-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="7334e-169">*Vistas/Inicio/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7334e-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="7334e-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="7334e-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) se utiliza para acceder a archivos incrustados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7334e-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7334e-172">`ManifestEmbeddedFileProvider` utiliza un manifiesto compilado en el ensamblado para reconstruir las rutas de acceso originales de los archivos incrustados.</span><span class="sxs-lookup"><span data-stu-id="7334e-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="7334e-173">`ManifestEmbeddedFileProvider` está disponible en ASP.NET Core 2.1 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="7334e-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="7334e-174">Para acceder a archivos incrustados en ensamblados en ASP.NET Core 2.0 o versiones anteriores, consulte el [versión 1.x de ASP.NET Core de este tema](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="7334e-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="7334e-175">Para generar un manifiesto de los archivos incrustados, establezca la propiedad `<GenerateEmbeddedFilesManifest>` en `true`.</span><span class="sxs-lookup"><span data-stu-id="7334e-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="7334e-176">Especifique los archivos que desea incrustar con [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span><span class="sxs-lookup"><span data-stu-id="7334e-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="7334e-177">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7334e-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7334e-178">La aplicación de ejemplo crea `ManifestEmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7334e-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7334e-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7334e-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7334e-180">Las sobrecargas adicionales le permiten:</span><span class="sxs-lookup"><span data-stu-id="7334e-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="7334e-181">Especificar una ruta de acceso de archivo relativa.</span><span class="sxs-lookup"><span data-stu-id="7334e-181">Specify a relative file path.</span></span>
* <span data-ttu-id="7334e-182">Definir el ámbito de archivos a la fecha de la última modificación.</span><span class="sxs-lookup"><span data-stu-id="7334e-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="7334e-183">Asignar nombre al recurso incrustado que contiene el manifiesto del archivo incrustado.</span><span class="sxs-lookup"><span data-stu-id="7334e-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="7334e-184">Sobrecarga</span><span class="sxs-lookup"><span data-stu-id="7334e-184">Overload</span></span> | <span data-ttu-id="7334e-185">Descripción</span><span class="sxs-lookup"><span data-stu-id="7334e-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="7334e-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="7334e-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="7334e-187">Acepta parámetro de ruta de acceso relativa `root` opcional.</span><span class="sxs-lookup"><span data-stu-id="7334e-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="7334e-188">Especifique `root` para definir el ámbito de las llamadas en [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) en aquellos recursos que se encuentran bajo las rutas de acceso proporcionadas.</span><span class="sxs-lookup"><span data-stu-id="7334e-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="7334e-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="7334e-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="7334e-190">Acepta un parámetro de ruta de acceso relativa `root` opcional y un parámetro de fecha `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset)).</span><span class="sxs-lookup"><span data-stu-id="7334e-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="7334e-191">La fecha `lastModified` define el ámbito de la última fecha de modificación para las instancias de [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) que [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) devuelve.</span><span class="sxs-lookup"><span data-stu-id="7334e-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="7334e-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="7334e-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="7334e-193">Acepta una ruta de acceso relativa `root` opcional, una fecha `lastModified` y parámetros `manifestName`.</span><span class="sxs-lookup"><span data-stu-id="7334e-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="7334e-194">`manifestName` representa el nombre del recurso incrustado que contiene el manifiesto.</span><span class="sxs-lookup"><span data-stu-id="7334e-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="7334e-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="7334e-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) se utiliza para acceder a archivos incrustados dentro de ensamblados.</span><span class="sxs-lookup"><span data-stu-id="7334e-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7334e-197">Especifique los archivos que desea incrustar con la propiedad [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) propiedad en el archivo de proyecto:</span><span class="sxs-lookup"><span data-stu-id="7334e-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="7334e-198">Use [patrones globales](#glob-patterns) para especificar uno o varios archivos para incrustar en el ensamblado.</span><span class="sxs-lookup"><span data-stu-id="7334e-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7334e-199">La aplicación de ejemplo crea `EmbeddedFileProvider` y pasa el ensamblado que se está ejecutando actualmente a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7334e-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7334e-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7334e-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7334e-201">Los recursos insertados no exponen directorios.</span><span class="sxs-lookup"><span data-stu-id="7334e-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="7334e-202">En su lugar, la ruta de acceso al recurso (a través de su espacio de nombres) se inserta en su nombre de archivo con separadores `.`.</span><span class="sxs-lookup"><span data-stu-id="7334e-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="7334e-203">En la aplicación de ejemplo, `baseNamespace` es `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="7334e-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="7334e-204">El constructor [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) acepta un parámetro `baseNamespace` opcional.</span><span class="sxs-lookup"><span data-stu-id="7334e-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="7334e-205">Especifique el espacio de nombres base para definir el ámbito de las llamadas en [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) en aquellos recursos que se encuentran bajo el espacio de nombres proporcionado.</span><span class="sxs-lookup"><span data-stu-id="7334e-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="7334e-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7334e-206">CompositeFileProvider</span></span>

<span data-ttu-id="7334e-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combina instancias de `IFileProvider` y expone una única interfaz para trabajar con archivos de varios proveedores.</span><span class="sxs-lookup"><span data-stu-id="7334e-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7334e-208">Al crear `CompositeFileProvider`, se pasan una o varias instancias de `IFileProvider` a su constructor.</span><span class="sxs-lookup"><span data-stu-id="7334e-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7334e-209">En la aplicación de ejemplo, `PhysicalFileProvider` y `ManifestEmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7334e-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7334e-210">En la aplicación de ejemplo, `PhysicalFileProvider` y `EmbeddedFileProvider` proporcionan archivos a un `CompositeFileProvider` registrado en el contenedor de servicios de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7334e-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="7334e-211">Observación de cambios</span><span class="sxs-lookup"><span data-stu-id="7334e-211">Watch for changes</span></span>

<span data-ttu-id="7334e-212">El método [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) proporciona un escenario para ver uno o más archivos o directorios para detectar cambios.</span><span class="sxs-lookup"><span data-stu-id="7334e-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7334e-213">`Watch` acepta una cadena de ruta de acceso, que puede usar [patrones globales](#glob-patterns) para especificar varios archivos.</span><span class="sxs-lookup"><span data-stu-id="7334e-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="7334e-214">`Watch` devuelve [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span><span class="sxs-lookup"><span data-stu-id="7334e-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="7334e-215">El token de cambio expone:</span><span class="sxs-lookup"><span data-stu-id="7334e-215">The change token exposes:</span></span>

* <span data-ttu-id="7334e-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): Una propiedad que se puede inspeccionar para determinar si se ha producido un cambio.</span><span class="sxs-lookup"><span data-stu-id="7334e-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="7334e-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Se llama cuando se detectan cambios en la cadena de ruta de acceso especificada.</span><span class="sxs-lookup"><span data-stu-id="7334e-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7334e-218">Cada token de cambio solo llama a su devolución de llamada asociada en respuesta a un único cambio.</span><span class="sxs-lookup"><span data-stu-id="7334e-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7334e-219">Para habilitar la supervisión constante, puede usar [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (como se muestra a continuación) o volver a crear instancias de `IChangeToken` en respuesta a los cambios.</span><span class="sxs-lookup"><span data-stu-id="7334e-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7334e-220">En la aplicación de ejemplo, la aplicación de consola *WatchConsole* se configura para mostrar un mensaje cada vez que se modifica un archivo de texto:</span><span class="sxs-lookup"><span data-stu-id="7334e-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="7334e-221">Algunos sistemas de archivos, como contenedores de Docker y recursos compartidos de red, no pueden enviar notificaciones de cambio de forma confiable.</span><span class="sxs-lookup"><span data-stu-id="7334e-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7334e-222">Establezca la variable de entorno `DOTNET_USE_POLLING_FILE_WATCHER` en `1` o `true` para sondear el sistema de archivos en busca de cambios cada cuatro segundos (no configurable).</span><span class="sxs-lookup"><span data-stu-id="7334e-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="7334e-223">Patrones globales</span><span class="sxs-lookup"><span data-stu-id="7334e-223">Glob patterns</span></span>

<span data-ttu-id="7334e-224">Las rutas de acceso del sistema de archivos utilizan patrones de caracteres comodín denominados *patrones globales*.</span><span class="sxs-lookup"><span data-stu-id="7334e-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="7334e-225">Especifique grupos de archivos con estos patrones.</span><span class="sxs-lookup"><span data-stu-id="7334e-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="7334e-226">Los dos caracteres comodín son `*` y `**`:</span><span class="sxs-lookup"><span data-stu-id="7334e-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="7334e-227">Coincide con cualquier elemento del nivel de carpeta actual, con cualquier nombre de archivo o con cualquier extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="7334e-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="7334e-228">Las coincidencias finalizan con los caracteres `/` y `.` en la ruta de acceso de archivo.</span><span class="sxs-lookup"><span data-stu-id="7334e-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="7334e-229">Coincide con cualquier elemento de varios niveles de directorios.</span><span class="sxs-lookup"><span data-stu-id="7334e-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7334e-230">Puede usarse para coincidir de forma recursiva con muchos archivos dentro de una jerarquía de directorios.</span><span class="sxs-lookup"><span data-stu-id="7334e-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="7334e-231">**Ejemplos de patrones globales**</span><span class="sxs-lookup"><span data-stu-id="7334e-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="7334e-232">Coincide con un archivo concreto en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7334e-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="7334e-233">Coincide con todos los archivos que tengan la extensión *.txt* en un directorio específico.</span><span class="sxs-lookup"><span data-stu-id="7334e-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="7334e-234">Coincide con todos los archivos `appsettings.json` que estén en directorios exactamente un nivel por debajo de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7334e-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="7334e-235">Coincide con todos los archivos que tengan la extensión *.txt* y se encuentren en cualquier lugar de la carpeta *directorio*.</span><span class="sxs-lookup"><span data-stu-id="7334e-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
