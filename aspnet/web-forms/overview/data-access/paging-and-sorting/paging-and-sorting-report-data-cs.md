---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Paginación y ordenación de datosC#de informe () | Microsoft Docs
author: rick-anderson
description: La paginación y la ordenación son dos características muy comunes al mostrar los datos en una aplicación en línea. En este tutorial, echaremos un vistazo primero a agregar ordenación y...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f77040316dadc218b8183e52628dc0cfe3b35a1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587009"
---
# <a name="paging-and-sorting-report-data-c"></a>Paginar y ordenar datos de informes (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) de la aplicación de ejemplo o [descarga de PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> La paginación y la ordenación son dos características muy comunes al mostrar los datos en una aplicación en línea. En este tutorial, veremos un primer vistazo a la adición de ordenación y paginación a los informes, que posteriormente se basarán en futuros tutoriales.

## <a name="introduction"></a>Introducción

La paginación y la ordenación son dos características muy comunes al mostrar los datos en una aplicación en línea. Por ejemplo, al buscar libros de ASP.NET en una librería en línea, puede haber cientos de estos libros, pero el informe en el que se muestran los resultados de la búsqueda solo muestra diez coincidencias por página. Además, los resultados se pueden ordenar por título, precio, número de páginas, nombre de autor, etc. Mientras que los últimos 23 tutoriales han examinado cómo crear una variedad de informes, incluidas las interfaces que permiten agregar, editar y eliminar datos, no veremos cómo ordenar los datos y los únicos ejemplos de paginación que se han visto han sido con DetailsView y FormView permite.

En este tutorial, veremos cómo agregar ordenación y paginación a los informes, lo que se puede lograr con solo marcar algunas casillas. Desafortunadamente, esta implementación simplista tiene sus inconvenientes. la interfaz de ordenación deja un poco para ser deseada y las rutinas de paginación no están diseñadas para la paginación eficaz a través de grandes conjuntos de resultados. En los tutoriales futuros se explorará cómo superar las limitaciones de las soluciones de paginación y de ordenación preparadas.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial de paginación y ordenación

