---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Crear clases de modelo con el Entity FrameworkC#() | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Aprenderá a usar el Asistente para entidades para crear una entidad ADO.NET...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 2e0e365c287fc455015d237ea466301335805d14
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469441"
---
# <a name="creating-model-classes-with-the-entity-framework-c"></a>Crear clases de modelo con Entity Framework (C#)

por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Aprenderá a usar el Asistente para entidades para crear una Entity Data Model ADO.NET. En el transcurso de este tutorial, se crea una aplicación web que muestra cómo seleccionar, insertar, actualizar y eliminar datos de la base de datos mediante el Entity Framework.

El objetivo de este tutorial es explicar cómo puede crear clases de acceso a datos mediante Microsoft Entity Framework al compilar una aplicación ASP.NET MVC. En este tutorial se supone que no tiene ningún conocimiento previo de Microsoft Entity Framework. Al final de este tutorial, aprenderá a usar el Entity Framework para seleccionar, insertar, actualizar y eliminar registros de la base de datos.

Microsoft Entity Framework es una herramienta de asignación relacional de objetos (O/RM) que permite generar automáticamente una capa de acceso a datos a partir de una base de datos. El Entity Framework le permite evitar el trabajo tedioso de crear clases de acceso a datos a mano.

Para ilustrar cómo puede usar Microsoft Entity Framework con ASP.NET MVC, crearemos una sencilla aplicación de ejemplo. Vamos a crear una aplicación de base de datos de películas que le permita mostrar y editar los registros de la base de datos de películas.

En este tutorial se supone que tiene Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. Necesita el Service Pack 1 para poder usar el Entity Framework. Puede descargar Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 desde la siguiente dirección:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)

> [!NOTE] 
> 
> No hay ninguna conexión esencial entre ASP.NET MVC y Microsoft Entity Framework. Hay varias alternativas a la Entity Framework que puede usar con ASP.NET MVC. Por ejemplo, puede compilar las clases de modelo de MVC con otras herramientas de O/RM como Microsoft LINQ to SQL, NHibernate o Subsonic.

## <a name="creating-the-movie-sample-database"></a>Crear la base de datos de ejemplo Movie

La aplicación de base de datos de películas utiliza una tabla de base de datos denominada películas que contiene las columnas siguientes:

| Nombre de columna | Tipo de datos | ¿Permitir valores NULL? | ¿Es la clave principal? |
| --- | --- | --- | --- |
| Id. | int | False | True |
| Título | nvarchar(100) | False | False |
| Director | nvarchar(100) | False | False |

Puede Agregar esta tabla a un proyecto de ASP.NET MVC siguiendo estos pasos:

1. Haga clic con el botón derecho en la carpeta\_de datos de la aplicación en la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento.**
2. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **SQL Server base de datos**, asigne al nombre de la base de datos el nombre MoviesDB. MDF y haga clic en el botón **Agregar** .
3. Haga doble clic en el archivo MoviesDB. MDF para abrir la ventana Explorador de servidores/Explorador de bases de datos.
4. Expanda la conexión de base de datos MoviesDB. MDF, haga clic con el botón secundario en la carpeta tablas y seleccione la opción de menú **Agregar nueva tabla**.
5. En el Diseñador de tablas, agregue las columnas ID, title y director.
6. Haga clic en el botón **Guardar** (tiene el icono del disquete) para guardar la nueva tabla con el nombre movies.

Después de crear la tabla de base de datos de películas, debe agregar algunos datos de ejemplo a la tabla. Haga clic con el botón secundario en la tabla películas y seleccione la opción de menú **Mostrar datos de tabla**. Puede escribir datos de película falsos en la cuadrícula que aparece.

## <a name="creating-the-adonet-entity-data-model"></a>Crear la Entity Data Model ADO.NET

Para usar el Entity Framework, debe crear un Entity Data Model. Puede aprovechar las ventajas del *Asistente para Entity Data Model* de Visual Studio para generar un Entity Data Model a partir de una base de datos automáticamente.

Siga estos pasos:

1. Haga clic con el botón secundario en la carpeta modelos de la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento**.
2. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la categoría de datos (vea la ilustración 1).
3. Seleccione la plantilla **ADO.NET Entity Data Model** , asigne al Entity Data Model el nombre MoviesDBModel. edmx y haga clic en el botón **Agregar** . Al hacer clic en el botón **Agregar** se inicia el Asistente para modelo de datos.
4. En el paso **elegir contenido del modelo** , elija la opción **generar desde una base de datos** y haga clic en el botón **siguiente** (consulte la figura 2).
5. En el paso **elegir la conexión de datos** , seleccione la conexión de base de datos MoviesDB. MDF, escriba el nombre de configuración de la conexión de entidades MoviesDBEntities y haga clic en el botón **siguiente** (vea la figura 3).
6. En el paso **elegir los objetos de base de datos** , seleccione la tabla de la base de datos de películas y haga clic en el botón **Finalizar** (consulte la figura 4).

