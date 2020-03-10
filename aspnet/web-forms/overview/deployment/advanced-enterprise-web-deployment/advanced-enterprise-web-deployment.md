---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Implementación web de empresa avanzada | Microsoft Docs
author: jrjlee
description: En este tutorial se muestra cómo realizar varias tareas que son necesarias o deseables en muchos escenarios de implementación empresarial. Para una traducción de italiano...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474961"
---
# <a name="advanced-enterprise-web-deployment"></a>Implementación web avanzada de empresa

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tutorial se muestra cómo realizar varias tareas que son necesarias o deseables en muchos escenarios de implementación empresarial.
> 
> Para una traducción en Italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

Esto forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de&#x2014;ejemplo de la solución [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="scenario-overview"></a>Información general del escenario

El escenario de alto nivel de estos tutoriales se describe en [implementación web de la empresa: información general sobre el escenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Le recomendamos que revise este tema antes de empezar a trabajar en este tutorial.

## <a name="how-to-use-this-tutorial"></a>Cómo usar este tutorial

- Cada uno de los temas de este tutorial es independiente y aborda un determinado desafío o problema que se produce en los escenarios de implementación de la empresa. No es necesario que trabaje con estos temas en ningún orden determinado. Sin embargo, en este tutorial se tratan algunas tareas avanzadas. Como tal, debe familiarizarse con los conceptos y las técnicas que abarca la [implementación web en el tutorial empresarial](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) para sacar el máximo partido de este contenido.
- En este tutorial se incluyen los siguientes temas:
- [Realización de una implementación "What if"](performing-a-what-if-deployment.md). En muchos escenarios, querrá determinar el impacto de una implementación propuesta en un entorno de destino o en cualquier contenido existente antes de realizar los cambios. En este tema se describe cómo se puede ejecutar una implementación de tipo "What if" para generar archivos de registro y scripts de actualización de base de datos como si hubiera implementado contenido en un entorno de destino, sin realizar ningún cambio realmente. El análisis de estos recursos puede ayudarle a detectar cualquier posible problema antes de una implementación en vivo.
- [Personalización de implementaciones de base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Al implementar un proyecto de base de datos en varios destinos, a menudo querrá personalizar las propiedades de implementación de cada entorno de destino. Por ejemplo, en entornos de prueba, normalmente se vuelve a crear la base de datos en cada implementación, mientras que en entornos de ensayo o de producción es mucho más probable que se realicen actualizaciones incrementales para conservar los datos. En este tema se describe cómo puede incorporar estos cambios de propiedad a la lógica de implementación mediante la creación de un archivo de configuración de implementación específico del entorno (. sqldeployment) para cada entorno de destino.
- Implementar pertenencias a roles de base de datos en entornos de prueba. Cuando se vuelve a crear una base de datos&#x2014;en cada implementación, por ejemplo, como parte de una compilación de integración continua (CI) y&#x2014;se implementa en un entorno de prueba, normalmente tendrá que configurar la pertenencia a roles de base de datos cada vez. Por ejemplo, normalmente deberá conceder permisos a la identidad del grupo de aplicaciones asociada a la aplicación Web. En este tema se describe cómo se puede automatizar este proceso agregando un script SQL posterior a la implementación a la lógica de implementación.
- [Implementar bases de datos de pertenencia en entornos empresariales](deploying-membership-databases-to-enterprise-environments.md). Las bases de datos de pertenencia a ASP.NET tienen varias características que pueden complicar el proceso de implementación. Por ejemplo, una implementación de solo esquema dejará la base de datos en un estado no operativo. En la mayoría de los escenarios, es preferible crear una base de datos de pertenencia directamente en cada entorno de destino. Sin embargo, si tiene que implementar una base de datos de pertenencia, en este tema se describen algunos de los métodos que puede usar para cumplir los desafíos inherentes.
- [Excluir archivos y carpetas de la implementación](excluding-files-and-folders-from-deployment.md). En algunos escenarios, querrá personalizar el contenido del paquete Web en entornos de destino específicos. Por ejemplo, puede que desee incluir versiones completas de las bibliotecas de JavaScript al realizar la implementación en un entorno de prueba, para admitir la depuración del lado cliente, pero usar versiones de reducida de las bibliotecas al realizar la implementación en un entorno de ensayo o de producción. En este tema se describe cómo puede excluir archivos y carpetas específicos del proceso de creación del paquete.
- [Dejar las aplicaciones web sin conexión con Web deploy](taking-web-applications-offline-with-web-deploy.md). Al implementar soluciones en un entorno de ensayo o de producción, a menudo querrá dejar sin conexión las aplicaciones web mientras dure el proceso de implementación. En este tema se describe cómo puede Agregar una *aplicación\_archivo. htm sin conexión* a la aplicación web al inicio del proceso de implementación y quitarlo al final. Mientras la *aplicación\_archivo. htm sin conexión* está en su lugar, cualquier usuario que vaya a la aplicación web se redirigirá automáticamente a la *aplicación\_archivo. htm sin conexión* .
- [Ejecutar scripts de Windows PowerShell desde MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muchos escenarios de implementación requieren acciones posteriores a la implementación más complejas, como la adición de orígenes de eventos personalizados al registro o la configuración de la replicación entre SQL Server instancias. Estas acciones se realizan a menudo a través de scripts de Windows PowerShell. En este tema se describe cómo ejecutar scripts de Windows PowerShell desde un archivo de proyecto de Microsoft Build Engine (MSBuild) como parte del proceso de compilación e implementación.
- [Solucionar problemas del proceso de empaquetado](troubleshooting-the-packaging-process.md). La canalización de publicación web (WPP) define una propiedad de MSBuild denominada **EnablePackageProcessLoggingAndAssert** que puede usar para generar información detallada sobre el proceso de empaquetado de proyectos de aplicación Web. En este tema se describe lo que hace la propiedad y cómo utilizarla.

## <a name="key-technologies"></a>Tecnologías clave

Este tutorial se centra en el uso de estos productos y tecnologías para admitir la compilación automatizada y la implementación web:

- Visual Studio 2010 y Team Foundation Server (TFS) 2010
- Team Build de MSBuild y TFS
- Internet Information Services (IIS) 7,5
- Herramienta de implementación web de IIS (Web Deploy) 2,1
- La utilidad de implementación de base de datos de VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales sobre la implementación web a escala empresarial. Estos son otros tutoriales de la serie:

- [Implementación de aplicaciones web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial y muestra cómo las tareas y los tutoriales que se describen en la serie encajan en un proceso de administración del ciclo de vida de las aplicaciones (ALM) más amplio.
- [Implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). En este tutorial se proporciona una introducción conceptual a los archivos de proyecto de MSBuild, WPP, Web Deploy y otras tecnologías relacionadas. Explica cómo puede usar estas herramientas juntas para administrar procesos de implementación complejos.
- [Configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). En este tutorial se describe cómo configurar servidores de Windows para admitir varios escenarios de implementación, incluida la implementación de paquetes web remotos mediante el servicio de Deployment Agent web (el agente remoto) o el controlador de Web Deploy y la implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuado para su propio entorno y describe cómo usar el marco de granja de servidores Web (WFF) para replicar las aplicaciones web implementadas en todos los servidores Web de una granja de servidores.
- [Configuración de Team Foundation Server para la implementación web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). En este tutorial se describe cómo configurar TFS para admitir varios escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y la activación manual de implementaciones de compilaciones específicas.

> [!div class="step-by-step"]
> [Siguiente](performing-a-what-if-deployment.md)
