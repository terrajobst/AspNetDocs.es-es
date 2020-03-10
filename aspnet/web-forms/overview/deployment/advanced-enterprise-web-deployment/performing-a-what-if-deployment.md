---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Realización de una implementación de What If | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo realizar implementaciones "What if" (o simuladas) mediante la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) y V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510337"
---
# <a name="performing-a-what-if-deployment"></a>Realizar una implementación de hipótesis

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo realizar implementaciones "What if" (o simuladas) mediante la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) y VSDBCMD. Esto le permite determinar los efectos de la lógica de implementación en un entorno de destino determinado antes de implementar realmente la aplicación.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el que el&#x2014;proceso de compilación e implementación está controlado por dos archivos de proyecto que contienen instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Realización de una implementación "What If" para paquetes Web

Web Deploy incluye funcionalidad que permite realizar implementaciones en el modo "What if" (o prueba). Cuando se implementan artefactos en modo "What if", Web Deploy genera un archivo de registro como si hubiera realizado la implementación, pero en realidad no cambia nada en el servidor de destino. Revisar el archivo de registro puede ayudarle a entender el impacto que tendrá la implementación en el servidor de destino, en particular:

- Lo que se agregará.
- Lo que se actualizará.
- Lo que se eliminará.

Dado que una implementación "What if" no cambia realmente nada en el servidor de destino, lo que no puede hacer siempre es predecir si una implementación se realizará correctamente.

Tal y como se describe en [implementar paquetes web](../web-deployment-in-the-enterprise/deploying-web-packages.md), puede implementar paquetes web mediante Web deploy de dos&#x2014;maneras mediante la utilidad de línea de comandos MSDeploy. exe directamente o mediante la ejecución del archivo *. deploy. cmd* que genera el proceso de compilación.

Si usa MSDeploy. exe directamente, puede ejecutar una implementación de "What if" agregando la marca **– Whatif** al comando. Por ejemplo, para evaluar lo que ocurrirá si implementó el paquete ContactManager. Mvc. zip en un entorno de ensayo, el comando MSDeploy debe ser similar a este:

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Cuando esté satisfecho con los resultados de la implementación de "Qué sucede si", puede quitar la marca **– Whatif** para ejecutar una implementación en vivo.

> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para MSDeploy. exe, consulte [Web deploy configuración](https://technet.microsoft.com/library/dd569089(WS.10).aspx)de la operación.

Si usa el archivo *. deploy. cmd* , puede ejecutar una implementación de "What if" incluyendo la marca **/t** (modo de prueba) en lugar de la marca **/y** ("sí" o el modo de actualización) en el comando. Por ejemplo, para evaluar lo que ocurrirá si implementó el paquete ContactManager. Mvc. zip ejecutando el archivo *. deploy. cmd* , el comando debe ser similar al siguiente:

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Cuando esté satisfecho con los resultados de la implementación de "modo de prueba", puede reemplazar la marca **/t** por una marca **/y** para ejecutar una implementación en vivo:

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para los archivos *. deploy. cmd* , consulte [Cómo: instalar un paquete de implementación con el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Si ejecuta el archivo *. deploy. cmd* sin especificar ningún marcador, el símbolo del sistema mostrará una lista de marcas disponibles.

## <a name="performing-a-what-if-deployment-for-databases"></a>Realización de una implementación "What If" para las bases de datos

En esta sección se da por supuesto que está usando la utilidad VSDBCMD para realizar la implementación incremental de la base de datos basada en esquemas. Este enfoque se describe con más detalle en [implementación de proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Le recomendamos que se familiarice con este tema antes de aplicar los conceptos que se describen aquí.

Al usar VSDBCMD en modo de **implementación** , puede usar la marca **/DD** (o **/DEPLOYTODATABASE**) para controlar si VSDBCMD implementa realmente la base de datos o simplemente genera un script de implementación. Si va a implementar un archivo. dbschema, este es el comportamiento:

- Si especifica **/DD +** o **/DD**, VSDBCMD generará un script de implementación e implementará la base de datos.
- Si especifica **/DD-** u omite el modificador, VSDBCMD solo generará un script de implementación.

> [!NOTE]
> Si va a implementar un archivo. DeployManifest en lugar de un archivo. dbschema, el comportamiento del modificador **/DD** es mucho más complicado. En esencia, VSDBCMD omitirá el valor del modificador **/DD** si el archivo. DeployManifest incluye un elemento **DeployToDatabase** con un valor de **true**. La [implementación de proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md) describe este comportamiento en su totalidad.

Por ejemplo, para generar un script de implementación para la base de datos **ContactManager** sin implementar realmente la base de datos, el comando VSDBCMD debe ser similar a este:

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD es una herramienta de implementación diferencial de bases de datos y, como tal, el script de implementación se genera dinámicamente para contener todos los comandos SQL necesarios para actualizar la base de datos actual, si existe, al esquema especificado. Revisar el script de implementación es una manera útil de determinar qué impacto tendrá la implementación en la base de datos actual y en los datos que contiene. Por ejemplo, puede que desee determinar:

- Si se quitarán las tablas existentes y si eso dará lugar a la pérdida de datos.
- Si el orden de las operaciones conlleva un riesgo de pérdida de datos, por ejemplo, si está dividiendo o combinando tablas.

Si está satisfecho con el script de implementación, puede repetir el comando VSDBCMD con una marca **/DD +** para realizar los cambios. Como alternativa, puede editar el script de implementación para que se ajuste a sus requisitos y, a continuación, ejecutarlo manualmente en el servidor de base de datos.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integración de la funcionalidad "What If" en archivos de proyecto personalizados

En escenarios de implementación más complejos, querrá usar un archivo de proyecto de Microsoft Build Engine personalizado (MSBuild) para encapsular la lógica de compilación e implementación, como se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Por ejemplo, en la solución de ejemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , el archivo *Publish. proj* :

- Compila la solución.
- Usa Web Deploy para empaquetar e implementar la aplicación ContactManager. Mvc.
- Utiliza Web Deploy para empaquetar e implementar la aplicación ContactManager. Service.
- Implementa la base de datos **ContactManager** .

Al integrar la implementación de varios paquetes web y bases de datos en un proceso de un solo paso de esta manera, puede que también desee la opción de realizar toda la implementación en un modo "Qué si".

El archivo *Publish. proj* muestra cómo puede hacerlo. En primer lugar, debe crear una propiedad para almacenar el valor "What if":

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

En este caso, ha creado una propiedad denominada **Whatif** con un valor predeterminado de **false**. Los usuarios pueden invalidar este valor estableciendo la propiedad en **true** en un parámetro de línea de comandos, como verá en breve.

La siguiente fase consiste en parametrizar los comandos Web Deploy y VSDBCMD para que los marcadores reflejen el valor de la propiedad **Whatif** . Por ejemplo, el siguiente destino (tomado del archivo *Publish. proj* y simplificado) ejecuta el archivo *. deploy. cmd* para implementar un paquete Web. De forma predeterminada, el comando incluye un modificador **/y** ("sí" o el modo de actualización). Si **Whatif** está establecido en **true**, se sustituye por un modificador **/t** (Trial o "What if").

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

Del mismo modo, el siguiente destino usa la utilidad VSDBCMD para implementar una base de datos. De forma predeterminada, no se incluye un modificador **/DD** . Esto significa que VSDBCMD generará un script de implementación pero no implementará la&#x2014;base de datos en otras palabras, un escenario "What if". Si la propiedad **Whatif** no está establecida en **true**, se agrega un modificador **/DD** y VSDBCMD implementará la base de datos.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

Puede usar el mismo enfoque para parametrizar todos los comandos pertinentes en el archivo del proyecto. Si desea ejecutar una implementación de tipo "What if", puede simplemente proporcionar un valor de propiedad **Whatif** desde la línea de comandos:

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

De esta manera, puede ejecutar una implementación "What if" para todos los componentes del proyecto en un solo paso.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo ejecutar implementaciones "What if" con Web Deploy, VSDBCMD y MSBuild. Una implementación "What if" le permite evaluar el impacto de una implementación propuesta antes de realizar cambios en el entorno de destino.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre Web Deploy sintaxis de la línea de comandos, vea configuración de la [operación de web deploy](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Para obtener instrucciones sobre las opciones de línea de comandos cuando se usa el archivo *. deploy. cmd* , vea [Cómo: instalar un paquete de implementación con el archivo deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Para obtener instrucciones sobre la sintaxis de línea de comandos de VSDBCMD, vea [referencia de la línea de comandos para VSDBCMD. EXE (implementación y importación de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Anterior](advanced-enterprise-web-deployment.md)
> [Siguiente](customizing-database-deployments-for-multiple-environments.md)
