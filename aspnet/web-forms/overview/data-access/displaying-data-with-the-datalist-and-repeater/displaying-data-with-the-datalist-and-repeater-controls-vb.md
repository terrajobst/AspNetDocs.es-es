---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
title: Mostrar datos con los controles DataList y Repeater (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores se ha usado el control GridView para mostrar los datos. A partir de este tutorial, veremos la creación de patrones de informes comunes con...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 58618954-a9ed-4ca0-8c2d-95a5ffd9c03e
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 4e7aaa1701da67aec61505b64a835ef41031bb13
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614291"
---
# <a name="displaying-data-with-the-datalist-and-repeater-controls-vb"></a>Mostrar datos con los controles DataList y Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_VB.exe) de la aplicación de ejemplo o [descarga de PDF](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/datatutorial29vb1.pdf)

> En los tutoriales anteriores se ha usado el control GridView para mostrar los datos. A partir de este tutorial, veremos la creación de patrones comunes de informes con los controles DataList y Repeater, empezando por los aspectos básicos de la visualización de datos con estos controles.

## <a name="introduction"></a>Introducción

En todos los ejemplos de los últimos 28 tutoriales, si es necesario mostrar varios registros de un origen de datos, se ha desactivado el control GridView. GridView representa una fila por cada registro del origen de datos, que muestra los campos de datos de registro en columnas. Aunque GridView lo convierte en un ajuste para mostrar, paginar, ordenar, editar y eliminar datos, su aspecto es un poco boxy. Además, el marcado responsable de la estructura de GridView es fijo incluye una `<table>` HTML con una fila de tabla (`<tr>`) para cada registro y una celda de tabla (`<td>`) para cada campo.

Para proporcionar un mayor grado de personalización en el marcado de apariencia y representación al mostrar varios registros, ASP.NET 2,0 ofrece los controles DataList y Repeater (los cuales también estaban disponibles en la versión 1. x de ASP.NET). Los controles DataList y Repeater representan su contenido mediante plantillas en lugar de BoundFields, CheckBoxFields, ButtonFields, etc. Al igual que GridView, DataList se representa como una `<table>`HTML, pero permite que se muestren varios registros de origen de datos por fila de tabla. Por otro lado, el repetidor no representa ningún marcado adicional de lo que se especifica explícitamente, y es un candidato ideal cuando se necesita un control preciso sobre el marcado emitido.

En los próximos docenas de tutoriales o, veremos cómo crear patrones de informes comunes con los controles DataList y Repeater, empezando por los aspectos básicos de la visualización de datos con estas plantillas de controles. Veremos cómo dar formato a estos controles, cómo modificar el diseño de los registros de origen de datos en la lista de datos, escenarios principales y detalles comunes, formas de editar y eliminar datos, cómo paginar los registros, etc.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial de DataList y Repeater

Antes de comenzar este tutorial, vamos a dedicar primero un momento a agregar las páginas de ASP.NET que necesitamos para este tutorial y los siguientes tutoriales para mostrar los datos con los controles DataList y Repeater. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterBasics`. A continuación, agregue las siguientes cinco páginas de ASP.NET a esta carpeta, de forma que todas ellas estén configuradas para usar la página maestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`

![Cree una carpeta DataListRepeaterBasics y agregue el tutorial ASP.NET páginas](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image1.png)

**Figura 1**: creación de una carpeta de `DataListRepeaterBasics` y adición de las páginas del tutorial ASP.net

Abra la página `Default.aspx` y arrastre el control de usuario `SectionLevelTutorialListing.ascx` desde la carpeta `UserControls` hasta la superficie de diseño. Este control de usuario, que hemos creado en el tutorial [páginas maestras y navegación de sitios](../introduction/master-pages-and-site-navigation-vb.md) , enumera el mapa del sitio y muestra los tutoriales de la sección actual en una lista con viñetas.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image2.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image4.png))

Para que la lista con viñetas muestre los tutoriales DataList y Repeater que vamos a crear, es necesario agregarlos al mapa del sitio. Abra el archivo `Web.sitemap` y agregue el siguiente marcado después del marcado de nodo del mapa del sitio de agregar botones personalizados:

[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample1.xml)]

