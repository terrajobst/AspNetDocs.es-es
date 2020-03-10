---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 5 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 75328e67abb4295b619cac5423a9eb970942fff7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78423871"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 5

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="working-with-related-data-continued"></a>Trabajar con datos relacionados, continuación

En el tutorial anterior comenzó a usar el control de `EntityDataSource` para trabajar con datos relacionados. En las propiedades de navegación se muestran varios niveles de jerarquía y datos editados. En este tutorial, continuará trabajando con datos relacionados agregando y eliminando relaciones y agregando una nueva entidad que tenga una relación con una entidad existente.

Creará una página que agrega cursos asignados a los departamentos. Los departamentos ya existen y, cuando se crea un nuevo curso, al mismo tiempo establecerá una relación entre él y un departamento existente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

También creará una página que funciona con una relación de varios a varios asignando un instructor a un curso (agregando una relación entre dos entidades que seleccione) o quitando un instructor de un curso (quitando una relación entre dos entidades que usted Seleccione). En la base de datos, al agregar una relación entre un instructor y un curso, se agrega una nueva fila a la tabla de asociación `CourseInstructor`. la eliminación de una relación implica la eliminación de una fila de la tabla de asociación `CourseInstructor`. Sin embargo, esto se hace en el Entity Framework estableciendo las propiedades de navegación, sin hacer referencia a la tabla de `CourseInstructor` explícitamente.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Agregar una entidad con una relación a una entidad existente

Cree una nueva página web denominada *CoursesAdd. aspx* que use la página maestra *site. Master* y agregue el marcado siguiente al control `Content` denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Este marcado crea un control `EntityDataSource` que selecciona los cursos, que habilita la inserción y que especifica un controlador para el evento `Inserting`. Usará el controlador para actualizar la propiedad de navegación `Department` cuando se crea una nueva entidad de `Course`.

El marcado también crea un control `DetailsView` que se utilizará para agregar nuevas entidades `Course`. El marcado usa campos enlazados para las propiedades de la entidad `Course`. Tiene que escribir el valor `CourseID` porque no es un campo de identificador generado por el sistema. En su lugar, es un número de curso que se debe especificar manualmente al crear el curso.

Use un campo de plantilla para la propiedad de navegación `Department` porque las propiedades de navegación no se pueden usar con controles de `BoundField`. El campo de plantilla proporciona una lista desplegable para seleccionar el Departamento. La lista desplegable se enlaza al conjunto de entidades `Departments` mediante `Eval` en lugar de `Bind`, de nuevo porque no se pueden enlazar directamente las propiedades de navegación para actualizarlas. Se especifica un controlador para el evento de `Init` del control `DropDownList` para que pueda almacenar una referencia al control para que lo use el código que actualiza la clave externa `DepartmentID`.

En *CoursesAdd.aspx.CS* inmediatamente después de la declaración de clase parcial, agregue un campo de clase para que contenga una referencia al control `DepartmentsDropDownList`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Agregue un controlador para el evento `Init` del control `DepartmentsDropDownList` para que pueda almacenar una referencia al control. Esto le permite obtener el valor que el usuario ha escrito y usarlo para actualizar el valor `DepartmentID` de la entidad `Course`.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Agregue un controlador para el evento de `Inserting` del control `DetailsView`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Cuando el usuario hace clic en `Insert`, se genera el evento de `Inserting` antes de que se inserte el nuevo registro. El código del controlador obtiene el `DepartmentID` del control `DropDownList` y lo usa para establecer el valor que se utilizará para la propiedad `DepartmentID` de la entidad `Course`.

El Entity Framework se encargará de agregar este curso a la propiedad de navegación `Courses` de la entidad `Department` asociada. También agrega el Departamento a la propiedad de navegación `Department` de la entidad `Course`.

Ejecute la página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Escriba un identificador, un título, un número de créditos y seleccione un departamento y, a continuación, haga clic en **Insertar**.

Ejecute la página *Courses. aspx* y seleccione el mismo Departamento para ver el nuevo curso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Trabajar con relaciones de varios a varios

