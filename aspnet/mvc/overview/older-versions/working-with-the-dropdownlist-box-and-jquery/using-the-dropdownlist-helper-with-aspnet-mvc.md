---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
title: Usar la aplicación auxiliar DropDownList con ASP.NET MVC | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 53767e05-c8ab-42e1-a94b-22d906195200
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 6375bb2be158cea18309ffa71c71ac3e67bc91ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433081"
---
# <a name="using-the-dropdownlist-helper-with-aspnet-mvc"></a>Usar el asistente DropDownList con ASP.NET MVC

por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tutorial le enseñará los conceptos básicos sobre cómo trabajar con la aplicación auxiliar de [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) y la aplicación auxiliar de [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) en una aplicación web MVC de ASP.net. Puede usar Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio para seguir el tutorial. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:

- [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)<a id="post"></a>
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)

Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack). En este tutorial se da por supuesto que ha completado el tutorial [Introducción a ASP.NET MVC](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o el tutorial de[ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) o está familiarizado con el desarrollo de ASP.NET MVC. Este tutorial comienza con un proyecto modificado en el tutorial de [ASP.NET MVC Music Store](../mvc-music-store/mvc-music-store-part-1.md) . Puede descargar el proyecto de inicio con el siguiente vínculo [descargar la C# versión](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829).

Hay disponible un proyecto de Visual Web Developer con C# el código fuente del tutorial completado para acompañar este tema. [Descargar](https://code.msdn.microsoft.com/Using-the-DropDownList-67f9367d).

### <a name="what-youll-build"></a>Lo que creará

Creará métodos de acción y vistas que utilicen la aplicación auxiliar [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) para seleccionar una categoría. También se utilizará **jQuery** para agregar un cuadro de diálogo de la categoría de inserción que se puede usar cuando se necesita una nueva categoría (como el género o el artista). A continuación se muestra una captura de pantalla de la vista crear que muestra vínculos para agregar un nuevo género y agregar un nuevo artista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image1.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aprenderá lo siguiente:

- Cómo usar la aplicación auxiliar [DropDownList](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.dropdownlist.aspx) para seleccionar datos de categoría.
- Cómo agregar un cuadro de diálogo de **jQuery** para agregar nuevas categorías.

### <a name="getting-started"></a>Introducción

Para empezar, descargue el proyecto de inicio con el siguiente vínculo, [Descargue](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15829). En el explorador de Windows, haga clic con el botón derecho en el archivo *dll\_Starter. zip* y seleccione Propiedades. En el cuadro de diálogo **propiedades de DDL\_Starter. zip** , seleccione Desbloquear.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image2.png)

Haga clic con el botón secundario en el archivo dll\_Starter. zip y seleccione **extraer todo** para descomprimir el archivo. Abra el archivo *StartMusicStore. sln* con Visual web Developer 2010 Express ("Visual Web Developer" o "vWD existente" para abreviar) o Visual Studio 2010.

Presione CTRL + F5 para ejecutar la aplicación y haga clic en el vínculo **probar** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image3.png)

Seleccione el vínculo **seleccionar categoría de película (simple)** . Se muestra una lista de selección de tipo de película, con comedia el valor seleccionado.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image4.png)

Haga clic con el botón derecho en el explorador y seleccione Ver código fuente. Se muestra el código HTML de la página. El código siguiente muestra el HTML para el elemento Select.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample1.html)]

Puede ver que cada elemento de la lista de selección tiene un valor (0 para Action, 1 para series, 2 para comedia y 3 para ciencia ficción) y un nombre para mostrar (Action, series, comedia y Science ficción). El código anterior es HTML estándar para una lista de selección.

Cambie la lista de selección a series y presione el botón **Enviar** . La dirección URL del explorador es `http://localhost:2468/Home/CategoryChosen?MovieType=1` y se muestra la página **seleccionada: 1**.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image5.png)

Abra el archivo *Controllers\HomeController.CS* y examine el método `SelectCategory`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample2.cs)]

