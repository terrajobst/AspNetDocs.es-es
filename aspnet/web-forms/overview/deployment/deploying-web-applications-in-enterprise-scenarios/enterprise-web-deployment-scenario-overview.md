---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Implementación web de la empresa: información general del escenario | Microsoft Docs'
author: jrjlee
description: Este conjunto de tutoriales utiliza una solución de ejemplo con un nivel de complejidad realista, junto con un escenario de implementación de empresa ficticia, para proporcionar una referencia...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463411"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Implementación web de la empresa: información general del escenario

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriales utiliza una solución de ejemplo con un nivel de complejidad realista, junto con un escenario de implementación de empresa ficticia, para proporcionar una implementación de referencia y proporcionar a las tareas y tutoriales un contexto común. En este tema se describe el escenario del tutorial y se presenta la solución de ejemplo.

## <a name="scenario-description"></a>Descripción del escenario

Fabrikam, Inc., una empresa ficticia, está creando una solución que permite a los equipos de ventas remotos almacenar y recuperar información de contacto de una interfaz Web.

Los procesos de administración del ciclo de vida de las aplicaciones (ALM) en Fabrikam, Inc. requieren que la solución se implemente en tres entornos de servidor en varias fases del proceso de desarrollo de software:

- Una prueba de desarrollador o un entorno de "espacio aislado".
- Un entorno de ensayo basado en intranet.
- Un entorno de producción orientado a Internet.

Cada uno de estos entornos tiene diferentes requisitos de configuración y seguridad, y cada uno de ellos plantea desafíos de implementación únicos.

### <a name="the-fabrikam-inc-server-infrastructure"></a>La infraestructura de servidor de Fabrikam, Inc.

Esta es la infraestructura de desarrollo e implementación de alto nivel en Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

Las estaciones de trabajo de desarrollador, la infraestructura de control de código fuente, el entorno de prueba del desarrollador y el entorno de ensayo residen en la red de la intranet dentro del dominio Fabrikam.net. El entorno de producción reside en una red perimetral (también conocida como DMZ, zona desmilitarizada y subred filtrada), que está aislada de la red de la intranet mediante un firewall. Este es un escenario de implementación común: normalmente aísla los servidores web con conexión a Internet desde la infraestructura de servidor interna mediante el uso de firewalls o servidores de puerta de enlace.

En este ejemplo:

- Un servidor de Team Foundation Server (TFS) 2010 con un servidor de compilación independiente proporciona funcionalidad de control de código fuente y de integración continua (CI).
- El entorno de prueba para desarrolladores incluye un servidor Web de Internet Information Services (IIS) 7,5 y un servidor de base de datos de SQL Server 2008 R2.
- El entorno de producción incluye varios servidores Web de IIS 7,5 sincronizados por un servidor de controlador del marco de granja de servidores Web (WFF), junto con un servidor de base de datos de SQL Server 2008 R2. En la práctica, el servidor de base de datos puede usar la agrupación en clústeres o la creación de reflejo para mejorar la escalabilidad y la disponibilidad.
- El entorno de ensayo está diseñado para replicar la configuración del entorno de producción lo más cerca posible.
- Las directivas de aislamiento de red y firewall de no permiten la implementación directa y automatizada desde la intranet a la red perimetral.

