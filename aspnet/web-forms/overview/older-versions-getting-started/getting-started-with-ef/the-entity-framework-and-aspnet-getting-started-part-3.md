---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 3 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 88debb11a9157dce9ff000b1cb459b876dbceaf3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78522643"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 3

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="filtering-ordering-and-grouping-data"></a>Filtrar, ordenar y agrupar datos

En el tutorial anterior, usó el control `EntityDataSource` para mostrar y editar datos. En este tutorial, filtrará, ordenará y agrupará los datos. Al hacer esto estableciendo las propiedades del control `EntityDataSource`, la sintaxis es diferente de otros controles de origen de datos. Como verá, sin embargo, puede usar el control `QueryExtender` para minimizar estas diferencias.

Cambiará la página *Students. aspx* para filtrar a los estudiantes, ordenar por nombre y buscar por nombre. También cambiará la página *Courses. aspx* para mostrar los cursos del Departamento seleccionado y buscar cursos por nombre. Por último, agregará estadísticas de estudiante a la página *About. aspx* .

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Usar la propiedad EntityDataSource "where" para filtrar los datos

Abra la página *Students. aspx* que creó en el tutorial anterior. Como está configurado actualmente, el control de `GridView` en la página muestra todos los nombres del conjunto de entidades `People`. Sin embargo, desea mostrar solo los estudiantes, que puede buscar seleccionando `Person` entidades que tienen fechas de inscripción no nulas.

Cambie a la vista de **diseño** y seleccione el control `EntityDataSource`. En la ventana **Propiedades** , establezca la propiedad `Where` en `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintaxis que se usa en la propiedad `Where` del control `EntityDataSource` es Entity SQL. Entity SQL es similar a Transact-SQL, pero se ha personalizado para su uso con entidades en lugar de objetos de base de datos. En el `it.EnrollmentDate is not null`de la expresión, la palabra `it` representa una referencia a la entidad devuelta por la consulta. Por lo tanto, `it.EnrollmentDate` hace referencia a la propiedad `EnrollmentDate` de la entidad `Person` que devuelve el control `EntityDataSource`.

Ejecute la página. La lista Students ahora solo contiene estudiantes. (No se muestra ninguna fila en la que no hay ninguna fecha de inscripción).

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Usar la propiedad EntityDataSource "OrderBy" para ordenar los datos

También desea que esta lista esté en orden de nombres cuando se muestra por primera vez. Con la página *Students. aspx* todavía abierta en la vista **diseño** y, con el control `EntityDataSource` aún seleccionado, en la ventana **propiedades** , establezca la propiedad **OrderBy** en `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Ejecute la página. La lista Students ahora está ordenada por Apellido.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Usar un parámetro de control para establecer la propiedad "where"

Como con otros controles de origen de datos, puede pasar valores de parámetro a la propiedad `Where`. En la página *Courses. aspx* que creó en la parte 2 del tutorial, puede usar este método para mostrar los cursos asociados al Departamento que un usuario selecciona en la lista desplegable.

Abra *Courses. aspx* y cambie a la vista **diseño** . Agregue un segundo control `EntityDataSource` a la página y asígnele el nombre `CoursesEntityDataSource`. Conéctelo al modelo de `SchoolEntities` y seleccione `Courses` como el valor de **entitySetName** .

