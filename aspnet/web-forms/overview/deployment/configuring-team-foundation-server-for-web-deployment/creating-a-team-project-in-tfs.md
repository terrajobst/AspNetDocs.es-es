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
# <a name="creating-a-team-project-in-tfs"></a>Crear un proyecto de equipo en TFS

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

Para aprovisionar y usar un nuevo proyecto de equipo en TFS, debe completar estos pasos de alto nivel:

- Conceda permisos al usuario que va a crear el nuevo proyecto de equipo.
- Cree el proyecto de equipo.
- Conceda permisos a los miembros del equipo que vayan a trabajar en el proyecto.
- Proteja algún contenido.

En este tema se mostrará cómo realizar estos procedimientos, y se identificarán los usuarios y los roles de trabajo que es probable que sean responsables de cada procedimiento. Tenga en cuenta que, en función de la estructura de su organización, cada una de estas tareas puede ser responsabilidad de una persona distinta.

En las tareas y los tutoriales de este tema se supone que ha instalado y configurado TFS, y que ha creado una colección de proyectos de equipo como parte del proceso de configuración. Para obtener más información sobre estas suposiciones y obtener información general sobre el escenario, vea [configurar un servidor de compilación de TFS para la implementación web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Conceder permisos al creador del proyecto de equipo

Para crear un nuevo proyecto de equipo, necesitará estos permisos:

- Debe tener el permiso **crear nuevos proyectos** en la capa de aplicación de TFS. Normalmente, para conceder este permiso, agregue usuarios al grupo administradores de la **colección de proyectos** de TFS. El grupo global **Team Foundation Administrators** también incluye este permiso.
- Debe tener permiso para crear nuevos sitios de grupo en la colección de sitios de SharePoint que se corresponda con la colección de proyectos de equipo de TFS. Normalmente, este permiso se concede agregando el usuario a un grupo de SharePoint con derechos de **control total** en la colección de sitios de SharePoint.
- Si está utilizando SQL Server Reporting Services características, debe ser miembro del rol **Administrador de contenido de Team Foundation** en Reporting Services.

### <a name="who-performs-these-procedures"></a>¿Quién realiza estos procedimientos?

Normalmente, la persona o el grupo que administra la implementación de TFS también realiza estos procedimientos.

Dado que se trata de un conjunto de permisos con privilegios elevados, los nuevos proyectos de equipo los crean normalmente un pequeño subconjunto de usuarios con la responsabilidad de administrar una implementación de TFS. Normalmente, a los desarrolladores no se les concederán los permisos necesarios para crear nuevos proyectos de equipo.

### <a name="grant-permissions-in-tfs"></a>Conceder permisos en TFS

Si desea permitir que un usuario cree nuevos proyectos de equipo, la primera tarea de alto nivel es agregar el usuario al grupo administradores de la **colección de proyectos** para la colección de proyectos de equipo.

**Para agregar un usuario al grupo administradores de la colección de proyectos**

1. En el servidor de TFS, en el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft Team Foundation Server 2010**y, a continuación, haga clic en **consola de administración de Team Foundation**.
2. En la vista de árbol de navegación, expanda **capa de aplicación**y, a continuación, haga clic en colecciones de **proyectos de equipo**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. En el panel **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. En la pestaña **General** , haga clic en **pertenencia a grupos**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. En el cuadro de diálogo **grupos globales** , seleccione el grupo administradores de la **colección de proyectos** y, a continuación, haga clic en **propiedades**.
6. En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , seleccione **usuario o grupo de Windows**y, a continuación, haga clic en **Agregar**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre de usuario del usuario que desea que pueda crear nuevos proyectos de equipo, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , haga clic en **Aceptar**.
9. En el cuadro de diálogo **grupos globales** , haga clic en **cerrar**.

### <a name="grant-permissions-in-sharepoint-services"></a>Conceder permisos en SharePoint Services

A continuación, debe conceder al usuario permiso para crear nuevos sitios de grupo en la colección de sitios de SharePoint que se corresponda con la colección de proyectos de equipo de TFS.

**Para conceder permisos de control total en la colección de sitios de SharePoint**

1. En el consola de administración de Team Foundation Server, en la página **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.
2. En la pestaña **sitio de SharePoint** , anote el valor de la dirección URL predeterminada de la ubicación del **sitio actual** .

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Abra Internet Explorer y, a continuación, vaya a la dirección URL que anotó en el paso 2.

    > [!NOTE]
    > Si no ha iniciado sesión en Windows como el usuario que creó la colección de proyectos de equipo, debe iniciar sesión en SharePoint como este usuario para poder continuar.
4. En el menú **Acciones del sitio** , haga clic en **Configuración del sitio**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. En la página **configuración del sitio** , en **usuarios y permisos**, haga clic en **personas y grupos**.
6. En el panel de navegación izquierdo, haga clic en **grupos**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. En la página **personas y grupos: todos los grupos** , haga clic en **configurar grupos para este sitio**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Es posible que reciba un error <strong>HTTP 404 no encontrado</strong> debido a un error de codificación http doble. Si esto ocurre, reemplace la dirección URL por esta:   
   > Por ejemplo: `[site_collection_URL]/_layouts/permsetup.aspx`  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. En la página **configurar grupos para este sitio** , agregue el usuario que creará los proyectos de equipo al grupo **propietarios** y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Para obtener más información sobre cómo permitir a los usuarios crear nuevos proyectos de equipo en una colección de proyectos de equipo, vea [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Crear un nuevo proyecto de equipo y agregar usuarios

Una vez que tenga los permisos necesarios, puede usar la ventana de **Team Explorer** de Visual Studio 2010 para crear un nuevo proyecto de equipo. Este enfoque proporciona un asistente que recopila toda la información necesaria y realiza las tareas necesarias en TFS, SharePoint y SQL Server Reporting Services. También deberá conceder permisos en el nuevo proyecto de equipo a los miembros del equipo de desarrolladores para permitirles agregar y modificar el contenido.

### <a name="who-performs-these-procedures"></a>¿Quién realiza estos procedimientos?

Normalmente, un administrador de TFS o un jefe de equipo de desarrolladores realiza estos procedimientos.

### <a name="create-a-new-team-project"></a>Crear un nuevo proyecto de equipo

En el procedimiento siguiente se describe cómo crear un nuevo proyecto de equipo en TFS 2010.

**Para crear un nuevo proyecto de equipo**

1. En el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic con el botón secundario en **Microsoft Visual Studio 2010**y, a continuación, haga clic en **Ejecutar como administrador**.

    > [!NOTE]
    > Si no ejecuta Visual Studio 2010 como administrador, se producirá un error en el Asistente para nuevo proyecto de equipo en el último paso.
2. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.
3. En Visual Studio, en el menú **equipo** , haga clic en **conectar a Team Foundation Server**.

    > [!NOTE]
    > Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 4-7.
4. En el cuadro de diálogo **conexión al proyecto de equipo** , haga clic en **servidores**.
5. En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **Agregar**.
6. En el cuadro de diálogo **agregar Team Foundation Server** , proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **cerrar**.
8. En el cuadro de diálogo **conectar al proyecto de equipo** , seleccione la instancia de TFS a la que desea conectarse, seleccione la colección de proyectos de equipo a la que desea agregar y, a continuación, haga clic en **conectar**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. En la ventana **Team Explorer** , haga clic con el botón secundario en la colección de proyectos de equipo y, a continuación, haga clic en **nuevo proyecto de equipo**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. En el cuadro de diálogo **nuevo proyecto de equipo** , proporcione un nombre y una descripción para el proyecto de equipo y, a continuación, haga clic en **siguiente**.

    > [!NOTE]
    > Si el proyecto de equipo incluye espacios, puede que surjan algunos problemas cuando use la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para implementar paquetes desde la ruta de acceso de salida. Los espacios en la ruta de acceso pueden hacer que sea mucho más difícil ejecutar comandos Web Deploy.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. En la página **seleccionar una plantilla de proceso** , seleccione la plantilla de proceso que desea usar para administrar el proceso de desarrollo y, a continuación, haga clic en **siguiente**.

    > [!NOTE]
    > Para obtener más información acerca de las plantillas de proceso de TFS, consulte [herramientas y plantillas de proceso](https://msdn.microsoft.com/vstudio/aa718795).
12. En la página **configuración del sitio de grupo** , deje la configuración predeterminada sin cambios y, a continuación, haga clic en **siguiente**.
13. Esta configuración crea o identifica un sitio de grupo de SharePoint que está asociado al proyecto de equipo de TFS. El equipo de desarrollo puede utilizar este sitio para administrar la documentación, participar en los hilos de discusión, crear páginas wiki y realizar otras tareas que no están relacionadas con el código. Para obtener más información, vea [interacciones entre productos de SharePoint y Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. En la página **especificar configuración de control de código fuente** , deje la configuración predeterminada sin cambios y, a continuación, haga clic en **siguiente**.
15. Esta configuración identifica o crea la ubicación en la jerarquía de carpetas de TFS que actuará como carpeta raíz para el contenido.
16. En la página **confirmar configuración del proyecto de equipo** , haga clic en **Finalizar**.
17. Cuando el nuevo proyecto de equipo se cree correctamente, en la página **proyecto de equipo creado** , haga clic en **cerrar**.

### <a name="add-users-to-a-team-project"></a>Agregar usuarios a un proyecto de equipo

Ahora que ha creado el nuevo proyecto de equipo, puede conceder permisos a los usuarios para que puedan empezar a agregar y colaborar en el contenido.

**Para agregar usuarios a un proyecto de equipo**

1. En Visual Studio 2010, en la ventana de **Team Explorer** , haga clic con el botón secundario en el proyecto de equipo, seleccione **configuración del proyecto de equipo**y, a continuación, haga clic en **pertenencia a grupos**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Para permitir que un usuario agregue, modifique y quite código bajo control de código fuente, debe agregarlo al grupo **Contributors** .
3. En el cuadro de diálogo **grupos de proyectos** , seleccione el grupo **Contributors** y, a continuación, haga clic en **propiedades**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , seleccione **usuario o grupo de Windows**y, a continuación, haga clic en **Agregar**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre de usuario del usuario que desea agregar al proyecto de equipo, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. En el cuadro de diálogo **propiedades de grupo de Team Foundation Server** , haga clic en **Aceptar**.
7. En el cuadro de diálogo **grupos de proyectos** , haga clic en **cerrar**.

## <a name="conclusion"></a>Conclusión

En este momento, el nuevo proyecto de equipo está listo para usarse y el equipo de desarrolladores puede empezar a agregar contenido y colaborar en el proceso de desarrollo.

En el tema siguiente, [agregar contenido al control de código fuente](adding-content-to-source-control.md), se describe cómo agregar contenido al control de código fuente.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones más amplias sobre cómo crear proyectos de equipo en TFS, vea [crear un proyecto de equipo](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Para obtener más información sobre cómo permitir a los usuarios crear nuevos proyectos de equipo en una colección de proyectos de equipo, vea [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx). Para obtener más información sobre cómo agregar usuarios a proyectos de equipo, vea [Agregar usuarios a proyectos de equipo](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Siguiente](adding-content-to-source-control.md)
