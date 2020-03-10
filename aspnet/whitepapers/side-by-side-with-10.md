---
uid: whitepapers/side-by-side-with-10
title: ASP.NET ejecución en paralelo de .NET Framework 1,0 y 1,1 | Microsoft Docs
author: rick-anderson
description: En estas notas del producto se describe cómo instalar .NET 1,0 y .NET 1,1 en el equipo, lo que permite que una aplicación Web ASP.NET se ejecute en cualquiera de las versiones de fotogramas...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513835"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="f783b-103">Ejecución en paralelo de ASP.NET de .NET Framework 1.0 y 1.1</span><span class="sxs-lookup"><span data-stu-id="f783b-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>

> <span data-ttu-id="f783b-104">En estas notas del producto se describe cómo instalar .NET 1,0 y .NET 1,1 en el equipo, lo que permite que una aplicación Web ASP.NET se ejecute en cualquier versión del marco.</span><span class="sxs-lookup"><span data-stu-id="f783b-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="f783b-105">Se aplica a ASP.NET 1,0 y ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>

<span data-ttu-id="f783b-106">En ASP.NET, se dice que las aplicaciones se ejecutan en paralelo cuando están instaladas en el mismo equipo, pero utilizan versiones diferentes de la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="f783b-107">En el siguiente tema se describe cómo configurar aplicaciones de ASP.NET para la ejecución en paralelo y se proporcionan pasos detallados para:</span><span class="sxs-lookup"><span data-stu-id="f783b-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="f783b-108">Mantenimiento de la asignación de la aplicación web a .NET Framework versión 1,0 durante la instalación</span><span class="sxs-lookup"><span data-stu-id="f783b-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="f783b-109">Asignar una aplicación web a una versión específica de la .NET Framework</span><span class="sxs-lookup"><span data-stu-id="f783b-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="f783b-110">Buscar la versión del .NET Framework que utiliza un sitio web</span><span class="sxs-lookup"><span data-stu-id="f783b-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="f783b-111">Tradicionalmente, cuando se actualiza un componente o una aplicación en un equipo, se quita la versión anterior y se reemplaza por la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="f783b-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="f783b-112">Si la nueva versión no es compatible con la versión anterior, normalmente interrumpe otras aplicaciones que usan el componente o la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f783b-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="f783b-113">El .NET Framework proporciona compatibilidad con la ejecución en paralelo, que permite instalar varias versiones de un ensamblado o una aplicación en el mismo equipo al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="f783b-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="f783b-114">Dado que se pueden instalar varias versiones simultáneamente, las aplicaciones administradas pueden seleccionar qué versión usar sin afectar a las aplicaciones que usan una versión diferente.</span><span class="sxs-lookup"><span data-stu-id="f783b-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="f783b-115">De forma predeterminada, durante la instalación de la versión 1,1 de .NET Framework, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente para usar la versión más reciente del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="f783b-116">Si no desea que las aplicaciones de ASP.NET tengan como valor predeterminado .NET Framework 1,1, haga clic [aquí](#1) para obtener información sobre cómo evitar esto durante la instalación.</span><span class="sxs-lookup"><span data-stu-id="f783b-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="f783b-117">Si actualiza el servidor Web a .NET Framework 1,1 y desea que una o varias aplicaciones web se ejecuten .NET Framework 1,0, debe actualizar la asignación de scripts de Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="f783b-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="f783b-118">La asignación de script es el mecanismo para asignar la extensión de archivo. aspx para una aplicación web específica a una versión de la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="f783b-119">Haga clic [aquí](#2) para obtener información sobre cómo asignar una aplicación web a una versión específica de la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="f783b-120">Puede usar Internet Information Manager o la herramienta de registro de IIS de ASP.NET (ASPNET\_regiis. exe) para averiguar qué .NET Framework versión está ejecutando una aplicación web determinada.</span><span class="sxs-lookup"><span data-stu-id="f783b-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="f783b-121">Haga clic [aquí](#3) para obtener información sobre cómo buscar la versión del .NET Framework que utiliza un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="f783b-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="f783b-122">Una consideración de importación al migrar a .NET Framework 1,1 es que cada versión de la .NET Framework utiliza su propio archivo Machine. config.</span><span class="sxs-lookup"><span data-stu-id="f783b-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="f783b-123">Como resultado, si un administrador web ha realizado cambios en el archivo Machine. config, esos cambios se deben migrar al archivo Machine. config de la .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="f783b-124">Mantenimiento de la asignación de la aplicación web a .NET Framework 1,0 durante la instalación</span><span class="sxs-lookup"><span data-stu-id="f783b-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="f783b-125">De forma predeterminada, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente durante la instalación para usar la versión más reciente de la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="f783b-126">Con la versión más reciente de la .NET Framework, las aplicaciones pueden aprovechar al máximo las mejoras y las nuevas características incluidas en la nueva versión.</span><span class="sxs-lookup"><span data-stu-id="f783b-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="f783b-127">Al mismo tiempo, el administrador web, que podría querer un control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones de ASP.NET existentes durante la instalación del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="f783b-128">Para evitar la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de la .NET Framework, el administrador web puede usar la opción de línea de comandos/noaspupgrade con el programa de instalación Dotnetfx. exe.</span><span class="sxs-lookup"><span data-stu-id="f783b-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="f783b-129">**Para evitar la reasignación total de la aplicación ASP.NET a la versión más reciente**</span><span class="sxs-lookup"><span data-stu-id="f783b-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="f783b-130">Vaya a **Inicio**.</span><span class="sxs-lookup"><span data-stu-id="f783b-130">Go to **Start**.</span></span>
2. <span data-ttu-id="f783b-131">Haga clic en **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="f783b-131">Click on **run**.</span></span>
3. <span data-ttu-id="f783b-132">Escriba **cmd**.</span><span class="sxs-lookup"><span data-stu-id="f783b-132">Type **cmd**.</span></span>
4. <span data-ttu-id="f783b-133">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f783b-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="f783b-134">En el símbolo del sistema, escriba la siguiente línea para iniciar la instalación del .NET Framework: **Dotnetfx. exe/c: "Install/noaspupgrade?** .</span><span class="sxs-lookup"><span data-stu-id="f783b-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="f783b-135">Haga clic en **sí** en el programa de instalación de Microsoft .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="f783b-136">Se iniciará el proceso de instalación del .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="f783b-137">Asignar una aplicación web a una versión específica de la .NET Framework</span><span class="sxs-lookup"><span data-stu-id="f783b-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="f783b-138">Cada versión del .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (ASPNET\_regiis. exe).</span><span class="sxs-lookup"><span data-stu-id="f783b-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="f783b-139">Esta herramienta permite a los administradores especificar que una aplicación web se ejecutará en una versión determinada del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="f783b-140">Esto se conoce como la asignación de una aplicación web a una versión de la .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f783b-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="f783b-141">Los administradores deben seleccionar el ASPNET\_regiis. exe que se corresponda con la versión de la .NET Framework que se asociará con la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="f783b-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="f783b-142">Por ejemplo, un administrador que desee especificar que un sitio web use .NET Framework 1,1 debe usar el ASPNET\_regiis. exe que viene con .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="f783b-143">ASPNET\_regiis. exe para la versión 1,0 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="f783b-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="f783b-144">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\_regiis</span><span class="sxs-lookup"><span data-stu-id="f783b-144">C:\WINDOWS\Microsoft.NET\Framework\\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="f783b-145">ASPNET\_regiis. exe para la versión 1, 1 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="f783b-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="f783b-146">C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis</span><span class="sxs-lookup"><span data-stu-id="f783b-146">C:\WINDOWS\Microsoft.NET\Framework\\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="f783b-147">ASPNET\_regiis. exe proporciona dos opciones para la asignación de scripts a una aplicación web:</span><span class="sxs-lookup"><span data-stu-id="f783b-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="f783b-148">**-s** establece el mapa de script en la ruta de acceso y en sus directorios secundarios.</span><span class="sxs-lookup"><span data-stu-id="f783b-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="f783b-149">**-SN** establece el mapa de script solo en la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="f783b-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="f783b-150">La ruta de acceso define la ruta de metadatos de IIS de la aplicación Web, que se define con el formato W3SVC/ROOT/{WebSiteNumber}/{nombre de la aplicación\_}.</span><span class="sxs-lookup"><span data-stu-id="f783b-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="f783b-151">Por ejemplo, para una aplicación web denominada portal ubicada en el sitio web predeterminado, la ruta de acceso de la metabase es W3SVC/1/ROOT/portal.</span><span class="sxs-lookup"><span data-stu-id="f783b-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="f783b-152">Nota también puede usar una herramienta denominada editor de metabase para obtener la ruta de la metabase.</span><span class="sxs-lookup"><span data-stu-id="f783b-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="f783b-153">Puede descargar esta herramienta desde el sitio de Soporte técnico de Microsoft en [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="f783b-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="f783b-154">Ejecute ASPNET\_regiis. exe-s W3SVC/1/ROOT/portal para actualizar la asignación de script IIS del portal y su subaplicación.</span><span class="sxs-lookup"><span data-stu-id="f783b-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="f783b-155">Ejecute ASPNET\_regiis. exe-SN W3SVC/1/ROOT/portal para actualizar el mapa de scripts de IIS del portal, sin afectar a las aplicaciones en el portal? subdirectorios.</span><span class="sxs-lookup"><span data-stu-id="f783b-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="f783b-156">Buscar la versión de .NET Framework que usa una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="f783b-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="f783b-157">Un administrador puede usar el Service Manager de Internet para buscar qué versión del .NET Framework ejecuta un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="f783b-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="f783b-158">Las distintas versiones de sistema operativo inician Internet Service Manager de forma diferente.</span><span class="sxs-lookup"><span data-stu-id="f783b-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="f783b-159">Para iniciar Service Manager, siga los pasos que se indican a continuación.</span><span class="sxs-lookup"><span data-stu-id="f783b-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="f783b-160">**Para iniciar Internet Service Manager**</span><span class="sxs-lookup"><span data-stu-id="f783b-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="f783b-161">Vaya a **Inicio**.</span><span class="sxs-lookup"><span data-stu-id="f783b-161">Go to **Start**.</span></span>
2. <span data-ttu-id="f783b-162">Haga clic en **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="f783b-162">Click on **run**.</span></span>
3. <span data-ttu-id="f783b-163">Escriba **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="f783b-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="f783b-164">En el Service Manager de Internet, seleccione la aplicación Web cuya versión de .NET Framework desea conocer.</span><span class="sxs-lookup"><span data-stu-id="f783b-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="f783b-165">Haga clic con el botón derecho en la aplicación web y haga clic en **propiedades.**</span><span class="sxs-lookup"><span data-stu-id="f783b-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="f783b-166">En la ventana de propiedades, seleccione **configuración.**</span><span class="sxs-lookup"><span data-stu-id="f783b-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="f783b-167">En la tabla asignación de aplicaciones, seleccione **. aspx**y haga clic en **Editar**.</span><span class="sxs-lookup"><span data-stu-id="f783b-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="f783b-168">En el cuadro de texto **ejecutable** , desplácese por el directorio de la versión.</span><span class="sxs-lookup"><span data-stu-id="f783b-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="f783b-169">Si el directorio de versión es v. 1.1.4322, la aplicación se asigna a .NET Framework 1,1.</span><span class="sxs-lookup"><span data-stu-id="f783b-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="f783b-170">Por el contrario, si el directorio de versiones es la versión 1.0.3705, la aplicación se asigna a .NET Framework 1,0.</span><span class="sxs-lookup"><span data-stu-id="f783b-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
