---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Filtrado de maestro y detalles con DropDownList (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo mostrar los informes maestros y detallados en una sola página web mediante DropDownLists para mostrar los registros ' maestros ' y una lista de archivos para que se exporten...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 8289f46fd6d0143802269d5c6196a4c40db9378c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78477241"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Filtrado de maestro y detalles con DropDownList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> En este tutorial, veremos cómo mostrar los informes maestro y detalles en una sola página web mediante DropDownLists para mostrar los registros "maestros" y una lista de datos para mostrar los detalles.

## <a name="introduction"></a>Introducción

El informe principal/detalle, que primero creamos con GridView en el tutorial de [filtrado maestro y detalles anterior con DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , comienza mostrando un conjunto de registros "maestros". A continuación, el usuario puede profundizar en uno de los registros maestros y, por lo tanto, ver los "detalles" de ese registro maestro. Los informes maestros y detallados son la opción ideal para visualizar relaciones uno a varios y para mostrar información detallada de tablas "anchas" (que tienen muchas columnas). Hemos explorado cómo se implementan los informes maestros y detallados con los controles GridView y DetailsView en los tutoriales anteriores. En este tutorial y en los dos siguientes, volveremos a examinar estos conceptos, pero nos centraremos en el uso de controles DataList y Repeater en su lugar.

En este tutorial, veremos cómo usar un DropDownList para contener los registros "maestros", con los registros "details" que se muestran en una lista de datos.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial principal/detalle

Antes de comenzar este tutorial, vamos a dedicar un momento a agregar la carpeta y las páginas de ASP.NET que necesitaremos en este tutorial y en las dos siguientes relacionadas con los informes maestros y detallados con los controles DataList y Repeater. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterFiltering`. A continuación, agregue las siguientes cinco páginas de ASP.NET a esta carpeta, de forma que todas ellas estén configuradas para usar la página maestra `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Cree una carpeta DataListRepeaterFiltering y agregue el tutorial ASP.NET páginas](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: creación de una carpeta de `DataListRepeaterFiltering` y adición de las páginas del tutorial ASP.net

A continuación, abra la página `Default.aspx` y arrastre el control de usuario `SectionLevelTutorialListing.ascx` desde la carpeta `UserControls` hasta la superficie de diseño. Este control de usuario, que hemos creado en el tutorial [páginas maestras y navegación de sitios](../introduction/master-pages-and-site-navigation-cs.md) , enumera el mapa del sitio y muestra los tutoriales de la sección actual en una lista con viñetas.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))

Para que la lista con viñetas muestre los tutoriales principales y detallados que vamos a crear, es necesario agregarlos al mapa del sitio. Abra el archivo de `Web.sitemap` y agregue el siguiente marcado después del marcado de nodo "Mostrar datos con el mapa de datos y repetidor":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]

