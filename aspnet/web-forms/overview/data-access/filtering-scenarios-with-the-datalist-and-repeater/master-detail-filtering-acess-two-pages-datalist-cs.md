---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtrado de maestro y detalles en dos páginasC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo separar un informe maestro y detalles en dos páginas. En la página ' maestra ' usamos un control Repeater para presentar una lista de categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 545b24a66476c55aff88ac62d3a6528105fea6c0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631475"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtrado de maestro y detalles en dos páginas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> En este tutorial, veremos cómo separar un informe maestro y detalles en dos páginas. En la página "maestra" usamos un control Repeater para presentar una lista de categorías que, al hacer clic en ella, llevarán al usuario a la página "detalles", donde una lista de datos de dos columnas muestra los productos que pertenecen a la categoría seleccionada.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) vimos cómo mostrar los informes maestro y detalles en una sola página web mediante DropDownLists para mostrar los registros "maestros" y una lista de datos para mostrar los "detalles". Otro patrón común que se usa para los informes maestros y detallados es tener los registros maestros en una página web y los detalles en otro. En el tutorial de [filtrado maestro y detalles anterior en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) , hemos examinado este patrón con GridView para mostrar todos los proveedores del sistema. Este GridView incluía un HyperLinkField, que se representó como un vínculo a una segunda página, pasando a lo largo del `SupplierID` en QueryString. En la segunda página se usó un control GridView para enumerar los productos proporcionados por el proveedor seleccionado.

Estos informes maestros y detallados de dos páginas se pueden llevar a cabo también mediante los controles DataList y Repeater. La única diferencia es que ni los controles DataList ni Repeater proporcionan compatibilidad con el control HyperLinkField. En su lugar, debemos agregar un control Web de hipervínculo o un elemento HTML de delimitador (`<a>`) dentro del `ItemTemplate`del control. La propiedad `NavigateUrl` del hipervínculo o el atributo de `href` del delimitador se pueden personalizar mediante enfoques declarativos o mediante programación.

En este tutorial exploraremos un ejemplo en el que se enumeran las categorías de una lista con viñetas en una sola página con un control Repeater. Cada elemento de la lista incluirá el nombre y la descripción de la categoría, con el nombre de la categoría que se muestra como un vínculo a una segunda página. Al hacer clic en este vínculo, se le redirigirá al usuario a la segunda página, donde un DataList mostrará los productos que pertenecen a la categoría seleccionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Paso 1: mostrar las categorías en una lista con viñetas

El primer paso en la creación de cualquier informe maestro y detalles es empezar por mostrar los registros "maestros". Por lo tanto, la primera tarea consiste en mostrar las categorías en la página "maestra". Abra la página `CategoryListMaster.aspx` de la carpeta `DataListRepeaterFiltering`, agregue un control Repeater y, desde la etiqueta inteligente, opte por agregar un nuevo ObjectDataSource. Configure el nuevo ObjectDataSource para que tenga acceso a sus datos desde el método `GetCategories` de la clase `CategoriesBLL` (consulte la figura 1).

[![configurar ObjectDataSource para usar el método GetCategories de la clase CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figura 1**: configuración de ObjectDataSource para usar el método de `GetCategories` de la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))

A continuación, defina las plantillas del repetidor de modo que muestre cada nombre de categoría y descripción como un elemento en una lista con viñetas. Vamos a no preocuparse de tener cada categoría vinculada a la página de detalles. A continuación se muestra el marcado declarativo del Repeater y ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Con este marcado completo, dedique un momento a ver nuestro progreso a través de un explorador. Como se muestra en la figura 2, el repetidor se representa como una lista con viñetas que muestra el nombre y la descripción de cada categoría.

