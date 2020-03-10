---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Crear y ejecutar un archivo de comandos de implementación | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo compilar un archivo de comandos que le permitirá ejecutar una implementación mediante archivos de proyecto de Microsoft Build Engine (MSBuild) como un solo paso,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514981"
---
# <a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="b1554-103">Crear y ejecutar un archivo de comandos de implementación</span><span class="sxs-lookup"><span data-stu-id="b1554-103">Creating and Running a Deployment Command File</span></span>

<span data-ttu-id="b1554-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b1554-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b1554-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="b1554-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b1554-106">En este tema se describe cómo crear un archivo de comandos que le permita ejecutar una implementación mediante archivos de proyecto de Microsoft Build Engine (MSBuild) como un proceso repetible de un solo paso.</span><span class="sxs-lookup"><span data-stu-id="b1554-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>

<span data-ttu-id="b1554-107">Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de&#x2014;ejemplo de la solución [Contact Manager](the-contact-manager-solution.md) para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b1554-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="b1554-108">El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del proceso de compilación](understanding-the-build-process.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno.</span><span class="sxs-lookup"><span data-stu-id="b1554-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="b1554-109">En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.</span><span class="sxs-lookup"><span data-stu-id="b1554-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="b1554-110">Información general del proceso</span><span class="sxs-lookup"><span data-stu-id="b1554-110">Process Overview</span></span>

<span data-ttu-id="b1554-111">En este tema, aprenderá a crear y ejecutar un archivo de comandos que usa estos archivos de proyecto para realizar una implementación repetible en el entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="b1554-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="b1554-112">Esencialmente, el archivo de comandos simplemente debe contener un comando de MSBuild que:</span><span class="sxs-lookup"><span data-stu-id="b1554-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="b1554-113">Indica a MSBuild que ejecute el archivo *Publish. proj* independiente del entorno.</span><span class="sxs-lookup"><span data-stu-id="b1554-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="b1554-114">Indica al archivo *Publish. proj* qué archivo contiene la configuración de proyecto específica del entorno y dónde se encuentra.</span><span class="sxs-lookup"><span data-stu-id="b1554-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="b1554-115">Crear un comando de MSBuild</span><span class="sxs-lookup"><span data-stu-id="b1554-115">Create an MSBuild Command</span></span>

<span data-ttu-id="b1554-116">Como se describe [en Descripción del proceso de compilación](understanding-the-build-process.md), el archivo&#x2014;de proyecto específico del entorno, por ejemplo, *env-dev. proj*&#x2014;está diseñado para importarse en el archivo *Publish. proj* independiente del entorno en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="b1554-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="b1554-117">Juntos, estos dos archivos proporcionan un conjunto completo de instrucciones que indican a MSBuild cómo compilar e implementar la solución.</span><span class="sxs-lookup"><span data-stu-id="b1554-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="b1554-118">El archivo *Publish. proj* usa un elemento **Import** para importar el archivo de proyecto específico del entorno.</span><span class="sxs-lookup"><span data-stu-id="b1554-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

<span data-ttu-id="b1554-119">Como tal, al usar MSBuild. exe para compilar e implementar la solución Contact Manager, necesitará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1554-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="b1554-120">Ejecute MSBuild. exe en el archivo *Publish. proj* .</span><span class="sxs-lookup"><span data-stu-id="b1554-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="b1554-121">Especifique la ubicación del archivo de proyecto específico del entorno proporcionando un parámetro de línea de comandos denominado **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="b1554-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="b1554-122">Para ello, el comando de MSBuild debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1554-122">To do this, your MSBuild command should resemble this:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

<span data-ttu-id="b1554-123">Desde aquí, es un paso sencillo para pasar a una implementación repetible y de un solo paso.</span><span class="sxs-lookup"><span data-stu-id="b1554-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="b1554-124">Lo único que debe hacer es agregar el comando de MSBuild a un archivo. cmd.</span><span class="sxs-lookup"><span data-stu-id="b1554-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="b1554-125">En la solución Contact Manager, la carpeta Publish incluye un archivo llamado *Publish-dev. cmd* que hace exactamente esto.</span><span class="sxs-lookup"><span data-stu-id="b1554-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> <span data-ttu-id="b1554-126">El modificador **/FL** indica a MSBuild que cree un archivo de registro denominado *MSBuild. log* en el directorio de trabajo en el que se invocó MSBuild. exe.</span><span class="sxs-lookup"><span data-stu-id="b1554-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>

