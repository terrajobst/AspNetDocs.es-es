---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalización de implementaciones de base de datos para varios entornos | Microsoft Docs
author: jrjlee
description: 'En este tema se describe cómo adaptar las propiedades de una base de datos a entornos de destino específicos como parte del proceso de implementación. Nota: el tema supone que...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489037"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Personalizar las implementaciones de la base de datos para varios entornos

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo adaptar las propiedades de una base de datos a entornos de destino específicos como parte del proceso de implementación.
> 
> > [!NOTE]
> > En el tema se supone que está implementando un proyecto de base de datos de Visual Studio 2010 con MSBuild. exe y VSDBCMD. exe. Para obtener más información sobre por qué podría elegir este enfoque, vea [implementación web en los proyectos de](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) [base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md)Enterprise e implementar.
> 
> 
> Al implementar un proyecto de base de datos en varios destinos, a menudo querrá personalizar las propiedades de implementación de la base de datos para cada entorno de destino. Por ejemplo, en entornos de prueba, normalmente se vuelve a crear la base de datos en cada implementación, mientras que en entornos de ensayo o de producción es mucho más probable que se realicen actualizaciones incrementales para conservar los datos.
> 
> En un proyecto de base de datos de Visual Studio 2010, la configuración de implementación se encuentra dentro de un archivo de configuración de implementación (. sqldeployment). En este tema se muestra cómo crear archivos de configuración de implementación específicos del entorno y cómo especificar el que quiere usar como parámetro de VSDBCMD.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En este tema se supone que:

- El enfoque de archivo de proyecto dividido se usa para la implementación de soluciones, como se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llame a VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear un sistema de implementación que admita variables de implementación de base de datos entre entornos de destino, deberá:

- Cree un archivo de configuración de implementación (. sqldeployment) para cada entorno de destino.
- Cree un comando de VSDBCMD que especifique el archivo de configuración de implementación como un modificador de la línea de comandos.
- Parametrizar el comando VSDBCMD en un archivo de proyecto de Microsoft Build Engine (MSBuild), de modo que las opciones de VSDBCMD sean adecuadas para el entorno de destino.

En este tema se muestra cómo realizar cada uno de estos procedimientos.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Crear archivos de configuración de implementación específicos del entorno

De forma predeterminada, un proyecto de base de datos contiene un archivo de configuración de implementación único denominado *Database. sqldeployment*. Si abre este archivo en Visual Studio 2010, puede ver las distintas opciones de implementación disponibles:

- **Intercalación**de la comparación de implementación. Esto le permite elegir si desea usar la intercalación de base de datos del proyecto (la intercalación de *origen* ) o la intercalación de base de datos del servidor de destino (la intercalación de *destino* ). En la mayoría de los casos, querrá usar la intercalación de origen al realizar la implementación en un entorno de desarrollo o de prueba. Al realizar la implementación en un entorno de ensayo o de producción, normalmente querrá dejar la intercalación de destino sin cambios para evitar problemas de interoperabilidad.
- **Implementar propiedades de base de datos**. Esto le permite elegir si desea aplicar las propiedades de la base de datos, tal y como se define en el archivo *Database. sqlsettings* . Cuando implemente una base de datos por primera vez, debe implementar las propiedades de la base de datos. Si está actualizando una base de datos existente, las propiedades ya deben estar en su lugar y no debe volver a implementarlas.
- **Vuelva a crear siempre la base de datos**. Esto le permite elegir si desea volver a crear la base de datos de destino cada vez que implemente o realice cambios incrementales para poner la base de datos de destino al día con el esquema. Si vuelve a crear la base de datos, perderá los datos de la base de datos existente. Por lo tanto, normalmente debe establecer este valor en **false** para las implementaciones en entornos de ensayo o de producción.
- **Bloquear la implementación incremental si puede producirse la pérdida de datos**. Esto le permite elegir si la implementación debe detenerse si un cambio en el esquema de la base de datos provocará la pérdida de datos. Normalmente, se establece en **true** para una implementación en un entorno de producción, para ofrecer la oportunidad de intervenir y proteger los datos importantes. Si ha establecido **volver a crear siempre la base de datos** en **false**, esta configuración no tendrá ningún efecto.
- **Ejecutar la implementación en modo de usuario único**. Esto no suele ser un problema en los entornos de desarrollo o pruebas. Sin embargo, normalmente debería establecerlo en **true** para las implementaciones en entornos de ensayo o de producción. Esto impide que los usuarios realicen cambios en la base de datos mientras se está llevando a cabo la implementación.
- **Copia de seguridad de la base de datos antes de la implementación**. Normalmente, se establece en **true** cuando se implementa en un entorno de producción, como precaución contra la pérdida de datos. También puede establecerlo en **true** al realizar la implementación en un entorno de ensayo, si la base de datos provisional contiene muchos datos.
- **Generar instrucciones Drop para objetos que se encuentran en la base de datos de destino, pero que no están en el proyecto de base de datos**. En la mayoría de los casos, se trata de una parte integral y esencial de realizar cambios incrementales en una base de datos. Si ha establecido **volver a crear siempre la base de datos** en **false**, esta configuración no tendrá ningún efecto.
- **No utilice instrucciones Alter Assembly para actualizar tipos CLR**. Esta configuración determina el modo en que SQL Server debe actualizar los tipos de Common Language Runtime (CLR) a las versiones de ensamblado más recientes. Debe establecerse en **false** en la mayoría de los escenarios.

