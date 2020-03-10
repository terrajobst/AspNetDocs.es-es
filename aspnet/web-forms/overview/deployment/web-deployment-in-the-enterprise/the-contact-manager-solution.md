---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: La solución Contact Manager | Microsoft Docs
author: jrjlee
description: En esta serie de tutoriales se usa una&#x2014;solución de ejemplo de&#x2014;la solución Contact Manager para representar una aplicación de escala empresarial con un leve realista...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473815"
---
# <a name="the-contact-manager-solution"></a>La solución Contact Manager

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En esta [serie de tutoriales](web-deployment-in-the-enterprise.md) se usa una&#x2014;solución de ejemplo de&#x2014;la solución Contact Manager para representar una aplicación de escala empresarial con un nivel de complejidad realista. En este tema se presenta la solución Contact Manager, se describen los componentes clave de la solución y se identifican los desafíos de la implementación de este tipo de aplicación en varias plataformas de destino en un entorno empresarial.
> 
> A medida que trabaja en los temas de estos tutoriales, puede usar la solución Contact Manager como una implementación de referencia que muestra cómo puede cumplir desafíos específicos en escenarios de implementación empresarial. En el tema siguiente, [configuración de la solución Contact Manager](setting-up-the-contact-manager-solution.md), se describe cómo descargar y ejecutar la solución en la estación de trabajo del desarrollador.

## <a name="solution-overview"></a>Información general de la solución

La solución Contact Manager consta de cuatro proyectos individuales:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. Mvc**. Se trata de un proyecto de aplicación web MVC 3 ASP.NET que representa el punto de entrada de la solución. Ofrece cierta funcionalidad básica de aplicaciones Web, como proporcionar a los usuarios la posibilidad de crear y ver los detalles de contacto. La aplicación se basa en un servicio de Windows Communication Foundation (WCF) para administrar contactos y una base de datos de servicios de aplicaciones de ASP.NET para administrar la autenticación y la autorización.
- **ContactManager. Database**. Se trata de un proyecto de base de datos de Visual Studio. El proyecto define el esquema de una base de datos que almacena los detalles de contacto.
- **ContactManager. Service**. Se trata de un proyecto de servicio Web WCF. El servicio WCF expone un punto de conexión que permite a los llamadores realizar operaciones de creación, recuperación, actualización y eliminación (CRUD) en la base de datos **ContactManager** . El servicio se basa en la base de datos **ContactManager** y en el ensamblado **ContactManager. Common. dll** .
- **ContactManager. Common**. Se trata de un proyecto de biblioteca de clases. El servicio WCF se basa en los tipos definidos en este ensamblado.

La solución también incluye una carpeta de soluciones denominada publicar. Contiene varios archivos de proyecto y archivos de comandos personalizados que muestran cómo se puede controlar y manipular el proceso de compilación e implementación. Estos se describen con más detalle más adelante en este tutorial.

En un nivel conceptual, los componentes de la solución encajan de la siguiente manera:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mientras que la aplicación Web ASP.NET MVC 3 usa el proveedor de pertenencia ASP.NET, todas las páginas de la aplicación web permiten el acceso anónimo. Esto no es una configuración realista. Sin embargo, la solución se configura de este modo para facilitar la implementación y prueba de la solución sin necesidad de configurar las cuentas de usuario y los roles.

## <a name="deployment-challenges"></a>Desafíos de la implementación

La solución Contact Manager muestra varios desafíos de implementación que son comunes a muchos escenarios de implementación empresarial:

- La solución consta de varios proyectos dependientes. Debe implementar estos proyectos simultáneamente.
- Las cadenas de conexión y los puntos de conexión de servicio deben actualizarse para cada entorno y, en muchos casos, esta información no estará disponible para el desarrollador.
- Al implementar la base de datos **ContactManager** en entornos de ensayo y producción, debe conservar los datos existentes en las implementaciones posteriores.
- Al implementar la base de datos de servicios de aplicaciones de ASP.NET, debe implementar algunos datos de configuración, pero omitir los datos de la cuenta de usuario.
- Los proyectos incluyen algunos archivos y carpetas que no se deben implementar. Debe excluir estos archivos y carpetas del proceso de implementación.
- La solución debe admitir la implementación automatizada desde un servidor de compilación de Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general de alto nivel de la solución Contact Manager y se han identificado algunos de los desafíos de implementación inherentes que son comunes a muchos de los escenarios de implementación empresarial. En los temas restantes de este tutorial se describen algunas de las técnicas que puede usar para satisfacer estos desafíos.

En el tema siguiente, [configuración de la solución Contact Manager](setting-up-the-contact-manager-solution.md), se describe cómo descargar y ejecutar la solución en la estación de trabajo del desarrollador.

> [!div class="step-by-step"]
> [Anterior](web-deployment-in-the-enterprise.md)
> [Siguiente](setting-up-the-contact-manager-solution.md)
