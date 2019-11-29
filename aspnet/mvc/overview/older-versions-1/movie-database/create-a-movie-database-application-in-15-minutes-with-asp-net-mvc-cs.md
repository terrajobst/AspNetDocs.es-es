---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Creación de una aplicación de base de datos de películas en 15C#minutos con ASP.NET MVC () | Microsoft Docs
author: StephenWalther
description: Stephen Walther crea una aplicación de ASP.NET MVC completa controlada por la base de datos de principio a fin. Este tutorial es una excelente introducción para las personas que son nuevas t...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 1be5d135a44feb27626dd26a544b64cfb57b18a9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596134"
---
# <a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Crear una aplicación de base de datos de película en 15 minutos con ASP.NET MVC (C#)

por [Stephen Walther](https://github.com/StephenWalther)

[Descargar código](https://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther crea una aplicación de ASP.NET MVC completa controlada por la base de datos de principio a fin. Este tutorial es una excelente introducción para las personas que son nuevas en el marco ASP.NET MVC y que quieren tener una idea del proceso de creación de una aplicación ASP.NET MVC.

El propósito de este tutorial es darle una idea de "lo que es como" para compilar una aplicación ASP.NET MVC. En este tutorial, se puede crear una aplicación ASP.NET MVC completa de principio a fin. Le mostramos cómo crear una aplicación simple controlada por base de datos que muestra cómo puede mostrar, crear y editar registros de base de datos.

Para simplificar el proceso de creación de la aplicación, aprovecharemos las características de scaffolding de Visual Studio 2008. Dejaremos que Visual Studio genere el código y el contenido iniciales para nuestros controladores, modelos y vistas.

Si ha trabajado con Active Server páginas o ASP.NET, debe encontrar ASP.NET MVC muy familiar. Las vistas de MVC de ASP.NET son muy parecidas a las páginas de una aplicación Active Server páginas. Y, al igual que una aplicación de formularios Web Forms de ASP.NET tradicional, ASP.NET MVC proporciona acceso completo al amplio conjunto de lenguajes y clases proporcionadas por .NET Framework.

Mi esperanza es que este tutorial le dará una idea de cómo la experiencia de creación de una aplicación ASP.NET MVC es similar y diferente a la experiencia de creación de páginas de Active Server o aplicación de formularios Web Forms de ASP.NET.

## <a name="overview-of-the-movie-database-application"></a>Información general de la aplicación de base de datos de películas

Dado que nuestro objetivo es simplificar las cosas, crearemos una aplicación de base de datos de películas muy simple. Nuestra sencilla aplicación de base de datos de películas nos permitirá hacer tres cosas:

1. Enumerar un conjunto de registros de la base de datos de películas
2. Crear un nuevo registro de la base de datos de películas
3. Editar un registro de base de datos de películas existente

Una vez más, dado que queremos simplificar las cosas, aprovecharemos el número mínimo de características del marco de MVC de ASP.NET necesario para compilar la aplicación. Por ejemplo, no se aprovechará el desarrollo controlado por pruebas.

Para crear la aplicación, es necesario completar cada uno de los siguientes pasos:

1. Crear el proyecto de aplicación web MVC de ASP.NET
2. Creación de la base de datos
3. Crear el modelo de base de datos
4. Creación del controlador de MVC de ASP.NET
5. Crear las vistas de MVC de ASP.NET

## <a name="preliminaries"></a>Windows°PE

Necesitará Visual Studio 2008 o Visual Web Developer 2008 Express para compilar una aplicación ASP.NET MVC. También debe descargar el marco de MVC de ASP.NET.

Si no tiene Visual Studio 2008, puede descargar una versión de prueba de 90 días de Visual Studio 2008 desde este sitio web:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Como alternativa, puede crear aplicaciones de ASP.NET MVC con Visual Web Developer Express 2008. Si decide usar Visual Web Developer Express, debe tener instalado Service Pack 1. Puede descargar Visual Web Developer 2008 Express con Service Pack 1 desde este sitio web:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Después de instalar Visual Studio 2008 o Visual Web Developer 2008, debe instalar el marco de MVC de ASP.NET. Puede descargar el marco de MVC de ASP.NET desde el siguiente sitio web:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> En lugar de descargar el marco de trabajo de ASP.NET y el marco de ASP.NET MVC de forma individual, puede aprovechar el instalador de plataforma Web. El instalador de plataforma web es una aplicación que permite administrar fácilmente las aplicaciones instaladas en el equipo:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)

## <a name="creating-an-aspnet-mvc-web-application-project"></a>Creación de un proyecto de aplicación web MVC de ASP.NET

Comencemos por crear un nuevo proyecto de aplicación web MVC de ASP.NET en Visual Studio 2008. Seleccione el archivo de opción de menú **, nuevo proyecto,** y verá el cuadro de diálogo nuevo proyecto en la figura 1. Seleccione C# como lenguaje de programación y seleccione la plantilla de proyecto aplicación web MVC de ASP.net. Asigne al proyecto el nombre MovieApp y haga clic en el botón Aceptar.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: el cuadro de diálogo nuevo proyecto ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))

Asegúrese de que selecciona .NET Framework 3,5 en la lista desplegable de la parte superior del cuadro de diálogo nuevo proyecto o que la plantilla de proyecto de aplicación web MVC de ASP.NET no aparece.

Siempre que cree un nuevo proyecto de aplicación web MVC, Visual Studio le pedirá que cree un proyecto de prueba unitaria independiente. Aparece el cuadro de diálogo de la figura 2. Dado que no vamos a crear pruebas en este tutorial debido a las restricciones de tiempo (y, sí, deberíamos sentir un poco culpable sobre esto) Seleccione la opción **no** y haga clic en el botón **Aceptar** .

> [!NOTE] 
> 
> Visual Web Developer no admite proyectos de prueba.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: el cuadro de diálogo crear proyecto de prueba unitaria ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))

