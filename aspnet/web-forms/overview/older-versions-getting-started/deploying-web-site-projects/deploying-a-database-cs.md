---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Implementar una base de datosC#() | Microsoft Docs
author: rick-anderson
description: La implementación de una aplicación Web ASP.NET implica obtener los archivos y recursos necesarios del entorno de desarrollo en el entorno de producción. Para da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 83657be794e1ea31f6ad2f2b4adc274724d60cf2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518323"
---
# <a name="deploying-a-database-c"></a>Implementar una base de datos (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> La implementación de una aplicación Web ASP.NET implica obtener los archivos y recursos necesarios del entorno de desarrollo en el entorno de producción. En el caso de las aplicaciones web controladas por datos, esto incluye el esquema y los datos de la base de datos. Este tutorial es el primero de una serie en el que se exploran los pasos necesarios para implementar correctamente la base de datos desde el entorno de desarrollo en producción.

## <a name="introduction"></a>Introducción

La implementación de una aplicación Web ASP.NET implica obtener los archivos y recursos necesarios del entorno de desarrollo en el entorno de producción. En el transcurso de los seis últimos tutoriales, hemos examinado la implementación de una aplicación Web de reseñas de un libro sencillo. Este sitio de demostración se compone de varios recursos del lado servidor: páginas ASP.NET, archivos de configuración, un archivo `Web.sitemap`, etc., junto con recursos del lado cliente, como imágenes y archivos CSS. Pero ¿qué ocurre con las aplicaciones web controladas por datos? ¿Qué pasos adicionales se deben realizar para implementar una aplicación web que use una base de datos?

En los siguientes tutoriales, abordaremos los pasos necesarios para implementar una aplicación web controlada por datos. Este tutorial comienza examinando cómo obtener el esquema y el contenido de la base de datos desde el entorno de desarrollo hasta el entorno de producción, mientras que en el tutorial siguiente se examinan los cambios de configuración necesarios. A continuación se explican los desafíos de la implementación de una base de datos que utiliza el Servicios de aplicación (pertenencia, roles, perfil, etc.).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examen de la aplicación Web de revisiones actualizadas del libro

Con el fin de demostrar la implementación de una aplicación web controlada por datos, he actualizado el libro de la aplicación web desde un sitio web sencillo y estático a uno controlado por datos. Como antes, en este tutorial se descargan dos versiones de la aplicación: una que usa el modelo de proyecto de aplicación web y otra que usa el modelo de proyecto de sitio Web.

La aplicación Web de revisiones actualizadas del libro utiliza una base de datos [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) , que se almacena en la carpeta site s `App_Data` (`~/App_Data/Reviews.mdf`). Si tiene SQL Server 2008 instalado en el equipo, la demostración debe ejecutarse sin errores. Si tiene una versión anterior de SQL Server puede instalar la edición gratuita SQL Server 2008 Express o puede usar los scripts de base de datos disponibles en esta descarga de tutorial s para crear la base de datos usted mismo.

La base de datos de `Reviews.mdf` contiene cuatro tablas:

- `Genres`: incluye un registro para cada género, como tecnología, ficción y negocios.
- `Books`: incluye un registro para cada revisión, con columnas como `Title`, `GenreId`, `ReviewDate`y `Review`, entre otros.
- `Authors`: incluye información sobre cada autor que ha contribuido a un libro revisado.
- `BooksAuthors`: tabla de combinación de varios a varios que especifica qué autores han escrito qué libros.

En la figura 1 se muestra un diagrama ER de estas cuatro tablas.

[![el libro revisa la base de datos de la aplicación web se compone de cuatro tablas](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Figura 1**: el libro revisa la base de datos de la aplicación web se compone de cuatro tablas ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image3.jpg))

La versión anterior del sitio web de revisiones del libro tiene una página ASP.NET independiente para cada libro. Por ejemplo, había una página denominada `~/Tech/TYASP35.aspx` que contenía la revisión para *enseñarte usted mismo ASP.NET 3,5 en 24 horas*. Esta nueva versión controlada por datos del sitio web tiene las revisiones almacenadas en la base de datos y una sola página de ASP.NET, revise. aspx? ID =*bookId*, que muestra la revisión del libro especificado. Del mismo modo, hay una página Genre. aspx? ID =*genreId* que enumera los libros revisados en el género especificado.

