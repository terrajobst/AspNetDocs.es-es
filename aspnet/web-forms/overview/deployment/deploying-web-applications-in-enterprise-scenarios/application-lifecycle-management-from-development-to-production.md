---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Administración del ciclo de vida de las aplicaciones: desde el desarrollo hasta la producción | Microsoft Docs'
author: jrjlee
description: En este tema se muestra cómo una empresa ficticia administra la implementación de una aplicación Web ASP.NET a través de entornos de prueba, ensayo y producción como par...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520429"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Administración del ciclo de vida de las aplicaciones: de desarrollo a producción

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se muestra cómo una empresa ficticia administra la implementación de una aplicación Web de ASP.NET a través de entornos de prueba, ensayo y producción como parte de un proceso de desarrollo continuo. A lo largo del tema, se proporcionan vínculos a información adicional y tutoriales sobre cómo realizar tareas específicas.
> 
> El tema está diseñado para proporcionar información general de alto nivel sobre una [serie de tutoriales](deploying-web-applications-in-enterprise-scenarios.md) sobre la implementación web en la empresa. No se preocupe si no está familiarizado con algunos de los conceptos que&#x2014;se describen aquí, los tutoriales que se indican a continuación proporcionan información detallada sobre todas estas tareas y técnicas.
> 
> > [!NOTE]
> > Por simplicidad, en este tema no se describe la actualización de bases de datos como parte del proceso de implementación. Sin embargo, realizar actualizaciones incrementales en las características de las bases de datos es un requisito de muchos escenarios de implementación empresarial, y puede encontrar instrucciones sobre cómo hacerlo más adelante en esta serie de tutoriales. Para obtener más información, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Información general

El proceso de implementación que se muestra aquí se basa en el escenario de implementación de Fabrikam, Inc. que se describe en [implementación web de la empresa: Introducción a los escenarios](enterprise-web-deployment-scenario-overview.md). Debe leer la información general del escenario antes de estudiar este tema. En esencia, el escenario examina cómo una organización administra la implementación de una aplicación web razonablemente compleja, la [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), a través de varias fases en un entorno empresarial típico.

En un nivel alto, la solución Contact Manager pasa por estas fases como parte del proceso de desarrollo e implementación:

1. Un desarrollador comprueba algún código en Team Foundation Server (TFS) 2010.
2. TFS compila el código y ejecuta las pruebas unitarias asociadas al proyecto de equipo.
3. TFS implementa la solución en el entorno de prueba.
4. El equipo del desarrollador comprueba y valida la solución en el entorno de prueba.
5. El administrador del entorno de ensayo realiza una implementación "What if" en el entorno de ensayo para establecer si la implementación producirá problemas.
6. El administrador del entorno de ensayo realiza una implementación en vivo en el entorno de ensayo.
7. La solución se somete a las pruebas de aceptación del usuario en el entorno de ensayo.
8. Los paquetes de implementación web se importan manualmente en el entorno de producción.

Estas fases forman parte de un ciclo de desarrollo continuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

En la práctica, el proceso es ligeramente más complicado, como verá cuando examinemos cada fase con más detalle. Fabrikam, Inc. utiliza un enfoque diferente para la implementación en cada entorno de destino.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

En el resto de este tema se examinan estas fases clave de este ciclo de vida de la implementación:

- **Requisitos previos**: Cómo necesita configurar la infraestructura de servidor antes de poner en marcha la lógica de implementación.
- **Desarrollo e implementación iniciales**: lo que debe hacer antes de implementar la solución por primera vez.
- **Implementación para probar**: cómo empaquetar e implementar contenido en un entorno de prueba automáticamente cuando un desarrollador protege un nuevo código.
- **Implementación en ensayo**: cómo implementar compilaciones específicas en un entorno de ensayo y cómo realizar implementaciones de tipo "What if" para asegurarse de que una implementación no causará ningún problema.
- **Implementación en producción**: Cómo importar paquetes Web en un entorno de producción cuando la infraestructura de red impide la implementación remota.

## <a name="prerequisites"></a>Requisitos previos

La primera tarea en cualquier escenario de implementación es asegurarse de que la infraestructura del servidor cumple los requisitos de las herramientas y las técnicas de implementación. En este caso, Fabrikam, Inc. ha configurado su infraestructura de servidor de la siguiente manera:

