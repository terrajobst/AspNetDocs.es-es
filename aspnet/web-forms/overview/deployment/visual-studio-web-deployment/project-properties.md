---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Implementación web de ASP.NET con Visual Studio: propiedades del proyecto | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614953"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implementación web de ASP.NET con Visual Studio: propiedades del proyecto

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

Algunas opciones de implementación se configuran en las propiedades del proyecto que se almacenan en el archivo del proyecto (el archivo *. csproj* o *. vbproj* ). En la mayoría de los casos, los valores predeterminados de esta configuración son los deseados, pero puede usar la interfaz de usuario de **propiedades del proyecto** integrada en Visual Studio para trabajar con esta configuración si tiene que cambiarlas. En este tutorial, revisará la configuración de implementación en **las propiedades del proyecto**. También creará un archivo de marcador de posición que provoque la implementación de una carpeta vacía.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurar las opciones de implementación en la ventana Propiedades del proyecto

La mayoría de las opciones de configuración que afectan a lo que ocurre durante la implementación se incluyen en el perfil de publicación, como verá en los siguientes tutoriales. Algunos valores de configuración que debe tener en cuenta se encuentran en las pestañas **empaquetar/publicar** de la ventana **propiedades del proyecto** . Esta configuración se especifica para cada configuración de compilación, es decir, puede tener valores diferentes para una compilación de versión que para una compilación de depuración.

En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **ContosoUniversity** , seleccione **propiedades**y, a continuación, seleccione la pestaña **empaquetar/publicar web** .

![Pestaña Empaquetar/publicar web](project-properties/_static/image1.png)

Cuando se muestra la ventana, se muestra de forma predeterminada la configuración de la configuración de compilación actualmente activa para la solución. Si el cuadro de **configuración** no indica **activo (versión)** , seleccione **liberar** para mostrar la configuración de la versión de lanzamiento. Implementará las compilaciones de versión en los entornos de prueba y producción.

![Seleccionar la configuración de compilación de versión](project-properties/_static/image2.png)

Con **Active (release)** o **Release** seleccionado, verá los valores que son efectivos al implementar mediante la configuración de la versión de lanzamiento:

- En el cuadro **elementos para implementar** , solo se seleccionan **los archivos necesarios para ejecutar la aplicación** . Otras opciones son **todos los archivos de este proyecto** o **todos los archivos de esta carpeta de proyecto**. Al mantener la selección predeterminada sin cambios, evita implementar archivos de código fuente, por ejemplo. Esta es la razón por la que se deben incluir en el proyecto las carpetas que contienen los SQL Server Compact archivos binarios. Para obtener más información acerca de esta configuración, consulte **¿por qué no se implementan todos los archivos de la carpeta del proyecto?** en [preguntas más frecuentes sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- Se selecciona **excluir símbolos de depuración generados** . No se realizará la depuración cuando use esta configuración de compilación.
- Se selecciona **incluir todas las bases de datos configuradas en la pestaña empaquetar/publicar SQL** . Especifica si Visual Studio implementará las bases de datos y los archivos. Aunque la etiqueta de casilla solo menciona la pestaña **empaquetar/publicar SQL** , al desactivar esta casilla también se deshabilitaría la implementación de la base de datos que está configurada en el perfil de publicación. Lo hará más adelante, por lo que la casilla debe permanecer seleccionada. La pestaña **empaquetar/publicar SQL** se usa para un método de publicación de base de datos heredado que no va a usar en estos tutoriales.
- La sección de **configuración del paquete de implementación web** no es aplicable porque usa la publicación con un solo clic en estos tutoriales.

Cambie el cuadro desplegable **configuración** para depurar y ver la configuración predeterminada para las compilaciones de depuración. Los valores son los mismos, excepto en que se **excluyen los símbolos de depuración generados** para que pueda depurar al implementar una compilación de depuración.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Asegúrese de que se implementa la carpeta Elmah

Como vimos en el tutorial anterior, el [paquete NuGet de Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona funcionalidad para el registro de errores y los informes. En la aplicación de Contoso University, se ha configurado Elmah para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta Elmah](project-properties/_static/image3.png)

Excluir archivos o carpetas específicos de la implementación es un requisito común; otro ejemplo sería una carpeta a la que los usuarios pueden cargar archivos. No quiere que los archivos de registro o los archivos cargados que se crearon en el entorno de desarrollo se implementen en producción. Y si está implementando una actualización en producción, no desea que el proceso de implementación elimine los archivos que existen en producción. (En función de cómo establezca una opción de implementación, si existe un archivo en el sitio de destino pero no en el sitio de origen al implementar, Web Deploy lo elimina del destino).

Como vimos anteriormente en este tutorial, la opción **elementos para implementar** de la pestaña **empaquetar/publicar web** está establecida en **solo los archivos necesarios para ejecutar esta aplicación**. Como resultado, no se implementarán los archivos de registro creados por Elmah en desarrollo, que es lo que desea que suceda. (Para su implementación, deben incluirse en el proyecto y la propiedad acción de **compilación** debe establecerse en **contenido**. Para obtener más información, consulte **¿por qué no se implementan todos los archivos de la carpeta del proyecto?** en las [preguntas más frecuentes sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiarlo. Por lo tanto, agregará un archivo *. txt* a la carpeta para que actúe como un marcador de posición para que se copie la carpeta.

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Elmah* , seleccione **Agregar nuevo elemento**y cree un archivo de texto denominado *placeholder. txt*. Escriba el texto siguiente: "este es un archivo de marcador de posición para asegurarse de que se implementa la carpeta". y guarde el archivo. Eso es todo lo que tiene que hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta en la que se encuentra, ya que la propiedad **acción de compilación** de los archivos *. txt* se establece en **contenido** de forma predeterminada.

## <a name="summary"></a>Resumen

Ya ha completado todas las tareas de configuración de la implementación. En el siguiente tutorial, implementará el sitio de Contoso University en el entorno de prueba y lo probará.

> [!div class="step-by-step"]
> [Anterior](web-config-transformations.md)
> [Siguiente](deploying-to-iis.md)
