---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Filtrado de maestro y detalles con dos DropDownLists (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se expande la relación principal-detalle para agregar una tercera capa, mediante dos controles DropDownList para seleccionar el elemento primario y el grabación primario que se desean...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: 166d6a7664a326361dc2a3f115eddb988cd39d20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424297"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Filtrado de maestro y detalles con dos DropDownLists (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> En este tutorial se expande la relación principal-detalle para agregar una tercera capa, mediante dos controles DropDownList para seleccionar los registros primarios y principales deseados.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-with-a-dropdownlist-vb.md) , hemos examinado cómo mostrar un informe sencillo de maestro y detalles con un solo DropDownList rellenado con las categorías y un control GridView que muestra los productos que pertenecen a la categoría seleccionada. Este patrón de informe funciona bien cuando se muestran registros que tienen una relación de uno a varios y se pueden ampliar fácilmente para trabajar en escenarios que incluyen varias relaciones uno a varios. Por ejemplo, un sistema de entrada de pedidos tendría tablas que corresponden a clientes, pedidos y artículos de línea de pedido. Un cliente determinado puede tener varios pedidos con cada pedido que consta de varios elementos. Estos datos se pueden presentar al usuario con dos DropDownLists y un control GridView. El primer DropDownList tendría un elemento de lista para cada cliente en la base de datos, donde el contenido de la segunda parte es el de los pedidos realizados por el cliente seleccionado. Un control GridView Enumeraría los elementos de línea del pedido seleccionado.

Aunque la base de datos Northwind incluye la información canónica Customer/Order/Order Details en sus `Customers`, `Orders`y `Order Details` tablas, estas tablas no se capturan en nuestra arquitectura. No obstante, podemos seguir ilustrando el uso de dos DropDownLists dependientes. En el primer DropDownList se enumeran las categorías y la segunda de los productos que pertenecen a la categoría seleccionada. A continuación, DetailsView mostrará los detalles del producto seleccionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Paso 1: crear y rellenar las categorías DropDownList

Nuestro primer objetivo es agregar el DropDownList que enumera las categorías. Estos pasos se examinaron detalladamente en el tutorial anterior, pero se resumen aquí para la integridad.

Abra la página `MasterDetailsDetails.aspx` de la carpeta `Filtering`, agregue un DropDownList a la página, establezca su propiedad `ID` en `Categories`y, a continuación, haga clic en el vínculo configurar origen de datos en su etiqueta inteligente. En el Asistente para la configuración de orígenes de datos, elija Agregar un nuevo origen de datos.

[![agregar un nuevo origen de datos para DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Figura 1**: agregar un nuevo origen de datos para el DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))

El nuevo origen de datos debe ser, de forma natural, una ObjectDataSource. Asigne a este nuevo `CategoriesDataSource` ObjectDataSource el nombre e invoque al método `GetCategories()` del objeto `CategoriesBLL`.

[![optar por usar la clase CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Figura 2**: elija usar la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))

[![configurar ObjectDataSource para usar el método GetCategories ()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Figura 3**: configuración de ObjectDataSource para usar el método `GetCategories()` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))

Después de configurar la ObjectDataSource, todavía necesitamos especificar qué campo de origen de datos se debe mostrar en el `Categories` DropDownList y cuál debe configurarse como el valor del elemento de lista. Establezca el campo `CategoryName` como la pantalla y `CategoryID` como el valor de cada elemento de la lista.

[![que el DropDownList muestre el campo CategoryName y use CategoryID como valor](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Figura 4**: haga que el DropDownList muestre el campo `CategoryName` y use `CategoryID` como valor ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))

En este momento, tenemos un control DropDownList (`Categories`) que se rellena con los registros de la tabla `Categories`. Cuando el usuario elige una nueva categoría de la clase DropDownList, queremos que se produzca un postback para actualizar el objeto DropDownList del producto que vamos a crear en el paso 2. Por lo tanto, active la opción Habilitar AutoPostBack en la etiqueta inteligente de `categories` DropDownList.

[![habilitar AutoPostBack para las categorías DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Figura 5**: habilitar AutoPostBack para el `Categories` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Paso 2: mostrar los productos de la categoría seleccionada en un segundo DropDownList

Con el `Categories` DropDownList completado, el siguiente paso es mostrar un DropDownList de productos que pertenecen a la categoría seleccionada. Para ello, agregue otro DropDownList a la página denominada `ProductsByCategory`. Como con el `Categories` DropDownList, cree un nuevo ObjectDataSource para el `ProductsByCategory` DropDownList denominado `ProductsByCategoryDataSource`.

[![agregar un nuevo origen de datos para el ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Figura 6**: agregar un nuevo origen de datos para el `ProductsByCategory` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))

[![crear un nuevo ObjectDataSource denominado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Figura 7**: creación de un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))

Dado que el `ProductsByCategory` DropDownList necesita mostrar solo los productos que pertenecen a la categoría seleccionada, haga que ObjectDataSource invoque el método `GetProductsByCategoryID(categoryID)` del objeto `ProductsBLL`.

[![optar por usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Figura 8**: elija usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))

[![configurar ObjectDataSource para usar el método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Figura 9**: configuración de ObjectDataSource para usar el método `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))

En el paso final del asistente, es necesario especificar el valor del parámetro *`categoryID`* . Asigne este parámetro al elemento seleccionado desde el `Categories` DropDownList.

[![extraer el valor del parámetro categoryID de las categorías DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Figura 10**: extracción del valor del parámetro *`categoryID`* del `Categories` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))

Con ObjectDataSource configurado, todo lo que queda es especificar qué campos de origen de datos se usan para la presentación y el valor de los elementos de DropDownList. Muestre el campo `ProductName` y use el campo `ProductID` como valor.

[![especificar los campos de origen de datos utilizados para las propiedades de texto y valor de ListItems de DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Figura 11**: especifique los campos de origen de datos usados para las propiedades `Text` y `Value` de `ListItem` s de DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))

Con ObjectDataSource y `ProductsByCategory` DropDownList configurados, la página mostrará dos DropDownLists: la primera enumerará todas las categorías, mientras que la segunda enumerará los productos que pertenecen a la categoría seleccionada. Cuando el usuario selecciona una nueva categoría del primer DropDownList, se producirá un postback y el segundo DropDownList se volverá a enlazar, mostrando los productos que pertenecen a la categoría recién seleccionada. Las figuras 12 y 13 muestran `MasterDetailsDetails.aspx` en acción cuando se ven a través de un explorador.

[![al visitar la página por primera vez, se selecciona la categoría bebidas](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Figura 12**: al visitar la página por primera vez, se selecciona la categoría bebidas ([haga clic para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))

[![elegir una categoría diferente muestra los productos de la nueva categoría](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Figura 13**: selección de una categoría diferente muestra los productos de la nueva categoría ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))

Actualmente, el `productsByCategory` DropDownList, cuando se cambia, *no* produce un postback. Sin embargo, se deseará que se produzca un postback una vez que se agregue un DetailsView para mostrar los detalles del producto seleccionado (paso 3). Por lo tanto, active la casilla habilitar AutoPostBack en la etiqueta inteligente de `productsByCategory` DropDownList.

[![habilitar la característica de postback para productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Figura 14**: habilitar la característica de postback para el `productsByCategory` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Paso 3: usar DetailsView para mostrar los detalles del producto seleccionado

El paso final consiste en mostrar los detalles del producto seleccionado en DetailsView. Para ello, agregue un DetailsView a la página, establezca su propiedad `ID` en `ProductDetails`y cree un nuevo ObjectDataSource para él. Configure este origen ObjectDataSource para que extraiga sus datos del método `GetProductByProductID(productID)` de la clase `ProductsBLL` mediante el valor seleccionado de la `ProductsByCategory` DropDownList para el valor del parámetro *`productID`* .

[![optar por usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Figura 15**: elija usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))

[![configurar ObjectDataSource para usar el método GetProductByProductID (productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Figura 16**: configuración de ObjectDataSource para usar el método `GetProductByProductID(productID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))

[![extraer el valor del parámetro productID de ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Figura 17**: extracción del valor del parámetro *`productID`* del `ProductsByCategory` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))

Puede optar por mostrar cualquiera de los campos disponibles en la `ProductDetails` DetailsView. He optado por quitar los campos `ProductID`, `SupplierID`y `CategoryID`, y volver a ordenar y dar formato a los campos restantes. Además, se borrón las propiedades `Height` y `Width` de DetailsView, lo que permite a DetailsView expandirse hasta el ancho necesario para mostrar mejor sus datos en lugar de limitarse a un tamaño especificado. A continuación se muestra el marcado completo:

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Dedique un momento a probar la página de `MasterDetailsDetails.aspx` en un explorador. A primera vista, puede parecer que todo funciona como se desea, pero hay un problema sutil. Al elegir una nueva categoría, el `ProductsByCategory` DropDownList se actualiza para incluir los productos de la categoría seleccionada, pero el `ProductDetails` DetailsView continuó mostrando la información anterior del producto. DetailsView se actualiza al elegir un producto diferente para la categoría seleccionada. Además, si realiza pruebas lo suficientemente minuciosas, observará que si elige continuamente nuevas categorías (por ejemplo, seleccionando bebidas en el `Categories` DropDownList, después condimentos y, después, confections), se actualizará el `ProductDetails` DetailsView.

Para ayudar a concretizar la este problema, echemos un vistazo a un ejemplo específico. Al visitar la página por primera vez, se selecciona la categoría bebidas y se cargan los productos relacionados en el `ProductsByCategory` DropDownList. Chai es el producto seleccionado y sus detalles se muestran en el `ProductDetails` DetailsView, como se muestra en la figura 18.

[![los detalles del producto seleccionado se muestran en DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Figura 18**: los detalles del producto seleccionado se muestran en DetailsView ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))

Si cambia la selección de categoría de bebidas a condimentos, se produce un postback y el `ProductsByCategory` DropDownList se actualiza en consecuencia, pero el DetailsView sigue mostrando los detalles de Chai.

[![se siguen mostrando los detalles del producto seleccionado anteriormente](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Figura 19**: los detalles del producto seleccionado anteriormente siguen apareciendo ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))

Al seleccionar un nuevo producto de la lista, se actualiza DetailsView según lo previsto. Si elige una nueva categoría después de cambiar el producto, el DetailsView no se actualizará. Sin embargo, si en lugar de elegir un nuevo producto seleccionó una nueva categoría, DetailsView se actualizaría. ¿Qué ocurre en el mundo aquí?

El problema es un problema de temporización en el ciclo de vida de la página. Cada vez que se solicita una página, pasa por una serie de pasos como representación. En uno de estos pasos, los controles ObjectDataSource comprueban si han cambiado los valores de `SelectParameters`. Si es así, el control Web de datos enlazado a ObjectDataSource sabe que necesita actualizar su presentación. Por ejemplo, cuando se selecciona una nueva categoría, el `ProductsByCategoryDataSource` ObjectDataSource detecta que sus valores de parámetro han cambiado y el `ProductsByCategory` DropDownList se vuelve a enlazar, obteniendo los productos de la categoría seleccionada.

El problema que surge en esta situación es que el punto del ciclo de vida de la página en el que la ObjectDataSource comprueba los parámetros modificados se produce *antes* del reenlace de los controles Web de datos asociados. Por lo tanto, al seleccionar una nueva categoría, el `ProductsByCategoryDataSource` ObjectDataSource detecta un cambio en el valor de su parámetro. Sin embargo, el origen ObjectDataSource utilizado por el `ProductDetails` DetailsView no anota tales cambios porque el `ProductsByCategory` DropDownList todavía se debe volver a enlazar. Más adelante, en el ciclo de vida, el `ProductsByCategory` DropDownList vuelve a enlazar a su ObjectDataSource, lo que capta los productos de la categoría recién seleccionada. Mientras el valor de la `ProductsByCategory` DropDownList ha cambiado, la `ProductDetails` ObjectDataSource de DetailsView ya ha hecho su comprobación de valor de parámetro; por lo tanto, DetailsView muestra los resultados anteriores. Esta interacción se representa en la figura 20.

[![el valor de ProductsByCategory de DropDownList cambia después de que la ObjectDataSource de ProductDetails de DetailsView busque cambios](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Figura 20**: el valor de `ProductsByCategory` DropDownList cambia después del `ProductDetails` ObjectDataSource de DetailsView comprueba si hay cambios ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))

Para solucionar este error, es necesario volver a enlazar explícitamente el `ProductDetails` DetailsView una vez enlazada la `ProductsByCategory` DropDownList. Podemos lograr esto llamando al método `DataBind()` de `ProductDetails` DetailsView cuando se activa el evento `DataBound` de la `ProductsByCategory` DropDownList. Agregue el siguiente código de controlador de eventos a la clase de código subyacente de la página `MasterDetailsDetails.aspx` (consulte "[establecer mediante programación los valores de parámetro de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para obtener una explicación sobre cómo agregar un controlador de eventos):

[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Después de agregar esta llamada explícita al método `DataBind()` de `ProductDetails` DetailsView, el tutorial funciona según lo esperado. En la figura 21 se resalta cómo este cambio ha solucionado el problema anterior.

[![se actualiza explícitamente DetailsView de ProductDetails cuando se activa el evento DataBound de ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Figura 21**: el `ProductDetails` DetailsView se actualiza explícitamente cuando se activa el evento `DataBound` de `ProductsByCategory` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))

## <a name="summary"></a>Resumen

DropDownList actúa como un elemento de interfaz de usuario ideal para los informes maestros y detallados en los que hay una relación de uno a varios entre los registros maestro y de detalle. En el tutorial anterior, vimos cómo usar un solo DropDownList para filtrar los productos mostrados por la categoría seleccionada. En este tutorial se ha reemplazado GridView de productos con DropDownList y se ha usado DetailsView para mostrar los detalles del producto seleccionado. Los conceptos descritos en este tutorial se pueden ampliar fácilmente a los modelos de datos que implican varias relaciones uno a varios, como clientes, pedidos y artículos de pedido. En general, siempre puede Agregar un DropDownList para cada una de las entidades "uno" en las relaciones uno a varios.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Siguiente](master-detail-filtering-across-two-pages-vb.md)
