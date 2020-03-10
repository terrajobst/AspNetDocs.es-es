---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Maestro y detalles mediante una lista con viñetas de registros maestros con un DataList de detallesC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial, comprimiremos el informe maestro y detalles de dos páginas del tutorial anterior en una sola página, donde se muestra una lista con viñetas de nombres de categoría en t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 4549ab8e64599b09c300c158bedfd5d85efafc4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78490969"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Maestro y detalles mediante una lista con viñetas de registros maestros con un control DataList de detalles (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> En este tutorial, comprimiremos el informe maestro y detalles de dos páginas del tutorial anterior en una sola página, donde se muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla y los productos de la categoría seleccionada a la derecha de la pantalla.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-cs.md) , hemos visto cómo separar un informe maestro y detalles en dos páginas. En la página maestra usamos un control Repeater para representar una lista con viñetas de categorías. Cada nombre de categoría era un hipervínculo que, al hacer clic en él, llevaría al usuario a la página de detalles, donde una lista de datos de dos columnas mostraba los productos que pertenecen a la categoría seleccionada.

En este tutorial, comprimiremos el tutorial de dos páginas en una sola página, donde se muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla con cada nombre de categoría representado como LinkButton. Al hacer clic en uno de los nombres de categoría LinkButtons, se induce un postback y se enlazan los productos de categoría s seleccionados a una lista de elementos de dos columnas a la derecha de la pantalla. Además de mostrar cada nombre de categoría, el repetidor de la izquierda muestra el número total de productos que hay en una determinada categoría (consulte la ilustración 1).

