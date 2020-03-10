---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Compilar y empaquetar proyectos de aplicación Web | Microsoft Docs
author: jrjlee
description: Si desea implementar un proyecto de aplicación web en un entorno de servidor remoto, la primera tarea consiste en compilar el proyecto y generar un paquete de implementación WEBA...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463231"
---
# <a name="building-and-packaging-web-application-projects"></a>Crear y empaquetar proyectos de aplicación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Si desea implementar un proyecto de aplicación web en un entorno de servidor remoto, la primera tarea consiste en compilar el proyecto y generar un paquete de implementación web. En este tema se describe cómo funciona el proceso de compilación para los proyectos de aplicación Web. En concreto, se explica lo siguiente:
> 
> - Cómo la canalización de publicación web (WPP) amplía el proceso de compilación para incluir la funcionalidad de implementación.
> - Cómo la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) convierte la aplicación web en un paquete de implementación.
> - Cómo funciona el proceso de compilación y empaquetado y qué archivos se crean.

En Visual Studio 2010, el proceso de compilación e implementación de los proyectos de aplicación web es compatible con WPP. WPP proporciona un conjunto de destinos Microsoft Build Engine (MSBuild) que amplían la funcionalidad de MSBuild y permiten que se integren con Web Deploy. En Visual Studio, puede ver esta funcionalidad extendida en las páginas de propiedades del proyecto de aplicación Web. La página **Web de paquete/publicación** , junto con la página **empaquetar/publicar SQL** , le permite configurar el modo en que se empaquetará el proyecto de aplicación web para la implementación cuando se complete el proceso de compilación.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>¿Cómo funciona WPP?

Si echa un vistazo al archivo de proyecto de un C#proyecto de aplicación web basado en, puede ver que importa dos archivos. targets.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

La primera instrucción de **importación** es común a todos C# los proyectos visuales. Este archivo, *Microsoft. CSharp. targets*, contiene los destinos y las tareas que son C#específicas de visual. Por ejemplo, la C# tarea del compilador (**CSC**) se invoca aquí. A su vez, el archivo *Microsoft. CSharp. targets* importa el archivo *Microsoft. Common. targets* . Esto define los destinos comunes a todos los proyectos, como **compilar**, **volver**a generar, **Ejecutar**, **compilar**y **limpiar**. La segunda instrucción **Import** es específica de los proyectos de aplicación Web. A su vez, el archivo *Microsoft. WebApplication. targets* importa el archivo *Microsoft. Web. Publishing. targets* . El archivo *Microsoft. Web. Publishing. targets* básicamente *es* WPP. Define destinos, como **Package** y **MSDeployPublish**, que invocan web deploy para completar varias tareas de implementación.

Para entender cómo se usan estos destinos adicionales, en la solución de ejemplo Contact Manager, abra el archivo *Publish. proj* y eche un vistazo al destino **BuildProjects** .

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Este destino utiliza la tarea **msbuild** para compilar varios proyectos. Observe las propiedades **DeployOnBuild** y **DeployTarget** :

- Esencialmente, la propiedad **DeployOnBuild = true** significa "deseo ejecutar un destino adicional cuando la compilación se complete correctamente".
- La propiedad **DeployTarget** identifica el nombre del destino que desea ejecutar cuando la propiedad **DeployOnBuild** es igual a **true**. En este caso, está especificando que desea que MSBuild ejecute el **paquete** de destino después de compilar el proyecto.

El destino del **paquete** se define en el archivo *Microsoft. Web. Publishing. targets* . Esencialmente, este destino toma el resultado de la compilación del proyecto de aplicación web y lo convierte en un paquete de implementación web que se puede publicar en un servidor Web de IIS.

