---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: Crear clases de modelo con LINQ to SQL (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear el modelo c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 88a5f1037d93ef3bdc95bf60b6005ebb254ab440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469531"
---
# <a name="creating-model-classes-with-linq-to-sql-vb"></a>Crear clases de modelo con LINQ to SQL (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear clases de modelo y a realizar el acceso a la base de datos aprovechando las ventajas de Microsoft LINQ to SQL.

El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, aprenderá a crear clases de modelo y a realizar el acceso a la base de datos aprovechando las ventajas de Microsoft LINQ to SQL.

En este tutorial, se crea una aplicación de base de datos de películas básica. Comenzaremos por crear la aplicación de base de datos de películas de la manera más rápida y sencilla posible. Se realiza todo el acceso a los datos directamente desde las acciones del controlador.

A continuación, aprenderá a usar el patrón de repositorio. El uso del patrón de repositorio requiere un poco más de trabajo. Sin embargo, la ventaja de adoptar este patrón es que le permite compilar aplicaciones que se pueden adaptar para cambiar y se pueden probar fácilmente.

## <a name="what-is-a-model-class"></a>¿Qué es una clase de modelo?

Un modelo de MVC contiene toda la lógica de la aplicación que no está incluida en una vista de MVC o en un controlador de MVC. En concreto, un modelo de MVC contiene toda la lógica de acceso a datos y la empresa de la aplicación.

Puede usar diversas tecnologías diferentes para implementar la lógica de acceso a datos. Por ejemplo, puede compilar las clases de acceso a datos mediante las clases Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

En este tutorial, se usa LINQ to SQL para consultar y actualizar la base de datos. LINQ to SQL proporciona un método muy sencillo de interactuar con una base de datos de Microsoft SQL Server. Sin embargo, es importante comprender que el marco de MVC de ASP.NET no está asociado a LINQ to SQL de ningún modo. ASP.NET MVC es compatible con cualquier tecnología de acceso a datos.

## <a name="create-a-movie-database"></a>Crear una base de datos de películas

En este tutorial, para ilustrar cómo puede crear clases de modelo, se crea una aplicación de base de datos de película simple. El primer paso es crear una nueva base de datos. Haga clic con el botón derecho en la carpeta\_de datos de la aplicación en la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento**. Seleccione la plantilla de base de datos SQL Server, asígnele el nombre MoviesDB. MDF y haga clic en el botón **Agregar** (vea la figura 1).

[![agregar una nueva base de datos de SQL Server](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Figura 01**: agregar una nueva base de datos de SQL Server ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))

Después de crear la nueva base de datos, puede abrir la base de datos haciendo doble clic en el archivo MoviesDB. MDF en la carpeta app\_Data. Al hacer doble clic en el archivo MoviesDB. MDF, se abre la ventana Explorador de servidores (vea la figura 2).

|   | La ventana de Explorador de servidores se denomina ventana de Explorador de bases de datos al usar Visual Web Developer. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![mediante la ventana de Explorador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Figura 02**: uso de la ventana de explorador de servidores ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))

Necesitamos agregar una tabla a nuestra base de datos que represente las películas. Haga clic con el botón secundario en la carpeta tablas y seleccione la opción de menú **Agregar nueva tabla**. Al seleccionar esta opción de menú, se abre el Diseñador de tablas (vea la figura 3).

[![mediante la ventana de Explorador de servidores](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Figura 03**: el diseñador de tablas ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))

Necesitamos agregar las siguientes columnas a la tabla de base de datos:

| **Nombre de la columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | Nvarchar(200) | False |
| Director | Nvarchar (50) | False |

Debe hacer dos cosas especiales en la columna ID. En primer lugar, debe marcar la columna ID como columna de clave principal seleccionando la columna en el Diseñador de tablas y haciendo clic en el icono de una tecla. LINQ to SQL requiere que especifique las columnas de clave principal al realizar inserciones o actualizaciones en la base de datos.

A continuación, debe marcar la columna ID como columna de identidad mediante la asignación del valor YES a la propiedad **is Identity** (vea la figura 3). Una columna de identidad es una columna a la que se asigna un nuevo número automáticamente cada vez que se agrega una nueva fila de datos a una tabla.

Después de realizar estos cambios, guarde la tabla con el nombre tblMovie. Puede guardar la tabla haciendo clic en el botón Guardar.

## <a name="create-linq-to-sql-classes"></a>Crear clases de LINQ to SQL

Nuestro modelo de MVC contendrá LINQ to SQL clases que representan la tabla de base de datos tblMovie. La forma más fácil de crear estas clases de LINQ to SQL es hacer clic con el botón derecho en la carpeta modelos, seleccionar **Agregar, nuevo elemento**, seleccionar la plantilla clases de LINQ to SQL, dar a las clases el nombre Movie. dbml y hacer clic en el botón **Agregar** (consulte la figura 4).

