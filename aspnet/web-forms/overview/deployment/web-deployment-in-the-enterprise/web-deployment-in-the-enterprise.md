---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implementación web en la empresa | Microsoft Docs
author: jrjlee
description: En este tutorial se describe cómo cumplir muchos de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial en desarrollo...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509719"
---
# <a name="web-deployment-in-the-enterprise"></a>Implementación web en la empresa

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tutorial se describe cómo cumplir muchos de los desafíos que encontrará al administrar la implementación de aplicaciones web a escala empresarial en entornos de desarrollo, pruebas, ensayo y producción. El tutorial incluye una solución de referencia junto con una combinación de contenido conceptual y orientado a tareas que le guiará a través de diversas tareas y procedimientos comunes.
> 
> Para una traducción en Italiano de estos tutoriales, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Desafíos de la implementación empresarial

A menudo, las organizaciones encuentran estos desafíos cuando buscan administrar la implementación de soluciones complejas de escala empresarial:

- Debe ser capaz de implementar proyectos en varios entornos, como los entornos de desarrollo o pruebas, las plataformas de ensayo y los servidores de producción. La solución debe implementarse con distintos valores de configuración para cada entorno.
- Debe implementar varios proyectos dependientes simultáneamente como parte de un proceso de compilación e implementación automatizado o de un solo paso.
- Debe poder impulsar la implementación desde un proceso automatizado. Por ejemplo, desea utilizar un proceso de integración continua (CI) para implementar aplicaciones web en un entorno de prueba cuando se protege un nuevo código.
- Debe ser capaz de controlar el proceso de implementación y establecer variables de implementación desde fuera de Visual Studio, ya que es poco probable que los desarrolladores tengan la configuración correcta o las credenciales necesarias para cada entorno de destino.
- Debe implementar proyectos de base de datos basados en esquemas y conservar los datos existentes en las implementaciones posteriores.
- Debe implementar las bases de datos de pertenencia de forma ad hoc sin implementar los datos de la cuenta de usuario. También es posible que necesite actualizar el esquema de las bases de datos de pertenencia implementadas sin perder los datos de las cuentas de usuario existentes.
- Debe excluir determinados archivos o carpetas al implementar contenido en varios entornos de destino.

## <a name="overview-of-approach"></a>Información general sobre el enfoque

En este tutorial, junto con los otros tutoriales de esta serie, se usa este enfoque de alto nivel para cumplir los desafíos descritos anteriormente.

- **Use archivos de proyecto de Microsoft Build Engine personalizados (MSBuild) para controlar el proceso general de compilación e implementación.**
- Esto le permite compilar e implementar todos los proyectos de la solución como parte de una única operación que admite scripts.
- La configuración específica del entorno se configura mediante archivos de proyecto sencillos específicos del entorno. A diferencia del enfoque centrado en Visual Studio del uso de configuraciones de soluciones y perfiles de publicación para configurar implementaciones para diferentes entornos, este enfoque le permite configurar y administrar el proceso de implementación desde fuera de Visual Studio. Esto significa que los desarrolladores no necesitan un conocimiento avanzado de las cadenas de conexión, los puntos de conexión de servicio, las credenciales del servidor y otras variables de implementación para los entornos de destino.
- Team Build puede invocar los archivos de proyecto personalizados como parte de un flujo de trabajo de Team Foundation Server (TFS). Esto le permite configurar la implementación automatizada para escenarios de CI.

**Use la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para empaquetar e implementar proyectos de aplicación Web.**

- Web Deploy proporciona un marco que permite empaquetar e implementar el contenido de la aplicación web en un servidor Web de IIS de destino, junto con las dependencias, las opciones de configuración, la configuración de seguridad y cualquier otro requisito.
- Puede controlar todo el proceso de empaquetado e implementación desde los archivos de proyecto de MSBuild personalizados. También puede manipular los valores de configuración que acompañan al paquete de implementación web, como las cadenas de conexión, los puntos de conexión de servicio y los detalles de destino de IIS.
- Web Deploy, junto con la canalización de publicación Web, ofrece numerosos puntos de extensibilidad que permiten personalizar las implementaciones. Por ejemplo, es fácil excluir archivos y carpetas no deseados de los paquetes de implementación web.

**Use la utilidad VSDBCMD. exe para implementar y actualizar esquemas de base de datos.**

