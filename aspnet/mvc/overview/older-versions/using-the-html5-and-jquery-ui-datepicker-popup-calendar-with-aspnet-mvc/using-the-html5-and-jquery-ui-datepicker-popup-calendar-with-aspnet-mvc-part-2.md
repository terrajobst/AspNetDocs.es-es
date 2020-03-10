---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 2 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 325cc90eb6e717c47863eda6253e0d48d796386b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498421"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 2

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.NET.

## <a name="adding-an-automatic-datetime-template"></a>Agregar una plantilla de fecha y hora automática

En la primera parte de este tutorial, ha visto cómo puede agregar atributos al modelo para especificar explícitamente el formato y cómo puede especificar explícitamente la plantilla que se usa para representar el modelo. Por ejemplo, el atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) del código siguiente especifica explícitamente el formato de la propiedad `ReleaseDate`.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

En el ejemplo siguiente, el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , mediante la enumeración `Date`, especifica que la plantilla de fecha debe usarse para representar el modelo. Si no hay ninguna plantilla de fecha en el proyecto, se usa la plantilla de fecha integrada.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

Sin embargo, ASP. MVC puede realizar la búsqueda de coincidencias de tipos mediante la Convención sobre la configuración, buscando una plantilla que coincida con el nombre de un tipo. Esto le permite crear una plantilla que aplica formato a los datos automáticamente sin usar ningún atributo o código. En esta parte del tutorial, creará una plantilla que se aplica automáticamente a las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). No necesitará usar un atributo u otra configuración para especificar que la plantilla se debe utilizar para representar todas las propiedades del modelo de tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

También aprenderá una manera de personalizar la presentación de propiedades individuales o incluso de campos individuales.

Para empezar, vamos a quitar la información de formato existente y mostrar las fechas completas en la aplicación.

Abra el archivo *Movie.CS* y comente el atributo `DataType` en la propiedad `ReleaseDate`:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Presione CTRL+F5 para ejecutar la aplicación.

Observe que la propiedad `ReleaseDate` ahora muestra la fecha y la hora, ya que es el valor predeterminado cuando no se proporciona información de formato.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Agregar estilos CSS para probar nuevas plantillas

Antes de crear una plantilla para dar formato a las fechas, agregará algunas reglas de estilo CSS que puede aplicar a las nuevas plantillas. Esto le ayudará a comprobar que la página representada usa la nueva plantilla.

Abra el archivo *Content\Site.CS*s y agregue las siguientes reglas CSS en la parte inferior del archivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Agregar plantillas de visualización de fecha y hora

Ahora puede crear la nueva plantilla. En la carpeta *Views\Movies* , cree una carpeta *DisplayTemplates* .

En la carpeta *Views\Shared* , cree una carpeta *DisplayTemplates* y una carpeta *EditorTemplates* .

Todas las plantillas de presentación de la carpeta *Views\Shared\DisplayTemplates.* se usarán en todos los controladores. Las plantillas para mostrar de la carpeta *Views\Movie\DisplayTemplates* solo las usará el controlador de `Movie`. (Si en ambas carpetas aparece una plantilla con el mismo nombre, la plantilla de la carpeta *Views\Movie\DisplayTemplates* (es decir, la plantilla más específica) tiene prioridad para las vistas devueltas por el controlador `Movie`).

En **Explorador de soluciones**, expanda la carpeta *vistas* , expanda la carpeta *compartida* y, a continuación, haga clic con el botón secundario en la carpeta *Views\Shared\DisplayTemplates.*

Haga clic en **Agregar** y, a continuación, en **Ver**. Se muestra el cuadro de diálogo **Agregar vista** .

En el cuadro **nombre de vista** , escriba `DateTime`. (Debe usar este nombre para que coincida con el nombre del tipo).

Active la casilla **crear como vista parcial** . Asegúrese de que las casillas **usar un diseño o página maestra** y **crear una vista fuertemente tipada** no estén seleccionadas.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Haga clic en **Agregar**. Una plantilla *DateTime. cshtml* se crea en *Views\Shared\DisplayTemplates.* .

En la imagen siguiente se muestra la carpeta *views* en **Explorador de soluciones** después de crear las plantillas de presentación de `DateTime` y editor.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra el archivo *Views\Shared\DisplayTemplates\DateTime.cshtml* y agregue el marcado siguiente, que utiliza el método [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) para dar formato a la propiedad como una fecha sin la hora. (El formato de `{0:d}` especifica el formato de fecha abreviado).

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita este paso para crear una plantilla de `DateTime` en la carpeta *Views\Movie\DisplayTemplates* . Use el siguiente código en el archivo *Views\Movie\DisplayTemplates\DateTime.cshtml* .

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

La clase `loud-1` CSS hace que la fecha se muestre en texto rojo en negrita. Ha agregado la clase `loud-1` CSS como medida temporal para que pueda ver fácilmente Cuándo se está usando esta plantilla determinada.

