---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configuración de entornos de servidor para la implementación web | Microsoft Docs
author: jrjlee
description: En este tutorial se muestra cómo configurar entornos de servidor para ofrecer compatibilidad con la implementación y publicación de sitios web con un solo clic, o automatizada, en diferentes escenario...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515119"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Configurar entornos de servidor para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tutorial se muestra cómo configurar entornos de servidor para admitir la implementación de sitios web con un solo clic, la automatización y la publicación en distintos escenarios. El tutorial incluye temas que le guiarán a través de la realización de varias tareas, como la configuración de un servidor web para admitir enfoques específicos de implementación y configuración de una granja de servidores web Farm Framework (WFF), junto con información general basada en escenarios que proporcionan instrucciones de un extremo a otro de nivel superior.
> 
> En el tutorial se usa el escenario de implementación de Fabrikam, Inc. descrito en [implementación web de la empresa: información general del escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) como punto de referencia para los ejemplos y la infraestructura de red.
> 
> Para una traducción en Italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

En este tutorial se incluyen los siguientes temas:

- [Elegir el enfoque adecuado para la implementación web](choosing-the-right-approach-to-web-deployment.md)
- [Escenario: Configurar un entorno de prueba para la implementación web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Escenario: Configurar un entorno de ensayo para la implementación web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Escenario: Configurar un entorno de producción para la implementación web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configurar un servidor web para la publicación de la implementación web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configurar un servidor web para la publicación de la implementación web (controlador de implementación web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configurar un servidor web para la publicación de la implementación web (implementación sin conexión)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configurar un servidor de base de datos para la publicación de la implementación web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Crear una granja de servidores con Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configurar las propiedades de implementación de un entorno de destino](configuring-deployment-properties-for-a-target-environment.md)

En el primer tema, [elegir el enfoque adecuado para la implementación web](choosing-the-right-approach-to-web-deployment.md), se describen los principales enfoques que puede usar para publicar aplicaciones web mediante la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) 2,0. También se identifican los escenarios que se asignan a cada enfoque. Desde aquí, cada tema del escenario proporciona información general de alto nivel de las tareas que debe completar e identifica los temas que necesitará para completar estas tareas.

Si utiliza el enfoque de archivo de proyecto dividido descrito en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md) para compilar e implementar la solución, el tema final, [configurar las propiedades de implementación para un entorno de destino](configuring-deployment-properties-for-a-target-environment.md), describe cómo configurar archivos de proyecto específicos del entorno para la implementación en diferentes entornos de destino.

## <a name="key-technologies"></a>Tecnologías clave

Este tutorial se centra en el uso de estos productos y tecnologías para admitir la implementación web:

- IIS 7,5
- Web Deploy 2. x
- WFF 2.x
- Servicio de administración web de IIS (WMSvc)

El tutorial también toca el uso de Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 y ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales sobre la implementación web a escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial y muestra cómo las tareas y los tutoriales que se describen en la serie encajan en un proceso de administración del ciclo de vida de las aplicaciones (ALM) más amplio.
- [Implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). En este tutorial se proporciona una introducción conceptual a los archivos de proyecto de Microsoft Build Engine (MSBuild), la canalización de publicación Web, el Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar estas herramientas juntas para administrar procesos de implementación complejos.
- [Configuración de Team Foundation Server para la implementación web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). En este tutorial se describe cómo configurar Team Foundation Server (TFS) para admitir varios escenarios de implementación, incluida la implementación automatizada como parte de un proceso de integración continua (CI) y la activación manual de implementaciones de compilaciones específicas.
- [Implementación web de la empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). En este tutorial se describe cómo realizar diversas tareas de implementación más avanzadas, como personalizar las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de la implementación y dejar las aplicaciones web sin conexión durante el proceso de implementación. .

> [!div class="step-by-step"]
> [Siguiente](choosing-the-right-approach-to-web-deployment.md)
