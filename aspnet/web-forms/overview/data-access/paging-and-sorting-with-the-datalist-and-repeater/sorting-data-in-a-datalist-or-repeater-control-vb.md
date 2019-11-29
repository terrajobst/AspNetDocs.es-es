---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Ordenar datos en un control DataList o Repeater (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo incluir la compatibilidad de ordenación en DataList y Repeater, y cómo crear una lista de datos o un repetidor cuyos datos puedan...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 81e07bec8569b9ee987dfaa84dec9eec95a2692f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74638236"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Ordenar datos en un control DataList o Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) de la aplicación de ejemplo o [descarga de PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> En este tutorial, veremos cómo incluir la compatibilidad de ordenación en DataList y Repeater, y cómo crear una lista de datos o un repetidor cuyos datos se puedan paginar y ordenar.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , hemos examinado cómo agregar compatibilidad de paginación a un DataList. Hemos creado un nuevo método en la clase `ProductsBLL` (`GetProductsAsPagedDataSource`) que devolvió un objeto `PagedDataSource`. Cuando está enlazado a una lista de controles o un repetidor, DataList o Repeater mostraría solo la página de datos solicitada. Esta técnica es similar a la que usan internamente los controles GridView, DetailsView y FormView para proporcionar su funcionalidad de paginación predeterminada integrada.

Además de ofrecer compatibilidad con la paginación, GridView también incluye compatibilidad con la ordenación de la caja. Ni DataList ni Repeater proporcionan funcionalidad de ordenación integrada; sin embargo, las características de ordenación se pueden agregar con un poco de código. En este tutorial, veremos cómo incluir la compatibilidad de ordenación en DataList y Repeater, y cómo crear una lista de datos o un repetidor cuyos datos se puedan paginar y ordenar.

## <a name="a-review-of-sorting"></a>Revisión de la ordenación

Como vimos en el tutorial de [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , el control GridView proporciona compatibilidad con la ordenación fuera del cuadro. Cada campo GridView puede tener un `SortExpression`asociado, que indica el campo de datos por el que ordenar los datos. Cuando la propiedad GridView s `AllowSorting` está establecida en `true`, cada campo de GridView que tenga un valor de propiedad `SortExpression` tiene su encabezado representado como LinkButton. Cuando un usuario hace clic en un encabezado de campo de GridView determinado, se produce un postback y los datos se ordenan de acuerdo con el `SortExpression`del campo en el que se hace clic.

El control GridView también tiene una propiedad `SortExpression`, que almacena el `SortExpression` del campo GridView por el que se ordenan los datos. Además, una propiedad `SortDirection` indica si los datos se van a ordenar en orden ascendente o descendente (si un usuario hace clic en un vínculo de encabezado s de campo de GridView determinado dos veces consecutivas, se alterna el criterio de ordenación).

Cuando GridView se enlaza a su control de origen de datos, entrega sus propiedades `SortExpression` y `SortDirection` al control de origen de datos. El control de origen de datos recupera los datos y, a continuación, los ordena según las propiedades `SortExpression` y `SortDirection` proporcionadas. Después de ordenar los datos, el control de origen de datos lo devuelve a GridView.

Para replicar esta funcionalidad con los controles DataList o Repeater, es necesario:

- Crear una interfaz de ordenación
- Recuerde el campo de datos por el que ordenar y si desea ordenar en orden ascendente o descendente.
- Indicar a ObjectDataSource que ordene los datos por un campo de datos determinado

Abordaremos estas tres tareas en los pasos 3 y 4. A continuación, examinaremos cómo incluir la compatibilidad con la paginación y la ordenación en una lista de elementos o un repetidor.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Paso 2: mostrar los productos en un repetidor

Antes de preocuparse por la implementación de alguna de las funcionalidades relacionadas con la ordenación, deje que se empiece por enumerar los productos en un control Repeater. Para empezar, abra la página `Sorting.aspx` de la carpeta `PagingSortingDataListRepeater`. Agregue un control Repeater a la página web, estableciendo su propiedad `ID` en `SortableProducts`. En la etiqueta inteligente Repeater s, cree un nuevo ObjectDataSource denominado `ProductsDataSource` y configúrelo para recuperar datos del método `ProductsBLL` Class s `GetProducts()`. Seleccione la opción (ninguno) en las listas desplegables de las pestañas insertar, actualizar y eliminar.

[![crear un ObjectDataSource y configurarlo para que use el método GetProductsAsPagedDataSource ()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: crear un ObjectDataSource y configurarlo para usar el método `GetProductsAsPagedDataSource()` ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Figura 2**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))

A diferencia de la lista de datos, Visual Studio no crea automáticamente un `ItemTemplate` para el control Repeater después de enlazarlo a un origen de datos. Además, debemos agregar este `ItemTemplate` mediante declaración, ya que la etiqueta inteligente control de repetidor no tiene la opción editar plantillas que se encuentra en la lista de elementos. Permita que use la misma `ItemTemplate` del tutorial anterior, que muestra el nombre, el proveedor y la categoría del producto.

Después de agregar el `ItemTemplate`, el marcado declarativo de Repeater y ObjectDataSource s debe ser similar al siguiente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

En la ilustración 3 se muestra esta página cuando se ve a través de un explorador.

[![se muestra cada nombre de producto, proveedor y categoría](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 3**: se muestra cada nombre de producto, proveedor y categoría ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Paso 3: indicar a ObjectDataSource que ordene los datos

Para ordenar los datos que se muestran en el repetidor, es necesario informar al origen ObjectDataSource de la expresión de ordenación por la que se deben ordenar los datos. Antes de que ObjectDataSource recupere sus datos, primero desencadena su [evento`Selecting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), lo que proporciona una oportunidad para que podamos especificar una expresión de ordenación. Al controlador de eventos `Selecting` se le pasa un objeto de tipo [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tiene una propiedad denominada [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) de tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). La clase `DataSourceSelectArguments` está diseñada para pasar solicitudes relacionadas con datos de un consumidor de datos al control de origen de datos e incluye una [propiedad`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para pasar información de ordenación de la página ASP.NET a ObjectDataSource, cree un controlador de eventos para el evento `Selecting` y use el código siguiente:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

El valor de *sortExpression* debe tener asignado el nombre del campo de datos para ordenar los datos (como NombreProducto). No hay ninguna propiedad relacionada con la dirección de ordenación, por lo que si desea ordenar los datos en orden descendente, anexe la cadena DESC al valor de *sortExpression* (como NombreProducto DESC).

Continúe y pruebe los distintos valores codificados de forma rígida para *sortExpression* y pruebe los resultados en un explorador. Como se muestra en la figura 4, cuando se usa ProductName DESC como *sortExpression*, los productos se ordenan por su nombre en orden alfabético inverso.

[![los productos se ordenan por su nombre en orden alfabético inverso](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 4**: los productos se ordenan por su nombre en orden alfabético inverso ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Paso 4: crear la interfaz de ordenación y recordar la expresión y la dirección de ordenación

La activación de la compatibilidad de ordenación en GridView convierte cada texto de encabezado de campo que se ordene en un LinkButton que, cuando se hace clic en él, ordena los datos según corresponda. Dicha interfaz de ordenación tiene sentido para GridView, donde sus datos se colocan de forma ordenada en las columnas. En el caso de los controles DataList y Repeater, sin embargo, se necesita una interfaz de ordenación diferente. Una interfaz de ordenación común para una lista de datos (en lugar de una cuadrícula de datos) es una lista desplegable que proporciona los campos por los que se pueden ordenar los datos. Permita que implemente una interfaz de este tipo para este tutorial.

Agregue un control Web DropDownList sobre el `SortableProducts` Repeater y establezca su propiedad `ID` en `SortBy`. En el ventana Propiedades, haga clic en los puntos suspensivos de la propiedad `Items` para abrir el editor de la colección ListItem. Agregue `ListItem` s para ordenar los datos por los campos `ProductName`, `CategoryName`y `SupplierName`. Agregue también un `ListItem` para ordenar los productos por su nombre en orden alfabético inverso.

El `ListItem` `Text` propiedades se pueden establecer en cualquier valor (como nombre), pero las propiedades de `Value` deben establecerse en el nombre del campo de datos (como NombreProducto). Para ordenar los resultados en orden descendente, anexe la cadena DESC al nombre del campo de datos, como ProductName DESC.

![Agregue un ListItem para cada uno de los campos de datos que se van a ordenar](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 5**: adición de una `ListItem` para cada uno de los campos de datos que se van a ordenar

Por último, agregue un control Web Button a la derecha de DropDownList. Establezca su `ID` en `RefreshRepeater` y su propiedad `Text` en actualizar.

Después de crear el `ListItem` s y agregar el botón de actualización, la sintaxis declarativa DropDownList y Button s debe ser similar a la siguiente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Una vez completado el DropDownList de ordenación, es necesario actualizar el controlador de eventos de `Selecting` de ObjectDataSource para que use la propiedad `SortBy``ListItem` s `Value` seleccionada en lugar de una expresión de ordenación codificada de forma rígida.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

En este momento, al visitar la página por primera vez, los productos se ordenarán inicialmente por el `ProductName` campo de datos, ya que los `SortBy` `ListItem` seleccionados de forma predeterminada (consulte la figura 6). Si selecciona una opción de ordenación diferente, como categoría, y hace clic en actualizar, se producirá un postback y se volverán a ordenar los datos por el nombre de la categoría, tal como se muestra en la figura 7.

[![los productos se ordenan inicialmente por su nombre](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Figura 6**: los productos se ordenan inicialmente por su nombre ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))

[![los productos están ahora ordenados por categoría](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Figura 7**: los productos están ahora ordenados por categoría ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))

> [!NOTE]
> Al hacer clic en el botón actualizar, los datos se vuelven a ordenar automáticamente porque se ha deshabilitado el estado de vista Repeater s, lo que provoca que el repetidor se vuelva a enlazar a su origen de datos en cada postback. Si deja el estado de vista Repeater s habilitado, el cambio de la lista desplegable de ordenación no tendrá ningún efecto en el criterio de ordenación. Para solucionar este error, cree un controlador de eventos para el evento del botón actualizar s `Click` y vuelva a enlazar el repetidor a su origen de datos (llamando al método Repeater s `DataBind()`).

## <a name="remembering-the-sort-expression-and-direction"></a>Recordar la expresión de ordenación y la dirección

Al crear una lista de datos que se puede ordenar o un repetidor en una página en la que pueden producirse postbacks relacionados sin orden, es imperativo que la expresión de ordenación y la dirección se recuerden en los postbacks. Por ejemplo, Imagine que hemos actualizado el repetidor en este tutorial para incluir un botón eliminar con cada producto. Cuando el usuario hace clic en el botón eliminar, se ejecuta código para eliminar el producto seleccionado y, a continuación, volver a enlazar los datos al repetidor. Si los detalles de ordenación no se conservan en el postback, los datos que se muestran en la pantalla se revertirán al criterio de ordenación original.

En este tutorial, el DropDownList guarda implícitamente la expresión de ordenación y la dirección en su estado de vista para nosotros. Si usamos una interfaz de ordenación diferente, por ejemplo, LinkButtons que proporcionó las distintas opciones de ordenación, es necesario tener cuidado de recordar el criterio de ordenación en los postbacks. Esto puede lograrse mediante el almacenamiento de los parámetros de ordenación en el estado de vista de la página, incluyendo el parámetro de ordenación en la cadena de consulta o a través de otra técnica de persistencia de estado.

En los siguientes ejemplos de este tutorial se explica cómo conservar los detalles de ordenación en el estado de vista de la página.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Paso 5: agregar compatibilidad de ordenación a una lista de usuarios que usa la paginación predeterminada

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , hemos examinado cómo implementar la paginación predeterminada con un DataList. Vamos a ampliar este ejemplo anterior para incluir la posibilidad de ordenar los datos paginados. Comience abriendo las páginas `SortingWithDefaultPaging.aspx` y `Paging.aspx` en la carpeta `PagingSortingDataListRepeater`. En la página `Paging.aspx`, haga clic en el botón origen para ver el marcado declarativo de la página. Copiar el texto seleccionado (vea la figura 8) y pegarlo en el marcado declarativo de `SortingWithDefaultPaging.aspx` entre las etiquetas de `<asp:Content>`.

[![replicar el marcado declarativo en las etiquetas &lt;ASP: Content&gt; de paging. aspx a SortingWithDefaultPaging. aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Figura 8**: replicación del marcado declarativo en las etiquetas de `<asp:Content>` de `Paging.aspx` a `SortingWithDefaultPaging.aspx` ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))

Después de copiar el marcado declarativo, copie los métodos y las propiedades de la clase de código subyacente de la página `Paging.aspx` en la clase de código subyacente para `SortingWithDefaultPaging.aspx`. A continuación, dedique un momento a ver la página de `SortingWithDefaultPaging.aspx` en un explorador. Debe presentar la misma funcionalidad y apariencia que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Mejorar ProductsBLL para incluir un método de paginación y ordenación predeterminados

En el tutorial anterior, creamos un método `GetProductsAsPagedDataSource(pageIndex, pageSize)` en la clase `ProductsBLL` que devolvía un objeto `PagedDataSource`. Este objeto de `PagedDataSource` se ha rellenado con *todos* los productos (a través del método de `GetProducts()` BLL), pero cuando se enlaza a la lista de datos, solo se mostraban los registros correspondientes a los parámetros de entrada *pageIndex* y *pageSize* especificados.

Anteriormente en este tutorial hemos agregado la compatibilidad de ordenación mediante la especificación de la expresión de ordenación del controlador de eventos ObjectDataSource s `Selecting`. Esto funciona bien cuando se devuelve ObjectDataSource a un objeto que se puede ordenar, como el `ProductsDataTable` devuelto por el método `GetProducts()`. Sin embargo, el objeto `PagedDataSource` devuelto por el método `GetProductsAsPagedDataSource` no admite la ordenación de su origen de datos interno. En su lugar, es necesario ordenar los resultados devueltos por el método `GetProducts()` *antes* de colocarlos en el `PagedDataSource`.

Para ello, cree un nuevo método en la clase `ProductsBLL` `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para ordenar los `ProductsDataTable` devueltos por el método `GetProducts()`, especifique la propiedad `Sort` de su `DataTableView`predeterminada:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

El método `GetProductsSortedAsPagedDataSource` difiere ligeramente del `GetProductsAsPagedDataSource` método creado en el tutorial anterior. En concreto, `GetProductsSortedAsPagedDataSource` acepta un parámetro de entrada adicional `sortExpression` y asigna este valor a la propiedad `Sort` del `DefaultView``ProductDataTable` s. Unas cuantas líneas de código más adelante, al `PagedDataSource` objeto s DataSource se le asigna el `DefaultView``ProductDataTable` s.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Llamar al método GetProductsSortedAsPagedDataSource y especificar el valor para el parámetro de entrada SortExpression

Con el método `GetProductsSortedAsPagedDataSource` completado, el siguiente paso es proporcionar el valor para este parámetro. ObjectDataSource in `SortingWithDefaultPaging.aspx` está configurado actualmente para llamar al método `GetProductsAsPagedDataSource` y pasa los dos parámetros de entrada a través de sus dos `QueryStringParameters`, que se especifican en la colección `SelectParameters`. Estos dos `QueryStringParameters` indican que el origen del método *`GetProductsAsPagedDataSource` s y* los parámetros *pageSize* proceden de los campos QueryString `pageIndex` y `pageSize`.

Actualice la propiedad ObjectDataSource s `SelectMethod` para que invoque el nuevo método `GetProductsSortedAsPagedDataSource`. A continuación, agregue una nueva `QueryStringParameter` de modo que se tenga acceso al parámetro de entrada *sortExpression* desde el campo QueryString `sortExpression`. Establezca el `DefaultValue` de `QueryStringParameter` s en ProductName.

Después de estos cambios, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

En este punto, la página `SortingWithDefaultPaging.aspx` ordenará los resultados alfabéticamente por el nombre del producto (consulte la figura 9). Esto se debe a que, de forma predeterminada, se pasa un valor de ProductName como el parámetro `GetProductsSortedAsPagedDataSource` Method s *sortExpression* .

[![de forma predeterminada, los resultados se ordenan por NombreProducto](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Figura 9**: de forma predeterminada, los resultados se ordenan por `ProductName` ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))

Si agrega manualmente un `sortExpression` campo QueryString como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` los resultados se ordenarán por el `sortExpression`especificado. Sin embargo, este parámetro `sortExpression` no se incluye en la cadena de QueryString al pasar a una página de datos diferente. De hecho, al hacer clic en los botones de la página siguiente o en la última página, se vuelve a `Paging.aspx`. Además, actualmente no hay ninguna interfaz de ordenación. La única forma en que un usuario puede cambiar el criterio de ordenación de los datos paginados es mediante la manipulación directa de QueryString.

## <a name="creating-the-sorting-interface"></a>Crear la interfaz de ordenación

Primero es necesario actualizar el método de `RedirectUser` para enviar al usuario a `SortingWithDefaultPaging.aspx` (en lugar de `Paging.aspx`) e incluir el valor `sortExpression` en la cadena de la cadena de actualización. También se debe agregar una propiedad con nombre de nivel de página de solo lectura `SortExpression`. Esta propiedad, similar a las propiedades `PageIndex` y `PageSize` creadas en el tutorial anterior, devuelve el valor del campo `sortExpression` QueryString si existe y el valor predeterminado (ProductName) en caso contrario.

Actualmente, el método `RedirectUser` acepta solo un parámetro de entrada único el índice de la página que se va a mostrar. Sin embargo, puede haber ocasiones en que desee redirigir al usuario a una página de datos determinada mediante una expresión de ordenación distinta de la especificada en la cadena de tipo. En un momento, vamos a crear la interfaz de ordenación para esta página, que incluirá una serie de controles Web de botón para ordenar los datos por una columna especificada. Cuando se hace clic en uno de estos botones, queremos redirigir al usuario pasando el valor de expresión de ordenación adecuado. Para proporcionar esta funcionalidad, cree dos versiones del método `RedirectUser`. El primero debe aceptar solo el índice de la página que se va a mostrar, mientras que el segundo acepta el índice de la página y la expresión de ordenación.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

En el primer ejemplo de este tutorial, creamos una interfaz de ordenación mediante DropDownList. En este ejemplo, vamos a usar tres controles Web de botón situados encima de la lista de elementos para ordenar por `ProductName`, uno para `CategoryName`y otro para `SupplierName`. Agregue los tres controles Web Button, estableciendo sus `ID` y `Text` propiedades de forma adecuada:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

A continuación, cree un controlador de eventos `Click` para cada uno. Los controladores de eventos deben llamar al método `RedirectUser`, devolviendo el usuario a la primera página con la expresión de ordenación adecuada.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Al visitar la página por primera vez, los datos se ordenan alfabéticamente por el nombre del producto (consulte la figura 9). Haga clic en el botón siguiente para avanzar a la segunda página de datos y, a continuación, haga clic en el botón Ordenar por categoría. Esto nos devuelve a la primera página de datos, ordenada por nombre de categoría (consulte la figura 10). Del mismo modo, al hacer clic en el botón Ordenar por proveedor se ordenan los datos por proveedor a partir de la primera página de datos. La opción de ordenación se recuerda a medida que se paginan los datos. En la figura 11 se muestra la página después de ordenar por categoría y, a continuación, avanzar hasta la decimotercer página de datos.

[![los productos se ordenan por categoría](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Figura 10**: los productos se ordenan por categoría ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))

[![se recuerda la expresión de ordenación al paginar a través de los datos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Figura 11**: la expresión de ordenación se recuerda al paginar a través de los datos ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Paso 6: paginación personalizada a través de los registros de un repetidor

El ejemplo de DataList examinado en el paso 5 páginas a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Al paginar a través de cantidades de datos suficientemente grandes, es fundamental que se use la paginación personalizada. De nuevo en la [paginación eficaz a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) y la ordenación de tutoriales de [datos paginados personalizados](../paging-and-sorting/sorting-custom-paged-data-vb.md) , hemos examinado las diferencias entre la paginación predeterminada y la paginación personalizada y los métodos creados en el BLL para usar la paginación personalizada y la ordenación de datos paginados personalizados. En concreto, en estos dos tutoriales anteriores se han agregado los tres métodos siguientes a la clase `ProductsBLL`:

- `GetProductsPaged(startRowIndex, maximumRows)` devuelve un subconjunto determinado de registros a partir de *startRowIndex* y no supera *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` devuelve un subconjunto determinado de registros ordenados por el parámetro de entrada *sortExpression* especificado.
- `TotalNumberOfProducts()` proporciona el número total de registros de la tabla de base de datos `Products`.

Estos métodos se pueden utilizar para paginar y ordenar de forma eficaz los datos mediante un control DataList o Repeater. Para ilustrar esto, deje que empiece por crear un control Repeater con compatibilidad con la paginación personalizada. a continuación, agregaremos funcionalidades de ordenación.

Abra la página `SortingWithCustomPaging.aspx` en la carpeta `PagingSortingDataListRepeater` y agregue un repetidor a la página, estableciendo su propiedad `ID` en `Products`. En la etiqueta inteligente Repeater s, cree un nuevo ObjectDataSource denominado `ProductsDataSource`. Configúrela para seleccionar los datos del método `ProductsBLL` Class s `GetProductsPaged`.

[![configurar ObjectDataSource para usar el método ProductsBLL de la clase GetProductsPaged](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Figura 12**: configuración de ObjectDataSource para usar el método `ProductsBLL` Class s `GetProductsPaged` ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))

Establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguno) y, a continuación, haga clic en el botón siguiente. Ahora, el Asistente para configurar el origen de datos solicita los orígenes de los parámetros de entrada `GetProductsPaged` Method s *startRowIndex* y *maximumRows* . En realidad, estos parámetros de entrada se omiten. En su lugar, los valores de *startRowIndex* y *maximumRows* se pasarán a través de la propiedad `Arguments` del controlador de eventos de `Selecting` de ObjectDataSource, al igual *que en* el caso de la primera demostración de este tutorial. Por lo tanto, deje las listas desplegables de origen del parámetro en el asistente establecido en ninguno.

[![dejar los orígenes de parámetro establecidos en ninguno](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Figura 13**: deje los orígenes de parámetro establecidos en ninguno ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))

> [!NOTE]
> *No* establezca la propiedad ObjectDataSource s `EnablePaging` en `true`. Esto hará que ObjectDataSource incluya automáticamente sus propios parámetros *startRowIndex* y *maximumRows* en la lista de parámetros existentes `SelectMethod` s. La propiedad `EnablePaging` es útil cuando se enlazan datos paginados personalizados a un control GridView, DetailsView o FormView porque estos controles esperan ciertos comportamientos de ObjectDataSource que solo están disponibles cuando se `true``EnablePaging` propiedad. Dado que tenemos que agregar manualmente la compatibilidad de paginación para los controles DataList y Repeater, deje esta propiedad establecida en `false` (el valor predeterminado), ya que vamos a incluir en la funcionalidad necesaria directamente dentro de nuestra página de ASP.NET.

Por último, defina los `ItemTemplate` repetidores para que se muestren el nombre del producto, la categoría y el proveedor. Después de estos cambios, la sintaxis declarativa Repeater y de ObjectDataSource s debe ser similar a la siguiente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Dedique un momento a visitar la página a través de un explorador y tenga en cuenta que no se devuelve ningún registro. Esto se debe a que aún no se deben especificar los valores de los parámetros *startRowIndex* y *maximumRows* . por lo tanto, los valores de 0 se pasan para ambos. Para especificar estos valores, cree un controlador de eventos para el evento ObjectDataSource s `Selecting` y establezca estos valores de parámetros mediante programación en valores codificados de forma rígida de 0 y 5, respectivamente:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Con este cambio, la página, cuando se ve a través de un explorador, muestra los primeros cinco productos.

[![se muestran los cinco primeros registros](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Figura 14**: se muestran los primeros cinco registros ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))

> [!NOTE]
> Los productos que se muestran en la figura 14 se ordenan por nombre de producto porque el `GetProductsPaged` procedimiento almacenado que realiza la consulta de paginación personalizada eficaz ordena los resultados por `ProductName`.

Con el fin de permitir que el usuario recorra las páginas, es necesario realizar un seguimiento del índice de la fila inicial y de las filas máximas, y recordar estos valores en los postbacks. En el ejemplo de paginación predeterminada usamos campos QueryString para conservar estos valores. en esta demostración, se conserva esta información en el estado de vista de la página. Cree las dos propiedades siguientes:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

A continuación, actualice el código del controlador de eventos de selección para que use las propiedades `StartRowIndex` y `MaximumRows` en lugar de los valores codificados de forma rígida de 0 y 5:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

En este momento, la página sigue mostrando solo los cinco primeros registros. Sin embargo, con estas propiedades en su lugar, se vuelve a preparar la creación de la interfaz de paginación.

## <a name="adding-the-paging-interface"></a>Agregar la interfaz de paginación

Permita a s usar la misma interfaz de paginación primera, anterior, siguiente y última que se usó en el ejemplo de paginación predeterminada, incluido el control Web de etiqueta que muestra la página de datos que se está viendo y el número total de páginas. Agregue los cuatro controles Web Button y la etiqueta debajo del repetidor.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

A continuación, cree controladores de eventos de `Click` para los cuatro botones. Cuando se hace clic en uno de estos botones, es necesario actualizar el `StartRowIndex` y volver a enlazar los datos al repetidor. El código de los botones primero, anterior y siguiente es lo bastante sencillo, pero para el último botón ¿cómo se determina el índice de la fila inicial de la última página de datos? Para calcular este índice, así como para poder determinar si se deben habilitar los botones siguiente y último, es necesario saber cuántos registros se van a paginar en total. Podemos determinar esto llamando al método `ProductsBLL` Class s `TotalNumberOfProducts()`. Permita crear una propiedad de nivel de página de solo lectura denominada `TotalRowCount` que devuelva los resultados del método `TotalNumberOfProducts()`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Con esta propiedad, ahora podemos determinar el índice de la fila de inicio de la última página. En concreto, es el resultado entero del `TotalRowCount` menos 1 dividido por `MaximumRows`, multiplicado por `MaximumRows`. Ahora podemos escribir los controladores de eventos de `Click` para los cuatro botones de la interfaz de paginación:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Por último, es necesario deshabilitar los botones primero y anterior de la interfaz de paginación al ver la primera página de datos y los botones siguiente y último al ver la última página. Para ello, agregue el código siguiente al controlador de eventos ObjectDataSource s `Selecting`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Después de agregar estos controladores de eventos `Click` y el código para habilitar o deshabilitar los elementos de la interfaz de paginación basados en el índice de la fila inicial actual, pruebe la página en un explorador. Como se muestra en la figura 15, al visitar la página por primera vez, se deshabilitan los botones primero y anterior. Al hacer clic en siguiente se muestra la segunda página de datos, mientras que al hacer clic en último se muestra la página final (vea las figuras 16 y 17). Al ver la última página de datos, se deshabilitan los botones siguiente y último.

[![los botones anterior y último están deshabilitados al ver la primera página de productos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Figura 15**: los botones anterior y último están deshabilitados al ver la primera página de productos ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))

[![se muestra la segunda página de productos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Figura 16**: se muestra la segunda página de productos ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))

[![al hacer clic en Last, se muestra la página final de los datos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Figura 17**: al hacer clic en Last, se muestra la página final de los datos ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Paso 7: incluir compatibilidad de ordenación con el repetidor personalizado paginado

Una vez implementada la paginación personalizada, se vuelve a preparar para incluir la compatibilidad con la ordenación. El método `ProductsBLL` Class s `GetProductsPagedAndSorted` tiene los mismos parámetros de entrada *startRowIndex* y *maximumRows* que `GetProductsPaged`, pero permite un parámetro de entrada de *sortExpression* adicional. Para usar el método `GetProductsPagedAndSorted` de `SortingWithCustomPaging.aspx`, es necesario realizar los pasos siguientes:

1. Cambie la propiedad ObjectDataSource s `SelectMethod` de `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Agregue un objeto *sortExpression* `Parameter` a la colección de `SelectParameters` de ObjectDataSource.
3. Cree una propiedad de `SortExpression` de nivel de página privada que conserve su valor en los postbacks mediante el estado de vista de la página.
4. Actualice el controlador de eventos ObjectDataSource s `Selecting` para asignar el parámetro ObjectDataSource s *sortExpression* al valor de la propiedad `SortExpression` de nivel de página.
5. Cree la interfaz de ordenación.

Empiece por actualizar la propiedad ObjectDataSource s `SelectMethod` y agregando un `Parameter`*sortExpression* . Asegúrese de que el valor de la propiedad *sortExpression* `Parameter` s `Type` está establecido en `String`. Después de completar estas dos primeras tareas, el marcado declarativo de ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

A continuación, necesitamos una propiedad de `SortExpression` de nivel de página cuyo valor se serialice en el estado de vista. Si no se ha establecido ningún valor de expresión de ordenación, use ProductName como valor predeterminado:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Antes de que ObjectDataSource invoque el método `GetProductsPagedAndSorted` es necesario establecer el `Parameter` *sortExpression* en el valor de la propiedad `SortExpression`. En el controlador de eventos `Selecting`, agregue la siguiente línea de código:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Todo lo que queda es implementar la interfaz de ordenación. Como hicimos en el último ejemplo, permita que la interfaz de ordenación se implemente mediante tres controles Web de botón que permiten al usuario ordenar los resultados por nombre de producto, categoría o proveedor.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Cree `Click` controladores de eventos para estos tres controles de botón. En el controlador de eventos, restablezca el `StartRowIndex` en 0, establezca el `SortExpression` en el valor adecuado y vuelva a enlazar los datos al repetidor:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Eso es todo lo que tiene que hacer. Aunque se han implementado varios pasos para obtener la paginación y la ordenación personalizadas, los pasos son muy similares a los necesarios para la paginación predeterminada. En la figura 18 se muestran los productos al ver la última página de datos cuando se ordenan por categoría.

[![se muestra la última página de datos, ordenada por categoría,](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Figura 18**: se muestra la última página de datos, ordenada por categoría, ([haga clic para ver la imagen de tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))

> [!NOTE]
> En los ejemplos anteriores, cuando se usaba la ordenación por el NombreProveedor del proveedor como expresión de ordenación. Sin embargo, para la implementación de paginación personalizada, es necesario usar CompanyName. Esto se debe a que el procedimiento almacenado responsable de la implementación de la paginación personalizada `GetProductsPagedAndSorted` pasa la expresión de ordenación a la palabra clave `ROW_NUMBER()`, la palabra clave `ROW_NUMBER()` requiere el nombre de columna real en lugar de un alias. Por lo tanto, se debe usar `CompanyName` (el nombre de la columna en la tabla de `Suppliers`) en lugar del alias usado en la consulta de `SELECT` (`SupplierName`) para la expresión de ordenación.

## <a name="summary"></a>Resumen

Ni los controles DataList ni Repeater ofrecen compatibilidad con la ordenación integrada, pero con un poco de código y una interfaz de ordenación personalizada, se puede Agregar esta funcionalidad. Al implementar la ordenación, pero no la paginación, la expresión de ordenación se puede especificar mediante el `DataSourceSelectArguments` objeto pasado al método de `Select` de ObjectDataSource. Esta propiedad `DataSourceSelectArguments` Object s `SortExpression` puede asignarse en el controlador de eventos ObjectDataSource s `Selecting`.

Para agregar funciones de ordenación a una lista de controles o un repetidor que ya proporciona compatibilidad con la paginación, el enfoque más sencillo es personalizar la capa de lógica de negocios para incluir un método que acepte una expresión de ordenación. Esta información se puede pasar a continuación a través de un parámetro en la `SelectParameters`de datos de origen de datos.

En este tutorial se realiza el examen de la paginación y la ordenación con los controles DataList y Repeater. En el tutorial siguiente y el final se examinará cómo agregar controles Web de botón a las plantillas DataList y Repeater s para proporcionar una funcionalidad personalizada iniciada por el usuario para cada elemento.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue David Suru. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