[![la categoría s Name y el número total de productos se muestran a la izquierda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Figura 1**: el nombre de la categoría y el número total de productos se muestran a la izquierda ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Paso 1: mostrar un repetidor en la parte izquierda de la pantalla

En este tutorial, es necesario que la lista con viñetas de categorías aparezca a la izquierda de los productos de la categoría seleccionados. El contenido de una página web se puede colocar mediante etiquetas de párrafo de elementos HTML estándar, espacios de no separación, `<table>` s, etc. o mediante técnicas de hoja de estilos en cascada (CSS). En este momento, todos nuestros tutoriales han usado técnicas de CSS para el posicionamiento. Cuando creamos la interfaz de usuario de navegación en nuestra página maestra en el tutorial [páginas maestras y navegación de sitios](../introduction/master-pages-and-site-navigation-cs.md) , usamos el *posicionamiento absoluto*, que indica el desplazamiento de píxeles preciso para la lista de navegación y el contenido principal. Como alternativa, se puede usar CSS para colocar un elemento a la derecha o a la izquierda de otro mediante *flotante*. Podemos hacer que la lista con viñetas de categorías aparezca a la izquierda de los productos de la categoría seleccionados, flotando el repetidor a la izquierda de la lista de objetos.

Abra la página `CategoriesAndProducts.aspx` de la carpeta `DataListRepeaterFiltering` y agregue a la página un repetidor y una lista de controles. Establezca el `ID`dor de repetición en `Categories` y los de DataList en `CategoryProducts`. Vaya a la vista de código fuente y coloque los controles Repeater y DataList dentro de sus propios elementos `<div>`. Es decir, encierre primero el repetidor dentro de un elemento de `<div>` y, a continuación, la lista de elementos de su propio elemento `<div>` directamente después del repetidor. El marcado en este punto debe ser similar al siguiente:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Para flotar el repetidor a la izquierda de la lista de DataList, es necesario usar el atributo de estilo CSS de `float`, como se muestra a continuación:

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

El `float: left;` flota el primer `<div>` elemento a la izquierda del segundo. La configuración de `width` y `padding-right` indica el primer `width` de `<div>` y la cantidad de relleno que se agrega entre el contenido del elemento `<div>` y su margen derecho. Para obtener más información sobre los elementos flotantes en CSS, consulte [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

En lugar de especificar la configuración de estilo directamente a través del primer `<p>` elemento s `style`, en su lugar, se crea una nueva clase CSS en `Styles.css` denominada `FloatLeft`:

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

A continuación, podemos reemplazar el `<div>` por `<div class="FloatLeft">`.

Después de agregar la clase CSS y configurar el marcado en la página `CategoriesAndProducts.aspx`, vaya al diseñador. Debería ver el repetidor flotante a la izquierda de la lista de datos (aunque ahora solo aparecen como cuadros grises, ya que aún no se pueden configurar sus orígenes de datos o plantillas).

[![el Repeater está flotando a la izquierda de DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Figura 2**: el repetidor está flotando a la izquierda de la lista de controles ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Paso 2: determinar el número de productos de cada categoría

Una vez que se completa el marcado de rellenado y de lista de control de datos, se vuelve a preparar para enlazar los datos de categoría al control Repeater. Sin embargo, como muestra la lista con viñetas de las categorías de la figura 1, además de cada nombre de categoría, también necesitamos mostrar el número de productos asociados a la categoría. Para obtener acceso a esta información, podemos:

- **Determine esta información en la clase de código subyacente de la página ASP.NET.** Dado un *`categoryID`* determinado, podemos determinar el número de productos asociados llamando al método `ProductsBLL` de clase s `GetProductsByCategoryID(categoryID)`. Este método devuelve un objeto `ProductsDataTable` cuya propiedad `Count` indica cuántos `ProductsRow` s existe, que es el número de productos para el *`categoryID`* especificado. Se puede crear un controlador de eventos `ItemDataBound` para el repetidor que, para cada categoría enlazada al repetidor, llama al método `ProductsBLL` de clase s `GetProductsByCategoryID(categoryID)` e incluye su recuento en la salida.
- **Actualice el `CategoriesDataTable` en el conjunto de DataSet con tipo para incluir una columna `NumberOfProducts`.** A continuación, podemos actualizar el método `GetCategories()` en el `CategoriesDataTable` para incluir esta información o, como alternativa, dejar `GetCategories()` tal cual y crear un nuevo método de `CategoriesDataTable` denominado `GetCategoriesAndNumberOfProducts()`.

Vamos a explorar ambas técnicas. El primer enfoque es más fácil de implementar, ya que no es necesario actualizar la capa de acceso a datos. sin embargo, requiere más comunicaciones con la base de datos. La llamada al método `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` del controlador de eventos `ItemDataBound` agrega una llamada de base de datos adicional para cada categoría mostrada en el repetidor. Con esta técnica existen *n* + 1 llamadas de base de datos, donde *N* es el número de categorías que se muestran en el repetidor. Con el segundo enfoque, el recuento de productos se devuelve con información sobre cada categoría del método `CategoriesBLL` Class s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`), lo que da como resultado un solo viaje a la base de datos.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinar el número de productos en el controlador de eventos de ItemDataBound

La determinación del número de productos de cada categoría en el controlador de eventos `ItemDataBound` repetidor no requiere ninguna modificación en la capa de acceso a datos existente. Todas las modificaciones se pueden realizar directamente en la página `CategoriesAndProducts.aspx`. Empiece agregando un nuevo ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente Repeater s. A continuación, configure el `CategoriesDataSource` ObjectDataSource para que recupere sus datos del método `CategoriesBLL` clase s `GetCategories()`.

[![configurar ObjectDataSource para usar el método CategoriesBLL de clase s GetCategories ()](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Figura 3**: configuración de ObjectDataSource para usar el método `CategoriesBLL` class s `GetCategories()` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))

Es necesario hacer clic en cada elemento del repetidor `Categories` y, al hacer clic en él, hacer que el `CategoryProducts` DataList muestre los productos de la categoría seleccionada. Esto puede lograrse mediante la conversión de cada categoría en un hipervínculo, la vinculación a esta misma página (`CategoriesAndProducts.aspx`), pero pasando el `CategoryID` a través de la cadena de tipo de datos, como vimos en el tutorial anterior. La ventaja de este enfoque es que una página que muestra los productos de una categoría determinada se puede marcar e indexar mediante un motor de búsqueda.

Como alternativa, podemos hacer que cada categoría sea un LinkButton, que es el enfoque que usaremos en este tutorial. El LinkButton se representa en el explorador del usuario como un hipervínculo, pero, cuando se hace clic en él, induce un postback; en el postback, se debe actualizar el origen de elementos de la lista de elementos para mostrar los productos que pertenecen a la categoría seleccionada. En este tutorial, el uso de un hipervínculo tiene más sentido que el uso de un LinkButton; sin embargo, puede haber otros escenarios en los que el uso de un LinkButton sea más ventajoso. Aunque el enfoque de hipervínculo sería ideal para este ejemplo, en su lugar, vamos a explorar mediante el LinkButton. Como veremos, el uso de un LinkButton presenta algunos desafíos que de otro modo no surgirían con un hipervínculo. Por lo tanto, el uso de un elemento LinkButton en este tutorial resaltará estos desafíos y le ayudará a proporcionar soluciones para esos escenarios en los que es posible que deseemos usar un elemento LinkButton en lugar de un hipervínculo.

> [!NOTE]
> Se recomienda repetir este tutorial con un control de hipervínculo o un elemento de `<a>` en lugar de LinkButton.

En el marcado siguiente se muestra la sintaxis declarativa del Repeater y el ObjectDataSource. Tenga en cuenta que las plantillas de repetidor representan una lista con viñetas con cada elemento como LinkButton:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> En este tutorial, el repetidor debe tener habilitado el estado de vista (tenga en cuenta la omisión de la `EnableViewState="False"` de la sintaxis declarativa de Repeater s). En el paso 3, vamos a crear un controlador de eventos para el evento Repeater `ItemCommand` en el que se va a actualizar la colección de `SelectParameters` de ObjectDataSource s. No obstante, los `ItemCommand`repetidor no se activarán si el estado de vista está deshabilitado. Vea [un inmejor de una pregunta ASP.net](http://scottonwriting.net/sowblog/posts/1263.aspx) y [su solución](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obtener más información sobre por qué el estado de vista debe estar habilitado para que se active un evento Repeater `ItemCommand`.

El LinkButton con el valor de propiedad `ID` de `ViewCategory` no tiene su propiedad `Text` establecida. Si solo Queríamos mostrar el nombre de la categoría, habríamos establecido la propiedad texto mediante declaración, a través de la sintaxis de enlace de los enlaces, como:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Sin embargo, queremos mostrar el nombre de la categoría *y* el número de productos que pertenecen a esa categoría. Esta información se puede recuperar del controlador de eventos Repeater s `ItemDataBound` realizando una llamada al método de `GetCategoriesByProductID(categoryID)` de `ProductBLL` Class y determinando el número de registros que se devuelven en el `ProductsDataTable`resultante, como se muestra en el código siguiente:

[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Comenzaremos asegurándose de que hemos vuelto a trabajar con un elemento de datos (uno cuyo `ItemType` es `Item` o `AlternatingItem`) y, a continuación, hace referencia a la instancia de `CategoriesRow` que acaba de enlazarse a la `RepeaterItem`actual. A continuación, determinamos el número de productos para esta categoría creando una instancia de la clase `ProductsBLL`, llamando a su método `GetCategoriesByProductID(categoryID)` y determinando el número de registros devueltos mediante la propiedad `Count`. Por último, el `ViewCategory` LinkButton en ItemTemplate es References y su propiedad `Text` está establecida en *CategoryName* (*NumberOfProductsInCategory*), donde *NumberOfProductsInCategory* tiene el formato de un número con cero posiciones decimales.

> [!NOTE]
> Como alternativa, podríamos haber agregado una *función de formato* a la clase de código subyacente de la página ASP.net que acepta una categoría s `CategoryName` y `CategoryID` valores y devuelve el `CategoryName` concatenado con el número de productos de la categoría (como se determina mediante una llamada al método `GetCategoriesByProductID(categoryID)`). Los resultados de esta función de formato se pueden asignar mediante declaración a la propiedad de texto LinkButton s reemplazando la necesidad del controlador de eventos `ItemDataBound`. Consulte el [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) o [el formato de DataList y Repeater en función de los](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) tutoriales de datos para obtener más información sobre el uso de funciones de formato.

Después de agregar este controlador de eventos, dedique un momento a probar la página a través de un explorador. Observe cómo cada categoría aparece en una lista con viñetas, que muestra el nombre de la categoría y el número de productos asociados a la categoría (consulte la figura 4).

[![se muestran los nombres y el número de productos de cada categoría.](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Figura 4**: se muestran los nombres de todas las categorías y el número de productos ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Actualización del`CategoriesDataTable`y`CategoriesTableAdapter`para incluir el número de productos de cada categoría

En lugar de determinar el número de productos de cada categoría a medida que se enlazan al repetidor, podemos agilizar este proceso ajustando el `CategoriesDataTable` y `CategoriesTableAdapter` en la capa de acceso a datos para incluir esta información de forma nativa. Para ello, debemos agregar una nueva columna a `CategoriesDataTable` para contener el número de productos asociados. Para agregar una nueva columna a un objeto DataTable, abra el conjunto de texto con tipo (`App_Code\DAL\Northwind.xsd`), haga clic con el botón derecho en la DataTable que se va a modificar y elija Agregar o columna. Agregue una nueva columna al `CategoriesDataTable` (consulte la figura 5).

[![agregar una nueva columna a CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Figura 5**: agregar una nueva columna a la `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))

Esto agregará una nueva columna denominada `Column1`, que puede cambiar simplemente escribiendo un nombre diferente. Cambie el nombre de esta nueva columna a `NumberOfProducts`. A continuación, es necesario configurar las propiedades de esta columna. Haga clic en la nueva columna y vaya al ventana Propiedades. Cambie la propiedad Column s `DataType` de `System.String` a `System.Int32` y establezca la propiedad `ReadOnly` en `True`, como se muestra en la figura 6.

![Establecer las propiedades DataType y ReadOnly de la nueva columna](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Figura 6**: establecimiento de las propiedades `DataType` y `ReadOnly` de la nueva columna

Aunque el `CategoriesDataTable` ahora tiene una columna `NumberOfProducts`, su valor no se establece en ninguna de las consultas de TableAdapter s correspondientes. Podemos actualizar el método de `GetCategories()` para que devuelva esta información si queremos que la información se devuelva cada vez que se recupere la información de la categoría. Sin embargo, si solo necesitamos obtener el número de productos asociados para las categorías en casos poco frecuentes (por ejemplo, solo para este tutorial), podemos dejar `GetCategories()` tal cual y crear un nuevo método que devuelva esta información. Vamos a usar este último enfoque, creando un nuevo método denominado `GetCategoriesAndNumberOfProducts()`.

Para agregar este nuevo método de `GetCategoriesAndNumberOfProducts()`, haga clic con el botón derecho en el `CategoriesTableAdapter` y elija nueva consulta. Se abre el Asistente para configuración de consultas de TableAdapter, que se usa varias veces en los tutoriales anteriores. Para este método, inicie el asistente indicando que la consulta utiliza una instrucción SQL ad hoc que devuelve filas.

[![crear el método mediante una instrucción SQL ad hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Figura 7**: crear el método mediante una instrucción SQL ad hoc ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))

[![la instrucción SQL devuelve filas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Figura 8**: la instrucción SQL devuelve filas ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))

En la pantalla del asistente siguiente se le pide que use la consulta. Para devolver los campos `CategoryID`, `CategoryName`y `Description` de cada categoría, junto con el número de productos asociados a la categoría, utilice la siguiente instrucción de `SELECT`:

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]

[![especificar la consulta que se va a usar](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Figura 9**: especificación de la consulta que se va a usar ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))

Tenga en cuenta que la subconsulta que calcula el número de productos asociados a la categoría tiene un alias como `NumberOfProducts`. Esta coincidencia de nomenclatura hace que el valor devuelto por esta subconsulta esté asociado a la columna `CategoriesDataTable` s `NumberOfProducts`.

Después de escribir esta consulta, el último paso es elegir el nombre del nuevo método. Utilice `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` para los patrones rellenar un DataTable y devolver un objeto DataTable, respectivamente.

[![nombre de los nuevos métodos de TableAdapter FillWithNumberOfProducts y GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Figura 10**: asigne un nombre a los nuevos métodos de TableAdapter `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))

En este momento, la capa de acceso a datos se ha ampliado para incluir el número de productos por categoría. Puesto que todo el nivel de presentación enruta todas las llamadas a la capa DAL a través de una capa de lógica de negocios independiente, es necesario agregar un método `GetCategoriesAndNumberOfProducts` correspondiente a la clase `CategoriesBLL`:

[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

Una vez completada la capa DAL y la capa BLL, se vuelve a preparar para enlazar estos datos a la `Categories` repetidor en `CategoriesAndProducts.aspx`. Si ya ha creado un ObjectDataSource para el repetidor en la sección determinar el número de productos en la `ItemDataBound` controlador de eventos, elimine este ObjectDataSource y quite el valor de la propiedad Repeater `DataSourceID`. Desconecte también el evento Repeater s `ItemDataBound` del controlador de eventos quitando la sintaxis de `Handles Categories.OnItemDataBound` en la clase de código subyacente de ASP.NET.

Con el repetidor en su estado original, agregue un nuevo ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente Repeater s. Configure ObjectDataSource para usar la clase `CategoriesBLL`, pero en lugar de hacer que use el método `GetCategories()`, haga que use `GetCategoriesAndNumberOfProducts()` en su lugar (consulte la figura 11).

[![configurar ObjectDataSource para usar el método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Figura 11**: configuración de ObjectDataSource para usar el método `GetCategoriesAndNumberOfProducts` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))

A continuación, actualice el `ItemTemplate` para que la propiedad `Text` de LinkButton se asigne mediante declaración con la sintaxis de DataBinding e incluya los campos de datos `CategoryName` y `NumberOfProducts`. El marcado declarativo completo para el Repeater y el `CategoriesDataSource` ObjectDataSource siguen:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

La salida representada mediante la actualización de la capa DAL para incluir una `NumberOfProducts` columna es igual que el uso del método de controlador de eventos `ItemDataBound` (consulte la figura 4 para ver una captura de pantalla del repetidor que muestra los nombres de categoría y el número de productos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Paso 3: mostrar los productos de la categoría seleccionada

En este punto, tenemos el `Categories` repetidor que muestra la lista de categorías junto con el número de productos de cada categoría. El repetidor usa un LinkButton para cada categoría que, cuando se hace clic en él, produce un postback, en el que es necesario mostrar esos productos para la categoría seleccionada en el `CategoryProducts` DataList.

Uno de los desafíos que nos enfrentamos es cómo hacer que la lista de usuarios muestre solo los productos de la categoría seleccionada. En la página [principal/detalle mediante el uso de un tutorial de GridView de información general seleccionable con detalles de DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , vimos cómo crear un control GridView cuyas filas se pueden seleccionar, con los detalles de la fila seleccionada que se muestran en DetailsView en la misma página. El origen de GridView s devuelve información sobre todos los productos mediante el método `ProductsBLL` s `GetProducts()` mientras que la ObjectDataSource s de DetailsView recuperó información sobre el producto seleccionado mediante el método `GetProductsByProductID(productID)`. El valor del parámetro *`productID`* se proporcionó mediante declaración al asociarlo con el valor de la propiedad `SelectedValue` de GridView. Desafortunadamente, el repetidor no tiene una propiedad `SelectedValue` y no puede servir como origen de parámetro.

> [!NOTE]
> Este es uno de los desafíos que aparecen al usar LinkButton en un repetidor. Si se hubiera usado un hipervínculo para pasar el `CategoryID` a través de QueryString, se podría usar ese campo QueryString como el origen del valor del parámetro s.

Sin embargo, antes de preocuparnos por la falta de una propiedad `SelectedValue` para el repetidor, se debe enlazar primero la lista de objetos a una ObjectDataSource y especificar su `ItemTemplate`.

En la etiqueta inteligente DataList s, opte por agregar un nuevo ObjectDataSource denominado `CategoryProductsDataSource` y configurarlo para usar el método `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`. Como la lista de componentes de este tutorial ofrece una interfaz de solo lectura, no dude en establecer las listas desplegables de las pestañas insertar, actualizar y eliminar en (ninguna).

[![configurar el método ObjectDataSource para usar ProductsBLL Class s GetProductsByCategoryID (categoryID)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Figura 12**: configuración del método ObjectDataSource para usar `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))

Dado que el método `GetProductsByCategoryID(categoryID)` espera un parámetro de entrada ( *`categoryID`* ), el Asistente para configurar el origen de datos nos permite especificar el origen del parámetro. Si las categorías se enumeran en un control GridView o DataList, se establece la lista desplegable origen del parámetro en el control y el ControlID en el `ID` del control Web de datos. Sin embargo, dado que el repetidor no tiene una propiedad `SelectedValue`, no se puede usar como origen de parámetro. Si comprueba, verá que la lista desplegable ControlID solo contiene un control `ID``CategoryProducts`, el `ID` de DataList.

Por ahora, establezca la lista desplegable origen del parámetro en ninguno. Terminaremos asignando mediante programación este valor de parámetro cuando se haga clic en una categoría LinkButton en el Repeater.

[![no especifique un origen de parámetro para el parámetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Figura 13**: no especifique un origen de parámetro para el parámetro *`categoryID`* ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio genera automáticamente el `ItemTemplate`de datos de DataList. Reemplace esta `ItemTemplate` predeterminada por la plantilla que usamos en el tutorial anterior; Además, establezca la propiedad DataList s `RepeatColumns` en 2. Después de realizar estos cambios, el marcado declarativo para la lista de elementos y su ObjectDataSource asociado deben tener un aspecto similar al siguiente:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Actualmente, el parámetro `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* nunca se establece, por lo que no se muestra ningún producto al ver la página. Lo que hay que hacer es tener este valor de parámetro establecido en función del `CategoryID` de la categoría en la que se hizo clic en el repetidor. Esto presenta dos desafíos: en primer lugar, cómo se determina cuándo se hace clic en un LinkButton en el `ItemTemplate` repetidor; y en segundo lugar, ¿cómo podemos determinar el `CategoryID` de la categoría correspondiente en la que se hizo clic en LinkButton?

El control LinkButton como los controles Button y ImageButton tiene un evento `Click` y un [evento`Command`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). El evento `Click` está diseñado para simplemente tener en cuenta que se ha hecho clic en el LinkButton. Sin embargo, en ocasiones, además de tener en cuenta que se ha realizado clic en el control LinkButton, también es necesario pasar cierta información adicional al controlador de eventos. En este caso, se puede asignar esta información adicional a las propiedades LinkButton s [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) y [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) . A continuación, cuando se hace clic en el control LinkButton, su evento `Command` se desencadena (en lugar de su evento `Click`) y se pasan los valores de las propiedades `CommandName` y `CommandArgument` al controlador de eventos.

Cuando se genera un evento de `Command` desde una plantilla en el repetidor, se activa el [evento repeater`ItemCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) y se pasan los valores `CommandName` y `CommandArgument` del LinkButton (o Button o ImageButton) en el que se hace clic. Por lo tanto, para determinar cuándo se ha realizado un clic en un LinkButton de categoría en el repetidor, es necesario hacer lo siguiente:

1. Establezca la propiedad `CommandName` de LinkButton en el `ItemTemplate` Repeater en algún valor (he usado ListProducts). Al establecer este valor de `CommandName`, el evento LinkButton s `Command` se desencadena cuando se hace clic en LinkButton.
2. Establezca la propiedad LinkButton s `CommandArgument` en el valor de la `CategoryID`Item s actual.
3. Cree un controlador de eventos para el evento Repeater s `ItemCommand`. En el controlador de eventos, establezca el parámetro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` en el valor de la `CommandArgument`pasada.

El siguiente marcado de `ItemTemplate` para las categorías el repetidor implementa los pasos 1 y 2. Observe cómo se asigna al valor de `CommandArgument` los elementos de datos `CategoryID` mediante la sintaxis de DataBinding:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Siempre que se crea un controlador de eventos `ItemCommand` es prudente comprobar primero el valor de `CommandName` entrante, ya que *cualquier* evento de `Command` generado por *cualquier* botón, LinkButton o ImageButton dentro del repetidor hará que se active el evento de `ItemCommand`. Aunque actualmente solo tenemos un solo grupo de este tipo, en el futuro (u otro desarrollador de nuestro equipo) podría agregar controles Web de botón adicionales al repetidor que, cuando se hace clic en él, genera el mismo `ItemCommand` controlador de eventos. Por lo tanto, es mejor asegurarse siempre de que se comprueba la propiedad `CommandName` y que solo se continúe con la lógica de programación si coincide con el valor esperado.

Después de asegurarse de que el valor de `CommandName` pasado es igual a ListProducts, el controlador de eventos asigna el parámetro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` al valor de la `CommandArgument`pasada. Esta modificación en la `SelectParameters` de datos de ObjectDataSource hace que la lista de datos se vuelva a enlazar automáticamente al origen de datos, mostrando los productos de la categoría recién seleccionada.

[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

Con estas adiciones, se ha completado nuestro tutorial. Tómese un momento para probarlo en un explorador. La figura 14 muestra la pantalla al visitar la página por primera vez. Puesto que todavía hay una categoría seleccionada, no se muestra ningún producto. Al hacer clic en una categoría, como producir, se muestran los productos de la categoría de producto en una vista de dos columnas (consulte la figura 15).

[![no se muestran productos al visitar la página por primera vez](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Figura 14**: no se muestra ningún producto al visitar la página por primera vez ([haga clic para ver la imagen de tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))

[![al hacer clic en la categoría de producción, se muestran los productos coincidentes a la derecha](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Figura 15**: hacer clic en la categoría de producción muestra los productos coincidentes a la derecha ([haga clic para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))

## <a name="summary"></a>Resumen

Como hemos visto en este tutorial y en el anterior, los informes maestros y detallados se pueden distribuir en dos páginas o consolidar en uno. Sin embargo, al mostrar un informe maestro/detalles en una sola página, se presentan algunos desafíos sobre la mejor forma de diseñar los registros maestro y detalles en la página. En la *Página principal/detalle mediante el uso de un control GridView maestro seleccionable con un* tutorial de DetailsView de detalles, los registros de detalles aparecen encima de los registros maestros. en este tutorial usamos las técnicas de CSS para que los registros maestros floten a la izquierda de los detalles.

Junto con la visualización de informes de maestro y detalles, también se ha tenido la oportunidad de explorar cómo recuperar el número de productos asociados a cada categoría y cómo realizar la lógica del lado servidor cuando se hace clic en un botón LinkButton (o un botón o un objeto ImageButton) desde un repetidor.

En este tutorial se realiza el examen de los informes principales y detallados con los controles DataList y Repeater. En el siguiente conjunto de tutoriales se muestra cómo agregar funcionalidades de edición y eliminación al control DataList.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un tutorial sobre elementos de CSS flotantes con CSS
- [Posicionamiento de CSS](http://www.brainjar.com/css/positioning/) más información sobre la colocación de elementos con CSS
- [Diseñar contenido con HTML](http://www.w3schools.com/html/html_layout.asp) utilizando `<table>` s y otros elementos HTML para colocarlos

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Zack Jones. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [Siguiente](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
