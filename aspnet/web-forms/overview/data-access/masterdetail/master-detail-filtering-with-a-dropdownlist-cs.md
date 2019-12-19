---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
title: Filtrado de maestro y detalles con DropDownList (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en GridView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 53e659cc-eefb-40c1-a1dc-559481c99443
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-a-dropdownlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 3ec549f9da7a2b3a021e77827f0039e6ae60b5c5
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616065"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Filtrado de maestro y detalles con DropDownList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_7_CS.exe) de la aplicación de ejemplo o [descarga de PDF](master-detail-filtering-with-a-dropdownlist-cs/_static/datatutorial07cs1.pdf)

> En este tutorial, veremos cómo mostrar los registros maestros en un control DropDownList y los detalles del elemento de lista seleccionado en GridView.

## <a name="introduction"></a>Introducción

Un tipo común de informe es el *Informe principal/detalle*, en el que el informe comienza mostrando algún conjunto de registros "maestros". A continuación, el usuario puede profundizar en uno de los registros maestros y, por lo tanto, ver los "detalles" de ese registro maestro. Los informes maestros y detallados son una opción ideal para visualizar relaciones uno a varios, como un informe que muestre todas las categorías y, a continuación, permitir que un usuario seleccione una categoría determinada y muestre sus productos asociados. Además, los informes maestros y detallados son útiles para mostrar información detallada de tablas "amplias" (que tienen muchas columnas). Por ejemplo, el nivel "principal" de un informe principal/detalle podría mostrar solo el nombre del producto y el precio unitario de los productos de la base de datos, y explorar en profundidad un producto concreto mostraría los campos de producto adicionales (categoría, proveedor, cantidad por unidad, etc.).

Hay muchas maneras con las que se puede implementar un informe maestro y detalles. En este y en los tres tutoriales siguientes veremos una variedad de informes maestros y detallados. En este tutorial, veremos cómo mostrar los registros maestros en un [control DropDownList](https://msdn.microsoft.com/library/dtx91y0z.aspx) y los detalles del elemento de lista seleccionado en GridView. En concreto, el informe maestro y detalles de este tutorial mostrará la información de la categoría y del producto.

## <a name="step-1-displaying-the-categories-in-a-dropdownlist"></a>Paso 1: Mostrar las categorías de un DropDownList

En el informe maestro y detalles se enumeran las categorías de un DropDownList, con los productos del elemento de la lista seleccionados más abajo en la página en un control GridView. La primera tarea, a continuación, es hacer que las categorías se muestren en un DropDownList. Abra la página `FilterByDropDownList.aspx` de la carpeta `Filtering`, arrastre un DropDownList desde el cuadro de herramientas hasta el diseñador de la página y establezca su propiedad `ID` en `Categories`. A continuación, haga clic en el vínculo elegir origen de datos de la etiqueta inteligente de DropDownList. Se mostrará el Asistente para la configuración de orígenes de datos.

[![especificar el origen de datos de DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image2.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image1.png)

**Figura 1**: Especifique el origen de datos de DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image3.png))

Elija Agregar un nuevo ObjectDataSource denominado `CategoriesDataSource` que invoque el método `GetCategories()` de la clase `CategoriesBLL`.

[![agregar un nuevo ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-cs/_static/image5.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image4.png)

**Figura 2**: Agregue un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image6.png))

[![optar por usar la clase CategoriesBLL](master-detail-filtering-with-a-dropdownlist-cs/_static/image8.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image7.png)

**Figura 3**: Elija usar la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image9.png))

[![configurar ObjectDataSource para usar el método GetCategories ()](master-detail-filtering-with-a-dropdownlist-cs/_static/image11.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image10.png)

**Figura 4**: Configurar ObjectDataSource para usar el método `GetCategories()` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image12.png))

Después de configurar el ObjectDataSource, todavía necesitamos especificar qué campo de origen de datos se debe mostrar en DropDownList y cuál debe asociarse como valor para el elemento de lista. Tenga el campo `CategoryName` como pantalla y `CategoryID` como el valor de cada elemento de la lista.

