---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitar la funcionalidad de modificación de datos basada en el usuario (VB) | Microsoft Docs
author: rick-anderson
description: En una aplicación web que permite a los usuarios editar datos, las distintas cuentas de usuario pueden tener privilegios de edición de datos diferentes. En este tutorial, examinaremos cómo...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: c257a930e4d27fcd42591a541e700786bf413bf0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626795"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitar la funcionalidad de modificación de datos basada en el usuario (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) de la aplicación de ejemplo o [descarga de PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> En una aplicación web que permite a los usuarios editar datos, las distintas cuentas de usuario pueden tener privilegios de edición de datos diferentes. En este tutorial, veremos cómo ajustar dinámicamente las funciones de modificación de datos en función del usuario visitante.

## <a name="introduction"></a>Introducción

Varias aplicaciones web admiten cuentas de usuario y proporcionan diferentes opciones, informes y funcionalidad en función del usuario que ha iniciado sesión. Por ejemplo, con nuestros tutoriales, es posible que desee permitir que los usuarios de las empresas proveedores inicien sesión en el sitio y actualicen información general sobre sus productos (su nombre y cantidad por unidad, quizás) junto con información de proveedores, como el nombre de su empresa. Dirección, la información de la persona de contacto, etc. Además, es posible que deseemos incluir algunas cuentas de usuario para las personas de nuestra empresa para que puedan iniciar sesión y actualizar la información del producto, como las unidades de existencias, el nivel de reordenación, etc. Nuestra aplicación web también puede permitir a los usuarios anónimos visitar (personas que no han iniciado sesión), pero limitarlas a la visualización de datos. Con este sistema de cuentas de usuario en su lugar, deseamos que los controles Web de datos de nuestras páginas de ASP.NET ofrezcan las capacidades de inserción, edición y eliminación apropiadas para el usuario que ha iniciado sesión actualmente.

En este tutorial, veremos cómo ajustar dinámicamente las funciones de modificación de datos en función del usuario visitante. En concreto, vamos a crear una página que muestra la información de los proveedores en un control DetailsView editable junto con un control GridView que muestra los productos proporcionados por el proveedor. Si el usuario que visita la página es de nuestra empresa, puede: ver la información de los proveedores. editar su dirección; y editar la información de cualquier producto proporcionado por el proveedor. No obstante, si el usuario pertenece a una determinada empresa, solo podrá ver y editar su propia información de dirección y solo podrá editar los productos que no se hayan marcado como Discontinued.

[![un usuario de nuestra empresa puede editar cualquier información de proveedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: un usuario de nuestra empresa puede editar cualquier información de proveedores ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))

[![un usuario de un proveedor determinado solo puede ver y editar su información](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: un usuario de un proveedor determinado solo puede ver y editar su información ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))

Vamos a empezar a trabajar.

> [!NOTE]
> El sistema de pertenencia de ASP.NET 2,0 s proporciona una plataforma extensible y estandarizada para crear, administrar y validar cuentas de usuario. Dado que un examen del sistema de pertenencia está fuera del ámbito de estos tutoriales, en su lugar se "falsifica" la pertenencia al permitir a los visitantes anónimos elegir si provienen de un proveedor determinado o de nuestra empresa. Para obtener más información sobre la pertenencia, consulte la serie de artículos sobre la [pertenencia a ASP.NET 2,0 s, roles y perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) .

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Paso 1: permitir que el usuario especifique sus derechos de acceso

En una aplicación web real, la información de la cuenta de usuario puede incluir si han funcionado para nuestra empresa o para un proveedor determinado, y esta información se puede acceder mediante programación desde nuestras páginas de ASP.NET una vez que el usuario ha iniciado sesión en el sitio. Esta información se podría capturar a través del sistema de roles de ASP.NET 2,0 s, como información de la cuenta de usuario a través del sistema de perfiles o a través de algunos medios personalizados.

