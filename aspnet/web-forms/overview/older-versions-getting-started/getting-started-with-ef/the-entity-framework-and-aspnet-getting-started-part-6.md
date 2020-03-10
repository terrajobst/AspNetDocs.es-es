---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 6 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 8bfbe74f90eb03ea9aab7610842ef2578e80d113
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454921"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 6

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementar la herencia de tabla por jerarquía

En el tutorial anterior trabajó con datos relacionados agregando y eliminando relaciones y agregando una nueva entidad que tenía una relación con una entidad existente. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la herencia para facilitar el trabajo con clases relacionadas. Por ejemplo, podría crear `Instructor` y `Student` clases que derivan de una clase base `Person`. Puede crear los mismos tipos de estructuras de herencia entre entidades en el Entity Framework.

En esta parte del tutorial, no creará nuevas páginas Web. En su lugar, agregará entidades derivadas al modelo de datos y modificará las páginas existentes para utilizar las nuevas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Herencia de tabla por jerarquía y de tabla por tipo

Una base de datos puede almacenar información sobre objetos relacionados en una tabla o en varias tablas. Por ejemplo, en la base de datos `School`, la tabla `Person` incluye información acerca de los estudiantes y los instructores en una sola tabla. Algunas de las columnas solo se aplican a los instructores (`HireDate`), algunas solo a los estudiantes (`EnrollmentDate`) y otras a ambos (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Puede configurar la Entity Framework para crear `Instructor` y `Student` entidades que heredan de la entidad `Person`. Este patrón de generación de una estructura de herencia de entidades a partir de una tabla de base de datos única se denomina herencia de *tabla por jerarquía* (TPH).

En el caso de los cursos, el `School` base de datos utiliza un patrón diferente. Los cursos en línea y los cursos en el sitio se almacenan en tablas independientes, cada una de las cuales tiene una clave externa que señala a la tabla `Course`. La información común a ambos tipos de curso solo se almacena en la tabla `Course`.

[![IMAGE12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Puede configurar el Entity Framework modelo de datos para que las entidades `OnlineCourse` y `OnsiteCourse` hereden de la entidad `Course`. Este patrón de generación de una estructura de herencia de entidades a partir de tablas independientes para cada tipo, donde cada tabla independiente hace referencia a una tabla que almacena los datos comunes a todos los tipos, se denomina herencia de *tabla por tipo* (TPT).

Los patrones de herencia TPH suelen ofrecer un mejor rendimiento en el Entity Framework que los patrones de herencia de TPT, ya que los patrones de TPT pueden dar lugar a consultas complejas de combinación. En este tutorial se muestra cómo implementar la herencia TPH. Para ello, realice los pasos siguientes:

- Cree `Instructor` y `Student` tipos de entidad que se deriven de `Person`.
- Mueva las propiedades que pertenecen a las entidades derivadas de la entidad `Person` a las entidades derivadas.
- Establezca restricciones en las propiedades de los tipos derivados.
- Convertir la entidad `Person` en una entidad abstracta.
- Asigne cada entidad derivada a la tabla de `Person` con una condición que especifique cómo determinar si una fila de `Person` representa ese tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Agregar entidades instructor y Student

Abra el archivo <em>SchoolModel. edmx</em> , haga clic con el botón secundario en un área no ocupada del diseñador, seleccione <strong>Agregar</strong>y, a continuación, <strong>entidad</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

En el cuadro de diálogo **Agregar entidad** , asigne a la entidad el nombre `Instructor` y establezca su opción de **tipo base** en `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Haga clic en **Aceptar**. El diseñador crea una entidad `Instructor` que se deriva de la entidad `Person`. La nueva entidad todavía no tiene ninguna propiedad.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita el procedimiento para crear una entidad `Student` que también derive de `Person`.

Solo los instructores tienen fechas de contratación, por lo que debe trasladar la propiedad de la entidad `Person` a la entidad `Instructor`. En la entidad `Person`, haga clic con el botón secundario en la propiedad `HireDate` y haga clic en **cortar**. A continuación, haga clic con el botón secundario en **propiedades** en la entidad `Instructor` y haga clic en **pegar**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La fecha de contratación de una entidad `Instructor` no puede ser null. Haga clic con el botón secundario en la propiedad `HireDate`, haga clic en **propiedades**y, a continuación, en la ventana **propiedades** , cambie `Nullable` a `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita el procedimiento para pasar la propiedad `EnrollmentDate` de la entidad `Person` a la entidad `Student`. Asegúrese de que también establece `Nullable` en `False` para la propiedad `EnrollmentDate`.

Ahora que una entidad `Person` tiene solo las propiedades que son comunes a `Instructor` y `Student` entidades (aparte de las propiedades de navegación, que no se mueven), la entidad solo se puede usar como entidad base en la estructura de herencia. Por lo tanto, debe asegurarse de que nunca se trata como una entidad independiente. Haga clic con el botón secundario en la entidad `Person`, seleccione **propiedades**y, a continuación, en la ventana **propiedades** , cambie el valor de la propiedad **abstracta** a **true**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Asignación de entidades instructor y Student a la tabla Person

Ahora debe indicar a la Entity Framework cómo diferenciar entre `Instructor` y `Student` entidades en la base de datos.

Haga clic con el botón secundario en la entidad `Instructor` y seleccione **asignación de tabla**. En la ventana detalles de la **asignación** , haga clic en **Agregar una tabla o vista** y seleccione **persona**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Haga clic en **Agregar una condición**y, a continuación, seleccione **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Cambie **operador** a **is** y **Value/Property** a **not null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita el procedimiento para la entidad `Students`, especificando que esta entidad se asigna a la tabla `Person` cuando la columna `EnrollmentDate` no es NULL. A continuación, guarde y cierre el modelo de datos.

Compile el proyecto para crear las nuevas entidades como clases y hacer que estén disponibles en el diseñador.

## <a name="using-the-instructor-and-student-entities"></a>Uso de las entidades instructor y Student

Al crear las páginas web que funcionan con los datos de los estudiantes y los instructores, se enlazan a la `Person` conjunto de entidades y se filtra en la propiedad `HireDate` o `EnrollmentDate` para restringir los datos devueltos a los estudiantes o instructores. Sin embargo, ahora, al enlazar cada control de origen de datos al conjunto de entidades `Person`, puede especificar que solo se deben seleccionar los tipos de entidad `Student` o `Instructor`. Dado que el Entity Framework sabe cómo diferenciar a los estudiantes y los instructores en el conjunto de entidades `Person`, puede quitar los valores de propiedades de `Where` que escribió manualmente para hacerlo.

En el diseñador de Visual Studio, puede especificar el tipo de entidad que debe seleccionar un control `EntityDataSource` en el cuadro desplegable **EntityTypeFilter** del asistente para `Configure Data Source`, como se muestra en el ejemplo siguiente.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Y, en la ventana **propiedades** , puede quitar `Where` valores de la cláusula que ya no son necesarios, como se muestra en el ejemplo siguiente.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Sin embargo, dado que ha cambiado el marcado de `EntityDataSource` controles para usar el atributo `ContextTypeName`, no puede ejecutar el Asistente para **configurar orígenes de datos** en `EntityDataSource` controles que ya ha creado. Por lo tanto, realizará los cambios necesarios cambiando el marcado en su lugar.

Abra la página *Students. aspx* . En el control `StudentsEntityDataSource`, quite el atributo `Where` y agregue un atributo `EntityTypeFilter="Student"`. El marcado ahora será similar al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Al establecer el atributo `EntityTypeFilter` se garantiza que el control `EntityDataSource` seleccione solo el tipo de entidad especificado. Si desea recuperar los tipos de entidad `Student` y `Instructor`, no debe establecer este atributo. (Tiene la opción de recuperar varios tipos de entidad con un control `EntityDataSource` solo si está utilizando el control para el acceso a datos de solo lectura. Si usa un control `EntityDataSource` para insertar, actualizar o eliminar entidades, y si el conjunto de entidades al que está enlazado puede contener varios tipos, solo puede trabajar con un tipo de entidad y debe establecer este atributo).

Repita el procedimiento para el control `SearchEntityDataSource`, excepto quitar solo la parte del atributo `Where` que selecciona `Student` entidades en lugar de quitar la propiedad por completo. La etiqueta de apertura del control ahora será similar a la del ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Ejecute la página para comprobar que sigue funcionando como antes.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Actualice las siguientes páginas que creó en los tutoriales anteriores para que usen las nuevas entidades `Student` y `Instructor` en lugar de `Person` entidades y, después, ejecutarlas para comprobar que funcionan como antes:

- En *StudentsAdd. aspx*, agregue `EntityTypeFilter="Student"` al control `StudentsEntityDataSource`. El marcado ahora será similar al ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- En *About. aspx*, agregue `EntityTypeFilter="Student"` al control `StudentStatisticsEntityDataSource` y quite `Where="it.EnrollmentDate is not null"`. El marcado ahora será similar al ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- En *instructors. aspx* y *InstructorsCourses. aspx*, agregue `EntityTypeFilter="Instructor"` al control `InstructorsEntityDataSource` y quite `Where="it.HireDate is not null"`. El marcado en *instructors. aspx* ahora es similar al ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    El marcado de *InstructorsCourses. aspx* ahora será similar al ejemplo siguiente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado de estos cambios, ha mejorado la facilidad de mantenimiento de la aplicación contoso University de varias maneras. Ha quitado la lógica de selección y validación del nivel de la interfaz de usuario (marcado *. aspx* ) y la ha convertido en una parte integral de la capa de acceso a datos. Esto ayuda a aislar el código de aplicación de los cambios que puede realizar en el futuro en el esquema de la base de datos o en el modelo de datos. Por ejemplo, podría decidir que los estudiantes se contrataran como ayudas de los profesores y, por lo tanto, obtendrían una fecha de contratación. Después, puede Agregar una nueva propiedad para diferenciar a los estudiantes de los instructores y actualizar el modelo de datos. No tendría que cambiar ningún código en la aplicación Web, excepto donde deseara mostrar una fecha de contratación para estudiantes. Otra ventaja de agregar entidades `Instructor` y `Student` es que el código es más fácil de entender que cuando se hace referencia a `Person` objetos que realmente son estudiantes o instructores.

Ahora ha visto una manera de implementar un patrón de herencia en el Entity Framework. En el siguiente tutorial, aprenderá a usar procedimientos almacenados para tener más control sobre cómo el Entity Framework tiene acceso a la base de datos.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-7.md)
