---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 2 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: bd6a2e29e6f0df04e39be29160e2e08cc99c4706
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78456451"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 2

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="the-entitydatasource-control"></a>Control EntityDataSource

En el tutorial anterior, creó un sitio web, una base de datos y un modelo de datos. En este tutorial, trabajará con el control de `EntityDataSource` que ASP.NET proporciona para facilitar el trabajo con un modelo de datos Entity Framework. Creará un control de `GridView` para mostrar y editar los datos de los estudiantes, un `DetailsView` control para agregar nuevos estudiantes y un control de `DropDownList` para seleccionar un departamento (que usará más adelante para mostrar los cursos asociados).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Tenga en cuenta que en esta aplicación no agregará la validación de entradas a páginas que actualicen la base de datos, y algunos de los errores de control no serán tan sólidos como serían necesarios en una aplicación de producción. Esto mantiene el tutorial centrado en el Entity Framework y evita que sea demasiado largo. Para obtener más información sobre cómo agregar estas características a la aplicación, consulte [validación de entradas de usuario en ASP.NET Web pages](https://msdn.microsoft.com/library/7kh55542.aspx) y [control de errores en páginas y aplicaciones de ASP.net](https://msdn.microsoft.com/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Agregar y configurar el control EntityDataSource

Comenzará configurando un control de `EntityDataSource` para leer `Person` entidades del conjunto de entidades `People`.

Asegúrese de que tiene Visual Studio abierto y de que está trabajando con el proyecto que creó en la parte 1. Si no ha compilado el proyecto desde que creó el modelo de datos o desde el último cambio realizado en él, compile el proyecto ahora. Los cambios en el modelo de datos no se ponen a disposición del diseñador hasta que se compile el proyecto.

Cree una nueva página web mediante el **formulario Web Forms con** la plantilla de página maestra y asígnele el nombre *Students. aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Especifique *site. Master* como la página maestra. Todas las páginas que cree para estos tutoriales usarán esta página maestra.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

En la vista **código fuente** , agregue un encabezado `h2` al control `Content` denominado `Content2`, como se muestra en el ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

En la pestaña **datos** del **cuadro de herramientas**, arrastre un control `EntityDataSource` a la página, colóquelo debajo del encabezado y cambie el identificador a `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Cambie a la vista **diseño** , haga clic en la etiqueta inteligente del control de origen de datos y, a continuación, haga clic en **Configurar origen de datos** para iniciar el Asistente para **Configurar origen de datos** .

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

En el paso del Asistente para **configurar el ObjectContext** , seleccione **SchoolEntities** como el valor de **conexión con nombre**y seleccione **SchoolEntities** como el valor **DefaultContainerName** . Después, haga clic en **Siguiente**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Nota: Si obtiene el siguiente cuadro de diálogo en este punto, debe compilar el proyecto antes de continuar.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

En el paso **Configurar selección de datos** , seleccione **People** como valor para **entitySetName**. En **seleccionar**, asegúrese de que la casilla de verificación **seleccionar un** ll está activada. A continuación, seleccione las opciones para habilitar Update y DELETE. Cuando haya terminado, haga clic en **Finalizar**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configuración de reglas de base de datos para permitir la eliminación

Creará una página que permite a los usuarios eliminar estudiantes de la tabla `Person`, que tiene tres relaciones con otras tablas (`Course`, `StudentGrade`y `OfficeAssignment`). De forma predeterminada, la base de datos impedirá que elimine una fila en `Person` si hay filas relacionadas en una de las otras tablas. Puede eliminar las filas relacionadas de forma manual en primer lugar o puede configurar la base de datos para eliminarlas automáticamente al eliminar una fila de `Person`. En el caso de los registros de estudiante de este tutorial, configurará la base de datos para eliminar automáticamente los datos relacionados. Dado que los alumnos solo pueden tener filas relacionadas en la tabla `StudentGrade`, solo tiene que configurar una de las tres relaciones.

Si utiliza el archivo *School. MDF* que descargó del proyecto que se incluye en este tutorial, puede omitir esta sección porque ya se han realizado estos cambios de configuración. Si creó la base de datos mediante la ejecución de un script, configure la base de datos mediante los procedimientos siguientes.

En **Explorador de servidores**, abra el diagrama de base de datos que creó en la parte 1. Haga clic con el botón secundario en la relación entre `Person` y `StudentGrade` (la línea entre las tablas) y, a continuación, seleccione **propiedades**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

En la ventana **propiedades** , expanda la **especificación de INSERT y Update** y establezca la propiedad **DeleteRule** en **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Guarde y cierre el diagrama. Si se le pregunta si desea actualizar la base de datos, haga clic en **sí**.

Para asegurarse de que el modelo mantiene las entidades que están en la memoria sincronizadas con lo que hace la base de datos, debe establecer las reglas correspondientes en el modelo de datos. Abra *SchoolModel. edmx*, haga clic con el botón secundario en la línea de asociación entre `Person` y `StudentGrade`y, a continuación, seleccione **propiedades**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

En la ventana **propiedades** , establezca **End1** en **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Guarde y cierre el archivo *SchoolModel. edmx* y, a continuación, recompile el proyecto.

En general, cuando la base de datos cambia, tiene varias opciones para sincronizar el modelo:

- Para determinados tipos de cambios (como agregar o actualizar tablas, vistas o procedimientos almacenados), haga clic con el botón derecho en el diseñador y seleccione **Actualizar modelo desde base de datos** para que el diseñador realice los cambios automáticamente.
- Vuelva a generar el modelo de datos.
- Realice actualizaciones manuales como esta.

En este caso, podría haber regenerado el modelo o actualizado las tablas afectadas por el cambio de relación, pero tendría que volver a realizar el cambio de nombre de campo (de `FirstName` a `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Usar un control GridView para leer y actualizar entidades

En esta sección, usará un control de `GridView` para mostrar, actualizar o eliminar alumnos.

Abra o cambie a *Students. aspx* y cambie a la vista **diseño** . En la pestaña **datos** del **cuadro de herramientas**, arrastre un control `GridView` a la derecha del control `EntityDataSource`, asígnele el nombre `StudentsGridView`, haga clic en la etiqueta inteligente y seleccione **StudentsEntityDataSource** como origen de datos.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Haga clic en **actualizar esquema** (haga clic en **sí** si se le pide confirmación) y, a continuación, haga clic en **Habilitar paginación**, **Habilitar ordenación**, **Habilitar edición**y **Habilitar eliminación**.

Haga clic en **Editar columnas**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

En el cuadro **campos seleccionados** , elimine **PersonID**, **LastName**y **HireDate**. Normalmente, no se muestra una clave de registro a los usuarios, la fecha de contratación no es relevante para los estudiantes y se colocan ambas partes del nombre en un campo, por lo que solo necesita uno de los campos de nombre).

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Seleccione el campo **FirstMidName** y, a continuación, haga clic en **convertir este campo en TemplateField**.

