---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Mostrar una tabla de datos de baseC#de datos () | Microsoft Docs
author: microsoft
description: En este tutorial, se muestran dos métodos para mostrar un conjunto de registros de base de datos. Mostramos dos métodos para dar formato a un conjunto de registros de base de datos en un elemento HTML TA...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c908d030076fc8400190ef3cf1672632ac1ed6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436849"
---
# <a name="displaying-a-table-of-database-data-c"></a>Mostrar una tabla de los datos de la base de datos (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> En este tutorial, se muestran dos métodos para mostrar un conjunto de registros de base de datos. Mostramos dos métodos para dar formato a un conjunto de registros de base de datos en una tabla HTML. En primer lugar, se muestra cómo se puede dar formato a los registros de la base de datos directamente dentro de una vista. A continuación, mostramos cómo puede aprovechar las ventajas de las parciales al dar formato a los registros de la base de datos.

El objetivo de este tutorial es explicar cómo puede mostrar una tabla HTML de datos de base de datos en una aplicación ASP.NET MVC. En primer lugar, aprenderá a usar las herramientas de scaffolding incluidas en Visual Studio para generar una vista que muestre un conjunto de registros automáticamente. A continuación, aprenderá a usar una parcial como plantilla al dar formato a los registros de la base de datos.

## <a name="create-the-model-classes"></a>Crear las clases de modelo

Vamos a mostrar el conjunto de registros de la tabla de la base de datos de películas. La tabla de la base de datos películas contiene las siguientes columnas:

<a id="0.3_table01"></a>

| **Nombre de la columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | Nvarchar(200) | False |
| Director | NVarchar(50) | False |
| DateReleased | DateTime | False |

Para representar la tabla de películas en nuestra aplicación ASP.NET MVC, es necesario crear una clase de modelo. En este tutorial, usamos Microsoft Entity Framework para crear las clases de modelo.

> [!NOTE] 
> 
> En este tutorial, usamos Microsoft Entity Framework. Sin embargo, es importante entender que puede usar diversas tecnologías diferentes para interactuar con una base de datos de una aplicación ASP.NET MVC, como LINQ to SQL, NHibernate o ADO.NET.

Siga estos pasos para iniciar el Asistente para Entity Data Model:

1. Haga clic con el botón secundario en la carpeta modelos de la ventana de Explorador de soluciones y seleccione la opción de menú **Agregar, nuevo elemento**.
2. Seleccione la categoría de **datos** y seleccione la plantilla **ADO.NET Entity Data Model** .
3. Asigne al modelo de datos el nombre *MoviesDBModel. edmx* y haga clic en el botón **Agregar** .

Después de hacer clic en el botón Agregar, aparece el Asistente para Entity Data Model (vea la figura 1). Siga estos pasos para completar el asistente:

1. En el paso **elegir contenido del modelo** , seleccione la opción **generar desde la base de datos** .
2. En el paso **elegir la conexión de datos** , use la conexión de datos *MoviesDB. MDF* y el nombre *MoviesDBEntities* para la configuración de conexión. Haga clic en el botón **Next** (Siguiente).
3. En el paso **Elija los objetos de base de datos** , expanda el nodo tablas y seleccione la tabla películas. Escriba los *modelos* de espacio de nombres y haga clic en el botón **Finalizar** .

[![crear clases de LINQ to SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01**: creación de LINQ to SQL clases ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image2.png))

Después de completar el Asistente para Entity Data Model, se abre el diseñador de Entity Data Model. El diseñador debe mostrar la entidad Movies (vea la ilustración 2).

[![el diseñador de Entity Data Model](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: el diseñador de Entity Data Model ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image4.png))

Necesitamos hacer un cambio antes de continuar. El Asistente para datos de entidad genera una clase de modelo denominada *películas* que representa la tabla de la base de datos de películas. Dado que vamos a usar la clase Movies para representar una película determinada, es necesario modificar el nombre de la clase para que sea *Movie* en lugar de *películas* (singular en lugar de plural).

Haga doble clic en el nombre de la clase en la superficie del diseñador y cambie el nombre de la clase de películas a película. Después de hacer este cambio, haga clic en el botón **Guardar** (el icono del disquete) para generar la clase Movie.

## <a name="create-the-movies-controller"></a>Crear el controlador de películas

Ahora que tenemos una manera de representar los registros de la base de datos, podemos crear un controlador que devuelva la colección de películas. En la ventana de Explorador de soluciones de Visual Studio, haga clic con el botón derecho en la carpeta Controllers y seleccione la opción de menú **Agregar, controlador** (vea la figura 3).

[![el menú Agregar controlador](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: menú Agregar controlador ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image6.png))

Cuando aparezca el cuadro de diálogo **Agregar controlador** , escriba el nombre del controlador MovieController (consulte la figura 4). Haga clic en el botón **Agregar** para agregar el nuevo controlador.

[![el cuadro de diálogo Agregar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: cuadro de diálogo Agregar controlador ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image8.png))

