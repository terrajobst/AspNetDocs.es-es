---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Crear diseños de página con páginas maestras de vista (VB) | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las ventajas de ver las páginas maestras. Puede usar...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 97c0ecf1953cc54030656dd710a5150243877110
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593915"
---
# <a name="creating-page-layouts-with-view-master-pages-vb"></a>Crear diseños de página con páginas maestras de vista (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las ventajas de ver las páginas maestras. Puede usar una página maestra de vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas de la aplicación Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Crear diseños de página con páginas maestras de vista

En este tutorial, aprenderá a crear un diseño de página común para varias páginas de la aplicación aprovechando las ventajas de ver las páginas maestras. Puede usar una página maestra de vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas de la aplicación Web.

También puede aprovechar las ventajas de ver las páginas maestras para compartir contenido común en varias páginas de la aplicación. Por ejemplo, puede colocar el logotipo del sitio web, los vínculos de navegación y los anuncios de banner en una página de vista maestra. De este modo, cada página de la aplicación mostraría este contenido automáticamente.

En este tutorial, aprenderá a crear una nueva página maestra de vista y crear una nueva página de contenido de vista basada en la página maestra.

### <a name="creating-a-view-master-page"></a>Crear una página maestra de vista

Comencemos por crear una página maestra de vista que defina un diseño de dos columnas. Para agregar una nueva página maestra de vista a un proyecto de MVC, haga clic con el botón secundario en la carpeta Views\Shared, seleccione la opción de menú **Agregar, nuevo elemento**y seleccione la plantilla de página maestra de vista de MVC (vea la figura 1).