La configuración de cada uno de estos entornos se describe con más detalle en el segundo tutorial, [configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Roles de equipo para ALM

Estos usuarios participan en la creación, administración, compilación y publicación de la solución Contact Manager:

- Matt HINK es un desarrollador de aplicaciones web en Fabrikam, Inc. Forma parte del equipo que desarrolló la solución Contact Manager mediante Visual Studio 2010. Matt tiene derechos de administrador completos en los servidores en el entorno de prueba para desarrolladores, que le permite configurar el entorno para satisfacer sus necesidades. También tiene acceso de usuario a la instancia de TFS de Visual Studio 2010, donde almacena el código fuente de la solución Contact Manager.
- Rob Walters es un administrador del servidor para el equipo de desarrollo de Fabrikam, Inc. Rob tiene acceso administrativo en el servidor TFS para que pueda configurar todos los aspectos de TFS y Team Build. Rob también tiene acceso administrativo a los servidores Web de prueba y ensayo, y actúa como administrador de la base de datos (DBA) para los servidores de bases de datos en los entornos de prueba y ensayo. Rob ha configurado Team Build en el servidor TFS para llevar a cabo estas tareas:

    - Compilar y ejecutar pruebas unitarias en la aplicación cada vez que un usuario protege un archivo en TFS. Esto se denomina CI.
    - Implemente la aplicación Contact Manager en el entorno de prueba automáticamente una vez que la aplicación pase las pruebas unitarias. Esto incluye la publicación de la base de datos en los servidores de prueba en la implementación inicial y las actualizaciones en la base de datos después de la implementación inicial.
    - Implemente la aplicación Contact Manager en el entorno de ensayo en un proceso de un solo paso.
    - Cree un paquete web que un administrador de servidor Web y un DBA puedan usar para publicar la aplicación en el entorno de producción.
- Lisa Andrews es un administrador del servidor responsable de la implementación de aplicaciones en los servidores de producción de Fabrikam, Inc. Tiene acceso de lectura al recurso compartido en el que la compilación del equipo de TFS almacena el paquete de implementación web una vez que compila la aplicación Contact Manager. También tiene acceso administrativo a los servidores Web de producción para que pueda implementar la aplicación en producción. Además, actúa como el DBA que implementa las bases de datos y las actualizaciones de la base de datos en el servidor de base de datos en el entorno de producción.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>La solución Contact Manager

La solución Contact Manager está diseñada para permitir que los usuarios registrados y que han iniciado sesión agreguen y editen información de contacto a través de una interfaz Web. La solución Contact Manager consta de cuatro proyectos individuales:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. Mvc**. Se trata de un proyecto de aplicación Web ASP.NET MVC3 que representa el punto de entrada de la solución. Ofrece cierta funcionalidad básica de aplicaciones Web, como proporcionar a los usuarios la posibilidad de crear y ver los detalles de contacto. La aplicación se basa en un servicio de Windows Communication Foundation (WCF) para administrar contactos y una base de datos de servicios de aplicaciones de ASP.NET para administrar la autenticación y la autorización.
- **ContactManager. Database**. Se trata de un proyecto de base de datos de Visual Studio 2010. El proyecto define el esquema de una base de datos que almacena los detalles de contacto.
- **ContactManager. Service**. Se trata de un proyecto de servicio Web WCF. WCF expone un punto de conexión que permite a los llamadores realizar operaciones de creación, recuperación, actualización y eliminación (CRUD) en la base de datos de Contact Manager. El servicio se basa en la base de datos de Contact Manager y en el ensamblado ContactManager. Common. dll.
- **ContactManager. Common**. Se trata de un proyecto de biblioteca de clases. El servicio WCF se basa en los tipos definidos en este ensamblado.

En el primer tutorial de esta serie, [implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md), se proporciona una revisión completa de la solución y sus requisitos de implementación.

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tareas de implementación

En la implementación de aplicaciones en diferentes entornos de una organización de gran tamaño, se implican varias tareas distintas. Estas son las tareas clave que cubren los tutoriales:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Esta es una lista de cada paso del proceso de implementación desde la perspectiva de los usuarios descritos anteriormente en este documento:

1. Todos los miembros del equipo deben revisar la solución Contact Manager en Visual Studio 2010 para determinar los requisitos y problemas de implementación clave.
2. Matt HINK puede implementar la solución Contact Manager directamente desde la estación de trabajo del desarrollador al entorno de prueba para desarrolladores, para realizar una prueba inicial de la lógica de implementación.
3. Matt HINK agrega la aplicación al control de código fuente en TFS.
4. Rob Walters crea varias definiciones de compilación para la solución Contact Manager en Team Build. Una definición de compilación usa CI para implementar la solución en el entorno de prueba del desarrollador cada vez que un usuario protege código nuevo. Otra definición de compilación permite a los usuarios desencadenar implementaciones en el entorno de ensayo según sea necesario.
5. Cada vez que un usuario protege el nuevo código, Team Build compila automáticamente los componentes de la solución, ejecuta las pruebas unitarias e implementa la solución en el entorno de prueba del desarrollador si la compilación se realizó correctamente y se superan las pruebas unitarias.
6. Cuando un usuario desencadena una implementación en el entorno de ensayo, la solución se empaqueta e implementa en un proceso de un solo paso. Este proceso también genera un paquete para la implementación manual en el entorno de producción.
7. Lisa Andrews implementa la aplicación en el entorno de producción importando manualmente el paquete web creado en el paso 6.

### <a name="key-deployment-issues"></a>Problemas de implementación clave

La solución Contact Manager y el escenario Fabrikam, Inc. resaltan varios problemas y desafíos comunes que pueden surgir al implementar soluciones complejas de escala empresarial. Por ejemplo:

- Debe ser capaz de implementar proyectos en varios entornos, como los entornos de desarrollo o pruebas, las plataformas de ensayo y los servidores de producción. La solución debe implementarse con distintos valores de configuración para cada entorno.
- Debe implementar varios proyectos dependientes simultáneamente como parte de un proceso de compilación e implementación automatizado o de un solo paso.
- Debe poder impulsar la implementación desde un proceso automatizado. Por ejemplo, desea utilizar un proceso de CI para implementar aplicaciones web en un entorno de ensayo cuando se protege un nuevo código.
- Debe ser capaz de controlar el proceso de implementación y establecer variables de implementación desde fuera de Visual Studio, ya que es poco probable que los desarrolladores tengan la configuración correcta o las credenciales necesarias para cada entorno de destino.
- Debe implementar proyectos de base de datos basados en esquemas y conservar los datos existentes en las implementaciones posteriores.
- Debe implementar las bases de datos de pertenencia de forma ad hoc sin implementar los datos de la cuenta de usuario. También es posible que necesite actualizar el esquema de las bases de datos de pertenencia implementadas sin perder los datos de las cuentas de usuario existentes.
- Debe excluir determinados archivos o carpetas al implementar contenido en varios entornos de destino.

Además, la administración de la implementación cuando las actualizaciones son frecuentes y incremental genera algunos desafíos adicionales. Por ejemplo:

- Las pruebas unitarias se ejecutan cada vez que un desarrollador protege el código nuevo. Solo desea implementar la solución si el código pasa las pruebas unitarias.
- Al implementar una aplicación web en un entorno de ensayo o de producción, desea redirigir a los usuarios a una *aplicación\_archivo. htm sin conexión* mientras dure el proceso de implementación.
- Desea registrar las actividades de implementación. El proceso de implementación debe enviar notificaciones por correo electrónico de implementaciones correctas o con errores a destinatarios designados.
- Si se produce un error en una implementación automatizada, el proceso de implementación debe reintentar la implementación actual o implementar el paquete web anterior en su lugar.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Siguiente](application-lifecycle-management-from-development-to-production.md)