[![que el DropDownList muestre el campo CategoryName y use CategoryID como valor](master-detail-filtering-with-a-dropdownlist-cs/_static/image14.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image13.png)

**Figura 5**: Haga que el DropDownList muestre el campo `CategoryName` y use `CategoryID` como valor ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image15.png))

En este momento, tenemos un control DropDownList que se rellena con los registros de la tabla `Categories` (todo se realiza en unos seis segundos). En la ilustración 6 se muestra nuestro progreso hasta ahora cuando se ve a través de un explorador.

[![una lista desplegable muestra las categorías actuales](master-detail-filtering-with-a-dropdownlist-cs/_static/image17.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image16.png)

**Ilustración 6**: Una lista desplegable muestra las categorías actuales ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image18.png))

## <a name="step-2-adding-the-products-gridview"></a>Paso 2: Agregar los productos GridView

El último paso en el informe maestro y detalles es enumerar los productos asociados a la categoría seleccionada. Para ello, agregue un control GridView a la página y cree un nuevo ObjectDataSource denominado `productsDataSource`. Haga que el control `productsDataSource` represente sus datos del método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`.

[![seleccionar el método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-cs/_static/image20.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image19.png)

**Figura 7**: Seleccione el método `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image21.png))

Después de elegir este método, el Asistente para ObjectDataSource nos pide el valor del parámetro *`categoryID`* del método. Para usar el valor del elemento `categories` DropDownList seleccionado, establezca el origen del parámetro en control y el ControlID en `Categories`.

[![establecer el parámetro categoryID en el valor de las categorías DropDownList](master-detail-filtering-with-a-dropdownlist-cs/_static/image23.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image22.png)

**Figura 8**: Establezca el parámetro *`categoryID`* en el valor de la `Categories` DropDownList ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image24.png))

Dedique un momento a consultar nuestro progreso en un explorador. Al visitar la página por primera vez, se muestran los productos que pertenecen a la categoría seleccionada (bebidas) (como se muestra en la figura 9), pero el cambio de DropDownList no actualiza los datos. Esto se debe a que hay que hacer un postback para que GridView se actualice. Para lograr esto, tenemos dos opciones (ninguno de los cuales requiere la escritura de código):

- **Establezca la propiedad** [AutoPostBack](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listcontrol.autopostback%28VS.80%29.aspx) **de la categoría de DropDownList en true.** (Para ello, active la opción Habilitar AutoPostBack en la etiqueta inteligente de DropDownList). Esto desencadenará un postback cuando el usuario cambie el elemento seleccionado de DropDownList. Por lo tanto, cuando el usuario selecciona una nueva categoría del DropDownList, se producirá un postback y el control GridView se actualizará con los productos de la categoría que se acaba de seleccionar. (Este es el enfoque que he usado en este tutorial).
- **Agregue un control Web Button junto a DropDownList.** Establezca su propiedad `Text` en actualizar o algo similar. Con este enfoque, el usuario deberá seleccionar una nueva categoría y, a continuación, hacer clic en el botón. Al hacer clic en el botón, se producirá un postback y se actualizará el control GridView para mostrar los productos de la categoría seleccionada.

Las figuras 9 y 10 muestran el informe maestro y detalles en acción.

[![al visitar la página por primera vez, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-cs/_static/image26.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image25.png)

**Ilustración 9**: Al visitar la página por primera vez, se muestran los productos de bebidas ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image27.png))

[![seleccionar un nuevo producto (producir) genera automáticamente un postback, actualizando GridView](master-detail-filtering-with-a-dropdownlist-cs/_static/image29.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image28.png)

**Figura 10**: Al seleccionar un nuevo producto (producir) se genera automáticamente un postback, actualizando GridView ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image30.png))

## <a name="adding-a----choose-a-category----list-item"></a>Adición de un elemento de lista "--Choose a Category--"

Al visitar por primera vez la página `FilterByDropDownList.aspx`, el primer elemento de la lista (bebidas) de DropDownList de la categoría está seleccionado de forma predeterminada y muestra los productos de bebidas en GridView. En lugar de mostrar los productos de la primera categoría, es posible que quiera tener un elemento DropDownList seleccionado que indique algo parecido a "--Choose a Category--".