Después de completar estos pasos, se abre ADO.NET Entity Data Model Designer (Entity Designer).

**Figura 1: creación de una nueva Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Ilustración 2: paso elegir el contenido del modelo**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Figura 3: selección de la conexión de datos**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Ilustración 4: elegir los objetos de base de datos**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modificar el Entity Data Model ADO.NET

Después de crear un Entity Data Model, puede modificar el modelo mediante el uso de Entity Designer (vea la figura 5). Puede abrir el diseñador de entidades en cualquier momento si hace doble clic en el archivo MoviesDBModel. edmx incluido en la carpeta modelos dentro de la ventana de Explorador de soluciones.

**Figura 5: diseñador de Entity Data Model de ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Por ejemplo, puede usar Entity Designer para cambiar los nombres de las clases generadas por el Asistente para datos del modelo de entidad. El asistente creó una nueva clase de acceso a datos denominada movies. En otras palabras, el asistente asignó a la clase el mismo nombre que la tabla de base de datos. Dado que usaremos esta clase para representar una instancia de película determinada, deberíamos cambiar el nombre de la clase de películas a película.

Si desea cambiar el nombre de una clase de entidad, puede hacer doble clic en el nombre de clase en Entity Designer y escribir un nuevo nombre (vea la figura 6). Como alternativa, puede cambiar el nombre de una entidad en el ventana Propiedades después de seleccionar una entidad en Entity Designer.

**Ilustración 6: cambiar el nombre de una entidad**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

Recuerde guardar el Entity Data Model después de realizar una modificación; para ello, haga clic en el botón Guardar (el icono del disquete). En segundo plano, Entity Designer genera un conjunto de C# clases. Puede ver estas clases abriendo el archivo MoviesDBModel.Designer.cs desde la ventana Explorador de soluciones.

No modifique el código en el archivo Designer.cs, ya que los cambios se sobrescribirán la próxima vez que use el diseñador de entidades. Si desea ampliar la funcionalidad de las clases de entidad definidas en el archivo Designer.cs, puede crear *clases parciales* en archivos independientes.

#### <a name="selecting-database-records-with-the-entity-framework"></a>Selección de registros de base de datos con el Entity Framework

Vamos a empezar a crear la aplicación de base de datos de películas mediante la creación de una página que muestra una lista de registros de películas. El controlador Home de la lista 1 expone una acción denominada index (). La acción index () devuelve todos los registros de película de la tabla de la base de datos Movie aprovechando el Entity Framework.

**Lista 1: Controllers\HomeController.cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Observe que el controlador de la lista 1 incluye un constructor. El constructor inicializa un campo de nivel de clase denominado \_dB. El campo \_dB representa las entidades de base de datos generadas por el Entity Framework de Microsoft. El campo \_dB es una instancia de la clase MoviesDBEntities generada por Entity Designer.

Para usar la clase theMoviesDBEntities en el controlador Home, debe importar el espacio de nombres MovieEntityApp. Models (*MVCProjectName*. Modelos).

El campo \_dB se utiliza dentro de la acción index () para recuperar los registros de la tabla de la base de datos movies. Expresión \_dB. MovieSet representa todos los registros de la tabla de base de datos de películas. El método ToList () se usa para convertir el conjunto de películas en una colección genérica de objetos Movie (list&lt;Movie&gt;).

Los registros de la película se recuperan con la ayuda de LINQ to Entities. La acción index () de la lista 1 utiliza la *Sintaxis del método* LINQ para recuperar el conjunto de registros de la base de datos. Si lo prefiere, puede usar la *Sintaxis de consulta* LINQ en su lugar. Las dos instrucciones siguientes hacen lo mismo:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Use la sintaxis de LINQ: sintaxis de método o sintaxis de consulta, que le resultará más intuitiva. No hay ninguna diferencia en el rendimiento entre los dos enfoques: la única diferencia es el estilo.

La vista de la lista 2 se usa para mostrar los registros de la película.

**Lista 2: Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La vista de la lista 2 contiene un bucle **foreach** que recorre en iteración cada registro de película y muestra los valores de las propiedades del título y del director del registro de la película. Observe que se muestra un vínculo de edición y eliminación junto a cada registro. Además, aparece un vínculo agregar película en la parte inferior de la vista (vea la figura 7).

**Ilustración 7: la vista de índice**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La vista de índice es una *vista con tipo*. La vista de índice incluye una directiva &lt;% @ Page%&gt; con un atributo *Inherits* que convierte la propiedad Model en una colección de lista genérica fuertemente tipada de objetos Movie (list&lt;Movie).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertar registros de base de datos con el Entity Framework

Puede utilizar la Entity Framework para facilitar la inserción de nuevos registros en una tabla de base de datos. La lista 3 contiene dos nuevas acciones agregadas a la clase de controlador de inicio que puede usar para insertar nuevos registros en la tabla de la base de datos de películas.

