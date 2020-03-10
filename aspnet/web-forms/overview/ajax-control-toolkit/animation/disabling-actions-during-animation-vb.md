---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Deshabilitar acciones durante la animación (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También admite la acción...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430933"
---
# <a name="disabling-actions-during-animation-vb"></a>Deshabilitar las acciones durante la animación (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También admite acciones, como clics del mouse. Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También admite acciones, como clics del mouse. Sin embargo, cuando un clic del mouse inicia una animación, es conveniente deshabilitar los clics del mouse durante la animación.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

La animación se aplicará a un botón HTML similar al siguiente:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Tenga en cuenta que se usa un control HTML en lugar de un control Web, ya que no se desea que el botón cree un postback; solo se iniciará la animación del lado cliente.

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Dentro del nodo `<Animations>`, `<OnClick>` es el elemento adecuado para controlar el clic del mouse. Sin embargo, también se puede hacer clic en el botón durante la animación. El elemento `<EnableAction>` puede encargarse de ello. Al establecer `Enabled="false"`, se deshabilita el botón como parte de la animación. Dado que usamos varias animaciones individuales (deshabilitando el botón y las animaciones reales), el elemento `<Parallel>` es necesario para pegar las animaciones individuales en una sola. Este es el marcado completo de `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

También sería posible volver a habilitar para el botón después de la animación, utilizando el siguiente elemento XML al final de la lista:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Sin embargo, en el escenario dado esto sería inútil, ya que el botón se atenúa y no está visible al final de la animación.

[![el botón está deshabilitado en cuanto se ejecuta la animación](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

El botón está deshabilitado en cuanto se ejecuta la animación ([haga clic para ver la imagen de tamaño completo](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-vb.md)
> [Siguiente](triggering-an-animation-in-another-control-vb.md)
