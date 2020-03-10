---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Crear un diseño de todo el sitio mediante páginas maestras (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se mostrarán los conceptos básicos de las páginas maestras. Es decir, qué son las páginas maestras, cómo crea una página maestra, qué son los marcadores de posición de contenido, cómo se realiza un CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: c0ee6ed9d944b9a8ff2b2996e93706b8416de905
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519229"
---
# <a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Crear un diseño de todo el sitio mediante páginas maestras (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> En este tutorial se mostrarán los conceptos básicos de las páginas maestras. Es decir, qué son las páginas maestras, cómo crea una página maestra, qué son los marcadores de posición de contenido, cómo crea una página ASP.NET que usa una página maestra, cómo se refleja automáticamente la modificación de la página maestra en las páginas de contenido asociadas, etc.

## <a name="introduction"></a>Introducción

Un atributo de un sitio web bien diseñado es un diseño de página coherente en todo el sitio. Tome el sitio web de www.asp.net, por ejemplo. En el momento de redactar este documento, todas las páginas tienen el mismo contenido en la parte superior e inferior de la página. Como se muestra en la figura 1, la parte superior de cada página muestra una barra gris con una lista de comunidades de Microsoft. Debajo es el logotipo del sitio, la lista de idiomas en la que se ha traducido el sitio y las secciones principales: Inicio, introducción, aprendizaje, descargas, etc. Del mismo modo, la parte inferior de la página incluye información sobre la publicidad en www.asp.net, una declaración de copyright y un vínculo a la declaración de privacidad.

[![el sitio web de www.asp.net emplea una apariencia coherente en todas las páginas](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Figura 01</strong>: el sitio web de www.asp.net emplea una apariencia y funcionamiento coherentes en todas las páginas ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))

Otro atributo de un sitio bien diseñado es la facilidad con la que se puede cambiar la apariencia del sitio. En la figura 1 se muestra la Página principal de www.asp.net a partir del 2008 de marzo, pero entre ahora y la publicación de este tutorial, es posible que haya cambiado la apariencia. Es posible que los elementos de menú que aparecen a lo largo de la parte superior se expandan para incluir una nueva sección para el marco de MVC. O quizás se desvelará un diseño radicalmente nuevo con diferentes colores, fuentes y diseño. Aplicar estos cambios a todo el sitio debe ser un proceso rápido y sencillo que no requiere modificar los miles de páginas web que componen el sitio.

La creación de una plantilla de página para todo el sitio en ASP.NET es posible mediante el uso de *páginas maestras*. En pocas palabras, una página maestra es un tipo especial de página de ASP.NET que define el marcado que es común entre todas *las páginas de contenido* , así como las regiones que se pueden personalizar en una base de contenido de página por contenido. (Una página de contenido es una página de ASP.NET que está enlazada a la página maestra). Cada vez que se cambia el diseño o el formato de una página maestra, todo el resultado de sus páginas de contenido se actualiza de forma inmediata, lo que hace que la aplicación de los cambios de aspecto en todo el sitio sea tan fácil como actualizar e implementar un solo archivo (es decir, la página maestra).

Este es el primer tutorial de una serie de tutoriales que exploran el uso de páginas maestras. En el transcurso de esta serie de tutoriales:

- Examinar la creación de páginas maestras y sus páginas de contenido asociadas
- Analice una variedad de sugerencias, trucos y capturas,
- Identificar los errores comunes en las páginas maestras y explorar soluciones alternativas
- Vea cómo tener acceso a la página maestra desde una página de contenido y viceversa.
- Obtenga información sobre cómo especificar la página maestra de una página de contenido en tiempo de ejecución.
- Otros temas avanzados de la página maestra.

Estos tutoriales están orientados a ser concisos y proporcionan instrucciones paso a paso con muchas capturas de pantalla para guiarle en el proceso visualmente. Cada tutorial está disponible en C# y Visual Basic versiones e incluye una descarga del código completo usado.

Este tutorial de inaugural comienza con un vistazo a los conceptos básicos de las páginas maestras. Se explica cómo funcionan las páginas maestras, cómo crear una página maestra y las páginas de contenido asociadas mediante Visual Web Developer, y cómo ver cómo los cambios en una página maestra se reflejan inmediatamente en sus páginas de contenido. Comencemos.

## <a name="understanding-how-master-pages-work"></a>Descripción de cómo funcionan las páginas maestras

La creación de un sitio web con un diseño de página coherente en todo el sitio requiere que cada página web emita marcado de formato común además de su contenido personalizado. Por ejemplo, mientras que cada uno de los tutoriales o publicaciones de www.asp.net tiene su propio contenido único, cada una de estas páginas también representa una serie de elementos `<div>` comunes que muestran los vínculos de la sección de nivel superior: Inicio, introducción, aprendizaje, etc.

Existen varias técnicas para crear páginas web con una apariencia coherente. Un enfoque Naive es simplemente copiar y pegar el marcado de diseño común en todas las páginas web, pero este enfoque tiene varias desventajas. En el caso de los iniciadores, cada vez que se crea una nueva página, debe recordar copiar y pegar el contenido compartido en la página. Tales operaciones de copia y pegado están maduradas por error, ya que puede copiar accidentalmente un subconjunto del marcado compartido en una nueva página. Y para desactivarla, este enfoque permite reemplazar el aspecto de todo el sitio existente por uno nuevo, ya que cada una de las páginas del sitio debe editarse para poder usar la nueva apariencia.

Antes de la versión 2,0 de ASP.NET, los desarrolladores de páginas normalmente colocaban el marcado común en [los controles de usuario](https://msdn.microsoft.com/library/y6wb1a0e.aspx) y, después, agregaban estos controles de usuario a cada una de las páginas. Este enfoque requiere que el desarrollador de páginas Recuerde agregar manualmente los controles de usuario a cada página nueva, pero se permite realizar modificaciones más sencillas en todo el sitio, ya que al actualizar el marcado común solo se necesitan modificar los controles de usuario. Desafortunadamente, Visual Studio .NET 2002 y 2003: las versiones de Visual Studio que se usan para crear aplicaciones de ASP.NET 1. x: los controles de usuario representados en el Vista de diseño como cuadros grises. Por lo tanto, los desarrolladores de páginas que usan este enfoque no disfrutan de un entorno en tiempo de diseño WYSIWYG.

Las deficiencias del uso de los controles de usuario se abordaban en la versión 2,0 de ASP.NET y Visual Studio 2005 con la introducción de *las páginas maestras*. Una página maestra es un tipo especial de página de ASP.NET que define el marcado en todo el sitio y las *regiones* en las que *las páginas de contenido* asociadas definen su marcado personalizado. Como veremos en el paso 1, estas regiones están definidas por los controles de ContentPlaceHolder. El control ContentPlaceHolder simplemente denota una posición en la jerarquía de control de la página maestra donde una página de contenido puede insertar contenido personalizado.

> [!NOTE]
> Los conceptos básicos y la funcionalidad de las páginas maestras no han cambiado desde la versión 2,0 de ASP.NET. Sin embargo, Visual Studio 2008 ofrece compatibilidad en tiempo de diseño para las páginas maestras anidadas, una característica que carecía de Visual Studio 2005. Veremos el uso de páginas maestras anidadas en un tutorial futuro.

En la ilustración 2 se muestra el aspecto que podría tener la página maestra de www.asp.net. Tenga en cuenta que la página maestra define el diseño común para todo el sitio: el marcado en la parte superior, inferior y derecha de cada página, así como un ContentPlaceHolder en el centro izquierdo, donde se encuentra el contenido único de cada página web individual.

![Una página maestra define el diseño de todo el sitio y las regiones que se pueden editar en una página de contenido en función del contenido.](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Figura 02**: una página maestra define el diseño de todo el sitio y las regiones editables en una página de contenido por contenido

Una vez definida una página maestra, se puede enlazar a las nuevas páginas de ASP.NET a través de la marca de una casilla. Estas páginas de ASP.NET: llamadas páginas de contenido: incluyen un control de contenido para cada uno de los controles ContentPlaceHolder de la página maestra. Cuando se visita la página de contenido a través de un explorador, el motor ASP.NET crea la jerarquía de controles de la página maestra e inserta la jerarquía de control de la página de contenido en los lugares adecuados. Esta jerarquía de controles combinados se representa y el HTML resultante se devuelve al explorador del usuario final. Por consiguiente, la página de contenido emite el marcado común definido en su página maestra fuera de los controles de ContentPlaceHolder y el marcado específico de la página definido dentro de sus propios controles de contenido. En la figura 3 se muestra este concepto.

[![el marcado de la página solicitada se fusiona en la página maestra](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Figura 03**: el marcado de la página solicitada se fusiona en la página maestra ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))

Ahora que hemos explicado cómo funcionan las páginas maestras, echemos un vistazo a la creación de una página maestra y las páginas de contenido asociadas mediante Visual Web Developer.

> [!NOTE]
> Para llegar a la mayor audiencia posible, el sitio web de ASP.NET que creamos a lo largo de esta serie de tutoriales se creará con ASP.NET 3,5 con la versión gratuita de Microsoft de Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Si aún no ha actualizado a ASP.NET 3,5, no se preocupe, los conceptos descritos en estos tutoriales funcionan igual de bien con ASP.NET 2,0 y Visual Studio 2005. Sin embargo, algunas aplicaciones de demostración pueden usar características nuevas en la .NET Framework versión 3,5; Cuando se usan características específicas de 3,5, se incluye una nota que explica cómo implementar una funcionalidad similar en la versión 2,0. Tenga en cuenta que las aplicaciones de demostración disponibles para su descarga de cada tutorial tienen como destino la versión .NET Framework 3,5, lo que da como resultado un archivo `Web.config` que incluye elementos de configuración específicos de 3,5. Larga historia breve, si todavía tiene que instalar .NET 3,5 en el equipo, la aplicación web descargable no funcionará sin quitar primero el marcado específico de 3,5 de `Web.config`. Vea [la sección sobre cómo desseccionar el archivo `Web.config` de la versión 3.5 de ASP.net](http://www.4guysfromrolla.com/articles/121207-1.aspx) para obtener más información acerca de este tema.

## <a name="step-1-creating-a-master-page"></a>Paso 1: crear una página maestra

Antes de que podamos explorar la creación y el uso de páginas maestras y de contenido, primero necesitamos un sitio web de ASP.NET. Empiece por crear un nuevo sitio web de ASP.NET basado en el sistema de archivos. Para ello, inicie Visual Web Developer y, a continuación, vaya al menú Archivo y elija nuevo sitio web, mostrando el cuadro de diálogo nuevo sitio web (vea la figura 4). Elija la plantilla sitio Web ASP.NET, establezca la lista desplegable ubicación en sistema de archivos, elija una carpeta para colocar el sitio web y establezca el idioma en Visual Basic. Se creará un nuevo sitio web con una `Default.aspx` página ASP.NET, una carpeta `App_Data` y un archivo `Web.config`.

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios web y proyectos de aplicación Web. Los proyectos de sitio web no tienen un archivo de proyecto, mientras que los proyectos de aplicación web imitan la arquitectura de proyecto en Visual Studio .NET 2002/2003: incluyen un archivo de proyecto y compilan el código fuente del proyecto en un único ensamblado, que se coloca en la carpeta `/bin`. Visual Studio 2005 inicialmente solo admitía proyectos de sitio web, aunque el modelo de proyecto de aplicación web se representó con Service Pack 1; Visual Studio 2008 ofrece ambos modelos de proyecto. Sin embargo, las ediciones 2005 y 2008 de Visual Web Developer, solo admiten proyectos de sitio Web. Utilizo el modelo de proyecto de sitio web para mis demostraciones en esta serie de tutoriales. Si usa una edición que no es Express y desea usar el modelo de [proyecto de aplicación web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) en su lugar, no dude en hacerlo, pero tenga en cuenta que puede haber algunas discrepancias entre lo que ve en la pantalla y los pasos que debe realizar en comparación con las capturas de pantalla que se muestran y las instrucciones proporcionadas en estos tutoriales.

[![crear un nuevo sitio web basado en el sistema de archivos](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Figura 04**: crear un nuevo sitio web basado en el sistema de archivos ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))

Después, agregue una página maestra al sitio en el directorio raíz; para ello, haga clic con el botón derecho en el nombre del proyecto, elija Agregar nuevo elemento y seleccione la plantilla página maestra. Tenga en cuenta que las páginas maestras terminan con la extensión `.master`. Asigne a esta nueva página maestra el nombre `Site.master` y haga clic en Agregar.

[![agregar una página maestra denominada site. Master al sitio web](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Figura 05**: agregar una página maestra denominada `Site.master` al sitio web ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))

Al agregar un nuevo archivo de página maestra a través de Visual Web Developer, se crea una página maestra con el siguiente marcado declarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

La primera línea en el marcado declarativo es la [Directiva de`@Master`](https://msdn.microsoft.com/library/ms228176.aspx). La Directiva `@Master` es similar a la [directiva`@Page`](https://msdn.microsoft.com/library/ydy4x04a.aspx) que aparece en las páginas de ASP.net. Define el lenguaje de servidor (VB) e información sobre la ubicación y la herencia de la clase de código subyacente de la página maestra.

Los `DOCTYPE` y el marcado declarativo de la página aparecen debajo de la Directiva `@Master`. La página incluye código HTML estático junto con cuatro controles del lado servidor:

- **Un formulario web (el `<form runat="server">`)** : dado que todas las páginas de ASP.net tienen normalmente un formulario web, y dado que la página maestra puede incluir controles Web que deben aparecer en un formulario web, asegúrese de agregar el formulario web a la página maestra (en lugar de agregar un formulario web a cada página de contenido).
- **Un control ContentPlaceHolder denominado `ContentPlaceHolder1`** : este control ContentPlaceHolder aparece en el formulario Web Forms y sirve como región de la interfaz de usuario de la página de contenido.
- **Un elemento de `<head>` de servidor** : el elemento `<head>` tiene el atributo `runat="server"`, lo que permite que sea accesible a través del código de servidor. El elemento `<head>` se implementa de esta manera para que el título de la página y otro marcado relacionado con `<head>`se puedan agregar o ajustar mediante programación. Por ejemplo, al establecer la propiedad `Title` de una página ASP.NET, se cambia el elemento `<title>` representado por el control de servidor `<head>`.
- **Un control ContentPlaceHolder denominado `head`** : este control ContentPlaceHolder aparece en el control de servidor `<head>` y se puede usar para agregar contenido mediante declaración al elemento `<head>`.

Este marcado declarativo de la página maestra predeterminada sirve como punto de partida para diseñar sus propias páginas maestras. No dude en editar el código HTML o agregar controles Web adicionales o ContentPlaceHolders a la página maestra.

> [!NOTE]
> Al diseñar una página maestra, asegúrese de que la página maestra contiene un formulario web y que aparece al menos un control ContentPlaceHolder dentro de este formulario Web Forms.

### <a name="creating-a-simple-site-layout"></a>Crear un diseño de sitio simple

Vamos a expandir el marcado declarativo predeterminado del `Site.master`para crear un diseño de sitio en el que todas las páginas compartan: un encabezado común; columna izquierda con navegación, noticias y otros contenidos en todo el sitio. y un pie de página que muestra el icono "con tecnología de Microsoft ASP.NET". En la ilustración 6 se muestra el resultado final de la página maestra cuando una de sus páginas de contenido se ve a través de un explorador. La región roja en el círculo de la figura 6 es específica de la página que se está visitando (`Default.aspx`); el otro contenido se define en la página maestra y, por tanto, es coherente en todas las páginas de contenido.

[![la página maestra define el marcado de las partes superior, izquierda e inferior](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Figura 06**: la página maestra define el marcado de las partes superior, izquierda y inferior ([haga clic para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))

Para lograr el diseño del sitio que se muestra en la figura 6, empiece por actualizar la página maestra de `Site.master` para que contenga el siguiente marcado declarativo:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

El diseño de la página maestra se define mediante una serie de elementos HTML `<div>`. El `<div>` de `topContent` contiene el marcado que aparece en la parte superior de cada página, mientras que los `mainContent`, `leftContent`y `footerContent` `<div>` s se usan para mostrar el contenido de la página, la columna izquierda y el icono "con alimentación Microsoft ASP.NET", respectivamente. Además de agregar estos elementos `<div>`, también cambió el nombre de la propiedad `ID` del control ContentPlaceHolder principal de `ContentPlaceHolder1` a `MainContent`.

Las reglas de formato y diseño de estos elementos `<div>` coordenados se escriben en el archivo de [hoja de estilos en cascada (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) `Styles.css`, que se especifica a través de un elemento `<link>` en el elemento `<head>` de la página maestra. Estas diversas reglas definen la apariencia y el funcionamiento de cada elemento `<div>` indicado anteriormente. Por ejemplo, el elemento `topContent` `<div>`, que muestra el texto y el vínculo "tutoriales de páginas principales", tiene sus reglas de formato especificadas en `Styles.css` de la siguiente manera:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Si está siguiendo el equipo, tendrá que descargar el código adjunto del tutorial y agregar el archivo de `Styles.css` al proyecto. Del mismo modo, también necesitará crear una carpeta llamada `Images` y copiar el icono "con tecnología Microsoft ASP.NET" del sitio web de demostración descargado en el proyecto.

> [!NOTE]
> Un debate sobre el formato de página web y CSS queda fuera del ámbito de este artículo. Para obtener más información sobre CSS, consulte los [tutoriales de CSS](http://www.w3schools.com/css/default.asp) en [w3schools.com](http://www.w3schools.com/). También recomiendo descargar el código adjunto de este tutorial y reproducirlo con la configuración de CSS en `Styles.css` para ver los efectos de las distintas reglas de formato.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Crear una página maestra mediante una plantilla de diseño existente

A lo largo de los años he creado una serie de aplicaciones Web de ASP.NET para pequeñas y medianas empresas. Algunos de mis clientes tenían un diseño de sitio existente que deseaban usar; otros contrataron un diseñador de gráficos competente. Me han confiado en el diseño del sitio Web. Como se puede apreciar en la figura 6, la tarea de un programador para diseñar el diseño de un sitio Web suele ser tan prudente como hacer que el contable realice una cirugía de corazón abierto mientras su médico realiza sus impuestos.

Afortunadamente, hay muchos sitios web que ofrecen plantillas de diseño HTML gratuitas: Google ha devuelto más de 6 millones resultados para el término de búsqueda "plantillas de sitio web gratis". Uno de mis favoritos es [OpenDesigns.org](http://opendesigns.org/). Una vez que encuentre la plantilla de sitio web que desee, agregue los archivos y las imágenes CSS al proyecto de sitio web e integre el código HTML de la plantilla en la página maestra.

> [!NOTE]
> Microsoft también ofrece una serie de [plantillas gratuitas del kit de inicio de diseño de ASP.net](https://msdn.microsoft.com/asp.net/aa336613.aspx) que se integran en el cuadro de diálogo nuevo sitio web de Visual Studio.

## <a name="step-2-creating-associated-content-pages"></a>Paso 2: crear páginas de contenido asociadas

Una vez creada la página maestra, estamos preparados para empezar a crear páginas ASP.NET enlazadas a la página maestra. Dichas páginas se conocen como *páginas de contenido*.

Vamos a agregar una nueva página de ASP.NET al proyecto y enlazarla a la página maestra de `Site.master`. Haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y elija la opción Agregar nuevo elemento. Seleccione la plantilla de formulario web, escriba el nombre `About.aspx`y, a continuación, active la casilla "seleccionar página maestra" como se muestra en la figura 7. Al hacerlo, se mostrará el cuadro de diálogo Seleccionar una página maestra (vea la figura 8) desde donde puede elegir la página maestra que se va a usar.

> [!NOTE]
> Si ha creado el sitio web de ASP.NET mediante el modelo de proyecto de aplicación web en lugar del modelo de proyecto de sitio web, no verá la casilla "seleccionar página maestra" en el cuadro de diálogo Agregar nuevo elemento que se muestra en la figura 7. Para crear una página de contenido al usar el modelo de proyecto de aplicación Web, debe elegir la plantilla de formulario de contenido web en lugar de la plantilla de formulario Web. Después de seleccionar la plantilla formulario de contenido web y hacer clic en agregar, aparecerá el mismo cuadro de diálogo Seleccionar una página maestra que se muestra en la figura 8.

[![agregar una nueva página de contenido](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Figura 07**: agregar una nueva página de contenido ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))

[![seleccionar la página maestra site. Master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Figura 08**: seleccione la página maestra de `Site.master` ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))

Como se muestra en el siguiente marcado declarativo, una nueva página de contenido contiene una directiva `@Page` que apunta de nuevo a su página maestra y a un control de contenido para cada uno de los controles ContentPlaceHolder de la página maestra.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> En la sección "creación de un diseño de sitio simple" en el paso 1 he cambiado el nombre `ContentPlaceHolder1` a `MainContent`. Si no cambió el nombre de la `ID` de este control ContentPlaceHolder de la misma manera, el marcado declarativo de la página de contenido diferirá ligeramente del marcado que se mostró anteriormente. Es decir, la `ContentPlaceHolderID` del segundo control de contenido reflejará el `ID` del control ContentPlaceHolder correspondiente en la página maestra.

Al representar una página de contenido, el motor ASP.NET debe confundir los controles de contenido de la página con los controles ContentPlaceHolder de la página maestra. El motor de ASP.NET determina la página maestra de la página de contenido del atributo `MasterPageFile` de la Directiva de `@Page`. Como se muestra en el marcado anterior, esta página de contenido se enlaza a `~/Site.master`.

Dado que la página maestra tiene dos controles ContentPlaceHolder-`head` y `MainContent`-Visual Web Developer genera dos controles de contenido. Cada control de contenido hace referencia a un ContentPlaceHolder determinado a través de su `ContentPlaceHolderID` propiedad.

En los casos en los que las páginas maestras destacan las técnicas de plantilla en todo el sitio anterior, se ofrece compatibilidad en tiempo de diseño. En la ilustración 9 se muestra la página de contenido de `About.aspx` cuando se ve a través de la Vista de diseño de Visual Web Developer. Tenga en cuenta que mientras el contenido de la página maestra está visible, está atenuado y no se puede modificar. No obstante, los controles de contenido correspondientes a la ContentPlaceHolders de la página maestra son editables. Y, al igual que con cualquier otra página de ASP.NET, puede crear la interfaz de la página de contenido agregando controles Web a través de las vistas de origen o diseño.

[![la vista de diseño de la página de contenido muestra el contenido de la Página principal y la página maestra](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Figura 09**: en la vista de diseño de la página de contenido se muestra el contenido de la Página principal y la página maestra ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Agregar marcado y controles Web a la página de contenido

Dedique un momento a crear algo de contenido para la página `About.aspx`. Como puede ver en la figura 10, he escrito un título "acerca del autor" y un par de párrafos de texto, pero también puede Agregar controles Web. Después de crear esta interfaz, visite la página `About.aspx` a través de un explorador.

[![visite la página about. aspx a través de un explorador](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Figura 10**: visite la página `About.aspx` a través de un explorador ([haga clic para ver la imagen de tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))

Es importante comprender que la página de contenido solicitada y su página maestra asociada se fusionan y representan completamente en su totalidad en el servidor Web. A continuación, el explorador del usuario final envía el HTML resultante fundido. Para comprobarlo, vea el código HTML recibido por el explorador; para ello, vaya al menú Ver y elija origen. Tenga en cuenta que no hay fotogramas ni otras técnicas especializadas para mostrar dos páginas web diferentes en una sola ventana.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Enlazar una página maestra a una página de ASP.NET existente

Como hemos visto en este paso, la adición de una nueva página de contenido a una aplicación Web de ASP.NET es tan sencilla como activar la casilla "seleccionar página maestra" y seleccionar la página maestra. Desafortunadamente, convertir una página de ASP.NET existente en una página maestra no es tan fácil.

Para enlazar una página maestra a una página de ASP.NET existente, debe realizar los siguientes pasos:

1. Agregue el atributo `MasterPageFile` a la Directiva `@Page` de la página ASP.NET, que apunta a la página maestra adecuada.
2. Agregue controles de contenido para cada uno de los ContentPlaceHolders de la página maestra.
3. Corte y pegue de forma selectiva el contenido existente de la página ASP.NET en los controles de contenido adecuados. En este caso, digo "selectivamente" porque la página ASP.NET probablemente contiene un marcado que ya está expresado en la página maestra, como el `DOCTYPE`, el elemento `<html>` y el formulario Web.

Para obtener instrucciones paso a paso sobre este proceso, junto con las capturas de pantalla, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/) [mediante las páginas maestras y](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) el tutorial de navegación del sitio. La sección "actualizar `Default.aspx` y `DataSample.aspx` para usar la página maestra" detalla estos pasos.

Dado que es mucho más fácil crear nuevas páginas de contenido que convertir páginas de ASP.NET existentes en páginas de contenido, recomiendo que siempre que cree un nuevo sitio web de ASP.NET agregue una página maestra al sitio. Enlazar todas las páginas nuevas de ASP.NET a esta página maestra. No se preocupe si la página maestra inicial es muy sencilla o simple; puede actualizar la página maestra más adelante.

> [!NOTE]
> Al crear una nueva aplicación de ASP.NET, Visual Web Developer agrega una página `Default.aspx` que no está enlazada a una página maestra. Si desea practicar la conversión de una página de ASP.NET existente en una página de contenido, continúe con `Default.aspx`. Como alternativa, puede eliminar `Default.aspx` y, a continuación, volver a agregarlo, pero esta vez Active la casilla "seleccionar página maestra".

## <a name="step-3-updating-the-master-pages-markup"></a>Paso 3: actualizar el marcado de la página maestra

Una de las principales ventajas de las páginas maestras es que se puede usar una sola página maestra para definir el diseño general de numerosas páginas del sitio. Por lo tanto, la actualización de la apariencia y el funcionamiento del sitio requiere la actualización de un solo archivo: la página maestra.

Para ilustrar este comportamiento, vamos a actualizar nuestra página maestra para incluir la fecha actual en en la parte superior de la columna izquierda. Agregue una etiqueta llamada `DateDisplay` al `<div>`de `leftContent`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

A continuación, cree un controlador de eventos `Page_Load` para la página maestra y agregue el código siguiente:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

El código anterior establece la propiedad `Text` de la etiqueta en la fecha y hora actuales con el formato del día de la semana, el nombre del mes y el día de dos dígitos (vea la figura 11). Con este cambio, vuelva a visitar una de las páginas de contenido. Como se muestra en la figura 11, el marcado resultante se actualiza inmediatamente para incluir el cambio en la página maestra.

[![los cambios en la página maestra se reflejan al ver la página de contenido](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Figura 11**: los cambios en la página maestra se reflejan al ver la página de contenido ([haga clic para ver la imagen a tamaño completo](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))

> [!NOTE]
> Como se muestra en este ejemplo, las páginas maestras pueden contener controles Web del servidor, código y controladores de eventos.

## <a name="summary"></a>Resumen

Las páginas maestras permiten a los desarrolladores de ASP.NET diseñar un diseño coherente en todo el sitio que sea fácilmente actualizable. Crear páginas maestras y sus páginas de contenido asociadas es tan sencillo como crear páginas ASP.NET estándar, ya que Visual Web Developer ofrece compatibilidad enriquecida en tiempo de diseño.

El ejemplo de página maestra que creamos en este tutorial tenía dos controles ContentPlaceHolder, Head y MainContent. Sin embargo, solo se ha especificado el marcado para el control ContentPlaceHolder de MainContent en nuestra página de contenido. En el siguiente tutorial, veremos usar varios controles de contenido en la página de contenido. También veremos cómo definir el marcado predeterminado para los controles de contenido dentro de la página maestra y cómo alternar entre el uso del marcado predeterminado definido en la página maestra y el marcado personalizado de la página de contenido.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [ASP.NET para diseñadores: plantillas de diseño gratis e instrucciones sobre la creación de sitios web de ASP.NET con estándares web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Información general de ASP.NET Master pages](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutoriales de hojas de estilos en cascada (CSS)](http://www.w3schools.com/css/default.asp)
- [Establecer de forma dinámica el título de la página](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Páginas maestras en ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Tutoriales de inicio rápido de páginas maestras](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](nested-master-pages-cs.md)
> [Siguiente](multiple-contentplaceholders-and-default-content-vb.md)