Es necesario modificar la acción de índice () expuesta por el controlador de película para que devuelva el conjunto de registros de la base de datos. Modifique el controlador para que se parezca al controlador en la lista 1.

**Lista 1: Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

En la lista 1, se usa la clase MoviesDBEntities para representar la base de datos MoviesDB. Para usar esta clase, debe importar el espacio de nombres MvcApplication1. Models del modo siguiente:

usar MvcApplication1. Models;

Entidades de expresión *. MovieSet. ToList ()* devuelve el conjunto de todas las películas de la tabla de base de datos de películas.

## <a name="create-the-view"></a>Crear la vista

La forma más fácil de mostrar un conjunto de registros de base de datos en una tabla HTML es aprovechar las ventajas de la técnica scaffolding que proporciona Visual Studio.

Compile la aplicación seleccionando la opción de menú **generar, compilar solución**. Debe compilar la aplicación antes de abrir el cuadro de diálogo **Agregar vista** o las clases de datos no aparecerán en el cuadro de diálogo.

Haga clic con el botón derecho en la acción index () y seleccione la opción de menú **Agregar vista** (vea la figura 5).

[![agregar una vista](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: agregar una vista ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image10.png))

En el cuadro de diálogo **Agregar vista** , active la casilla **crear una vista fuertemente tipada**. Seleccione la clase Movie como **clase de datos**de la vista. Seleccione *lista* como el **contenido** de la vista (vea la figura 6). Al seleccionar estas opciones se generará una vista fuertemente tipada que muestra una lista de películas.

[![el cuadro de diálogo Agregar vista](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: cuadro de diálogo Agregar vista ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image12.png))

Después de hacer clic en el botón **Agregar** , la vista de la lista 2 se genera automáticamente. Esta vista contiene el código necesario para recorrer en iteración la colección de películas y mostrar cada una de las propiedades de una película.

**Lista 2: Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Puede ejecutar la aplicación seleccionando la opción de menú **depurar, iniciar depuración** (o presionando la tecla F5). Al ejecutar la aplicación, se inicia Internet Explorer. Si navega a la dirección URL de/Movie, verá la página en la figura 7.

[![una tabla de películas](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: una tabla de películas ([haga clic para ver la imagen de tamaño completo](displaying-a-table-of-database-data-cs/_static/image14.png))

Si no le gusta nada sobre la apariencia de la cuadrícula de los registros de la base de datos en la figura 7, simplemente puede modificar la vista de índice. Por ejemplo, puede cambiar el encabezado *DateReleased* a *fecha de lanzamiento* modificando la vista de índice.

## <a name="create-a-template-with-a-partial"></a>Crear una plantilla con una parcial

Cuando una vista es demasiado complicada, es una buena idea empezar a dividir la vista en parciales. El uso de particiones hace que las vistas sean más fáciles de entender y mantener. Vamos a crear una parte que se puede usar como plantilla para dar formato a cada uno de los registros de la base de datos de películas.

Siga estos pasos para crear la parcial:

1. Haga clic con el botón secundario en la carpeta Views\Movie y seleccione la opción de menú **Agregar vista**.
2. Active la casilla *crear una vista parcial (. ascx)* .
3. Asigne un nombre a la *MovieTemplate*parcial.
4. Active la casilla de verificación **crear una vista fuertemente tipada**.
5. Seleccione película como *clase de datos*de la vista.
6. Seleccione vacío como el *contenido*de la vista.
7. Haga clic en el botón **Agregar** para agregar la parcial al proyecto.

Después de completar estos pasos, modifique el MovieTemplate parcial para que se parezca a la lista 3.

**Lista 3: Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

La parte de la lista 3 contiene una plantilla para una sola fila de registros.

La vista de índice modificada de la lista 4 utiliza el MovieTemplate parcial.

**Lista 4: Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

La vista de la lista 4 contiene un bucle foreach que recorre en iteración todas las películas. Para cada película, el MovieTemplate parcial se usa para dar formato a la película. El MovieTemplate se representa llamando al método auxiliar RenderPartial ().

La vista de índice modificada representa la misma tabla HTML de registros de base de datos. Sin embargo, la vista se ha simplificado en gran medida.

El método RenderPartial () es diferente de la mayoría de los demás métodos auxiliares porque no devuelve una cadena. Por lo tanto, debe llamar al método RenderPartial () mediante &lt;% HTML. RenderPartial (); %&gt; en lugar de &lt;% = HTML. RenderPartial (); %&gt;.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede mostrar un conjunto de registros de base de datos en una tabla HTML. En primer lugar, aprendió a devolver un conjunto de registros de base de datos a partir de una acción del controlador aprovechando las ventajas de Microsoft Entity Framework. A continuación, aprendió a usar la técnica de scaffolding de Visual Studio para generar una vista que muestre automáticamente una colección de elementos. Por último, aprendió a simplificar la vista aprovechando una parcial. Ha aprendido a usar una parcial como plantilla para que pueda dar formato a cada registro de base de datos.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-cs.md)
> [Siguiente](performing-simple-validation-cs.md)
