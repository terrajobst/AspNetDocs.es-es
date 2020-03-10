---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Controles Web de datos anidadosC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial, exploraremos cómo usar un repetidor anidado dentro de otro repetidor. En los ejemplos se muestra cómo rellenar el repetidor interno ambos d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8ef15bebb2c29976274b0cca1d6ace434ccc55ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78481273"
---
# <a name="nested-data-web-controls-c"></a>Controles web de datos anidados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) de la aplicación de ejemplo o [descarga de PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> En este tutorial, exploraremos cómo usar un repetidor anidado dentro de otro repetidor. En los ejemplos se muestra cómo rellenar el repetidor interno de forma declarativa y mediante programación.

## <a name="introduction"></a>Introducción

Además de la sintaxis HTML y de enlace de los enlaces de los elementos estáticos, las plantillas también pueden incluir controles Web y controles de usuario. Estos controles Web pueden tener sus propiedades asignadas a través de la sintaxis declarativa de DataBinding o se puede obtener acceso a ellos mediante programación en los controladores de eventos del servidor apropiados.

Al incrustar controles en una plantilla, la apariencia y la experiencia del usuario se pueden personalizar y mejorar en. Por ejemplo, en el tutorial [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) , vimos cómo personalizar la presentación de GridView mediante la adición de un control de calendario en TemplateField para mostrar la fecha de contratación de un empleado. en [Agregar controles de validación a las interfaces de edición e inserción](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) y [Personalización de los](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutoriales de la interfaz de modificación de datos, vimos cómo personalizar las interfaces de edición e inserción agregando controles de validación, cuadros de texto, DropDownLists y otros controles Web.

Las plantillas también pueden contener otros controles Web de datos. Es decir, podemos tener una lista de controles que contenga otra DataList (o Repeater, GridView o DetailsView, etc.) dentro de sus plantillas. El desafío con este tipo de interfaz es enlazar los datos adecuados al control Web de datos internos. Hay algunos enfoques diferentes disponibles, que van desde las opciones declarativas mediante el uso de ObjectDataSource hasta los mediante programación.

En este tutorial, exploraremos cómo usar un repetidor anidado dentro de otro repetidor. El repetidor externo contendrá un elemento para cada categoría en la base de datos, mostrando el nombre y la descripción de la categoría. Cada elemento de categoría s Repeater interior mostrará información de cada producto perteneciente a esa categoría (consulte la ilustración 1) en una lista con viñetas. En nuestros ejemplos se muestra cómo rellenar el repetidor interno de forma declarativa y mediante programación.

[![cada categoría, junto con sus productos, se muestran](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Figura 1**: cada categoría, junto con sus productos, se enumeran ([haga clic para ver la imagen de tamaño completo](nested-data-web-controls-cs/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Paso 1: creación de la lista de categorías

Al compilar una página que usa controles Web de datos anidados, me resulta útil diseñar, crear y probar primero el control Web de datos más externos, sin preocuparse por el control anidado interno. Por lo tanto, vamos a empezar a recorrer los pasos necesarios para agregar un repetidor a la página que muestra el nombre y la descripción de cada categoría.

Para empezar, abra la página `NestedControls.aspx` en la carpeta `DataListRepeaterBasics` y agregue un control Repeater a la página, estableciendo su propiedad `ID` en `CategoryList`. En la etiqueta inteligente Repeater s, elija crear un nuevo ObjectDataSource denominado `CategoriesDataSource`.

[![nombre de la nueva CategoriesDataSource de ObjectDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Figura 2**: asigne un nombre al nuevo `CategoriesDataSource` ObjectDataSource ([haga clic para ver la imagen de tamaño completo](nested-data-web-controls-cs/_static/image6.png))

Configure ObjectDataSource para que extraiga sus datos del método `CategoriesBLL` Class s `GetCategories`.

[![configurar ObjectDataSource para usar el método CategoriesBLL de la clase s GetCategories](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Figura 3**: configuración de ObjectDataSource para usar el método `CategoriesBLL` Class s `GetCategories` ([haga clic para ver la imagen de tamaño completo](nested-data-web-controls-cs/_static/image9.png))

Para especificar el contenido de la plantilla de repetición, es necesario ir a la vista de código fuente y escribir manualmente la sintaxis declarativa. Agregue un `ItemTemplate` que muestre el nombre de la categoría en un elemento `<h4>` y la categoría s Descripción en un elemento de párrafo (`<p>`). Además, vamos a separar cada categoría con una regla horizontal (`<hr>`). Después de realizar estos cambios, la página debe contener la sintaxis declarativa de Repeater e ObjectDataSource, que es similar a la siguiente:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

En la figura 4 se muestra nuestro progreso cuando se ve a través de un explorador.

[![se muestra el nombre y la descripción de cada categoría, separados por una regla horizontal](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Figura 4**: se enumeran los nombres y las descripciones de cada categoría, separados por una regla horizontal ([haga clic para ver la imagen de tamaño completo](nested-data-web-controls-cs/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Paso 2: agregar el repetidor del producto anidado

Una vez completada la lista de categorías, la siguiente tarea consiste en agregar un repetidor al `CategoryList` s `ItemTemplate` que muestra información sobre los productos que pertenecen a la categoría adecuada. Hay varias maneras de recuperar los datos para este repetidor interno, dos de los cuales exploraremos en breve. Por ahora, vamos a crear el repetidor de productos en el `CategoryList` Repeater s `ItemTemplate`. En concreto, dejar que el repetidor del producto muestre cada producto en una lista con viñetas con cada elemento de la lista, incluidos el nombre y el precio del producto.

Para crear este repetidor, es necesario escribir manualmente la sintaxis declarativa y las plantillas de Repeater de repetición internas en el `CategoryList` s `ItemTemplate`. Agregue el siguiente marcado en el `CategoryList` Repeater s `ItemTemplate`:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Paso 3: enlazar los productos específicos de categoría al repetidor ProductsByCategoryList

Si visita la página a través de un explorador en este punto, la pantalla tendrá el mismo aspecto que en la figura 4 porque todavía vamos a enlazar los datos al repetidor. Hay varias maneras de captar los registros de producto adecuados y enlazarlos al repetidor, algo más eficaz que otros. El principal desafío aquí es obtener los productos adecuados para la categoría especificada.

Se puede tener acceso a los datos que se van a enlazar al control Repeater interno mediante declaración, a través de un ObjectDataSource en el `CategoryList` Repeater s `ItemTemplate`, o bien mediante programación, desde la página de código subyacente de la página ASP.NET. De forma similar, estos datos se pueden enlazar al repetidor interno mediante declaración, a través de la propiedad de `DataSourceID` Repeater interna o mediante la sintaxis de enlace de datos declarativo, o mediante programación haciendo referencia al repetidor interno en el controlador de eventos `CategoryList` Repeater s `ItemDataBound`, estableciendo mediante programación su propiedad `DataSource` y llamando a su método `DataBind()`. Analicemos cada uno de estos enfoques.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Obtener acceso a los datos mediante declaración con un control ObjectDataSource y el controlador de eventos`ItemDataBound`

Dado que en esta serie de tutoriales se usa el origen de datos, la opción más natural para obtener acceso a los datos de este ejemplo es ajustarse a ObjectDataSource. La clase `ProductsBLL` tiene un método `GetProductsByCategoryID(categoryID)` que devuelve información sobre los productos que pertenecen a la *`categoryID`* especificada. Por lo tanto, podemos agregar un ObjectDataSource al `CategoryList` Repeater s `ItemTemplate` y configurarlo para tener acceso a sus datos desde este método de clase.

Desafortunadamente, el repetidor no permite que sus plantillas se editen a través del Vista de diseño por lo que necesitamos agregar la sintaxis declarativa para este control ObjectDataSource a mano. La siguiente sintaxis muestra el `CategoryList` Repeater s `ItemTemplate` después de agregar este nuevo ObjectDataSource (`ProductsByCategoryDataSource`):

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Al usar el enfoque de ObjectDataSource, es necesario establecer la propiedad `ProductsByCategoryList` Repeater s `DataSourceID` en el `ID` de ObjectDataSource (`ProductsByCategoryDataSource`). Observe también que ObjectDataSource tiene un elemento `<asp:Parameter>` que especifica el valor *`categoryID`* que se pasará al método `GetProductsByCategoryID(categoryID)`. Pero, ¿cómo se especifica este valor? Idealmente, solo se puede establecer la propiedad `DefaultValue` del elemento `<asp:Parameter>` mediante la sintaxis de DataBinding, como la siguiente:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Desafortunadamente, la sintaxis de DataBinding solo es válida en los controles que tienen un evento `DataBinding`. La clase `Parameter` no tiene este tipo de evento y, por tanto, la sintaxis anterior no es válida y producirá un error en tiempo de ejecución.

Para establecer este valor, es necesario crear un controlador de eventos para el evento `CategoryList` Repeater s `ItemDataBound`. Recuerde que el evento `ItemDataBound` se activa una vez por cada elemento enlazado al repetidor. Por lo tanto, cada vez que este evento se activa para el repetidor externo, podemos asignar el valor de `CategoryID` actual al parámetro `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID`.

Cree un controlador de eventos para el evento `CategoryList` Repeater s `ItemDataBound` con el siguiente código:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Este controlador de eventos se inicia asegurándose de que hemos revelado con un elemento de datos en lugar de con el encabezado, el pie de página o el elemento separador. A continuación, hacemos referencia a la instancia de `CategoriesRow` real que acaba de enlazarse al `RepeaterItem`actual. Por último, se hace referencia al origen ObjectDataSource en el `ItemTemplate` y se asigna el valor del parámetro `CategoryID` al `CategoryID` del `RepeaterItem`actual.

Con este controlador de eventos, el `ProductsByCategoryList` repetidor de cada `RepeaterItem` se enlaza a los productos de la categoría `RepeaterItem` s. En la figura 5 se muestra una captura de pantalla de la salida resultante.

[![el repetidor externo muestra cada categoría; el otro interno enumera los productos de esa categoría](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Figura 5**: el repetidor externo muestra cada categoría; el interno uno enumera los productos de esa categoría ([haga clic para ver la imagen de tamaño completo](nested-data-web-controls-cs/_static/image15.png))

## <a name="accessing-the-products-by-category-data-programmatically"></a>Obtener acceso a los productos por datos de categoría mediante programación

En lugar de usar un ObjectDataSource para recuperar los productos de la categoría actual, podríamos crear un método en la clase de código subyacente de la página ASP.NET (o en la carpeta `App_Code` o en un proyecto de biblioteca de clases independiente) que devuelve el conjunto de productos adecuado cuando se pasa en un `CategoryID`. Imagine que teníamos un método de este tipo en la clase de código subyacente de la página ASP.NET y que se llamaba `GetProductsInCategory(categoryID)`. Con este método en su lugar, podríamos enlazar los productos de la categoría actual al repetidor interno mediante la sintaxis declarativa siguiente:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

La propiedad Repeater s `DataSource` usa la sintaxis de DataBinding para indicar que sus datos proceden del método `GetProductsInCategory(categoryID)`. Como `Eval("CategoryID")` devuelve un valor de tipo `Object`, convertiremos el objeto en un `Integer` antes de pasarlo al método `GetProductsInCategory(categoryID)`. Tenga en cuenta que el `CategoryID` al que se accede aquí a través de la sintaxis de DataBinding es el `CategoryID` en el repetidor *externo* (`CategoryList`), el que se enlaza a los registros de la tabla de `Categories`. Por lo tanto, sabemos que `CategoryID` no puede ser un valor de `NULL` de base de datos, por lo que podemos convertir ciegamente el método `Eval` sin comprobar si revisamos una `DBNull`.

Con este enfoque, es necesario crear el método de `GetProductsInCategory(categoryID)` y hacer que recupere el conjunto de productos adecuado dado el *`categoryID`* proporcionado. Para ello, basta con devolver el `ProductsDataTable` devuelto por el método `GetProductsByCategoryID(categoryID)` de la clase `ProductsBLL`. Permita crear el método de `GetProductsInCategory(categoryID)` en la clase de código subyacente para la página de `NestedControls.aspx`. Para ello, use el código siguiente:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Este método simplemente crea una instancia del método `ProductsBLL` y devuelve los resultados del método `GetProductsByCategoryID(categoryID)`. Tenga en cuenta que el método debe estar marcado como `Public` o `Protected`; Si el método está marcado `Private`, no será accesible desde el marcado declarativo de la página ASP.NET.

Después de realizar estos cambios para usar esta nueva técnica, dedique un momento a ver la página a través de un explorador. La salida debe ser idéntica a la salida cuando se usa el método de control de eventos ObjectDataSource y `ItemDataBound` (consulte la figura 5 para ver una captura de pantalla).

> [!NOTE]
> Puede parecer que ocupados cree el método `GetProductsInCategory(categoryID)` en la clase de código subyacente de la página ASP.NET. Después de todo, este método simplemente crea una instancia de la clase `ProductsBLL` y devuelve los resultados de su método `GetProductsByCategoryID(categoryID)`. Por qué no basta con llamar a este método directamente desde la sintaxis de DataBinding del Repeater interno, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Aunque esta sintaxis no funcionará con nuestra implementación actual de la clase `ProductsBLL` (dado que el método `GetProductsByCategoryID(categoryID)` es un método de instancia), podría modificar `ProductsBLL` para incluir un método `GetProductsByCategoryID(categoryID)` estático o hacer que la clase incluya un método `Instance()` estático para devolver una nueva instancia de la clase `ProductsBLL`.

Aunque estas modificaciones eliminarían la necesidad del método `GetProductsInCategory(categoryID)` en la clase de código subyacente de la página ASP.NET, el método de clase de código subyacente nos proporciona más flexibilidad al trabajar con los datos recuperados, como veremos en breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperación de toda la información del producto a la vez

Las dos técnicas anteriores que hemos examinado captan esos productos para la categoría actual realizando una llamada al método de `GetProductsByCategoryID(categoryID)` de `ProductsBLL` clase (el primer enfoque lo hacía a través de un ObjectDataSource, el segundo a través del método `GetProductsInCategory(categoryID)` en la clase de código subyacente). Cada vez que se invoca este método, el nivel de lógica de negocios llama a la capa de acceso a datos, que consulta la base de datos con una instrucción SQL que devuelve filas de la tabla `Products` cuyo campo `CategoryID` coincide con el parámetro de entrada proporcionado.

Dado *n* categorías en el sistema, este enfoque se redirecciona *n* + 1 a la base de datos una consulta de base de datos para obtener todas las categorías y, a continuación, *N* llamadas para obtener los productos específicos de cada categoría. Sin embargo, podemos recuperar todos los datos necesarios en solo dos llamadas de base de datos una llamada para obtener todas las categorías y otra para obtener todos los productos. Una vez que tenemos todos los productos, podemos filtrar esos productos para que solo los productos que coincidan con el `CategoryID` actual se enlacen a la repetidor interior de esa categoría.

Para proporcionar esta funcionalidad, solo necesitamos realizar una pequeña modificación en el método `GetProductsInCategory(categoryID)` de nuestra clase de código subyacente de la página ASP.NET. En lugar de devolver ciegamente los resultados del método de `GetProductsByCategoryID(categoryID)` de clase `ProductsBLL` s, podemos acceder primero a *todos* los productos (si ya no se ha tenido acceso a ellos) y, a continuación, devolver solo la vista filtrada de los productos en función de la `CategoryID`pasada.

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Observe la adición de la variable de nivel de página, `allProducts`. Esto contiene información sobre todos los productos y se rellena la primera vez que se invoca el método de `GetProductsInCategory(categoryID)`. Después de asegurarse de que el objeto de `allProducts` se ha creado y rellenado, el método filtra los resultados de DataTable s de modo que solo se pueda tener acceso a las filas cuyo `CategoryID` coincida con el `CategoryID` especificado. Este enfoque reduce el número de veces que se tiene acceso a la base de datos de *N* + 1 a dos.

Esta mejora no introduce ningún cambio en el marcado representado de la página, ni devuelve menos registros que el otro enfoque. Simplemente reduce el número de llamadas a la base de datos.

> [!NOTE]
> Un motivo intuitivo podría ser que reducir el número de accesos de base de datos ciertamente mejorar el rendimiento. Sin embargo, esto podría no ser el caso. Si tiene un gran número de productos cuyo `CategoryID` es `NULL`, por ejemplo, la llamada al método `GetProducts` devuelve un número de productos que nunca se muestran. Además, la devolución de todos los productos puede ser una pérdida si solo se muestra un subconjunto de las categorías, lo que podría ser el caso si se ha implementado la paginación.

Como siempre, cuando se trata de analizar el rendimiento de dos técnicas, la única medida SureFire es ejecutar pruebas controladas adaptadas a los escenarios de casos comunes de la aplicación.

## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo anidar un control Web de datos dentro de otro, examinando específicamente cómo hacer que un repetidor externo muestre un elemento para cada categoría con un repetidor interno que Enumere los productos de cada categoría en una lista con viñetas. El principal desafío en la creación de una interfaz de usuario anidada radica en obtener acceso y enlazar los datos correctos al control Web de datos internos. Hay varias técnicas disponibles, dos de las cuales se han examinado en este tutorial. En el primer enfoque examinado se usaba un ObjectDataSource en el control Web de datos externos `ItemTemplate` que estaba enlazado al control Web de datos interno a través de su propiedad `DataSourceID`. La segunda técnica tuvo acceso a los datos a través de un método en la clase de código subyacente de la página ASP.NET. A continuación, este método se puede enlazar a la propiedad de `DataSource` de control Web de datos internos a través de la sintaxis de enlace de datos.

Aunque la interfaz de usuario anidada examinada en este tutorial usaba un repetidor anidado dentro de un repetidor, estas técnicas se pueden extender a otros controles Web de datos. Puede anidar un repetidor dentro de un control GridView, un control GridView dentro de un objeto DataList, etc.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Zack Jones y Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Siguiente](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
