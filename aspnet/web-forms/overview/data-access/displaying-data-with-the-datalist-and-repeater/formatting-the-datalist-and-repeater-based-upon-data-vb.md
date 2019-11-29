---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Aplicar formato a DataList y Repeater en función de los datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se explican los ejemplos de cómo se da formato a la apariencia de los controles DataList y Repeater, ya sea mediante el uso de funciones de formato con...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: c9b60e4dacd992962942034e84c01cb82e039c81
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636696"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Dar formato a los controles DataList y Repeater en función de los datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) de la aplicación de ejemplo o [descarga de PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> En este tutorial, veremos ejemplos de cómo se da formato a la apariencia de los controles DataList y Repeater, ya sea mediante las funciones de formato dentro de las plantillas o controlando el evento DataBound.

## <a name="introduction"></a>Introducción

Como vimos en el tutorial anterior, DataList ofrece una serie de propiedades relacionadas con el estilo que afectan a su apariencia. En concreto, vimos cómo asignar clases CSS predeterminadas a las propiedades `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`y `SelectedItemStyle` de DataList. Además de estas cuatro propiedades, la lista de elementos incluye varias propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`y `BorderWidth`, por nombrar algunas. El control Repeater no contiene ninguna propiedad relacionada con el estilo. Cualquier configuración de estilo de este tipo debe realizarse directamente en el marcado de las plantillas del repetidor.

Sin embargo, a menudo, el formato de los datos depende de los propios datos. Por ejemplo, al enumerar los productos, es posible que desee mostrar la información del producto en un color de fuente gris claro si se ha dejado de usar, o bien, es posible que desee resaltar el valor `UnitsInStock` si es cero. Como vimos en los tutoriales anteriores, GridView, DetailsView y FormView ofrecen dos maneras distintas de dar formato a su apariencia en función de sus datos:

- **El evento `DataBound`** crea un controlador de eventos para el evento de `DataBound` adecuado, que se activa después de que los datos se hayan enlazado a cada elemento (para el GridView que era el evento `RowDataBound`; para los controles DataList y Repeater, es el evento `ItemDataBound`). En ese controlador de eventos, se pueden examinar los datos que se acaban de enlazar y tomar las decisiones de formato tomadas. Hemos examinado esta técnica en el tutorial de [formato personalizado basado en datos](../custom-formatting/custom-formatting-based-upon-data-vb.md) .
- **Aplicar formato a funciones en plantillas** cuando se usa TemplateFields en los controles DetailsView o GridView, o una plantilla en el control FormView, se puede Agregar una función de formato a la clase de código subyacente de la página ASP.net, la capa de lógica de negocios o cualquier otra biblioteca de clases que sea accesible desde la aplicación Web. Esta función de formato puede aceptar un número arbitrario de parámetros de entrada, pero debe devolver el código HTML que se va a representar en la plantilla. Las funciones de formato se examinaron primero en el tutorial [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) .

Ambas técnicas de formato están disponibles con los controles DataList y Repeater. En este tutorial se recorren los ejemplos de uso de ambas técnicas para ambos controles.

## <a name="using-theitemdataboundevent-handler"></a>Usar el controlador de eventos`ItemDataBound`

Cuando los datos se enlazan a una lista de datos, ya sea desde un control de origen de datos o mediante programación asignando datos a la propiedad control s `DataSource` y llamando a su método de `DataBind()`, se desencadena el evento DataList s `DataBinding`, se enumera el origen de datos y cada registro de datos se enlaza a DataList. Para cada registro del origen de datos, la lista de datos crea un objeto [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) que después se enlaza al registro actual. Durante este proceso, el DataList genera dos eventos:

- **`ItemCreated`** se activa después de que se haya creado el `DataListItem`
- **`ItemDataBound`** se activa después de que el registro actual se ha enlazado al `DataListItem`

En los pasos siguientes se describe el proceso de enlace de datos para el control DataList.

1. El evento DataList s [`DataBinding`](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) se activa
2. Los datos están enlazados a DataList  
  
   Para cada registro del origen de datos 

    1. Crear un objeto `DataListItem`
    2. Desencadenar el [evento`ItemCreated`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Enlazar el registro al `DataListItem`
    4. Desencadenar el [evento`ItemDataBound`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Agregar el `DataListItem` a la colección de `Items`

Al enlazar datos al control Repeater, progresa a través de la misma secuencia de pasos. La única diferencia es que, en lugar de `DataListItem` las instancias que se crean, el repetidor utiliza [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Es posible que el lector astutos haya observado una ligera anomalía entre la secuencia de pasos que transcurrió cuando los controles DataList y Repeater se enlazan a datos en comparación con el control GridView enlazado a datos. Al final del proceso de enlace de datos, GridView genera el evento `DataBound`; sin embargo, ni los controles DataList ni Repeater tienen este tipo de evento. Esto se debe a que los controles DataList y Repeater se crearon en el período de tiempo ASP.NET 1. x, antes de que el patrón de controlador de eventos anterior y posterior se hubiera convertido en común.

Al igual que con GridView, una opción para dar formato en función de los datos consiste en crear un controlador de eventos para el evento `ItemDataBound`. Este controlador de eventos Inspeccione los datos que se acaban de enlazar al `DataListItem` o `RepeaterItem` y que afectan al formato del control según sea necesario.

En el caso del control DataList, el formato de los cambios de todo el elemento se puede implementar mediante las propiedades relacionadas con el estilo `DataListItem` s, entre las que se incluyen las `Font`estándar, `ForeColor`, `BackColor`, `CssClass`, etc. Para influir en el formato de controles Web concretos dentro de la plantilla DataList s, es necesario tener acceso y modificar el estilo de esos controles Web mediante programación. Vimos cómo hacerlo de nuevo en el *formato personalizado basado* en el tutorial de datos. Al igual que el control Repeater, la clase `RepeaterItem` no tiene propiedades relacionadas con el estilo; por lo tanto, todos los cambios relacionados con el estilo realizados en un `RepeaterItem` en el controlador de eventos `ItemDataBound` deben realizarse mediante programación con el acceso y la actualización de los controles web dentro de la plantilla.

Dado que la técnica de formato de `ItemDataBound` para los controles DataList y Repeater son prácticamente idénticas, nuestro ejemplo se centrará en el uso de DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Paso 1: Mostrar información del producto en la lista de datos

Antes de preocuparse por el formato, cree primero una página que use una lista de datos para mostrar la información del producto. En el [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md) , creamos un datalist cuya `ItemTemplate` muestra cada nombre de producto, categoría, proveedor, cantidad por unidad y precio. Vamos a repetir esta funcionalidad aquí en este tutorial. Para ello, puede volver a crear la lista de DataList y su ObjectDataSource desde cero, o bien puede copiar los controles de la página creada en el tutorial anterior (`Basics.aspx`) y pegarlos en la página para este tutorial (`Formatting.aspx`).

Una vez que haya replicado la funcionalidad DataList y ObjectDataSource de `Basics.aspx` en `Formatting.aspx`, dedique un momento a cambiar la propiedad DataList s `ID` de `DataList1` a un `ItemDataBoundFormattingExample`más descriptivo. A continuación, vea la lista de DataList en un explorador. Como se muestra en la figura 1, la única diferencia de formato entre cada producto es que el color de fondo alterna.

[![los productos se muestran en el control DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Figura 1**: los productos se muestran en el control DataList ([haga clic para ver la imagen de tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))

En este tutorial, vamos a dar formato a la lista de elementos de forma que todos los productos con un precio inferior a $20,00 tendrán su nombre y precio unitario resaltados en amarillo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Paso 2: determinar mediante programación el valor de los datos en el controlador de eventos de ItemDataBound

Puesto que solo se aplicará el formato personalizado a los productos con un precio inferior a $20,00, se debe poder determinar cada precio del producto. Al enlazar datos a una lista de datos, la lista de datos enumera los registros de su origen de datos y, para cada registro, crea una instancia de `DataListItem`, enlazando el registro del origen de datos al `DataListItem`. Una vez que los datos de registro se han enlazado al objeto de `DataListItem` actual, se desencadena el evento DataList s `ItemDataBound`. Podemos crear un controlador de eventos para este evento con el fin de inspeccionar los valores de datos del `DataListItem` actual y, según esos valores, realizar los cambios de formato necesarios.

Cree un evento `ItemDataBound` para la lista de elementos y agregue el código siguiente:

[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Aunque el concepto y la semántica que hay detrás del controlador de eventos de `ItemDataBound` de datos son los mismos que los usados por el controlador de eventos de `RowDataBound` de GridView en el tutorial de *formato personalizado basado en datos* , la sintaxis difiere ligeramente. Cuando se activa el evento de `ItemDataBound`, el `DataListItem` simplemente enlazado a los datos se pasa al controlador de eventos correspondiente a través de `e.Item` (en lugar de `e.Row`, como con el controlador de eventos de `RowDataBound` de GridView). El controlador de eventos DataList s `ItemDataBound` se activa para *cada* fila agregada a DataList, incluidas las filas de encabezado, las filas de pie de página y las filas de separadores. Sin embargo, la información del producto solo está enlazada a las filas de datos. Por lo tanto, al usar el evento `ItemDataBound` para inspeccionar los datos enlazados a la lista de datos, es necesario asegurarse primero de que se ha vuelto a trabajar con un elemento de datos. Esto puede realizarse mediante la comprobación de la [propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx)`DataListItem` s`ItemType`, que puede tener uno de [los ocho valores siguientes](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Tanto `Item` como `AlternatingItem``DataListItem` comparan los elementos de datos de DataList. Suponiendo que se ha vuelto a trabajar con una `Item` o `AlternatingItem`, se tiene acceso a la instancia real de `ProductsRow` enlazada a la `DataListItem`actual. La propiedad `DataListItem` s [`DataItem`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contiene una referencia al objeto `DataRowView`, cuya propiedad `Row` proporciona una referencia al objeto `ProductsRow` real.

A continuación, comprobamos la propiedad `ProductsRow` instancia s `UnitPrice`. Dado que el campo Products Table s `UnitPrice` permite valores de `NULL`, antes de intentar tener acceso a la propiedad `UnitPrice`, debemos comprobar primero si tiene un valor de `NULL` mediante el método `IsUnitPriceNull()`. Si el valor `UnitPrice` no es `NULL`, se comprueba si es inferior a $20,00. Si es realmente inferior a $20,00, necesitamos aplicar el formato personalizado.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Paso 3: resaltado del nombre y el precio del producto

Una vez que sabemos que el precio de un producto es inferior a $20,00, todo lo que queda es resaltar su nombre y precio. Para ello, primero debemos hacer referencia mediante programación a los controles Label en el `ItemTemplate` que muestran el nombre y el precio del producto. A continuación, es necesario que muestren un fondo amarillo. Esta información de formato se puede aplicar modificando directamente las etiquetas `BackColor` propiedades (`LabelID.BackColor = Color.Yellow`). Idealmente, sin embargo, todos los aspectos relacionados con la presentación deben expresarse mediante hojas de estilo en cascada. De hecho, ya tenemos una hoja de estilos que proporciona el formato deseado definido en `Styles.css` - `AffordablePriceEmphasis`, que se creó y describe en el tutorial de *formato personalizado basado en datos* .

Para aplicar el formato, basta con establecer los dos controles Web de etiqueta `CssClass` propiedades en `AffordablePriceEmphasis`, tal y como se muestra en el código siguiente:

[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Con el controlador de eventos `ItemDataBound` completado, vuelva a visitar la página `Formatting.aspx` en un explorador. Como se muestra en la figura 2, los productos con un precio inferior a $20,00 tienen el nombre y el precio resaltados.

[![se resaltan los productos inferiores a $20,00](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Figura 2**: los productos inferiores a $20,00 se resaltan ([haga clic para ver la imagen de tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))

> [!NOTE]
> Puesto que DataList se representa como una `<table>`HTML, sus instancias de `DataListItem` tienen propiedades relacionadas con el estilo que se pueden establecer para aplicar un estilo específico a todo el elemento. Por ejemplo, si quisiéramos resaltar *todo* el elemento amarillo cuando su precio fuera inferior a $20,00, podríamos haber reemplazado el código que hacía referencia a las etiquetas y establecer sus propiedades `CssClass` con la siguiente línea de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte la figura 3).

Sin embargo, los `RepeaterItem` que componen el control Repeater no ofrecen propiedades de nivel de estilo. Por lo tanto, aplicar el formato personalizado al repetidor requiere la aplicación de propiedades de estilo a los controles web dentro de las plantillas de Repeater, tal como hicimos en la figura 2.

[![se resalta todo el elemento del producto para los productos en $20,00](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Figura 3**: el elemento de producto completo se resalta para los productos en $20,00 ([haga clic para ver la imagen de tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Usar funciones de formato desde dentro de la plantilla

En el tutorial *uso de TemplateFields en el control GridView* , vimos cómo usar una función de formato en una TemplateField de GridView para aplicar formato personalizado basado en los datos enlazados a las filas de GridView. Una función de formato es un método que se puede invocar desde una plantilla y devuelve el código HTML que se va a emitir en su lugar. Las funciones de formato pueden residir en la clase de código subyacente de la página ASP.NET o se pueden centralizar en archivos de clase en la carpeta `App_Code` o en un proyecto de biblioteca de clases independiente. Mover la función de formato fuera de la clase de código subyacente de la página ASP.NET es ideal si tiene previsto usar la misma función de formato en varias páginas ASP.NET o en otras aplicaciones Web ASP.NET.

Para demostrar las funciones de formato, permita que la información del producto incluya el texto [Discontinued] junto al nombre del producto si ya no se incluye. Además, deje que el precio esté resaltado en amarillo si es menor que $20,00 (como hicimos en el ejemplo del controlador de eventos `ItemDataBound`); Si el precio es $20,00 o superior, deje que se muestre el precio real, pero en su lugar el texto, llame a para obtener una cotización de precios. En la figura 4 se muestra una captura de pantalla de la lista de productos con estas reglas de formato aplicadas.

[![para productos caros, el precio se sustituye por el texto, por favor, llame a para obtener una cotización de precios.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Figura 4**: en el caso de los productos costosos, el precio se sustituye por el texto. llame a para obtener una comilla de precios ([haga clic para ver la imagen a tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))

## <a name="step-1-create-the-formatting-functions"></a>Paso 1: crear las funciones de formato

En este ejemplo, necesitamos dos funciones de formato, una que muestra el nombre del producto junto con el texto [Discontinued], si es necesario, y otro que muestra un precio resaltado si es inferior a $20,00, o el texto, por lo que debe llamar a para obtener una comilla de precios. Vamos a crear estas funciones en la clase de código subyacente de la página ASP.NET y denominarlas `DisplayProductNameAndDiscontinuedStatus` y `DisplayPrice`. Ambos métodos deben devolver el código HTML que se va a representar como una cadena y ambos deben marcarse como `Protected` (o `Public`) para que se pueda invocar desde la parte de la sintaxis declarativa de la página ASP.NET. A continuación se muestra el código de estos dos métodos:

[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Tenga en cuenta que el método `DisplayProductNameAndDiscontinuedStatus` acepta los valores de los campos de datos `productName` y `discontinued` como valores escalares, mientras que el método `DisplayPrice` acepta una instancia de `ProductsRow` (en lugar de un valor escalar `unitPrice`). Cualquier enfoque funcionará; sin embargo, si la función de formato está trabajando con valores escalares que pueden contener valores de `NULL` de la base de datos (como `UnitPrice`; ni `ProductName` ni `Discontinued` permitir valores `NULL`), se debe prestar especial atención a la administración de estas entradas escalares.

En concreto, el parámetro de entrada debe ser de tipo `Object` ya que el valor de entrada puede ser una instancia de `DBNull` en lugar del tipo de datos esperado. Además, se debe realizar una comprobación para determinar si el valor entrante es una base de datos `NULL` valor. Es decir, si queremos que el método `DisplayPrice` acepte el precio como un valor escalar, es necesario usar el código siguiente:

[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Tenga en cuenta que la `unitPrice` parámetro de entrada es de tipo `Object` y que se ha modificado la instrucción condicional para determinar si `unitPrice` está `DBNull` o no. Además, puesto que el `unitPrice` parámetro de entrada se pasa como `Object`, se debe convertir en un valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Paso 2: llamar a la función de formato desde el ItemTemplate de DataList

Una vez que se han agregado las funciones de formato a nuestra clase de código subyacente de la página ASP.NET, todo lo que queda es invocar estas funciones de formato desde el `ItemTemplate`DataList. Para llamar a una función de formato desde una plantilla, coloque la llamada de función dentro de la sintaxis de DataBinding:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

En DataList s `ItemTemplate` el control Web etiqueta de `ProductNameLabel` actualmente muestra el nombre del producto asignando su propiedad `Text` al resultado de `<%# Eval("ProductName") %>`. Para que se muestre el nombre y el texto [Discontinued], si es necesario, actualice la sintaxis declarativa para que, en su lugar, asigne a la propiedad `Text` el valor del método `DisplayProductNameAndDiscontinuedStatus`. Al hacerlo, debemos pasar el nombre del producto y los valores descontinuos con la sintaxis de `Eval("columnName")`. `Eval` devuelve un valor de tipo `Object`, pero el método `DisplayProductNameAndDiscontinuedStatus` espera parámetros de entrada de tipo `String` y `Boolean`; por lo tanto, se deben convertir los valores devueltos por el método `Eval` en los tipos de parámetro de entrada esperados, por ejemplo:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Para mostrar el precio, podemos simplemente establecer la propiedad `UnitPriceLabel` etiqueta s `Text` en el valor devuelto por el método `DisplayPrice`, al igual que hicimos para mostrar el texto nombre del producto y [descontinuado]. Sin embargo, en lugar de pasar el `UnitPrice` como parámetro de entrada escalar, en su lugar pasamos toda la instancia de `ProductsRow`:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Con las llamadas a las funciones de formato en su lugar, tómese un momento para ver el progreso en un explorador. La pantalla debe tener un aspecto similar al de la figura 5, con los productos descontinuos, incluido el texto [Discontinued] y que los productos cuesten más de $20,00 con el precio que se ha sustituido por el texto, llame a para obtener una cotización de precios.