Antes de comenzar este tutorial, vamos a dedicar primero un momento a agregar las páginas de ASP.NET que necesitamos para este tutorial y las tres siguientes. Empiece por crear una nueva carpeta en el proyecto denominado `PagingAndSorting`. A continuación, agregue las siguientes cinco páginas de ASP.NET a esta carpeta, de forma que todas ellas estén configuradas para usar la página maestra `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Cree una carpeta PagingAndSorting y agregue el tutorial ASP.NET páginas](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: creación de una carpeta PagingAndSorting y adición del tutorial ASP.net páginas

A continuación, abra la página `Default.aspx` y arrastre el control de usuario `SectionLevelTutorialListing.ascx` desde la carpeta `UserControls` hasta la superficie de diseño. Este control de usuario, que hemos creado en el tutorial [páginas maestras y navegación de sitios](../introduction/master-pages-and-site-navigation-cs.md) , enumera el mapa del sitio y muestra los tutoriales en la sección actual de una lista con viñetas.

![Agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx

Para que la lista con viñetas muestre los tutoriales de paginación y ordenación que vamos a crear, es necesario agregarlos al mapa del sitio. Abra el archivo `Web.sitemap` y agregue el siguiente marcado después de la edición, inserción y eliminación del marcado del nodo del mapa del sitio:

[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]

![Actualización del mapa del sitio para incluir las nuevas páginas de ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: actualización del mapa del sitio para incluir las nuevas páginas de ASP.net

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Paso 2: Mostrar información del producto en un control GridView

Antes de implementar realmente funciones de paginación y ordenación, vamos a crear primero un GridView estándar no ordenable y no paginable que muestra la información del producto. Esta es una tarea que se realiza muchas veces antes de esta serie de tutoriales, por lo que estos pasos deben ser familiares. Para empezar, abra la página `SimplePagingSorting.aspx` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador y establezca su propiedad `ID` en `Products`. A continuación, cree un nuevo ObjectDataSource que use el método `GetProducts()` de la clase ProductsBLL para devolver toda la información del producto.

![Recuperar información sobre todos los productos mediante el método GetProducts ()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperación de información sobre todos los productos mediante el método GetProducts ()

Puesto que este informe es un informe de solo lectura, no es necesario asignar los métodos de `Insert()`, `Update()`o `Delete()` de ObjectDataSource a los métodos de `ProductsBLL` correspondientes. por lo tanto, elija (ninguno) en la lista desplegable de las pestañas actualizar, insertar y eliminar.

![Elija la opción (ninguno) en la lista desplegable de las pestañas actualizar, insertar y eliminar.](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: elija la opción (ninguno) de la lista desplegable de las pestañas actualizar, insertar y eliminar.

A continuación, permita personalizar los campos de GridView para que solo se muestren los nombres de los productos, los proveedores, las categorías, los precios y los Estados descontinuos. Además, no dude en realizar cualquier cambio de formato de nivel de campo, como el ajuste de las propiedades del `HeaderText` o el formato de la moneda. Después de estos cambios, el marcado declarativo de GridView s debe ser similar al siguiente:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

En la ilustración 6 se muestra nuestro progreso hasta ahora cuando se ve a través de un explorador. Tenga en cuenta que en la página se enumeran todos los productos de una pantalla, mostrando el nombre de cada producto, la categoría, el proveedor, el precio y el estado descontinuado.

[![se muestra cada uno de los productos](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: cada uno de los productos se muestra ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Paso 3: agregar compatibilidad con la paginación

Enumerar *todos* los productos en una pantalla puede conducir a la sobrecarga de información para el usuario que usa los datos. Para ayudar a que los resultados sean más fáciles de administrar, podemos dividir los datos en páginas de datos más pequeñas y permitir que el usuario desplazarse por los datos una página a la vez. Para ello, active la casilla habilitar paginación de la etiqueta inteligente GridView s (Esto establece la propiedad GridView s [`AllowPaging`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) en `true`).

[![Active la casilla habilitar paginación para agregar compatibilidad con paginación](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: Active la casilla habilitar paginación para agregar compatibilidad con la paginación ([haga clic para ver la imagen a tamaño completo](paging-and-sorting-report-data-cs/_static/image11.png))

Al habilitar la paginación, se limita el número de registros que se muestran por página y se agrega una *interfaz de paginación* a GridView. La interfaz de paginación predeterminada, que se muestra en la figura 7, es una serie de números de página, lo que permite al usuario desplazarse rápidamente de una página de datos a otra. Esta interfaz de paginación debe ser familiar, ya que se ve al agregar compatibilidad de paginación a los controles DetailsView y FormView en los tutoriales anteriores.

Los controles DetailsView y FormView solo muestran un único registro por página. Sin embargo, GridView consulta su [propiedad`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar el número de registros que se mostrarán por página (el valor predeterminado de esta propiedad es 10).

Esta interfaz de paginación GridView, DetailsView y FormView se puede personalizar con las siguientes propiedades:

- `PagerStyle` indica la información de estilo para la interfaz de paginación; puede especificar valores como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, etc.
- `PagerSettings` contiene un serie de propiedades que pueden personalizar la funcionalidad de la interfaz de paginación; `PageButtonCount` indica el número máximo de números de página numéricos mostrados en la interfaz de paginación (el valor predeterminado es 10); la [propiedad`Mode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica cómo funciona la interfaz de paginación y se puede establecer en: 

    - `NextPrevious` muestra los botones siguiente y anterior, lo que permite al usuario avanzar o retroceder una página a la vez
    - `NextPreviousFirstLast` además de los botones siguiente y anterior, también se incluyen los botones primero y último, lo que permite al usuario desplazarse rápidamente a la primera o última página de datos.
    - `Numeric` muestra una serie de números de página, lo que permite al usuario saltar inmediatamente a cualquier página
    - `NumericFirstLast` además de los números de página, incluye los botones primero y último, lo que permite al usuario desplazarse rápidamente a la primera o última página de datos; los primeros y últimos botones solo se muestran si no caben todos los números de página numéricos

Además, GridView, DetailsView y FormView ofrecen las propiedades `PageIndex` y `PageCount`, que indican la página actual que se está viendo y el número total de páginas de datos, respectivamente. La propiedad `PageIndex` se indiza a partir de 0, lo que significa que al ver la primera página de datos `PageIndex` será igual a 0. `PageCount`, por otro lado, comienza el recuento en 1, lo que significa que `PageIndex` está limitado a los valores entre 0 y `PageCount - 1`.

Tómese un momento para mejorar la apariencia predeterminada de la interfaz de paginación de GridView s. En concreto, deje la interfaz de paginación alineada a la derecha con un fondo gris claro. En lugar de establecer estas propiedades directamente a través de la propiedad GridView s `PagerStyle`, cree una clase CSS en `Styles.css` denominada `PagerRowStyle` y, a continuación, asigne la propiedad `PagerStyle` s `CssClass` a través de nuestro tema. Comience abriendo `Styles.css` y agregando la siguiente definición de clase CSS:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

A continuación, abra el archivo de `GridView.skin` en la carpeta `DataWebControls` en la carpeta `App_Themes`. Como se explicó en el tutorial *páginas maestras y navegación de sitios* , los archivos de máscara se pueden usar para especificar los valores de propiedad predeterminados de un control Web. Por lo tanto, aumente la configuración existente para incluir el establecimiento de la propiedad `PagerStyle` s `CssClass` en `PagerRowStyle`. Además, vamos a configurar la interfaz de paginación para mostrar como máximo cinco botones de página numéricos mediante la interfaz de paginación de `NumericFirstLast`.

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>La experiencia del usuario de paginación

En la figura 8 se muestra la página web cuando se visita a través de un explorador después de que se haya activado la casilla de verificación de la paginación de GridView s habilitada y se hayan realizado las configuraciones de `PagerStyle` y `PagerSettings` a través del archivo de `GridView.skin`. Observe que solo se muestran diez registros y la interfaz de paginación indica que estamos viendo la primera página de datos.

[![con la paginación habilitada, solo se muestra un subconjunto de los registros a la vez.](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: con la paginación habilitada, solo se muestra un subconjunto de los registros a la vez ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image14.png))

Cuando el usuario hace clic en uno de los números de página de la interfaz de paginación, se muestra un postback y la página se recarga mostrando los registros de la página solicitada. En la ilustración 9 se muestran los resultados tras optar por ver la página final de los datos. Tenga en cuenta que la página final solo tiene un registro; Esto se debe a que hay 81 registros en total, lo que da lugar a ocho páginas de 10 registros por página más una página con un registro único.

[![hacer clic en un número de página produce un postback y muestra el subconjunto de registros adecuado](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: al hacer clic en un número de página, se genera un postback y se muestra el subconjunto de registros adecuado ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Paginación del flujo de trabajo del lado servidor

Cuando el usuario final hace clic en un botón de la interfaz de paginación, se inicia un postback y comienza el siguiente flujo de trabajo del servidor:

1. Se desencadena el evento de `PageIndexChanging` GridView s (o DetailsView o FormView)
2. ObjectDataSource vuelve a solicitar *todos* los datos de la capa BLL; los valores de las propiedades `PageIndex` y `PageSize` de GridView se usan para determinar qué registros devueltos por la capa BLL deben mostrarse en GridView
3. El evento GridView s `PageIndexChanged` se activa

En el paso 2, ObjectDataSource vuelve a solicitar todos los datos de su origen de datos. Este estilo de paginación se conoce comúnmente como *paginación predeterminada*, ya que es el comportamiento de paginación que se utiliza de forma predeterminada al establecer la propiedad `AllowPaging` en `true`. Con la paginación predeterminada, el control Web de datos recupera todos los registros de cada página de datos, aunque en realidad solo un subconjunto de registros se representa en el HTML que se envía al explorador. A menos que el BLL o ObjectDataSource almacenen en memoria caché los datos de la base de datos, la paginación predeterminada no funciona para conjuntos de resultados suficientemente grandes o para aplicaciones web con muchos usuarios simultáneos.

En el siguiente tutorial, veremos cómo implementar la *paginación personalizada*. Con la paginación personalizada, puede indicar específicamente a ObjectDataSource que solo recupere el conjunto preciso de registros necesarios para la página de datos solicitada. Como puede imaginar, la paginación personalizada mejora en gran medida la eficacia de la paginación a través de grandes conjuntos de resultados.

> [!NOTE]
> Aunque la paginación predeterminada no es adecuada para la paginación a través de conjuntos de resultados suficientemente grandes o para sitios con muchos usuarios simultáneos, tenga en cuentan que la paginación personalizada requiere más cambios y esfuerzo para implementar y no es tan sencillo como activar una casilla (como es el valor predeterminado). paginación). Por lo tanto, la paginación predeterminada puede ser la opción ideal para sitios web pequeños de bajo tráfico o al paginar a través de conjuntos de resultados relativamente pequeños, ya que es mucho más fácil y rápido de implementar.

Por ejemplo, si sabemos que nunca tendremos más de 100 productos en nuestra base de datos, la ganancia de rendimiento mínima que disfrutará de la paginación personalizada es probable que esté desplazada por el esfuerzo necesario para implementarlo. Sin embargo, si es posible que un día tenga miles o decenas de miles de productos, *no* implementar la paginación personalizada dificultaría enormemente la escalabilidad de nuestra aplicación.

## <a name="step-4-customizing-the-paging-experience"></a>Paso 4: personalización de la experiencia de paginación

Los controles Web de datos proporcionan una serie de propiedades que se pueden utilizar para mejorar la experiencia de paginación del usuario. Por ejemplo, la propiedad `PageCount` indica el número total de páginas que hay, mientras que la propiedad `PageIndex` indica la página actual que se está visitando y se puede establecer para que mueva rápidamente un usuario a una página específica. Para ilustrar el uso de estas propiedades para mejorar la experiencia de paginación de los usuarios, es posible agregar un control Web de etiqueta a la página que informa al usuario de la página que visitan actualmente, junto con un control DropDownList que les permite saltar rápidamente a una página determinada. .

En primer lugar, agregue un control Web de etiqueta a la página, establezca su propiedad `ID` en `PagingInformation`y borre su propiedad `Text`. A continuación, cree un controlador de eventos para el evento GridView s `DataBound` y agregue el código siguiente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Este controlador de eventos asigna la propiedad `PagingInformation` etiqueta s `Text` a un mensaje que informa al usuario de la página que está visitando actualmente `Products.PageIndex + 1` el número total de páginas `Products.PageCount` (agregamos 1 a la propiedad `Products.PageIndex` porque `PageIndex` se indiza a partir de 0). Elijo la propiedad asignar esta etiqueta s `Text` en el controlador de eventos de `DataBound` en lugar del controlador de eventos de `PageIndexChanged` porque el evento `DataBound` se activa cada vez que se enlazan datos a GridView, mientras que el controlador de eventos `PageIndexChanged` solo se activa cuando se cambia el índice de la página. Cuando GridView está inicialmente enlazado a datos en la primera página, el evento `PageIndexChanging` no se activa (mientras que el evento `DataBound` sí lo hace).

Con esta adición, ahora se muestra al usuario un mensaje que indica la página que están visitando y el número total de páginas de datos que contiene.

[![se muestran el número de página actual y el número total de páginas](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: se muestra el número de página actual y el número total de páginas ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image20.png))

Además del control etiqueta, permita también agregar un control DropDownList que Enumere los números de página de GridView con la página que se está viendo seleccionada. La idea es que el usuario puede pasar rápidamente de la página actual a otra simplemente seleccionando el nuevo índice de página en el DropDownList. Para empezar, agregue un control DropDownList al diseñador y establezca su propiedad `ID` en `PageList` y active la opción Habilitar AutoPostBack en la etiqueta inteligente.

A continuación, vuelva al controlador de eventos `DataBound` y agregue el código siguiente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Este código comienza por borrar los elementos del `PageList` DropDownList. Esto puede parecer superfluo, ya que no cabría esperar que el número de páginas cambiara, pero otros usuarios pueden usar el sistema simultáneamente, agregando o quitando registros de la tabla de `Products`. Tales inserciones o eliminaciones pueden modificar el número de páginas de datos.

A continuación, es necesario volver a crear los números de página y hacer que el que se asigna al GridView actual `PageIndex` seleccionado de forma predeterminada. Esto se logra con un bucle de 0 a `PageCount - 1`, agregando un nuevo `ListItem` en cada iteración y estableciendo su propiedad `Selected` en true si el índice de iteración actual es igual a la propiedad de `PageIndex` de GridView.

Por último, es necesario crear un controlador de eventos para el evento DropDownList s `SelectedIndexChanged`, que se activa cada vez que el usuario elige un elemento diferente de la lista. Para crear este controlador de eventos, simplemente haga doble clic en el DropDownList en el diseñador y, a continuación, agregue el código siguiente:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Como se muestra en la figura 11, si solo se cambia la propiedad GridView s `PageIndex`, los datos se reenlazan a GridView. En el controlador de eventos GridView s `DataBound`, se selecciona el `ListItem` de DropDownList adecuado.

[![el usuario se lleva automáticamente a la sexta página al seleccionar el elemento de lista desplegable página 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: el usuario se coloca automáticamente en la sexta página al seleccionar el elemento de lista desplegable página 6 ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image23.png))

## <a name="step-5-adding-bi-directional-sorting-support"></a>Paso 5: agregar compatibilidad con la ordenación bidireccional

Agregar compatibilidad de ordenación bidireccional es tan sencillo como agregar compatibilidad con la paginación simplemente active la opción Habilitar ordenación de la etiqueta inteligente de GridView s (que establece la propiedad GridView s [`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) en `true`). Esto representa cada uno de los encabezados de los campos de GridView como LinkButtons que, cuando se hace clic en él, provocan un postback y devuelven los datos ordenados por la columna en la que se hace clic en orden ascendente. Al hacer clic en el mismo encabezado, el LinkButton vuelve a ordenar los datos en orden descendente.

