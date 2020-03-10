---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animar en respuesta a la interacción del usuario (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Las animaciones pueden estrella...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497929"
---
# <a name="animating-in-response-to-user-interaction-vb"></a>Animaciones en respuesta a la interacción del usuario (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Las animaciones pueden iniciarse automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Las animaciones pueden iniciarse automáticamente o pueden activarse mediante la interacción del usuario, por ejemplo, haciendo clic con el mouse.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

Dentro del nodo `<Animations>`, hay cinco maneras de iniciar la animación a través de la interacción del usuario (el elemento que falta se `<OnLoad>` que se ejecuta una vez que la página completa se ha cargado completamente):

- `<OnClick>` (hacer clic con el mouse en el control)
- `<OnHoverOut>` (el mouse sale del control)
- `<OnHoverOver>` (se mantiene el mouse sobre un control y se detiene la animación `<OnHoverOut>`)
- `<OnMouseOut>` (el mouse sale de un control)
- `<OnMouseOver>` (el mouse se mantiene sobre un control, sin detener la animación `<OnMouseOut>`)

En este escenario, se utiliza `<OnClick>`. Cuando el usuario hace clic en el panel, se cambia de tamaño y se atenúa al mismo tiempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

[![un clic del mouse inicia la animación](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Un clic del mouse inicia la animación ([haga clic para ver la imagen de tamaño completo](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-vb.md)
> [Siguiente](disabling-actions-during-animation-vb.md)