**Lista 3: Controllers\HomeController.cs (agregar métodos)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La primera acción Add () simplemente devuelve una vista. La vista contiene un formulario para agregar un nuevo registro de la base de datos de películas (vea la figura 8). Cuando se envía el formulario, se invoca la segunda acción Add ().

Observe que la segunda acción Add () se decora con el atributo AcceptVerbs. Esta acción solo se puede invocar cuando se realiza una operación POST de HTTP. En otras palabras, esta acción solo se puede invocar al publicar un formulario HTML.

La segunda acción Add () crea una nueva instancia de la clase Entity Framework Movie con la ayuda del método TryUpdateModel MVC ASP.NET (). El método TryUpdateModel () toma los campos del FormCollection que se pasan al método Add () y asigna los valores de estos campos de formulario HTML a la clase Movie.

Al utilizar Entity Framework, debe proporcionar una "lista de permitidos" de propiedades cuando se usan métodos TryUpdateModel o UpdateModel para actualizar las propiedades de una clase de entidad.

A continuación, la acción Add () realiza una validación de formulario simple. La acción comprueba que las propiedades title y director tienen valores. Si se produce un error de validación, se agrega un mensaje de error de validación a ModelState.

Si no hay ningún error de validación, se agrega un nuevo registro de película a la tabla de base de datos de películas con la ayuda del Entity Framework. El nuevo registro se agrega a la base de datos con las dos líneas de código siguientes:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La primera línea de código agrega la nueva entidad Movie al conjunto de películas cuyo seguimiento realiza el Entity Framework. La segunda línea de código guarda los cambios que se hayan realizado en las películas de las que se realiza el seguimiento en la base de datos subyacente.

**Figura 8: la vista agregar**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Actualización de registros de base de datos con el Entity Framework

Puede seguir casi el mismo enfoque para editar un registro de base de datos con el Entity Framework como el enfoque que hemos seguido para insertar un nuevo registro de base de datos. La lista 4 contiene dos nuevas acciones de controlador denominadas Edit (). La primera acción Edit () devuelve un formulario HTML para editar un registro de película. La segunda acción de edición () intenta actualizar la base de datos.

**Lista 4: Controllers\HomeController.cs (editar métodos)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La segunda acción Edit () se inicia al recuperar el registro de la película de la base de datos que coincide con el identificador de la película que se está editando. La siguiente instrucción LINQ to Entities obtiene el primer registro de base de datos que coincide con un identificador determinado:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

A continuación, se usa el método TryUpdateModel () para asignar los valores de los campos de formulario HTML a las propiedades de la entidad Movie. Tenga en cuenta que se proporciona una lista de permitidos para especificar las propiedades exactas para actualizar.

Después, se realiza una validación simple para comprobar que el título de la película y las propiedades del Director tienen valores. Si falta un valor en alguna de las propiedades, se agrega un mensaje de error de validación a ModelState y ModelState. IsValid devuelve el valor false.

Por último, si no hay ningún error de validación, la tabla de base de datos de películas subyacentes se actualiza con los cambios llamando al método SaveChanges ().

Al editar registros de base de datos, debe pasar el identificador del registro que se está editando a la acción de controlador que realiza la actualización de la base de datos. De lo contrario, la acción del controlador no sabrá qué registro se va a actualizar en la base de datos subyacente. La vista de edición, incluida en la lista 5, incluye un campo de formulario oculto que representa el identificador del registro de base de datos que se está editando.

**Lista 5: Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminar registros de base de datos con el Entity Framework

La última operación de base de datos, que necesitamos abordar en este tutorial, es la eliminación de registros de base de datos. Puede usar la acción del controlador en la lista 6 para eliminar un registro de base de datos determinado.

**Listado 6--\Controllers\HomeController.cs (acción de eliminación)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

La acción Delete () recupera primero la entidad Movie que coincide con el identificador pasado a la acción. A continuación, la película se elimina de la base de datos llamando al método DeleteObject () seguido del método SaveChanges (). Por último, se redirige al usuario a la vista de índice.

## <a name="summary"></a>Resumen

El propósito de este tutorial fue demostrar cómo puede compilar aplicaciones web controladas por la base de datos aprovechando ASP.NET MVC y Microsoft Entity Framework. Aprendió a crear una aplicación que le permite seleccionar, insertar, actualizar y eliminar registros de la base de datos.

En primer lugar, se describe cómo puede usar el Asistente para Entity Data Model con el fin de generar una Entity Data Model desde Visual Studio. A continuación, aprenderá a usar LINQ to Entities para recuperar un conjunto de registros de base de datos de una tabla de base de datos. Por último, usamos el Entity Framework para insertar, actualizar y eliminar registros de la base de datos.

> [!div class="step-by-step"]
> [Siguiente](creating-model-classes-with-linq-to-sql-cs.md)
