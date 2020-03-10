---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Implementar paquetes Web | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo puede publicar paquetes de implementación web en un servidor remoto mediante la herramienta de implementación web de Internet Information Services (IIS) (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464971"
---
# <a name="deploying-web-packages"></a>Implementar paquetes web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo puede publicar paquetes de implementación web en un servidor remoto mediante la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) 2,0.
> 
> Hay dos formas principales en las que puede implementar un paquete Web en un servidor remoto:
> 
> - Puede usar la utilidad de línea de comandos MSDeploy. exe directamente.
> - Puede ejecutar el archivo *[nombre de proyecto]. deploy. cmd* que genera el proceso de compilación.
> 
> El resultado final es el mismo, independientemente del enfoque que use. En esencia, todo el archivo *. deploy. cmd* es ejecutar MSDeploy. exe con algunos valores predeterminados, de modo que no tenga que proporcionar tanta información como para implementar el paquete. Esto simplifica el proceso de implementación. Por otro lado, el uso de MSDeploy. exe le proporciona mucha más flexibilidad sobre el modo en que se implementa el paquete.
> 
> El método que use dependerá de diversos factores, como el control que necesita sobre el proceso de implementación y si tiene como destino el servicio del agente Web Deploy remoto o el controlador de Web Deploy. En este tema se explica cómo usar cada enfoque y se identifica Cuándo es adecuado cada enfoque.
> 
> En las tareas y los tutoriales de este tema se da por supuesto que:
> 
> - Ha compilado y empaquetado su aplicación Web, tal como se describe en [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).
> - Ha modificado el archivo *SetParameters. XML* para proporcionar los valores de parámetro correctos para el entorno de destino, tal como se describe en [configuración de parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md).

Ejecutar el archivo [*nombre de proyecto*] *. deploy. cmd* es la manera más sencilla de implementar un paquete Web. En concreto, el uso del archivo *. deploy. cmd* ofrece estas ventajas con respecto al uso de MSDeploy. exe directamente:

- No es necesario especificar la ubicación del paquete&#x2014;de implementación web. el archivo *deploy. cmd* ya sabe dónde está.
- No es necesario especificar la ubicación del archivo&#x2014; *SetParameters. XML* . el archivo *. deploy. cmd* ya sabe dónde está.
- No es necesario especificar los proveedores&#x2014;de MSDeploy de origen y de destino. el archivo *deploy. cmd* ya sabe qué valores se deben usar.
- No es necesario especificar la configuración&#x2014;de la operación de MsDeploy. el archivo deploy. *cmd* agrega automáticamente los valores necesarios al comando MSDeploy. exe.

Antes de usar el archivo *. deploy. cmd* para implementar un paquete Web, debe asegurarse de lo siguiente:

- El archivo *. deploy. cmd* , el [*nombre del proyecto*]. El archivo *SetParameters. XML* y el paquete web ([*nombre del proyecto*]. *zip*) en la misma carpeta.
- Web Deploy (MSDeploy. exe) está instalado en el equipo que ejecuta el archivo *. deploy. cmd* .

El archivo *. deploy. cmd* admite varias opciones de línea de comandos. Al ejecutar el archivo desde un símbolo del sistema, esta es la sintaxis básica:

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

Debe especificar una marca **/t** o **/y** para indicar si desea realizar una ejecución de prueba o una implementación en vivo, respectivamente (no use ambas marcas en el mismo comando). En esta tabla se explica el propósito de cada una de estas marcas.

| Marcar | Description |
| --- | --- |
| **/T** | Llama a MSDeploy. exe con la marca **– Whatif** , que indica una ejecución de prueba. En lugar de implementar el paquete, se crea un informe de lo que sucedería si se implementara el paquete. |
| **/Y** | Llama a MSDeploy. exe sin la marca **– Whatif** . Esto implementa el paquete en el equipo local o en el servidor de destino especificado. |
| **/M** | Especifica el nombre del servidor de destino o la dirección URL del servicio. Para obtener más información sobre los valores que puede proporcionar aquí, consulte la sección **consideraciones sobre puntos de conexión** de este tema. Si omite la marca **/m** , el paquete se implementará en el equipo local. |
| **/A** | Especifica el tipo de autenticación que MSDeploy. exe debe usar para realizar la implementación. Los valores posibles son **NTLM** y **Basic**. Si omite la marca **/a** , el tipo de autenticación tiene como valor predeterminado **NTLM** para la implementación en el servicio de agente remoto de web deploy y en **básica** para la implementación en el controlador de web deploy. |
| **/U** | Especifica el nombre de usuario. Esto solo se aplica si utiliza la autenticación básica. |
| **/P** | Especifica la contraseña. Esto solo se aplica si utiliza la autenticación básica. |
| **/L** | Indica que el paquete debe implementarse en la instancia de IIS Express local. |
| **/G** | Especifica que el paquete se implementa mediante la [configuración del proveedor tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Si se omite la marca **/g** , el valor predeterminado **es false**. |

> [!NOTE]
> Cada vez que el proceso de compilación crea un paquete Web, también crea un archivo denominado *[nombre del proyecto]. deploy-README. txt* que explica estas opciones de implementación.

Además de estas marcas, puede especificar Web Deploy configuración de operación como parámetros adicionales *. deploy. cmd* . Cualquier configuración adicional que especifique se pasa simplemente al comando MSDeploy. exe subyacente. Para obtener más información sobre esta configuración, vea [Web deploy la configuración](https://technet.microsoft.com/library/dd569089(WS.10).aspx)de la operación.

Supongamos que desea implementar el proyecto de aplicación web ContactManager. MVC en un entorno de prueba ejecutando el archivo *. deploy. cmd* . El entorno de prueba está configurado para usar el servicio del agente Web Deploy remoto, tal y como se describe en [configurar un servidor web para la publicación de web deploy (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Para implementar la aplicación Web, debe completar los pasos siguientes.

**Para implementar una aplicación Web mediante el archivo. deploy. cmd**

1. Compile y empaquete el proyecto de aplicación Web, tal como se describe en [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).
2. Modifique el archivo *ContactManager. Mvc. SetParameters. XML* para que contenga los valores de parámetro correctos para el entorno de prueba, como se describe en [configuración de parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y navegue hasta la ubicación del archivo *ContactManager. Mvc. deploy. cmd* .
4. Escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

En este ejemplo:

- La marca **/y** indica que desea implementar realmente el paquete, en lugar de realizar una ejecución de prueba.
- La marca **/m** indica que desea implementar el paquete en el servidor denominado TESTWEB1. A partir de este valor, MSDeploy. exe intentará implementar el paquete en el Web Deploy servicio del agente remoto en http://TESTWEB1/MSDeployAgentService.
- La marca **/a** indica que desea utilizar la autenticación NTLM. Como tal, no es necesario especificar un nombre de usuario y una contraseña.

Para ilustrar cómo el uso del archivo *. deploy. cmd* simplifica el proceso de implementación, eche un vistazo al comando MSDeploy. exe que se genera y ejecuta al ejecutar *ContactManager. Mvc. deploy. cmd* con las opciones mostradas anteriormente.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Para obtener más información sobre el uso del archivo *. deploy. cmd* para implementar un paquete Web, vea [Cómo: instalar un paquete de implementación con el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Usar MSDeploy. exe

Aunque el uso del archivo *. deploy. cmd* generalmente simplifica el proceso de implementación, hay algunas situaciones en las que es preferible usar MSDeploy. exe directamente. Por ejemplo:

- Si desea implementar en el controlador de Web Deploy como un usuario que no es administrador, no puede usar el archivo *. deploy. cmd* . Esto se debe a un error en Web Deploy 2,0, tal y como se describe en **consideraciones sobre puntos de conexión**.
- Si desea cambiar manualmente entre distintos archivos *SetParameters. XML* en diferentes ubicaciones, puede que prefiera usar MSDeploy. exe directamente.
- Si desea invalidar varios argumentos de la línea de comandos de MSDeploy. exe, es posible que prefiera usar MSDeploy. exe directamente.

Cuando se usa MSDeploy. exe, es necesario proporcionar tres partes clave de la información:

- Parámetro **– source** que indica de dónde proceden los datos.
- Un parámetro **– dest** que indica el lugar al que se van a dirigir los datos.
- Un parámetro **– Verb** que indica la [operación](https://technet.microsoft.com/library/dd568989(WS.10).aspx) que desea realizar.

MSDeploy. exe se basa en [Web deploy proveedores](https://technet.microsoft.com/library/dd569040(WS.10).aspx) para procesar los datos de origen y de destino. Web Deploy incluye muchos proveedores que representan el intervalo de aplicaciones y orígenes de datos con&#x2014;los que puede trabajar, por ejemplo, hay proveedores para SQL Server bases de datos, servidores Web de IIS, certificados, ensamblados de caché global de ensamblados (GAC), varios archivos de configuración diferentes y muchos otros tipos de datos. Tanto el **parámetro – Source** como el **parámetro – dest** deben especificar un proveedor, con el formato **-source**: [*providerName*] = [*Location*]. Al implementar un paquete Web en un sitio web de IIS, debe usar estos valores:

- El proveedor **– source** siempre es [Package](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- El proveedor **-dest** siempre es [automático](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Por ejemplo:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- El **verbo –** siempre está **sincronizado**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Además, deberá especificar otros [valores de configuración específicos del proveedor y de](https://technet.microsoft.com/library/dd569001(WS.10).aspx) [operaciones](https://technet.microsoft.com/library/dd569089(WS.10).aspx)generales. Por ejemplo, supongamos que desea implementar la aplicación web ContactManager. MVC en un entorno de ensayo. La implementación se destinará al controlador de Web Deploy y debe usar la autenticación básica. Para implementar la aplicación Web, debe completar los pasos siguientes.

**Para implementar una aplicación web con MSDeploy. exe**

1. Compile y empaquete el proyecto de aplicación Web, tal como se describe en [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).
2. Modifique el archivo *ContactManager. Mvc. SetParameters. XML* para que contenga los valores de parámetro correctos para el entorno de ensayo, tal como se describe en [configuración de parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md).
3. Abra una ventana del símbolo del sistema y vaya a la ubicación de MSDeploy. exe. Normalmente, se encuentra en%PROGRAMFILES%\IIS\Microsoft Web Deploy V2\msdeploy.exe.
4. Escriba este comando y, a continuación, presione Entrar (descartar los saltos de línea):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

En este ejemplo:

- El parámetro **– source** especifica el proveedor del **paquete** e indica la ubicación del paquete Web.
- El parámetro **– dest** especifica el proveedor **auto** . El valor **computerName** proporciona la dirección URL del servicio del controlador de web deploy en el servidor de destino. El valor **AuthType** indica que desea usar la autenticación básica y, como tal, debe proporcionar un nombre de **usuario** y una **contraseña**. Por último, el valor de **includeAcls = "false"** indica que no desea copiar las listas de control de acceso (ACL) de los archivos de la aplicación Web de origen en el servidor de destino.
- El argumento **– verb: Sync** indica que desea replicar el contenido de origen en el servidor de destino.
- Los argumentos **– disableLink** indican que no desea replicar los grupos de aplicaciones, la configuración del directorio virtual o los certificados de capa de sockets seguros (SSL) en el servidor de destino. Para obtener más información, consulte [Web deploy Link Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- El parámetro **– setParamFile** proporciona la ubicación del archivo *SetParameters. XML* .
- El modificador **– allowUntrusted** indica que Web deploy debe aceptar certificados SSL que no haya emitido una entidad de certificación de confianza. Si va a implementar en el controlador de Web Deploy y ha usado un certificado autofirmado para proteger la dirección URL del servicio, debe incluir este modificador.

## <a name="automating-web-package-deployment"></a>Automatizar la implementación de paquetes Web

En muchos escenarios empresariales, querrá implementar los paquetes web como parte de una implementación más grande o automatizada. Independientemente de si decide implementar los paquetes Web mediante la ejecución del archivo *. deploy. cmd* o mediante MSDeploy. exe directamente, puede parametrizar los comandos y llamarlos desde un destino en un archivo de proyecto de Microsoft Build Engine (MSBuild).

En la solución de ejemplo Contact Manager, eche un vistazo al destino **PublishWebPackages** en el archivo *Publish. proj* . Este destino se ejecuta una vez para cada archivo *. deploy. cmd* identificado por una lista de elementos denominada **PublishPackages**. El destino usa propiedades y metadatos de elemento para crear un conjunto completo de valores de argumento para cada archivo *. deploy. cmd* y, a continuación, usa la tarea **exec** para ejecutar el comando.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Para obtener una introducción más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizados en general, vea [Descripción del archivo de proyecto](understanding-the-project-file.md) y [Descripción del proceso de compilación](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Consideraciones sobre puntos de conexión

Independientemente de si implementa el paquete Web mediante la ejecución del archivo *. deploy. cmd* o mediante MSDeploy. exe directamente, debe especificar un nombre de equipo o un punto de conexión de servicio para la implementación.

Si el servidor Web de destino está configurado para la implementación mediante el Web Deploy servicio del agente remoto, debe especificar la dirección URL del servicio de destino como destino.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

Como alternativa, puede especificar el nombre del servidor solo como destino y Web Deploy deducirá la dirección URL del servicio de agente remoto.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Si el servidor Web de destino está configurado para la implementación mediante el controlador de Web Deploy, debe especificar la dirección del extremo del servicio de administración web de IIS (WMSvc) como destino. De forma predeterminada, tiene el siguiente formato:

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

Puede tener como destino cualquiera de estos extremos mediante el archivo *. deploy.* exe o MSDeploy. exe directamente. Sin embargo, si desea implementar en el controlador de Web Deploy como un usuario que no es administrador, tal y como se describe en [configurar un servidor web para la publicación de web deploy (controlador de web deploy)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), debe agregar una cadena de consulta a la dirección del extremo de servicio.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Esto se debe a que el usuario que no es administrador no tiene acceso de nivel de servidor a IIS; solo tiene acceso a un sitio web de IIS específico. En el momento de la escritura, debido a un error en la canalización de publicación web (WPP), no puede ejecutar el archivo *. deploy. cmd* con una dirección de punto de conexión que incluya una cadena de consulta. En este escenario, debe implementar el paquete Web mediante MSDeploy. exe directamente.

> [!NOTE]
> Para obtener más información sobre el servicio de agente remoto de Web Deploy y el controlador de Web Deploy, consulte [elección del enfoque adecuado para la implementación web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Para obtener instrucciones sobre cómo configurar los archivos de proyecto específicos del entorno para implementarlos en estos puntos de conexión, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Consideraciones de la autenticación

Independientemente de si implementa el paquete Web mediante la ejecución del archivo *. deploy. cmd* o mediante MSDeploy. exe directamente, debe especificar un tipo de autenticación. Web Deploy acepta dos valores posibles: **NTLM** o **básico**. Si especifica la autenticación básica, también debe proporcionar un nombre de usuario y una contraseña. Hay varios factores que debe tener en cuenta al seleccionar un tipo de autenticación:

- Si va a realizar la implementación en el Web Deploy servicio del agente remoto, debe usar la autenticación NTLM. El servicio de agente remoto no acepta credenciales de autenticación básica.
- Si va a implementar en el controlador de Web Deploy, puede usar la autenticación NTLM o básica. La configuración predeterminada es autenticación básica. Aunque la autenticación básica se basa en los nombres de usuario y contraseñas que se transmiten en texto sin formato, las credenciales están protegidas, ya que el controlador de Web Deploy siempre usa el cifrado SSL.
- Si el paquete web incluye una base de datos y el servidor Web y el servidor de base de datos son equipos independientes, no podrá implementar la base de datos mediante la autenticación NTLM debido a la [limitación de "doble salto" de NTLM](https://go.microsoft.com/?linkid=9805120). Debe usar SQL Server credenciales en la cadena de conexión de implementación o proporcionar credenciales de autenticación básica a Web Deploy. Este problema se describe con más detalle en [implementación de bases de datos de pertenencia en entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo se puede implementar un paquete Web mediante la ejecución del archivo *. deploy. cmd* o mediante MSDeploy. exe directamente. Se explicó cuando cada enfoque podría ser adecuado y se describe cómo se puede parametrizar y ejecutar un comando de implementación como parte de un proceso de compilación automatizado o de un solo paso mayor.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo crear y parametrizar un paquete de implementación web, vea [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md) y [Configurar parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md). Para obtener instrucciones sobre cómo compilar e implementar paquetes web a partir de una instancia de Team Foundation Server (TFS), consulte [configuración de Team Foundation Server para la implementación web automática](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Para obtener información sobre cómo personalizar y solucionar problemas del proceso de implementación, vea [excluir archivos y carpetas de la implementación](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-parameters-for-web-package-deployment.md)
> [Siguiente](deploying-database-projects.md)
