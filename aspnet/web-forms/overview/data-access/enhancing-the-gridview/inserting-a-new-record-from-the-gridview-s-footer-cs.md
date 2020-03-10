---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Insertar un nuevo registro desde el pie de página de GridView (C#) | Microsoft Docs
author: rick-anderson
description: Aunque el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, en este tutorial se muestra cómo aumentar el GridView para incluir un...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b337fe395a0ed54b03767111e73ea6d6c0ba23a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78477529"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Insertar un nuevo registro desde el pie de página de GridView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) de la aplicación de ejemplo o [descarga de PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Aunque el control GridView no proporciona compatibilidad integrada para insertar un nuevo registro de datos, en este tutorial se muestra cómo aumentar el GridView para incluir una interfaz de inserción.

## <a name="introduction"></a>Introducción

Como se describe en [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , los controles Web GridView, DetailsView y FormView incluyen funciones de modificación de datos integradas. Cuando se usa con controles de origen de datos declarativos, estos tres controles Web se pueden configurar de forma rápida y sencilla para modificar los datos y en escenarios sin necesidad de escribir una sola línea de código. Desafortunadamente, solo los controles DetailsView y FormView proporcionan capacidades integradas de inserción, edición y eliminación. GridView solo ofrece compatibilidad para editar y eliminar. Sin embargo, con un pequeño grasa angular, podemos aumentar el GridView para incluir una interfaz de inserción.

Al agregar funcionalidades de inserción a GridView, somos responsables de decidir cómo se agregarán los nuevos registros, crear la interfaz de inserción y escribir el código para insertar el nuevo registro. En este tutorial, veremos cómo agregar la interfaz de inserción a la fila de pie de página de GridView (vea la ilustración 1). La celda de pie de página de cada columna incluye el elemento de interfaz de usuario de recopilación de datos adecuado (un cuadro de texto para el nombre del producto, un DropDownList para el proveedor, etc.). También se necesita una columna para un botón Agregar que, al hacer clic en él, producirá un postback e insertará un nuevo registro en la tabla de `Products` con los valores proporcionados en la fila de pie de página.

[![la fila de pie de página proporciona una interfaz para agregar nuevos productos](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: la fila de pie de página proporciona una interfaz para agregar nuevos productos ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Paso 1: Mostrar información del producto en un control GridView

Antes de preocuparnos por la creación de la interfaz de inserción en el pie de página de GridView, vamos a centrarse primero en agregar un control GridView a la página que muestra los productos de la base de datos. Comience abriendo la página `InsertThroughFooter.aspx` en la carpeta `EnhancedGridView` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, estableciendo la propiedad GridView s `ID` en `Products`. A continuación, use la etiqueta inteligente de GridView s para enlazarla a un nuevo ObjectDataSource denominado `ProductsDataSource`.

[![crear un nuevo ObjectDataSource denominado ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Figura 2**: creación de un nuevo ObjectDataSource denominado `ProductsDataSource` ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))

Configure ObjectDataSource para usar el método `ProductsBLL` Class s `GetProducts()` para recuperar la información del producto. En este tutorial, vamos a centrarse estrictamente en agregar funcionalidades de inserción y no preocuparse por la edición y la eliminación. Por lo tanto, asegúrese de que la lista desplegable de la pestaña insertar esté establecida en `AddProduct()` y que las listas desplegables de las pestañas actualizar y eliminar estén establecidas en (ninguno).

[![asignar el método AddProduct al método ObjectDataSource Insert ()](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Figura 3**: asignación del método `AddProduct` al método ObjectDataSource s `Insert()` ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))

[![establecer las listas desplegables de las pestañas actualizar y eliminar en (ninguna)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Figura 4**: establecimiento de las listas desplegables de las pestañas actualizar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))

Después de completar el Asistente para configurar origen de datos de ObjectDataSource, Visual Studio agregará automáticamente campos a GridView para cada uno de los campos de datos correspondientes. Por ahora, deje todos los campos agregados por Visual Studio. Más adelante en este tutorial, volveremos a quitar algunos de los campos cuyos valores no se deben especificar al agregar un nuevo registro.

Puesto que hay cerca de 80 productos en la base de datos, el usuario tendrá que desplazarse hasta la parte inferior de la página web para agregar un nuevo registro. Por lo tanto, permita que la paginación permita que la interfaz de inserción sea más visible y accesible. Para activar la paginación, active la casilla habilitar paginación en la etiqueta inteligente de GridView s.

En este momento, el marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]

[![todos los campos de datos del producto se muestran en un control GridView paginado](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Figura 5**: todos los campos de datos de producto se muestran en un control GridView paginado ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Paso 2: agregar una fila de pie de página

Junto con el encabezado y las filas de datos, GridView incluye una fila de pie de página. Las filas de encabezado y de pie de página se muestran en función de los valores de las propiedades [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) y [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) de GridView. Para mostrar la fila de pie de página, basta con establecer la propiedad `ShowFooter` en `true`. Como se muestra en la figura 6, al establecer la propiedad `ShowFooter` en `true` se agrega una fila de pie de página a la cuadrícula.

[![para mostrar la fila de pie de página, establezca ShowFooter en true.](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Figura 6**: para mostrar la fila de pie de página, establezca `ShowFooter` en `True` ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))

Tenga en cuenta que la fila de pie de página tiene un color de fondo rojo oscuro. Esto se debe al tema DataWebControls que hemos creado y aplicado a todas las páginas en el tutorial de [visualización de datos con el diseño ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) . En concreto, el archivo `GridView.skin` configura la propiedad `FooterStyle` de modo que utiliza la clase `FooterStyle` CSS. La clase `FooterStyle` se define en `Styles.css` de la siguiente manera:

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Hemos explorado el uso de la fila de pie de página de GridView en los tutoriales anteriores. Si es necesario, consulte la [información de resumen que se muestra en el](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) tutorial de pie de página de GridView para obtener un actualizador.

Después de establecer la propiedad `ShowFooter` en `true`, dedique un momento a ver el resultado en un explorador. Actualmente, la fila de pie de página no contiene ningún texto o control Web. En el paso 3 modificaremos el pie de página de cada campo de GridView para que incluya la interfaz de inserción adecuada.

[![se muestra la fila de pie de página vacía encima de los controles de la interfaz de paginación](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Figura 7**: se muestra la fila de pie de página vacía encima de los controles de la interfaz de paginación ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Paso 3: personalización de la fila de pie de página

De nuevo en el tutorial [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) , vimos cómo personalizar en gran medida la presentación de una columna de GridView determinada con TemplateFields (en lugar de BoundFields o CheckBoxFields). en [la personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) , hemos examinado el uso de TemplateFields para personalizar la interfaz de edición en un control GridView. Recuerde que un TemplateField se compone de un número de plantillas que define la combinación de marcado, controles Web y sintaxis de enlace de los tipos de filas que se usan para determinados tipos de filas. El `ItemTemplate`, por ejemplo, especifica la plantilla que se utiliza para las filas de solo lectura, mientras que la `EditItemTemplate` define la plantilla para la fila modificable.

Junto con los `ItemTemplate` y `EditItemTemplate`, TemplateField también incluye un `FooterTemplate` que especifica el contenido de la fila de pie de página. Por lo tanto, podemos agregar los controles Web necesarios para cada campo que inserte la interfaz en el `FooterTemplate`. Para empezar, convierta todos los campos de GridView a TemplateFields. Para ello, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView s, seleccione cada campo en la esquina inferior izquierda y haga clic en el vínculo convertir este campo en un TemplateField.

![Convertir cada campo en TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Figura 8**: conversión de cada campo en TemplateField

Al hacer clic en la conversión de este campo en TemplateField, el tipo de campo actual se convierte en un TemplateField equivalente. Por ejemplo, cada BoundField se reemplaza por TemplateField con una `ItemTemplate` que contiene una etiqueta que muestra el campo de datos correspondiente y una `EditItemTemplate` que muestra el campo de datos en un cuadro de texto. El `ProductName` BoundField se ha convertido en el siguiente marcado de TemplateField:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Del mismo modo, el `Discontinued` CheckBoxField se ha convertido en TemplateField, cuyos `ItemTemplate` y `EditItemTemplate` contienen un control Web CheckBox (con la casilla `ItemTemplate` s deshabilitada). El `ProductID` de solo lectura que se ha convertido en TemplateField con un control etiqueta en el `ItemTemplate` y `EditItemTemplate`. En Resumen, convertir un campo de GridView existente en un TemplateField es una forma rápida y sencilla de cambiar a TemplateField más personalizable sin perder ninguna de las funciones de campo existentes.

Dado que el control GridView con el que trabajamos no admite la edición, no dude en quitar el `EditItemTemplate` de cada TemplateField, lo que solo se mantiene el `ItemTemplate`. Después de hacerlo, el marcado declarativo de GridView debe ser similar al siguiente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Ahora que cada campo de GridView se ha convertido en TemplateField, podemos especificar la interfaz de inserción adecuada en cada campo `FooterTemplate`. Algunos de los campos no tendrán una interfaz de inserción (`ProductID`, por ejemplo); otros variarán en los controles Web que se usan para recopilar la nueva información del producto.

Para crear la interfaz de edición, elija el vínculo editar plantillas de la etiqueta inteligente de GridView s. A continuación, en la lista desplegable, seleccione el campo apropiado `FooterTemplate` y arrastre el control correspondiente desde el cuadro de herramientas hasta el diseñador.

[![agregar la interfaz de inserción adecuada a cada campo FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Figura 9**: agregar la interfaz de inserción adecuada a cada campo `FooterTemplate` ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))

La siguiente lista con viñetas enumera los campos de GridView, especificando la interfaz de inserción que se va a agregar:

- `ProductID` ninguno.
- `ProductName` agregue un cuadro de texto y establezca su `ID` en `NewProductName`. Agregue también un control RequiredFieldValidator para asegurarse de que el usuario escribe un valor para el nuevo nombre del producto.
- `SupplierID` ninguno.
- `CategoryID` ninguno.
- `QuantityPerUnit` agregar un cuadro de texto y establecer su `ID` en `NewQuantityPerUnit`.
- `UnitPrice` agregar un cuadro de texto denominado `NewUnitPrice` y un control CompareValidator que garantice que el valor especificado es un valor de moneda mayor o igual que cero.
- `UnitsInStock` usar un cuadro de texto cuyo `ID` está establecido en `NewUnitsInStock`. Incluya un CompareValidator que garantice que el valor especificado es un valor entero mayor o igual que cero.
- `UnitsOnOrder` usar un cuadro de texto cuyo `ID` está establecido en `NewUnitsOnOrder`. Incluya un CompareValidator que garantice que el valor especificado es un valor entero mayor o igual que cero.
- `ReorderLevel` usar un cuadro de texto cuyo `ID` está establecido en `NewReorderLevel`. Incluya un CompareValidator que garantice que el valor especificado es un valor entero mayor o igual que cero.
- `Discontinued` agregar una casilla y establecer su `ID` en `NewDiscontinued`.
- `CategoryName` agregue un DropDownList y establezca su `ID` en `NewCategoryID`. Enlácelo a un nuevo ObjectDataSource denominado `CategoriesDataSource` y configúrelo para que use el método de `GetCategories()` de `CategoriesBLL` Class s. Haga que los `ListItem` de DropDownList muestren el campo de datos `CategoryName`, mediante el campo de datos `CategoryID` como sus valores.
- `SupplierName` agregue un DropDownList y establezca su `ID` en `NewSupplierID`. Enlácelo a un nuevo ObjectDataSource denominado `SuppliersDataSource` y configúrelo para que use el método de `GetSuppliers()` de `SuppliersBLL` Class s. Haga que los `ListItem` de DropDownList muestren el campo de datos `CompanyName`, mediante el campo de datos `SupplierID` como sus valores.

Para cada uno de los controles de validación, desactive la propiedad `ForeColor` de modo que se use el color de primer plano blanco de la clase CSS del `FooterStyle` en lugar del rojo predeterminado. Utilice también la propiedad `ErrorMessage` para obtener una descripción detallada, pero establezca la propiedad `Text` en un asterisco. Para evitar que el texto del control de validación provoque que la interfaz de inserción se ajuste a dos líneas, establezca la propiedad `FooterStyle` s `Wrap` en false para cada una de las `FooterTemplate` que usan un control de validación. Por último, agregue un control ValidationSummary debajo de GridView y establezca su propiedad `ShowMessageBox` en `true` y su propiedad `ShowSummary` en `false`.

Al agregar un nuevo producto, es necesario proporcionar el `CategoryID` y `SupplierID`. Esta información se captura a través de DropDownLists en las celdas de pie de página de los campos `CategoryName` y `SupplierName`. Elegí usar estos campos en lugar de los `CategoryID` y `SupplierID` TemplateFields porque, en las filas de datos de la cuadrícula, es probable que el usuario le interese ver la categoría y los nombres de proveedor en lugar de sus valores de identificador. Puesto que los valores de `CategoryID` y `SupplierID` se están capturando en las interfaces `CategoryName` y `SupplierName` Field s Inserting, podemos quitar el `CategoryID` y `SupplierID` TemplateFields de GridView.

Del mismo modo, el `ProductID` no se usa al agregar un nuevo producto, por lo que también se puede quitar el `ProductID` TemplateField. Sin embargo, deje el campo `ProductID` en la cuadrícula. Además de los cuadros de texto, DropDownLists, casillas y controles de validación que componen la interfaz de inserción, también necesitaremos un botón Agregar que, al hacer clic en él, realice la lógica para agregar el nuevo producto a la base de datos. En el paso 4 incluiremos un botón Agregar en la interfaz de inserción en el `ProductID` TemplateField s `FooterTemplate`.

No dude en mejorar la apariencia de los distintos campos de GridView. Por ejemplo, puede que desee dar formato de moneda a los valores `UnitPrice`, alinear a la derecha los campos `UnitsInStock`, `UnitsOnOrder`y `ReorderLevel`, y actualizar los valores `HeaderText` para TemplateFields.

Después de crear el paso de insertar interfaces en el `FooterTemplate`, quitar el `SupplierID`y `CategoryID` TemplateFields y mejorar la estética de la cuadrícula mediante el formato y la alineación del TemplateFields, el marcado declarativo de GridView debe ser similar al siguiente:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Cuando se ve a través de un explorador, la fila de pie de página de GridView ahora incluye la interfaz de inserción completada (vea la figura 10). En este momento, la interfaz de inserción no incluye un medio para que el usuario indique que ha introducido los datos para el nuevo producto y desea insertar un nuevo registro en la base de datos. Además, todavía vamos a abordar cómo se traducirán los datos introducidos en el pie de página en un nuevo registro en la base de datos de `Products`. En el paso 4 veremos cómo incluir un botón Agregar en la interfaz de inserción y cómo ejecutar el código en el PostBack cuando se hace clic en él. En el paso 5 se muestra cómo insertar un nuevo registro con los datos del pie de página.

[![el pie de página de GridView proporciona una interfaz para agregar un nuevo registro](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Figura 10**: el pie de página de GridView proporciona una interfaz para agregar un nuevo registro ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Paso 4: incluir un botón Agregar en la interfaz de inserción

Necesitamos incluir un botón Agregar en algún lugar de la interfaz de inserción, ya que la interfaz de pie de página de la fila de pie de página no tiene los medios para que el usuario indique que ha completado la entrada de la nueva información de productos. Esto podría colocarse en uno de los `FooterTemplate` existentes o podríamos agregar una columna nueva a la cuadrícula para este propósito. En este tutorial, vamos a colocar el botón Agregar en el `ProductID` TemplateField s `FooterTemplate`.

En el diseñador, haga clic en el vínculo editar plantillas de la etiqueta inteligente de GridView s y, a continuación, elija el `ProductID` campo s `FooterTemplate` en la lista desplegable. Agregue un control Web Button (o LinkButton o ImageButton, si lo prefiere) a la plantilla, estableciendo su identificador en `AddProduct`, su `CommandName` para insertar y su propiedad `Text` para agregar como se muestra en la figura 11.

[![Coloque el botón Agregar en el ProductID TemplateField s FooterTemplate](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Figura 11**: Coloque el botón Agregar en el `ProductID` TemplateField s `FooterTemplate` ([haga clic para ver la imagen a tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))

Una vez que haya incluido el botón Agregar, pruebe la página en un explorador. Tenga en cuenta que al hacer clic en el botón Agregar con datos no válidos en la interfaz de inserción, el PostBack se cortocircuita y el control ValidationSummary indica los datos no válidos (vea la figura 12). Con los datos adecuados especificados, al hacer clic en el botón Agregar se produce un postback. Sin embargo, no se agrega ningún registro a la base de datos. Tendremos que escribir un poco de código para realizar realmente la inserción.

[![se cortocircuita el PostBack del botón Agregar en caso de que haya datos no válidos en la interfaz de inserción](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Figura 12**: el PostBack del botón Agregar se cortocircuita si hay datos no válidos en la interfaz de inserción ([haga clic para ver la imagen de tamaño completo](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))

> [!NOTE]
> Los controles de validación de la interfaz de inserción no se asignaron a un grupo de validación. Esto funciona bien siempre y cuando la interfaz de inserción sea el único conjunto de controles de validación de la página. Sin embargo, si hay otros controles de validación en la página (por ejemplo, los controles de validación en la interfaz de edición de la cuadrícula), se debe asignar el mismo valor a los controles de validación de la interfaz de inserción y agregar el botón `ValidationGroup` propiedades para asociar estos controles a un grupo de validaciones determinado. Vea [dessección de los controles de validación en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obtener más información sobre la creación de particiones de los controles de validación y los botones de una página en los grupos de validación.

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Paso 5: insertar un nuevo registro en la tabla`Products`

Al usar las características de edición integradas de GridView, GridView controla automáticamente todo el trabajo necesario para realizar la actualización. En concreto, cuando se hace clic en el botón actualizar, copia los valores especificados desde la interfaz de edición a los parámetros de la colección de `UpdateParameters` de origen de datos de origen e inicia la actualización mediante la invocación del método ObjectDataSource s `Update()`. Dado que GridView no proporciona la funcionalidad integrada para la inserción, debemos implementar código que llame al método de `Insert()` ObjectDataSource s y copie los valores de la interfaz de inserción en la colección de `InsertParameters` de ObjectDataSource.

Esta lógica de inserción se debe ejecutar después de hacer clic en el botón Agregar. Tal y como se describe en los [botones agregar y responder a un tutorial de GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) , cada vez que se hace clic en un botón, LinkButton o ImageButton en un control GridView, se desencadena el evento gridview s `RowCommand` en el PostBack. Este evento se desencadena si el botón, el control LinkButton o el control ImageButton se agregaron explícitamente, como el botón Agregar de la fila de pie de página o si el control GridView lo agregó automáticamente (por ejemplo, el LinkButtons en la parte superior de cada columna cuando se selecciona habilitar ordenación, o bien el LinkButtons en la interfaz de paginación cuando está seleccionada la opción Habilitar paginación).

Por lo tanto, para responder al usuario haciendo clic en el botón Agregar, es necesario crear un controlador de eventos para el evento GridView s `RowCommand`. Dado que este evento se activa siempre que se hace clic en *un* botón, LinkButton o ImageButton en GridView, es vital que solo avancemos con la lógica de inserción si la `CommandName` propiedad que se pasa al controlador de eventos se asigna al valor `CommandName` del botón Agregar (Insert). Además, también deberíamos continuar si los controles de validación notifican datos válidos. Para ello, cree un controlador de eventos para el evento `RowCommand` con el código siguiente:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Es posible que se pregunte por qué el controlador de eventos comprueba la propiedad `Page.IsValid`. Después de All, no se suprimirá el PostBack si se proporcionan datos no válidos en la interfaz de inserción. Esta suposición es correcta siempre que el usuario no haya deshabilitado JavaScript o haya realizado pasos para eludir la lógica de validación del lado cliente. En Resumen, nunca debe confiar estrictamente en la validación del lado cliente; siempre debe realizarse una comprobación del lado del servidor para comprobar la validez antes de trabajar con los datos.

En el paso 1, creamos el `ProductsDataSource` ObjectDataSource de tal forma que su método de `Insert()` está asignado al método de la clase `ProductsBLL` `AddProduct`. Para insertar el nuevo registro en la tabla `Products`, simplemente se puede invocar el método ObjectDataSource s `Insert()`:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Ahora que se ha invocado el método de `Insert()`, lo único que queda es copiar los valores de la interfaz de inserción en los parámetros pasados al método `AddProduct` de la clase `ProductsBLL`. Como hemos visto en el tutorial [examen de los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , esto se puede realizar a través del evento de `Inserting` de datos de origen de datos. En el evento `Inserting` es necesario hacer referencia mediante programación a los controles de la fila de pie de página de `Products` GridView s y asignar sus valores a la colección de `e.InputParameters`. Si el usuario omite un valor como dejar el cuadro de texto `ReorderLevel` en blanco, es necesario especificar que el valor insertado en la base de datos debe ser `NULL`. Dado que el método `AddProducts` acepta tipos que aceptan valores NULL para los campos de base de datos que aceptan valores NULL, basta con usar un tipo que acepta valores NULL y establecer su valor en `null` en el caso de que se omita la entrada del usuario.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Con el controlador de eventos `Inserting` completado, se pueden agregar nuevos registros a la tabla de base de datos de `Products` a través de la fila de pie de página de GridView. Continúe y pruebe a agregar varios productos nuevos.

## <a name="enhancing-and-customizing-the-add-operation"></a>Mejorar y personalizar la operación de agregar

Actualmente, al hacer clic en el botón Agregar se agrega un nuevo registro a la tabla de base de datos, pero no se proporciona ningún tipo de comentario visual de que el registro se haya agregado correctamente. Lo ideal es que un cuadro de alerta del lado cliente o un control Web de etiqueta informe al usuario de que su inserción se ha completado correctamente. Dejo esto como un ejercicio para el lector.

El GridView usado en este tutorial no aplica ningún criterio de ordenación a los productos de la lista, ni permite al usuario final ordenar los datos. Por lo tanto, los registros se ordenan tal como están en la base de datos por su campo de clave principal. Puesto que cada nuevo registro tiene un valor `ProductID` mayor que el último, cada vez que se agrega un nuevo producto, se agrega al final de la cuadrícula. Por lo tanto, puede que desee enviar automáticamente el usuario a la última página de GridView después de agregar un nuevo registro. Esto se puede lograr agregando la siguiente línea de código después de la llamada a `ProductsDataSource.Insert()` en el controlador de eventos `RowCommand` para indicar que el usuario debe enviarse a la última página después de enlazar los datos a GridView:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` es una variable booleana de nivel de página a la que se asigna inicialmente un valor de `false`. En el controlador de eventos GridView s `DataBound`, si `SendUserToLastPage` es false, se actualiza la propiedad `PageIndex` para enviar al usuario a la última página.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

La razón por la que se establece la propiedad `PageIndex` en el controlador de eventos `DataBound` (en lugar del controlador de eventos `RowCommand`) es porque cuando el controlador de eventos `RowCommand` se activa, todavía se agrega el nuevo registro a la tabla de base de datos `Products`. Por lo tanto, en el controlador de eventos `RowCommand` el índice de la última página (`PageCount - 1`) representa el último índice de la página *antes* de que se haya agregado el nuevo producto. Para la mayoría de los productos que se van a agregar, el índice de la última página es el mismo después de agregar el nuevo producto. Pero cuando el producto agregado da como resultado un nuevo índice de última página, si actualizamos incorrectamente el `PageIndex` en el controlador de eventos de `RowCommand`, se le dirigirá a la página de la segunda a la última (el índice de la última página antes de agregar el nuevo producto) en lugar de al nuevo índice de la página anterior. Puesto que el controlador de eventos `DataBound` se activa después de agregar el nuevo producto y de volver a enlazar los datos a la cuadrícula, al establecer la propiedad `PageIndex`, sabemos que se va a obtener el índice de la última página correcto.

Por último, la GridView usada en este tutorial es bastante extensa debido al número de campos que se deben recopilar para agregar un nuevo producto. Debido a este ancho, podría ser preferible un diseño vertical de DetailsView. El ancho total de GridView se puede reducir mediante la recopilación de menos entradas. Es posible que no necesitemos recopilar los campos `UnitsOnOrder`, `UnitsInStock`y `ReorderLevel` al agregar un nuevo producto, en cuyo caso estos campos podrían quitarse de GridView.

Para ajustar los datos recopilados, podemos usar uno de estos dos enfoques:

- Siga usando el método `AddProduct` que espera valores para los campos `UnitsOnOrder`, `UnitsInStock`y `ReorderLevel`. En el controlador de eventos `Inserting`, proporcione valores predeterminados codificados de forma rígida para usarlos con estas entradas que se han quitado de la interfaz de inserción.
- Cree una nueva sobrecarga del método `AddProduct` en la clase `ProductsBLL` que no acepte entradas para los campos `UnitsOnOrder`, `UnitsInStock`y `ReorderLevel`. A continuación, en la página ASP.NET, configure ObjectDataSource para usar esta nueva sobrecarga.

Ambas opciones también funcionarán de forma similar. En los tutoriales anteriores usamos la última opción, creando varias sobrecargas para el método de `UpdateProduct` de clase `ProductsBLL`.

## <a name="summary"></a>Resumen

GridView carece de las funciones de inserción integradas que se encuentran en DetailsView y FormView, pero con un poco de esfuerzo se puede Agregar una interfaz de inserción a la fila de pie de página. Para mostrar la fila de pie de página en un control GridView, simplemente establezca su propiedad `ShowFooter` en `true`. El contenido de la fila de pie de página se puede personalizar para cada campo convirtiendo el campo en TemplateField y agregando la interfaz de inserción a la `FooterTemplate`. Como vimos en este tutorial, el `FooterTemplate` puede contener botones, cuadros de texto, DropDownLists, casillas, controles de origen de datos para rellenar controles Web controlados por datos (como DropDownLists) y controles de validación. Junto con los controles para recopilar la entrada del usuario, se necesita un botón Agregar, LinkButton o ImageButton.

Cuando se hace clic en el botón Agregar, se invoca el método de `Insert()` ObjectDataSource s para iniciar el flujo de trabajo de inserción. A continuación, ObjectDataSource llamará al método de inserción configurado (el método `ProductsBLL` Class s `AddProduct`, en este tutorial). Debemos copiar los valores de la interfaz de inserción de GridView en la colección de `InsertParameters` de ObjectDataSource antes de que se invoque el método INSERT. Esto puede realizarse mediante programación para hacer referencia a los controles Web de la interfaz de inserción en el controlador de eventos ObjectDataSource s `Inserting`.

En este tutorial se completan las técnicas para mejorar la apariencia de GridView. En el siguiente conjunto de tutoriales se examinará cómo trabajar con datos binarios como imágenes, archivos PDF, documentos de Word, etc. y los controles Web de datos.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor de clientes potenciales de este tutorial era de Bernadette Leigh. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-cs.md)
> [Siguiente](adding-a-gridview-column-of-radio-buttons-vb.md)
