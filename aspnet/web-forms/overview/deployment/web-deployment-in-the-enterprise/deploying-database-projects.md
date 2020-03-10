---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Implementar proyectos de base de datos | Microsoft Docs
author: jrjlee
description: 'Nota: en muchos escenarios de implementación empresarial, necesita la capacidad de publicar actualizaciones incrementales en una base de datos implementada. La alternativa consiste en volver a crear...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514945"
---
# <a name="deploying-database-projects"></a>Implementar proyectos de base de datos

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> En muchos escenarios de implementación empresarial, se necesita la capacidad de publicar actualizaciones incrementales en una base de datos implementada. La alternativa consiste en volver a crear la base de datos en cada implementación, lo que significa que se pierden todos los datos de la base de datos existente. Cuando se trabaja con Visual Studio 2010, el uso de VSDBCMD es el enfoque recomendado para la publicación incremental de bases de datos. Sin embargo, la siguiente versión de Visual Studio y la canalización de publicación web (WPP) incluirán herramientas que admiten la publicación incremental directamente.

Si abre la solución de ejemplo Contact Manager en Visual Studio 2010, verá que el proyecto de base de datos incluye una carpeta propiedades que contiene cuatro archivos.

![](deploying-database-projects/_static/image1.png)

Junto con el archivo de proyecto (*ContactManager. Database. dbproj* en este caso), estos archivos controlan distintos aspectos del proceso de compilación e implementación:

- El archivo *Database. sqlcmdvars* proporciona valores para las variables SQLCMD que se usan al implementar el proyecto. Cada configuración de soluciones (por ejemplo, depuración y lanzamiento) puede especificar un archivo. sqlcmdvars diferente.
- El archivo *Database. sqldeployment* proporciona la configuración específica de la implementación, como si se va a usar la intercalación definida en el proyecto o la intercalación del servidor de destino, si se debe volver a crear la base de datos de destino cada vez o simplemente se debe modificar la base de datos existente para actualizarla y así sucesivamente. Cada configuración de soluciones puede especificar un archivo. sqldeployment diferente.
- El archivo *Database. sqlpermissions* es un documento XML que puede utilizar para definir los permisos que desee agregar a la base de datos de destino. Todas las configuraciones de soluciones comparten el mismo archivo. sqlpermissions.
- El archivo *Database. sqlsettings* especifica las propiedades de nivel de base de datos que se van a utilizar al crear la base de datos, como la intercalación que se va a usar, el comportamiento de los operadores de comparación, etc. Todas las configuraciones de soluciones comparten el mismo archivo. sqlsettings.

Merece la pena dedicar un momento a abrir estos archivos en Visual Studio y familiarizarse con el contenido.

Al compilar un proyecto de base de datos, el proceso de compilación crea dos archivos:

- Un *esquema* de la base de datos (archivo. dbschema). Esto describe el esquema de la base de datos que desea crear en formato XML.
- Un *manifiesto de implementación* (archivo. DeployManifest). Contiene toda la información necesaria para crear e implementar la base de datos. Hace referencia al archivo. dbschema junto con otros recursos, como las instrucciones de implementación (el archivo. sqldeployment) y cualquier script SQL anterior a la implementación o posterior a la implementación.

Esto muestra la relación entre estos recursos:

![](deploying-database-projects/_static/image2.png)

Como puede ver, el archivo. sqlsettings y el archivo. sqlpermissions son entradas para el proceso de compilación. Junto con el archivo de proyecto de base de datos, estos archivos se usan para crear el archivo de esquema de la base de datos. El archivo. sqldeployment y el archivo. sqlcmdvars pasan por el proceso de compilación sin cambios. El manifiesto de implementación indica la ubicación del esquema de la base de datos, el archivo. sqldeployment, el archivo. sqlcmdvars y cualquier script SQL anterior a la implementación o posterior a la implementación.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>¿Por qué usar VSDBCMD para implementar un proyecto de base de datos?

Hay varios enfoques diferentes para implementar proyectos de base de datos. Sin embargo, no todas ellas son adecuadas para implementar un proyecto de base de datos en servidores remotos en un entorno empresarial. Considere lo que desea en la implementación de un proyecto de base de datos. En escenarios de implementación empresarial, es probable que desee:

- La capacidad de implementar el proyecto de base de datos desde una ubicación remota.
- La capacidad de hacer actualizaciones incrementales en una base de datos existente.
- La capacidad de incluir scripts anteriores o posteriores a la implementación.
- La capacidad de personalizar la implementación en varios entornos de destino.
- La capacidad de implementar el proyecto de base de datos como parte de una implementación de soluciones de un solo paso más grande, con scripts.

Existen tres enfoques principales que puede usar para implementar un proyecto de base de datos:

