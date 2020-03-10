---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Agregando contenido al control de código fuente | Microsoft Docs
author: jrjlee
description: En este tema se explica cómo agregar contenido al control de código fuente en Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un equipo referen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515113"
---
# <a name="adding-content-to-source-control"></a><span data-ttu-id="e41a0-104">Agregar contenido al control de código fuente</span><span class="sxs-lookup"><span data-stu-id="e41a0-104">Adding Content to Source Control</span></span>

<span data-ttu-id="e41a0-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e41a0-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e41a0-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="e41a0-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e41a0-107">En este tema se explica cómo agregar contenido al control de código fuente en Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="e41a0-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="e41a0-108">Describe cómo agregar soluciones y proyectos a un proyecto de equipo en TFS, y explica cómo agregar dependencias externas, como marcos de trabajo o ensamblados, al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>

<span data-ttu-id="e41a0-109">Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="e41a0-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="e41a0-110">Información general sobre tareas</span><span class="sxs-lookup"><span data-stu-id="e41a0-110">Task Overview</span></span>

<span data-ttu-id="e41a0-111">En la mayoría de los casos, todos los miembros del equipo de desarrolladores deben poder agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="e41a0-112">Para agregar una solución al control de código fuente en TFS, debe completar estos pasos de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="e41a0-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="e41a0-113">Conéctese a un proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="e41a0-113">Connect to a team project.</span></span>
- <span data-ttu-id="e41a0-114">Asigne la estructura de carpetas del proyecto de equipo en el servidor a una estructura de carpetas en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="e41a0-115">Agregue la solución y su contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="e41a0-116">Agregue las dependencias externas al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="e41a0-117">En este tema se muestra cómo llevar a cabo estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="e41a0-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="e41a0-118">En las tareas y los tutoriales de este tema se supone que ya ha creado un nuevo proyecto de equipo de TFS para administrar el contenido.</span><span class="sxs-lookup"><span data-stu-id="e41a0-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="e41a0-119">Para obtener más información sobre cómo crear un nuevo proyecto de equipo, vea [crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="e41a0-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="e41a0-120">¿Quién realiza estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="e41a0-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="e41a0-121">En la mayoría de los casos, todos los miembros del equipo de desarrolladores deben poder agregar y modificar contenido dentro de proyectos de equipo específicos.</span><span class="sxs-lookup"><span data-stu-id="e41a0-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="e41a0-122">Conectarse a un proyecto de equipo y crear una asignación de carpetas</span><span class="sxs-lookup"><span data-stu-id="e41a0-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="e41a0-123">Antes de agregar contenido al control de código fuente, debe conectarse a un proyecto de equipo y crear una asignación entre la estructura de carpetas en el servidor y el sistema de archivos en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="e41a0-124">**Para conectarse a un proyecto de equipo y asignar una ruta de acceso local**</span><span class="sxs-lookup"><span data-stu-id="e41a0-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="e41a0-125">En la estación de trabajo del desarrollador, abra Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e41a0-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="e41a0-126">En Visual Studio, en el menú **equipo** , haga clic en **conectar a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e41a0-127">Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 3-6.</span><span class="sxs-lookup"><span data-stu-id="e41a0-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="e41a0-128">En el cuadro de diálogo **conexión al proyecto de equipo** , haga clic en **servidores**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="e41a0-129">En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="e41a0-130">En el cuadro de diálogo **agregar Team Foundation Server** , proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="e41a0-131">En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="e41a0-132">En el cuadro de diálogo **conectar al proyecto de equipo** , seleccione la instancia de TFS a la que desea conectarse, seleccione la colección de proyectos de equipo, seleccione el proyecto de equipo al que desea agregar y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="e41a0-133">En la ventana de **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="e41a0-134">En la pestaña **Explorador de control de código fuente** , haga clic en **sin asignar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="e41a0-135">En el cuadro de diálogo **asignar** , en el cuadro **carpeta local** , busque (o cree) una carpeta local que actúe como carpeta raíz para el proyecto de equipo y, a continuación, haga clic en **asignar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="e41a0-136">Cuando se le pida que descargue los archivos de origen, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="e41a0-137">En este punto, ha asignado la carpeta del servidor para el proyecto de equipo a una carpeta local en la estación de trabajo del desarrollador.</span><span class="sxs-lookup"><span data-stu-id="e41a0-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="e41a0-138">También ha descargado el contenido existente del proyecto de equipo en la estructura de carpetas local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="e41a0-139">Ahora puede empezar a agregar su propio contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="e41a0-140">Agregar proyectos y soluciones al control de código fuente</span><span class="sxs-lookup"><span data-stu-id="e41a0-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="e41a0-141">Para agregar proyectos y soluciones al control de código fuente, primero debe moverlos a la carpeta asignada del proyecto de equipo en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="e41a0-142">Después, puede proteger el contenido para sincronizar las adiciones con el servidor.</span><span class="sxs-lookup"><span data-stu-id="e41a0-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="e41a0-143">**Para agregar proyectos al control de código fuente**</span><span class="sxs-lookup"><span data-stu-id="e41a0-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="e41a0-144">En la estación de trabajo de desarrollador, mueva sus proyectos y soluciones a una ubicación adecuada dentro de la estructura de carpetas asignada para el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="e41a0-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e41a0-145">Muchas organizaciones tendrán un método preferido para la organización de proyectos y soluciones en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="e41a0-146">Para obtener instrucciones sobre cómo estructurar carpetas, consulte [Cómo: estructurar las carpetas de control de código fuente en Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="e41a0-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="e41a0-147">Abra la solución en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e41a0-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="e41a0-148">En la ventana **Explorador de soluciones** , haga clic con el botón secundario en la solución y, a continuación, haga clic en **Agregar solución al control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="e41a0-149">En algunos casos, en función de cómo le guste la organización el contenido de la estructura en TFS, puede que tenga que agregar proyectos al control de código fuente de forma individual para proporcionar un control más preciso sobre cómo se organiza el código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="e41a0-150">Compruebe que la pestaña **Explorador de control de código fuente** muestra el contenido que ha agregado en la estructura de carpetas del servidor para el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="e41a0-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="e41a0-151">En la pestaña **Explorador de control de código fuente** se muestra el contenido sin preguntar más porque se ha agregado la solución a una carpeta asignada en el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="e41a0-152">Si la solución estaba en una ubicación no asignada, se le pedirá que especifique las ubicaciones de carpeta en TFS y en el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="e41a0-153">En la pestaña **Explorador de control de código fuente** , en el panel **carpetas** , haga clic con el botón secundario en el proyecto de equipo (por ejemplo, **ContactManager**) y, a continuación, haga clic en **proteger los cambios pendientes**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="e41a0-154">En el cuadro de diálogo **proteger: archivos de origen** , escriba un comentario y, a continuación, haga clic en **proteger**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="e41a0-155">Llegados a este punto, ha agregado la solución al control de código fuente en TFS.</span><span class="sxs-lookup"><span data-stu-id="e41a0-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="e41a0-156">Agregar dependencias externas al control de código fuente</span><span class="sxs-lookup"><span data-stu-id="e41a0-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="e41a0-157">Al agregar un proyecto o una solución al control de código fuente, también se agregarán los archivos y las carpetas del proyecto o la solución.</span><span class="sxs-lookup"><span data-stu-id="e41a0-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="e41a0-158">Sin embargo, en muchos casos, los proyectos y las soluciones también dependen de las dependencias externas, como los ensamblados locales, para funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="e41a0-159">Debe agregar estos recursos a control de código fuente para permitir que Team build y otros miembros del equipo de desarrolladores compilen el código correctamente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="e41a0-160">Por ejemplo, la estructura de carpetas de la solución de ejemplo Contact Manager incluye una carpeta denominada packages.</span><span class="sxs-lookup"><span data-stu-id="e41a0-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="e41a0-161">Contiene el ensamblado y varios recursos de soporte para el ADO.NET Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="e41a0-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="e41a0-162">La carpeta Packages no forma parte de la solución Contact Manager, pero la solución no se compilará correctamente sin él.</span><span class="sxs-lookup"><span data-stu-id="e41a0-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="e41a0-163">Para habilitar Team Build para compilar la solución, debe agregar la carpeta Packages al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="e41a0-164">La inclusión de una carpeta de paquetes es habitual de lo que sucede cuando se agrega el Entity Framework, o recursos similares, a la solución con la extensión NuGet para Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e41a0-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>

<span data-ttu-id="e41a0-165">**Para agregar contenido que no sea del proyecto al control de código fuente**</span><span class="sxs-lookup"><span data-stu-id="e41a0-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="e41a0-166">Asegúrese de que los elementos que desea agregar (por ejemplo, la carpeta Packages) estén en una ubicación adecuada dentro de una carpeta asignada en el sistema de archivos local.</span><span class="sxs-lookup"><span data-stu-id="e41a0-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="e41a0-167">En Visual Studio 2010, en la ventana de **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **control de código fuente**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="e41a0-168">En la pestaña **Explorador de control de código fuente** , en el panel **carpetas** , seleccione la carpeta que contiene el elemento o elementos que desea agregar.</span><span class="sxs-lookup"><span data-stu-id="e41a0-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="e41a0-169">Haga clic en el botón **Agregar elementos a la carpeta** .</span><span class="sxs-lookup"><span data-stu-id="e41a0-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="e41a0-170">En el cuadro de diálogo **Agregar al control de código fuente** , seleccione la carpeta o los elementos que desee agregar y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="e41a0-171">En la pestaña **elementos excluidos** , seleccione los elementos necesarios que se han excluido automáticamente (por ejemplo, ensamblados) y, a continuación, haga clic en **incluir elementos**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="e41a0-172">En la pestaña **elementos para agregar** , compruebe que se muestran todos los archivos que desea incluir y, a continuación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="e41a0-173">En la ventana de **Explorador de control de código fuente** , haga clic en el botón **proteger** .</span><span class="sxs-lookup"><span data-stu-id="e41a0-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="e41a0-174">En el cuadro de diálogo **proteger: archivos de origen** , escriba un comentario y, a continuación, haga clic en **proteger**.</span><span class="sxs-lookup"><span data-stu-id="e41a0-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="e41a0-175">En este punto, ha agregado las dependencias externas de la solución al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e41a0-176">Conclusión</span><span class="sxs-lookup"><span data-stu-id="e41a0-176">Conclusion</span></span>

<span data-ttu-id="e41a0-177">En este tema se describe cómo conectarse a un proyecto de equipo, asignar una estructura de carpetas y agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="e41a0-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="e41a0-178">Para obtener más información sobre cómo trabajar con elementos bajo control de código fuente, vea [usar el control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="e41a0-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="e41a0-179">En el tema siguiente, [configurar un servidor de compilación de TFS para la implementación web](configuring-a-tfs-build-server-for-web-deployment.md), se describe cómo preparar un servidor TFS Team Build para compilar e implementar la solución.</span><span class="sxs-lookup"><span data-stu-id="e41a0-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="e41a0-180">Información adicional</span><span class="sxs-lookup"><span data-stu-id="e41a0-180">Further Reading</span></span>

<span data-ttu-id="e41a0-181">Para obtener información más completa sobre cómo trabajar con el control de código fuente en TFS, vea [usar el control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="e41a0-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e41a0-182">[Anterior](creating-a-team-project-in-tfs.md)
> [Siguiente](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="e41a0-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
