---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Escenario: configurar un entorno de prueba para la implementación web | Microsoft Docs'
author: jrjlee
description: En este tema se describe un escenario de implementación web típico para entornos de desarrollo o pruebas, y se explican las tareas que debe completar para configurar un si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517987"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Escenario: configurar un entorno de prueba para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe un escenario de implementación web típico para entornos de desarrollo o pruebas, y se explican las tareas que debe completar para configurar un entorno similar.

Cuando los desarrolladores trabajan en aplicaciones Web, a menudo se les proporciona acceso a un entorno de servidor que pueden usar para probar los cambios en sus aplicaciones en un valor realista. Este tipo de entorno de desarrollo o de prueba tiene normalmente estas características:

- El entorno consta de un único servidor Web y un único servidor de base de datos.
- Los desarrolladores suelen tener privilegios de administrador en los servidores para permitirles configurar el entorno según los requisitos de sus aplicaciones.
- Los cambios en las aplicaciones se implementan con frecuencia, por lo que el entorno debe admitir la implementación de un solo paso o automatizada.

Por ejemplo, en nuestro [escenario de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt HINK es un desarrollador de Fabrikam, Inc. Matt está trabajando en la solución Contact Manager y tiene que implementar periódicamente cambios en un entorno de prueba. Matt es un administrador del servidor Web de prueba y el servidor de base de datos de prueba. Inicialmente, Matt debe ser capaz de implementar la solución en el entorno de prueba directamente.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

A medida que progresa el trabajo y más desarrolladores se unen al equipo, la solución Contact Manager está configurada para la integración continua (CI) en Team Foundation Server (TFS). Cada vez que un desarrollador protege el contenido, Team Build debe compilar la solución, ejecutar las pruebas unitarias e implementar automáticamente la solución en el entorno de prueba.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Información general de la solución

El entorno de prueba debe admitir la implementación de un solo paso o automatizada desde un equipo remoto, por lo que tiene la opción de elegir entre dos enfoques principales. Puede realizar lo siguiente:

- Configure el servidor Web de prueba para admitir la implementación mediante el servicio de Deployment Agent web (el "agente remoto").
- Configure el servidor Web de prueba para admitir la implementación mediante el controlador de Web Deploy.

> [!NOTE]
> También puede usar [Web deploy a petición](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (el "agente Temp"). Esto es similar al enfoque del agente remoto en cuanto a requisitos y restricciones.

En este caso, los desarrolladores tienen privilegios de administrador en los servidores de destino y el entorno de prueba no está sujeto a restricciones estrictas de seguridad, por lo que la opción lógica es configurar el servidor Web de prueba para admitir la implementación mediante el agente remoto. Esto es menos complejo y requiere menos configuración inicial que el método del controlador de Web Deploy. También necesitará configurar el servidor de base de datos para que admita la implementación y el acceso remoto.

En estos temas se proporciona toda la información necesaria para completar estas tareas:

- [Configurar un servidor web para la publicación de web deploy (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). En este tema se describe cómo crear un servidor Web que admite la publicación de Web Deploy, mediante el enfoque de agente remoto, a partir de una compilación limpia de Windows Server 2008 R2.
- [Configurar un servidor de base de datos para la publicación de web deploy](configuring-a-database-server-for-web-deploy-publishing.md). En este tema se describe cómo configurar un servidor de base de datos para admitir el acceso y la implementación remotos, empezando por una instalación predeterminada de SQL Server 2008 R2.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la configuración de un entorno de ensayo típico, consulte [escenario: configuración de un entorno de ensayo para la implementación web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obtener instrucciones sobre la configuración de un entorno de producción típico, consulte [escenario: configuración de un entorno de producción para la implementación web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Siguiente](scenario-configuring-a-staging-environment-for-web-deployment.md)
