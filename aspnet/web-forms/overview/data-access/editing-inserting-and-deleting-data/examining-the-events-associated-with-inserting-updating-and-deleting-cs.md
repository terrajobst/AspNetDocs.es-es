---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examinar los eventos asociados a la inserción, actualización y eliminación (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial se examinará el uso de los eventos que se producen antes, durante y después de una operación de inserción, actualización o eliminación de un control Web de datos ASP.NET. W...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: a8c1388b73524a8bb918b67aa265db894c07636f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74572369"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examinar los eventos relacionados con la inserción, actualización y eliminación (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) de la aplicación de ejemplo o [descarga de PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> En este tutorial se examinará el uso de los eventos que se producen antes, durante y después de una operación de inserción, actualización o eliminación de un control Web de datos ASP.NET. También veremos cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos del producto.

## <a name="introduction"></a>Introducción

Al utilizar las características integradas de inserción, edición o eliminación de los controles GridView, DetailsView o FormView, se lleva a cabo una serie de pasos cuando el usuario final completa el proceso de agregar un nuevo registro o actualizar o eliminar un registro existente. Como hemos comentado en el [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md), cuando se edita una fila en el control GridView, el botón Editar se reemplaza por los botones actualizar y cancelar y los BoundFields se convierten en cuadros de texto. Una vez que el usuario final actualice los datos y haga clic en actualizar, se realizarán los pasos siguientes en el PostBack:

1. GridView rellena la `UpdateParameters` de ObjectDataSource con los campos de identificación únicos del registro editado (a través de la propiedad `DataKeyNames`) junto con los valores especificados por el usuario.
2. GridView invoca el método `Update()` de ObjectDataSource, que a su vez invoca el método adecuado en el objeto subyacente (`ProductsDAL.UpdateProduct`, en el tutorial anterior).
3. Los datos subyacentes, que ahora incluyen los cambios actualizados, se reenlazan a GridView

Durante esta secuencia de pasos, se desencadena una serie de eventos, lo que nos permite crear controladores de eventos para agregar lógica personalizada cuando sea necesario. Por ejemplo, antes del paso 1, se activa el evento `RowUpdating` de GridView. En este punto, se puede cancelar la solicitud de actualización si hay algún error de validación. Cuando se invoca el método de `Update()`, se activa el evento de `Updating` de ObjectDataSource, lo que proporciona una oportunidad para agregar o personalizar los valores de cualquiera de las `UpdateParameters`. Una vez finalizada la ejecución del método del objeto subyacente de ObjectDataSource, se genera el evento `Updated` de ObjectDataSource. Un controlador de eventos para el evento `Updated` puede inspeccionar los detalles sobre la operación de actualización, como el número de filas afectadas y si se produjo o no una excepción. Por último, después del paso 2, se desencadena el evento `RowUpdated` de GridView; un controlador de eventos para este evento puede examinar información adicional sobre la operación de actualización que se acaba de realizar.

La figura 1 muestra esta serie de eventos y pasos al actualizar un control GridView. El patrón de eventos de la figura 1 no es único para la actualización con GridView. Al insertar, actualizar o eliminar datos de GridView, DetailsView o FormView, se precipita la misma secuencia de eventos anteriores y posteriores para el control Web de datos y ObjectDataSource.

[![una serie de eventos previos y posteriores que se activan al actualizar los datos en un control GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: se activa una serie de eventos previos y posteriores al actualizar los datos en un control GridView ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))

En este tutorial se examinará el uso de estos eventos para ampliar las capacidades integradas de inserción, actualización y eliminación de los controles Web de datos de ASP.NET. También veremos cómo personalizar la interfaz de edición para actualizar solo un subconjunto de los campos del producto.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Paso 1: actualización de los campos de`ProductName`y`UnitPrice`de un producto

En las interfaces de edición del tutorial anterior, se tenían que incluir *todos* los campos de producto que no eran de solo lectura. Si fuera a quitar un campo del `QuantityPerUnit` de GridView: al actualizar los datos, el control Web de datos no establecería el valor de `UpdateParameters` de la `QuantityPerUnit` de ObjectDataSource. A continuación, ObjectDataSource pasaría un valor `null` al método `UpdateProduct` capa de lógica de negocios (BLL), que cambiaría la columna `QuantityPerUnit` del registro de la base de datos editada por un valor de `NULL`. Del mismo modo, si se quita un campo obligatorio, como `ProductName`, de la interfaz de edición, se producirá un error en la actualización con una excepción "la*columna ' ProductName ' no permite valores NULL*. La razón de este comportamiento se debe a que ObjectDataSource se configuró para llamar al método `UpdateProduct` de la clase `ProductsBLL`, que esperaba un parámetro de entrada para cada uno de los campos del producto. Por lo tanto, la colección de `UpdateParameters` de ObjectDataSource contenía un parámetro para cada uno de los parámetros de entrada del método.

Si queremos proporcionar un control Web de datos que permita al usuario final actualizar solo un subconjunto de campos, es necesario establecer mediante programación los valores de `UpdateParameters` que faltan en el controlador de eventos de `Updating` de ObjectDataSource o crear y llamar a un método BLL que espera solo un subconjunto de los campos. Vamos a explorar este último enfoque.

En concreto, vamos a crear una página que muestra solo los campos `ProductName` y `UnitPrice` en un control GridView modificable. La interfaz de edición de GridView solo permitirá al usuario actualizar los dos campos mostrados, `ProductName` y `UnitPrice`. Dado que esta interfaz de edición solo proporciona un subconjunto de los campos de un producto, es necesario crear un ObjectDataSource que use el método de `UpdateProduct` de la capa BLL existente y que tenga los valores de campo de producto que faltan establecidos mediante programación en su controlador de eventos `Updating`, o bien, es necesario crear un nuevo método BLL que espera solo el subconjunto de campos definidos en GridView. En este tutorial, vamos a usar la última opción y crearemos una sobrecarga del método `UpdateProduct`, una que toma solo tres parámetros de entrada: `productName`, `unitPrice`y `productID`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Al igual que el método de `UpdateProduct` original, esta sobrecarga comienza comprobando si hay un producto en la base de datos con el `ProductID`especificado. En caso contrario, devuelve `false`, que indica que se produjo un error en la solicitud para actualizar la información del producto. De lo contrario, actualiza los campos `ProductName` y `UnitPrice` del registro del producto existentes en consecuencia y confirma la actualización llamando al método `Update()` del TableAdapter, pasando la instancia de `ProductsRow`.

Con esta adición a nuestra `ProductsBLL` clase, estamos listos para crear la interfaz de GridView simplificada. Abra el `DataModificationEvents.aspx` en la carpeta `EditInsertDelete` y agregue un control GridView a la página. Cree un nuevo ObjectDataSource y configúrelo para utilizar la `ProductsBLL` clase con su `Select()` asignación de método a `GetProducts` y su `Update()` de asignación de método a la sobrecarga `UpdateProduct` que toma solo los parámetros de entrada `productName`, `unitPrice`y `productID`. En la ilustración 2 se muestra el Asistente para crear orígenes de datos al asignar el método `Update()` de ObjectDataSource a la nueva sobrecarga del método `UpdateProduct` de la clase `ProductsBLL`.

[![asignar el método Update () de ObjectDataSource a la nueva sobrecarga UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: asignación del método `Update()` de ObjectDataSource a la nueva sobrecarga de `UpdateProduct` ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))

Dado que nuestro ejemplo solo necesitará la capacidad de editar datos, pero no insertar o eliminar registros, dedique un momento a indicar explícitamente que los métodos `Insert()` y `Delete()` de ObjectDataSource no se deben asignar a ninguno de los métodos de la clase `ProductsBLL`. para ello, vaya a las pestañas insertar y eliminar y elija (ninguno) en la lista desplegable.

[![elegir (ninguno) en la lista desplegable de las pestañas insertar y eliminar](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: elegir (ninguno) en la lista desplegable de las pestañas insertar y eliminar ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))

Después de completar este asistente, active la casilla habilitar edición de la etiqueta inteligente de GridView.

Con la finalización del Asistente para crear orígenes de datos y el enlace a GridView, Visual Studio ha creado la sintaxis declarativa para ambos controles. Vaya a la vista de código fuente para inspeccionar el marcado declarativo de ObjectDataSource, que se muestra a continuación:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Dado que no hay ninguna asignación para los métodos `Insert()` y `Delete()` de ObjectDataSource, no hay ninguna sección `InsertParameters` o `DeleteParameters`. Además, dado que el método de `Update()` se asigna a la sobrecarga del método `UpdateProduct` que solo acepta tres parámetros de entrada, la sección `UpdateParameters` tiene solo tres instancias de `Parameter`.

Tenga en cuenta que la propiedad `OldValuesParameterFormatString` de ObjectDataSource está establecida en `original_{0}`. Visual Studio establece esta propiedad automáticamente al usar el Asistente para configurar orígenes de datos. Sin embargo, dado que nuestros métodos de BLL no esperan que se pase el valor de `ProductID` original, quite la asignación de esta propiedad por completo de la sintaxis declarativa de ObjectDataSource.

> [!NOTE]
> Si simplemente borra el valor de la propiedad `OldValuesParameterFormatString` de la ventana Propiedades en el Vista de diseño, la propiedad seguirá existiendo en la sintaxis declarativa, pero se establecerá en una cadena vacía. Quite la propiedad de la sintaxis declarativa o, en el ventana Propiedades, establezca el valor en el predeterminado `{0}`.

Aunque ObjectDataSource solo tiene `UpdateParameters` para el nombre, el precio y el identificador del producto, Visual Studio ha agregado BoundField o CheckBoxField en GridView para cada uno de los campos del producto.

[![GridView contiene un CheckBoxField para cada uno de los campos del producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: el control GridView contiene un control BoundField o CheckBoxField para cada uno de los campos del producto ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))

Cuando el usuario final edita un producto y hace clic en su botón actualizar, el control GridView enumera los campos que no eran de solo lectura. A continuación, establece el valor del parámetro correspondiente en la colección de `UpdateParameters` de ObjectDataSource en el valor especificado por el usuario. Si no hay un parámetro correspondiente, el control GridView agrega uno a la colección. Por lo tanto, si el control GridView contiene BoundFields y CheckBoxFields para todos los campos del producto, ObjectDataSource terminará invocando la `UpdateProduct` sobrecarga que toma todos estos parámetros, a pesar de que el marcado declarativo de ObjectDataSource especifique solo tres parámetros de entrada (vea la figura 5). De forma similar, si hay una combinación de campos de producto que no son de solo lectura en GridView que no se corresponde con los parámetros de entrada de una sobrecarga `UpdateProduct`, se generará una excepción al intentar actualizar.

[![GridView agregará parámetros a la colección UpdateParameters de ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: el control GridView agregará parámetros a la colección de `UpdateParameters` de ObjectDataSource ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))

Para asegurarse de que ObjectDataSource invoca la sobrecarga `UpdateProduct` que toma solo el nombre, el precio y el identificador del producto, es necesario restringir el control GridView para tener campos editables solo para los `ProductName` y `UnitPrice`. Esto puede realizarse quitando los demás BoundFields y CheckBoxFields, estableciendo los otros campos de la propiedad `ReadOnly` en `true`, o bien mediante alguna combinación de ambos. En este tutorial, vamos a quitar simplemente todos los campos de GridView excepto el `ProductName` y `UnitPrice` BoundFields, después del cual el marcado declarativo de GridView tendrá el siguiente aspecto:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Aunque la sobrecarga `UpdateProduct` espera tres parámetros de entrada, solo tenemos dos BoundFields en nuestro GridView. Esto se debe a que el `productID` parámetro de entrada es un valor de clave principal y se pasa a través del valor de la propiedad `DataKeyNames` de la fila modificada.

Nuestro GridView, junto con la sobrecarga de `UpdateProduct`, permite a los usuarios editar solo el nombre y el precio de un producto sin perder ningún otro campo de producto.

[![la interfaz permite editar solo el nombre y el precio del producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: la interfaz permite editar solo el nombre y el precio del producto ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))

> [!NOTE]
> Tal como se ha explicado en el tutorial anterior, es importante que el estado de vista de GridView esté habilitado (comportamiento predeterminado). Si establece la propiedad GridView s `EnableViewState` en `false`, corre el riesgo de que los usuarios simultáneos eliminen o editen registros involuntariamente. Vea [ADVERTENCIA: problema de simultaneidad con ASP.NET 2,0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) para obtener más información.

## <a name="improving-theunitpriceformatting"></a>Mejora del formato de`UnitPrice`

Aunque el ejemplo de GridView que se muestra en la figura 6 funciona, no se da formato al campo `UnitPrice`, lo que da lugar a una presentación de precios que carece de símbolos de moneda y tiene cuatro posiciones decimales. Para aplicar un formato de moneda a las filas no editables, basta con establecer la propiedad `DataFormatString` de `UnitPrice` BoundField en `{0:c}` y su propiedad `HtmlEncode` en `false`.

[![establecer las propiedades DataFormatString y HtmlEncode del UnitPrice en consecuencia](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: establecimiento de las propiedades `DataFormatString` y `HtmlEncode` del `UnitPrice`en consecuencia ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))

Con este cambio, las filas no editables dan formato al precio como moneda; la fila modificada, sin embargo, sigue mostrando el valor sin el símbolo de moneda y con cuatro posiciones decimales.

[![filas no editables ahora tienen el formato de valores de moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: ahora, las filas no editables tienen el formato de valores de moneda ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))

Las instrucciones de formato especificadas en la propiedad `DataFormatString` se pueden aplicar a la interfaz de edición estableciendo la propiedad `ApplyFormatInEditMode` de BoundField en `true` (el valor predeterminado es `false`). Dedique un momento a establecer esta propiedad en `true`.

[![establecer la propiedad ApplyFormatInEditMode de UnitPrice BoundField en true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: establezca la propiedad `ApplyFormatInEditMode` de `UnitPrice` BoundField en `true` ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))

Con este cambio, el valor de la `UnitPrice` que se muestra en la fila editada también tiene el formato de moneda.

[![el valor UnitPrice de la fila editada ahora tiene el formato de moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: el valor de `UnitPrice` de la fila editada ahora tiene el formato de moneda ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))

Sin embargo, al actualizar un producto con el símbolo de moneda en el cuadro de texto, como $19,00, se produce una `FormatException`. Cuando GridView intenta asignar los valores proporcionados por el usuario a la colección de `UpdateParameters` de ObjectDataSource, no puede convertir la cadena de `UnitPrice` "$19,00" en el `decimal` requerido por el parámetro (consulte la figura 11). Para solucionar este error, podemos crear un controlador de eventos para el evento de `RowUpdating` de GridView y hacer que analice el `UnitPrice` proporcionado por el usuario como un `decimal`con formato de moneda.

El evento `RowUpdating` de GridView acepta como segundo parámetro un objeto de tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que incluye un diccionario de `NewValues` como una de sus propiedades que contiene los valores proporcionados por el usuario listos para ser asignados a la colección de `UpdateParameters` de ObjectDataSource. Podemos sobrescribir el valor de la `UnitPrice` existente en la colección `NewValues` con un valor decimal analizado con el formato de moneda con las siguientes líneas de código en el controlador de eventos `RowUpdating`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Si el usuario ha proporcionado un valor `UnitPrice` (como "$19,00"), este valor se sobrescribe con el valor decimal calculado por [decimal. Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analizando el valor como moneda. Esto analizará correctamente el decimal en el caso de cualquier símbolo de moneda, comas, puntos decimales, etc., y usará la [enumeración NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) en el espacio de nombres [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) .

En la figura 11 se muestra el problema que provocan los símbolos de moneda en el `UnitPrice`proporcionado por el usuario, junto con el modo en que se puede usar el controlador de eventos `RowUpdating` de GridView para analizar correctamente dicha entrada.

[![el valor UnitPrice de la fila editada ahora tiene el formato de moneda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: el valor de `UnitPrice` de la fila editada ahora tiene el formato de moneda ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Paso 2: prohibir`NULL UnitPrices`

Aunque la base de datos está configurada para permitir `NULL` valores en la columna `UnitPrice` de la tabla `Products`, es posible que desee evitar que los usuarios visiten esta página en concreto para especificar un valor de `UnitPrice` `NULL`. Es decir, si un usuario no puede especificar un valor `UnitPrice` al editar una fila de producto, en lugar de guardar los resultados en la base de datos, deseamos mostrar un mensaje que informa al usuario de que, a través de esta página, los productos editados deben tener un precio especificado.

El objeto `GridViewUpdateEventArgs` que se pasa al controlador de eventos `RowUpdating` de GridView contiene una propiedad `Cancel` que, si se establece en `true`, finaliza el proceso de actualización. Vamos a extender el controlador de eventos `RowUpdating` para establecer `e.Cancel` en `true` y mostrar un mensaje que explique por qué se `NewValues` el valor `UnitPrice` de la colección `null`.

Empiece agregando un control Web de etiqueta a la página denominada `MustProvideUnitPriceMessage`. Este control de etiqueta se mostrará si el usuario no puede especificar un valor `UnitPrice` al actualizar un producto. Establezca la propiedad `Text` de la etiqueta en "debe proporcionar un precio para el producto". También he creado una nueva clase CSS en `Styles.css` denominada `Warning` con la siguiente definición:

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Por último, establezca la propiedad `CssClass` de la etiqueta en `Warning`. En este momento, el diseñador debería mostrar el mensaje de advertencia en un tamaño de fuente rojo, negrita, cursiva y extra grande por encima de GridView, como se muestra en la figura 12.

[![se ha agregado una etiqueta sobre GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: se ha agregado una etiqueta encima de GridView ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))

De forma predeterminada, esta etiqueta debe estar oculta, así que establezca su propiedad `Visible` en `false` en el controlador de eventos `Page_Load`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Si el usuario intenta actualizar un producto sin especificar el `UnitPrice`, deseamos cancelar la actualización y mostrar la etiqueta de advertencia. Aumente el controlador de eventos `RowUpdating` de GridView como se indica a continuación:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Si un usuario intenta guardar un producto sin especificar un precio, se cancela la actualización y se muestra un mensaje útil. Aunque la base de datos (y la lógica de negocios) permite `NULL` `UnitPrice` s, esta página de ASP.NET en particular no.

[![un usuario no puede dejar UnitPrice en blanco](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: un usuario no puede dejar `UnitPrice` en blanco ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))

Hasta ahora hemos aprendido a usar el evento `RowUpdating` de GridView para modificar mediante programación los valores de parámetro asignados a la colección de `UpdateParameters` de ObjectDataSource y cómo cancelar el proceso de actualización. Estos conceptos se llevan a los controles DetailsView y FormView y también se aplican a la inserción y eliminación.

Estas tareas también se pueden realizar en el nivel de ObjectDataSource a través de los controladores de eventos para sus eventos `Inserting`, `Updating`y `Deleting`. Estos eventos se activan antes de que se invoque el método asociado del objeto subyacente y proporcionan una oportunidad de última oportunidad de modificar la colección de parámetros de entrada o cancelar la operación. A los controladores de eventos de estos tres eventos se les pasa un objeto de tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tiene dos propiedades de interés:

- [Cancel](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, si se establece en `true`, cancela la operación que se está realizando.
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que es la colección de `InsertParameters`, `UpdateParameters`o `DeleteParameters`, dependiendo de si el controlador de eventos es para el evento `Inserting`, `Updating`o `Deleting`

Para ilustrar el trabajo con los valores de parámetro en el nivel de ObjectDataSource, vamos a incluir un DetailsView en la página que permita a los usuarios agregar un nuevo producto. Este DetailsView se usará para proporcionar una interfaz para agregar rápidamente un nuevo producto a la base de datos. Para mantener una interfaz de usuario coherente al agregar un nuevo producto, vamos a permitir que el usuario solo escriba valores para los campos `ProductName` y `UnitPrice`. De forma predeterminada, los valores que no se proporcionan en la interfaz de inserción de DetailsView se establecerán en un valor de base de datos `NULL`. Sin embargo, podemos usar el evento `Inserting` de ObjectDataSource para insertar distintos valores predeterminados, como veremos en breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Paso 3: proporcionar una interfaz para agregar nuevos productos

Arrastre un control DetailsView desde el cuadro de herramientas hasta el diseñador situado encima de GridView, borre sus propiedades `Height` y `Width` y enlácelo al ObjectDataSource que ya está presente en la página. Esto agregará BoundField o CheckBoxField para cada uno de los campos del producto. Como queremos usar este DetailsView para agregar nuevos productos, es necesario activar la opción Habilitar inserción desde la etiqueta inteligente. sin embargo, no hay ninguna opción tal que el método `Insert()` de ObjectDataSource no esté asignado a un método de la clase `ProductsBLL` (Recuerde que establecemos esta asignación en (ninguno) al configurar el origen de datos, consulte la figura 3).

Para configurar ObjectDataSource, seleccione el vínculo configurar origen de datos de su etiqueta inteligente e inicie el asistente. La primera pantalla permite cambiar el objeto subyacente al que está enlazado ObjectDataSource; déjelo establecido en `ProductsBLL`. En la pantalla siguiente se enumeran las asignaciones de los métodos de ObjectDataSource a la del objeto subyacente. Aunque hemos indicado explícitamente que los métodos `Insert()` y `Delete()` no deben asignarse a ningún método, si va a las pestañas insertar y eliminar, verá que hay una asignación ahí. Esto se debe a que los métodos `AddProduct` y `DeleteProduct` del `ProductsBLL`usan el atributo `DataObjectMethodAttribute` para indicar que son los métodos predeterminados para `Insert()` y `Delete()`, respectivamente. Por lo tanto, el Asistente de ObjectDataSource las selecciona cada vez que se ejecuta el asistente, a menos que se especifique otro valor explícitamente.

Deje el método de `Insert()` que apunta al método `AddProduct`, pero establezca de nuevo la lista desplegable de la pestaña eliminar en (ninguno).

[![establecer la lista desplegable de la pestaña insertar en el método AddProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: establezca la lista desplegable de la pestaña insertar en el método `AddProduct` ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))

[![establecer la lista desplegable de la pestaña eliminar en (ninguna)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: establezca la lista desplegable de la pestaña eliminar en (ninguna) ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))

Después de realizar estos cambios, la sintaxis declarativa de ObjectDataSource se expandirá para incluir una `InsertParameters` colección, como se muestra a continuación:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

Al volver a ejecutar el asistente, se agrega la propiedad `OldValuesParameterFormatString`. Dedique un momento a borrar esta propiedad estableciéndolo en el valor predeterminado (`{0}`) o quitándolo por completo de la sintaxis declarativa.

Con ObjectDataSource que proporciona funcionalidades de inserción, la etiqueta inteligente de DetailsView incluirá ahora la casilla habilitar inserción. vuelva al diseñador y Active esta opción. A continuación, reduzca el objeto DetailsView para que solo tenga dos `ProductName` BoundFields y `UnitPrice`, y CommandField. En este punto, la sintaxis declarativa de DetailsView debe ser similar a la siguiente:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

La figura 16 muestra esta página cuando se ve a través de un explorador en este momento. Como puede ver, DetailsView muestra el nombre y el precio del primer producto (Chai). Lo que queremos, sin embargo, es una interfaz de inserción que proporciona un medio para que el usuario agregue rápidamente un nuevo producto a la base de datos.

[![DetailsView se representa actualmente en modo de solo lectura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: DetailsView se representa actualmente en modo de solo lectura ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))

Para mostrar DetailsView en el modo de inserción, es necesario establecer la propiedad `DefaultMode` en `Inserting`. Esto representa DetailsView en modo de inserción cuando se visita por primera vez y lo mantiene después de insertar un nuevo registro. Como se muestra en la figura 17, este tipo de DetailsView proporciona una interfaz rápida para agregar un nuevo registro.

[![DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: DetailsView proporciona una interfaz para agregar rápidamente un nuevo producto ([haga clic para ver la imagen de tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))

Cuando el usuario escribe un nombre de producto y un precio (como "Acme Water" y 1,99, como en la figura 17) y hace clic en INSERT, se genera un postback y el flujo de trabajo de inserción comienza, y culmina en un nuevo registro de producto que se va a agregar a la base de datos. DetailsView mantiene su interfaz de inserción y el control GridView se reenlaza automáticamente a su origen de datos para incluir el nuevo producto, como se muestra en la figura 18.

![El producto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: el producto "Acme Water" se ha agregado a la base de datos

Aunque el control GridView de la figura 18 no lo muestra, los campos de producto que carecen de la interfaz DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`, etc., se asignan `NULL` valores de base de datos. Para ver esto, realice los pasos siguientes:

1. Vaya al Explorador de servidores en Visual Studio
2. Expandir el nodo de base de datos `NORTHWND.MDF`
3. Haga clic con el botón secundario en el nodo tabla de base de datos `Products`
4. Seleccionar Mostrar datos de tabla

Se mostrarán todos los registros de la tabla `Products`. Como se muestra en la figura 19, todas las columnas de los nuevos productos que no sean `ProductID`, `ProductName`y `UnitPrice` tienen `NULL` valores.

[![los campos Product no proporcionados en DetailsView tienen asignados valores NULL](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: los campos de producto no proporcionados en DetailsView se asignan `NULL` valores ([haga clic para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))

Es posible que deseemos proporcionar un valor predeterminado que no sea `NULL` para uno o varios de estos valores de columna, ya sea porque `NULL` no es la mejor opción predeterminada o porque la propia columna de base de datos no permite `NULL` s. Para lograr esto, se pueden establecer mediante programación los valores de los parámetros de la colección de `InputParameters` de DetailsView. Esta asignación se puede realizar en el controlador de eventos para el evento `ItemInserting` de DetailsView o el evento `Inserting` de ObjectDataSource. Dado que ya hemos examinado el uso de los eventos de nivel anterior y posterior en el nivel de control Web de datos, vamos a explorar el uso de los eventos de ObjectDataSource esta vez.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Paso 4: asignar valores a los parámetros`CategoryID`y`SupplierID`

En este tutorial, supongamos que para nuestra aplicación al agregar un nuevo producto a través de esta interfaz, se le debe asignar un `CategoryID` y `SupplierID` valor de 1. Como se mencionó anteriormente, ObjectDataSource tiene un par de eventos anteriores y posteriores que se activan durante el proceso de modificación de datos. Cuando se invoca su método de `Insert()`, ObjectDataSource genera primero su evento `Inserting`, llama al método al que se ha asignado su método `Insert()` y, por último, genera el evento `Inserted`. El controlador de eventos `Inserting` nos ofrece una última oportunidad para ajustar los parámetros de entrada o cancelar la operación.

> [!NOTE]
> En una aplicación real, probablemente desee permitir que el usuario especifique la categoría y el proveedor, o bien elegir este valor en función de algunos criterios o lógica de negocios (en lugar de seleccionar ciegamente un identificador de 1). Independientemente de, en el ejemplo se muestra cómo establecer mediante programación el valor de un parámetro de entrada desde el evento de nivel anterior de ObjectDataSource.

Dedique un momento a crear un controlador de eventos para el evento de `Inserting` de ObjectDataSource. Observe que el segundo parámetro de entrada del controlador de eventos es un objeto de tipo `ObjectDataSourceMethodEventArgs`, que tiene una propiedad para tener acceso a la colección de parámetros (`InputParameters`) y una propiedad para cancelar la operación (`Cancel`).

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

En este punto, la propiedad `InputParameters` contiene la colección de `InsertParameters` de ObjectDataSource con los valores asignados desde DetailsView. Para cambiar el valor de uno de estos parámetros, basta con usar: `e.InputParameters["paramName"] = value`. Por lo tanto, para establecer el `CategoryID` y el `SupplierID` en los valores de 1, ajuste el controlador de eventos `Inserting` de modo que se parezca a lo siguiente:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Esta vez, al agregar un nuevo producto (por ejemplo, Acme soda), las columnas `CategoryID` y `SupplierID` del nuevo producto se establecen en 1 (vea la figura 20).

[![nuevos productos tienen los valores CategoryID y SupplierID establecidos en 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: ahora, los nuevos productos tienen sus valores `CategoryID` y `SupplierID` establecidos en 1 ([haga clic para ver la imagen a tamaño completo](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))

## <a name="summary"></a>Resumen

Durante el proceso de edición, inserción y eliminación, tanto el control Web de datos como ObjectDataSource continúan a través de una serie de eventos previos y posteriores. En este tutorial se han examinado los eventos de nivel anterior y se ha visto cómo se usan para personalizar los parámetros de entrada o cancelar la operación de modificación de datos, tanto desde el control Web de datos como desde los eventos de ObjectDataSource. En el siguiente tutorial, veremos cómo crear y usar controladores de eventos para los eventos de nivel posterior.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores de clientes potenciales de este tutorial eran Jackie Goor y Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
