---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
title: Formato personalizado basado en datos (C#) | Microsoft Docs
author: rick-anderson
description: El ajuste del formato de GridView, DetailsView o FormView en función de los datos enlazados a él puede realizarse de varias maneras. En este tutorial, vamos a l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 871a4574-f89c-4214-b786-79253ed3653b
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d8f3fa337eda0ceed041475ecb52f8b378b9fbba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600559"
---
# <a name="custom-formatting-based-upon-data-c"></a>Formato personalizado basado en datos (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_11_CS.exe) de la aplicación de ejemplo o [descarga de PDF](custom-formatting-based-upon-data-cs/_static/datatutorial11cs1.pdf)

> El ajuste del formato de GridView, DetailsView o FormView en función de los datos enlazados a él puede realizarse de varias maneras. En este tutorial veremos cómo lograr el formato enlazado a datos mediante el uso de los controladores de eventos DataBound y RowDataBound.

## <a name="introduction"></a>Introducción

La apariencia de los controles GridView, DetailsView y FormView se puede personalizar con una gran cantidad de propiedades relacionadas con el estilo. Propiedades como `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`y `Height`, entre otros, dictan la apariencia general del control representado. Las propiedades, como `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`y otras, permiten aplicar la misma configuración de estilo a secciones concretas. Del mismo modo, esta configuración de estilo se puede aplicar en el nivel de campo.

Sin embargo, en muchos escenarios, los requisitos de formato dependen del valor de los datos mostrados. Por ejemplo, para atraer la atención a los productos fuera de las existencias, un informe en el que se muestre información de productos podría establecer el color de fondo en amarillo para los productos cuyos campos `UnitsInStock` y `UnitsOnOrder` sean iguales a 0. Para resaltar los productos más caros, es posible que desee mostrar los precios de los productos con un costo superior a $75,00 en una fuente en negrita.

El ajuste del formato de GridView, DetailsView o FormView en función de los datos enlazados a él puede realizarse de varias maneras. En este tutorial veremos cómo lograr el formato enlazado a datos mediante el uso de los controladores de eventos `DataBound` y `RowDataBound`. En el siguiente tutorial exploraremos un enfoque alternativo.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Usar el controlador de eventos`DataBound`del control DetailsView

Cuando los datos se enlazan a DetailsView, ya sea desde un control de origen de datos o mediante programación asignando datos a la propiedad `DataSource` del control y llamando a su método `DataBind()`, se produce la siguiente secuencia de pasos:

1. Se desencadena el evento `DataBinding` del control Web de datos.
2. Los datos se enlazan al control Web de datos.
3. Se desencadena el evento `DataBound` del control Web de datos.

La lógica personalizada se puede insertar inmediatamente después de los pasos 1 y 3 a través de un controlador de eventos. Mediante la creación de un controlador de eventos para el evento de `DataBound`, se pueden determinar mediante programación los datos que se han enlazado al control Web de datos y ajustar el formato según sea necesario. Para ilustrar esto, vamos a crear un DetailsView que enumerará información general acerca de un producto, pero mostrará el valor `UnitPrice` en una ***fuente de negrita, cursiva*** si supera $75,00.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Paso 1: Mostrar la información del producto en DetailsView

Abra la página `CustomColors.aspx` de la carpeta `CustomFormatting`, arrastre un control DetailsView desde el cuadro de herramientas hasta el diseñador, establezca su valor de la propiedad `ID` en `ExpensiveProductsPriceInBoldItalic`y enlácelo a un nuevo control ObjectDataSource que invoca el método `ProductsBLL` de la clase `GetProducts()`. Los pasos detallados para llevar esto a cabo se omiten aquí por motivos de brevedad, ya que se han examinado en detalle en los tutoriales anteriores.

Una vez que haya enlazado ObjectDataSource a DetailsView, dedique un momento a modificar la lista de campos. He optado por quitar las `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`y `Discontinued` BoundFields y cambiado el nombre y el formato del BoundFields restante. También borró la configuración de `Width` y `Height`. Puesto que DetailsView muestra un solo registro, es necesario habilitar la paginación para permitir que el usuario final pueda ver todos los productos. Para ello, active la casilla habilitar paginación en la etiqueta inteligente de DetailsView.

[![Active la casilla habilitar paginación en la etiqueta inteligente de DetailsView](custom-formatting-based-upon-data-cs/_static/image2.png)](custom-formatting-based-upon-data-cs/_static/image1.png)

**Figura 1**: Active la casilla habilitar paginación en la etiqueta inteligente de DetailsView ([haga clic para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image3.png))

Después de estos cambios, el marcado de DetailsView será:

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample1.aspx)]

