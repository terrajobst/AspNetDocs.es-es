---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Descripción del proceso de compilación | Microsoft Docs
author: jrjlee
description: En este tema se proporciona un tutorial sobre un proceso de compilación e implementación a escala empresarial. El enfoque que se describe en este tema usa la compilación personalizada de Microsoft Engin...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520837"
---
# <a name="understanding-the-build-process"></a>Descripción del proceso de compilación

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se proporciona un tutorial sobre un proceso de compilación e implementación a escala empresarial. El enfoque que se describe en este tema usa archivos de proyecto de Microsoft Build Engine personalizado (MSBuild) para proporcionar un control exhaustivo sobre cada aspecto del proceso. Dentro de los archivos de proyecto, los destinos de MSBuild personalizados se utilizan para ejecutar utilidades de implementación como la herramienta de implementación web de Internet Information Services (IIS) (MSDeploy. exe) y la utilidad de implementación de base de datos VSDBCMD. exe.
> 
> > [!NOTE]
> > En el tema anterior, [Descripción del archivo de proyecto](understanding-the-project-file.md), se describen los componentes clave de un archivo de proyecto de MSBuild y se presentó el concepto de archivos de proyecto divididos para admitir la implementación en varios entornos de destino. Si no está familiarizado con estos conceptos, debe revisar [la descripción del archivo de proyecto antes de](understanding-the-project-file.md) trabajar en este tema.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="build-and-deployment-overview"></a>Información general de Compilación e implementación

En la [solución Contact Manager](the-contact-manager-solution.md), tres archivos controlan el proceso de compilación e implementación:

- Un *archivo de proyecto universal* (*Publish. proj*). Contiene instrucciones de compilación e implementación que no cambian entre los entornos de destino.
- Un *archivo de proyecto específico del entorno* (*env-dev. proj*). Contiene la configuración de compilación e implementación específica de un entorno de destino determinado. Por ejemplo, puede usar el archivo *env-dev. proj* para proporcionar la configuración de un entorno de desarrollo o de prueba y crear un archivo alternativo denominado *env-Stage. proj* para proporcionar la configuración de un entorno de ensayo.
- Un *archivo de comandos* (*Publish-dev. cmd*). Contiene un comando MSBuild. exe que especifica los archivos de proyecto que desea ejecutar. Puede crear un archivo de comandos para cada entorno de destino, donde cada archivo contiene un comando MSBuild. exe que especifica un archivo de proyecto específico del entorno diferente. Esto permite que el desarrollador se implemente en entornos diferentes con solo ejecutar el archivo de comandos adecuado.

En la solución de ejemplo, puede encontrar estos tres archivos en la carpeta de la solución de publicación.

![](understanding-the-build-process/_static/image1.png)

Antes de examinar estos archivos con más detalle, echemos un vistazo a cómo funciona el proceso de compilación general cuando se usa este enfoque. En un nivel alto, el proceso de compilación e implementación tiene el siguiente aspecto:

![](understanding-the-build-process/_static/image2.png)

Lo primero que ocurre es que los dos archivos&#x2014;de proyecto que contiene instrucciones de compilación e implementación universales y otro que contiene la configuración&#x2014;específica del entorno se combinan en un único archivo de proyecto. A continuación, MSBuild funciona a través de las instrucciones del archivo de proyecto. Genera cada uno de los proyectos de la solución mediante el archivo de proyecto para cada proyecto. A continuación, llama a otras herramientas, como Web Deploy (MSDeploy. exe) y la utilidad VSDBCMD para implementar el contenido web y las bases de datos en el entorno de destino.

De principio a fin, el proceso de compilación e implementación realiza estas tareas:

1. Elimina el contenido del directorio de salida, como preparación para una compilación nueva.
2. Compila cada proyecto en la solución:

    1. En los proyectos&#x2014;Web, en este caso, una aplicación web MVC de ASP.net y un&#x2014;servicio Web WCF el proceso de compilación crea un paquete de implementación web para cada proyecto.
    2. En los proyectos de base de datos, el proceso de compilación crea un manifiesto de implementación (archivo. DeployManifest) para cada proyecto.
3. Usa la utilidad VSDBCMD. exe para implementar cada proyecto de base de datos en la solución, utilizando varias propiedades de los&#x2014;archivos de proyecto una cadena de conexión de&#x2014;destino y un nombre de base de datos junto con el archivo. DeployManifest.
4. Usa la utilidad MSDeploy. exe para implementar cada proyecto web en la solución, utilizando varias propiedades de los archivos de proyecto para controlar el proceso de implementación.

