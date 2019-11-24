---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Descripción del archivo de proyecto | Microsoft Docs
author: jrjlee
description: Los archivos de proyecto de Microsoft Build Engine (MSBuild) se encuentran en el núcleo del proceso de compilación e implementación. En este tema se empieza con una introducción a los conceptos de MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445694"
---
# <a name="understanding-the-project-file"></a>Descripción del archivo de proyecto

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Los archivos de proyecto de Microsoft Build Engine (MSBuild) se encuentran en el núcleo del proceso de compilación e implementación. En este tema se empieza con una introducción a los conceptos de MSBuild y el archivo de proyecto. Describe los componentes clave que se aplicarán al trabajar con archivos de proyecto y funciona a través de un ejemplo de cómo puede usar archivos de proyecto para implementar aplicaciones reales.
> 
> Lo que aprenderá:
> 
> - Cómo usa MSBuild los archivos de proyecto de MSBuild para compilar proyectos.
> - Cómo se integra MSBuild con tecnologías de implementación, como la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy).
> - Cómo comprender los componentes clave de un archivo de proyecto.
> - Cómo puede usar archivos de proyecto para compilar e implementar aplicaciones complejas.

## <a name="msbuild-and-the-project-file"></a>MSBuild y el archivo de proyecto

Al crear y compilar soluciones en Visual Studio, Visual Studio usa MSBuild para compilar cada proyecto en la solución. Cada proyecto de Visual Studio incluye un archivo de proyecto de MSBuild, con una extensión de archivo que refleja&#x2014;el tipo de proyecto C# , por ejemplo, un proyecto (. csproj), un proyecto de Basic.net visual (. vbproj) o un proyecto de base de datos (. dbproj). Para compilar un proyecto, MSBuild debe procesar el archivo de proyecto asociado al proyecto. El archivo de proyecto es un documento XML que contiene toda la información y las instrucciones que MSBuild necesita para compilar el proyecto, como el contenido que se debe incluir, los requisitos de plataforma, la información de control de versiones, la configuración del servidor web o del servidor de bases de datos, y el tareas que se deben realizar.

Los archivos de proyecto de MSBuild se basan en el [esquema XML de MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)y, como resultado, el proceso de compilación está completamente abierto y transparente. Además, no es necesario instalar Visual Studio para poder usar el motor&#x2014;de MSBuild. el ejecutable de MSBuild. exe forma parte del .NET Framework y se puede ejecutar desde un símbolo del sistema. Como programador, puede crear sus propios archivos de proyecto de MSBuild, mediante el esquema XML de MSBuild, para imponer un control sofisticado y preciso sobre cómo se compilan e implementan los proyectos. Estos archivos de proyecto personalizados funcionan exactamente de la misma manera que los archivos de proyecto que Visual Studio genera automáticamente.

