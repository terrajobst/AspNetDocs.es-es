---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Agregar animación a un control (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. En este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607177"
---
# <a name="adding-animation-to-a-control-vb"></a>Agregar animación a un control (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. En este tutorial se muestra cómo configurar este tipo de animación.

## <a name="overview"></a>Información general del

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. En este tutorial se muestra cómo configurar este tipo de animación.

## <a name="steps"></a>Pasos

El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

La animación de este escenario se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

La clase CSS asociada para el panel define un color de fondo y un ancho:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

A continuación, necesitamos el `AnimationExtender`. Después de proporcionar un `ID` y el `runat="server"`habitual, el atributo `TargetControlID` debe establecerse en el control para animar en nuestro caso, el panel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

La animación completa se aplica mediante declaración, mediante una sintaxis XML, lamentablemente no es totalmente compatible con IntelliSense de Visual Studio. El nodo raíz se `<Animations>;` dentro de este nodo; se permiten varios eventos que determinan cuándo se realizan las animaciones:

- `OnClick` (clic del mouse)
- `OnHoverOut` (cuando el mouse sale de un control)
- `OnHoverOver` (cuando se mantiene el mouse sobre un control y se detiene la animación `OnHoverOut`)
- `OnLoad` (cuando se carga la página)
- `OnMouseOut` (cuando el mouse sale de un control)
- `OnMouseOver` (cuando se mantiene el mouse sobre un control, sin detener la animación de `OnMouseOut`)

El marco de trabajo incluye un conjunto de animaciones, cada una representada por su propio elemento XML. Esta es una selección:

- `<Color>` (cambio de un color)
- `<FadeIn>` (fundido en)
- `<FadeOut>` (fundido)
- `<Property>` (cambiar la propiedad de un control)
- `<Pulse>` (pulsating)
- `<Resize>` (cambiar el tamaño)
- `<Scale>` (cambiar proporcionalmente el tamaño)

En este ejemplo, el panel se atenúa. La animación debe tomar 1,5 segundos (`Duration` atributo), mostrando 24 fotogramas (pasos de animación) por segundo (`Fps` atributo). Este es el marcado completo del control `AnimationExtender`:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Al ejecutar este script, el panel se muestra y se atenúa en unos segundos.

[![se atenúa el panel](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

El panel se atenúa ([haga clic para ver la imagen de tamaño completo](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Siguiente](executing-several-animations-at-the-same-time-vb.md)
