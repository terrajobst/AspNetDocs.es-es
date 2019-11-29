---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paginación de datos de informe en un control DataList o Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Aunque ni los controles DataList ni Repeater ofrecen compatibilidad con la paginación o la ordenación automática, en este tutorial se muestra cómo agregar compatibilidad de paginación a las,... DataList o Repeater.
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c65ca1f263e41748d99323dbdf1c28fdd077246
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74570059"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Paginar datos de informe en un control DataList o Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) de la aplicación de ejemplo o [descarga de PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Aunque ni los controles DataList ni Repeater ofrecen compatibilidad con la paginación o la ordenación automática, en este tutorial se muestra cómo agregar compatibilidad de paginación a DataList o Repeater, lo que permite interfaces de visualización de datos y paginación mucho más flexibles.

## <a name="introduction"></a>Introducción

La paginación y la ordenación son dos características muy comunes al mostrar los datos en una aplicación en línea. Por ejemplo, al buscar libros de ASP.NET en una librería en línea, puede haber cientos de estos libros, pero el informe en el que se muestran los resultados de la búsqueda solo muestra diez coincidencias por página. Además, los resultados se pueden ordenar por título, precio, número de páginas, nombre de autor, etc. Como se explicó en el tutorial [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , los controles GridView, DetailsView y FormView proporcionan compatibilidad con la paginación integrada que se puede habilitar en el momento de una casilla. GridView también incluye compatibilidad con la ordenación.

Desafortunadamente, ni la lista de controles ni el repetidor ofrecen compatibilidad con la paginación o la ordenación automática. En este tutorial, veremos cómo agregar compatibilidad de paginación a los controles DataList o Repeater. Debemos crear manualmente la interfaz de paginación, Mostrar la página de registros adecuada y recordar la página que se está visitando en los postbacks. Aunque esto tarda más tiempo y código que con GridView, DetailsView o FormView, los controles DataList y Repeater permiten interfaces de visualización de datos y paginación mucho más flexibles.

> [!NOTE]
> Este tutorial se centra exclusivamente en la paginación. En el siguiente tutorial, nos centraremos en la incorporación de funcionalidades de ordenación.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: agregar las páginas web del tutorial de paginación y ordenación

Antes de comenzar este tutorial, vamos a dedicar primero un momento a agregar las páginas de ASP.NET que necesitamos para este tutorial y el siguiente. Empiece por crear una nueva carpeta en el proyecto denominado `PagingSortingDataListRepeater`. A continuación, agregue las siguientes cinco páginas de ASP.NET a esta carpeta, de forma que todas ellas estén configuradas para usar la página maestra `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Cree una carpeta PagingSortingDataListRepeater y agregue el tutorial ASP.NET páginas](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: creación de una carpeta de `PagingSortingDataListRepeater` y adición de las páginas del tutorial ASP.net

A continuación, abra la página `Default.aspx` y arrastre el control de usuario `SectionLevelTutorialListing.ascx` desde la carpeta `UserControls` hasta la superficie de diseño. Este control de usuario, que hemos creado en el tutorial [páginas maestras y navegación de sitios](../introduction/master-pages-and-site-navigation-vb.md) , enumera el mapa del sitio y muestra los tutoriales en la sección actual de una lista con viñetas.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))

Para que la lista con viñetas muestre los tutoriales de paginación y ordenación que vamos a crear, es necesario agregarlos al mapa del sitio. Abra el archivo `Web.sitemap` y agregue el siguiente marcado después de la edición y eliminación con el marcado del nodo del mapa del sitio de DataList:

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]

![Actualización del mapa del sitio para incluir las nuevas páginas de ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: actualización del mapa del sitio para incluir las nuevas páginas de ASP.net

## <a name="a-review-of-paging"></a>Revisión de la paginación

En los tutoriales anteriores vimos cómo paginar los datos en los controles GridView, DetailsView y FormView. Estos tres controles ofrecen una forma sencilla de paginación denominada *paginación predeterminada* que se puede implementar simplemente mediante la comprobación de la opción Habilitar paginación en la etiqueta inteligente control s. Con la paginación predeterminada, cada vez que se solicita una página de datos en la primera visita de la página o cuando el usuario navega a otra página de datos, el control GridView, DetailsView o FormView vuelve a solicitar *todos* los datos de ObjectDataSource. A continuación, recorta el conjunto determinado de registros que se van a mostrar según el índice de página solicitado y el número de registros que se van a mostrar por página. Analizamos la paginación predeterminada en detalle en el tutorial [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-vb.md) .

Dado que la paginación predeterminada vuelve a solicitar todos los registros de cada página, no resulta práctico al paginar a través de cantidades de datos suficientemente grandes. Por ejemplo, Imagine que realiza la paginación a través de 50.000 registros con un tamaño de página de 10. Cada vez que el usuario se mueve a una nueva página, se deben recuperar todos los registros 50.000 de la base de datos, aunque solo se muestren diez de ellos.

La *paginación personalizada* resuelve los problemas de rendimiento de la paginación predeterminada al capturar solo el subconjunto preciso de registros que se va a mostrar en la página solicitada. Al implementar la paginación personalizada, debemos escribir la consulta SQL que devolverá de forma eficaz solo el conjunto de registros correcto. Vimos cómo crear este tipo de consulta mediante la [palabra clave new`ROW_NUMBER()`](http://www.4guysfromrolla.com/webtech/010406-1.shtml) de SQL Server 2005 s en el tutorial [paginación eficaz a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) .

Para implementar la paginación predeterminada en los controles DataList o Repeater, podemos usar la [clase`PagedDataSource`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como un contenedor en torno a la `ProductsDataTable` cuyo contenido se va a paginar. La clase `PagedDataSource` tiene una propiedad `DataSource` que se puede asignar a cualquier objeto enumerable y propiedades [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) y [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) que indican el número de registros que se muestran por página y el índice de la página actual. Una vez que se han establecido estas propiedades, el `PagedDataSource` se puede usar como origen de datos de cualquier control Web de datos. El `PagedDataSource`, cuando se enumera, solo devolverá el subconjunto adecuado de registros de su `DataSource` interno en función de las propiedades `PageSize` y `CurrentPageIndex`. En la figura 4 se describe la funcionalidad de la clase `PagedDataSource`.

![PagedDataSource ajusta un objeto enumerable con una interfaz paginable](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: el `PagedDataSource` encapsula un objeto enumerable con una interfaz paginable

El objeto `PagedDataSource` puede crearse y configurarse directamente desde el nivel de lógica de negocios y enlazarse a un objeto DataList o Repeater a través de un ObjectDataSource, o bien se puede crear y configurar directamente en la clase de código subyacente de la página ASP.NET. Si se usa el último enfoque, debemos renunciar al uso de ObjectDataSource y, en su lugar, enlazar los datos paginados a DataList o Repeater mediante programación.

El objeto `PagedDataSource` también tiene propiedades para admitir la paginación personalizada. Sin embargo, podemos omitir el uso de un `PagedDataSource` para la paginación personalizada porque ya disponemos de métodos BLL en la clase `ProductsBLL` diseñada para la paginación personalizada que devuelven los registros precisos que se van a mostrar.

En este tutorial veremos cómo implementar la paginación predeterminada en una lista de objetos agregando un nuevo método a la clase `ProductsBLL` que devuelve un objeto `PagedDataSource` configurado correctamente. En el siguiente tutorial, veremos cómo usar la paginación personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Paso 2: agregar un método de paginación predeterminado en el nivel de lógica de negocios

La clase `ProductsBLL` tiene actualmente un método para devolver toda la información del producto `GetProducts()` y otra para devolver un subconjunto determinado de productos en un índice de inicio `GetProductsPaged(startRowIndex, maximumRows)`. Con la paginación predeterminada, los controles GridView, DetailsView y FormView usan el método `GetProducts()` para recuperar todos los productos, pero, a continuación, usan un `PagedDataSource` internamente para mostrar solo el subconjunto de registros correcto. Para replicar esta funcionalidad con los controles DataList y Repeater, podemos crear un nuevo método en el BLL que imite este comportamiento.

Agregue un método a la clase `ProductsBLL` denominada `GetProductsAsPagedDataSource` que toma dos parámetros de entrada enteros:

- `pageIndex` el índice de la página que se va a mostrar, indizada en cero y
- `pageSize` el número de registros que se van a mostrar por página.

`GetProductsAsPagedDataSource` comienza recuperando *todos* los registros de `GetProducts()`. A continuación, crea un objeto `PagedDataSource`, estableciendo sus propiedades `CurrentPageIndex` y `PageSize` en los valores de los parámetros `pageIndex` y `pageSize` pasados. El método finaliza devolviendo esta `PagedDataSource`configurada:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Paso 3: Mostrar la información del producto en una lista de datos mediante la paginación predeterminada

Con el método `GetProductsAsPagedDataSource` agregado a la clase `ProductsBLL`, ahora podemos crear un DataList o Repeater que proporcione paginación predeterminada. Para empezar, abra la página `Paging.aspx` en la carpeta `PagingSortingDataListRepeater` y arrastre un DataList desde el cuadro de herramientas hasta el diseñador, estableciendo la propiedad DataList s `ID` en `ProductsDefaultPaging`. En la etiqueta inteligente DataList s, cree un nuevo ObjectDataSource denominado `ProductsDefaultPagingDataSource` y configúrelo para que recupere datos mediante el método `GetProductsAsPagedDataSource`.

[![crear un ObjectDataSource y configurarlo para que use el método GetProductsAsPagedDataSource ()](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: crear un ObjectDataSource y configurarlo para usar el método de `()` de `GetProductsAsPagedDataSource` ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

Establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

Dado que el método `GetProductsAsPagedDataSource` espera dos parámetros de entrada, el asistente nos pide el origen de estos valores de parámetro.

Los valores de tamaño de página y de índice de página deben recordarse en los postbacks. Pueden almacenarse en el estado de vista, conservarse en la cadena de consulta, almacenarse en variables de sesión o recordarse mediante alguna otra técnica. En este tutorial, usaremos QueryString, que tiene la ventaja de permitir que se marque una página de datos determinada.

En concreto, use los campos de cadena de consulta pageIndex y PageSize para los parámetros `pageIndex` y `pageSize`, respectivamente (consulte la figura 7). Dedique un momento a establecer los valores predeterminados para estos parámetros, ya que los valores de QueryString no estarán presentes cuando un usuario visite esta página por primera vez. Por `pageIndex`, establezca el valor predeterminado en 0 (que mostrará la primera página de datos) y `pageSize` valor predeterminado de s en 4.

[![usar la cadena de la cadena como origen para los parámetros pageIndex y PageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: uso de la cadena de consulta como origen para los parámetros `pageIndex` y `pageSize` ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))

Después de configurar ObjectDataSource, Visual Studio crea automáticamente un `ItemTemplate` para la lista de objetos. Personalice el `ItemTemplate` para que solo se muestren el nombre del producto, la categoría y el proveedor. Establezca también la propiedad DataList s `RepeatColumns` en 2, su `Width` en 100% y su `ItemStyle` s `Width` a 50%. Estos valores de ancho proporcionarán el mismo espaciado entre las dos columnas.

Después de realizar estos cambios, el marcado DataList y ObjectDataSource s deben tener un aspecto similar al siguiente:

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Dado que no vamos a realizar ninguna funcionalidad de actualización o eliminación en este tutorial, puede deshabilitar el estado de vista de DataList s para reducir el tamaño de página representado.

Al visitar inicialmente esta página a través de un explorador, no se proporcionan los parámetros `pageIndex` ni `pageSize` QueryString. Por lo tanto, se usan los valores predeterminados 0 y 4. Como se muestra en la figura 8, esto da como resultado una lista de listas que muestra los cuatro primeros productos.

[![se muestran los cuatro primeros productos](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: se muestran los cuatro primeros productos ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))

Sin una interfaz de paginación, actualmente no hay ningún medio directo para que un usuario navegue a la segunda página de datos. Vamos a crear una interfaz de paginación en el paso 4. Por el momento, sin embargo, la paginación solo se puede lograr especificando directamente los criterios de paginación en la cadena de búsqueda. Por ejemplo, para ver la segunda página, cambie la dirección URL en la barra de direcciones del explorador desde `Paging.aspx` a `Paging.aspx?pageIndex=2` y presione Entrar. Esto hace que se muestre la segunda página de datos (vea la ilustración 9).

[![se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: se muestra la segunda página de datos ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Paso 4: crear la interfaz de paginación

Hay una variedad de interfaces de paginación diferentes que se pueden implementar. Los controles GridView, DetailsView y FormView proporcionan cuatro interfaces diferentes para elegir entre:

- **Después,** los usuarios anteriores pueden desplace una página cada vez a la siguiente o a la anterior.
- **Siguiente, anterior, primero, último** además de los botones siguiente y anterior, esta interfaz incluye los botones primero y último para desplazarse a la primera o a la última página.
- **Numeric** muestra los números de página en la interfaz de paginación, lo que permite al usuario saltar rápidamente a una página determinada.
- **Numeric, First, Last,** además de los números de página numéricos, incluye botones para desplazarse a la primera o a la última página.

En el caso de DataList y Repeater, somos responsables de decidir sobre una interfaz de paginación e implementarla. Esto implica crear los controles Web necesarios en la página y mostrar la página solicitada cuando se hace clic en un botón de la interfaz de paginación en particular. Además, es posible que sea necesario deshabilitar ciertos controles de interfaz de paginación. Por ejemplo, al ver la primera página de datos con la siguiente, anterior, primera, última interfaz, se deshabilitarán los dos botones primero y anterior.

En este tutorial, vamos a usar la interfaz siguiente, anterior, primera y última. Agregue cuatro controles Web de botón a la página y establezca sus `ID` s en `FirstPage`, `PrevPage`, `NextPage`y `LastPage`. Establezca las propiedades `Text` en &lt;&lt; primero, &lt; anterior, siguiente &gt;y último &gt;&gt;.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

A continuación, cree un controlador de eventos `Click` para cada uno de estos botones. En un momento, agregaremos el código necesario para mostrar la página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Recordar el número total de registros que se van a paginar mediante

Independientemente de la interfaz de paginación seleccionada, es necesario calcular y recordar el número total de registros que se van a paginar. El recuento total de filas (junto con el tamaño de página) determina el número total de páginas de datos que se van a paginar, lo que determina qué controles de interfaz de paginación se agregan o están habilitados. En la siguiente interfaz, anterior, primera, última que se va a compilar, el recuento de páginas se usa de dos maneras:

- Para determinar si estamos viendo la última página, en cuyo caso los botones siguiente y último están deshabilitados.
- Si el usuario hace clic en el último botón, debemos whisky en la última página, cuyo índice es uno menos que el recuento de páginas.

El número de páginas se calcula como el límite superior del recuento total de filas dividido por el tamaño de página. Por ejemplo, si se paginan los registros 79 con cuatro registros por página, el recuento de páginas es 20 (el límite superior de 79/4). Si usamos la interfaz de paginación numérica, esta información nos informa del número de botones de página numéricos que se van a mostrar. Si la interfaz de paginación incluye botones siguiente o último, el recuento de páginas se usa para determinar cuándo deshabilitar los botones siguiente o último.

Si la interfaz de paginación incluye un último botón, es imperativo que el número total de registros que se van a paginar se recuerde en los postbacks de modo que, cuando se haga clic en el último botón, podamos determinar el índice de la última página. Para facilitar esto, cree una propiedad `TotalRowCount` en la clase de código subyacente de la página ASP.NET que conserve su valor en el estado de vista:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Además de `TotalRowCount`, dedique un minuto a crear propiedades de nivel de página de solo lectura para acceder fácilmente al índice de página, el tamaño de página y el recuento de páginas:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinar el número total de registros que se van a paginar

El objeto `PagedDataSource` devuelto desde el método ObjectDataSource s `Select()` tiene *todos* los registros de producto, aunque solo se muestre un subconjunto de ellos en la lista de objetos. La propiedad `PagedDataSource` s [`Count`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) devuelve solo el número de elementos que se mostrarán en la lista de objetos; la [propiedad`DataSourceCount`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) devuelve el número total de elementos de la `PagedDataSource`. Por lo tanto, es necesario asignar la propiedad `TotalRowCount` de la página ASP.NET el valor de la propiedad `PagedDataSource` s `DataSourceCount`.

Para ello, cree un controlador de eventos para el evento ObjectDataSource s `Selected`. En el controlador de eventos `Selected` tenemos acceso al valor devuelto del método ObjectDataSource s `Select()` en este caso, el `PagedDataSource`.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Mostrar la página de datos solicitada

Cuando el usuario hace clic en uno de los botones de la interfaz de paginación, es necesario mostrar la página de datos solicitada. Puesto que los parámetros de paginación se especifican a través de la cadena de consulta, para mostrar la página solicitada de uso de datos `Response.Redirect(url)` para que el explorador del usuario vuelva a solicitar la página de `Paging.aspx` con los parámetros de paginación adecuados. Por ejemplo, para mostrar la segunda página de datos, se redirigirá al usuario a `Paging.aspx?pageIndex=1`.

Para facilitar esto, cree un método `RedirectUser(sendUserToPageIndex)` que redirija al usuario a `Paging.aspx?pageIndex=sendUserToPageIndex`. A continuación, llame a este método desde el botón cuatro `Click` controladores de eventos. En el controlador de eventos `FirstPage` `Click`, llame a `RedirectUser(0)`para enviarlos a la primera página; en el controlador de eventos `PrevPage` `Click`, utilice `PageIndex - 1` como índice de la página; etc.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Una vez que se completan los controladores de eventos de `Click`, se pueden paginar los registros de DataList s haciendo clic en los botones. Tómese un momento para probarlo.

## <a name="disabling-paging-interface-controls"></a>Deshabilitar controles de interfaz de paginación

Actualmente, los cuatro botones están habilitados, independientemente de la página que se está viendo. Sin embargo, queremos deshabilitar los botones primero y anterior al mostrar la primera página de datos y los botones siguiente y último al mostrar la última página. El `PagedDataSource` objeto que devuelve el método ObjectDataSource s `Select()` tiene propiedades [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) y [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que se pueden examinar para determinar si se está viendo la primera o la última página de datos.

Agregue lo siguiente al controlador de eventos ObjectDataSource s `Selected`:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Con esta adición, se deshabilitarán los botones primero y anterior al ver la primera página, mientras que los botones siguiente y último se deshabilitarán al ver la última página.

Deje que complete la interfaz de paginación informando al usuario de la página que se está viendo en ese momento y del número total de páginas que existen. Agregue un control Web de etiqueta a la página y establezca su propiedad `ID` en `CurrentPageNumber`. Establezca su propiedad `Text` en el controlador de eventos seleccionado de ObjectDataSource s de modo que incluya la página actual que se está viendo (`PageIndex + 1`) y el número total de páginas (`PageCount`).

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

En la figura 10 se muestra `Paging.aspx` cuando se visita por primera vez. Como QueryString está vacío, el valor predeterminado de DataList es mostrar los cuatro primeros productos; los botones primero y anterior están deshabilitados. Al hacer clic en siguiente se muestran los cuatro registros siguientes (vea la figura 11); ahora se habilitan los botones primero y anterior.

[![se muestra la primera página de datos](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: se muestra la primera página de datos ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))

[![se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: se muestra la segunda página de datos ([haga clic para ver la imagen de tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))

> [!NOTE]
> La interfaz de paginación se puede mejorar aún más si se permite al usuario especificar el número de páginas que se van a ver por página. Por ejemplo, se podría agregar un DropDownList con las opciones de tamaño de página como 5, 10, 25, 50 y todos. Al seleccionar un tamaño de página, el usuario tendría que redirigirse de nuevo a `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Dejo de implementar esta mejora como un ejercicio para el lector.

## <a name="using-custom-paging"></a>Usar la paginación personalizada

Las páginas DataList a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Al paginar a través de cantidades de datos suficientemente grandes, es fundamental que se use la paginación personalizada. Aunque los detalles de implementación difieren ligeramente, los conceptos que hay detrás de la implementación de la paginación personalizada en una lista de datos son los mismos que con la paginación predeterminada. Con la paginación personalizada, use el método `ProductBLL` Class s `GetProductsPaged` (en lugar de `GetProductsAsPagedDataSource`). Tal y como se describe en el tutorial [paginación eficaz a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) , `GetProductsPaged` se debe pasar el índice de la fila inicial y el número máximo de filas que se van a devolver. Estos parámetros se pueden mantener a través de QueryString de la misma manera que los parámetros `pageIndex` y `pageSize` que se usan en la paginación predeterminada.

Dado que no hay ningún `PagedDataSource` con paginación personalizada, se deben usar técnicas alternativas para determinar el número total de registros que se van a paginar y si se va a mostrar la primera o la última página de datos. El método `TotalNumberOfProducts()` de la clase `ProductsBLL` devuelve el número total de productos que se van a paginar. Para determinar si se está viendo la primera página de datos, examine el índice de la fila inicial si es cero y, a continuación, se verá la primera página. Se está viendo la última página si el índice de la fila inicial más las filas máximas que se van a devolver es mayor o igual que el número total de registros que se van a paginar.

Exploraremos la implementación de la paginación personalizada con mayor detalle en el siguiente tutorial.

## <a name="summary"></a>Resumen

Aunque ni los controles DataList ni Repeater ofrecen la compatibilidad de paginación lista para usar que se encuentra en los controles GridView, DetailsView y FormView, esta funcionalidad se puede Agregar con un esfuerzo mínimo. La manera más sencilla de implementar la paginación predeterminada es ajustar todo el conjunto de productos dentro de un `PagedDataSource` y, a continuación, enlazar el `PagedDataSource` a la lista de controles o al Repeater. En este tutorial se ha agregado el método `GetProductsAsPagedDataSource` a la clase `ProductsBLL` para devolver el `PagedDataSource`. La clase `ProductsBLL` ya contiene los métodos necesarios para la paginación personalizada `GetProductsPaged` y `TotalNumberOfProducts`.

Además de recuperar el conjunto preciso de registros que se van a mostrar en la paginación personalizada o en todos los registros de un `PagedDataSource` para la paginación predeterminada, también es necesario agregar manualmente la interfaz de paginación. En este tutorial se ha creado una interfaz Next, Previous, First, Last con cuatro controles Web de botón. Además, se ha agregado un control de etiqueta que muestra el número de página actual y el número total de páginas.

En el tutorial siguiente, veremos cómo agregar compatibilidad de ordenación a los controles DataList y Repeater. También veremos cómo crear una lista de valores que se puede paginar y ordenar (con ejemplos mediante la paginación predeterminada y personalizada).

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Liz Shulok, Ken Pespisa y Bernadette Leigh. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Siguiente](sorting-data-in-a-datalist-or-repeater-control-vb.md)