- Puede usar la funcionalidad de implementación con el tipo de proyecto de base de datos en Visual Studio 2010. Al compilar e implementar un proyecto de base de datos en Visual Studio 2010, el proceso de implementación utiliza el manifiesto de implementación para generar un archivo de implementación basado en SQL específico para la configuración de compilación. Esto creará la base de datos si aún no existe o realizará los cambios necesarios en la base de datos si ya existe. Puede usar SQLCMD. exe para ejecutar este archivo en el servidor de destino o puede establecer Visual Studio para crear y ejecutar el archivo. El inconveniente de este enfoque es que solo tiene un control limitado sobre la configuración de implementación. A menudo, es posible que también necesite modificar el archivo de implementación de SQL para proporcionar valores de variable específicos del entorno. Este enfoque solo se puede usar desde un equipo que tenga instalado Visual Studio 2010 y el desarrollador deberá conocer y proporcionar las cadenas de conexión y las credenciales para todos los entornos de destino.
- Puede usar la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para [implementar una base de datos como parte de un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343.aspx). Sin embargo, este enfoque es mucho más complejo si desea implementar un proyecto de base de datos en lugar de replicar simplemente una base de datos local existente en un servidor de destino. Puede configurar Web Deploy para ejecutar el script de implementación de SQL que genera el proyecto de base de datos, pero para ello debe crear un archivo de destinos WPP personalizado para el proyecto de aplicación Web. Esto agrega una cantidad considerable de complejidad al proceso de implementación. Además, Web Deploy no admite directamente las actualizaciones incrementales de las bases de datos existentes. Para obtener más información sobre este enfoque, vea [extender la canalización de publicación web al proyecto de base de datos de paquete implementado de SQL](https://go.microsoft.com/?linkid=9805121).
- Puede usar la utilidad VSDBCMD para implementar la base de datos, mediante el esquema de la base de datos o el manifiesto de implementación. Puede llamar a VSDBCMD. exe desde un destino de MSBuild, que le permite publicar bases de datos como parte de un proceso de implementación de script mayor. Puede invalidar las variables en el archivo. sqlcmdvars y muchas otras propiedades de la base de datos desde un comando de VSDBCMD, que le permite personalizar la implementación para diferentes entornos sin crear varias configuraciones de compilación. VSDBCMD proporciona funcionalidad de diferenciación, lo que significa que solo realizará los cambios necesarios para alinear una base de datos de destino con el esquema de la base de datos. VSDBCMD también ofrece una amplia gama de opciones de línea de comandos, que proporcionan un control exhaustivo sobre el proceso de implementación.

En esta información general, puede ver que el uso de VSDBCMD con MSBuild es el método más adecuado para un escenario de implementación empresarial típico:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| ¿Admite la implementación remota? | Sí | Sí | Sí |
| ¿Admite actualizaciones incrementales? | Sí | No | Sí |
| ¿Admite scripts anteriores o posteriores a la implementación? | Sí | Sí | Sí |
| ¿Admite la implementación en varios entornos? | Limitado | Limitado | Sí |
| ¿Admite la implementación con scripts? | Limitado | Sí | Sí |

En el resto de este tema se describe el uso de VSDBCMD con MSBuild para implementar proyectos de base de datos.

## <a name="understanding-the-deployment-process"></a>Descripción del proceso de implementación

La utilidad VSDBCMD permite implementar una base de datos mediante el esquema de la base de datos (el archivo. dbschema) o el manifiesto de implementación (el archivo. DeployManifest). En la práctica, casi siempre usará el manifiesto de implementación, ya que el manifiesto de implementación le permite proporcionar valores predeterminados para varias propiedades de implementación e identificar cualquier script SQL anterior o posterior a la implementación que desee ejecutar. Por ejemplo, este comando de VSDBCMD se usa para implementar la base de datos **ContactManager** en un servidor de base de datos en un entorno de prueba:

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

En este caso:

- El modificador **/a** (o **/Action**) especifica lo que desea que realice VSDBCMD. Puede establecer esta configuración en **importar** o **implementar**. La opción de **importación** se usa para generar un archivo. dbschema a partir de una base de datos existente y la opción **implementar** se usa para implementar un archivo. dbschema en una base de datos de destino.
- El modificador **/manifest** (o **/ManifestFile**) identifica el archivo. DeployManifest que desea implementar. Si desea utilizar el archivo. dbschema en su lugar, debe usar el modificador **/Model** (o **/ModelFile**).
- El modificador **/CS** (o **/ConnectionString**) proporciona la cadena de conexión para el servidor de base de datos de destino. Tenga en cuenta que esto no incluye el nombre de&#x2014;la base de datos que VSDBCMD debe conectarse al servidor para crear la base de datos. no es necesario conectarse a una base de datos individual. Si el archivo. DeployManifest incluye una cadena de conexión, puede omitir este modificador. Si usa el modificador de todos modos, el valor del modificador invalidará el valor de. DeployManifest.
- La propiedad <strong>/p: TargetDatabase</strong> proporciona el nombre que desea asignar a la base de datos de destino durante la creación. Esto invalida el valor de la propiedad <strong>TargetDatabase</strong> en el archivo. DeployManifest. Puede usar la sintaxis <strong>/p:</strong> <em>[nombre de propiedad]</em>para establecer una gran variedad de propiedades de implementación e INvalidar las variables SQLCMD declaradas en el archivo. sqlcmdvars.
- El modificador de **/DD +** (o **/DeployToDatabase +** ) indica que desea crear una implementación e implementarla en el entorno de destino. Si especifica **/DD-** u omitir el modificador, VSDBCMD generará un script de implementación, pero no lo implementará en el entorno de destino. Este modificador suele ser el origen de confusión y se explica con más detalle en la sección siguiente.
- El modificador **/script** (o **/DeploymentScriptFile**) especifica dónde desea generar el script de implementación. Este valor no afecta al proceso de implementación.

Para obtener más información sobre VSDBCMD, vea [referencia de la línea de comandos para VSDBCMD. EXE (implementación y importación de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obtener un ejemplo de cómo se puede usar VSDBCMD desde un archivo de proyecto de MSBuild, vea [Descripción del proceso de compilación](understanding-the-build-process.md). Para ver ejemplos de cómo configurar las opciones de implementación de bases de datos para varios entornos, consulte [Personalización de implementaciones de bases de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Descripción del modificador DeployToDatabase

El comportamiento del modificador **/DD** o **/DeployToDatabase** depende de si está usando VSDBCMD con un archivo. dbschema o un archivo. DeployManifest. Si utiliza un archivo. dbschema, el comportamiento es bastante sencillo:

- Si especifica **/DD +** o **/DD**, VSDBCMD generará un script de implementación e implementará la base de datos.
- Si especifica **/DD-** u omite el modificador, VSDBCMD solo generará un script de implementación.

Si utiliza un archivo. DeployManifest, el comportamiento es mucho más complicado. Esto se debe a que el archivo. DeployManifest contiene un nombre de propiedad **DeployToDatabase** que también determina si la base de datos está implementada.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

El valor de esta propiedad se establece de acuerdo con las propiedades del proyecto de base de datos. Si establece la **acción** de implementación para **crear un script de implementación (. SQL)** , el valor será **false**. Si establece la **acción implementar** para **crear un script de implementación (. SQL) e implementarlo en la base de datos**, el valor será **true**.

> [!NOTE]
> Estos valores están asociados a una configuración y plataforma de compilación específica. Por ejemplo, si configura los valores para la configuración de **depuración** y después publica con la configuración de **lanzamiento** , la configuración no se usará.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> En este escenario, la **acción de implementación** siempre debe establecerse para **crear un script de implementación (. SQL)** , ya que no desea que Visual Studio 2010 implemente la base de datos. En otras palabras, la propiedad **DeployToDatabase** siempre debe ser **false**.

Cuando se especifica una propiedad **DeployToDatabase** , el modificador **/DD** solo invalidará la propiedad si el valor de la propiedad es **false**:

- Si la propiedad **DeployToDatabase** es **false**y especifica **/DD +** o **/DD**, VSDBCMD invalidará la propiedad **DeployToDatabase** e implementará la base de datos.
- Si la propiedad **DeployToDatabase** es **false**y especifica **/DD-** u omitir el modificador, VSDBCMD no implementará la base de datos.
- Si la propiedad **DeployToDatabase** es **true**, VSDBCMD omitirá el modificador e implementará la base de datos.
- En cada caso, se genera un script de implementación, independientemente de si está implementando la base de datos también.

## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general sobre el proceso de compilación e implementación de proyectos de base de datos en Visual Studio 2010. También se describe cómo puede usar VSDBCMD. exe con MSBuild para admitir la implementación de bases de datos a escala empresarial.

Para obtener más información sobre cómo funciona esto en la práctica, vea [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo personalizar las implementaciones de bases de datos mediante la creación de un archivo de configuración de implementación independiente para cada entorno, consulte [Personalización de implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obtener instrucciones sobre cómo configurar la pertenencia a roles de base de datos mediante la ejecución de un script posterior a la implementación, vea [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obtener instrucciones sobre la administración de algunos de los desafíos únicos que imponen las bases de datos de pertenencia, consulte [implementación de bases de datos de pertenencia en entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

En estos temas de MSDN se proporcionan más instrucciones e información general sobre los proyectos de base de datos de Visual Studio y el proceso de implementación de bases de datos:

- [Proyectos de base de datos de SQL Server de Visual Studio 2010](https://msdn.microsoft.com/library/ff678491.aspx)
- [Administrar cambios de base de datos](https://msdn.microsoft.com/library/aa833404.aspx)
- [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EJECUTABLE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Información general sobre el Compilación e implementación de base de datos](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-web-packages.md)
> [Siguiente](creating-and-running-a-deployment-command-file.md)
