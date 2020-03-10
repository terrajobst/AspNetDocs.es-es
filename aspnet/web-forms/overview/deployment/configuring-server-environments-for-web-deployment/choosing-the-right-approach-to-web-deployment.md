---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Elección del enfoque adecuado para la implementación web | Microsoft Docs
author: jrjlee
description: Al trabajar con la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) 2,0 o posterior, hay tres enfoques principales que puede usar para obtener...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441427"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Elegir el enfoque adecuado para la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Al trabajar con la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) 2,0 o posterior, hay tres enfoques principales que puede usar para obtener las aplicaciones web empaquetadas en un servidor Web. Puede:
> 
> - Implemente la aplicación desde una ubicación remota mediante el destino del *servicio de Deployment Agent web* (también conocido como "agente remoto") en el servidor de destino.
> - Implemente la aplicación desde una ubicación remota mediante Web Deploy a petición (también conocido como "agente Temp").
> - Implemente la aplicación desde una ubicación remota destinada al *controlador de web deploy de IIS* en el servidor de destino.
> - Implemente la aplicación copiando manualmente el paquete Web en el servidor de destino e importándolos a través del administrador de IIS.
> 
> La forma de configurar los servidores Web de destino dependerá del enfoque de implementación que desee usar. Este tema le ayudará a decidir qué enfoque de implementación es el adecuado para usted.

En esta tabla se muestran las principales ventajas y desventajas de cada enfoque de implementación, junto con los escenarios que normalmente se ajustan a cada enfoque.

| Enfoque | Ventajas | Desventajas | Escenarios típicos |
| --- | --- | --- | --- |
| Agente remoto | Es fácil de configurar. Es adecuado para las actualizaciones periódicas de las aplicaciones web y el contenido. | El usuario debe ser un administrador en el servidor de destino. el usuario no puede proporcionar credenciales alternativas. | Entornos de desarrollo. Entornos de prueba. |
| Agente Temp | No es necesario instalar Web Deploy en el equipo de destino. La versión más reciente de Web Deploy se usa automáticamente. | El usuario debe ser un administrador en el servidor de destino. el usuario no puede proporcionar credenciales alternativas. | Entornos de desarrollo. Entornos de prueba. |
| Controlador de Web Deploy | Los usuarios que no son administradores pueden implementar contenido. Es adecuado para las actualizaciones periódicas de las aplicaciones web y el contenido. | Es mucho más complejo de configurar. | Entornos de ensayo. Entornos de producción de intranet. Entornos hospedados. |
| Implementación sin conexión | Es muy fácil de configurar. Es adecuado para entornos aislados. | El administrador del servidor debe copiar e importar manualmente el paquete web cada vez. | Entornos de producción orientados a Internet. Entornos de red aislados. |

## <a name="using-the-remote-agent"></a>Usar el agente remoto

Al instalar Web Deploy con la configuración predeterminada en un servidor de destino, el servicio de Deployment Agent web (el "agente remoto") se instala y se inicia automáticamente. De forma predeterminada, el agente remoto expone un punto de conexión HTTP en esta dirección:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> Puede reemplazar [*Server*] por el nombre de equipo del servidor Web, una dirección IP del servidor web o un nombre de host que se resuelva en el servidor Web.

Los administradores del servidor pueden implementar paquetes web desde una ubicación remota, como un equipo del desarrollador o un servidor de compilación, especificando la dirección del punto de conexión. Por ejemplo, suponga que Matt HINK en Fabrikam, Inc. ha compilado el proyecto de aplicación web ContactManager. MVC en su equipo del desarrollador. El proceso de compilación genera un paquete Web, junto con un archivo *. deploy. cmd* que contiene los comandos web deploy necesarios para instalar el paquete. Si Matt es un administrador del servidor de TESTWEB1, puede implementar la aplicación web en el servidor Web de prueba ejecutando este comando en su equipo del Desarrollador:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

