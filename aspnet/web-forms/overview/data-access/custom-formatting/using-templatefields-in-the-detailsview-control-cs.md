---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
title: Usar TemplateFields en el control DetailsView (C#) | Microsoft Docs
author: rick-anderson
description: Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial se mostrará un producto...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 83efb21f-b231-446e-9356-f4c6cbcc6713
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 55c667800a9fb19d200bcf4b68e2c59ca4ef5d0e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622230"
---
# <a name="using-templatefields-in-the-detailsview-control-c"></a>Usar TemplateFields en el control DetailsView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_13_CS.exe) de la aplicación de ejemplo o [descarga de PDF](using-templatefields-in-the-detailsview-control-cs/_static/datatutorial13cs1.pdf)

> Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial, se mostrará un producto a la vez mediante un objeto DetailsView que contiene TemplateFields.

## <a name="introduction"></a>Introducción

TemplateField ofrece un mayor grado de flexibilidad en la representación de datos que los controles de campo de datos BoundField, CheckBoxField, HyperLinkField y otros. En el [tutorial anterior](using-templatefields-in-the-gridview-control-cs.md) , analizamos el uso de TemplateField en un control GridView para:

- Muestra varios valores de campo de datos en una columna. En concreto, los campos `FirstName` y `LastName` se combinaron en una columna de GridView.
- Use un control Web alternativo para expresar un valor de campo de datos. Vimos cómo mostrar el valor de `HiredDate` mediante un control de calendario.
- Mostrar información de estado basada en los datos subyacentes. Aunque la tabla `Employees` no contiene una columna que devuelve el número de días que un empleado ha estado en el trabajo, pudimos mostrar dicha información en el ejemplo de GridView del tutorial anterior con el uso de un método TemplateField y Format.

Las mismas capacidades de TemplateFields disponibles con GridView también están disponibles con el control DetailsView. En este tutorial, se mostrará un producto a la vez mediante un DetailsView que contiene dos TemplateFields. La primera TemplateField combinará los campos de datos `UnitPrice`, `UnitsInStock`y `UnitsOnOrder` en una fila de DetailsView. El segundo TemplateField mostrará el valor del campo `Discontinued`, pero usará un método de formato para mostrar "sí" si `Discontinued` está `true`y "NO" en caso contrario.

[![se usan dos TemplateFields para personalizar la pantalla](using-templatefields-in-the-detailsview-control-cs/_static/image2.png)](using-templatefields-in-the-detailsview-control-cs/_static/image1.png)

**Figura 1**: se usan dos TemplateFields para personalizar la pantalla ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image3.png))

Comencemos.

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Paso 1: enlazar los datos a DetailsView

Como se explicó en el tutorial anterior, al trabajar con TemplateFields a menudo es más fácil empezar por crear el control DetailsView que contiene solo BoundFields y, a continuación, agregar New TemplateFields o convertir el BoundFields existente en TemplateFields según sea necesario. . Por lo tanto, inicie este tutorial agregando un DetailsView a la página a través del diseñador y enlazándolo a un ObjectDataSource que devuelve la lista de productos. En estos pasos se creará un DetailsView con BoundFields para cada uno de los campos de valor que no son booleanos del producto y un CheckBoxField para el campo de valor booleano (discontinuo).

Abra la página `DetailsViewTemplateField.aspx` y arrastre un DetailsView desde el cuadro de herramientas hasta el diseñador. En la etiqueta inteligente de DetailsView, elija Agregar un nuevo control ObjectDataSource que invoque el método de `GetProducts()` de la clase `ProductsBLL`.

[![agregar un nuevo control ObjectDataSource que invoca el método GetProducts ()](using-templatefields-in-the-detailsview-control-cs/_static/image5.png)](using-templatefields-in-the-detailsview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo control ObjectDataSource que invoca el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image6.png))

Para este informe, quite los `ProductID`, `SupplierID`, `CategoryID`y `ReorderLevel` BoundFields. A continuación, reordene el BoundFields para que los BoundFields de `CategoryName` y `SupplierName` aparezcan inmediatamente después de la `ProductName` BoundField. No dude en ajustar las propiedades del `HeaderText` y las propiedades de formato del BoundFields como considere oportuno. Al igual que con GridView, estas ediciones de nivel BoundField se pueden realizar a través del cuadro de diálogo campos (al que se tiene acceso al hacer clic en el vínculo editar campos de la etiqueta inteligente de DetailsView) o a través de la sintaxis declarativa. Por último, borre los valores de las propiedades `Height` y `Width` de DetailsView para permitir que el control DetailsView se expanda en función de los datos que se muestran y active la casilla habilitar paginación en la etiqueta inteligente.

