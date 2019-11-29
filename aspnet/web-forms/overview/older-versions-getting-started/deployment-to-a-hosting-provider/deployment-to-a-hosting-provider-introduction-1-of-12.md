---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción 1 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a2d7f33b-8c4a-4b48-9fb1-9139cf9b9878
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12
msc.type: authoredcontent
ms.openlocfilehash: ea88da1e6d510f706fc7ca370cfa32974c1243f8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587745"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-introduction---1-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio: Introducción 1 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web.
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).
> 
> Estos tutoriales le guían en la implementación de primero en IIS en el equipo de desarrollo local para realizar pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia a ASP.NET. Puede empezar a usar SQL Server Compact e implementar en SQL Server Compact, y en los tutoriales posteriores se muestra cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.
> 
> En los tutoriales se supone que sabe cómo trabajar con ASP.NET en Visual Studio. Si no lo hace, un buen punto de partida es un [tutorial básico de formularios Web Forms de ASP.net](../tailspin-spyworks/tailspin-spyworks-part-1.md) o un [tutorial básico de MVC de ASP.net](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md).
> 
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [implementación de ASP.net](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment).

## <a name="overview"></a>Información general del

Estos tutoriales le guían en la implementación de primero en IIS en el equipo de desarrollo local para realizar pruebas y, a continuación, en un proveedor de hospedaje de terceros. La aplicación que va a implementar utiliza una base de datos de aplicación y una base de datos de pertenencia a ASP.NET. Puede empezar a usar SQL Server Compact e implementar en SQL Server Compact, y en los tutoriales posteriores se muestra cómo implementar los cambios de la base de datos y cómo migrar a SQL Server.

El número de tutoriales – 11 en total más una página de solución de problemas: puede hacer que el proceso de implementación parezca desalentadora. De hecho, los procedimientos básicos para implementar un sitio constituyen una parte relativamente pequeña del conjunto de tutoriales. Sin embargo, en situaciones reales, a menudo se necesita información sobre algunos aspectos de implementación pequeños pero importantes, por ejemplo, la configuración de permisos de carpeta en el servidor de destino. Hemos incluido muchas de estas técnicas adicionales en los tutoriales, con la esperanza de que los tutoriales no dejen de incluir información que pueda impedirle implementar correctamente una aplicación real.

Los tutoriales están diseñados para ejecutarse en secuencia y cada parte se basa en la parte anterior. Sin embargo, puede omitir partes que no sean relevantes para su situación. (Si se omiten las partes, es posible que sea necesario ajustar los procedimientos en los tutoriales posteriores).

## <a name="intended-audience"></a>Público al que está dirigido

Los tutoriales están dirigidos a desarrolladores de ASP.NET que trabajan en pequeñas organizaciones u otros entornos en los que:

- No se usa un proceso de integración continua (compilaciones automatizadas e implementación).
- El entorno de producción es un proveedor de hospedaje de terceros.
- Normalmente, una persona rellena varios roles (la misma persona desarrolla, prueba e implementa).

En entornos empresariales, es más habitual implementar procesos de integración continua y el entorno de producción se hospeda normalmente en los propios servidores de la empresa. Otras personas también suelen realizar diferentes roles. Para obtener información acerca de la implementación empresarial, consulte [implementar aplicaciones web en escenarios empresariales](../../deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).