Puede usar la solución de ejemplo para realizar un seguimiento de este proceso con más detalle.

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Invocar el proceso de Compilación e implementación

Para implementar la solución Contact Manager en un entorno de prueba de desarrollador, el desarrollador ejecuta el archivo de comandos *Publish-dev. cmd* . Esto invoca MSBuild. exe, especificando *Publish. proj* como el archivo de proyecto que se va a ejecutar y *env-dev. proj* como valor de parámetro.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> El modificador **/FL** (Short para **/fileLogger**) registra la salida de la compilación en un archivo denominado *MSBuild. log* en el directorio actual. Para obtener más información, vea la referencia de la [línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

En este punto, MSBuild comienza a ejecutarse, carga el archivo *Publish. proj* y comienza el procesamiento de las instrucciones que contiene. La primera instrucción indica a MSBuild que importe el archivo de proyecto que especifica el parámetro **TargetEnvPropsFile** .

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

El parámetro **TargetEnvPropsFile** especifica el archivo *env-dev. proj* , de modo que MSBuild combina el contenido del archivo *env-dev. proj* en el archivo *Publish. proj* .

Los elementos siguientes que MSBuild encuentra en el archivo de proyecto combinado son grupos de propiedades. Las propiedades se procesan en el orden en que aparecen en el archivo. MSBuild crea un par clave-valor para cada propiedad, siempre que se cumplan las condiciones especificadas. Las propiedades que se definen más adelante en el archivo sobrescribirán cualquier propiedad con el mismo nombre definido anteriormente en el archivo. Por ejemplo, considere las propiedades de **OutputRoot** .

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Cuando MSBuild procesa el primer elemento **OutputRoot** , no se proporciona un parámetro con el mismo nombre, sino que establece el valor de la propiedad **OutputRoot** en **. \Publish\Out**. Cuando encuentra el segundo elemento **OutputRoot** , si la condición se evalúa como **true**, sobrescribirá el valor de la propiedad **OutputRoot** con el valor del parámetro **outdir** .

El siguiente elemento que MSBuild encuentra es un grupo de elementos único, que contiene un elemento denominado **ProjectsToBuild**.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

MSBuild procesa esta instrucción mediante la creación de una lista de elementos denominada **ProjectsToBuild**. En este caso, la lista de elementos contiene un valor&#x2014;único de la ruta de acceso y el nombre del archivo de solución.

En este punto, los elementos restantes son destinos. Los destinos se procesan de forma diferente&#x2014;a las propiedades y los elementos básicamente, los destinos no se procesan a menos que el usuario los especifique explícitamente o que otro constructor lo invoque en el archivo de proyecto. Recuerde que la etiqueta de apertura del **proyecto** incluye un atributo **DefaultTargets** .

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Esto indica a MSBuild que invoque el destino **FullPublish** si los destinos no se especifican cuando se invoca MSBuild. exe. El destino **FullPublish** no contiene ninguna tarea; en su lugar, simplemente especifica una lista de dependencias.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Esta dependencia indica a MSBuild que, para ejecutar el destino **FullPublish** , debe invocar esta lista de destinos en el orden proporcionado:

1. Debe invocar el destino **Clean** .
2. Debe invocar el destino **BuildProjects** .
3. Debe invocar el destino **GatherPackagesForPublishing** .
4. Debe invocar el destino **PublishDbPackages** .
5. Debe invocar el destino **PublishWebPackages** .

### <a name="the-clean-target"></a>El destino Clean

Básicamente, el destino **Clean** elimina el directorio de salida y todo su contenido, como preparación para una compilación nueva.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Observe que el destino incluye un elemento **ItemGroup** . Al definir las propiedades o los elementos dentro de un elemento de **destino** , se crean propiedades y elementos *dinámicos* . En otras palabras, las propiedades o los elementos no se procesan hasta que se ejecuta el destino. Es posible que el directorio de salida no exista o que contenga archivos hasta que comience el proceso de compilación, por lo que no puede compilar la **\_lista FilesToDelete** como un elemento estático; tiene que esperar hasta que se esté realizando la ejecución. Como tal, se genera la lista como un elemento dinámico dentro del destino.

> [!NOTE]
> En este caso, dado que el destino **Clean** es el primero que se ejecuta, no hay ninguna necesidad real de usar un grupo de elementos dinámicos. Sin embargo, es recomendable usar las propiedades y los elementos dinámicos en este tipo de escenario, ya que es posible que desee ejecutar destinos en un orden diferente en algún momento.  
> También debe intentar evitar declarar elementos que nunca se usarán. Si tiene elementos que solo usará un destino específico, considere la posibilidad de colocarlos dentro del destino para quitar cualquier sobrecarga innecesaria en el proceso de compilación.

Aparte de los elementos dinámicos, el destino **limpio** es bastante sencillo y hace uso de las tareas integradas **Message**, **Delete**y **RemoveDir** para:

1. Envíe un mensaje al registrador.
2. Cree una lista de los archivos que se van a eliminar.
3. Elimine los archivos.
4. Quite el directorio de salida.

### <a name="the-buildprojects-target"></a>Destino de BuildProjects

Básicamente, el destino **BuildProjects** compila todos los proyectos de la solución de ejemplo.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Este destino se ha descrito en algunos detalles del tema anterior, [Descripción del archivo del proyecto](understanding-the-project-file.md), para ilustrar cómo las tareas y los destinos hacen referencia a las propiedades y los elementos. Llegados a este punto, está especialmente interesado en la tarea **msbuild** . Puede usar esta tarea para compilar varios proyectos. La tarea no crea una nueva instancia de MSBuild. exe; usa la instancia actual en ejecución para compilar cada proyecto. Los puntos clave de interés en este ejemplo son las propiedades de implementación:

- La propiedad **DeployOnBuild** indica a MSBuild que ejecute cualquier instrucción de implementación en la configuración del proyecto cuando se complete la compilación de cada proyecto.
- La propiedad **DeployTarget** identifica el destino que se va a invocar una vez compilado el proyecto. En este caso, el destino del **paquete** genera el resultado del proyecto en un paquete web que se pueden implementar.

> [!NOTE]
> El destino del **paquete** invoca la canalización de publicación web (WPP), que proporciona la integración entre MSBuild y Web deploy. Si desea echar un vistazo a los destinos integrados que proporciona WPP, revise el archivo *Microsoft. Web. Publishing. targets* en la carpeta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

### <a name="the-gatherpackagesforpublishing-target"></a>Destino de GatherPackagesForPublishing

Si estudia el destino **GatherPackagesForPublishing** , observará que en realidad no contiene ninguna tarea. En su lugar, contiene un único grupo de elementos que define tres elementos dinámicos.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Estos elementos hacen referencia a los paquetes de implementación que se crearon cuando se ejecutó el destino **BuildProjects** . Estos elementos no se pudieron definir de forma estática en el archivo de proyecto, ya que los archivos a los que se hace referencia no existen hasta que se ejecuta el destino **BuildProjects** . En su lugar, los elementos se deben definir dinámicamente dentro de un destino que no se invoca hasta después de que se ejecute el destino **BuildProjects** .

Los elementos no se usan en este destino&#x2014;este destino simplemente compila los elementos y los metadatos asociados a cada valor de elemento. Una vez procesados estos elementos, el elemento **PublishPackages** contendrá dos valores: la ruta de acceso al archivo *ContactManager. Mvc. deploy. cmd* y la ruta de acceso al archivo *ContactManager. Service. deploy. cmd* . Web Deploy crea estos archivos como parte del paquete web para cada proyecto y estos son los archivos que se deben invocar en el servidor de destino para implementar los paquetes. Si abre uno de estos archivos, básicamente verá un comando MSDeploy. exe con varios valores de parámetro específicos de la compilación.

El elemento **DbPublishPackages** contendrá un valor único, la ruta de acceso al archivo *ContactManager. Database. DeployManifest* .

> [!NOTE]
> Un archivo. DeployManifest se genera al compilar un proyecto de base de datos y usa el mismo esquema que un archivo de proyecto de MSBuild. Contiene toda la información necesaria para implementar una base de datos, incluida la ubicación del esquema de la base de datos (. dbschema) y los detalles de los scripts anteriores y posteriores a la implementación. Para obtener más información, vea información [General sobre el compilación e implementación de base de datos](https://msdn.microsoft.com/library/aa833165.aspx).

Aprenderá más sobre cómo se crean y se usan los paquetes de implementación y los manifiestos de implementación de base de datos en la [compilación y el empaquetado de proyectos de aplicación web](building-and-packaging-web-application-projects.md) y la [implementación de proyectos de base de datos](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Destino de PublishDbPackages

Brevemente, el destino **PublishDbPackages** invoca la utilidad VSDBCMD para implementar la base de datos **ContactManager** en un entorno de destino. La configuración de la implementación de bases de datos implica una gran cantidad de decisiones y matices, y obtendrá más información al respecto en [implementación de proyectos de base de datos](deploying-database-projects.md) y [Personalización de implementaciones de bases de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). En este tema, nos centraremos en cómo funciona realmente este destino.

En primer lugar, observe que la etiqueta de apertura incluye un atributo **Outputs** .

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Este es un ejemplo de *procesamiento por lotes de destino*. En los archivos de proyecto de MSBuild, el procesamiento por lotes es una técnica para iterar colecciones. El valor del atributo **Outputs** , **"% (DbPublishPackages. Identity)"** , hace referencia a la propiedad **Identity** metadata de la lista de elementos **DbPublishPackages** . Esta notación, **OUTPUTS =% * * * (ItemList. ItemMetadataName)* , se traduce como:

- Divida los elementos de **DbPublishPackages** en lotes de elementos que contengan el mismo valor de metadatos de **identidad** .
- Ejecute el destino una vez por lote.

> [!NOTE]
> **Identity** es uno de los [valores de metadatos integrados](https://msdn.microsoft.com/library/ms164313.aspx) que se asigna a cada elemento en la creación. Hace referencia al valor del atributo **include** en el elemento&#x2014; **Item** en otras palabras, la ruta de acceso y el nombre de archivo del elemento.

En este caso, dado que nunca debe haber más de un elemento con la misma ruta de acceso y el mismo nombre de archivo, básicamente estamos trabajando con tamaños de lote de uno. El destino se ejecuta una vez para cada paquete de base de datos.

Puede ver una notación similar en la **\_propiedad cmd** , que genera un comando VSDBCMD con los modificadores adecuados.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

En este caso, **% (DbPublishPackages. DatabaseConnectionString)** , **% (DbPublishPackages. TargetDatabase)** y **% (DbPublishPackages. FullPath)** hacen referencia a los valores de los metadatos de la colección de elementos **DbPublishPackages** . La tarea **exec** , que invoca el comando, usa la propiedad **\_cmd** .

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

Como resultado de esta notación, la tarea **exec** creará lotes basados en combinaciones únicas de los valores de metadatos **DatabaseConnectionString**, **TargetDatabase**y **FullPath** , y la tarea se ejecutará una vez para cada lote. Este es un ejemplo de *procesamiento por lotes de tareas*. Sin embargo, dado que el procesamiento por lotes de nivel de destino ya ha dividido la colección de elementos en lotes de un solo elemento, la tarea **exec** se ejecutará una vez y solo una vez para cada iteración del destino. En otras palabras, esta tarea invoca la utilidad VSDBCMD una vez para cada paquete de base de datos de la solución.

> [!NOTE]
> Para obtener más información sobre el procesamiento por lotes de destino y tareas, vea [procesamiento por lotes](https://msdn.microsoft.com/library/ms171473.aspx)de MSBuild, [metadatos de elementos en procesamiento por lotes de destino](https://msdn.microsoft.com/library/ms228229.aspx)y [metadatos de elementos en el procesamiento por lotes de tareas](https://msdn.microsoft.com/library/ms171474.aspx).

### <a name="the-publishwebpackages-target"></a>Destino de PublishWebPackages

En este punto, se ha invocado el destino **BuildProjects** , que genera un paquete de implementación web para cada proyecto de la solución de ejemplo. Acompañar cada paquete es un archivo *deploy. cmd* , que contiene los comandos MSDeploy. exe necesarios para implementar el paquete en el entorno de destino y un archivo *SetParameters. XML* , que especifica los detalles necesarios del entorno de destino. También ha invocado el destino **GatherPackagesForPublishing** , que genera una colección de elementos que contiene los archivos *deploy. cmd* que le interesan. En esencia, el destino **PublishWebPackages** realiza estas funciones:

- Manipula el archivo *SetParameters. XML* de cada paquete para incluir los detalles correctos para el entorno de destino, mediante la tarea **xmlpoke (** .
- Invoca el archivo *deploy. cmd* para cada paquete, con los modificadores adecuados.

Al igual que el destino **PublishDbPackages** , el destino **PublishWebPackages** usa el procesamiento por lotes de destino para asegurarse de que el destino se ejecuta una vez para cada paquete Web.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

Dentro del destino, se usa la tarea **exec** para ejecutar el archivo *deploy. cmd* para cada paquete Web.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Para obtener más información sobre la configuración de la implementación de paquetes Web, vea [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona un tutorial sobre cómo se usan los archivos de proyecto divididos para controlar el proceso de compilación e implementación de principio a fin en la solución de ejemplo Contact Manager. El uso de este enfoque le permite ejecutar implementaciones complejas de escala empresarial en un único paso repetible, simplemente ejecutando un archivo de comandos específico del entorno.

## <a name="further-reading"></a>Información adicional

Para obtener una introducción más detallada sobre los archivos de proyecto y WPP, vea [dentro del Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-project-file.md)
> [Siguiente](building-and-packaging-web-application-projects.md)
