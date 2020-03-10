---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 4 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: eb75a76038466bf30738387ed4739687de1df944
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518287"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 4

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="working-with-related-data"></a>Trabajar con datos relacionados

En el tutorial anterior, usó el control `EntityDataSource` para filtrar, ordenar y agrupar los datos. En este tutorial, mostrará y actualizará los datos relacionados.

Creará la página Instructors que muestra una lista de instructores. Al seleccionar un instructor, verá una lista de los cursos impartidos por ese instructor. Al seleccionar un curso, verá los detalles del curso y una lista de los estudiantes inscritos en el curso. Puede editar el nombre del instructor, la fecha de contratación y la asignación de la oficina. La asignación de Office es un conjunto de entidades independiente al que se tiene acceso a través de una propiedad de navegación.

Puede vincular datos maestros a datos detallados en el marcado o en el código. En esta parte del tutorial, usará ambos métodos.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Mostrar y actualizar entidades relacionadas en un control GridView

Cree una nueva página web denominada *instructors. aspx* que use la página maestra *site. Master* y agregue el marcado siguiente al control `Content` denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Este marcado crea un control de `EntityDataSource` que selecciona los instructores y habilita las actualizaciones. El elemento `div` configura el marcado para que se represente a la izquierda, de modo que pueda agregar una columna a la derecha más adelante.

Entre el marcado de `EntityDataSource` y la etiqueta de cierre de `</div>`, agregue el marcado siguiente que crea un control de `GridView` y un control de `Label` que se usará para los mensajes de error:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Este control de `GridView` habilita la selección de filas, resalta la fila seleccionada con un color de fondo gris claro y especifica los controladores (que creará más adelante) para los eventos `SelectedIndexChanged` y `Updating`. También especifica `PersonID` para la propiedad `DataKeyNames`, de modo que el valor de clave de la fila seleccionada se pueda pasar a otro control que se agregará más adelante.

La última columna contiene la asignación de la oficina del instructor, que se almacena en una propiedad de navegación de la entidad `Person` porque procede de una entidad asociada. Observe que el elemento `EditItemTemplate` especifica `Eval` en lugar de `Bind`, porque el control de `GridView` no se puede enlazar directamente a las propiedades de navegación para actualizarlos. Actualizará la asignación de la oficina en el código. Para ello, necesitará una referencia al control `TextBox` y lo obtendrá y lo guardará en el evento `Init` del control `TextBox`.

Después del control de `GridView` se trata de un control `Label` que se usa para los mensajes de error. La propiedad `Visible` del control es `false`y el estado de vista está desactivado, por lo que la etiqueta aparecerá solo cuando el código la haga visible en respuesta a un error.

Abra el archivo *instructors.aspx.CS* y agregue la siguiente instrucción `using`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Agregue un campo de clase privada inmediatamente después de la declaración de nombre de clase parcial para que contenga una referencia al cuadro de texto asignación de Office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Agregue un código auxiliar para el controlador de eventos `SelectedIndexChanged` que agregará código para más adelante. Agregue también un controlador para el evento `Init` del control de `TextBox` de asignación de Office para que pueda almacenar una referencia al control `TextBox`. Usará esta referencia para obtener el valor que el usuario especificó para actualizar la entidad asociada a la propiedad de navegación.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Usará el evento `Updating` del control `GridView` para actualizar la propiedad `Location` de la entidad `OfficeAssignment` asociada. Agregue el siguiente controlador para el evento `Updating`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Este código se ejecuta cuando el usuario hace clic en **Actualizar** en una fila de `GridView`. El código usa LINQ to Entities para recuperar la entidad `OfficeAssignment` asociada a la entidad `Person` actual, utilizando el `PersonID` de la fila seleccionada en el argumento del evento.

A continuación, el código realiza una de las siguientes acciones según el valor del control `InstructorOfficeTextBox`:

- Si el cuadro de texto tiene un valor y no hay ninguna entidad `OfficeAssignment` para actualizar, crea uno.
- Si el cuadro de texto tiene un valor y hay una entidad `OfficeAssignment`, actualiza el valor de la propiedad `Location`.
- Si el cuadro de texto está vacío y existe una entidad `OfficeAssignment`, elimina la entidad.

Después, guarda los cambios en la base de datos. Si se produce una excepción, se muestra un mensaje de error.

Ejecute la página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Haga clic en **Editar** y todos los campos cambian a cuadros de texto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Cambie cualquiera de estos valores, incluida la **asignación de Office**. Haga clic en **Actualizar** para ver los cambios reflejados en la lista.

## <a name="displaying-related-entities-in-a-separate-control"></a>Mostrar entidades relacionadas en un control independiente

Cada instructor puede enseñar uno o varios cursos, por lo que agregará un control de `EntityDataSource` y un control de `GridView` para mostrar los cursos asociados con el instructor seleccionado en el control de `GridView` de instructores. Para crear un encabezado y el `EntityDataSource` control para las entidades Courses, agregue el siguiente marcado entre el mensaje de error `Label` control y la etiqueta de cierre `</div>`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