[![agregar una página maestra de vista](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: agregar una página maestra de vista ([haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))

Puede crear más de una página maestra de vista en una aplicación. Cada página maestra de vista puede definir un diseño de página diferente. Por ejemplo, puede que desee que determinadas páginas tengan un diseño de dos columnas y otras páginas para tener un diseño de tres columnas.

Una página maestra de vista tiene un aspecto muy similar al de una vista de MVC de ASP.NET estándar. Sin embargo, a diferencia de una vista normal, una página maestra de vista contiene una o varias etiquetas de `<asp:ContentPlaceHolder>`. Las etiquetas de `<contentplaceholder>` se usan para marcar las áreas de la página maestra que se pueden invalidar en una página de contenido individual.

Por ejemplo, la página ver maestro de la lista 1 define un diseño de dos columnas. Contiene dos etiquetas de `<contentplaceholder>`. Una `<ContentPlaceHolder>` para cada columna.

**Lista 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

El cuerpo de la Página principal de vista de la lista 1 contiene dos etiquetas `<div>` que corresponden a las dos columnas. La clase de columna de hoja de estilos en cascada se aplica a ambas etiquetas de `<div>`. Esta clase se define en la hoja de estilos declarada en la parte superior de la página maestra. Puede obtener una vista previa de cómo se representará la página maestra de vista cambiando a Vista de diseño. Haga clic en la pestaña diseño en la parte inferior izquierda del editor de código fuente (vea la figura 2).

[![obtener una vista previa de una página maestra en el diseñador](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: vista previa de una página maestra en el diseñador ([haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Crear una página de contenido de vista

Después de crear una página maestra de vista, puede crear una o varias páginas de contenido de vista basadas en la página maestra de vista. Por ejemplo, puede crear una página de contenido de vista de índice para el controlador Home; para ello, haga clic con el botón secundario en la carpeta Views\Home, seleccione **Agregar, nuevo elemento**, seleccione la plantilla **Página de contenido de vista de MVC** , escriba el nombre index. aspx y haga clic en el botón Agregar (vea la figura 3).

[![agregar una página de contenido de vista](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: agregar una página de contenido de vista ([haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))

Después de hacer clic en el botón Agregar, aparece un nuevo cuadro de diálogo que le permite seleccionar una página maestra de vista para asociarla a la página ver contenido (vea la figura 4). Puede navegar a la página maestra de la vista site. Master que hemos creado en la sección anterior.

[![seleccionar una página maestra](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: selección de una página maestra ([haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))

Después de crear una nueva página de contenido de vista basada en la página maestra site. Master, obtendrá el archivo en la lista 2.

**Lista 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Tenga en cuenta que esta vista contiene una etiqueta `<asp:Content>` que corresponde a cada una de las etiquetas de `<asp:ContentPlaceHolder>` en la página maestra de vista. Cada etiqueta `<asp:Content>` incluye un atributo ContentPlaceHolderID que apunta a la `<asp:ContentPlaceHolder>` determinada que reemplaza.

Tenga en cuenta, además, que la página vista de contenido de la lista 2 no contiene ninguna de las etiquetas HTML normales de apertura y cierre. Por ejemplo, no contiene las etiquetas de apertura y cierre `<html>` ni de `<head>`. Todas las etiquetas normales de apertura y cierre están contenidas en la página maestra de vista.

Cualquier contenido que desee mostrar en una página de contenido de vista debe colocarse dentro de una etiqueta de `<asp:Content>`. Si coloca cualquier código HTML u otro contenido fuera de estas etiquetas, recibirá un error al intentar ver la página.

No es necesario invalidar cada etiqueta de `<asp:ContentPlaceHolder>` de una página maestra en una página de vista de contenido. Solo necesita invalidar una etiqueta de `<asp:ContentPlaceHolder>` cuando desee reemplazar la etiqueta por contenido determinado.

Por ejemplo, la vista de índice modificada de la lista 3 contiene solo dos etiquetas `<asp:Content>`. Cada una de las etiquetas de `<asp:Content>` incluye texto.

**Lista 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Cuando se solicita la vista de la lista 3, se representa la página en la figura 5. Observe que la vista representa una página con dos columnas. Tenga en cuenta, además, que el contenido de la página ver contenido se combina con el contenido de la página ver maestro.

[![la página contenido de la vista de índice](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: Página de contenido de la vista de índice ([haga clic para ver la imagen de tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modificar el contenido de la página maestra de vista

Un problema que se encuentra casi inmediatamente al trabajar con las páginas maestras de vista es el problema de modificar el contenido de la página maestra de vista cuando se solicitan páginas de contenido de vista diferentes. Por ejemplo, desea que cada página de la aplicación web tenga un título único. Sin embargo, el título se declara en la página ver maestro y no en la página ver contenido. Por lo tanto, ¿cómo se personaliza el título de la página para cada página de contenido de vista?

Hay dos formas de modificar el título que se muestra en una página de contenido de vista. En primer lugar, puede asignar un título de página al atributo title de la Directiva `<%@ page %>` declarada en la parte superior de una página de contenido de la vista. Por ejemplo, si desea asignar el título de la página "súper fantástico website" a la vista de índice, puede incluir la siguiente directiva en la parte superior de la vista de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Cuando la vista de índice se representa en el explorador, el título deseado aparece en la barra de título del explorador:

[![barra de título del explorador](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)

Hay un requisito importante que una página de vista maestra debe cumplir para que el atributo de título funcione. La página maestra de vista debe contener una etiqueta de `<head runat="server">` en lugar de una etiqueta de `<head>` normal para su encabezado. Si la etiqueta de `<head>` no incluye el atributo runat = "Server", el título no aparecerá. La página maestra de vista predeterminada incluye la etiqueta `<head runat="server">` requerida.

Un enfoque alternativo para modificar el contenido de la página maestra desde una página de contenido de vista individual es ajustar la región que desea modificar en una etiqueta de `<asp:ContentPlaceHolder>`. Por ejemplo, Imagine que desea cambiar no solo el título, sino también las etiquetas meta, representadas por una página de vista maestra. La página de vista maestra de la lista 4 contiene una `<asp:ContentPlaceHolder>` etiqueta dentro de su etiqueta de `<head>`.

**Lista 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Observe que la etiqueta `<asp:ContentPlaceHolder>` de la lista 4 incluye el contenido predeterminado: un título predeterminado y etiquetas meta predeterminadas. Si no invalida esta `<asp:ContentPlaceHolder>` etiqueta en una página de contenido de vista individual, se mostrará el contenido predeterminado.

La página vista de contenido de la lista 5 invalida la etiqueta `<asp:ContentPlaceHolder>` para mostrar un título personalizado y etiquetas meta personalizadas.

**Lista 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Resumen

En este tutorial se proporciona una introducción básica para ver páginas maestras y ver páginas de contenido. Aprendió a crear nuevas páginas maestras de vista y crear páginas de contenido de vista basadas en ellas. También hemos examinado cómo puede modificar el contenido de una página maestra de vista desde una página de contenido de vista determinada.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Siguiente](passing-data-to-view-master-pages-vb.md)
