---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
title: Filtrado de maestro y detalles en dos páginas (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, se implementará este patrón mediante el uso de un control GridView para enumerar los proveedores en la base de datos. Cada fila de proveedor de GridView contendrá una...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 361d6a44-3f1f-4daf-85df-d4c2b8bf065d
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 252c6d5e48aff0087e090e3ddc1f58c84a2c030e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622881"
---
# <a name="masterdetail-filtering-across-two-pages-vb"></a>Filtrado de maestro y detalles en dos páginas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_9_VB.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-filtering-across-two-pages-vb/_static/datatutorial09vb1.pdf)

> En este tutorial, se implementará este patrón mediante el uso de un control GridView para enumerar los proveedores en la base de datos. Cada fila de proveedor de GridView contendrá un vínculo Ver productos que, al hacer clic en él, llevará al usuario a una página independiente en la que se muestran los productos del proveedor seleccionado.

## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores, vimos cómo mostrar los informes maestro y detalles en una sola página web mediante DropDownLists para mostrar los registros "maestros" y un control [GridView](master-detail-filtering-with-a-dropdownlist-vb.md) o [DetailsView](master-detail-filtering-with-two-dropdownlists-vb.md) para mostrar los "detalles". Otro patrón común que se usa para los informes maestro y detalles es tener los registros maestros en una página web y los detalles mostrados en otro. Un sitio web de foros, como [los foros de ASP.net](https://forums.asp.net/), es un buen ejemplo de este patrón en la práctica. Los foros de ASP.NET se componen de diversos foros Introducción, formularios Web Forms, controles de presentación de datos, etc. Cada foro se compone de varios subprocesos y cada subproceso se compone de un número de publicaciones. En la Página principal de foros de ASP.NET, se muestran los foros. Al hacer clic en un foro, se muestra una página de `ShowForum.aspx`, en la que se muestran los subprocesos de ese foro. Del mismo modo, al hacer clic en un subproceso se le lleva a `ShowPost.aspx`, que muestra las publicaciones del subproceso en el que se hizo clic.

En este tutorial, se implementará este patrón mediante el uso de un control GridView para enumerar los proveedores en la base de datos. Cada fila de proveedor de GridView contendrá un vínculo Ver productos que, al hacer clic en él, llevará al usuario a una página independiente en la que se muestran los productos del proveedor seleccionado.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Paso 1: agregar páginas de`SupplierListMaster.aspx`y`ProductsForSupplierDetails.aspx`a la carpeta`Filtering`

Al definir el diseño de página en el tercer tutorial, se han agregado varias páginas de "Inicio" en las carpetas `BasicReporting`, `Filtering`y `CustomFormatting`. Sin embargo, en ese momento no hemos agregado una página de inicio para este tutorial, por lo que dedique un momento a agregar dos páginas nuevas a la carpeta `Filtering`: `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` mostrará los registros "maestros" (los proveedores) mientras `ProductsForSupplierDetails.aspx` mostrará los productos del proveedor seleccionado.

Al crear estas dos páginas nuevas, asegúrese de asociarlas a la página maestra de `Site.master`.

![Agregar las páginas SupplierListMaster. aspx y ProductsForSupplierDetails. aspx a la carpeta de filtrado](master-detail-filtering-across-two-pages-vb/_static/image1.png)

**Figura 1**: agregar las páginas `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` a la carpeta `Filtering`

Además, al agregar nuevas páginas al proyecto, asegúrese de actualizar el archivo de mapa del sitio, `Web.sitemap`, según corresponda. Para este tutorial, basta con agregar la página `SupplierListMaster.aspx` al mapa del sitio con el siguiente contenido XML como elemento secundario del elemento `<siteMapNode>` de informes de filtrado:

[!code-xml[Main](master-detail-filtering-across-two-pages-vb/samples/sample1.xml)]

> [!NOTE]
> Puede ayudar a automatizar el proceso de actualización del archivo de mapa del sitio al agregar nuevas páginas de ASP.NET mediante la macro [K. Scott Allen](http://odetocode.com/Blogs/scott/)gratuita de [mapa del sitio](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)de Visual Studio.

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Paso 2: Mostrar la lista de proveedores en`SupplierListMaster.aspx`

Con las páginas `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` creadas, el siguiente paso es crear el GridView de proveedores en `SupplierListMaster.aspx`. Agregue un control GridView a la página y enlácelo a un nuevo ObjectDataSource. Este origen ObjectDataSource debe utilizar el método `GetSuppliers()` de la clase `SuppliersBLL` para devolver todos los proveedores.

[![seleccionar la clase SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image3.png)](master-detail-filtering-across-two-pages-vb/_static/image2.png)

**Figura 2**: seleccione la clase `SuppliersBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image4.png))

[![configurar ObjectDataSource para usar el método GetSuppliers ()](master-detail-filtering-across-two-pages-vb/_static/image6.png)](master-detail-filtering-across-two-pages-vb/_static/image5.png)

**Figura 3**: configuración de ObjectDataSource para usar el método `GetSuppliers()` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image7.png))

Necesitamos incluir un vínculo titulado Ver productos en cada fila de GridView que, al hacer clic en él, lleva al usuario a `ProductsForSupplierDetails.aspx` pasando el valor `SupplierID` de la fila seleccionada a través de la cadena de consulta. Por ejemplo, si el usuario hace clic en el vínculo Ver productos del proveedor de Tokyo Traders (que tiene un valor de `SupplierID` de 4), se deben enviar a `ProductsForSupplierDetails.aspx?SupplierID=4`.

Para ello, agregue un [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) a GridView, que agrega un hipervínculo a cada fila de GridView. Para empezar, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView. Después, seleccione HyperLinkField en la lista de la parte superior izquierda y haga clic en Agregar para incluir el HyperLinkField en la lista de campos de GridView.

[![agregar un HyperLinkField a GridView](master-detail-filtering-across-two-pages-vb/_static/image9.png)](master-detail-filtering-across-two-pages-vb/_static/image8.png)

**Ilustración 4**: agregar un HyperLinkField a GridView ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image10.png))

HyperLinkField puede configurarse para usar los mismos valores de texto o dirección URL que el vínculo en cada fila de GridView, o puede basar estos valores en los valores de datos enlazados a cada fila en particular. Para especificar un valor estático en todas las filas, use las propiedades `Text` o `NavigateUrl` de HyperLinkField. Como queremos que el texto del vínculo sea el mismo para todas las filas, establezca la propiedad `Text` de HyperLinkField para ver los productos.

[![establecer la propiedad Text de HyperLinkField para ver productos](master-detail-filtering-across-two-pages-vb/_static/image12.png)](master-detail-filtering-across-two-pages-vb/_static/image11.png)

**Figura 5**: establecimiento de la propiedad `Text` de HyperLinkField para ver productos ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image13.png))

Para establecer los valores de texto o dirección URL que se van a basar en los datos subyacentes enlazados a la fila de GridView, especifique los campos de datos de los que se deben extraer los valores de texto o dirección URL en las propiedades `DataTextField` o `DataNavigateUrlFields`. `DataTextField` solo se puede establecer en un campo de datos único; No obstante, `DataNavigateUrlFields`se puede establecer en una lista delimitada por comas de campos de datos. A menudo es necesario basar el texto o la dirección URL en una combinación del valor del campo de datos de la fila actual y algún marcado estático. En este tutorial, por ejemplo, queremos que la dirección URL de los vínculos de HyperLinkField sea `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, donde *`supplierID`* es el valor de `SupplierID` Row's de GridView. Observe que necesitamos valores estáticos y controlados por datos aquí: la parte `ProductsForSupplierDetails.aspx?SupplierID=` de la dirección URL del vínculo es estática, mientras que la parte *`supplierID`* está controlada por datos, ya que su valor es el propio valor `SupplierID` de cada fila.

Para indicar una combinación de valores estáticos y basados en datos, use las propiedades `DataTextFormatString` y `DataNavigateUrlFormatString`. En estas propiedades, escriba el marcado estático según sea necesario y, a continuación, use el marcador `{0}` donde desee que aparezca el valor del campo especificado en las propiedades `DataTextField` o `DataNavigateUrlFields`. Si la propiedad `DataNavigateUrlFields` tiene varios campos especificados, use `{0}` donde desea insertar el primer valor de campo, `{1}` para el segundo valor de campo, etc.

Al aplicar esto a nuestro tutorial, es necesario establecer la propiedad `DataNavigateUrlFields` en `SupplierID`, ya que ese es el campo de datos cuyo valor necesitamos personalizar por fila y la propiedad `DataNavigateUrlFormatString` para `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![configurar HyperLinkField para incluir la dirección URL de vínculo adecuada en función del IdProveedor](master-detail-filtering-across-two-pages-vb/_static/image15.png)](master-detail-filtering-across-two-pages-vb/_static/image14.png)

**Figura 6**: configuración de HyperLinkField para incluir la dirección URL de vínculo adecuada en función del `SupplierID` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image16.png))

Después de agregar HyperLinkField, no dude en personalizar y reordenar los campos de GridView. El marcado siguiente muestra GridView después de haber realizado algunas personalizaciones de nivel de campo secundarias.

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample2.aspx)]

Tómese un momento para ver la página `SupplierListMaster.aspx` a través de un explorador. Como se muestra en la figura 7, la página enumera actualmente todos los proveedores, incluido un vínculo Ver productos. Al hacer clic en el vínculo Ver productos, se le llevará a `ProductsForSupplierDetails.aspx`, pasando a lo largo del `SupplierID` del proveedor en la cadena de consulta.

[![cada fila de proveedor contiene un vínculo Ver productos](master-detail-filtering-across-two-pages-vb/_static/image18.png)](master-detail-filtering-across-two-pages-vb/_static/image17.png)

**Figura 7**: cada fila de proveedor contiene un vínculo Ver productos ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Paso 3: enumeración de los productos del proveedor en`ProductsForSupplierDetails.aspx`

En este momento, la página `SupplierListMaster.aspx` envía usuarios a `ProductsForSupplierDetails.aspx`, pasando el `SupplierID` del proveedor seleccionado en la cadena de mensajes. El paso final del tutorial consiste en mostrar los productos en un control GridView en `ProductsForSupplierDetails.aspx` cuyo `SupplierID` es igual al `SupplierID` pasado a través de la cadena de presentación. Para llevar a cabo este paso, agregue un control GridView a la página `ProductsForSupplierDetails.aspx`, utilizando un nuevo control ObjectDataSource denominado `ProductsBySupplierDataSource` que invoca el método `GetProductsBySupplierID(supplierID)` de la clase `ProductsBLL`.

[![agregar un nuevo ObjectDataSource denominado ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-vb/_static/image21.png)](master-detail-filtering-across-two-pages-vb/_static/image20.png)

**Figura 8**: agregar un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image22.png))

[![seleccionar la clase ProductsBLL](master-detail-filtering-across-two-pages-vb/_static/image24.png)](master-detail-filtering-across-two-pages-vb/_static/image23.png)

**Figura 9**: seleccione la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image25.png))

[![tienen el método ObjectDataSource invocación del método GetProductsBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image27.png)](master-detail-filtering-across-two-pages-vb/_static/image26.png)

**Figura 10**: hacer que ObjectDataSource invoque el método `GetProductsBySupplierID(supplierID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image28.png))

El paso final del Asistente para configurar orígenes de datos nos pide que proporcione el origen del parámetro *`supplierID`* del método `GetProductsBySupplierID(supplierID)`. Para usar el valor QueryString, establezca el origen del parámetro en QueryString y escriba el nombre del valor QueryString que se va a usar en el cuadro de texto QueryStringField (`SupplierID`).

[![rellenar el valor del parámetro supplierID desde el valor de QueryString de IdProveedor](master-detail-filtering-across-two-pages-vb/_static/image30.png)](master-detail-filtering-across-two-pages-vb/_static/image29.png)

**Figura 11**: Rellene el valor del parámetro *`supplierID`* del valor de la cadena de consulta `SupplierID` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image31.png))

