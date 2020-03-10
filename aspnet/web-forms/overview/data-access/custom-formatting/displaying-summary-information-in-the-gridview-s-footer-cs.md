---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
title: Mostrar información de resumen en el pie de página deC#GridView () | Microsoft Docs
author: rick-anderson
description: La información de resumen se muestra a menudo en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas se puede PR...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d50edc31-9286-4c6a-8635-be09e72752a4
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: b258a2bdeaea8da4e9c5c5d8043b167d94e1e817
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441781"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-c"></a>Mostrar información de resumen en el pie de página de GridView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_15_CS.exe) de la aplicación de ejemplo o [descarga de PDF](displaying-summary-information-in-the-gridview-s-footer-cs/_static/datatutorial15cs1.pdf)

> La información de resumen se muestra a menudo en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas se pueden insertar datos agregados mediante programación. En este tutorial, veremos cómo Mostrar datos agregados en esta fila de pie de página.

## <a name="introduction"></a>Introducción

Además de ver los precios de los productos, las unidades en existencias, las unidades en orden y los niveles de reordenación, un usuario podría estar interesado también en la información agregada, como el precio medio, el número total de unidades en existencias, etc. Esta información de resumen se muestra a menudo en la parte inferior del informe en una fila de resumen. El control GridView puede incluir una fila de pie de página en cuyas celdas se pueden insertar datos agregados mediante programación.

Esta tarea presenta tres desafíos:

1. Configurar GridView para mostrar su fila de pie de página
2. Determinar los datos de Resumen; es decir, ¿cómo se calcula el precio medio o el total de las unidades en existencias?
3. Insertar los datos de resumen en las celdas adecuadas de la fila de pie de página

En este tutorial, veremos cómo superar estos desafíos. En concreto, vamos a crear una página que enumera las categorías de una lista desplegable con los productos de la categoría seleccionada mostrados en un control GridView. GridView incluirá una fila de pie de página que muestra el precio medio y el número total de unidades en stock y en el orden de los productos de esa categoría.

[![se muestra información de resumen en la fila de pie de página de GridView](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: la información de resumen se muestra en la fila de pie de página de GridView ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image3.png))

En este tutorial, con su categoría to Products Master/detail interface, se basa en los conceptos descritos en los [filtros maestro y detalle anteriores con un](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial de DropDownList. Si aún no ha trabajado en el tutorial anterior, hágalo antes de continuar con este.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Paso 1: agregar las categorías DropDownList y Products GridView

Antes de que nos agreguemos información de resumen al pie de página de GridView, vamos a crear simplemente el informe principal/detalle. Una vez completado este primer paso, veremos cómo incluir datos de resumen.

Para empezar, abra la página `SummaryDataInFooter.aspx` de la carpeta `CustomFormatting`. Agregue un control DropDownList y establezca su `ID` en `Categories`. A continuación, haga clic en el vínculo elegir origen de datos de la etiqueta inteligente de DropDownList y opte por agregar un nuevo ObjectDataSource denominado `CategoriesDataSource` que invoca el método `GetCategories()` de la clase `CategoriesBLL`.

[![agregar un nuevo ObjectDataSource denominado CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image4.png)

**Figura 2**: adición de un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image6.png))

[![que ObjectDataSource invoque el método GetCategories () de la clase CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image7.png)

**Figura 3**: hacer que ObjectDataSource invoque el método `GetCategories()` de la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image9.png))

Después de configurar ObjectDataSource, el asistente devuelve el Asistente para configuración de orígenes de datos de DropDownList en el que es necesario especificar qué valor del campo de datos se debe mostrar y cuál debe corresponderse con el valor de los `ListItem` s de DropDownList. Haga que se muestre el campo `CategoryName` y utilice el `CategoryID` como valor.

[![usar los campos CategoryName y CategoryID como texto y valor para ListItems, respectivamente](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image10.png)

**Figura 4**: uso de los campos `CategoryName` y `CategoryID` como `Text` y `Value` para `ListItem` s, respectivamente ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image12.png))

