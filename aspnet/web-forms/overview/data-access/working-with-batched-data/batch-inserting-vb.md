---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Inserción por lotes (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo insertar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, se extiende GridView para permitir que el usuario escriba varios...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 413a0e209b1899414eaab70aff07ee0d3223f28f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643121"
---
# <a name="batch-inserting-vb"></a>Inserción por lotes (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) o [Descargar PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Obtenga información sobre cómo insertar varios registros de base de datos en una sola operación. En la capa de la interfaz de usuario se extiende GridView para permitir que el usuario escriba varios registros nuevos. En la capa de acceso a datos se ajustan las distintas operaciones de inserción dentro de una transacción para asegurarse de que todas las inserciones se realizan correctamente o se revierten todas las inserciones.

## <a name="introduction"></a>Introducción

En el tutorial de [actualización por lotes](batch-updating-vb.md) , examinamos la personalización del control GridView para presentar una interfaz en la que varios registros eran editables. El usuario que visita la página puede realizar una serie de cambios y, a continuación, con un solo clic de botón, realizar una actualización por lotes. En situaciones en las que los usuarios suelen actualizar muchos registros de una sola vez, este tipo de interfaz puede ahorrar innumerables clics y modificadores de contexto de teclado al mouse en comparación con las características de edición por filas predeterminadas que se exploraron primero de nuevo en la [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Este concepto también se puede aplicar al agregar registros. Imagine que aquí en Northwind Traders reciben normalmente envíos de proveedores que contienen varios productos para una categoría determinada. Por ejemplo, es posible que recibamos un envío de seis productos diferentes de té y café de Tokio Traders. Si un usuario introduce los seis productos a la vez a través de un control DetailsView, tendrán que elegir muchos de los mismos valores una y otra vez: deben elegir la misma categoría (bebidas), el mismo proveedor (Tokyo Traders), el mismo valor que no se ha suspendido ( False) y las mismas unidades en el valor de orden (0). Esta entrada repetitiva de datos no solo lleva mucho tiempo, pero es propenso a errores.

Con un poco de trabajo, podemos crear una interfaz de inserción por lotes que permita al usuario elegir el proveedor y la categoría una vez, especificar una serie de nombres de producto y precios de unidad y, a continuación, hacer clic en un botón para agregar los nuevos productos a la base de datos (consulte la figura 1). A medida que se agrega cada producto, se asignan a los campos de datos `ProductName` y `UnitPrice` los valores especificados en los cuadros de texto, mientras que los valores `CategoryID` y `SupplierID` se asignan a los valores de DropDownLists en la parte superior del formulario. Los valores `Discontinued` y `UnitsOnOrder` se establecen en los valores codificados de forma rígida de `False` y 0, respectivamente.

[![la interfaz de inserción por lotes](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figura 1**: la interfaz de inserción por lotes ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image3.png))

En este tutorial, se creará una página que implementa la interfaz de inserción por lotes que se muestra en la figura 1. Al igual que con los dos tutoriales anteriores, se ajustarán las inserciones dentro del ámbito de una transacción para garantizar la atomicidad. Vamos a empezar a trabajar.

## <a name="step-1-creating-the-display-interface"></a>Paso 1: creación de la interfaz de visualización

Este tutorial constará de una sola página que se divide en dos regiones: una región de presentación y una región de inserción. La interfaz de visualización, que crearemos en este paso, muestra los productos en un control GridView e incluye un botón titulado envío de producto de proceso. Al hacer clic en este botón, la interfaz de visualización se sustituye por la interfaz de inserción, que se muestra en la figura 1. La interfaz de visualización se devuelve después de hacer clic en los botones agregar productos desde envío o cancelar. Vamos a crear la interfaz de inserción en el paso 2.

Al crear una página que tiene dos interfaces, solo una de las cuales es visible a la vez, cada interfaz normalmente se coloca dentro de un [control Web de panel](http://www.w3schools.com/aspnet/control_panel.asp), que actúa como contenedor de otros controles. Por lo tanto, la página tendrá dos controles de panel uno por cada interfaz.

Para empezar, abra la página `BatchInsert.aspx` en la carpeta `BatchData` y arrastre un panel desde el cuadro de herramientas hasta el diseñador (vea la figura 2). Establezca la propiedad panel s `ID` en `DisplayInterface`. Al agregar el panel al diseñador, sus propiedades `Height` y `Width` se establecen en 50px y 125px, respectivamente. Borre estos valores de propiedad del ventana Propiedades.

[![arrastrar un panel desde el cuadro de herramientas hasta el diseñador](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figura 2**: arrastre un panel desde el cuadro de herramientas hasta el diseñador ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image6.png))

A continuación, arrastre un botón y un control GridView al panel. Establezca la propiedad Button s `ID` en `ProcessShipment` y su propiedad `Text` para procesar el envío del producto. Establezca la propiedad GridView s `ID` en `ProductsGrid` y, desde su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para que extraiga sus datos del método `ProductsBLL` Class s `GetProducts`. Puesto que este GridView solo se usa para Mostrar datos, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguno). Haga clic en finalizar para completar el Asistente para configurar orígenes de datos.

[![mostrar los datos devueltos por el método ProductsBLL de la clase GetProducts](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figura 3**: mostrar los datos devueltos por el método `ProductsBLL` Class s `GetProducts` ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image9.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figura 4**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](batch-inserting-vb/_static/image12.png))

Después de completar el Asistente de ObjectDataSource, Visual Studio agregará BoundFields y CheckBoxField para los campos de datos del producto. Quite todos los campos, excepto los `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`y `Discontinued`. No dude en hacer personalizaciones estéticas. Decidí dar formato al campo `UnitPrice` como un valor de moneda, reordenar los campos y cambiar el nombre de varios de los campos `HeaderText` valores. Configure también GridView para incluir la compatibilidad de paginación y ordenación. para ello, active las casillas habilitar paginación y habilitar ordenación en la etiqueta inteligente de GridView s.

Después de agregar los controles panel, botón, GridView y ObjectDataSource y personalizar los campos de GridView, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Tenga en cuenta que el marcado del botón y el control GridView aparecen dentro de las etiquetas de apertura y cierre de `<asp:Panel>`. Dado que estos controles se encuentran dentro del panel de `DisplayInterface`, se pueden ocultar simplemente estableciendo la propiedad panel s `Visible` en `False`. El paso 3 examina mediante programación el cambio de la propiedad panel s `Visible` en respuesta a un clic en un botón para mostrar una interfaz ocultando la otra.

Dedique un momento a ver nuestro progreso a través de un explorador. Como se muestra en la figura 5, debería ver un botón procesar envío de producto sobre un control GridView que enumera los productos diez a la vez.

[![GridView muestra los productos y ofrece funciones de ordenación y paginación](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figura 5**: el control GridView muestra los productos y ofrece funciones de ordenación y paginación ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image15.png))

## <a name="step-2-creating-the-inserting-interface"></a>Paso 2: creación de la interfaz de inserción

Una vez completada la interfaz de pantalla, se vuelve a preparar para crear la interfaz de inserción. En este tutorial, vamos a crear una interfaz de inserción que solicita un único proveedor y un valor de categoría y, a continuación, permite al usuario escribir hasta cinco nombres de producto y valores de precio por unidad. Con esta interfaz, el usuario puede Agregar de uno a cinco productos nuevos que compartan la misma categoría y proveedor, pero que tengan nombres de producto y precios únicos.

Empiece arrastrando un panel desde el cuadro de herramientas hasta el diseñador, colocándolo debajo del panel de `DisplayInterface` existente. Establezca la propiedad `ID` de este panel recién agregado en `InsertingInterface` y establezca su propiedad `Visible` en `False`. En el paso 3, agregaremos el código que establece la propiedad `Visible` del panel `InsertingInterface` en `True`. Borre también los valores de la propiedad panel s `Height` y `Width`.

A continuación, necesitamos crear la interfaz de inserción que se mostró en la figura 1. Esta interfaz se puede crear a través de diversas técnicas HTML, pero vamos a usar una tabla bastante sencilla: una tabla de cuatro columnas y siete filas.

> [!NOTE]
> Al escribir el marcado para los elementos de `<table>` HTML, prefiero usar la vista de código fuente. Aunque Visual Studio tiene herramientas para agregar elementos `<table>` a través del diseñador, el diseñador parece estar demasiado dispuesto para insertar la configuración de `style` en el marcado. Una vez que se ha creado el marcado de `<table>`, normalmente se vuelve al diseñador para agregar los controles Web y establecer sus propiedades. Al crear tablas con columnas y filas predeterminadas, prefiero usar HTML estático en lugar del [control Web Table](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) , ya que todos los controles Web situados dentro de un control Web Table solo pueden tener acceso mediante el patrón `FindControl("controlID")`. No obstante, se usan controles Web de tabla para tablas de tamaño dinámico (aquellas cuyas filas o columnas se basan en algunos criterios especificados por el usuario o en una base de datos), ya que el control Web Table se puede construir mediante programación.

Escriba el siguiente marcado dentro de las etiquetas `<asp:Panel>` del panel `InsertingInterface`:

[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Este marcado de `<table>` no incluye todavía ningún control Web, los agregaremos en breve. Tenga en cuenta que cada elemento `<tr>` contiene una configuración de clase CSS determinada: `BatchInsertHeaderRow` para la fila de encabezado donde va a ir el proveedor y la categoría DropDownLists; `BatchInsertFooterRow` para la fila de pie de página en la que Irán los botones agregar productos de envío y cancelar. y alternan `BatchInsertRow` y `BatchInsertAlternatingRow` valores de las filas que contendrán los controles de cuadro de texto Product y Unit Price. He creado las clases CSS correspondientes en el archivo `Styles.css` para dar a la interfaz de inserción una apariencia similar a la de los controles GridView y DetailsView que se usan en estos tutoriales. Estas clases CSS se muestran a continuación.

[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Con este marcado escrito, vuelva a la Vista de diseño. Este `<table>` debe mostrarse como una tabla de cuatro columnas de siete filas en el diseñador, tal como se muestra en la figura 6.

[![la interfaz de inserción se compone de una tabla de cuatro columnas y siete filas](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figura 6**: la interfaz de inserción se compone de una tabla de cuatro columnas y siete filas ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image18.png))

Ahora estamos listos para agregar los controles Web a la interfaz de inserción. Arrastre dos DropDownLists desde el cuadro de herramientas hasta las celdas adecuadas de la tabla uno para el proveedor y otro para la categoría.

Establezca la propiedad Supplier DropDownList s `ID` en `Suppliers` y enlácelo a un nuevo ObjectDataSource denominado `SuppliersDataSource`. Configure el nuevo ObjectDataSource para recuperar sus datos del método `SuppliersBLL` Class s `GetSuppliers` y establezca la lista desplegable actualizar pestaña s en (ninguno). Haga clic en Finalizar para completar el asistente.

[![configurar ObjectDataSource para usar el método SuppliersBLL de la clase GetSuppliers](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figura 7**: configuración de ObjectDataSource para usar el método `SuppliersBLL` Class s `GetSuppliers` ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image21.png))

Haga que el `Suppliers` DropDownList muestre el `CompanyName` campo de datos y use el campo de datos `SupplierID` como sus valores `ListItem` s.

[![mostrar el campo de datos CompanyName y usar SupplierID como valor](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figura 8**: mostrar el `CompanyName` campo de datos y usar `SupplierID` como valor ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image24.png))

Denomine al segundo `Categories` de DropDownList y enlácelo a un nuevo ObjectDataSource denominado `CategoriesDataSource`. Configure el `CategoriesDataSource` ObjectDataSource para usar el método de `GetCategories` `CategoriesBLL` Class s; establezca las listas desplegables de las pestañas actualizar y eliminar en (ninguna) y haga clic en finalizar para completar el asistente. Por último, haga que el DropDownList muestre el `CategoryName` campo de datos y use el `CategoryID` como valor.

Después de que estos dos DropDownLists se hayan agregado y enlazado al ObjectDataSource configurado correctamente, la pantalla debe tener un aspecto similar al de la ilustración 9.

[![la fila de encabezado contiene ahora los proveedores y las categorías DropDownLists](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figura 9**: la fila de encabezado contiene ahora el `Suppliers` y `Categories` DropDownLists ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image27.png))

Ahora tenemos que crear los cuadros de texto para recopilar el nombre y el precio de cada nuevo producto. Arrastre un control TextBox desde el cuadro de herramientas hasta el diseñador para cada una de las cinco filas Product Name y price. Establezca las propiedades del `ID` de los cuadros de texto en `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, etc.

Agregue un control CompareValidator después de cada uno de los cuadros de texto de precio por unidad y establezca la propiedad `ControlToValidate` en el `ID`adecuado. Establezca también la propiedad `Operator` en `GreaterThanEqual`, `ValueToCompare` en 0 y `Type` en `Currency`. Esta configuración indica a CompareValidator que asegúrese de que el precio, si se especifica, es un valor de moneda válido que es mayor o igual que cero. Establezca la propiedad `Text` en \*y `ErrorMessage` en el precio debe ser mayor o igual que cero. También puede omitir cualquier símbolo de moneda.

> [!NOTE]
> La interfaz de inserción no incluye ningún control RequiredFieldValidator, aunque el campo de `ProductName` de la tabla de base de datos `Products` no permite valores `NULL`. Esto se debe a que queremos permitir que el usuario escriba hasta cinco productos. Por ejemplo, si el usuario proporcionara el nombre del producto y el precio por unidad de las tres primeras filas y deja las dos últimas filas en blanco, solo agregaremos tres nuevos productos al sistema. No obstante, dado que `ProductName` es necesario, se debe comprobar mediante programación para asegurarse de que, si se especifica un precio por unidad, se proporciona un valor de nombre de producto correspondiente. Abordaremos esta comprobación en el paso 4.

Al validar la entrada del usuario, el CompareValidator informa de los datos no válidos si el valor contiene un símbolo de moneda. Agregue un signo $ delante de cada uno de los cuadros de texto precio por unidad para servir como una indicación visual que indica al usuario que omita el símbolo de moneda al especificar el precio.

Por último, agregue un control ValidationSummary en el panel `InsertingInterface`, configuración de su propiedad `ShowMessageBox` a `True` y su propiedad `ShowSummary` a `False`. Con esta configuración, si el usuario escribe un valor de precio unitario no válido, aparecerá un asterisco junto a los controles de cuadro de texto infractores y el control ValidationSummary mostrará un cuadro de mensajes del lado cliente que muestra el mensaje de error que se especificó anteriormente.

En este punto, la pantalla debe tener un aspecto similar al de la figura 10.

[![la interfaz de inserción ahora incluye cuadros de texto para los nombres y precios de los productos](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figura 10**: la interfaz de inserción ahora incluye cuadros de texto para los nombres y precios de los productos ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image30.png))

A continuación, es necesario agregar los botones agregar productos desde envío y cancelar a la fila de pie de página. Arrastre dos controles de botón desde el cuadro de herramientas al pie de página de la interfaz de inserción, estableciendo los botones `ID` propiedades en `AddProducts` y `CancelButton` y `Text` propiedades para agregar productos de envío y cancelación, respectivamente. Además, establezca la propiedad `CancelButton` control s `CausesValidation` en `false`.

Por último, es necesario agregar un control Web de etiqueta que mostrará mensajes de estado para las dos interfaces. Por ejemplo, cuando un usuario agrega correctamente un nuevo envío de productos, queremos volver a la interfaz de pantalla y mostrar un mensaje de confirmación. Sin embargo, si el usuario proporciona un precio para un nuevo producto pero deja el nombre del producto, es necesario mostrar un mensaje de advertencia, ya que el campo `ProductName` es obligatorio. Puesto que necesitamos que este mensaje se muestre en ambas interfaces, colóquelo en la parte superior de la página fuera de los paneles.

Arrastre un control Web Label desde el cuadro de herramientas hasta la parte superior de la página en el diseñador. Establezca la propiedad `ID` en `StatusLabel`, borre la propiedad `Text` y establezca las propiedades `Visible` y `EnableViewState` en `False`. Como hemos podido ver en los tutoriales anteriores, al establecer la propiedad `EnableViewState` en `False` nos permite cambiar mediante programación los valores de la propiedad label s y hacer que se reviertan automáticamente a sus valores predeterminados en el PostBack posterior. Esto simplifica el código para mostrar un mensaje de estado en respuesta a alguna acción del usuario que desaparece en el PostBack posterior. Por último, establezca la propiedad `StatusLabel` control s `CssClass` en warning, que es el nombre de una clase CSS definida en `Styles.css` que muestra texto en una fuente grande, cursiva, negrita y rojo.

En la figura 11 se muestra el diseñador de Visual Studio una vez agregada y configurada la etiqueta.

[![Coloque el control StatusLabel sobre los dos controles de panel](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figura 11**: Coloque el control `StatusLabel` sobre los dos controles de panel ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image33.png))

## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Paso 3: cambiar entre las interfaces de presentación e inserción

Llegados a este punto, hemos completado el marcado para nuestras interfaces de visualización e inserción, pero todavía hemos dejado dos tareas:

- Cambiar entre las interfaces de presentación e inserción
- Agregar los productos del envío a la base de datos

Actualmente, la interfaz de visualización está visible pero la interfaz de inserción está oculta. Esto se debe a que la propiedad panel `DisplayInterface` s `Visible` está establecida en `True` (el valor predeterminado), mientras que la propiedad `Visible` del panel `InsertingInterface` está establecida en `False`. Para cambiar entre las dos interfaces, simplemente tenemos que alternar cada control s `Visible` valor de propiedad.

Queremos pasar de la interfaz de pantalla a la interfaz de inserción cuando se haga clic en el botón procesar envío de producto. Por lo tanto, cree un controlador de eventos para este evento de botón `Click` que contenga el código siguiente:

[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Este código simplemente oculta el panel de `DisplayInterface` y muestra el panel de `InsertingInterface`.

A continuación, cree controladores de eventos para los controles de botón Agregar productos desde el envío y cancelar en la interfaz de inserción. Cuando se hace clic en cualquiera de estos botones, es necesario volver a la interfaz de pantalla. Cree `Click` controladores de eventos para ambos controles de botón de modo que llamen a `ReturnToDisplayInterface`, un método que se agregará momentáneamente. Además de ocultar el panel de `InsertingInterface` y mostrar el panel de `DisplayInterface`, el método `ReturnToDisplayInterface` debe devolver los controles Web a su estado de edición previa. Esto implica establecer las propiedades de DropDownLists `SelectedIndex` en 0 y desactivar las propiedades `Text` de los controles de cuadro de texto.

> [!NOTE]
> Tenga en cuenta lo que puede suceder si no devolvía los controles a su estado de edición previa antes de volver a la interfaz de visualización. Es posible que un usuario haga clic en el botón procesar envío de producto, escriba los productos del envío y, a continuación, haga clic en agregar productos del envío. Esto agregaría los productos y devolvería al usuario a la interfaz de pantalla. Llegados a este punto, es posible que el usuario quiera agregar otro envío. Al hacer clic en el botón procesar envío de producto, se devolverían a la interfaz de inserción, pero las selecciones de DropDownList y los valores de cuadro de texto se seguirían rellenando con los valores anteriores.

[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Ambos controladores de eventos `Click` llaman simplemente al método `ReturnToDisplayInterface`, aunque volveremos al controlador de eventos Add Products from reenvío `Click` en el paso 4 y agregaremos código para guardar los productos. `ReturnToDisplayInterface` comienza devolviendo el `Suppliers` y `Categories` DropDownLists a sus primeras opciones. Las dos constantes `firstControlID` y `lastControlID` marcan los valores de índice de control inicial y final usados para asignar nombres a los cuadros de texto nombre de producto y precio unitario en la interfaz de inserción y se usan en los límites del bucle `For` que establece las propiedades `Text` de los controles TextBox de nuevo en una cadena vacía. Por último, se restablecen las propiedades de los paneles `Visible` para que la interfaz de inserción se oculte y se muestre la interfaz de pantalla.

Dedique un momento a probar esta página en un explorador. Al visitar la página por primera vez, debería ver la interfaz de visualización como se muestra en la figura 5. Haga clic en el botón procesar envío de producto. La página se devolverá como postback y ahora debería ver la interfaz de inserción como se muestra en la figura 12. Al hacer clic en los botones agregar productos desde el envío o cancelar, se muestra la interfaz de pantalla.

> [!NOTE]
> Al ver la interfaz de inserción, dedique un momento a probar el CompareValidators en los cuadros de texto precio por unidad. Debería ver una advertencia de cuadro de mensajes del lado cliente al hacer clic en el botón Agregar productos de envío con valores de moneda o precios no válidos con un valor inferior a cero.

[![se muestra la interfaz de inserción después de hacer clic en el botón procesar envío de producto](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figura 12**: la interfaz de inserción se muestra después de hacer clic en el botón procesar envío de producto ([haga clic para ver la imagen a tamaño completo](batch-inserting-vb/_static/image36.png))

## <a name="step-4-adding-the-products"></a>Paso 4: agregar los productos

Lo único que queda para este tutorial es guardar los productos en la base de datos en el controlador de eventos agregar productos desde el botón de envío `Click`. Esto puede realizarse mediante la creación de una `ProductsDataTable` y la adición de una instancia de `ProductsRow` para cada uno de los nombres de producto suministrados. Una vez que se han agregado estos `ProductsRow` se realizará una llamada al método `ProductsBLL` de clase `UpdateWithTransaction` pasando el `ProductsDataTable`. Recuerde que el método `UpdateWithTransaction`, que se creó en el tutorial [ajustar las modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) , pasa el `ProductsDataTable` al método `UpdateWithTransaction` del `ProductsTableAdapter`. A partir de ahí, se inicia una transacción ADO.NET y el TableAdapter emite una instrucción `INSERT` a la base de datos para cada `ProductsRow` agregada en la DataTable. Suponiendo que todos los productos se agregan sin errores, se confirma la transacción; en caso contrario, se revierte.

También es necesario realizar una comprobación de errores en el código del controlador de eventos agregar productos desde el botón de envío s `Click`. Dado que no se usa ningún RequiredFieldValidators en la interfaz de inserción, un usuario puede especificar un precio para un producto omitiendo su nombre. Dado que el nombre del producto es obligatorio, si esta condición se despliega, es necesario avisar al usuario y no continuar con las inserciones. A continuación se muestra el código completo del controlador de eventos `Click`:

[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

El controlador de eventos se inicia asegurándose de que la propiedad `Page.IsValid` devuelve un valor de `True`. Si devuelve `False`, eso significa que uno o varios de los CompareValidators notifican datos no válidos. en tal caso, no queremos intentar insertar los productos especificados o bien, terminaremos con una excepción al intentar asignar el valor del precio unitario especificado por el usuario a la propiedad `ProductsRow` s `UnitPrice`.

A continuación, se crea una nueva instancia de `ProductsDataTable` (`products`). Un bucle `For` se usa para recorrer en iteración los cuadros de texto nombre del producto y precio de la unidad y las propiedades `Text` se leen en las variables locales `productName` y `unitPrice`. Si el usuario ha especificado un valor para el precio por unidad pero no para el nombre de producto correspondiente, el `StatusLabel` muestra el mensaje si proporciona un precio por unidad también debe incluir el nombre del producto y el controlador de eventos se ha cerrado.

Si se ha proporcionado un nombre de producto, se crea una nueva instancia de `ProductsRow` mediante el método `ProductsDataTable` s `NewProductsRow`. Esta nueva `ProductsRow` instancia de `ProductName` propiedad se establece en el cuadro de texto nombre del producto actual, mientras que las propiedades `SupplierID` y `CategoryID` se asignan a las propiedades `SelectedValue` de DropDownLists en el encabezado insertar interfaz s. Si el usuario ha especificado un valor para el precio del producto, se asigna a la propiedad `ProductsRow` instancia s `UnitPrice`; de lo contrario, la propiedad se deja sin asignar, lo que dará como resultado un valor de `NULL` para `UnitPrice` en la base de datos. Por último, las propiedades `Discontinued` y `UnitsOnOrder` se asignan a los valores codificados de forma rígida `False` y 0, respectivamente.

Una vez asignadas las propiedades a la instancia de `ProductsRow`, se agrega a la `ProductsDataTable`.

Al finalizar el bucle de `For`, se comprueba si se ha agregado algún producto. Después de todo, el usuario puede hacer clic en agregar productos del envío antes de escribir cualquier nombre de producto o precio. Si hay al menos un producto en el `ProductsDataTable`, se llama al método `ProductsBLL` clase s `UpdateWithTransaction`. Después, los datos se reenlazan a la `ProductsGrid` GridView para que los productos recién agregados aparezcan en la interfaz de pantalla. El `StatusLabel` se actualiza para mostrar un mensaje de confirmación y se invoca el `ReturnToDisplayInterface`, ocultando la interfaz de inserción y mostrando la interfaz de pantalla.

Si no se especificó ningún producto, la interfaz de inserción permanece visible pero el mensaje no se agregó ningún producto. Escriba los nombres de producto y los precios de unidad en los cuadros de texto que se muestran.

Las figuras s 13, 14 y 15 muestran las interfaces de inserción y visualización en acción. En la figura 13, el usuario ha especificado un valor de precio por unidad sin un nombre de producto correspondiente. La figura 14 muestra la interfaz de visualización después de que se hayan agregado tres nuevos productos correctamente, mientras que la figura 15 muestra dos de los productos recién agregados en GridView (el tercero está en la página anterior).

[![se requiere un nombre de producto al especificar un precio por unidad](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figura 13**: se requiere un nombre de producto al especificar un precio por unidad ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image39.png))

[![se han agregado tres nuevos veggies para el proveedor Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figura 14**: se han agregado tres nuevos veggies para el proveedor Mayumi s ([haga clic para ver la imagen de tamaño completo](batch-inserting-vb/_static/image42.png))

[![los nuevos productos se pueden encontrar en la última página de GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figura 15**: los nuevos productos se pueden encontrar en la última página de GridView ([haga clic para ver la imagen a tamaño completo](batch-inserting-vb/_static/image45.png))

> [!NOTE]
> La lógica de inserción de lotes utilizada en este tutorial incluye las inserciones en el ámbito de la transacción. Para comprobarlo, introduzca intencionadamente un error de nivel de base de datos. Por ejemplo, en lugar de asignar la propiedad New `ProductsRow` Instance s `CategoryID` al valor seleccionado en el `Categories` DropDownList, asígnelo a un valor como `i * 5`. Aquí `i` es el indizador de bucle y tiene valores comprendidos entre 1 y 5. Por lo tanto, al agregar dos o más productos en batch INSERT, el primer producto tendrá un valor de `CategoryID` válido (5), pero los productos subsiguientes tendrán `CategoryID` valores que no coincidan con `CategoryID` valores en la tabla de `Categories`. El efecto neto es que, mientras que la primera `INSERT` se realizará correctamente, se producirá un error en las subsiguientes con una infracción de la restricción FOREIGN KEY. Dado que la inserción por lotes es atómica, se revertirá la primera `INSERT` y se devolverá la base de datos a su estado anterior al inicio del proceso de inserción por lotes.

## <a name="summary"></a>Resumen

A través de este y los dos tutoriales anteriores, hemos creado interfaces que permiten actualizar, eliminar e insertar lotes de datos, todos los cuales usaban la compatibilidad con transacciones que agregamos a la capa de acceso a datos en el tutorial [ajustar modificaciones de base de datos dentro de una transacción](wrapping-database-modifications-within-a-transaction-vb.md) . En ciertos escenarios, estas interfaces de usuario de procesamiento por lotes mejoran en gran medida la eficiencia del usuario final, ya que reducen el número de clics, los postbacks y los modificadores de contexto de teclado a mouse, a la vez que se mantiene la integridad de los datos subyacentes.

En este tutorial se completa el trabajo con datos por lotes. El siguiente conjunto de tutoriales explora diversos escenarios avanzados de nivel de acceso a datos, incluido el uso de procedimientos almacenados en los métodos de TableAdapter s, la configuración de los valores de conexión y de nivel de comando en la capa DAL, el cifrado de cadenas de conexión, etc.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Hilton Giesenow y S Ren Jacob Lauritsen. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-deleting-vb.md)
