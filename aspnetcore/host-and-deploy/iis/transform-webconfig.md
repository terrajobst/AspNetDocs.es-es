---
title: Transformación de web.config
author: guardrex
description: Obtenga información sobre cómo transformar el archivo web.config al publicar una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2019
uid: host-and-deploy/iis/transform-webconfig
ms.openlocfilehash: bd8cf7d8515e874eefd2c326727f56d0a4b502a7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025802"
---
# <a name="transform-webconfig"></a><span data-ttu-id="fb24e-103">Transformación de web.config</span><span class="sxs-lookup"><span data-stu-id="fb24e-103">Transform web.config</span></span>

<span data-ttu-id="fb24e-104">Por [Vijay Ramakrishnan](https://github.com/vijayrkn) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fb24e-104">By [Vijay Ramakrishnan](https://github.com/vijayrkn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fb24e-105">Las transformaciones en el archivo *web.config* se pueden aplicar automáticamente al publicarse una aplicación en función de:</span><span class="sxs-lookup"><span data-stu-id="fb24e-105">Transformations to the *web.config* file can be applied automatically when an app is published based on:</span></span>

* [<span data-ttu-id="fb24e-106">Configuración de compilación</span><span class="sxs-lookup"><span data-stu-id="fb24e-106">Build configuration</span></span>](#build-configuration)
* [<span data-ttu-id="fb24e-107">Profile</span><span class="sxs-lookup"><span data-stu-id="fb24e-107">Profile</span></span>](#profile)
* [<span data-ttu-id="fb24e-108">Entorno</span><span class="sxs-lookup"><span data-stu-id="fb24e-108">Environment</span></span>](#environment)
* [<span data-ttu-id="fb24e-109">Custom</span><span class="sxs-lookup"><span data-stu-id="fb24e-109">Custom</span></span>](#custom)

<span data-ttu-id="fb24e-110">Estas transformaciones se producen en cualquiera de los escenarios de generación *web.config* siguientes:</span><span class="sxs-lookup"><span data-stu-id="fb24e-110">These transformations occur for either of the following *web.config* generation scenarios:</span></span>

* <span data-ttu-id="fb24e-111">Generadas automáticamente por el SDK `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="fb24e-111">Generated automatically by the `Microsoft.NET.Sdk.Web` SDK.</span></span>
* <span data-ttu-id="fb24e-112">Proporcionadas por el desarrollador en la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fb24e-112">Provided by the developer in the content root of the app.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="fb24e-113">Configuración de compilación</span><span class="sxs-lookup"><span data-stu-id="fb24e-113">Build configuration</span></span>

<span data-ttu-id="fb24e-114">Las transformaciones de la configuración de compilación se ejecutan en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="fb24e-114">Build configuration transforms are run first.</span></span>

<span data-ttu-id="fb24e-115">Incluya un archivo *web.{CONFIGURATION}.config* para cada [configuración de compilación (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) que requiera una transformación de *web.config*.</span><span class="sxs-lookup"><span data-stu-id="fb24e-115">Include a *web.{CONFIGURATION}.config* file for each [build configuration (Debug|Release)](/dotnet/core/tools/dotnet-publish#options) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="fb24e-116">En el siguiente ejemplo, una variable de entorno específica de la configuración se establece en *web.Release.config*:</span><span class="sxs-lookup"><span data-stu-id="fb24e-116">In the following example, a configuration-specific environment variable is set in *web.Release.config*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Configuration_Specific" 
                               value="Configuration_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="fb24e-117">La transformación se aplica cuando la configuración se establece en *Release*:</span><span class="sxs-lookup"><span data-stu-id="fb24e-117">The transform is applied when the configuration is set to *Release*:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="fb24e-118">La propiedad MSBuild de la configuración es `$(Configuration)`.</span><span class="sxs-lookup"><span data-stu-id="fb24e-118">The MSBuild property for the configuration is `$(Configuration)`.</span></span>

## <a name="profile"></a><span data-ttu-id="fb24e-119">Perfil</span><span class="sxs-lookup"><span data-stu-id="fb24e-119">Profile</span></span>

<span data-ttu-id="fb24e-120">Las transformaciones del perfil se ejecutan en segundo lugar, una vez transformada la [configuración de compilación](#build-configuration).</span><span class="sxs-lookup"><span data-stu-id="fb24e-120">Profile transformations are run second, after [Build configuration](#build-configuration) transforms.</span></span>

<span data-ttu-id="fb24e-121">Incluya un archivo *web.{PROFILE}.config* para cada configuración de perfil que requiera una transformación de *web.config*.</span><span class="sxs-lookup"><span data-stu-id="fb24e-121">Include a *web.{PROFILE}.config* file for each profile configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="fb24e-122">En el siguiente ejemplo, una variable de entorno específica del perfil se establece en *web.FolderProfile.config* para un perfil de publicación de carpetas:</span><span class="sxs-lookup"><span data-stu-id="fb24e-122">In the following example, a profile-specific environment variable is set in *web.FolderProfile.config* for a folder publish profile:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Profile_Specific" 
                               value="Profile_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="fb24e-123">La transformación se aplica cuando el perfil es *FolderProfile*:</span><span class="sxs-lookup"><span data-stu-id="fb24e-123">The transform is applied when the profile is *FolderProfile*:</span></span>

```console
dotnet publish --configuration Release /p:PublishProfile=FolderProfile
```

<span data-ttu-id="fb24e-124">La propiedad MSBuild del nombre de perfil es `$(PublishProfile)`.</span><span class="sxs-lookup"><span data-stu-id="fb24e-124">The MSBuild property for the profile name is `$(PublishProfile)`.</span></span>

<span data-ttu-id="fb24e-125">Si no se pasa ningún perfil, el nombre de perfil predeterminado es **FileSystem** y *web.FileSystem.config* se aplica si el archivo está presente en la raíz del contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fb24e-125">If no profile is passed, the default profile name is **FileSystem** and *web.FileSystem.config* is applied if the file is present in the app's content root.</span></span>

## <a name="environment"></a><span data-ttu-id="fb24e-126">Entorno</span><span class="sxs-lookup"><span data-stu-id="fb24e-126">Environment</span></span>

<span data-ttu-id="fb24e-127">Las transformaciones del entorno se ejecutan en tercer lugar, una vez transformados la [configuración de compilación](#build-configuration) y el [perfil](#profile).</span><span class="sxs-lookup"><span data-stu-id="fb24e-127">Environment transformations are run third, after [Build configuration](#build-configuration) and [Profile](#profile) transforms.</span></span>

<span data-ttu-id="fb24e-128">Incluya un archivo *web.{ENVIRONMENT}.config* para cada [entorno](xref:fundamentals/environments) que requiera una transformación de *web.config*.</span><span class="sxs-lookup"><span data-stu-id="fb24e-128">Include a *web.{ENVIRONMENT}.config* file for each [environment](xref:fundamentals/environments) requiring a *web.config* transformation.</span></span>

<span data-ttu-id="fb24e-129">En el siguiente ejemplo, una variable de entorno específica del entorno se establece en *web.Production.config* para el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="fb24e-129">In the following example, a environment-specific environment variable is set in *web.Production.config* for the Production environment:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Environment_Specific" 
                               value="Environment_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="fb24e-130">La transformación se aplica cuando el entorno es *Production*:</span><span class="sxs-lookup"><span data-stu-id="fb24e-130">The transform is applied when the environment is *Production*:</span></span>

```console
dotnet publish --configuration Release /p:EnvironmentName=Production
```

<span data-ttu-id="fb24e-131">La propiedad MSBuild del entorno es `$(EnvironmentName)`.</span><span class="sxs-lookup"><span data-stu-id="fb24e-131">The MSBuild property for the environment is `$(EnvironmentName)`.</span></span>

<span data-ttu-id="fb24e-132">Al publicar desde Visual Studio y usar un perfil de publicación, consulte <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span><span class="sxs-lookup"><span data-stu-id="fb24e-132">When publishing from Visual Studio and using a publish profile, see <xref:host-and-deploy/visual-studio-publish-profiles#set-the-environment>.</span></span>

<span data-ttu-id="fb24e-133">La variable de entorno `ASPNETCORE_ENVIRONMENT` se agrega automáticamente al archivo *web.config* al especificarse el nombre del entorno.</span><span class="sxs-lookup"><span data-stu-id="fb24e-133">The `ASPNETCORE_ENVIRONMENT` environment variable is automatically added to the *web.config* file when the environment name is specified.</span></span>

## <a name="custom"></a><span data-ttu-id="fb24e-134">Personalizados</span><span class="sxs-lookup"><span data-stu-id="fb24e-134">Custom</span></span>

<span data-ttu-id="fb24e-135">Las transformaciones personalizadas se ejecutan en último lugar, una vez transformados la [configuración de compilación](#build-configuration), el [perfil](#profile) y el [entorno](#environment).</span><span class="sxs-lookup"><span data-stu-id="fb24e-135">Custom transformations are run last, after [Build configuration](#build-configuration), [Profile](#profile), and [Environment](#environment) transforms.</span></span>

<span data-ttu-id="fb24e-136">Incluya un archivo *{CUSTOM_NAME}.transform* para cada configuración personalizada que requiera una transformación de *web.config*.</span><span class="sxs-lookup"><span data-stu-id="fb24e-136">Include a *{CUSTOM_NAME}.transform* file for each custom configuration requiring a *web.config* transformation.</span></span>

<span data-ttu-id="fb24e-137">En el siguiente ejemplo, una variable de entorno de transformación personalizada se establece en *custom.transform*:</span><span class="sxs-lookup"><span data-stu-id="fb24e-137">In the following example, a custom transform environment variable is set in *custom.transform*:</span></span>

```xml
<?xml version="1.0"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <location>
    <system.webServer>
      <aspNetCore>
        <environmentVariables xdt:Transform="InsertIfMissing">
          <environmentVariable name="Custom_Specific" 
                               value="Custom_Specific_Value" 
                               xdt:Locator="Match(name)" 
                               xdt:Transform="InsertIfMissing" />
        </environmentVariables>
      </aspNetCore>
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="fb24e-138">La transformación se aplica cuando la propiedad `CustomTransformFileName` se pasa al comando [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="fb24e-138">The transform is applied when the `CustomTransformFileName` property is passed to the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

```console
dotnet publish --configuration Release /p:CustomTransformFileName=custom.transform
```

<span data-ttu-id="fb24e-139">La propiedad MSBuild del nombre de perfil es `$(CustomTransformFileName)`.</span><span class="sxs-lookup"><span data-stu-id="fb24e-139">The MSBuild property for the profile name is `$(CustomTransformFileName)`.</span></span>

## <a name="prevent-webconfig-transformation"></a><span data-ttu-id="fb24e-140">Evitar la transformación de web.config</span><span class="sxs-lookup"><span data-stu-id="fb24e-140">Prevent web.config transformation</span></span>

<span data-ttu-id="fb24e-141">Para evitar las transformaciones del archivo *web.config*, establezca la propiedad MSBuild `$(IsWebConfigTransformDisabled)`:</span><span class="sxs-lookup"><span data-stu-id="fb24e-141">To prevent transformations of the *web.config* file, set the MSBuild property `$(IsWebConfigTransformDisabled)`:</span></span>

```console
dotnet publish /p:IsWebConfigTransformDisabled=true
```

## <a name="additional-resources"></a><span data-ttu-id="fb24e-142">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fb24e-142">Additional resources</span></span>

* [<span data-ttu-id="fb24e-143">Sintaxis de transformación de Web.config para la implementación de proyectos de aplicación web</span><span class="sxs-lookup"><span data-stu-id="fb24e-143">Web.config Transformation Syntax for Web Application Project Deployment</span></span>](http://go.microsoft.com/fwlink/?LinkId=301874)
* <span data-ttu-id="fb24e-144">[Sintaxis de transformación de Web.config para la implementación de proyectos web usando Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span><span class="sxs-lookup"><span data-stu-id="fb24e-144">[Web.config Transformation Syntax for Web Project Deployment Using Visual Studio](https://docs.microsoft.com/previous-versions/aspnet/dd465326(v=vs.110))</span></span>