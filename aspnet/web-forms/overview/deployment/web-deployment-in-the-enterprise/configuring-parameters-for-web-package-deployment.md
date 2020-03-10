---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configuración de parámetros para la implementación de paquetes Web | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo establecer valores de parámetro, como nombres de aplicaciones Web de Internet Information Services (IIS), cadenas de conexión y extremos de servicio,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438403"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Configurar los parámetros para la implementación de paquetes web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo establecer los valores de los parámetros, como los nombres de las aplicaciones Web de Internet Information Services (IIS), las cadenas de conexión y los puntos de conexión de servicio, cuando se implementa un paquete Web en un servidor Web de IIS remoto.

Al compilar un proyecto de aplicación Web, el proceso de compilación y empaquetado genera tres archivos de clave:

- Un archivo *[nombre de proyecto]. zip* . Este es el paquete de implementación web para el proyecto de aplicación Web. Este paquete contiene todos los ensamblados, archivos, scripts de base de datos y recursos necesarios para volver a crear la aplicación web en un servidor Web de IIS remoto.
- Un archivo *[nombre de proyecto]. deploy. cmd* . Contiene un conjunto de comandos de Web Deploy con parámetros (MSDeploy. exe) que publican el paquete de implementación web en un servidor Web de IIS remoto.
- *[Nombre de proyecto]. Archivo SetParameters. XML* . Esto proporciona un conjunto de valores de parámetro al comando MSDeploy. exe. Puede actualizar los valores de este archivo y pasarlo a Web Deploy como un parámetro de línea de comandos al implementar el paquete Web.

> [!NOTE]
> Para obtener más información sobre el proceso de compilación y empaquetado, vea [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md).

El archivo *SetParameters. XML* se genera dinámicamente a partir del archivo de proyecto de aplicación web y los archivos de configuración del proyecto. Al compilar y empaquetar el proyecto, la canalización de publicación web (WPP) detectará automáticamente muchas de las variables que es probable que cambien entre los entornos de implementación, como la aplicación Web de IIS de destino y cualquier cadena de conexión de base de datos. Estos valores se parametrizan automáticamente en el paquete de implementación web y se agregan al archivo *SetParameters. XML* . Por ejemplo, si agrega una cadena de conexión al archivo *Web. config* en el proyecto de aplicación Web, el proceso de compilación detectará este cambio y agregará una entrada al archivo *SetParameters. XML* en consecuencia.

En muchos casos, esta parametrización automática será suficiente. Sin embargo, si los usuarios necesitan variar otros valores entre los entornos de implementación, como la configuración de la aplicación o las direcciones URL del punto de conexión de servicio, debe indicar a WPP que aplique la parametrización de estos valores en el paquete de implementación y que agregue las entradas correspondientes al archivo *SetParameters. XML* . En las secciones siguientes se explica cómo hacerlo.

### <a name="automatic-parameterization"></a>Parametrización automática

Al compilar y empaquetar una aplicación Web, WPP realizará una parametrización automática de estas cosas:

- La ruta de acceso y el nombre de la aplicación Web de IIS de destino.
- Cualquier cadena de conexión en el archivo *Web. config* .
- Cadenas de conexión para las bases de datos que se agregan a la pestaña **empaquetar/publicar SQL** de las páginas de propiedades del proyecto.

Por ejemplo, si fuera a compilar y empaquetar la solución de ejemplo de [Contact Manager](the-contact-manager-solution.md) sin tocar el proceso de parametrización de ningún modo, el WPP generaría este archivo *ContactManager. Mvc. SetParameters. XML* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

En este caso:

- El parámetro de **nombre de aplicación Web de IIS** es la ruta de acceso de IIS donde desea implementar la aplicación Web. El valor predeterminado se toma de la página **Web Package/Publish** en las páginas de propiedades del proyecto.
- El parámetro de **cadena de conexión ApplicationServices-Web. config** se generó a partir de un elemento **connectionStrings/Add** en el archivo *Web. config* . Representa la cadena de conexión que debe utilizar la aplicación para ponerse en contacto con la base de datos de pertenencia. El valor que proporcione aquí se sustituirá en el archivo *Web. config* implementado. El valor predeterminado se toma del archivo *Web. config* anterior a la implementación.

