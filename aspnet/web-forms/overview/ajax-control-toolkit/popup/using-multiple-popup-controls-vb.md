---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usar varios controles Popup (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. También es posible usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611591"
---
# <a name="using-multiple-popup-controls-vb"></a>Usar varios controles emergentes (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. También es posible usar más de un control popup en una página.

## <a name="overview"></a>Información general del

El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. También es posible usar más de un control popup en una página.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

A continuación, agregue un panel que actúe como el elemento emergente. En el escenario actual, el panel contiene un control de `Calendar`. Con el fin de evitar que se actualice la página causada por las devoluciones del calendario, el panel se coloca dentro de un control de `UpdatePanel`:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

La página también contiene dos cuadros de texto. Para cada cuadro de texto, el cuadro emergente del calendario aparecerá una vez que se active el cuadro de texto.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Ahora, extienda cada uno de los dos cuadros de texto con un `PopupControlExtender`. El atributo `TargetControlID` proporciona el identificador del control asociado al objeto extender. El atributo `PopupControlID` contiene el identificador del panel emergente. En este caso, ambos objetos extender muestran el mismo panel, pero también se pueden aplicar paneles diferentes.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Ahora, cada vez que haga clic en un campo de texto, aparecerá un calendario debajo del campo, lo que le permitirá seleccionar una fecha. (La obtención de la fecha seleccionada en los cuadros de texto se tratará en un tutorial diferente).

[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Siguiente](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
