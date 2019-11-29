---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animar un control UpdatePanelC#() | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599940"
---
# <a name="animating-an-updatepanel-control-c"></a>Animar un control UpdatePanel (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un extensor especial que se basa en gran medida en el marco de animación: UpdatePanelAnimation. En este tutorial se muestra cómo configurar este tipo de animación para un UpdatePanel.

## <a name="overview"></a>Información general del

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa en gran medida en el marco de animación: `UpdatePanelAnimation`. En este tutorial se muestra cómo configurar este tipo de animación para un `UpdatePanel`.

## <a name="steps"></a>Pasos

El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

La animación de este escenario se aplicará a un control Web de ASP.NET `Wizard` que resida en un `UpdatePanel`. Tres pasos (arbitrarios) proporcionan suficientes opciones para desencadenar postbacks:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

El marcado necesario para el control `UpdatePanelAnimationExtender` es bastante similar al marcado que se usa para la `AnimationExtender`. En el atributo `TargetControlID` se proporciona el `ID` de la `UpdatePanel` que se va a animar; en el control `UpdatePanelAnimationExtender`, el elemento `<Animations>` contiene el marcado XML para las animaciones. Sin embargo, hay una diferencia: la cantidad de eventos y controladores de eventos se limita en comparación con `AnimationExtender`. Por `UpdatePanels`, solo existen dos de ellos:

- `<OnUpdated>` cuando se ha actualizado el UpdatePanel
- `<OnUpdating>` cuando se inicia la actualización de UpdatePanel

En este escenario, el nuevo contenido del `UpdatePanel` (después del postback) debe atenuarse. Este es el marcado necesario para ello:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Ahora, cada vez que se produce un postback en UpdatePanel, el nuevo contenido del panel se desvanece sin problemas.

[![se atenúa el siguiente paso del asistente](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

Se difumina el siguiente paso del asistente ([haga clic para ver la imagen de tamaño completo](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](changing-an-animation-using-client-side-code-cs.md)
> [Siguiente](dynamically-controlling-updatepanel-animations-cs.md)
