---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
title: Eliminación por lotes (C#) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, se basa en un GridView mejorado creado en una versión anterior de TUT...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: ac6916d0-a5ab-4218-9760-7ba9e72d258c
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: ed832c38b4972f440ab64c141e29c85f0a9df920
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78476611"
---
# <a name="batch-deleting-c"></a>Eliminación por lotes (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_CS.zip) o [Descargar PDF](batch-deleting-cs/_static/datatutorial65cs1.pdf)

> Obtenga información sobre cómo eliminar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, se crea un GridView mejorado creado en un tutorial anterior. En la capa de acceso a datos se ajustan las operaciones de eliminación múltiples dentro de una transacción para asegurarse de que todas las eliminaciones se realizan correctamente o se revierten todas las eliminaciones.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](batch-updating-cs.md) se ha explorado cómo crear una interfaz de edición por lotes con un control GridView totalmente modificable. En situaciones en las que los usuarios están editando normalmente muchos registros a la vez, una interfaz de edición de lotes requerirá mucho menos postbacks y modificadores de contexto de teclado a mouse, mejorando así la eficacia de los usuarios finales. Esta técnica es igualmente útil para las páginas en las que es habitual que los usuarios eliminen muchos registros de una sola vez.

Cualquier persona que haya usado un cliente de correo electrónico en línea ya está familiarizado con una de las interfaces de eliminación de lotes más comunes: una casilla en cada fila de una cuadrícula con el botón eliminar todos los elementos seleccionados correspondiente (vea la figura 1). Este tutorial es bastante breve porque ya hemos hecho todo el trabajo en los tutoriales anteriores en la creación de la interfaz basada en Web y un método para eliminar una serie de registros como una única operación atómica. En el tutorial [Agregar una columna GridView de casillas](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) , hemos creado un control GridView con una columna de casillas y en el tutorial [ajustar modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-cs.md) se crea un método en el BLL que usaría una transacción para eliminar un `List<T>` de valores `ProductID`. En este tutorial, compilaremos y combinaremos nuestras experiencias anteriores para crear un ejemplo de eliminación de lotes de trabajo.

[![cada fila incluye una casilla](batch-deleting-cs/_static/image1.gif)](batch-deleting-cs/_static/image1.png)