Una aplicación ASP.NET MVC tiene un conjunto estándar de carpetas: una carpeta de modelos, vistas y controladores. Puede ver este conjunto estándar de carpetas en la ventana Explorador de soluciones. Tendremos que agregar archivos a cada una de las carpetas de modelos, vistas y controladores para poder compilar la aplicación de base de datos de películas.

Cuando se crea una nueva aplicación MVC con Visual Studio, se obtiene una aplicación de ejemplo. Dado que queremos empezar desde cero, es necesario eliminar el contenido de esta aplicación de ejemplo. Debe eliminar el siguiente archivo y la siguiente carpeta:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Crear la base de datos

Necesitamos crear una base de datos para almacenar los registros de la base de datos de películas. Afortunadamente, Visual Studio incluye una base de datos gratuita denominada SQL Server Express. Siga estos pasos para crear la base de datos:

1. Haga clic con el botón derecho en la carpeta\_de datos de la aplicación en la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento**.
2. Seleccione la categoría de **datos** y seleccione la plantilla **SQL Server Database** (vea la figura 3).
3. Asigne a la nueva base de datos el nombre *MoviesDB. MDF* y haga clic en el botón **Agregar** .

Después de crear la base de datos, puede conectarse a la base de datos haciendo doble clic en el archivo MoviesDB. MDF que se encuentra en la carpeta app\_Data. Al hacer doble clic en el archivo MoviesDB. MDF, se abre la ventana Explorador de servidores.

> [!NOTE] 
> 
> La ventana de Explorador de servidores se denomina ventana de Explorador de bases de datos en el caso de Visual Web Developer.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: creación de una base de datos de Microsoft SQL Server ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))

A continuación, es necesario crear una nueva tabla de base de datos. Desde la ventana Explorador de servidores, haga clic con el botón secundario en la carpeta tablas y seleccione la opción de menú **Agregar nueva tabla**. Al seleccionar esta opción de menú, se abre el diseñador de tablas de base de datos. Cree las columnas de base de datos siguientes:

<a id="0.1_table01"></a>

| **Nombre de columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | Valor int. | Falso |
| Title | Nvarchar (100) | Falso |
| DataDirectory | Nvarchar (100) | Falso |
| DateReleased | DateTime | Falso |

