---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Crear aplicaciones auxiliares HTML personalizadas (C#) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC. Aprovechando la aplicación auxiliar HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594522"
---
# <a name="creating-custom-html-helpers-c"></a>Crear asistentes de HTML personalizados (C#)

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC. Al aprovechar las aplicaciones auxiliares HTML, puede reducir la cantidad de tipos de etiquetas HTML tediosas que debe realizar para crear una página HTML estándar.

El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC. Al aprovechar las aplicaciones auxiliares HTML, puede reducir la cantidad de tipos de etiquetas HTML tediosas que debe realizar para crear una página HTML estándar.

En la primera parte de este tutorial, se describen algunas de las aplicaciones auxiliares HTML existentes incluidas con el marco de MVC de ASP.NET. A continuación, se describen dos métodos para crear aplicaciones auxiliares HTML personalizadas: se explica cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método estático y la creación de un método de extensión.

## <a name="understanding-html-helpers"></a>Descripción de las aplicaciones auxiliares HTML

Una aplicación auxiliar HTML es simplemente un método que devuelve una cadena. La cadena puede representar cualquier tipo de contenido que desee. Por ejemplo, puede usar aplicaciones auxiliares HTML para representar etiquetas HTML estándar, como HTML `<input>` y etiquetas de `<img>`. También puede usar aplicaciones auxiliares HTML para representar contenido más complejo, como una franja de pestañas o una tabla HTML de datos de base de datos.

El marco de MVC de ASP.NET incluye el siguiente conjunto de aplicaciones auxiliares estándar de HTML (esta no es una lista completa):

- HTML. ActionLink ()
- HTML. BeginForm ()
- HTML. CheckBox ()
- HTML. DropDownList ()
- HTML. EndForm ()
- HTML. Hidden ()
- HTML. ListBox ()
- HTML. Password ()
- HTML. RadioButton ()
- HTML. TextArea ()
- HTML. TextBox ()

Por ejemplo, considere el formulario de la lista 1. Este formulario se representa con la ayuda de dos de las aplicaciones auxiliares de HTML estándar (vea la ilustración 1). Este formulario utiliza los métodos auxiliares `Html.BeginForm()` y `Html.TextBox()` para representar un formulario HTML sencillo.

[![página representada con aplicaciones auxiliares HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: página representada con aplicaciones auxiliares HTML ([haga clic para ver la imagen de tamaño completo](creating-custom-html-helpers-cs/_static/image3.png))

**Lista 1: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

El método auxiliar HTML. BeginForm () se usa para crear las etiquetas HTML `<form>` de apertura y cierre. Observe que se llama al método `Html.BeginForm()` dentro de una instrucción using. La instrucción using garantiza que la etiqueta de `<form>` se cierre al final del bloque using.

Si lo prefiere, en lugar de crear un bloque Using, puede llamar al método auxiliar HTML. EndForm () para cerrar la etiqueta `<form>`. Use cualquier enfoque para crear una etiqueta de `<form>` de apertura y cierre que le parezca más intuitiva.

Los métodos auxiliares de `Html.TextBox()` se usan en la enumeración 1 para representar etiquetas HTML `<input>`. Si selecciona ver código fuente en el explorador, verá el origen HTML en la lista 2. Observe que el origen contiene etiquetas HTML estándar.

> [!IMPORTANT]
> Tenga en cuenta que la aplicación auxiliar `Html.TextBox()`-HTML se representa con `<%= %>` etiquetas en lugar de `<% %>` etiquetas. Si no incluye el signo igual, no se representa nada en el explorador.

El marco de MVC de ASP.NET contiene un pequeño conjunto de aplicaciones auxiliares. Lo más probable es que necesite extender el marco de MVC con aplicaciones auxiliares HTML personalizadas. En el resto de este tutorial, aprenderá dos métodos de creación de aplicaciones auxiliares HTML personalizadas.

**Lista 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Crear aplicaciones auxiliares HTML con métodos estáticos

La forma más fácil de crear una aplicación auxiliar HTML es crear un método estático que devuelva una cadena. Imagine, por ejemplo, que decide crear una nueva aplicación auxiliar HTML que representa una etiqueta de `<label>` HTML. Puede usar la clase en la lista 2 para representar un `<label>`.

**Lista 2: `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

No hay nada especial sobre la clase en la lista 2. El método `Label()` simplemente devuelve una cadena.

La vista de índice modificada de la lista 3 utiliza el `LabelHelper` para representar etiquetas HTML `<label>`. Observe que la vista incluye una directiva de `<%@ imports %>` que importa el espacio de nombres `Application1.Helpers`.

**Lista 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Crear aplicaciones auxiliares HTML con métodos de extensión

Si desea crear aplicaciones auxiliares HTML que funcionen igual que las aplicaciones auxiliares estándar de HTML incluidas en el marco de MVC de ASP.NET, debe crear métodos de extensión. Los métodos de extensión permiten agregar nuevos métodos a una clase existente. Al crear un método auxiliar HTML, se agregan nuevos métodos a la clase HtmlHelper representada por la propiedad HTML de una vista.

La clase de la lista 3 agrega un método de extensión a la clase `HtmlHelper` denominada `Label()`. Hay un par de cosas que debe tener en cuenta sobre esta clase. En primer lugar, observe que la clase es una clase estática. Debe definir un método de extensión con una clase estática.

En segundo lugar, observe que el primer parámetro del método `Label()` está precedido por la palabra clave `this`. El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.

**Lista 3: `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en IntelliSense de Visual Studio como todos los demás métodos de una clase (vea la figura 2). La única diferencia es que los métodos de extensión aparecen con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).

[![mediante el método de extensión html. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: uso del método de extensión html. Label () ([haga clic para ver la imagen de tamaño completo](creating-custom-html-helpers-cs/_static/image6.png))

La vista de índice modificada de la lista 4 usa el método de extensión html. Label () para representar todas sus etiquetas de `<label>`.

**Lista 4: `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido dos métodos de creación de aplicaciones auxiliares HTML personalizadas. En primer lugar, aprendió a crear una aplicación auxiliar HTML de `Label()` personalizada mediante la creación de un método estático que devuelve una cadena. A continuación, aprendió a crear un método auxiliar HTML personalizado `Label()` mediante la creación de un método de extensión en la clase `HtmlHelper`.

En este tutorial, me hemos centrado en la creación de un método auxiliar HTML muy sencillo. Observe que una aplicación auxiliar HTML puede ser tan complicada como desee. Puede compilar aplicaciones auxiliares HTML que representen contenido enriquecido, como vistas de árbol, menús o tablas de datos de base de datos.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-cs.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
