---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Colocar un ModalPopup (VB) | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, el control no ofrece...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: fb79a08a339588ed8adc4b4236911819ea9286b4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496897"
---
# <a name="positioning-a-modalpopup-vb"></a>Colocar un ModalPopup (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, el control no ofrece una funcionalidad integrada para colocar el elemento emergente.

## <a name="overview"></a>Información general

El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, el control no ofrece una funcionalidad integrada para colocar el elemento emergente.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, el `ScriptManager`. el control debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

A continuación, agregue un panel que actúe como elemento emergente modal. Se usa un botón para cerrar el elemento emergente:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Siempre que se muestre el elemento emergente, se colocará en un lugar determinado de la página. Para esta tarea, se crea una función de JavaScript del lado cliente. Primero intenta acceder al panel. Si se realiza correctamente, la posición del panel se establece mediante CSS y JavaScript (cambie la posición del menú emergente en). Sin embargo, el control `ModalPopupExtender` también intenta colocar el elemento emergente. Por lo tanto, el código JavaScript coloca la ventana emergente repetidamente, cada décima de segundo.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Como puede ver, el valor devuelto del método `setTimeout()` JavaScript se guarda en una variable global. Esto permite detener el posicionamiento repetido de la ventana emergente a petición, mediante el método `clearTimeout()`:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Ahora todo lo que queda por hacer es hacer que el explorador llame a estas funciones siempre que sea necesario. Se debe llamar a la función de JavaScript `movePanel()` cuando se hace clic en el botón que desencadena el panel:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

Y la función `stopMoving()` entra en juego cuando se cierra el elemento emergente que se puede desencadenar en el control `ModalPopupExtender`:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]

[![aparece la ventana emergente modal en la posición designada](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

La ventana emergente modal aparece en la posición designada ([haga clic para ver la imagen de tamaño completo](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-modalpopup-vb.md)