Así de simple. En la ilustración 12 se muestra la página `ProductsForSupplierDetails.aspx` cuando se visita haciendo clic en el vínculo de Tokyo traders de `SupplierListMaster.aspx`.

[![se muestran los productos suministrados por Tokyo Traders](master-detail-filtering-across-two-pages-vb/_static/image33.png)](master-detail-filtering-across-two-pages-vb/_static/image32.png)

**Figura 12**: se muestran los productos suministrados por Tokyo Traders ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Mostrar información de proveedor en`ProductsForSupplierDetails.aspx`

Como se muestra en la figura 12, en la página `ProductsForSupplierDetails.aspx` simplemente se enumeran los productos suministrados por el `SupplierID` especificado en QueryString. Sin embargo, alguien que envió directamente a esta página no sabría que la figura 12 muestre los productos de Tokyo Traders. Para solucionarlo, se puede mostrar información de proveedor también en esta página.

Empiece agregando un FormView sobre los productos GridView. Cree un nuevo control ObjectDataSource denominado `SuppliersDataSource` que invoque el método `GetSupplierBySupplierID(supplierID)` de la clase `SuppliersBLL`.

[![seleccionar la clase SuppliersBLL](master-detail-filtering-across-two-pages-vb/_static/image36.png)](master-detail-filtering-across-two-pages-vb/_static/image35.png)

