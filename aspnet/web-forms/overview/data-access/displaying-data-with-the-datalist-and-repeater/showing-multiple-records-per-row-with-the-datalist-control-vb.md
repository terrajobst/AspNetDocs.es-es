---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
title: Mostrar varios registros por fila con el control DataList (VB) | Microsoft Docs
author: rick-anderson
description: En este breve tutorial exploraremos cómo personalizar el diseño de DataList a través de sus propiedades RepeatColumns y RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: f555c531-bf33-4699-9987-42dbfef23c1f
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 17283dae192896fbaa48f1d7fe49afdbaf4c9a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78495307"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-vb"></a>Mostrar varios registros por fila con el control DataList (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_VB.exe) de la aplicación de ejemplo o [descarga de PDF](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/datatutorial31vb1.pdf)

> En este breve tutorial exploraremos cómo personalizar el diseño de DataList a través de sus propiedades RepeatColumns y RepeatDirection.

## <a name="introduction"></a>Introducción

Los ejemplos de DataList que vimos en los dos últimos tutoriales han representado cada registro de su origen de datos como una fila en un `<table>`HTML de una sola columna. Aunque éste es el comportamiento predeterminado de DataList, es muy fácil personalizar la presentación de DataList de modo que los elementos de origen de datos se repartan entre una tabla de varias filas y varias filas. Además, es posible que se muestren todos los elementos de origen de datos en una lista de datos de varias columnas y una sola fila.

Podemos personalizar el diseño de DataList s a través de sus propiedades `RepeatColumns` y `RepeatDirection`, que, respectivamente, indican el número de columnas que se representan y si esos elementos se colocan vertical u horizontalmente. La figura 1, por ejemplo, muestra una lista de datos que muestra información del producto en una tabla con tres columnas.

[![el DataList muestra tres productos por fila](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image1.png)

**Figura 1**: el DataList muestra tres productos por fila ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image3.png))

Al mostrar varios elementos de origen de datos por fila, DataList puede emplear más eficazmente el espacio horizontal de la pantalla. En este breve tutorial Exploraremos estas dos propiedades de DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Paso 1: Mostrar la información del producto en una lista de datos

Antes de examinar las propiedades `RepeatColumns` y `RepeatDirection`, vamos a crear primero una lista de datos en la página que muestra la información del producto mediante el diseño de tabla de varias filas estándar de una sola columna. En este ejemplo, vamos a mostrar el nombre del producto, la categoría y el precio con el siguiente marcado:

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample1.html)]

Hemos aprendido cómo enlazar datos a una lista de datos en ejemplos anteriores, por lo que seguiremos estos pasos rápidamente. Comience abriendo la página `RepeatColumnAndDirection.aspx` en la carpeta `DataListRepeaterBasics` y arrastre una lista de controles desde el cuadro de herramientas hasta el diseñador. En la etiqueta inteligente DataList s, opte por crear un nuevo ObjectDataSource y configurarlo para extraer sus datos del método `ProductsBLL` Class s `GetProducts`, eligiendo la opción (ninguno) en las pestañas insertar, actualizar y eliminar del asistente.

Después de crear y enlazar el nuevo ObjectDataSource a DataList, Visual Studio creará automáticamente una `ItemTemplate` que muestra el nombre y el valor de cada uno de los campos de datos del producto. Ajuste el `ItemTemplate` directamente a través del marcado declarativo o desde la opción editar plantillas de la etiqueta inteligente de DataList s para que use el marcado mostrado anteriormente, reemplazando el *nombre del producto*, el nombre de la *categoría*y el texto del *precio* por los controles de etiqueta que usan la sintaxis de enlace de los enlaces para asignar valores a sus propiedades `Text`. Después de actualizar el `ItemTemplate`, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-vb/samples/sample2.aspx)]

Observe que he incluido un especificador de formato en la `Eval` sintaxis de enlace de los enlaces de la `UnitPrice`, dando formato al valor devuelto como moneda-`Eval("UnitPrice", "{0:C}").`

Dedique un momento a visitar la página en un explorador. Como se muestra en la figura 2, DataList se representa como una tabla de productos de varias filas de una sola columna.