![Actualización del mapa del sitio para incluir las nuevas páginas de ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image5.png)

**Figura 3**: actualización del mapa del sitio para incluir las nuevas páginas de ASP.net

## <a name="step-2-displaying-product-information-with-the-datalist"></a>Paso 2: Mostrar información del producto con la lista de datos

Al igual que FormView, la salida representada del control DataList depende de las plantillas en lugar de BoundFields, CheckBoxFields, etc. A diferencia de FormView, DataList está diseñado para mostrar un conjunto de registros en lugar de uno solitarios. Vamos a empezar este tutorial con un vistazo a la información del producto de enlace a una lista de datos. Para empezar, abra la página `Basics.aspx` de la carpeta `DataListRepeaterBasics`. A continuación, arrastre un DataList desde el cuadro de herramientas hasta el diseñador. Como se muestra en la figura 4, antes de especificar las plantillas DataList, el diseñador la muestra como un cuadro gris.

[![arrastrar la lista de controles desde el cuadro de herramientas hasta el diseñador](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image6.png)

**Figura 4**: arrastre DataList desde el cuadro de herramientas hasta el diseñador ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image8.png))

En la etiqueta inteligente DataList s, agregue un nuevo ObjectDataSource y configúrelo para usar el método `ProductsBLL` Class s `GetProducts`. Puesto que vamos a crear una lista de elementos de solo lectura en este tutorial, establezca la lista desplegable en (ninguno) en las pestañas insertar, actualizar y eliminar del asistente.

[![optar por crear un nuevo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image9.png)

**Figura 5**: opte por crear un nuevo ObjectDataSource ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image11.png))

[![configurar ObjectDataSource para usar la clase ProductsBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image12.png)

**Figura 6**: configuración de ObjectDataSource para usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image14.png))

[![recuperar información sobre todos los productos mediante el método GetProducts](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image15.png)

**Figura 7**: recuperar información sobre todos los productos con el método `GetProducts` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image17.png))

Después de configurar ObjectDataSource y asociarlo a la lista de datos a través de su etiqueta inteligente, Visual Studio creará automáticamente una `ItemTemplate` en la lista de datos que muestra el nombre y el valor de cada campo de datos devuelto por el origen de datos (vea el marcado siguiente). Esta apariencia `ItemTemplate` s predeterminada es idéntica a la de las plantillas creadas automáticamente al enlazar un origen de datos a FormView a través del diseñador.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample2.aspx)]

> [!NOTE]
> Recuerde que al enlazar un origen de datos a un control FormView a través de la etiqueta inteligente FormView s, Visual Studio creó un `ItemTemplate`, `InsertItemTemplate`y `EditItemTemplate`. Sin embargo, con DataList, solo se crea un `ItemTemplate`. Esto se debe a que la lista de DataList no tiene la misma compatibilidad integrada de edición e inserción que ofrece FormView. DataList contiene eventos relacionados con la edición y la eliminación, y la edición y eliminación de la compatibilidad se puede Agregar con un poco de código, pero no hay ninguna compatibilidad sencilla integrada como con FormView. Veremos cómo incluir la compatibilidad con la edición y eliminación de la lista de DataList en un tutorial futuro.

Tómese un momento para mejorar la apariencia de esta plantilla. En lugar de Mostrar todos los campos de datos, deje que solo se muestre el nombre del producto, el proveedor, la categoría, la cantidad por unidad y el precio por unidad. Además, vamos a mostrar el nombre en un encabezado de `<h4>` y colocar los campos restantes con un `<table>` debajo del encabezado.

Para realizar estos cambios, puede usar las características de edición de plantillas en el diseñador desde la etiqueta inteligente de DataList s. para ello, haga clic en el vínculo editar plantillas o bien, puede modificar la plantilla manualmente mediante la sintaxis declarativa de la página s. Si usa la opción editar plantillas en el diseñador, el marcado resultante puede no coincidir exactamente con el marcado siguiente, pero cuando se ve a través de un explorador debe ser muy similar a la captura de pantalla que se muestra en la figura 8.

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample3.aspx)]

