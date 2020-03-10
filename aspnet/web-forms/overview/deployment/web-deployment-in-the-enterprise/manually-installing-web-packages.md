---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Instalación manual de paquetes Web | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo importar manualmente un paquete de implementación web en Internet Information Services (IIS). El tema Building and Packaging Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514891"
---
# <a name="manually-installing-web-packages"></a>Instalación manual de paquetes web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo importar manualmente un paquete de implementación web en Internet Information Services (IIS).
> 
> En el tema [crear y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md) se describe cómo la herramienta de implementación web de IIS (Web deploy), junto con la Microsoft Build Engine (MSBuild) y la canalización de publicación web (WPP), permite empaquetar los proyectos de aplicación web en un solo archivo zip. Este archivo, conocido normalmente como un paquete de implementación web (o simplemente un paquete de implementación), contiene todo el contenido y la información de configuración que necesita IIS para volver a crear la aplicación web en un servidor Web.
> 
> Una vez que haya creado un paquete de implementación web, puede publicarlo en un servidor IIS de varias maneras. En muchos escenarios, querrá aprovechar los puntos de integración entre MSBuild, WPP y Web Deploy para crear e instalar paquetes Web de forma remota como parte de un proceso de compilación e implementación de un solo paso o automatizado. Este proceso se describe en [implementar paquetes web](deploying-web-packages.md). Sin embargo, esto no siempre es posible. Supongamos que desea implementar una aplicación web en un entorno de producción orientado a Internet. Por motivos de seguridad, este tipo de entorno de producción es al menos probable que esté detrás de un firewall en una subred independiente del servidor de compilación, en una red perimetral (también conocida como DMZ, zona desmilitarizada y subred filtrada). En muchos casos, el entorno de producción estará en un dominio independiente o en una red físicamente aislada.
> 
> En estos casos, la única opción puede ser portar el paquete Web en el servidor de destino e importarlo manualmente en IIS. Aunque este enfoque impide la implementación automatizada, sigue siendo una técnica muy eficaz para publicar una aplicación&#x2014;web simplemente copia un único archivo zip en el servidor Web y usa un asistente que le guiará a través del proceso de importación.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

Deberá completar estas tareas de alto nivel para importar un paquete de implementación web en IIS:

- Cree un paquete de implementación web mediante la línea de comandos de MSBuild, Team Build o Visual Studio 2010.
- Copie el paquete Web en el servidor Web de destino.
- Use el Asistente para importar paquete de aplicación en el administrador de IIS para instalar el paquete web y proporcionar valores para variables como cadenas de conexión y extremos de servicio.