Después de realizar estos cambios, el marcado declarativo del control DetailsView debe ser similar al siguiente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample1.aspx)]

Tómese un momento para ver la página a través de un explorador. En este momento debería ver un único producto en la lista (Chai) con filas que muestran el nombre del producto, la categoría, el proveedor, el precio, las unidades en existencias, las unidades en el pedido y su estado descontinuado.

[![los detalles del producto se muestran mediante una serie de BoundFields](using-templatefields-in-the-detailsview-control-cs/_static/image8.png)](using-templatefields-in-the-detailsview-control-cs/_static/image7.png)

**Figura 3**: los detalles del producto se muestran mediante una serie de BoundFields ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Paso 2: combinación del precio, las unidades en existencias y las unidades en orden en una fila

DetailsView tiene una fila para los campos `UnitPrice`, `UnitsInStock`y `UnitsOnOrder`. Los campos de datos se pueden combinar en una sola fila con TemplateField, ya sea mediante la adición de una nueva TemplateField o mediante la conversión de uno de los `UnitPrice`existentes, `UnitsInStock`y `UnitsOnOrder` BoundFields en TemplateField. Aunque personalmente prefiero convertir los BoundFields existentes, vamos a practicar agregando una nueva TemplateField.

Para empezar, haga clic en el vínculo editar campos de la etiqueta inteligente de DetailsView para abrir el cuadro de diálogo campos. Después, agregue una nueva TemplateField y establezca su propiedad `HeaderText` en "Price e Inventory" y mueva el nuevo TemplateField para que esté situado encima del `UnitPrice` BoundField.

[![agregar un nuevo TemplateField al control DetailsView](using-templatefields-in-the-detailsview-control-cs/_static/image11.png)](using-templatefields-in-the-detailsview-control-cs/_static/image10.png)

**Ilustración 4**: agregar una nueva TemplateField al control DetailsView ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image12.png))

Dado que esta nueva TemplateField contendrá los valores que se muestran actualmente en el `UnitPrice`, `UnitsInStock`y `UnitsOnOrder` BoundFields, vamos a quitarlos.

La última tarea de este paso es definir el marcado de `ItemTemplate` para los valores de precio e inventario TemplateField, que se pueden realizar mediante la interfaz de edición de plantillas de DetailsView en el diseñador o a mano a través de la sintaxis declarativa del control. Al igual que con GridView, se puede tener acceso a la interfaz de edición de plantillas de DetailsView haciendo clic en el vínculo editar plantillas de la etiqueta inteligente. Desde aquí puede seleccionar la plantilla que se va a editar en la lista desplegable y, a continuación, agregar los controles Web desde el cuadro de herramientas.

Para este tutorial, empiece por agregar un control de etiqueta al `ItemTemplate`de costo e inventario de la misma. A continuación, haga clic en el vínculo editar DataBindings de la etiqueta inteligente del control Web de etiqueta y enlace la propiedad `Text` al campo `UnitPrice`.

[![enlazar la propiedad Text de la etiqueta al campo de datos UnitPrice](using-templatefields-in-the-detailsview-control-cs/_static/image14.png)](using-templatefields-in-the-detailsview-control-cs/_static/image13.png)

**Figura 5**: enlace de la propiedad `Text` de la etiqueta al campo de datos `UnitPrice` ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Dar formato al precio como moneda

Con esta adición, el precio del control Web Label y el inventario TemplateField mostrarán ahora solo el precio del producto seleccionado. En la ilustración 6 se muestra una captura de pantalla de nuestro progreso hasta ahora cuando se ve a través de un explorador.

[![el precio y el inventario de TemplateField muestra el precio](using-templatefields-in-the-detailsview-control-cs/_static/image17.png)](using-templatefields-in-the-detailsview-control-cs/_static/image16.png)

**Figura 6**: el precio y el inventario de TemplateField muestran el precio ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image18.png))