La primera columna, la columna ID, tiene dos propiedades especiales. En primer lugar, debe marcar la columna ID como la columna de clave principal. Después de seleccionar la columna ID, haga clic en el botón **set Primary Key (establecer clave principal** ) (es decir, el icono que se parece a una clave). En segundo lugar, debe marcar la columna ID como una columna de identidad. En el ventana Propiedades de columna, desplácese hacia abajo hasta la sección especificación de identidad y expándala. Cambie la propiedad **is Identity** por el valor **yes**. Cuando haya terminado, la tabla debe tener un aspecto similar al de la figura 4.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: la tabla de la base de datos de películas ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))

El paso final consiste en guardar la nueva tabla. Haga clic en el botón Guardar (el icono del disquete) y asigne el nombre Movie a la nueva tabla.

Cuando termine de crear la tabla, agregue algunos registros de película a la tabla. Haga clic con el botón secundario en la tabla películas de la ventana de Explorador de servidores y seleccione la opción de menú **Mostrar datos de tabla**. Escriba una lista de sus películas favoritas (consulte la figura 5).

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: entrada de registros de película ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))

## <a name="creating-the-model"></a>Crear el modelo

A continuación, necesitamos crear un conjunto de clases para representar nuestra base de datos. Es necesario crear un modelo de base de datos. Aprovecharemos las ventajas de Microsoft Entity Framework para generar automáticamente las clases para nuestro modelo de base de datos.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está vinculado a Microsoft Entity Framework. Puede crear las clases de modelo de base de datos aprovechando una variedad de herramientas de asignación relacional de objetos (o/M), como LINQ to SQL, Subsonic y NHibernate.

Siga estos pasos para iniciar el Asistente para Entity Data Model:

1. Haga clic con el botón secundario en la carpeta modelos de la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento**.
2. Seleccione la categoría de **datos** y seleccione la plantilla **ADO.NET Entity Data Model** .
3. Asigne al modelo de datos el nombre *MoviesDBModel. edmx* y haga clic en el botón **Agregar** .

Después de hacer clic en el botón Agregar, aparece el Asistente para Entity Data Model (vea la figura 6). Siga estos pasos para completar el asistente:

1. En el paso **elegir contenido del modelo** , seleccione la opción **generar desde la base de datos** .
2. En el paso **elegir la conexión de datos** , use la conexión de datos *MoviesDB. MDF* y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el botón **siguiente** .
3. En el paso **Elija los objetos de base de datos** , expanda el nodo tablas y seleccione la tabla películas. Escriba el espacio de nombres *MovieApp. Models* y haga clic en el botón **Finalizar** .

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: generación de un modelo de base de datos con el asistente para Entity Data Model ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))

Después de completar el Asistente para Entity Data Model, se abre el diseñador de Entity Data Model. El diseñador debe mostrar la tabla de la base de datos de películas (vea la figura 7).

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: el diseñador de Entity Data Model ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))

Necesitamos hacer un cambio antes de continuar. El Asistente para datos de entidad genera una clase de modelo denominada *películas* que representa la tabla de la base de datos de películas. Dado que vamos a usar la clase Movies para representar una película determinada, es necesario modificar el nombre de la clase para que sea *Movie* en lugar de *películas* (singular en lugar de plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de películas a película. Después de hacer este cambio, haga clic en el botón **Guardar** (el icono del disquete) para generar la clase Movie.

## <a name="creating-the-aspnet-mvc-controller"></a>Creación del controlador de MVC de ASP.NET

El siguiente paso consiste en crear el controlador ASP.NET MVC. Un controlador es responsable de controlar el modo en que un usuario interactúa con una aplicación ASP.NET MVC.

Siga estos pasos:

1. En la ventana de Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers y seleccione la opción de menú **Agregar, controlador**.
2. En el cuadro de diálogo Agregar controlador, escriba el nombre *HomeController* y active la casilla **Agregar métodos de acción para los escenarios de creación, actualización y detalles** (consulte la figura 8).
3. Haga clic en el botón **Agregar** para agregar el nuevo controlador al proyecto.

Después de completar estos pasos, se crea el controlador en la lista 1. Tenga en cuenta que contiene métodos denominados index, details, Create y Edit. En las secciones siguientes, agregaremos el código necesario para que funcionen estos métodos.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: incorporación de un nuevo controlador ASP.NET MVC ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))

**Lista 1: Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Enumerar registros de base de datos