La relación entre el conjunto de entidades `Courses` y el conjunto de entidades `People` es una relación de varios a varios. Una entidad `Course` tiene una propiedad de navegación llamada `People` que puede contener cero, una o más entidades `Person` relacionadas (que representan a los instructores asignados para enseñar ese curso). Y una entidad `Person` tiene una propiedad de navegación llamada `Courses` que puede contener cero, una o más entidades `Course` relacionadas (que representan los cursos que el instructor tiene asignados para enseñar). Un instructor puede enseñar varios cursos y un curso puede ser impartido por varios instructores. En esta sección del tutorial, agregará y quitará las relaciones entre `Person` y `Course` entidades mediante la actualización de las propiedades de navegación de las entidades relacionadas.

Cree una nueva página web denominada *InstructorsCourses. aspx* que use la página maestra *site. Master* y agregue el marcado siguiente al control `Content` denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Este marcado crea un control de `EntityDataSource` que recupera el nombre y `PersonID` de `Person` entidades para los instructores. Un control `DropDrownList` se enlaza al control `EntityDataSource`. El control `DropDownList` especifica un controlador para el evento `DataBound`. Usará este controlador para enlazar los dos listas desplegables que muestran los cursos.

El marcado también crea el siguiente grupo de controles que se utiliza para asignar un curso al instructor seleccionado:

- Un control de `DropDownList` para seleccionar el curso que se va a asignar. Este control se rellenará con los cursos que actualmente no están asignados al instructor seleccionado.
- Control `Button` para iniciar la asignación.
- Un control `Label` para mostrar un mensaje de error si se produce un error en la asignación.

Por último, el marcado también crea un grupo de controles que se usarán para quitar un curso del instructor seleccionado.

En *InstructorsCourses.aspx.CS*, agregue una instrucción using:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Agregue un método para rellenar las dos listas desplegables que muestran los cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Este código obtiene todos los cursos del conjunto de entidades `Courses` y obtiene los cursos de la propiedad de navegación `Courses` de la entidad `Person` para el instructor seleccionado. A continuación, determina qué cursos están asignados a ese instructor y rellena las listas desplegables en consecuencia.

Agregue un controlador para el evento de `Click` del botón `Assign`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Este código obtiene la entidad `Person` del instructor seleccionado, obtiene la entidad `Course` para el curso seleccionado y agrega el curso seleccionado a la propiedad de navegación `Courses` de la entidad `Person` del instructor. Después guarda los cambios en la base de datos y vuelve a rellenar las listas desplegables para que los resultados se puedan ver inmediatamente.

Agregue un controlador para el evento de `Click` del botón `Remove`:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Este código obtiene la entidad `Person` del instructor seleccionado, obtiene la entidad `Course` para el curso seleccionado y quita el curso seleccionado de la propiedad de navegación `Courses` de la entidad `Person`. Después guarda los cambios en la base de datos y vuelve a rellenar las listas desplegables para que los resultados se puedan ver inmediatamente.

Agregue código al método `Page_Load` que asegúrese de que los mensajes de error no estén visibles cuando no haya ningún error para informar y agregue Controladores para los eventos `DataBound` y `SelectedIndexChanged` de la lista desplegable instructores para rellenar las listas desplegables de los cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Ejecute la página.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Seleccione un instructor. La lista desplegable <strong>asignar un curso</strong> muestra los cursos que el instructor no enseña y la lista desplegable <strong>quitar un curso</strong> muestra los cursos a los que ya está asignado el instructor. En la sección <strong>asignar un curso</strong> , seleccione un curso y, a continuación, haga clic en <strong>asignar</strong>. El curso se desplaza a la lista desplegable <strong>quitar un curso</strong> . Seleccione un curso en la sección <strong>quitar un curso</strong> y haga clic en <strong>quitar</strong><em>.</em> El curso se mueve a la lista desplegable <strong>asignar un curso</strong> .

Ahora ha detectado algunas maneras más de trabajar con datos relacionados. En el siguiente tutorial, aprenderá a usar la herencia en el modelo de datos para mejorar el mantenimiento de la aplicación.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-6.md)
