---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configuración de permisos para la implementación de Team Build | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo configurar permisos para habilitar el servidor de compilación para implementar contenido en servidores web y servidores de bases de datos como parte de una b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518509"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Configurar permisos para la implementación de compilaciones de equipo

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar los permisos para permitir que el servidor de compilación implemente contenido en servidores web y servidores de bases de datos como parte de un proceso de compilación automatizado.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Al instalar el servicio de compilación de Team Foundation Server (TFS) 2010, especifique la identidad con la que desea que se ejecute el servicio. De forma predeterminada, esta es la cuenta de servicio de red. Como alternativa, puede configurar el servicio de compilación para que se ejecute con una cuenta de dominio.

Cualquier tarea de implementación que requiera la autenticación de Windows y que planee automatizar mediante Team Build, se ejecutará con la identidad del servicio de compilación. Como tal, deberá conceder a la identidad del servicio de compilación los permisos necesarios para los servidores web y los servidores de bases de datos.

> [!NOTE]
> La cuenta servicio de red usa la cuenta de equipo para autenticarse en otros equipos. Las cuentas de equipo adoptan el formato * [nombre de dominio]\[nombre de máquina] * **$** &#x2014;por ejemplo, **FABRIKAM\TFSBUILD $** . Como tal, si el servicio de compilación se ejecuta con la identidad de servicio de red, debe conceder los permisos necesarios a la identidad de la cuenta de equipo para el servidor de compilación.

## <a name="configuring-web-server-permissions"></a>Configuración de permisos del servidor Web

Como se describe en [elegir el enfoque adecuado para la implementación web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), hay dos enfoques principales que puede usar si desea implementar paquetes Web en un servidor Web remoto:

- Implemente la aplicación desde una ubicación remota mediante el destino del *servicio de Deployment Agent web* (también conocido como agente remoto) en el servidor de destino.
- Implemente la aplicación desde una ubicación remota estableciendo como destino el controlador de Web Deploy de *Internet Information Services* (*IIS)* en el servidor de destino.

El agente remoto tiene dos limitaciones clave en este caso:

- El agente remoto solo admite la autenticación NTLM. En otras palabras, la implementación debe usar la identidad&#x2014;del servicio de compilación no se puede suplantar a otra cuenta.
- Para usar el agente remoto, la cuenta que realiza la implementación debe ser un administrador en el servidor de destino.

Juntas, estas dos limitaciones hacen que el enfoque del agente remoto sea no deseable para una implementación automatizada de Team Build. Para usar este enfoque, debe convertir la cuenta de servicio de compilación en Administrador en cualquier servidor Web de destino.

Por el contrario, el enfoque del controlador de Web Deploy ofrece varias ventajas:

- El controlador de Web Deploy admite la autenticación básica a través de HTTPS, que le permite pasar las credenciales de una cuenta alternativa a la herramienta de implementación web de IIS (Web Deploy).
- Puede configurar los servidores Web de destino para permitir que los usuarios que no son administradores implementen contenido en sitios web de IIS específicos mediante el controlador de Web Deploy.

Como resultado, es claramente preferible destinar el controlador de Web Deploy al automatizar la implementación de paquetes web desde Team Build. Este es el proceso recomendado:

1. Cree una cuenta de dominio con pocos privilegios para usarla en la implementación.
2. Configure el controlador de Web Deploy y conceda a la cuenta los permisos necesarios para implementar contenido en un sitio web de IIS específico, como se describe en [configuración de un servidor web para la publicación de web deploy (controlador de web deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invoque Web Deploy y destine el controlador de Web Deploy mediante la autenticación básica y proporcione las credenciales de la cuenta de dominio que creó para realizar la implementación.

En la solución de ejemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , especifique el tipo de autenticación (básico o NTLM), las credenciales de web deploy y la dirección del punto de conexión (agente remoto o controlador de web deploy) en el archivo de proyecto específico del entorno. Estos valores se usan para formular y ejecutar un comando de Web Deploy cuando se ejecuta el archivo de proyecto. Para obtener más información, vea [implementar paquetes web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obtener más información sobre cómo configurar el controlador de Web Deploy, incluido cómo configurar permisos, vea [configurar un servidor web para la publicación de web deploy (controlador de web deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obtener más información sobre la configuración del agente remoto, vea [configurar un servidor web para la publicación de web deploy (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configuración de permisos de servidor de base de datos

Para implementar una base de datos en SQL Server, debe:

- Cree un inicio de sesión para la cuenta de implementación en la instancia de SQL Server.
- Conceda los permisos de inicio de sesión **DBCreator** en la instancia de SQL Server.
- Después de la implementación inicial, agregue el inicio de sesión al rol de propietario de la base de datos **\_** en la base de datos de destino. Esto es necesario porque en las implementaciones posteriores, está modificando una base de datos existente en lugar de crear una nueva base de datos.

Puede autenticarse en una instancia de SQL Server mediante la autenticación NTLM o la autenticación de SQL Server:

- Si usa la autenticación NTLM, debe conceder los permisos descritos anteriormente a la cuenta del servicio de compilación.
- Si usa la autenticación de SQL Server, debe conceder los permisos descritos anteriormente a la cuenta SQL Server. También debe incluir el SQL Server nombre de usuario y la contraseña en la cadena de conexión que usa para implementar la base de datos.

Para obtener información paso a paso sobre cómo configurar permisos para la implementación de bases de datos, vea [configurar un servidor de base de datos para la publicación de web deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusión

En este momento, debe comprender los permisos necesarios, junto con las opciones de autenticación abiertas, al automatizar las implementaciones de aplicaciones web y bases de datos desde Team Build. También debe poder implementar los permisos necesarios en los servidores Web de IIS y SQL Server servidores de bases de datos.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre la configuración de entornos de Windows Server para admitir la implementación remota, vea [configuración de entornos de servidor para la implementación web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
