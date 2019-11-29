---
uid: web-forms/overview/deployment/visual-studio-web-deployment/introduction
title: 'Implementación web de ASP.NET con Visual Studio: introducción | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, mediante el uso de la...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 24ad086d-865e-433c-9ac9-05f1a553da16
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/introduction
msc.type: authoredcontent
ms.openlocfilehash: 96dd31d949633e001fc595621bedbf74e98000fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640233"
---
# <a name="aspnet-web-deployment-using-visual-studio-introduction"></a>Implementación web de ASP.NET con Visual Studio: introducción

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o un proveedor de hospedaje de terceros mediante Visual Studio 2012 con el SDK de Azure para .NET. La mayoría de los procedimientos son similares para Visual Studio 2013.
> 
> Desarrolle una aplicación web para que esté disponible para los usuarios a través de Internet. Sin embargo, los tutoriales de programación web normalmente se detienen justo después de que le hayan mostrado cómo trabajar en el equipo de desarrollo. En esta serie de tutoriales comienza el lugar en el que se dejan de usar los otros: ha creado una aplicación Web, la ha probado y está lista para comenzar. Pasos adicionales Estos tutoriales muestran cómo implementar primero en IIS en el equipo de desarrollo local para realizar pruebas y, a continuación, en Azure o en un proveedor de hospedaje de terceros para el ensayo y la producción. La aplicación de ejemplo que va a implementar es un proyecto de aplicación web que utiliza el Entity Framework, SQL Server y el sistema de pertenencia de ASP.NET. La aplicación de ejemplo usa formularios Web Forms de ASP.NET, pero los procedimientos que se muestran también se aplican a ASP.NET MVC y Web API.
> 
> En estos tutoriales se supone que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, un buen punto de partida es un [tutorial básico de formularios Web Forms de ASP.net](../../older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1.md) o un [tutorial básico de MVC de ASP.net](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).
> 
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [implementación de ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) o en [stackoverflow](http://stackoverflow.com).
> 
> Este contenido también está disponible como libro electrónico gratuito en [la galería de libros electrónicos de TechNet](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#ASPNETWebDeploymentusingVisualStudio).

## <a name="overview"></a>Información general del

Estos tutoriales le guían en la implementación de una aplicación Web ASP.NET que incluye bases de datos de SQL Server. Primero implementará en IIS en el equipo de desarrollo local para realizar pruebas y, a continuación, Web Apps en Azure App Service y Azure SQL Database para el almacenamiento provisional y la producción. Verá cómo realizar la implementación mediante la publicación con un solo clic de Visual Studio y verá cómo realizar la implementación mediante la línea de comandos.

El número de tutoriales puede hacer que el proceso de implementación parezca desalentadora. De hecho, los procedimientos básicos son sencillos. Sin embargo, en situaciones reales, a menudo es necesario realizar tareas de implementación adicionales, por ejemplo, establecer permisos de carpeta en el servidor de destino. Hemos mostrado algunas de estas tareas adicionales, en la esperanza de que los tutoriales no dejen de incluir información que pueda impedir la implementación correcta de una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia y cada parte se basa en la parte anterior. Puede omitir partes que no son relevantes para su situación, pero es posible que tenga que ajustar los procedimientos en los tutoriales posteriores.

## <a name="intended-audience"></a>Público previsto

Los tutoriales están dirigidos a desarrolladores de ASP.NET que trabajan en entornos en los que:

- El entorno de producción es Azure App Service Web Apps o un proveedor de hospedaje de terceros.
- La implementación no se limita a un proceso de integración continua, pero se puede realizar directamente desde Visual Studio.

La implementación desde el [control de código fuente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md) mediante un proceso de [entrega continua](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) no se trata en estos tutoriales, excepto en un tutorial que muestra cómo implementar desde la línea de comandos. Para obtener información acerca de la entrega continua, consulte los siguientes recursos:

- [Integración continua y entrega continua (creación de aplicaciones en la nube reales con Windows Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)
- [Implementación de una aplicación web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/)
- [Implementación de aplicaciones web en escenarios empresariales](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md) (un conjunto anterior de tutoriales escritos para Visual Studio 2010, que aún tiene información útil para entornos empresariales).

## <a name="using-a-third-party-hosting-provider"></a>Uso de un proveedor de hospedaje de terceros

Los tutoriales le guiarán a través del proceso de configuración de una cuenta de Azure e implementación de la aplicación en Web Apps en Azure App Service para el almacenamiento provisional y la producción. Sin embargo, puede usar los mismos procedimientos básicos para implementar en un proveedor de hospedaje de terceros de su elección. En los tutoriales que se refieren a los procesos exclusivos de Azure, se explican y se aconsejan las diferencias que se pueden esperar en un proveedor de hospedaje de terceros.

## <a name="deploying-web-app-projects"></a>Implementación de proyectos de aplicación Web

La aplicación de ejemplo que se descarga e implementa para estos tutoriales es un proyecto de aplicación Web de Visual Studio. Sin embargo, si instala la actualización de publicación Web más reciente para Visual Studio, puede usar los mismos métodos y herramientas de implementación para los proyectos de aplicación Web.

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

La aplicación de ejemplo es un proyecto de formularios Web Forms ASP.NET, pero todo lo que aprenda a hacer es aplicable también a ASP.NET MVC. Un proyecto de Visual Studio MVC es simplemente otra forma de proyecto de aplicación Web. La única diferencia es que si va a implementar en un proveedor de hospedaje que no es compatible con ASP.NET MVC o con la versión de destino, debe asegurarse de que ha instalado el paquete NuGet adecuado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0), [MVC 4](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/4.0.30506.0)o [MVC 5](http://www.nuget.org/packages/Microsoft.AspNet.Mvc)) en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo C# utiliza pero los tutoriales no requieren conocimientos de C#, y las técnicas de implementación que se muestran en los tutoriales no son específicas del idioma.

## <a name="database-deployment-methods"></a>Métodos de implementación de base de datos

Hay tres maneras de implementar una base de datos de SQL Server junto con la implementación web en Visual Studio:

- Migraciones de Entity Framework Code First
- Proveedor de Web Deploy de dbDacFx
- Proveedor de Web Deploy de dbFullSql

En este tutorial usará los dos primeros métodos. El proveedor de Web Deploy de dbFullSql es un método heredado que ya no se recomienda, excepto en algunos escenarios específicos, como la migración de SQL Server Compact a SQL Server.

Los métodos que se muestran en este tutorial son para las bases de datos de SQL Server, no SQL Server Compact. Para obtener información sobre cómo implementar una base de datos de SQL Server Compact, vea [implementación web de Visual Studio con SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md).

Los métodos que se muestran en este tutorial requieren que se use el método de publicación Web Deploy. Si prefiere un método de publicación diferente, como FTP, el sistema de archivos o FPSE, vea [implementar una base de datos de forma independiente de la implementación de aplicaciones web](https://go.microsoft.com/fwlink/p/?LinkId=282413#databaseseparate) en el mapa de contenido de implementación web para Visual Studio y ASP.net.

### <a name="entity-framework-code-first-migrations"></a>Migraciones de Entity Framework Code First

En el Entity Framework versión 4,3, Microsoft presentó Migraciones de Code First. Migraciones de Code First automatiza el proceso de realizar cambios incrementales en un modelo de datos y propagar los cambios a la base de datos. En versiones anteriores de Code First, normalmente se permite al Entity Framework quitar y volver a crear la base de datos cada vez que se cambia el modelo de datos. Esto no es un problema en el desarrollo porque los datos de prueba se vuelven a crear fácilmente, pero en producción normalmente desea actualizar el esquema de la base de datos sin quitar la base de datos. La característica migraciones habilita Code First para actualizar la base de datos sin quitarla y volver a crearla. Puede permitir que Code First decida automáticamente cómo realizar los cambios de esquema necesarios, o puede escribir código que Personalice los cambios. Para obtener una introducción a Migraciones de Code First, vea [migraciones de Code First](https://msdn.microsoft.com/library/hh770484.aspx).

Cuando se implementa un proyecto Web, Visual Studio puede automatizar el proceso de implementación de una base de datos administrada por Migraciones de Code First. Al crear el perfil de publicación, se activa una casilla de verificación con la etiqueta ejecutar Migraciones de Code First (se ejecuta al iniciar la aplicación). Esta configuración hace que el proceso de implementación configure automáticamente el archivo Web. config de la aplicación en el servidor de destino para que Code First utilice la clase de inicializador `MigrateDatabaseToLatestVersion`.

Visual Studio no realiza ninguna acción con la base de datos durante el proceso de implementación. Cuando la aplicación implementada tiene acceso a la base de datos por primera vez después de la implementación, Code First crea automáticamente la base de datos o actualiza el esquema de la base de datos a la versión más reciente. Si la aplicación implementa un método de inicialización de migraciones, el método se ejecuta después de que se cree la base de datos o se actualice el esquema.

En este tutorial, usará Migraciones de Code First para implementar la base de datos de la aplicación.

### <a name="the-dbdacfx-web-deploy-provider"></a>Proveedor de Web Deploy de dbDacFx

En el caso de una base de datos de SQL Server que no esté administrada por Entity Framework Code First, puede activar la casilla de verificación actualizar base de datos al configurar el perfil de publicación. Durante la implementación inicial, el proveedor dbDacFx crea tablas y otros objetos de base de datos en la base de datos de destino para que coincidan con la base de datos de origen. En las implementaciones posteriores, el proveedor determina qué es diferente entre las bases de datos de origen y de destino, y actualiza el esquema de la base de datos de destino para que coincida con la base de datos de origen. De forma predeterminada, el proveedor no realizará ningún cambio que provoque la pérdida de datos, como cuando se quita una tabla o una columna.

Este método no automatiza la implementación de los datos en tablas de base de datos, pero puede crear scripts para ello y configurar Visual Studio para ejecutarlos durante la implementación. Otro motivo para ejecutar scripts durante la implementación consiste en realizar cambios de esquema que no se pueden realizar automáticamente porque podrían provocar la pérdida de datos.

En este tutorial, usará el proveedor dbDacFx para implementar la base de datos de pertenencia de ASP.NET.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución obvia. Para ayudarle con algunos escenarios de problemas comunes, hay disponible una [Página de referencia de solución de problemas](troubleshooting.md) . Si recibe un mensaje de error o algo no funciona a medida que avanza por los tutoriales, asegúrese de consultar la página de solución de problemas.

## <a name="comments-welcome"></a>Página de comentarios

Los comentarios sobre los tutoriales son bienvenidos y, cuando se actualice el tutorial, se realizarán todas las acciones necesarias para tener en cuenta las correcciones o sugerencias para las mejoras que se proporcionan en los comentarios del tutorial.

<a id="prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

Este tutorial se ha escrito para los siguientes productos:

- Windows 8 o Windows 7.
- Visual Studio 2012 o Visual Studio 2012 Express for Web con [la actualización más reciente](https://go.microsoft.com/fwlink/?LinkId=272486).
- [SDK de Azure para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=254364)

Puede seguir el tutorial con Visual Studio 2010 SP1 o Visual Studio 2013, pero algunas capturas de pantalla serán diferentes y algunas características serán diferentes.

Si utiliza Visual Studio 2013, instale el [SDK de Azure para Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkID=324322).

Si utiliza Visual Studio 2010 SP1, instale el siguiente software:

- [SDK de Azure para Visual Studio 2010](https://go.microsoft.com/fwlink/?LinkID=254269)
- [SQL Server Express LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=SQLLocalDBOnly_11_0)
- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh500335.aspx).

En función de la cantidad de dependencias del SDK que ya tenga en el equipo, la instalación del SDK de Azure podría tardar mucho tiempo, desde unos minutos a media hora o más. Necesita el SDK de Azure incluso si planea publicar en un proveedor de hospedaje de terceros en lugar de en Azure, ya que el SDK incluye las actualizaciones más recientes de las características de publicación Web de Visual Studio.

> [!NOTE]
> Este tutorial se ha escrito con la versión 1.8.1 del SDK de Azure. Desde entonces, se han lanzado versiones más recientes con características adicionales. Los tutoriales se han actualizado para mencionar estas características y un vínculo a los recursos que tienen más información sobre ellas.

Las instrucciones y las capturas de pantalla se basan en Windows 8, pero en los tutoriales se explican las diferencias de Windows 7.

Se requiere algún otro software para completar el tutorial, pero no es necesario que esté instalado todavía. El tutorial le guiará por los pasos necesarios para instalarlo cuando lo necesite.

## <a name="download-the-sample-application"></a>Descarga de la aplicación de ejemplo

La aplicación que va a implementar se denomina contoso University y ya se ha creado. Es una versión simplificada de un sitio web de la Universidad, en función de la aplicación contoso University que se describe en el [Entity Framework tutoriales del sitio de ASP.net](https://asp.net/entity-framework/tutorials).

Una vez instalados los requisitos previos, descargue la [aplicación web contoso University](https://go.microsoft.com/fwlink/p/?LinkId=282627). El archivo *. zip* contiene varias versiones del proyecto. Para trabajar con los pasos del tutorial, empiece con el proyecto ubicado en la C# carpeta. Para ver el aspecto del proyecto al final de los tutoriales, abra el proyecto en la carpeta ContosoUniversity-end.

Para preparar el proyecto para trabajar con los pasos del tutorial, realice los pasos siguientes:

1. Guarde los archivos de solución de ContosoUniversity C# desde la carpeta en una carpeta denominada ContosoUniversity en la carpeta que use para trabajar con proyectos de Visual Studio.

    De forma predeterminada, esta es la siguiente carpeta para Visual Studio 2012:

    `C:\Users\<username>\Documents\Visual Studio 2012\Projects`

    (En el caso de las capturas de pantalla de este tutorial, la carpeta del proyecto se encuentra en el directorio raíz de la unidad `C`:).
2. Inicie Visual Studio y abra el proyecto.
3. En **Explorador de soluciones**, haga clic con el botón secundario en la solución y haga clic en **restauración de paquetes EnableNuGet**.
4. Compile la solución.
5. Si obtiene errores de compilación, restaure manualmente los paquetes NuGet:

    1. En **Explorador de soluciones**, haga clic con el botón secundario en la solución y, a continuación, haga clic en **administrar paquetes NuGet para la solución**.
    2. En la parte superior del cuadro de diálogo **administrar paquetes Nuget** verá **algunos paquetes Nuget que faltan en esta solución. Haga clic para restaurar.** Haga clic en el botón **restaurar** .
    3. Recompilar la solución.
6. Presione CTRL-F5 para ejecutar la aplicación.

    La aplicación se abre en la Página principal de Contoso University.

    ![Desarrollo de la Página principal](introduction/_static/image1.png)

    (Puede haber un tiempo de espera mientras Visual Studio inicia el SQL Server Express instancia de LocalDB y podría obtener un error de tiempo de espera si el proceso tarda demasiado tiempo. En ese caso, simplemente vuelva a iniciar el proyecto).

Las páginas del sitio web son accesibles desde la barra de menús y permiten realizar las siguientes funciones:

- Mostrar estadísticas de estudiante (la página acerca de).
- Mostrar, editar, eliminar y agregar alumnos.
- Mostrar y editar cursos.
- Mostrar y editar instructores.
- Mostrar y editar departamentos.

A continuación se muestran capturas de pantalla de algunas páginas representativas.

![Página Students dev](introduction/_static/image2.png)

![Agregar página de estudiantes dev](introduction/_static/image3.png)

## <a name="review-application-features-that-affect-deployment"></a>Revisar las características de la aplicación que afectan a la implementación

Las siguientes características de la aplicación afectan al modo en que se implementa o lo que hay que hacer para implementarlo. Cada una de ellas se explica con más detalle en los siguientes tutoriales de la serie.

- Contoso University usa una SQL Server base de datos para almacenar datos de aplicaciones, como los nombres de estudiantes y del instructor. La base de datos contiene una combinación de datos de prueba y datos de producción y, cuando se implementa en producción, es necesario excluir los datos de prueba.
- La aplicación usa el sistema de pertenencia de ASP.NET, que almacena la información de la cuenta de usuario en una base de datos de SQL Server. La aplicación define un usuario administrador que tiene acceso a cierta información restringida. Debe implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- La aplicación utiliza un registro de errores y una utilidad de generación de informes de terceros. Esta utilidad se proporciona en un ensamblado que se debe implementar con la aplicación.
- La utilidad de registro de errores escribe información de error en archivos XML en una carpeta de archivos. Debe asegurarse de que la cuenta con la que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta y que tiene que excluir esta carpeta de la implementación. (De lo contrario, es posible que se eliminen los datos del registro de errores del entorno de prueba en producción o en los archivos de registro de errores de producción).
- La aplicación incluye algunos valores de configuración que se deben cambiar en el archivo *Web. config* implementado, en función del entorno de destino (prueba, ensayo o producción) y de otras opciones que deben cambiarse en función de la configuración de compilación (depuración o lanzamiento).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Solo se debe implementar el ensamblado generado por este proyecto, no el propio proyecto.

## <a name="summary"></a>Resumen

En el primer tutorial de la serie, descargó el proyecto de Visual Studio de ejemplo y revisó las características del sitio que afectan al modo en que se implementa la aplicación. En los tutoriales siguientes, se prepara para la implementación mediante la configuración de algunas de estas cosas para que se controlen automáticamente. Otros usuarios que se encargan manualmente.

> [!div class="step-by-step"]
> [Siguiente](preparing-databases.md)
