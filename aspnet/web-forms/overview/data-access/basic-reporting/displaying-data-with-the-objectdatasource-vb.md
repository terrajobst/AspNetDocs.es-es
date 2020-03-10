---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Mostrar datos con ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se examina el control ObjectDataSource mediante este control, puede enlazar los datos recuperados de la capa BLL creada en el tutorial anterior sin HAVI...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 754188352cbfb08e610027f5b7890a32bd88ae26
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483055"
---
# <a name="displaying-data-with-the-objectdatasource-vb"></a>Visualizar datos con ObjectDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) de la aplicación de ejemplo o [descarga de PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> En este tutorial se examina el control ObjectDataSource mediante este control, puede enlazar los datos recuperados de la capa BLL creada en el tutorial anterior sin tener que escribir una línea de código.

## <a name="introduction"></a>Introducción

Una vez que el diseño de la página del sitio web y la arquitectura de la aplicación, estamos listos para empezar a explorar cómo realizar una serie de tareas comunes relacionadas con los datos y los informes. En los tutoriales anteriores, hemos visto cómo enlazar datos de la capa DAL y BLL a un control Web de datos en una página ASP.NET. Esta sintaxis asigna la propiedad `DataSource` del control Web de datos a los datos que se van a mostrar y, a continuación, llamar al método de `DataBind()` del control era el patrón utilizado en las aplicaciones de ASP.NET 1. x y puede seguir utilizándose en las aplicaciones 2,0. Sin embargo, los nuevos controles de origen de datos de ASP.NET 2.0 ofrecen una manera declarativa de trabajar con datos. Con estos controles, puede enlazar los datos recuperados de la capa BLL creada en el tutorial anterior sin tener que escribir una línea de código.

ASP.NET 2,0 se distribuye con cinco controles de origen de datos integrados [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)y [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) , aunque puede crear sus propios [controles de origen de datos personalizados](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), si es necesario. Como hemos desarrollado una arquitectura para nuestra aplicación de tutorial, usaremos ObjectDataSource en nuestras clases de BLL.

