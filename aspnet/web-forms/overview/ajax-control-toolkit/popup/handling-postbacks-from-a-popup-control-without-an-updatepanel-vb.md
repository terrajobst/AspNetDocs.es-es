---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Controlar los postbacks desde un control emergente sin un UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: aaecf77c1d25f2c99ef4e9948d79fc01b837169b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446143"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Controlar los postbacks desde un control emergente sin un UpdatePanel (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.

## <a name="overview"></a>Información general

El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con un postback, pero sin tener un `UpdatePanel` en la página, el kit de herramientas de control no ofrece una manera de determinar qué elemento de cliente ha desencadenado el elemento emergente que, a su vez, causó el PostBack. Sin embargo, un truco pequeño proporciona una solución alternativa para este escenario.

En primer lugar, esta es la configuración básica: dos cuadros de texto que desencadenan el mismo elemento emergente, un calendario. Dos `PopupControlExtenders` incorporar cuadros de texto y ventanas emergentes.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

La idea básica es agregar un campo de formulario oculto en el &lt;`form`elemento de &gt; que contiene el cuadro de texto que inició la ventana emergente:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Cuando se carga la página, el código JavaScript agrega un controlador de eventos a ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, su nombre se escribe en el campo de formulario oculto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

En el código del lado servidor, se debe leer el valor del campo oculto. Puesto que son triviales para manipular campos ocultos de formulario, se requiere un enfoque de lista de permitidos para validar el valor hidden. Una vez identificado el cuadro de texto correcto, se escribe en él la fecha del calendario.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]

[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))

[![al hacer clic en una fecha, la coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Al hacer clic en una fecha, se coloca en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