En este momento, tenemos un DropDownList (`Categories`) que enumera las categorías del sistema. Ahora tenemos que agregar un control GridView que muestre los productos que pertenecen a la categoría seleccionada. Sin embargo, antes de hacerlo, active la casilla habilitar AutoPostBack en la etiqueta inteligente de DropDownList. Tal y como se describe en el tutorial de *filtrado de maestro y detalles con DropDownList* , estableciendo la propiedad `AutoPostBack` de dropdownlist en `true` la página se devolverá cada vez que se cambie el valor de DropDownList. Esto hará que se actualice GridView, mostrando los productos de la categoría recién seleccionada. Si la propiedad `AutoPostBack` está establecida en `false` (el valor predeterminado), el cambio de la categoría no producirá un postback y, por lo tanto, no actualizará los productos de la lista.

[![Active la casilla habilitar AutoPostBack en la etiqueta inteligente de DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image13.png)

**Figura 5**: Active la casilla habilitar AutoPostBack en la etiqueta inteligente de DropDownList ([haga clic para ver la imagen a tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image15.png))

Agregue un control GridView a la página para mostrar los productos de la categoría seleccionada. Establezca el `ID` de GridView en `ProductsInCategory` y enlácelo a un nuevo ObjectDataSource denominado `ProductsInCategoryDataSource`.

[![agregar un nuevo ObjectDataSource denominado ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image16.png)

**Ilustración 6**: agregar un nuevo ObjectDataSource denominado `ProductsInCategoryDataSource` ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image18.png))

Configure ObjectDataSource para que invoque el método de `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`.

[![tener ObjectDataSource invocación del método GetProductsByCategoryID (categoryID)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image19.png)

**Figura 7**: hacer que ObjectDataSource invoque el método `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image21.png))

Dado que el método `GetProductsByCategoryID(categoryID)` toma un parámetro de entrada, en el paso final del asistente podemos especificar el origen del valor del parámetro. Para mostrar los productos de la categoría seleccionada, haga que el parámetro se extraiga del `Categories` DropDownList.

[![obtener el valor del parámetro categoryID de las categorías seleccionadas DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image22.png)

**Figura 8**: obtener el valor del parámetro *`categoryID`* de las categorías seleccionadas DropDownList ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image24.png))

Después de completar el asistente, GridView tendrá un BoundField para cada una de las propiedades del producto. Vamos a limpiar estos BoundFields para que solo se muestren los BoundFields `ProductName`, `UnitPrice`, `UnitsInStock`y `UnitsOnOrder`. No dude en agregar cualquier configuración de nivel de campo al resto de BoundFields (por ejemplo, dar formato al `UnitPrice` como moneda). Después de realizar estos cambios, el marcado declarativo de GridView debe ser similar al siguiente:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample1.aspx)]

En este punto, tenemos un informe maestro/detalle totalmente funcional que muestra el nombre, el precio por unidad, las unidades en existencias y las unidades en el orden de los productos que pertenecen a la categoría seleccionada.

[![obtener el valor del parámetro categoryID de las categorías seleccionadas DropDownList](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image25.png)

**Ilustración 9**: obtener el valor del parámetro *`categoryID`* de las categorías seleccionadas DropDownList ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Paso 2: mostrar un pie de página en GridView

El control GridView puede mostrar una fila de encabezado y de pie de página. Estas filas se muestran en función de los valores de las propiedades `ShowHeader` y `ShowFooter`, respectivamente, con `ShowHeader` valor predeterminado `true` y `ShowFooter` para `false`. Para incluir un pie de página en GridView, simplemente establezca su propiedad `ShowFooter` en `true`.

[![establecer la propiedad ShowFooter de GridView en true](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image28.png)

**Figura 10**: establezca la propiedad `ShowFooter` de GridView en `true` ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image30.png))

La fila de pie de página tiene una celda para cada uno de los campos definidos en GridView; sin embargo, estas celdas están vacías de forma predeterminada. Tómese un momento para ver el progreso en un explorador. Con la propiedad `ShowFooter` ahora establecida en `true`, GridView incluye una fila de pie de página vacía.

[![GridView ahora incluye una fila de pie de página](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image31.png)

**Figura 11**: el control GridView ahora incluye una fila de pie de página ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image33.png))

La fila de pie de página de la figura 11 no destaca, ya que tiene un fondo blanco. Vamos a crear una clase `FooterStyle` CSS en `Styles.css` que especifica un fondo rojo oscuro y, a continuación, configurar el archivo de máscara `GridView.skin` en el tema `DataWebControls` para asignar esta clase CSS a la propiedad `FooterStyle`del `CssClass` de GridView. Si necesita realizar un barrido en máscaras y temas, consulte el tutorial de [visualización de datos con el origen de ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) .

Comience agregando la siguiente clase CSS a `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample2.css)]

