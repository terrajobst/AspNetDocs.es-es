---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 7c4aa34567ac4fb31d1ed874cf65986c4e779e66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434491"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Obtener acceso a los datos del modelo desde un controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta sección, creará una nueva clase de `MoviesController` y escribirá código que recupera los datos de la película y los muestra en el explorador con una plantilla de vista.

**Compile la aplicación** antes de pasar al paso siguiente.

Haga clic con el botón secundario en la carpeta *Controllers* y cree un nuevo controlador de `MoviesController`. Las opciones siguientes no aparecerán hasta que compile la aplicación. Seleccione las opciones siguientes:

- Nombre del controlador: **MoviesController**. (Este es el valor predeterminado. )
- Plantilla: **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework**.
- Clase de modelo: **Movie (MvcMovie. Models)** .
- Clase de contexto de datos: **MovieDBContext (MvcMovie. Models)** .
- Vistas: **Razor (CSHTML)** . (El valor predeterminado).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Haga clic en **Agregar**. Visual Studio Express crea los siguientes archivos y carpetas:

- *Un archivo MoviesController.CS* en la carpeta de *Controladores* del proyecto.
- Una carpeta *películas* en la carpeta *vistas* del proyecto.
- *Cree. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* en la nueva carpeta *Views\Movies*

ASP.NET MVC 4 creó automáticamente las vistas y los métodos de acción CRUD (crear, leer, actualizar y eliminar) automáticamente (la creación automática de vistas y métodos de acción CRUD se conoce como scaffolding). Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar entradas de películas.

Ejecute la aplicación y vaya al controlador de `Movies` anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Dado que la aplicación se basa en el enrutamiento predeterminado (definido en el archivo *global. asax* ), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta al método de acción de `Index` predeterminado del controlador de `Movies`. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es realmente la misma que la solicitud del explorador `http://localhost:xxxxx/Movies/Index`. El resultado es una lista vacía de películas, ya que aún no ha agregado ninguna.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Crear una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles sobre una película y, a continuación, haga clic en el botón **crear** .

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Al hacer clic en el botón **crear** , el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la dirección URL de */Movies* , donde podrá ver la película recién creada en la lista.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra el archivo *Controllers\MoviesController.CS* y examine el método de `Index` generado. A continuación se muestra una parte del controlador de película con el método `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La siguiente línea de la clase `MoviesController` crea una instancia de un contexto de la base de datos Movie, tal como se ha descrito anteriormente. Puede usar el contexto de la base de datos de películas para consultar, editar y eliminar películas.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una solicitud al controlador de `Movies` devuelve todas las entradas de la tabla `Movies` de la base de datos de películas y, a continuación, pasa los resultados a la vista `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fuertemente tipados y la palabra clave @model

Anteriormente en este tutorial, vio cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante el objeto `ViewBag`. La `ViewBag` es un objeto dinámico que proporciona una cómoda manera de enlazar información a una vista.

ASP.NET MVC también proporciona la capacidad de pasar datos o objetos fuertemente tipados a una plantilla de vista. Este enfoque fuertemente tipado permite una mejor comprobación en tiempo de compilación del código y IntelliSense más enriquecida en el editor de Visual Studio. El mecanismo de scaffolding de Visual Studio usó este enfoque con la clase `MoviesController` y las plantillas de vista cuando creó los métodos y las vistas.

En el archivo *Controllers\MoviesController.CS* , examine el método de `Details` generado. A continuación se muestra una parte del controlador de película con el método `Details`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Si se encuentra un `Movie`, se pasa una instancia del modelo de `Movie` a la vista de detalles. Examine el contenido del archivo *Views\Movies\Details.cshtml* .

Al incluir una instrucción `@model` en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera la vista. Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la plantilla *details. cshtml* , el código pasa cada campo de película a las aplicaciones auxiliares HTML `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) con el objeto `Model` fuertemente tipado. Los métodos Create y Edit y las plantillas de vista también pasan un objeto de modelo de película.

Examine la plantilla de vista *index. cshtml* y el método `Index` en el archivo *MoviesController.CS* . Observe cómo el código crea un objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) cuando llama al método auxiliar `View` en el método de acción `Index`. A continuación, el código pasa esta `Movies` lista del controlador a la vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Al crear el controlador de película, Visual Studio Express incluye automáticamente la siguiente instrucción de `@model` en la parte superior del archivo *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta directiva de `@model` permite tener acceso a la lista de películas que el controlador pasó a la vista mediante un objeto de `Model` fuertemente tipado. Por ejemplo, en la plantilla *index. cshtml* , el código recorre en bucle las películas mediante una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Dado que el objeto `Model` está fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada `item` objeto del bucle se escribe como `Movie`. Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y la compatibilidad completa con IntelliSense en el editor de código:

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Trabajar con SQL Server LocalDB

Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó apuntaba a una base de datos de `Movies` que todavía no existía, por lo que Code First creó la base de datos automáticamente. Para comprobar que se ha creado, busque en la carpeta *App\_Data* . Si no ve el archivo *movies. MDF* , haga clic en el botón **Mostrar todos los archivos** en la barra de herramientas **Explorador de soluciones** , haga clic en el botón **Actualizar** y, a continuación, expanda la carpeta *App\_Data* .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Haga doble clic en *movies. MDF* para abrir el **Explorador de bases de datos**y, a continuación, expanda la carpeta **tablas** para ver la tabla películas.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Si el explorador de bases de datos no aparece, en el menú **herramientas** , seleccione **conectar con base**de datos y, a continuación, cancele el cuadro de diálogo **elegir origen de datos** . Esto obligará a abrir el explorador de base de datos.

> [!NOTE]
> Si está usando vWD existente o Visual Studio 2010 y recibe un error similar al siguiente:
> 
> - La base de datos ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. No se puede abrir MDF porque es la versión 706. Este servidor es compatible con la versión 655 y versiones anteriores. No se admite esta ruta de actualización.
> - El código de usuario no controló &quot;excepción InvalidOperation&quot; el SqlConnection proporcionado no especifica un catálogo inicial.
> 
> Debe instalar el [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) y [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Compruebe la cadena de conexión de `MovieDBContext` especificada en la página anterior.

Haga clic con el botón secundario en la tabla `Movies` y seleccione **Mostrar datos de tabla** para ver los datos que ha creado.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Haga clic con el botón secundario en la tabla `Movies` y seleccione **abrir definición de tabla** para ver la estructura de la tabla que Entity Framework Code First crear automáticamente.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Observe cómo se asigna el esquema de la tabla `Movies` a la clase `Movie` que creó anteriormente. Entity Framework Code First crea automáticamente este esquema en función de la clase `Movie`.

Cuando haya terminado, cierre la conexión haciendo clic con el botón secundario en *MovieDBContext* y seleccionando **cerrar conexión**. (Si no cierra la conexión, es posible que reciba un error la próxima vez que ejecute el proyecto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Ahora tiene la base de datos y una página de lista simple para mostrar su contenido. En el siguiente tutorial, examinaremos el resto del código con scaffolding y agregaremos un método `SearchIndex` y una vista `SearchIndex` que le permita buscar películas en esta base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