![ASP.NET 2,0 incluye cinco controles de origen de datos integrados](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: ASP.net 2,0 incluye cinco controles de origen de datos integrados

ObjectDataSource actúa como proxy para trabajar con otro objeto. Para configurar el ObjectDataSource, se especifica este objeto subyacente y cómo se asignan sus métodos a los métodos `Select`, `Insert`, `Update`y `Delete` de ObjectDataSource. Una vez que se ha especificado este objeto subyacente y sus métodos se han asignado a la base de datos ObjectDataSource, podemos enlazar el origen ObjectDataSource a un control Web de datos. ASP.NET se incluye con muchos controles Web de datos, incluidos GridView, DetailsView, RadioButtonList y DropDownList, entre otros. Durante el ciclo de vida de la página, es posible que el control Web de datos necesite tener acceso a los datos a los que está enlazado, lo que realizará invocando el método `Select` de ObjectDataSource; Si el control Web de datos permite insertar, actualizar o eliminar, se pueden realizar llamadas a los métodos `Insert`, `Update`o `Delete` de ObjectDataSource. A continuación, el ObjectDataSource enruta estas llamadas a los métodos del objeto subyacente adecuados tal y como se muestra en el diagrama siguiente.

[![ObjectDataSource actúa como proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: ObjectDataSource actúa como proxy ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image4.png))

Aunque ObjectDataSource puede usarse para invocar métodos para insertar, actualizar o eliminar datos, vamos a centrarse en la devolución de datos. en los próximos tutoriales se explorará el uso de los controles Web de datos e ObjectDataSource que modifican los datos.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Paso 1: agregar y configurar el control ObjectDataSource

Para empezar, abra la página `SimpleDisplay.aspx` de la carpeta `BasicReporting`, cambie a Vista de diseño y, a continuación, arrastre un control ObjectDataSource desde el cuadro de herramientas hasta la superficie de diseño de la página. ObjectDataSource aparece como un cuadro gris en la superficie de diseño porque no produce ningún marcado. simplemente tiene acceso a los datos invocando un método desde un objeto especificado. Los datos devueltos por un ObjectDataSource se pueden mostrar mediante un control Web de datos, como GridView, DetailsView, FormView, etc.

> [!NOTE]
> Como alternativa, primero puede Agregar el control Web de datos a la página y, a continuación, desde su etiqueta inteligente, elegir la opción &lt;nuevo origen de datos&gt; en la lista desplegable.

Para especificar el objeto subyacente de ObjectDataSource y cómo se asignan los métodos de ese objeto a la base de datos de origen, haga clic en el vínculo configurar origen de datos de la etiqueta inteligente de ObjectDataSource.

[![haga clic en el vínculo configurar origen de datos de la etiqueta inteligente](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: haga clic en el vínculo configurar origen de datos de la etiqueta inteligente ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image7.png))

Se abrirá el Asistente para configurar el origen de datos. En primer lugar, debemos especificar el objeto con el que se va a trabajar ObjectDataSource. Si la casilla "Mostrar solo los componentes de datos" está activada, en la lista desplegable de esta pantalla solo se muestran los objetos que se han decorado con el `DataObject` atributo. Actualmente, nuestra lista incluye los TableAdapters en el conjunto de DataSet con tipo y las clases de BLL que creamos en el tutorial anterior. Si olvidó agregar el atributo `DataObject` a las clases de la capa de lógica de negocios, no los verá en esta lista. En ese caso, desactive la casilla "Mostrar solo los componentes de datos" para ver todos los objetos, que deben incluir las clases BLL (junto con las demás clases del conjunto de datos con tipo que las tablas DataTable, DataRows, etc.).

En la primera pantalla, elija la clase de `ProductsBLL` de la lista desplegable y haga clic en siguiente.

[![especificar el objeto que se va a utilizar con el control ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Figura 4**: especificar el objeto que se va a usar con el control ObjectDataSource ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image10.png))

La siguiente pantalla del asistente le pedirá que seleccione el método que ObjectDataSource debe invocar. La lista desplegable muestra los métodos que devuelven datos en el objeto seleccionado en la pantalla anterior. Aquí vemos `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`y `GetProductsBySupplierID`. Seleccione el método de `GetProducts` en la lista desplegable y haga clic en finalizar (si agregó el `DataObjectMethodAttribute` a los métodos de la `ProductBLL`como se muestra en el tutorial anterior, esta opción estará seleccionada de forma predeterminada).

[![elegir el método para devolver datos desde la pestaña seleccionar](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Figura 5**: elegir el método para devolver datos desde la pestaña seleccionar ([haga clic para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Configuración manual de ObjectDataSource

El Asistente para configurar el origen de datos de ObjectDataSource ofrece una forma rápida de especificar el objeto que utiliza y de asociar los métodos del objeto que se invocan. Sin embargo, puede configurar ObjectDataSource a través de sus propiedades, ya sea a través de la ventana Propiedades o directamente en el marcado declarativo. Basta con establecer la propiedad `TypeName` en el tipo del objeto subyacente que se va a utilizar y el `SelectMethod` en el método que se va a invocar al recuperar los datos.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Incluso si prefiere el Asistente para configurar orígenes de datos, puede haber ocasiones en las que necesite configurar manualmente ObjectDataSource, ya que el asistente solo muestra las clases creadas por el desarrollador. Si desea enlazar el origen ObjectDataSource a una clase en el .NET Framework como la [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx), para tener acceso a la información de la cuenta de usuario o la [clase de directorio](https://msdn.microsoft.com/library/system.io.directory.aspx) para trabajar con la información del sistema de archivos, debe establecer manualmente las propiedades de ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Paso 2: agregar un control Web de datos y enlazarlo a ObjectDataSource

Una vez que ObjectDataSource se ha agregado a la página y configurado, estamos listos para agregar controles Web de datos a la página para mostrar los datos devueltos por el método `Select` de ObjectDataSource. Cualquier control Web de datos se puede enlazar a un ObjectDataSource; Echemos un vistazo a la visualización de los datos de ObjectDataSource en GridView, DetailsView y FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Enlazar un control GridView a ObjectDataSource

Agregue un control GridView desde el cuadro de herramientas a la superficie de diseño de `SimpleDisplay.aspx`. En la etiqueta inteligente de GridView, elija el control ObjectDataSource que hemos agregado en el paso 1. Esto creará automáticamente una BoundField en GridView para cada propiedad devuelta por los datos del método `Select` de ObjectDataSource (es decir, las propiedades definidas por la DataTable Products).

[![se ha agregado un control GridView a la página y está enlazado a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Figura 6**: se ha agregado un control GridView a la página y está enlazado a ObjectDataSource ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image16.png))

Después, puede personalizar, reorganizar o quitar el BoundFields de GridView haciendo clic en la opción Editar columnas de la etiqueta inteligente.

[![administrar los BoundFields de GridView mediante el cuadro de diálogo Editar columnas](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Figura 7**: administración de BoundFields de GridView mediante el cuadro de diálogo Editar columnas ([haga clic para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image19.png))

Dedique un momento a modificar el BoundFields de GridView, quitando los `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`y `ReorderLevel` BoundFields. Simplemente seleccione BoundField en la lista de la parte inferior izquierda y haga clic en el botón Eliminar (la X roja) para quitarlos. A continuación, reorganice el BoundFields para que el `CategoryName` y `SupplierName` BoundFields precedan a la `UnitPrice` BoundField; para ello, seleccione estos BoundFields y haga clic en la flecha arriba. Establezca las propiedades `HeaderText` del BoundFields restante en `Products`, `Category`, `Supplier`y `Price`, respectivamente. A continuación, haga que el `Price` BoundField tenga el formato de moneda estableciendo la propiedad `HtmlEncode` de BoundField en false y su propiedad `DataFormatString` en `{0:c}`. Por último, Alinee horizontalmente el `Price` a la derecha y la casilla `Discontinued` en el centro a través de la `ItemStyle`/propiedad `HorizontalAlign`.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

[![se han personalizado los BoundFields de GridView](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Figura 8**: los BoundFields de GridView se han personalizado ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Usar temas para una apariencia coherente

Estos tutoriales esfuerzan por quitar cualquier configuración de estilo de nivel de control, en lugar de usar hojas de estilos en cascada definidas en un archivo externo siempre que sea posible. El archivo de `Styles.css` contiene `DataWebControlStyle`, `HeaderStyle`, `RowStyle`y `AlternatingRowStyle` clases CSS que se deben usar para dictar la apariencia de los controles Web de datos utilizados en estos tutoriales. Para ello, podríamos establecer la propiedad `CssClass` de GridView en `DataWebControlStyle`, y sus propiedades `HeaderStyle`, `RowStyle`y `AlternatingRowStyle` ' `CssClass` propiedades en consecuencia.

Si establecemos estas propiedades de `CssClass` en el control Web, es necesario recordar explícitamente establecer estos valores de propiedad para cada uno de los controles Web de datos agregados a nuestros tutoriales. Un enfoque más fácil de administrar consiste en definir las propiedades predeterminadas relacionadas con CSS para los controles GridView, DetailsView y FormView mediante un tema. Un tema es una colección de valores de propiedades de nivel de control, imágenes y clases CSS que se pueden aplicar a las páginas de un sitio para aplicar una apariencia común.

Nuestro tema no incluirá imágenes ni archivos CSS (dejaremos la hoja de estilos `Styles.css` tal cual, definida en la carpeta raíz de la aplicación web), pero incluirá dos máscaras. Una máscara es un archivo que define las propiedades predeterminadas de un control Web. En concreto, tendremos un archivo de máscara para los controles GridView y DetailsView, que indica las propiedades predeterminadas relacionadas con el `CssClass`.

Para empezar, agregue un nuevo archivo de máscara al proyecto denominado `GridView.skin`; para ello, haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.

[![agregar un archivo de máscara denominado GridView. Skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Ilustración 9**: agregar un archivo de máscara denominado `GridView.skin` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image25.png))

Los archivos de máscara deben colocarse en un tema, que se encuentra en la carpeta `App_Themes`. Como todavía no tenemos una carpeta de este tipo, Visual Studio le ofrecerá la posibilidad de crear una para nosotros al agregar nuestra primera máscara. Haga clic en sí para crear la carpeta `App_Theme` y colocar el nuevo archivo de `GridView.skin`.

[![permitir que Visual Studio cree la carpeta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Figura 10**: dejar que Visual Studio cree la carpeta `App_Theme` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image28.png))

Se creará un nuevo tema en la carpeta `App_Themes` denominada GridView con el archivo de máscara `GridView.skin`.

![El tema de GridView se ha agregado a la carpeta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Figura 11**: el tema de GridView se ha agregado a la carpeta `App_Theme`

Cambie el nombre del tema de GridView a DataWebControls (haga clic con el botón derecho en la carpeta GridView en la carpeta `App_Theme` y elija cambiar nombre). A continuación, escriba el siguiente marcado en el archivo de `GridView.skin`:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Esto define las propiedades predeterminadas de las propiedades relacionadas con el `CssClass`para cualquier GridView en cualquier página que use el tema DataWebControls. Vamos a agregar otra máscara para DetailsView, un control Web de datos que usaremos en breve. Agregue una nueva máscara al tema DataWebControls denominado `DetailsView.skin` y agregue el siguiente marcado:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Con nuestro tema definido, el último paso es aplicar el tema a nuestra página de ASP.NET. Un tema se puede aplicar página por página o para todas las páginas de un sitio Web. Vamos a usar este tema para todas las páginas del sitio Web. Para ello, agregue el marcado siguiente a la sección `<system.web>` de `Web.config`:

[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

Así de simple. El valor `styleSheetTheme` indica que las propiedades especificadas en el tema *no* deben reemplazar las propiedades especificadas en el nivel de control. Para especificar que la configuración del tema debe contraplazarse a la configuración del control, utilice el atributo `theme` en lugar de `styleSheetTheme`; Desafortunadamente, la configuración de tema no aparece en la Vista de diseño de Visual Studio. Consulte [información general sobre los temas y las máscaras de ASP.net](https://msdn.microsoft.com/library/ykzx33wh.aspx) y los [estilos del lado servidor mediante temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) para obtener más información sobre temas y máscaras. consulte [Cómo: aplicar temas de ASP.net](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) para más información sobre la configuración de una página para usar un tema.

[![GridView muestra el nombre del producto, la categoría, el proveedor, el precio y la información no incluida.](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: el control GridView muestra el nombre del producto, la categoría, el proveedor, el precio y la información no incluida ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Mostrar un registro cada vez en DetailsView

GridView muestra una fila por cada registro devuelto por el control de origen de datos al que está enlazado. No obstante, hay ocasiones en las que es posible que deseemos mostrar un único registro o solo un registro cada vez. El [control DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) ofrece esta funcionalidad, que se representa como un `<table>` HTML con dos columnas y una fila por cada columna o propiedad enlazada al control. Puede considerar DetailsView como GridView con un solo registro girado 90 grados.

Empiece agregando un control DetailsView *sobre* GridView en `SimpleDisplay.aspx`. A continuación, enlácelo al mismo control ObjectDataSource que GridView. Al igual que con GridView, se agregará una BoundField a DetailsView para cada propiedad del objeto devuelto por el método `Select` de ObjectDataSource. La única diferencia es que los BoundFields de DetailsView se colocan horizontalmente en lugar de vertical.

[![agregar DetailsView a la página y enlazarlo a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Figura 13**: agregar un DetailsView a la página y enlazarlo a ObjectDataSource ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image35.png))

Al igual que GridView, el BoundFields de DetailsView se puede ajustar para proporcionar una visualización más personalizada de los datos devueltos por ObjectDataSource. En la figura 14 se muestra DetailsView después de que se han configurado las propiedades BoundFields y `CssClass` para que su apariencia sea similar a la del ejemplo GridView.

[![DetailsView muestra un solo registro](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Figura 14**: DetailsView muestra un solo registro ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image38.png))

Tenga en cuenta que DetailsView solo muestra el primer registro devuelto por su origen de datos. Para permitir que el usuario recorra todos los registros, de uno en uno, debemos habilitar la paginación para DetailsView. Para ello, vuelva a Visual Studio y active la casilla habilitar paginación en la etiqueta inteligente de DetailsView.

[![habilitar la paginación en el control DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Figura 15**: habilitar la paginación en el control DetailsView ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image41.png))

[![con la paginación habilitada, DetailsView permite al usuario ver cualquiera de los productos.](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Figura 16**: con la paginación habilitada, DetailsView permite al usuario ver cualquiera de los productos ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image44.png))

Hablaremos más sobre la paginación en futuros tutoriales.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Un diseño más flexible para mostrar un registro cada vez

DetailsView es bastante rígido en cómo muestra cada registro devuelto desde ObjectDataSource. Es posible que desee una vista más flexible de los datos. Por ejemplo, en lugar de mostrar el nombre del producto, la categoría, el proveedor, el precio y la información no incluida en una fila independiente, es posible que desee mostrar el nombre y el precio del producto en un encabezado de `<h4>`, con la categoría y la información del proveedor que aparece por debajo del nombre y el precio en un tamaño de fuente menor. Y es posible que no le interese mostrar los nombres de propiedad (producto, categoría, etc.) junto a los valores.

El [control FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) proporciona este nivel de personalización. En lugar de usar campos (como GridView y DetailsView), FormView usa plantillas, que permiten una combinación de controles Web, HTML estático y [Sintaxis de enlace](http://www.15seconds.com/issue/040630.htm)de los enlaces. Si está familiarizado con el control Repeater de ASP.NET 1. x, puede pensar en FormView como el repetidor para mostrar un único registro.

Agregue un control FormView a la superficie de diseño de la página `SimpleDisplay.aspx`. Inicialmente, FormView se muestra como un bloque gris que le informa de que necesitamos proporcionar, como mínimo, el `ItemTemplate`del control.

[![FormView debe incluir ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Figura 17**: el FormView debe incluir un `ItemTemplate` ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image47.png))

Puede enlazar el FormView directamente a un control de origen de datos a través de la etiqueta inteligente de FormView, que creará una `ItemTemplate` predeterminada automáticamente (junto con una `EditItemTemplate` y `InsertItemTemplate`, si se establecen las propiedades `InsertMethod` y `UpdateMethod` del control ObjectDataSource). Sin embargo, para este ejemplo, vamos a enlazar los datos a FormView y especificar su `ItemTemplate` manualmente. Para empezar, establezca la propiedad `DataSourceID` de FormView en el `ID` del control ObjectDataSource `ObjectDataSource1`. A continuación, cree el `ItemTemplate` para que muestre el nombre y el precio del producto en un elemento `<h4>` y los nombres de categoría y de remitente debajo de este en un tamaño de fuente menor.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]

[![el primer producto (Chai) se muestra en un formato personalizado](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Figura 18**: el primer producto (Chai) se muestra en un formato personalizado ([haga clic para ver la imagen de tamaño completo](displaying-data-with-the-objectdatasource-vb/_static/image50.png))

La `<%# Eval(propertyName) %>` es la sintaxis de DataBinding. El método `Eval` devuelve el valor de la propiedad especificada para el objeto actual que se está enlazando al control FormView. Consulte la sintaxis de enlace de [datos simplificado y extendido](http://www.15seconds.com/issue/040630.htm) del artículo de Alex homero en ASP.net 2,0 para obtener más información sobre los pros y los contras de los enlaces de datos.

Como DetailsView, FormView solo muestra el primer registro devuelto desde ObjectDataSource. Puede habilitar la paginación en FormView para permitir que los visitantes desplazan los productos de uno en uno.

## <a name="summary"></a>Resumen

El acceso y la visualización de los datos de una capa de lógica de negocios se pueden realizar sin escribir una línea de código gracias al control ObjectDataSource de ASP.NET 2.0. ObjectDataSource invoca un método especificado de una clase y devuelve los resultados. Estos resultados se pueden mostrar en un control Web de datos enlazado a ObjectDataSource. En este tutorial, hemos examinado el enlace de los controles GridView, DetailsView y FormView a ObjectDataSource.

Hasta ahora solo hemos visto cómo usar ObjectDataSource para invocar un método sin parámetros, pero ¿qué ocurre si se desea invocar un método que espera parámetros de entrada, como el `GetProductsByCategoryID(categoryID)`de la clase `ProductBLL`? Para llamar a un método que espera uno o más parámetros, se debe configurar ObjectDataSource para especificar los valores de estos parámetros. Veremos cómo hacerlo en el [siguiente tutorial](declarative-parameters-vb.md).

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Crear sus propios controles de origen de datos](https://msdn.microsoft.com/library/ms364049.aspx)
- [Ejemplos de GridView para ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Sintaxis de enlace de datos simplificada y extendida en ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Temas en ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Estilos en el lado servidor mediante temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Cómo: aplicar temas de ASP.NET mediante programación](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Siguiente](declarative-parameters-vb.md)
