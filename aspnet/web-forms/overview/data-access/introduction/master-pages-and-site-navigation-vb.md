---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: Páginas maestras y navegación del sitio (VB) | Microsoft Docs
author: rick-anderson
description: Una característica común de los sitios web descriptivos es que tienen un diseño de página coherente y un esquema de navegación. En este tutorial se examina cómo...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4a2b5ba8c1781f1194f951a44661a8f7dd095f41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78426031"
---
# <a name="master-pages-and-site-navigation-vb"></a>Páginas maestras y navegación del sitio (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe) de la aplicación de ejemplo o [descarga de PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> Una característica común de los sitios web descriptivos es que tienen un diseño de página coherente y un esquema de navegación. En este tutorial se describe cómo puede crear una apariencia coherente en todas las páginas que se pueden actualizar fácilmente.

## <a name="introduction"></a>Introducción

Una característica común de los sitios web descriptivos es que tienen un diseño de página coherente y un esquema de navegación. ASP.NET 2,0 presenta dos nuevas características que simplifican considerablemente la implementación de un esquema de navegación y un diseño de página en todo el sitio: páginas maestras y navegación del sitio. Las páginas maestras permiten a los desarrolladores crear una plantilla para todo el sitio con regiones modificables designadas. A continuación, esta plantilla se puede aplicar a las páginas de ASP.NET en el sitio. Estas páginas de ASP.NET solo necesitan proporcionar contenido para las regiones modificables especificadas de la página maestra. el resto del marcado en la página maestra es idéntico en todas las páginas de ASP.NET que usan la página maestra. Este modelo permite a los desarrolladores definir y centralizar un diseño de página para todo el sitio, lo que facilita la creación de una apariencia coherente en todas las páginas que se pueden actualizar fácilmente.

El [sistema de navegación del sitio](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) proporciona un mecanismo para que los desarrolladores de páginas puedan definir un mapa del sitio y una API para ese mapa del sitio que se consultará mediante programación. La nueva web de navegación controla el menú, TreeView y SiteMapPath facilitan la representación de todo o parte del mapa del sitio en un elemento de la interfaz de usuario de navegación común. Vamos a usar el proveedor de navegación del sitio predeterminado, lo que significa que nuestro mapa del sitio se definirá en un archivo con formato XML.

Para ilustrar estos conceptos y hacer que nuestro sitio web de tutoriales sea más fácil de usar, dedique esta lección a definir un diseño de página para todo el sitio, implementar un mapa del sitio y agregar la interfaz de usuario de navegación. Al final de este tutorial tendremos un diseño de sitio web pulido para crear las páginas web del tutorial.

