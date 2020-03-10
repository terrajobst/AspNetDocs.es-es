---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Seleccionar una animación de una lista (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también per...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430825"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a>Seleccionar una animación de una lista (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El marco de trabajo también permite al programador seleccionar una animación de una lista de animaciones, en función de la evaluación de algún código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo. En lugar de una de las animaciones normales, el elemento `<Case>` entra en juego. Se evalúa el valor de su atributo SelectScript; el valor devuelto debe ser numérico. Dependiendo de este número, se ejecuta una de las subanimaciones en &lt;caso&gt;. Por ejemplo, si SelectScript se evalúa como 2, el kit de herramientas de control ejecuta la tercera animación en &lt;caso&gt; (el recuento comienza en 0).

El marcado siguiente define tres Subanimaciones: cambiar el tamaño del ancho, cambiar el tamaño del alto y atenuarla. A continuación, el código JavaScript (`Math.floor(3 * Math.random())`) selecciona un número entre 0 y 2 para que se ejecute una de las tres animaciones:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

[![una de las tres animaciones posibles: el panel se amplía](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Una de las tres posibles animaciones: el panel es más amplio ([haga clic para ver la imagen de tamaño completo](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-vb.md)
> [Siguiente](animating-in-response-to-user-interaction-vb.md)
