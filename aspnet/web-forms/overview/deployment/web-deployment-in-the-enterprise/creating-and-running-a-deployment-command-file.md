---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Crear y ejecutar un archivo de comandos de implementación | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo compilar un archivo de comandos que le permitirá ejecutar una implementación mediante archivos de proyecto de Microsoft Build Engine (MSBuild) como un solo paso,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514981"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Crear y ejecutar un archivo de comandos de implementación

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo crear un archivo de comandos que le permita ejecutar una implementación mediante archivos de proyecto de Microsoft Build Engine (MSBuild) como un proceso repetible de un solo paso.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de&#x2014;ejemplo de la solución [Contact Manager](the-contact-manager-solution.md) para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del proceso de compilación](understanding-the-build-process.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="process-overview"></a>Información general del proceso

En este tema, aprenderá a crear y ejecutar un archivo de comandos que usa estos archivos de proyecto para realizar una implementación repetible en el entorno de destino. Esencialmente, el archivo de comandos simplemente debe contener un comando de MSBuild que:

- Indica a MSBuild que ejecute el archivo *Publish. proj* independiente del entorno.
- Indica al archivo *Publish. proj* qué archivo contiene la configuración de proyecto específica del entorno y dónde se encuentra.

## <a name="create-an-msbuild-command"></a>Crear un comando de MSBuild

Como se describe [en Descripción del proceso de compilación](understanding-the-build-process.md), el archivo&#x2014;de proyecto específico del entorno, por ejemplo, *env-dev. proj*&#x2014;está diseñado para importarse en el archivo *Publish. proj* independiente del entorno en tiempo de compilación. Juntos, estos dos archivos proporcionan un conjunto completo de instrucciones que indican a MSBuild cómo compilar e implementar la solución.

El archivo *Publish. proj* usa un elemento **Import** para importar el archivo de proyecto específico del entorno.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

Como tal, al usar MSBuild. exe para compilar e implementar la solución Contact Manager, necesitará lo siguiente:

- Ejecute MSBuild. exe en el archivo *Publish. proj* .
- Especifique la ubicación del archivo de proyecto específico del entorno proporcionando un parámetro de línea de comandos denominado **TargetEnvPropsFile**.

Para ello, el comando de MSBuild debe ser similar al siguiente:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

Desde aquí, es un paso sencillo para pasar a una implementación repetible y de un solo paso. Lo único que debe hacer es agregar el comando de MSBuild a un archivo. cmd. En la solución Contact Manager, la carpeta Publish incluye un archivo llamado *Publish-dev. cmd* que hace exactamente esto.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> El modificador **/FL** indica a MSBuild que cree un archivo de registro denominado *MSBuild. log* en el directorio de trabajo en el que se invocó MSBuild. exe.

Para implementar o volver a implementar la solución Contact Manager, lo único que debe hacer es ejecutar el archivo *Publish-dev. cmd* . Al ejecutar el archivo, MSBuild hará lo siguiente:

- Compilar todos los proyectos de la solución.
- Generar paquetes web que se pueden implementar para los proyectos de aplicación Web.
- Genere archivos. dbschema y. DeployManifest para los proyectos de base de datos.
- Implemente los paquetes Web en el servidor Web.
- Implemente la base de datos en el servidor de base de datos.

## <a name="run-the-deployment"></a>Ejecutar la implementación

Cuando haya creado un archivo de comandos para el entorno de destino, podrá completar toda la implementación con solo ejecutar el archivo.

**Para implementar la solución Contact Manager en el entorno de prueba**

1. En la estación de trabajo del desarrollador, abra el explorador de Windows y, a continuación, busque la ubicación del archivo *Publish-dev. cmd* .
2. Haga doble clic en el archivo para ejecutarlo.
3. Si aparece un cuadro de diálogo **Abrir archivo: ADVERTENCIA de seguridad** , haga clic en **Ejecutar**.
4. Si las opciones de configuración y los servidores de prueba están configurados correctamente, la ventana del símbolo del sistema mostrará un mensaje **compilar correctamente** cuando MSBuild haya terminado de procesar los archivos del proyecto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Si es la primera vez que ha implementado la solución en este entorno, deberá agregar la cuenta de equipo del servidor Web de prueba a los roles **db\_** **datareader y DB\_DataReader** en la base de datos **ContactManager** . Este procedimiento se describe en [configurar un servidor de base de datos para la publicación de web deploy](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Solo tiene que asignar estos permisos al crear la base de datos. De forma predeterminada, el proceso de compilación no volverá a crear la base de&#x2014;datos en cada implementación, sino que comparará la base de datos existente con el esquema más reciente y hará solo los cambios necesarios. Como resultado, solo necesitará asignar estos roles de base de datos la primera vez que implemente la solución.
6. Abra Internet Explorer y vaya a la dirección URL de la aplicación Contact Manager (por ejemplo, `http://testweb1:85/ContactManager/`).
7. Compruebe que la aplicación funciona según lo previsto y que puede agregar contactos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusión

La creación de un archivo de comandos que contenga las instrucciones de MSBuild le proporciona una forma rápida y sencilla de crear e implementar una solución de varios proyectos en un entorno de destino específico. Si necesita implementar la solución repetidamente en varios entornos de destino, puede crear varios archivos de comandos. En cada archivo de comandos, el comando MSBuild compilará el mismo archivo de proyecto universal, pero especificará un archivo de proyecto específico del entorno diferente. Por ejemplo, un archivo de comandos para publicar en un entorno de desarrollo o de prueba podría contener este comando de MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Un archivo de comandos para publicar en un entorno de ensayo podría contener este comando de MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

También puede personalizar el proceso de compilación para cada entorno invalidando las propiedades o estableciendo otros modificadores en el comando de MSBuild. Para obtener más información, vea referencia de la [línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-projects.md)
> [Siguiente](manually-installing-web-packages.md)
