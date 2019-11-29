---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Control de la simultaneidad con el Entity Framework en una aplicación ASP.NET MVC (7 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 0383974baa16bb0d5fc588f9303290bdb0fd979c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595343"
---
# <a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Control de la simultaneidad con el Entity Framework en una aplicación ASP.NET MVC (7 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En los dos tutoriales anteriores, trabajó con datos relacionados. En este tutorial se muestra cómo controlar la simultaneidad. Creará páginas web que funcionan con la entidad `Department` y las páginas que editan y eliminan `Department` entidades controlarán los errores de simultaneidad. En las ilustraciones siguientes se muestran las páginas de índice y de eliminación, incluidos algunos mensajes que se muestran si se produce un conflicto de simultaneidad.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Los conflictos de simultaneidad ocurren cuando un usuario muestra los datos de una entidad para editarlos y, después, otro usuario actualiza los datos de la misma entidad antes de que el primer cambio del usuario se escriba en la base de datos. Si no habilita la detección de este tipo de conflictos, quien actualice la base de datos en último lugar sobrescribe los cambios del otro usuario. En muchas aplicaciones, el riesgo es aceptable: si hay pocos usuarios o pocas actualizaciones, o si no es realmente importante si se sobrescriben algunos cambios, el costo de programación para la simultaneidad puede superar el beneficio obtenido. En ese caso, no tendrá que configurar la aplicación para que controle los conflictos de simultaneidad.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en casos de simultaneidad, una manera de hacerlo es usar los bloqueos de base de datos. Esto se denomina *simultaneidad pesimista*. Por ejemplo, antes de leer una fila de una base de datos, solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para acceso de actualización, no se permite que ningún otro usuario bloquee la fila como solo lectura o para acceso de actualización, porque recibirían una copia de los datos que se están modificando. Si bloquea una fila para acceso de solo lectura, otras personas también pueden bloquearla para acceso de solo lectura pero no para actualización.

Administrar los bloqueos tiene desventajas. Puede ser bastante complicado de programar. Requiere importantes recursos de administración de bases de datos y puede provocar problemas de rendimiento a medida que aumenta el número de usuarios de una aplicación (es decir, no se escala bien). Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. El Entity Framework no proporciona compatibilidad integrada para ello, y en este tutorial no se muestra cómo implementarlo.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es la *simultaneidad optimista*. La simultaneidad optimista implica permitir que se produzcan conflictos de simultaneidad y reaccionar correctamente si ocurren. Por ejemplo, Juan ejecuta la página de edición departments, cambia la cantidad de **presupuesto** del Departamento inglés de $350.000,00 a $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Antes de que John haga clic en **Guardar**, Julia ejecuta la misma página y cambia el campo de **fecha de inicio** de 9/1/2007 a 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Juan hace clic en **Guardar** primero y ve su cambio cuando el explorador vuelve a la página de índice y, a continuación, Julia hace clic en **Guardar**. Lo que sucede después viene determinado por cómo controla los conflictos de simultaneidad. Algunas de las opciones se exponen a continuación:

- Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos. En el escenario de ejemplo, no se perdería ningún dato porque los dos usuarios actualizaron diferentes propiedades. La próxima vez que un usuario examine el Departamento de inglés, verá los cambios de Juan y Julia, es decir, una fecha de inicio de 8/8/2013 y un presupuesto de cero dólares.

    Este método de actualización puede reducir el número de conflictos que pueden dar lugar a una pérdida de datos, pero no puede evitar la pérdida de datos si se realizan cambios paralelos a la misma propiedad de una entidad. Si Entity Framework funciona de esta manera o no, depende de cómo implemente el código de actualización. A menudo no resulta práctico en una aplicación web, porque puede requerir mantener grandes cantidades de estado con el fin de realizar un seguimiento de todos los valores de propiedad originales de una entidad, así como los valores nuevos. Mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor o debe incluirse en la propia página web (por ejemplo, en campos ocultos).
- Puede dejar que el cambio de Julia sobrescriba el cambio de John. La próxima vez que un usuario examine el Departamento de inglés, verá 8/8/2013 y el valor de $350.000,00 restaurado. Esto se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*. (Los valores del cliente tienen prioridad sobre lo que hay en el almacén de datos). Como se indicó en la introducción a esta sección, si no realiza ninguna codificación para el control de simultaneidad, esto ocurrirá automáticamente.
- Puede impedir que el cambio de Julia se actualice en la base de datos. Normalmente, se muestra un mensaje de error, se muestra el estado actual de los datos y se le permite volver a aplicar los cambios si aún desea realizarlos. Esto se denomina un escenario de *Prevalece el almacén*. (Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente). En este tutorial implementará el escenario de WINS de la tienda. Este método garantiza que ningún cambio se sobrescriba sin que se avise al usuario de lo que está sucediendo.

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

Puede resolver los conflictos controlando las excepciones de [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) que inicia el Entity Framework. Para saber cuándo se producen dichas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar correctamente la base de datos y el modelo de datos. Algunas opciones para habilitar la detección de conflictos son las siguientes:

- En la tabla de la base de datos, incluya una columna de seguimiento que pueda usarse para determinar si una fila ha cambiado. Después, puede configurar el Entity Framework para incluir esa columna en la cláusula `Where` de los comandos SQL `Update` o `Delete`.

    El tipo de datos de la columna Tracking suele ser [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). El valor [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) es un número secuencial que se incrementa cada vez que se actualiza la fila. En un comando `Update` o `Delete`, la cláusula `Where` incluye el valor original de la columna Tracking (la versión de fila original). Si otro usuario ha cambiado la fila que se está actualizando, el valor de la columna `rowversion` es diferente del valor original, por lo que la instrucción `Update` o `Delete` no puede encontrar la fila que se va a actualizar debido a la cláusula `Where`. Cuando el Entity Framework encuentra que no se ha actualizado ninguna fila con el comando `Update` o `Delete` (es decir, cuando el número de filas afectadas es cero), lo interpreta como un conflicto de simultaneidad.
- Configure el Entity Framework para incluir los valores originales de cada columna de la tabla en la cláusula `Where` de los comandos `Update` y `Delete`.

    Como en la primera opción, si alguna de las filas ha cambiado desde la primera vez que se leyó la fila, la cláusula `Where` no devolverá una fila para actualizar, lo que el Entity Framework interpreta como un conflicto de simultaneidad. En el caso de las tablas de base de datos que tienen muchas columnas, este enfoque puede dar lugar a cláusulas de `Where` muy grandes y puede requerir que se mantengan grandes cantidades de estado. Como se indicó anteriormente, el mantenimiento de grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor o debe incluirse en la propia página web. Por lo general, no se recomienda este enfoque y no es el método utilizado en este tutorial.

    Si desea implementar este enfoque en la simultaneidad, tiene que marcar todas las propiedades que no sean de clave principal en la entidad para la que desea realizar un seguimiento de la simultaneidad agregando el atributo [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) . Ese cambio permite que el Entity Framework incluya todas las columnas de la cláusula SQL `WHERE` de instrucciones `UPDATE`.

En el resto de este tutorial, agregará una propiedad de seguimiento de [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) a la entidad `Department`, creará un controlador y vistas, y probará para comprobar que todo funciona correctamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Agregar una propiedad de simultaneidad optimista a la entidad Department

En *Models\Department.CS*, agregue una propiedad de seguimiento denominada `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

El atributo [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) especifica que esta columna se incluirá en la cláusula `Where` de `Update` y `Delete` comandos que se envían a la base de datos. El atributo se denomina [timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) porque las versiones anteriores de SQL Server usaban un tipo de datos [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) de SQL antes de que el [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) de SQL lo reemplazara. El tipo .net para `rowversion` es una matriz de bytes. Si prefiere usar la API fluida, puede usar el método [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) para especificar la propiedad de seguimiento, tal y como se muestra en el ejemplo siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Al agregar una propiedad cambió el modelo de base de datos, por lo que necesita realizar otra migración. En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Crear un controlador de Departamento

Cree un controlador de `Department` y las vistas de la misma manera que los demás controladores, con las siguientes opciones:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

En *Controllers\DepartmentController.CS*, agregue una instrucción `using`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Cambie "LastName" por "FullName" en cualquier lugar de este archivo (cuatro repeticiones) para que las listas desplegables de administrador de Departamento contengan el nombre completo del instructor en lugar de simplemente el apellido.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Reemplace el código existente para el método `HttpPost` `Edit` por el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

La vista almacenará el valor de `RowVersion` original en un campo oculto. Cuando el enlazador de modelos crea la instancia de `department`, ese objeto tendrá el valor de la propiedad `RowVersion` original y los nuevos valores para las demás propiedades, como lo escribió el usuario en la página de edición. Después, cuando el Entity Framework crea un comando de `UPDATE` SQL, ese comando incluirá una cláusula `WHERE` que busca una fila que tenga el valor de `RowVersion` original.

Si no hay ninguna fila afectada por el comando `UPDATE` (ninguna fila tiene el valor de la `RowVersion` original), el Entity Framework produce una excepción de `DbUpdateConcurrencyException` y el código del bloque `catch` obtiene la entidad `Department` afectada del objeto de excepción. Esta entidad tiene los valores leídos de la base de datos y los nuevos valores especificados por el usuario:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Después, el código agrega un mensaje de error personalizado para cada columna que tiene valores de base de datos diferentes de lo que el usuario especificó en la página de edición:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Un mensaje de error más largo explica lo que ha sucedido y qué hacer con él:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Por último, el código establece el valor `RowVersion` del objeto `Department` en el nuevo valor recuperado de la base de datos. Este nuevo valor `RowVersion` se almacenará en el campo oculto cuando se vuelva a mostrar la página Edit y, la próxima vez que el usuario haga clic en **Save**, solo se detectarán los errores de simultaneidad que se produzcan desde que se vuelva a mostrar la página Edit.

En *Views\Department\Edit.cshtml*, agregue un campo oculto para guardar el valor de la propiedad `RowVersion`, inmediatamente después del campo oculto para la propiedad `DepartmentID`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

En *Views\Department\Index.cshtml*, reemplace el código existente por el código siguiente para que se muevan los vínculos de fila a la izquierda y cambie el título de la página y los encabezados de columna para mostrar `FullName` en lugar de `LastName` en la columna **Administrador** :

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Probar el control de simultaneidad optimista

Ejecute el sitio y haga clic en **departamentos**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Haga clic con el botón derecho en el hipervínculo de **edición** de Kim Alberti y seleccione **abrir en Nueva pestaña** y, a continuación, haga clic en el hipervínculo de **edición** para Kim Alberti. Las dos ventanas muestran la misma información.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Cambie un campo en la primera ventana del explorador y haga clic en **Guardar**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

El explorador muestra la página de índice con el valor modificado.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Cambie el campo any en la segunda ventana del explorador y haga clic en **Save (guardar**).

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Haga clic en **Guardar** en la segunda ventana del explorador. Verá un mensaje de error:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Vuelva a hacer clic en **Save**. El valor especificado en el segundo explorador se guarda junto con el valor original de los datos que se cambian en el primer explorador. Verá los valores guardados cuando aparezca la página de índice.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Actualización de la página eliminar

Para la página Delete, Entity Framework detecta los conflictos de simultaneidad causados por una persona que edita el departamento de forma similar. Cuando el método de `Delete` de `HttpGet` muestra la vista de confirmación, la vista incluye el valor de `RowVersion` original en un campo oculto. Ese valor está disponible para el método `HttpPost` `Delete` al que se llama cuando el usuario confirma la eliminación. Cuando el Entity Framework crea el comando de `DELETE` SQL, incluye una cláusula `WHERE` con el valor de `RowVersion` original. Si el comando da como resultado cero filas afectadas (lo que significa que la fila se cambió después de mostrarse la página de confirmación de eliminación), se produce una excepción de simultaneidad y se llama al método `HttpGet Delete` con una marca de error establecida en `true` para volver a mostrar la página de confirmación con un mensaje de error. También es posible que se vean afectadas cero filas porque otro usuario ha eliminado la fila, de modo que, en ese caso, se muestra un mensaje de error diferente.

En *DepartmentController.CS*, reemplace el método `Delete` `HttpGet` por el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

El método acepta un parámetro opcional que indica si la página volverá a aparecer después de un error de simultaneidad. Si esta marca está `true`, se envía un mensaje de error a la vista mediante una propiedad `ViewBag`.

Reemplace el código del método de `Delete` de `HttpPost` (denominado `DeleteConfirmed`) por el código siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

En el código al que se aplicó la técnica scaffolding que acaba de reemplazar, este método solo acepta un identificador de registro:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Ha cambiado este parámetro a una `Department` instancia de entidad creada por el enlazador de modelos. Esto proporciona acceso al valor de la propiedad `RowVersion` además de la clave de registro.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado `HttpPost` `Delete` método `DeleteConfirmed` para dar al método `HttpPost` una firma única. (CLR requiere que los métodos sobrecargados tengan parámetros de método diferentes). Ahora que las firmas son únicas, puede ceñirse a la Convención MVC y usar el mismo nombre para los métodos `HttpPost` y `HttpGet` DELETE.

Si se detecta un error de simultaneidad, el código vuelve a mostrar la página de confirmación de Delete y proporciona una marca que indica que se debería mostrar un mensaje de error de simultaneidad.

En *Views\Department\Delete.cshtml*, reemplace el código con scaffolding con el siguiente código que realice algunos cambios de formato y agregue un campo de mensaje de error. Los cambios aparecen resaltados.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Este código agrega un mensaje de error entre los encabezados `h2` y `h3`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Reemplaza `LastName` por `FullName` en el campo `Administrator`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Por último, agrega campos ocultos para las propiedades `DepartmentID` y `RowVersion` después de la instrucción `Html.BeginForm`:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ejecute la página de índice de departments. Haga clic con el botón derecho en el hipervínculo **eliminar** del Departamento de inglés y seleccione **abrir en nueva ventana** . a continuación, en la primera ventana, haga clic en el hipervínculo **Editar** del Departamento de inglés.

En la primera ventana, cambie uno de los valores y haga clic en **Guardar** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La página de índice confirma el cambio.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

En la segunda ventana, haga clic en **eliminar**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Verá el mensaje de error de simultaneidad y se actualizarán los valores de Department con lo que está actualmente en la base de datos.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Si vuelve a hacer clic en **Delete**, se le redirigirá a la página de índice, que muestra que se ha eliminado el departamento.

## <a name="summary"></a>Resumen

Con esto finaliza la introducción para el control de los conflictos de simultaneidad. Para obtener información sobre otras formas de controlar varios escenarios de simultaneidad, vea [patrones de simultaneidad optimista](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) y [trabajar con valores de propiedad](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) en el blog del equipo de Entity Framework. En el siguiente tutorial se muestra cómo implementar la herencia de tabla por jerarquía para las entidades `Instructor` y `Student`.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