En este tema se muestra cómo llevar a cabo estos procedimientos. En las tareas y los tutoriales de este tema se da por hecho que ya está familiarizado con los conceptos relacionados con los paquetes Web, Web Deploy y WPP. Para obtener más información, vea [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Este tema se utiliza mejor junto con [configurar un servidor web para la publicación de web deploy (implementación sin conexión)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), en el que se explica cómo instalar los componentes necesarios y preparar un sitio web de IIS para la importación de paquetes.

## <a name="create-a-web-deployment-package"></a>Crear un paquete de implementación web

La primera tarea consiste en crear un paquete de implementación web para el proyecto de aplicación web que desea implementar. Puede crear paquetes Web de varias maneras.

**Enfoque 1: crear un paquete como parte del proceso de compilación con Visual Studio**

Puede configurar el proyecto de aplicación web para crear un paquete de implementación web después de cada compilación a través de la pestaña **empaquetar/publicar web** en las páginas de propiedades del proyecto. Este proceso se describe en [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).

**Enfoque 2: crear un paquete como parte del proceso de compilación con MSBuild**

Si compila el proyecto de aplicación Web mediante MSBuild directamente, ya sea a través de un archivo de proyecto de MSBuild personalizado o desde la línea de comandos, puede crear un paquete de implementación web como parte del proceso de compilación mediante la inclusión de las propiedades **DeployOnBuild = true** y **DeployTarget = Package** en el comando. Este proceso se describe en [Descripción del proceso de compilación](understanding-the-build-process.md).

**Enfoque 3: creación de un paquete a petición en Visual Studio**

Puede crear un paquete de implementación web para un proyecto de aplicación web en cualquier momento en Visual Studio 2010. Para ello, en la ventana de **Explorador de soluciones** , haga clic con el botón secundario en el proyecto de aplicación web y, a continuación, haga clic en **compilar paquete de implementación**.

![](manually-installing-web-packages/_static/image1.png)

**Enfoque 4: creación de un paquete a petición desde la línea de comandos**

Puede crear un paquete de implementación web desde la línea de comandos mediante la invocación del destino del **paquete** en el proyecto de aplicación Web mediante MSBuild. El comando debe ser similar al siguiente:

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

Sea cual sea el enfoque que use, el resultado final es el mismo. WPP crea un paquete de implementación web como un archivo zip, junto con varios recursos auxiliares, en la carpeta de salida del proyecto de aplicación Web.

![](manually-installing-web-packages/_static/image2.png)

Cuando planea importar el paquete Web manualmente, solo necesita el archivo zip. Copie este archivo en el servidor Web de destino y podrá comenzar el proceso de importación.

## <a name="import-a-web-package-into-iis"></a>Importar un paquete Web en IIS

Puede usar el siguiente procedimiento para importar un paquete de implementación web desde el sistema de archivos local a un sitio web de IIS. Antes de realizar este procedimiento, asegúrese de que tiene lo siguiente:

- Copió el paquete de implementación web en el servidor Web.
- Configuró un servidor Web de IIS para hospedar la aplicación.

Para obtener más información sobre la configuración de un servidor Web de IIS para admitir paquetes de implementación web, vea [configurar un servidor web para la publicación de web deploy (implementación sin conexión)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Para importar un paquete de implementación web mediante el administrador de IIS**

1. En el administrador de IIS, en el panel **conexiones** , haga clic con el botón secundario en el sitio web de IIS, seleccione **implementar**y, a continuación, haga clic en **importar aplicación**.

    ![](manually-installing-web-packages/_static/image3.png)
2. En el Asistente para importar paquete de aplicación, en la página **seleccionar el paquete** , busque la ubicación del paquete de implementación web y, a continuación, haga clic en **siguiente**.
3. En la página **seleccionar el contenido del paquete** , borre cualquier contenido que no necesite y, a continuación, haga clic en **siguiente**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > En muchos casos, es posible que no desee importar todo lo que viene con un paquete de implementación web. Por ejemplo, es posible que no desee permitir que Web Deploy Reemplace la base de datos asociada.  
    > Las entradas de **permisos de concesión** establecen permisos en el sistema de archivos de destino para asegurarse de que la identidad del grupo de aplicaciones puede tener acceso a la carpeta física que almacena el contenido del sitio Web. Además, al usuario de autenticación anónima se le concede permiso de lectura en la carpeta para permitir que la aplicación sirva a archivos de tipo MIME (Extensiones multipropósito de correo Internet). Si lo prefiere, puede quitar estas entradas y configurar los permisos manualmente.
4. En la página **especificar información de paquete de aplicación** , proporcione la información solicitada.

    ![](manually-installing-web-packages/_static/image5.png)
5. Cuando se crea un paquete Web, WPP analiza el archivo de configuración de la aplicación y detecta las variables, como las cadenas de conexión y los puntos de conexión de servicio. En este caso:

    1. **Ruta de acceso** de la aplicación es la ruta de acceso de IIS donde desea instalar la aplicación. Esta configuración es común a todos los paquetes de implementación que crea WPP.
    2. La **dirección del punto de conexión de servicio ContactService** es la dirección que debe usar la aplicación para comunicarse con el servicio WCF implementado. Este valor corresponde a una entrada en el archivo *Web. config* .
    3. La primera configuración de la **cadena de conexión** es la cadena de conexión que Web deploy debe usar para implementar la base de datos asociada a la aplicación (en este caso, una base de datos de pertenencia a ASP.net). Este valor corresponde al valor de la pestaña **empaquetar/publicar SQL** de Visual Studio.
    4. El segundo valor de la **cadena de conexión** es la cadena de conexión que la aplicación usará realmente para comunicarse con la base de datos cuando esté en funcionamiento. Esto corresponde a una entrada de cadena de conexión en el archivo *Web. config* .

        > [!NOTE]
        > Para obtener más información sobre el procedencia de estos parámetros, vea [Configuring Parameters for Web Package Deployment](configuring-parameters-for-web-package-deployment.md).
6. Haga clic en **Siguiente**.
7. Si no es la primera vez que ha implementado la aplicación en este sitio web, se le pedirá que especifique si desea eliminar todo el contenido existente antes de la instalación. Elija la opción que sea adecuada para sus requisitos y, a continuación, haga clic en **siguiente**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Cuando IIS haya terminado de instalar el paquete, haga clic en **Finalizar**.

    ![](manually-installing-web-packages/_static/image7.png)

En este punto, ha publicado correctamente la aplicación web en IIS.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo importar un paquete de implementación web en un sitio web de IIS mediante el administrador de IIS. Este enfoque para la publicación de aplicaciones web es adecuado cuando las restricciones de seguridad o de infraestructura hacen que la implementación remota sea imposible o no deseable.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar un servidor Web de IIS para que admita la importación manual de un paquete Web, vea [configurar un servidor web para la publicación de web deploy (implementación sin conexión)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Para obtener instrucciones generales sobre cómo implementar paquetes Web, vea [Tutorial: implementar un proyecto de aplicación Web mediante un paquete de implementación web (parte 1 de 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-and-running-a-deployment-command-file.md)
