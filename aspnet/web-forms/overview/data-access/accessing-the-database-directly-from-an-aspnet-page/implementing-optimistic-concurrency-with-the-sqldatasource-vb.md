---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: Implementar la simultaneidad optimista con SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, revisaremos los aspectos básicos del control de simultaneidad optimista y, a continuación, exploraremos cómo implementarlo mediante el control SqlDataSource.
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 431734b5245c20ac840147cf0827fa7f8d1e4d17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445315"
---
# <a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementar la simultaneidad optimista con SqlDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) de la aplicación de ejemplo o [descarga de PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> En este tutorial, revisaremos los aspectos básicos del control de simultaneidad optimista y, a continuación, exploraremos cómo implementarlo mediante el control SqlDataSource.

## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos examinado cómo agregar funciones de inserción, actualización y eliminación al control SqlDataSource. En Resumen, para proporcionar estas características, necesitábamos especificar el `INSERT`, `UPDATE`o `DELETE` instrucción SQL correspondiente en las propiedades control s `InsertCommand`, `UpdateCommand`o `DeleteCommand`, junto con los parámetros adecuados en las colecciones `InsertParameters`, `UpdateParameters`y `DeleteParameters`. Aunque estas propiedades y recopilaciones se pueden especificar manualmente, el botón Opciones avanzadas del Asistente para configurar orígenes de datos ofrece una instrucción generar `INSERT`, `UPDATE`y `DELETE` que creará automáticamente estas instrucciones basadas en la instrucción `SELECT`.

Junto con las instrucciones generar `INSERT`, `UPDATE`y `DELETE`, el cuadro de diálogo Opciones de generación SQL avanzadas incluye una opción usar simultaneidad optimista (vea la figura 1). Cuando esta opción está activada, las cláusulas `WHERE` de las instrucciones autogeneradas `UPDATE` y `DELETE` se modifican para realizar solo la actualización o la eliminación si no se han modificado los datos de la base de datos subyacente desde que el usuario cargó los datos por última vez en la cuadrícula.