[![de forma predeterminada, DataList se representa como una tabla de una sola columna y varias filas](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image4.png)

**Figura 2**: de forma predeterminada, DataList se representa como una tabla de una sola columna y varias filas ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Paso 2: cambiar la dirección de diseño de DataList

Aunque el comportamiento predeterminado para la lista de objetos es colocar sus elementos verticalmente en una tabla de una sola columna y varias filas, este comportamiento se puede cambiar fácilmente a través de la propiedad DataList s [`RepeatDirection`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). La propiedad `RepeatDirection` puede aceptar uno de dos valores posibles: `Horizontal` o `Vertical` (valor predeterminado).

Al cambiar la propiedad `RepeatDirection` de `Vertical` a `Horizontal`, el objeto DataList representa sus registros en una sola fila y crea una columna por cada elemento de origen de datos. Para ilustrar este efecto, haga clic en la lista de propiedades en el diseñador y, a continuación, en el ventana Propiedades, cambie la propiedad `RepeatDirection` de `Vertical` a `Horizontal`. Inmediatamente después de hacerlo, el diseñador ajusta el diseño de DataList s y crea una interfaz de varias columnas y una sola fila (vea la figura 3).

[![la propiedad RepeatDirection determina cómo se distribuyen los elementos de DataList](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image7.png)

**Figura 3**: la propiedad `RepeatDirection` determina cómo se colocan los elementos de DataList en la lista ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image9.png))

Al mostrar pequeñas cantidades de datos, una tabla de una sola fila y varias columnas puede ser una manera ideal de maximizar el estado real de la pantalla. Sin embargo, en el caso de volúmenes de datos de mayor tamaño, una sola fila requerirá numerosas columnas, lo que empujará a la derecha los elementos que se pueden ajustar a la pantalla. La figura 4 muestra los productos cuando se representan en una lista de filas de una sola fila. Dado que hay muchos productos (más de 80), el usuario tendrá que desplazarse hacia la derecha para ver información acerca de cada uno de los productos.

[![para orígenes de datos suficientemente grandes, una sola columna de lista de columnas requerirá un desplazamiento horizontal](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image10.png)

**Figura 4**: para orígenes de datos suficientemente grandes, una sola columna de lista de columnas requerirá un desplazamiento horizontal ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Paso 3: mostrar los datos en una tabla de varias filas y varias filas

Para crear una lista de columnas de varias filas, es necesario establecer la [propiedad`RepeatColumns`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) en el número de columnas que se van a mostrar. De forma predeterminada, la propiedad `RepeatColumns` está establecida en 0, lo que hará que la lista de objetos muestre todos sus elementos en una sola fila o columna (dependiendo del valor de la propiedad `RepeatDirection`).

En nuestro ejemplo, vamos a mostrar tres productos por fila de tabla. Por lo tanto, establezca la propiedad `RepeatColumns` en 3. Después de realizar este cambio, dedique un momento a ver los resultados en un explorador. Como se muestra en la figura 5, los productos se muestran ahora en una tabla de tres columnas y varias filas.

[![tres productos se muestran por fila](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image13.png)

**Figura 5**: se muestran tres productos por fila ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image15.png))

La propiedad `RepeatDirection` afecta a la disposición de los elementos de la lista de objetos. La figura 5 muestra los resultados con la propiedad `RepeatDirection` establecida en `Horizontal`. Tenga en cuenta que los tres primeros productos Chai, Chang y jarabe de sirope se disponen de izquierda a derecha y de arriba abajo. Los tres productos siguientes (a partir de chef Anton s Cajun Seasoning) aparecen en una fila por debajo de los tres primeros. Sin embargo, al cambiar la propiedad `RepeatDirection` a `Vertical`, se muestran estos productos de arriba abajo, de izquierda a derecha, como se muestra en la figura 6.

[![aquí, los productos se colocan verticalmente](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image16.png)

**Figura 6**: aquí, los productos se colocan verticalmente ([haga clic para ver la imagen de tamaño completo](showing-multiple-records-per-row-with-the-datalist-control-vb/_static/image18.png))

El número de filas mostradas en la tabla resultante depende del número total de registros enlazados a DataList. Precisamente, es el límite superior del número total de elementos de origen de datos dividido por el valor de la propiedad `RepeatColumns`. Dado que la tabla `Products` tiene actualmente 84 productos, que es divisible por 3, hay 28 filas. Si el número de elementos del origen de datos y el valor de la propiedad `RepeatColumns` no son divisibles, la última fila o columna tendrá celdas en blanco. Si el `RepeatDirection` se establece en `Vertical`, la última columna tendrá celdas vacías. Si `RepeatDirection` se `Horizontal`, la última fila tendrá las celdas vacías.

## <a name="summary"></a>Resumen

De forma predeterminada, DataList enumera los elementos de una tabla de una sola columna y varias filas, lo que imita el diseño de un control GridView con una única TemplateField. Aunque este diseño predeterminado es aceptable, podemos maximizar el espacio real de la pantalla mostrando varios elementos de origen de datos por fila. Para ello, basta con establecer la propiedad DataList s `RepeatColumns` en el número de columnas que se mostrarán por fila. Además, la propiedad DataList s `RepeatDirection` se puede usar para indicar si el contenido de la tabla de varias filas y de varias filas debe estar horizontalmente de izquierda a derecha, de arriba a abajo o verticalmente de arriba abajo, de izquierda a derecha.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue John Suru. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
> [Siguiente](nested-data-web-controls-vb.md)
