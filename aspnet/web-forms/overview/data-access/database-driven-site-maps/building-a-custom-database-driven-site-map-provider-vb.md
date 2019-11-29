---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Crear un proveedor personalizado de mapa del sitio controlado por base de datos (VB) | Microsoft Docs
author: rick-anderson
description: El proveedor del mapa del sitio predeterminado en ASP.NET 2,0 recupera sus datos de un archivo XML estático. Aunque el proveedor basado en XML es adecuado para muchas pequeñas y medianas siz...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 78051696bd75e1d574f55b1c5d5891fe67c3030d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74630447"
---
# <a name="building-a-custom-database-driven-site-map-provider-vb"></a>Crear un proveedor personalizado de mapas del sitio controlado por base de datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) o [Descargar PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> El proveedor del mapa del sitio predeterminado en ASP.NET 2,0 recupera sus datos de un archivo XML estático. Aunque el proveedor basado en XML es adecuado para muchos sitios web pequeños y de tamaño medio, las aplicaciones web más grandes requieren un mapa de sitio más dinámico. En este tutorial, vamos a crear un proveedor de mapa del sitio personalizado que recupera sus datos de la capa de lógica de negocios, que a su vez recupera los datos de la base de datos.

## <a name="introduction"></a>Introducción

La característica de mapa del sitio de ASP.NET 2,0 s permite a un desarrollador de páginas definir un mapa del sitio de la aplicación web en un medio persistente, como en un archivo XML. Una vez definidos, se puede tener acceso a los datos del mapa del sitio mediante programación a través de la [clase`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) en el [espacio de nombres`System.Web`](https://msdn.microsoft.com/library/system.web.aspx) o a través de una variedad de controles Web de navegación, como los controles SiteMapPath, MENU y TreeView. El sistema del mapa del sitio utiliza el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) para que se puedan crear y conectar diferentes implementaciones de serialización de mapa del sitio en una aplicación Web. El proveedor del mapa del sitio predeterminado que se incluye con ASP.NET 2,0 conserva la estructura del mapa del sitio en un archivo XML. De vuelta en las [páginas maestras y](../introduction/master-pages-and-site-navigation-vb.md) el tutorial de navegación del sitio, creamos un archivo denominado `Web.sitemap` que contenía esta estructura y se ha actualizado su XML con cada nueva sección del tutorial.

El proveedor del mapa del sitio basado en XML predeterminado funciona bien si la estructura del mapa del sitio es bastante estática, como en estos tutoriales. En muchos casos, sin embargo, se necesita un mapa de sitio más dinámico. Considere el mapa del sitio que se muestra en la figura 1, donde cada categoría y producto aparecen como secciones en la estructura del sitio Web. Con este mapa del sitio, la visita a la página web correspondiente al nodo raíz puede enumerar todas las categorías, mientras que al visitar una determinada página web de una categoría, se mostrarán los productos de la categoría s y la visualización de una página web de productos en particular mostraría los detalles del producto.

[![las categorías y los productos de la estructura del mapa del sitio](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Figura 1**: la composición de categorías y productos de la estructura del mapa del sitio ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))

Aunque esta estructura basada en categorías y productos podría estar codificada de forma rígida en el archivo `Web.sitemap`, es necesario actualizar el archivo cada vez que se agrega, quita o cambia el nombre de una categoría o producto. Por lo tanto, el mantenimiento del mapa del sitio se simplificaría enormemente si su estructura se recupera de la base de datos o, idealmente, de la capa de lógica de negocios de la arquitectura de la aplicación. De este modo, a medida que se agregan, cambian de nombre o eliminan productos y categorías, el mapa del sitio se actualiza automáticamente para reflejar estos cambios.

Como la serialización del mapa del sitio de ASP.NET 2,0 s se basa en el modelo del proveedor, podemos crear nuestro propio proveedor de mapa del sitio personalizado que obtiene sus datos de un almacén de datos alternativo, como la base de datos o la arquitectura. En este tutorial, vamos a crear un proveedor personalizado que recupera sus datos de la capa BLL. Vamos a empezar a trabajar.

> [!NOTE]
> El proveedor del mapa del sitio personalizado creado en este tutorial está estrechamente acoplado a la arquitectura y al modelo de datos de la aplicación. Jeff Prosise s [almacenando mapas de sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) y [el proveedor del mapa del sitio de SQL que espera que](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) los artículos examinen un enfoque generalizado para almacenar los datos del mapa del sitio en SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Paso 1: creación de las páginas web del proveedor del mapa del sitio personalizado

Antes de empezar a crear un proveedor de mapa del sitio personalizado, vamos a agregar primero las páginas de ASP.NET que necesitamos para este tutorial. Comience agregando una nueva carpeta denominada `SiteMapProvider`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Agregue también una subcarpeta `CustomProviders` a la carpeta `App_Code`.

![Agregue las páginas ASP.NET para los tutoriales relacionados con el proveedor del mapa del sitio](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Figura 2**: agregar las páginas ASP.net para los tutoriales relacionados con el proveedor del mapa del sitio

Dado que solo hay un tutorial para esta sección, no es necesario `Default.aspx` para enumerar los tutoriales de la sección. En su lugar, `Default.aspx` mostrará las categorías en un control GridView. Abordaremos esto en el paso 2.

A continuación, actualice `Web.sitemap` para incluir una referencia a la página de `Default.aspx`. En concreto, agregue el siguiente marcado después del `<siteMapNode>`de almacenamiento en caché:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora un elemento para el tutorial del proveedor del mapa del sitio único.

![El mapa del sitio ahora incluye una entrada para el tutorial del proveedor del mapa del sitio](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Figura 3**: ahora, el mapa del sitio incluye una entrada para el tutorial del proveedor del mapa del sitio

El enfoque principal de este tutorial es ilustrar la creación de un proveedor de mapa del sitio personalizado y la configuración de una aplicación web para que use ese proveedor. En concreto, crearemos un proveedor que devuelva un mapa del sitio que incluya un nodo raíz junto con un nodo para cada categoría y producto, como se muestra en la figura 1. En general, cada nodo del mapa del sitio puede especificar una dirección URL. En nuestro mapa del sitio, la dirección URL del nodo raíz se `~/SiteMapProvider/Default.aspx`, que enumerará todas las categorías de la base de datos. Cada nodo de categoría del mapa del sitio tendrá una dirección URL que señala a `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, que enumerará todos los productos del *CategoryID*especificado. Por último, cada nodo del mapa del sitio del producto apuntará a `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, que mostrará los detalles específicos del producto.

Para empezar, es necesario crear las páginas `Default.aspx`, `ProductsByCategory.aspx`y `ProductDetails.aspx`. Estas páginas se completan en los pasos 2, 3 y 4, respectivamente. Dado que el paso de este tutorial está en los proveedores del mapa del sitio, y como los tutoriales anteriores han explicado la creación de estos tipos de informes maestros y detallados de varias páginas, se realizarán prisas pasos del 2 al 4. Si necesita un actualizador para crear informes maestros y detallados que abarquen varias páginas, consulte el tutorial de [filtrado maestro y detalles en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Paso 2: mostrar una lista de categorías

Abra la página `Default.aspx` en la carpeta `SiteMapProvider` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, estableciendo su `ID` en `Categories`. En la etiqueta inteligente de GridView s, enlácelo a un nuevo ObjectDataSource denominado `CategoriesDataSource` y configúrelo para que recupere sus datos mediante el método `CategoriesBLL` de clase s `GetCategories`. Puesto que este GridView solo muestra las categorías y no proporciona capacidades de modificación de datos, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![configurar ObjectDataSource para devolver categorías mediante el método GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Figura 4**: configuración de ObjectDataSource para devolver las categorías con el método `GetCategories` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Figura 5**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará un BoundField para `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`y `BrochurePath`. Edite GridView para que solo contenga el `CategoryName` y `Description` BoundFields y actualice la propiedad `CategoryName` BoundField s `HeaderText` a Category.

Después, agregue un HyperLinkField y colóquelo para que sea el campo situado más a la izquierda. Establezca la propiedad `DataNavigateUrlFields` en `CategoryID` y la propiedad `DataNavigateUrlFormatString` en `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Establezca la propiedad `Text` para ver productos.

![Agregar un HyperLinkField a las categorías GridView](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Ilustración 6**: agregar un HyperLinkField a la `Categories` GridView

Después de crear el origen ObjectDataSource y personalizar los campos de GridView, los dos controles declarativos de marcado tendrán un aspecto similar al siguiente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

La figura 7 muestra `Default.aspx` cuando se ve a través de un explorador. Al hacer clic en un vínculo categoría, ver productos le llevará a `ProductsByCategory.aspx?CategoryID=categoryID`, que crearemos en el paso 3.

[![cada categoría aparece junto con un vínculo Ver productos](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Figura 7**: cada categoría aparece junto con un vínculo Ver productos ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Paso 3: enumerar los productos de la categoría seleccionada

Abra la página `ProductsByCategory.aspx` y agregue un control GridView que le asigne un nombre `ProductsByCategory`. Desde su etiqueta inteligente, enlace GridView a un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource`. Configure ObjectDataSource para usar el método `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` y establezca las listas desplegables en (ninguno) en las pestañas actualizar, insertar y eliminar.

[![usar el método ProductsBLL de la clase GetProductsByCategoryID (categoryID)](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Figura 8**: uso del método `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))

El paso final del Asistente para configurar orígenes de datos solicita un origen de parámetro para *CategoryID*. Puesto que esta información se pasa a través del campo QueryString `CategoryID`, seleccione QueryString en la lista desplegable y escriba CategoryID en el cuadro de texto QueryStringField como se muestra en la figura 9. Haga clic en Finalizar para completar el asistente.

[![usar el campo QueryString QueryString para el parámetro categoryID](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Figura 9**: Use el campo `CategoryID` QueryString para el parámetro *CategoryID* ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))

Después de completar el asistente, Visual Studio agregará los valores de BoundFields y CheckBoxField correspondientes a GridView para los campos de datos del producto. Quite todos los `ProductName`, `UnitPrice`y `SupplierName` BoundFields. Personalice estas tres propiedades de `HeaderText` de BoundFields para leer productos, precios y proveedores, respectivamente. Dé formato a la `UnitPrice` BoundField como moneda.

A continuación, agregue un HyperLinkField y muévalo a la posición situada más a la izquierda. Establezca su propiedad `Text` en ver detalles, su propiedad `DataNavigateUrlFields` en `ProductID`y su propiedad `DataNavigateUrlFormatString` en `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Agregar un HyperLinkField de detalles de vista que apunte a ProductDetails. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Figura 10**: agregar un HyperLinkField de detalles de vista que apunte a `ProductDetails.aspx`

Después de llevar a cabo estas personalizaciones, el marcado declarativo de GridView y ObjectDataSource es similar al siguiente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Vuelva a ver `Default.aspx` a través de un explorador y haga clic en el vínculo Ver productos para bebidas. Esto le llevará a `ProductsByCategory.aspx?CategoryID=1`, que muestra los nombres, precios y proveedores de los productos de la base de datos Northwind que pertenecen a la categoría Beverages (consulte la figura 11). No dude en mejorar aún más esta página para incluir un vínculo para devolver a los usuarios a la página de lista de categorías (`Default.aspx`) y un control DetailsView o FormView que muestre el nombre y la descripción de la categoría seleccionada.

[![se muestran los nombres, precios y proveedores de las bebidas](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Figura 11**: se muestran los nombres, precios y proveedores de las bebidas ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Paso 4: mostrar los detalles de un producto

En la página final, `ProductDetails.aspx`, se muestran los detalles de los productos seleccionados. Abra `ProductDetails.aspx` y arrastre un DetailsView desde el cuadro de herramientas hasta el diseñador. Establezca la propiedad DetailsView s `ID` en `ProductInfo` y borre sus valores de propiedad `Height` y `Width`. A partir de su etiqueta inteligente, enlace DetailsView a un nuevo ObjectDataSource denominado `ProductDataSource`, configurando ObjectDataSource para extraer sus datos del método de `GetProductByProductID(productID)` `ProductsBLL` Class s. Al igual que con las páginas web anteriores creadas en los pasos 2 y 3, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![configurar ObjectDataSource para usar el método GetProductByProductID (productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Figura 12**: configuración de ObjectDataSource para usar el método `GetProductByProductID(productID)` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))

El último paso del Asistente para configurar orígenes de datos solicita el origen del parámetro *ProductID* . Dado que estos datos se incluyen en el campo QueryString `ProductID`, establezca la lista desplegable en QueryString y el cuadro de texto QueryStringField en ProductID. Por último, haga clic en el botón Finalizar para completar el asistente.

[![configurar el parámetro productID para extraer su valor del campo de QueryString de ProductID](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Figura 13**: configure el parámetro *ProductID* para extraer su valor del campo `ProductID` QueryString ([haga clic para ver la imagen a tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))

Después de completar el Asistente para configurar el origen de datos, Visual Studio creará BoundFields y CheckBoxField correspondientes en DetailsView para los campos de datos del producto. Quite los `ProductID`, `SupplierID`y `CategoryID` BoundFields y configure los campos restantes como considere oportuno. Después de una serie de configuraciones estéticas, el marcado declarativo de DetailsView y ObjectDataSource es similar al siguiente:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Para probar esta página, vuelva a `Default.aspx` y haga clic en Ver productos para la categoría bebidas. En la lista de productos de bebidas, haga clic en el vínculo ver detalles de la Tea de Chai. Esto le llevará a `ProductDetails.aspx?ProductID=1`, que muestra los detalles de las Chai (véase la figura 14).

[![se muestra la información sobre el proveedor, la categoría, el precio y otros datos de las Chai](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Figura 14**: se muestra el proveedor de té de Chai, la categoría, el precio y otra información ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Paso 5: Descripción del funcionamiento interno de un proveedor del mapa del sitio

El mapa del sitio se representa en la memoria del servidor web como una colección de instancias de `SiteMapNode` que forman una jerarquía. Debe haber exactamente una raíz, todos los nodos que no sean raíz deben tener exactamente un nodo primario y todos los nodos pueden tener un número arbitrario de elementos secundarios. Cada objeto `SiteMapNode` representa una sección en la estructura del sitio Web. Normalmente, estas secciones tienen una página web correspondiente. Por consiguiente, la [clase`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) tiene propiedades como `Title`, `Url`y `Description`, que proporcionan información para la sección que representa el `SiteMapNode`. También hay una propiedad `Key` que identifica de forma única cada `SiteMapNode` en la jerarquía, así como las propiedades que se usan para establecer esta jerarquía `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`, etc.

En la figura 15 se muestra la estructura del mapa del sitio general de la figura 1, pero con los detalles de la implementación esbozados con más detalle.

[![cada SiteMapNode tiene propiedades como título, dirección URL, clave, etc.](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Figura 15**: cada `SiteMapNode` tiene propiedades como `Title`, `Url`, `Key`, etc. ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))

El mapa del sitio es accesible a través de la [clase`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) en el [espacio de nombres`System.Web`](https://msdn.microsoft.com/library/system.web.aspx). Esta propiedad de clase s `RootNode` devuelve la instancia de `SiteMapNode` raíz del mapa del sitio; `CurrentNode` devuelve el `SiteMapNode` cuya propiedad `Url` coincide con la dirección URL de la página solicitada actualmente. Esta clase la usan internamente los controles Web de navegación de ASP.NET 2,0 s.

Cuando se tiene acceso a las propiedades de la clase `SiteMap`, se debe serializar la estructura del mapa del sitio de un medio persistente en la memoria. Sin embargo, la lógica de serialización del mapa del sitio no está codificada de forma rígida en la clase `SiteMap`. En su lugar, en tiempo de ejecución, la clase `SiteMap` determina qué *proveedor* de mapa del sitio se va a usar para la serialización. De forma predeterminada, se utiliza la [clase`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) , que lee la estructura del mapa del sitio desde un archivo XML con el formato correcto. Sin embargo, con un poco de trabajo podemos crear nuestro propio proveedor de mapa del sitio personalizado.

Todos los proveedores del mapa del sitio deben derivarse de la [clase`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), que incluye los métodos y las propiedades esenciales necesarios para los proveedores del mapa del sitio, pero omite muchos de los detalles de la implementación. Una segunda clase, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), extiende la `SiteMapProvider` clase y contiene una implementación más sólida de la funcionalidad necesaria. Internamente, el `StaticSiteMapProvider` almacena las instancias de `SiteMapNode` del mapa del sitio en un `Hashtable` y proporciona métodos como `AddNode(child, parent)`, `RemoveNode(siteMapNode),` y `Clear()` que agregan y quitan `SiteMapNode` s al `Hashtable`interno. La clase `XmlSiteMapProvider` se deriva de la clase `StaticSiteMapProvider`.

Al crear un proveedor de mapa del sitio personalizado que extienda `StaticSiteMapProvider`, hay dos métodos abstractos que se deben invalidar: [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) y [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, como su nombre implica, es responsable de cargar la estructura del mapa del sitio desde el almacenamiento persistente y de construirla en la memoria. `GetRootNodeCore` devuelve el nodo raíz del mapa del sitio.

Antes de que una aplicación Web pueda usar un proveedor de mapa del sitio, debe registrarse en la configuración de la aplicación. De forma predeterminada, la clase `XmlSiteMapProvider` se registra con el nombre `AspNetXmlSiteMapProvider`. Para registrar proveedores adicionales del mapa del sitio, agregue el marcado siguiente a `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

El valor *Name* asigna un nombre legible al proveedor mientras que *Type* especifica el nombre de tipo completo del proveedor del mapa del sitio. Exploraremos los valores concretos de los valores de *nombre* y *tipo* en el paso 7, después de crear nuestro proveedor de mapa del sitio personalizado.

Se crea una instancia de la clase del proveedor del mapa del sitio la primera vez que se tiene acceso a ella desde la clase `SiteMap` y permanece en memoria durante la vigencia de la aplicación Web. Puesto que solo hay una instancia del proveedor del mapa del sitio que se puede invocar desde varios visitantes de sitios web simultáneos, es imperativo que los métodos del proveedor sean *seguros para subprocesos*.

Por motivos de rendimiento y escalabilidad, es importante que almacenemos en caché la estructura del mapa del sitio en memoria y devuelvan esta estructura almacenada en caché en lugar de volver a crearla cada vez que se invoque el método de `BuildSiteMap`. se puede llamar a `BuildSiteMap` varias veces por cada solicitud de página por usuario, dependiendo de los controles de navegación en uso en la página y la profundidad de la estructura del mapa del sitio. En cualquier caso, si no se almacena en caché la estructura del mapa del sitio en `BuildSiteMap` cada vez que se invoca, es necesario volver a recuperar la información del producto y de la categoría de la arquitectura (lo que daría lugar a una consulta a la base de datos). Como hemos comentado en los tutoriales de Caching anteriores, los datos almacenados en caché pueden quedar obsoletos. Para combatir esto, podemos usar Expires basados en la memoria caché de tiempo o SQL.

> [!NOTE]
> Un proveedor del mapa del sitio puede reemplazar opcionalmente el [método`Initialize`](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` se invoca cuando se crea una instancia del proveedor del mapa del sitio y se pasan los atributos personalizados asignados al proveedor en `Web.config` en el elemento `<add>` como: `<add name="name" type="type" customAttribute="value" />`. Resulta útil si desea permitir que un desarrollador de páginas especifique varios valores relacionados con el proveedor del mapa del sitio sin tener que modificar el código del proveedor. Por ejemplo, si se leían los datos de la categoría y los productos directamente desde la base de datos en lugar de hacerlo a través de la arquitectura, es probable que desee permitir que el desarrollador de la página especifique la cadena de conexión de base de datos a través de `Web.config` en lugar de usar un valor codificado de forma rígida en el código del proveedor. El proveedor del mapa del sitio personalizado que crearemos en el paso 6 no invalida este método `Initialize`. Para obtener un ejemplo de cómo usar el método `Initialize`, consulte [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [almacenamiento de mapas de sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) artículo.

## <a name="step-6-creating-the-custom-site-map-provider"></a>Paso 6: creación del proveedor del mapa del sitio personalizado

Para crear un proveedor de mapa del sitio personalizado que genere el mapa del sitio a partir de las categorías y los productos de la base de datos Northwind, es necesario crear una clase que extienda `StaticSiteMapProvider`. En el paso 1, se le pidió agregar una carpeta `CustomProviders` en la carpeta `App_Code`: agregar una nueva clase a esta carpeta denominada `NorthwindSiteMapProvider`. Agregue el código siguiente a la clase `NorthwindSiteMapProvider`:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Comencemos con la exploración de esta clase `BuildSiteMap` método, que comienza con una [instrucción`lock`](https://msdn.microsoft.com/library/c5kehkcz.aspx). La instrucción `lock` solo permite que un subproceso cada vez escriba, con lo que se serializa el acceso a su código y se evita que dos subprocesos simultáneos se recorran en otro pies s.

La variable de `SiteMapNode` de nivel de clase `root` se utiliza para almacenar en caché la estructura del mapa del sitio. Cuando el mapa del sitio se crea por primera vez, o por primera vez después de que se hayan modificado los datos subyacentes, se `Nothing`rá `root` y se construirá la estructura del mapa del sitio. El nodo raíz del mapa del sitio se asigna a `root` durante el proceso de construcción para que la próxima vez que se llame a este método, `root` no se `Nothing`. Por consiguiente, siempre y cuando `root` no sea `Nothing` la estructura del mapa del sitio se devolverá al llamador sin tener que volver a crearlo.

Si la raíz es `Nothing`, se crea la estructura del mapa del sitio a partir de la información del producto y de la categoría. El mapa del sitio se crea creando las instancias de `SiteMapNode` y, a continuación, formando la jerarquía mediante llamadas al método `AddNode` de la clase `StaticSiteMapProvider`. `AddNode` realiza la contabilidad interna, almacenando las instancias de `SiteMapNode` ordenadas en un `Hashtable`. Antes de empezar a construir la jerarquía, comenzamos llamando al método `Clear`, que borra los elementos del `Hashtable`interno. A continuación, el método de `GetProducts` de `ProductsBLL` Class y el `ProductsDataTable` resultante se almacenan en variables locales.

La construcción del mapa del sitio empieza por crear el nodo raíz y asignarlo a `root`. La sobrecarga del [constructor`SiteMapNode` s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) que se usa aquí y a lo largo de este `BuildSiteMap` se pasa a la siguiente información:

- Referencia al proveedor del mapa del sitio (`Me`).
- `Key``SiteMapNode` s. Este valor obligatorio debe ser único para cada `SiteMapNode`.
- `Url``SiteMapNode` s. `Url` es opcional, pero si se proporciona, cada `SiteMapNode` `Url` valor debe ser único.
- `SiteMapNode` s `Title`, que es necesario.

La llamada al método `AddNode(root)` agrega el `root` de `SiteMapNode` al mapa del sitio como raíz. A continuación, se enumeran todos los `ProductRow` en el `ProductsDataTable`. Si ya existe un `SiteMapNode` para la categoría de producto actual, se hace referencia a él. De lo contrario, se crea un nuevo `SiteMapNode` para la categoría y se agrega como un elemento secundario del `SiteMapNode``root` a través de la llamada al método `AddNode(categoryNode, root)`. Una vez que se ha encontrado o creado la categoría adecuada `SiteMapNode` nodo, se crea un `SiteMapNode` para el producto actual y se agrega como un elemento secundario de la categoría `SiteMapNode` a través de `AddNode(productNode, categoryNode)`. Tenga en cuenta que el valor de la propiedad Category `SiteMapNode` s `Url` es `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` mientras que la propiedad Product `SiteMapNode` s `Url` está asignada `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Los productos que tienen un valor de `NULL` de base de datos para sus `CategoryID` se agrupan en una categoría `SiteMapNode` cuya propiedad `Title` está establecida en None y cuya propiedad `Url` está establecida en una cadena vacía. Decidí establecer `Url` en una cadena vacía, ya que el método `ProductBLL` clase s `GetProductsByCategory(categoryID)` actualmente no tiene la capacidad de devolver solo los productos con un valor de `CategoryID` `NULL`. Además, quería demostrar cómo los controles de navegación representan un `SiteMapNode` que carece de un valor para su propiedad `Url`. Le recomiendo ampliar este tutorial para que la propiedad None `SiteMapNode` s `Url` apunte a `ProductsByCategory.aspx`, pero solo muestra los productos con `NULL` valores `CategoryID`.

Después de construir el mapa del sitio, se agrega un objeto arbitrario a la memoria caché de datos utilizando una dependencia de caché de SQL en las tablas `Categories` y `Products` a través de un objeto `AggregateCacheDependency`. Hemos explorado el uso de las dependencias de caché de SQL en el tutorial anterior, *con las dependencias de caché de SQL*. Sin embargo, el proveedor del mapa del sitio personalizado usa una sobrecarga del método `Insert` de la caché de datos que se va a explorar todavía. Esta sobrecarga acepta como su parámetro de entrada final un delegado al que se llama cuando el objeto se quita de la memoria caché. En concreto, pasamos un nuevo [delegado`CacheItemRemovedCallback`](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) que apunta al método `OnSiteMapChanged` definido más abajo en la clase `NorthwindSiteMapProvider`.

> [!NOTE]
> La representación en memoria del mapa del sitio se almacena en la memoria caché a través de la variable de nivel de clase `root`. Dado que solo hay una instancia de la clase del proveedor del mapa del sitio personalizado y que la instancia se comparte entre todos los subprocesos de la aplicación Web, esta variable de clase actúa como una memoria caché. El método `BuildSiteMap` también utiliza la memoria caché de datos, pero solo como un medio para recibir notificaciones cuando cambian los datos de la base de datos subyacente en las tablas `Categories` o `Products`. Tenga en cuenta que el valor colocado en la memoria caché de datos es solo la fecha y hora actuales. Los datos reales del mapa del sitio *no* se colocan en la memoria caché de datos.

El método `BuildSiteMap` finaliza devolviendo el nodo raíz del mapa del sitio.

Los métodos restantes son bastante sencillos. `GetRootNodeCore` es responsable de devolver el nodo raíz. Puesto que `BuildSiteMap` devuelve la raíz, `GetRootNodeCore` simplemente devuelve el valor devuelto de `BuildSiteMap` s. El método `OnSiteMapChanged` establece `root` de nuevo en `Nothing` cuando se quita el elemento de la memoria caché. Con la raíz establecida en `Nothing`, la próxima vez que se invoque `BuildSiteMap`, se volverá a generar la estructura del mapa del sitio. Por último, la propiedad `CachedDate` devuelve el valor de fecha y hora almacenado en la memoria caché de datos, si este valor existe. El desarrollador de páginas puede usar esta propiedad para determinar cuándo se han almacenado en caché los datos del mapa del sitio por última vez.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Paso 7: registrar el`NorthwindSiteMapProvider`

Para que nuestra aplicación web use el proveedor del mapa del sitio de `NorthwindSiteMapProvider` creado en el paso 6, es necesario registrarlo en la sección `<siteMap>` de `Web.config`. En concreto, agregue el siguiente marcado en el elemento `<system.web>` de `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Este marcado hace dos cosas: en primer lugar, indica que el `AspNetXmlSiteMapProvider` integrado es el proveedor del mapa del sitio predeterminado; en segundo lugar, registra el proveedor del mapa del sitio personalizado creado en el paso 6 con el nombre descriptivo Northwind.

> [!NOTE]
> En el caso de los proveedores del mapa del sitio ubicados en la carpeta Application s `App_Code`, el valor del atributo `type` es simplemente el nombre de clase. Como alternativa, el proveedor del mapa del sitio personalizado podría haberse creado en un proyecto de biblioteca de clases independiente con el ensamblado compilado colocado en el directorio de `/Bin` de la aplicación Web. En ese caso, el valor del atributo `type` sería el *espacio de nombres*. *ClassName*, *AssemblyName* .

Después de actualizar `Web.config`, dedique un momento a ver cualquier página de los tutoriales en un explorador. Tenga en cuenta que la interfaz de navegación de la izquierda sigue mostrando las secciones y los tutoriales definidos en `Web.sitemap`. Esto se debe a que hemos dejado `AspNetXmlSiteMapProvider` como proveedor predeterminado. Con el fin de crear un elemento de interfaz de usuario de navegación que use el `NorthwindSiteMapProvider`, deberá especificar explícitamente que se debe usar el proveedor de mapa del sitio de Northwind. Veremos cómo hacerlo en el paso 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Paso 8: Mostrar información del mapa del sitio mediante el proveedor del mapa del sitio personalizado

Con el proveedor del mapa del sitio personalizado creado y registrado en `Web.config`, se vuelve a preparar para agregar controles de navegación a las páginas `Default.aspx`, `ProductsByCategory.aspx`y `ProductDetails.aspx` de la carpeta `SiteMapProvider`. Comience abriendo la página de `Default.aspx` y arrastre una `SiteMapPath` desde el cuadro de herramientas hasta el diseñador. El control SiteMapPath se encuentra en la sección de navegación del cuadro de herramientas.

[![agregar un SiteMapPath a default. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Figura 16**: agregar un SiteMapPath a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))

El control SiteMapPath muestra una ruta de navegación, que indica la ubicación de la página actual en el mapa del sitio. Se ha agregado un SiteMapPath a la parte superior de la página maestra en el tutorial de navegación por el *sitio y las páginas maestras* .

Tómese un momento para ver esta página a través de un explorador. El SiteMapPath agregado en la figura 16 utiliza el proveedor del mapa del sitio predeterminado, extrayendo sus datos de `Web.sitemap`. Por lo tanto, la ruta de navegación muestra Inicio &gt; personalizar el mapa del sitio, al igual que la ruta de navegación en la esquina superior derecha.

[![la ruta de navegación utiliza el proveedor del mapa del sitio predeterminado](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Figura 17**: la ruta de navegación utiliza el proveedor del mapa del sitio predeterminado ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))

Para que el SiteMapPath agregado en la figura 16 use el proveedor del mapa del sitio personalizado que creamos en el paso 6, establezca su [propiedad`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) en Northwind, el nombre que asignamos a la `NorthwindSiteMapProvider` en `Web.config`. Desafortunadamente, el diseñador sigue usando el proveedor del mapa del sitio predeterminado, pero si visita la página a través de un explorador después de hacer este cambio de propiedad, verá que la ruta de navegación ahora usa el proveedor del mapa del sitio personalizado.

[![la ruta de navegación ahora utiliza el proveedor del mapa del sitio personalizado NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Figura 18**: ahora, la ruta de navegación utiliza el proveedor del mapa del sitio personalizado `NorthwindSiteMapProvider` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))

El control SiteMapPath muestra una interfaz de usuario más funcional en las páginas `ProductsByCategory.aspx` y `ProductDetails.aspx`. Agregue un SiteMapPath a estas páginas y establezca la propiedad `SiteMapProvider` en Northwind. En `Default.aspx` haga clic en el vínculo Ver productos para bebidas y, a continuación, en el vínculo ver detalles de la té de Chai. Como se muestra en la figura 19, la ruta de navegación incluye la sección del mapa del sitio actual (té de Chai) y sus antecesores: bebidas y todas las categorías.

[![la ruta de navegación ahora utiliza el proveedor del mapa del sitio personalizado NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Figura 19**: la ruta de navegación ahora usa el proveedor del mapa del sitio personalizado `NorthwindSiteMapProvider` ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))

Se pueden usar otros elementos de la interfaz de usuario de navegación además de SiteMapPath, como los controles menu y TreeView. Las páginas `Default.aspx`, `ProductsByCategory.aspx`y `ProductDetails.aspx` de la descarga para este tutorial, por ejemplo, todos los controles de menú de inclusión (vea la figura 20). Consulte [examen de las características de navegación del sitio de ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) y la sección uso de los [controles de navegación de sitios](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) de las guías de [Inicio rápido de ASP.net 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/) para obtener información más detallada sobre los controles de navegación y el sistema del mapa del sitio en ASP.net 2,0.

[![el control de menú muestra cada una de las categorías y productos](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Figura 20**: el control de menú muestra cada una de las categorías y los productos ([haga clic para ver la imagen de tamaño completo](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))

Como se mencionó anteriormente en este tutorial, se puede obtener acceso a la estructura del mapa del sitio mediante programación a través de la clase `SiteMap`. El código siguiente devuelve el `SiteMapNode` raíz del proveedor predeterminado:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Como el `AspNetXmlSiteMapProvider` es el proveedor predeterminado para nuestra aplicación, el código anterior devolvería el nodo raíz definido en `Web.sitemap`. Para hacer referencia a un proveedor del mapa del sitio que no sea el predeterminado, use la propiedad `SiteMap` clase s [`Providers`](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) de la siguiente manera:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Donde *Name* es el nombre del proveedor del mapa del sitio personalizado (Northwind, para nuestra aplicación web).

Para tener acceso a un miembro específico de un proveedor del mapa del sitio, utilice `SiteMap.Providers["name"]` para recuperar la instancia del proveedor y, a continuación, conviértala al tipo adecuado. Por ejemplo, para mostrar la propiedad `NorthwindSiteMapProvider` s `CachedDate` en una página de ASP.NET, use el código siguiente:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Asegúrese de probar la característica de dependencia de caché de SQL. Después de visitar las páginas `Default.aspx`, `ProductsByCategory.aspx`y `ProductDetails.aspx`, vaya a uno de los tutoriales de la sección edición, inserción y eliminación y edite el nombre de una categoría o producto. A continuación, vuelva a una de las páginas de la carpeta `SiteMapProvider`. Suponiendo que haya transcurrido el tiempo suficiente para que el mecanismo de sondeo tenga en cuenta el cambio en la base de datos subyacente, el mapa del sitio debe actualizarse para mostrar el nuevo nombre de producto o categoría.

## <a name="summary"></a>Resumen

Las características del mapa del sitio de ASP.NET 2,0 s incluyen una clase `SiteMap`, una serie de controles Web de navegación integrados y un proveedor del mapa del sitio predeterminado que espera que la información del mapa del sitio se conserve en un archivo XML. Para usar la información del mapa del sitio de otro origen, como una base de datos, la arquitectura de la aplicación o un servicio Web remoto, es necesario crear un proveedor del mapa del sitio personalizado. Esto implica la creación de una clase que deriva, directa o indirectamente, de la clase `SiteMapProvider`.

En este tutorial, vimos cómo crear un proveedor de mapa del sitio personalizado basado en el mapa del sitio en la información del producto y de la categoría que se ha quitado de la arquitectura de la aplicación. Nuestro proveedor amplió la `StaticSiteMapProvider` clase e informaba de la creación de un `BuildSiteMap` método que recuperó los datos, construyó la jerarquía del mapa del sitio y almacenó en caché la estructura resultante en una variable de nivel de clase. Usamos una dependencia de caché de SQL con una función de devolución de llamada para invalidar la estructura almacenada en caché cuando se modifican los datos subyacentes `Categories` o `Products`.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Almacenar mapas de sitio en SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) y [el proveedor del mapa del sitio de SQL al que está esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Un vistazo al modelo de proveedor de ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [El kit de herramientas del proveedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examen de las características de navegación del sitio de ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Dave Gardner, Zack Jones, Teresa Murphy y Bernadette Leigh. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](building-a-custom-database-driven-site-map-provider-cs.md)