Tenga en cuenta que el precio del producto no tiene el formato de moneda. Con un BoundField, el formato es posible estableciendo la propiedad `HtmlEncode` en `false` y la propiedad `DataFormatString` en `{0:formatSpecifier}`. En el caso de TemplateField, sin embargo, las instrucciones de formato deben especificarse en la sintaxis de DataBinding o mediante el uso de un método de formato definido en algún lugar del código de la aplicación (por ejemplo, en la clase de código subyacente de la página ASP.NET).

Para especificar el formato de la sintaxis de DataBinding utilizada en el control Web Label, vuelva al cuadro de diálogo DataBindings; para ello, haga clic en el vínculo editar DataBindings de la etiqueta inteligente de la etiqueta. Puede escribir las instrucciones de formato directamente en la lista desplegable formato o seleccionar una de las cadenas de formato definidas. Al igual que con la propiedad `DataFormatString` de BoundField, el formato se especifica mediante `{0:formatSpecifier}`.

En el campo `UnitPrice` use el formato de moneda especificado seleccionando el valor de la lista desplegable correspondiente o escribiendo `{0:C}` a mano.

[![formato del precio como moneda](using-templatefields-in-the-detailsview-control-cs/_static/image20.png)](using-templatefields-in-the-detailsview-control-cs/_static/image19.png)

**Figura 7**: formato del precio como moneda ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image21.png))

Mediante declaración, la especificación de formato se indica como un segundo parámetro en los métodos `Bind` o `Eval`. La configuración que se acaba de realizar en el diseñador da como resultado la siguiente expresión de enlace de enlaces en el marcado declarativo:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Agregar los campos de datos restantes a TemplateField

En este punto, hemos mostrado y formateado el `UnitPrice` campo de datos en el precio y el inventario de TemplateField, pero todavía deben mostrar los campos `UnitsInStock` y `UnitsOnOrder`. Vamos a mostrarlos en una línea por debajo del precio y entre paréntesis. En la interfaz de edición de plantillas del diseñador, este marcado se puede Agregar colocando el cursor dentro de la plantilla y escribiendo simplemente el texto que se va a mostrar. Como alternativa, este marcado se puede escribir directamente en la sintaxis declarativa.

Agregue el marcado estático, los controles Web de etiqueta y la sintaxis de enlace de datos para que el precio y el inventario TemplateField muestren la información de precio y de inventario, como por ejemplo:

*UnitPrice*  
(**En existencias/en orden:** *UnidadesEnExistencias* / *UnidadesEnPedido*)

Después de realizar esta tarea, el marcado declarativo de DetailsView debe ser similar al siguiente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample3.aspx)]

Con estos cambios, hemos consolidado la información de precios e inventario en una sola fila de DetailsView.

[![la información de precios e inventario se muestra en una sola fila](using-templatefields-in-the-detailsview-control-cs/_static/image23.png)](using-templatefields-in-the-detailsview-control-cs/_static/image22.png)

**Figura 8**: el precio y la información de inventario se muestran en una sola fila ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Paso 3: personalización de la información del campo no incluida

La columna `Discontinued` de la tabla `Products` es un valor de bit que indica si el producto ya no se ha suspendido. Al enlazar un control DetailsView (o GridView) a un control de origen de datos, los campos de valores booleanos, como `Discontinued`, se implementan como CheckBoxFields, mientras que los campos de valores no booleanos, como `ProductID`, `ProductName`, etc., se implementan como BoundFields. La CheckBoxField se representa como una casilla deshabilitada que se comprueba si el valor del campo de datos es true y no está activada en caso contrario.

En lugar de mostrar el CheckBoxField, es posible que quiera mostrar texto que indique si el producto ya no está disponible. Para lograr esto, podríamos quitar CheckBoxField de DetailsView y, a continuación, agregar un BoundField cuya propiedad `DataField` estaba establecida en `Discontinued`. Dedique un momento a hacerlo. Después de este cambio, DetailsView muestra el texto "true" para los productos descontinuados y "false" para los productos que todavía están activos.

[![las cadenas true y false para mostrar el estado de descontinuado](using-templatefields-in-the-detailsview-control-cs/_static/image26.png)](using-templatefields-in-the-detailsview-control-cs/_static/image25.png)