Las figuras 2 y 3 muestran las páginas `Genre.aspx` y `Review.aspx` en acción. Observe la dirección URL en la barra de direcciones de cada página. En la figura 2, es género. aspx? ID = 85d164ba-1123-4c47-82a0-c8ec75de7e0e. Dado que 85d164ba-1123-4c47-82a0-c8ec75de7e0e es el valor `GenreId` para el género de la tecnología, el encabezado de la página indica "revisiones tecnológicas" y la lista con viñetas enumera las revisiones en el sitio que se encuentran bajo este género.

[![la página género de tecnología](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Figura 2**: Página género de tecnología ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image6.jpg))

[![la revisión para enseñar su ASP.NET 3,5 en 24 horas](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Figura 3**: la revisión para *enseñarse a usted mismo ASP.net 3,5 en 24 horas* ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image9.jpg))

La aplicación Web de reseñas de libros también incluye una sección de administración en la que los administradores pueden agregar, editar y eliminar los géneros, las revisiones y la información de autor. Actualmente, cualquier visitante puede acceder a la sección de administración. En un futuro tutorial, agregaremos compatibilidad con las cuentas de usuario y solo permitiría a los usuarios autorizados en las páginas de administración.

Si descarga la aplicación de revisiones del libro, tenga en cuenta que su finalidad es demostrar la implementación de una aplicación controlada por datos. No se muestran los procedimientos recomendados hasta el momento del diseño de la aplicación. Por ejemplo, no hay una capa de acceso a datos (DAL) independiente; las páginas de ASP.NET se comunican directamente con la base de datos a través del control SqlDataSource o el código ADO.NET en las clases de código subyacente. Para obtener información más detallada sobre la creación de aplicaciones controladas por datos mediante una arquitectura en capas, consulte los [Tutoriales sobre cómo *trabajar con datos* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bases de datos en desarrollo frente a producción

Al iniciar el desarrollo en una aplicación web controlada por datos, debe especificar una cadena de conexión de base de datos, que proporciona los detalles de la aplicación para conectarse a la base de datos. Esta cadena de conexión especifica, entre otras cosas, el servidor de base de datos, el nombre de la base de datos y la información de seguridad. La mayoría de las veces, la base de datos utilizada por la aplicación durante el desarrollo es diferente de la que se usa cuando se encuentra en producción. El uso de diferentes bases de datos para el desarrollo en comparación con la producción ofrece muchas ventajas. Tener una base de datos diferente en desarrollo significa que no tiene que preocuparse por la modificación o eliminación accidental de datos dinámicos. También le permite colocar datos de prueba ficticios o realizar cambios importantes en el modelo de datos sin tener que preocuparse por los efectos en la aplicación en producción. La desventaja de tener una base de datos diferente en los entornos de desarrollo y producción es que cuando la aplicación se implementa en la base de datos y también se deben implementar los cambios pertinentes en el esquema o los datos de la base de datos.

Antes de la primera implementación, solo hay una instancia de la base de datos y esa instancia se encuentra en el entorno de desarrollo. Cuando se implementa la aplicación en producción por primera vez, no solo se deben copiar los archivos de cliente y de servidor necesarios, sino que también se puede copiar la base de datos del entorno de desarrollo en el entorno de producción. Aquí es donde nos encontramos ahora con la aplicación Web de reseñas de libros: la base de datos reside en la carpeta `App_Data` en nuestro entorno de desarrollo, pero aún no se ha insertado en el entorno de producción.

Una vez implementada la aplicación, hay dos copias de la base de datos. A medida que la aplicación madura, se pueden agregar nuevas características, que requieren un cambio en el modelo de datos (como agregar nuevas columnas a las tablas existentes, realizar cambios en las columnas existentes, agregar nuevas tablas, etc.). La próxima vez que se implemente la aplicación Web, los cambios aplicados a la base de datos en el entorno de desarrollo desde la última implementación deben aplicarse a la base de datos de producción. Algunas estrategias para administrar este proceso se describen en un tutorial futuro. Este tutorial se centra en la implementación de toda la base de datos desde el entorno de desarrollo en producción.

## <a name="deploying-the-database-to-the-production-environment"></a>Implementar la base de datos en el entorno de producción

En el resto de este tutorial se examina cómo implementar la base de datos desde el entorno de desarrollo hasta el entorno de producción. Si está siguiendo los pasos que necesita para asegurarse de que su cuenta con el proveedor de host web incluye Microsoft SQL Server compatibilidad con bases de datos. También necesitará información a mano, es decir, el nombre del servidor de base de datos, el nombre de la base de datos y el nombre de usuario y la contraseña usados para conectarse a la base de datos.

Como se indicó anteriormente en este tutorial, el libro Reviews website s Database es una base de datos de SQL Server 2008 Express Edition almacenada en la carpeta `App_Data`. Esto se debe a que la implementación de este tipo de base de datos sería tan simple como copiar la carpeta `App_Data` del entorno de desarrollo en el entorno de producción. Sin embargo, la mayoría de los proveedores de hosts web no admiten el hospedaje de bases de datos en la carpeta `App_Data` por razones de seguridad. En su lugar, los hosts Web proporcionan una cuenta en un servidor de base de datos de SQL Server dentro de su entorno. La implementación de la base de datos desde el entorno de desarrollo en el entorno de producción requiere la obtención de la base de datos registrada en el servidor de base de datos del host Web.

¿Cómo se obtiene la base de datos del entorno de desarrollo al entorno de producción? Hay un par de formas de hacerlo en función de los servicios que ofrece el host Web. Con algunos hosts, como DiscountASP.NET, puede crear una copia de seguridad de la base de datos o el archivo de `.mdf` real en el sitio web y, a continuación, desde el panel de control, restaurar el archivo de copia de seguridad o adjuntar el archivo de `.mdf` al servidor de base de datos de SQL Server. Con estas herramientas, la implementación de la base de datos es tan sencilla como copiar la `App_Data` carpeta en el entorno de producción y, a continuación, adjuntarla a través del panel de control. Tal vez sea la forma más sencilla y rápida de publicar la base de datos por primera vez.

Otro enfoque consiste en usar el Asistente para la publicación de bases de datos. El Asistente para la publicación de bases de datos es una aplicación de escritorio de Windows que generará los comandos SQL para crear el esquema de la base de datos (tablas, procedimientos almacenados, vistas, funciones definidas por el usuario, etc.) y, opcionalmente, los datos de las tablas. Después, puede conectarse al servidor de base de datos del proveedor de host web a través de SQL Server Management Studio y, a continuación, ejecutar este script para duplicar la base de datos en producción. Incluso mejor, si el proveedor de host Web admite Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) , puede hacer que el script generado por el Asistente para la publicación de bases de datos se ejecute automáticamente en el servidor de base de datos en su nombre. Dado que el Asistente para la publicación de bases de datos genera un script que crea el esquema y los datos de la base de datos, funcionará independientemente de si el proveedor de host web ofrece características como adjuntar un archivo `.mdf` cargado.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generar los comandos SQL para crear el esquema y los datos de la base de datos mediante el Asistente para la publicación de bases de datos

