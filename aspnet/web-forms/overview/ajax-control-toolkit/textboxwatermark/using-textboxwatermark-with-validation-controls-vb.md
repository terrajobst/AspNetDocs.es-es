---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usar TextBoxWatermark con controles de validación (VB) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro,...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 141cae26c9e52be510e2a5a8f816cbac977dcf3d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508753"
---
# <a name="using-textboxwatermark-with-validation-controls-vb"></a>Usar TextBoxWatermark con los controles de validación (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.

## <a name="overview"></a>Información general

El control `TextBoxWatermark` en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado. Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.

## <a name="steps"></a>Pasos

La configuración básica del ejemplo es la siguiente: un control de `TextBox` se marca mediante un control `TextBoxWatermarkExtender`. Un botón desencadena un postback y se usará más adelante para desencadenar los controles de validación en la página. Además, se requiere un control `ScriptManager` para inicializar ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Ahora, agregue un control `RequiredFieldValidator` que compruebe si hay texto en el campo cuando se envía el formulario. La propiedad `InitialValue` del validador debe establecerse en el mismo valor que se utiliza en el control de `TextBoxWatermarkExtender`: cuando se envía el formulario, el valor de un cuadro de texto sin modificar es el valor de marca de agua que contiene:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Sin embargo, hay un problema con este enfoque: Si el cliente deshabilita JavaScript, el campo de texto no se rellena previamente con el texto de marca de agua, por lo que el `RequiredFieldValidator` no desencadena un mensaje de error. Por lo tanto, se requiere un segundo control `RequiredFieldValidator` que compruebe si hay un cuadro de texto vacío (omitiendo el atributo `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Dado que ambos validadores usan `Display`=`"Dynamic"`, el usuario final no puede distinguir de la apariencia visual de los dos validadores que se ha desencadenado; en su lugar, parece que solo uno de ellos.

Por último, agregue algún código del lado servidor para generar el texto en el campo si ningún validador emitió un mensaje de error:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]

[![el validador se queja de que no hay texto en el campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

El validador se queja de que no hay texto en el campo ([haga clic para ver la imagen de tamaño completo](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-vb.md)
