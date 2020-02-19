---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Acceso a los datos del modelo desde un controlador (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e9c7f24d426635760b613f570b85455ce71b3dc
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458173"
---
# <a name="accessing-your-models-data-from-a-controller-c"></a>Obtener acceso a los datos del modelo desde un controlador (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Hay disponible un proyecto de Visual C# Web Developer con código fuente para acompañar este tema. [Descargue la C# versión](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.

En esta sección, creará una nueva clase de `MoviesController` y escribirá código que recupera los datos de la película y los muestra en el explorador con una plantilla de vista. Asegúrese de compilar la aplicación antes de continuar.

Haga clic con el botón secundario en la carpeta *Controllers* y cree un nuevo controlador de `MoviesController`. Seleccione las opciones siguientes:

- Nombre del controlador: **MoviesController**. (Este es el valor predeterminado. )
- Plantilla: **controlador con acciones de lectura/escritura y vistas, utilizando Entity Framework**.
- Clase de modelo: **Movie (MvcMovie. Models)** .
- Clase de contexto de datos: **MovieDBContext (MvcMovie. Models)** .
- Vistas: **Razor (CSHTML)** . (El valor predeterminado).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Haga clic en **Agregar**. Visual Web Developer crea los siguientes archivos y carpetas:

- *Un archivo MoviesController.CS* en la carpeta de *Controladores* del proyecto.
- Una carpeta *películas* en la carpeta *vistas* del proyecto.
- *Cree. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* en la nueva carpeta *Views\Movies*

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

El mecanismo de scaffolding de ASP.NET MVC 3 crea automáticamente las vistas y los métodos de acción CRUD (crear, leer, actualizar y eliminar). Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar entradas de películas.

Ejecute la aplicación y vaya al controlador de `Movies` anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Dado que la aplicación se basa en el enrutamiento predeterminado (definido en el archivo *global. asax* ), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta al método de acción de `Index` predeterminado del controlador de `Movies`. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es realmente la misma que la solicitud del explorador `http://localhost:xxxxx/Movies/Index`. El resultado es una lista vacía de películas, ya que aún no ha agregado ninguna.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Crear una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles sobre una película y, a continuación, haga clic en el botón **crear** .

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Al hacer clic en el botón **crear** , el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la dirección URL de */Movies* , donde podrá ver la película recién creada en la lista.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra el archivo *Controllers\MoviesController.CS* y examine el método de `Index` generado. A continuación se muestra una parte del controlador de película con el método `Index`.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La siguiente línea de la clase `MoviesController` crea una instancia de un contexto de la base de datos Movie, tal como se ha descrito anteriormente. Puede usar el contexto de la base de datos de películas para consultar, editar y eliminar películas.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una solicitud al controlador de `Movies` devuelve todas las entradas de la tabla `Movies` de la base de datos de películas y, a continuación, pasa los resultados a la vista `Index`.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fuertemente tipados y la palabra clave @model

Anteriormente en este tutorial, vio cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante el objeto `ViewBag`. La `ViewBag` es un objeto dinámico que proporciona una cómoda manera de enlazar información a una vista.

ASP.NET MVC también proporciona la capacidad de pasar datos o objetos fuertemente tipados a una plantilla de vista. Este enfoque fuertemente tipado permite una mejor comprobación en tiempo de compilación del código y IntelliSense más enriquecida en el editor de Visual Web Developer. Estamos usando este enfoque con la clase `MoviesController` y la plantilla de vista *index. cshtml* .

Observe cómo el código crea un objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) cuando llama al método auxiliar `View` en el método de acción `Index`. A continuación, el código pasa esta `Movies` lista del controlador a la vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Al incluir una instrucción `@model` en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera la vista. Al crear el controlador de película, Visual Web Developer incluye automáticamente la siguiente instrucción `@model` en la parte superior del archivo *index. cshtml* :

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta directiva de `@model` permite tener acceso a la lista de películas que el controlador pasó a la vista mediante un objeto de `Model` fuertemente tipado. Por ejemplo, en la plantilla *index. cshtml* , el código recorre en bucle las películas mediante una instrucción `foreach` sobre el objeto `Model` fuertemente tipado:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Dado que el objeto `Model` está fuertemente tipado (como un objeto `IEnumerable<Movie>`), cada `item` objeto del bucle se escribe como `Movie`. Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y la compatibilidad completa con IntelliSense en el editor de código:

[![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntelliSense")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Trabajar con SQL Server Compact

Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó apuntaba a una base de datos de `Movies` que todavía no existía, por lo que Code First creó la base de datos automáticamente. Para comprobar que se ha creado, busque en la carpeta *App\_Data* . Si no ve el archivo *movies. sdf* , haga clic en el botón **Mostrar todos los archivos** en la barra de herramientas **Explorador de soluciones** , haga clic en el botón **Actualizar** y, a continuación, expanda la carpeta *App\_Data* .

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Haga doble clic en *movies. sdf* para abrir **Explorador de servidores**. A continuación, expanda la carpeta **tablas** para ver las tablas que se han creado en la base de datos.

> [!NOTE]
> Si recibe un error al hacer doble clic en *movies. sdf*, asegúrese de que ha instalado [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con el motor de tiempo de ejecución y las herramientas). (Para obtener vínculos al software, consulte la lista de requisitos previos en la primera parte de esta serie de tutoriales). Si instala la versión ahora, tendrá que cerrar y volver a abrir Visual Web Developer.

[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Hay dos tablas, una para el conjunto de entidades `Movie` y, a continuación, la tabla `EdmMetadata`. El Entity Framework usa la tabla `EdmMetadata` para determinar cuándo el modelo y la base de datos no están sincronizados.

Haga clic con el botón secundario en la tabla `Movies` y seleccione **Mostrar datos de tabla** para ver los datos que ha creado.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Haga clic con el botón secundario en la tabla `Movies` y seleccione **Editar esquema de tabla**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Observe cómo se asigna el esquema de la tabla `Movies` a la clase `Movie` que creó anteriormente. Entity Framework Code First crea automáticamente este esquema en función de la clase `Movie`.

Cuando haya terminado, cierre la conexión. (Si no cierra la conexión, es posible que reciba un error la próxima vez que ejecute el proyecto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Ahora tiene la base de datos y una página de lista simple para mostrar su contenido. En el siguiente tutorial, examinaremos el resto del código con scaffolding y agregaremos un método `SearchIndex` y una vista `SearchIndex` que le permita buscar películas en esta base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