El parámetro `Where` contiene el valor de la `PersonID` del instructor cuya fila está seleccionada en el control `InstructorsGridView`. La propiedad `Where` contiene un comando de Subselección que obtiene todas las entidades `Person` asociadas de la propiedad de navegación `People` de una entidad `Course` y selecciona la entidad `Course` solo si una de las entidades `Person` asociadas contiene el valor de `PersonID` seleccionado.

Para crear el control de `GridView`., agregue el siguiente marcado inmediatamente después del control `CoursesEntityDataSource` (antes de la etiqueta de cierre `</div>`):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Dado que no se mostrará ningún curso si no se selecciona ningún instructor, se incluye un elemento `EmptyDataTemplate`.

Ejecute la página.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Seleccione un instructor que tenga uno o varios cursos asignados, y el curso o los cursos aparecen en la lista. (Nota: aunque el esquema de la base de datos permite varios cursos, en los datos de prueba suministrados con la base de datos, ningún instructor tiene más de un curso. Puede Agregar cursos a la base de datos mediante la **Explorador de servidores** ventana o la página *CoursesAdd. aspx* , que agregará en un tutorial posterior).

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

En el control `CoursesGridView` solo se muestran algunos campos de cursos. Para mostrar todos los detalles de un curso, usará un control de `DetailsView` para el curso que selecciona el usuario. En *instructors. aspx*, agregue el siguiente marcado después de la etiqueta de cierre `</div>` (Asegúrese de colocar este marcado **después** de la etiqueta div de cierre, no antes):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Este marcado crea un control de `EntityDataSource` que está enlazado al conjunto de entidades `Courses`. La propiedad `Where` selecciona un curso mediante el valor `CourseID` de la fila seleccionada en el control `GridView` Courses. El marcado especifica un controlador para el evento `Selected`, que usará más adelante para mostrar las calificaciones de los estudiantes, que es otro nivel inferior de la jerarquía.

En *instructors.aspx.CS*, cree el siguiente código auxiliar para el método `CourseDetailsEntityDataSource_Selected`. (Rellenará este código auxiliar más adelante en el tutorial; por ahora, lo necesitará para que la página se compile y se ejecute).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Ejecute la página.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inicialmente no hay ningún detalle del curso porque no hay ningún curso seleccionado. Seleccione un instructor que tenga un curso asignado y, a continuación, seleccione un curso para ver los detalles.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Usar el evento EntityDataSource "seleccionado" para Mostrar datos relacionados

Por último, desea mostrar todos los estudiantes inscritos y sus calificaciones para el curso seleccionado. Para ello, utilizará el evento `Selected` del control `EntityDataSource` enlazado al `DetailsView`del curso.

En *instructors. aspx*, agregue el siguiente marcado después del control `DetailsView`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Este marcado crea un `ListView` control que muestra una lista de estudiantes y sus calificaciones para el curso seleccionado. No se especifica ningún origen de datos porque enlazará el control en el código. El elemento `EmptyDataTemplate` proporciona un mensaje para mostrar cuando no hay ningún curso seleccionado; en ese caso, no hay ningún estudiante que mostrar. El elemento `LayoutTemplate` crea una tabla HTML para mostrar la lista y el `ItemTemplate` especifica las columnas que se van a mostrar. El identificador de estudiante y el de estudiante proceden de la entidad `StudentGrade` y el nombre de estudiante procede de la entidad `Person` que la Entity Framework pone a disposición en la propiedad de navegación `Person` de la entidad `StudentGrade`.

En *instructors.aspx.CS*, reemplace el método `CourseDetailsEntityDataSource_Selected` stub-out por el código siguiente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

El argumento de evento para este evento proporciona los datos seleccionados en forma de colección, que tendrá cero elementos si no hay nada seleccionado o un elemento si se selecciona una entidad `Course`. Si se selecciona una entidad de `Course`, el código utiliza el método `First` para convertir la colección en un único objeto. A continuación, obtiene `StudentGrade` entidades de la propiedad de navegación, las convierte en una colección y enlaza el control `GradesListView` a la colección.

Esto es suficiente para mostrar las calificaciones, pero desea asegurarse de que el mensaje en la plantilla de datos vacíos se muestra la primera vez que se muestra la página y siempre que no se selecciona un curso. Para ello, cree el método siguiente, al que llamará desde dos lugares:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Llame a este nuevo método desde el método `Page_Load` para mostrar la plantilla de datos vacía la primera vez que se muestre la página. Y se llama desde el método `InstructorsGridView_SelectedIndexChanged` porque ese evento se desencadena cuando se selecciona un instructor, lo que significa que se cargan nuevos cursos en el control de cursos `GridView` y no se selecciona ninguno todavía. Estas son las dos llamadas:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Ejecute la página.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Seleccione un instructor que tenga un curso asignado y, a continuación, seleccione el curso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Ahora ha encontrado algunas maneras de trabajar con datos relacionados. En el siguiente tutorial, aprenderá a agregar relaciones entre las entidades existentes, cómo quitar relaciones y cómo agregar una nueva entidad que tenga una relación con una entidad existente.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-5.md)
