---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 583e782641efea9a9517edb31f7718b28203d756
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433255"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.NET.

### <a name="adding-a-template-for-editing-dates"></a>Agregar una plantilla para editar fechas

En esta sección, creará una plantilla para editar las fechas que se aplicarán cuando ASP.NET MVC muestre la interfaz de usuario para editar las propiedades del modelo marcadas con la enumeración **Date** del atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) . La plantilla solo representará la fecha; no se mostrará la hora. En la plantilla, usará el calendario emergente DatePicker de la [interfaz de usuario de jQuery](http://jqueryui.com/demos/datepicker/) para proporcionar una manera de editar fechas.

Para empezar, abra el archivo *Movie.CS* y agregue el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) con la enumeración **Date** a la propiedad `ReleaseDate`, como se muestra en el código siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Este código hace que el campo de `ReleaseDate` se muestre sin el tiempo en las plantillas para mostrar y en las plantillas de edición. Si la aplicación contiene una plantilla *Date. cshtml* en la carpeta *Views\Shared\EditorTemplates* o en la carpeta *Views\Movies\EditorTemplates* , esa plantilla se utilizará para representar cualquier `DateTime` propiedad mientras se edita. De lo contrario, el sistema integrado de plantillas de ASP.NET mostrará la propiedad como una fecha.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición para comprobar que el campo de entrada de la fecha de lanzamiento muestra solo la fecha.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

En **Explorador de soluciones**, expanda la carpeta *vistas* , expanda la carpeta *compartida* y, a continuación, haga clic con el botón secundario en la carpeta *Views\Shared\EditorTemplates*

Haga clic en **Agregar**y, a continuación, en **Ver**. Se muestra el cuadro de diálogo **Agregar vista** .

En el cuadro **nombre de vista** , escriba &quot;fecha&quot;.

Active la casilla **crear como vista parcial** . Asegúrese de que las casillas **usar un diseño o página maestra** y **crear una vista fuertemente tipada** no estén seleccionadas.

Haga clic en **Agregar**. Se crea la plantilla *Views\Shared\EditorTemplates\Date.cshtml* .

Agregue el código siguiente a la plantilla *Views\Shared\EditorTemplates\Date.cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