En la ventana **propiedades** , haga clic en los puntos suspensivos del cuadro de propiedades **Where** . (Asegúrese de que el control `CoursesEntityDataSource` todavía está seleccionado antes de usar la ventana **propiedades** ).

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Se muestra el cuadro de diálogo **Editor de expresiones** . En este cuadro de diálogo, seleccione **generar automáticamente la expresión Where según los parámetros proporcionados**y, a continuación, haga clic en **Agregar parámetro**. Asigne al parámetro el nombre `DepartmentID`, seleccione **control** como valor de **origen del parámetro** y seleccione **DepartmentsDropDownList** como valor de **ControlID** .

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Haga clic en **Mostrar propiedades avanzadas**y, en la ventana **propiedades** del cuadro de diálogo **Editor de expresiones** , cambie la propiedad `Type` a `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Cuando haya terminado, haga clic en **Aceptar**.

Debajo de la lista desplegable, agregue un control `GridView` a la página y asígnele el nombre `CoursesGridView`. Conéctelo al control de origen de datos `CoursesEntityDataSource`, haga clic en **actualizar esquema**, haga clic en **Editar columnas**y quite la columna `DepartmentID`. El marcado del control `GridView` es similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Cuando el usuario cambia el Departamento seleccionado en la lista desplegable, desea que la lista de cursos asociados cambie automáticamente. Para que esto suceda, seleccione la lista desplegable y, en la ventana **propiedades** , establezca la propiedad `AutoPostBack` en `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ahora que ha terminado de usar el diseñador, cambie a la vista **código fuente** y reemplace las propiedades `ConnectionString` y `DefaultContainer` nombre del control `CoursesEntityDataSource` con el atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Cuando haya terminado, el marcado del control será similar al del ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Ejecute la página y use la lista desplegable para seleccionar distintos departamentos. En el control `GridView` solo se muestran los cursos que ofrece el Departamento seleccionado.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Usar la propiedad EntityDataSource "GroupBy" para agrupar datos

Supongamos que contoso University quiere incluir algunas estadísticas de cuerpo de estudiante en su página acerca de. En concreto, desea mostrar un desglose de números de estudiantes en la fecha en que se inscribieron.

Abra *About. aspx*y, en la vista **código fuente** , reemplace el contenido existente del control `BodyContent` por "Student Body statistics" entre `h2` Etiquetas:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Después del encabezado, agregue un control `EntityDataSource` y asígnele el nombre `StudentStatisticsEntityDataSource`. Conéctela a `SchoolEntities`, seleccione el conjunto de entidades `People` y deje el cuadro **seleccionar** del asistente sin cambios. Establezca las siguientes propiedades en la ventana **propiedades** :