[![crear clases de LINQ to SQL](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Figura 04**: creación de LINQ to SQL clases ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))

Inmediatamente después de crear las clases de LINQ to SQL de película, aparece el Object Relational Designer. Puede arrastrar las tablas de base de datos desde la ventana de Explorador de servidores hasta el Object Relational Designer para crear LINQ to SQL clases que representen tablas de base de datos determinadas. Necesitamos agregar la tabla de base de datos tblMovie en el Object Relational Designer (consulte la figura 4).

[![mediante el Object Relational Designer](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Figura 05**: uso del Object Relational Designer ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))

De forma predeterminada, la Object Relational Designer crea una clase con el mismo nombre que la tabla de base de datos que se arrastra al diseñador. Sin embargo, no queremos llamar a nuestra clase tblMovie. Por lo tanto, haga clic en el nombre de la clase en el diseñador y cambie el nombre de la clase a Movie.

Por último, recuerde hacer clic en el botón **Guardar** (la imagen del disquete) para guardar las clases de LINQ to SQL. De lo contrario, el Object Relational Designer no generará las clases de LINQ to SQL.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usar LINQ to SQL en una acción de controlador

Ahora que tenemos las clases de LINQ to SQL, podemos usar estas clases para recuperar datos de la base de datos. En esta sección, aprenderá a usar clases de LINQ to SQL directamente dentro de una acción de controlador. Mostraremos la lista de películas de la tabla de base de datos tblMovies en una vista de MVC.

En primer lugar, es necesario modificar la clase HomeController. Esta clase se puede encontrar en la carpeta Controllers de la aplicación. Modifique la clase para que se parezca a la clase de la lista 1.

**Lista 1: `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

La acción index () de la lista 1 utiliza una LINQ to SQL clase DataContext (MovieDataContext) para representar la base de datos MoviesDB. La clase MoveDataContext se generó mediante el Object Relational Designer de Visual Studio.

Se realiza una consulta LINQ en el DataContext para recuperar todas las películas de la tabla de base de datos tblMovies. La lista de películas se asigna a una variable local denominada movies. Por último, la lista de películas se pasa a la vista a través de los datos de la vista.

Para mostrar las películas, es necesario modificar la vista de índice. Puede encontrar la vista de índice en la carpeta Views\Home\. Actualice la vista de índice para que se parezca a la vista de la lista 2.

**Lista 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Observe que la vista de índice modificada incluye una &lt;% @ Import namespace%&gt; Directiva en la parte superior de la vista. Esta directiva importa el espacio de nombres MvcApplication1. Necesitamos este espacio de nombres para trabajar con las clases de modelo (en particular, la clase de película) en la vista.

La vista de la lista 2 contiene un bucle for each que recorre en iteración todos los elementos representados por la propiedad ViewData. Model. El valor de la propiedad título se muestra para cada película.

Observe que el valor de la propiedad ViewData. Model se convierte en IEnumerable. Esto es necesario para recorrer el contenido de ViewData. Model. Otra opción consiste en crear una vista fuertemente tipada. Cuando se crea una vista fuertemente tipada, la propiedad ViewData. Model se convierte en un tipo determinado en la clase de código subyacente de una vista.

Si ejecuta la aplicación después de modificar la clase HomeController y la vista de índice, obtendrá una página en blanco. Obtendrá una página en blanco porque no hay ningún registro de película en la tabla de base de datos tblMovies.

Para agregar registros a la tabla de base de datos tblMovies, haga clic con el botón secundario en la tabla de base de datos tblMovies de la ventana Explorador de servidores (Explorador de bases de datos ventana de Visual Web Developer) y seleccione la opción de menú **Mostrar datos de tabla**. Puede insertar registros de películas mediante la cuadrícula que aparece (vea la figura 5).

[![insertar películas](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Figura 06**: inserción de películas ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))

Después de agregar algunos registros de base de datos a la tabla tblMovies y ejecutar la aplicación, verá la página en la figura 7. Todos los registros de la base de datos de películas se muestran en una lista con viñetas.

[![visualización de películas con la vista de índice](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Figura 07**: visualización de películas con la vista de índice ([haga clic para ver la imagen de tamaño completo](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))

## <a name="using-the-repository-pattern"></a>Uso del patrón de repositorio

En la sección anterior, usamos LINQ to SQL clases directamente dentro de una acción de controlador. Usamos la clase MovieDataContext directamente desde la acción del controlador index (). En el caso de una aplicación simple, no hay ningún problema. Sin embargo, trabajar directamente con LINQ to SQL en una clase de controlador crea problemas cuando es necesario crear una aplicación más compleja.

El uso de LINQ to SQL dentro de una clase de controlador dificulta el cambio de las tecnologías de acceso a datos en el futuro. Por ejemplo, podría decidir pasar de usar Microsoft LINQ to SQL a usar Microsoft Entity Framework como tecnología de acceso a datos. En ese caso, deberá volver a escribir todos los controladores que tengan acceso a la base de datos dentro de la aplicación.

El uso de LINQ to SQL dentro de una clase de controlador también dificulta la compilación de pruebas unitarias para la aplicación. Normalmente, no desea interactuar con una base de datos al realizar pruebas unitarias. Quiere usar las pruebas unitarias para probar la lógica de la aplicación y no el servidor de base de datos.

Para compilar una aplicación MVC que sea más adaptable al cambio futuro y que se pueda probar más fácilmente, debería considerar la posibilidad de usar el patrón de repositorio. Cuando se usa el patrón de repositorio, se crea una clase de repositorio independiente que contiene toda la lógica de acceso a la base de datos.

Cuando se crea la clase de repositorio, se crea una interfaz que representa todos los métodos usados por la clase de repositorio. Dentro de los controladores, escriba el código en la interfaz en lugar del repositorio. De este modo, puede implementar el repositorio mediante distintas tecnologías de acceso a datos en el futuro.

La interfaz de la lista 3 se denomina IMovieRepository y representa un único método denominado ListAll ().

**Lista 3: `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

