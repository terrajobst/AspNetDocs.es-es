---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Desencadenar una animación en otro control (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6a4af2324afab7519170c123b6ea7c57ab3e03fb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430789"
---
# <a name="triggering-an-animation-in-another-control-vb"></a>Desencadenar una animación en otro control (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y después animar otro control.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y después animar otro control.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

Para iniciar la animación del panel, se usa un botón HTML. Tenga en cuenta que `<input type="button" />` es favorable a `<asp:Button />` ya que no deseamos un postback cuando el usuario hace clic en ese botón.

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio. Es importante establecer `TargetControlID` en el identificador del botón (el elemento que desencadena la animación), no en el identificador del panel (el elemento que se anima)

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

Dentro del nodo `<Animations>`, coloque las animaciones como de costumbre. Para que cambien el panel, no el botón, establezca el `AnimationTarget` atributo para cada elemento de animación dentro de `AnimationExtender`. El valor de `AnimationTarget` es el identificador del panel, por supuesto. De este modo, las animaciones se producen con el panel, no con el botón desencadenador. Este es el marcado de `AnimationExtender` para este escenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

Observe el orden especial en el que aparecen las animaciones individuales. En primer lugar, el botón se desactiva una vez que se ejecuta la animación. Puesto que no hay ningún atributo `AnimationTarget` en el elemento `<EnableAction>`, esta animación se aplica al control de origen: el botón. Los dos pasos siguientes de animación deben realizarse en paralelo (elemento`<Parallel>`). Ambos tienen los atributos `AnimationTarget` establecidos en `"Panel1"`, lo que anima el panel, no el botón.

[![un clic del mouse en el botón inicia la animación del panel](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)

Al hacer clic con el mouse en el botón, se inicia la animación del panel ([haga clic para ver la imagen de tamaño completo](triggering-an-animation-in-another-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-vb.md)
> [Siguiente](modifying-animations-from-the-server-side-vb.md)
