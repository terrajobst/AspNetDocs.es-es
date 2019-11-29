---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
title: Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón seleccionar. Hacer clic en el botón seleccionar de un particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 1d1a7c93-971d-4690-9c5e-dac0e5014a09
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb
msc.type: authoredcontent
ms.openlocfilehash: a953c00acc4c37fd563321477b6b21689d6e686c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74576440"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-vb"></a>Maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_10_VB.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/datatutorial10vb1.pdf)

> Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón seleccionar. Al hacer clic en el botón seleccionar de un producto determinado, se mostrarán los detalles completos en un control DetailsView de la misma página.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-across-two-pages-vb.md) , vimos cómo crear un informe maestro y detalles con dos páginas web: una página web "maestra", desde la que se muestra la lista de proveedores. y una página web de "detalles" en la que se enumeran los productos proporcionados por el proveedor seleccionado. Este formato de informe de dos páginas se puede condensar en una sola página. Este tutorial tendrá un control GridView cuyas filas incluyen el nombre y el precio de cada producto junto con un botón seleccionar. Al hacer clic en el botón seleccionar de un producto determinado, se mostrarán los detalles completos en un control DetailsView de la misma página.

[![al hacer clic en el botón seleccionar se muestran los detalles del producto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image1.png)

**Figura 1**: al hacer clic en el botón seleccionar se muestran los detalles del producto ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Paso 1: crear un control GridView seleccionable

Recuerde que en el informe maestro y detalles de dos páginas, cada registro maestro incluye un hipervínculo que, cuando se hace clic en él, envía al usuario a la página de detalles que pasa el valor de `SupplierID` de la fila en la que se hizo clic en QueryString. Este tipo de hipervínculo se ha agregado a cada fila de GridView mediante un HyperLinkField. Para el informe de maestro y detalles de una sola página, se necesitará un botón para cada fila de GridView que, al hacer clic en él, muestra los detalles. El control GridView puede configurarse para incluir un botón Seleccionar para cada fila que produce un postback y marca esa fila como [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)de GridView.

Empiece agregando un control GridView a la página `DetailsBySelecting.aspx` de la carpeta `Filtering`, estableciendo su propiedad `ID` en `ProductsGrid`. A continuación, agregue un nuevo ObjectDataSource denominado `AllProductsDataSource` que invoque el método `GetProducts()` de la clase `ProductsBLL`.

[![crear un ObjectDataSource denominado AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image4.png)

**Figura 2**: creación de un ObjectDataSource llamado `AllProductsDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image6.png))

[![usar la clase ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image7.png)

**Figura 3**: uso de la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image9.png))

[![configurar ObjectDataSource para invocar el método GetProducts ()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image10.png)

**Figura 4**: configuración de ObjectDataSource para invocar el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image12.png))

Edite los campos de GridView quitando todos los `ProductName` y `UnitPrice` BoundFields. Además, puede personalizar estos BoundFields según sea necesario, como aplicar formato a la `UnitPrice` BoundField como moneda y cambiar las propiedades de `HeaderText` de BoundFields. Estos pasos se pueden realizar de forma gráfica; para ello, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView o configure manualmente la sintaxis declarativa.

[![quitar todos los, excepto el NombreProducto y UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image13.png)

**Figura 5**: Quite todos los `ProductName` y `UnitPrice` BoundFields ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image15.png))

El marcado final para GridView es:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample1.aspx)]

A continuación, es necesario marcar GridView como seleccionable, lo que agregará un botón seleccionar a cada fila. Para ello, solo tiene que activar la casilla habilitar selección en la etiqueta inteligente de GridView.

[![hacer que las filas de GridView sean seleccionables](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image16.png)

**Figura 6**: hacer que las filas de GridView se seleccionen ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image18.png))

Al activar la opción Habilitar selección, se agrega un CommandField al `ProductsGrid` GridView con su propiedad `ShowSelectButton` establecida en true. Esto da como resultado un botón Seleccionar para cada fila de GridView, tal como se muestra en la figura 6. De forma predeterminada, los botones de selección se representan como LinkButtons, pero puede usar los botones o ImageButtons en su lugar a través de la propiedad `ButtonType` de la CommandField.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample2.aspx)]

Cuando se hace clic en el botón seleccionar de una fila de GridView, el PostBack se encuentra y se actualiza la propiedad `SelectedRow` de GridView. Además de la propiedad `SelectedRow`, GridView proporciona las propiedades [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)y [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) . La propiedad `SelectedIndex` devuelve el índice de la fila seleccionada, mientras que las propiedades `SelectedValue` y `SelectedDataKey` devuelven valores basados en la [propiedad DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)de GridView.

La propiedad `DataKeyNames` se utiliza para asociar uno o más valores de campos de datos a cada fila y se utiliza normalmente para atribuir información de identificación única de los datos subyacentes con cada fila de GridView. La propiedad `SelectedValue` devuelve el valor del primer campo de datos `DataKeyNames` de la fila seleccionada, donde la propiedad `SelectedDataKey` devuelve el objeto `DataKey` de la fila seleccionada, que contiene todos los valores de los campos de clave de datos especificados para esa fila.

La propiedad `DataKeyNames` se establece automáticamente en los campos de datos que identifican de forma única al enlazar un origen de datos a GridView, DetailsView o FormView a través del diseñador. Aunque esta propiedad se ha establecido para nosotros automáticamente en los tutoriales anteriores, los ejemplos habrían funcionado sin la propiedad `DataKeyNames` especificada. Sin embargo, para el GridView seleccionable en este tutorial, así como para futuros tutoriales en los que se va a examinar la inserción, actualización y eliminación, la propiedad `DataKeyNames` debe estar establecida correctamente. Dedique un momento a asegurarse de que la propiedad `DataKeyNames` de GridView está establecida en `ProductID`.

Veamos nuestro progreso hasta el momento a través de un explorador. Tenga en cuenta que GridView muestra el nombre y el precio de todos los productos junto con un control LinkButton Select. Al hacer clic en el botón seleccionar se produce un postback. En el paso 2 veremos cómo hacer que DetailsView responda a este postback mostrando los detalles del producto seleccionado.

[![cada fila de producto contiene una selección de LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image19.png)

**Figura 7**: cada fila de producto contiene una selección de LinkButton ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image21.png))

## <a name="highlighting-the-selected-row"></a>Resaltado de la fila seleccionada

El `ProductsGrid` GridView tiene una propiedad `SelectedRowStyle` que se puede utilizar para dictar el estilo visual de la fila seleccionada. Usado correctamente, esto puede mejorar la experiencia del usuario al mostrar más claramente qué fila de GridView está seleccionada actualmente. En este tutorial, vamos a resaltar la fila seleccionada con un fondo amarillo.

Al igual que con nuestros tutoriales anteriores, vamos a esforzarnos por mantener la configuración relacionada con la estética definida como clases CSS. Por lo tanto, cree una nueva clase CSS en `Styles.css` denominada `SelectedRowStyle`.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample3.css)]

Para aplicar esta clase CSS a la propiedad `SelectedRowStyle` de *todos los* GridView de la serie de tutoriales, edite la máscara de `GridView.skin` en el tema `DataWebControls` para incluir la configuración de `SelectedRowStyle` como se muestra a continuación:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/samples/sample4.aspx)]

Con esta adición, la fila de GridView seleccionada se resalta con un color de fondo amarillo.

[![personalizar la apariencia de la fila seleccionada mediante la propiedad SelectedRowStyle de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image22.png)

**Figura 8**: personalización de la apariencia de la fila seleccionada mediante la propiedad `SelectedRowStyle` de GridView ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Paso 2: mostrar los detalles del producto seleccionado en DetailsView

Con el `ProductsGrid` GridView completo, lo único que queda es agregar un control DetailsView que muestre información sobre el producto determinado seleccionado. Agregue un control DetailsView sobre GridView y cree un nuevo ObjectDataSource denominado `ProductDetailsDataSource`. Como queremos que este DetailsView muestre información específica sobre el producto seleccionado, configure el `ProductDetailsDataSource` para usar el método `GetProductByProductID(productID)` de la clase `ProductsBLL`.

[![invocar el método GetProductByProductID (productID) de la clase ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image25.png)

**Figura 9**: invocación del método de `GetProductByProductID(productID)` de la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image27.png))

Tenga el valor del parámetro *`productID`* Obtenido de la propiedad `SelectedValue` del control GridView. Como se explicó anteriormente, la propiedad `SelectedValue` de GridView devuelve el primer valor de clave de datos de la fila seleccionada. Por lo tanto, es imperativo que la propiedad `DataKeyNames` de GridView esté establecida en `ProductID`, de modo que `SelectedValue`devuelva el valor `ProductID` de la fila seleccionada.

[![establecer el parámetro productID en la propiedad SelectedValue de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image28.png)

**Figura 10**: establezca el parámetro *`productID`* en la propiedad `SelectedValue` de GridView ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image30.png))

Una vez que se ha configurado correctamente el `productDetailsDataSource` ObjectDataSource y está enlazado a DetailsView, se completa este tutorial. Cuando la página se visita por primera vez, no se selecciona ninguna fila, por lo que la propiedad `SelectedValue` de GridView devuelve `Nothing`. Dado que no hay ningún producto con un valor de `ProductID` `NULL`, el método `GetProductByProductID(productID)` no devuelve ningún registro, lo que significa que no se muestra DetailsView (vea la figura 11). Al hacer clic en el botón seleccionar de una fila de GridView, se reentra un postback y se actualiza DetailsView. Esta vez, la propiedad `SelectedValue` de GridView devuelve el `ProductID` de la fila seleccionada, el método `GetProductByProductID(productID)` devuelve un `ProductsDataTable` con información sobre ese producto en particular y el control DetailsView muestra estos detalles (vea la figura 12).

[![cuando se visita por primera vez, solo se muestra GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image31.png)

**Figura 11**: cuando se visita por primera vez, solo se muestra GridView ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image33.png))

[![al seleccionar una fila, se muestran los detalles del producto.](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image34.png)

**Figura 12**: al seleccionar una fila, se muestran los detalles del producto ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb/_static/image36.png))

## <a name="summary"></a>Resumen

En este y en los tres tutoriales anteriores, hemos visto varias técnicas para mostrar informes maestros y detallados. En este tutorial, hemos examinado el uso de un control GridView seleccionable para hospedar los registros maestros y un control DetailsView para mostrar detalles sobre el registro maestro seleccionado en la misma página. En los tutoriales anteriores, hemos visto cómo mostrar los informes maestro y detalles con DropDownLists y cómo mostrar los registros maestros en una página web y los registros detallados en otro.

Este tutorial concluye el examen de los informes maestros y detallados. A partir del siguiente tutorial, comenzaremos a explorar el formato personalizado con GridView, DetailsView y FormView. Veremos cómo personalizar la apariencia de estos controles en función de los datos enlazados a ellos, cómo resumir los datos en el pie de página de GridView y cómo usar plantillas para obtener un mayor grado de control sobre el diseño.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-across-two-pages-vb.md)