WPP también parametriza estas propiedades en el paquete de implementación que genera. Puede proporcionar valores para estas propiedades al instalar el paquete de implementación. Si instala el paquete manualmente a través del administrador de IIS, como se describe en [instalación manual de paquetes web](manually-installing-web-packages.md), el Asistente para la instalación le solicita que proporcione valores para los parámetros. Si instala el paquete de forma remota mediante el archivo *. deploy. cmd* , tal y como se describe en [implementar paquetes web](deploying-web-packages.md), Web deploy buscará en este archivo *SetParameters. XML* para proporcionar los valores de los parámetros. Puede editar los valores del archivo *SetParameters. XML* manualmente o puede personalizar el archivo como parte de un proceso automatizado de compilación e implementación. Este proceso se describe con más detalle más adelante en este tema.

### <a name="custom-parameterization"></a>Parametrización personalizada

En escenarios de implementación más complejos, a menudo querrá parametrizar propiedades adicionales antes de implementar el proyecto. En general, debe parametrizar cualquier propiedad y configuración que varíe entre los entornos de destino. Estos pueden incluir:

- Puntos de conexión de servicio en el archivo *Web. config* .
- Configuración de la aplicación en el archivo *Web. config* .
- Cualquier otra propiedad declarativa que desee solicitar a los usuarios que especifiquen.

La forma más fácil de parametrizar estas propiedades es agregar un archivo *Parameters. XML* a la carpeta raíz del proyecto de aplicación Web. Por ejemplo, en la solución Contact Manager, el proyecto ContactManager. MVC incluye un archivo *Parameters. XML* en la carpeta raíz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Si abre este archivo, verá que contiene una sola entrada de **parámetro** . La entrada utiliza una consulta XPath (lenguaje de rutas XML) para localizar y parametrizar la dirección URL del punto de conexión del servicio ContactService Windows Communication Foundation (WCF) en el archivo *Web. config* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Además de parametrizar la dirección URL del punto de conexión en el paquete de implementación, WPP también agrega una entrada correspondiente al archivo *SetParameters. XML* que se genera junto con el paquete de implementación.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

Si instala el paquete de implementación manualmente, el administrador de IIS le pedirá la dirección del punto de conexión de servicio junto con las propiedades que se parametrizaron automáticamente. Si instala el paquete de implementación mediante la ejecución del archivo *. deploy. cmd* , puede editar el archivo *SetParameters. XML* para proporcionar un valor para la dirección del punto de conexión de servicio junto con los valores de las propiedades que se parametrizaron automáticamente.