Vamos a examinar el uso del Asistente para la publicación de bases de datos para implementar el libro Reviews Database en producción. Si usa Visual Studio 2008 o posterior, el Asistente para la publicación de bases de datos ya está instalado. Si usa Visual Studio 2005, deberá [Descargar e instalar](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) primero el asistente.

Abra Visual Studio y vaya a la base de datos `Reviews.mdf`. Si usa Visual Web Developer, vaya al Explorador de bases de datos; Si usa Visual Studio, use el Explorador de servidores. En la figura 4 se muestra el `Reviews.mdf` base de datos en el Explorador de bases de datos en Visual Web Developer. Como se muestra en la figura 4, la base de datos de `Reviews.mdf` se compone de cuatro tablas, tres procedimientos almacenados y una función definida por el usuario.

[![encontrar la base de datos en el Explorador de bases de datos o Explorador de servidores](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Figura 4**: Busque la base de datos en el Explorador de bases de datos o explorador de servidores ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image12.jpg))

Haga clic con el botón derecho en el nombre de la base de datos y elija la opción "publicar en proveedor" en el menú contextual. Se iniciará el Asistente para la publicación de bases de datos (vea la figura 5). Haga clic en siguiente para avanzar más allá de la pantalla de presentación.

[![la pantalla de presentación del Asistente para la publicación de bases de datos](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Figura 5**: la pantalla de presentación del Asistente para la publicación de bases de datos ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image15.jpg))

