---
uid: whitepapers/aspnet-data-access-content-map
title: 'ASP.NET Data Access: recursos recomendados | Microsoft Docs'
author: rick-anderson
description: En este tema se proporcionan vínculos a recursos de documentación sobre cómo obtener acceso a los datos de aplicaciones Web de ASP.NET, principalmente mediante el Entity Framework y SQL se...
ms.author: riande
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 357851f195bf233c7c34a32bd156e4408d3e1b24
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513973"
---
# <a name="aspnet-data-access---recommended-resources"></a>Acceso a los datos de ASP.NET - Recursos recomendados

> En este tema se proporcionan vínculos a recursos de documentación sobre cómo obtener acceso a los datos de aplicaciones Web de ASP.NET, principalmente mediante el Entity Framework y el SQL Server.
> 
> Si conoce una gran entrada de blog, un subproceso de [stackoverflow](http://stackoverflow.com) o cualquier otro vínculo que le resulte útil, [envíenos un correo electrónico](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) con el vínculo.
> 
> Última actualización 4/3/2014

El tema contiene las siguientes secciones:

- [Introducción con acceso a datos en ASP.NET](#gettingstarted)
- [Usar el Entity Framework](#ef)

    - [Usar Entity Framework Code First](#cf)
    - [Usar Migraciones de Entity Framework Code First](#efcfmigrations)
    - [Usar Entity Framework Database First o Model First (el diseñador de EF)](#efdbf)
    - [Cargar datos relacionados en Entity Framework (carga diferida, carga diligente y carga explícita)](#efrelateddata)
    - [Optimizar el rendimiento de Entity Framework](#optimizingef)
    - [Controlar la simultaneidad en una aplicación Entity Framework](#efconcurrency)
    - [Libros sobre el Entity Framework](#efbooks)
    - [Recursos de Entity Framework adicionales](#otherefresources)
- [Enlace de datos en aplicaciones de formularios Web Forms ASP.NET](#wfdatabinding)

    - [Usar el enlace de modelos de formularios Web Forms](#wfmodelbinding)
    - [Usar controles de origen de datos de formularios Web Forms](#wfdsc)
    - [Usar controles enlazados a datos de formularios Web Forms y expresiones de enlace de datos](#wfdbc)
- [Trabajar con bases de datos de SQL Server](#sqlserver)

    - [Trabajar con bases de datos de SQL Server Express LocalDB](#sslocaldb)
    - [Trabajar con bases de datos de SQL Server Express](#sse)
    - [Trabajar con Windows Azure SQL Database](#ssdb)
    - [Elección entre SQL Server y Windows Azure SQL Database](#ssdbchoosing)
- [Trabajar con sistemas de administración de bases de datos NoSQL](#nosql)
- [Usar consultas LINQ en aplicaciones ASP.NET](#linq)
- [Uso de datos dinámicos scaffolding](#dd)
- [Proteger el acceso a los datos](#securing)
- [Optimizar el rendimiento del acceso a datos](#optimizingdataaccess)
- [Implementar una base de datos](#deploying)
- [Acceder a los datos a través de un servicio Web](#webservice)
- [Recursos adicionales](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>Introducción con acceso a datos en ASP.NET

- [Opciones de almacenamiento de datos (creación de aplicaciones en la nube reales con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Capítulo de un libro electrónico sobre el desarrollo para la nube. Presenta las bases de datos NoSQL como alternativa a que muchos desarrolladores familiarizados con las bases de datos relacionales tienden a pasar por alto. Presenta instrucciones sobre lo que se debe considerar al elegir relacional o NoSQL, o elegir una plataforma determinada.
- [ASP.net opciones de acceso a datos](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). Una introducción a las opciones de acceso a datos para las bases de datos relacionales para ASP.NET e instrucciones sobre cómo elegir las plataformas y métodos de acceso adecuados para su escenario.
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). Si no ha trabajado con bases de datos relacionales, consulte esta página para obtener una introducción a la terminología y los conceptos de las bases de datos relacionales. Para obtener una introducción a SQL Server en concreto, vea [trabajar con bases de datos de SQL Server](#sqlserver) más adelante en este tema.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Usar el Entity Framework

- [Enfoques de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Instrucciones sobre cómo elegir un enfoque de desarrollo de Entity Framework Database First, Model First o Code First.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Usar Entity Framework Code First

Los siguientes tutoriales ofrecen aplicaciones de ejemplo que se pueden descargar:

- [Introducción con EF 6 con MVC 5](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Abarca una amplia gama de escenarios de Entity Framework Code First, como migraciones y características de EF 6, como resistencia de conexión, interceptación de comandos y Async. Se trata de una versión actualizada de la [serie EF 5/MVC 4](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). La serie anterior incluye un tutorial sobre el repositorio y patrones de unidad de trabajo que no se incluyen en la nueva serie.
- [Introducción a ASP.NET MVC 5](../mvc/overview/getting-started/introduction/getting-started.md). Cubre una gama más estrecha de escenarios de Entity Framework Code First, pero realiza un trabajo más completo de introducción de las características de MVC.
- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Utiliza Code First en una aplicación de formularios Web Forms.
- [Introducción con formularios Web Forms de ASP.NET 4,5](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Introducción a los formularios Web Forms con cierta cobertura de Code First. Utiliza el enlace de modelos.
- [Almacén de música de MVC](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Utiliza Code First en una aplicación de MVC 3 de comercio electrónico que también implementa la pertenencia y la autorización. La versión de MVC y el sistema de pertenencia a ASP.NET (autenticación y autorización) que se usan aquí están anticuados; para obtener más información actualizada sobre la pertenencia a ASP.NET, consulte [https://asp.net/identity](https://asp.net/identity).

Otros recursos:

- [Entity Framework-Code First a una base de datos existente](https://msdn.microsoft.com/data/jj200620). Sólo. Vídeo y tutorial que muestra cómo usar Code First con una base de datos existente.
- [Centro para desarrolladores de datos: Entity Framework](https://msdn.microsoft.com/data/ef). Sólo. Para obtener una guía de Entity Framework documentación que se ha creado y mantenido por el equipo de Entity Framework, [consulte el vínculo introducción.](https://msdn.microsoft.com/data/ee712907)

Vea también [los libros sobre el Entity Framework](#efbooks) y [recursos de Entity Framework adicionales](#otherefresources) más adelante en este tema.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Usar Migraciones de Entity Framework Code First

La mayoría de los tutoriales Code First enumerados anteriormente cubren las migraciones. Vea también los siguientes recursos.

- [Implementación web de ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie de tutoriales de dos partes que muestra cómo usar Migraciones de Code First para implementar una base de datos.
- [Implemente una aplicación ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Cómo usar las migraciones para implementar datos de pertenencia y de aplicación en Azure.
- [Información general sobre la implementación web para Visual Studio y ASP.net](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Vea la sección configuración de la **implementación de bases de datos en Visual Studio** para obtener una explicación de cómo se integra migraciones de Code First en las características de implementación web de Visual Studio.
- [Centro para desarrolladores de datos: migraciones de Code First](https://msdn.microsoft.com/data/jj591621) (MSDN). La documentación de las migraciones del equipo de Entity Framework.
- [Series screencast de migraciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). Blog de EF). Tres vídeos sobre temas avanzados en Migraciones de Code First.
- [Migraciones de Code First con sitios de ASP.NET Web pages](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Blog de Mikesdotnetting). Muestra cómo usar las migraciones de Code First con un sitio ASP.NET Web Pages colocando el contexto de datos en un proyecto de biblioteca de clases de Visual Studio.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Usar Entity Framework Database First o Model First (el diseñador de EF)

- [Introducción con Entity Framework 6 Database First con MVC 5](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Ejecute un script en Explorador de servidores para crear una base de datos y, a continuación, use el diseñador de Entity Framework para crear el modelo de datos. Muestra cómo crear páginas web CRUD sencillas y para otras funciones de control de datos, puede seguir uno de los tutoriales Code First, ya que todos los flujos de trabajo EF usan la misma API DbContext.

Los siguientes recursos son más antiguos. Resultan útiles si desea utilizar la versión 4,0 del Entity Framework y desea utilizar un control de origen de datos para el enlace de datos en una aplicación de formularios Web Forms.

- [Introducción con el Entity Framework 4,0](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Muestra cómo usar el control **EntityDataSource** .
- [Continuar con el Entity Framework](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(muestra cómo usar el control **ObjectDataSource** . Incluye un tutorial sobre el control de simultaneidad, un tutorial sobre el rendimiento de EF y un tutorial sobre las novedades de EF 4,0.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>Control de los datos relacionados en Entity Framework (carga diferida, carga diligente y carga explícita)

- [Leer los datos relacionados con el Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, aplicación de ejemplo MVC. Los métodos que se muestran se aplican también al enlace de modelos de formularios Web Forms y al flujo de trabajo Database First.
- [Centro para desarrolladores de datos: cargar entidades relacionadas](https://msdn.microsoft.com/data/jj574232) (MSDN). La documentación del equipo de Entity Framework acerca de la carga de datos relacionados.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Optimizar el rendimiento de Entity Framework

- [Escenarios de Entity Framework avanzados para una aplicación ASP.net](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Muestra cómo ejecutar sus propias instrucciones SQL o llamar a sus propios procedimientos almacenados, cómo deshabilitar la detección de cambios y cómo deshabilitar la validación al guardar los cambios.
- [Consideraciones de rendimiento para Entity Framework 5](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Consideraciones de rendimiento (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Maximizar el rendimiento con el Entity Framework en una aplicación Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4,0.
- Vea también [optimizar el acceso a datos ASP.net](#optimizingdataaccess) más adelante en este tema.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Controlar la simultaneidad en una aplicación Entity Framework

- [Controlar la simultaneidad con el Entity Framework en una aplicación ASP.NET MVC](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). Code First, API DbContext, uso de una aplicación de ejemplo MVC.
- [Centro para desarrolladores de datos: patrones de simultaneidad optimista](https://msdn.microsoft.com/data/jj592904) (MSDN). La documentación de simultaneidad del equipo de Entity Framework.
- [Controlar la simultaneidad con el Entity Framework en una aplicación Web ASP.net](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Se aplica a Entity Framework 4,0. Database First, API de ObjectContext, uso de una aplicación de ejemplo de formularios Web Forms.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Libros sobre el Entity Framework

- [Entity Framework de programación: DbContext](http://shop.oreilly.com/product/0636920022237.do) de Julie Lerman y Rowan Miller.
- [Programación Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) de Julie Lerman y Rowan Miller.

Ambos libros están actualizados con las técnicas recomendadas actuales. Proporcionan una introducción más completa y fácil de seguir para el Entity Framework que cualquier cosa disponible en Internet. Otro libro, [programming Entity Framework](http://shop.oreilly.com/product/9780596807252.do) by Julia Lerman, es más grande y más completo, pero es más antiguo y muchas de las técnicas que cubre ya no son la forma recomendada de usar el Entity Framework. Vea también la lista de libros que recomienda el equipo de Entity Framework en el [Centro para desarrolladores de datos, libros](https://msdn.microsoft.com/data/aa937716) en el sitio de MSDN.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Otros recursos de Entity Framework

- [Blog del equipo de Entity Framework (ADO.net)](https://blogs.msdn.com/b/adonet/). Uno de los mejores recursos para la información más reciente y anuncios de nuevas mejoras. Para ver otros blogs relacionados con EF, consulte blogroll en [Introducción a Entity Framework](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Vea la columna **puntos de datos** , que suele tratarse de temas relacionados con el Entity Framework.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Enlace de datos en aplicaciones de formularios Web Forms ASP.NET

- [ASP.net opciones de acceso a datos de formularios Web Forms](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Usar el enlace de modelos de formularios Web Forms

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales con EF Code First.
- [Enlace de modelos de formularios Web Forms, parte 1: seleccionar datos](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie). En estas entradas de blog anteriores, la propiedad que se llama actualmente a ItemType se denomina ModelType, pero, de lo contrario, la información que contienen es válida.
- [Enlace de modelos de formularios Web Forms, parte 2: filtrar datos](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Enlace de modelos de formularios Web Forms, parte 3: actualización y validación](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (blog de Scott Guthrie).
- [ASP.NET 4,5 enlace de modelos de formularios Web Forms](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (vídeo).
- [Enlace de modelos, parte 1: selección de datos](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (vídeo).
- [Enlace de modelos, parte 2: filtrado](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (vídeo).
- [Introducción con formularios Web Forms de ASP.NET 4,5: Mostrar elementos de datos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Usar controles de origen de datos de formularios Web Forms

- [Controles de servidor Web de origen de datos](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Anuncio del lanzamiento de datos dinámicos proveedor y el control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (blog de desarrollo web de Microsoft).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Usar controles enlazados a datos de formularios Web Forms y expresiones de enlace de datos

- [Enlace de modelos y formularios Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Serie de tutoriales que usa EF Code First.
- [Introducción con formularios Web Forms de ASP.NET 4,5: Mostrar elementos de datos y detalles](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Controles de datos fuertemente tipados](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (blog de Scott Guthrie).
- [Controles de datos fuertemente tipados](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (vídeo).
- [ASP.NET 4,5 formularios Web Forms fuertemente](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) tipados controles de datos (vídeo).
- [Controles de servidor Web enlazados a datos](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Información general sobre las expresiones de enlace de datos](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Esta página solo cubre **eval** y **BIND**; no se ha actualizado para incluir **Item** y **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>Trabajar con bases de datos de SQL Server

- [SQL Server Database Features](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Para obtener una introducción general a una gran variedad de temas de SQL Server, vea las entradas que hay debajo de este en la TDC.
- [Ediciones de SQL Server](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Un resumen de las ediciones de SQL Server disponibles, con vínculos a más información sobre cada una de ellas.
- [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Usar SQL Server Compact para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: ejemplos de productos de base de datos](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Bases de datos de ejemplo de AdventureWorks.
- [Instalar bases de datos de ejemplo](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Además de los métodos que se muestran aquí, también puede descargar uno de los archivos. MDF de ejemplo en la carpeta app\_data de un proyecto Web, convertir la base de datos a LocalDB y crear una cadena de conexión de LocalDB. Para obtener información sobre cómo hacerlo, consulte [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx).

Vea también las siguientes secciones sobre cómo trabajar con SQL Server Express y LocalDB y cómo elegir entre SQL Server y SQL Database.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>Trabajar con bases de datos de SQL Server Express LocalDB

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). La introducción oficial a MSDN para LocalDB.
- [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Cómo: actualizar a LocalDB](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). Cómo migrar un archivo. MDF de una versión anterior de SQL Server Express a LocalDB. También debe seguir este proceso si descarga una de las bases de datos de [ejemplo de SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=117483).
- [Introducción a LocalDB, un blog mejorado de SQL Express](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express). Tiene más información sobre el motivo por el que se creó LocalDB que se incluye en MSDN.
- [LocalDB: ¿Dónde está mi base de datos?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express blog). Información sobre dónde se crean los archivos de base de datos de LocalDB.
- [Usar LocalDB con IIS completo, parte 1: Perfil de usuario](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express blog). LocalDB no está diseñado para funcionar con IIS. En esta serie de entradas de blog se explican los problemas y algunas soluciones.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>Trabajar con bases de datos de SQL Server Express

- [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). Si usa la configuración de la cadena de conexión AttachDBFileName con SQL Server Express, vea especialmente la sección instancia de usuario de esta página.
- [Cómo tomar posesión del SQL Server Express local 2008](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (blog de SQL Server Express). Un problema común no es poder trabajar con las bases de datos de SQL Server Express porque no es administrador en la instancia de SQL Server Express. De forma predeterminada, solo la persona que instaló SQL Server Express es un administrador. En este blog se explica cómo hacerlo como administrador de SQL Server Express si es un administrador del equipo.
- [¿Puede mi aplicación Web de ASP.NET usar una base de datos SQL Server Express en producción?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Trabajar con Windows Azure SQL Database

- [Implemente una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en un sitio web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (sitio Microsoft Azure).
- [SQL Database](https://docs.microsoft.com/azure/sql-database/) (sitio Microsoft Azure). Tutoriales de introducción y guías de procedimientos.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). El nodo de nivel superior de la tabla de contenido para SQL Database en MSDN.
- [Índice de artículos de Windows Azure SQL Database TechNet wiki](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (sitio de Microsoft TechNet).
- [Bloque de aplicaciones de control de errores transitorios](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Marco que le permite controlar los errores transitorios de la red y los errores de conexión resultantes de la limitación. Disponible en un paquete NuGet: [Enterprise Library 5,0-Transient Fault Handling Application Block](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [Introducción con SQL Database y Entity Framework](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Kit de aprendizaje de Windows Azure](https://www.microsoft.com/download/details.aspx?id=8396) (centro de descarga de Microsoft). Incluye laboratorios prácticos para SQL Database.
- [Foro de la comunidad de Windows Azure SQL Database](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Pasar a Windows Azure SQL Database](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Un capítulo de un escenario completo de un extremo a otro del equipo de patrones y prácticas de Microsoft. Explica por qué podría querer migrar y cómo migrar de SQL Server a SQL Database.
- [Migrar bases de datos de SQL Server a Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL Database el Asistente para migración](http://sqlazuremw.codeplex.com/). Herramienta de código abierto para migrar bases de datos a y desde SQL Database.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>Elección entre SQL Server y Windows Azure SQL Database

- [Comparar SQL Server con Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (sitio de Microsoft TechNet).
- [Migración de datos a Windows Azure SQL Database: herramientas y técnicas](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). Incluye secciones que comparan SQL Server a SQL Database y proporcionan instrucciones sobre cuándo migrar de SQL Server a SQL Database.
- [Guía de entrega de Windows Azure SQL Database](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (sitio de Microsoft TechNet).
- [Limitaciones de las características de SQL Server (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure Table Storage y windows Azure SQL Database: comparación y](https://msdn.microsoft.com/library/jj553018.aspx) diferencias (MSDN). En el caso de una aplicación que se implementa en Windows Azure, el almacenamiento de tablas de Windows Azure puede ser una alternativa a Windows Azure SQL Database. Este tema le ayuda a decidir entre estas alternativas.
- [Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Instrucciones y limitaciones (Windows Azure SQL Database)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>Trabajar con sistemas de administración de bases de datos NoSQL

- [Data Services de Windows Azure](https://www.windowsazure.com/develop/net/data/) (sitio Microsoft Azure). Consulte la [Guía de características de Table Service](https://docs.microsoft.com/azure/) y la sección **macrodatos** de la página.
- [ASP.net la aplicación de niveles múltiples con tablas, colas y blobs de almacenamiento](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio Microsoft Azure). Tutorial de un extremo a otro con una aplicación de ejemplo descargable que usa tablas NoSQL de almacenamiento de Windows Azure.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>Usar consultas LINQ en aplicaciones ASP.NET

- [ASP.net opciones de acceso a datos](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Incluye una introducción a LINQ.
- [Vídeos de aprendizaje de LINQ](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (blog de Joe Stagner).
- [Subproceso del Foro de ASP.net con vínculos a recursos dinámicos de LINQ](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Uso de datos dinámicos scaffolding

- [Plantillas de proyecto de datos dinámicos](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Instrucciones sobre Cuándo usar proyectos de datos dinámicos.
- [Datos dinámicos ASP.net](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Proteger el acceso a los datos

- [Proteger el acceso a los datos en ASP.net](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Consideraciones de seguridad (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Cómo: proteger cadenas de conexión cuando se usan controles de origen de datos](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Optimizar el rendimiento del acceso a datos

- [Información general sobre el rendimiento de ASP.net](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [Caching de ASP.net](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [Mejorar el rendimiento de ASP.net](https://msdn.microsoft.com/library/ff647787) (MSDN). Hay una advertencia de "contenido retirado" en la parte superior de esta página, pero la mayor parte de la información sigue siendo relevante y no hay ningún recurso actualizado comparable.
- [Mejorar el rendimiento de SQL Server](https://msdn.microsoft.com/library/ff647793) (MSDN). El mismo comentario que el vínculo anterior.

Vea también [optimizar el rendimiento de Entity Framework](#optimizingef) anteriormente en este tema.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Implementar una base de datos

- [ASP.net web Deployment: recursos recomendados](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Acceder a los datos a través de un servicio Web

- [Obtener acceso a los datos a través de un servicio Web](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Instrucciones sobre Cuándo usar Web API frente a WCF.
- [Introducción con ASP.net web API](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Preguntas más frecuentes sobre el acceso a datos ASP.net](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [Tutoriales de formularios Web Forms de ASP.net: datos](../web-forms/overview/data-access/index.md). La mayoría de estos tutoriales son relativamente antiguos; Asegúrese de leer [Opciones de acceso a datos de ASP.net](https://msdn.microsoft.com/library/ms178359.aspx) y opciones de almacenamiento de [datos (creación de aplicaciones en la nube reales con Windows Azure)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) para no llegar demasiado lejos a un método de acceso a datos que no sea adecuado para su escenario.
- [Mapa de contenido de ASP.NET MVC](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web pages tutoriales: datos](../web-pages/overview/data/index.md).
- [Obtener acceso a datos en Visual Studio](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Proporciona una lista de vínculos similar a este mapa de contenido, pero con el foco en Visual Studio en lugar de ASP.NET.