[![el resultado final de este tutorial](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**Figura 1**: resultado final de este tutorial ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Paso 1: crear la página maestra

El primer paso es crear la página maestra para el sitio. Ahora nuestro sitio web solo está formado por el conjunto de datos con tipo (`Northwind.xsd`, en la carpeta `App_Code`), las clases BLL (`ProductsBLL.vb`, `CategoriesBLL.vb`, etc., todas en la carpeta `App_Code`), la base de datos (`NORTHWND.MDF`, en la carpeta `App_Data`), el archivo de configuración (`Web.config`) y un archivo de hoja de estilos CSS (`Styles.css`). He limpiado las páginas y los archivos que mostraban el uso de la capa DAL y BLL de los dos primeros tutoriales, ya que vamos a examinar estos ejemplos con mayor detalle en los tutoriales futuros.

![Archivos en nuestro proyecto](master-pages-and-site-navigation-vb/_static/image4.png)

**Figura 2**: los archivos del proyecto

Para crear una página maestra, haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y elija Agregar nuevo elemento. A continuación, seleccione el tipo de página maestra en la lista de plantillas y asígnele el nombre `Site.master`.

[![agregar una nueva página maestra al sitio web](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**Figura 3**: agregar una nueva página maestra al sitio web ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image7.png))

Defina el diseño de página para todo el sitio aquí en la página maestra. Puede usar el Vista de diseño y agregar el diseño o los controles Web que necesite, o puede Agregar manualmente el marcado de forma manual en la vista Código fuente. En mi página maestra utilizo [hojas de estilos en cascada](http://www.w3schools.com/css/default.asp) para colocar y aplicar estilos a la configuración de CSS definida en el `Style.css`de archivos externos. Aunque no se puede determinar a partir del marcado que se muestra a continuación, las reglas de CSS se definen de forma que el contenido del `<div>`de navegación está absolutamente posicionado para que aparezca en la parte izquierda y tenga un ancho fijo de 200 píxeles.

Site. Master

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

Una página maestra define el diseño de página estático y las regiones que pueden editar las páginas de ASP.NET que usan la página maestra. Estas regiones modificables de contenido se indican mediante el control ContentPlaceHolder, que puede verse en el `<div>`de contenido. Nuestra página maestra tiene un solo ContentPlaceHolder (`MainContent`), pero la página maestra puede tener varios ContentPlaceHolders.

Con el marcado especificado anteriormente, al cambiar a la Vista de diseño se muestra el diseño de la página maestra. Cualquier página de ASP.NET que use esta página maestra tendrá este diseño uniforme, con la capacidad de especificar el marcado para la región de `MainContent`.

[![la página maestra, cuando se ve a través de la vista de diseño](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**Figura 4**: la página maestra, cuando se ve a través de la vista de diseño ([haga clic para ver la imagen a tamaño completo](master-pages-and-site-navigation-vb/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Paso 2: agregar una página principal al sitio web

Una vez definida la página maestra, estamos listos para agregar las páginas de ASP.NET para el sitio Web. Comencemos agregando `Default.aspx`, la Página principal del sitio Web. Haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y elija Agregar nuevo elemento. Elija la opción formulario web de la lista de plantillas y asigne al archivo el nombre `Default.aspx`. Además, active la casilla "seleccionar página maestra".

[![agregar un formulario web nuevo y activar la casilla seleccionar página maestra](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**Figura 5**: agregar un nuevo formulario Web Forms, activar la casilla seleccionar página maestra ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image13.png))

Después de hacer clic en el botón Aceptar, se le pedirá que elija la página maestra que debe usar esta nueva página de ASP.NET. Aunque puede tener varias páginas maestras en el proyecto, solo tenemos una.

[![elegir la página maestra que debe usar esta página de ASP.NET](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**Figura 6**: elegir la página maestra que debe usar esta página ASP.net ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image16.png))

Después de seleccionar la página maestra, las nuevas páginas de ASP.NET contendrán el siguiente marcado:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

En la Directiva de `@Page` hay una referencia al archivo de página maestra utilizado (`MasterPageFile="~/Site.master"`) y el marcado de la página ASP.NET contiene un control de contenido para cada uno de los controles ContentPlaceHolder definidos en la página maestra, con la `ContentPlaceHolderID` del control que asigna el control de contenido a un ContentPlaceHolder específico. El control de contenido es donde se coloca el marcado que se desea que aparezca en el ContentPlaceHolder correspondiente. Establezca el atributo `Title` de la Directiva de `@Page` en Home y agregue contenido de bienvenida al control de contenido:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

El atributo `Title` de la Directiva `@Page` nos permite establecer el título de la página en la página ASP.NET, aunque el elemento `<title>` se define en la página maestra. También podemos establecer el título mediante programación, usando `Page.Title`. Tenga en cuenta también que las referencias de la página maestra a hojas de estilo (como `Style.css`) se actualizan automáticamente para que funcionen en cualquier página de ASP.NET, independientemente del directorio en el que se encuentre la página ASP.NET relativa a la página maestra.

Al cambiar a la Vista de diseño podemos ver el aspecto que tendrá la página en un explorador. Tenga en cuenta que en la Vista de diseño de la página ASP.NET que solo las regiones editables de contenido son editables, el marcado no ContentPlaceHolder definido en la página maestra está atenuado.

[![la vista de diseño de la página ASP.NET muestra las regiones modificables y no modificables](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**Figura 7**: en la vista de diseño de la página ASP.net se muestran las regiones modificables y no modificables ([haga clic para ver la imagen a tamaño completo](master-pages-and-site-navigation-vb/_static/image19.png))

Cuando un explorador visita la página `Default.aspx`, el motor ASP.NET combina automáticamente el contenido de la página maestra de la página y el ASP. El contenido de la red y representa el contenido combinado en el HTML final que se envía al explorador que lo solicita. Cuando se actualiza el contenido de la página maestra, se volverán a combinar el contenido de todas las páginas de ASP.NET que usen esta página maestra con el nuevo contenido de la página maestra la próxima vez que se soliciten. En Resumen, el modelo de página maestra permite que se defina una sola plantilla de diseño de página (la página maestra) cuyos cambios se reflejan inmediatamente en todo el sitio.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Agregar páginas ASP.NET adicionales al sitio web

Dedique un momento a agregar códigos auxiliares de página de ASP.NET adicionales al sitio que finalmente retendrá las distintas demostraciones de informes. Habrá más de 35 demostraciones en total, por lo que en lugar de crear todas las páginas de código auxiliar, vamos a crear solo las primeras. Dado que también habrá muchas categorías de demostraciones, para administrar mejor las demostraciones, agregue una carpeta para las categorías. Agregue las siguientes tres carpetas por ahora:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Por último, agregue nuevos archivos como se muestra en la Explorador de soluciones en la figura 8. Al agregar cada archivo, recuerde activar la casilla "seleccionar página maestra".

![Agregue los siguientes archivos](master-pages-and-site-navigation-vb/_static/image20.png)

**Figura 8**: incorporación de los siguientes archivos

## <a name="step-2-creating-a-site-map"></a>Paso 2: creación de un mapa del sitio

Uno de los desafíos de administrar un sitio web compuesto por más de una serie de páginas es ofrecer una forma sencilla de que los visitantes naveguen por el sitio. Para empezar, debe definirse la estructura de navegación del sitio. A continuación, esta estructura debe traducirse en elementos de interfaz de usuario navegables, como menús o rutas de navegación. Por último, todo el proceso debe mantenerse y actualizarse a medida que se agregan nuevas páginas al sitio y se quitan las existentes. Antes de ASP.NET 2,0, los desarrolladores eran los suyos propios para crear la estructura de navegación del sitio, mantenerla y traducirla en elementos de la interfaz de usuario navegables. Sin embargo, con ASP.NET 2,0, los desarrolladores pueden emplear el sistema de navegación del sitio integrado muy flexible.

El sistema de navegación del sitio de ASP.NET 2,0 proporciona un medio para que un desarrollador defina un mapa del sitio y, a continuación, acceda a esta información a través de una API de programación. ASP.NET se suministra con un proveedor del mapa del sitio que espera que los datos del mapa del sitio se almacenen en un archivo XML con un formato determinado. Sin embargo, dado que el sistema de navegación del sitio se basa en el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , se puede extender para admitir formas alternativas de serialización de la información del mapa del sitio. Artículo de Jeff Prosise, [el proveedor del mapa del sitio de SQL que ha estado esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) muestra cómo crear un proveedor del mapa del sitio que almacena el mapa del sitio en una base de datos de SQL Server. otra opción consiste en crear [un proveedor de mapa del sitio basado en la estructura del sistema de archivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

En este tutorial, sin embargo, vamos a usar el proveedor del mapa del sitio predeterminado que se incluye con ASP.NET 2,0. Para crear el mapa del sitio, simplemente haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones, elija Agregar nuevo elemento y elija la opción mapa del sitio. Deje el nombre como `Web.sitemap` y haga clic en el botón Agregar.

[![agregar un mapa del sitio al proyecto](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**Figura 9**: agregar un mapa del sitio al proyecto ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image23.png))

El archivo de mapa del sitio es un archivo XML. Tenga en cuenta que Visual Studio proporciona IntelliSense para la estructura del mapa del sitio. El archivo de mapa del sitio debe tener el nodo de `<siteMap>` como nodo raíz, que debe contener exactamente un elemento secundario `<siteMapNode>`. Ese primer elemento `<siteMapNode>` puede contener un número arbitrario de elementos de `<siteMapNode>` descendientes.

Defina el mapa del sitio para imitar la estructura del sistema de archivos. Es decir, agregue un elemento `<siteMapNode>` para cada una de las tres carpetas y elementos `<siteMapNode>` secundarios para cada una de las páginas de ASP.NET de esas carpetas, como:

Web. sitemap

[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

El mapa del sitio define la estructura de navegación del sitio web, que es una jerarquía que describe las distintas secciones del sitio. Cada `<siteMapNode>` elemento de `Web.sitemap` representa una sección en la estructura de navegación del sitio.

[![el mapa del sitio representa una estructura jerárquica de navegación](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**Figura 10**: el mapa del sitio representa una estructura jerárquica de navegación ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image26.png))

ASP.NET expone la estructura del mapa del sitio a través de la [clase Sitemap](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del .NET Framework. Esta clase tiene una propiedad `CurrentNode`, que devuelve información sobre la sección que el usuario visita actualmente; la propiedad `RootNode` devuelve la raíz del mapa del sitio (Inicio, en nuestro mapa del sitio). Las propiedades `CurrentNode` y `RootNode` devuelven instancias de [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) , que tienen propiedades como `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`, etc., que permiten que se represente la jerarquía del mapa del sitio.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Paso 3: mostrar un menú basado en el mapa del sitio

El acceso a los datos en ASP.NET 2,0 puede realizarse mediante programación, como en ASP.NET 1. x o mediante declaración, a través de los nuevos [controles de origen de datos](https://msdn.microsoft.com/library/ms227679.aspx). Hay varios controles de origen de datos integrados, como el control SqlDataSource, para tener acceso a los datos de la base de datos relacional, al control ObjectDataSource, para tener acceso a los datos de las clases y a otros. Puede incluso crear sus propios [controles de origen de datos personalizados](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Los controles de origen de datos actúan como un proxy entre la página de ASP.NET y los datos subyacentes. Para mostrar los datos recuperados de un control de origen de datos, normalmente agregaremos otro control Web a la página y lo enlazaremos al control de origen de datos. Para enlazar un control Web a un control de origen de datos, basta con establecer la propiedad `DataSourceID` del control Web en el valor de la propiedad `ID` del control de origen de datos.

Para ayudar a trabajar con los datos del mapa del sitio, ASP.NET incluye el control SiteMapDataSource, que nos permite enlazar un control Web con el mapa del sitio del sitio Web. Dos controles Web: la vista de árbol y el menú se utilizan normalmente para proporcionar una interfaz de usuario de navegación. Para enlazar los datos del mapa del sitio a uno de estos dos controles, basta con agregar un SiteMapDataSource a la página junto con un control TreeView o menu cuya propiedad `DataSourceID` esté establecida en consecuencia. Por ejemplo, podríamos agregar un control de menú a la página maestra con el siguiente marcado:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Para un mayor grado de control sobre el código HTML emitido, podemos enlazar el control SiteMapDataSource al control Repeater, como por ejemplo:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

El control SiteMapDataSource devuelve la jerarquía del mapa del sitio de un nivel a la vez, empezando por el nodo del mapa del sitio raíz (Inicio, en nuestro mapa del sitio), después por el siguiente nivel (informes básicos, filtrado de informes y formato personalizado), etc. Al enlazar el SiteMapDataSource a un repetidor, enumera el primer nivel devuelto y crea una instancia del `ItemTemplate` para cada instancia de `SiteMapNode` en ese primer nivel. Para tener acceso a una propiedad determinada de la `SiteMapNode`, podemos usar `Eval(propertyName)`, que es cómo obtenemos los `Url` de cada `SiteMapNode`y las propiedades de `Title` del control de hipervínculo.

En el ejemplo de Repeater anterior se representará el marcado siguiente:

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

Estos nodos del mapa del sitio (informes básicos, filtros de filtrado y formato personalizado) comprenden el *segundo* nivel del mapa del sitio que se está representando, no el primero. Esto se debe a que la propiedad `ShowStartingNode` de SiteMapDataSource está establecida en false, lo que hace que el SiteMapDataSource omita el nodo del mapa del sitio raíz y, en su lugar, comienza devolviendo el segundo nivel de la jerarquía del mapa del sitio.

Para mostrar los elementos secundarios de los informes básicos, los informes de filtrado y el formato personalizado `SiteMapNode` s, se puede agregar otro repetidor a la `ItemTemplate`del repetidor inicial. Este segundo repetidor se enlazará a la propiedad `ChildNodes` de la instancia de `SiteMapNode`, como se indica a continuación:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

Estos dos repetidores producen el siguiente marcado (se ha quitado algún marcado para mayor brevedad):

[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

Mediante el uso de los estilos CSS elegidos en el libro de [Rachel](http://www.rachelandrew.co.uk/)de [Anthology de CSS: 101 sugerencias esenciales, trucos &amp; hackers](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), los elementos `<ul>` y `<li>` tienen un estilo tal que el marcado produce el siguiente resultado visual:

![Menú compuesto por dos repetidores y algunos CSS](master-pages-and-site-navigation-vb/_static/image27.png)

**Figura 11**: un menú compuesto por dos repetidores y algunos CSS

Este menú está en la página maestra y está enlazado al mapa del sitio definido en `Web.sitemap`, lo que significa que cualquier cambio en el mapa del sitio se reflejará inmediatamente en todas las páginas que utilicen la página maestra de `Site.master`.

## <a name="disabling-viewstate"></a>Deshabilitar ViewState

Todos los controles ASP.NET pueden conservar de manera opcional su estado en el [Estado de vista](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), que se serializa como un campo de formulario oculto en el HTML representado. Los controles usan el estado de vista para recordar su estado cambiado mediante programación en los postbacks, como los datos enlazados a un control Web de datos. Aunque el estado de vista permite que se recuerde la información en los postbacks, aumenta el tamaño del marcado que se debe enviar al cliente y puede dar lugar a una saturación de página grave si no se supervisa en estrecha profundidad. Los controles Web de datos especialmente GridView son especialmente destacados para agregar docenas de kilobytes adicionales de marcado a una página. Aunque este aumento puede ser insignificante para los usuarios de banda ancha o de la intranet, el estado de vista puede agregar varios segundos al viaje de ida y vuelta para los usuarios de acceso telefónico.

Para ver el impacto del estado de vista, visite una página en un explorador y, a continuación, vea el origen enviado por la página web (en Internet Explorer, vaya al menú Ver y elija la opción origen). También puede activar el [seguimiento de páginas](https://msdn.microsoft.com/library/sfbfw58f.aspx) para ver la asignación de estado de vista utilizada por cada uno de los controles de la página. La información de estado de vista se serializa en un campo de formulario oculto denominado `__VIEWSTATE`, situado en un elemento `<div>` inmediatamente después de la etiqueta de `<form>` de apertura. El estado de vista solo se conserva cuando se usa un formulario Web. Si la página de ASP.NET no incluye un `<form runat="server">` en su sintaxis declarativa, no habrá un `__VIEWSTATE` campo de formulario oculto en el marcado representado.

El `__VIEWSTATE` campo de formulario generado por la página maestra agrega aproximadamente 1.800 bytes al marcado generado de la página. Esta recarga adicional se debe principalmente al control Repeater, ya que el contenido del control SiteMapDataSource se conserva en el estado de vista. Aunque es posible que un número extra de 1.800 bytes no parezca tan muy entusiasmado, cuando se usa un control GridView con muchos campos y registros, el estado de vista puede crecer fácilmente en un factor de 10 o más.

El estado de vista se puede deshabilitar en el nivel de página o control estableciendo la propiedad `EnableViewState` en `False`, lo que reduce el tamaño del marcado representado. Puesto que el estado de vista de un control Web de datos conserva los datos enlazados al control Web de datos entre los postbacks, al deshabilitar el estado de vista de un control Web de datos, los datos se deben enlazar en cada postback. En la versión 1. x de ASP.NET, esta responsabilidad se descendió de los hombros del desarrollador de páginas; sin embargo, con ASP.NET 2,0, los controles Web de datos se volverán a enlazar a su control de origen de datos en cada postback si es necesario.

Para reducir el estado de vista de la página, establezca la propiedad `EnableViewState` del control Repeater en `False`. Esto puede realizarse a través del ventana Propiedades en el diseñador o mediante declaración en la vista de código fuente. Después de efectuar este cambio, el marcado declarativo del repetidor debería ser similar al siguiente:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

Después de este cambio, el tamaño de estado de vista representado de la página se ha reducido a un mero 52 bytes, un ahorro del 97% en el estado de la vista. En los tutoriales de esta serie, deshabilitaremos el estado de vista de los controles Web de datos de forma predeterminada para reducir el tamaño del marcado representado. En la mayoría de los ejemplos, la propiedad `EnableViewState` se establecerá en `False` y se realizará sin mención. El único estado de vista de tiempo se tratará en escenarios donde debe estar habilitado para que el control Web de datos proporcione su funcionalidad esperada.

## <a name="step-4-adding-breadcrumb-navigation"></a>Paso 4: agregar la ruta de navegación

Para completar la página maestra, vamos a agregar un elemento de la interfaz de usuario de navegación por la ruta de navegación a cada página. La ruta de navegación muestra rápidamente a los usuarios su posición actual dentro de la jerarquía del sitio. Agregar una ruta de navegación en ASP.NET 2,0 es fácil simplemente agregar un control SiteMapPath a la página; no se necesita ningún código.

En nuestro sitio, agregue este control al encabezado `<div>`:

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

La ruta de navegación muestra la página actual que el usuario está visitando en la jerarquía del mapa del sitio, así como los "antecesores" del nodo del mapa del sitio, hasta la raíz (Inicio, en nuestro mapa del sitio).

![La ruta de navegación muestra la página actual y sus antecesores en la jerarquía del mapa del sitio.](master-pages-and-site-navigation-vb/_static/image28.png)

**Figura 12**: la ruta de navegación muestra la página actual y sus antecesores en la jerarquía del mapa del sitio

## <a name="step-5-adding-the-default-page-for-each-section"></a>Paso 5: agregar la página predeterminada para cada sección

Los tutoriales de nuestro sitio se dividen en distintas categorías informes básicos, filtrado, formato personalizado, etc., con una carpeta para cada categoría y los tutoriales correspondientes como páginas de ASP.NET dentro de esa carpeta. Además, cada carpeta contiene una página `Default.aspx`. En esta página predeterminada, vamos a mostrar todos los tutoriales de la sección actual. Es decir, para el `Default.aspx` en la carpeta `BasicReporting` tenemos vínculos a `SimpleDisplay.aspx`, `DeclarativeParams.aspx`y `ProgrammaticParams.aspx`. Aquí, una vez más, podemos usar la clase `SiteMap` y un control Web de datos para mostrar esta información en función del mapa del sitio definido en `Web.sitemap`.

Vamos a mostrar una lista sin ordenar con un repetidor de nuevo, pero esta vez mostraremos el título y la descripción de los tutoriales. Dado que el marcado y el código para lograr esto deberán repetirse en cada página `Default.aspx`, podemos encapsular esta lógica de la interfaz de [usuario](https://msdn.microsoft.com/library/y6wb1a0e.aspx)en un control de usuario. Cree una carpeta en el sitio web denominada `UserControls` y agregue a un nuevo elemento de tipo control de usuario Web denominado `SectionLevelTutorialListing.ascx`y agregue el marcado siguiente:

[![agregar un nuevo control de usuario Web a la carpeta UserControls](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**Figura 13**: agregar un nuevo control de usuario Web a la carpeta `UserControls` ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image31.png))

SectionLevelTutorialListing.ascx

[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb

[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

En el ejemplo de Repeater anterior, enlazamos el `SiteMap` datos al repetidor mediante declaración. sin embargo, el control de usuario `SectionLevelTutorialListing` lo hace mediante programación. En el controlador de eventos `Page_Load`, se realiza una comprobación para asegurarse de que la dirección URL de esta página se asigna a un nodo del mapa del sitio. Si este control de usuario se utiliza en una página que no tiene una entrada de `<siteMapNode>` correspondiente, `SiteMap.CurrentNode` devolverá `Nothing` y no se enlazará ningún dato al repetidor. Suponiendo que tenemos un `CurrentNode`, enlazamos su colección de `ChildNodes` con el repetidor. Puesto que el mapa del sitio está configurado de tal forma que la página `Default.aspx` de cada sección es el nodo primario de todos los tutoriales de esa sección, este código mostrará vínculos y descripciones de todos los tutoriales de la sección, tal y como se muestra en la captura de pantalla siguiente.

Una vez creado este repetidor, abra las páginas de `Default.aspx` de cada una de las carpetas, vaya al Vista de diseño y, simplemente, arrastre el control de usuario desde el Explorador de soluciones a la superficie de diseño en la que desea que aparezca la lista de tutoriales.

[![el control de usuario se ha agregado a default. aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**Figura 14**: el control de usuario se ha agregado a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image34.png))

[![se enumeran los tutoriales de informes básicos](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**Figura 15**: se enumeran los tutoriales de informes básicos ([haga clic para ver la imagen de tamaño completo](master-pages-and-site-navigation-vb/_static/image37.png))

## <a name="summary"></a>Resumen

Una vez definido el mapa del sitio y finalizada la página maestra, ahora tenemos un diseño de página y un esquema de navegación coherentes para nuestros tutoriales relacionados con los datos. Independientemente del número de páginas que agreguemos a nuestro sitio, la actualización del diseño de página en todo el sitio o la información de navegación del sitio es un proceso rápido y sencillo debido a que esta información está centralizada. En concreto, la información de diseño de página se define en la página maestra `Site.master` y el mapa del sitio en `Web.sitemap`. No es necesario escribir *ningún* código para lograr este diseño de página en todo el sitio y el mecanismo de navegación, y se conserva la compatibilidad completa con el diseñador WYSIWYG en Visual Studio.

Una vez que haya completado el nivel de acceso a datos y la capa de lógica de negocios y haya definido un diseño de página y una navegación de sitio coherentes, estamos preparados para empezar a explorar patrones de informes comunes. En los tres tutoriales siguientes, veremos las tareas de informes básicas que muestran los datos recuperados de la capa BLL en los controles GridView, DetailsView y FormView.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Información general de ASP.NET Master pages](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Páginas maestras en ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [Plantillas de diseño de ASP.NET 2,0](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Introducción a la navegación del sitio de ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examinar la navegación del sitio de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Características de navegación del sitio de ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Descripción del estado de vista de ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Cómo: habilitar el seguimiento de una página de ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controles de usuario de ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Liz Shulok, Dennis Patterson y Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-business-logic-layer-vb.md)