![Actualización del mapa del sitio para incluir las nuevas páginas de ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: actualización del mapa del sitio para incluir las nuevas páginas de ASP.net

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Paso 2: mostrar las categorías en un DropDownList

En el informe maestro y detalles se enumeran las categorías de un DropDownList, con los productos del elemento de la lista seleccionados más abajo en la página de una lista de elementos. La primera tarea, a continuación, es hacer que las categorías se muestren en un DropDownList. Comience abriendo la página `FilterByDropDownList.aspx` en la carpeta `DataListRepeaterFiltering` y arrastre un DropDownList desde el cuadro de herramientas hasta el diseñador de la página. A continuación, establezca la propiedad `ID` de DropDownList en `Categories`. Haga clic en el vínculo elegir origen de datos de la etiqueta inteligente de DropDownList y cree un nuevo ObjectDataSource denominado `CategoriesDataSource`.

[![agregar un nuevo ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: adición de un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))

Configure el nuevo ObjectDataSource para que invoque el método de `GetCategories()` de la clase `CategoriesBLL`. Después de configurar el ObjectDataSource, todavía es necesario especificar qué campo de origen de datos se debe mostrar en el DropDownList y cuál debe asociarse como valor de cada elemento de la lista. Tenga el campo `CategoryName` como pantalla y `CategoryID` como el valor de cada elemento de la lista.

[![que el DropDownList muestre el campo CategoryName y use CategoryID como valor](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: haga que el DropDownList muestre el campo `CategoryName` y use `CategoryID` como valor ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))

En este momento, tenemos un control DropDownList que se rellena con los registros de la tabla `Categories` (todo se realiza en unos seis segundos). En la ilustración 6 se muestra nuestro progreso hasta ahora cuando se ve a través de un explorador.

[![una lista desplegable muestra las categorías actuales](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: lista desplegable de las categorías actuales ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Paso 2: agregar la lista de los productos

El último paso del informe maestro y detalles es enumerar los productos asociados a la categoría seleccionada. Para ello, agregue un DataList a la página y cree un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource`. Haga que el control `ProductsByCategoryDataSource` recupere sus datos del método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`. Puesto que este informe maestro y detalles es de solo lectura, elija la opción (ninguno) en las pestañas insertar, actualizar y eliminar.

[![seleccionar el método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: seleccione el método `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))

Después de hacer clic en siguiente, el Asistente para ObjectDataSource nos solicitará el origen del valor del parámetro *`categoryID`* del método `GetProductsByCategoryID(categoryID)`. Para usar el valor del elemento `categories` DropDownList seleccionado, establezca el origen del parámetro en control y el ControlID en `Categories`.

[![establecer el parámetro categoryID en el valor de las categorías DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: establezca el parámetro *`categoryID`* en el valor de la `Categories` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio generará automáticamente una `ItemTemplate` para la lista de datos que muestra el nombre y el valor de cada campo de datos. Vamos a mejorar la lista de elementos para usar en su lugar una `ItemTemplate` que muestra solo el nombre, la categoría, el proveedor, la cantidad por unidad y el precio del producto junto con un `SeparatorTemplate` que inserta un elemento `<hr>` entre cada elemento. Voy a usar la `ItemTemplate` de un ejemplo en el tutorial [Mostrar datos con los controles DataList y Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) , pero no dude en usar el marcado de la plantilla que considere más atractivo visualmente.

Después de realizar estos cambios, la lista de elementos y el marcado de ObjectDataSource deben tener un aspecto similar al siguiente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Dedique un momento a consultar nuestro progreso en un explorador. Al visitar la página por primera vez, se muestran los productos que pertenecen a la categoría seleccionada (bebidas) (como se muestra en la figura 9), pero el cambio de DropDownList no actualiza los datos. Esto se debe a que hay que hacer un postback para que la lista de DataList se actualice. Para ello, podemos establecer la propiedad `AutoPostBack` de DropDownList en `true` o agregar un control Web de botón a la página. En este tutorial, he optado por establecer la propiedad `AutoPostBack` de DropDownList en `true`.

Las figuras 9 y 10 muestran el informe maestro y detalles en acción.

[![al visitar la página por primera vez, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: al visitar la página por primera vez, se muestran los productos de bebidas ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))

[![seleccionar un nuevo producto (producir) genera automáticamente un postback, actualizando DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: seleccionar un nuevo producto (producir) genera automáticamente un postback, actualizar la lista de los ([hacer clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Adición de un elemento de lista "--Choose a Category--"

Al visitar por primera vez la página `FilterByDropDownList.aspx`, el primer elemento de la lista (bebidas) de DropDownList de la categoría está seleccionado de forma predeterminada y muestra los productos de bebidas en la lista de elementos. En el tutorial *maestro y detalle filtrado con DropDownList* , se ha agregado la opción "--Choose a Category--" a DropDownList que se seleccionó de forma predeterminada y, cuando se selecciona, se muestran *todos* los productos de la base de datos. Este enfoque se podía administrar al enumerar los productos en un control GridView, ya que cada fila del producto ocupaba una pequeña cantidad de espacio real en pantalla. Sin embargo, con la lista de datos, la información de cada producto consume un fragmento mucho más grande de la pantalla. Vamos a agregar una opción "--Choose a Category--" y seleccionarla de forma predeterminada, pero en lugar de Mostrar todos los productos cuando se seleccionan, vamos a configurarlo para que no muestre ningún producto.

Para agregar un nuevo elemento de lista a DropDownList, vaya al ventana Propiedades y haga clic en los puntos suspensivos de la propiedad `Items`. Agregue un nuevo elemento de lista con el `Text` "--Choose a Category--" y el `Value` `0`.

![Agregar un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: adición de un elemento de lista "--Choose a Category--"

Como alternativa, puede Agregar el elemento de lista agregando el siguiente marcado a DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Además, es necesario establecer el `AppendDataBoundItems` del control DropDownList en `true` porque, si se establece en `false` (valor predeterminado), cuando las categorías se enlazan a DropDownList desde ObjectDataSource, se sobrescribirán todos los elementos de lista agregados manualmente.

![Establezca la propiedad AppendDataBoundItems en true.](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: establecimiento de la propiedad `AppendDataBoundItems` en true

La razón por la que hemos elegido el valor `0` para el elemento de lista "--Choose a Category--" es porque no hay ninguna categoría en el sistema con un valor de `0`, por lo que no se devolverá ningún registro del producto cuando se seleccione el elemento de lista "--Choose a Category--". Para confirmarlo, dedique un momento a visitar la página a través de un explorador. Como se muestra en la figura 13, al ver la página inicialmente, se selecciona el elemento de lista "--elegir una categoría--" y no se muestra ningún producto.

[![cuando el](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: cuando se selecciona el elemento de lista "--elegir una categoría--", no se muestra ningún producto ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))

Si prefiere mostrar *todos* los productos cuando está seleccionada la opción "--elegir una categoría--", use un valor de `-1` en su lugar. El lector astutos lo recordará de nuevo en el tutorial de *filtrado maestro y detalles con DropDownList* . hemos actualizado el método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL` para que, si se pasa un valor *`categoryID`* de `-1`, se devolvieran todos los registros del producto.

## <a name="summary"></a>Resumen

Al Mostrar datos relacionados jerárquicamente, a menudo resulta útil presentar los datos mediante los informes maestros y detallados, desde el que el usuario puede empezar a usar los datos de la parte superior de la jerarquía y profundizar en los detalles. En este tutorial, hemos examinado la creación de un informe de maestro y detalles sencillo que muestra los productos de una categoría seleccionada. Esto se logra mediante el uso de un DropDownList para la lista de categorías y un DataList para los productos que pertenecen a la categoría seleccionada.

En el siguiente tutorial, veremos cómo separar los registros maestro y detalles en dos páginas. En la primera página, se mostrará una lista de los registros "maestros", con un vínculo para ver los detalles. Al hacer clic en el vínculo, se le redirigirá al usuario a la segunda página, con lo que se mostrarán los detalles del registro maestro seleccionado.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Randy Schmidt. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](master-detail-filtering-acess-two-pages-datalist-cs.md)
