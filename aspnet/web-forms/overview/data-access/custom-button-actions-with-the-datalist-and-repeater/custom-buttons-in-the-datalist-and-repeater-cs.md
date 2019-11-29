---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Botones personalizados en los controles DataList y Repeater (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, vamos a crear una interfaz que usa un repetidor para enumerar las categorías del sistema, donde cada categoría proporciona un botón para mostrar su associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: e8cb1054068327c25e057b6df1cc7506feec8d37
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601777"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Botones personalizados en los controles DataList y Repeater (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) de la aplicación de ejemplo o [descarga de PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> En este tutorial, vamos a crear una interfaz que usa un repetidor para enumerar las categorías del sistema, donde cada categoría proporciona un botón para mostrar sus productos asociados mediante un control BulletedList.

## <a name="introduction"></a>Introducción

En los últimos diecisiete tutoriales de DataList y Repeater, hemos creado ejemplos de solo lectura y ejemplos de edición y eliminación. Para facilitar la edición y eliminación de las capacidades de una lista de elementos, se han agregado botones a la `ItemTemplate` DataList, que, cuando se hace clic en él, causó un postback y genera un evento DataList correspondiente a la propiedad Button s `CommandName`. Por ejemplo, si se agrega un botón a la `ItemTemplate` con un valor de propiedad `CommandName` de Edit, el `EditCommand` DataList se activará en el PostBack; una con la `CommandName` Delete genera el `DeleteCommand`.

Además de los botones editar y eliminar, los controles DataList y Repeater también pueden incluir botones, LinkButtons o ImageButtons que, cuando se hace clic en él, realizan alguna lógica personalizada del lado servidor. En este tutorial, vamos a crear una interfaz que usa un repetidor para enumerar las categorías del sistema. Para cada categoría, el repetidor incluirá un botón para mostrar los productos de categoría asociados mediante un control BulletedList (vea la figura 1).

[![hacer clic en el vínculo Mostrar productos muestra los productos de la categoría s en una lista con viñetas](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: al hacer clic en el vínculo Mostrar productos se muestran los productos de la categoría s en una lista con viñetas ([haga clic para ver la imagen a tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial de botones personalizados

Antes de echar un vistazo a cómo agregar un botón personalizado, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial. Comience agregando una nueva carpeta denominada `CustomButtonsDataListRepeater`. Después, agregue las dos páginas de ASP.NET siguientes a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Agregue las páginas ASP.NET para los tutoriales relacionados con los botones personalizados.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: adición de las páginas de ASP.net para los tutoriales relacionados con los botones personalizados

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `CustomButtonsDataListRepeater` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))

Por último, agregue las páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de la paginación y la ordenación con las `<siteMapNode>`DataList y Repeater:

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición, inserción y eliminación.

![Ahora, el mapa del sitio incluye la entrada para el tutorial de botones personalizados.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: ahora, el mapa del sitio incluye la entrada para el tutorial de botones personalizados.

## <a name="step-2-adding-the-list-of-categories"></a>Paso 2: agregar la lista de categorías

En este tutorial, es necesario crear un repetidor que muestre todas las categorías junto con un objeto LinkButton show Products que, cuando se hace clic en él, muestra los productos de categoría s asociados en una lista con viñetas. Vamos a crear primero un repetidor sencillo que enumera las categorías del sistema. Para empezar, abra la página `CustomButtons.aspx` de la carpeta `CustomButtonsDataListRepeater`. Arrastre un repetidor desde el cuadro de herramientas hasta el diseñador y establezca su propiedad `ID` en `Categories`. A continuación, cree un nuevo control de origen de datos a partir de la etiqueta inteligente Repeater s. En concreto, cree un nuevo control ObjectDataSource denominado `CategoriesDataSource` que seleccione sus datos del método `GetCategories()` de la clase `CategoriesBLL`.

[![configurar ObjectDataSource para usar el método CategoriesBLL de clase s GetCategories ()](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configuración de ObjectDataSource para usar el método `CategoriesBLL` Class s `GetCategories()` ([haga clic para ver la imagen de tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))

A diferencia del control DataList, para el que Visual Studio crea un `ItemTemplate` predeterminado basado en el origen de datos, las plantillas Repeater s deben definirse manualmente. Además, las plantillas de repetidor deben crearse y editarse mediante declaración (es decir, no hay ninguna opción editar plantillas en la etiqueta inteligente Repeater s).

Haga clic en la pestaña origen en la esquina inferior izquierda y agregue una `ItemTemplate` que muestre el nombre de la categoría en un elemento `<h3>` y su descripción en una etiqueta de párrafo; incluya una `SeparatorTemplate` que muestre una regla horizontal (`<hr />`) entre cada categoría. Agregue también un LinkButton con su propiedad `Text` establecida para mostrar los productos. Después de completar estos pasos, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

En la ilustración 6 se muestra la página cuando se ve a través de un explorador. Se enumeran los nombres y las descripciones de cada categoría. El botón Mostrar productos, cuando se hace clic en él, produce un postback, pero aún no realiza ninguna acción.

[![se muestra el nombre y la descripción de cada categoría, junto con un LinkButton de show Products](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: se muestra el nombre y la descripción de cada categoría, junto con un LinkButton Mostrar productos ([haga clic para ver la imagen de tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Paso 3: ejecutar la lógica del lado servidor cuando se hace clic en Mostrar productos LinkButton

Cada vez que se hace clic en un botón, LinkButton o ImageButton dentro de una plantilla de una lista de controles o un Repeater, se produce un postback y se activa el evento DataList o Repeater `ItemCommand`. Además del evento `ItemCommand`, el control DataList también puede producir otro evento más específico si la propiedad Button s `CommandName` está establecida en una de las cadenas reservadas (Delete, Edit, CANCEL, Update o Select), pero *siempre* se activa el evento `ItemCommand`.

Cuando se hace clic en un botón dentro de un control DataList o Repeater, a menudo es necesario pasar el botón en el que se hizo clic (en el caso de que haya varios botones dentro del control, como el botón Editar y eliminar) y quizás alguna información adicional (por ejemplo, valor de la clave principal del elemento cuyo botón se hizo clic. El botón, LinkButton e ImageButton proporcionan dos propiedades cuyos valores se pasan al controlador de eventos `ItemCommand`:

- `CommandName` una cadena que se usa normalmente para identificar cada botón de la plantilla
- `CommandArgument` suele usarse para contener el valor de algún campo de datos, como el valor de clave principal

En este ejemplo, establezca la propiedad LinkButton s `CommandName` en ShowProducts y enlace el valor de la clave principal del registro actual `CategoryID` con la propiedad `CommandArgument` mediante la sintaxis de DataBinding `CategoryArgument='<%# Eval("CategoryID") %>'`. Después de especificar estas dos propiedades, la sintaxis declarativa de LinkButton s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Al hacer clic en el botón, se produce un postback y se desencadena el evento DataList o Repeater `ItemCommand`. Al controlador de eventos se le pasan los valores de los botones `CommandName` y `CommandArgument`.

Cree un controlador de eventos para el evento Repeater s `ItemCommand` y anote el segundo parámetro que se pasa al controlador de eventos (denominado `e`). Este segundo parámetro es de tipo [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) y tiene las cuatro propiedades siguientes:

- `CommandArgument` el valor de la propiedad Button `CommandArgument`
- `CommandName` el valor de la propiedad Button `CommandName`
- `CommandSource` una referencia al control de botón en el que se ha hecho clic
- `Item` una referencia al [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) que contiene el botón en el que se hizo clic; cada registro enlazado al repetidor se manifiesta como un `RepeaterItem`

Como la categoría `CategoryID` seleccionada se pasa a través de la propiedad `CommandArgument`, podemos obtener el conjunto de productos asociados a la categoría seleccionada en el controlador de eventos `ItemCommand`. Después, estos productos se pueden enlazar a un control BulletedList en el `ItemTemplate` (que todavía se van a agregar). Lo único que queda, es agregar el BulletedList, hacer referencia a él en el controlador de eventos `ItemCommand` y enlazarlo con el conjunto de productos de la categoría seleccionada, que abordaremos en el paso 4.

> [!NOTE]
> El controlador de eventos DataList s `ItemCommand` se pasa a un objeto de tipo [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), que ofrece las mismas cuatro propiedades que la clase `RepeaterCommandEventArgs`.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Paso 4: mostrar los productos de la categoría seleccionados en una lista con viñetas

Los productos de la categoría s seleccionados se pueden mostrar en el `ItemTemplate` repetidor con cualquier número de controles. Podríamos agregar otro Repeater anidado, un objeto DataList, DropDownList, GridView, etc. A pesar de que queremos mostrar los productos como una lista con viñetas, usaremos el control BulletedList. Volviendo al marcado declarativo de la página de `CustomButtons.aspx`, agregue un control BulletedList al `ItemTemplate` después del LinkButton Mostrar productos. Establezca BulletedLists s `ID` en `ProductsInCategory`. BulletedList muestra el valor del campo de datos especificado a través de la propiedad `DataTextField`; puesto que este control tendrá información de productos enlazada, establezca la propiedad `DataTextField` en `ProductName`.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

En el controlador de eventos `ItemCommand`, haga referencia a este control mediante `e.Item.FindControl("ProductsInCategory")` y enlácelo al conjunto de productos asociados a la categoría seleccionada.

[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Antes de realizar cualquier acción en el controlador de eventos `ItemCommand`, es prudente comprobar primero el valor de la `CommandName`entrante. Puesto que el controlador de eventos `ItemCommand` se activa cuando se hace clic en *cualquier* botón, si hay varios botones en la plantilla, use el valor `CommandName` para discernir qué acción realizar. La comprobación de la `CommandName` aquí es Moot, ya que solo tenemos un botón, pero es un buen hábito formar. A continuación, el `CategoryID` de la categoría seleccionada se recupera de la propiedad `CommandArgument`. A continuación, se hace referencia al control BulletedList de la plantilla y se enlaza a los resultados del método de `GetProductsByCategoryID(categoryID)` de clase `ProductsBLL`.

En los tutoriales anteriores que usaban los botones de una lista de datos, como [información general sobre la edición y eliminación de datos en la lista de datos](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), determinamos el valor de clave principal de un elemento determinado a través de la colección de `DataKeys`. Aunque este enfoque funciona bien con DataList, el repetidor no tiene una propiedad `DataKeys`. En su lugar, se debe usar un enfoque alternativo para proporcionar el valor de clave principal, por ejemplo, a través de la propiedad Button s `CommandArgument` o mediante la asignación del valor de la clave principal a un control Web de etiqueta oculto dentro de la plantilla y la lectura de su valor en el controlador de eventos `ItemCommand` mediante `e.Item.FindControl("LabelID")`.

Después de completar el controlador de eventos `ItemCommand`, dedique un momento a probar esta página en un explorador. Como se muestra en la figura 7, al hacer clic en el vínculo Mostrar productos se produce un postback y se muestran los productos de la categoría seleccionada en BulletedList. Además, tenga en cuenta que esta información del producto permanece aunque se haga clic en otras categorías mostrar los vínculos de los productos.

> [!NOTE]
> Si desea modificar el comportamiento de este informe, de modo que solo se muestren los productos de una categoría a la vez, basta con establecer la propiedad control BulletedList `EnableViewState` en `False`.

[![se usa BulletedList para mostrar los productos de la categoría seleccionada](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: se usa BulletedList para mostrar los productos de la categoría seleccionada ([haga clic para ver la imagen de tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))

## <a name="summary"></a>Resumen

Los controles DataList y Repeater pueden incluir cualquier número de botones, LinkButtons o ImageButtons dentro de sus plantillas. Estos botones, cuando se hace clic en ellos, producen un postback y generan el evento `ItemCommand`. Para asociar una acción personalizada del lado servidor a un botón en el que se hace clic, cree un controlador de eventos para el evento `ItemCommand`. En este controlador de eventos, compruebe primero el valor de `CommandName` entrante para determinar en qué botón se hizo clic. Opcionalmente, se puede proporcionar información adicional a través de la propiedad Button s `CommandArgument`.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Dennis Patterson. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](custom-buttons-in-the-datalist-and-repeater-vb.md)