**Figura 13**: seleccione la clase `SuppliersBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image37.png))

[![tienen el método ObjectDataSource invocación del método GetSupplierBySupplierID (supplierID)](master-detail-filtering-across-two-pages-vb/_static/image39.png)](master-detail-filtering-across-two-pages-vb/_static/image38.png)

**Figura 14**: hacer que ObjectDataSource invoque el método `GetSupplierBySupplierID(supplierID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image40.png))

Al igual que con el `ProductsBySupplierDataSource`, tenga el parámetro *`supplierID`* asignado al valor de `SupplierID` QueryString.

[![rellenar el valor del parámetro supplierID desde el valor de QueryString de IdProveedor](master-detail-filtering-across-two-pages-vb/_static/image42.png)](master-detail-filtering-across-two-pages-vb/_static/image41.png)

**Figura 15**: Rellene el valor del parámetro *`supplierID`* del valor `SupplierID` QueryString ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image43.png))

Al enlazar el FormView a ObjectDataSource en el Vista de diseño, Visual Studio creará automáticamente los `ItemTemplate`, `InsertItemTemplate`y `EditItemTemplate` de FormView con controles Web de etiqueta y de cuadro de texto para cada uno de los campos de datos devueltos por ObjectDataSource. Puesto que solo deseamos Mostrar información del proveedor, no dude en quitar el `InsertItemTemplate` y `EditItemTemplate`. Después, edite el ItemTemplate para que muestre el nombre de la empresa del proveedor en un `<h3>` elemento y la dirección, la ciudad, el país y el número de teléfono por debajo del nombre de la empresa. Como alternativa, puede establecer manualmente la `DataSourceID` de FormView y crear el marcado de `ItemTemplate`, como hicimos en el tutorial "[Mostrar datos con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)".