Dedique un momento a probar esta página en el explorador.

[![el control DetailsView muestra un producto a la vez](custom-formatting-based-upon-data-cs/_static/image5.png)](custom-formatting-based-upon-data-cs/_static/image4.png)

**Figura 2**: el control DetailsView muestra un producto a la vez ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 2: determinar mediante programación el valor de los datos en el controlador de eventos DataBound

Para mostrar el precio en una fuente en negrita, cursiva para los productos cuyo valor `UnitPrice` supera $75,00, es necesario poder determinar primero mediante programación el valor de `UnitPrice`. En DetailsView, esto se puede realizar en el controlador de eventos `DataBound`. Para crear el controlador de eventos, haga clic en DetailsView en el diseñador y, a continuación, navegue hasta el ventana Propiedades. Presione F4 para mostrarla, si no está visible, o vaya al menú Ver y seleccione la opción de menú ventana de propiedades. En el ventana Propiedades, haga clic en el icono de rayo para mostrar los eventos de DetailsView. A continuación, haga doble clic en el evento `DataBound` o escriba el nombre del controlador de eventos que desea crear.

![Crear un controlador de eventos para el evento DataBound](custom-formatting-based-upon-data-cs/_static/image7.png)

**Figura 3**: creación de un controlador de eventos para el evento `DataBound`

Al hacerlo, se creará automáticamente el controlador de eventos y se le llevará a la parte del código donde se ha agregado. En este momento, verá:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample2.cs)]

Se puede tener acceso a los datos enlazados a DetailsView a través de la propiedad `DataItem`. Recuerde que estamos enlazando nuestros controles a un objeto DataTable fuertemente tipado, que se compone de una colección de instancias de DataRow fuertemente tipadas. Cuando DataTable se enlaza a DetailsView, el primer DataRow de la DataTable se asigna a la propiedad `DataItem` de DetailsView. En concreto, se asigna a la propiedad `DataItem` un objeto `DataRowView`. Podemos utilizar la propiedad `Row` del `DataRowView`para obtener acceso al objeto DataRow subyacente, que es realmente una instancia de `ProductsRow`. Una vez que tenemos esta `ProductsRow` instancia, podemos tomar nuestra decisión simplemente inspeccionando los valores de propiedad del objeto.

En el código siguiente se muestra cómo determinar si el valor de `UnitPrice` enlazado al control DetailsView es mayor que $75,00:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample3.cs)]

> [!NOTE]
> Como `UnitPrice` puede tener un valor de `NULL` en la base de datos, primero debemos asegurarnos de que no estamos tratando con un valor de `NULL` antes de tener acceso a la propiedad `UnitPrice` del `ProductsRow`. Esta comprobación es importante porque si se intenta tener acceso a la propiedad `UnitPrice` cuando tiene un valor `NULL`, el objeto `ProductsRow` producirá una [excepción StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Paso 3: dar formato al valor UnitPrice en DetailsView

En este momento, podemos determinar si el valor de `UnitPrice` enlazado a DetailsView tiene un valor que supera $75,00, pero todavía hemos visto cómo ajustar mediante programación el formato de DetailsView en consecuencia. Para modificar el formato de una fila completa en DetailsView, puede obtener acceso mediante programación a la fila mediante `DetailsViewID.Rows[index]`; para modificar una celda determinada, use el `DetailsViewID.Rows[index].Cells[index]`de acceso. Una vez que tenemos una referencia a la fila o celda, podemos ajustar su apariencia estableciendo sus propiedades relacionadas con el estilo.

Para obtener acceso a una fila mediante programación, es necesario conocer el índice de la fila, que comienza en 0. La fila `UnitPrice` es la quinta fila de DetailsView, lo que le asigna un índice de 4 y permite el acceso mediante programación mediante `ExpensiveProductsPriceInBoldItalic.Rows[4]`. En este momento, podríamos hacer que el contenido de la fila completa se muestre en una fuente en negrita y cursiva mediante el código siguiente:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample4.cs)]

