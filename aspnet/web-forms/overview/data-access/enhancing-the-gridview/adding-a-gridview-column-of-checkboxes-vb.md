---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Agregar una columna GridView de casillas (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se examina cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario una manera intuitiva de seleccionar varias filas de la...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: c620b2eac5844d4030c1309b45e7d6a72d1f386a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592337"
---
# <a name="adding-a-gridview-column-of-checkboxes-vb"></a>Agregar una columna GridView de casillas de verificación (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) de la aplicación de ejemplo o [descarga de PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> En este tutorial se examina cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario una manera intuitiva de seleccionar varias filas de GridView.

## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos examinado cómo agregar una columna de botones de radio a GridView con el fin de seleccionar un registro determinado. Una columna de botones de radio es una interfaz de usuario adecuada cuando el usuario está limitado a elegir como máximo un elemento de la cuadrícula. Sin embargo, en ocasiones, es posible que desee permitir que el usuario seleccione un número arbitrario de elementos de la cuadrícula. Los clientes de correo electrónico basados en Web, por ejemplo, suelen mostrar la lista de mensajes con una columna de casillas. El usuario puede seleccionar un número arbitrario de mensajes y, a continuación, realizar alguna acción, como mover los mensajes de correo electrónico a otra carpeta o eliminarlos.

En este tutorial, veremos cómo agregar una columna de casillas y cómo determinar qué casillas se comprobó en el PostBack. En concreto, vamos a crear un ejemplo que imita la interfaz de usuario del cliente de correo electrónico basado en Web. En nuestro ejemplo se incluye un control GridView paginado que muestra los productos de la tabla de base de datos `Products` con una casilla en cada fila (vea la ilustración 1). Al hacer clic en el botón eliminar productos seleccionados, se eliminarán los productos seleccionados.

[![cada fila de producto incluye una casilla](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figura 1**: cada fila de producto incluye una casilla ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Paso 1: agregar un control GridView paginado que muestra información del producto

Antes de preocuparnos por agregar una columna de casillas, deje que se Centre primero en enumerar los productos en un control GridView que admita la paginación. Comience abriendo la página `CheckBoxField.aspx` en la carpeta `EnhancedGridView` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, estableciendo su `ID` en `Products`. A continuación, elija enlazar GridView a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para usar la clase `ProductsBLL`, llamando al método `GetProducts()` para devolver los datos. Puesto que este GridView será de solo lectura, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![crear un nuevo ObjectDataSource denominado ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figura 2**: creación de un nuevo ObjectDataSource denominado `ProductsDataSource` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))

[![configurar ObjectDataSource para recuperar datos mediante el método GetProducts ()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figura 3**: configuración de ObjectDataSource para recuperar datos con el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figura 4**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará automáticamente BoundColumns y CheckBoxColumn para los campos de datos relacionados con el producto. Como hicimos en el tutorial anterior, elimine todos los `ProductName`, `CategoryName`y `UnitPrice` BoundFields, y cambie las propiedades de `HeaderText` a product, Category y price. Configure el `UnitPrice` BoundField para que su valor tenga el formato de moneda. Configure también GridView para que admita la paginación. para ello, active la casilla habilitar paginación en la etiqueta inteligente.

Vamos a agregar también la interfaz de usuario para eliminar los productos seleccionados. Agregue un control Web Button debajo de GridView, estableciendo su `ID` en `DeleteSelectedProducts` y su propiedad `Text` para eliminar los productos seleccionados. En lugar de eliminar realmente los productos de la base de datos, en este ejemplo solo se mostrará un mensaje que indica los productos que se habrían eliminado. Para ello, agregue un control Web de etiqueta debajo del botón. Establezca su identificador en `DeleteResults`, borre su propiedad `Text` y establezca sus propiedades `Visible` y `EnableViewState` en `False`.

Después de realizar estos cambios, el marcado declarativo GridView, ObjectDataSource, Button y Label s debe ser similar al siguiente:

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Tómese un momento para ver la página en un explorador (vea la figura 5). En este punto, debería ver el nombre, la categoría y el precio de los diez primeros productos.

[![el nombre, la categoría y el precio de los diez primeros productos se muestran](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figura 5**: se enumeran el nombre, la categoría y el precio de los primeros diez productos ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Paso 2: agregar una columna de casillas

Como ASP.NET 2,0 incluye un CheckBoxField, es posible que se pueda usar para agregar una columna de casillas a un control GridView. Desafortunadamente, no es el caso, ya que CheckBoxField está diseñado para trabajar con un campo de datos booleano. Es decir, para poder usar CheckBoxField, se debe especificar el campo de datos subyacente cuyo valor se consulta para determinar si la casilla representada está activada. No se puede usar CheckBoxField para incluir solo una columna de casillas no comprobadas.

En su lugar, debemos agregar un TemplateField y agregar un control Web CheckBox a su `ItemTemplate`. Continúe y agregue un TemplateField al `Products` GridView y conviértalo en el primer campo (extremo izquierdo). En la etiqueta inteligente de GridView s, haga clic en el vínculo editar plantillas y, a continuación, arrastre un control Web CheckBox desde el cuadro de herramientas hasta el `ItemTemplate`. Establezca la propiedad CheckBox s `ID` en `ProductSelector`.

[![agregar un control Web CheckBox denominado ProductSelector a TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figura 6**: agregar un control Web CheckBox denominado `ProductSelector` al `ItemTemplate` TemplateField s ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))

Con el control Web TemplateField y CheckBox agregado, cada fila incluye ahora una casilla. En la figura 7 se muestra esta página, cuando se ve a través de un explorador, después de haber agregado TemplateField y CheckBox.

[![cada fila de producto incluye ahora una casilla](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figura 7**: ahora cada fila de producto incluye una casilla ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Paso 3: determinar qué casillas se comprobó en el PostBack

En este punto, tenemos una columna de casillas pero no hay forma de determinar qué casillas se comprobó en el PostBack. Sin embargo, cuando se hace clic en el botón eliminar productos seleccionados, es necesario saber qué casillas se comprobó para eliminar esos productos.

La propiedad GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) proporciona acceso a las filas de datos de GridView. Podemos recorrer en iteración estas filas, tener acceso mediante programación al control CheckBox y, a continuación, consultar su propiedad `Checked` para determinar si se ha seleccionado la casilla.

Cree un controlador de eventos para el evento de control Web Button `DeleteSelectedProducts` `Click` y agregue el código siguiente:

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

La propiedad `Rows` devuelve una colección de instancias de `GridViewRow` que estructuran las filas de datos de GridView. El bucle `For Each` aquí enumera esta colección. Para cada `GridViewRow` objeto, se tiene acceso mediante programación a la casilla Row s mediante `row.FindControl("controlID")`. Si la casilla está activada, la fila correspondiente `ProductID` valor se recupera de la colección `DataKeys`. En este ejercicio, simplemente se muestra un mensaje informativo en la etiqueta `DeleteResults`, aunque en una aplicación en funcionamiento, se realiza una llamada al método `DeleteProduct(productID)` de la clase `ProductsBLL`.

Con la adición de este controlador de eventos, al hacer clic en el botón eliminar productos seleccionados ahora se muestran los `ProductID` s de los productos seleccionados.

[![cuando se hace clic en el botón eliminar productos seleccionados, se muestran los productos seleccionados ProductIDs](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figura 8**: cuando se hace clic en el botón eliminar productos seleccionados, se muestran los productos seleccionados `ProductID` s ([haga clic para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Paso 4: agregar los botones marcar todo y desactivar todos

Si un usuario desea eliminar todos los productos de la página actual, deben comprobar cada una de las diez casillas. Podemos ayudarle a acelerar este proceso agregando un botón comprobar todo que, al hacer clic en él, selecciona todas las casillas de la cuadrícula. Un botón desactivar todo sería igualmente útil.

Agregue dos controles Web de botón a la página y colóquelos encima de GridView. Establezca los primeros `ID` en `CheckAll` y su propiedad `Text` en comprobar todo; Establezca la segunda `ID` s en `UncheckAll` y su propiedad `Text` en desactive todo.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

A continuación, cree un método en la clase de código subyacente denominado `ToggleCheckState(checkState)` que, cuando se invoca, enumera la colección `Products` GridView s `Rows` y establece cada propiedad CheckBox s `Checked` en el valor del parámetro *checkState* pasado.

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

A continuación, cree `Click` controladores de eventos para los botones `CheckAll` y `UncheckAll`. En el controlador de eventos `CheckAll` s, simplemente llame a `ToggleCheckState(True)`; en `UncheckAll`, llame a `ToggleCheckState(False)`.

[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Con este código, al hacer clic en el botón comprobar todo se produce un postback y se comprueban todas las casillas en GridView. Del mismo modo, al hacer clic en desactive todas las casillas. En la ilustración 9 se muestra la pantalla después de que se haya activado el botón comprobar todo.

[![hacer clic en el botón marcar todo selecciona todas las casillas](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figura 9**: al hacer clic en el botón marcar todo, se seleccionan todas las casillas de verificación ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))

> [!NOTE]
> Al mostrar una columna de casillas, una opción para seleccionar o anular la selección de todas las casillas se realiza a través de una casilla en la fila de encabezado. Además, la marca de verificación actual All/uncheck All Implementation requiere un postback. Sin embargo, las casillas se pueden activar o desactivar, en su totalidad mediante script de cliente, lo que proporciona una experiencia de usuario de snappier. Para explorar el uso de una casilla fila de encabezado para marcar todo y desproteger todo en detalle, junto con una explicación sobre el uso de las técnicas del lado cliente, consulte [activar todas las casillas en un control GridView mediante el script del lado cliente y una casilla Seleccionar todo](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).

## <a name="summary"></a>Resumen

En los casos en los que debe permitir que los usuarios elijan un número arbitrario de filas de un control GridView antes de continuar, la adición de una columna de casillas es una opción. Como vimos en este tutorial, la inclusión de una columna de casillas en GridView implica agregar un TemplateField con un control Web CheckBox. Mediante el uso de un control Web (e inyección de marcado directamente en la plantilla, como hicimos en el tutorial anterior) ASP.NET recuerda automáticamente qué casillas de verificación eran y no se comprobó en el PostBack. También se puede tener acceso mediante programación a las casillas del código para determinar si una casilla determinada está activada o para cambiar el estado de activación.

En este tutorial y en el último se ha examinado la adición de una columna de selector de filas a GridView. En el siguiente tutorial, veremos cómo, con un poco de trabajo, podemos agregar funcionalidades de inserción a GridView.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-radio-buttons-vb.md)
> [Siguiente](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