Dado que el objetivo de este tutorial es demostrar el ajuste de las funciones de modificación de datos en función del usuario que ha iniciado sesión y no está diseñado para presentar la pertenencia a ASP.NET 2,0 s, los roles y los sistemas de perfiles, usaremos un mecanismo muy sencillo para determinar el funcionalidad para el usuario que visita la página: un DropDownList desde el que el usuario puede indicar si debe poder ver y editar cualquiera de la información de los proveedores o, como alternativa, qué información de proveedor concreta pueden ver y editar. Si el usuario indica que puede ver y editar toda la información del proveedor (valor predeterminado), puede redirigirse a todos los proveedores, editar cualquier información de dirección de los proveedores y editar el nombre y la cantidad por unidad de cualquier producto proporcionado por el proveedor seleccionado. Sin embargo, si el usuario indica que solo puede ver y editar un proveedor determinado, solo podrá ver los detalles y los productos de ese proveedor y solo podrá actualizar el nombre y la cantidad por la información de unidad de los productos que *no* se hayan suspendido.

El primer paso de este tutorial es crear este DropDownList y rellenarlo con los proveedores del sistema. Abra la página `UserLevelAccess.aspx` de la carpeta `EditInsertDelete`, agregue un DropDownList cuya propiedad `ID` esté establecida en `Suppliers`y enlace este DropDownList a un nuevo ObjectDataSource denominado `AllSuppliersDataSource`.

[![crear un nuevo ObjectDataSource denominado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: creación de un nuevo ObjectDataSource denominado `AllSuppliersDataSource` ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))

Dado que deseamos que este DropDownList incluya todos los proveedores, configure el origen ObjectDataSource para invocar el método `SuppliersBLL` Class s `GetSuppliers()`. Asegúrese también de que el método ObjectDataSource s `Update()` esté asignado al método `SuppliersBLL` de clase s `UpdateSupplierAddress`, ya que el DetailsView también lo usará el DetailsView que agregaremos en el paso 2.

Después de completar el Asistente de ObjectDataSource, complete los pasos configurando el `Suppliers` DropDownList para que muestre el `CompanyName` campo de datos y use el campo de datos `SupplierID` como valor para cada `ListItem`.

[![configurar el DropDownList de proveedores para usar los campos de datos CompanyName y SupplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configuración del `Suppliers` DropDownList para usar los campos de datos `CompanyName` y `SupplierID` ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))

En este momento, el DropDownList enumera los nombres de la compañía de los proveedores en la base de datos. Sin embargo, también es necesario incluir la opción "Mostrar/editar todos los proveedores" en el DropDownList. Para ello, establezca la propiedad `Suppliers` DropDownList s `AppendDataBoundItems` en `true` y, a continuación, agregue una `ListItem` cuya propiedad `Text` sea "Mostrar/editar todos los proveedores" y cuyo valor sea `-1`. Esto se puede agregar directamente a través del marcado declarativo o a través del diseñador. para ello, vaya a la ventana Propiedades y haga clic en los puntos suspensivos de la propiedad DropDownList s `Items`.

> [!NOTE]
> Consulte el tutorial de [*filtrado maestro y detalles con un objeto DropDownList*](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) para obtener información más detallada sobre cómo agregar un elemento Select All a un DropDownList de datos.

Una vez que se ha establecido la propiedad `AppendDataBoundItems` y se ha agregado el `ListItem`, el marcado declarativo de DropDownList es similar al siguiente:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

En la figura 5 se muestra una captura de pantalla de nuestro progreso actual, cuando se ve a través de un explorador.

[![el DropDownList de proveedores contiene una muestra todos ListItem, más una para cada proveedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: el `Suppliers` DropDownList contiene un `ListItem`Mostrar todos, más uno para cada proveedor ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))

Como queremos actualizar la interfaz de usuario inmediatamente después de que el usuario haya cambiado su selección, establezca la propiedad `Suppliers` DropDownList s `AutoPostBack` en `true`. En el paso 2, vamos a crear un control DetailsView que mostrará la información de los proveedores en función de la selección de DropDownList. Después, en el paso 3, vamos a crear un controlador de eventos para este evento de `SelectedIndexChanged` DropDownList, en el que se agregará código que enlaza la información de proveedor adecuada a DetailsView según el proveedor seleccionado.