> [!NOTE]
> En el ejemplo anterior se usan controles Web de etiqueta cuya propiedad `Text` tiene asignado el valor de la sintaxis de DataBinding. Como alternativa, podríamos haber omitido las etiquetas por completo, escribiendo simplemente la sintaxis de DataBinding. Es decir, en lugar de usar `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` podríamos haber usado en su lugar la sintaxis declarativa `<%# Eval("CategoryName") %>`.

Sin embargo, la salida de los controles Web Label ofrece dos ventajas. En primer lugar, proporciona un medio más sencillo para dar formato a los datos en función de los datos, como veremos en el siguiente tutorial. En segundo lugar, la opción editar plantillas en el diseñador no muestra la sintaxis de enlace de los enlaces declarativos que aparece fuera de un control Web. En su lugar, la interfaz editar plantillas se ha diseñado para facilitar el trabajo con marcado estático y controles Web, y supone que cualquier enlace de los mismos se realizará a través del cuadro de diálogo Editar DataBindings, al que se puede acceder desde las etiquetas inteligentes de los controles Web.

Por lo tanto, al trabajar con la lista de objetos, que ofrece la opción de editar las plantillas a través del diseñador, prefiero usar controles Web de etiqueta para que el contenido sea accesible a través de la interfaz de edición de plantillas. Como veremos en breve, el repetidor requiere que el contenido de la plantilla se edite desde la vista de código fuente. Por lo tanto, al crear las plantillas de repetidor, a menudo omitiría los controles Web de etiqueta a menos que sé que necesito dar formato al texto enlazado a datos en función de la lógica de programación.

[![cada salida de producto se representa mediante el ItemTemplate de DataList.](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image18.png)

**Figura 8**: cada salida de producto se representa mediante el `ItemTemplate` de lista de elementos de trabajo ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image20.png))

## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Paso 3: mejorar la apariencia de DataList

Al igual que GridView, DataList ofrece una serie de propiedades relacionadas con el estilo, como `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, etc. Al trabajar con los controles GridView y DetailsView, creamos archivos de máscara en el `DataWebControls` tema que predefinía las propiedades `CssClass` de estos dos controles y la propiedad `CssClass` para algunas de sus subpropiedades (`RowStyle`, `HeaderStyle`, etc.). Permita hacer lo mismo para la lista de objetos.

Como se describe en el tutorial [Mostrar datos con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) , un archivo de máscara especifica las propiedades predeterminadas relacionadas con el aspecto de un control Web. un tema es una colección de archivos de máscara, CSS, de imagen y JavaScript que definen una apariencia determinada para un sitio Web. En el tutorial *Mostrar datos con ObjectDataSource* , creamos un tema `DataWebControls` (que se implementa como una carpeta dentro de la carpeta `App_Themes`) que tiene actualmente dos archivos de máscara: `GridView.skin` y `DetailsView.skin`. Permite agregar un tercer archivo de máscara para especificar la configuración de estilo predefinida para DataList.

Para agregar un archivo de máscara, haga clic con el botón derecho en la carpeta `App_Themes/DataWebControls`, elija Agregar un nuevo elemento y seleccione la opción archivo de máscara en la lista. Asigne al archivo el nombre `DataList.skin`.

[![crear un nuevo archivo de máscara denominado DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image21.png)

**Figura 9**: cree un nuevo archivo de máscara denominado `DataList.skin` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image23.png))

Use el siguiente marcado para el archivo de `DataList.skin`:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample4.aspx)]

Esta configuración asigna las mismas clases CSS a las propiedades de DataList apropiadas que se utilizaron con los controles GridView y DetailsView. Las clases CSS que se usan aquí `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, etc. se definen en el archivo de `Styles.css` y se agregaron en los tutoriales anteriores.

Con la adición de este archivo de máscara, la apariencia de DataList s se actualiza en el diseñador (puede que tenga que actualizar la vista de diseñador para ver los efectos del nuevo archivo de máscara; en el menú Ver, elija actualizar). Como se muestra en la figura 10, cada producto alterno tiene un color de fondo rosa claro.

[![crear un nuevo archivo de máscara denominado DataList. Skin](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image24.png)

**Figura 10**: cree un nuevo archivo de máscara denominado `DataList.skin` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image26.png))

## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Paso 4: exploración de las otras plantillas de DataList

Además de la `ItemTemplate`, DataList admite seis otras plantillas opcionales:

