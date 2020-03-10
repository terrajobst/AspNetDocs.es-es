---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Información general sobre la inserción, actualización y eliminación de datos (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo asignar los métodos Insert (), Update () y Delete () de ObjectDataSource a los métodos de las clases BLL, así como cómo configuración...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e26b8e841f86272a158b0c09b62ab2790d01d191
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78479365"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Información general sobre la inserción, actualización y eliminación de datos (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) de la aplicación de ejemplo o [descarga de PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> En este tutorial, veremos cómo asignar los métodos Insert (), Update () y Delete () de ObjectDataSource a los métodos de las clases de BLL, y cómo configurar los controles GridView, DetailsView y FormView para proporcionar funciones de modificación de datos.

## <a name="introduction"></a>Introducción

En los últimos tutoriales, hemos examinado cómo mostrar los datos en una página de ASP.NET con los controles GridView, DetailsView y FormView. Estos controles simplemente funcionan con los datos que se les proporcionan. Normalmente, estos controles obtienen acceso a los datos mediante el uso de un control de origen de datos, como ObjectDataSource. Hemos visto cómo la ObjectDataSource actúa como un proxy entre la página ASP.NET y los datos subyacentes. Cuando un control GridView necesita Mostrar datos, invoca el método `Select()` de ObjectDataSource, que a su vez invoca un método de nuestra capa de lógica de negocios (BLL), que llama a un método en el TableAdapter de la capa de acceso a datos (DAL) adecuado, que a su vez envía una consulta de `SELECT` a la base de datos Northwind.

Recuerde que cuando creamos los TableAdapters en el DAL en [nuestro primer tutorial](../introduction/creating-a-data-access-layer-cs.md), Visual Studio agregó automáticamente métodos para insertar, actualizar y eliminar datos de la tabla de base de datos subyacente. Además, al [crear una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md) , hemos diseñado métodos en el BLL que llamaron en estos métodos dal de modificación de datos.

Además de su método `Select()`, ObjectDataSource también tiene `Insert()`, `Update()`y métodos de `Delete()`. Al igual que el método `Select()`, estos tres métodos pueden asignarse a métodos en un objeto subyacente. Cuando se configura para insertar, actualizar o eliminar datos, los controles GridView, DetailsView y FormView proporcionan una interfaz de usuario para modificar los datos subyacentes. Esta interfaz de usuario llama a los métodos `Insert()`, `Update()`y `Delete()` de ObjectDataSource, que luego invocan los métodos asociados del objeto subyacente (vea la ilustración 1).

[![los métodos Insert (), Update () y Delete () de ObjectDataSource sirven como proxy en el BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: los métodos `Insert()`, `Update()`y `Delete()` de ObjectDataSource sirven como proxy en la capa BLL ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))

En este tutorial, veremos cómo asignar los métodos `Insert()`, `Update()`y `Delete()` de ObjectDataSource a los métodos de clases de la capa BLL y cómo configurar los controles GridView, DetailsView y FormView para proporcionar funciones de modificación de datos.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Paso 1: crear páginas web de tutoriales de inserción, actualización y eliminación

Antes de empezar a explorar cómo insertar, actualizar y eliminar datos, primero vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los siguientes. Comience agregando una nueva carpeta denominada `EditInsertDelete`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Agregar las páginas ASP.NET para los tutoriales relacionados con la modificación de datos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: adición de las páginas de ASP.net para los tutoriales relacionados con la modificación de datos

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `EditInsertDelete` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta el Vista de diseño de la página.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))

Por último, agregue las páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después del `<siteMapNode>`de formato personalizado:

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición, inserción y eliminación.

