---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: b3249397e54e64538c4dc78e5fe8b94656e8962b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457900"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 3

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.NET.

## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

En esta sección, creará una clase de dirección y aprenderá a crear una plantilla para mostrarla.

En la carpeta *Models* , cree un nuevo archivo de clase denominado *Person.CS* donde colocará dos tipos: una clase `Person` y una clase `Address`. La clase `Person` contendrá una propiedad con el tipo `Address`. El tipo de `Address` es un tipo complejo, lo que significa que no es uno de los tipos integrados como `int`, `string`o `double`. En su lugar, tiene varias propiedades. El código de las nuevas clases tiene el siguiente aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

En el controlador de `Movie`, agregue la siguiente acción de `PersonDetail` para mostrar una instancia de Person:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Después, agregue el código siguiente al controlador de `Movie` para rellenar el modelo de `Person` con algunos datos de ejemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra el archivo *Views\Movies\PersonDetail.cshtml* y agregue el marcado siguiente para la vista `PersonDetail`.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Presione Ctrl + F5 para ejecutar la aplicación y vaya a *Movies/PersonDetail*.

La vista `PersonDetail` no contiene el `Address` tipo complejo, como puede ver en esta captura de pantalla. (No se muestra ninguna dirección).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

No se muestran los datos del modelo de `Address` porque es un tipo complejo. Para mostrar la información de la dirección, vuelva a abrir el archivo *Views\Movies\PersonDetail.cshtml* y agregue el marcado siguiente.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

El marcado completo de la vista `PersonDetail` Now tiene el siguiente aspecto:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Vuelva a ejecutar la aplicación y muestre la vista `PersonDetail`. Ahora se muestra la información de la dirección:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Crear una plantilla para un tipo complejo

En esta sección, creará una plantilla que se usará para representar el `Address` tipo complejo. Al crear una plantilla para el tipo de `Address`, ASP.NET MVC puede usarla automáticamente para dar formato a un modelo de dirección en cualquier parte de la aplicación. Esto le ofrece una manera de controlar la representación del tipo de `Address` desde un solo lugar de la aplicación.

En la carpeta *Views\Shared\DisplayTemplates.* , cree una vista parcial fuertemente tipada denominada **Address**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Haga clic en **Agregar**y, a continuación, abra el nuevo archivo *Views\Shared\DisplayTemplates\Address.cshtml* . La nueva vista contiene el siguiente marcado generado:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Ejecute la aplicación y muestre la vista `PersonDetail`. Esta vez, la plantilla de `Address` que acaba de crear se usa para mostrar el `Address` tipo complejo, por lo que la presentación tiene el siguiente aspecto:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumen: formas de especificar el formato de presentación del modelo y la plantilla

Ha visto que puede especificar el formato o la plantilla para una propiedad del modelo mediante los siguientes enfoques:

- Aplicar el atributo `DisplayFormat` a una propiedad del modelo. Por ejemplo, el código siguiente hace que se muestre la fecha sin la hora:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicar un atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) a una propiedad en el modelo y especificar el tipo de datos. Por ejemplo, el código siguiente hace que se muestre la fecha sin la hora.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Si la aplicación contiene una plantilla *Date. cshtml* en la carpeta *Views\Shared\DisplayTemplates.* o en la carpeta *Views\Movies\DisplayTemplates* , esa plantilla se utilizará para representar la propiedad `DateTime`. De lo contrario, el sistema integrado de plantillas de ASP.NET muestra la propiedad como una fecha.
- Crear una plantilla de presentación en la carpeta *Views\Shared\DisplayTemplates.* o en la carpeta *Views\Movies\DisplayTemplates* cuyo nombre coincida con el tipo de datos al que desea dar formato. Por ejemplo, vio que el *Views\Shared\DisplayTemplates\DateTime.cshtml* se usó para representar las propiedades de `DateTime` en un modelo, sin agregar un atributo al modelo y sin agregar ningún marcado a las vistas.
- Usar el atributo [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) en el modelo para especificar la plantilla para mostrar la propiedad del modelo.
- Agregar explícitamente el nombre de la plantilla para mostrar a la llamada [HTML. DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) en una vista.

El método que use dependerá de lo que necesite hacer en la aplicación. No es raro mezclar estos enfoques para obtener exactamente el tipo de formato que necesita.

En la siguiente sección, cambiará los engranajes un poco y pasará de la personalización de la forma en que se muestran los datos para personalizar cómo se escriben. Enlazará el DatePicker de jQuery a las vistas de edición de la aplicación para proporcionar una manera sencilla de especificar fechas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