El método index () del controlador Home es el método predeterminado para una aplicación ASP.NET MVC. Al ejecutar una aplicación ASP.NET MVC, el método index () es el primer método de controlador que se llama.

Usaremos el método index () para mostrar la lista de registros de la tabla de base de datos de películas. Aprovecharemos las clases de modelo de base de datos que creamos anteriormente para recuperar los registros de la base de datos de películas con el método index ().

He modificado la clase HomeController en la lista 2 para que contenga un nuevo campo privado denominado \_dB. La clase MoviesDBEntities representa nuestro modelo de base de datos y usaremos esta clase para comunicarse con nuestra base de datos.

También he modificado el método index () en la lista 2. El método index () utiliza la clase MoviesDBEntities para recuperar todos los registros de la película de la tabla de la base de datos movies. Expresión *\_dB. MovieSet. ToList ()* devuelve una lista de todos los registros de películas de la tabla de base de datos de películas.

La lista de películas se pasa a la vista. Todo lo que se pasa al método View () se pasa a la vista como datos de vista.

**Enumeración 2: Controllers/HomeController. CS (método de índice modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

El método index () devuelve una vista denominada index. Necesitamos crear esta vista para mostrar la lista de registros de la base de datos de películas. Siga estos pasos:

Debe compilar el proyecto (seleccione la opción de menú **generar, compilar solución**) antes de abrir el cuadro de diálogo **Agregar vista** o no se mostrará ninguna clase en la lista desplegable **Ver clase de datos** .

1. Haga clic con el botón derecho en el método index () en el editor de código y seleccione la opción de menú **Agregar vista** (vea la ilustración 9).
2. En el cuadro de diálogo Agregar vista, compruebe que está activada la casilla **crear una vista fuertemente tipada** .
3. En la lista desplegable **ver contenido** , seleccione la *lista*valor.
4. En la lista desplegable **Ver clase de datos** , seleccione el valor *MovieApp. Models. Movie*.
5. Haga clic en el botón Agregar para crear la nueva vista (vea la figura 10).

Después de completar estos pasos, se agrega una nueva vista denominada index. aspx a la carpeta Views\Home. El contenido de la vista de índice se incluye en la lista 3.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: agregar una vista desde una acción de controlador ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: creación de una nueva vista con el cuadro de diálogo Agregar vista ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))

**Lista 3: Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

La vista de índice muestra todos los registros de película de la tabla de la base de datos películas en una tabla HTML. La vista contiene un bucle foreach que recorre en iteración cada película representada por la propiedad ViewData. Model. Si ejecuta la aplicación presionando la tecla F5, verá la página web en la figura 11.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: la vista de índice ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))

## <a name="creating-new-database-records"></a>Crear nuevos registros de base de datos

La vista de índice que hemos creado en la sección anterior incluye un vínculo para crear nuevos registros de base de datos. Vamos a implementar la lógica y crear la vista necesaria para crear nuevos registros de la base de datos de películas.

El controlador Home contiene dos métodos denominados Create (). El primer método Create () no tiene parámetros. Esta sobrecarga del método Create () se usa para mostrar el formulario HTML para crear un nuevo registro de la base de datos de películas.

El segundo método Create () tiene un parámetro FormCollection. Se llama a esta sobrecarga del método Create () cuando el formulario HTML para crear una nueva película se envía al servidor. Observe que este segundo método Create () tiene un atributo AcceptVerbs que impide que se llame al método a menos que se realice una operación HTTP POST.

Este segundo método Create () se ha modificado en la clase HomeController actualizada en la lista 4. La nueva versión del método Create () acepta un parámetro de película y contiene la lógica para insertar una nueva película en la tabla de base de datos de películas.

> [!NOTE] 
> 
> Observe el atributo BIND. Dado que no deseamos actualizar la propiedad ID de la película del formulario HTML, es necesario excluir explícitamente esta propiedad.

