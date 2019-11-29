---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Seleccionar una animación de una lista (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también per...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575114"
---
# <a name="picking-one-animation-out-of-a-list-c"></a>Seleccionar una animación de una lista (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.

## <a name="overview"></a>Información general del

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo. En lugar de una de las animaciones normales, el elemento `<Case>` entra en juego. Se evalúa el valor de su atributo SelectScript; el valor devuelto debe ser numérico. Dependiendo de este número, se ejecuta una de las subanimaciones en &lt;caso&gt;. Por ejemplo, si SelectScript se evalúa como 2, el kit de herramientas de control ejecuta la tercera animación en &lt;caso&gt; (el recuento comienza en 0).

El marcado siguiente define tres Subanimaciones: cambiar el tamaño del ancho, cambiar el tamaño del alto y atenuarla. A continuación, el código JavaScript (`Math.floor(3 * Math.random())`) selecciona un número entre 0 y 2 para que se ejecute una de las tres animaciones:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

[![una de las tres animaciones posibles: el panel se amplía](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Una de las tres posibles animaciones: el panel es más amplio ([haga clic para ver la imagen de tamaño completo](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-cs.md)
> [Siguiente](animating-in-response-to-user-interaction-cs.md)