> [!NOTE]
> Si usa una capa de acceso a datos personalizada en lugar de un conjunto de datos con tipo, es posible que no tenga una opción para habilitar la ordenación en la etiqueta inteligente de GridView s. Solo los GridView enlazados a los orígenes de datos que admiten la ordenación de forma nativa tienen esta casilla disponible. El DataSet con tipo proporciona compatibilidad de ordenación integrada, ya que ADO.NET DataTable proporciona un método de `Sort` que, cuando se invoca, ordena las filas de objetos DataTable con los criterios especificados.

Si la capa DAL no devuelve objetos que admitan la ordenación de forma nativa, deberá configurar ObjectDataSource para pasar información de ordenación a la capa de lógica de negocios, que puede ordenar los datos o hacer que los datos estén ordenados por la capa DAL. Exploraremos cómo ordenar los datos en las capas de lógica empresarial y de acceso a los datos en un tutorial futuro.

Los LinkButtonss de ordenación se representan como hipervínculos HTML, cuyos colores actuales (azul para un vínculo no visitado y un rojo oscuro para un vínculo visitado) entran en conflicto con el color de fondo de la fila de encabezado. En su lugar, permita que se muestren todos los vínculos de fila de encabezado en blanco, independientemente de si se han visitado o no. Esto puede hacerse agregando lo siguiente a la clase `Styles.css`:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Esta sintaxis indica el uso de texto en blanco al mostrar los hipervínculos dentro de un elemento que utiliza la clase HeaderStyle.

