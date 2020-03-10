---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Escenario: configuración de un entorno de producción para la implementación web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típico para un entorno de producción y se explican las tareas que se deben completar para configurar un similar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515935"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Escenario: configuración de un entorno de producción para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típico para un entorno de producción y se explican las tareas que debe completar para configurar un entorno similar.

El entorno de producción es el destino final de una aplicación web o un sitio Web. En este momento, la aplicación se ha implementado en un entorno de ensayo y está lista para "comenzar". Las características de un entorno de producción pueden variar ampliamente según la naturaleza y el propósito del contenido Web, el tamaño de la organización, el público de destino y muchos otros factores. En un escenario de escala empresarial, el entorno de producción puede tener estas características:

- El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de bases de datos, a menudo con clústeres de conmutación por error y creación de reflejo de la base de datos.
- Si el entorno es accesible desde Internet, es probable que se segrega de la red interna. Puede estar en una subred diferente en una red perimetral, puede estar en un dominio diferente y puede estar en una infraestructura de red totalmente diferente.
- Es muy improbable que los desarrolladores y las cuentas de proceso del servidor de compilación tengan privilegios de administrador en los servidores de producción.
- Los cambios en las aplicaciones se implementan con menos frecuencia que las implementaciones de prueba o ensayo.

> [!NOTE]
> Escalar horizontalmente una implementación de base de datos en varios servidores queda fuera del ámbito de este tutorial. Para obtener más información sobre esta área, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Por ejemplo, en nuestro [escenario de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un servidor de Team Build incluye definiciones de compilación que permiten a los usuarios compilar la solución Contact Manager e implementarla en un entorno de ensayo en un solo paso. Cuando la aplicación está lista para implementarse en producción, debido a las restricciones impuestas por los requisitos de seguridad y la infraestructura de red, el administrador del entorno de producción debe copiar manualmente el paquete Web en un servidor Web de producción e importar a través del administrador de Internet Information Services (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Información general de la solución

En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:

- Debido a las restricciones de seguridad y la configuración de red, no se puede configurar el entorno de producción para que admita una implementación con un solo clic o automatizada. La implementación sin conexión es el único enfoque viable en este escenario.
- El entorno de producción incluye varios servidores Web, por lo que puede usar el marco de granja de servidores Web (WFF) para crear una granja de servidores. Con este enfoque, el administrador solo tiene que importar la aplicación en un servidor Web (el servidor principal) y WFF replicará la implementación en todos los demás servidores web del entorno de producción.

En estos temas se proporciona toda la información necesaria para completar estas tareas:

- [Cree una granja de servidores con el marco de granja de servidores web](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo crear y configurar una granja de servidores mediante WFF, de modo que los productos y componentes de la plataforma web, los valores de configuración y los sitios web y las aplicaciones se repliquen entre varios servidores web con equilibrio de carga.
- [Configurar un servidor web para la publicación de web deploy (implementación sin conexión)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). En este tema se describe cómo crear un servidor Web que permita a los administradores importar e implementar paquetes Web manualmente, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos para la publicación de web deploy](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir el acceso y la implementación remotos, empezando por una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la configuración de un entorno de prueba de desarrollador típico, consulte [escenario: configurar un entorno de prueba para la implementación web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obtener instrucciones sobre la configuración de un entorno de ensayo típico, consulte [escenario: configuración de un entorno de ensayo para la implementación web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Siguiente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