Se crearán las plantillas personalizadas que ASP.NET usará para mostrar las fechas. La plantilla más general (en la carpeta *Views\Shared\DisplayTemplates.* ) muestra una fecha corta simple. La plantilla específica del controlador de `Movie` (en la carpeta *Views\Movies\DisplayTemplates* ) muestra una fecha corta que también tiene el formato de texto rojo en negrita.

Presione CTRL+F5 para ejecutar la aplicación. El explorador representa la vista de índice de la aplicación.

La propiedad `ReleaseDate` muestra ahora la fecha en una fuente de color rojo en negrita sin la hora. Esto le ayudará a confirmar que la aplicación auxiliar con plantilla `DateTime` en la carpeta *Views\Movies\DisplayTemplates* está seleccionada en la aplicación auxiliar de `DateTime` plantilla en la carpeta compartida (*Views\Shared\DisplayTemplates.* ).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Ahora, cambie el nombre del archivo *Views\Movies\DisplayTemplates\DateTime.cshtml* a *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Presione CTRL+F5 para ejecutar la aplicación.

Esta vez, la propiedad `ReleaseDate` muestra una fecha sin la hora y sin la fuente roja en negrita. Esto muestra que una plantilla con el nombre del tipo de datos (en este caso `DateTime`) se usa automáticamente para mostrar todas las propiedades del modelo de ese tipo. Después de cambiar el nombre del archivo *DateTime. cshtml* a *LoudDateTime. cshtml*, ASP.net ya no encontró una plantilla en la carpeta *Views\Movies\DisplayTemplates* , por lo que usó la plantilla *DateTime. cshtml* de la carpeta * Views\Movies\Shared\*.

(La coincidencia de plantillas no distingue entre mayúsculas y minúsculas, por lo que podría haber creado el nombre del archivo de plantilla con cualquier grafía. Por ejemplo, *DateTime. cshtml, DateTime. cshtml*y *DateTime. cshtml* coincidirían con el tipo de `DateTime`).

Para revisar: en este punto, el campo de `ReleaseDate` se muestra mediante la plantilla *Views\Movies\DisplayTemplates\DateTime.cshtml* , que muestra los datos con un formato de fecha corta, pero de lo contrario no agrega ningún formato especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usar UIHint para especificar una plantilla para mostrar

Si la aplicación web tiene muchos campos de `DateTime` y, de forma predeterminada, desea mostrar todos o más en formato de solo fecha, la plantilla *DateTime. cshtml* es un buen enfoque. Pero, ¿qué ocurre si tiene unas cuantas fechas en las que desea mostrar la fecha y la hora completas? No hay problema. Puede crear una plantilla adicional y usar el atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) para especificar el formato de la fecha y hora completas. Después, puede aplicar selectivamente esa plantilla. Puede usar el atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) en el nivel de modelo o puede especificar la plantilla dentro de una vista. En esta sección, verá cómo usar el atributo `UIHint` para cambiar selectivamente el formato de algunas instancias de campos de fecha y hora.

Abra el archivo *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* y reemplace el código existente por lo siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Esto hace que se muestren la fecha y la hora completas y agrega la clase CSS que hace que el texto sea verde y grande.

Abra el archivo *Movie.CS* y agregue el atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) a la propiedad `ReleaseDate`, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Esto indica a ASP.NET MVC que, cuando muestra la propiedad `ReleaseDate` (concretamente, y no solo un objeto `DateTime`), debe usar la plantilla *LoudDateTime. cshtml* .

Presione CTRL+F5 para ejecutar la aplicación.

Observe que la propiedad `ReleaseDate` ahora muestra la fecha y la hora en una fuente verde grande.

Vuelva al atributo `UIHint` del archivo *Movie.CS* y comente que no se usará la plantilla *LoudDateTime. cshtml* . Vuelva a ejecutar la aplicación. La fecha de lanzamiento no se muestra grande ni verde. Esto comprueba que la plantilla *Views\Shared\DisplayTemplates\DateTime.cshtml* se usa en las vistas de índice y detalles.

Como se mencionó anteriormente, también puede aplicar una plantilla en una vista, lo que le permite aplicar la plantilla a una instancia individual de algunos datos. Abra la vista *Views\Movies\Details.cshtml* . Agregue `"LoudDateTime"` como segundo parámetro de la llamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) para el campo `ReleaseDate`. El código completo es similar al siguiente:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Esto especifica que se debe usar la plantilla de `LoudDateTime` para mostrar la propiedad del modelo, independientemente de los atributos que se apliquen al modelo.

Presione CTRL+F5 para ejecutar la aplicación.

Compruebe que la página de índice de la película esté usando la plantilla *Views\Shared\DisplayTemplates\DateTime.cshtml* (negrita roja) y que la página *Movie\Details* use la plantilla *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* (grande y verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

En la sección siguiente, creará una plantilla para un tipo complejo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
