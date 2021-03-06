---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Controlar los postbacks desde un control popup con un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Se debe tener especial cuidado...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: dd045ae56696c7944df98cf805ba812fde1bb4ff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496639"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a>Controlar los postbacks desde un control emergente con un UpdatePanel (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)

> El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Se debe tener especial cuidado cuando se produce un postback dentro de este tipo de elemento emergente.

## <a name="overview"></a>Información general

El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Se debe tener especial cuidado cuando se produce un postback dentro de este tipo de elemento emergente.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con un postback, una `UpdatePanel` puede evitar la actualización de la página causada por el PostBack. El marcado siguiente define un par de elementos importantes:

- Un control `ScriptManager` para que el kit de herramientas de control de AJAX de ASP.NET funcione
- Dos controles `TextBox` que desencadenarán un elemento popup
- Control `Panel` que servirá como elemento emergente.
- Dentro del panel, se inserta un control `Calendar` dentro de un control `UpdatePanel`
- Dos `PopupControlExtender` controles que asignan el panel a los cuadros de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

Tenga en cuenta que el atributo `OnSelectionChanged` del control `Calendar` está establecido. Por tanto, cuando el usuario selecciona una fecha en el calendario, se produce un postback y se ejecuta el método del servidor `c1_SelectionChanged()`. Dentro de ese método, la fecha actual se debe recuperar y volver a escribir en el cuadro de texto.

La sintaxis de es la siguiente: en primer lugar, se debe generar un objeto de proxy para el `PopupControlExtender` en la página. ASP.NET AJAX control Toolkit ofrece el método `GetProxyForCurrentPopup()`. El objeto que devuelve este método admite el método `Commit()` que envía un valor al control que desencadenó el elemento emergente (no el control que desencadenó la llamada al método). El código siguiente proporciona la fecha seleccionada como argumento para el método `Commit()`, lo que hace que el código vuelva a escribir la fecha seleccionada en el cuadro de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

Ahora, cada vez que haga clic en una fecha de calendario, la fecha seleccionada aparecerá en el cuadro de texto asociado y creará un control selector de fecha que se puede encontrar actualmente en muchos sitios Web.

[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))

[![al hacer clic en una fecha, la coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)

Al hacer clic en una fecha, se coloca en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-vb.md)
> [Siguiente](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)
