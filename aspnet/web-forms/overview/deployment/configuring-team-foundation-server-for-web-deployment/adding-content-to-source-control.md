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
# <a name="adding-content-to-source-control"></a>Agregar contenido al control de código fuente

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se explica cómo agregar contenido al control de código fuente en Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo en TFS, y explica cómo agregar dependencias externas, como marcos de trabajo o ensamblados, al control de código fuente.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

En la mayoría de los casos, todos los miembros del equipo de desarrolladores deben poder agregar contenido al control de código fuente. Para agregar una solución al control de código fuente en TFS, debe completar estos pasos de alto nivel:

- Conéctese a un proyecto de equipo.
- Asigne la estructura de carpetas del proyecto de equipo en el servidor a una estructura de carpetas en el equipo local.
- Agregue la solución y su contenido al control de código fuente.
- Agregue las dependencias externas al control de código fuente.

En este tema se muestra cómo llevar a cabo estos procedimientos.

En las tareas y los tutoriales de este tema se supone que ya ha creado un nuevo proyecto de equipo de TFS para administrar el contenido. Para obtener más información sobre cómo crear un nuevo proyecto de equipo, vea [crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>¿Quién realiza estos procedimientos?

En la mayoría de los casos, todos los miembros del equipo de desarrolladores deben poder agregar y modificar contenido dentro de proyectos de equipo específicos.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Conectarse a un proyecto de equipo y crear una asignación de carpetas

Antes de agregar contenido al control de código fuente, debe conectarse a un proyecto de equipo y crear una asignación entre la estructura de carpetas en el servidor y el sistema de archivos en el equipo local.

**Para conectarse a un proyecto de equipo y asignar una ruta de acceso local**

1. En la estación de trabajo del desarrollador, abra Visual Studio 2010.
2. En Visual Studio, en el menú **equipo** , haga clic en **conectar a Team Foundation Server**.

    > [!NOTE]
    > Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos 3-6.
3. En el cuadro de diálogo **conexión al proyecto de equipo** , haga clic en **servidores**.
4. En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **Agregar**.
5. En el cuadro de diálogo **agregar Team Foundation Server** , proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.

    ![](adding-content-to-source-control/_static/image1.png)
6. En el cuadro de diálogo **Agregar o quitar Team Foundation Server** , haga clic en **cerrar**.
7. En el cuadro de diálogo **conectar al proyecto de equipo** , seleccione la instancia de TFS a la que desea conectarse, seleccione la colección de proyectos de equipo, seleccione el proyecto de equipo al que desea agregar y, a continuación, haga clic en **conectar**.

    ![](adding-content-to-source-control/_static/image2.png)
8. En la ventana de **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **control de código fuente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. En la pestaña **Explorador de control de código fuente** , haga clic en **sin asignar**.

    ![](adding-content-to-source-control/_static/image4.png)
10. En el cuadro de diálogo **asignar** , en el cuadro **carpeta local** , busque (o cree) una carpeta local que actúe como carpeta raíz para el proyecto de equipo y, a continuación, haga clic en **asignar**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Cuando se le pida que descargue los archivos de origen, haga clic en **sí**.

    ![](adding-content-to-source-control/_static/image6.png)

En este punto, ha asignado la carpeta del servidor para el proyecto de equipo a una carpeta local en la estación de trabajo del desarrollador. También ha descargado el contenido existente del proyecto de equipo en la estructura de carpetas local. Ahora puede empezar a agregar su propio contenido al control de código fuente.

## <a name="add-projects-and-solutions-to-source-control"></a>Agregar proyectos y soluciones al control de código fuente

Para agregar proyectos y soluciones al control de código fuente, primero debe moverlos a la carpeta asignada del proyecto de equipo en el equipo local. Después, puede proteger el contenido para sincronizar las adiciones con el servidor.

**Para agregar proyectos al control de código fuente**

1. En la estación de trabajo de desarrollador, mueva sus proyectos y soluciones a una ubicación adecuada dentro de la estructura de carpetas asignada para el proyecto de equipo.

    > [!NOTE]
    > Muchas organizaciones tendrán un método preferido para la organización de proyectos y soluciones en el control de código fuente. Para obtener instrucciones sobre cómo estructurar carpetas, consulte [Cómo: estructurar las carpetas de control de código fuente en Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Abra la solución en Visual Studio 2010.
3. En la ventana **Explorador de soluciones** , haga clic con el botón secundario en la solución y, a continuación, haga clic en **Agregar solución al control de código fuente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > En algunos casos, en función de cómo le guste la organización el contenido de la estructura en TFS, puede que tenga que agregar proyectos al control de código fuente de forma individual para proporcionar un control más preciso sobre cómo se organiza el código fuente.
4. Compruebe que la pestaña **Explorador de control de código fuente** muestra el contenido que ha agregado en la estructura de carpetas del servidor para el proyecto de equipo.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > En la pestaña **Explorador de control de código fuente** se muestra el contenido sin preguntar más porque se ha agregado la solución a una carpeta asignada en el sistema de archivos local. Si la solución estaba en una ubicación no asignada, se le pedirá que especifique las ubicaciones de carpeta en TFS y en el sistema de archivos local.
5. En la pestaña **Explorador de control de código fuente** , en el panel **carpetas** , haga clic con el botón secundario en el proyecto de equipo (por ejemplo, **ContactManager**) y, a continuación, haga clic en **proteger los cambios pendientes**.
6. En el cuadro de diálogo **proteger: archivos de origen** , escriba un comentario y, a continuación, haga clic en **proteger**.

    ![](adding-content-to-source-control/_static/image9.png)

Llegados a este punto, ha agregado la solución al control de código fuente en TFS.

## <a name="add-external-dependencies-to-source-control"></a>Agregar dependencias externas al control de código fuente

Al agregar un proyecto o una solución al control de código fuente, también se agregarán los archivos y las carpetas del proyecto o la solución. Sin embargo, en muchos casos, los proyectos y las soluciones también dependen de las dependencias externas, como los ensamblados locales, para funcionar correctamente. Debe agregar estos recursos a control de código fuente para permitir que Team build y otros miembros del equipo de desarrolladores compilen el código correctamente.

Por ejemplo, la estructura de carpetas de la solución de ejemplo Contact Manager incluye una carpeta denominada packages. Contiene el ensamblado y varios recursos de soporte para el ADO.NET Entity Framework 4,1. La carpeta Packages no forma parte de la solución Contact Manager, pero la solución no se compilará correctamente sin él. Para habilitar Team Build para compilar la solución, debe agregar la carpeta Packages al control de código fuente.

> [!NOTE]
> La inclusión de una carpeta de paquetes es habitual de lo que sucede cuando se agrega el Entity Framework, o recursos similares, a la solución con la extensión NuGet para Visual Studio 2010.

**Para agregar contenido que no sea del proyecto al control de código fuente**

1. Asegúrese de que los elementos que desea agregar (por ejemplo, la carpeta Packages) estén en una ubicación adecuada dentro de una carpeta asignada en el sistema de archivos local.
2. En Visual Studio 2010, en la ventana de **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **control de código fuente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. En la pestaña **Explorador de control de código fuente** , en el panel **carpetas** , seleccione la carpeta que contiene el elemento o elementos que desea agregar.
4. Haga clic en el botón **Agregar elementos a la carpeta** .

    ![](adding-content-to-source-control/_static/image11.png)
5. En el cuadro de diálogo **Agregar al control de código fuente** , seleccione la carpeta o los elementos que desee agregar y, a continuación, haga clic en **siguiente**.

    ![](adding-content-to-source-control/_static/image12.png)
6. En la pestaña **elementos excluidos** , seleccione los elementos necesarios que se han excluido automáticamente (por ejemplo, ensamblados) y, a continuación, haga clic en **incluir elementos**.

    ![](adding-content-to-source-control/_static/image13.png)
7. En la pestaña **elementos para agregar** , compruebe que se muestran todos los archivos que desea incluir y, a continuación, haga clic en **Finalizar**.

    ![](adding-content-to-source-control/_static/image14.png)
8. En la ventana de **Explorador de control de código fuente** , haga clic en el botón **proteger** .

    ![](adding-content-to-source-control/_static/image15.png)
9. En el cuadro de diálogo **proteger: archivos de origen** , escriba un comentario y, a continuación, haga clic en **proteger**.

En este punto, ha agregado las dependencias externas de la solución al control de código fuente.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo conectarse a un proyecto de equipo, asignar una estructura de carpetas y agregar contenido al control de código fuente. Para obtener más información sobre cómo trabajar con elementos bajo control de código fuente, vea [usar el control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).

En el tema siguiente, [configurar un servidor de compilación de TFS para la implementación web](configuring-a-tfs-build-server-for-web-deployment.md), se describe cómo preparar un servidor TFS Team Build para compilar e implementar la solución.

## <a name="further-reading"></a>Información adicional

Para obtener información más completa sobre cómo trabajar con el control de código fuente en TFS, vea [usar el control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-team-project-in-tfs.md)
> [Siguiente](configuring-a-tfs-build-server-for-web-deployment.md)
