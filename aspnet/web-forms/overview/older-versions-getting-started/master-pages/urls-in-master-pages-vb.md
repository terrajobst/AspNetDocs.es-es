---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Direcciones URL en páginas maestras (VB) | Microsoft Docs
author: rick-anderson
description: Soluciona cómo se pueden interrumpir las direcciones URL de la página maestra debido a que el archivo de la página maestra está en un directorio relativo diferente que la página de contenido. Examina el reajuste...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 01627988f68bb619969a5fe3cfaae68fe70b5d4f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441109"
---
# <a name="urls-in-master-pages-vb"></a>Direcciones URL en páginas maestras (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Soluciona cómo se pueden interrumpir las direcciones URL de la página maestra debido a que el archivo de la página maestra está en un directorio relativo diferente que la página de contenido. Examina las direcciones URL de reajuste mediante ~ en la sintaxis declarativa y el uso de ResolveUrl y ResolveClientUrl mediante programación. (Consulte también

## <a name="introduction"></a>Introducción

En todos los ejemplos que hemos visto hasta ahora, las páginas maestra y de contenido se encuentran en la misma carpeta (la carpeta raíz del sitio web). Sin embargo, no hay ningún motivo por el que las páginas maestra y de contenido deben estar en la misma carpeta. En realidad, puede crear páginas de contenido en subcarpetas. Del mismo modo, puede crear una carpeta `~/MasterPages/` en la que coloque las páginas maestras del sitio.

Un posible problema con la colocación de páginas maestras y de contenido en carpetas diferentes implica direcciones URL rotas. Si la página maestra contiene direcciones URL relativas en hipervínculos, imágenes u otros elementos, el vínculo no será válido para las páginas de contenido que residen en una carpeta diferente. En este tutorial se examina el origen de este problema, así como soluciones alternativas.

## <a name="the-problem-with-relative-urls"></a>El problema con las direcciones URL relativas

Se dice que una dirección URL de una página web es una *dirección URL relativa* si la ubicación del recurso al que apunta es relativa a la ubicación de la página web en la estructura de carpetas del sitio Web. Cualquier dirección URL que no empiece por una barra diagonal (`/`) inicial o un protocolo (como `http://`) es relativa porque la resuelve el explorador en función de la ubicación de la página web que contiene la dirección URL.

Por ejemplo, el sitio web tiene una carpeta `~/Images/` con un solo archivo de imagen, `PoweredByASPNET.gif`. El archivo de la página maestra `Site.master` tiene un elemento `<img>` en la región `footerContent` con el marcado siguiente:

[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

El valor del atributo `src` del elemento `<img>` es una dirección URL relativa porque no comienza con `/` o `http://`. En Resumen, el valor del atributo `src` indica al explorador que busque en la subcarpeta `Images` de un archivo denominado `PoweredByASPNET.gif`.

Al visitar una página de contenido, el marcado anterior se envía directamente al explorador. Dedique un momento a visitar `About.aspx` y ver el origen HTML enviado al explorador. Observará que se envió al explorador exactamente el mismo marcado en la página maestra.

[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Si la página de contenido se encuentra en la carpeta raíz (como está `About.aspx`) todo funciona según lo esperado, ya que hay una subcarpeta `Images` relativa a la carpeta raíz. Sin embargo, las cosas se dividen si la página de contenido está en una carpeta diferente de la página maestra. Para ilustrar esto, cree una subcarpeta denominada `Admin`. A continuación, agregue una página de contenido denominada `Default.aspx` a la carpeta `Admin`, asegurándose de enlazar la nueva página a la página maestra de `Site.master`.

> [!NOTE]
> En el tutorial sobre cómo [*especificar el título, etiquetas meta y otros encabezados HTML en la página maestra,* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) hemos creado una clase de página base personalizada denominada `BasePage` que establece automáticamente el título de la página de contenido (si no se ha asignado explícitamente). No se olvide de que la clase de código subyacente de la página recién creada se derive de `BasePage` para que pueda utilizar esta funcionalidad.

Después de crear esta página de contenido, el Explorador de soluciones debe ser similar a la ilustración 1.

![Se han agregado una nueva carpeta y una página de ASP.NET al proyecto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: se ha agregado una nueva carpeta y una página de ASP.net al proyecto

A continuación, actualice el archivo de `Web.sitemap` para incluir una nueva entrada de `<siteMapNode>` para esta lección. En el código XML siguiente se muestra el marcado completo `Web.sitemap`, que ahora incluye la adición de un tercer elemento `<siteMapNode>`.

[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

La página de `Default.aspx` recién creada debe tener cuatro controles de contenido correspondientes a los cuatro ContentPlaceHolders en `Site.master`. Agregue texto al control de contenido que hace referencia a la `MainContent` ContentPlaceHolder y, a continuación, visite la página a través de un explorador. Como se muestra en la figura 2, el explorador no puede encontrar el archivo de imagen de `PoweredByASPNET.gif`. ¿Qué ocurre aquí?

La página de contenido de `~/Admin/Default.aspx` se envía al mismo código HTML para la región de `footerContent` que la página `About.aspx`:

[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Dado que el atributo `src` del elemento `<img>` es una dirección URL relativa, el explorador intenta buscar una carpeta `Images` relativa a la ubicación de la carpeta de la Página Web. En otras palabras, el explorador busca el archivo de imagen `Admin/Images/PoweredByASPNET.gif`.

[No se encuentra ![el archivo de imagen PoweredByASPNET. gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: no se encuentra el archivo de imagen de `PoweredByASPNET.gif` ([haga clic para ver la imagen de tamaño completo](urls-in-master-pages-vb/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Reemplazar direcciones URL relativas con direcciones URL absolutas

Lo contrario de una dirección URL relativa es una *dirección URL absoluta*, que es aquella que empieza con una barra diagonal (`/`) o un protocolo como `http://`. Dado que una dirección URL absoluta especifica la ubicación de un recurso desde un punto fijo conocido, la misma dirección URL absoluta es válida en cualquier página web, independientemente de la ubicación de la página web en la estructura de carpetas del sitio Web.

Para remediar la imagen rota que se muestra en la figura 2, es necesario actualizar el atributo `src` del elemento de la `<img>` para que use una dirección URL absoluta en lugar de una relativa. Para determinar la dirección URL absoluta correcta, visite una de las páginas web de su sitio web y examine la barra de direcciones. Como se muestra en la barra de direcciones de la figura 2, la ruta de acceso completa a la aplicación web es `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Por lo tanto, podríamos actualizar el atributo de `src` del elemento de la `<img>` a cualquiera de las dos direcciones URL absolutas siguientes:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Dedique un momento a actualizar el atributo de `src` del elemento de la `<img>` a una dirección URL absoluta mediante uno de los formularios mostrados anteriormente y, a continuación, visite la página `~/Admin/Default.aspx` a través de un explorador. Esta vez el explorador buscará y mostrará correctamente el archivo de imagen de `PoweredByASPNET.gif` (vea la figura 3).

[![se muestra la imagen PoweredByASPNET. gif](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: ahora se muestra la imagen de `PoweredByASPNET.gif` ([haga clic para ver la imagen de tamaño completo](urls-in-master-pages-vb/_static/image7.png))

Aunque la codificación rígida en la dirección URL absoluta funciona, acopla el código HTML a la ubicación del servidor y de la carpeta del sitio web, lo que puede cambiar. El uso de una dirección URL absoluta con el formato `http://localhost:3908/...` es frágil porque el número de puerto anterior localhost se selecciona automáticamente cada vez que se inicia el servidor Web de desarrollo ASP.NET integrado de Visual Studio. Del mismo modo, el elemento `http://localhost` solo es válido cuando se prueba localmente. Una vez implementado el código en un servidor de producción, la base de la dirección URL cambiará a otra cosa, como `http://www.yourserver.com`. La dirección URL absoluta con el formato `/ASPNET_MasterPages_Tutorial_04_VB/...` también se ve afectada por la misma fragilidad porque a menudo esta ruta de acceso de la aplicación difiere entre los servidores de desarrollo y de producción.

La buena noticia es que ASP.NET ofrece un método para generar una dirección URL relativa válida en tiempo de ejecución.

## <a name="usingandresolveclienturl"></a>Usar`~`y`ResolveClientUrl`

En lugar de codificar una dirección URL absoluta, ASP.NET permite a los desarrolladores de páginas usar la tilde (`~`) para indicar la raíz de la aplicación Web. Por ejemplo, anteriormente en este tutorial, usé la notación `~/Admin/Default.aspx` del texto para hacer referencia a la página `Default.aspx` de la carpeta `Admin`. El `~` indica que la carpeta de `Admin` es una subcarpeta de la raíz de la aplicación Web.

El [método`ResolveClientUrl`](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) de la clase `Control` toma una dirección URL y la modifica en una dirección URL relativa adecuada para la página web en la que reside el control. Por ejemplo, al llamar a `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` desde `About.aspx` se devuelve `Images/PoweredByASPNET.gif`. Sin embargo, al llamarlo desde `~/Admin/Default.aspx`, se devuelve `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Dado que todos los controles de servidor ASP.NET derivan de la clase `Control`, todos los controles de servidor tienen acceso al método `ResolveClientUrl`. Incluso la clase `Page` deriva de la clase `Control`, lo que significa que puede usar este método directamente desde las clases de código subyacente de las páginas de ASP.NET.

### <a name="usingin-the-declarative-markup"></a>Usar`~`en el marcado declarativo

Varios controles Web ASP.NET incluyen propiedades relacionadas con la dirección URL: el control de hipervínculo tiene una propiedad `NavigateUrl`; el control de imagen tiene una propiedad `ImageUrl`; etc. Cuando se representan, estos controles pasan sus valores de propiedad relacionados con la dirección URL a `ResolveClientUrl`. Por consiguiente, si estas propiedades contienen un `~` para indicar la raíz de la aplicación Web, la dirección URL se modificará en una dirección URL relativa válida.

Tenga en cuenta que solo los controles de servidor ASP.NET transforman el `~` en sus propiedades relacionadas con la dirección URL. Si aparece un `~` en el marcado HTML estático, como `<img src="~/Images/PoweredByASPNET.gif" />`, el motor ASP.NET envía el `~` al explorador junto con el resto del contenido HTML. El explorador supone que el `~` forma parte de la dirección URL. Por ejemplo, si el explorador recibe el marcado `<img src="~/Images/PoweredByASPNET.gif" />` supone que hay una subcarpeta denominada `~` con una subcarpeta `Images` que contiene el archivo de imagen `PoweredByASPNET.gif`.

Para corregir el marcado de la imagen en `Site.master`, reemplace el elemento de `<img>` existente por un control Web de imagen ASP.NET. Establezca el `ID` del control Web de la imagen en `PoweredByImage`, su propiedad `ImageUrl` en `~/Images/PoweredByASPNET.gif`y su propiedad `AlternateText` en "Powered by ASP.NET!"

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Después de realizar este cambio en la página maestra, vuelva a visitar la página `~/Admin/Default.aspx`. Esta vez el archivo de imagen de `PoweredByASPNET.gif` aparece en la página (vea la figura 3). Cuando se representa el control Web Image, usa el método `ResolveClientUrl` para resolver el valor de la propiedad `ImageUrl`. En `~/Admin/Default.aspx` el `ImageUrl` se convierte en una dirección URL relativa adecuada, como se muestra en el siguiente fragmento de código HTML:

[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Además de usarse en las propiedades de los controles Web basados en URL, el `~` también se puede usar al llamar a los métodos `Response.Redirect` y `Server.MapPath`, entre otros. Además, el método de `ResolveClientUrl` se puede invocar directamente desde el marcado declarativo de ASP.NET o de una página maestra, si es necesario. Vea la entrada de blog de [Fritz Onion](https://www.pluralsight.com/blogs/fritz/) [mediante `ResolveClientUrl` en el marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Corregir las direcciones URL relativas restantes de la página maestra

Además del elemento `<img>` en el `footerContent` que acabamos de corregir, la página maestra contiene una dirección URL más relativa que requiere la atención. La región `topContent` incluye el vínculo "tutoriales de páginas principales", que apunta a `Default.aspx`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Dado que esta dirección URL es relativa, enviará al usuario a la página `Default.aspx` de la carpeta de la página de contenido que está visitando. Para que este vínculo Apunte siempre `Default.aspx` en la carpeta raíz, es necesario reemplazar el elemento `<a>` por un control Web de hipervínculo para que podamos usar la notación `~`.

Quite el marcado del elemento de `<a>` y agregue un control de hipervínculo en su lugar. Establezca el `ID` del hipervínculo en `lnkHome`, su propiedad `NavigateUrl` en `~/Default.aspx`y su propiedad `Text` en "tutoriales de páginas maestras".

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

Ya está. En este momento, todas las direcciones URL de nuestra página maestra se basan correctamente cuando se representan en una página de contenido independientemente de en qué carpetas se encuentran la página maestra y la página de contenido.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolución automática de direcciones URL en la sección`<head>`

En el tutorial [*crear un diseño de todo el sitio mediante páginas maestras*](creating-a-site-wide-layout-using-master-pages-vb.md) , se ha agregado un `<link>` al archivo de `Styles.css` en la región `<head>`:

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Mientras que el atributo `href` del elemento `<link>` es relativo, se convierte automáticamente en una ruta de acceso adecuada en tiempo de ejecución. Como se explicó en el tutorial [*especificar el título, etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , la región `<head>` es realmente un control del lado servidor, lo que le permite modificar el contenido de sus controles internos cuando se representa.

Para comprobarlo, vuelva a visitar la página `~/Admin/Default.aspx` y vea el origen HTML enviado al explorador. Como se muestra en el siguiente fragmento de código, el atributo `href` del elemento `<link>` se ha modificado automáticamente en una dirección URL relativa adecuada, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Resumen

Las páginas maestras suelen incluir vínculos, imágenes y otros recursos externos que deben especificarse a través de una dirección URL. Dado que la página maestra y las páginas de contenido podrían no existir en la misma carpeta, es importante abstain del uso de direcciones URL relativas. Aunque es posible usar direcciones URL absolutas codificadas de forma rígida, al hacerlo se acopla estrechamente la dirección URL absoluta a la aplicación Web. Si cambia la dirección URL absoluta, como suele hacer al mover o implementar una aplicación Web, tendrá que recordar volver atrás y actualizar las direcciones URL absolutas.

El enfoque ideal es usar la tilde (`~`) para indicar la raíz de la aplicación. Los controles Web ASP.NET que contienen propiedades relacionadas con la dirección URL asignan el `~` a la raíz de la aplicación en tiempo de ejecución. Internamente, los controles web utilizan el método `ResolveClientUrl` de la clase `Control` para generar una dirección URL relativa válida. Este método es público y está disponible en todos los controles de servidor (incluida la clase `Page`), por lo que se puede utilizar mediante programación desde las clases de código subyacente, si es necesario.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Páginas maestras en ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Rebasar direcciones URL en una página maestra](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usar `ResolveClientUrl` en el marcado](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [Siguiente](control-id-naming-in-content-pages-vb.md)
