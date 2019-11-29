---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Iniciar una ventana emergente modal desde el código delC#servidor () | Microsoft Docs
author: wenz
description: El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, algunos escenarios requieren que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fec0ce2cdd24333f65201301718440e1a09d930e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599040"
---
# <a name="launching-a-modal-popup-window-from-server-code-c"></a>Iniciar una ventana emergente modal desde el código del servidor (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, algunos escenarios requieren que la apertura del elemento emergente modal se desencadene en el lado servidor.

## <a name="overview"></a>Información general del

El control ModalPopup en el kit de herramientas de control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios del lado cliente. Sin embargo, algunos escenarios requieren que la apertura del elemento emergente modal se desencadene en el lado servidor.

## <a name="steps"></a>Pasos

En primer lugar, se requiere un control Web del botón ASP.NET para demostrar cómo funciona el control ModalPopup. Agregue este tipo de botón en el &lt;formulario&gt; elemento en una nueva página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

A continuación, necesitará el marcado para el elemento emergente que desea crear. Defina como un control de `<asp:Panel>` y asegúrese de que incluye un control de botón. El control ModalPopup ofrece la funcionalidad de hacer que un botón de este tipo cierre el elemento emergente. de lo contrario, no hay ninguna manera fácil de dejar de desaparecer.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

A continuación, agregue el control ModalPopup desde el kit de herramientas de ASP.NET AJAX a la página. Establezca las propiedades del botón que carga el control, el botón que hace desaparecer y el identificador del elemento emergente real.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Como con todas las páginas web basadas en ASP.NET AJAX; el administrador de scripts es necesario para cargar las bibliotecas de JavaScript necesarias para los diferentes exploradores de destino:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Ejecute el ejemplo en el explorador. Al hacer clic en el botón, aparece la ventana emergente modal. Para lograr el mismo efecto con el código del lado servidor, se requiere un botón nuevo:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Como puede ver, un clic en el botón genera un postback y ejecuta el método `ServerButton_Click()` en el servidor. En este método, se ejecuta una función de JavaScript llamada `launchModal()` para ser exacta; la función de JavaScript se ejecutará una vez que se haya cargado la página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

El trabajo de `launchModal()` es mostrar el ModalPopup. La función `launchModal()` se ejecuta una vez cargada la página HTML completa. En ese momento, sin embargo, el marco de AJAX de ASP.NET aún no se ha cargado completamente. Por lo tanto, la función `launchModal()` simplemente establece una variable que el control ModalPopup se debe mostrar más adelante:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

La función `pageLoad()` JavaScript es una función especial que se ejecuta una vez que ASP.NET AJAX se ha cargado por completo. Por lo tanto, agregamos código a esta función para mostrar el control ModalPopup, pero solo si se ha llamado a `launchModal()` antes de:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

La función `$find()` busca un elemento con nombre en la página y espera el identificador del lado servidor como parámetro. Por consiguiente, `$find("mpe")` devuelve la representación del cliente del control ModalPopup; su método `show()` permite que aparezca la ventana emergente.

[![aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

La ventana emergente modal aparece cuando se hace clic en cualquiera de los botones ([haga clic para ver la imagen de tamaño completo](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-modalpopup-with-a-repeater-control-cs.md)
