---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlar dinámicamente las animaciones UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430861"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a>Controlar dinámicamente las animaciones UpdatePanel (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de un UpdatePanel, existe un extensor especial que se basa en gran medida en el marco de animación: UpdatePanelAnimation. También puede funcionar junto con los desencadenadores UpdatePanel.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa en gran medida en el marco de animación: `UpdatePanelAnimation`. También puede trabajar junto con desencadenadores `UpdatePanel`.

## <a name="steps"></a>Pasos

El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

La animación de este escenario se aplicará a una presentación de la hora actual. Esta información se puede escribir en una etiqueta mediante el método `Page_Load()`, o (por simplicidad) se usa el siguiente código en línea:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Además, se crea un botón para desencadenar la actualización del tiempo:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

A continuación, este código se coloca en la sección `<ContentTemplate>` de un elemento `UpdatePanel`. El atributo `UpdateMode` del panel debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel. En la sección `<Triggers>` del `UpdatePanel`, se crea un desencadenador de postback asincrónico y se vincula al evento `Click` del botón. Por lo tanto, si el usuario hace clic en el botón, se actualiza el `UpdatePanel`. Este es el marcado del control `UpdatePanel`:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Por último, se debe configurar la `UpdatePanelAnimationExtender`: establezca el atributo `TargetControlID` en el identificador del panel y defina una animación dentro del objeto extender. La difuminación en tiene sentido, lo que crea un atractivo visual interesante en la hora actualizada. El marcado del extensor puede tener el siguiente aspecto:

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Ejecute el archivo en el explorador. Siempre que se hace clic en el botón, la hora actual se muestra en el panel, siempre atenuada durante un segundo.

[![se difumina la hora actual](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

La hora actual se difumina en ([haga clic para ver la imagen de tamaño completo](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-an-updatepanel-control-vb.md)
