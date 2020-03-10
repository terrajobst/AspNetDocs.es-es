---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configuración de Team Foundation Server para la implementación web | Microsoft Docs
author: jrjlee
description: En este tutorial se muestra cómo configurar Team Foundation Server (TFS) 2010 para compilar soluciones e implementar contenido web en varios entornos de destino. ...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424207"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Configurar Team Foundation Server para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tutorial se muestra cómo configurar Team Foundation Server (TFS) 2010 para compilar soluciones e implementar contenido web en varios entornos de destino. Esto incluye escenarios de integración continua (CI), donde se implementa el contenido automáticamente cada vez que un desarrollador realiza un cambio. También puede incluir escenarios de desencadenador manual, en los que un administrador puede querer desencadenar la implementación de una compilación específica en un entorno de ensayo una vez que la compilación se ha comprobado y validado en el entorno de prueba. Los temas de este tutorial le guiarán a través del proceso de configuración completo, incluidos:
> 
> - Cómo crear un nuevo proyecto de equipo en TFS.
> - Cómo agregar contenido al control de código fuente.
> - Cómo configurar un servidor de compilación para admitir CI e implementación.
> - Cómo crear una definición de compilación que incluya lógica de implementación.
> - Cómo configurar permisos para la implementación automatizada.
> 
> Para una traducción en Italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

En este tutorial se supone que ha instalado TFS 2010 y ha creado una colección de proyectos de equipo como parte del proceso de configuración inicial. La [Guía de instalación de Team Foundation para Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) proporciona instrucciones completas sobre estas tareas.

## <a name="context"></a>Contexto

Esto forma parte de una serie de tutoriales en función de los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de&#x2014;ejemplo de la solución [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

El escenario de alto nivel de estos tutoriales se describe en [implementación web de la empresa: información general sobre el escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Le recomendamos que revise este tema antes de empezar a trabajar en este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

Si es la primera vez que realiza las tareas descritas en este tutorial, o si desea seguir los ejemplos mediante la solución de ejemplo, debe trabajar en los temas del tutorial en orden. Como alternativa, puede usar temas individuales como guía para tareas específicas. En este tutorial se incluyen los siguientes temas:

- [Crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md). Un proyecto de equipo es la unidad básica para el control de código fuente, la administración de procesos y la compilación en TFS. Debe crear un proyecto de equipo para poder agregar contenido al control de código fuente o crear definiciones de compilación.
- [Agregar contenido al control de código fuente](adding-content-to-source-control.md). Una vez que haya creado un proyecto de equipo, puede empezar a agregar contenido al control de código fuente. Deberá agregar sus proyectos y soluciones, junto con las dependencias externas, para poder empezar a configurar las compilaciones.
- [Configurar un servidor de compilación de TFS para la implementación web](configuring-a-tfs-build-server-for-web-deployment.md). Si desea compilar el contenido del proyecto de equipo, deberá configurar un servidor de compilación. En la mayoría de los casos, debe estar en un equipo independiente de la instalación de TFS. Para configurar un servidor de compilación, debe instalar y configurar el servicio de compilación de TFS, instalar Visual Studio 2010, crear controladores de compilación y agentes de compilación, instalar los productos o componentes que necesite el código para compilarlos correctamente e instalar el Herramienta de implementación web de Internet Information Services (IIS) (Web Deploy).
- [Crear una definición de compilación que admita la implementación](creating-a-build-definition-that-supports-deployment.md). Para poder iniciar la puesta en cola o desencadenar compilaciones en TFS, debe crear al menos una definición de compilación para el proyecto de equipo. La definición de compilación define todos los aspectos de la compilación, incluidos los elementos que se deben incluir en la compilación, qué debe desencadenar la compilación y dónde Team Build debe enviar los resultados de la compilación. Puede configurar una definición de compilación para ejecutar archivos de proyecto de Microsoft Build Engine personalizado (MSBuild), lo que permite incluir lógica de implementación en las compilaciones automatizadas.
- [Implementar una compilación específica](deploying-a-specific-build.md). En muchos escenarios, querrá implementar una compilación específica, en lugar de la compilación más reciente, en un entorno de destino. En este caso, puede configurar una definición de compilación que implementa contenido desde una carpeta de entrega específica.
- [Configuración de permisos para la implementación de Team Build](configuring-permissions-for-team-build-deployment.md). Si el servicio de compilación va a implementar contenido como parte de un proceso de compilación automatizado, debe conceder varios permisos a la cuenta de servicio de compilación en todos los servidores web y servidores de bases de datos de destino.

## <a name="key-technologies"></a>Tecnologías clave

Este tutorial se centra en el uso de estos productos y tecnologías para admitir la compilación automatizada y la implementación web:

- Visual Studio Team Foundation Server 2010
- Team build y MSBuild
- Web Deploy

El tutorial también toca el uso de Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 y ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales sobre la implementación web a escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial y muestra cómo las tareas y los tutoriales que se describen en la serie encajan en un proceso de administración del ciclo de vida de las aplicaciones (ALM) más amplio.
- [Implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). En este tutorial se proporciona una introducción conceptual a los archivos de proyecto de MSBuild, la canalización de publicación web (WPP), el Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar estas herramientas juntas para administrar procesos de implementación complejos.
- [Configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). En este tutorial se describe cómo configurar servidores de Windows para admitir varios escenarios de implementación, incluida la implementación de paquetes web remotos mediante el servicio de Deployment Agent web (el agente remoto) o el controlador de Web Deploy y la implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuado para su propio entorno y describe cómo usar el marco de granja de servidores Web (WFF) para replicar las aplicaciones web implementadas en todos los servidores Web de una granja de servidores.
- [Implementación web de la empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). En este tutorial se describe cómo realizar diversas tareas de implementación más avanzadas, como personalizar las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de la implementación y dejar las aplicaciones web sin conexión durante el proceso de implementación. .

> [!div class="step-by-step"]
> [Siguiente](creating-a-team-project-in-tfs.md)