La primera línea declara el modelo para que sea un tipo de `DateTime`. Aunque no es necesario declarar el tipo de modelo en las plantillas de edición y visualización, es un procedimiento recomendado para obtener la comprobación en tiempo de compilación del modelo que se pasa a la vista. (Otra ventaja es que, a continuación, obtiene IntelliSense para el modelo en la vista de Visual Studio). Si no se declara el tipo de modelo, ASP.NET MVC lo considera un tipo [dinámico](https://msdn.microsoft.com/library/dd264741.aspx) y no hay ninguna comprobación de tipos en tiempo de compilación. Si declara que el modelo es de tipo `DateTime`, se vuelve fuertemente tipado.

La segunda línea es simplemente un marcado HTML literal que muestra &quot;mediante una plantilla de fecha&quot; antes de un campo de fecha. Usará esta línea temporalmente para comprobar que se está usando esta plantilla de fecha.

La siguiente línea es una aplicación auxiliar [HTML. TextBox](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) que representa un campo de `input` que es un cuadro de texto. El tercer parámetro del ayudante usa un tipo anónimo para establecer la clase del cuadro de texto que se va a `datefield` y el tipo que se va a `date`. (Dado que `class` es un reservado C#en, debe usar el carácter `@` para escapar el atributo `class` en el C# analizador).

El tipo de `date` es un tipo de entrada de HTML5 que permite que los exploradores compatibles con HTML5 representen un control de calendario de HTML5. Más adelante, agregará JavaScript para enlazar el DatePicker de jQuery al elemento `Html.TextBox` con la clase `datefield`.

Presione CTRL+F5 para ejecutar la aplicación. Puede comprobar que la propiedad `ReleaseDate` de la vista edición está usando la plantilla editar porque la plantilla muestra &quot;mediante la plantilla de fecha&quot; justo antes del cuadro de entrada texto `ReleaseDate`, como se muestra en esta imagen:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

En el explorador, vea el origen de la página. (Por ejemplo, haga clic con el botón secundario en la página y seleccione **Ver código fuente**). En el ejemplo siguiente se muestra parte del marcado de la página, que ilustra los atributos `class` y `type` en el HTML representado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Vuelva a la plantilla *Views\Shared\EditorTemplates\Date.cshtml* y quite el &quot;mediante el marcado de la plantilla de fecha&quot;. Ahora la plantilla completada tiene el siguiente aspecto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Adición de un calendario emergente DatePicker de jQuery UI con NuGet

En esta sección, agregará el calendario emergente DatePicker de la [interfaz de usuario de jQuery](http://jqueryui.com/demos/datepicker/) a la plantilla de fecha y edición. La biblioteca de [jQuery UI](http://jqueryui.com/) proporciona compatibilidad con animación, efectos avanzados y widgets personalizables. Se basa en la biblioteca de jQuery JavaScript. El calendario emergente DatePicker facilita y natural la entrada de fechas mediante un calendario en lugar de escribir una cadena. El calendario emergente también limita a los usuarios a fechas legales: la entrada de texto normal para una fecha le permite escribir algo como `2/33/1999` (febrero 33rd, 1999), pero el calendario emergente DatePicker de la [interfaz de usuario de jQuery](http://jqueryui.com/demos/datepicker/) no lo permite.

En primer lugar, debe instalar las bibliotecas de jQuery UI. Para ello, usará NuGet, que es un administrador de paquetes que se incluye en las versiones SP1 de Visual Studio 2010 y Visual Web Developer.

En Visual Web Developer, en el menú **herramientas** , seleccione **Administrador de paquetes Nuget** y, a continuación, seleccione **administrar paquetes Nuget**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Nota: Si el menú **herramientas** no muestra el comando **Administrador de paquetes Nuget** , debe instalar Nuget siguiendo las instrucciones de la página [instalación de NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) del sitio web de Nuget.   
  
Si usa Visual Studio en lugar de Visual Web Developer, en el menú **herramientas** , seleccione Administrador de **paquetes NuGet** y, a continuación, seleccione **Agregar referencia de paquete de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

En el cuadro de diálogo **MVCMovie-Manage NuGet Packages** , haga clic en la pestaña **en línea** de la izquierda y, a continuación, escriba &quot;jQuery. UI&quot; en el cuadro de búsqueda. Seleccione j **query UI widgets: DatePicker**y, luego, seleccione el botón **instalar** .

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet agrega estas versiones de depuración y las versiones de reducida de jQuery UI Core y el selector de fecha de la interfaz de usuario de jQuery al proyecto:

- *jQuery. UI. Core. js*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. js*
- *jQuery. UI. DatePicker. min. js*

Nota: las versiones de depuración (los archivos sin la extensión *. min. js* ) son útiles para la depuración, pero en un sitio de producción, solo incluiría las versiones de reducida.

Para usar el selector de fecha de jQuery, debe crear un script de jQuery que enlazará el widget de calendario a la plantilla de edición. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *scripts* y seleccione **Agregar**, **nuevo elemento**y, a continuación, **archivo JScript**. Asigne al archivo el nombre *DatePickerReady. js*.

Agregue el código siguiente al archivo *DatePickerReady. js* :

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Si no está familiarizado con jQuery, esta es una breve explicación de lo que hace esto: la primera línea es la &quot;función de&quot; preparada para jQuery, a la que se llama cuando todos los elementos DOM de una página se han cargado. La segunda línea selecciona todos los elementos DOM que tienen el nombre de clase `datefield`y, a continuación, invoca la función de `datepicker` para cada uno de ellos. (Recuerde que ha agregado la clase `datefield` a la plantilla *Views\Shared\EditorTemplates\Date.cshtml* anteriormente en el tutorial).

A continuación, abra el archivo *Views\Shared\\_Layout. cshtml* . Debe agregar referencias a los siguientes archivos, que son necesarios para que pueda usar el selector de fecha:

- *Content/themes/base/jQuery. UI. Core. CSS*
- *Content/themes/base/jQuery. UI. DatePicker. CSS*
- *Content/themes/base/jQuery. UI. theme. CSS*
- *jQuery. UI. Core. min. js*
- *jQuery. UI. DatePicker. min. js*
- *DatePickerReady. js*

En el ejemplo siguiente se muestra el código real que se debe agregar en la parte inferior del elemento `head` en el archivo *Views\Shared\\_Layout. cshtml* .

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Aquí se muestra la sección `head` completa:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

El método auxiliar de contenido de la [dirección URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) convierte la ruta de acceso al recurso en una ruta de acceso absoluta. Debe utilizar `@URL.Content` para hacer referencia correctamente a estos recursos cuando la aplicación se ejecuta en IIS.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione un vínculo de edición y, a continuación, coloque el punto de inserción en el campo **ReleaseDate** . Se muestra el calendario emergente de la interfaz de usuario de jQuery.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Como la mayoría de los controles de jQuery, el DatePicker le permite personalizarlo en gran medida. Para obtener más información, vea [Personalización visual: diseñar un tema de jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) en el sitio de [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) .

### <a name="supporting-the-html5-date-input-control"></a>Compatibilidad con el control de entrada de fecha HTML5

A medida que más exploradores admitan HTML5, querrá usar la entrada nativa de HTML5, como el elemento de entrada `date` y no usar el calendario de jQuery UI. Puede agregar lógica a la aplicación para usar automáticamente los controles HTML5 si el explorador los admite. Para ello, reemplace el contenido del archivo *DatePickerReady. js* por lo siguiente:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

La primera línea de este script usa Modernizr para comprobar que se admite la entrada de fecha de HTML5. Si no se admite, el selector de fecha de la interfaz de usuario de jQuery se enlaza en su lugar. ([Modernizr](http://www.modernizr.com/docs/) es una biblioteca JavaScript de código abierto que detecta la disponibilidad de las implementaciones nativas de HTML5 y CSS3. Modernizr se incluye en cualquier nuevo proyecto de ASP.NET MVC que cree).

Después de realizar este cambio, puede probarlo mediante un explorador que admita HTML5, como Opera 11. Ejecute la aplicación mediante un explorador compatible con HTML5 y edite una entrada de película. El control de fecha de HTML5 se usa en lugar del calendario emergente de la interfaz de usuario de jQuery:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Dado que las nuevas versiones de los exploradores implementan HTML5 de forma incremental, un buen método para ahora es agregar código a su sitio web que admita una amplia variedad de compatibilidad con HTML5. Por ejemplo, a continuación se muestra un script de *DatePickerReady. js* más sólido que permite que el sitio admita exploradores que solo admiten parcialmente el control de fecha de HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Este script selecciona los elementos de `input` HTML5 de tipo `date` que no son totalmente compatibles con el control de fecha de HTML5. Para esos elementos, enlaza el calendario emergente de la interfaz de usuario de jQuery y, a continuación, cambia el atributo `type` de `date` a `text`. Al cambiar el atributo `type` de `date` a `text`, se elimina la compatibilidad parcial de la fecha de HTML5. Puede encontrar un script de *DatePickerReady. js* aún más sólido en [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Agregar fechas que aceptan valores NULL a las plantillas

Si usa una de las plantillas de fecha existentes y pasa una fecha nula, obtendrá un error en tiempo de ejecución. Para que las plantillas de fecha sean más sólidas, las cambiará para controlar valores NULL. Para admitir fechas que aceptan valores NULL, cambie el código de *Views\Shared\DisplayTemplates\DateTime.cshtml* por lo siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

El código devuelve una cadena vacía cuando el modelo es **null**.

Cambie el código del archivo *Views\Shared\EditorTemplates\Date.cshtml* por lo siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Cuando se ejecuta este código, si el modelo no es null, se usa el valor de `DateTime` del modelo. Si el modelo es null, se utiliza la fecha actual en su lugar.

### <a name="wrapup"></a>Wrapup

En este tutorial se han tratado los aspectos básicos de las aplicaciones auxiliares con plantilla de ASP.NET y se muestra cómo usar el calendario emergente DatePicker de la interfaz de usuario de jQuery en una aplicación ASP.NET MVC. Para obtener más información, pruebe estos recursos:

- Para obtener información sobre la localización, consulte el blog de Rajeesh [JQueryUI DatePicker en ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obtener información sobre jQuery UI, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obtener información sobre cómo localizar el control DatePicker, consulte [UI/DatePicker/Localization](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obtener más información acerca de las plantillas de ASP.NET MVC, consulte la serie de blog de Brad Wilson en [las plantillas de ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Aunque la serie se ha escrito para ASP.NET MVC 2, el material se aplica también a la versión actual de ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
