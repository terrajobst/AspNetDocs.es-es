---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Usar TextBoxWatermark con controles de validaciónC#() | Microsoft Docs
author: wenz
description: El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro,...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610894"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a>Usar TextBoxWatermark con los controles de validación (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.

## <a name="overview"></a>Información general del

El control `TextBoxWatermark` en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.

## <a name="steps"></a>Pasos

La configuración básica del ejemplo es la siguiente: un control de `TextBox` se marca mediante un control `TextBoxWatermarkExtender`. Un botón desencadena un postback y se usará más adelante para desencadenar los controles de validación en la página. Además, se requiere un control `ScriptManager` para inicializar ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Ahora, agregue un control `RequiredFieldValidator` que compruebe si hay texto en el campo cuando se envía el formulario. La propiedad `InitialValue` del validador debe establecerse en el mismo valor que se utiliza en el control de `TextBoxWatermarkExtender`: cuando se envía el formulario, el valor de un cuadro de texto sin modificar es el valor de marca de agua que contiene:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Sin embargo, hay un problema con este enfoque: Si el cliente deshabilita JavaScript, el campo de texto no se rellena previamente con el texto de marca de agua, por lo que el `RequiredFieldValidator` no desencadena un mensaje de error. Por lo tanto, se requiere un segundo control `RequiredFieldValidator` que compruebe si hay un cuadro de texto vacío (omitiendo el atributo `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Dado que ambos validadores usan `Display`=`"Dynamic"`, el usuario final no puede distinguir de la apariencia visual de los dos validadores que se ha desencadenado; en su lugar, parece que solo uno de ellos.

Por último, agregue algún código del lado servidor para generar el texto en el campo si ningún validador emitió un mensaje de error:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

[![el validador se queja de que no hay texto en el campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

El validador se queja de que no hay texto en el campo ([haga clic para ver la imagen de tamaño completo](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Siguiente](using-textboxwatermark-in-a-formview-vb.md)