En la segunda pantalla del asistente se enumeran las bases de datos a las que puede tener acceso el Asistente para la publicación de bases de datos y se puede elegir si se deben incluir en el script todos los objetos de la base de datos seleccionada o elegir los objetos que se van a incluir Seleccione la base de datos adecuada y deje activada la opción "incluir todos los objetos en la base de datos seleccionada".

> [!NOTE]
> Si aparece el error "no hay objetos en la base de datos *DatabaseName* de los tipos que puede incluir en el script este asistente" al hacer clic en siguiente en la pantalla que aparece en la figura 6, asegúrese de que la ruta de acceso al archivo de base de datos no sea demasiado larga. Se ha descubierto que este error puede producirse si la ruta de acceso al archivo de base de datos es demasiado larga.

[![la pantalla de presentación del Asistente para la publicación de bases de datos](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Figura 6**: la pantalla de presentación del Asistente para la publicación de bases de datos ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image18.jpg))

En la pantalla siguiente puede generar un archivo de script o, si el host web lo admite, publicar la base de datos directamente en el servidor de base de datos del proveedor de host Web. Como se muestra en la figura 7, tengo el script escrito en el archivo `C:\REVIEWS.MDF.sql`.

[![incluir la base de datos en un archivo o publicarla directamente en el proveedor de hosts Web](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Figura 7**: crear un script para la base de datos en un archivo o publicarla directamente en el proveedor de host web ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image21.jpg))

En la pantalla siguiente se le pide una variedad de opciones de scripting. Puede especificar si el script debe incluir instrucciones Drop para quitar estos objetos existentes. El valor predeterminado es true, que es correcto al implementar una base de datos por primera vez. También puede especificar si la base de datos de destino es SQL Server 2000, SQL Server 2005 o SQL Server 2008. Por último, puede indicar si desea crear un script para el esquema y los datos, solo los datos o simplemente el esquema. El esquema es la colección de objetos de base de datos, las tablas, los procedimientos almacenados, las vistas, etc. Los datos son la información que reside en las tablas.

Como se muestra en la figura 8, el asistente se ha configurado para quitar objetos de base de datos existentes, para generar scripts para una base de datos SQL Server 2008 y para publicar el esquema y los datos.

[![especificar las opciones de publicación](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Figura 8**: especificación de las opciones de publicación ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image24.jpg))

En las dos pantallas finales se resumen las acciones que están a punto de tomarse y, a continuación, se muestra el estado de la secuencia de comandos. El resultado neto de la ejecución del asistente es que tenemos un archivo de script que contiene los comandos SQL necesarios para crear la base de datos en producción y rellenarla con los mismos datos que en el desarrollo.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Ejecutar los comandos SQL en la base de datos del entorno de producción

Ahora que tenemos el script que contiene los comandos SQL para crear la base de datos y sus datos, todo lo que queda es ejecutar el script en la base de datos de producción. Algunos proveedores de host Web ofrecen un cuadro de texto en el panel de control, donde puede especificar comandos SQL para ejecutarlos en la base de datos. Si tiene un archivo de script muy grande, es posible que esta opción no funcione (el archivo de script de `REVIEWS.MDF.sql` tiene un tamaño superior a 425 KB, por ejemplo).