La aplicación auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) utilizada para crear una lista de selección HTML requiere un **&gt;IEnumerable&lt;SelectListItem** , ya sea de forma explícita o implícita. Es decir, puede pasar la **interfaz ienumerable&lt;SelectListItem &gt;** explícitamente a la aplicación auxiliar [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) o puede agregar la **&gt;IEnumerable&lt;SelectListItem** a [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el mismo nombre para **SelectListItem** que la propiedad Model. Pasar el **SelectListItem** implícitamente y explícitamente se trata en la siguiente parte del tutorial. El código anterior muestra la manera más sencilla de crear un **&gt;IEnumerable&lt;SelectListItem** y rellenarlo con texto y valores. Tenga en cuenta que la `Comedy`[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) tiene la propiedad [selected](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.selected.aspx) establecida en **true;** esto hará que la lista de selección representada muestre **comedia** como el elemento seleccionado en la lista.

El **&gt;de IEnumerable&lt;SelectListItem** creado anteriormente se agrega a [ViewBag](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) con el nombre MovieType. Así es como pasamos el método **IEnumerable&lt;SelectListItem &gt;** implícitamente a la aplicación auxiliar de [DropDownList](https://msdn.microsoft.com/library/dd492738.aspx) que se muestra a continuación.

Abra el archivo *Views\Home\SelectCategory.cshtml* y examine el marcado.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample3.cshtml)]

En la tercera línea, establecemos el diseño en views/Shared/\_simple\_layout. cshtml, que es una versión simplificada del archivo de diseño estándar. Hacemos esto para que la presentación y el HTML representado sean sencillos.

En este ejemplo, no se cambia el estado de la aplicación, por lo que se enviarán los datos mediante **http Get**, no **http post**. Consulte la sección [lista de comprobación rápida de W3C para elegir http get o post](http://www.w3.org/2001/tag/doc/whenToUseGet.html#checklist). Dado que no estamos cambiando la aplicación y publicando el formulario, usamos la sobrecarga [HTML. BeginForm](https://msdn.microsoft.com/library/dd460344.aspx) que nos permite especificar el método de acción, el controlador y el método de formulario (**http post** o **http Get**). Normalmente, las vistas contienen la sobrecarga [HTML. BeginForm](https://msdn.microsoft.com/library/dd505244.aspx) que no toma ningún parámetro. La versión del parámetro no tiene como valor predeterminado la publicación de los datos del formulario en la versión posterior del método de acción y el controlador.

La línea siguiente

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample4.cshtml)]

pasa un argumento de cadena a la aplicación auxiliar de **DropDownList** . Esta cadena, "MovieType" en nuestro ejemplo, hace dos cosas:

- Proporciona la clave para que la aplicación auxiliar de **DropDownList** encuentre un **&gt;IEnumerable&lt;SelectListItem** en **ViewBag**.
- Está enlazado a datos con el elemento de formulario MovieType. Si el método de envío es **http Get**, `MovieType` será una cadena de consulta. Si el método de envío es **http post**, se agregará `MovieType` en el cuerpo del mensaje. En la imagen siguiente se muestra la cadena de consulta con el valor 1.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image6.png)

En el código siguiente se muestra el método `CategoryChosen` al que se envió el formulario.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample5.cs)]

Vuelva a la página de prueba y seleccione el vínculo **SelectList HTML** . La página HTML representa un elemento Select similar a la página de prueba de ASP.NET MVC simple. Haga clic con el botón derecho en la ventana del explorador y seleccione **Ver código fuente**. El marcado HTML para la lista de selección es esencialmente idéntico. Pruebe la página HTML, que funciona como el método de acción de ASP.NET MVC y la vista que hemos probado anteriormente.

### <a name="improving-the-movie-select-list-with-enums"></a>Mejora de la lista de selección de películas con enumeraciones

Si las categorías de la aplicación son fijas y no cambiarán, puede aprovechar las ventajas de las enumeraciones para que el código sea más robusto y más sencillo de ampliar. Cuando se agrega una nueva categoría, se genera el valor correcto de la categoría. El evita errores de copia y pegado al agregar una nueva categoría, pero olvida actualizar el valor de la categoría.

Abra el archivo *Controllers\HomeController.CS* y examine el código siguiente:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample6.cs)]

La [enumeración](https://msdn.microsoft.com/library/sbbt4032(VS.80).aspx) `eMovieCategories` captura los cuatro tipos de película. El método `SetViewBagMovieType` crea el **&gt;IEnumerable&lt;SelectListItem** a partir de la **enumeración**`eMovieCategories`y establece la propiedad `Selected` del parámetro `selectedMovie`. El método de acción `SelectCategoryEnum` usa la misma vista que el método de acción `SelectCategory`.

Vaya a la página de prueba y haga clic en el vínculo `Select Movie Category (Enum)`. Esta vez, en lugar de mostrar un valor (número), se muestra una cadena que representa la enumeración.

### <a name="posting-enum-values"></a>Publicar valores de enumeración

Normalmente, los formularios HTML se utilizan para enviar datos al servidor. En el código siguiente se muestran las versiones `HTTP GET` y `HTTP POST` del método `SelectCategoryEnumPost`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample7.cs)]