**Figura 9**: las cadenas true y false se usan para mostrar el estado de descontinuado ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image27.png))

Imagine que no queríamos usar las cadenas "true" o "false", pero "YES" y "NO", en su lugar. Esta personalización se puede realizar con la ayuda de TemplateField y un método de formato. Un método de formato puede tomar cualquier número de parámetros de entrada, pero debe devolver el código HTML (como una cadena) para insertarlo en la plantilla.

Agregue un método de formato a la clase de código subyacente de la página `DetailsViewTemplateField.aspx` denominada `DisplayDiscontinuedAsYESorNO` que acepte un valor booleano como parámetro de entrada y devuelva una cadena. Como se explicó en el tutorial anterior, este método se *debe* marcar como `protected` o `public` para que sea accesible desde la plantilla.

[!code-csharp[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample4.cs)]

Este método comprueba el parámetro de entrada (`discontinued`) y devuelve "YES" si se `true`; de lo contrario, es "NO".

> [!NOTE]
> En el método de formato examinado en el tutorial anterior, recuerde que pasamos un campo de datos que podría contener `NULL` s y, por lo tanto, necesitaba comprobar si el valor de la propiedad de `HiredDate` del empleado tenía una base de datos `NULL` valor antes de tener acceso a la propiedad `HiredDate` del `EmployeesRow`. Esta comprobación no es necesaria aquí, ya que la columna de `Discontinued` nunca puede tener asignados los valores de `NULL` de base de datos. Además, este es el motivo por el que el método puede aceptar un parámetro de entrada booleano en lugar de tener que aceptar una instancia de `ProductsRow` o un parámetro de tipo `object`.

Con este método de formato completo, todo lo que queda es llamarlo desde el `ItemTemplate`de TemplateField. Para crear TemplateField, quite la `Discontinued` BoundField y agregue una nueva TemplateField o convierta el `Discontinued` BoundField en TemplateField. A continuación, en la vista de marcado declarativo, edite TemplateField para que contenga solo una ItemTemplate que invoque el método de `DisplayDiscontinuedAsYESorNO`, pasando el valor de la propiedad `Discontinued` de la instancia de `ProductRow` actual. Se puede tener acceso a esta a través del método `Eval`. En concreto, el marcado de TemplateField debería ser similar al siguiente:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-cs/samples/sample5.aspx)]

Esto hará que se invoque el método `DisplayDiscontinuedAsYESorNO` al representar DetailsView, pasando el valor `Discontinued` de la instancia de `ProductRow`. Dado que el método `Eval` devuelve un valor de tipo `object`, pero el método `DisplayDiscontinuedAsYESorNO` espera un parámetro de entrada de tipo `bool`, se convierte el valor devuelto de los métodos `Eval` en `bool`. El método `DisplayDiscontinuedAsYESorNO` devolverá "YES" o "NO" dependiendo del valor que recibe. El valor devuelto es lo que se muestra en esta fila de DetailsView (vea la figura 10).

[![los valores sí o NO se muestran ahora en la fila descontinuada](using-templatefields-in-the-detailsview-control-cs/_static/image29.png)](using-templatefields-in-the-detailsview-control-cs/_static/image28.png)

**Figura 10**: los valores sí o no se muestran ahora en la fila descontinuada ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-detailsview-control-cs/_static/image30.png))

## <a name="summary"></a>Resumen

TemplateField en el control DetailsView permite un mayor grado de flexibilidad a la hora de mostrar los datos que están disponibles con los demás controles de campo y son ideales para situaciones en las que:

- Los campos de datos múltiples deben mostrarse en una columna de GridView
- Los datos se expresan mejor con un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como Mostrar metadatos o volver a formatear los datos.

Aunque TemplateFields permiten un mayor grado de flexibilidad en la representación de los datos subyacentes de DetailsView, el resultado de DetailsView sigue siendo un poco boxy, ya que cada campo se representa como una fila en un `<table>`HTML.

El control FormView ofrece un mayor grado de flexibilidad a la hora de configurar la salida representada. FormView no contiene campos, sino solo una serie de plantillas (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`, etc.). Veremos cómo usar FormView para lograr un mayor control del diseño representado en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue dan Jagers. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-cs.md)
> [Siguiente](using-the-formview-s-templates-cs.md)