La clase `FooterStyle` CSS es similar en estilo a la clase `HeaderStyle`, aunque el color de fondo de la `HeaderStyle`es más oscuro y su texto se muestra en negrita. Además, el texto del pie de página está alineado a la derecha, mientras que el texto del encabezado está centrado.

A continuación, para asociar esta clase CSS a cada pie de página de GridView, abra el archivo `GridView.skin` en el tema `DataWebControls` y establezca la propiedad `CssClass` del `FooterStyle`. Después de esta adición, el marcado del archivo debe ser similar al siguiente:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample3.aspx)]

Como se muestra en la siguiente captura de pantalla, este cambio hace que el pie de página se destaque más claramente.

[![la fila de pie de página de GridView tiene ahora un color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image34.png)

**Figura 12**: ahora, la fila de pie de página de GridView tiene un color de fondo rojizo ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Paso 3: calcular los datos de Resumen

Con el pie de página de GridView mostrado, el siguiente desafío que nos enfrenta es cómo calcular los datos de resumen. Hay dos maneras de calcular esta información de agregado:

1. A través de una consulta SQL podríamos emitir una consulta adicional a la base de datos para calcular los datos de resumen para una categoría determinada. SQL incluye una serie de funciones de agregado junto con una cláusula `GROUP BY` para especificar los datos sobre los que se deben resumir los datos. La siguiente consulta SQL devolverá la información necesaria:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample4.sql)]

    Por supuesto, no desearía emitir esta consulta directamente desde la página `SummaryDataInFooter.aspx`, sino mediante la creación de un método en el `ProductsTableAdapter` y el `ProductsBLL`.
2. Calcule esta información a medida que se agrega a GridView como se describe en el tutorial de [formato personalizado basado en datos](custom-formatting-based-upon-data-cs.md) , el controlador de eventos `RowDataBound` de GridView se activa una vez por cada fila que se agrega a GridView después de que se haya enlazado a datos. Al crear un controlador de eventos para este evento, se puede mantener un total acumulado de los valores que queremos agregar. Una vez que la última fila de datos se ha enlazado a GridView, tenemos los totales y la información necesaria para calcular el promedio.

Normalmente se emplea el segundo enfoque, ya que guarda un viaje a la base de datos y el esfuerzo necesario para implementar la funcionalidad de resumen en el nivel de acceso a datos y el nivel de lógica de negocios, pero cualquiera de estos enfoques sería suficiente. En este tutorial, vamos a usar la segunda opción y realizar un seguimiento del total acumulado mediante el controlador de eventos `RowDataBound`.

Cree un controlador de eventos `RowDataBound` para GridView; para ello, seleccione GridView en el diseñador, haga clic en el icono de rayo del ventana Propiedades y haga doble clic en el evento `RowDataBound`. Se creará un nuevo controlador de eventos denominado `ProductsInCategory_RowDataBound` en la clase de código subyacente de la página de `SummaryDataInFooter.aspx`.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample5.cs)]

