---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
title: Información general sobre la edición y eliminación de datos en DataList (C#) | Microsoft Docs
author: rick-anderson
description: Aunque la lista de los recursos carece de capacidades integradas de edición y eliminación, en este tutorial veremos cómo crear una lista de DataList que admita la edición y eliminación de o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: c3b0c86e-fe98-41ee-b26f-ca38cddaa75e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 481c9a14b1ebfe36ffcddd0237701bc04266e393
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74629540"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-c"></a>Información general sobre la edición y eliminación de datos en DataList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_CS.exe) de la aplicación de ejemplo o [descarga de PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/datatutorial36cs1.pdf)

> Aunque la lista de datos carece de capacidades integradas de edición y eliminación, en este tutorial veremos cómo crear una lista de datos que admita la edición y eliminación de los datos subyacentes.

## <a name="introduction"></a>Introducción

En [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, se ha examinado cómo insertar, actualizar y eliminar datos mediante la arquitectura de la aplicación, un ObjectDataSource y los controles GridView, DetailsView y FormView. Con ObjectDataSource y estos tres controles Web de datos, la implementación de interfaces simples de modificación de datos era un complemento y estaba implicado simplemente marcando una casilla de una etiqueta inteligente. No es necesario escribir ningún código.

Desafortunadamente, la lista de DataList no tiene las capacidades integradas de edición y eliminación inherentes en el control GridView. Esta funcionalidad que falta se debe en parte del hecho de que DataList es un Relic de la versión anterior de ASP.NET, cuando los controles de origen de datos declarativos y las páginas de modificación de datos sin código no estaban disponibles. Aunque la lista de datos de ASP.NET 2,0 no ofrece las mismas funcionalidades de modificación de datos preparadas que GridView, podemos usar las técnicas ASP.NET 1. x para incluir dicha funcionalidad. Este enfoque requiere un poco de código pero, como veremos en este tutorial, el DataList tiene algunos eventos y propiedades para ayudar en este proceso.

En este tutorial, veremos cómo crear una lista de datos que admita la edición y eliminación de los datos subyacentes. En los tutoriales futuros se examinarán escenarios de edición y eliminación más avanzados, incluida la validación de campos de entrada, el control correcto de las excepciones que se producen desde el acceso a datos o las capas de lógica de negocios, etc.

> [!NOTE]
> Al igual que en DataList, el control Repeater carece de la funcionalidad de prebox para insertar, actualizar o eliminar. Aunque se puede Agregar esta funcionalidad, el DataList incluye propiedades y eventos que no se encuentran en el repetidor que simplifican la adición de estas capacidades. Por lo tanto, este tutorial y los futuros que examinen la edición y la eliminación se centrarán estrictamente en la lista de DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Paso 1: creación de las páginas web de tutoriales de edición y eliminación

Antes de empezar a explorar cómo actualizar y eliminar datos de una lista de datos, deje que en primer lugar dedique un momento a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los siguientes. Comience agregando una nueva carpeta denominada `EditDeleteDataList`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Agregar las páginas ASP.NET para los tutoriales](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image1.png)

**Figura 1**: adición de las páginas de ASP.net para los tutoriales

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `EditDeleteDataList` muestra los tutoriales de la sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image2.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image4.png))

Por último, agregue las páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de los informes maestro y detalles con los `<siteMapNode>`DataList y Repeater:

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición y eliminación de DataList.

![El mapa del sitio ahora incluye entradas para los tutoriales de edición y eliminación de DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image5.png)

**Figura 3**: ahora, el mapa del sitio incluye entradas para los tutoriales de edición y eliminación de DataList.

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Paso 2: examinar las técnicas para actualizar y eliminar datos