**Lista 4: Controllers\HomeController.cs (método de creación modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio facilita la creación del formulario para crear un nuevo registro de la base de datos de películas (consulte la figura 12). Siga estos pasos:

1. Haga clic con el botón secundario en el método Create () en el editor de código y seleccione la opción de menú **Agregar vista**.
2. Compruebe que la casilla de verificación **crear una vista fuertemente tipada** está activada.
3. En la lista desplegable **ver contenido** , seleccione el valor *crear*.
4. En la lista desplegable **Ver clase de datos** , seleccione el valor *MovieApp. Models. Movie*.
5. Haga clic en el botón **Agregar** para crear la nueva vista.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: adición de la vista de creación ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))

Visual Studio genera la vista en la lista 5 automáticamente. Esta vista contiene un formulario HTML que incluye los campos que se corresponden con cada una de las propiedades de la clase Movie.

**Lista 5: Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> El formulario HTML generado por el cuadro de diálogo Agregar vista genera un campo de formulario de identificador. Dado que la columna ID es una columna de identidad, no se necesita este campo de formulario y se puede quitar de forma segura.

Después de agregar la vista de creación, puede agregar nuevos registros de película a la base de datos. Ejecute la aplicación presionando la tecla F5 y haga clic en el vínculo crear nuevo para ver el formulario en la figura 13. Si completa y envía el formulario, se crea un nuevo registro de la base de datos de películas.

Tenga en cuenta que la validación del formulario se obtiene automáticamente. Si no especifica una fecha de lanzamiento para una película o escribe una fecha de lanzamiento no válida, se vuelve a mostrar el formulario y se resalta el campo fecha de lanzamiento.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: creación de un nuevo registro de la base de datos de películas ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))

## <a name="editing-existing-database-records"></a>Editar registros de bases de datos existentes

En las secciones anteriores, hemos explicado cómo puede enumerar y crear nuevos registros de base de datos. En esta última sección, se explica cómo puede editar los registros de base de datos existentes.

En primer lugar, es necesario generar el formulario de edición. Este paso es sencillo, ya que Visual Studio generará el formulario de edición para nosotros automáticamente. Abra la clase HomeController.cs en el editor de código de Visual Studio y siga estos pasos:

1. Haga clic con el botón secundario en el método Edit () en el editor de código y seleccione la opción de menú **Agregar vista** (vea la figura 14).
2. Active la casilla de verificación **crear una vista fuertemente tipada**.
3. En la lista desplegable **ver contenido** , seleccione el valor *Editar*.
4. En la lista desplegable **Ver clase de datos** , seleccione el valor *MovieApp. Models. Movie*.
5. Haga clic en el botón **Agregar** para crear la nueva vista.

Al completar estos pasos, se agrega una nueva vista denominada Edit. aspx a la carpeta Views\Home. Esta vista contiene un formulario HTML para editar un registro de película.

[![el cuadro de diálogo nuevo proyecto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: adición de la vista de edición ([haga clic para ver la imagen de tamaño completo](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))

> [!NOTE] 
> 
> La vista de edición contiene un campo de formulario HTML que corresponde a la propiedad ID. de la película. Dado que no desea que los usuarios editen el valor de la propiedad ID, debe quitar este campo de formulario.

Por último, es necesario modificar el controlador Home para que admita la edición de un registro de base de datos. La clase HomeController actualizada está incluida en la lista 6.

**Lista 6 – Controllers\HomeController.cs (editar métodos)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

En la enumeración 6, he agregado lógica adicional a las dos sobrecargas del método Edit (). El primer método Edit () devuelve el registro de la base de datos de películas que corresponde al parámetro id pasado al método. La segunda sobrecarga realiza las actualizaciones en un registro de película en la base de datos.

Tenga en cuenta que debe recuperar la película original y, a continuación, llamar a ApplyPropertyChanges () para actualizar la película existente en la base de datos.

## <a name="summary"></a>Resumen

El propósito de este tutorial fue ofrecer una idea de la experiencia de creación de una aplicación ASP.NET MVC. Espero que haya descubierto que la creación de una aplicación web MVC de ASP.NET es muy similar a la experiencia de creación de una aplicación Active Server páginas o de ASP.NET.

En este tutorial, hemos examinado solo las características más básicas del marco ASP.NET MVC. En futuros tutoriales, profundizamos en temas como controladores, acciones del controlador, vistas, datos de la vista y aplicaciones auxiliares HTML.

> [!div class="step-by-step"]
> [Siguiente](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