![Puede Agregar compatibilidad con simultaneidad optimista desde el cuadro de diálogo Opciones de generación SQL avanzadas.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Figura 1**: puede Agregar compatibilidad con simultaneidad optimista desde el cuadro de diálogo Opciones de generación SQL avanzadas.

De nuevo en el tutorial de [implementación de simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , hemos examinado los aspectos básicos del control de simultaneidad optimista y cómo agregarlo a ObjectDataSource. En este tutorial, volveremos a tocar el control de simultaneidad optimista y, a continuación, exploraremos cómo implementarlo mediante el uso de SqlDataSource.

## <a name="a-recap-of-optimistic-concurrency"></a>Resumen de la simultaneidad optimista

En el caso de las aplicaciones web que permiten que varios usuarios simultáneos editen o eliminen los mismos datos, existe la posibilidad de que un usuario pueda sobrescribir accidentalmente otros cambios. En el tutorial [implementación de simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) , proporcioné el ejemplo siguiente:

Imagine que dos usuarios, Jisun y Sam, visitaron una página de una aplicación que permitía a los visitantes actualizar y eliminar productos a través de un control GridView. Ambos haga clic en el botón Editar para Chai en torno a la misma hora. Jisun cambia el nombre del producto a té de Chai y hace clic en el botón actualizar. El resultado neto es una instrucción `UPDATE` que se envía a la base de datos, que establece *todos* los campos actualizables de los productos (aunque Jisun solo se actualice un campo, `ProductName`). En este momento, la base de datos tiene los valores de Chai té, la categoría Beverages, los líquidos exóticos del proveedor, etc. para este producto en particular. Sin embargo, la pantalla GridView en Sam s sigue mostrando el nombre del producto en la fila de GridView modificable como Chai. Unos segundos después de que se hayan confirmado los cambios de JISUN, Sam actualiza la categoría a los condimentos y hace clic en actualizar. Esto da como resultado una `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto en Chai, el `CategoryID` en el identificador de la categoría de condimentos correspondiente, etc. Se han sobrescrito los cambios de Jisun s en el nombre del producto.

En la ilustración 2 se muestra esta interacción.

[![cuando dos usuarios actualizan simultáneamente un registro, es posible que un usuario cambie para sobrescribir los demás](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Figura 2**: cuando dos usuarios actualizan simultáneamente un registro, es posible que un usuario cambie para sobrescribir los otros ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))

Para evitar que este escenario se despliega, se debe implementar una forma de [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) . La [simultaneidad optimista](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) el enfoque de este tutorial funciona en la suposición de que, aunque puede haber conflictos de simultaneidad cada vez y, a continuación, no surgirá la inmensa mayoría del tiempo. Por lo tanto, si se produce un conflicto, el control de simultaneidad optimista informa al usuario de que sus cambios se pueden guardar porque otro usuario ha modificado los mismos datos.

> [!NOTE]
> En el caso de las aplicaciones en las que se supone que habrá muchos conflictos de simultaneidad o si estos conflictos no son tolerables, se puede usar en su lugar el control de simultaneidad pesimista. Consulte el tutorial [implementación de simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) para obtener una explicación más detallada sobre el control de simultaneidad pesimista.

El control de simultaneidad optimista funciona asegurándose de que el registro que se está actualizando o eliminando tiene los mismos valores que cuando se inició el proceso de actualización o eliminación. Por ejemplo, al hacer clic en el botón Editar en un control GridView modificable, los valores del registro se leen en la base de datos y se muestran en los cuadros de texto y otros controles Web. GridView guarda estos valores originales. Más adelante, después de que el usuario realice los cambios y haga clic en el botón actualizar, la instrucción `UPDATE` utilizada debe tener en cuenta los valores originales más los valores nuevos y solo actualizar el registro de la base de datos subyacente si los valores originales que el usuario ha empezado a editar son idénticos a los valores que se encuentran en la base de datos. En la ilustración 3 se muestra esta secuencia de eventos.

[![para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores de la base de datos actual.](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores de la base de datos actual ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))

Existen varios enfoques para implementar la simultaneidad optimista (vea la [lógica de actualización de simultaneidad optimista](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter a. Bromberg](http://peterbromberg.net/)para ver una breve descripción de una serie de opciones). La técnica que se usa en SqlDataSource (así como en los conjuntos de datos con tipo ADO.NET utilizados en nuestra capa de acceso a datos) aumenta la cláusula `WHERE` para incluir una comparación de todos los valores originales. Por ejemplo, la siguiente instrucción `UPDATE` actualiza el nombre y el precio de un producto solo si los valores de la base de datos actual son iguales a los valores que se recuperaron originalmente al actualizar el registro en GridView. Los parámetros `@ProductName` y `@UnitPrice` contienen los nuevos valores introducidos por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en el control GridView cuando se hizo clic en el botón Editar:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Como veremos en este tutorial, la habilitación del control de simultaneidad optimista con SqlDataSource es tan sencilla como activar una casilla.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Paso 1: crear un SqlDataSource que admita la simultaneidad optimista

Para empezar, abra la página de `OptimisticConcurrency.aspx` de la carpeta `SqlDataSource`. Arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador, conconfiguración de su propiedad `ID` en `ProductsDataSourceWithOptimisticConcurrency`. A continuación, haga clic en el vínculo configurar origen de datos de la etiqueta inteligente control s. En la primera pantalla del asistente, elija trabajar con la `NORTHWINDConnectionString` y haga clic en siguiente.

[![elegir trabajar con NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: elegir trabajar con la `NORTHWINDConnectionString` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))

En este ejemplo, vamos a agregar un control GridView que permite a los usuarios editar la tabla `Products`. Por lo tanto, en la pantalla configurar la instrucción SELECT, elija la tabla `Products` de la lista desplegable y seleccione las columnas `ProductID`, `ProductName`, `UnitPrice`y `Discontinued`, tal como se muestra en la figura 5.

[![de la tabla Products, devolver las columnas ProductID, ProductName, UnitPrice y Discontinued](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Figura 5**: en la tabla `Products`, devuelva las columnas `ProductID`, `ProductName`, `UnitPrice`y `Discontinued` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))

Después de seleccionar las columnas, haga clic en el botón avanzadas para abrir el cuadro de diálogo Opciones avanzadas de generación de SQL. Active la casilla generar `INSERT`, `UPDATE`y `DELETE` y use las casillas de simultaneidad optimista y haga clic en Aceptar (consulte la figura 1 para obtener una captura de pantalla). Para completar el asistente, haga clic en siguiente y, a continuación, en finalizar.

Después de completar el Asistente para configurar el origen de datos, dedique un momento a examinar el `DeleteCommand` resultante y `UpdateCommand` las propiedades y las colecciones `DeleteParameters` y `UpdateParameters`. La forma más fácil de hacerlo es hacer clic en la pestaña origen en la esquina inferior izquierda para ver la sintaxis declarativa de la página s. Allí encontrará un valor `UpdateCommand` de:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Con siete parámetros en la colección `UpdateParameters`:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Del mismo modo, la propiedad `DeleteCommand` y la colección `DeleteParameters` deben tener un aspecto similar al siguiente:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Además de aumentar las cláusulas `WHERE` de las propiedades `UpdateCommand` y `DeleteCommand` (y agregar los parámetros adicionales a las colecciones de parámetros respectivas), al seleccionar la opción usar simultaneidad optimista, se ajustan otras dos propiedades:

- Cambia la [propiedad`ConflictDetection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) de `OverwriteChanges` (valor predeterminado) a `CompareAllValues`
- Cambia la [propiedad`OldValuesParameterFormatString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) de {0} (valor predeterminado) a la {0} original\_.

Cuando el control Web de datos invoca el método SqlDataSource s `Update()` o `Delete()`, pasa los valores originales. Si la propiedad SqlDataSource s `ConflictDetection` está establecida en `CompareAllValues`, estos valores originales se agregan al comando. La propiedad `OldValuesParameterFormatString` proporciona el patrón de nomenclatura que se usa para estos parámetros de valor original. En el Asistente para configurar orígenes de datos se usa el\_original {0} y se asigna el nombre de cada parámetro original en las propiedades `UpdateCommand` y `DeleteCommand` y `UpdateParameters` y `DeleteParameters` colecciones en consecuencia.

> [!NOTE]
> Dado que no se van a usar las funcionalidades de inserción del control SqlDataSource, no dude en quitar la propiedad `InsertCommand` y su colección `InsertParameters`.

## <a name="correctly-handlingnullvalues"></a>Controlar correctamente los valores de`NULL`

Desafortunadamente, las instrucciones `UPDATE` y `DELETE` aumentadas automáticamente por el Asistente para configurar orígenes de datos cuando se usa la simultaneidad optimista *no* funcionan con registros que contienen valores `NULL`. Para ver por qué, considere nuestra `UpdateCommand`de SqlDataSource:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

La columna `UnitPrice` de la tabla `Products` puede tener valores `NULL`. Si un registro determinado tiene un valor de `NULL` para `UnitPrice`, la parte de la cláusula `WHERE` `[UnitPrice] = @original_UnitPrice` *siempre* se evaluará como false porque `NULL = NULL` siempre devuelve false. Por lo tanto, los registros que contienen valores `NULL` no se pueden editar ni eliminar, ya que las instrucciones `UPDATE` y `DELETE` `WHERE` cláusulas no devuelven ninguna fila para actualizar o eliminar.

> [!NOTE]
> Este error se comunicó por primera vez a Microsoft en junio de 2004 en [SqlDataSource genera instrucciones SQL incorrectas](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) y está programada para corregirse en la próxima versión de ASP.net.

Para solucionar este comportamiento, es necesario actualizar manualmente las cláusulas de `WHERE` en las propiedades `UpdateCommand` y `DeleteCommand` de **todas** las columnas que pueden tener `NULL` valores. En general, cambie `[ColumnName] = @original_ColumnName` a:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Esta modificación se puede realizar directamente a través del marcado declarativo, a través de las opciones UpdateQuery o DeleteQuery del ventana Propiedades, o a través de las pestañas actualizar y eliminar de la opción especificar una instrucción SQL personalizada o un procedimiento almacenado en configurar datos Asistente para orígenes. De nuevo, esta modificación se debe realizar para *cada* columna de la cláusula `UpdateCommand` y `DeleteCommand` s `WHERE` que pueda contener valores de `NULL`.

Aplicar esto a nuestro ejemplo produce los siguientes valores de `UpdateCommand` y `DeleteCommand` modificados:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Paso 2: agregar un control GridView con opciones de edición y eliminación

Con SqlDataSource configurado para admitir la simultaneidad optimista, lo único que queda es agregar un control Web de datos a la página que use este control de simultaneidad. En este tutorial, vamos a agregar un control GridView que proporcione la funcionalidad de edición y eliminación. Para ello, arrastre un control GridView desde el cuadro de herramientas hasta el diseñador y establezca su `ID` en `Products`. En la etiqueta inteligente de GridView s, enlácelo al control SqlDataSource `ProductsDataSourceWithOptimisticConcurrency` agregado en el paso 1. Por último, active las opciones habilitar edición y habilitar eliminación de la etiqueta inteligente.

[![enlazar GridView a SqlDataSource y habilitar la edición y eliminación](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Ilustración 6**: enlazar GridView a SqlDataSource y habilitar la edición y eliminación ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))

Después de agregar GridView, configure su apariencia quitando el `ProductID` BoundField, cambiando la propiedad `ProductName` BoundField s `HeaderText` a product y actualizando el `UnitPrice` BoundField para que su propiedad `HeaderText` sea simplemente Price. Idealmente, se mejora la interfaz de edición para incluir un RequiredFieldValidator para el valor `ProductName` y un CompareValidator para el valor `UnitPrice` (para asegurarse de que es un valor numérico con el formato correcto). Consulte el tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obtener una visión más detallada de la personalización de la interfaz de edición de GridView.

> [!NOTE]
> El estado de vista de GridView se debe habilitar, ya que los valores originales pasados desde GridView a SqlDataSource se almacenan en el estado de vista.

Después de realizar estas modificaciones en GridView, el marcado declarativo de GridView y SqlDataSource debe ser similar al siguiente:

[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Para ver el control de simultaneidad optimista en acción, abra dos ventanas del explorador y cargue la página `OptimisticConcurrency.aspx` en ambos. Haga clic en los botones de edición del primer producto en los dos exploradores. En un explorador, cambie el nombre del producto y haga clic en actualizar. El explorador devolverá el valor de postback y GridView volverá a su modo de edición previa, mostrando el nuevo nombre de producto para el registro que se acaba de editar.

En la segunda ventana del explorador, cambie el precio (pero deje el nombre del producto como su valor original) y haga clic en actualizar. En el postback, la cuadrícula vuelve al modo de edición previa, pero no se registra el cambio en el precio. El segundo explorador muestra el mismo valor que el nombre del nuevo producto con el precio antiguo. Se han perdido los cambios realizados en la segunda ventana del explorador. Además, los cambios se han perdido en silencio, ya que no había ninguna excepción o mensaje que indicara que se ha producido una infracción de simultaneidad.

[![los cambios en la segunda ventana del explorador se han perdido de forma silenciosa](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Figura 7**: los cambios en la segunda ventana del explorador se han perdido de forma silenciosa ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))

La razón por la que no se confirmaron los cambios del segundo explorador se debía a que la cláusula `UPDATE` instrucción s `WHERE` filtraba todos los registros y, por tanto, no afectaba a ninguna fila. Vuelva a ver la instrucción `UPDATE`:

[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Cuando la segunda ventana del explorador actualiza el registro, el nombre del producto original especificado en la cláusula `WHERE` no coincide con el nombre del producto existente (ya que se cambió en el primer explorador). Por lo tanto, la instrucción `[ProductName] = @original_ProductName` devuelve false y el `UPDATE` no afecta a ningún registro.

> [!NOTE]
> La eliminación funciona de la misma manera. Con dos ventanas del explorador abiertas, empiece editando un producto determinado con uno y, a continuación, guardando los cambios. Después de guardar los cambios en un explorador, haga clic en el botón Eliminar para el mismo producto en el otro. Dado que los valores originales no coinciden en la cláusula `DELETE` instrucción s `WHERE`, se produce un error en la eliminación en modo silencioso.

Desde la perspectiva del usuario final en la segunda ventana del explorador, después de hacer clic en el botón actualizar, la cuadrícula vuelve al modo de edición previa, pero se han perdido sus cambios. Sin embargo, no hay ningún comentario visual de que los cambios no se adhieren. Idealmente, si se pierden los cambios de un usuario en una infracción de simultaneidad, es posible notificarlos y, quizás, mantener la cuadrícula en el modo de edición. Echemos un vistazo a cómo hacerlo.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Paso 3: determinar cuándo se ha producido una infracción de simultaneidad

Dado que una infracción de simultaneidad rechaza los cambios realizados, sería estupendo avisar al usuario cuando se ha producido una infracción de simultaneidad. Para avisar al usuario, agregue un control Web de etiqueta en la parte superior de la página denominada `ConcurrencyViolationMessage` cuya propiedad `Text` muestre el siguiente mensaje: ha intentado actualizar o eliminar un registro que otro usuario ha actualizado simultáneamente. Revise los cambios del otro usuario y, a continuación, vuelva a hacer la actualización o la eliminación. Establezca la propiedad control de etiqueta s `CssClass` en ADVERTENCIA, que es una clase CSS definida en `Styles.css` que muestra texto en una fuente roja, cursiva, negrita y grande. Por último, establezca las propiedades etiqueta s `Visible` y `EnableViewState` en `False`. Esto ocultará la etiqueta salvo solo los postbacks en los que establecemos explícitamente su propiedad `Visible` en `True`.

[![agregar un control etiqueta a la página para mostrar la advertencia](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Figura 8**: agregar un control etiqueta a la página para mostrar la advertencia ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))

Al realizar una actualización o una eliminación, los controladores de eventos `RowUpdated` y `RowDeleted` de GridView se activan después de que el control de origen de datos haya realizado la actualización o eliminación solicitada. Podemos determinar el número de filas afectadas por la operación de estos controladores de eventos. Si se vieron afectadas cero filas, queremos mostrar el `ConcurrencyViolationMessage` etiqueta.

Cree un controlador de eventos para los eventos `RowUpdated` y `RowDeleted` y agregue el código siguiente:

[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

En ambos controladores de eventos, se comprueba la propiedad `e.AffectedRows` y, si es igual a 0, se establece la propiedad `ConcurrencyViolationMessage` etiqueta s `Visible` en `True`. En el controlador de eventos `RowUpdated`, también se indica a GridView que permanezca en modo de edición estableciendo la propiedad `KeepInEditMode` en true. Al hacerlo, es necesario volver a enlazar los datos a la cuadrícula para que los demás datos del usuario se carguen en la interfaz de edición. Esto se logra llamando al método `DataBind()` de GridView.

Como se muestra en la figura 9, con estos dos controladores de eventos, se muestra un mensaje muy perceptible siempre que se produce una infracción de simultaneidad.

[![un mensaje se muestra en la superficie de una infracción de simultaneidad](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Figura 9**: un mensaje se muestra en la superficie de una infracción de simultaneidad ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))

## <a name="summary"></a>Resumen

Al crear una aplicación web donde varios usuarios simultáneos pueden estar editando los mismos datos, es importante tener en cuenta las opciones de control de simultaneidad. De forma predeterminada, los controles Web de datos y los controles de origen de datos de ASP.NET no emplean ningún control de simultaneidad. Como vimos en este tutorial, implementar el control de simultaneidad optimista con SqlDataSource es relativamente rápido y sencillo. SqlDataSource controla la mayoría de los tareas de la adición de cláusulas de `WHERE` aumentadas a las instrucciones `UPDATE` y `DELETE` generadas automáticamente, pero hay algunos matices en el control de las columnas de valor de `NULL`, como se describe en la sección control de valores `NULL` correctamente.

En este tutorial concluye el examen de SqlDataSource. Nuestros demás tutoriales volverán a trabajar con datos mediante la arquitectura de ObjectDataSource y en capas.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
