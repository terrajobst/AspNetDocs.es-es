---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Especificar el título, etiquetas meta y otros encabezados HTML en la página maestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina las distintas técnicas para definir los elementos&gt; de &lt;principales de la página maestra desde la página de contenido.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 160af664cdf27f9ede1273aaf915da749a39ad48
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637699"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Especificar el título, etiquetas meta y otros encabezados HTML en la página maestra (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examina las distintas técnicas para definir los elementos&gt; de &lt;principales de la página maestra desde la página de contenido.

## <a name="introduction"></a>Introducción

Las nuevas páginas maestras creadas en Visual Studio 2008 tienen, de forma predeterminada, dos controles ContentPlaceHolder: uno denominado `head`y se encuentra en el elemento `<head>`; y uno denominado `ContentPlaceHolder1`, colocado en el formulario Web Forms. El propósito de `ContentPlaceHolder1` es definir una región en el formulario web que se puede personalizar página por página. El `head` ContentPlaceHolder permite a las páginas agregar contenido personalizado a la sección `<head>`. (Por supuesto, estos dos ContentPlaceHolders se pueden modificar o quitar y se puede Agregar un ContentPlaceHolder adicional a la página maestra. Nuestra página maestra, `Site.master`, tiene actualmente cuatro controles ContentPlaceHolder).

El elemento HTML `<head>` actúa como repositorio para obtener información sobre el documento de página web que no forma parte del propio documento. Esto incluye información como el título de la página web, la metainformación usada por los motores de búsqueda o los rastreadores internos, y vínculos a recursos externos, como fuentes RSS, JavaScript y archivos CSS. Parte de esta información puede ser pertinente para todas las páginas del sitio Web. Por ejemplo, puede que desee importar globalmente las mismas reglas de CSS y archivos JavaScript para cada página de ASP.NET. Sin embargo, hay partes del elemento `<head>` que son específicas de la página. El título de la página es un ejemplo primo.

En este tutorial, se examina cómo definir el marcado de la sección `<head>` específica de la página y global en la página maestra y en sus páginas de contenido.

## <a name="examining-the-master-pagesheadsection"></a>Examinar la sección de`<head>`de la página maestra