En esta tabla se muestra la configuración típica de implementación para distintos entornos de destino. Sin embargo, la configuración puede variar en función de los requisitos exactos.

|  | Desarrollador/prueba | Almacenamiento provisional/integración | Producción |
| --- | --- | --- | --- |
| **Intercalación de comparación de implementación** | Origen | Destino | Destino |
| **Implementar propiedades de base de datos** | True | Solo la primera vez | Solo la primera vez |
| **Volver a crear siempre la base de datos** | True | False | False |
| **Bloquear la implementación incremental si se pueden perder datos** | False | Es posible | True |
| **Ejecutar script de implementación en modo de usuario único** | False | True | True |
| **Copia de seguridad de base de datos antes de la implementación** | False | Es posible | True |
| **Generar instrucciones DROP para objetos que se encuentran en la base de datos de destino pero que no están en el proyecto de base de datos** | False | True | True |
| **No usar instrucciones ALTER ASSEMBLy para actualizar tipos CLR** | False | False | False |

> [!NOTE]
> Para obtener más información sobre las propiedades de implementación de bases de datos y consideraciones de entorno, vea [información general sobre la configuración del proyecto de base de](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)datos, [Cómo: configurar propiedades para detalles de implementación](https://msdn.microsoft.com/library/dd172125.aspx), [generar e implementar una base de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/library/dd193409.aspx)y [compilar e implementar bases de datos en un entorno de ensayo o de producción](https://msdn.microsoft.com/library/dd193413.aspx).

Para admitir la implementación de un proyecto de base de datos en varios destinos, debe crear un archivo de configuración de implementación para cada entorno de destino.

**Para crear un archivo de configuración específico del entorno**

1. En Visual Studio 2010, en la ventana de **Explorador de soluciones** , haga clic con el botón secundario en el proyecto de base de datos y, a continuación, haga clic en **propiedades**.
2. En la página Propiedades del proyecto de base de datos, en la pestaña **implementar** , en la fila **archivo de configuración de implementación** , haga clic en **nuevo**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. En el cuadro de diálogo **nuevo archivo de configuración de implementación** , asigne al archivo un nombre descriptivo (por ejemplo, **TestEnvironment. sqldeployment**) y, a continuación, haga clic en **Guardar**.
4. En la página *[FILENAME] * *. sqldeployment**, establezca las propiedades de implementación para que coincidan con los requisitos de su entorno de destino y, a continuación, guarde el archivo.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Tenga en cuenta que el nuevo archivo se agrega a la carpeta Propiedades del proyecto de base de datos.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Especificar el archivo de configuración de implementación en VSDBCMD

Al usar configuraciones de soluciones (como depuración y lanzamiento) en Visual Studio 2010, puede asociar un archivo de configuración de implementación con cada configuración. Al compilar una configuración determinada, el proceso de compilación genera un archivo de manifiesto de implementación específico de la configuración que apunta al archivo de configuración de implementación específico de la configuración. Sin embargo, uno de los principales objetivos del enfoque de implementación que se describe en estos tutoriales es proporcionar a los usuarios la capacidad de controlar el proceso de implementación sin usar Visual Studio 2010 y configuraciones de soluciones. En este enfoque, la configuración de la solución es la misma independientemente del entorno de implementación de destino. Para personalizar la implementación de la base de datos en un entorno de destino específico, puede usar las opciones de línea de comandos de VSDBCMD para especificar el archivo de configuración de implementación.

Para especificar un archivo de configuración de implementación en VSDBCMD, use el modificador **p:/DeploymentConfigurationFile** y proporcione la ruta de acceso completa al archivo. Esto invalidará el archivo de configuración de implementación que identifica el manifiesto de implementación. Por ejemplo, podría usar este comando de VSDBCMD para implementar la base de datos **ContactManager** en un entorno de prueba:

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Tenga en cuenta que el proceso de compilación puede cambiar el nombre del archivo. sqldeployment cuando copia el archivo en el directorio de salida.

Si usa variables de comandos SQL en los scripts SQL anteriores o posteriores a la implementación, puede usar un enfoque similar para asociar un archivo. sqlcmdvars específico del entorno a la implementación. En este caso, se usa el modificador **p:/SqlCommandVariablesFile** para identificar el archivo. sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Ejecutar el comando VSDBCMD desde un archivo de proyecto de MSBuild

Puede invocar un comando de VSDBCMD desde un archivo de proyecto de MSBuild mediante una tarea **exec** dentro de un destino de MSBuild. En su forma más sencilla, tendría el siguiente aspecto:

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- En la práctica, para facilitar la lectura y la reutilización de los archivos de proyecto, querrá crear propiedades para almacenar los distintos parámetros de la línea de comandos. Esto facilita a los usuarios proporcionar valores de propiedad en un archivo de proyecto específico del entorno o reemplazar los valores predeterminados de la línea de comandos de MSBuild. Si usa el enfoque de archivo de proyecto dividido descrito en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), debe dividir las instrucciones de compilación y las propiedades entre los dos archivos según corresponda:
- La configuración específica del entorno, como el nombre de archivo de configuración de implementación, la cadena de conexión de base de datos y el nombre de la base de datos de destino, debe ir en el archivo de proyecto específico del entorno.
- El destino de MSBuild que ejecuta el comando VSDBCMD, junto con cualquier propiedad universal como la ubicación del ejecutable de VSDBCMD, debe ir en el archivo de proyecto universal.