Sin embargo, esto hará que la etiqueta (precio) y el *valor estén en* negrita y cursiva. Si queremos poner el valor en negrita y en cursiva, necesitamos aplicar este formato a la segunda celda de la fila, lo que se puede lograr mediante el siguiente procedimiento:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample5.cs)]

Dado que nuestros tutoriales han usado las hojas de estilos para mantener una separación limpia entre el marcado representado y la información relacionada con el estilo, en lugar de establecer las propiedades de estilo específicas como se muestra anteriormente, en su lugar, se usa una clase CSS. Abra la hoja de estilos `Styles.css` y agregue una nueva clase CSS denominada `ExpensivePriceEmphasis` con la siguiente definición:

[!code-css[Main](custom-formatting-based-upon-data-cs/samples/sample6.css)]

A continuación, en el controlador de eventos `DataBound`, establezca la propiedad `CssClass` de la celda en `ExpensivePriceEmphasis`. En el código siguiente se muestra el controlador de eventos `DataBound` en su totalidad:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample7.cs)]

Al ver Chai, que cuesta menos de $75,00, el precio se muestra en una fuente normal (consulte la figura 4). Sin embargo, al ver MISHI Kobe Niku, que tiene un precio de $97,00, el precio se muestra en negrita, fuente en cursiva (véase la figura 5).

[![precios inferiores a $75,00 se muestran en una fuente normal](custom-formatting-based-upon-data-cs/_static/image9.png)](custom-formatting-based-upon-data-cs/_static/image8.png)

**Figura 4**: los precios inferiores a $75,00 se muestran en una fuente normal ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image10.png))

[![precios de productos caros se muestran en negrita, fuente en cursiva](custom-formatting-based-upon-data-cs/_static/image12.png)](custom-formatting-based-upon-data-cs/_static/image11.png)

**Figura 5**: los precios de los productos caros se muestran en una fuente en negrita y cursiva ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Usar el controlador de eventos`DataBound`del control FormView

Los pasos para determinar los datos subyacentes enlazados a un objeto FormView son idénticos a los de una interfaz DetailsView Create a `DataBound` controlador de eventos, convierten la propiedad `DataItem` en el tipo de objeto adecuado enlazado al control y determinan cómo proceder. FormView y DetailsView difieren, sin embargo, en cómo se actualiza la apariencia de la interfaz de usuario.

FormView no contiene ningún BoundFields y, por tanto, carece de la colección `Rows`. En su lugar, FormView se compone de plantillas, que pueden contener una combinación de HTML estático, controles Web y sintaxis de enlace de los enlaces. El ajuste del estilo de un FormView normalmente implica el ajuste del estilo de uno o varios controles web dentro de las plantillas de FormView.

Para ilustrar esto, vamos a usar FormView para enumerar productos como en el ejemplo anterior, pero esta vez vamos a mostrar solo el nombre del producto y las unidades en existencias con las unidades en existencias que se muestran en una fuente roja si es menor o igual que 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Paso 4: Mostrar la información del producto en FormView

Agregue un FormView a la página `CustomColors.aspx` bajo DetailsView y establezca su propiedad `ID` en `LowStockedProductsInRed`. Enlace el FormView al control ObjectDataSource creado en el paso anterior. Esto creará un `ItemTemplate`, `EditItemTemplate`y `InsertItemTemplate` para FormView. Quite los `EditItemTemplate` y `InsertItemTemplate` y simplifique la `ItemTemplate` para incluir solo los valores `ProductName` y `UnitsInStock`, cada uno en sus propios controles de etiqueta con el nombre adecuado. Al igual que con DetailsView en el ejemplo anterior, Active también la casilla habilitar paginación en la etiqueta inteligente de FormView.

Después de estos cambios, el marcado de FormView debe ser similar al siguiente:

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample8.aspx)]

Tenga en cuenta que la `ItemTemplate` contiene:

