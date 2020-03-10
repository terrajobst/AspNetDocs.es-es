---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de las propiedades del proyecto: 4 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507739"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: configuración de las propiedades del proyecto (4 de 12)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

Algunas opciones de implementación se configuran en las propiedades del proyecto que se almacenan en el archivo del proyecto (el archivo *. csproj* o *. vbproj* ). En la mayoría de los casos, los valores predeterminados de esta configuración son los deseados, pero puede usar la interfaz de usuario de **propiedades del proyecto** integrada en Visual Studio para trabajar con esta configuración si tiene que cambiarlas. En este tutorial, revisará la configuración de implementación en **las propiedades del proyecto**. También creará un archivo de marcador de posición que provoque la implementación de una carpeta vacía.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configuración de las opciones de implementación en la ventana Propiedades del proyecto

La mayoría de las opciones de configuración que afectan a lo que ocurre durante la implementación se incluyen en el perfil de publicación, como verá en los siguientes tutoriales. Algunos valores de configuración que debe tener en cuenta se encuentran en las pestañas **empaquetar/publicar** de la ventana **propiedades del proyecto** . Esta configuración se especifica para cada configuración de compilación, es decir, puede tener valores diferentes para una compilación de versión que para una compilación de depuración.

En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **ContosoUniversity** , seleccione **propiedades**y, a continuación, seleccione la pestaña **empaquetar/publicar web** .

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Cuando se muestra la ventana, se muestra de forma predeterminada la configuración de la configuración de compilación actualmente activa para la solución. Si el cuadro de **configuración** no indica **activo (versión)** , seleccione **liberar** para mostrar la configuración de la versión de lanzamiento. Implementará las compilaciones de versión en los entornos de prueba y producción.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Con **Active (release)** o **Release** seleccionado, verá los valores que son efectivos al implementar mediante la configuración de la versión de lanzamiento:

- En el cuadro **elementos para implementar** , solo se seleccionan **los archivos necesarios para ejecutar la aplicación** . Otras opciones son **todos los archivos de este proyecto** o **todos los archivos de esta carpeta de proyecto**. Al mantener la selección predeterminada sin cambios, evita implementar archivos de código fuente, por ejemplo. Esta es la razón por la que se deben incluir en el proyecto las carpetas que contienen los SQL Server Compact archivos binarios. Para obtener más información acerca de esta configuración, consulte **¿por qué no se implementan todos los archivos de la carpeta del proyecto?** en [preguntas más frecuentes sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- Se selecciona **excluir símbolos de depuración generados** . No se realizará la depuración cuando use esta configuración de compilación.
- **Excluya los archivos de la aplicación\_carpeta datos** no está seleccionada. El archivo de SQL Server Compact para la base de datos de pertenencia está en esa carpeta y tiene que implementarlo. Al implementar actualizaciones que no incluyen cambios en la base de datos, seleccionará esta casilla.
- **Precompilar esta aplicación antes de la publicación** no está seleccionada. En la mayoría de los escenarios, no es necesario precompilar proyectos de aplicación Web. Para obtener más información sobre esta opción, vea [empaquetar/publicar web (pestaña), propiedades del proyecto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) y [Configuración avanzada de precompilación cuadro de diálogo](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Incluir todas las bases de datos configuradas en la pestaña empaquetar/publicar SQL** está seleccionada, pero esta opción no tiene ningún efecto ahora porque no está configurando la pestaña **empaquetar/publicar SQL** . Esta pestaña es para un método de implementación de base de datos heredado que solía ser la única opción para implementar bases de datos de SQL Server. Usará la pestaña **empaquetar/publicar SQL** del tutorial [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- La sección de **configuración del paquete de implementación web** no es aplicable porque usa la publicación con un solo clic en estos tutoriales.

Cambie el cuadro desplegable **configuración** para depurar y ver la configuración predeterminada para las compilaciones de depuración. Los valores son los mismos, excepto en que se **excluyen los símbolos de depuración generados** para que pueda depurar al implementar una compilación de depuración.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Asegurarse de que se implementa la carpeta Elmah

Como vimos en el tutorial anterior, el [paquete NuGet de Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) proporciona funcionalidad para el registro de errores y los informes. En la aplicación de Contoso University, se ha configurado Elmah para almacenar los detalles del error en una carpeta denominada *Elmah*:

![Carpeta Elmah](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluir archivos o carpetas específicos de la implementación es un requisito común; otro ejemplo sería una carpeta a la que los usuarios pueden cargar archivos. No quiere que los archivos de registro o los archivos cargados que se crearon en el entorno de desarrollo se implementen en producción. Y si está implementando una actualización en producción, no desea que el proceso de implementación elimine los archivos que existen en producción. (En función de cómo establezca una opción de implementación, si existe un archivo en el sitio de destino pero no en el sitio de origen al implementar, Web Deploy lo elimina del destino).

Como vimos anteriormente en este tutorial, la opción **elementos para implementar** de la pestaña **empaquetar/publicar web** está establecida en **solo los archivos necesarios para ejecutar esta aplicación**. Como resultado, no se implementarán los archivos de registro creados por Elmah en desarrollo, que es lo que desea que suceda. (Para su implementación, deben incluirse en el proyecto y la propiedad acción de **compilación** debe establecerse en **contenido**. Para obtener más información, consulte **¿por qué no se implementan todos los archivos de la carpeta del proyecto?** en las [preguntas más frecuentes sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). Sin embargo, Web Deploy no creará una carpeta en el sitio de destino a menos que haya al menos un archivo para copiarlo. Por lo tanto, agregará un archivo *. txt* a la carpeta para que actúe como un marcador de posición para que se copie la carpeta.

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Elmah* , seleccione **Agregar nuevo elemento**y cree un archivo denominado *placeholder. txt*. Escriba el texto siguiente: "este es un archivo de marcador de posición para asegurarse de que se implementa la carpeta". y guarde el archivo. Eso es todo lo que tiene que hacer para asegurarse de que Visual Studio implementa este archivo y la carpeta en la que se encuentra, ya que la propiedad **acción de compilación** de los archivos *. txt* se establece en **contenido** de forma predeterminada.

Ya ha completado todas las tareas de configuración de la implementación. En el siguiente tutorial, implementará el sitio de Contoso University en el entorno de prueba y lo probará.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
