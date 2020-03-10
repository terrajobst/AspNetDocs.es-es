---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Crear un proyecto de equipo en TFS | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519565"
---
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="10c6f-103">Crear un proyecto de equipo en TFS</span><span class="sxs-lookup"><span data-stu-id="10c6f-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="10c6f-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="10c6f-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="10c6f-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="10c6f-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="10c6f-106">En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="10c6f-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="10c6f-107">Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="10c6f-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="10c6f-108">Información general sobre tareas</span><span class="sxs-lookup"><span data-stu-id="10c6f-108">Task Overview</span></span>

<span data-ttu-id="10c6f-109">Para aprovisionar y usar un nuevo proyecto de equipo en TFS, debe completar estos pasos de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="10c6f-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="10c6f-110">Conceda permisos al usuario que va a crear el nuevo proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="10c6f-111">Cree el proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-111">Create the team project.</span></span>
- <span data-ttu-id="10c6f-112">Conceda permisos a los miembros del equipo que vayan a trabajar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="10c6f-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="10c6f-113">Proteja algún contenido.</span><span class="sxs-lookup"><span data-stu-id="10c6f-113">Check in some content.</span></span>

<span data-ttu-id="10c6f-114">En este tema se mostrará cómo realizar estos procedimientos, y se identificarán los usuarios y los roles de trabajo que es probable que sean responsables de cada procedimiento.</span><span class="sxs-lookup"><span data-stu-id="10c6f-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="10c6f-115">Tenga en cuenta que, en función de la estructura de su organización, cada una de estas tareas puede ser responsabilidad de una persona distinta.</span><span class="sxs-lookup"><span data-stu-id="10c6f-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="10c6f-116">En las tareas y los tutoriales de este tema se supone que ha instalado y configurado TFS, y que ha creado una colección de proyectos de equipo como parte del proceso de configuración.</span><span class="sxs-lookup"><span data-stu-id="10c6f-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="10c6f-117">Para obtener más información sobre estas suposiciones y obtener información general sobre el escenario, vea [configurar un servidor de compilación de TFS para la implementación web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="10c6f-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="10c6f-118">Conceder permisos al creador del proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="10c6f-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="10c6f-119">Para crear un nuevo proyecto de equipo, necesitará estos permisos:</span><span class="sxs-lookup"><span data-stu-id="10c6f-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="10c6f-120">Debe tener el permiso **crear nuevos proyectos** en la capa de aplicación de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="10c6f-121">Normalmente, para conceder este permiso, agregue usuarios al grupo administradores de la **colección de proyectos** de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="10c6f-122">El grupo global **Team Foundation Administrators** también incluye este permiso.</span><span class="sxs-lookup"><span data-stu-id="10c6f-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="10c6f-123">Debe tener permiso para crear nuevos sitios de grupo en la colección de sitios de SharePoint que se corresponda con la colección de proyectos de equipo de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="10c6f-124">Normalmente, este permiso se concede agregando el usuario a un grupo de SharePoint con derechos de **control total** en la colección de sitios de SharePoint.</span><span class="sxs-lookup"><span data-stu-id="10c6f-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="10c6f-125">Si está utilizando SQL Server Reporting Services características, debe ser miembro del rol **Administrador de contenido de Team Foundation** en Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="10c6f-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="10c6f-126">¿Quién realiza estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="10c6f-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="10c6f-127">Normalmente, la persona o el grupo que administra la implementación de TFS también realiza estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="10c6f-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="10c6f-128">Dado que se trata de un conjunto de permisos con privilegios elevados, los nuevos proyectos de equipo los crean normalmente un pequeño subconjunto de usuarios con la responsabilidad de administrar una implementación de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="10c6f-129">Normalmente, a los desarrolladores no se les concederán los permisos necesarios para crear nuevos proyectos de equipo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="10c6f-130">Conceder permisos en TFS</span><span class="sxs-lookup"><span data-stu-id="10c6f-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="10c6f-131">Si desea permitir que un usuario cree nuevos proyectos de equipo, la primera tarea de alto nivel es agregar el usuario al grupo administradores de la **colección de proyectos** para la colección de proyectos de equipo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="10c6f-132">**Para agregar un usuario al grupo administradores de la colección de proyectos**</span><span class="sxs-lookup"><span data-stu-id="10c6f-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="10c6f-133">En el servidor de TFS, en el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft Team Foundation Server 2010**y, a continuación, haga clic en **consola de administración de Team Foundation**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="10c6f-134">En la vista de árbol de navegación, expanda **capa de aplicación**y, a continuación, haga clic en colecciones de **proyectos de equipo**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="10c6f-135">En el panel **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.</span><span class="sxs-lookup"><span data-stu-id="10c6f-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="10c6f-136">En la pestaña **General** , haga clic en **pertenencia a grupos**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="10c6f-137">En el cuadro de diálogo **grupos globales** , seleccione el grupo administradores de la **colección de proyectos** y, a continuación, haga clic en **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="10c6f-138">En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , seleccione **usuario o grupo de Windows**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="10c6f-139">En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre de usuario del usuario que desea que pueda crear nuevos proyectos de equipo, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="10c6f-140">En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="10c6f-141">En el cuadro de diálogo **grupos globales** , haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="10c6f-142">Conceder permisos en SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="10c6f-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="10c6f-143">A continuación, debe conceder al usuario permiso para crear nuevos sitios de grupo en la colección de sitios de SharePoint que se corresponda con la colección de proyectos de equipo de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="10c6f-144">**Para conceder permisos de control total en la colección de sitios de SharePoint**</span><span class="sxs-lookup"><span data-stu-id="10c6f-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="10c6f-145">En el consola de administración de Team Foundation Server, en la página **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.</span><span class="sxs-lookup"><span data-stu-id="10c6f-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="10c6f-146">En la pestaña **sitio de SharePoint** , anote el valor de la dirección URL predeterminada de la ubicación del **sitio actual** .</span><span class="sxs-lookup"><span data-stu-id="10c6f-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="10c6f-147">Abra Internet Explorer y, a continuación, vaya a la dirección URL que anotó en el paso 2.</span><span class="sxs-lookup"><span data-stu-id="10c6f-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10c6f-148">Si no ha iniciado sesión en Windows como el usuario que creó la colección de proyectos de equipo, debe iniciar sesión en SharePoint como este usuario para poder continuar.</span><span class="sxs-lookup"><span data-stu-id="10c6f-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="10c6f-149">En el menú **Acciones del sitio** , haga clic en **Configuración del sitio**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="10c6f-150">En la página **configuración del sitio** , en **usuarios y permisos**, haga clic en **personas y grupos**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="10c6f-151">En el panel de navegación izquierdo, haga clic en **grupos**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="10c6f-152">En la página **personas y grupos: todos los grupos** , haga clic en **configurar grupos para este sitio**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="10c6f-153">Es posible que reciba un error <strong>HTTP 404 no encontrado</strong> debido a un error de codificación http doble.</span><span class="sxs-lookup"><span data-stu-id="10c6f-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="10c6f-154">Si esto ocurre, reemplace la dirección URL por esta:</span><span class="sxs-lookup"><span data-stu-id="10c6f-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="10c6f-155">Por ejemplo: `[site_collection_URL]/_layouts/permsetup.aspx`</span><span class="sxs-lookup"><span data-stu-id="10c6f-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="10c6f-156">En la página **configurar grupos para este sitio** , agregue el usuario que creará los proyectos de equipo al grupo **propietarios** y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="10c6f-157">Para obtener más información sobre cómo permitir a los usuarios crear nuevos proyectos de equipo en una colección de proyectos de equipo, vea [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="10c6f-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="10c6f-158">Crear un nuevo proyecto de equipo y agregar usuarios</span><span class="sxs-lookup"><span data-stu-id="10c6f-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="10c6f-159">Una vez que tenga los permisos necesarios, puede usar la ventana de **Team Explorer** de Visual Studio 2010 para crear un nuevo proyecto de equipo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="10c6f-160">Este enfoque proporciona un asistente que recopila toda la información necesaria y realiza las tareas necesarias en TFS, SharePoint y SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="10c6f-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="10c6f-161">También deberá conceder permisos en el nuevo proyecto de equipo a los miembros del equipo de desarrolladores para permitirles agregar y modificar el contenido.</span><span class="sxs-lookup"><span data-stu-id="10c6f-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="10c6f-162">¿Quién realiza estos procedimientos?</span><span class="sxs-lookup"><span data-stu-id="10c6f-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="10c6f-163">Normalmente, un administrador de TFS o un jefe de equipo de desarrolladores realiza estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="10c6f-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="10c6f-164">Crear un nuevo proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="10c6f-164">Create a New Team Project</span></span>

<span data-ttu-id="10c6f-165">En el procedimiento siguiente se describe cómo crear un nuevo proyecto de equipo en TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="10c6f-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="10c6f-166">**Para crear un nuevo proyecto de equipo**</span><span class="sxs-lookup"><span data-stu-id="10c6f-166">**To create a new team project**</span></span>

1. <span data-ttu-id="10c6f-167">En el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic con el botón secundario en **Microsoft Visual Studio 2010**y, a continuación, haga clic en **Ejecutar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10c6f-168">Si no ejecuta Visual Studio 2010 como administrador, se producirá un error en el Asistente para nuevo proyecto de equipo en el último paso.</span><span class="sxs-lookup"><span data-stu-id="10c6f-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="10c6f-169">Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="10c6f-170">En Visual Studio, en el menú **equipo** , haga clic en **conectar a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10c6f-171">Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 4-7.</span><span class="sxs-lookup"><span data-stu-id="10c6f-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="10c6f-172">En el cuadro de diálogo **conexión al proyecto de equipo** , haga clic en **servidores**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="10c6f-173">En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="10c6f-174">En el cuadro de diálogo **agregar Team Foundation Server** , proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="10c6f-175">En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="10c6f-176">En el cuadro de diálogo **conectar al proyecto de equipo** , seleccione la instancia de TFS a la que desea conectarse, seleccione la colección de proyectos de equipo a la que desea agregar y, a continuación, haga clic en **conectar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="10c6f-177">En la ventana **Team Explorer** , haga clic con el botón secundario en la colección de proyectos de equipo y, a continuación, haga clic en **nuevo proyecto de equipo**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="10c6f-178">En el cuadro de diálogo **nuevo proyecto de equipo** , proporcione un nombre y una descripción para el proyecto de equipo y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10c6f-179">Si el proyecto de equipo incluye espacios, puede que surjan algunos problemas cuando use la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para implementar paquetes desde la ruta de acceso de salida.</span><span class="sxs-lookup"><span data-stu-id="10c6f-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="10c6f-180">Los espacios en la ruta de acceso pueden hacer que sea mucho más difícil ejecutar comandos Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="10c6f-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="10c6f-181">En la página **seleccionar una plantilla de proceso** , seleccione la plantilla de proceso que desea usar para administrar el proceso de desarrollo y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="10c6f-182">Para obtener más información acerca de las plantillas de proceso de TFS, consulte [herramientas y plantillas de proceso](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="10c6f-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="10c6f-183">En la página **configuración del sitio de grupo** , deje la configuración predeterminada sin cambios y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="10c6f-184">Esta configuración crea o identifica un sitio de grupo de SharePoint que está asociado al proyecto de equipo de TFS.</span><span class="sxs-lookup"><span data-stu-id="10c6f-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="10c6f-185">El equipo de desarrollo puede utilizar este sitio para administrar la documentación, participar en los hilos de discusión, crear páginas wiki y realizar otras tareas que no están relacionadas con el código.</span><span class="sxs-lookup"><span data-stu-id="10c6f-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="10c6f-186">Para obtener más información, vea [interacciones entre productos de SharePoint y Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="10c6f-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="10c6f-187">En la página **especificar configuración de control de código fuente** , deje la configuración predeterminada sin cambios y, a continuación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="10c6f-188">Esta configuración identifica o crea la ubicación en la jerarquía de carpetas de TFS que actuará como carpeta raíz para el contenido.</span><span class="sxs-lookup"><span data-stu-id="10c6f-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="10c6f-189">En la página **confirmar configuración del proyecto de equipo** , haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="10c6f-190">Cuando el nuevo proyecto de equipo se cree correctamente, en la página **proyecto de equipo creado** , haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="10c6f-191">Agregar usuarios a un proyecto de equipo</span><span class="sxs-lookup"><span data-stu-id="10c6f-191">Add Users to a Team Project</span></span>

<span data-ttu-id="10c6f-192">Ahora que ha creado el nuevo proyecto de equipo, puede conceder permisos a los usuarios para que puedan empezar a agregar y colaborar en el contenido.</span><span class="sxs-lookup"><span data-stu-id="10c6f-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="10c6f-193">**Para agregar usuarios a un proyecto de equipo**</span><span class="sxs-lookup"><span data-stu-id="10c6f-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="10c6f-194">En Visual Studio 2010, en la ventana de **Team Explorer** , haga clic con el botón secundario en el proyecto de equipo, seleccione **configuración del proyecto de equipo**y, a continuación, haga clic en **pertenencia a grupos**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="10c6f-195">Para permitir que un usuario agregue, modifique y quite código bajo control de código fuente, debe agregarlo al grupo **Contributors** .</span><span class="sxs-lookup"><span data-stu-id="10c6f-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="10c6f-196">En el cuadro de diálogo **grupos de proyectos** , seleccione el grupo **Contributors** y, a continuación, haga clic en **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="10c6f-197">En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , seleccione **usuario o grupo de Windows**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="10c6f-198">En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre de usuario del usuario que desea agregar al proyecto de equipo, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="10c6f-199">En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="10c6f-200">En el cuadro de diálogo **grupos de proyectos** , haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="10c6f-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="10c6f-201">Conclusión</span><span class="sxs-lookup"><span data-stu-id="10c6f-201">Conclusion</span></span>

<span data-ttu-id="10c6f-202">En este momento, el nuevo proyecto de equipo está listo para usarse y el equipo de desarrolladores puede empezar a agregar contenido y colaborar en el proceso de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="10c6f-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="10c6f-203">En el tema siguiente, [agregar contenido al control de código fuente](adding-content-to-source-control.md), se describe cómo agregar contenido al control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="10c6f-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="10c6f-204">Información adicional</span><span class="sxs-lookup"><span data-stu-id="10c6f-204">Further Reading</span></span>

<span data-ttu-id="10c6f-205">Para obtener instrucciones más amplias sobre cómo crear proyectos de equipo en TFS, vea [crear un proyecto de equipo](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="10c6f-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="10c6f-206">Para obtener más información sobre cómo permitir a los usuarios crear nuevos proyectos de equipo en una colección de proyectos de equipo, vea [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="10c6f-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="10c6f-207">Para obtener más información sobre cómo agregar usuarios a proyectos de equipo, vea [Agregar usuarios a proyectos de equipo](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="10c6f-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10c6f-208">[Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Siguiente](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="10c6f-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