- **HTML estático** el texto "Product:" y "Units in stock:" junto con los elementos `<br />` y `<b>`.
- **Controles Web** los dos controles etiqueta, `ProductNameLabel` y `UnitsInStockLabel`.
- **Sintaxis de DataBinding** la sintaxis de `<%# Bind("ProductName") %>` y `<%# Bind("UnitsInStock") %>`, que asigna los valores de estos campos a las propiedades de `Text` de los controles de etiqueta.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Paso 5: determinar mediante programación el valor de los datos en el controlador de eventos DataBound

Con el marcado de FormView completado, el paso siguiente consiste en determinar mediante programación si el valor de `UnitsInStock` es menor o igual que 10. Esto se logra de la misma manera con el FormView que con DetailsView. Empiece por crear un controlador de eventos para el evento `DataBound` de FormView.

![Crear el controlador de eventos DataBound](custom-formatting-based-upon-data-cs/_static/image14.png)

**Figura 6**: creación del controlador de eventos `DataBound`

En el controlador de eventos, convierta la propiedad `DataItem` de FormView en una instancia de `ProductsRow` y determine si el valor de `UnitsInPrice` es tal que necesite mostrarlo en una fuente de color rojo.

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample9.cs)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Paso 6: dar formato al control de etiqueta UnitsInStockLabel en ItemTemplate de FormView

El paso final consiste en dar formato al valor de `UnitsInStock` mostrado en una fuente roja si el valor es 10 o menos. Para lograr esto, es necesario tener acceso mediante programación al control `UnitsInStockLabel` en el `ItemTemplate` y establecer sus propiedades de estilo para que el texto se muestre en rojo. Para tener acceso a un control Web en una plantilla, use el método `FindControl("controlID")` como el siguiente:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample10.cs)]

En nuestro ejemplo, queremos acceder a un control de etiqueta cuyo valor `ID` sea `UnitsInStockLabel`, por lo que usaremos:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample11.cs)]

Una vez que tenemos una referencia mediante programación al control Web, podemos modificar sus propiedades relacionadas con el estilo según sea necesario. Como en el ejemplo anterior, he creado una clase CSS en `Styles.css` denominada `LowUnitsInStockEmphasis`. Para aplicar este estilo al control Web Label, establezca su propiedad `CssClass` en consecuencia.

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample12.cs)]

> [!NOTE]
> La sintaxis para dar formato a una plantilla que tiene acceso mediante programación al control Web mediante `FindControl("controlID")` y, a continuación, establecer sus propiedades relacionadas con el estilo también se puede usar al usar [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) en los controles DetailsView o GridView. Examinaremos TemplateFields en el siguiente tutorial.

En las figuras 7 se muestra el FormView al ver un producto cuyo valor de `UnitsInStock` es mayor que 10, mientras que el producto de la figura 8 tiene un valor inferior a 10.

[![para productos con unidades suficientemente grandes en existencias, no se aplica ningún formato personalizado](custom-formatting-based-upon-data-cs/_static/image16.png)](custom-formatting-based-upon-data-cs/_static/image15.png)

**Figura 7**: para los productos con unidades suficientemente grandes en existencias, no se aplica ningún formato personalizado ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image17.png))

[![las unidades del número de existencias se muestran en rojo para los productos con valores de 10 o menos.](custom-formatting-based-upon-data-cs/_static/image19.png)](custom-formatting-based-upon-data-cs/_static/image18.png)

**Figura 8**: las unidades de stock Number se muestran en rojo para los productos con valores de 10 o menos ([haga clic para ver la imagen a tamaño completo](custom-formatting-based-upon-data-cs/_static/image20.png))

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Aplicar formato al evento`RowDataBound`de GridView

Anteriormente examinamos la secuencia de pasos que los controles DetailsView y FormView progresan durante el enlace de los enlaces. Echemos un vistazo de nuevo a estos pasos como un actualizador.

1. Se desencadena el evento `DataBinding` del control Web de datos.
2. Los datos se enlazan al control Web de datos.
3. Se desencadena el evento `DataBound` del control Web de datos.

Estos tres sencillos pasos son suficientes para DetailsView y FormView porque solo muestran un único registro. Para GridView, que muestra *todos* los registros enlazados a él (no solo el primero), el paso 2 es un poco más complicado.

En el paso 2, GridView enumera el origen de datos y, para cada registro, crea una instancia de `GridViewRow` y enlaza el registro actual a ella. Para cada `GridViewRow` agrega a GridView, se generan dos eventos:

