---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Desencadenar una animación en otro control (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599641"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Desencadenar una animación en otro control (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y después animar otro control.

## <a name="overview"></a>Información general del

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Por lo general, el inicio de una animación lo desencadena la interacción del usuario con el mismo control. Sin embargo, también es posible interactuar con un control y después animar otro control.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Para iniciar la animación del panel, se usa un botón HTML. Tenga en cuenta que `<input type="button" />` es favorable a `<asp:Button />` ya que no deseamos un postback cuando el usuario hace clic en ese botón.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio. Es importante establecer `TargetControlID` en el identificador del botón (el elemento que desencadena la animación), no en el identificador del panel (el elemento que se anima)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

Dentro del nodo `<Animations>`, coloque las animaciones como de costumbre. Para que cambien el panel, no el botón, establezca el `AnimationTarget` atributo para cada elemento de animación dentro de `AnimationExtender`. El valor de `AnimationTarget` es el identificador del panel, por supuesto. De este modo, las animaciones se producen con el panel, no con el botón desencadenador. Este es el marcado de `AnimationExtender` para este escenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Observe el orden especial en el que aparecen las animaciones individuales. En primer lugar, el botón se desactiva una vez que se ejecuta la animación. Puesto que no hay ningún atributo `AnimationTarget` en el elemento `<EnableAction>`, esta animación se aplica al control de origen: el botón. Los dos pasos siguientes de animación deben realizarse en paralelo (elemento`<Parallel>`). Ambos tienen los atributos `AnimationTarget` establecidos en `"Panel1"`, lo que anima el panel, no el botón.

[![un clic del mouse en el botón inicia la animación del panel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Al hacer clic con el mouse en el botón, se inicia la animación del panel ([haga clic para ver la imagen de tamaño completo](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-cs.md)
> [Siguiente](modifying-animations-from-the-server-side-cs.md)