El archivo de página maestra predeterminado creado por Visual Studio 2008 contiene el marcado siguiente en su `<head>` sección:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Observe que el elemento `<head>` contiene un atributo `runat="server"`, que indica que se trata de un control de servidor (en lugar de HTML estático). Todas las páginas de ASP.NET derivan de la [clase`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx), que se encuentra en el espacio de nombres `System.Web.UI`. Esta clase contiene una [propiedad de`Header`](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) que proporciona acceso a la región de `<head>` de la página. Con la propiedad `Header` podemos establecer el título de una página ASP.NET o agregar marcado adicional a la sección `<head>` representada. Es posible, a continuación, personalizar el elemento de `<head>` de una página de contenido escribiendo un fragmento de código en el controlador de eventos `Page_Load` de la página. Examinamos cómo establecer mediante programación el título de la página en el paso 1.

El marcado que se muestra en el elemento `<head>` anterior también incluye un control ContentPlaceHolder denominado `head`. Este control ContentPlaceHolder no es necesario, ya que las páginas de contenido pueden agregar contenido personalizado al elemento `<head>` mediante programación. Sin embargo, resulta útil en situaciones en las que una página de contenido necesita agregar marcado estático al elemento `<head>`, ya que el marcado estático se puede agregar mediante declaración al control de contenido correspondiente en lugar de mediante programación.

Además del elemento `<title>` y `head` ContentPlaceHolder, el elemento `<head>` de la página maestra debe contener cualquier marcado de nivel de `<head>`que sea común a todas las páginas. En nuestro sitio web, todas las páginas usan las reglas de CSS definidas en el archivo de `Styles.css`. Por lo tanto, se actualizó el elemento `<head>` en el tutorial [*crear un diseño de todo el sitio con páginas maestras*](creating-a-site-wide-layout-using-master-pages-vb.md) para incluir un elemento de `<link>` correspondiente. A continuación se muestra el marcado de `<head>` actual de la página maestra de `Site.master`.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Paso 1: establecer el título de una página de contenido

El título de la página web se especifica mediante el elemento `<title>`. Es importante establecer el título de cada página en un valor adecuado. Al visitar una página, su título se muestra en la barra de título del explorador. Además, al marcar una página, los exploradores usan el título de la página como el nombre sugerido para el marcador. Además, muchos motores de búsqueda muestran el título de la página al mostrar los resultados de la búsqueda.

> [!NOTE]
> De forma predeterminada, Visual Studio establece el elemento `<title>` en la página maestra en "página sin título". Del mismo modo, las páginas ASP.NET nuevas tienen su `<title>` establecida en "página sin título". Dado que puede ser fácil olvidar establecer el título de la página en un valor adecuado, hay muchas páginas en Internet con el título "página sin título". La búsqueda de páginas web de Google con este título devuelve aproximadamente 2.460.000 resultados. Incluso Microsoft es susceptible a publicar páginas web con el título "página sin título". En el momento de redactar este documento, una búsqueda de Google comunicó 236 estas páginas web en el dominio Microsoft.com.

Una página ASP.NET puede especificar su título de una de las siguientes maneras:

- Colocando el valor directamente en el elemento `<title>`
- Usar el atributo `Title` en la Directiva `<%@ Page %>`
- Establecer mediante programación la propiedad `Title` de la página mediante código como `Page.Title="title"` o `Page.Header.Title="title"`.

Las páginas de contenido no tienen un elemento `<title>`, como se define en la página maestra. Por lo tanto, para establecer el título de una página de contenido, puede usar el atributo `Title` de la Directiva de `<%@ Page %>` o establecerlo mediante programación.

### <a name="setting-the-pages-title-declaratively"></a>Establecer el título de la página mediante declaración

El título de una página de contenido se puede establecer mediante declaración a través del atributo `Title` de la [directiva`<%@ Page %>`](https://msdn.microsoft.com/library/ydy4x04a.aspx). Esta propiedad se puede establecer modificando directamente la Directiva de `<%@ Page %>` o mediante el ventana Propiedades. Echemos un vistazo a ambos enfoques.

En la vista Código fuente, busque la Directiva `<%@ Page %>`, que se encuentra en la parte superior del marcado declarativo de la página. A continuación se muestra la Directiva de `<%@ Page %>` para `Default.aspx`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

La Directiva `<%@ Page %>` especifica atributos específicos de la página utilizados por el motor de ASP.NET al analizar y compilar la página. Esto incluye su archivo de página maestra, la ubicación de su archivo de código y su título, entre otra información.

De forma predeterminada, al crear una nueva página de contenido, Visual Studio establece el atributo `Title` en "página sin título". Cambie el atributo de `Title` de `Default.aspx`de "página sin título" a "tutoriales de página maestra" y, a continuación, vea la página a través de un explorador. En la figura 1 se muestra la barra de título del explorador, que refleja el título de la nueva página.

![La barra de título del explorador muestra ahora &quot;tutoriales de página maestra&quot; en lugar de &quot;página sin título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figura 01**: la barra de título del explorador muestra ahora "tutoriales de página maestra" en lugar de "página sin título"

El título de la página también se puede establecer desde el ventana Propiedades. En el ventana Propiedades, seleccione documento en la lista desplegable para cargar las propiedades de nivel de página, que incluye la propiedad `Title`. En la ilustración 2 se muestra el ventana Propiedades después de que `Title` se haya establecido en "tutoriales de página maestra".

![También puede configurar el título desde la ventana Propiedades.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figura 02**: también puede configurar el título desde la ventana Propiedades.

### <a name="setting-the-pages-title-programmatically"></a>Establecer el título de la página mediante programación

El marcado de `<head runat="server">` de la página maestra se convierte en una instancia de la [clase`HtmlHead`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) cuando el motor ASP.net representa la página. La clase `HtmlHead` tiene una [propiedad`Title`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cuyo valor se refleja en el elemento `<title>` representado. Esta propiedad es accesible desde la clase de código subyacente de una página de ASP.NET a través de `Page.Header.Title`; también se puede tener acceso a esta misma propiedad a través de `Page.Title`.

Para practicar el establecimiento del título de la página mediante programación, navegue hasta la clase de código subyacente de la página de `About.aspx` y cree un controlador de eventos para el evento `Load` de la página. A continuación, establezca el título de la página en "master page tutoriales:: about:: *Date*", donde *Date* es la fecha actual. Después de agregar este código, el controlador de eventos `Page_Load` debe ser similar al siguiente:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

En la ilustración 3 se muestra la barra de título del explorador al visitar la página `About.aspx`.

![El título de la página se establece mediante programación e incluye la fecha actual.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figura 03**: el título de la página se establece mediante programación e incluye la fecha actual

## <a name="step-2-automatically-assigning-a-page-title"></a>Paso 2: asignar automáticamente un título de página

Como vimos en el paso 1, el título de una página se puede establecer de forma declarativa o mediante programación. Sin embargo, si olvida cambiar explícitamente el título a algo más descriptivo, la página tendrá el título predeterminado "página sin título". Idealmente, el título de la página se establecería automáticamente para nosotros en caso de que no se especifique explícitamente su valor. Por ejemplo, si en tiempo de ejecución el título de la página es "página sin título", es posible que quiera que el título se actualice automáticamente para que sea el mismo que el nombre de archivo de la página ASP.NET. La buena noticia es que, con un poco de trabajo inicial, es posible que el título se asigne automáticamente.

Todas las páginas web de ASP.NET derivan de la clase `Page` en el espacio de nombres System. Web. UI. La clase `Page` define la funcionalidad mínima que necesita una página ASP.NET y expone propiedades esenciales como `IsPostBack`, `IsValid`, `Request`y `Response`, entre muchas otras. A menudo, cada página de una aplicación web requiere características o funciones adicionales. Una forma habitual de proporcionar esto es crear una clase de página base personalizada. Una clase de página base personalizada es una clase que se crea y que deriva de la clase `Page` e incluye funcionalidad adicional. Una vez creada esta clase base, puede hacer que las páginas de ASP.NET se deriven de ella (en lugar de la clase `Page`), lo que ofrece la funcionalidad extendida a las páginas de ASP.NET.

En este paso se crea una página base que establece automáticamente el título de la página en el nombre de archivo de la página ASP.NET si el título no se ha establecido explícitamente. En el paso 3 se examina la configuración del título de la página en función del mapa del sitio.

> [!NOTE]
> Un examen exhaustivo de la creación y el uso de clases de páginas base personalizadas queda fuera del ámbito de esta serie de tutoriales. Para obtener más información, lea [uso de una clase base personalizada para las clases de código subyacente de las páginas de ASP.net](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Crear la clase de página base

La primera tarea consiste en crear una clase de página base, que es una clase que extiende la clase `Page`. Para empezar, agregue una carpeta de `App_Code` al proyecto haciendo clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones, eligiendo Agregar carpeta ASP.NET y, a continuación, seleccione `App_Code`. A continuación, haga clic con el botón derecho en la carpeta `App_Code` y agregue una nueva clase denominada `BasePage.vb`. En la figura 4 se muestra el Explorador de soluciones después de agregar la carpeta `App_Code` y la clase `BasePage.vb`.

![Agregue una carpeta App_Code y una clase denominada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figura 04**: agregar una carpeta `App_Code` y una clase denominada `BasePage`

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios web y proyectos de aplicación Web. La carpeta `App_Code` está diseñada para usarse con el modelo de proyecto de sitio Web. Si está utilizando el modelo de proyecto de aplicación Web, coloque la clase `BasePage.vb` en una carpeta denominada algo distinto de `App_Code`, como `Classes`. Para obtener más información sobre este tema, consulte [migración de un proyecto de sitio web a un proyecto de aplicación web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).

Dado que la página base personalizada actúa como la clase base para las clases de código subyacente de las páginas de ASP.NET, debe extender la clase `Page`.

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Cada vez que se solicita una página de ASP.NET, continúa a través de una serie de fases que culminan en la página solicitada que se representa en HTML. Podemos aprovechar una fase invalidando el método `OnEvent` de la clase `Page`. En la página base, se establecerá automáticamente el título si no se ha especificado explícitamente en la fase `LoadComplete` (que, como podría haber adivinado, se produce después de la fase `Load`).

Para ello, invalide el método `OnLoadComplete` y escriba el código siguiente:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

El método `OnLoadComplete` comienza determinando si la propiedad `Title` no se ha establecido explícitamente. Si la propiedad `Title` es `Nothing`, una cadena vacía o tiene el valor "página sin título", se asigna al nombre de archivo de la página ASP.NET solicitada. La ruta de acceso física a la página ASP.NET solicitada-`C:\MySites\Tutorial03\Login.aspx`, por ejemplo, es accesible a través de la propiedad `Request.PhysicalPath`. El método `Path.GetFileNameWithoutExtension` se usa para extraer solo la parte del nombre de archivo y, a continuación, este nombre de archivo se asigna a la propiedad `Page.Title`.

> [!NOTE]
> Le invitamos a mejorar esta lógica para mejorar el formato del título. Por ejemplo, si el nombre de archivo de la página es `Company-Products.aspx`, el código anterior generará el título "Company-Products", pero idealmente, el guión se reemplazaría por un espacio, como en "productos de empresa". Además, considere la posibilidad de agregar un espacio siempre que se produzca un cambio de mayúsculas y minúsculas. Es decir, considere la posibilidad de agregar código que transforme el nombre de archivo `OurBusinessHours.aspx` por un título de "nuestras horas de trabajo".

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Hacer que las páginas de contenido hereden la clase de página base

Ahora es necesario actualizar las páginas de ASP.NET en nuestro sitio para que se deriven de la página base personalizada (`BasePage`) en lugar de la clase `Page`. Para ello, vaya a cada clase de código subyacente y cambie la declaración de clase de:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

A:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Después de hacerlo, visite el sitio a través de un explorador. Si visita una página cuyo título se establece explícitamente, como `Default.aspx` o `About.aspx`, se usa el título especificado explícitamente. Sin embargo, si visita una página cuyo título no se ha cambiado con respecto a la predeterminada ("página sin título"), la clase de página base establece el título en el nombre de archivo de la página.

En la figura 5 se muestra la página `MultipleContentPlaceHolders.aspx` cuando se ve a través de un explorador. Tenga en cuenta que el título es precisamente el nombre de archivo de la página (menos la extensión), "MultipleContentPlaceHolders".

[![si no se especifica explícitamente un título, el nombre de archivo de la página se usa automáticamente.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figura 05**: Si no se especifica explícitamente un título, el nombre de archivo de la página se usa automáticamente ([haga clic para ver la imagen de tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Paso 3: basar el título de la página en el mapa del sitio

ASP.NET ofrece un marco sólido de mapa del sitio que permite a los desarrolladores de páginas definir un mapa jerárquico del sitio en un recurso externo (como un archivo XML o una tabla de base de datos) junto con controles Web para mostrar información sobre el mapa del sitio (como, por ejemplo, SiteMapPath). Menú y controles TreeView).

También se puede obtener acceso a la estructura del mapa del sitio mediante programación desde la clase de código subyacente de una página de ASP.NET. De esta manera, podemos establecer automáticamente el título de una página en el título de su nodo correspondiente en el mapa del sitio. Vamos a mejorar la clase de `BasePage` creada en el paso 2 para que ofrezca esta funcionalidad. Pero primero es necesario crear un mapa del sitio para nuestro sitio.

> [!NOTE]
> En este tutorial se da por supuesto que el lector ya está familiarizado con ASP. Características del mapa del sitio de la red. Para obtener más información sobre el uso del mapa del sitio, consulte la serie de artículos de varias partes y [examine ASP. Navegación del sitio de la red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Crear el mapa del sitio

El sistema del mapa del sitio se basa en el [modelo del proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que DESACOPLA la API del mapa del sitio de la lógica que serializa la información del mapa del sitio entre la memoria y un almacén persistente. El .NET Framework se suministra con la [clase`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que es el proveedor del mapa del sitio predeterminado. Como su nombre implica, `XmlSiteMapProvider` usa un archivo XML como almacén del mapa del sitio. Vamos a usar este proveedor para definir nuestro mapa del sitio.

Empiece por crear un archivo de mapa del sitio en la carpeta raíz del sitio web denominada `Web.sitemap`. Para ello, haga clic con el botón derecho en el nombre del sitio web en Explorador de soluciones, elija Agregar nuevo elemento y seleccione la plantilla mapa del sitio. Asegúrese de que el archivo se llama `Web.sitemap` y haga clic en Agregar.

[![agregar un archivo denominado Web. sitemap a la carpeta raíz del sitio web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figura 06**: Agregue un archivo denominado `Web.sitemap` a la carpeta raíz del sitio web ([haga clic para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))

Agregue el siguiente código XML al archivo `Web.sitemap`:

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Este XML define la estructura jerárquica del mapa del sitio que se muestra en la figura 7.

![El mapa del sitio se compone actualmente de tres nodos de mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figura 07**: el mapa del sitio se compone actualmente de tres nodos de mapa del sitio

Actualizaremos la estructura del mapa del sitio en futuros tutoriales a medida que agregamos nuevos ejemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Actualizar la página maestra para incluir controles Web de navegación

Ahora que tenemos un mapa del sitio definido, vamos a actualizar la página maestra para que incluya controles Web de navegación. En concreto, vamos a agregar un control ListView a la columna izquierda en la sección lecciones que representa una lista sin ordenar con un elemento de lista para cada nodo definido en el mapa del sitio.

> [!NOTE]
> El control ListView es nuevo en la versión 3,5 de ASP.NET. Si usa una versión anterior de ASP.NET, utilice en su lugar el control Repeater. Para obtener más información sobre el control ListView, vea [usar los controles ListView y DataControl de ASP.net 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Comience quitando el marcado de lista sin ordenar existente de la sección lecciones. A continuación, arrastre un control ListView desde el cuadro de herramientas y colóquelo debajo del encabezado lecciones. ListView se encuentra en la sección de datos del cuadro de herramientas, junto con los demás controles de vista: GridView, DetailsView y FormView. Establezca la propiedad `ID` de ListView en `LessonsList`.

En el Asistente para la configuración de orígenes de datos, elija enlazar ListView a un nuevo control SiteMapDataSource denominado `LessonsDataSource`. El control SiteMapDataSource devuelve la estructura jerárquica del sistema del mapa del sitio.

[![enlazar un control SiteMapDataSource al control ListView de LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figura 08**: enlace de un control SiteMapDataSource al control ListView de LessonsList ([haga clic para ver la imagen de tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))

Después de crear el control SiteMapDataSource, es necesario definir las plantillas de ListView para que represente una lista sin ordenar con un elemento de lista para cada nodo devuelto por el control SiteMapDataSource. Esto puede realizarse mediante el siguiente marcado de plantilla:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

El `LayoutTemplate` genera el marcado para una lista sin ordenar (`<ul>...</ul>`) mientras que la `ItemTemplate` representa cada elemento devuelto por el SiteMapDataSource como un elemento de lista (`<li>`) que contiene un vínculo a la lección en cuestión.

Después de configurar las plantillas de ListView, visite el sitio Web. Como se muestra en la figura 9, la sección lecciones contiene un solo elemento con viñetas, Inicio. ¿Dónde están las lecciones acerca de y usan varios controles de ContentPlaceHolder? El SiteMapDataSource está diseñado para devolver un conjunto jerárquico de datos, pero el control ListView solo puede mostrar un único nivel de la jerarquía. Por consiguiente, solo se muestra el primer nivel de los nodos del mapa del sitio devueltos por SiteMapDataSource.

[![la sección lecciones contiene un solo elemento de lista](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figura 09**: la sección lecciones contiene un solo elemento de lista ([haga clic para ver la imagen de tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))

Para mostrar varios niveles, podríamos anidar varios controles ListView en el `ItemTemplate`. Esta técnica se examinó en el [tutorial *páginas maestras y navegación de sitios* ](../../data-access/introduction/master-pages-and-site-navigation-vb.md) de mi [trabajo con la serie de tutoriales de datos](../../data-access/index.md). Sin embargo, para esta serie de tutoriales, nuestro mapa del sitio contendrá solo dos niveles: Inicio (el nivel superior). y cada lección como un elemento secundario de Home. En lugar de diseñar un ListView anidado, podemos indicar a SiteMapDataSource que no devuelva el nodo de inicio estableciendo su [propiedad`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) en `False`. El efecto neto es que el SiteMapDataSource comienza devolviendo el segundo nivel de los nodos del mapa del sitio.

Con este cambio, el control ListView muestra elementos de viñetas para las lecciones acerca de y uso de varios controles ContentPlaceHolder, pero omite un elemento de viñeta para inicio. Para solucionarlo, podemos agregar explícitamente un elemento de viñeta para inicio en el `LayoutTemplate`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Al configurar SiteMapDataSource para omitir el nodo de inicio y agregar explícitamente un elemento de viñeta de inicio, en la sección lecciones se muestra ahora la salida deseada.

[![la sección lecciones contiene un elemento de viñeta para inicio y cada nodo secundario](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figura 10**: la sección lecciones contiene un elemento de viñeta para inicio y cada nodo secundario ([haga clic para ver la imagen de tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Establecer el título en función del mapa del sitio

Con el mapa del sitio en su lugar, podemos actualizar la clase `BasePage` para usar el título especificado en el mapa del sitio. Como hicimos en el paso 2, solo queremos usar el título del nodo del mapa del sitio si el desarrollador de la página no ha establecido explícitamente el título de la página. Si la página que se solicita no tiene un título de página establecido explícitamente y no se encuentra en el mapa del sitio, se volverá a usar el nombre de archivo de la página solicitada (menos la extensión), como hicimos en el paso 2. En la figura 11 se muestra este proceso de toma de decisiones.

![En ausencia de un título de página establecido explícitamente, se utiliza el título del nodo del mapa del sitio correspondiente.](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figura 11**: en ausencia de un título de página establecido explícitamente, se utiliza el título del nodo del mapa del sitio correspondiente.

Actualice el método de `OnLoadComplete` de la clase `BasePage` para incluir el código siguiente:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Como antes, el método `OnLoadComplete` se inicia determinando si el título de la página se ha establecido explícitamente. Si `Page.Title` es `Nothing`, una cadena vacía o tiene asignado el valor "página sin título", el código asigna automáticamente un valor a `Page.Title`.

Para determinar el título que se va a usar, el código comienza haciendo referencia a la [propiedad`CurrentNode`](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)de la [clase`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` devuelve la instancia de [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) del mapa del sitio que corresponde a la página solicitada actualmente. Suponiendo que la página solicitada actualmente se encuentra dentro del mapa del sitio, la propiedad `Title` del `SiteMapNode`está asignada al título de la página. Si la página solicitada actualmente no está en el mapa del sitio, `CurrentNode` devuelve `Nothing` y el nombre de archivo de la página solicitada se usa como título (como se hizo en el paso 2).

En la ilustración 12 se muestra la página `MultipleContentPlaceHolders.aspx` cuando se ve a través de un explorador. Dado que el título de esta página no se ha establecido explícitamente, en su lugar se usa el título del nodo del mapa del sitio correspondiente.

![El título de la página MultipleContentPlaceHolders. aspx se extrae del mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figura 12**: el título de la página MultipleContentPlaceHolders. aspx se extrae del mapa del sitio

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Paso 4: agregar otro marcado específico de la página a la sección`<head>`

En los pasos 1, 2 y 3 se ha examinado la personalización del elemento `<title>` página por página. Además de `<title>`, la sección `<head>` puede contener elementos `<meta>` y `<link>` elementos. Como se indicó anteriormente en este tutorial, la sección `<head>` de `Site.master`incluye un elemento `<link>` para `Styles.css`. Dado que este elemento `<link>` se define dentro de la página maestra, se incluye en la sección `<head>` para todas las páginas de contenido. Pero, ¿cómo vamos a agregar `<meta>` y `<link>` elementos página por página?

La manera más sencilla de agregar contenido específico de la página a la sección `<head>` es crear un control ContentPlaceHolder en la página maestra. Ya tenemos un ContentPlaceHolder de este tipo (denominado `head`). Por lo tanto, para agregar marcado de `<head>` personalizado, cree un control de contenido correspondiente en la página y coloque el marcado ahí.

Para ilustrar cómo agregar el marcado de `<head>` personalizado a una página, vamos a incluir un elemento de Descripción `<meta>` en nuestro conjunto actual de páginas de contenido. El elemento Description `<meta>` proporciona una breve descripción de la página web; la mayoría de los motores de búsqueda incorporan esta información de alguna forma al mostrar los resultados de la búsqueda.

Un elemento de descripción de `<meta>` tiene el formato siguiente:

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Para agregar este marcado a una página de contenido, agregue el texto anterior al control de contenido que se asigna al `head` ContentPlaceHolder de la página maestra. Por ejemplo, para definir un elemento de descripción de `<meta>` para `Default.aspx`, agregue el siguiente marcado:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Dado que el `head` ContentPlaceHolder no está dentro del cuerpo de la página HTML, el marcado agregado al control de contenido no se muestra en el Vista de diseño. Para ver el elemento `<meta>` Descripción, visite `Default.aspx` a través de un explorador. Una vez cargada la página, vea el origen y observe que la sección `<head>` incluye el marcado especificado en el control de contenido.

Dedique un momento a agregar elementos de descripción de `<meta>` a `About.aspx`, `MultipleContentPlaceHolders.aspx`y `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Agregar marcado mediante programación a la región`<head>`

El `head` ContentPlaceHolder permite agregar mediante declaración el marcado personalizado a la región de `<head>` de la página maestra. También se puede Agregar el marcado personalizado mediante programación. Recuerde que la propiedad `Header` de la clase `Page` devuelve la instancia de `HtmlHead` definida en la página maestra (la `<head runat="server">`).

Poder agregar contenido a la región `<head>` mediante programación es útil cuando el contenido que se va a agregar es dinámico. Quizás se basa en el usuario que visita la página; es posible que se extraiga de una base de datos. Independientemente del motivo, puede agregar contenido al `HtmlHead` agregando controles a su colección de `Controls` como se hace:

[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

En el código anterior se agrega el elemento `<meta>` Keywords a la región `<head>`, que proporciona una lista delimitada por comas de palabras clave que describen la página. Tenga en cuenta que para agregar una etiqueta de `<meta>` se crea una instancia de [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) , se establecen sus propiedades `Name` y `Content` y, a continuación, se agregan a la colección `Header`de `Controls`. Del mismo modo, para agregar mediante programación un elemento `<link>`, cree un objeto [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) , establezca sus propiedades y, a continuación, agréguelo a la colección `Controls` del `Header`.

> [!NOTE]
> Para agregar marcado arbitrario, cree una instancia de [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) , establezca su propiedad `Text` y, a continuación, agréguela a la colección `Controls` de `Header`.

## <a name="summary"></a>Resumen

En este tutorial se han examinado varias maneras de agregar `<head>` el marcado de la región página por página. Una página maestra debe incluir una instancia de `HtmlHead` (`<head runat="server">`) con un ContentPlaceHolder. La instancia de `HtmlHead` permite a las páginas de contenido tener acceso mediante programación a la región de `<head>` y establecer mediante declaración y mediante programación el título de la página. el control ContentPlaceHolder permite agregar marcado personalizado a la sección `<head>` mediante declaración a través de un control de contenido.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Establecer de forma dinámica el título de la página en ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examinando ASP. Navegación del sitio de la red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Cómo usar etiquetas meta HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas maestras en ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Usar los controles ListView y webpager de ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usar una clase base personalizada para las clases de código subyacente de las páginas de ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Zack Jones y este Banerjee. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-vb.md)
> [Siguiente](urls-in-master-pages-vb.md)