- **`RowCreated`** se activa después de que se haya creado el `GridViewRow`
- **`RowDataBound`** se activa después de que el registro actual se ha enlazado al `GridViewRow`.

En el caso de GridView, el enlace de datos se describe con más precisión mediante la siguiente secuencia de pasos:

1. Se desencadena el evento `DataBinding` de GridView.
2. Los datos están enlazados a GridView.   
  
   Para cada registro del origen de datos 

    1. Crear un objeto `GridViewRow`
    2. Desencadenar el evento `RowCreated`
    3. Enlazar el registro al `GridViewRow`
    4. Desencadenar el evento `RowDataBound`
    5. Agregar el `GridViewRow` a la colección de `Rows`
3. Se desencadena el evento `DataBound` de GridView.

Para personalizar el formato de los registros individuales de GridView, es necesario crear un controlador de eventos para el evento `RowDataBound`. Para ilustrar esto, vamos a agregar un control GridView a la página `CustomColors.aspx` que muestra el nombre, la categoría y el precio de cada producto, resaltando los productos cuyo precio es inferior a $10,00 con un color de fondo amarillo.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Paso 7: Mostrar información del producto en un control GridView

Agregue un control GridView debajo del FormView en el ejemplo anterior y establezca su propiedad `ID` en `HighlightCheapProducts`. Dado que ya tenemos un ObjectDataSource que devuelve todos los productos de la página, enlace GridView a ese. Por último, edite el BoundFields de GridView para incluir solo los nombres, las categorías y los precios de los productos. Después de estos cambios, el marcado de GridView debe ser similar a:

[!code-aspx[Main](custom-formatting-based-upon-data-cs/samples/sample13.aspx)]

En la ilustración 9 se muestra nuestro progreso hasta este punto cuando se ve a través de un explorador.

[![GridView muestra el nombre, la categoría y el precio de cada producto](custom-formatting-based-upon-data-cs/_static/image22.png)](custom-formatting-based-upon-data-cs/_static/image21.png)

**Figura 9**: el control GridView muestra el nombre, la categoría y el precio de cada producto ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Paso 8: determinar mediante programación el valor de los datos en el controlador de eventos RowDataBound

Cuando el `ProductsDataTable` se enlaza a GridView, se enumeran sus instancias `ProductsRow` y para cada `ProductsRow` se crea un `GridViewRow`. La propiedad `DataItem` del `GridViewRow`se asigna al `ProductRow`determinado, después del cual se genera el controlador de eventos `RowDataBound` de GridView. Para determinar el valor `UnitPrice` de cada producto enlazado a GridView, es necesario crear un controlador de eventos para el evento `RowDataBound` de GridView. En este controlador de eventos, podemos inspeccionar el valor de `UnitPrice` de la `GridViewRow` actual y tomar una decisión de formato para esa fila.

Este controlador de eventos se puede crear con la misma serie de pasos que con FormView y DetailsView.

![Crear un controlador de eventos para el evento RowDataBound de GridView](custom-formatting-based-upon-data-cs/_static/image24.png)

**Figura 10**: creación de un controlador de eventos para el evento `RowDataBound` de GridView

Al crear el controlador de eventos de esta manera, el código siguiente se agregará automáticamente a la parte del código de la página ASP.NET:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample14.cs)]

Cuando se desencadena el evento `RowDataBound`, el controlador de eventos se pasa como su segundo parámetro a un objeto de tipo `GridViewRowEventArgs`, que tiene una propiedad denominada `Row`. Esta propiedad devuelve una referencia al `GridViewRow` que solo estaba enlazado a datos. Para tener acceso a la instancia de `ProductsRow` enlazada a la `GridViewRow` usamos la propiedad `DataItem` como la siguiente:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample15.cs)]

Al trabajar con el controlador de eventos `RowDataBound` es importante tener en cuenta que GridView se compone de distintos tipos de filas y que este evento se desencadena para *todos los* tipos de fila. El tipo de `GridViewRow`puede determinarse mediante su propiedad `RowType` y puede tener uno de los valores posibles:

