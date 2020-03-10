---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Controlar los postbacks desde un ModalPopup (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Se debe tener especial cuidado cuando un PDV...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504025"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a>Controlar postbacks desde un ModalPopup (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.

## <a name="overview"></a>Información general

El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Se debe tener especial cuidado cuando se crea un postback desde el elemento emergente.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

A continuación, agregue un panel que actúe como elemento emergente modal. Allí, el usuario puede escribir un nombre y una dirección de correo electrónico. Se usa un botón para cerrar el elemento emergente y guardar la información. Tenga en cuenta que el atributo `OnClick` está establecido para que se produzca un postback al hacer clic en este botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

La propia página consta de dos etiquetas para exactamente la misma información: nombre y dirección de correo electrónico. Se usa un botón para desencadenar el elemento emergente modal:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Para que aparezca el elemento emergente, agregue el control `ModalPopupExtender`. Establezca el atributo `PopupControlID` en el identificador del panel y `TargetControlID` en el identificador del botón:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Ahora, siempre que se haga clic en el botón `Save` en el menú emergente modal, se ejecutará el método de `SaveData()` del servidor. Allí, puede guardar los datos especificados en un almacén de datos. Por simplicidad, los nuevos datos se generan en la etiqueta:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Además, los controles de cuadro de texto dentro del elemento emergente modal deben rellenarse con el nombre y el correo electrónico actuales. Sin embargo, esto solo es necesario cuando no se produce un postback. Si hay un postback, la característica ASP.NET ViewState rellenará automáticamente los cuadros de texto con los valores adecuados.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

[![el elemento emergente modal produce un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

El elemento emergente modal produce un postback ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-modalpopup-with-a-repeater-control-vb.md)
> [Siguiente](positioning-a-modalpopup-vb.md)