## <a name="step-2-adding-a-detailsview-control"></a>Paso 2: agregar un control DetailsView

Permita que use un DetailsView para mostrar información del proveedor. Para el usuario que puede ver y editar todos los proveedores, DetailsView será compatible con la paginación, lo que permite al usuario recorrer la información del proveedor de un registro cada vez. Sin embargo, si el usuario trabaja para un proveedor determinado, DetailsView solo mostrará la información de un proveedor determinado y no incluirá una interfaz de paginación. En cualquier caso, DetailsView debe permitir que el usuario edite los campos de dirección, ciudad y país del proveedor.

Agregue un DetailsView a la página debajo del `Suppliers` DropDownList, establezca su propiedad `ID` en `SupplierDetails`y enlácelo al `AllSuppliersDataSource` ObjectDataSource creado en el paso anterior. A continuación, active las casillas habilitar paginación y habilitar edición de la etiqueta inteligente DetailsView s.

> [!NOTE]
> Si no ve la opción Habilitar edición en la etiqueta inteligente de DetailsView s porque no ha asignado el método ObjectDataSource s `Update()` al método `SuppliersBLL` Class s `UpdateSupplierAddress`. Tómese un momento para volver atrás y realizar este cambio de configuración, tras lo cual la opción Habilitar edición debe aparecer en la etiqueta inteligente de DetailsView.

Dado que el método de `UpdateSupplierAddress` de `SuppliersBLL` clase solo acepta cuatro parámetros: `supplierID`, `address`, `city`y `country`-modificar el BoundFields de DetailsView para que los `CompanyName` y `Phone` BoundFields sean de solo lectura. Además, quite por completo el `SupplierID` BoundField. Por último, el `AllSuppliersDataSource` ObjectDataSource tiene actualmente su propiedad `OldValuesParameterFormatString` establecida en `original_{0}`. Dedique un momento a quitar este valor de propiedad de la sintaxis declarativa, o establézcalo en el valor predeterminado, `{0}`.

Después de configurar la `SupplierDetails` DetailsView y `AllSuppliersDataSource` ObjectDataSource, tendrá el siguiente marcado declarativo:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

En este punto, se puede realizar una paginación de DetailsView y se puede actualizar la información de dirección de los proveedores seleccionados, independientemente de la selección realizada en el `Suppliers` DropDownList (vea la figura 6).

