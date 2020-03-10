---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Solucionar problemas del proceso de empaquetado | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo puede recopilar información detallada sobre el proceso de empaquetado mediante la propiedad EnablePackageProcessLoggingAndAssert en la sección M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509755"
---
# <a name="troubleshooting-the-packaging-process"></a>Solucionar problemas del proceso de empaquetado

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo se puede recopilar información detallada sobre el proceso de empaquetado mediante la propiedad **EnablePackageProcessLoggingAndAssert** en el Microsoft Build Engine (MSBuild).
> 
> Cuando establezca la propiedad **EnablePackageProcessLoggingAndAssert** en **true**, MSBuild hará lo siguiente:
> 
> - Agregue información adicional sobre el proceso de empaquetado a los registros de compilación.
> - Registre los errores en determinadas condiciones, por ejemplo, si se encuentran archivos duplicados en la lista de empaquetado.
> - Cree un directorio de registro en la carpeta *ProjectName*\_Package y úselo para registrar información acerca de los archivos que está empaquetando.
> 
> Si se produce un error en el proceso de empaquetado o los paquetes de implementación web no contienen los archivos esperados, puede utilizar esta información para solucionar los problemas del proceso e indicar dónde se está produciendo un error.
> 
> > [!NOTE]
> > La propiedad **EnablePackageProcessLoggingAndAssert** solo funciona Si compila el proyecto con la configuración de **depuración** . La propiedad se omite en otras configuraciones.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Descripción de la propiedad EnablePackageProcessLoggingAndAssert

[Compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) describe cómo la canalización de publicación web (WPP) proporciona un conjunto de destinos de MSBuild que amplían la funcionalidad de MSBuild y permiten que se integren con la herramienta de implementación Web de Internet Information Services (IIS) (Web deploy). Al empaquetar un proyecto de aplicación Web, está invocando destinos WPP.

Muchos de estos destinos WPP incluyen lógica condicional que registra información adicional cuando la propiedad **EnablePackageProcessLoggingAndAssert** se establece en **true**. Por ejemplo, si revisa el destino del **paquete** , puede ver que crea un directorio de registro adicional y escribe una lista de archivos en un archivo de texto si **EnablePackageProcessLoggingAndAssert** es igual a **true**.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Los destinos WPP se definen en el archivo *Microsoft. Web. Publishing. targets* en la carpeta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web Puede abrir este archivo y revisar los destinos en Visual Studio 2010 o cualquier editor XML. Tenga cuidado de no modificar el contenido del archivo.

## <a name="enabling-the-additional-logging"></a>Habilitación del registro adicional

Puede proporcionar un valor para la propiedad **EnablePackageProcessLoggingAndAssert** de varias maneras, en función de cómo compile el proyecto.

Si compila el proyecto desde la línea de comandos, puede proporcionar un valor para la propiedad **EnablePackageProcessLoggingAndAssert** como argumento de la línea de comandos:

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Si utiliza un archivo de proyecto personalizado para compilar los proyectos, puede incluir el valor **EnablePackageProcessLoggingAndAssert** en el atributo **Properties** de la tarea **msbuild** :

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Si utiliza una definición de compilación de Team Foundation Server (TFS) para compilar los proyectos, puede proporcionar un valor para la propiedad **EnablePackageProcessLoggingAndAssert** en la fila de **argumentos de MSBuild** :![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Para obtener más información sobre cómo crear y configurar definiciones de compilación, vea [crear una definición de compilación que admita la implementación](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Como alternativa, si desea incluir el paquete en cada compilación, puede modificar el archivo de proyecto del proyecto de aplicación web para establecer la propiedad **EnablePackageProcessLoggingAndAssert** en **true**. Debe agregar la propiedad al primer elemento **PropertyGroup** del archivo. csproj o. vbproj.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Revisar los archivos de registro

Al compilar y empaquetar un proyecto de aplicación web con **EnablePackageProcessLoggingAndAssert** establecido en **true**, MSBuild crea una carpeta adicional denominada log en la carpeta *projectname*\_package. La carpeta de registro contiene varios archivos:

![](troubleshooting-the-packaging-process/_static/image2.png)

La lista de archivos que se ven variará en función de las cosas del proyecto y del proceso de compilación. Sin embargo, estos archivos se utilizan normalmente para registrar la lista de archivos que el WPP está recopilando para el empaquetado, en varias fases del proceso:

- En el archivo *PreExcludePipelineCollectFilesPhaseFileList. txt* se muestran los archivos que MSBuild recopila para empaquetar antes de que se quiten los archivos que se especifican para la exclusión.
- El archivo *AfterExcludeFilesFilesList. txt* contiene la lista de archivos modificados después de quitar los archivos especificados para la exclusión.

    > [!NOTE]
    > Para obtener más información sobre cómo excluir archivos y carpetas del proceso de empaquetado, vea [excluir archivos y carpetas de la implementación](excluding-files-and-folders-from-deployment.md).
- El archivo *AfterTransformWebConfig. txt* enumera los archivos recopilados para empaquetar después de que se hayan realizado las transformaciones *Web. config* . En esta lista, los archivos de transformación *Web. config* específicos de la configuración, como *Web. Debug. config* y *Web. Release. config*, se excluyen de la lista de archivos para el empaquetado. Un único *Web. config* transformado se incluye en su lugar.
- El archivo *PostAutoParameterizationWebConfigConnectionStrings. txt* contiene la lista de archivos después de que se hayan parametrizado las cadenas de conexión en el archivo *Web. config* . Este es el proceso que permite reemplazar las cadenas de conexión con la configuración correcta para el entorno de destino al implementar el paquete.
- El archivo *prepackage. txt* contiene la lista de archivos de precompilación finalizada que se incluirá en el paquete.

> [!NOTE]
> Los nombres de los archivos de registro adicionales se corresponden normalmente con destinos WPP. Puede revisar estos destinos examinando el archivo *Microsoft. Web. Publishing. targets* en la carpeta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

Si el contenido del paquete web no es el esperado, la revisión de estos archivos puede ser una forma útil de identificar en qué momento del proceso se produjo un error.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede usar la propiedad **EnablePackageProcessLoggingAndAssert** en MSBuild para solucionar problemas del proceso de empaquetado. Se han explicado las distintas formas en que se puede proporcionar el valor de propiedad al proceso de compilación y se describe la información adicional que se registra cuando se establece la propiedad en **true**.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre WPP y cómo administra el proceso de empaquetado, vea [compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Para obtener instrucciones sobre cómo excluir archivos y carpetas específicos de los paquetes de implementación web, vea [excluir archivos y carpetas de la implementación](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](running-windows-powershell-scripts-from-msbuild-project-files.md)