> [!NOTE]
> Para ver un archivo de proyecto (por ejemplo, <em>ContactManager. Mvc. csproj</em>) en Visual Studio 2010, primero debe descargar el proyecto de la solución. En la ventana <strong>Explorador de soluciones</strong> , haga clic con el botón secundario en el nodo del proyecto y, a continuación, haga clic en <strong>Descargar proyecto</strong>. Vuelva a hacer clic con el botón secundario en el nodo del proyecto y, a continuación, haga clic en <strong>Editar</strong><em>[archivo de proyecto]</em>). El archivo de proyecto se abrirá en su forma XML sin formato. Recuerde volver a cargar el proyecto cuando haya terminado.  
> Para obtener más información sobre los destinos, las tareas y las instrucciones de <strong>importación</strong> de MSBuild, vea [Descripción del archivo de proyecto](understanding-the-project-file.md). Para obtener una introducción más detallada sobre los archivos de proyecto y WPP, vea [dentro del Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>¿Qué es un paquete de implementación web?

Al compilar e implementar un proyecto de aplicación Web, ya sea mediante Visual Studio 2010 o mediante MSBuild directamente, el resultado final suele ser un *paquete de implementación web*. El paquete de implementación web es un archivo. zip. Contiene todo lo que necesita IIS y Web Deploy para volver a crear la aplicación Web, entre los que se incluyen:

- La salida compilada de la aplicación Web, incluido el contenido, los archivos de recursos, los archivos de configuración, los recursos de JavaScript y las hojas de estilos en cascada (CSS), etc.
- Ensamblados para el proyecto de aplicación web y para todos los proyectos a los que se hace referencia en la solución.
- Scripts SQL para generar las bases de datos que se van a implementar con la aplicación Web.

Una vez generado el paquete de implementación web, puede publicarlo en un servidor Web de IIS de varias maneras. Por ejemplo, puede implementarla de forma remota al establecer como destino el servicio del agente remoto Web Deploy o el controlador de Web Deploy en el servidor Web de destino, o puede usar el administrador de IIS para importar manualmente el paquete en el servidor Web de destino. Para obtener más información sobre estos métodos de implementación, consulte [elección del enfoque adecuado para la implementación web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>¿Cómo funciona el proceso de compilación?

Esto muestra lo que ocurre al compilar y empaquetar un proyecto de aplicación web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Al compilar un proyecto de aplicación Web, el proceso de compilación genera un archivo denominado *[nombre del proyecto]. SourceManifest. XML*. Junto con el archivo de proyecto y la salida de la compilación, este *. El archivo SourceManifest. XML* indica web deploy lo que necesita incluir en el paquete de implementación web. Con estas entradas, Web Deploy genera un paquete de implementación web denominado *[Project Name]. zip*.

Junto con el paquete de implementación web, el proceso de compilación genera dos archivos que pueden ayudarle a usar el paquete:

- El archivo *. deploy. cmd* incluye un conjunto de comandos de web deploy con parámetros (MSDeploy. exe) que publican el paquete de implementación web en un servidor Web remoto de IIS. La ejecución del archivo *. deploy. cmd* , con los parámetros adecuados, suele proporcionar una alternativa más rápida y sencilla para construir manualmente los comandos MSDeploy. exe.
- El archivo *SetParameters. XML* proporciona un conjunto de valores de parámetro al comando MSDeploy. exe. Estos valores incluyen propiedades como el nombre de la aplicación Web de IIS en la que desea implementar el paquete, los valores de los extremos de servicio y las cadenas de conexión definidos en el archivo *Web. config* y los valores de propiedad de implementación definidos en las páginas de propiedades del proyecto.

El archivo *SetParameters. XML* es fundamental para administrar el proceso de implementación. Este archivo se genera dinámicamente según el contenido del proyecto de aplicación Web. Por ejemplo, si agrega una cadena de conexión al archivo *Web. config* , el proceso de compilación detectará automáticamente la cadena de conexión, parametrizará la implementación en consecuencia y creará una entrada en el archivo *SetParameters. XML* para que pueda modificar la cadena de conexión como parte del proceso de implementación. En el tema siguiente, [configurar los parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md), se explica con más detalle el rol de este archivo y se describen las distintas formas en que se puede modificar durante la compilación y la implementación.

> [!NOTE]
> En Visual Studio 2010, WPP no admite la precompilación de las páginas de una aplicación Web antes del empaquetado. La siguiente versión de Visual Studio y WPP incluirán la capacidad de precompilar una aplicación web como una opción de empaquetado.

## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general sobre el proceso de compilación y empaquetado de proyectos de aplicación web en Visual Studio 2010. Describe cómo el WPP le permite invocar Web Deploy comandos desde MSBuild y explica cómo funciona el proceso de compilación y empaquetado.

Una vez que haya creado un paquete de implementación web, el siguiente paso es implementarlo. Para obtener más información, vea [configurar los parámetros para la implementación de paquetes web](configuring-parameters-for-web-package-deployment.md) e [implementar paquetes web](deploying-web-packages.md).

## <a name="further-reading"></a>Información adicional

Los temas siguientes de este tutorial, [configurar los parámetros para la implementación de](configuring-parameters-for-web-package-deployment.md) paquetes web e [implementar paquetes web](deploying-web-packages.md), proporcionan instrucciones sobre cómo usar el paquete web que ha creado. El último tutorial de esta serie, [Advanced Enterprise web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), proporciona instrucciones sobre cómo personalizar y solucionar problemas del proceso de empaquetado.

Para obtener una introducción más detallada sobre los archivos de proyecto y WPP, vea [dentro del Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-build-process.md)
> [Siguiente](configuring-parameters-for-web-package-deployment.md)
