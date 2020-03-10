---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Actualización de datos relacionados con el Entity Framework en una aplicación ASP.NET MVC (6 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d29cb172d642b67947b461d1a7e55d01872bb8c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468289"
---
# <a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Actualización de datos relacionados con el Entity Framework en una aplicación ASP.NET MVC (6 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior se muestran datos relacionados. en este tutorial, actualizará los datos relacionados. Para la mayoría de las relaciones, esto se puede hacer actualizando los campos de clave externa adecuados. En el caso de las relaciones de varios a varios, el Entity Framework no expone directamente la tabla de combinación, por lo que debe agregar y quitar explícitamente entidades de las propiedades de navegación adecuadas.

En las ilustraciones siguientes se muestran las páginas con las que va a trabajar.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar las páginas Create y Edit de Courses

Cuando se crea una entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y vistas de Create y Edit que incluyen una lista desplegable para seleccionar el departamento. La lista desplegable establece la propiedad de clave externa `Course.DepartmentID`, y eso es todo lo que necesita Entity Framework para cargar la propiedad de navegación `Department` con la entidad `Department` adecuada. Podrá usar el código con scaffolding, pero cámbielo ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CourseController.CS*, elimine los cuatro `Edit` y `Create` métodos y reemplácelos por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

El método `PopulateDepartmentsDropDownList` obtiene una lista de todos los departamentos ordenados por nombre, crea una colección `SelectList` para una lista desplegable y pasa la colección a la vista en una propiedad `ViewBag`. El método acepta el parámetro opcional `selectedDepartment`, que permite al código que realiza la llamada especificar el elemento que se seleccionará cuando se procese la lista desplegable. La vista pasará el nombre `DepartmentID` a [la aplicación auxiliar de `DropDownList`](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md)y, a continuación, el ayudante sabrá buscar en el objeto `ViewBag` un `SelectList` denominado `DepartmentID`.

El método de `Create` de `HttpGet` llama al método `PopulateDepartmentsDropDownList` sin establecer el elemento seleccionado, porque, para un nuevo curso, el Departamento no se ha establecido todavía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El método de `Edit` de `HttpGet` establece el elemento seleccionado, en función del identificador del Departamento que ya está asignado al curso que se está editando:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Los métodos de `HttpPost` para `Create` y `Edit` también incluyen código que establece el elemento seleccionado cuando vuelve a mostrar la página después de un error:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Este código garantiza que cuando se vuelva a mostrar la página para mostrar el mensaje de error, el Departamento que se haya seleccionado permanecerá seleccionado.

En *Views\Course\Create.cshtml*, agregue el código resaltado para crear un nuevo campo de número de curso antes del campo de **título** . Como se explicó en un tutorial anterior, los campos de clave principal no tienen scaffolding de forma predeterminada, pero esta clave principal es significativa, por lo que desea que el usuario pueda escribir el valor de la clave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

En *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*y *Views\Course\Details.cshtml*, agregue un campo de número de curso antes del campo de **título** . Dado que es la clave principal, se muestra, pero no se puede cambiar.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute la página **crear** (muestre la página de índice del curso y haga clic en **crear nuevo**) y escriba los datos de un nuevo curso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Haga clic en **Crear**. La página de índice del curso se muestra con el nuevo curso agregado a la lista. El nombre de departamento de la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ejecute la página **Editar** (muestre la página de índice del curso y haga clic en **Editar** en un curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Cambie los datos en la página y haga clic en **Save**. La página de índice del curso se muestra con los datos del curso actualizados.

## <a name="adding-an-edit-page-for-instructors"></a>Agregar una página de edición para los instructores

Al editar un registro de instructor, necesita poder actualizar la asignación de la oficina del instructor. La entidad `Instructor` tiene una relación de uno a cero o uno con la entidad `OfficeAssignment`, lo que significa que debe administrar las siguientes situaciones:

- Si el usuario borra la asignación de la oficina y originalmente tenía un valor, debe quitar y eliminar la entidad `OfficeAssignment`.
- Si el usuario escribe un valor de asignación de la oficina y originalmente estaba vacío, debe crear una nueva entidad `OfficeAssignment`.
- Si el usuario cambia el valor de una asignación de Office, debe cambiar el valor de una entidad `OfficeAssignment` existente.

Abra *InstructorController.CS* y examine el método `HttpGet` `Edit`:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

El código con scaffolding no es lo que desea. Está configurando los datos de una lista desplegable, pero lo que necesita es un cuadro de texto. Reemplace este método por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Este código quita la instrucción `ViewBag` y agrega carga diligente para la entidad `OfficeAssignment` asociada. No se puede realizar la carga diligente con el método `Find`, por lo que se usan los métodos `Where` y `Single` en su lugar para seleccionar el instructor.

Reemplace el método `Edit` `HttpPost` por el código siguiente. que controla las actualizaciones de asignación de Office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El código realiza lo siguiente:

- Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`. Esto es lo mismo que hizo en el método de `Edit` de `HttpGet`.
- Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos. La sobrecarga de [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) utilizada le permite incluir en la *lista blanca* las propiedades que desea incluir. Esto evita el exceso de registro, como se explica en [el segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Si la ubicación de la oficina está en blanco, establece la propiedad `Instructor.OfficeAssignment` en null para que se elimine la fila relacionada en la tabla de `OfficeAssignment`.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Guarda los cambios en la base de datos.

En *Views\Instructor\Edit.cshtml*, después de la `div` elementos para el campo de **fecha de contratación** , agregue un nuevo campo para editar la ubicación de la oficina:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Ejecute la página (seleccione la pestaña **instructores** y, a continuación, haga clic en **Editar** en un instructor). Cambie el valor de **Office Location** y haga clic en **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Agregar asignaciones de cursos a la página de edición del instructor

Los instructores pueden impartir cualquier número de cursos. Ahora mejorará la página Edit Instructor al agregar la capacidad de cambiar las asignaciones de cursos mediante un grupo de casillas, tal y como se muestra en la siguiente captura de pantalla:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relación entre las entidades `Course` y `Instructor` es de varios a varios, lo que significa que no tiene acceso directo a la tabla de combinación. En su lugar, agregará y quitará entidades en el `Instructor.Courses` propiedad de navegación.

La interfaz de usuario que le permite cambiar los cursos a los que está asignado un instructor es un grupo de casillas. Se muestra una casilla para cada curso en la base de datos y se seleccionan aquellos a los que está asignado actualmente el instructor. El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos. Si el número de cursos era mucho mayor, probablemente desee usar un método diferente para presentar los datos en la vista, pero usaría el mismo método para manipular las propiedades de navegación con el fin de crear o eliminar relaciones.

Para proporcionar datos a la vista de la lista de casillas, deberá usar una clase de modelo de vista. Cree *AssignedCourseData.CS* en la carpeta *ViewModels* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

En *InstructorController.CS*, reemplace el método `Edit` `HttpGet` por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

El código agrega carga diligente para la propiedad de navegación `Courses` y llama al método `PopulateAssignedCourseData` nuevo para proporcionar información de la matriz de casilla mediante la clase de modelo de vista `AssignedCourseData`.

El código del método `PopulateAssignedCourseData` lee todas las entidades `Course` para cargar una lista de cursos mediante la clase de modelo de vista. Para cada curso, el código comprueba si existe el curso en la propiedad de navegación `Courses` del instructor. Para crear una búsqueda eficaz al comprobar si un curso está asignado al instructor, los cursos asignados al instructor se colocan en una colección [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) . La propiedad `Assigned` está establecida en `true` para los cursos a los que está asignado el instructor. La vista usará esta propiedad para determinar qué casilla debe mostrarse como seleccionada. Por último, la lista se pasa a la vista en una `ViewBag` propiedad.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **Save**. Reemplace el método `HttpPost` `Edit` por el código siguiente, que llama a un nuevo método que actualiza la propiedad de navegación `Courses` de la entidad `Instructor`. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Dado que la vista no tiene una colección de entidades `Course`, el enlazador de modelos no puede actualizar automáticamente la propiedad de navegación `Courses`. En lugar de usar el enlazador de modelos para actualizar la propiedad de navegación Courses, lo hará en el método New `UpdateInstructorCourses`. Por lo tanto, tendrá que excluir la propiedad `Courses` del enlace de modelos. Esto no requiere ningún cambio en el código que llama a [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque está usando la sobrecarga de la lista de *permitidos* y `Courses` no está en la lista de inclusión.

Si no se ha seleccionado ninguna casilla, el código de `UpdateInstructorCourses` inicializa la propiedad de navegación `Courses` con una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

A continuación, el código recorre en bucle todos los cursos de la base de datos y coteja los que están asignados actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, estas dos últimas colecciones se almacenan en objetos `HashSet`.

Si se ha activado la casilla para un curso pero este no se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Si no se ha activado la casilla para un curso pero este se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se quita de la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

En *Views\Instructor\Edit.cshtml*, agregue un campo **Courses** con una matriz de casillas agregando el siguiente código resaltado inmediatamente después de los elementos `div` del campo `OfficeAssignment`:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Este código crea una tabla HTML que tiene tres columnas. En cada columna hay una casilla seguida de un título que está formado por el número y el título del curso. Todas las casillas tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se va a tratar como un grupo. El `value` atributo de cada casilla se establece en el valor de `CourseID.` cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que consta de los valores de `CourseID` solo para las casillas seleccionadas.

Cuando se representan inicialmente las casillas, las que se aplican a los cursos asignados al instructor tienen `checked` atributos, que las selecciona (las muestra activadas).

Después de cambiar las asignaciones de cursos, querrá comprobar los cambios cuando el sitio vuelva a la página `Index`. Por lo tanto, debe agregar una columna a la tabla en esa página. En este caso, no es necesario usar el objeto `ViewBag`, porque la información que desea mostrar ya está en la propiedad de navegación `Courses` de la entidad `Instructor` que va a pasar a la página como modelo.

En *Views\Instructor\Index.cshtml*, agregue un encabezado **Courses** inmediatamente después del título de **Office** , tal como se muestra en el ejemplo siguiente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Después, agregue una nueva celda de detalle inmediatamente después de la celda de detalle de la ubicación de la oficina:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Ejecute la página de **Índice del instructor** para ver los cursos asignados a cada instructor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en **Editar** en un instructor para ver la página de edición.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Cambie algunas asignaciones de cursos y haga clic en **Guardar**. Los cambios que haga se reflejan en la página de índice.

 Nota: el enfoque que se lleva a cabo para editar los datos del curso del instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, se necesitaría una interfaz de usuario y un método de actualización diferentes.  

## <a name="update-the-delete-method"></a>Actualización del método Delete

Cambie el código del método HttpPost delete para que el registro de asignación de Office (si existe) se elimine cuando se elimine el instructor:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]

Si intenta eliminar un instructor asignado a un departamento como administrador, obtendrá un error de integridad referencial. Vea [la versión actual de este tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) para obtener código adicional que quitará automáticamente el instructor de cualquier departamento en el que el instructor esté asignado como administrador.

## <a name="summary"></a>Resumen

Ya ha completado esta introducción al trabajo con datos relacionados. Hasta ahora en estos tutoriales ha realizado una gama completa de operaciones CRUD, pero no ha abordado los problemas de simultaneidad. En el siguiente tutorial se presenta el tema de simultaneidad, se explican las opciones para controlarlo y se agrega el control de simultaneidad al código CRUD que ya se ha escrito para un tipo de entidad.

Los vínculos a otros recursos de Entity Framework, se pueden encontrar al final del [último tutorial de esta serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
