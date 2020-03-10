---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Escenario: configuración de un entorno de ensayo para la implementación web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típico para un entorno de ensayo y se explican las tareas que debe completar para configurar un env...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518011"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Escenario: configuración de un entorno de ensayo para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típico para un entorno de ensayo y se explican las tareas que debe completar para configurar un entorno similar.

Muchas organizaciones usan entornos de ensayo para obtener una vista previa de las actualizaciones de aplicaciones o sitios Web. Esto proporciona a los usuarios de la organización la oportunidad de explorar y revisar la nueva funcionalidad o el contenido antes de que el sitio "se encuentre activo", o en otras palabras, se implementa en un entorno de producción. El entorno de ensayo está diseñado para replicar el entorno de producción lo más cerca posible, con el fin de proporcionar una vista previa realista. Este tipo de entorno de ensayo tiene normalmente estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de bases de datos, a menudo con clústeres de conmutación por error y creación de reflejo de la base de datos.
- Las aplicaciones pueden ser implementadas manualmente por un equipo de desarrollo o automáticamente por un servidor de Team Build.
- No es probable que los usuarios o las cuentas de proceso que implementan aplicaciones tengan privilegios de administrador en los servidores de ensayo.
- Los cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir la implementación de un solo paso o automatizada.

> [!NOTE]
> Escalar horizontalmente una implementación de base de datos en varios servidores queda fuera del ámbito de este tutorial. Para obtener más información sobre esta área, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Por ejemplo, en nuestro [escenario de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) administra la solución Contact Manager. El administrador de TFS, Rob Walters, ha creado una definición de compilación que permite a los desarrolladores desencadenar una implementación en el entorno de ensayo según sea necesario.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Tenga en cuenta que en la mayoría de los casos no es necesario implementar la compilación más reciente en el entorno de ensayo. En su lugar, es mucho más probable que desee implementar una compilación específica que ya se haya sometido a la validación y comprobación en el entorno de prueba.

## <a name="solution-overview"></a>Información general de la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- La cuenta de usuario o de proceso que realiza la implementación no tendrá privilegios de administrador en los servidores provisionales, por lo que los servidores Web de ensayo deben admitir la implementación que no sea de administrador. Como tal, deberá configurar los servidores Web de ensayo para usar el controlador de Web Deploy en lugar del agente remoto.
- El entorno de ensayo incluye varios servidores Web, pero necesita admitir una implementación con un solo clic o automatizada, por lo que deberá usar el marco de granja de servidores Web (WFF) para crear una granja de servidores. Con este enfoque, puede implementar una aplicación en un servidor Web (el servidor principal) y WFF replicará la implementación en todos los demás servidores web del entorno de ensayo.
- La cuenta de usuario o de proceso que realiza la implementación debe tener permisos para crear bases de datos. Por lo tanto, tendrá que agregar la cuenta al rol de servidor **dbcreator** en el servidor de base de datos, además de configurar el servidor de base de datos para admitir el acceso y la implementación remotos.

En estos temas se proporciona toda la información necesaria para completar estas tareas:

- [Cree una granja de servidores con el marco de granja de servidores web](creating-a-server-farm-with-the-web-farm-framework.md). En este tema se describe cómo crear y configurar una granja de servidores mediante WFF, de modo que los productos y componentes de la plataforma web, los valores de configuración y los sitios web y las aplicaciones se repliquen entre varios servidores web con equilibrio de carga.
- [Configurar un servidor web para la publicación de web deploy (controlador de web deploy)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). En este tema se describe cómo crear un servidor Web que admite la publicación de Web Deploy, mediante el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos para la publicación de web deploy](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir el acceso y la implementación remotos, empezando por una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la configuración de un entorno de prueba de desarrollador típico, consulte [escenario: configurar un entorno de prueba para la implementación web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones sobre la configuración de un entorno de producción típico, consulte [escenario: configuración de un entorno de producción para la implementación web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Siguiente](scenario-configuring-a-production-environment-for-web-deployment.md)