Al pasar una enumeración `eMovieCategories` al método `POST`, podemos extraer el valor de enumeración y la cadena de enumeración. Ejecute el ejemplo y navegue hasta la página de prueba. Haga clic en el vínculo `Select Movie Category(Enum Post)`. Seleccione un tipo de película y, a continuación, presione el botón Enviar. La pantalla muestra el valor y el nombre del tipo de película.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image7.png)

### <a name="creating-a-multiple-section-select-element"></a>Crear un elemento Select de varias secciones

La aplicación auxiliar HTML de [ListBox](https://msdn.microsoft.com/library/system.web.mvc.html.selectextensions.listbox.aspx) representa el elemento HTML `<select>` con el atributo `multiple`, que permite a los usuarios realizar selecciones múltiples. Navegue hasta el vínculo de prueba y, a continuación, seleccione el vínculo **Seleccionar país de selección múltiple** . La interfaz de usuario representada permite seleccionar varios países. En la imagen siguiente, se seleccionan Canadá y China.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image8.png)

### <a name="examining-the-multiselectcountry-code"></a>Examinar el código de MultiSelectCountry

Examine el siguiente código del archivo *Controllers\HomeController.CS* .

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample8.cs)]

El método `GetCountries` crea una lista de países y, a continuación, lo pasa al constructor `MultiSelectList`. La sobrecarga del constructor `MultiSelectList` utilizada en el método `GetCountries` anterior toma cuatro parámetros:

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample9.cs)]

1. *elementos*: objeto [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior, la lista de países.
2. *dataValueField*: el nombre de la propiedad en la lista **IEnumerable** que contiene el valor. En el ejemplo anterior, la propiedad `ID`.
3. *DataTextField*: el nombre de la propiedad en la lista **IEnumerable** que contiene la información que se va a mostrar. En el ejemplo anterior, la propiedad `name`.
4. *selectedValues*: la lista de valores seleccionados.

En el ejemplo anterior, el método `MultiSelectCountry` pasa un valor `null` para los países seleccionados, de modo que no se selecciona ningún país cuando se muestra la interfaz de usuario. En el código siguiente se muestra el marcado de Razor que se usa para representar la vista `MultiSelectCountry`.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample10.cshtml)]

El método de [cuadro de lista](https://msdn.microsoft.com/library/dd470200.aspx) auxiliar HTML utilizado anteriormente toma dos parámetros, el nombre de la propiedad para el enlace de modelos y el [MultiSelectList](https://msdn.microsoft.com/library/system.web.mvc.multiselectlist.aspx) que contiene las opciones y los valores de Select. El código `ViewBag.YouSelected` anterior se usa para mostrar los valores de los países que ha seleccionado al enviar el formulario. Examine la sobrecarga HTTP POST del método `MultiSelectCountry`.

[!code-csharp[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample11.cs)]

La propiedad dinámica `ViewBag.YouSelected` contiene los países seleccionados obtenidos para la entrada `Countries` en la colección de formularios. En esta versión, se pasa al método GetCountries una lista de los países seleccionados, por lo que cuando se muestra la vista `MultiSelectCountry`, los países seleccionados se seleccionan en la interfaz de usuario.

### <a name="making-a-select-element-friendly-with-the-harvest-chosen-jquery-plugin"></a>Crear un elemento de selección descriptivo con el complemento jQuery elegido de la cosecha

El complemento jQuery [elegido](https://harvesthq.github.com/chosen/) de la cosecha se puede Agregar a un HTML &lt;seleccione&gt; elemento para crear una interfaz de usuario fácil de usar. En las imágenes siguientes se muestra el complemento jQuery [elegido](https://harvesthq.github.com/chosen/) de la cosecha con `MultiSelectCountry` vista.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image9.png)

En las dos imágenes siguientes, se selecciona **Canadá** .

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image10.png)

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image11.png)

En la imagen anterior, se selecciona Canadá y contiene una **x** en la que se puede hacer clic para quitar la selección. La imagen siguiente muestra los Canadá, China y Japón seleccionados.

![](using-the-dropdownlist-helper-with-aspnet-mvc/_static/image12.png)