La clase Repository de la lista 4 implementa la interfaz IMovieRepository. Observe que contiene un método denominado ListAll () que corresponde al método requerido por la interfaz IMovieRepository.

**Lista 4: `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Por último, la clase MoviesController de la lista 5 usa el patrón de repositorio. Ya no utiliza clases de LINQ to SQL directamente.

**Lista 5: `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Observe que la clase MoviesController de la lista 5 tiene dos constructores. El primer constructor, el constructor sin parámetros, se llama cuando se ejecuta la aplicación. Este constructor crea una instancia de la clase MovieRepository y la pasa al segundo constructor.

El segundo constructor tiene un parámetro único: un parámetro IMovieRepository. Este constructor simplemente asigna el valor del parámetro a un campo de nivel de clase denominado \_repositorio.

La clase MoviesController aprovecha un patrón de diseño de software denominado patrón de inserción de dependencias. En concreto, se usa algo denominado inyección de dependencia de constructor. Para obtener más información sobre este patrón, lea el siguiente artículo de Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Tenga en cuenta que todo el código de la clase MoviesController (con la excepción del primer constructor) interactúa con la interfaz IMovieRepository en lugar de la clase MovieRepository real. El código interactúa con una interfaz abstracta en lugar de una implementación concreta de la interfaz.

Si desea modificar la tecnología de acceso a datos utilizada por la aplicación, puede simplemente implementar la interfaz IMovieRepository con una clase que use la tecnología de acceso a bases de datos alternativa. Por ejemplo, podría crear una clase EntityFrameworkMovieRepository o una clase SubSonicMovieRepository. Dado que la clase de controlador se programa en la interfaz, puede pasar una nueva implementación de IMovieRepository a la clase de controlador y la clase seguiría funcionando.

Además, si desea probar la clase MoviesController, puede pasar una clase de repositorio de películas falsa a MoviesController. Puede implementar la clase IMovieRepository con una clase que no tenga acceso realmente a la base de datos, pero que contenga todos los métodos necesarios de la interfaz IMovieRepository. De este modo, puede realizar una prueba unitaria de la clase MoviesController sin tener acceso realmente a una base de datos real.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era mostrar cómo puede crear clases de modelo de MVC aprovechando las ventajas de Microsoft LINQ to SQL. Hemos examinado dos estrategias para mostrar los datos de la base de datos en una aplicación ASP.NET MVC. En primer lugar, creamos LINQ to SQL clases y usaban las clases directamente dentro de una acción de controlador. El uso de LINQ to SQL clases dentro de un controlador permite mostrar los datos de la base de datos de forma rápida y sencilla en una aplicación MVC.

A continuación, se ha explorado un poco más difícil, pero uso realmente virtuosomente más, la ruta de acceso para mostrar los datos de la base de datos. Aprovechamos el patrón del repositorio y colocamos toda la lógica de acceso a la base de datos en una clase de repositorio independiente. En nuestro controlador, escribimos todo el código en una interfaz en lugar de una clase concreta. La ventaja del modelo de repositorio es que nos permite cambiar fácilmente las tecnologías de acceso a bases de datos en el futuro y nos permite probar fácilmente las clases de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-vb.md)
> [Siguiente](displaying-a-table-of-database-data-vb.md)