Después de estos cambios, el marcado declarativo de FormView debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-across-two-pages-vb/samples/sample3.aspx)]

En la figura 16 se muestra una captura de pantalla de la página `ProductsForSupplierDetails.aspx` una vez incluida la información de proveedor detallada anteriormente.

[![la lista de productos incluye un resumen sobre el proveedor](master-detail-filtering-across-two-pages-vb/_static/image45.png)](master-detail-filtering-across-two-pages-vb/_static/image44.png)

**Figura 16**: la lista de productos incluye un resumen del proveedor ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Aplicar los toques finales para la interfaz de usuario de`ProductsForSupplierDetails.aspx`

Para mejorar la experiencia del usuario en este informe, hay un par de adiciones que debemos hacer en la página `ProductsForSupplierDetails.aspx`. Actualmente, la única manera en que un usuario puede ir de la página `ProductsForSupplierDetails.aspx` a la lista de proveedores es hacer clic en el botón atrás del explorador. Vamos a agregar un control de hipervínculo a la página `ProductsForSupplierDetails.aspx` que vuelve a vincular a `SupplierListMaster.aspx`, lo que proporciona otra manera para que el usuario vuelva a la lista maestra.

[![agregar un control de hipervínculo para devolver el usuario a SupplierListMaster. aspx](master-detail-filtering-across-two-pages-vb/_static/image48.png)](master-detail-filtering-across-two-pages-vb/_static/image47.png)