### <a name="hooking-up-the-harvest-chosen-jquery-plugin"></a>Enlazar el complemento jQuery elegido de la cosecha

La siguiente sección es más fácil de seguir si tiene alguna experiencia con jQuery. Si nunca ha usado jQuery antes, es posible que desee probar uno de los siguientes tutoriales de jQuery.

- [Cómo funciona jQuery](http://docs.jquery.com/Tutorials:How_jQuery_Works) en [John Resig](http://ejohn.org/)
- [Introducción con jQuery](http://docs.jquery.com/Tutorials:Getting_Started_with_jQuery) por [Jörn Zaefferer](http://bassistance.de/)
- [Ejemplos en directo de jQuery](http://codylindley.com/blogstuff/js/jquery/#) por [Cody Lindley](http://codylindley.com/)

El complemento elegido se incluye en los proyectos de ejemplo de inicio y finalización que acompañan a este tutorial. En este tutorial, solo necesitará usar jQuery para enlazarlo a la interfaz de usuario. Para usar el complemento jQuery elegido de la cosecha en un proyecto de ASP.NET MVC, debe:

1. Descargue el complemento elegido de [GitHub](https://github.com/harvesthq/chosen/). Este paso se ha realizado automáticamente.
2. Agregue la carpeta elegida al proyecto de ASP.NET MVC. Agregue los recursos del complemento elegido que descargó en el paso anterior a la carpeta elegida. Este paso se ha realizado automáticamente.
3. Enlazar el complemento elegido a la aplicación auxiliar HTML **DropDownList** o **ListBox** .

### <a name="hooking-up-the-chosen-plugin-to-the-multiselectcountry-view"></a>Enlazar el complemento elegido a la vista MultiSelectCountry.

Abra el archivo *Views\Home\MultiSelectCountry.cshtml* y agregue un parámetro `htmlAttributes` a la `Html.ListBox`. El parámetro que va a agregar contiene un nombre de clase para la lista de selección (`@class = "chzn-select"`). A continuación se muestra el código completado:

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample12.cshtml)]

En el código anterior, agregamos el atributo HTML y el valor de atributo `class = "chzn-select"`. El carácter \@ anterior clase no tiene nada que ver con el motor de vistas de Razor. `class` es una [ C# palabra clave](https://msdn.microsoft.com/library/x53a06bb.aspx). C#las palabras clave no se pueden usar como identificadores a menos que incluyan \@ como prefijo. En el ejemplo anterior, `@class` es un identificador válido pero la **clase** no es porque la **clase** es una palabra clave.

Agregue referencias a los archivos elegidos u elegidos. *jQuery. js* y elegidos */elegidos. CSS* . El *elegido u elegido. jQuery. js* e implementa funcionalmente el complemento elegido. El archivo *. CSS elegido u* elegido proporciona el estilo. Agregue estas referencias a la parte inferior del archivo *Views\Home\MultiSelectCountry.cshtml* . En el código siguiente se muestra cómo hacer referencia al complemento elegido.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample13.cshtml)]

Active el complemento elegido con el nombre de clase usado en el código **HTML. ListBox** . En el ejemplo anterior, el nombre de la clase es `chzn-select`. Agregue la siguiente línea a la parte inferior del archivo de vista *Views\Home\MultiSelectCountry.cshtml* . Esta línea activa el complemento elegido.

[!code-html[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample14.html)]

La siguiente línea es la sintaxis para llamar a la función preparada de jQuery, que selecciona el elemento DOM con el nombre de clase `chzn-select`.

[!code-powershell[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample15.ps1)]

A continuación, el conjunto ajustado devuelto por la llamada anterior aplica el método elegido (`.chosen();`), que enlaza el complemento elegido.

En el código siguiente se muestra el archivo de vista *Views\Home\MultiSelectCountry.cshtml* completado.

[!code-cshtml[Main](using-the-dropdownlist-helper-with-aspnet-mvc/samples/sample16.cshtml)]

Ejecute la aplicación y vaya a la vista `MultiSelectCountry`. Intente agregar y eliminar países. La descarga de ejemplo proporcionada también contiene un método `MultiCountryVM` y una vista que implementa la funcionalidad de MultiSelectCountry con un modelo de vista en lugar de **ViewBag**.

En la sección siguiente verá cómo funciona el mecanismo de scaffolding de ASP.NET MVC con la aplicación auxiliar de **DropDownList** .

> [!div class="step-by-step"]
> [Siguiente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