Las organizaciones de todos los tamaños también pueden implementar aplicaciones web en Azure, y la mayoría de los procedimientos que se muestran en estos tutoriales también se aplican a App de Azure Services Web Apps. Para ver una introducción a Azure, consulte [https://azure.microsoft.com](https://azure.microsoft.com).

## <a name="the-hosting-provider-shown-in-the-tutorials"></a>El proveedor de hospedaje que se muestra en los tutoriales

Los tutoriales le guiarán a través del proceso de configuración de una cuenta con una empresa de hospedaje e implementación de la aplicación en ese proveedor de hospedaje. Se eligió una empresa de hospedaje específica para que los tutoriales puedan ilustrar la experiencia completa de implementación en un sitio Web activo. Cada empresa de hospedaje proporciona diferentes características y la experiencia de implementación en sus servidores varía en cierto modo. sin embargo, el proceso descrito en este tutorial es típico para el proceso general.

El proveedor de hospedaje que se usa para este tutorial, Cytanium.com, es uno de los muchos que están disponibles y su uso en este tutorial no constituye una aprobación o recomendación.

## <a name="deploying-web-site-projects"></a>Implementar proyectos de sitios web

Contoso University es un proyecto de aplicación Web de Visual Studio. La mayoría de los métodos y herramientas de implementación que se muestran en este tutorial no se aplican a los [proyectos de sitios web](https://msdn.microsoft.com/library/dd547590.aspx). Para obtener información sobre cómo implementar proyectos de sitios web, vea [mapa de contenido de implementación de ASP.net](https://msdn.microsoft.com/library/bb386521.aspx#deployment_for_web_site_projects).

## <a name="deploying-aspnet-mvc-projects"></a>Implementación de proyectos de ASP.NET MVC

En este tutorial implementará un proyecto de formularios Web Forms ASP.NET, pero todo lo que aprenda a hacer es aplicable también a ASP.NET MVC. Un proyecto de Visual Studio MVC es simplemente otra forma de proyecto de aplicación Web. La única diferencia es que si está implementando en un proveedor de hospedaje que no es compatible con ASP.NET MVC o con la versión de destino, debe asegurarse de que ha instalado el paquete NuGet adecuado ([MVC 3](http://nuget.org/packages/AspNetMvc/3.0.20105.0) o [MVC 4](http://nuget.org/packages/aspnetmvc)) en el proyecto.

## <a name="programming-language"></a>Lenguaje de programación

La aplicación de ejemplo C# utiliza pero los tutoriales no requieren conocimientos de C#, y las técnicas de implementación que se muestran en los tutoriales no son específicas del idioma.

## <a name="troubleshooting-during-this-tutorial"></a>Solución de problemas durante este tutorial

Cuando se produce un error durante la implementación, o si el sitio implementado no se ejecuta correctamente, los mensajes de error no siempre proporcionan una solución. Para ayudarle con algunos escenarios de problemas comunes, hay disponible una [Página de referencia de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md) . Si recibe un mensaje de error o algo no funciona a medida que avanza por los tutoriales, asegúrese de consultar la página de solución de problemas.

## <a name="comments-welcome"></a>Página de comentarios

Los comentarios sobre los tutoriales son bienvenidos y, cuando se actualice el tutorial, se realizarán todas las acciones necesarias para tener en cuenta las correcciones o sugerencias para las mejoras que se proporcionan en los comentarios del tutorial.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene Windows 7 o posterior y uno de los siguientes productos instalados en el equipo:

- [Visual Studio 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
- [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VWD2010SP1Pack)
- [Visual Studio 2012 RC o Visual Studio Express 2012 RC for Web](https://go.microsoft.com/fwlink/?LinkId=240162)

Si tiene Visual Studio 2010 SP1 o Visual Web Developer Express 2010 SP1, instale los siguientes productos también:

- [SDK de Azure para .net (VS 2010 SP1)](https://go.microsoft.com/fwlink/?LinkID=208120) (incluye la actualización de publicación web)
- [Microsoft Visual Studio 2010 SP1 Tools para SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCEVSTools)

Se requiere algún otro software para completar el tutorial, pero no es necesario que se cargue todavía. El tutorial le guiará por los pasos necesarios para instalarlo cuando lo necesite.

## <a name="downloading-the-sample-application"></a>Descarga de la aplicación de ejemplo

La aplicación que va a implementar se denomina contoso University y ya se ha creado. Es una versión simplificada de un sitio web de la Universidad, en función de la aplicación contoso University que se describe en el [Entity Framework tutoriales del sitio de ASP.net](https://asp.net/entity-framework/tutorials).

Una vez instalados los requisitos previos, descargue la [aplicación web contoso University](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b). El archivo *. zip* contiene varias versiones del proyecto y un archivo PDF que contiene los 12 tutoriales. Para trabajar con los pasos del tutorial, empiece por ContosoUniversity-Begin. Para ver el aspecto del proyecto al final de los tutoriales, abra ContosoUniversity-end. Para ver el aspecto del proyecto antes de la migración a SQL Server completo en el tutorial 10, abra ContosoUniversity-AfterTutorial09.

Para prepararse para trabajar con los pasos del tutorial, guarde ContosoUniversity-Begin en cualquier carpeta que use para trabajar con proyectos de Visual Studio. De forma predeterminada, esta es la siguiente carpeta:

`C:\Users\<username>\Documents\Visual Studio 2012\Projects`

(En el caso de las capturas de pantalla de este tutorial, la carpeta del proyecto se encuentra en el directorio raíz de la unidad `C`:).

Inicie Visual Studio, abra el proyecto y presione CTRL-F5 para ejecutarlo.

[![Home_page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image2.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image1.png)

Las páginas del sitio web son accesibles desde la barra de menús y permiten realizar las siguientes funciones:

- Mostrar estadísticas de estudiante (la página acerca de).
- Mostrar, editar, eliminar y agregar alumnos.
- Mostrar y editar cursos.
- Mostrar y editar instructores.
- Mostrar y editar departamentos.

A continuación se muestran capturas de pantalla de algunas páginas representativas.

[![Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image4.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image3.png)

[![Add_Students_Page](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image6.png)](deployment-to-a-hosting-provider-introduction-1-of-12/_static/image5.png)

## <a name="reviewing-application-features-that-affect-deployment"></a>Revisar las características de la aplicación que afectan a la implementación

Las siguientes características de la aplicación afectan al modo en que se implementa o lo que hay que hacer para implementarlo. Cada una de ellas se explica con más detalle en los siguientes tutoriales de la serie.

- Contoso University usa una SQL Server Compact base de datos para almacenar datos de aplicaciones, como los nombres de estudiantes y del instructor. La base de datos contiene una combinación de datos de prueba y datos de producción y, cuando se implementa en producción, es necesario excluir los datos de prueba. Más adelante en la serie de tutoriales, migrará de SQL Server Compact a SQL Server.
- La aplicación usa el sistema de pertenencia de ASP.NET, que almacena la información de la cuenta de usuario en una base de datos de SQL Server Compact. La aplicación define un usuario administrador que tiene acceso a cierta información restringida. Debe implementar la base de datos de pertenencia sin cuentas de prueba, pero con una cuenta de administrador.
- Dado que la base de datos de la aplicación y la base de datos de pertenencia usan SQL Server Compact como el motor de base de datos, tiene que implementar el motor de base de datos en el proveedor de hospedaje, así como en las propias bases de datos.
- La aplicación usa proveedores de pertenencia universal de ASP.NET para que el sistema de pertenencia pueda almacenar sus datos en una base de datos de SQL Server Compact. El ensamblado que contiene los proveedores de pertenencia universal debe implementarse con la aplicación.
- La aplicación utiliza el Entity Framework 5,0 para tener acceso a los datos de la base de datos de la aplicación. El ensamblado que contiene Entity Framework 5,0 debe implementarse con la aplicación.
- La aplicación utiliza un registro de errores y una utilidad de generación de informes de terceros. Esta utilidad se proporciona en un ensamblado que se debe implementar con la aplicación.
- La utilidad de registro de errores escribe información de error en archivos XML en una carpeta de archivos. Debe asegurarse de que la cuenta con la que ASP.NET se ejecuta en el sitio implementado tiene permiso de escritura en esta carpeta y que tiene que excluir esta carpeta de la implementación. (De lo contrario, es posible que se eliminen los datos del registro de errores del entorno de prueba en producción o en los archivos de registro de errores de producción).
- La aplicación incluye algunos valores de configuración que se deben cambiar en el archivo *Web. config* implementado, en función del entorno de destino (prueba o producción) y de otros valores que deben cambiarse en función de la configuración de compilación (depuración o lanzamiento).
- La solución de Visual Studio incluye un proyecto de biblioteca de clases. Solo se debe implementar el ensamblado generado por este proyecto, no el propio proyecto.

En el primer tutorial de la serie, descargó el proyecto de Visual Studio de ejemplo y revisó las características del sitio que afectan al modo en que se implementa la aplicación. En los tutoriales siguientes, se prepara para la implementación mediante la configuración de algunas de estas cosas para que se controlen automáticamente. Otros usuarios que se encargan manualmente.

> [!div class="step-by-step"]
> [Siguiente](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
