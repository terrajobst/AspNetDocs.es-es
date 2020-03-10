---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Implementación de aplicaciones web en escenarios empresariales con Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: En este conjunto de tutoriales se describen las herramientas y técnicas que puede usar para implementar aplicaciones web en varios escenarios empresariales. Explica cómo hacer el mejor uso...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463435"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implementar aplicaciones web en escenarios empresariales mediante Visual Studio 2010

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este conjunto de tutoriales se describen las herramientas y técnicas que puede usar para implementar aplicaciones web en varios escenarios empresariales. Explica cómo hacer el mejor uso de tecnologías como Visual Studio 2010, Microsoft Build Engine (MSBuild), Internet Information Services (IIS) 7,5, la herramienta de implementación web de IIS (Web Deploy), el marco de granja de servidores Web (WFF) y utilidades como VSDBCMD. exe para Simplifique y administre el proceso de implementación. Incluye información general conceptual y orientación orientada a tareas que le ayudará a:
> 
> - Revise y establezca los requisitos de implementación para una aplicación web a escala empresarial.
> - Configure entornos de servidor Web de prueba, de ensayo y de producción para admitir la implementación web.
> - Configure procesos de integración continua (CI) de Team Foundation Server (TFS) para admitir la implementación web automatizada.
> - Implemente aplicaciones web a escala empresarial en diferentes entornos de servidor con requisitos y restricciones variables.
> - Implementar cambios en las aplicaciones web que se ejecutan en entornos de servidor diferentes.
> 
> > [!NOTE]
> > Aunque estos tutoriales describen el uso de TFS como un servidor de CI, las instrucciones se adaptan fácilmente a cualquier servidor de CI. No necesita tener un conocimiento detallado de TFS para comprender y aprovechar los tutoriales.
> 
> 
> Para una traducción en Italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>Acerca de los autores

Jason Lee es un tecnólogo principal con el [maestro de contenido](http://www.contentmaster.com/) en el que ha trabajado con productos y tecnologías de Microsoft, especialmente SharePoint y ASP.net, durante varios años. Jason tiene un doctorado en informática y actualmente está certificado por MCPD y MCTS. Puede leer el blog técnico de Jason en [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin es un tecnólogo principal con el [maestro de contenido](http://www.contentmaster.com/) que ha escrito notas del producto, documentación del SDK, presentaciones de PowerPoint y cursos de aprendizaje en línea y dirigidos por un instructor durante su carrera. Un miembro original del equipo de documentación de ASP.NET ha trabajado con las tecnologías Web de Microsoft durante más de una década.

## <a name="target-audience"></a>Público de destino

Este conjunto de tutoriales es para los desarrolladores de aplicaciones Web de ASP.NET y los arquitectos de soluciones que usan Visual Studio 2010 para crear aplicaciones web a escala empresarial. Para obtener el máximo provecho del contenido, debe sentirse cómodo con Visual Studio 2010 y tener conocimientos básicos de TFS, junto con un conocimiento de las tecnologías de plataforma web de Microsoft como ASP.NET MVC 3, Windows Communication Foundation (WCF), IIS, SQL Proyectos de base de datos de servidor y Visual Studio. Sin embargo, no es necesario que esté familiarizado con las tecnologías y las herramientas de implementación o que necesite saber cómo configurar sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir los tutoriales y realizar las tareas que se describen en estos tutoriales, debe instalar este software en el equipo de desarrollo:

- Visual Studio 2010 Premium o Ultimate Edition con Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 con Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Para realizar los pasos de implementación descritos en estos tutoriales, debe tener acceso a los entornos de implementación de aplicaciones Web de ejemplo. Para obtener los mejores resultados, estos entornos deben reflejar el patrón de implementación empresarial de su organización. Después, puede modificar los tutoriales proporcionados en esta documentación para reflejar los entornos de implementación y los requisitos de su propia organización.

## <a name="series-contents"></a>Contenido de la serie

Esta sección introductoria consta de dos temas adicionales. Están diseñados para proporcionar un contexto más amplio para los tutoriales siguientes:

- [Implementación web de la empresa: información general del escenario](enterprise-web-deployment-scenario-overview.md). En este tema se describe el escenario en el que se subancla cada uno de los tutoriales de esta serie. El escenario se centra en los requisitos de Application Lifecycle Management (ALM) de una empresa ficticia denominada Fabrikam, Inc. a medida que desarrolla una aplicación web a escala empresarial.
- [Administración del ciclo de vida de las aplicaciones: desde el desarrollo hasta el entorno de producción](application-lifecycle-management-from-development-to-production.md). En este tema se proporciona información general de un extremo a otro de alto nivel de un proceso de implementación. Muestra cómo Fabrikam, Inc. mueve una aplicación Web de ASP.NET a escala empresarial a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo.

La serie incluye cuatro conjuntos de tutoriales. Cada una de ellas se centra en distintos aspectos de la implementación web:

- [Implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). En este tutorial se proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación Web, el Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar estas herramientas juntas para administrar procesos de implementación complejos.
- [Configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). En este tutorial se describe cómo configurar servidores de Windows para admitir varios escenarios de implementación, incluida la implementación de paquetes web remotos mediante el servicio de Deployment Agent web (el "agente remoto") o el controlador de Web Deploy y la implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuado para su propio entorno y describe cómo usar el WFF para replicar las aplicaciones web implementadas en todos los servidores Web de una granja de servidores.
- [Configuración de Team Foundation Server para la implementación web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). En este tutorial se describe cómo configurar TFS para admitir varios escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y la activación manual de implementaciones de compilaciones específicas.
- [Implementación web de la empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). En este tutorial se describe cómo realizar diversas tareas de implementación más avanzadas, como personalizar las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de la implementación y dejar las aplicaciones web sin conexión durante el proceso de implementación. .

## <a name="where-to-start"></a>Dónde empezar

Este conjunto de tutoriales utiliza una solución de ejemplo con un nivel de complejidad realista, junto con un escenario de implementación de empresa ficticia, para proporcionar una implementación de referencia y proporcionar a las tareas y tutoriales un contexto común. En el tema siguiente, [implementación web empresarial: información general del escenario](enterprise-web-deployment-scenario-overview.md), se presentan el escenario y la solución de ejemplo. Desde allí puede trabajar con los tutoriales y los temas que mejor se adapten a sus necesidades.

> [!div class="step-by-step"]
> [Siguiente](enterprise-web-deployment-scenario-overview.md)