- `DataRow` una fila enlazada a un registro de la `DataSource` de GridView
- `EmptyDataRow` la fila mostrada si el `DataSource` de GridView está vacío
- `Footer` la fila de pie de página; se muestra si la propiedad `ShowFooter` de GridView está establecida en `true`
- `Header` la fila de encabezado; se muestra si la propiedad ShowHeader de GridView está establecida en `true` (el valor predeterminado)
- `Pager` para GridView que implementan la paginación, la fila que muestra la interfaz de paginación
- `Separator` no se usa para GridView, pero lo usan las propiedades `RowType` de los controles DataList y Repeater, dos controles Web de datos se tratarán en futuros tutoriales.

Puesto que las filas `EmptyDataRow`, `Header`, `Footer`y `Pager` no están asociadas a un registro de `DataSource`, siempre tendrán un valor `null` para la propiedad `DataItem`. Por este motivo, antes de intentar trabajar con la propiedad `DataItem` del `GridViewRow`actual, primero debemos asegurarnos de que estamos tratando con un `DataRow`. Esto puede realizarse mediante la comprobación de la propiedad de `RowType` del `GridViewRow`de la manera siguiente:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample16.cs)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Paso 9: resaltado de la fila amarillo cuando el valor de UnitPrice es menor que $10,00

El último paso es resaltar el `GridViewRow` completo mediante programación si el valor de `UnitPrice` para esa fila es menor que $10,00. La sintaxis para tener acceso a las filas o celdas de GridView es igual que con la `GridViewID.Rows[index]` DetailsView para tener acceso a toda la fila, `GridViewID.Rows[index].Cells[index]` para tener acceso a una celda determinada. Sin embargo, cuando el controlador de eventos `RowDataBound` activa el `GridViewRow` enlazado a datos, todavía se debe agregar a la colección de `Rows` de GridView. Por lo tanto, no se puede tener acceso a la instancia de `GridViewRow` actual desde el controlador de eventos `RowDataBound` mediante la colección Rows.

En lugar de `GridViewID.Rows[index]`, podemos hacer referencia a la instancia de `GridViewRow` actual en el controlador de eventos de `RowDataBound` con `e.Row`. Es decir, para resaltar la instancia actual de `GridViewRow` de la `RowDataBound` controlador de eventos usaremos:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample17.cs)]

En lugar de establecer la propiedad `BackColor` del `GridViewRow`directamente, vamos a usar clases CSS. He creado una clase CSS denominada `AffordablePriceEmphasis` que establece el color de fondo en amarillo. A continuación se muestra el controlador de eventos `RowDataBound` completado:

[!code-csharp[Main](custom-formatting-based-upon-data-cs/samples/sample18.cs)]

[![los productos más asequibles se resaltan en amarillo](custom-formatting-based-upon-data-cs/_static/image26.png)](custom-formatting-based-upon-data-cs/_static/image25.png)

**Figura 11**: los productos más asequibles se resaltan en amarillo ([haga clic para ver la imagen de tamaño completo](custom-formatting-based-upon-data-cs/_static/image27.png))

## <a name="summary"></a>Resumen

En este tutorial vimos cómo dar formato a GridView, DetailsView y FormView en función de los datos enlazados al control. Para ello, creamos un controlador de eventos para los eventos `DataBound` o `RowDataBound`, donde se examinaron los datos subyacentes junto con un cambio de formato, si es necesario. Para tener acceso a los datos enlazados a DetailsView o FormView, usamos la propiedad `DataItem` en el controlador de eventos `DataBound`. en el caso de un control GridView, cada propiedad `DataItem` de una instancia de `GridViewRow` contiene los datos enlazados a esa fila, que está disponible en el controlador de eventos `RowDataBound`.

La sintaxis para ajustar mediante programación el formato del control Web de datos depende del control Web y de cómo se muestran los datos a los que se va a dar formato. En el caso de los controles DetailsView y GridView, se puede tener acceso a las filas y celdas mediante un índice ordinal. Para el FormView, que utiliza plantillas, el método `FindControl("controlID")` se usa normalmente para buscar un control Web desde dentro de la plantilla.

En el siguiente tutorial veremos cómo usar plantillas con GridView y DetailsView. Además, veremos otra técnica para personalizar el formato basado en los datos subyacentes.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores de clientes potenciales de este tutorial se E.R. Gilmore, Dennis Patterson y dan Jagers. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](using-templatefields-in-the-gridview-control-cs.md)