También debe asegurarse de que compila el proyecto de base de datos antes de invocar a VSDBCMD para que se cree el archivo. DeployManifest y esté listo para usarse. Puede ver un ejemplo completo de este enfoque en el tema [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), que le guía por los archivos de proyecto de la [solución de ejemplo Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo se pueden adaptar las propiedades de base de datos a diferentes entornos de destino cuando se implementan proyectos de base de datos con MSBuild y VSDBCMD. Este enfoque es útil cuando se necesita implementar proyectos de base de datos como parte de soluciones de escala empresarial más grandes. Estas soluciones se suelen implementar en varios destinos, como entornos de desarrollo o pruebas en espacio aislado, plataformas de ensayo o integración, y entornos de producción o en vivo. Cada uno de estos entornos de destino normalmente requiere un conjunto único de propiedades de implementación de base de datos.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre la implementación de proyectos de base de datos mediante VSDBCMD. exe, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

En estos artículos de MSDN se proporcionan más instrucciones generales sobre la implementación de bases de datos:

- [Información general sobre la configuración del proyecto de base de datos](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Cómo: configurar propiedades para los detalles de la implementación](https://msdn.microsoft.com/library/dd172125.aspx)
- [Crear e implementar bases de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/library/dd193409.aspx)
- [Crear e implementar bases de datos en un entorno de ensayo o de producción](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Anterior](performing-a-what-if-deployment.md)
> [Siguiente](deploying-database-role-memberships-to-test-environments.md)
