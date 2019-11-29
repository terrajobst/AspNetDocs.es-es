---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Controlar los postbacks desde un control emergente sin un UpdatePanelC#() | Microsoft Docs
author: wenz
description: El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598712"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Controlar los postbacks desde un control emergente sin un UpdatePanel (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.

## <a name="overview"></a>Información general del

El extensor PopupControl en el kit de herramientas de control de AJAX ofrece una manera sencilla de desencadenar un elemento emergente cuando se activa cualquier otro control. Cuando se produce un postback en este tipo de panel y hay varios paneles en la página, es difícil determinar en qué panel se hizo clic.

## <a name="steps"></a>Pasos

Cuando se usa un `PopupControl` con un postback, pero sin tener un `UpdatePanel` en la página, el kit de herramientas de control no ofrece una manera de determinar qué elemento de cliente ha desencadenado el elemento emergente que, a su vez, causó el PostBack. Sin embargo, un truco pequeño proporciona una solución alternativa para este escenario.

En primer lugar, esta es la configuración básica: dos cuadros de texto que desencadenan el mismo elemento emergente, un calendario. Dos `PopupControlExtenders` incorporar cuadros de texto y ventanas emergentes.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

La idea básica es agregar un campo de formulario oculto en el &lt;`form`elemento de &gt; que contiene el cuadro de texto que inició la ventana emergente:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Cuando se carga la página, el código JavaScript agrega un controlador de eventos a ambos cuadros de texto: cada vez que el usuario hace clic en un cuadro de texto, su nombre se escribe en el campo de formulario oculto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

En el código del lado servidor, se debe leer el valor del campo oculto. Dado que los campos de formulario ocultos son triviales de manipular, es necesario un enfoque de lista blanca para validar el valor oculto. Una vez identificado el cuadro de texto correcto, se escribe en él la fecha del calendario.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

[![el calendario aparece cuando el usuario hace clic en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

El calendario aparece cuando el usuario hace clic en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))

[![al hacer clic en una fecha, la coloca en el cuadro de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Al hacer clic en una fecha, se coloca en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Siguiente](using-multiple-popup-controls-vb.md)