<span data-ttu-id="b1554-127">Para implementar o volver a implementar la solución Contact Manager, lo único que debe hacer es ejecutar el archivo *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="b1554-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="b1554-128">Al ejecutar el archivo, MSBuild hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b1554-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="b1554-129">Compilar todos los proyectos de la solución.</span><span class="sxs-lookup"><span data-stu-id="b1554-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="b1554-130">Generar paquetes web que se pueden implementar para los proyectos de aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="b1554-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="b1554-131">Genere archivos. dbschema y. DeployManifest para los proyectos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b1554-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="b1554-132">Implemente los paquetes Web en el servidor Web.</span><span class="sxs-lookup"><span data-stu-id="b1554-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="b1554-133">Implemente la base de datos en el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b1554-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="b1554-134">Ejecutar la implementación</span><span class="sxs-lookup"><span data-stu-id="b1554-134">Run the Deployment</span></span>

<span data-ttu-id="b1554-135">Cuando haya creado un archivo de comandos para el entorno de destino, podrá completar toda la implementación con solo ejecutar el archivo.</span><span class="sxs-lookup"><span data-stu-id="b1554-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="b1554-136">**Para implementar la solución Contact Manager en el entorno de prueba**</span><span class="sxs-lookup"><span data-stu-id="b1554-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="b1554-137">En la estación de trabajo del desarrollador, abra el explorador de Windows y, a continuación, busque la ubicación del archivo *Publish-dev. cmd* .</span><span class="sxs-lookup"><span data-stu-id="b1554-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="b1554-138">Haga doble clic en el archivo para ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="b1554-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="b1554-139">Si aparece un cuadro de diálogo **Abrir archivo: ADVERTENCIA de seguridad** , haga clic en **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="b1554-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="b1554-140">Si las opciones de configuración y los servidores de prueba están configurados correctamente, la ventana del símbolo del sistema mostrará un mensaje **compilar correctamente** cuando MSBuild haya terminado de procesar los archivos del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b1554-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="b1554-141">Si es la primera vez que ha implementado la solución en este entorno, deberá agregar la cuenta de equipo del servidor Web de prueba a los roles **db\_** **datareader y DB\_DataReader** en la base de datos **ContactManager** .</span><span class="sxs-lookup"><span data-stu-id="b1554-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="b1554-142">Este procedimiento se describe en [configurar un servidor de base de datos para la publicación de web deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="b1554-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="b1554-143">Solo tiene que asignar estos permisos al crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b1554-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="b1554-144">De forma predeterminada, el proceso de compilación no volverá a crear la base de&#x2014;datos en cada implementación, sino que comparará la base de datos existente con el esquema más reciente y hará solo los cambios necesarios.</span><span class="sxs-lookup"><span data-stu-id="b1554-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="b1554-145">Como resultado, solo necesitará asignar estos roles de base de datos la primera vez que implemente la solución.</span><span class="sxs-lookup"><span data-stu-id="b1554-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="b1554-146">Abra Internet Explorer y vaya a la dirección URL de la aplicación Contact Manager (por ejemplo, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="b1554-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="b1554-147">Compruebe que la aplicación funciona según lo previsto y que puede agregar contactos.</span><span class="sxs-lookup"><span data-stu-id="b1554-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="b1554-148">Conclusión</span><span class="sxs-lookup"><span data-stu-id="b1554-148">Conclusion</span></span>

<span data-ttu-id="b1554-149">La creación de un archivo de comandos que contenga las instrucciones de MSBuild le proporciona una forma rápida y sencilla de crear e implementar una solución de varios proyectos en un entorno de destino específico.</span><span class="sxs-lookup"><span data-stu-id="b1554-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="b1554-150">Si necesita implementar la solución repetidamente en varios entornos de destino, puede crear varios archivos de comandos.</span><span class="sxs-lookup"><span data-stu-id="b1554-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="b1554-151">En cada archivo de comandos, el comando MSBuild compilará el mismo archivo de proyecto universal, pero especificará un archivo de proyecto específico del entorno diferente.</span><span class="sxs-lookup"><span data-stu-id="b1554-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="b1554-152">Por ejemplo, un archivo de comandos para publicar en un entorno de desarrollo o de prueba podría contener este comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b1554-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

<span data-ttu-id="b1554-153">Un archivo de comandos para publicar en un entorno de ensayo podría contener este comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b1554-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> <span data-ttu-id="b1554-154">Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="b1554-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>

<span data-ttu-id="b1554-155">También puede personalizar el proceso de compilación para cada entorno invalidando las propiedades o estableciendo otros modificadores en el comando de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="b1554-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="b1554-156">Para obtener más información, vea referencia de la [línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1554-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b1554-157">[Anterior](deploying-database-projects.md)
> [Siguiente](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="b1554-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