- Para filtrar solo a los estudiantes, establezca la propiedad `Where` en `it.EnrollmentDate is not null`.
- Para agrupar los resultados por la fecha de inscripción, establezca la propiedad `GroupBy` en `it.EnrollmentDate`.
- Para seleccionar la fecha de inscripción y el número de estudiantes, establezca la propiedad `Select` en `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Para ordenar los resultados por la fecha de inscripción, establezca la propiedad `OrderBy` en `it.EnrollmentDate`.

En la vista **código fuente** , reemplace las propiedades `ConnectionString` y `DefaultContainer` nombre por una propiedad `ContextTypeName`. El marcado del control `EntityDataSource` ahora es similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintaxis de las propiedades `Select`, `GroupBy`y `Where` es similar a Transact-SQL, a excepción de la palabra clave `it` que especifica la entidad actual.

Agregue el marcado siguiente para crear un control de `GridView` para mostrar los datos.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Ejecute la página para ver una lista que muestra el número de alumnos por fecha de inscripción.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Usar el control QueryExtender para filtrar y ordenar

El control `QueryExtender` proporciona una manera de especificar el filtrado y la ordenación en el marcado. La sintaxis es independiente del sistema de administración de bases de datos (DBMS) que está usando. También es normalmente independiente del Entity Framework, con la excepción de que la sintaxis que se usa para las propiedades de navegación es única en el Entity Framework.

En esta parte del tutorial, usará un control de `QueryExtender` para filtrar y ordenar los datos, y uno de los campos order by será una propiedad de navegación.

(Si prefiere utilizar código en lugar de marcado para extender las consultas generadas automáticamente por el control `EntityDataSource`, puede hacerlo controlando el evento `QueryCreated`. Así es como el control `QueryExtender` amplía también `EntityDataSource` las consultas de control).

Abra la página *Courses. aspx* y debajo del marcado que agregó anteriormente, inserte el marcado siguiente para crear un encabezado, un cuadro de texto para escribir cadenas de búsqueda, un botón de búsqueda y un control de `EntityDataSource` enlazado al conjunto de entidades `Courses`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Observe que la propiedad `Include` del control `EntityDataSource` está establecida en `Department`. En la base de datos, la tabla `Course` no contiene el nombre del Departamento; contiene una `DepartmentID` columna de clave externa. Si estaba consultando la base de datos directamente, para obtener el nombre del Departamento junto con los datos del curso, tendría que combinar las tablas `Course` y `Department`. Al establecer la propiedad `Include` en `Department`, se especifica que el Entity Framework debe hacer el trabajo de obtener la entidad de `Department` relacionada cuando obtiene una entidad de `Course`. A continuación, la entidad `Department` se almacena en la propiedad de navegación `Department` de la entidad `Course`. (De forma predeterminada, la clase `SchoolEntities` generada por el diseñador de modelos de datos recupera los datos relacionados cuando es necesario y se ha enlazado el control de origen de datos a esa clase, por lo que no es necesario establecer la propiedad `Include`. Sin embargo, si se establece, se mejora el rendimiento de la página, ya que de lo contrario el Entity Framework realizar llamadas independientes a la base de datos para recuperar los datos de las entidades `Course` y de las entidades `Department` relacionadas).

Después del control de `EntityDataSource` que acaba de crear, inserte el marcado siguiente para crear un `QueryExtender` control que esté enlazado a ese control de `EntityDataSource`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

El elemento `SearchExpression` especifica que desea seleccionar los cursos cuyos títulos coinciden con el valor especificado en el cuadro de texto. Solo se compararán tantos caracteres como se escriban en el cuadro de texto, porque la propiedad `SearchType` especifica `StartsWith`.

El elemento `OrderByExpression` especifica que el conjunto de resultados se ordenará por el título del curso dentro del nombre del Departamento. Observe cómo se especifica el nombre de Departamento: `Department.Name`. Dado que la asociación entre la entidad `Course` y la entidad `Department` es de uno a uno, la propiedad de navegación `Department` contiene una entidad `Department`. (Si se trata de una relación de uno a varios, la propiedad incluiría una colección). Para obtener el nombre del Departamento, debe especificar la propiedad `Name` de la entidad `Department`.

Por último, agregue un control `GridView` para mostrar la lista de cursos:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La primera columna es un campo de plantilla que muestra el nombre del Departamento. La expresión DataBinding especifica `Department.Name`, tal como se vio en el control `QueryExtender`.

Ejecute la página. En la pantalla inicial se muestra una lista de todos los cursos en orden por departamento y, a continuación, por título del curso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Escriba una "m" y haga clic en **Buscar** para ver todos los cursos cuyos títulos empiecen por "m" (la búsqueda no distingue mayúsculas de minúsculas).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Usar el operador "like" para filtrar datos

Puede lograr un efecto similar al de los tipos de búsqueda `StartsWith`, `Contains`y `EndsWith` del control `QueryExtender` mediante el uso de un operador `Like` en la propiedad `EntityDataSource` del control `Where`. En esta parte del tutorial, verá cómo usar el operador `Like` para buscar un estudiante por nombre.

Abra *Students. aspx* en la vista **código fuente** . Después del control de `GridView`, agregue el siguiente marcado:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Este marcado es similar a lo que se ha visto anteriormente, excepto por el valor de la propiedad `Where`. La segunda parte de la expresión `Where` define una búsqueda de subcadena (`LIKE %FirstMidName% or LIKE %LastName%`) que busca el nombre y los apellidos de lo que se especifique en el cuadro de texto.

Ejecute la página. Inicialmente, verá todos los alumnos porque el valor predeterminado del parámetro `StudentName` es "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Escriba la letra "g" en el cuadro de texto y haga clic en **Buscar**. Verá una lista de estudiantes que tienen una "g" en el nombre o en el apellido.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Ahora ha mostrado, actualizado, filtrado, ordenado y agrupado los datos de las tablas individuales. En el siguiente tutorial, comenzará a trabajar con datos relacionados (escenarios principales-detalle).

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-4.md)
