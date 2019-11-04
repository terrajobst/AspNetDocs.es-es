---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: lectura de datos relacionados con EF en una aplicación ASP.NET MVC'
description: En este tutorial podrá leer y Mostrar datos relacionados, es decir, los datos que el Entity Framework carga en las propiedades de navegación.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d804c8dd45ad131949260c85d9d9c6683bfe9646
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445661"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: lectura de datos relacionados con EF en una aplicación ASP.NET MVC

En el tutorial anterior, completó el modelo de datos School. En este tutorial podrá leer y Mostrar datos relacionados, es decir, los datos que el Entity Framework carga en las propiedades de navegación.

En las ilustraciones siguientes se muestran las páginas con las que va a trabajar.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 5 con el Code First de Entity Framework 6 y Visual Studio. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

En este tutorial ha:

> [!div class="checklist"]
> * Obtiene información sobre cómo cargar datos relacionados
> * Crea una página de cursos
> * Crea una página de instructores

## <a name="prerequisites"></a>Requisitos previos

* [Crear un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Obtiene información sobre cómo cargar datos relacionados

Los Entity Framework pueden cargar datos relacionados en las propiedades de navegación de una entidad de varias maneras:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado que se envíen varias consultas a la base de datos, una para la propia entidad y otra cada vez que se deben recuperar los datos relacionados de la entidad. La clase `DbContext` habilita la carga diferida de forma predeterminada.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. La carga diligente se especifica mediante el método `Include`.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carga explícita*. Esto es similar a la carga diferida, salvo que recupera explícitamente los datos relacionados en el código; no se produce automáticamente cuando se obtiene acceso a una propiedad de navegación. Los datos relacionados se cargan manualmente obteniendo la entrada del administrador de estado de objetos para una entidad y llamando al método [Collection. Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) para colecciones o al método [Reference. Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) para las propiedades que contienen una sola entidad. (En el ejemplo siguiente, si desea cargar la propiedad de navegación de administrador, debe reemplazar `Collection(x => x.Courses)` por `Reference(x => x.Administrator)`). Normalmente se usaría la carga explícita solo cuando se desactiva la carga diferida.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Dado que no recuperan los valores de propiedad de inmediato, la carga diferida y la carga explícita también se conocen como *carga diferida*.

### <a name="performance-considerations"></a>Consideraciones sobre el rendimiento

Si sabe que necesita datos relacionados para cada entidad que se recupere, la carga diligente suele ofrecer el mejor rendimiento, dado que una sola consulta que se envía a la base de datos normalmente es más eficaz que consultas independientes para cada entidad recuperada. Por ejemplo, en los ejemplos anteriores, supongamos que cada departamento tiene diez cursos relacionados. El ejemplo de carga diligente daría como resultado una sola consulta (combinación) y un único viaje de ida y vuelta a la base de datos. Los ejemplos de carga diferida y de carga explícita darían como resultado once consultas y once viajes de ida y vuelta a la base de datos. Los recorridos de ida y vuelta adicionales a la base de datos afectan especialmente de forma negativa al rendimiento cuando la latencia es alta.

Por otro lado, en algunos escenarios la carga diferida es más eficaz. La carga diligente podría provocar la generación de una combinación muy compleja, lo que SQL Server no se puede procesar de forma eficaz. O bien, si necesita tener acceso a las propiedades de navegación de una entidad solo para un subconjunto de un conjunto de las entidades que está procesando, la carga diferida podría mejorar mejor porque la carga diligente recuperaría más datos de los que necesita. Si el rendimiento es crítico, es mejor probarlo de ambas formas para elegir la mejor opción.

La carga diferida puede enmascarar el código que causa problemas de rendimiento. Por ejemplo, el código que no especifica la carga diligente o explícita, pero procesa un gran volumen de entidades y utiliza varias propiedades de navegación en cada iteración podría ser muy ineficaz (debido a muchos viajes de ida y vuelta a la base de datos). Una aplicación que funciona bien en el desarrollo con un servidor SQL Server local puede tener problemas de rendimiento cuando se mueve a Azure SQL Database debido a la mayor latencia y carga diferida. La generación de perfiles de las consultas de base de datos con una carga de pruebas realista le ayudará a determinar si la carga diferida es adecuada. Para obtener más información, vea [desmitificación de Entity Framework estrategias: cargar datos relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) y [usar el Entity Framework para reducir la latencia de red a SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Deshabilitar la carga diferida antes de la serialización

Si deja la carga diferida habilitada durante la serialización, puede acabar consultando significativamente más datos de los previstos. La serialización funciona generalmente mediante el acceso a cada propiedad en una instancia de un tipo. El acceso de propiedad desencadena la carga diferida y las entidades cargadas diferidos se serializan. A continuación, el proceso de serialización accede a cada propiedad de las entidades de carga diferida, lo que puede provocar incluso una carga y una serialización más diferidas. Para evitar esta reacción de la cadena de ejecución, desactive la carga diferida antes de serializar una entidad.

La serialización también puede ser complicada por las clases de proxy que utiliza el Entity Framework, como se explica en el [tutorial de escenarios avanzados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Una manera de evitar problemas de serialización es serializar los objetos de transferencia de datos (DTO) en lugar de los objetos entidad, como se muestra en el tutorial uso de la [API Web con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) .

Si no usa dto, puede deshabilitar la carga diferida y evitar problemas de proxy [deshabilitando la creación de proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Estas son algunas otras [formas de deshabilitar la carga diferida](https://msdn.microsoft.com/data/jj574232):

- Para propiedades de navegación específicas, omita la palabra clave `virtual` cuando declare la propiedad.
- Para todas las propiedades de navegación, establezca `LazyLoadingEnabled` en `false`, coloque el siguiente código en el constructor de la clase de contexto:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Crea una página de cursos

La entidad `Course` incluye una propiedad de navegación que contiene la entidad `Department` del Departamento al que está asignado el curso. Para mostrar el nombre del Departamento asignado en una lista de cursos, debe obtener la propiedad `Name` de la entidad `Department` que se encuentra en la propiedad de navegación `Course.Department`.

Cree un controlador denominado `CourseController` (no CoursesController) para el tipo de entidad `Course`, con las mismas opciones para el **controlador MVC 5 con vistas, con Entity Framework** el scaffolding que hizo anteriormente para el controlador de `Student`:

| Parámetro | Valor |
| ------- | ----- |
| Clase de modelo | Seleccione **Course (ContosoUniversity. Models)** . |
| Clase de contexto de datos | Seleccione **SchoolContext (ContosoUniversity. Dal)** . |
| Nombre del controlador | Escriba *CourseController*. De nuevo, no *CoursesController* con un *s*. Al seleccionar **Course (ContosoUniversity. Models)** , se rellena automáticamente el valor del **nombre del controlador** . Tiene que cambiar el valor. |

Deje los demás valores predeterminados y agregue el controlador.

Abra *Controllers\CourseController.CS* y examine el método `Index`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El scaffolding automático ha especificado la carga diligente para la propiedad de navegación `Department` mediante el método `Include`.

Abra *Views\Course\Index.cshtml* y reemplace el código de plantilla por el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Ha realizado los cambios siguientes en el código con scaffolding:

- Se ha cambiado el encabezado del **Índice** a los **cursos**.
- Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`. De forma predeterminada, las claves principales no tienen scaffolding porque normalmente no tienen sentido para los usuarios finales. Pero en este caso, la clave principal es significativa y quiere mostrarla.
- Se ha quitado la columna **Department** del lado derecho y se ha cambiado su encabezado. El scaffolding decidió correctamente Mostrar la propiedad `Name` de la entidad `Department`, pero aquí en la página del curso el encabezado de columna debe ser **Department** en lugar de **Name**.

Observe que para la columna Department, el código con scaffolding muestra la propiedad `Name` de la entidad `Department` que se carga en la propiedad de navegación `Department`:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Ejecute la página (seleccione la pestaña **cursos** en la Página principal de Contoso University) para ver la lista con los nombres de departamento.

## <a name="create-an-instructors-page"></a>Crea una página de instructores

En esta sección, creará un controlador y una vista para la entidad `Instructor` para mostrar la página instructores. En esta página se leen y muestran los datos relacionados de las maneras siguientes:

- La lista de instructores muestra los datos relacionados de la entidad `OfficeAssignment`. Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno. Usará la carga diligente para las entidades `OfficeAssignment`. Como se explicó anteriormente, la carga diligente normalmente es más eficaz cuando se necesitan los datos relacionados para todas las filas recuperadas de la tabla principal. En este caso, quiere mostrar las asignaciones de oficina para todos los instructores que se muestran.
- Cuando el usuario selecciona un instructor, se muestran las entidades `Course` relacionadas. Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios. Usará la carga diligente para las entidades `Course` y sus entidades `Department` relacionadas. En este caso, la carga diferida podría ser más eficaz porque solo necesita cursos para el instructor seleccionado. Pero en este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación dentro de entidades que, a su vez, se encuentran en propiedades de navegación.
- Cuando el usuario selecciona un curso, se muestran los datos relacionados del conjunto de entidades `Enrollments`. Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios. Agregará la carga explícita para las entidades `Enrollment` y sus entidades `Student` relacionadas. (La carga explícita no es necesaria porque la carga diferida está habilitada, pero muestra cómo realizar la carga explícita).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice de instructor

En la página Instructors se muestran tres tablas diferentes. Por tanto, creará un modelo de vista que incluye tres propiedades, cada una con los datos de una de las tablas.

En la carpeta *ViewModels* , cree *InstructorIndexData.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Crear el controlador y las vistas del instructor

Cree un controlador `InstructorController` (no InstructorsController) con una acción de lectura/escritura de EF:

| Parámetro | Valor |
| ------- | ----- |
| Clase de modelo | Seleccione **instructor (ContosoUniversity. Models)** . |
| Clase de contexto de datos | Seleccione **SchoolContext (ContosoUniversity. Dal)** . |
| Nombre del controlador | Escriba *InstructorController*. De nuevo, no *InstructorsController* con un *s*. Al seleccionar **Course (ContosoUniversity. Models)** , se rellena automáticamente el valor del **nombre del controlador** . Tiene que cambiar el valor. |

Deje los demás valores predeterminados y agregue el controlador.

Abra *Controllers\InstructorController.CS* y agregue una instrucción `using` para el espacio de nombres `ViewModels`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

El código con scaffolding del método `Index` especifica la carga diligente solo para la propiedad de navegación `OfficeAssignment`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Reemplace el método `Index` por el código siguiente para cargar datos relacionados adicionales y colocarlos en el modelo de vista:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

El método acepta datos de ruta opcionales (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcionan los valores de identificador del instructor seleccionado y el curso seleccionado, y pasa todos los datos necesarios a la vista. Los parámetros se proporcionan mediante los hipervínculos **Select** de la página.

El código comienza creando una instancia del modelo de vista y coloca en ella la lista de instructores. El código especifica la carga diligente para el `Instructor.OfficeAssignment` y la propiedad de navegación `Instructor.Courses`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

El segundo método `Include` carga cursos y, para cada curso que se carga, realiza la carga diligente para la propiedad de navegación `Course.Department`.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Como se mencionó anteriormente, la carga diligente no es necesaria, pero se realiza para mejorar el rendimiento. Dado que la vista siempre requiere la entidad `OfficeAssignment`, es más eficaz recuperarla en la misma consulta. `Course` entidades son necesarias cuando se selecciona un instructor en la página web, por lo que la carga diligente es mejor que la carga diferida solo si la página se muestra más a menudo con un curso seleccionado que sin.

Si se seleccionó un ID. de instructor, el instructor seleccionado se recupera de la lista de instructores en el modelo de vista. A continuación, se carga la propiedad `Courses` del modelo de vista con las entidades `Course` de la propiedad de navegación `Courses` del instructor.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El método `Where` devuelve una colección, pero en este caso los criterios que se pasan a ese método dan como resultado que solo se devuelva una única entidad de `Instructor`. El método `Single` convierte la colección en una sola entidad `Instructor`, que proporciona acceso a la propiedad `Courses` de esa entidad.

Use el método [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) en una colección cuando sepa que la colección tendrá un solo elemento. El método `Single` produce una excepción si la colección que se le pasa está vacía o si hay más de un elemento. Una alternativa es [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que devuelve un valor predeterminado (`null` en este caso) si la colección está vacía. Sin embargo, en este caso, esto podría dar lugar a una excepción (al intentar buscar una propiedad `Courses` en una referencia `null`) y el mensaje de excepción indicaría menos claramente la causa del problema. Al llamar al método `Single`, también puede pasar la condición `Where` en lugar de llamar al método `Where` por separado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

En lugar de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

A continuación, si se ha seleccionado un curso, se recupera de la lista de cursos en el modelo de vista. A continuación, se carga la propiedad `Enrollments` del modelo de vista con las entidades `Enrollment` de la propiedad de navegación `Enrollments` del curso.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificar la vista de índice de instructor

En *Views\Instructor\Index.cshtml*, reemplace el código de plantilla por el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Ha realizado los cambios siguientes en el código existente:

- Ha cambiado la clase de modelo por `InstructorIndexData`.
- Ha cambiado el título de la página de **Index** a **Instructors**.
- Se ha agregado una columna de **Office** que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es NULL. (Dado que se trata de una relación de uno a cero o uno, es posible que no haya una entidad `OfficeAssignment` relacionada).

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Se ha agregado código que agregará dinámicamente `class="success"` al elemento `tr` del instructor seleccionado. Esto establece un color de fondo para la fila seleccionada mediante una clase [bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) .

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Se ha agregado una nueva `ActionLink` etiquetada como **Select** inmediatamente antes de los demás vínculos de cada fila, lo que hace que el ID. del instructor seleccionado se envíe al método `Index`.

Ejecute la aplicación y seleccione la pestaña **Instructors** . En la página se muestra la propiedad `Location` de las entidades `OfficeAssignment` relacionadas y una celda de tabla vacía cuando no hay ninguna entidad `OfficeAssignment` relacionada.

En el archivo *Views\Instructor\Index.cshtml* , después del elemento de `table` de cierre (al final del archivo), agregue el código siguiente. Este código muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Este código lee la propiedad `Courses` del modelo de vista para mostrar una lista de cursos. También proporciona un hipervínculo `Select` que envía el identificador del curso seleccionado al método de acción `Index`.

Ejecute la página y seleccione un instructor. Ahora verá una cuadrícula en la que se muestran los cursos asignados al instructor seleccionado, y para cada curso, el nombre del departamento asignado.

Después del bloque de código que se acaba de agregar, agregue el código siguiente. Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona ese curso.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Este código lee la propiedad `Enrollments` del modelo de vista para mostrar una lista de los estudiantes inscritos en el curso.

Ejecute la página y seleccione un instructor. Después, seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.

### <a name="adding-explicit-loading"></a>Agregando carga explícita

Abra *InstructorController.CS* y observe cómo el método `Index` obtiene la lista de inscripciones de un curso seleccionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Cuando recuperó la lista de instructores, especificó la carga diligente para la propiedad de navegación `Courses` y para la propiedad `Department` de cada curso. A continuación, coloque la colección de `Courses` en el modelo de vista y ahora va a tener acceso a la propiedad de navegación `Enrollments` desde una entidad de esa colección. Dado que no especificó la carga diligente para la propiedad de navegación `Course.Enrollments`, los datos de esa propiedad aparecen en la página como resultado de la carga diferida.

Si deshabilitó la carga diferida sin cambiar el código de ninguna otra forma, la propiedad `Enrollments` sería null independientemente de cuántas inscripciones haya tenido realmente el curso. En ese caso, para cargar la propiedad `Enrollments`, tendría que especificar la carga diligente o la carga explícita. Ya ha visto cómo realizar la carga diligente. Para ver un ejemplo de la carga explícita, reemplace el método `Index` por el código siguiente, que carga explícitamente la propiedad `Enrollments`. El código cambiado se resalta.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Después de obtener la entidad de `Course` seleccionada, el nuevo código carga explícitamente la propiedad de navegación `Enrollments` de ese curso:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

A continuación, carga explícitamente cada entidad `Enrollment` relacionada `Student`:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Tenga en cuenta que usa el método `Collection` para cargar una propiedad de colección, pero para una propiedad que contiene una sola entidad, use el método `Reference`.

Ejecute la página de índice del instructor ahora y no verá ninguna diferencia en lo que se muestra en la página, aunque ha cambiado la forma en que se recuperan los datos.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en el [acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Obtenido información sobre cómo cargar datos relacionados
> * Creado una página de cursos
> * Creado una página de instructores

Pase al artículo siguiente para obtener información sobre cómo actualizar datos relacionados.

> [!div class="nextstepaction"]
> [Actualización de datos relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