Para obtener detalles completos sobre cómo crear un archivo *Parameters. XML* , consulte [Cómo: usar parámetros para configurar las opciones de implementación cuando se instala un paquete](https://msdn.microsoft.com/library/ff398068.aspx). El procedimiento denominado **para usar parámetros de implementación para la configuración del archivo Web. config** proporciona instrucciones paso a paso.

## <a name="modifying-the-setparametersxml-file"></a>Modificación del archivo SetParameters. XML

Si tiene previsto implementar el paquete de aplicación Web manualmente&#x2014;mediante la ejecución del archivo *. deploy. cmd* o mediante la ejecución de MSDeploy. exe desde la&#x2014;línea de comandos, no hay nada que le detenga editar manualmente el archivo *SetParameters. XML* antes de la implementación. Sin embargo, si está trabajando en una solución de escala empresarial, puede que tenga que implementar un paquete de aplicación web como parte de un proceso de compilación e implementación automatizado de mayor tamaño. En este escenario, necesitará el Microsoft Build Engine (MSBuild) para modificar el archivo *SetParameters. XML* . Puede hacerlo mediante la tarea **xmlpoke (** de MSBuild.

La [solución de ejemplo Contact Manager](the-contact-manager-solution.md) ilustra este proceso. Los ejemplos de código siguientes se han editado para mostrar solo los detalles que son relevantes para este ejemplo.

> [!NOTE]
> Para obtener una introducción más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizados en general, vea [Descripción del archivo de proyecto](understanding-the-project-file.md) y [Descripción del proceso de compilación](understanding-the-build-process.md).

En primer lugar, los valores de parámetro de interés se definen como propiedades en el archivo de proyecto específico del entorno (por ejemplo, *env-dev. proj*).

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configuración de las propiedades de implementación para un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

A continuación, el archivo *Publish. proj* importa estas propiedades. Dado que cada archivo *SetParameters. XML* está asociado a un archivo *. deploy. cmd* y, en última instancia, deseamos que el archivo de proyecto invoque cada archivo *. deploy. cmd* , el archivo de proyecto crea un *elemento* de MSBuild para cada archivo *. deploy. cmd* y define las propiedades de interés como *metadatos de elemento*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

En este caso:

- El valor de metadatos **ParametersXml** indica la ubicación del archivo *SetParameters. XML* .
- El valor de **IisWebAppName** es la ruta de acceso de IIS en la que desea implementar la aplicación Web.
- El valor de **MembershipDBConnectionString** es la cadena de conexión para la base de datos de pertenencia y el valor de **MembershipDBConnectionName** es el atributo de **nombre** del parámetro correspondiente en el archivo *SetParameters. XML* .
- El valor **ServiceEndpointValue** es la dirección del extremo para el servicio WCF en el servidor de destino y el valor **ServiceEndpointParamName** es el atributo name del parámetro correspondiente en el archivo *SetParameters. XML* .

Por último, en el archivo *Publish. proj* , el destino **PublishWebPackages** usa la tarea **xmlpoke (** para modificar estos valores en el archivo *SetParameters. XML* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Observará que cada tarea **xmlpoke (** especifica cuatro valores de atributo:

- El atributo **XmlInputPath** indica a la tarea dónde encontrar el archivo que desea modificar.
- El atributo **query** es una consulta XPath que identifica el nodo XML que se desea cambiar.
- El atributo **valor** es el nuevo valor que se desea insertar en el nodo XML seleccionado.
- El atributo **Condition** es el criterio en el que la tarea se debe ejecutar o no. En estos casos, la condición garantiza que no intente insertar un valor nulo o vacío en el archivo *SetParameters. XML* .

## <a name="conclusion"></a>Conclusión

En este tema se describe el rol del archivo *SetParameters. XML* y se explica cómo se genera al compilar un proyecto de aplicación Web. Explica cómo puede parametrizar la configuración adicional agregando un archivo *Parameters. XML* al proyecto. También se describe cómo puede modificar el archivo *SetParameters. XML* como parte de un proceso de compilación más grande y automatizado mediante la tarea **xmlpoke (** en los archivos del proyecto.

En el tema siguiente, [implementar paquetes web](deploying-web-packages.md), se describe cómo se puede implementar un paquete Web mediante la ejecución del archivo *. deploy. cmd* o mediante los comandos MSDeploy. exe directamente. En ambos casos, puede especificar el archivo *SetParameters. XML* como un parámetro de implementación.

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo crear paquetes Web, vea [compilar y empaquetar proyectos de aplicación web](building-and-packaging-web-application-projects.md). Para obtener instrucciones sobre cómo implementar realmente un paquete Web, vea [implementar paquetes web](deploying-web-packages.md). Para obtener un tutorial paso a paso sobre cómo crear un archivo *Parameters. XML* , consulte [Cómo: usar parámetros para establecer la configuración de implementación cuando se instala un paquete](https://msdn.microsoft.com/library/ff398068.aspx).

Para obtener más información general sobre la parametrización en Web Deploy, vea [Web deploy parametrización en acción](https://go.microsoft.com/?linkid=9805119) (entrada de blog).

> [!div class="step-by-step"]
> [Anterior](building-and-packaging-web-application-projects.md)
> [Siguiente](deploying-web-packages.md)