Para agregar un nuevo elemento de lista a DropDownList, vaya al ventana Propiedades y haga clic en los puntos suspensivos de la propiedad `Items`. Agregue un nuevo elemento de lista con el `Text` "--Choose a Category--" y el `Value` `-1`.

[![agregar a: elegir una categoría: elemento de lista](master-detail-filtering-with-a-dropdownlist-cs/_static/image32.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image31.png)

**Figura 11**: Agregar un: elija una categoría: elemento de lista ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image33.png))

Como alternativa, puede Agregar el elemento de lista agregando el siguiente marcado a DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample1.aspx)]

Además, es necesario establecer el `AppendDataBoundItems` del control DropDownList en true porque cuando las categorías están enlazadas a DropDownList desde ObjectDataSource, se sobrescribirán los elementos de la lista agregados manualmente si `AppendDataBoundItems` no es true.

![Establezca la propiedad AppendDataBoundItems en true.](master-detail-filtering-with-a-dropdownlist-cs/_static/image34.png)

**Figura 12**: Establezca la propiedad `AppendDataBoundItems` en true.

Después de estos cambios, al visitar la página por primera vez, se selecciona la opción "--elegir una categoría--" y no se muestra ningún producto.

[![en la carga de página inicial no se muestran productos](master-detail-filtering-with-a-dropdownlist-cs/_static/image36.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image35.png)

**Figura 13**: En la página inicial, no se muestra ningún producto ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image37.png))

Motivo de que no se muestre ningún producto cuando se selecciona el elemento de lista "--elegir una categoría--" es porque su valor es `-1` y no hay ningún producto en la base de datos con un `CategoryID` de `-1`. Si este es el comportamiento que desea, habrá terminado en este momento. Sin embargo, si desea mostrar *todas* las categorías cuando se selecciona el elemento de lista "--Choose a Category--", vuelva a la clase `ProductsBLL` y personalice el método `GetProductsByCategoryID(categoryID)` para que invoque el método `GetProducts()` si el parámetro pasado *`categoryID`* es menor que cero:

[!code-csharp[Main](master-detail-filtering-with-a-dropdownlist-cs/samples/sample2.cs)]

La técnica que se usa aquí es similar al enfoque que usamos para volver a mostrar todos los proveedores en el tutorial de [parámetros declarativos](../basic-reporting/declarative-parameters-cs.md) , aunque en este ejemplo usamos un valor de `-1` para indicar que todos los registros deben recuperarse en contraposición a `null`. Esto se debe a que el parámetro *`categoryID`* del método `GetProductsByCategoryID(categoryID)` espera como valor entero pasado, mientras que en el tutorial de parámetros declarativos pasamos en un parámetro de entrada de cadena.

En la ilustración 14 se muestra una captura de pantalla de `FilterByDropDownList.aspx` cuando está seleccionada la opción "--elegir una categoría--". En este caso, todos los productos se muestran de forma predeterminada y el usuario puede restringir la presentación seleccionando una categoría específica.

[![todos los productos aparecen ahora de forma predeterminada](master-detail-filtering-with-a-dropdownlist-cs/_static/image39.png)](master-detail-filtering-with-a-dropdownlist-cs/_static/image38.png)

**Figura 14**: Todos los productos aparecen ahora de forma predeterminada ([haga clic para ver la imagen de tamaño completo](master-detail-filtering-with-a-dropdownlist-cs/_static/image40.png))

## <a name="summary"></a>Resumen

Al Mostrar datos relacionados jerárquicamente, a menudo resulta útil presentar los datos mediante los informes maestros y detallados, desde el que el usuario puede empezar a usar los datos de la parte superior de la jerarquía y profundizar en los detalles. En este tutorial, hemos examinado la creación de un informe de maestro y detalles sencillo que muestra los productos de una categoría seleccionada. Esto se logra mediante el uso de un DropDownList para la lista de categorías y un control GridView para los productos que pertenecen a la categoría seleccionada.

En el [siguiente tutorial](master-detail-filtering-with-two-dropdownlists-cs.md) , vamos a seguir la interfaz de DropDownList un paso más, con dos DropDownLists.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Siguiente](master-detail-filtering-with-two-dropdownlists-cs.md)