[![se puede ver la información de los proveedores y actualizar su dirección](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: se puede ver la información de todos los proveedores y se actualiza su dirección ([haga clic para ver la imagen a tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Paso 3: mostrar solo la información de proveedor seleccionada

En la página se muestra actualmente la información de todos los proveedores, con independencia de si se ha seleccionado un proveedor determinado en el `Suppliers` DropDownList. Para mostrar solo la información de proveedor para el proveedor seleccionado, es necesario agregar otro origen ObjectDataSource a nuestra página, uno que recupera información sobre un proveedor determinado.

Agregue un nuevo ObjectDataSource a la página y asígnele el nombre `SingleSupplierDataSource`. En su etiqueta inteligente, haga clic en el vínculo configurar origen de datos y haga que use el método `SuppliersBLL` Class s `GetSupplierBySupplierID(supplierID)`. Como con el `AllSuppliersDataSource` ObjectDataSource, tenga el método `SingleSupplierDataSource` ObjectDataSource s `Update()` asignado al método `UpdateSupplierAddress` de la clase `SuppliersBLL`.

[![configurar el origen de SingleSupplierDataSource para que use el método GetSupplierBySupplierID (supplierID)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configuración del `SingleSupplierDataSource` ObjectDataSource para usar el método `GetSupplierBySupplierID(supplierID)` ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))

A continuación, se le pedirá que especifique el origen del parámetro para el `GetSupplierBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Puesto que queremos mostrar la información del proveedor seleccionado en DropDownList, use la propiedad `Suppliers` DropDownList s `SelectedValue` como origen del parámetro.

[![usar el DropDownList Suppliers como el origen del parámetro supplierID](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: uso de la `Suppliers` DropDownList como el origen del parámetro `supplierID` ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))

Incluso con este segundo ObjectDataSource agregado, el control DetailsView está configurado actualmente para usar siempre el `AllSuppliersDataSource` ObjectDataSource. Es necesario agregar lógica para ajustar el origen de datos que utiliza DetailsView en función del elemento `Suppliers` DropDownList seleccionado. Para ello, cree un controlador de eventos `SelectedIndexChanged` para el DropDownList de proveedores. Esto se puede crear con más facilidad haciendo doble clic en el DropDownList en el diseñador. Este controlador de eventos debe determinar qué origen de datos usar y debe volver a enlazar los datos a DetailsView. Esto se logra con el código siguiente:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

El controlador de eventos comienza determinando si se seleccionó la opción "Mostrar/editar todos los proveedores". Si es así, establece el `SupplierDetails` DetailsView s `DataSourceID` en `AllSuppliersDataSource` y devuelve al usuario al primer registro del conjunto de proveedores estableciendo la propiedad `PageIndex` en 0. Sin embargo, si el usuario ha seleccionado un proveedor determinado desde el DropDownList, se asigna el `DataSourceID` DetailsView s a `SingleSuppliersDataSource`. Independientemente del origen de datos que se use, el modo de `SuppliersDetails` se revierte al modo de solo lectura y los datos se vuelven a enlazar a DetailsView mediante una llamada al método `DataBind()` de control `SuppliersDetails`.

Con este controlador de eventos en su lugar, el control DetailsView muestra ahora el proveedor seleccionado, a menos que se haya seleccionado la opción "Mostrar/editar todos los proveedores", en cuyo caso todos los proveedores pueden verse a través de la interfaz de buscapersonas. En la ilustración 9 se muestra la página con la opción "Mostrar/editar todos los proveedores" seleccionada; Tenga en cuenta que la interfaz de paginación está presente, lo que permite al usuario visitar y actualizar cualquier proveedor. En la figura 10 se muestra la página con el proveedor MA Maison seleccionado. En este caso, solo se puede ver y editar la información de ma Maison s.

[![se puede ver y editar toda la información de los proveedores](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: se puede ver y editar toda la información de los proveedores ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))

[![solo se puede ver y editar la información de los proveedores seleccionados](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: solo se puede ver y editar la información de los proveedores seleccionados ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))

> [!NOTE]
> En este tutorial, los controles DropDownList y DetailsView `EnableViewState` deben establecerse en `true` (valor predeterminado) porque los cambios de la propiedad DropDownList s `SelectedIndex` y DetailsView s `DataSourceID` s se deben recordar en los postbacks.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Paso 4: enumeración de los productos de los proveedores en un GridView modificable

Con el control DetailsView completo, el paso siguiente consiste en incluir un control GridView modificable que muestre los productos proporcionados por el proveedor seleccionado. Este control GridView debe permitir las modificaciones solo en los campos `ProductName` y `QuantityPerUnit`. Además, si el usuario que visita la página es de un proveedor determinado, solo debe permitir actualizaciones de los productos que *no* se hayan suspendido. Para ello, primero es necesario agregar una sobrecarga del método `ProductsBLL` Class s `UpdateProducts` que toma solo los campos `ProductID`, `ProductName`y `QuantityPerUnit` como entradas. Seguimos paso a paso por este proceso de antemano en numerosos tutoriales, por lo que basta con examinar el código aquí, que debe agregarse a `ProductsBLL`:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Con esta sobrecarga creada, se vuelve a preparar para agregar el control GridView y su ObjectDataSource asociado. Agregue un nuevo GridView a la página, establezca su propiedad `ID` en `ProductsBySupplier`y configúrela para utilizar un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource`. Dado que queremos que este GridView muestre los productos por el proveedor seleccionado, use el método `ProductsBLL` Class s `GetProductsBySupplierID(supplierID)`. Asigne también el método `Update()` a la nueva sobrecarga de `UpdateProduct` que acabamos de crear.

[![configurar ObjectDataSource para usar la sobrecarga UpdateProduct que acaba de crear](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configuración de ObjectDataSource para usar la `UpdateProduct` sobrecarga que se acaba de crear ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))

Se le pedirá que seleccione el origen del parámetro para el `GetProductsBySupplierID(supplierID)` método s `supplierID` parámetro de entrada. Dado que deseamos mostrar los productos para el proveedor seleccionado en DetailsView, use la propiedad `SuppliersDetails` DetailsView control s `SelectedValue` como origen del parámetro.

[![usar la propiedad SelectedValue de SuppliersDetails DetailsView s como el origen del parámetro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: usar la propiedad `SuppliersDetails` DetailsView s `SelectedValue` como el origen del parámetro ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))

Al volver a GridView, quite todos los campos de GridView excepto `ProductName`, `QuantityPerUnit`y `Discontinued`, marcando el `Discontinued` CheckBoxField como de solo lectura. Además, active la opción Habilitar edición de la etiqueta inteligente de GridView s. Una vez realizados estos cambios, el marcado declarativo para GridView e ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Como con el ObjectDataSource anterior, esta propiedad de uno `OldValuesParameterFormatString` se establece en `original_{0}`, lo que provocará problemas al intentar actualizar un nombre de producto o cantidad por unidad. Quite esta propiedad de la sintaxis declarativa, o establézcala en su valor predeterminado, `{0}`.

Una vez finalizada esta configuración, la página muestra ahora los productos proporcionados por el proveedor seleccionado en GridView (consulte la figura 13). Actualmente se puede actualizar *cualquier* nombre de producto o cantidad por unidad. Sin embargo, es necesario actualizar la lógica de página para que esta funcionalidad esté prohibida para los productos descontinuados para los usuarios asociados a un proveedor determinado. Abordaremos esta parte final en el paso 5.

[![se muestran los productos proporcionados por el proveedor seleccionado](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: se muestran los productos proporcionados por el proveedor seleccionado ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))

> [!NOTE]
> Con la adición de este GridView modificable, el controlador de eventos `Suppliers` DropDownList s `SelectedIndexChanged` debe actualizarse para devolver GridView a un estado de solo lectura. De lo contrario, si se selecciona un proveedor diferente mientras se está editando la información del producto, también se podrá editar el índice correspondiente de GridView para el nuevo proveedor. Para evitarlo, simplemente establezca la propiedad GridView s `EditIndex` en `-1` en el controlador de eventos `SelectedIndexChanged`.

Además, recuerde que es importante que esté habilitado el estado de vista de GridView s (comportamiento predeterminado). Si establece la propiedad GridView s `EnableViewState` en `false`, corre el riesgo de que los usuarios simultáneos eliminen o editen registros involuntariamente. Vea [ADVERTENCIA: problema de simultaneidad con ASP.NET 2,0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Paso 5: no permitir la edición de productos descontinuados cuando Mostrar/editar todos los proveedores no está seleccionado

Aunque el `ProductsBySupplier` GridView es totalmente funcional, actualmente concede demasiado acceso a los usuarios que provienen de un proveedor determinado. Según nuestras reglas de negocios, estos usuarios no deben ser capaces de actualizar los productos que no se han suspendido. Para aplicar esto, podemos ocultar (o deshabilitar) el botón Editar de esas filas de GridView con productos descontinuos cuando un usuario visita la página a un proveedor.

Cree un controlador de eventos para el evento GridView s `RowDataBound`. En este controlador de eventos, es necesario determinar si el usuario está asociado a un proveedor determinado, que, para este tutorial, se puede determinar mediante la comprobación de la propiedad Suppliers DropDownList s `SelectedValue`: si es un valor distinto de-1, el usuario está asociado a un proveedor determinado. Para estos usuarios, es necesario determinar si el producto está descontinuado. Se puede obtener una referencia a la instancia de `ProductRow` real enlazada a la fila de GridView a través de la propiedad `e.Row.DataItem`, como se describe en el tutorial de [*visualización de información de resumen en el pie de página de GridView*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) . Si el producto ya no está disponible, podemos obtener una referencia mediante programación al botón Editar de GridView s CommandField con las técnicas descritas en el tutorial anterior, [*Agregar confirmación del lado cliente al eliminar*](adding-client-side-confirmation-when-deleting-vb.md). Una vez que tenemos una referencia, podemos ocultar o deshabilitar el botón.

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Con este controlador de eventos en su lugar, cuando visite esta página como un usuario de un proveedor específico, los productos que se hayan suspendido no serán editables, ya que el botón Editar está oculto para estos productos. Por ejemplo, chef Anton s Gumbo Mix es un producto discontinuo de la Nueva Orleans Cajun inlights. Al visitar la página de este proveedor en particular, el botón Editar de este producto se oculta de la vista (vea la figura 14). Sin embargo, al visitar con "Mostrar/editar todos los proveedores", el botón Editar está disponible (vea la figura 15).

[![para usuarios específicos del proveedor, se oculta el botón de edición de la combinación de chef Anton s gumbo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: para usuarios específicos del proveedor, el botón Editar de la combinación de chef Anton s Gumbo está oculto ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))

[![para los usuarios Mostrar/editar todos los proveedores, se muestra el botón Editar de chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: para los usuarios Mostrar/editar todos los proveedores, se muestra el botón Editar para la combinación de chef Anton s gumbo ([haga clic para ver la imagen de tamaño completo](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Comprobar los derechos de acceso en el nivel de lógica de negocios

En este tutorial, la página ASP.NET controla toda la lógica con respecto a la información que el usuario puede ver y qué productos puede actualizar. Idealmente, esta lógica también estará presente en el nivel de lógica de negocios. Por ejemplo, el método `SuppliersBLL` Class s `GetSuppliers()` (que devuelve todos los proveedores) puede incluir una comprobación para asegurarse de que el usuario que ha iniciado sesión actualmente *no* está asociado a un proveedor específico. Del mismo modo, el método de `UpdateSupplierAddress` podría incluir una comprobación para asegurarse de que el usuario que ha iniciado sesión actualmente ha funcionado en nuestra empresa (y, por tanto, puede actualizar la información de dirección de todos los proveedores) o está asociado con el proveedor cuyos datos se están actualizando.

No incluía estas comprobaciones de nivel de BLL aquí porque en nuestro tutorial los derechos de usuario se determinan mediante un DropDownList en la página, al que las clases de BLL no pueden tener acceso. Cuando se usa el sistema de pertenencia o uno de los esquemas de autenticación preestablecidos proporcionados por ASP.NET (como la autenticación de Windows), se puede tener acceso a la información de la información y los roles del usuario que ha iniciado sesión en ese momento desde la capa BLL, con lo que se obtiene dicho acceso. comprobaciones de derechos posibles en los niveles de presentación y de BLL.

## <a name="summary"></a>Resumen

La mayoría de los sitios que proporcionan cuentas de usuario necesitan personalizar la interfaz de modificación de datos basada en el usuario que ha iniciado sesión. Es posible que los usuarios administrativos puedan eliminar y editar cualquier registro, mientras que los usuarios no administrativos pueden limitarse únicamente a actualizar o eliminar registros creados por ellos mismos. Sea cual sea el escenario, los controles Web de datos, ObjectDataSource y las clases de capa de lógica de negocios se pueden extender para agregar o denegar ciertas funciones basadas en el usuario que ha iniciado sesión. En este tutorial, hemos visto cómo limitar los datos visibles y editables en función de si el usuario estaba asociado a un proveedor determinado o si ha trabajado para nuestra empresa.

En este tutorial se concluye el examen de la inserción, actualización y eliminación de datos mediante los controles GridView, DetailsView y FormView. A partir del siguiente tutorial, nos centraremos en la incorporación de compatibilidad con la paginación y la ordenación.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-vb.md)