- TFS está configurado para incluir una colección de proyectos de equipo, controladores de compilación y agentes de compilación. Consulte [configuración de Team Foundation Server para la implementación web automatizada](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) para obtener más información.
- El entorno de prueba se configura para aceptar implementaciones remotas mediante el servicio de Deployment Agent web (el "agente remoto"), como se describe en [escenario: configurar un entorno de prueba para la implementación web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) y [configurar un servidor web para la publicación de web deploy (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- El entorno de ensayo se configura para aceptar implementaciones remotas mediante el punto de conexión del controlador de Web Deploy, como se describe en [escenario: configurar un entorno de ensayo para la implementación web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) y [configurar un servidor web para la publicación de web deploy (controlador de web deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- El entorno de producción está configurado para permitir que un administrador importe manualmente los paquetes de implementación web en Internet Information Services (IIS), tal como se describe en [escenario: configurar un entorno de producción para la implementación web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) y [configurar un servidor web para la publicación de web deploy (implementación sin conexión)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Desarrollo e implementación iniciales

Antes de que el equipo de desarrollo de Fabrikam, Inc. pueda implementar la solución Contact Manager por primera vez, debe realizar estas tareas:

- Cree un nuevo proyecto de equipo en TFS.
- Cree los archivos de proyecto de Microsoft Build Engine (MSBuild) que contienen la lógica de implementación.
- Cree las definiciones de compilación de TFS que desencadenan los procesos de implementación.

### <a name="create-a-new-team-project"></a>Crear un nuevo proyecto de equipo

- El administrador de TFS, Rob Walters, crea un nuevo proyecto de equipo para la aplicación, como se describe en [crear un proyecto de equipo en TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). A continuación, el desarrollador jefe, Matt HINK, crea una solución esqueleto. Comprueba sus archivos en el nuevo proyecto de equipo en TFS, como se describe en [agregar contenido al control de código fuente](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Crear la lógica de implementación

Matt HINK crea varios archivos de proyecto de MSBuild personalizados, mediante el enfoque de archivo de proyecto dividido descrito en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt crea:

- Un archivo de proyecto denominado *Publish. proj* que ejecuta el proceso de implementación. Este archivo contiene destinos de MSBuild que compilan los proyectos de la solución, crean paquetes web e implementan los paquetes en un entorno de servidor de destino.
- Archivos de proyecto específicos del entorno denominados *env-dev. proj* y *env-Stage. proj*. Contienen opciones de configuración específicas del entorno de prueba y el entorno de ensayo, respectivamente, como cadenas de conexión, puntos de conexión de servicio y los detalles del servicio remoto que recibirá el paquete Web. Para obtener instrucciones sobre cómo elegir la configuración correcta para entornos de destino específicos, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Para ejecutar la implementación, un usuario ejecuta el archivo *Publish. proj* con MSBuild o Team build y especifica la ubicación del archivo de proyecto específico del entorno correspondiente (*env-dev. proj* o *env-Stage. proj*) como argumento de línea de comandos. Después, el archivo *Publish. proj* importa el archivo de proyecto específico del entorno para crear un conjunto completo de instrucciones de publicación para cada entorno de destino.

> [!NOTE]
> La forma en que funcionan estos archivos de proyecto personalizados es independiente del mecanismo que se usa para invocar MSBuild. Por ejemplo, puede usar la línea de comandos de MSBuild directamente, como se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Puede ejecutar los archivos de proyecto desde un archivo de comandos, tal y como se describe en [crear y ejecutar un archivo de comandos de implementación](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Como alternativa, puede ejecutar los archivos de proyecto a partir de una definición de compilación en TFS, tal como se describe en [crear una definición de compilación que admita la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> En cada caso, el resultado final es el&#x2014;mismo que MSBuild ejecuta el archivo de proyecto combinado e implementa la solución en el entorno de destino. Esto proporciona una gran flexibilidad a la hora de desencadenar el proceso de publicación.

Una vez que haya creado los archivos de proyecto personalizados, Matt los agrega a una carpeta de soluciones y los protege en el control de código fuente.

### <a name="create-build-definitions"></a>Crear definiciones de compilación

Como tarea de preparación final, Matt y Rob trabajan juntos para crear tres definiciones de compilación para el nuevo proyecto de equipo:

- **DeployToTest**. Esto crea la solución Contact Manager y la implementa en el entorno de prueba cada vez que se produce una inserción en el repositorio.
- **DeployToStaging**. Esto implementa los recursos de una compilación anterior especificada en el entorno de ensayo cuando un desarrollador pone en cola la compilación.
- **DeployToStaging-Whatif**. Esto realiza una implementación "What if" en el entorno de ensayo cuando un desarrollador pone en cola la compilación.

En las secciones siguientes se proporcionan más detalles sobre cada una de estas definiciones de compilación.

## <a name="deployment-to-test"></a>Implementación que se va a probar

El equipo de desarrollo de Fabrikam, Inc. mantiene entornos de prueba para llevar a cabo una serie de actividades de pruebas de software, como la comprobación y validación, las pruebas de facilidad de uso, las pruebas de compatibilidad y las pruebas exploratorias o ad hoc.

El equipo de desarrollo ha creado una definición de compilación en TFS denominada **DeployToTest**. Esta definición de compilación usa un desencadenador de integración continua, lo que significa que el proceso de compilación se ejecuta cada vez que un miembro del equipo de desarrollo de Fabrikam, Inc. realiza una inserción en el repositorio. Cuando se desencadena una compilación, la definición de compilación hará lo siguiente:

- Compile la solución ContactManager. sln. Esto, a su vez, compila todos los proyectos de la solución.
- Ejecute las pruebas unitarias en la estructura de carpetas de la solución (si la solución se compila correctamente).
- Ejecute los archivos de proyecto personalizados que controlan el proceso de implementación (si la solución se compila correctamente y pasa las pruebas unitarias).

El resultado final es que si la solución se compila correctamente y pasa las pruebas unitarias, los paquetes web y cualquier otro recurso de implementación se implementan en el entorno de prueba.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

La definición de compilación **DeployToTest** proporciona estos argumentos a MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

Las propiedades **DeployOnBuild = true** y **DeployTarget = Package** se usan cuando Team Build compila los proyectos dentro de la solución. Cuando el proyecto es un proyecto de aplicación Web, estas propiedades indican a MSBuild que cree un paquete de implementación web para el proyecto. La propiedad **TargetEnvPropsFile** indica al archivo *Publish. proj* dónde encontrar el archivo de proyecto específico del entorno que se va a importar.

> [!NOTE]
> Para obtener un tutorial detallado sobre cómo crear una definición de compilación como esta, vea [crear una definición de compilación que admita la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

El archivo *Publish. proj* contiene los destinos que compilan cada proyecto de la solución. Sin embargo, también incluye lógica condicional que omite estos destinos de compilación si está ejecutando el archivo en Team Build. Esto le permite aprovechar la funcionalidad de compilación adicional que ofrece Team Build, como la capacidad de ejecutar pruebas unitarias. Si se produce un error en la compilación de la solución o en las pruebas unitarias, el archivo *Publish. proj* no se ejecutará y no se implementará la aplicación.

La lógica condicional se logra mediante la evaluación de la propiedad **BuildingInTeamBuild** . Se trata de una propiedad de MSBuild que se establece automáticamente en **true** cuando se usa Team Build para compilar los proyectos.

## <a name="deployment-to-staging"></a>Implementación en ensayo

Cuando una compilación cumple todos los requisitos del equipo de desarrollo en el entorno de prueba, es posible que el equipo desee implementar la misma compilación en un entorno de ensayo. Los entornos de ensayo se configuran normalmente para que coincidan con las características del entorno de producción o "activo", por ejemplo, en términos de especificaciones del servidor, sistemas operativos y software y configuración de red. Los entornos de ensayo se suelen usar para las pruebas de carga, las pruebas de aceptación del usuario y las revisiones internas más amplias. Las compilaciones se implementan en el entorno de ensayo directamente desde el servidor de compilación.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

Las definiciones de compilación que se usan para implementar la solución en el entorno de ensayo, **DeployToStaging-Whatif** y **DeployToStaging**, comparten estas características:

- En realidad, no compilan nada. Cuando Rob implementa la solución en el entorno de ensayo, desea implementar una compilación específica existente que ya se ha comprobado y validado en el entorno de prueba. Las definiciones de compilación solo deben ejecutar los archivos de proyecto personalizados que controlan el proceso de implementación.
- Cuando Rob desencadena una compilación, usa los parámetros de compilación para especificar qué compilación contiene los recursos que desea implementar desde el servidor de compilación.
- Las definiciones de compilación no se activan automáticamente. Rob pone en cola manualmente una compilación cuando desea implementar la solución en el entorno de ensayo.

Este es el proceso de alto nivel para una implementación en el entorno de ensayo:

1. El administrador del entorno de ensayo, Rob Walters, pone en cola una compilación con la definición **de compilación DeployToStaging-Whatif** . Rob usa los parámetros de definición de compilación para especificar la compilación que desea implementar.
2. La definición de compilación **DeployToStaging-Whatif** ejecuta los archivos de proyecto personalizados en el modo "What if". Esto genera archivos de registro como si Rob realizara una implementación activa, pero en realidad no realiza ningún cambio en el entorno de destino.
3. Rob revisa los archivos de registro para determinar los efectos de la implementación en el entorno de ensayo. En concreto, Rob desea comprobar lo que se agregará, qué se actualizará y qué se eliminará.
4. Si se satisface la certeza de que la implementación no realizará ningún cambio no deseado en los recursos o datos existentes, pone en cola una compilación con la definición de compilación **DeployToStaging** .
5. La definición de compilación **DeployToStaging** ejecuta los archivos de proyecto personalizados. Estos recursos de implementación se publican en el servidor web principal en el entorno de ensayo.
6. El controlador del marco de granja de servidores Web (WFF) sincroniza los servidores Web en el entorno de ensayo. Esto hace que la aplicación esté disponible en todos los servidores Web de la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

La definición de compilación **DeployToStaging** proporciona estos argumentos a MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

La propiedad **TargetEnvPropsFile** indica al archivo *Publish. proj* dónde encontrar el archivo de proyecto específico del entorno que se va a importar. La propiedad **OutputRoot** invalida el valor integrado e indica la ubicación de la carpeta de compilación que contiene los recursos que desea implementar. Cuando Rob pone en cola la compilación, usa la pestaña **parámetros** para proporcionar un valor actualizado para la propiedad **OutputRoot** .

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Para obtener más información sobre cómo crear una definición de compilación como esta, vea [implementar una compilación específica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

La definición **de compilación DeployToStaging-Whatif** contiene la misma lógica de implementación que la definición de compilación **DeployToStaging** . Sin embargo, incluye el argumento adicional **Whatif = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

En el archivo *Publish. proj* , la propiedad **Whatif** indica que todos los recursos de implementación deben publicarse en el modo "What if". En otras palabras, los archivos de registro se generan como si la implementación hubiera terminado, pero no se cambia realmente nada en el entorno de destino. Esto le permite evaluar el impacto de una implementación&#x2014;propuesta en particular, lo que se agregará, lo que se actualizará y lo que se&#x2014;eliminará antes de realizar los cambios.

> [!NOTE]
> Para obtener más información sobre cómo configurar las implementaciones de "What if", vea [realizar una implementación de "What if"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Una vez que haya implementado la aplicación en el servidor web principal en el entorno de ensayo, WFF sincronizará automáticamente la aplicación en todos los servidores de la granja de servidores.

> [!NOTE]
> Para obtener más información sobre la configuración de WFF para sincronizar servidores Web, vea [crear una granja de servidores con el marco de granja](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md)de servidores Web.

## <a name="deployment-to-production"></a>Implementación en producción

Cuando se ha aprobado una compilación en el entorno de ensayo, el equipo de Fabrikam, Inc. puede publicar la aplicación en el entorno de producción. El entorno de producción es donde la aplicación se "activa" y llega a la audiencia de destino de los usuarios finales.

El entorno de producción se encuentra en una red perimetral accesible desde Internet. Está aislado de la red interna que contiene el servidor de compilación. El administrador del entorno de producción, lisa Andrews, debe copiar manualmente los paquetes de implementación web del servidor de compilación e importarlos en IIS en el servidor Web de producción principal.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Este es el proceso de alto nivel para una implementación en el entorno de producción:

1. El equipo de desarrolladores aconseja que una compilación esté lista para su implementación en producción. El equipo aconseja a Lisa de la ubicación de los paquetes de implementación web dentro de la carpeta de entrega en el servidor de compilación.
2. Lisa recopila los paquetes web del servidor de compilación y los copia en el servidor web principal en el entorno de producción.
3. Lisa utiliza el administrador de IIS para importar y publicar los paquetes Web en el servidor web principal.
4. El controlador WFF sincroniza los servidores Web en el entorno de producción. Esto hace que la aplicación esté disponible en todos los servidores Web de la granja de servidores.

### <a name="how-does-the-deployment-process-work"></a>¿Cómo funciona el proceso de implementación?

El administrador de IIS incluye un asistente para importar paquetes de aplicaciones que facilita la publicación de paquetes Web en un sitio web de IIS. Para ver un tutorial sobre cómo realizar este procedimiento, consulte [instalación manual de paquetes web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona una ilustración del ciclo de vida de la implementación de una aplicación Web de escala empresarial típica.

Este tema forma parte de una serie de tutoriales que proporcionan instrucciones sobre distintos aspectos de la implementación de aplicaciones Web. En la práctica, hay muchas tareas y consideraciones adicionales en cada fase del proceso de implementación, y no es posible tratarlas todas en un solo tutorial. Para obtener más información, consulte estos tutoriales:

- [Implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). En este tutorial se proporciona una introducción completa a las técnicas de implementación web con MSBuild y la herramienta de implementación web de IIS (Web Deploy).
- [Configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). En este tutorial se proporcionan instrucciones sobre cómo configurar entornos de Windows Server para admitir diversos escenarios de implementación.
- [Configuración de Team Foundation Server para la implementación web automatizada](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). En este tutorial se proporcionan instrucciones sobre cómo integrar la lógica de implementación en procesos de compilación de TFS.
- [Implementación web de la empresa avanzada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). En este tutorial se proporcionan instrucciones sobre cómo cumplir algunos de los desafíos de implementación más complejos a los que se enfrentan las organizaciones.

> [!div class="step-by-step"]
> [Anterior](enterprise-web-deployment-scenario-overview.md)