- `HeaderTemplate` si se proporciona, agrega una fila de encabezado a la salida y se usa para representar esta fila
- `AlternatingItemTemplate` usa para representar elementos alternos
- `SelectedItemTemplate` utilizada para representar el elemento seleccionado; el elemento seleccionado es el elemento cuyo índice corresponde a la propiedad DataList s [`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilizado para representar el elemento que se está editando
- `SeparatorTemplate` si se proporciona, agrega un separador entre cada elemento y se usa para representar este separador.
- `FooterTemplate`: si se proporciona, agrega una fila de pie de página a la salida y se usa para representar esta fila

Al especificar el `HeaderTemplate` o el `FooterTemplate`, el DataList agrega una fila de encabezado o de pie de página adicional a la salida representada. Al igual que con las filas de encabezado y pie de página de GridView, el encabezado y el pie de página de una lista de datos no están enlazados a datos. Por lo tanto, cualquier sintaxis de enlace de datos de la `HeaderTemplate` o `FooterTemplate` que intente obtener acceso a los datos enlazados devolverá una cadena en blanco.

> [!NOTE]
> Como vimos en el tutorial [Mostrar información de resumen en el pie de página de GridView s](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) , mientras que las filas de encabezado y pie de página no admiten la sintaxis de enlace de datos, la información específica de los datos se puede insertar directamente en estas filas desde el controlador de eventos GridView s `RowDataBound`. Esta técnica se puede utilizar para calcular los totales en ejecución u otra información de los datos enlazados al control, además de asignar esa información al pie de página. Este mismo concepto se puede aplicar a los controles DataList y Repeater; la única diferencia es que para los controles DataList y Repeater, cree un controlador de eventos para el evento `ItemDataBound` (en lugar de para el evento `RowDataBound`).

En nuestro ejemplo, vamos a hacer que la información del título del producto se muestre en la parte superior de los resultados de DataList en un encabezado `<h3>`. Para ello, agregue un `HeaderTemplate` con el marcado adecuado. En el diseñador, esto se puede hacer haciendo clic en el vínculo editar plantillas de la etiqueta inteligente de DataList s, eligiendo la plantilla de encabezado en la lista desplegable y escribiendo el texto después de elegir la opción de encabezado 3 en la lista desplegable estilo (vea la figura 11).

[![agregar HeaderTemplate con el texto información del producto](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image27.png)

**Figura 11**: agregar un `HeaderTemplate` con la información del producto de texto ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image29.png))

Como alternativa, se puede agregar mediante declaración escribiendo el siguiente marcado dentro de las etiquetas de `<asp:DataList>`:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample5.html)]

Para agregar un poco de espacio entre cada lista de productos, supongamos que agrega un `SeparatorTemplate` que incluye una línea entre cada sección. La etiqueta de regla horizontal (`<hr>`), agrega este divisor. Cree el `SeparatorTemplate` para que tenga el marcado siguiente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample6.html)]

> [!NOTE]
> Al igual que los `HeaderTemplate` y `FooterTemplates`, el `SeparatorTemplate` no está enlazado a ningún registro del origen de datos y, por tanto, no puede acceder directamente a los registros del origen de datos enlazados a DataList.

Después de hacerlo, al ver la página a través de un explorador, debería ser similar a la ilustración 12. Observe la fila de encabezado y la línea que hay entre cada lista de productos.

[![la lista de DataList incluye una fila de encabezado y una regla horizontal entre cada lista de productos](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image30.png)

**Figura 12**: el DataList incluye una fila de encabezado y una regla horizontal entre cada lista de productos ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image32.png))

## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Paso 5: representar el marcado específico con el control Repeater

Si realiza una vista o código fuente desde el explorador al visitar el ejemplo de DataList en la figura 12, verá que el objeto DataList emite una `<table>` HTML que contiene una fila de tabla (`<tr>`) con una sola celda de tabla (`<td>`) para cada elemento enlazado a DataList. De hecho, esta salida es idéntica a la que se emitiría desde un control GridView con una única TemplateField. Como veremos en un tutorial futuro, el DataList permite personalizar aún más la salida, lo que nos permite mostrar varios registros de origen de datos por fila de tabla.

No obstante, ¿qué ocurre si no desea emitir una `<table>`HTML? Para obtener un control total y completo sobre el marcado generado por un control Web de datos, se debe usar el control Repeater. Al igual que en DataList, el repetidor se construye en función de las plantillas. El repetidor, sin embargo, solo ofrece las cinco plantillas siguientes:

- `HeaderTemplate` si se proporciona, agrega el marcado especificado antes que los elementos.
- `ItemTemplate` utilizado para representar elementos
- `AlternatingItemTemplate` si se proporciona, se usa para representar elementos alternos
- `SeparatorTemplate` si se proporciona, agrega el marcado especificado entre cada elemento.
- `FooterTemplate`: si se proporciona, agrega el marcado especificado después de los elementos.

En ASP.NET 1. x, el control Repeater se usaba normalmente para mostrar una lista con viñetas cuyos datos procedían de algún origen de datos. En tal caso, el `HeaderTemplate` y el `FooterTemplates` contendrían las etiquetas `<ul>` de apertura y cierre, respectivamente, mientras que el `ItemTemplate` incluiría elementos `<li>` con la sintaxis de DataBinding. Este enfoque puede seguir utilizándose en ASP.NET 2,0 como vimos en dos ejemplos en el tutorial de navegación por el [sitio y las páginas maestras](../introduction/master-pages-and-site-navigation-vb.md) :

- En la página maestra de `Site.master`, se usó un repetidor para mostrar una lista con viñetas del contenido del mapa del sitio de nivel superior (informes básicos, filtrado de informes, formato personalizado, etc.); otro Repeater anidado se usaba para mostrar las secciones secundarias de las secciones de nivel superior.
- En `SectionLevelTutorialListing.ascx`, se usó un repetidor para mostrar una lista con viñetas de las secciones secundarias de la sección del mapa del sitio actual.

> [!NOTE]
> ASP.NET 2,0 presenta el nuevo [control BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), que se puede enlazar a un control de origen de datos para mostrar una lista con viñetas simple. Con el control BulletedList, no es necesario especificar ninguno de los HTML relacionados con la lista; en su lugar, simplemente se indica el campo de datos que se va a mostrar como texto de cada elemento de la lista.

El repetidor actúa como un control Web de detección de todos los datos. Si no hay un control existente que genere el marcado necesario, se puede usar el control Repeater. Para ilustrar el uso del repetidor, permita que la lista de categorías se muestre por encima de la lista de datos de información del producto creada en el paso 2. En concreto, permita que las categorías se muestren en un `<table>` HTML de una sola fila con cada categoría mostrada como columna en la tabla.

Para ello, empiece arrastrando un control Repeater desde el cuadro de herramientas hasta el diseñador, encima de la lista de datos de información del producto. Al igual que en DataList, el repetidor se muestra inicialmente como un cuadro gris hasta que se hayan definido sus plantillas.

[![agregar un repetidor al diseñador](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image33.png)

**Figura 13**: agregar un repetidor al diseñador ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image35.png))

Solo hay una opción en la etiqueta inteligente Repeater s: elegir origen de datos. Opte por crear un nuevo ObjectDataSource y configurarlo para que use el método de `GetCategories` de `CategoriesBLL` Class s.

[![crear un nuevo ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image36.png)

**Figura 14**: creación de un nuevo ObjectDataSource ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image38.png))

[![configurar ObjectDataSource para usar la clase CategoriesBLL](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image39.png)

**Figura 15**: configuración de ObjectDataSource para usar la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image41.png))

[![recuperar información sobre todas las categorías mediante el método GetCategories](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image42.png)

**Figura 16**: recupere información acerca de todas las categorías mediante el método `GetCategories` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image44.png))

A diferencia de DataList, Visual Studio no crea automáticamente un ItemTemplate para el repetidor después de enlazarlo a un origen de datos. Además, las plantillas de repetidor no se pueden configurar a través del diseñador y se deben especificar mediante declaración.

Para mostrar las categorías como un `<table>` de una sola fila con una columna para cada categoría, necesitamos que el repetidor emita un marcado similar al siguiente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample7.html)]

Puesto que el `<td>Category X</td>` texto es la parte que se repite, aparecerá en el ItemTemplate del repetidor. El marcado que aparece antes de `<table><tr>`: se colocará en el `HeaderTemplate` mientras que el `</tr></table>` de marcado de cierre se colocará en el `FooterTemplate`. Para especificar esta configuración de plantilla, vaya a la parte declarativa de la página ASP.NET haciendo clic en el botón origen en la esquina inferior izquierda y escriba la siguiente sintaxis:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample8.aspx)]

El repetidor emite el marcado preciso según lo especificado por sus plantillas, nada más, nada menos. En la figura 17 se muestra el resultado repetidor cuando se ve a través de un explorador.

[![una tabla de &lt;HTML de fila única&gt; enumera cada categoría en una columna independiente](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image45.png)

**Figura 17**: una `<table>` HTML de una sola fila muestra cada categoría en una columna independiente ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image47.png))

## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Paso 6: mejorar la apariencia del repetidor

Dado que el repetidor emite precisamente el marcado especificado por sus plantillas, no debería sorprender que no haya propiedades relacionadas con el estilo para el repetidor. Para modificar la apariencia del contenido generado por el repetidor, debemos agregar manualmente el contenido HTML o CSS necesario directamente a las plantillas de Repeater.

En nuestro ejemplo, vamos a que las columnas de categoría tengan colores de fondo alternativos, como con las filas alternas de la lista de objetos. Para ello, es necesario asignar el `RowStyle` clase CSS a cada elemento repetidor y el `AlternatingRowStyle` clase CSS a cada elemento repetidor alterno a través de las plantillas `ItemTemplate` y `AlternatingItemTemplate`, como por ejemplo:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample9.aspx)]

Vamos a agregar también una fila de encabezado a la salida con las categorías de producto de texto. Dado que no sabemos cuántas columnas se incluirán en el `<table>` resultante, la manera más sencilla de generar una fila de encabezado que se garantiza que abarque todas las columnas es usar *dos* `<table>` s. La primera `<table>` contendrá dos filas en la fila de encabezado y una fila que contendrán el segundo `<table>` de fila única que tiene una columna para cada categoría del sistema. Es decir, queremos emitir el marcado siguiente:

[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample10.html)]

El siguiente `HeaderTemplate` y `FooterTemplate` resultado en el marcado deseado:

[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-vb/samples/sample11.aspx)]

La figura 18 muestra el repetidor una vez realizados estos cambios.

[![las columnas de categoría alternativas en el color de fondo e incluye una fila de encabezado](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image48.png)

**Figura 18**: las columnas de categorías alternativas en el color de fondo e incluye una fila de encabezado ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-datalist-and-repeater-controls-vb/_static/image50.png))

## <a name="summary"></a>Resumen

Aunque el control GridView facilita la visualización, edición, eliminación, ordenación y paginación de los datos, la apariencia es muy boxy y similar a la cuadrícula. Para tener un mayor control sobre la apariencia, es necesario activar los controles DataList o Repeater. Ambos controles muestran un conjunto de registros mediante plantillas en lugar de BoundFields, CheckBoxFields, etc.

DataList se representa como una `<table>` HTML que, de forma predeterminada, muestra cada registro de origen de datos en una sola fila de la tabla, al igual que un control GridView con una única TemplateField. Como veremos en un tutorial futuro, sin embargo, la lista de archivos permite que se muestren varios registros por fila de tabla. El repetidor, por otro lado, emite de forma estricta el marcado especificado en sus plantillas. no agrega ningún marcado adicional y, por tanto, se utiliza normalmente para Mostrar datos en elementos HTML distintos de un `<table>` (por ejemplo, en una lista con viñetas).

Aunque los controles DataList y Repeater ofrecen más flexibilidad en su salida representada, carecen de muchas de las características integradas que se encuentran en GridView. Como examinaremos en próximos tutoriales, algunas de estas características se pueden conectar de nuevo sin demasiado esfuerzo, pero tenga en cuenta que el uso de DataList o Repeater en lugar de GridView limita las características que puede usar sin tener que implementar esas características. personal.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Yaakov Ellis, Liz Shulok, Randy Schmidt y Stacy Park. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](nested-data-web-controls-cs.md)
> [Siguiente](formatting-the-datalist-and-repeater-based-upon-data-vb.md)