> [!NOTE]
> También puede usar archivos de proyecto de MSBuild con el servicio Team Build en Team Foundation Server (TFS). Por ejemplo, puede usar archivos de proyecto en escenarios de integración continua (CI) para automatizar la implementación en un entorno de prueba cuando se protege un nuevo código. Para obtener más información, consulte [configuración de Team Foundation Server para la implementación web automática](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Convenciones de nomenclatura de archivos de proyecto

Cuando cree sus propios archivos de proyecto, puede utilizar cualquier extensión de archivo que desee. Sin embargo, para facilitar la comprensión de las soluciones, debe usar estas convenciones comunes:

- Use la extensión. proj cuando cree un archivo de proyecto que compile proyectos de.
- Use la extensión. targets al crear un archivo de proyecto reutilizable para importar en otros archivos de proyecto. Los archivos con una extensión. targets no suelen compilar nada, simplemente contienen instrucciones que puede importar en los archivos. proj.

### <a name="integration-with-deployment-technologies"></a>Integración con tecnologías de implementación

Si ha trabajado con proyectos de aplicación web en Visual Studio 2010, como las aplicaciones Web de ASP.NET y las aplicaciones Web de ASP.NET MVC, sabrá que estos proyectos incluyen compatibilidad integrada para empaquetar e implementar la aplicación web en un entorno de destino. Las páginas de **propiedades** de estos proyectos incluyen las pestañas **empaquetar/publicar web** y **empaquetar/publicar SQL** , que puede usar para configurar cómo se empaquetan e implementan los componentes de la aplicación. Se muestra la pestaña **empaquetar/publicar web** :

![](understanding-the-project-file/_static/image1.png)

La tecnología subyacente que subyace a estas funcionalidades se conoce como la canalización de publicación web (WPP). WPP básicamente incorpora MSBuild y [Web deploy](https://go.microsoft.com/?linkid=9805122) para proporcionar un proceso completo de compilación, paquete e implementación para las aplicaciones Web.

La buena noticia es que puede aprovechar los puntos de integración que proporciona WPP al crear archivos de proyecto personalizados para proyectos Web. Puede incluir instrucciones de implementación en el archivo de proyecto, lo que le permite compilar los proyectos, crear paquetes de implementación web e instalar estos paquetes en servidores remotos a través de un único archivo de proyecto y una única llamada a MSBuild. También puede llamar a cualquier otro ejecutable como parte del proceso de compilación. Por ejemplo, puede ejecutar la herramienta de línea de comandos VSDBCMD. exe para implementar una base de datos desde un archivo de esquema. En el transcurso de este tema, verá cómo puede aprovechar estas capacidades para satisfacer los requisitos de los escenarios de implementación de su empresa.

> [!NOTE]
> Para obtener más información sobre cómo funciona el proceso de implementación de aplicaciones Web, vea [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>Anatomía de un archivo de proyecto

Antes de examinar el proceso de compilación con más detalle, merece la pena dedicar unos minutos a familiarizarse con la estructura básica de un archivo de proyecto de MSBuild. En esta sección se proporciona información general sobre los elementos más comunes que encontrará al revisar, editar o crear un archivo de proyecto. En concreto, aprenderá lo siguiente:

- Cómo usar *las propiedades* para administrar variables para el proceso de compilación.
- Cómo usar *elementos* para identificar las entradas en el proceso de compilación, como los archivos de código.
- Cómo usar *destinos* y *tareas* para proporcionar instrucciones de ejecución a MSBuild mediante *las propiedades* y *los elementos* definidos en otra parte del archivo de proyecto.

Esto muestra la relación entre los elementos clave de un archivo de proyecto de MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>El elemento Project

El elemento [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) es el elemento raíz de cada archivo de proyecto. Además de identificar el esquema XML para el archivo de proyecto, el elemento de **proyecto** puede incluir atributos para especificar los puntos de entrada para el proceso de compilación. Por ejemplo, en la [solución de ejemplo de Contact Manager](the-contact-manager-solution.md), el archivo *Publish. proj* especifica que la compilación debe comenzar llamando al destino denominado **FullPublish**.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Propiedades y condiciones

Normalmente, un archivo de proyecto necesita proporcionar una gran cantidad de información diferente para compilar e implementar correctamente sus proyectos. Estos fragmentos de información podrían incluir nombres de servidor, cadenas de conexión, credenciales, configuraciones de compilación, rutas de acceso de archivos de origen y de destino, así como cualquier otra información que desee incluir para admitir la personalización. En un archivo de proyecto, las propiedades se deben definir dentro de un elemento [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) . Las propiedades de MSBuild se componen de pares de clave y valor. Dentro del elemento **PropertyGroup** , el nombre del elemento define la clave de la propiedad y el contenido del elemento define el valor de la propiedad. Por ejemplo, puede definir propiedades denominadas **ServerName** y **ConnectionString** para almacenar un nombre de servidor estático y una cadena de conexión.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Para recuperar un valor de propiedad, use el formato *$ (PropertyName)* . Por ejemplo, para recuperar el valor de la propiedad **ServerName** , debe escribir:

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Verá ejemplos de cómo y Cuándo usar los valores de propiedad más adelante en este tema.

Insertar información como propiedades estáticas en un archivo de proyecto no es siempre el enfoque ideal para administrar el proceso de compilación. En muchos escenarios, querrá obtener la información de otros orígenes o permitir al usuario proporcionar la información desde el símbolo del sistema. MSBuild le permite especificar cualquier valor de propiedad como parámetro de línea de comandos. Por ejemplo, el usuario podría proporcionar un valor para **ServerName** cuando ejecute MSBuild. exe desde la línea de comandos.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Para obtener más información sobre los argumentos y modificadores que se pueden usar con MSBuild. exe, vea referencia de la [línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

Puede usar la misma sintaxis de propiedad para obtener los valores de las variables de entorno y las propiedades de proyecto integradas. Muchas de las propiedades que se usan con frecuencia se definen automáticamente y se pueden usar en los archivos del proyecto incluyendo el nombre del parámetro pertinente. Por ejemplo, para recuperar la&#x2014;plataforma de proyecto actual, por ejemplo, **x86** o **AnyCpu**&#x2014;, puede incluir la referencia de propiedad **$ (plataforma)** en el archivo de proyecto. Para obtener más información, vea [macros para comandos y propiedades de compilación](https://msdn.microsoft.com/library/c02as0cs.aspx), [propiedades comunes del proyecto de MSBuild](https://msdn.microsoft.com/library/bb629394.aspx)y [propiedades reservadas](https://msdn.microsoft.com/library/ms164309.aspx).

Las propiedades se usan a menudo junto con *las condiciones*. La mayoría de los elementos de MSBuild admiten el atributo **Condition** , que permite especificar los criterios sobre los que MSBuild debe evaluar el elemento. Por ejemplo, considere esta definición de propiedad:

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Cuando MSBuild procesa esta definición de propiedad, primero comprueba si un valor de la propiedad **$ (OutputRoot)** está disponible. Si el valor de la propiedad&#x2014;está en blanco en otras palabras, el usuario no proporcionó un&#x2014;valor para esta propiedad, la condición se evalúa como **true** y el valor de la propiedad se establece en **. \Publish\Out**. Si el usuario ha proporcionado un valor para esta propiedad, la condición se evalúa como **false** y no se utiliza el valor de la propiedad estática.

Para obtener más información sobre las distintas formas en que se pueden especificar condiciones, vea [condiciones de MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementos y grupos de elementos

Uno de los roles importantes del archivo de proyecto es definir las entradas para el proceso de compilación. Normalmente, estas entradas son archivos&#x2014;de código, archivos de configuración, archivos de comandos y otros archivos que se deben procesar o copiar como parte del proceso de compilación. En el esquema del proyecto de MSBuild, estas entradas se representan mediante elementos [Item](https://msdn.microsoft.com/library/ms164283.aspx) . En un archivo de proyecto, los elementos se deben definir dentro de un elemento [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) . Al igual que los elementos de **propiedad** , puede asignar un nombre a un elemento de **elemento** . Sin embargo, debe especificar un atributo **include** para identificar el archivo o el carácter comodín que representa el elemento.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

Al especificar varios elementos de **elemento** con el mismo nombre, se crea una lista de recursos con nombre. Una buena manera de verlo en acción consiste en echar un vistazo al interior de uno de los archivos de proyecto que Visual Studio crea. Por ejemplo, el archivo *ContactManager. Mvc. csproj* de la solución de ejemplo incluye muchos grupos de elementos, cada uno de ellos con varios elementos de **elemento** con el mismo nombre.

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

De esta manera, el archivo de proyecto indica a MSBuild que construya listas de archivos que deben procesarse de la misma&#x2014;manera que la lista de **referencia** incluye los ensamblados que deben estar en su lugar para una compilación correcta, la lista de **compilación** incluye los archivos de código que se deben compilar y la lista de **contenido** incluye los recursos que se deben copiar sin modificar. Veremos cómo el proceso de compilación hace referencia a estos elementos y los usa más adelante en este tema.

Los elementos de elemento también pueden incluir elementos secundarios de [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) . Estos son pares clave-valor definidos por el usuario y básicamente representan propiedades que son específicas de ese elemento. Por ejemplo, muchos de los elementos de elemento de **compilación** del archivo de proyecto incluyen elementos secundarios **DependentUpon** .

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> Además de los metadatos de los elementos creados por el usuario, a todos los elementos se les asignan varios metadatos comunes en la creación. Para obtener más información, consulte [Metadatos de elementos conocidos](https://msdn.microsoft.com/library/ms164313.aspx).

Puede crear elementos **ItemGroup** en el elemento de **proyecto** de nivel raíz o en elementos de **destino** específicos. Los elementos **ItemGroup** también admiten atributos de **condición** , lo que permite adaptar las entradas al proceso de compilación en función de condiciones como la configuración del proyecto o la plataforma.

### <a name="targets-and-tasks"></a>Destinos y tareas

En el esquema de MSBuild, un elemento [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) representa una instrucción de compilación individual (o tarea). MSBuild incluye una gran variedad de tareas predefinidas. Por ejemplo:

- La tarea de **copia** copia los archivos en una nueva ubicación.
- La tarea **CSC** invoca el compilador visual C# .
- La tarea **VBC** invoca el compilador de Visual Basic.
- La tarea **exec** ejecuta un programa especificado.
- La tarea **Message** escribe un mensaje en un registrador.

> [!NOTE]
> Para obtener detalles completos de las tareas que están disponibles de un cuadro, vea [referencia de tareas de MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Para obtener más información sobre las tareas, incluido cómo crear sus propias tareas personalizadas, vea [tareas de MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).

Las tareas siempre deben estar contenidas dentro de los elementos de [destino](https://msdn.microsoft.com/library/t50z2hka.aspx) . Un elemento de **destino** es un conjunto de una o varias tareas que se ejecutan secuencialmente y un archivo de proyecto puede contener varios destinos. Cuando se desea ejecutar una tarea o un conjunto de tareas, se invoca el destino que las contiene. Por ejemplo, supongamos que tiene un archivo de proyecto simple que registra un mensaje.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

Puede invocar el destino desde la línea de comandos mediante el modificador **/t** para especificar el destino.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

Como alternativa, puede Agregar un atributo **DefaultTargets** al elemento **Project** , para especificar los destinos que desea invocar.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

En este caso, no es necesario especificar el destino desde la línea de comandos. Puede simplemente especificar el archivo del proyecto y MSBuild invocará el destino **FullPublish** .

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Ambos destinos y tareas pueden incluir atributos de **condición** . Como tal, puede elegir omitir destinos completos o tareas individuales si se cumplen ciertas condiciones.

Por lo general, cuando cree tareas y destinos útiles, deberá hacer referencia a las propiedades y los elementos que haya definido en otra parte del archivo de proyecto:

- Para usar un valor de propiedad, escriba **$ (***PropertyName***)** , donde *PropertyName* es el nombre del elemento de **propiedad** o el nombre del parámetro.
- Para usar un elemento, escriba **@ (***itemname***)** , donde *itemname* **es el nombre del elemento.**

> [!NOTE]
> Recuerde que si crea varios elementos con el mismo nombre, está generando una lista. Por el contrario, si crea varias propiedades con el mismo nombre, el último valor de propiedad que proporcione sobrescribirá las propiedades anteriores con el mismo&#x2014;nombre. una propiedad solo puede contener un valor.

Por ejemplo, en el archivo *Publish. proj* de la solución de ejemplo, eche un vistazo al destino **BuildProjects** .

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

En este ejemplo, puede observar estos puntos clave:

- Si se especifica el parámetro **BuildingInTeamBuild** y tiene un valor de **true**, no se ejecutará ninguna de las tareas de este destino.
- El destino contiene una única instancia de la tarea [msbuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) . Esta tarea le permite compilar otros proyectos de MSBuild.
- El elemento **ProjectsToBuild** se pasa a la tarea. Este elemento podría representar una lista de archivos de proyecto o de solución, todos definidos por elementos de elemento de **ProjectsToBuild** dentro de un grupo de elementos. En este caso, el elemento **ProjectsToBuild** hace referencia a un único archivo de solución.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Los valores de propiedad pasados a la tarea **msbuild** incluyen parámetros denominados **OutputRoot** y **Configuration**. Se establecen en valores de parámetro si se proporcionan o valores de propiedad estáticos si no lo están.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

También puede ver que la tarea **msbuild** invoca un destino denominado **Build**. Se trata de uno de varios destinos integrados que se usan ampliamente en los archivos de proyecto de Visual Studio y que están disponibles en los archivos de proyecto personalizados, como **compilar**, **limpiar**, **recompilar**y **publicar**. Obtendrá más información sobre el uso de destinos y tareas para controlar el proceso de compilación y sobre la tarea de **msbuild** en concreto, más adelante en este tema.

> [!NOTE]
> Para obtener más información sobre los destinos, consulte [destinos de MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Dividir archivos de proyecto para admitir varios entornos

Supongamos que desea poder implementar una solución en varios entornos, como los servidores de prueba, las plataformas de ensayo y los entornos de producción. La configuración puede variar considerablemente entre estos entornos&#x2014;, no solo en términos de nombres de servidor, cadenas de conexión, etc., sino también en términos de credenciales, configuración de seguridad y muchos otros factores. Si necesita hacer esto con regularidad, no es realmente conveniente editar varias propiedades en el archivo de proyecto cada vez que cambia el entorno de destino. Tampoco es una solución ideal para requerir una lista interminable de valores de propiedad que se proporcionarán al proceso de compilación.

Afortunadamente, hay una alternativa. MSBuild le permite dividir la configuración de compilación en varios archivos de proyecto. Para ver cómo funciona esto, en la solución de ejemplo, observe que hay dos archivos de proyecto personalizados:

- *Publish. proj*, que contiene propiedades, elementos y destinos comunes a todos los entornos.
- *Env-dev. proj*, que contiene propiedades que son específicas de un entorno de desarrollo.

Observe ahora que el archivo *Publish. proj* incluye un elemento [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) , inmediatamente debajo de la etiqueta de apertura del **proyecto** .

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

El elemento **Import** se usa para importar el contenido de otro archivo de proyecto de MSBuild en el archivo de proyecto de MSBuild actual. En este caso, el parámetro **TargetEnvPropsFile** proporciona el nombre de archivo del archivo de proyecto que desea importar. Puede proporcionar un valor para este parámetro al ejecutar MSBuild.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

Esto combina eficazmente el contenido de los dos archivos en un único archivo de proyecto. Con este enfoque, puede crear un archivo de proyecto que contenga la configuración de compilación universal y varios archivos de proyecto complementarios que contengan propiedades específicas del entorno. Como resultado, la ejecución de un comando con un valor de parámetro diferente permite implementar la solución en un entorno diferente.

![](understanding-the-project-file/_static/image3.png)

Es recomendable seguir dividiendo los archivos de proyecto de esta manera. Permite a los desarrolladores implementar en varios entornos mediante la ejecución de un solo comando, a la vez que evita la duplicación de propiedades de compilación universal en varios archivos de proyecto.

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona una introducción general a los archivos de proyecto de MSBuild y se explica cómo puede crear sus propios archivos de proyecto personalizados para controlar el proceso de compilación. También se ha introducido el concepto de dividir archivos de proyecto en instrucciones de compilación universal y propiedades de compilación específicas del entorno, para facilitar la compilación e implementación de proyectos en varios destinos.

En el tema siguiente, se describe [el proceso de compilación](understanding-the-build-process.md), se proporciona más información sobre cómo se pueden usar los archivos de proyecto para controlar la compilación y la implementación, ya que le guiará a través de la implementación de una solución con un nivel realista de complejidad.

## <a name="further-reading"></a>Información adicional

Para obtener una introducción más detallada sobre los archivos de proyecto y WPP, vea [dentro del Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi y William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](setting-up-the-contact-manager-solution.md)
> [Siguiente](understanding-the-build-process.md)