Haga lo mismo para **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Haga clic en **Aceptar** y, a continuación, cambie a la vista **código fuente** . Los cambios restantes serán más fáciles de realizar directamente en el marcado. El marcado del control `GridView` ahora es similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La primera columna después del campo de comando es un campo de plantilla que actualmente muestra el nombre. Cambie el marcado de este campo de plantilla para que sea similar al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

En el modo de presentación, dos controles `Label` muestran el nombre y el apellido. En el modo de edición, se proporcionan dos cuadros de texto para que pueda cambiar el nombre y el apellido. Al igual que con los controles `Label` en el modo de presentación, se usan las expresiones `Bind` y `Eval` exactamente como lo haría con los controles de origen de datos ASP.NET que se conectan directamente a las bases de datos. La única diferencia es que está especificando las propiedades de la entidad en lugar de las columnas de la base de datos.

La última columna es un campo de plantilla que muestra la fecha de inscripción. Cambie el marcado de este campo para que sea similar al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

En el modo de presentación y de edición, la cadena de formato "{0, d}" hace que la fecha se muestre en el formato "fecha corta". (Es posible que el equipo esté configurado para mostrar este formato de forma diferente a las imágenes de pantalla que se muestran en este tutorial).

Observe que en cada uno de estos campos de plantilla, el diseñador utilizó una expresión `Bind` de forma predeterminada, pero se ha cambiado a una expresión `Eval` en los elementos `ItemTemplate`. La expresión `Bind` hace que los datos estén disponibles en `GridView` propiedades del control en caso de que necesite tener acceso a los datos en el código. En esta página no es necesario tener acceso a estos datos en el código, por lo que puede usar `Eval`, que es más eficaz. Para obtener más información, consulte [obtención de los datos de los controles de datos](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisar el marcado del control EntityDataSource para mejorar el rendimiento

En el marcado del control `EntityDataSource`, quite los atributos `ConnectionString` y `DefaultContainerName` y reemplácelos por un atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. Este es un cambio que debe tomar cada vez que cree un control de `EntityDataSource`, a menos que necesite usar una conexión diferente de la que está codificada de forma rígida en la clase de contexto del objeto. El uso del atributo `ContextTypeName` proporciona las siguientes ventajas:

- Mejor rendimiento. Cuando el control `EntityDataSource` inicializa el modelo de datos mediante los atributos `ConnectionString` y `DefaultContainerName`, realiza un trabajo adicional para cargar los metadatos en cada solicitud. Esto no es necesario si se especifica el atributo `ContextTypeName`.
- La carga diferida está activada de forma predeterminada en las clases de contexto de objetos generados (como `SchoolEntities` en este tutorial) en Entity Framework 4,0. Esto significa que las propiedades de navegación se cargan con datos relacionados automáticamente justo cuando se necesitan. La carga diferida se explica con más detalle más adelante en este tutorial.
- Cualquier personalización que haya aplicado a la clase de contexto de objeto (en este caso, la clase `SchoolEntities`) estará disponible para los controles que utilicen el control `EntityDataSource`. La personalización de la clase de contexto del objeto es un tema avanzado que no se trata en esta serie de tutoriales. Para obtener más información, vea [extender Entity Framework tipos generados](https://msdn.microsoft.com/library/dd456844.aspx).

El marcado ahora será similar al ejemplo siguiente (el orden de las propiedades podría ser diferente):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

El atributo `EnableFlattening` hace referencia a una característica que se necesitaba en versiones anteriores del Entity Framework porque las columnas de clave externa no se exponían como propiedades de entidad. La versión actual hace posible el uso de *asociaciones de clave externa*, lo que significa que las propiedades de clave externa se exponen para todas las asociaciones de menos de varios a varios. Si las entidades tienen propiedades de clave externa y no hay [tipos complejos](https://msdn.microsoft.com/library/bb738472.aspx), puede dejar este atributo establecido en `False`. No quite el atributo del marcado, ya que el valor predeterminado es `True`. Para obtener más información, vea [Flatten (objetos) (EntityDataSource)](https://msdn.microsoft.com/library/ee404746.aspx).

Ejecute la página y verá una lista de estudiantes y empleados (va a filtrar solo para estudiantes en el siguiente tutorial). El nombre y los apellidos se muestran juntos.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Para ordenar la presentación, haga clic en el nombre de una columna.

Haga clic en **Editar** en cualquier fila. Se muestran cuadros de texto donde puede cambiar el nombre y el apellido.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

El botón **eliminar** también funciona. Haga clic en eliminar para una fila que tenga una fecha de inscripción y la fila desaparezca. (Las filas sin una fecha de inscripción representan a los instructores y es posible que obtenga un error de integridad referencial. En el siguiente tutorial, filtrará esta lista para incluir solo a los estudiantes).

## <a name="displaying-data-from-a-navigation-property"></a>Mostrar datos de una propiedad de navegación

Supongamos ahora que desea saber cuántos cursos está inscrito cada estudiante en. El Entity Framework proporciona esa información en la propiedad de navegación `StudentGrades` de la entidad `Person`. Dado que el diseño de la base de datos no permite que un estudiante se inscriba en un curso sin tener una categoría asignada, en este tutorial puede suponer que tener una fila en la `StudentGrade` fila de la tabla asociada a un curso es la misma que la inscripción en el curso. (La propiedad de navegación `Courses` es solo para los instructores).

Cuando se usa el atributo `ContextTypeName` del control `EntityDataSource`, el Entity Framework recupera automáticamente la información de una propiedad de navegación cuando se obtiene acceso a esa propiedad. Esto se denomina *carga diferida*. Sin embargo, esto puede ser ineficaz, ya que resulta en una llamada independiente a la base de datos cada vez que se necesita información adicional. Si necesita datos de la propiedad de navegación para cada entidad devuelta por el control `EntityDataSource`, resulta más eficaz recuperar los datos relacionados junto con la propia entidad en una única llamada a la base de datos. Esto se denomina *carga diligente*y se especifica la carga diligente para una propiedad de navegación estableciendo la propiedad `Include` del control `EntityDataSource`.

En *Students. aspx*, desea mostrar el número de cursos de cada estudiante, por lo que la carga diligente es la mejor opción. Si estuviera mostrando todos los estudiantes pero mostrando el número de cursos solo para algunos de ellos (lo que requeriría escribir código adicional al marcado), la carga diferida podría ser una opción mejor.

Abra o cambie a *Students. aspx*, cambie a la vista **diseño** , seleccione `StudentsEntityDataSource`y, en la ventana **propiedades** , establezca la propiedad **include** en **StudentGrades**. (Si desea obtener varias propiedades de navegación, puede especificar sus nombres separados por comas, por ejemplo, **StudentGrades, Courses**).

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Cambie a la vista **código fuente** . En el control `StudentsGridView`, después del último elemento `asp:TemplateField`, agregue el siguiente campo de plantilla nuevo:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

En la expresión `Eval`, puede hacer referencia a la propiedad de navegación `StudentGrades`. Dado que esta propiedad contiene una colección, tiene una `Count` propiedad que puede usar para mostrar el número de cursos en los que se inscribe el estudiante. En un tutorial posterior, verá cómo mostrar los datos de las propiedades de navegación que contienen entidades individuales en lugar de colecciones. (Tenga en cuenta que no puede usar `BoundField` elementos para Mostrar datos de las propiedades de navegación).

Ejecute la página y ahora verá Cuántos cursos están inscritos en cada estudiante.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Usar un control DetailsView para insertar entidades

El siguiente paso es crear una página que tenga un control `DetailsView` que le permita agregar nuevos alumnos. Cierre el explorador y, a continuación, cree una nueva página web mediante la página maestra *site. Master* . Asigne a la página el nombre *StudentsAdd. aspx*y cambie a la vista **código fuente** .

Agregue el marcado siguiente para reemplazar el marcado existente para el control `Content` denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Este marcado crea un control `EntityDataSource` que es similar a la que creó en *Students. aspx*, salvo que habilita la inserción. Al igual que con el control de `GridView`, los campos enlazados del control de `DetailsView` se codifican exactamente como serían para un control de datos que se conecta directamente a una base de datos, salvo que hacen referencia a las propiedades de la entidad. En este caso, el control de `DetailsView` solo se usa para insertar filas, por lo que se ha establecido el modo predeterminado en `Insert`.

Ejecute la página y agregue un estudiante nuevo.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

No se producirá nada después de insertar un estudiante nuevo, pero si ahora ejecuta Student *. aspx*, verá la nueva información de estudiante.

## <a name="displaying-data-in-a-drop-down-list"></a>Mostrar datos en una lista desplegable

En los pasos siguientes se enlazará un control `DropDownList` a un conjunto de entidades mediante un control `EntityDataSource`. En esta parte del tutorial, no hará mucho con esta lista. Sin embargo, en los elementos siguientes, usará la lista para permitir que los usuarios seleccionen un departamento para mostrar los cursos asociados al Departamento.

Cree una nueva página web denominada *Courses. aspx*. En la vista **código fuente** , agregue un encabezado al control `Content` denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

En la vista **diseño** , agregue un control `EntityDataSource` a la página como hizo antes, con la excepción de que en este momento se le denomina `DepartmentsEntityDataSource`. Seleccione **departments (departamentos** ) como el valor de **entitySetName** y seleccione solo las propiedades **departmentId** y **Name** .

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

En la pestaña **estándar** del **cuadro de herramientas**, arrastre un control `DropDownList` a la página, asígnele el nombre `DepartmentsDropDownList`, haga clic en la etiqueta inteligente y seleccione **elegir origen de datos** para iniciar el Asistente para configuración de **origen**de datos.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

En el paso **elegir un origen de datos** , seleccione **DepartmentsEntityDataSource** como origen de datos, haga clic en **actualizar esquema**y, a continuación, seleccione **nombre** como el campo de datos que se va a mostrar y **departmentId** como el campo de datos del valor. Haga clic en **Aceptar**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

El método que se usa para enlazar el control mediante el Entity Framework es el mismo que con otros controles de origen de datos de ASP.NET, excepto que se especifican las entidades y las propiedades de la entidad.

Cambie a la vista **código fuente** y agregue "seleccionar un departamento:" inmediatamente antes del control `DropDownList`.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Como recordatorio, cambie el marcado del control `EntityDataSource` en este punto reemplazando los atributos `ConnectionString` y `DefaultContainerName` por un atributo `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"`. A menudo es mejor esperar hasta que haya creado el control enlazado a datos que está vinculado al control de origen de datos antes de cambiar el marcado de control de `EntityDataSource`, porque después de realizar el cambio, el diseñador no le proporcionará una opción **actualizar esquema** en el control enlazado a datos.

Ejecute la página y seleccione un departamento en la lista desplegable.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Esto completa la introducción al uso del control `EntityDataSource`. Trabajar con este control no suele ser diferente de trabajar con otros controles de origen de datos de ASP.NET, salvo que se hace referencia a las entidades y propiedades en lugar de a las tablas y columnas. La única excepción es cuando se desea tener acceso a las propiedades de navegación. En el siguiente tutorial, verá que la sintaxis que se usa con `EntityDataSource` control también podría diferir de otros controles de origen de datos al filtrar, agrupar y ordenar los datos.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-1.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-3.md)