Un mejor enfoque consiste en conectarse directamente al servidor de base de datos de producción mediante SQL Server Management Studio (SSMS). Si tiene una edición no Express de SQL Server instalada en el equipo, es probable que ya tenga instalado SSMS. De lo contrario, puede [Descargar e instalar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) una copia gratuita de SQL Server Management Studio Express Edition.

Inicie SSMS y conéctese al servidor de base de datos del host de Web mediante la información proporcionada por el proveedor de host Web.

[![conectarse al servidor de base de datos del proveedor de host Web](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Figura 9**: Conéctese al servidor de base de datos del proveedor de host web ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image27.jpg))

Expanda la pestaña bases de datos y busque la base de datos. Haga clic en el botón Nueva consulta en la esquina superior izquierda de la barra de herramientas, pegue los comandos SQL del archivo de script creado por el Asistente para la publicación de bases de datos y haga clic en el botón ejecutar para ejecutar estos comandos en el servidor de base de datos de producción. Si el archivo de script es especialmente grande, puede tardar varios minutos en ejecutar los comandos.

[![conectarse al servidor de base de datos del proveedor de host Web](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Figura 10**: Conéctese al servidor de base de datos del proveedor de host web ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image30.jpg))

Eso es todo lo que tiene que hacer. En este momento, la base de datos de desarrollo se ha duplicado en producción. Si actualiza la base de datos en SSMS, debería ver los nuevos objetos de base de datos. La figura 11 muestra las tablas, los procedimientos almacenados y las funciones definidas por el usuario de la base de datos de producción, que reflejan los reflejos en la base de datos de desarrollo. Y como se indicó al Asistente para la publicación de bases de datos que publique los datos, las tablas de la base de datos de producción tienen los mismos datos que las tablas de la base de datos de desarrollo en el momento en que se ejecutó el asistente. En la ilustración 12 se muestran los datos de la tabla `Books` en la base de datos de producción.

[![los objetos de base de datos se han duplicado en la base de datos de producción](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Figura 11**: los objetos de base de datos se han duplicado en la base de datos de producción ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image33.jpg))

[![la base de datos de producción contiene los mismos datos que en la base de datos de desarrollo](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Figura 12**: la base de datos de producción contiene los mismos datos que en la base de datos de desarrollo ([haga clic para ver la imagen de tamaño completo](deploying-a-database-cs/_static/image36.jpg))

En este momento, solo hemos implementado la base de datos de desarrollo en producción. Todavía no se ha analizado la implementación de la propia aplicación web o se han examinado los cambios de configuración necesarios para que la aplicación en producción use la base de datos de producción. Trataremos estos problemas en el siguiente tutorial.

## <a name="summary"></a>Resumen

La implementación de una aplicación web controlada por datos requiere copiar la base de datos utilizada durante el desarrollo en el entorno de producción. Muchos proveedores de hosts Web ofrecen herramientas para simplificar el proceso de implementación de una base de datos. Por ejemplo, con DiscountASP.NET puede obtener una FTP de la base de datos `.mdf` archivo (o una copia de seguridad) y, a continuación, adjuntar la base de datos al servidor de base de datos desde el panel de control. Otra opción que funciona independientemente de las características que ofrece el proveedor de host web es la herramienta del Asistente para la publicación de bases de datos de Microsoft, que genera un script de comandos SQL para crear el esquema y los datos de la base de datos de desarrollo. Una vez generado este script, puede ejecutarlo en la base de datos de producción.

Ahora que el libro revisa la base de datos de la aplicación web en producción, podemos implementar la aplicación. Sin embargo, la información de configuración de la aplicación web especifica la cadena de conexión a la base de datos y esa cadena de conexión hace referencia a la base de datos de desarrollo. Necesitamos actualizar esta información de la cadena de conexión al implementar el sitio en producción. En el siguiente tutorial se examinan estas diferencias de configuración y se explican los pasos necesarios para publicar el sitio de revisiones de libros controlado por datos en producción.

¡ Feliz programación!

#### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Descargar el Asistente para publicación de Microsoft SQL Server Database 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Descargar el Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Anterior](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Siguiente](configuring-the-production-web-application-to-use-the-production-database-cs.md)
