---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Agregar y responder a los botones en un control GridView (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos cómo agregar botones personalizados, tanto a una plantilla como a los campos de un control GridView o DetailsView. En particular, Bui...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8727d8faead02340d223c75845bf29f63d1a0834
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601006"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Agregar y responder a los botones en un control GridView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) de la aplicación de ejemplo o [descarga de PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> En este tutorial veremos cómo agregar botones personalizados, tanto a una plantilla como a los campos de un control GridView o DetailsView. En concreto, crearemos una interfaz que tenga un FormView que permita al usuario paginar a través de los proveedores.

## <a name="introduction"></a>Introducción

Aunque muchos escenarios de informes implican acceso de solo lectura a los datos del informe, no es raro que los informes incluyan la capacidad de realizar acciones en función de los datos mostrados. Normalmente, esto implica agregar un control Web Button, LinkButton o ImageButton con cada registro mostrado en el informe que, cuando se hace clic en él, produce un postback e invoca algún código del lado servidor. La edición y eliminación de los datos por registro es el ejemplo más común. De hecho, como vimos a partir de la [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , la edición y la eliminación son tan comunes que los controles GridView, DetailsView y FormView pueden admitir dicha funcionalidad sin necesidad de escribir una sola línea de código.

Además de los botones editar y eliminar, los controles GridView, DetailsView y FormView también pueden incluir botones, LinkButtons o ImageButtons que, cuando se hace clic en él, realizan alguna lógica personalizada del lado servidor. En este tutorial veremos cómo agregar botones personalizados, tanto a una plantilla como a los campos de un control GridView o DetailsView. En concreto, crearemos una interfaz que tenga un FormView que permita al usuario paginar a través de los proveedores. Para un proveedor determinado, FormView mostrará información sobre el proveedor junto con un control Web Button que, si se hace clic en él, marcará todos los productos asociados como descontinuados. Además, un control GridView muestra los productos proporcionados por el proveedor seleccionado, con cada fila que contiene los botones aumentar precio y precio de descuento que, si se hace clic en ellos, elevan o reducen el producto s `UnitPrice` en un 10% (consulte la figura 1).

[![tanto FormView como GridView contienen botones que realizan acciones personalizadas](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: FormView y GridView contienen botones que realizan acciones personalizadas ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial de botones

Antes de echar un vistazo a cómo agregar botones personalizados, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial. Comience agregando una nueva carpeta denominada `CustomButtons`. Después, agregue las dos páginas de ASP.NET siguientes a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Agregue las páginas ASP.NET para los tutoriales relacionados con los botones personalizados.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: adición de las páginas de ASP.net para los tutoriales relacionados con los botones personalizados

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `CustomButtons` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))

Por último, agregue las páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de la paginación y la ordenación `<siteMapNode>`:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición, inserción y eliminación.

![Ahora, el mapa del sitio incluye la entrada para el tutorial de botones personalizados.](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: ahora, el mapa del sitio incluye la entrada para el tutorial de botones personalizados.

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Paso 2: agregar un FormView que Enumere los proveedores

Vamos a empezar a trabajar con este tutorial agregando el FormView que enumera los proveedores. Tal y como se describe en la introducción, este FormView permitirá al usuario desplazarse por los proveedores y mostrar los productos proporcionados por el proveedor en un control GridView. Además, este FormView incluirá un botón que, cuando se hace clic en él, marcará todos los productos de proveedores como descontinuados. Antes de preocuparnos por agregar el botón personalizado a FormView, vamos a crear primero el FormView para que muestre la información del proveedor.

Para empezar, abra la página `CustomButtons.aspx` de la carpeta `CustomButtons`. Agregue un FormView a la página arrastrándolo desde el cuadro de herramientas hasta el diseñador y establezca su propiedad `ID` en `Suppliers`. En la etiqueta inteligente FormView s, opte por crear un nuevo ObjectDataSource denominado `SuppliersDataSource`.

[![crear un nuevo ObjectDataSource denominado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))

Configure este nuevo ObjectDataSource para que realice consultas desde el método `SuppliersBLL` Class s `GetSuppliers()` (consulte la figura 6). Puesto que este FormView no proporciona una interfaz para actualizar la información de proveedor, seleccione la opción (ninguno) en la lista desplegable de la pestaña actualizar.

[![configurar el origen de datos para utilizar el método SuppliersBLL de la clase GetSuppliers ()](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configuración del origen de datos para usar el método `SuppliersBLL` Class s `GetSuppliers()` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))

Después de configurar ObjectDataSource, Visual Studio generará un `InsertItemTemplate`, `EditItemTemplate`y `ItemTemplate` para FormView. Quite los `InsertItemTemplate` y `EditItemTemplate` y modifique la `ItemTemplate` para que muestre solo el nombre de la compañía y el número de teléfono del proveedor. Por último, active la casilla habilitar paginación de la etiqueta inteligente para activar la compatibilidad con la paginación (o establezca su propiedad `AllowPaging` en `True`). Después de estos cambios, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

La figura 7 muestra la página CustomButtons. aspx cuando se ve a través de un explorador.

[![el FormView muestra los campos CompanyName y Phone del proveedor actualmente seleccionado](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: el FormView enumera los campos `CompanyName` y `Phone` del proveedor actualmente seleccionado ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Paso 3: agregar un control GridView que muestre los productos de proveedores seleccionados

Antes de agregar el botón Discontinue All Products a la plantilla FormView s, vamos a agregar primero un control GridView debajo del FormView que muestra los productos proporcionados por el proveedor seleccionado. Para ello, agregue un control GridView a la página, establezca su propiedad `ID` en `SuppliersProducts`y agregue un nuevo ObjectDataSource denominado `SuppliersProductsDataSource`.

[![crear un nuevo ObjectDataSource denominado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: creación de un nuevo ObjectDataSource denominado `SuppliersProductsDataSource` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))

Configure esta ObjectDataSource para usar el método de `GetProductsBySupplierID(supplierID)` de clase ProductsBLL (consulte la figura 9). Aunque este control GridView permitirá ajustar el precio de un producto, no usará las características de edición o eliminación integradas de GridView. Por lo tanto, podemos establecer la lista desplegable en (ninguno) para las pestañas de la actualización, inserción y eliminación de ObjectDataSource.

[![configurar el origen de datos para usar el método ProductsBLL de la clase GetProductsBySupplierID (IdProveedor)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configuración del origen de datos para usar el método `ProductsBLL` Class s `GetProductsBySupplierID(supplierID)` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))

Dado que el método `GetProductsBySupplierID(supplierID)` acepta un parámetro de entrada, el Asistente para ObjectDataSource nos solicita el origen de este valor de parámetro. Para pasar el valor `SupplierID` desde FormView, establezca la lista desplegable origen del parámetro en control y la lista desplegable ControlID en `Suppliers` (el identificador del FormView creado en el paso 2).

[![indican que el parámetro supplierID debe proviene del control FormView Suppliers](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicar que el parámetro *`supplierID`* debe proviene del control FormView `Suppliers` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))

Después de completar el Asistente de ObjectDataSource, GridView contendrá un BoundField o CheckBoxField para cada uno de los campos de datos de los productos. Vamos a recortar esto para mostrar solo el `ProductName` y `UnitPrice` BoundFields junto con el `Discontinued` CheckBoxField; Además, vamos a dar formato a la `UnitPrice` BoundField de modo que su texto tenga el formato de moneda. El marcado declarativo de GridView y `SuppliersProductsDataSource` debe ser similar al marcado siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

En este momento, nuestro tutorial muestra un informe de maestro y detalles, lo que permite al usuario elegir un proveedor del FormView en la parte superior y ver los productos proporcionados por ese proveedor a través de GridView en la parte inferior. En la figura 11 se muestra una captura de pantalla de esta página al seleccionar el proveedor de Tokyo Traders desde FormView.

[![los productos de proveedores seleccionados se muestran en GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: los productos de proveedores seleccionados se muestran en GridView ([haga clic para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Paso 4: crear los métodos DAL y BLL para interrumpir todos los productos de un proveedor

Antes de poder agregar un botón a FormView que, cuando se hace clic en él, deja de incluir todos los productos de proveedores, primero es necesario agregar un método a la capa DAL y a la capa BLL que realiza esta acción. En concreto, este método se denominará `DiscontinueAllProductsForSupplier(supplierID)`. Cuando se haga clic en el botón FormView, llamaremos a este método en el nivel de lógica de negocios, pasando el `SupplierID`de proveedor seleccionado; a continuación, el BLL llamará al método de capa de acceso a datos correspondiente, que emitirá una instrucción `UPDATE` a la base de datos que deja de ser los productos de proveedores especificados.

Como hemos hecho en los tutoriales anteriores, usaremos un enfoque de abajo, comenzando por la creación del método DAL, el método BLL y, por último, la implementación de la funcionalidad en la página ASP.NET. Abra el `Northwind.xsd` conjunto de tipos en la carpeta `App_Code/DAL` y agregue un nuevo método a la `ProductsTableAdapter` (haga clic con el botón derecho en el `ProductsTableAdapter` y elija Agregar consulta). Al hacerlo, se abrirá el Asistente para configuración de consultas de TableAdapter, que nos guiará por el proceso de agregar el nuevo método. Empiece por indicar que nuestro método DAL usará una instrucción SQL ad hoc.

[![crear el método DAL mediante una instrucción SQL ad hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: creación del método Dal mediante una instrucción SQL ad hoc ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))

A continuación, el asistente nos solicitará el tipo de consulta que se va a crear. Dado que el método `DiscontinueAllProductsForSupplier(supplierID)` deberá actualizar la tabla de base de datos `Products`, estableciendo el campo `Discontinued` en 1 para todos los productos proporcionados por el *`supplierID`* especificado, es necesario crear una consulta que actualice los datos.

[![elegir el tipo de consulta UPDATE](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: elegir el tipo de consulta de actualización ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))

La siguiente pantalla del asistente proporciona la instrucción de `UPDATE` de TableAdapter s existente, que actualiza cada uno de los campos definidos en el `Products` DataTable. Reemplace el texto de la consulta por la siguiente instrucción:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Después de escribir esta consulta y hacer clic en siguiente, la última pantalla del asistente solicita el nombre de método nuevo use `DiscontinueAllProductsForSupplier`. Para completar el asistente, haga clic en el botón Finalizar. Al volver al diseñador de DataSet, debería ver un nuevo método en el `ProductsTableAdapter` denominado `DiscontinueAllProductsForSupplier(@SupplierID)`.

[![nombre al nuevo método DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nombre del nuevo método Dal `DiscontinueAllProductsForSupplier` ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))

Con el método `DiscontinueAllProductsForSupplier(supplierID)` creado en la capa de acceso a datos, la siguiente tarea consiste en crear el método de `DiscontinueAllProductsForSupplier(supplierID)` en el nivel de lógica de negocios. Para ello, abra el archivo de clase `ProductsBLL` y agregue lo siguiente:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Este método simplemente llama al método `DiscontinueAllProductsForSupplier(supplierID)` de la capa DAL, pasando a lo largo del valor del parámetro *`supplierID`* proporcionado. Si hay alguna regla de negocios que solo permitiera que los productos de un proveedor se dessigan en determinadas circunstancias, esas reglas se deben implementar aquí, en el BLL.

> [!NOTE]
> A diferencia de las sobrecargas de `UpdateProduct` en la clase `ProductsBLL`, la firma del método `DiscontinueAllProductsForSupplier(supplierID)` no incluye el atributo `DataObjectMethodAttribute` (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Esto impide el método de `DiscontinueAllProductsForSupplier(supplierID)` de la lista desplegable Asistente para configurar orígenes de datos de ObjectDataSource s en la pestaña actualizar. He omitido este atributo porque llamaremos al método `DiscontinueAllProductsForSupplier(supplierID)` directamente desde un controlador de eventos en nuestra página de ASP.NET.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Paso 5: agregar un botón Discontinue All Products a FormView

Con el método `DiscontinueAllProductsForSupplier(supplierID)` en BLL y DAL completado, el último paso para agregar la capacidad de interrumpir todos los productos para el proveedor seleccionado es agregar un control Web Button al `ItemTemplate`FormView s. Permite agregar un botón de este tipo debajo del número de teléfono del proveedor con el texto del botón, interrumpir todos los productos y un valor de propiedad `ID` de `DiscontinueAllProductsForSupplier`. Puede Agregar este control Web de botón a través del diseñador haciendo clic en el vínculo editar plantillas de la etiqueta inteligente FormView s (vea la figura 15), o directamente a través de la sintaxis declarativa.

[![agregar un control Web del botón Discontinue All Products a FormView s ItemTemplate](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: agregar un control Web del botón Discontinue All Products al `ItemTemplate` FormView s ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))

Cuando un usuario que visita la página hace clic en el botón, se produce un postback y se desencadena el evento FormView s [`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) . Para ejecutar código personalizado en respuesta a que se haga clic en este botón, podemos crear un controlador de eventos para este evento. Sin embargo, comprenda que el evento `ItemCommand` se activa cada vez que se hace clic en *un* control Web Button, LinkButton o ImageButton en FormView. Esto significa que cuando el usuario se mueve de una página a otra en FormView, se desencadena el evento `ItemCommand`; lo mismo sucede cuando el usuario hace clic en nuevo, editar o eliminar en un FormView que admite la inserción, actualización o eliminación.

Dado que el `ItemCommand` se activa independientemente del botón en el que se haga clic, en el controlador de eventos se necesita una manera de determinar si se hizo clic en el botón Discontinue All Products (interrumpir todos los productos) o si era otro botón. Para ello, podemos establecer la propiedad Button web control s `CommandName` en algún valor de identificación. Al hacer clic en el botón, este valor `CommandName` se pasa al controlador de eventos `ItemCommand`, lo que nos permite determinar si se hizo clic en el botón Discontinue All Products. Establezca la propiedad Discontinue All Products Button s `CommandName` en DiscontinueProducts.

Por último, vamos a usar un cuadro de diálogo de confirmación del lado cliente para asegurarse de que el usuario realmente desea dejar de utilizar los productos de proveedor seleccionados. Como vimos en el tutorial de adición de la [confirmación del cliente al eliminar](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) , esto se puede realizar con un poco de JavaScript. En concreto, establezca la propiedad control Web Button s OnClientClick en `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Después de realizar estos cambios, la sintaxis declarativa de FormView s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

A continuación, cree un controlador de eventos para el evento FormView s `ItemCommand`. En este controlador de eventos, es necesario determinar primero si se ha hecho clic en el botón Discontinue All Products. Si es así, queremos crear una instancia de la clase `ProductsBLL` e invocar su método `DiscontinueAllProductsForSupplier(supplierID)`, pasando el `SupplierID` del objeto FormView seleccionado:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Tenga en cuenta que se puede tener acceso al `SupplierID` del proveedor seleccionado actualmente en FormView mediante la propiedad FormView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). La propiedad `SelectedValue` devuelve el primer valor de clave de datos para el registro que se muestra en FormView. La propiedad FormView s [`DataKeyNames`](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), que indica los campos de datos de los que se extraen los valores de clave de datos, se estableció automáticamente en `SupplierID` de Visual Studio al enlazar el origen ObjectDataSource al FormView en el paso 2.

Con el controlador de eventos `ItemCommand` creado, dedique un momento a probar la página. Vaya al proveedor de cooperativa de quesos ' las cabras ' (es decir, el quinto proveedor del FormView para mí). Este proveedor proporciona dos productos, queso Cabrales y queso manchego la Pastora, que *no* se han suspendido.

Imagine que cooperativa de quesos ' las cabras ' ha salido del negocio y, por lo tanto, sus productos se van a dejar de usar. Haga clic en el botón Discontinue All Products. Se mostrará el cuadro de diálogo de confirmación del lado cliente (vea la figura 16).

[![cooperativa de quesos las cabras proporciona dos productos activos](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: Cooperativa de quesos las cabras proporciona dos productos activos ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))

Si hace clic en aceptar en el cuadro de diálogo confirmar del lado cliente, el envío del formulario continuará, lo que producirá un postback en el que se activará el evento FormView s `ItemCommand`. A continuación, el controlador de eventos que hemos creado se ejecutará, invocará el método `DiscontinueAllProductsForSupplier(supplierID)` y descontinuará los productos queso Cabrales y queso manchego la Pastora.

Si ha deshabilitado el estado de vista de GridView, el control GridView se está reenlazando al almacén de datos subyacente en cada postback y, por tanto, se actualizará inmediatamente para reflejar que estos dos productos se han suspendido (consulte la figura 17). Sin embargo, si no ha deshabilitado el estado de vista en el control GridView, tendrá que volver a enlazar manualmente los datos a GridView después de realizar este cambio. Para ello, basta con realizar una llamada al método GridView s `DataBind()` inmediatamente después de invocar el método `DiscontinueAllProductsForSupplier(supplierID)`.

[![después de hacer clic en el botón Discontinue All Products, los productos de los proveedores se actualizan en consecuencia](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: después de hacer clic en el botón Discontinue All Products, los productos de proveedores se actualizan según corresponda ([haga clic para ver la imagen a tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Paso 6: crear una sobrecarga de UpdateProduct en el nivel de lógica de negocios para ajustar un precio de producto

Al igual que con el botón Discontinue All Products en FormView, para agregar botones para aumentar y disminuir el precio de un producto en GridView, primero es necesario agregar los métodos de capa de acceso a datos y de nivel de lógica de negocios adecuados. Dado que ya tenemos un método que actualiza una sola fila de producto en la capa DAL, podemos proporcionar esta funcionalidad mediante la creación de una nueva sobrecarga para el método `UpdateProduct` en el BLL.

Nuestras últimas sobrecargas `UpdateProduct` han tomado en alguna combinación de campos Product como valores de entrada escalares y, a continuación, han actualizado solo esos campos para el producto especificado. En esta sobrecarga, cambiaremos ligeramente de este estándar y, en su lugar, pasaremos el `ProductID` del producto y el porcentaje por el que ajustar el `UnitPrice` (en lugar de pasar la nueva `UnitPrice` ajustada). Este enfoque simplificará el código que se debe escribir en la clase de código subyacente de la página ASP.NET, ya que no es necesario molestarnos en la determinación de los `UnitPrice`actuales del producto.

A continuación se muestra la sobrecarga de `UpdateProduct` de este tutorial:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Esta sobrecarga recupera información sobre el producto especificado a través del método de `GetProductByProductID(productID)` DAL s. A continuación, comprueba si el producto s `UnitPrice` tiene asignado un valor de `NULL` de base de datos. Si es así, el precio se deja sin modificar. Sin embargo, si hay un valor de `UnitPrice` que no es de`NULL`, el método actualiza las `UnitPrice` del producto según el porcentaje especificado (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Paso 7: agregar los botones aumentar y disminuir a GridView

GridView (y DetailsView) se compone de una colección de campos. Además de BoundFields, CheckBoxFields y TemplateFields, ASP.NET incluye ButtonField, que, como su nombre implica, se representa como una columna con un botón, LinkButton o ImageButton para cada fila. Similar a FormView, al hacer clic en *cualquier* botón de los botones de paginación de GridView, botones de edición o eliminación, botones de ordenación, etc., se produce un postback y se genera el [evento`RowCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)de GridView.

ButtonField tiene una propiedad `CommandName` que asigna el valor especificado a cada uno de sus botones `CommandName` propiedades. Al igual que con FormView, el controlador de eventos `RowCommand` usa el valor `CommandName` para determinar en qué botón se hizo clic.

Vamos a agregar dos nuevos ButtonFields a GridView, uno con un botón de texto precio + 10% y otro con el precio del texto-10%. Para agregar estos ButtonFields, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView s, seleccione el tipo de campo ButtonField en la lista de la parte superior izquierda y haga clic en el botón Agregar.

![Agregar dos ButtonFields a GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: adición de dos ButtonFields a GridView

Mueva los dos ButtonFields para que aparezcan como los dos primeros campos de GridView. A continuación, establezca las propiedades de `Text` de estos dos ButtonFields en Price + 10% y Price-10% y las propiedades `CommandName` en IncreasePrice y DecreasePrice, respectivamente. De forma predeterminada, un ButtonField representa su columna de botones como LinkButtons. Sin embargo, esto se puede cambiar a través de la [propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)ButtonField s`ButtonType`. Permita que los dos ButtonFields se representen como botones de instalación normales; por lo tanto, establezca la propiedad `ButtonType` en `Button`. En la figura 19 se muestra el cuadro de diálogo campos una vez realizados estos cambios; a continuación se encuentra el marcado declarativo de GridView s.

![Configurar las propiedades Text, CommandName y ButtonType de ButtonFields](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configuración de las propiedades ButtonFields `Text`, `CommandName`y `ButtonType`

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Con estos ButtonFields creados, el último paso es crear un controlador de eventos para el evento GridView s `RowCommand`. Este controlador de eventos, si se activa porque se hizo clic en los botones precio + 10% o precio-10%, necesita determinar el `ProductID` de la fila cuyo botón se hizo clic y, a continuación, invocar el método de `UpdateProduct` de `ProductsBLL` clase, pasando el ajuste de porcentaje de `UnitPrice` adecuado junto con el `ProductID`. El siguiente código realiza estas tareas:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Con el fin de determinar el `ProductID` de la fila cuyo botón de precio + 10% o precio-10% se hizo clic, es necesario consultar la colección de `DataKeys` de GridView. Esta colección contiene los valores de los campos especificados en la propiedad `DataKeyNames` de cada fila de GridView. Como la propiedad GridView s `DataKeyNames` se estableció en ProductID mediante Visual Studio al enlazar ObjectDataSource a GridView, `DataKeys(rowIndex).Value` proporciona el `ProductID` para el *rowIndex*especificado.

ButtonField pasa automáticamente el *rowIndex* de la fila cuyo botón se hizo clic a través del parámetro `e.CommandArgument`. Por lo tanto, para determinar el `ProductID` de la fila cuyo botón de precio + 10% o precio-10% se hizo clic, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Al igual que con el botón Discontinue All Products, si ha deshabilitado el estado de vista de GridView, el control GridView se está reenlazando al almacén de datos subyacente en cada postback y, por tanto, se actualizará inmediatamente para reflejar un cambio de precio que se produce al hacer clic en cualquiera de los botones. Sin embargo, si no ha deshabilitado el estado de vista en el control GridView, tendrá que volver a enlazar manualmente los datos a GridView después de realizar este cambio. Para ello, basta con realizar una llamada al método GridView s `DataBind()` inmediatamente después de invocar el método `UpdateProduct`.

La figura 20 muestra la página al ver los productos proporcionados por Homestead de abuela Kelly. En la figura 21 se muestran los resultados una vez que se ha hecho clic dos veces en el botón Price + 10% para Boysenberry spread de abuela y el botón Price-10% una vez para la salsa Northwoods Cranberry.

[![GridView incluye los botones precio + 10% y precio-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: el control GridView incluye los botones precio + 10% y precio-10% ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))

[![los precios del primer y el tercer producto se han actualizado a través de los botones precio + 10% y precio-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: los precios del primer y el tercer producto se han actualizado a través de los botones precio + 10% y precio-10% ([haga clic para ver la imagen de tamaño completo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))

> [!NOTE]
> GridView (y DetailsView) también puede tener botones, LinkButtons o ImageButtons agregados a su TemplateFields. Al igual que con el BoundField, estos botones, cuando se hace clic en ellos, provocarán un postback, lo que provocará el evento `RowCommand` de GridView. Sin embargo, al agregar botones en TemplateField, el botón s `CommandArgument` no se establece automáticamente en el índice de la fila como cuando se usa ButtonFields. Si necesita determinar el índice de fila del botón en el que se hizo clic en el controlador de eventos `RowCommand`, deberá establecer manualmente la propiedad Button s `CommandArgument` en su sintaxis declarativa dentro de TemplateField, mediante código como el siguiente:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.

## <a name="summary"></a>Resumen

Los controles GridView, DetailsView y FormView pueden incluir botones, LinkButtons o ImageButtons. Tales botones, cuando se hace clic en ellos, producen un postback y generan el evento `ItemCommand` en los controles FormView y DetailsView y el evento `RowCommand` en GridView. Estos controles Web de datos tienen funcionalidad integrada para controlar las acciones comunes relacionadas con los comandos, como eliminar o editar registros. Sin embargo, también podemos usar botones que, al hacer clic en ellos, respondan con la ejecución de nuestro propio código personalizado.

Para ello, es necesario crear un controlador de eventos para el evento `ItemCommand` o `RowCommand`. En este controlador de eventos, primero comprobamos el valor de `CommandName` entrante para determinar en qué botón se hizo clic y, a continuación, realizar la acción personalizada adecuada. En este tutorial vimos cómo usar los botones y ButtonFields para interrumpir todos los productos de un proveedor especificado o para aumentar o disminuir el precio de un producto determinado en un 10%.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-and-responding-to-buttons-to-a-gridview-cs.md)