Editar y eliminar datos con GridView es tan sencillo porque, en segundo plano, GridView y ObjectDataSource funcionan en concierto. Tal y como se describe en el tutorial [examinar los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , cuando se hace clic en un botón actualizar de una fila, el control GridView asigna automáticamente los campos que usaron el enlace de los dos sentidos a la colección de `UpdateParameters` de su ObjectDataSource y, a continuación, invoca ese método de `Update()` de ObjectDataSource.

Lamentablemente, DataList no proporciona ninguna de estas funciones integradas. Es responsabilidad suya asegurarse de que los valores de los usuarios se asignan a los parámetros de ObjectDataSource s y que se llama a su método `Update()`. Para ayudarnos en este esfuerzo, DataList proporciona las siguientes propiedades y eventos:

- **La [propiedad`DataKeyField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  al actualizar o eliminar, es necesario poder identificar de forma única cada elemento en la lista de elementos. Establezca esta propiedad en el campo de clave principal de los datos mostrados. Al hacerlo, se rellenará la [colección de`DataKeys`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) de DataList con el valor de `DataKeyField` especificado para cada elemento de la lista de elementos.
- **El [evento`EditCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  se desencadena cuando se hace clic en un botón, LinkButton o ImageButton cuya propiedad `CommandName` está establecida en Edit.
- **El [evento`CancelCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  se desencadena cuando se hace clic en un botón, LinkButton o ImageButton cuya propiedad `CommandName` está establecida en Cancelar.
- **El [evento`UpdateCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  se desencadena cuando se hace clic en un botón, LinkButton o ImageButton cuya propiedad `CommandName` está establecida en Update.
- **El [evento`DeleteCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  se desencadena cuando se hace clic en un botón, LinkButton o ImageButton cuya propiedad `CommandName` está establecida en Delete.

Mediante el uso de estas propiedades y eventos, hay cuatro métodos que se pueden usar para actualizar y eliminar datos de la lista de datos:

1. **Con las técnicas de ASP.net 1. x** , la lista de datos existía antes de ASP.net 2,0 y ObjectDataSource y era capaz de actualizar y eliminar completamente los datos a través de medios de programación. Esta técnica quita el conjunto ObjectDataSource y requiere que enlacemos los datos a la lista de datos directamente desde el nivel de lógica de negocios, tanto en la recuperación de los datos que se van a mostrar como al actualizar o eliminar un registro.
2. Al **usar un único control ObjectDataSource en la página para seleccionar, actualizar y eliminar** , mientras que la lista de objetos no tiene las capacidades inherentes de edición y eliminación de GridView, no hay ninguna razón por la que podamos agregarlas. Con este enfoque, usamos un ObjectDataSource como en los ejemplos de GridView, pero debe crear un controlador de eventos para el evento DataList s `UpdateCommand` en el que se establecen los parámetros de ObjectDataSource s y se llama a su método `Update()`.
3. **Con un control ObjectDataSource para seleccionar, pero actualizando y eliminando directamente en el BLL** cuando se usa la opción 2, es necesario escribir un poco de código en el `UpdateCommand` evento, asignar valores de parámetro, etc. En su lugar, podemos usar el ObjectDataSource para seleccionar, pero realizar las llamadas de actualización y eliminación directamente en el BLL (como con la opción 1). En mi opinión, la actualización de datos mediante la interconexión directa con la capa BLL conduce a un código más legible que la asignación de los `UpdateParameters` de datos de origen y la llamada a su método de `Update()`.
4. El **uso de medios declarativos a través de varios ObjectDataSource** los tres enfoques anteriores requieren un poco de código. Si prefiere usar tantas sintaxis declarativa como sea posible, una última opción consiste en incluir varios ObjectDataSource en la página. El primer ObjectDataSource recupera los datos de la capa BLL y los enlaza a DataList. Para la actualización, se agrega otro ObjectDataSource, pero se agrega directamente dentro del `EditItemTemplate`de DataList. Para incluir la eliminación de la compatibilidad, se necesitará otro ObjectDataSource en el `ItemTemplate`. Con este enfoque, estos objetos ObjectDataSource insertados usan `ControlParameters` para enlazar mediante declaración los parámetros de ObjectDataSource s a los controles de entrada de usuario (en lugar de tener que especificarlos mediante programación en el controlador de eventos `UpdateCommand` de DataList). Este enfoque requiere un poco de código, por lo que es necesario llamar a los `Update()` de ObjectDataSource insertados o `Delete()` comando, pero requiere mucho menos que con los otros tres enfoques. El inconveniente es que las distintas Objectdatasources se descargan de la página, lo que resta de la legibilidad general.

Si se ve obligado a usar solo uno de estos métodos, d elegir la opción 1 porque proporciona la máxima flexibilidad y porque la lista de DataList se diseñó originalmente para dar cabida a este patrón. Mientras que DataList se amplió para trabajar con los controles de origen de datos de ASP.NET 2,0, no tiene todos los puntos de extensibilidad o características de los controles Web de datos oficiales ASP.NET 2,0 (GridView, DetailsView y FormView). Sin embargo, las opciones del 2 al 4 no se merecen.

Esto y los tutoriales de edición y eliminación futuros usarán ObjectDataSource para recuperar los datos para mostrar y dirigir las llamadas a la capa BLL para actualizar y eliminar datos (opción 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Paso 3: agregar DataList y configurar su ObjectDataSource

En este tutorial, se creará una lista de datos que muestra la información del producto y, para cada producto, proporciona al usuario la capacidad de editar el nombre y el precio y de eliminar todo el producto. En concreto, se recuperarán los registros que se van a mostrar mediante ObjectDataSource, pero se realizarán las acciones de actualización y eliminación interconectando directamente con el BLL. Antes de preocuparse por la implementación de las funcionalidades de edición y eliminación en DataList, primero debe obtener la página para mostrar los productos en una interfaz de solo lectura. Dado que hemos examinado estos pasos en los tutoriales anteriores, los seguiré rápidamente.

Para empezar, abra la página `Basics.aspx` en la carpeta `EditDeleteDataList` y, en el Vista de diseño, agregue una lista de DataList a la página. A continuación, en la etiqueta inteligente DataList s, cree un nuevo ObjectDataSource. Como estamos trabajando con los datos del producto, configúrelo para usar la clase `ProductsBLL`. Para recuperar *todos* los productos, elija el método de `GetProducts()` en la pestaña seleccionar.

[![configurar ObjectDataSource para usar la clase ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image6.png)

**Figura 4**: configuración de ObjectDataSource para usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image8.png))

[![devolver la información del producto mediante el método GetProducts ()](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image9.png)

**Figura 5**: devolver la información del producto mediante el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image11.png))

DataList, al igual que GridView, no está diseñado para insertar nuevos datos. por lo tanto, seleccione la opción (ninguno) en la lista desplegable de la pestaña insertar. Elija también (ninguno) para las pestañas actualizar y eliminar, ya que las actualizaciones y eliminaciones se realizarán mediante programación a través de la capa BLL.

[![confirmar que las listas desplegables de las pestañas de inserción, actualización y eliminación de ObjectDataSource están establecidas en (ninguno)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image12.png)

**Figura 6**: confirmación de que las listas desplegables de las pestañas de inserción, actualización y eliminación de ObjectDataSource están establecidas en (ninguno) ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image14.png))

Después de configurar ObjectDataSource, haga clic en finalizar y vuelva al diseñador. Como se ve en los ejemplos anteriores, al completar la configuración de ObjectDataSource, Visual Studio crea automáticamente un `ItemTemplate` para el DropDownList, mostrando cada uno de los campos de datos. Reemplace esta `ItemTemplate` por una que solo muestre el nombre y el precio del producto. Además, establezca la propiedad `RepeatColumns` en 2.

> [!NOTE]
> Como se describe en *información general sobre la inserción, actualización y eliminación de datos* tutorial, al modificar datos con la arquitectura ObjectDataSource, es necesario quitar la propiedad `OldValuesParameterFormatString` del marcado declarativo de ObjectDataSource s (o restablecer su valor predeterminado, `{0}`). En este tutorial, sin embargo, usamos ObjectDataSource solo para recuperar datos. Por lo tanto, no es necesario modificar el valor de la propiedad ObjectDataSource s `OldValuesParameterFormatString` (aunque no se ve afectado).

Después de reemplazar el `ItemTemplate` de lista de elementos de configuración predeterminado con uno personalizado, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample2.aspx)]

Dedique un momento a ver nuestro progreso a través de un explorador. Como se muestra en la figura 7, el DataList muestra el nombre del producto y el precio unitario de cada producto en dos columnas.

[![los nombres de los productos y los precios se muestran en una lista de columnas de dos columnas](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image15.png)

**Figura 7**: los nombres y precios de los productos se muestran en una lista de columnas de dos columnas ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image17.png))

> [!NOTE]
> DataList tiene varias propiedades que son necesarias para el proceso de actualización y eliminación, y estos valores se almacenan en el estado de vista. Por lo tanto, al compilar una lista de datos que admite la edición o eliminación de datos, es fundamental habilitar el estado de vista de DataList.  
>   
> El lector astutos puede recordar que pudimos deshabilitar el estado de vista al crear GridView editable, DetailsViews y FormViews. Esto se debe a que los controles Web de ASP.NET 2,0 pueden incluir el *Estado del control*, que es el estado persistente en los postbacks como el estado de vista, pero se considera esencial.

Deshabilitar el estado de vista en GridView simplemente omite la información de estado trivial, pero mantiene el estado de control (que incluye el estado necesario para editar y eliminar). DataList, que se ha creado en el período de tiempo de ASP.NET 1. x, no utiliza el estado de control y, por lo tanto, debe tener habilitado el estado de vista. Para obtener más información sobre el estado del control y el modo en que difiere del estado de la vista, vea estado del control [frente](https://msdn.microsoft.com/library/1whwt1k7.aspx) al estado de la vista.

## <a name="step-4-adding-an-editing-user-interface"></a>Paso 4: agregar una interfaz de usuario de edición

El control GridView se compone de una colección de campos (BoundFields, CheckBoxFields, TemplateFields, etc.). Estos campos pueden ajustar su marcado representado en función de su modo. Por ejemplo, cuando está en modo de solo lectura, un BoundField muestra su valor de campo de datos como texto. en el modo de edición, representa un control Web TextBox cuya propiedad `Text` tiene asignado el valor del campo de datos.

Por otro lado, DataList representa sus elementos mediante plantillas. Los elementos de solo lectura se representan mediante el `ItemTemplate`, mientras que los elementos del modo de edición se representan mediante el `EditItemTemplate`. En este momento, la lista de DataList tiene solo un `ItemTemplate`. Para admitir la funcionalidad de edición de nivel de elemento, es necesario agregar una `EditItemTemplate` que contiene el marcado que se va a mostrar para el elemento editable. En este tutorial, usaremos controles Web de cuadro de texto para editar el nombre y el precio unitario del producto.

El `EditItemTemplate` se puede crear de forma declarativa o a través del diseñador (mediante la selección de la opción editar plantillas de la etiqueta inteligente de DataList s). Para usar la opción editar plantillas, haga clic primero en el vínculo editar plantillas de la etiqueta inteligente y, a continuación, seleccione el elemento de `EditItemTemplate` en la lista desplegable.

[![optar por trabajar con la EditItemTemplate de DataList s](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image18.png)

**Figura 8**: opte por trabajar con la `EditItemTemplate` de DataList s ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image20.png))

A continuación, escriba en nombre de producto: y Price: y, a continuación, arrastre dos controles de cuadro de texto desde el cuadro de herramientas a la interfaz `EditItemTemplate` en el diseñador. Establezca los cuadros de texto `ID` propiedades en `ProductName` y `UnitPrice`.

[![agregar un cuadro de texto para el nombre del producto y el precio](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image21.png)

**Figura 9**: agregar un cuadro de texto para el nombre del producto y el precio ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image23.png))

Necesitamos enlazar los valores de campo de datos de producto correspondientes a las propiedades de `Text` de los dos cuadros de texto. En las etiquetas inteligentes de cuadros de texto, haga clic en el vínculo editar DataBindings y, a continuación, asocie el campo de datos correspondiente a la propiedad `Text`, como se muestra en la figura 10.

> [!NOTE]
> Al enlazar el `UnitPrice` campo de datos al campo de cuadro de texto Price s `Text`, puede aplicarle formato de valor de divisa (`{0:C}`), un número general (`{0:N}`) o dejarlo sin formato.

![Enlazar los campos de datos ProductName y UnitPrice a las propiedades Text de los cuadros de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image24.png)

**Figura 10**: enlace de los campos de datos `ProductName` y `UnitPrice` a las propiedades `Text` de los cuadros de texto

Observe que el cuadro de diálogo Editar DataBindings de la figura 10 *no* incluye la casilla de enlace de los dos sentidos que está presente al editar TemplateField en GridView o DetailsView, o una plantilla en FormView. La característica de enlace de datos bidireccional permitía que el valor especificado en el control Web de entrada se asignara automáticamente a los `InsertParameters` de ObjectDataSource correspondientes o `UpdateParameters` al insertar o actualizar datos. DataList no admite el enlace de datos bidireccional, como veremos más adelante en este tutorial, después de que el usuario realice sus cambios y esté listo para actualizar los datos, deberá tener acceso mediante programación a estos cuadros de texto `Text` propiedades y pasar sus valores al método `UpdateProduct` adecuado en la clase `ProductsBLL`.

Por último, es necesario agregar los botones actualizar y cancelar al `EditItemTemplate`. Como vimos en el [maestro/detalle mediante una lista con viñetas de registros maestros con un](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) tutorial de DataList de detalles, cuando se hace clic en un botón, LinkButton o ImageButton cuya propiedad `CommandName` está establecida en un Repeater o en una lista de datos, se genera el evento Repeater o DataList `ItemCommand`. En el caso de DataList, si la propiedad `CommandName` está establecida en un valor determinado, también se puede generar un evento adicional. Entre los valores de propiedad de `CommandName` especiales se incluyen, entre otros:

- Cancelar provoca el evento `CancelCommand`
- Editar provoca el evento `EditCommand`
- Update genera el evento `UpdateCommand`

Tenga en cuenta que estos eventos se producen *además* del evento `ItemCommand`.

Agregue a la `EditItemTemplate` dos controles Web Button, uno cuyo `CommandName` está establecido en Update y los otros s establecidos en Cancel. Después de agregar estos dos controles Web de botón, el diseñador debería tener un aspecto similar al siguiente:

[![botones agregar actualización y cancelar a la EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image25.png)

**Figura 11**: agregar botones actualizar y cancelar a la `EditItemTemplate` ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image27.png))

Con el `EditItemTemplate` completar el marcado declarativo de DataList s debería ser similar al siguiente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Paso 5: adición de la fontanería para entrar en el modo de edición

En este momento, nuestra lista de DataList tiene una interfaz de edición definida a través de su `EditItemTemplate`; sin embargo, actualmente no hay ninguna manera de que un usuario visite nuestra página para indicar que desea editar la información de un producto. Es necesario agregar un botón Editar a cada producto que, cuando se hace clic en él, representa el elemento DataList en modo de edición. Comience agregando un botón Editar al `ItemTemplate`, ya sea a través del diseñador o mediante declaración. Asegúrese de establecer la propiedad Edit Button s `CommandName` en Edit.

Después de agregar este botón de edición, dedique un momento a ver la página a través de un explorador. Con esta adición, cada lista de productos debe incluir un botón Editar.

[![botones agregar actualización y cancelar a la EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image28.png)

**Figura 12**: agregar botones actualizar y cancelar a la `EditItemTemplate` ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image30.png))

Al hacer clic en el botón, se produce un postback, pero *no* la lista de productos en modo de edición. Para que el producto sea editable, es necesario:

1. Establezca la propiedad DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice del `DataListItem` en cuyo botón Editar se hizo clic.
2. Vuelva a enlazar los datos a DataList. Cuando se vuelve a representar el DataList, el `DataListItem` cuyo `ItemIndex` corresponde a los `EditItemIndex` de lista de los que se van a representar mediante su `EditItemTemplate`.

Dado que el evento DataList s `EditCommand` se desencadena cuando se hace clic en el botón Editar, cree un controlador de eventos de `EditCommand` con el siguiente código:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample4.cs)]

El controlador de eventos `EditCommand` se pasa en un objeto de tipo `DataListCommandEventArgs` como su segundo parámetro de entrada, que incluye una referencia a la `DataListItem` cuyo botón Editar se hizo clic (`e.Item`). El controlador de eventos establece primero la `EditItemIndex` DataList s en el `ItemIndex` de la `DataListItem` editable y, a continuación, vuelve a enlazar los datos a la lista de datos llamando al método DataList s `DataBind()`.

Después de agregar este controlador de eventos, vuelva a visitar la página en un explorador. Al hacer clic en el botón Editar ahora se hace que se pueda editar el producto en el que se hace clic (consulte la figura 13).

[![hacer clic en el botón Editar permite modificar el producto](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image31.png)

**Figura 13**: al hacer clic en el botón Editar se hace que el producto sea editable ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Paso 6: guardar los cambios del usuario

Al hacer clic en los botones productos editados actualizar o cancelar no se hace nada en este momento. para agregar esta funcionalidad, es necesario crear controladores de eventos para los eventos `UpdateCommand` y `CancelCommand` de DataList. Empiece por crear el controlador de eventos de `CancelCommand`, que se ejecutará cuando se haga clic en el botón de cancelación del producto editado y se le asignará una tarea que devuelva la lista de elementos a su estado de edición previa.

Para que el elemento DataList represente todos sus elementos en el modo de solo lectura, es necesario:

1. Establezca la propiedad DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice de un índice de `DataListItem` que no existe. `-1` es una opción segura, ya que los índices de `DataListItem` se inician en `0`.
2. Vuelva a enlazar los datos a DataList. Puesto que ningún `DataListItem` `ItemIndex` se corresponde con el `EditItemIndex`de DataList, toda la lista de elementos se representará en modo de solo lectura.

Estos pasos se pueden realizar con el siguiente código de controlador de eventos:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample5.cs)]

Con esta adición, al hacer clic en el botón Cancelar se devuelve la lista de elementos de estado de edición previa.

El último controlador de eventos que necesitamos completar es el controlador de eventos `UpdateCommand`. Este controlador de eventos debe:

1. Acceder mediante programación al precio y el nombre de producto especificados por el usuario, así como a los `ProductID`de productos editados.
2. Inicie el proceso de actualización mediante una llamada a la sobrecarga de `UpdateProduct` adecuada en la clase `ProductsBLL`.
3. Establezca la propiedad DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) en el índice de un índice de `DataListItem` que no existe. `-1` es una opción segura, ya que los índices de `DataListItem` se inician en `0`.
4. Vuelva a enlazar los datos a DataList. Puesto que ningún `DataListItem` `ItemIndex` se corresponde con el `EditItemIndex`de DataList, toda la lista de elementos se representará en modo de solo lectura.

Los pasos 1 y 2 son responsables de guardar los cambios de los usuarios. los pasos 3 y 4 devuelven la lista de valores a su estado de edición previa una vez que se han guardado los cambios y son idénticos a los pasos realizados en el controlador de eventos `CancelCommand`.

Para obtener el precio y el nombre de producto actualizados, es necesario usar el método `FindControl` para hacer referencia mediante programación a los controles Web TextBox dentro de la `EditItemTemplate`. También necesitamos obtener el valor del `ProductID` del producto editado. Al enlazar inicialmente la ObjectDataSource a la lista de datos, Visual Studio asignó la propiedad DataList s `DataKeyField` al valor de la clave principal del origen de datos (`ProductID`). A continuación, este valor se puede recuperar de la colección de `DataKeys` de DataList. Dedique un momento a asegurarse de que la propiedad `DataKeyField` se establece realmente en `ProductID`.

En el código siguiente se implementan los cuatro pasos:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample6.cs)]

El controlador de eventos se inicia mediante la lectura de los `ProductID` de productos editados de la colección de `DataKeys`. A continuación, se hace referencia a los dos cuadros de texto del `EditItemTemplate` y sus `Text` propiedades almacenadas en variables locales, `productNameValue` y `unitPriceValue`. Usamos el método `Decimal.Parse()` para leer el valor del cuadro de texto `UnitPrice` de modo que si el valor especificado tiene un símbolo de divisa, todavía se puede convertir correctamente en un valor `Decimal`.

> [!NOTE]
> Los valores de los cuadros de texto `ProductName` y `UnitPrice` solo se asignan a las variables productNameValue y unitPriceValue si las propiedades Text de TextBox tienen un valor especificado. De lo contrario, se utiliza un valor de `Nothing` para las variables, lo que tiene el efecto de actualizar los datos con un valor de `NULL` de base de datos. Es decir, el código trata las cadenas vacías en los valores de `NULL` de la base de datos, que es el comportamiento predeterminado de la interfaz de edición en los controles GridView, DetailsView y FormView.

Después de leer los valores, se llama al método `ProductsBLL` Class s `UpdateProduct`, pasando el nombre del producto, el precio y el `ProductID`. El controlador de eventos se completa devolviendo la lista de bits a su estado de edición previa usando exactamente la misma lógica que en el controlador de eventos `CancelCommand`.

Una vez completados los controladores de eventos `EditCommand`, `CancelCommand`y `UpdateCommand`, un visitante puede editar el nombre y el precio de un producto. Las figuras 14-16 muestran este flujo de trabajo de edición en acción.

[![al visitar la página por primera vez, todos los productos están en modo de solo lectura.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image34.png)

**Figura 14**: al visitar la página por primera vez, todos los productos están en modo de solo lectura ([haga clic para ver la imagen de tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image36.png))

[![para actualizar el precio o el nombre de un producto, haga clic en el botón Editar.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image37.png)

**Figura 15**: para actualizar el precio o el nombre de un producto, haga clic en el botón Editar ([haga clic para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image39.png))

[![después de cambiar el valor, haga clic en actualizar para volver al modo de solo lectura.](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image40.png)

**Figura 16**: después de cambiar el valor, haga clic en actualizar para volver al modo de solo lectura ([haga clic para ver la imagen a tamaño completo](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/_static/image42.png))

## <a name="step-7-adding-delete-capabilities"></a>Paso 7: agregar funcionalidades de eliminación

Los pasos para agregar capacidades de eliminación a una lista de usuarios son similares a los de la adición de funciones de edición. En Resumen, es necesario agregar un botón eliminar al `ItemTemplate` que, cuando se hace clic en él:

1. Lee en los `ProductID` de productos correspondientes a través de la colección de `DataKeys`.
2. Realiza la eliminación mediante una llamada al método `ProductsBLL` Class s `DeleteProduct`.
3. Vuelve a enlazar los datos a DataList.

Deje que empiece por agregar un botón de eliminación al `ItemTemplate`.

Cuando se hace clic en un botón cuyo `CommandName` es editar, actualizar o cancelar, se genera el evento DataList s `ItemCommand` junto con un evento adicional (por ejemplo, al usar Edit, también se genera el evento `EditCommand`). Del mismo modo, cualquier botón, LinkButton o ImageButton de la lista de propiedades cuya propiedad `CommandName` esté establecida en Delete hace que se active el evento `DeleteCommand` (junto con `ItemCommand`).

Agregue un botón Eliminar junto al botón Editar en el `ItemTemplate`, estableciendo su propiedad `CommandName` en eliminar. Después de agregar este botón control de la lista de `ItemTemplate` sintaxis declarativa debe ser similar a la siguiente:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el evento DataList s `DeleteCommand`, mediante el código siguiente:

[!code-csharp[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-cs/samples/sample8.cs)]

Al hacer clic en el botón eliminar, se produce un postback y se activa el evento DataList s `DeleteCommand`. En el controlador de eventos, se tiene acceso al valor de `ProductID` Product s en la colección de `DataKeys`. Después, el producto se elimina llamando al método `ProductsBLL` Class s `DeleteProduct`.

Después de eliminar el producto, es importante que se vuelvan a enlazar los datos a la lista de datos (`DataList1.DataBind()`); de lo contrario, la lista de datos continuará mostrando el producto que se acaba de eliminar.

## <a name="summary"></a>Resumen

Aunque la lista de elementos no tiene el punto y hace clic en editar y eliminar la compatibilidad disfrutada por GridView, con un poco de código, se puede mejorar para incluir estas características. En este tutorial, hemos visto cómo crear una lista de dos columnas de productos que podrían eliminarse y cuyo nombre y precio se pueden editar. Agregar compatibilidad de edición y eliminación es cuestión de incluir los controles Web adecuados en el `ItemTemplate` y `EditItemTemplate`, crear los controladores de eventos correspondientes, leer los valores especificados por el usuario y clave principal, e interactuar con el nivel de lógica de negocios.

Aunque hemos agregado funcionalidades básicas de edición y eliminación a la lista de datos, carece de características más avanzadas. Por ejemplo, no hay ninguna validación de campo de entrada: Si un usuario especifica un precio de demasiado caro, `Decimal.Parse` producirá una excepción al intentar convertir en un `Decimal`demasiado caro. De forma similar, si hay un problema al actualizar los datos en las capas de lógica de negocios o de acceso a datos, el usuario verá la pantalla de error estándar. Sin ningún tipo de confirmación en el botón eliminar, es muy probable que se elimine un producto accidentalmente.

En los tutoriales futuros veremos cómo mejorar la experiencia del usuario de edición.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Zack Jones, Ken Pespisa y Randy Schmidt. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](performing-batch-updates-cs.md)