- VSDBCMD le permite implementar las bases de datos desde un archivo de esquema de base de datos (. dbschema), que se genera al compilar un proyecto de base de datos de Visual Studio. En cambio, la funcionalidad de implementación de base de datos incluida en Web Deploy es más adecuada para implementar bases de datos existentes desde una instancia de SQL Server local.
- A diferencia de la funcionalidad de Visual Studio para implementar proyectos de base de datos, VSDBCMD permite implementar actualizaciones diferenciales en una base de datos de destino existente. Esto le permite conservar cualquier dato existente mientras actualiza el esquema de la base de datos.
- Puede ejecutar comandos de VSDBCMD desde los archivos de proyecto de MSBuild personalizados.

## <a name="content-map"></a>Mapa de contenido

En este tutorial se incluyen temas que se dividen en cuatro áreas principales.

En estos temas se presenta la&#x2014;solución de referencia de&#x2014;la solución Contact Manager y se describe cómo descargarla y configurarla en el equipo local:

- [La solución Contact Manager](the-contact-manager-solution.md)
- [Configurar la solución Contact Manager](setting-up-the-contact-manager-solution.md)

En estos temas se presentan los archivos de proyecto de MSBuild, se describe cómo se pueden crear y usar archivos de proyecto personalizados y se explica el proceso de implementación de la solución Contact Manager:

- [Descripción del archivo de proyecto](understanding-the-project-file.md)
- [Descripción del proceso de compilación](understanding-the-build-process.md)

En estos temas se describe la implementación de aplicaciones Web, incluida la forma en que funciona el proceso de compilación y empaquetado, cómo se integra el proceso de compilación con la canalización de publicación Web, cómo se modifican los parámetros de implementación y cómo se implementan paquetes Web en el destino. entorno

- [Crear y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md)
- [Configurar los parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md)
- [Implementar paquetes web](deploying-web-packages.md)

- En [implementación de proyectos de base de datos](deploying-database-projects.md) se describen las distintas técnicas que puede usar para implementar proyectos de base de datos de Visual Studio, junto con las ventajas y desventajas de cada enfoque. [Crear y ejecutar un archivo de comandos de implementación](creating-and-running-a-deployment-command-file.md) describe cómo crear un archivo de comandos simple que encapsula la lógica de implementación y le permite implementar soluciones complejas como un proceso de un solo paso.
- Por último, la [instalación manual de paquetes web](manually-installing-web-packages.md) concluye el tutorial y le muestra cómo importar paquetes Web en IIS.

## <a name="key-technologies"></a>Tecnologías clave

En los temas de este tutorial se usan principalmente estas tecnologías para administrar la compilación y la implementación:

- Visual Studio 2010
- MSBuild
- IIS 7,5
- Web Deploy 2.0
- La utilidad de implementación de base de datos de VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Otros tutoriales de esta serie

Esto forma parte de una serie de cinco tutoriales sobre la implementación web a escala empresarial. Estos son los otros tutoriales de la serie:

- [Implementación de aplicaciones web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este contenido introductorio proporciona el fondo contextual para la serie de tutoriales. Describe el escenario del tutorial y muestra cómo las tareas y los tutoriales que se describen en la serie encajan en un proceso de administración del ciclo de vida de las aplicaciones (ALM) más amplio.
- [Configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). En este tutorial se describe cómo configurar servidores de Windows para admitir varios escenarios de implementación, incluida la implementación de paquetes web remotos mediante el servicio de Deployment Agent web (el agente remoto) o el controlador de Web Deploy y la implementación de base de datos remota. Proporciona instrucciones sobre cómo elegir el método de implementación adecuado para su propio entorno y describe cómo usar el marco de granja de servidores Web (WFF) para replicar las aplicaciones web implementadas en todos los servidores Web de una granja de servidores.
- [Configuración de Team Foundation Server para la implementación web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). En este tutorial se describe cómo configurar TFS para admitir varios escenarios de implementación, incluida la implementación automatizada como parte de un proceso de CI y la activación manual de implementaciones de compilaciones específicas.
- [Implementación web de la empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). En este tutorial se describe cómo realizar diversas tareas de implementación más avanzadas, como personalizar las implementaciones de base de datos para varios entornos, excluir archivos y carpetas de la implementación y dejar las aplicaciones web sin conexión durante el proceso de implementación. .

> [!div class="step-by-step"]
> [Siguiente](the-contact-manager-solution.md)