Después de esta adición de CSS, al visitar la página a través de un explorador, la pantalla debe tener un aspecto similar al de la figura 12. En concreto, en la figura 12 se muestran los resultados después de haber clic en el vínculo del encabezado s del campo de precio.

[![los resultados se han ordenado en orden ascendente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: los resultados se han ordenado por el PrecioUnitario en orden ascendente ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Examinar el flujo de trabajo de ordenación

Todos los campos de GridView, CheckBoxField, TemplateField, etc., tienen una propiedad `SortExpression` que indica la expresión que se debe usar para ordenar los datos cuando se hace clic en el vínculo del encabezado de ordenación de ese campo. GridView también tiene una propiedad `SortExpression`. Cuando se hace clic en un control LinkButton de encabezado de ordenación, GridView asigna ese campo `SortExpression` valor a su propiedad `SortExpression`. A continuación, los datos se recuperan de la ObjectDataSource y se ordenan de acuerdo con la propiedad `SortExpression` de GridView. La lista siguiente detalla la secuencia de pasos que se da cuando un usuario final ordena los datos en un control GridView:

1. Se desencadena el [evento de ordenación](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) de GridView
2. La [propiedad`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) de GridView está establecida en el `SortExpression` del campo cuyo encabezado de ordenación se hizo clic en LinkButton
3. ObjectDataSource vuelve a recuperar todos los datos de la capa BLL y, a continuación, ordena los datos mediante el `SortExpression` de GridView.
4. La propiedad GridView s `PageIndex` se restablece en 0, lo que significa que al ordenar el usuario se devuelve a la primera página de datos (suponiendo que se ha implementado la compatibilidad con la paginación)
5. El evento GridView s [`Sorted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) se activa

Al igual que con la paginación predeterminada, la opción de ordenación predeterminada vuelve a recuperar *todos* los registros de la capa BLL. Cuando se usa la ordenación sin paginación o cuando se usa la ordenación con paginación predeterminada, no hay ninguna manera de evitar este descenso de rendimiento (breve de almacenar en caché los datos de la base de datos). Sin embargo, como veremos en un futuro tutorial, es posible ordenar los datos de forma eficaz cuando se usa la paginación personalizada.

Al enlazar un ObjectDataSource a GridView a través de la lista desplegable de la etiqueta inteligente de GridView s, cada campo de GridView tiene automáticamente su `SortExpression` propiedad asignada al nombre del campo de datos en la clase `ProductsRow`. Por ejemplo, la `ProductName` BoundField s `SortExpression` está establecida en `ProductName`, como se muestra en el marcado declarativo siguiente:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Un campo se puede configurar para que no se pueda ordenar desactivando su `SortExpression` propiedad (asignándole un valor de cadena vacía). Para ilustrar esto, Imagine que no queremos dejar que nuestros clientes ordenen nuestros productos por precio. La propiedad `UnitPrice` BoundField s `SortExpression` se puede quitar del marcado declarativo o a través del cuadro de diálogo campos (al que se tiene acceso al hacer clic en el vínculo editar columnas de la etiqueta inteligente GridView s).

![Los resultados se han ordenado por el UnitPrice en orden ascendente.](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: los resultados se han ordenado por el UnitPrice en orden ascendente

Una vez que se ha quitado la propiedad `SortExpression` de la `UnitPrice` BoundField, el encabezado se representa como texto en lugar de como un vínculo, lo que impide que los usuarios ordenen los datos por precio.

[![quitando la propiedad SortExpression, los usuarios ya no pueden ordenar los productos por precio](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: al quitar la propiedad SortExpression, los usuarios ya no pueden ordenar los productos por precio ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image30.png))

## <a name="programmatically-sorting-the-gridview"></a>Ordenar mediante programación el control GridView

También puede ordenar el contenido de GridView mediante programación con el [método`Sort`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx)de GridView. Solo tiene que pasar el valor de `SortExpression` para ordenar por junto con el [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`) y se volverán a ordenar los datos de GridView.

Imagine que la razón por la que hemos desactivado la ordenación por el `UnitPrice` fue porque nos preocupa que nuestros clientes simplemente compren solo los productos con el precio más bajo. Sin embargo, queremos alentarles a que compren los productos más caros, por lo que es posible que podamos ordenar los productos por precio, pero solo del precio más caro al menos.

Para ello, agregue un control Web de botón a la página, establezca su propiedad `ID` en `SortPriceDescending`y su propiedad `Text` para ordenar por precio. A continuación, cree un controlador de eventos para el evento Button s `Click`; para ello, haga doble clic en el control Button en el diseñador. Agregue el código siguiente a este controlador de eventos:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Al hacer clic en este botón, se devuelve al usuario a la primera página con los productos ordenados por precio, de más caros a menos costosos (vea la figura 15).

[![hacer clic en el botón ordena los productos de más caros al menos](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: hacer clic en el botón ordena los productos de más caros al menos ([haga clic para ver la imagen de tamaño completo](paging-and-sorting-report-data-cs/_static/image33.png))

## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo implementar funciones de paginación y ordenación predeterminadas, las cuales eran tan sencillas como activar una casilla. Cuando un usuario ordena las páginas o a través de los datos, se despliega un flujo de trabajo similar:

1. Un postback se deriva
2. Desencadenador de eventos de nivel previo de control Web de datos (`PageIndexChanging` o `Sorting`)
3. El origen de datos recupera todos los datos.
4. Desencadenador de eventos de nivel posterior del control Web de datos (`PageIndexChanged` o `Sorted`)

Aunque la implementación de la paginación y ordenación básica es una tarea muy sencilla, se debe ejercer más esfuerzo para usar la paginación personalizada más eficaz o para mejorar aún más la paginación o la interfaz de ordenación. En los tutoriales futuros se explorarán estos temas.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Siguiente](efficiently-paging-through-large-amounts-of-data-cs.md)