![El mapa del sitio ahora incluye entradas para los tutoriales de edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: ahora, el mapa del sitio incluye entradas para los tutoriales de edición, inserción y eliminación.

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Paso 2: agregar y configurar el control ObjectDataSource

Dado que GridView, DetailsView y FormView difieren en su diseño y capacidades de modificación de datos, vamos a examinar cada uno individualmente. Sin embargo, en lugar de tener cada control que use su propio ObjectDataSource, vamos a crear un único ObjectDataSource que los tres ejemplos de control pueden compartir.

Abra la página `Basics.aspx`, arrastre un ObjectDataSource desde el cuadro de herramientas hasta el diseñador y haga clic en el vínculo configurar origen de datos de su etiqueta inteligente. Dado que el `ProductsBLL` es la única clase BLL que proporciona métodos de edición, inserción y eliminación, configure el origen ObjectDataSource para utilizar esta clase.

[![configurar ObjectDataSource para usar la clase ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configuración de ObjectDataSource para usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))

En la siguiente pantalla se pueden especificar los métodos de la clase `ProductsBLL` que se asignan a las `Select()`, `Insert()`, `Update()`y `Delete()` de ObjectDataSource seleccionando la pestaña adecuada y eligiendo el método en la lista desplegable. Figura 6, que debería ser familiar ahora, asigna el método `Select()` de ObjectDataSource al método `GetProducts()` de la clase `ProductsBLL`. Los métodos `Insert()`, `Update()`y `Delete()` se pueden configurar seleccionando la pestaña correspondiente en la lista de la parte superior.

[![que ObjectDataSource devuelva todos los productos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Ilustración 6**: hacer que ObjectDataSource devuelva todos los productos ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))

Las figuras 7, 8 y 9 muestran las pestañas de actualización, inserción y eliminación de ObjectDataSource. Configure estas pestañas para que los métodos `Insert()`, `Update()`y `Delete()` invoquen los métodos `UpdateProduct`, `AddProduct`y `DeleteProduct` de la clase `ProductsBLL`, respectivamente.

[![asignar el método Update () de ObjectDataSource al método UpdateProduct de la clase ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: asignación del método `Update()` de ObjectDataSource al método `UpdateProduct` de la clase `ProductBLL` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))

[![asignar el método Insert () de ObjectDataSource al método AddProduct de la clase ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: asignación del método `Insert()` de ObjectDataSource al método Add `Product` de la clase `ProductBLL` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))

[![asignar el método Delete () de ObjectDataSource al método DeleteProduct de la clase ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: asignación del método `Delete()` de ObjectDataSource al método `DeleteProduct` de la clase `ProductBLL` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))

Es posible que haya observado que las listas desplegables de las pestañas actualizar, insertar y eliminar ya tenían estos métodos seleccionados. Esto se debe al uso de la `DataObjectMethodAttribute` que decora los métodos de `ProductsBLL`. Por ejemplo, el método DeleteProduct tiene la siguiente firma:

[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

El atributo `DataObjectMethodAttribute` indica el propósito de cada método si es para seleccionar, insertar, actualizar o eliminar, y si es o no el valor predeterminado. Si omitió estos atributos al crear las clases de BLL, deberá seleccionar manualmente los métodos de las pestañas actualizar, insertar y eliminar.

Después de asegurarse de que los métodos de `ProductsBLL` adecuados están asignados a los métodos `Insert()`, `Update()`y `Delete()` de ObjectDataSource, haga clic en finalizar para completar el asistente.

## <a name="examining-the-objectdatasources-markup"></a>Examinar el marcado de ObjectDataSource

Después de configurar ObjectDataSource mediante su asistente, vaya a la vista de código fuente para examinar el marcado declarativo generado. La etiqueta `<asp:ObjectDataSource>` especifica el objeto subyacente y los métodos que se van a invocar. Además, hay `DeleteParameters`, `UpdateParameters`y `InsertParameters` que se asignan a los parámetros de entrada de los métodos `AddProduct`, `UpdateProduct`y `DeleteProduct` de la clase `ProductsBLL`:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource incluye un parámetro para cada uno de los parámetros de entrada de sus métodos asociados, al igual que una lista de `SelectParameter` s está presente cuando ObjectDataSource está configurado para llamar a un método SELECT que espera un parámetro de entrada (por ejemplo, `GetProductsByCategoryID(categoryID)`). Como veremos en breve, GridView, DetailsView y FormView establecen automáticamente los valores de estos `DeleteParameters`, `UpdateParameters`y `InsertParameters` antes de invocar el método de `Insert()`, `Update()`o `Delete()` de ObjectDataSource. Estos valores también se pueden establecer mediante programación según sea necesario, como veremos en un tutorial futuro.

Un efecto secundario del uso del Asistente para configurar en ObjectDataSource es que Visual Studio establece la [propiedad OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) en `original_{0}`. Este valor de propiedad se usa para incluir los valores originales de los datos que se están editando y es útil en dos escenarios:

- Si, al editar un registro, los usuarios pueden cambiar el valor de la clave principal. En este caso, se deben proporcionar el nuevo valor de clave principal y el valor de clave principal original para que se pueda encontrar el registro con el valor de clave principal original y se actualice su valor en consecuencia.
- Al usar la simultaneidad optimista. La simultaneidad optimista es una técnica para asegurarse de que dos usuarios simultáneos no sobrescriben los cambios de los demás y es el tema de un tutorial futuro.

La propiedad `OldValuesParameterFormatString` indica el nombre de los parámetros de entrada en los métodos de actualización y eliminación del objeto subyacente para los valores originales. Trataremos esta propiedad y su finalidad con mayor detalle cuando Exploremos la simultaneidad optimista. Ahora lo hago, ya que los métodos de la capa BLL no esperan los valores originales y, por lo tanto, es importante que quitemos esta propiedad. Si se deja la propiedad `OldValuesParameterFormatString` establecida en cualquier cosa que no sea la predeterminada (`{0}`), se producirá un error cuando un control Web de datos intente invocar los métodos `Update()` o `Delete()` de ObjectDataSource porque ObjectDataSource intentará pasar tanto el `UpdateParameters` como el `DeleteParameters` especificados, así como los parámetros de valor original.

Si esto no es tan claro en este momento, no se preocupe, examinaremos esta propiedad y su utilidad en un tutorial futuro. Por ahora, asegúrese de quitar esta declaración de propiedad completamente de la sintaxis declarativa o establecer el valor en el valor predeterminado ({0}).

> [!NOTE]
> Si simplemente borra el valor de la propiedad `OldValuesParameterFormatString` de la ventana Propiedades en el Vista de diseño, la propiedad seguirá existiendo en la sintaxis declarativa, pero se establecerá en una cadena vacía. Por desgracia, se producirá el mismo problema descrito anteriormente. Por consiguiente, quite la propiedad por completo de la sintaxis declarativa o, en el ventana Propiedades, establezca el valor en el predeterminado `{0}`.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Paso 3: agregar un control Web de datos y configurarlo para la modificación de datos

Una vez que ObjectDataSource se ha agregado a la página y configurado, estamos listos para agregar controles Web de datos a la página para mostrar los datos y proporcionar un medio para que el usuario final lo modifique. Veremos GridView, DetailsView y FormView por separado, ya que estos controles Web de datos difieren en sus capacidades y configuración de modificación de datos.

Como veremos en el resto de este artículo, la adición de compatibilidad de edición, inserción y eliminación muy básica a través de los controles GridView, DetailsView y FormView es realmente tan sencilla como activar un par de casillas. Hay muchos casos de matices y extremos en el mundo real que facilitan la aportación de esta funcionalidad que simplemente apuntar y hacer clic. Sin embargo, este tutorial se centra únicamente en probar las funcionalidades de modificación de datos simplistas. En los tutoriales futuros se examinarán los problemas que surjan sin duda en una configuración del mundo real.

## <a name="deleting-data-from-the-gridview"></a>Eliminar datos de GridView

Empiece arrastrando un control GridView desde el cuadro de herramientas hasta el diseñador. Después, enlace el ObjectDataSource a GridView seleccionándolo en la lista desplegable de la etiqueta inteligente de GridView. En este momento, el marcado declarativo de GridView será:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Enlazar GridView a ObjectDataSource a través de su etiqueta inteligente tiene dos ventajas:

- BoundFields y CheckBoxFields se crean automáticamente para cada uno de los campos devueltos por ObjectDataSource. Además, las propiedades de BoundField y CheckBoxField se establecen basándose en los metadatos del campo subyacente. Por ejemplo, los campos `ProductID`, `CategoryName`y `SupplierName` se marcan como de solo lectura en el `ProductsDataTable` y, por lo tanto, no se pueden actualizar al editar. Para dar cabida a esto, estas [propiedades de solo lectura](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) de BoundFields se establecen en `true`.
- La [propiedad DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) se asigna a los campos de clave principal del objeto subyacente. Esto es esencial al usar GridView para editar o eliminar datos, ya que esta propiedad indica el campo (o conjunto de campos) que identifica cada registro. Para obtener más información acerca de la propiedad `DataKeyNames`, consulte la [Página principal y los detalles con un control GridView de la página maestra seleccionable con un](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial de DetailView de detalles.

Aunque GridView se puede enlazar a ObjectDataSource a través de la sintaxis ventana Propiedades o declarativa, para ello es necesario agregar manualmente el marcado de BoundField y `DataKeyNames` adecuado.

El control GridView proporciona compatibilidad integrada para la edición y eliminación en el nivel de fila. Al configurar un control GridView para que admita la eliminación, se agrega una columna de botones eliminar. Cuando el usuario final hace clic en el botón eliminar de una fila determinada, se genera un postback y el control GridView realiza los pasos siguientes:

1. Se asignan los valores de `DeleteParameters` de ObjectDataSource
2. Se invoca el método de `Delete()` de ObjectDataSource, eliminando el registro especificado
3. GridView se vuelve a enlazar a la ObjectDataSource mediante la invocación de su método `Select()`

Los valores asignados a la `DeleteParameters` son los valores de los campos `DataKeyNames` de la fila en la que se hizo clic en el botón Eliminar. Por lo tanto, es fundamental que la propiedad `DataKeyNames` de GridView se establezca correctamente. Si falta, se asignará a la `DeleteParameters` un valor `null` en el paso 1, que a su vez no producirá ningún registro eliminado en el paso 2.

> [!NOTE]
> La colección de `DataKeys` se almacena en el estado de control de GridView, lo que significa que los valores de `DataKeys` se recordarán en el PostBack incluso si se ha deshabilitado el estado de vista de GridView. Sin embargo, es muy importante que el estado de vista permanezca habilitado para GridView que admiten la edición o eliminación (comportamiento predeterminado). Si establece la propiedad GridView s `EnableViewState` en `false`, el comportamiento de edición y eliminación funcionará bien para un solo usuario, pero si hay usuarios simultáneos que eliminan datos, existe la posibilidad de que estos usuarios simultáneos eliminen o modifiquen accidentalmente los registros que no deseaban. Vea la entrada de blog sobre la [ADVERTENCIA: problema de simultaneidad con ASP.NET 2,0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx)para obtener más información.

Esta misma advertencia también se aplica a DetailsViews y FormViews.

Para agregar capacidades de eliminación a un control GridView, simplemente vaya a su etiqueta inteligente y active la casilla habilitar eliminación.

![Active la casilla habilitar eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: Active la casilla habilitar eliminación

Al activar la casilla habilitar eliminación de la etiqueta inteligente, se agrega un CommandField a GridView. CommandField representa una columna en GridView con botones para realizar una o varias de las siguientes tareas: seleccionar un registro, editar un registro y eliminar un registro. Anteriormente vimos el CommandField en acción con la selección de registros en el [maestro o el detalle mediante un tutorial de GridView con detalles de DetailView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

CommandField contiene una serie de propiedades `ShowXButton` que indican qué serie de botones se muestran en el CommandField. Al activar la casilla habilitar eliminación, se agrega una CommandField cuya propiedad `ShowDeleteButton` es `true` a la colección de columnas de GridView.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

En este momento, Créalo o no, hemos terminado de agregar compatibilidad con el control GridView. Como se muestra en la figura 11, cuando visite esta página a través de un explorador, aparecerá una columna de botones de eliminación.

[![CommandField agrega una columna de botones de eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: CommandField agrega una columna de botones de eliminación ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))

Si ha creado este tutorial desde el principio, al probar esta página, al hacer clic en el botón eliminar, se producirá una excepción. Siga leyendo para obtener información sobre por qué se generaron estas excepciones y cómo corregirlas.

> [!NOTE]
> Si está siguiendo con la descarga que acompaña a este tutorial, estos problemas ya se han contabilizado. Sin embargo, le recomiendo que lea los detalles que se muestran a continuación para ayudar a identificar los problemas que pueden surgir y las soluciones alternativas adecuadas.

Si, al intentar eliminar un producto, recibe una excepción cuyo mensaje es similar a "*ObjectDataSource ' ObjectDataSource1 ' no pudo encontrar un método no genérico ' DeleteProduct ' que tenga parámetros: ProductID, original\_ProductID*", probablemente olvidó quitar la propiedad `OldValuesParameterFormatString` de ObjectDataSource. Con la propiedad `OldValuesParameterFormatString` especificada, ObjectDataSource intenta pasar los parámetros de entrada `productID` y `original_ProductID` al método `DeleteProduct`. sin embargo, `DeleteProduct`solo acepta un único parámetro de entrada, por lo tanto, la excepción. Al quitar la propiedad `OldValuesParameterFormatString` (o establecerla en `{0}`), se indica a ObjectDataSource que no intente pasar el parámetro de entrada original.

[![asegurarse de que se ha borrado la propiedad OldValuesParameterFormatString](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: Asegúrese de que se ha borrado la propiedad `OldValuesParameterFormatString` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))

Incluso si ha quitado la propiedad `OldValuesParameterFormatString`, se producirá una excepción al intentar eliminar un producto con el mensaje: "*la instrucción Delete está en conflicto con la restricción de referencia ' FK\_Order\_Details\_products '* ". La base de datos Northwind contiene una restricción FOREIGN KEY entre el `Order Details` y `Products` tabla, lo que significa que no se puede eliminar un producto del sistema si hay uno o más registros en la tabla `Order Details`. Puesto que todos los productos de la base de datos Northwind tienen al menos un registro en `Order Details`, no se pueden eliminar los productos hasta que primero se eliminen los registros de detalles de pedidos asociados del producto.

[![una restricción FOREIGN KEY prohíbe la eliminación de productos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: una restricción de clave externa prohíbe la eliminación de productos ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))

En nuestro tutorial, vamos a eliminar solo todos los registros de la tabla `Order Details`. En una aplicación real, es necesario:

- Tener otra pantalla para administrar la información de detalles del pedido
- Aumente el método `DeleteProduct` para incluir la lógica para eliminar los detalles del pedido del producto especificado.
- Modifique la consulta SQL utilizada por el TableAdapter para incluir la eliminación de los detalles del pedido del producto especificado.

Vamos a eliminar solo todos los registros de la tabla `Order Details` para eludir la restricción FOREIGN KEY. Vaya al Explorador de servidores en Visual Studio, haga clic con el botón derecho en el nodo `NORTHWND.MDF` y elija nueva consulta. A continuación, en la ventana de consulta, ejecute la siguiente instrucción SQL: `DELETE FROM [Order Details]`

[![eliminar todos los registros de la tabla Order Details](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: eliminación de todos los registros de la tabla `Order Details` ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))

Después de borrar la tabla `Order Details` al hacer clic en el botón eliminar, se eliminará el producto sin errores. Si al hacer clic en el botón eliminar no se elimina el producto, asegúrese de que la propiedad `DataKeyNames` de GridView está establecida en el campo de clave principal (`ProductID`).

> [!NOTE]
> Al hacer clic en el botón eliminar, se quita un postback y se elimina el registro. Esto puede ser peligroso, ya que es fácil hacer clic accidentalmente en el botón eliminar de una fila equivocada. En un futuro tutorial, veremos cómo agregar una confirmación del lado cliente cuando se elimina un registro.

## <a name="editing-data-with-the-gridview"></a>Editar datos con GridView

Además de eliminar, el control GridView también proporciona compatibilidad de edición de nivel de fila integrada. Al configurar un control GridView para que admita la edición, se agrega una columna de botones de edición. Desde la perspectiva del usuario final, al hacer clic en el botón de edición de una fila, la fila se vuelve editable, convirtiendo las celdas en cuadros de texto que contienen los valores existentes y reemplazando el botón Editar por los botones actualizar y cancelar. Después de realizar los cambios deseados, el usuario final puede hacer clic en el botón actualizar para confirmar los cambios o en el botón Cancelar para descartarlos. En cualquier caso, después de hacer clic en actualizar o cancelar, el control GridView vuelve a su estado de edición previa.

Desde nuestra perspectiva como desarrollador de páginas, cuando el usuario final hace clic en el botón Editar de una fila determinada, se genera un postback y GridView realiza los pasos siguientes:

1. La propiedad `EditItemIndex` de GridView está asignada al índice de la fila en cuyo botón Editar se hizo clic.
2. GridView se vuelve a enlazar a la ObjectDataSource mediante la invocación de su método `Select()`
3. El índice de fila que coincide con el `EditItemIndex` se representa en "modo de edición". En este modo, el botón Editar se sustituye por los botones actualizar y cancelar y BoundFields cuyas propiedades `ReadOnly` son false (el valor predeterminado) se representan como controles Web TextBox cuyas propiedades de `Text` se asignan a los valores de los campos de datos.

En este momento, el marcado se devuelve al explorador, lo que permite al usuario final realizar cualquier cambio en los datos de la fila. Cuando el usuario hace clic en el botón actualizar, se produce un postback y GridView realiza los pasos siguientes:

1. A los valores de `UpdateParameters` de ObjectDataSource se les asignan los valores especificados por el usuario final en la interfaz de edición de GridView.
2. Se invoca el método de `Update()` de ObjectDataSource, actualizando el registro especificado
3. GridView se vuelve a enlazar a la ObjectDataSource mediante la invocación de su método `Select()`

Los valores de clave principal asignados al `UpdateParameters` en el paso 1 proceden de los valores especificados en la propiedad `DataKeyNames`, mientras que los valores de clave no principal provienen del texto de los controles Web TextBox de la fila modificada. Al igual que con la eliminación, es fundamental que la propiedad `DataKeyNames` de GridView se establezca correctamente. Si falta, el valor de la clave principal `UpdateParameters` se asignará a un valor `null` en el paso 1, que a su vez no producirá ningún registro actualizado en el paso 2.

La funcionalidad de edición se puede activar con solo activar la casilla habilitar edición en la etiqueta inteligente de GridView.

![Active la casilla habilitar edición.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: Active la casilla habilitar edición

Al activar la casilla habilitar edición, se agregará un CommandField (si es necesario) y se establecerá su propiedad `ShowEditButton` en `true`. Como vimos anteriormente, el CommandField contiene una serie de propiedades `ShowXButton` que indican qué serie de botones se muestran en el CommandField. Al activar la casilla habilitar edición, se agrega la propiedad `ShowEditButton` al CommandField existente:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Eso es todo lo que hay que hacer para agregar compatibilidad de edición rudimentaria. Como se muestra en Figure16, la interfaz de edición es, en realidad, cada BoundField cuya propiedad `ReadOnly` está establecida en `false` (el valor predeterminado) se representa como un cuadro de texto. Esto incluye campos como `CategoryID` y `SupplierID`, que son claves de otras tablas.

[![clic en el botón de edición de Chai muestra la fila en modo de edición](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: hacer clic en el botón de edición de Chai muestra la fila en modo de edición ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))

Además de pedir a los usuarios que editen los valores de clave externa directamente, la interfaz de la interfaz de edición carece de las siguientes maneras:

- Si el usuario especifica un `CategoryID` o `SupplierID` que no existe en la base de datos, el `UPDATE` infringirá una restricción de clave externa, lo que provocará que se produzca una excepción.
- La interfaz de edición no incluye ninguna validación. Si no proporciona un valor necesario (como `ProductName`), o escriba un valor de cadena en el que se espera un valor numérico (por ejemplo, si escribe "demasiados!" en el cuadro de texto `UnitPrice`), se iniciará una excepción. En un futuro tutorial se examinará cómo agregar controles de validación a la interfaz de usuario de edición.
- Actualmente, *todos* los campos de producto que no son de solo lectura deben incluirse en GridView. Si fuera a quitar un campo de GridView, por ejemplo `UnitPrice`, al actualizar los datos, GridView no establecería el valor de `UpdateParameters` `UnitPrice`, lo que cambiaría el `UnitPrice` del registro de la base de datos a un valor `NULL`. Del mismo modo, si un campo obligatorio, como `ProductName`, se quita de GridView, se producirá un error en la actualización con la misma "*columna ' ProductName ' no permite valores NULL" que*se menciona anteriormente.
- El formato de la interfaz de edición deja mucho que desee. El `UnitPrice` se muestra con cuatro puntos decimales. Idealmente, los valores de `CategoryID` y `SupplierID` contienen DropDownLists que enumeran las categorías y los proveedores del sistema.

Estos son todos los inconvenientes con los que tendremos que vivir por ahora, pero se abordarán en futuros tutoriales.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Insertar, modificar y eliminar datos con DetailsView

Como hemos visto en los tutoriales anteriores, el control DetailsView muestra un registro cada vez y, al igual que GridView, permite editar y eliminar el registro que se muestra actualmente. La experiencia del usuario final con la edición y eliminación de elementos de DetailsView y el flujo de trabajo del lado ASP.NET es idéntica a la de GridView. Cuando DetailsView difiere de GridView es que también proporciona compatibilidad con la inserción integrada.

Para demostrar las funcionalidades de modificación de datos de GridView, empiece agregando un control DetailsView a la página `Basics.aspx` sobre el control GridView existente y enlácelo al origen de datos existente a través de la etiqueta inteligente de DetailsView. A continuación, desactive las propiedades `Height` y `Width` de DetailsView y active la opción Habilitar paginación de la etiqueta inteligente. Para habilitar la compatibilidad de edición, inserción y eliminación, solo tiene que activar las casillas habilitar edición, habilitar inserción y habilitar eliminación en la etiqueta inteligente.

![Configurar DetailsView para admitir la edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configuración de DetailsView para admitir la edición, inserción y eliminación

Al igual que con GridView, al agregar compatibilidad de edición, inserción o eliminación, se agrega un CommandField a DetailsView, como se muestra en la sintaxis declarativa siguiente:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Tenga en cuenta que para DetailsView, CommandField aparece al final de la colección de columnas de forma predeterminada. Dado que los campos de DetailsView se representan como filas, CommandField aparece como una fila con los botones insertar, editar y eliminar en la parte inferior de DetailsView.

[![configurar DetailsView para admitir la edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configuración de DetailsView para admitir la edición, inserción y eliminación ([haga clic para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))

Al hacer clic en el botón eliminar se inicia la misma secuencia de eventos que con GridView: un postback; seguido de DetailsView rellenar la `DeleteParameters` de ObjectDataSource en función de los valores de `DataKeyNames`; y se completa con una llamada al método `Delete()` de ObjectDataSource, que realmente quita el producto de la base de datos. La edición en DetailsView también funciona de forma idéntica a la de GridView.

Para la inserción, al usuario final se le presenta un botón nuevo que, cuando se hace clic en él, representa DetailsView en "modo de inserción". Con "modo de inserción", el botón nuevo se reemplaza por los botones insertar y cancelar y solo se muestran los BoundFields cuya propiedad `InsertVisible` esté establecida en `true` (valor predeterminado). Los campos de datos identificados como campos de incremento automático, como `ProductID`, tienen la [propiedad InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) establecida en `false` al enlazar DetailsView con el origen de datos a través de la etiqueta inteligente.

Al enlazar un origen de datos a DetailsView a través de la etiqueta inteligente, Visual Studio establece la propiedad `InsertVisible` en `false` solo para los campos de incremento automático. Los campos de solo lectura, como `CategoryName` y `SupplierName`, se mostrarán en la interfaz de usuario "modo de inserción" a menos que su propiedad `InsertVisible` se establezca explícitamente en `false`. Dedique unos instantes a establecer las propiedades de `InsertVisible` de estos dos campos en `false`, bien mediante la sintaxis declarativa de DetailsView o mediante el vínculo editar campos de la etiqueta inteligente. En la figura 19 se muestra cómo establecer las propiedades de `InsertVisible` en `false` haciendo clic en el vínculo editar campos.

[![Northwind Traders ofrece ahora un té de Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Northwind Traders ahora ofrece Acme té ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))

Después de establecer las propiedades de `InsertVisible`, vea la página `Basics.aspx` en un explorador y haga clic en el botón nuevo. En la figura 20 se muestra DetailsView al agregar un nuevo área de bebidas, Acme, a nuestra línea de productos.

[![Northwind Traders ofrece ahora un té de Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Northwind Traders Now ofrece Acme té ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))

Después de escribir los detalles del té de Acme y hacer clic en el botón Insertar, el PostBack aparece y se agrega el nuevo registro a la tabla de base de datos de `Products`. Dado que este objeto DetailsView muestra los productos con el orden en que se encuentran en la tabla de la base de datos, debemos paginar en el último producto para poder ver el nuevo producto.

[![detalles del té de Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: detalles del té de Acme ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))

> [!NOTE]
> La [propiedad CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) de DetailsView indica la interfaz que se muestra y puede tener uno de los valores siguientes: `Edit`, `Insert`o `ReadOnly`. La [propiedad DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica el modo en el que se devuelve DetailsView después de haber completado una edición o una inserción, y es útil para mostrar un DetailsView que esté permanentemente en modo de edición o inserción.

El punto y hacer clic en las funciones de inserción y edición de DetailsView sufren las mismas limitaciones que GridView: el usuario debe especificar los valores de `CategoryID` y `SupplierID` existentes a través de un cuadro de texto. la interfaz no tiene ninguna lógica de validación; todos los campos de producto que no permiten valores `NULL` o que no tienen un valor predeterminado especificado en el nivel de base de datos deben estar incluidos en la interfaz de inserción, y así sucesivamente.

Las técnicas que examinaremos para extender y mejorar la interfaz de edición de GridView en artículos futuros se pueden aplicar también a las interfaces de edición e inserción del control DetailsView.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Usar FormView para una interfaz de usuario de modificación de datos más flexible

FormView ofrece compatibilidad integrada para insertar, editar y eliminar datos, pero dado que usa plantillas en lugar de campos, no hay ningún lugar para agregar el BoundFields o CommandField que usan los controles GridView y DetailsView para proporcionar los datos. interfaz de modificación. En su lugar, esta interfaz los controles Web para recopilar datos proporcionados por el usuario al agregar un nuevo elemento o editar uno existente junto con los botones nuevo, editar, eliminar, insertar, actualizar y cancelar deben agregarse manualmente a las plantillas adecuadas. Afortunadamente, Visual Studio creará automáticamente la interfaz necesaria al enlazar FormView a un origen de datos a través de la lista desplegable de su etiqueta inteligente.

Para ilustrar estas técnicas, empiece agregando FormView a la página `Basics.aspx` y, desde la etiqueta inteligente de FormView, enlácelo a la ObjectDataSource ya creada. Esto generará un `EditItemTemplate`, `InsertItemTemplate`y `ItemTemplate` para FormView con controles Web de cuadro de texto para recopilar los controles Web de botones y de entrada del usuario para los botones nuevo, editar, eliminar, insertar, actualizar y cancelar. Además, la propiedad `DataKeyNames` de FormView está establecida en el campo de clave principal (`ProductID`) del objeto devuelto por ObjectDataSource. Por último, active la opción Habilitar paginación en la etiqueta inteligente de FormView.

A continuación se muestra el marcado declarativo del `ItemTemplate` de FormView una vez enlazado FormView a ObjectDataSource. De forma predeterminada, cada campo Product de valor no booleano se enlaza a la propiedad `Text` de un control Web Label mientras cada campo de valor booleano (`Discontinued`) se enlaza a la propiedad `Checked` de un control Web CheckBox deshabilitado. Para que los botones nuevo, editar y eliminar desencadenen determinados comportamientos de FormView al hacer clic en él, es imperativo que sus valores de `CommandName` se establezcan en `New`, `Edit`y `Delete`, respectivamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

En la figura 22 se muestra el `ItemTemplate` de FormView cuando se ve a través de un explorador. Cada campo de producto aparece con los botones nuevo, editar y eliminar de la parte inferior.

[![predeterminada FormView ItemTemplate enumera cada campo de producto junto con los botones nuevo, editar y eliminar](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: el `ItemTemplate` FormView de predeterminada muestra cada campo de producto junto con los botones nuevo, editar y eliminar ([haga clic para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))

Al igual que con GridView y DetailsView, al hacer clic en el botón eliminar o en cualquier botón, LinkButton o ImageButton cuya propiedad `CommandName` esté establecida en Delete, se rellena el `DeleteParameters` de ObjectDataSource basándose en el valor `DataKeyNames` de FormView y se invoca el método `Delete()` de ObjectDataSource.

Cuando se hace clic en el botón Editar, se muestra un postback y los datos se reenlazan a la `EditItemTemplate`, que es responsable de la representación de la interfaz de edición. Esta interfaz incluye los controles Web para editar datos junto con los botones actualizar y cancelar. La `EditItemTemplate` predeterminada generada por Visual Studio contiene una etiqueta para los campos de incremento automático (`ProductID`), un cuadro de texto para cada campo de valor no booleano y una casilla para cada campo de valor booleano. Este comportamiento es muy similar al BoundFields generado automáticamente en los controles GridView y DetailsView.

> [!NOTE]
> Un pequeño problema con la generación automática de FormView del `EditItemTemplate` es que representa controles Web de cuadro de texto para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Veremos cómo tener esto en cuenta en breve.

Los controles de cuadro de texto de la `EditItemTemplate` tienen su propiedad `Text` enlazada al valor de su campo de datos correspondiente mediante el *enlace de datos bidireccional*. El enlace de datos bidireccional, indicado por `<%# Bind("dataField") %>`, realiza el enlace de datos al enlazar datos a la plantilla y al rellenar los parámetros de ObjectDataSource para insertar o editar registros. Es decir, cuando el usuario hace clic en el botón Editar desde el `ItemTemplate`, el método `Bind()` devuelve el valor del campo de datos especificado. Después de que el usuario realice los cambios y haga clic en actualizar, los valores devueltos que corresponden a los campos de datos especificados mediante `Bind()` se aplican a la `UpdateParameters`de ObjectDataSource. Como alternativa, el enlace de datos unidireccional, indicado por `<%# Eval("dataField") %>`, solo recupera los valores de los campos de datos al enlazar datos a la plantilla y *no* devuelve los valores especificados por el usuario a los parámetros del origen de datos en el PostBack.

El siguiente marcado declarativo muestra la `EditItemTemplate`de FormView. Tenga en cuenta que el método `Bind()` se usa aquí en la sintaxis de DataBinding y que los controles Web del botón actualizar y cancelar tienen sus propiedades `CommandName` establecidas en consecuencia.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

En este momento, nuestro `EditItemTemplate`hará que se produzca una excepción si se intenta usar. El problema es que los campos `CategoryName` y `SupplierName` se representan como controles Web TextBox en el `EditItemTemplate`. Es necesario cambiar estos cuadros de texto a etiquetas o quitarlos por completo. Vamos a eliminarlos completamente del `EditItemTemplate`.

En la Figura 23 se muestra el FormView en un explorador después de hacer clic en el botón Editar para Chai. Tenga en cuenta que los campos `SupplierName` y `CategoryName` que se muestran en el `ItemTemplate` ya no están presentes, ya que solo se han quitado de la `EditItemTemplate`. Cuando se hace clic en el botón actualizar, FormView continúa a través de la misma secuencia de pasos que los controles GridView y DetailsView.

[![de forma predeterminada, EditItemTemplate muestra cada campo de producto editable como un cuadro de texto o una casilla.](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: de forma predeterminada, la `EditItemTemplate` muestra cada campo de producto editable como un cuadro de texto o una casilla ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))

Cuando se hace clic en el botón Insertar, se `ItemTemplate` un postback. Sin embargo, no se enlaza ningún dato a FormView porque se agrega un nuevo registro. La interfaz de `InsertItemTemplate` incluye los controles Web para agregar un nuevo registro junto con los botones insertar y cancelar. La `InsertItemTemplate` predeterminada generada por Visual Studio contiene un cuadro de texto para cada campo de valor no booleano y una casilla para cada campo de valor booleano, similar a la interfaz del `EditItemTemplate`generado automáticamente. Los controles TextBox tienen su propiedad `Text` enlazada al valor de su campo de datos correspondiente mediante el enlace de datos bidireccional.

El siguiente marcado declarativo muestra la `InsertItemTemplate`de FormView. Tenga en cuenta que el método `Bind()` se usa aquí en la sintaxis de DataBinding y que los controles Web del botón Insertar y cancelar tienen sus propiedades `CommandName` establecidas en consecuencia.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Hay un detalle en cuanto a la generación automática de FormView del `InsertItemTemplate`. En concreto, los controles Web TextBox se crean incluso para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Al igual que con el `EditItemTemplate`, es necesario quitar estos cuadros de texto de la `InsertItemTemplate`.

En la Figura 24 se muestra el FormView en un explorador al agregar un nuevo producto, Acme Coffee. Tenga en cuenta que los campos `SupplierName` y `CategoryName` que se muestran en el `ItemTemplate` ya no están presentes, ya que solo se han quitado. Cuando se hace clic en el botón Insertar, FormView continúa a través de la misma secuencia de pasos que el control DetailsView, agregando un nuevo registro a la tabla `Products`. En la figura 25 se muestran los detalles del producto Acme Coffee en FormView una vez insertado.

[![InsertItemTemplate dicta la interfaz de inserción de FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: el `InsertItemTemplate` dicta la interfaz de inserción de FormView ([haga clic para ver la imagen de tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))

[![los detalles del nuevo producto, Acme Coffee, se muestran en FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: los detalles del nuevo producto, Acme Coffee, se muestran en FormView ([haga clic para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))

Al separar las interfaces de solo lectura, edición e inserción en tres plantillas independientes, FormView permite un mayor grado de control sobre estas interfaces que DetailsView y GridView.

> [!NOTE]
> Al igual que el DetailsView, la propiedad `CurrentMode` de FormView indica la interfaz que se muestra y su propiedad `DefaultMode` indica el modo en el que FormView vuelve después de que se haya completado una instrucción Edit o INSERT.

## <a name="summary"></a>Resumen

En este tutorial, hemos examinado los aspectos básicos de la inserción, edición y eliminación de datos mediante GridView, DetailsView y FormView. Los tres controles proporcionan cierto nivel de capacidades de modificación de datos integradas que se pueden usar sin necesidad de escribir una sola línea de código en la página ASP.NET, gracias a los controles Web de datos y al ObjectDataSource. Sin embargo, las técnicas sencillas de punto y clic representan una interfaz de usuario de modificación de datos bastante Frail y Naive. Para proporcionar validación, inyectar valores de programación, controlar las excepciones correctamente, personalizar la interfaz de usuario, etc., tendremos que confiar en un serie de técnicas que se tratarán en los siguientes tutoriales.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Siguiente](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
