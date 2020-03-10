---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: actualización de datos relacionados con EF en una aplicación ASP.NET MVC'
description: En este tutorial, actualizará los datos relacionados. Para la mayoría de las relaciones, esto puede hacerse mediante la actualización de campos de clave externa o de propiedades de navegación.
author: tdykstra
ms.author: riande
ms.date: 01/19/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4d5f6447fdccefdcdf9497a9e94f23243302a0e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499303"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: actualización de datos relacionados con EF en una aplicación ASP.NET MVC

En el tutorial anterior se muestran datos relacionados. En este tutorial, actualizará los datos relacionados. Para la mayoría de las relaciones, esto puede hacerse mediante la actualización de campos de clave externa o de propiedades de navegación. En el caso de las relaciones de varios a varios, el Entity Framework no expone directamente la tabla de combinación, por lo que se agregan y se quitan entidades de las propiedades de navegación adecuadas.

En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Edición del instructor con cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

En este tutorial va a:

> [!div class="checklist"]
> * Páginas de personalización de cursos
> * Página agregar Office a los instructores
> * Página agregar cursos a instructores
> * Actualizar DeleteConfirmed
> * Agregar la ubicación de la oficina y cursos a la página Create

## <a name="prerequisites"></a>Requisitos previos

* [Lectura de datos relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Páginas de personalización de cursos

Cuando se crea una entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y vistas de Create y Edit que incluyen una lista desplegable para seleccionar el departamento. La lista desplegable establece la propiedad de clave externa `Course.DepartmentID`, y eso es todo lo que necesita Entity Framework para cargar la propiedad de navegación `Department` con la entidad `Department` adecuada. Podrá usar el código con scaffolding, pero cámbielo ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CourseController.CS*, elimine los cuatro `Create` y `Edit` métodos y reemplácelos por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Agregue la siguiente instrucción `using` al principio del archivo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El método `PopulateDepartmentsDropDownList` obtiene una lista de todos los departamentos ordenados por nombre, crea una colección `SelectList` para una lista desplegable y pasa la colección a la vista en una propiedad `ViewBag`. El método acepta el parámetro opcional `selectedDepartment`, que permite al código que realiza la llamada especificar el elemento que se seleccionará cuando se procese la lista desplegable. La vista pasará el nombre `DepartmentID` a la aplicación auxiliar de [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) y, a continuación, el ayudante sabe que debe buscar en el objeto `ViewBag` una `SelectList` denominada `DepartmentID`.

El método de `Create` de `HttpGet` llama al método `PopulateDepartmentsDropDownList` sin establecer el elemento seleccionado, porque, para un nuevo curso, el Departamento no se ha establecido todavía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

El método de `Edit` de `HttpGet` establece el elemento seleccionado, en función del identificador del Departamento que ya está asignado al curso que se está editando:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Los métodos de `HttpPost` para `Create` y `Edit` también incluyen código que establece el elemento seleccionado cuando vuelve a mostrar la página después de un error:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Este código garantiza que cuando se vuelva a mostrar la página para mostrar el mensaje de error, el Departamento que se haya seleccionado permanecerá seleccionado.

Las vistas del curso ya tienen scaffolding con listas desplegables para el campo Department, pero no desea el título DepartmentID para este campo, por lo que debe hacer el siguiente cambio resaltado en el archivo *Views\Course\Create.cshtml* para cambiar el título.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Realice el mismo cambio en *Views\Course\Edit.cshtml*.

Normalmente, el scaffolding no aplica la técnica scaffolding a una clave principal porque el valor de clave se genera por la base de datos y no se puede cambiar y no es un valor significativo que se muestre a los usuarios. En el caso de las entidades Course, el scaffolding incluye un cuadro de texto para el campo `CourseID` porque entiende que el atributo `DatabaseGeneratedOption.None` significa que el usuario debe poder escribir el valor de la clave principal. Pero no entiende que, dado que el número es significativo, querrá verlo en las otras vistas, por lo que deberá agregarlo manualmente.

En *Views\Course\Edit.cshtml*, agregue un campo de número de curso antes del campo **título** . Dado que es la clave principal, se muestra, pero no se puede cambiar.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Ya existe un campo oculto (aplicación auxiliar de`Html.HiddenFor`) para el número de curso en la vista de edición. Agregar una aplicación auxiliar *HTML. LabelFor* no elimina la necesidad del campo oculto porque no hace que el número de curso se incluya en los datos enviados cuando el usuario hace clic en **Guardar** en la página Editar.

En *Views\Course\Delete.cshtml* y *Views\Course\Details.cshtml*, cambie la leyenda del nombre del Departamento de "nombre" a "Departamento" y agregue un campo de número de curso antes del campo **título** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Ejecute la página **crear** (muestre la página de índice del curso y haga clic en **crear nuevo**) y escriba los datos de un nuevo curso:

| Valor | Configuración |
| ----- | ------- |
| número | Escriba *1000*. |
| Título | Escriba *álgebra*. |
| Créditos | Escriba *4*. |
|Department | Seleccione **matemáticas**. |

Haga clic en **Crear**. La página de índice del curso se muestra con el nuevo curso agregado a la lista. El nombre de departamento de la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

Ejecute la página **Editar** (muestre la página de índice del curso y haga clic en **Editar** en un curso).

Cambie los datos en la página y haga clic en **Save**. La página de índice del curso se muestra con los datos del curso actualizados.

## <a name="add-office-to-instructors-page"></a>Página agregar Office a los instructores

Al editar un registro de instructor, necesita poder actualizar la asignación de la oficina del instructor. La entidad `Instructor` tiene una relación de uno a cero o uno con la entidad `OfficeAssignment`, lo que significa que debe administrar las siguientes situaciones:

- Si el usuario borra la asignación de la oficina y originalmente tenía un valor, debe quitar y eliminar la entidad `OfficeAssignment`.
- Si el usuario escribe un valor de asignación de la oficina y originalmente estaba vacío, debe crear una nueva entidad `OfficeAssignment`.
- Si el usuario cambia el valor de una asignación de Office, debe cambiar el valor de una entidad `OfficeAssignment` existente.

Abra *InstructorController.CS* y examine el método `HttpGet` `Edit`:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El código con scaffolding no es lo que desea. Está configurando los datos de una lista desplegable, pero lo que necesita es un cuadro de texto. Reemplace este método por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Este código quita la instrucción `ViewBag` y agrega carga diligente para la entidad `OfficeAssignment` asociada. No se puede realizar la carga diligente con el método `Find`, por lo que se usan los métodos `Where` y `Single` en su lugar para seleccionar el instructor.

Reemplace el método `Edit` `HttpPost` por el código siguiente. que controla las actualizaciones de asignación de Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La referencia a `RetryLimitExceededException` requiere una instrucción de `using`; para agregarlo, mantenga el mouse sobre `RetryLimitExceededException`. Aparece el siguiente mensaje: ![ mensaje de excepción de reintento](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Seleccione **Mostrar posibles correcciones**y, a continuación, **use System. Data. Entity. Infrastructure**

![Resolver excepción de reintento](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

El código realiza lo siguiente:

- Cambia el nombre del método a `EditPost` porque la firma es ahora la misma que el método `HttpGet` (el atributo `ActionName` especifica que se sigue usando la dirección URL/Edit/).
- Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`. Esto es lo mismo que hizo en el método de `Edit` de `HttpGet`.
- Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos. La sobrecarga de [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) utilizada le permite incluir en la *lista blanca* las propiedades que desea incluir. Esto evita el exceso de registro, como se explica en [el segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Si la ubicación de la oficina está en blanco, establece la propiedad `Instructor.OfficeAssignment` en null para que se elimine la fila relacionada en la tabla de `OfficeAssignment`.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Guarda los cambios en la base de datos.

En *Views\Instructor\Edit.cshtml*, después de la `div` elementos para el campo de **fecha de contratación** , agregue un nuevo campo para editar la ubicación de la oficina:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Ejecute la página (seleccione la pestaña **instructores** y, a continuación, haga clic en **Editar** en un instructor). Cambie el valor de **Office Location** y haga clic en **Save**.

## <a name="add-courses-to-instructors-page"></a>Página agregar cursos a instructores

Los instructores pueden impartir cualquier número de cursos. Ahora mejorará la página de edición del instructor agregando la capacidad de cambiar las asignaciones de cursos mediante un grupo de casillas.

La relación entre las entidades `Course` y `Instructor` es de varios a varios, lo que significa que no tiene acceso directo a las propiedades de clave externa que se encuentran en la tabla de combinación. En su lugar, puede Agregar y quitar entidades de la propiedad de navegación `Instructor.Courses`.

La interfaz de usuario que le permite cambiar los cursos a los que está asignado un instructor es un grupo de casillas. Se muestra una casilla para cada curso en la base de datos y se seleccionan aquellos a los que está asignado actualmente el instructor. El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos. Si el número de cursos era mucho mayor, probablemente desee usar un método diferente para presentar los datos en la vista, pero usaría el mismo método para manipular las propiedades de navegación con el fin de crear o eliminar relaciones.

Para proporcionar datos a la vista de la lista de casillas, deberá usar una clase de modelo de vista. Cree *AssignedCourseData.CS* en la carpeta *ViewModels* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

En *InstructorController.CS*, reemplace el método `Edit` `HttpGet` por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

El código agrega carga diligente para la propiedad de navegación `Courses` y llama al método `PopulateAssignedCourseData` nuevo para proporcionar información de la matriz de casilla mediante la clase de modelo de vista `AssignedCourseData`.

El código del método `PopulateAssignedCourseData` lee todas las entidades `Course` para cargar una lista de cursos mediante la clase de modelo de vista. Para cada curso, el código comprueba si existe el curso en la propiedad de navegación `Courses` del instructor. Para crear una búsqueda eficaz al comprobar si un curso está asignado al instructor, los cursos asignados al instructor se colocan en una colección [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La propiedad `Assigned` está establecida en `true` para los cursos a los que está asignado el instructor. La vista usará esta propiedad para determinar qué casilla debe mostrarse como seleccionada. Por último, la lista se pasa a la vista en una `ViewBag` propiedad.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **Save**. Reemplace el método `EditPost` por el código siguiente, que llama a un nuevo método que actualiza la propiedad de navegación `Courses` de la entidad `Instructor`. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La firma del método es ahora diferente de la `HttpGet` `Edit` método, por lo que el nombre del método cambia de `EditPost` a `Edit`.

Dado que la vista no tiene una colección de entidades `Course`, el enlazador de modelos no puede actualizar automáticamente la propiedad de navegación `Courses`. En lugar de utilizar el enlazador de modelos para actualizar la propiedad de navegación `Courses`, lo hará en el nuevo método `UpdateInstructorCourses`. Por lo tanto, tendrá que excluir la propiedad `Courses` del enlace de modelos. Esto no requiere ningún cambio en el código que llama a [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque está usando la sobrecarga de la lista de *permitidos* y `Courses` no está en la lista de inclusión.

Si no se ha seleccionado ninguna casilla, el código de `UpdateInstructorCourses` inicializa la propiedad de navegación `Courses` con una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

A continuación, el código recorre en bucle todos los cursos de la base de datos y coteja los que están asignados actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, estas dos últimas colecciones se almacenan en objetos `HashSet`.

Si se ha activado la casilla para un curso pero este no se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Si no se ha activado la casilla para un curso pero este se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se quita de la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

En *Views\Instructor\Edit.cshtml*, agregue un campo **Courses** con una matriz de casillas agregando el código siguiente inmediatamente después de los elementos `div` para el campo `OfficeAssignment` y antes del elemento `div` para el botón **Guardar** :

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Después de pegar el código, si los saltos de línea y la sangría no tienen el mismo aspecto, corríjalos manualmente para que se parezca a lo que se ve aquí. No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@</tr>` deben estar en una única línea tal y como se muestra, de lo contrario, obtendrá un error en tiempo de ejecución.

Este código crea una tabla HTML que tiene tres columnas. En cada columna hay una casilla seguida de un título que está formado por el número y el título del curso. Todas las casillas tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se va a tratar como un grupo. El `value` atributo de cada casilla se establece en el valor de `CourseID.` cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que consta de los valores de `CourseID` solo para las casillas seleccionadas.

Cuando se representan inicialmente las casillas, las que se aplican a los cursos asignados al instructor tienen `checked` atributos, que las selecciona (las muestra activadas).

Después de cambiar las asignaciones de cursos, querrá comprobar los cambios cuando el sitio vuelva a la página `Index`. Por lo tanto, debe agregar una columna a la tabla en esa página. En este caso, no es necesario usar el objeto `ViewBag`, porque la información que desea mostrar ya está en la propiedad de navegación `Courses` de la entidad `Instructor` que va a pasar a la página como modelo.

En *Views\Instructor\Index.cshtml*, agregue un encabezado **Courses** inmediatamente después del título de **Office** , tal como se muestra en el ejemplo siguiente:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Después, agregue una nueva celda de detalle inmediatamente después de la celda de detalle de la ubicación de la oficina:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Ejecute la página de **Índice del instructor** para ver los cursos asignados a cada instructor.

Haga clic en **Editar** en un instructor para ver la página de edición.

Cambie algunas asignaciones de cursos y haga clic en **Guardar**. Los cambios que haga se reflejan en la página de índice.

 Nota: el enfoque que se realiza aquí para editar los datos del curso del instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, se necesitaría una interfaz de usuario y un método de actualización diferentes.

## <a name="update-deleteconfirmed"></a>Actualizar DeleteConfirmed

En *InstructorController.CS*, elimine el método `DeleteConfirmed` e inserte el siguiente código en su lugar.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Este código realiza el siguiente cambio:

- Si el instructor está asignado como administrador de cualquier departamento, quita la asignación del instructor de ese departamento. Sin este código, obtendría un error de integridad referencial si intenta eliminar un instructor asignado como administrador para un departamento.

Este código no controla el escenario de un instructor asignado como administrador para varios departamentos. En el último tutorial, agregará código que impide que se produzca ese escenario.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Agregar la ubicación de la oficina y cursos a la página Create

En *InstructorController.CS*, elimine los métodos `HttpGet` y `HttpPost` `Create` y, a continuación, agregue el siguiente código en su lugar:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Este código es similar a lo que vio para los métodos de edición, salvo que inicialmente no se selecciona ningún curso. El método de `Create` de `HttpGet` llama al método `PopulateAssignedCourseData` no porque puede haber cursos seleccionados, pero con el fin de proporcionar una colección vacía para el bucle de `foreach` en la vista (de lo contrario, el código de vista produciría una excepción de referencia nula).

El método HttpPost Create agrega cada curso seleccionado a la propiedad de navegación Courses antes del código de plantilla que comprueba los errores de validación y agrega el nuevo instructor a la base de datos. Los cursos se agregan, incluso si hay errores de modelo, de modo que cuando se produzcan errores de modelo (por ejemplo, el usuario convirtió una fecha no válida), de modo que cuando se vuelva a mostrar la página con un mensaje de error, las selecciones de cursos realizadas se restauren automáticamente.

Tenga en cuenta que, para poder agregar cursos a la propiedad de navegación `Courses`, debe inicializar la propiedad como una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Como alternativa a hacerlo en el código de control, podría hacerlo en el modelo de Instructor cambiando el captador de propiedad para que cree automáticamente la colección si no existe, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Si modifica la propiedad `Courses` de esta manera, puede quitar el código de inicialización de propiedad explícito del controlador.

En *Views\Instructor\Create.cshtml*, agregue un cuadro de texto ubicación de la oficina y las casillas del curso después del campo fecha de contratación y antes del botón **Enviar** .

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Después de pegar el código, corrija los saltos de línea y la sangría tal como hizo anteriormente para la página de edición.

Ejecute la página crear y agregue un instructor.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Controlar transacciones

Como se explica en el [tutorial básico de la funcionalidad CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), de forma predeterminada el Entity Framework implementa las transacciones de manera implícita. En escenarios donde necesita más control, por ejemplo, si desea incluir operaciones realizadas fuera de Entity Framework en una transacción, vea [trabajar con transacciones](https://msdn.microsoft.com/data/dn456843) en MSDN.

## <a name="get-the-code"></a>Obtención del código

[Descargar el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en [el acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Paso siguiente

En este tutorial va a:

> [!div class="checklist"]
> * Páginas de cursos personalizados
> * Página de Office a Instructors agregada
> * Página de cursos agregados a instructores
> * Actualizado DeleteConfirmed
> * Se han agregado la ubicación de la oficina y los cursos a la página crear

Avance al siguiente artículo para aprender a implementar un modelo de programación asincrónica.
> [!div class="nextstepaction"]
> [Modelo de programación asincrónica](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