[![para productos caros, el precio se sustituye por el texto, por favor, llame a para obtener una cotización de precios.](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Figura 5**: para los productos costosos, el precio se sustituye por el texto, llame a para obtener una comilla de precios ([haga clic para ver la imagen de tamaño completo](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png)).

## <a name="summary"></a>Resumen

El formato del contenido de un control DataList o Repeater en función de los datos se puede realizar con dos técnicas. La primera técnica consiste en crear un controlador de eventos para el evento `ItemDataBound`, que se activa como cada registro del origen de datos se enlaza a una nueva `DataListItem` o `RepeaterItem`. En el controlador de eventos `ItemDataBound`, se pueden examinar los datos del elemento actual y, a continuación, se puede aplicar formato al contenido de la plantilla o, para `DataListItem` s, a todo el elemento.

Como alternativa, se puede realizar un formato personalizado a través de las funciones de formato. Una función de formato es un método que se puede invocar desde las plantillas DataList o Repeater s que devuelve el código HTML que se va a emitir en su lugar. A menudo, el código HTML devuelto por una función de formato viene determinado por los valores que se enlazan al elemento actual. Estos valores se pueden pasar a la función de formato, ya sea como valores escalares o pasando todo el objeto que se está enlazando al elemento (por ejemplo, la instancia de `ProductsRow`).

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Yaakov Ellis, Randy Schmidt y Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [Siguiente](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
