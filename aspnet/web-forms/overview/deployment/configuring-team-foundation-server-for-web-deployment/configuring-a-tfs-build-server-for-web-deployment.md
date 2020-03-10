---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configuración de un servidor de compilación de TFS para la implementación web | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo preparar un servidor de compilación de Team Foundation Server (TFS) para compilar e implementar soluciones con Team Build e Internet informat...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512227"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configurar un servidor de compilación de TFS para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo preparar un servidor de compilación de Team Foundation Server (TFS) para compilar e implementar soluciones con Team build y la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy).

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Para preparar un servidor de compilación para compilar e implementar sus soluciones, necesitará:

- Instalar y configurar el servicio de compilación de TFS.
- Instale Visual Studio 2010.
- Instale los productos o componentes necesarios para compilar la solución, como las versiones del .NET Framework o ASP.NET MVC.
- Instale Web Deploy 2,0 o posterior.

En este tema se muestra cómo realizar estos procedimientos o apuntar a otros recursos en los que existan. En las tareas y los tutoriales de este tema se da por supuesto que:

- Está empezando con una compilación de servidor limpio que ejecuta Windows Server 2008 R2 Service Pack 1.
- El servidor está unido a un dominio con una dirección IP estática.
- Ha instalado la capa de aplicación de TFS en un servidor independiente, tal como se describe en [implementación web de la empresa: información general sobre el escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>¿Quién realiza estos procedimientos?

En la mayoría de los casos, un administrador de TFS será responsable de configurar los servidores de compilación. En algunos casos, el equipo de desarrolladores puede tomar posesión de servidores de compilación específicos.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalar y configurar el servicio de compilación de TFS

Al configurar un servidor de compilación, la primera tarea consiste en instalar y configurar el servicio de compilación de TFS. Como parte de este proceso, necesitará:

- Instale el servicio de compilación de TFS y configure una cuenta de servicio. Cualquier tarea de compilación, incluida la implementación, se ejecutará con la identidad de la cuenta de servicio de compilación.
- Cree un *controlador de compilación* y uno o más *agentes de compilación*. Cada controlador de compilación administra un conjunto de agentes de compilación. Al poner en cola una compilación, el controlador de compilación asigna la tarea de compilación a un agente de compilación disponible. Cada colección de proyectos de equipo de TFS se asigna a un solo controlador de compilación.
- Configurar una carpeta de entrega para los resultados de la compilación. Se trata de un recurso compartido de red. Cualquier salida de compilación, como los paquetes de implementación web, se envía a la carpeta de entrega.

El capítulo [Administración de Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) de MSDN contiene todos los recursos que necesita para realizar estas tareas:

- Para obtener información general conceptual de Team Foundation Build, incluidos el servicio de compilación, los controladores de compilación y los agentes de compilación, vea [Descripción de un sistema de Team Foundation Build](https://msdn.microsoft.com/library/dd793166.aspx).
- Para obtener información sobre cómo instalar y configurar el servicio de compilación, vea [configurar un equipo de compilación](https://msdn.microsoft.com/library/ms181712.aspx).
- Para obtener información sobre cómo crear controladores de compilación, vea [crear y trabajar con un controlador de compilación](https://msdn.microsoft.com/library/ee330987.aspx).
- Para obtener información sobre la creación de agentes de compilación, vea [crear y trabajar con agentes de compilación](https://msdn.microsoft.com/library/bb399135.aspx).
- Para obtener información sobre cómo crear y configurar carpetas de entrega, vea [configurar carpetas de entrega](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalar los productos y componentes necesarios

Para que el servidor de compilación pueda compilar las soluciones, debe instalar los productos, componentes o ensamblados que requiera la solución. Antes de instalar cualquier componente de plataforma web, debe instalar Visual Studio 2010 (cualquier versión) en el servidor de compilación. Esto garantiza que los archivos de destino de los Microsoft Build Engine básicos (MSBuild) y los archivos de destino de canalización de publicación web (WPP) están disponibles para el servicio de compilación. El instalador de Visual Studio también debe instalar Web Deploy, que necesitará si tiene previsto implementar paquetes web como parte del proceso de compilación.

La mejor manera de instalar los componentes comunes de la plataforma web es usar el [instalador de plataforma web](https://go.microsoft.com/?linkid=9805118). Esto garantiza que va a instalar la versión más reciente de cada producto y que también detecta e instala automáticamente los requisitos previos de cada producto. En el caso de la solución [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , debe usar el instalador de plataforma web para instalar estos productos y componentes:

- **.NET Framework 4,0**. Esto es necesario para ejecutar aplicaciones que se compilaron en esta versión del .NET Framework.
- **Herramienta de implementación Web 2,1 o posterior**. Esto instala Web Deploy (y su archivo ejecutable subyacente, MSDeploy. exe) en el servidor. Como parte de este proceso, instala e inicia el servicio de Deployment Agent Web. Este servicio le permite implementar paquetes web desde un equipo remoto.
- **ASP.NET MVC 3**. Esto instala los ensamblados que necesita para ejecutar aplicaciones ASP.NET MVC 3.

**Para instalar los productos y componentes necesarios**

1. Instale Visual Studio 2010. Cuando se le pida que seleccione las características que desea instalar, debe incluir:

    1. Cualquier lenguaje de programación que necesite compilar.
    2. Visual Web Developer. Esto garantiza que los destinos WPP se agregan al servidor de compilación.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Una vez completada la instalación de Visual Studio 2010, descargue e instale [visual studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (si aún no está incluido en el medio de instalación).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 resuelve un error que puede impedir que MSBuild busque el archivo ejecutable de MSDeploy.
3. Descargue e inicie el [instalador de plataforma web](https://go.microsoft.com/?linkid=9805118).
4. En la parte superior de la ventana **instalador de plataforma Web 3,0** , haga clic en **productos**.
5. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos de trabajo**.
6. En la fila **Microsoft .NET Framework 4** , si el .NET Framework no está ya instalado, haga clic en **Agregar**.

    > [!NOTE]
    > Es posible que ya haya instalado el .NET Framework 4,0 a través de Windows Update. Si ya hay instalado un producto o componente, el instalador de plataforma web lo indicará reemplazando el botón **Agregar** con el texto **instalado**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. En la fila **ASP.NET MVC 3 (Visual Studio 2010)** , haga clic en **Agregar**.
8. En el panel de navegación, haga clic en **servidor**.
9. En la fila **herramienta de implementación Web 2,1** , haga clic en **Agregar**.
10. Haga clic en **Instalar**. El instalador de plataforma Web mostrará una lista de productos &#x2014; junto con las dependencias asociadas &#x2014; esté instalado y se le pedirá que acepte los términos de licencia.
11. Revise los términos de licencia y, si da su consentimiento a los términos **, haga clic en Acepto.**
12. Una vez completada la instalación, haga clic en **Finalizar**y, a continuación, cierre la ventana **instalador de plataforma web 3,0** .

> [!NOTE]
> Si el proceso de implementación incluye el uso de herramientas como VSDBCMD. exe o SQLCMD. exe, deberá asegurarse de que están instaladas en el servidor de compilación. VSDBCMD. exe es una herramienta de Visual Studio y se agrega normalmente al servidor al instalar Team Foundation Build. SQLCMD. exe es una herramienta de SQL Server. Puede descargar una versión independiente de SQLCMD. exe en la página [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) .

## <a name="conclusion"></a>Conclusión

En este momento, el servidor de compilación está listo para empezar a compilar e implementar los proyectos de aplicación Web. En el tema siguiente, [crear una definición de compilación que admita la implementación](creating-a-build-definition-that-supports-deployment.md), se describe cómo crear y configurar una definición de compilación para controlar cuándo y cómo se compilan e implementan los proyectos.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones generales sobre cómo trabajar con Team Build, vea [administrar Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Anterior](adding-content-to-source-control.md)
> [Siguiente](creating-a-build-definition-that-supports-deployment.md)