**Figura 17**: agregar un control de hipervínculo para devolver el usuario a `SupplierListMaster.aspx` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image49.png))

Si el usuario hace clic en el vínculo Ver productos de un proveedor que no tiene ningún producto, el `ProductsBySupplierDataSource` ObjectDataSource en `ProductsForSupplierDetails.aspx` no devolverá ningún resultado. GridView enlazado a ObjectDataSource no representará ningún marcado que dé lugar a una región en blanco en la página del explorador del usuario. Para comunicarse más claramente al usuario de que no hay ningún producto asociado al proveedor seleccionado, podemos establecer la propiedad `EmptyDataText` de GridView en el mensaje que queremos que se muestre cuando surja esta situación. He establecido esta propiedad en "no hay productos proporcionados por este proveedor".

De forma predeterminada, todos los proveedores de la base de datos Northwind proporcionan al menos un producto. Sin embargo, para este tutorial he modificado manualmente la tabla `Products` de modo que el proveedor Escargots Nouveaux ya no esté asociado con ningún producto. En la figura 18 se muestra la página de detalles de Escargots Nouveaux después de realizar este cambio.

[![se informa a los usuarios de que el proveedor no proporciona ningún producto](master-detail-filtering-across-two-pages-vb/_static/image51.png)](master-detail-filtering-across-two-pages-vb/_static/image50.png)

**Figura 18**: se informa a los usuarios de que el proveedor no proporciona ningún producto ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-across-two-pages-vb/_static/image52.png))

## <a name="summary"></a>Resumen

Aunque los informes maestros y detallados pueden mostrar los registros maestro y detalle en una sola página, en muchos sitios web se separan en dos páginas Web. En este tutorial, hemos examinado cómo implementar este tipo de informe maestro y detalles mediante la lista de los proveedores de GridView en la página web "maestra" y los productos asociados que se enumeran en la página "detalles". Cada fila de proveedor de la página web maestra contenía un vínculo a la página de detalles que pasó a lo largo del valor de `SupplierID` de la fila. Estos vínculos específicos de la fila se pueden agregar fácilmente mediante el HyperLinkField de GridView.

En la página de detalles, la recuperación de esos productos para el proveedor especificado se ha realizado mediante la invocación del método `GetProductsBySupplierID(supplierID)` de la clase `ProductsBLL`. El valor del parámetro *`supplierID`* se especificó mediante declaración con QueryString como el origen del parámetro. También veremos cómo mostrar los detalles del proveedor en la página de detalles mediante FormView.

El [siguiente tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) es el último de los informes maestros y detallados. Veremos cómo mostrar una lista de los productos en un control GridView en el que cada fila tiene un botón seleccionar. Al hacer clic en el botón seleccionar, se mostrarán los detalles del producto en un control DetailsView de la misma página.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-two-dropdownlists-vb.md)
> [Siguiente](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md)