[![cada categoría se muestra como un elemento de lista con viñetas](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figura 2**: cada categoría se muestra como un elemento de lista con viñetas ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Paso 2: convertir el nombre de la categoría en un vínculo a la página de detalles

Para que un usuario pueda mostrar la información "details" de una categoría determinada, es necesario agregar un vínculo a cada elemento de la lista con viñetas que, al hacer clic en él, llevará al usuario a la segunda página (`ProductsForCategoryDetails.aspx`). En esta segunda página se muestran los productos de la categoría seleccionada mediante una lista de DataList. Con el fin de determinar la categoría en la que se hizo clic en el vínculo, necesitamos pasar el `CategoryID` de la categoría en la segunda página a través de algún mecanismo. La forma más sencilla y sencilla de transferir datos escalares de una página a otra es a través de QueryString, que es la opción que usaremos en este tutorial. En concreto, la página `ProductsForCategoryDetails.aspx` esperará que el valor de *`categoryID`* seleccionado se pase a través de un campo querystring denominado `CategoryID`. Por ejemplo, para ver los productos de la categoría bebidas, que tiene una `CategoryID` de 1, un usuario visitará `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para crear un hipervínculo para cada elemento de lista con viñetas en el repetidor, es necesario agregar un control Web de hipervínculo o un elemento delimitador HTML (`<a>`) al `ItemTemplate`. En los escenarios en los que el hipervínculo se muestra igual para cada fila, bastará con cualquier enfoque. En el caso de los repetidores, prefiero usar el elemento delimitador. Para usar el elemento delimitador, actualice el ItemTemplate del repetidor a:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Tenga en cuenta que el `CategoryID` se puede insertar directamente en el atributo `href` del elemento delimitador; sin embargo, para ello, hay que delimitar el valor del atributo `href` con apóstrofos (y comillas tipográficas), ya que el método `Eval` dentro del atributo `href` delimita su cadena (`"CategoryID"`) entre comillas. Como alternativa, se puede usar un control Web de hipervínculo en su lugar:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Observe cómo se anexa la parte estática de la dirección URL (`ProductsForCategoryDetails.aspx?CategoryID`) al resultado de `Eval("CategoryID")` directamente dentro de la sintaxis de DataBinding mediante la concatenación de cadenas.

Una ventaja de utilizar el control de hipervínculo es que se puede tener acceso mediante programación desde el controlador de eventos `ItemDataBound` del repetidor, si es necesario. Por ejemplo, puede que desee mostrar el nombre de la categoría como texto en lugar de como un vínculo para las categorías sin productos asociados. Esta comprobación se puede realizar mediante programación en el controlador de eventos `ItemDataBound`; en el caso de las categorías que no tienen ningún producto asociado, la propiedad `NavigateUrl` del hipervínculo se podría establecer en una cadena en blanco, lo que da lugar a la representación de un nombre de categoría determinado como texto sin formato (en lugar de como un vínculo). Consulte el tutorial [dar formato al control DataList y Repeater en función](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) de los datos para obtener más información sobre cómo dar formato al control DataList y al contenido del Repeater en función de la lógica mediante programación a través del controlador de eventos `ItemDataBound`.

Si está siguiendo, no dude en usar el elemento delimitador o el enfoque de control de hipervínculo en la página. Independientemente del enfoque, al ver la página a través de un explorador, cada nombre de categoría debe representarse como un vínculo a `ProductsForCategoryDetails.aspx`, pasando el valor de `CategoryID` aplicable (vea la figura 3).

[![los nombres de categoría ahora se vinculan a ProductsForCategoryDetails. aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figura 3**: ahora, los nombres de categoría se vinculan a `ProductsForCategoryDetails.aspx` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Paso 3: enumerar los productos que pertenecen a la categoría seleccionada

Una vez completada la página `CategoryListMaster.aspx`, estamos preparados para activar la atención sobre la implementación de la página "detalles", `ProductsForCategoryDetails.aspx`. Abra esta página, arrastre un DataList desde el cuadro de herramientas hasta el diseñador y establezca su propiedad `ID` en `ProductsInCategory`. A continuación, en la etiqueta inteligente de DataList, elija Agregar un nuevo ObjectDataSource a la página y asígnele el nombre `ProductsInCategoryDataSource`. Configúrela de modo que llame al método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`; establezca las listas desplegables de las pestañas insertar, actualizar y eliminar en (ninguna).

[![configurar ObjectDataSource para usar el método GetProductsByCategoryID (categoryID) de la clase ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figura 4**: configuración de ObjectDataSource para usar el método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))

Dado que el método `GetProductsByCategoryID(categoryID)` acepta un parámetro de entrada ( *`categoryID`* ), el asistente elegir origen de datos nos ofrece la oportunidad de especificar el origen del parámetro. Establezca el origen del parámetro en QueryString mediante el `CategoryID`QueryStringField.

[![usar el campo de cadena de cadena CategoryID como origen del parámetro](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figura 5**: usar el campo QueryString `CategoryID` como origen del parámetro ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))

Como hemos visto en los tutoriales anteriores, después de completar el Asistente para elegir orígenes de datos, Visual Studio crea automáticamente un `ItemTemplate` para la lista de datos que muestra cada nombre y valor de los campos de datos. Reemplace esta plantilla por una que muestre únicamente el nombre, el proveedor y el precio del producto. Además, establezca la propiedad `RepeatColumns` de DataList en 2. Después de estos cambios, el marcado declarativo de DataList y ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Para ver esta página en acción, inicie desde la página `CategoryListMaster.aspx`; a continuación, haga clic en un vínculo de la lista de categorías con viñetas. Si lo hace, le llevará a `ProductsForCategoryDetails.aspx`, pasando el `CategoryID` a través de QueryString. El `ProductsInCategoryDataSource` ObjectDataSource en `ProductsForCategoryDetails.aspx` obtendrá entonces solo esos productos para la categoría especificada y los mostrará en la lista de DataList, que presenta dos productos por fila. En la ilustración 6 se muestra una captura de pantalla de `ProductsForCategoryDetails.aspx` al ver las bebidas.

[![se muestran las bebidas, dos por fila](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figura 6**: se muestran las bebidas, dos por fila ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Paso 4: Mostrar información de categoría en ProductsForCategoryDetails. aspx

Cuando un usuario hace clic en una categoría de `CategoryListMaster.aspx`, se le lleva a `ProductsForCategoryDetails.aspx` y se muestran los productos que pertenecen a la categoría seleccionada. Sin embargo, en `ProductsForCategoryDetails.aspx` no hay ninguna indicación visual de la categoría seleccionada. Un usuario que destinaba hacer clic en bebidas, pero que hizo clic accidentalmente en condimentos, no tiene forma de obtener su error una vez que llega a `ProductsForCategoryDetails.aspx`. Para mitigar este posible problema, podemos Mostrar información sobre la categoría seleccionada (su nombre y descripción) en la parte superior de la página `ProductsForCategoryDetails.aspx`.

Para ello, agregue un FormView sobre el control Repeater en `ProductsForCategoryDetails.aspx`. Después, agregue un nuevo ObjectDataSource a la página desde la etiqueta inteligente de FormView denominada `CategoryDataSource` y configúrela para utilizar el método `GetCategoryByCategoryID(categoryID)` de la clase `CategoriesBLL`.

[![la información de acceso sobre la categoría a través del método GetCategoryByCategoryID (categoryID) de la clase CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figura 7**: acceso a la información sobre la categoría a través del método de `GetCategoryByCategoryID(categoryID)` de la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))

Al igual que con la `ProductsInCategoryDataSource` ObjectDataSource agregada en el paso 3, el Asistente para configurar el origen de datos del `CategoryDataSource`nos solicita un origen para el parámetro de entrada del método `GetCategoryByCategoryID(categoryID)`. Use exactamente la misma configuración que antes, estableciendo el origen del parámetro en QueryString y el valor QueryStringField en `CategoryID` (consulte la figura 5).

Después de completar el asistente, Visual Studio crea automáticamente un `ItemTemplate`, `EditItemTemplate`y `InsertItemTemplate` para FormView. Como estamos proporcionando una interfaz de solo lectura, no dude en quitar el `EditItemTemplate` y `InsertItemTemplate`. Además, no dude en personalizar el `ItemTemplate`de FormView. Después de quitar las plantillas superfluas y personalizar ItemTemplate, el marcado declarativo de FormView y ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

En la ilustración 8 se muestra una captura de pantalla al ver esta página a través de un explorador.

> [!NOTE]
> Además de FormView, también he agregado un control de hipervínculo sobre FormView que devolverá el usuario a la lista de categorías (`CategoryListMaster.aspx`). No dude en colocar este vínculo en otro lugar o en omitirlo por completo.

[![la información de la categoría se muestra ahora en la parte superior de la página](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figura 8**: la información de la categoría se muestra ahora en la parte superior de la página ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Paso 5: mostrar un mensaje si ningún producto pertenece a la categoría seleccionada

En la página `CategoryListMaster.aspx` se enumeran todas las categorías del sistema, independientemente de si hay productos asociados. Si un usuario hace clic en una categoría sin productos asociados, el objeto DataList de `ProductsForCategoryDetails.aspx` no se representará, ya que su origen de datos no tendrá ningún elemento. Como hemos visto en los tutoriales anteriores, GridView proporciona una `EmptyDataText` propiedad que se puede usar para especificar un mensaje de texto que se mostrará si no hay ningún registro en su origen de datos. Desafortunadamente, ni los controles DataList ni Repeater tienen una propiedad de este tipo.

Para mostrar un mensaje que informa al usuario de que no hay productos coincidentes para la categoría seleccionada, es necesario agregar un control etiqueta a la página cuya propiedad `Text` tenga asignado el mensaje que se mostrará en caso de que no haya productos coincidentes. A continuación, es necesario establecer mediante programación su propiedad `Visible` en función de si la lista de objetos contiene o no elementos.

Para ello, empiece agregando una etiqueta debajo de DataList. Establezca su propiedad `ID` en `NoProductsMessage` y su propiedad `Text` en "no hay productos para la categoría seleccionada..." A continuación, es necesario establecer mediante programación la propiedad `Visible` de la etiqueta en función de si los datos se han enlazado a la `ProductsInCategory` DataList. Esta asignación se debe realizar una vez que los datos se han enlazado a DataList. Para GridView, DetailsView y FormView, podríamos crear un controlador de eventos para el evento `DataBound` del control, que se activa después de que se haya completado el enlace de los mismos. Sin embargo, ni los controles DataList ni Repeater tienen un evento `DataBound` disponible.

En este ejemplo concreto, se puede asignar la propiedad `Visible` de la etiqueta en el controlador de eventos `Page_Load`, ya que los datos se asignarán a la lista de datos antes del evento `Load` de la página. Sin embargo, este enfoque no funcionaría en el caso general, ya que los datos de ObjectDataSource podrían estar enlazados a la lista de datos más adelante en el ciclo de vida de la página. Por ejemplo, si los datos mostrados se basan en el valor de otro control, como es cuando se muestra un informe principal/detalle mediante un DropDownList para contener los registros "maestros", es posible que los datos no se reenlacen al control Web de datos hasta que la fase `PreRender` del ciclo de vida de la página.

Una solución que funcionará en todos los casos es asignar la propiedad `Visible` a `False` en el controlador de eventos `ItemDataBound` (o `ItemCreated`) de DataList al enlazar un tipo de elemento de `Item` o `AlternatingItem`. En tal caso, sabemos que hay al menos un elemento de datos en el origen de datos y, por tanto, puede ocultar la etiqueta de `NoProductsMessage`. Además de este controlador de eventos, también se necesita un controlador de eventos para el evento `DataBinding` de DataList, donde se inicializa la propiedad `Visible` de la etiqueta en `True`. Puesto que el evento `DataBinding` se activa antes que los eventos de `ItemDataBound`, la propiedad `Visible` de la etiqueta se establecerá inicialmente en `True`; sin embargo, si hay elementos de datos, se establecerá en `False`. En el código siguiente se implementa esta lógica:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Todas las categorías de la base de datos Northwind están asociadas a uno o varios productos. Para probar esta característica, he ajustado manualmente la base de datos Northwind para este tutorial, reasignando todos los productos asociados a la categoría de producción (`CategoryID` = 7) a la categoría mariscos (`CategoryID` = 8). Esto puede realizarse desde el Explorador de servidores eligiendo nueva consulta y usando la siguiente instrucción de `UPDATE`:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Después de actualizar la base de datos en consecuencia, vuelva a la página `CategoryListMaster.aspx` y haga clic en el vínculo generar. Dado que ya no hay productos que pertenezcan a la categoría de producción, debería ver la "no hay productos para la categoría seleccionada..." , como se muestra en la figura 9.

[![se muestra un mensaje si no hay ningún producto que pertenezca a la categoría seleccionada](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figura 9**: se muestra un mensaje si no hay ningún producto que pertenezca a la categoría seleccionada ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))

## <a name="summary"></a>Resumen

Aunque los informes maestros y detallados pueden mostrar los registros maestro y detalle en una sola página, en muchos sitios web se separan en dos páginas Web. En este tutorial, hemos examinado cómo implementar este tipo de informe maestro y detalles con las categorías enumeradas en una lista con viñetas mediante un repetidor en la página web "maestra" y los productos asociados que aparecen en la página "detalles". Cada elemento de lista de la página web maestra contenía un vínculo a la página de detalles que pasó a lo largo del valor de `CategoryID` de la fila.

En la página de detalles, la recuperación de esos productos para el proveedor especificado se ha realizado a través del método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`. El valor del parámetro *`categoryID`* se especificó mediante declaración con el valor `CategoryID` QueryString como origen del parámetro. También veremos cómo mostrar los detalles de la categoría en la página de detalles mediante FormView y cómo mostrar un mensaje si no había ningún producto perteneciente a la categoría seleccionada.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Zack Jones y Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Siguiente](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
