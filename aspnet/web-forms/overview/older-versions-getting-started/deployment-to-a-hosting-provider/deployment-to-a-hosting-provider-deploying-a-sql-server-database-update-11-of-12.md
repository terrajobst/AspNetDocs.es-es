---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una base de datos de SQL Server Update-11 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78423979"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una base de datos de SQL Server Update-11 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo implementar en sitios web de Windows Azure, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo implementar una actualización de base de datos en una base de datos de SQL Server completa. Dado que Migraciones de Code First realiza todo el trabajo de actualización de la base de datos, el proceso es casi idéntico al que hizo para SQL Server Compact en el tutorial [implementación de una actualización de base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Agregar una nueva columna a una tabla

En esta sección del tutorial, realizará un cambio en la base de datos y los cambios de código correspondientes, y los probará en Visual Studio como preparación para implementarlos en los entornos de prueba y producción. El cambio implica agregar una columna `OfficeHours` a la entidad `Instructor` y mostrar la nueva información en la Página Web de **instructores** .

En el proyecto ContosoUniversity. DAL, Abra *instructor.CS* y agregue la siguiente propiedad entre las propiedades `HireDate` y `Courses`:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Actualice la clase de inicializador para que inicialice la nueva columna con datos de prueba. Abra *Migrations\Configuration.CS* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` por el siguiente bloque de código, que incluye la nueva columna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

En el proyecto ContosoUniversity, Abra *instructors. aspx* y agregue un nuevo campo de plantilla para Office hours justo antes de la etiqueta de cierre `</Columns>` en el primer control `GridView`:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compile la solución.

Abra la ventana de la **consola del administrador de paquetes** y seleccione CONTOSOUNIVERSITY. Dal como **proyecto predeterminado**.

Escriba los siguientes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Ejecute la aplicación y seleccione la página **Instructors** . La página tarda un poco más de lo habitual en cargarse, ya que la Entity Framework vuelve a crear la base de datos y la inicializa con datos de prueba.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implementar la actualización de la base de datos en el entorno de prueba

Cuando se utiliza Migraciones de Code First, el método para implementar un cambio en la base de datos en SQL Server es el mismo que para SQL Server Compact. Sin embargo, tiene que cambiar el perfil de publicación de prueba porque todavía está configurado para migrar de SQL Server Compact a SQL Server.

El primer paso consiste en quitar las transformaciones de la cadena de conexión que creó en el tutorial anterior. Ya no son necesarias porque especificará transformaciones de cadena de conexión en el perfil de publicación, como hizo antes de configurar la pestaña **empaquetar/publicar SQL** para migrar a SQL Server.

Abra el archivo *Web. test. config* y quite el elemento `connectionStrings`. La única transformación restante en el archivo *Web. test. config* es para el valor `Environment` del elemento `appSettings`.

Ahora puede actualizar el perfil de publicación y publicarlo en el entorno de prueba.

Abra el Asistente para **publicación web** y, a continuación, cambie a la pestaña **perfil** .

Seleccione el perfil de publicación de **prueba** .

Seleccione la pestaña **Configuración**.

Haga clic en **habilitar las nuevas mejoras de publicación de bases de datos**.

En el cuadro cadena de conexión de **SchoolContext**, escriba el mismo valor que usó en el archivo de transformación *Web. test. config* en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** . (En su versión de Visual Studio, la casilla podría tener la etiqueta **aplicar migraciones de Code First**).

En el cuadro cadena de conexión de **DefaultConnection**, escriba el mismo valor que usó en el archivo de transformación *Web. test. config* en el tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deje la **base de datos de actualización** desactivada.

Haga clic en **Publicar**.

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador en la Página principal de Contoso University.

Seleccione la página instructors.

Cuando la aplicación ejecuta esta página, intenta tener acceso a la base de datos. Migraciones de Code First comprueba si la base de datos está actualizada y detecta que aún no se ha aplicado la última migración. Migraciones de Code First aplica la última migración, ejecuta el método `Seed` y, a continuación, la página se ejecuta con normalidad. Verá la nueva columna horas de oficina con los datos inicializados.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implementación de la actualización de la base de datos en el entorno de producción

También tiene que cambiar el perfil de publicación para el entorno de producción. En este caso, quitará el perfil existente y creará uno nuevo mediante la importación de un archivo. publishsettings actualizado. El archivo actualizado incluirá la cadena de conexión para el SQL Server base de datos en Cytanium.

Como vio al implementar en el entorno de prueba, ya no necesita transformaciones de cadena de conexión en el archivo de transformación *Web. Production. config* . Abra el archivo y quite el elemento `connectionStrings`. Las transformaciones restantes son para el valor `Environment` del elemento `appSettings` y el elemento `location` que restringe el acceso a los informes de errores Elmah.

Antes de crear un nuevo perfil de publicación para producción, descargue un archivo. publishsettings actualizado de la misma manera que en el tutorial [implementación del entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . (En el panel de control de Cytanium, haga clic en **sitios web**y, a continuación, haga clic en el sitio web de **contosouniversity.com** . Seleccione la pestaña **publicación web** y, a continuación, haga clic en **Descargar Perfil de publicación para este sitio web**). La razón por la que se hace es recoger la cadena de conexión de base de datos en el archivo. publishsettings. La cadena de conexión no estaba disponible la primera vez que se descargó el archivo, porque todavía se usaba SQL Server Compact y no se creó la base de datos de SQL Server en Cytanium todavía.

Ahora puede actualizar el perfil de publicación y publicarlo en el entorno de producción.

Abra el Asistente para **publicación web** y, a continuación, cambie a la pestaña **perfil** .

Haga clic en **administrar perfiles**y, a continuación, elimine el perfil de producción.

Cierre el Asistente para **publicación web** para guardar este cambio.

Vuelva a abrir el Asistente para **publicación web** y, a continuación, haga clic en **importar**.

En la pestaña **conexión** , cambie la **dirección URL de destino** al valor adecuado si usa una dirección URL temporal.

Haga clic en **Siguiente**.

En la pestaña **configuración** , haga clic en **habilitar las nuevas mejoras de publicación de bases de datos**.

En la lista desplegable cadena de conexión de **SchoolContext**, seleccione la cadena de conexión Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Seleccione **Ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** .

En la lista desplegable cadena de conexión de **DefaultConnection**, seleccione la cadena de conexión Cytanium.

Seleccione la pestaña **perfil** , haga clic en **administrar perfiles**y cambie el nombre del perfil de "contosouniversity.com-web deploy" a "producción".

Cierre el perfil de publicación para guardar el cambio y vuelva a abrirlo.

Haga clic en **Publicar**. (Para un sitio web de producción real, debe copiar *app\_offline. htm* en producción y colocarlo en la carpeta del proyecto antes de la publicación y, a continuación, quitarlo cuando se complete la implementación).

Visual Studio implementa los cambios de código en el entorno de prueba y abre el explorador en la Página principal de Contoso University.

Seleccione la página instructors.

Migraciones de Code First actualiza la base de datos de la misma manera que en el entorno de prueba. Verá la nueva columna horas de oficina con los datos inicializados.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Ha implementado correctamente una actualización de la aplicación que incluía un cambio en la base de datos mediante una base de datos de SQL Server.

## <a name="more-information"></a>Más información

Esto completa esta serie de tutoriales sobre la implementación de una aplicación Web de ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información acerca de cualquiera de los temas descritos en estos tutoriales, consulte el [mapa de contenido de implementación de ASP.net](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) en el sitio web de MSDN.

## <a name="acknowledgements"></a>Reconocimientos

Me gustaría agradecer a las siguientes personas que realizaran importantes contribuciones al contenido de esta serie de tutoriales:

- [Alberto poblacion, MVP &amp; MCT, España](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de desarrollo de plataforma de datos, Estados Unidos
- Mittal duras, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
