---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Usar el extensor del control ColorPicker (VB) | Microsoft Docs
author: microsoft
description: ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control Popup. Se puede adjuntar a cualquier ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 77e2e3bc61a5e1498570959ca40acff83dc3fc82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446587"
---
# <a name="using-the-colorpicker-control-extender-vb"></a>Usar el extensor del control ColorPicker (VB)

por [Microsoft](https://github.com/microsoft)

> ColorPicker es un extensor de AJAX de ASP.NET que proporciona la funcionalidad de selección de color del lado cliente con la interfaz de usuario en un control Popup. Se puede adjuntar a cualquier control TextBox de ASP.NET. Instalar.

El objetivo de este tutorial es explicar cómo se puede usar el extensor de control de los controles ColorPicker del kit de herramientas de AJAX. El extensor del control ColorPicker muestra un cuadro de diálogo emergente que le permite seleccionar un color. El componente ColorPicker es útil siempre que desee proporcionar una interfaz de usuario intuitiva para que un usuario elija un color.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Extender un control TextBox con el extensor del control ColorPicker

Imagine, por ejemplo, que desea crear un sitio web que permita a los visitantes crear tarjetas de presentación personalizadas. Los visitantes pueden escribir el texto de una tarjeta de presentación y seleccionar el color. La página ASP.NET de la lista 1 contiene dos controles TextBox denominados txtCardText y txtCardColor. Cuando envíe el formulario, se mostrarán los valores seleccionados (vea la figura 1).

[![forma sencilla para crear una tarjeta de presentación](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Figura 01**: forma sencilla de crear una tarjeta de presentación ([haga clic para ver la imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image2.png))

**Lista 1-CreateCard. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

El formulario de la lista 1 funciona, pero no proporciona una experiencia de usuario excelente. El usuario tiene que escribir un color en el cuadro de texto. Si el usuario desea un color especializado, por ejemplo, solo el tono derecho de PEA verde, el usuario debe averiguar el código de color HTML sin ayuda.

Puede usar el extensor del control ColorPicker para crear una mejor experiencia del usuario. El componente ColorPicker muestra un cuadro de diálogo de color cuando se mueve el foco a un control TextBox (vea la figura 2).

[![el extensor del control ColorPicker](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Figura 02**: extensor del control ColorPicker ([haga clic para ver la imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image4.png))

Debe completar dos pasos para usar el extensor del control ColorPicker con el formato en la lista 1:

1. Agregar un control ScriptManager a la página
2. Agregar el extensor del control ColorPicker a la página

Antes de poder usar ColorPicker, debe agregar un ScriptManager a la página. Un buen lugar para agregar el ScriptManager es justo debajo del formulario de &lt;de apertura del lado servidor&gt; etiqueta. Puede arrastrar el ScriptManager a la página desde el cuadro de herramientas (el ScriptManager se encuentra en la pestaña extensiones AJAX). Como alternativa, puede escribir la siguiente etiqueta en la vista Código fuente debajo de la etiqueta del formulario de apertura del lado servidor:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

La forma más fácil de agregar el extensor del control ColorPicker a la página está en la vista Diseño. Si mantiene el mouse sobre el cuadro de texto txtCardColor, aparece una opción tarea inteligente que le permite agregar un extensor (vea la figura 3). Si elige esta opción, aparecerá el Asistente para extender (consulte la figura 4).

[![agregar un objeto extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Figura 03**: agregar un objeto extender ([haga clic para ver la imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image6.png))

[![seleccionar un extensor de control con el Asistente para extender](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Figura 04**: seleccionar un extensor de control con el Asistente para extender ([haga clic para ver la imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image8.png))

Puede elegir el extensor ColorPicker para extender el cuadro de texto txtCardColor con el extensor ColorPicker. Haga clic en Aceptar para cerrar el cuadro de diálogo.

Después de realizar estos cambios, el origen de la página es como el de la lista 2.

**Lista 2-CreateCard. aspx (con ColorPicker)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Observe que la página contiene ahora un control ColorPickerExtender que aparece directamente debajo del control de cuadro de texto txtCardColor. El control ColorPickerExtender extiende el control txtCardColor para que muestre un cuadro de diálogo Selector de colores.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usar un botón para iniciar el cuadro de diálogo Selector de colores

El extensor ColorPicker admite las siguientes propiedades:

- PopupButtonId: el identificador de un botón de la página que hace que aparezca el cuadro de diálogo Selector de colores.
- PopupPosition: la posición, relativa al control de destino, del cuadro de diálogo del selector de colores. Los valores posibles son Absolute, Center, BottomLeft, BottomRight, Left, Right, Right y Left (el valor predeterminado es BottomLeft).
- SampleControlId: el identificador de un control que muestra el color seleccionado.
- SelectedColor: el color inicial seleccionado por ColorPicker.

Puede usar estas propiedades para personalizar cómo se muestra el cuadro de diálogo Selector de colores y cómo se muestra el color seleccionado. En la página de la lista 3 se muestra cómo puede usar varias de estas propiedades.

**Lista 3-CreateCardButton. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

La página de la lista 3 incluye un botón seleccionar color (vea la figura 5). Al hacer clic en este botón, el cuadro de diálogo Selector de colores aparece sobre el cuadro de texto. Si selecciona un color del cuadro de diálogo, el color seleccionado aparece como el color de fondo del control etiqueta lblSample.

La propiedad PopupButtonID de ColorPicker se usa para asociar el botón seleccionar color con el extensor ColorPicker. Cuando se proporciona un valor para la propiedad PopupButtonID, el cuadro de diálogo Selector de colores ya no aparece cuando el control de destino tiene el foco. Debe hacer clic en el botón para mostrar el cuadro de diálogo.

La propiedad SampleControlID se usa para asociar un control que muestra el color seleccionado con ColorPicker. ColorPicker cambia el color de fondo de este control por el color seleccionado actualmente.

[![mostrar el cuadro de diálogo Selector de colores con un botón](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Figura 05**: mostrar el cuadro de diálogo Selector de colores con un botón ([haga clic para ver la imagen de tamaño completo](using-the-colorpicker-control-extender-vb/_static/image10.png))

## <a name="summary"></a>Resumen

En este tutorial, aprendió a usar el extensor del control ColorPicker para mostrar un cuadro de diálogo de selector de colores emergente. En primer lugar, hemos examinado cómo puede mostrar el cuadro de diálogo cuando el foco se mueve a un control de cuadro de texto. A continuación, aprendió a crear un botón que muestra el cuadro de diálogo Selector de colores al hacer clic en el botón.

> [!div class="step-by-step"]
> [Anterior](using-the-colorpicker-control-extender-cs.md)