**Figura 1**: cada fila incluye una casilla ([haga clic para ver la imagen de tamaño completo](batch-deleting-cs/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Paso 1: creación de la interfaz de eliminación de lotes

Como ya hemos creado la interfaz de eliminación de lotes en el tutorial [Agregar una columna GridView de casillas](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) , podemos simplemente copiarla en `BatchDelete.aspx` en lugar de crearla desde cero. Para empezar, abra la página `BatchDelete.aspx` en la carpeta `BatchData` y la página `CheckBoxField.aspx` en la carpeta `EnhancedGridView`. En la página `CheckBoxField.aspx`, vaya a la vista de código fuente y copie el marcado entre las etiquetas de `<asp:Content>` como se muestra en la figura 2.

[![copiar el marcado declarativo de CheckBoxField. aspx en el portapapeles](batch-deleting-cs/_static/image2.gif)](batch-deleting-cs/_static/image3.png)

**Figura 2**: copiar el marcado declarativo de `CheckBoxField.aspx` en el portapapeles ([haga clic para ver la imagen de tamaño completo](batch-deleting-cs/_static/image4.png))

A continuación, vaya a la vista Código fuente en `BatchDelete.aspx` y pegue el contenido del portapapeles dentro de las etiquetas de `<asp:Content>`. También copie y pegue el código desde la clase de código subyacente de `CheckBoxField.aspx.cs` en dentro de la clase de código subyacente en `BatchDelete.aspx.cs` (el controlador de eventos `DeleteSelectedProducts` Button s `Click`, el método `ToggleCheckState` y los controladores de eventos `Click` para los botones `CheckAll` y `UncheckAll`). Después de copiar sobre este contenido, la clase de código subyacente de la página de `BatchDelete.aspx` debe contener el código siguiente:

[!code-csharp[Main](batch-deleting-cs/samples/sample1.cs)]

Después de copiar sobre el marcado declarativo y el código fuente, dedique un momento a probar `BatchDelete.aspx` viéndolo a través de un explorador. Debería ver una lista de GridView con los diez primeros productos de un control GridView con cada fila que muestre el nombre, la categoría y el precio del producto junto con una casilla. Debe haber tres botones: activar todo, desactivar todo y eliminar productos seleccionados. Al hacer clic en el botón marcar todo, se seleccionan todas las casillas y se desactiva todas las casillas. Al hacer clic en eliminar productos seleccionados, se muestra un mensaje que indica los valores de `ProductID` de los productos seleccionados, pero no elimina realmente los productos.

[![la interfaz de CheckBoxField. aspx se ha pasado a BatchDeleting. aspx](batch-deleting-cs/_static/image3.gif)](batch-deleting-cs/_static/image5.png)

**Figura 3**: la interfaz de `CheckBoxField.aspx` se ha pasado a `BatchDeleting.aspx` ([haga clic para ver la imagen de tamaño completo](batch-deleting-cs/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Paso 2: eliminación de los productos comprobados mediante transacciones

Con la interfaz de eliminación de lotes copiada correctamente en `BatchDeleting.aspx`, lo único que queda es actualizar el código para que el botón eliminar productos seleccionados elimine los productos comprobados mediante el método `DeleteProductsWithTransaction` de la clase `ProductsBLL`. Este método, agregado en el tutorial [ajustar las modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-cs.md) , acepta como entrada una `List<T>` de `ProductID` valores y elimina cada `ProductID` correspondiente en el ámbito de una transacción.

El controlador de eventos `DeleteSelectedProducts` Button `Click` usa actualmente el siguiente bucle `foreach` para recorrer en iteración cada fila de GridView:

[!code-csharp[Main](batch-deleting-cs/samples/sample2.cs)]

Para cada fila, se hace referencia mediante programación al control Web de la casilla de `ProductSelector`. Si está activada, la fila s `ProductID` se recupera de la colección `DataKeys` y la propiedad `DeleteResults` etiqueta s `Text` se actualiza para incluir un mensaje que indica que la fila se ha seleccionado para su eliminación.

En realidad, el código anterior no elimina ningún registro a medida que la llamada a la clase `ProductsBLL` `Delete` método se marca como comentario. ¿Se ha aplicado esta lógica de eliminación? el código eliminaría los productos pero no dentro de una operación atómica. Es decir, si las primeras eliminaciones de la secuencia se realizaron correctamente, pero se produjo un error en una versión posterior (quizás debido a una infracción de la restricción de clave externa), se produciría una excepción, pero esos productos ya eliminados permanecerían eliminados.

Para garantizar la atomicidad, necesitamos usar el método de `DeleteProductsWithTransaction` de `ProductsBLL` Class s. Dado que este método acepta una lista de valores `ProductID`, es necesario compilar primero esta lista desde la cuadrícula y, a continuación, pasarla como un parámetro. Primero se crea una instancia de una `List<T>` de tipo `int`. En el bucle `foreach` es necesario agregar los productos seleccionados `ProductID` valores a este `List<T>`. Después del bucle, este `List<T>` debe pasarse al método `ProductsBLL` Class s `DeleteProductsWithTransaction`. Actualice el controlador de eventos `DeleteSelectedProducts` Button `Click` con el siguiente código:

[!code-csharp[Main](batch-deleting-cs/samples/sample3.cs)]

El código actualizado crea un `List<T>` del tipo `int` (`productIDsToDelete`) y lo rellena con los valores `ProductID` que se van a eliminar. Después del bucle `foreach`, si hay al menos un producto seleccionado, se llama al método `ProductsBLL` clase s `DeleteProductsWithTransaction` y se pasa esta lista. También se muestra la etiqueta de `DeleteResults` y los datos se reenlazan a GridView (de modo que los registros recién eliminados ya no aparecen como filas en la cuadrícula).

En la figura 4 se muestra GridView después de haber seleccionado un número de filas para su eliminación. En la figura 5 se muestra la pantalla inmediatamente después de hacer clic en el botón eliminar productos seleccionados. Tenga en cuenta que en la figura 5 los `ProductID` valores de los registros eliminados se muestran en la etiqueta debajo de GridView y esas filas ya no están en el control GridView.

[![los productos seleccionados se eliminarán](batch-deleting-cs/_static/image4.gif)](batch-deleting-cs/_static/image7.png)

**Figura 4**: se eliminarán los productos seleccionados ([haga clic para ver la imagen de tamaño completo](batch-deleting-cs/_static/image8.png))

[![los valores de ProductID de los productos eliminados aparecen debajo de GridView](batch-deleting-cs/_static/image5.gif)](batch-deleting-cs/_static/image9.png)

**Figura 5**: los productos eliminados `ProductID` valores aparecen debajo de GridView ([haga clic para ver la imagen de tamaño completo](batch-deleting-cs/_static/image10.png))

> [!NOTE]
> Para probar el `DeleteProductsWithTransaction` atomicidad del método, agregue manualmente una entrada para un producto en la tabla de `Order Details` y, a continuación, intente eliminar el producto (junto con otros). Recibirá una infracción de la restricción de clave externa al intentar eliminar el producto con un pedido asociado, pero tenga en cuenta cómo se revierten los otros productos seleccionados.

## <a name="summary"></a>Resumen

La creación de una interfaz de eliminación de lotes implica agregar un control GridView con una columna de casillas y un control Web de botón que, cuando se hace clic en él, eliminará todas las filas seleccionadas como una única operación atómica. En este tutorial, creamos una interfaz de este tipo juntar conjuntamente el trabajo realizado en dos tutoriales anteriores, [agregando una columna GridView de casillas](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs.md) y [ajustando las modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-cs.md). En el primer tutorial, se crea un control GridView con una columna de casillas y, en el último, se implementa un método en la capa BLL que, cuando se pasa una `List<T>` de valores de `ProductID`, se eliminan todos en el ámbito de una transacción.

En el siguiente tutorial, vamos a crear una interfaz para realizar inserciones por lotes.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Hilton Giesenow y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-cs.md)
> [Siguiente](batch-inserting-cs.md)