En realidad, el ejecutable de Web Deploy puede deducir la dirección del extremo del agente remoto si proporciona el nombre del equipo, por lo que Matt solo tiene que escribir lo siguiente:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Para obtener más información sobre la sintaxis de línea de comandos de Web Deploy y los archivos *. deploy. cmd* , vea [Cómo: instalar un paquete de implementación con el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

El agente remoto ofrece una manera sencilla de implementar contenido desde una ubicación remota, y este enfoque puede funcionar bien con una implementación con un solo clic o automatizada. Sin embargo, el usuario que ejecuta el comando de implementación también debe ser un administrador de dominio o un miembro del grupo de administradores local en el servidor de destino. Además, el agente remoto no admite la autenticación básica, por lo que no se pueden pasar credenciales alternativas en la línea de comandos.

El agente remoto proporciona un enfoque útil para la implementación en escenarios de desarrollo o pruebas, en los que no es raro que los desarrolladores tengan un control total del administrador sobre un entorno de servidor de prueba y, por lo general, las aplicaciones se vuelven a generar y volver a implementar. mayor. Sin embargo, este enfoque suele ser menos aceptable para entornos de ensayo o de producción.

Para obtener un ejemplo de un extremo a otro de un escenario que usa el enfoque del agente remoto, vea [escenario: configurar un entorno de prueba para la implementación web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Uso del agente Temp

El enfoque del agente temporal para la implementación es similar al enfoque del agente remoto. Sin embargo, a diferencia del enfoque del agente remoto, no es necesario instalar Web Deploy en el servidor Web de destino. En su lugar, al realizar la implementación, Web Deploy instalará una versión temporal del servicio del agente de implementación web en el servidor de destino y la usará para implementar el contenido en IIS. Una vez completada la implementación, se quitan todos los archivos temporales.

Si desea usar la configuración del proveedor del agente Temp, agregue la marca **/g** al comando de implementación:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> No se puede usar el agente Temp si el servicio del agente de implementación web está instalado en el equipo de destino, incluso si el servicio no se está ejecutando.

La ventaja de este enfoque es que no es necesario mantener las instalaciones de Web Deploy en los servidores de destino. Además, no es necesario asegurarse de que los equipos de origen y de destino ejecuten la misma versión de Web Deploy. Sin embargo, este enfoque se ve afectado por las mismas limitaciones principales que el enfoque del agente remoto, es decir, que debe ser un administrador local en el servidor de destino para implementar contenido y solo se admite la autenticación NTLM. El enfoque del agente temporal también requiere una configuración inicial mucho más del entorno de destino.

Para obtener más información sobre el uso del agente Temp, consulte [Cómo: instalar un paquete de implementación con el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) y [Web deploy a petición](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Usar el controlador de Web Deploy

En el caso de IIS 7 y versiones posteriores, Web Deploy ofrece un enfoque de implementación alternativo a través del controlador de Web Deploy de IIS. El controlador de Web Deploy está estrechamente integrado con el servicio de administración web (WMSvc) de IIS, que está diseñado para permitir a los usuarios administrar sitios web de IIS desde ubicaciones remotas.

De forma predeterminada, el agente remoto expone un punto de conexión HTTP en esta dirección:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> Puede reemplazar [*Server*] por el nombre de equipo del servidor Web, una dirección IP del servidor web o un nombre de host que se resuelva en el servidor Web.

La gran ventaja del controlador de Web Deploy sobre el agente remoto y el agente Temp, es que puede configurar IIS para permitir que los usuarios que no son administradores implementen aplicaciones y contenido en sitios web de IIS específicos. El controlador de Web Deploy también admite la autenticación básica, por lo que puede proporcionar credenciales alternativas como parámetros en los comandos de Web Deploy. El principal inconveniente es que el controlador de Web Deploy es inicialmente mucho más complicado de instalar y configurar.

En el caso de los usuarios que no son administradores, el servicio de administración web (WMSvc) solo permitirá que el usuario se conecte a IIS mediante una conexión de nivel de sitio, en lugar de una conexión de nivel de servidor. Para tener acceso a un sitio determinado, puede incluir una cadena de consulta específica del sitio en la dirección del punto de conexión:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Por ejemplo, supongamos que un proceso de compilación está configurado para implementar automáticamente una aplicación web en un entorno de ensayo después de cada compilación correcta. Si ha usado el enfoque de agente remoto, debe hacer que la identidad del proceso de compilación sea un administrador en los servidores de destino. Por el contrario, el uso del método de controlador de web deploy se puede conceder&#x2014;a un usuario que&#x2014;no sea administrador**FABRIKAM\stagingdeployer** en este caso solo a un sitio web de IIS específico y el proceso de compilación puede proporcionar estas credenciales para implementar el paquete Web.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Para obtener más información sobre Web Deploy las operaciones de línea de comandos y la sintaxis, vea [Web deploy referencia](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)de la línea de comandos. Para obtener más información sobre el uso del archivo *. deploy. cmd* , consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

El controlador de Web Deploy proporciona un enfoque útil para la implementación en entornos de ensayo, entornos hospedados y entornos de producción basados en intranet, donde el acceso remoto al servidor está disponible pero no las credenciales de administrador.

Para obtener un ejemplo de un extremo a otro de un escenario que usa el enfoque de controlador de Web Deploy, consulte [escenario: configurar un entorno de ensayo para la implementación web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Uso de la implementación sin conexión

En algunos casos, no es posible o práctico implementar aplicaciones y contenido en un sitio web de IIS desde una ubicación remota. Por ejemplo, los equipos de origen y de destino pueden estar en redes aisladas o segmentos de red, o la Directiva de Firewall puede no permitir el acceso remoto.

En escenarios como estos, puede seguir utilizando las capacidades de empaquetado y publicación de Web Deploy; simplemente no puede usarlas desde una ubicación remota. En su lugar, un administrador en el servidor de destino debe copiar el paquete Web en el servidor e importarlo a través del administrador de IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

El enfoque de implementación sin conexión suele ser útil en entornos de producción orientados a Internet, donde los servidores de una red perimetral pueden tener conectividad restringida con equipos de la red interna.

Para obtener un ejemplo de un extremo a otro de un escenario que usa el enfoque de implementación sin conexión, consulte [escenario: configurar un entorno de producción para la implementación web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre Web Deploy las operaciones de línea de comandos y la sintaxis, vea [Web deploy referencia](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx)de la línea de comandos. Para obtener más información sobre el uso del archivo *. deploy. cmd* , consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Para obtener instrucciones generales sobre las distintas formas en las que puede implementar paquetes web desde un equipo remoto, consulte [uso de web deploy de forma remota](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Para obtener más información sobre el uso de Web Deploy a petición, consulte [Web deploy a petición](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-server-environments-for-web-deployment.md)
> [Siguiente](scenario-configuring-a-test-environment-for-web-deployment.md)