Con el fin de mantener un total en ejecución, es necesario definir variables fuera del ámbito del controlador de eventos. Cree las cuatro variables de nivel de página siguientes:

- `_totalUnitPrice`, de tipo `decimal`
- `_totalNonNullUnitPriceCount`, de tipo `int`
- `_totalUnitsInStock`, de tipo `int`
- `_totalUnitsOnOrder`, de tipo `int`

A continuación, escriba el código para incrementar estas tres variables para cada fila de datos que se encuentre en el controlador de eventos `RowDataBound`.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample6.cs)]

El controlador de eventos `RowDataBound` se inicia asegurándose de que estamos tratando con una DataRow. Una vez que se ha establecido, la instancia de `Northwind.ProductsRow` que se acaba de enlazar al objeto `GridViewRow` de `e.Row` se almacena en la variable `product`. A continuación, la ejecución de variables totales se incrementa según los valores correspondientes del producto actual (siempre que no contengan una base de datos `NULL` valor). Realizamos un seguimiento de la ejecución de `UnitPrice` total y del número de registros `UnitPrice` no`NULL` porque el precio medio es el cociente de estos dos números.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Paso 4: mostrar los datos de resumen en el pie de página

Con los datos de Resumen totales, el último paso es mostrarlo en la fila de pie de página de GridView. Esta tarea también se puede realizar mediante programación a través del controlador de eventos `RowDataBound`. Recuerde que el controlador de eventos `RowDataBound` se activa para *cada* fila enlazada a GridView, incluida la fila de pie de página. Por lo tanto, podemos aumentar el controlador de eventos para mostrar los datos en la fila de pie de página mediante el código siguiente:

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample7.cs)]

Puesto que la fila de pie de página se agrega a GridView después de que se hayan agregado todas las filas de datos, podemos estar seguros de que en el momento en que estamos listos para mostrar los datos de resumen en el pie de página, se completarán los cálculos totales en ejecución. El último paso, a continuación, es establecer estos valores en las celdas del pie de página.

Para mostrar texto en una celda de pie de página determinada, use `e.Row.Cells[index].Text = value`, donde la indización de `Cells` comienza en 0. En el código siguiente se calcula el precio medio (el precio total dividido por el número de productos) y se muestra junto con el número total de unidades en stock y unidades en el orden en las celdas de pie de página adecuadas de GridView.

[!code-csharp[Main](displaying-summary-information-in-the-gridview-s-footer-cs/samples/sample8.cs)]

La figura 13 muestra el informe después de agregar este código. Observe cómo el `ToString("c")` hace que la información de resumen del precio medio se formatee como una moneda.

[![la fila de pie de página de GridView tiene ahora un color de fondo rojizo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image37.png)

**Figura 13**: ahora, la fila de pie de página de GridView tiene un color de fondo rojizo ([haga clic para ver la imagen de tamaño completo](displaying-summary-information-in-the-gridview-s-footer-cs/_static/image39.png))

## <a name="summary"></a>Resumen

Mostrar los datos de resumen es un requisito de informe común y el control GridView facilita la inclusión de dicha información en la fila de pie de página. La fila de pie de página se muestra cuando la propiedad `ShowFooter` de GridView está establecida en `true` y puede hacer que el texto de las celdas se establezca mediante programación a través del controlador de eventos `RowDataBound`. El cálculo de los datos de resumen puede realizarse volviendo a consultar la base de datos o usando el código de la clase de código subyacente de la página ASP.NET para calcular los datos de Resumen mediante programación.

En este tutorial concluye el examen del formato personalizado con los controles GridView, DetailsView y FormView. En el siguiente tutorial se inicia la exploración de inserción, actualización y eliminación de datos mediante estos mismos controles.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-the-formview-s-templates-cs.md)
> [Siguiente](custom-formatting-based-upon-data-vb.md)
