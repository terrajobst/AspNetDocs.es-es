---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipular las propiedades de sombra de un código de cliente (VB) | Microsoft Docs
author: wenz
description: El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Las propiedades de este extensor también se pueden cambiar mediante JavaScript de cliente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497413"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipular propiedades DropShadow desde el código de cliente (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Las propiedades de este extensor también se pueden cambiar mediante el código JavaScript del cliente.

## <a name="overview"></a>Información general

El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Las propiedades de este extensor también se pueden cambiar mediante el código JavaScript del cliente.

## <a name="steps"></a>Pasos

El código comienza con un panel que contiene algunas líneas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

La clase CSS asociada proporciona al panel un color de fondo agradable:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

El `DropShadowExtender` se agrega para extender el panel con un efecto de sombra paralela, que se establece en 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Después, el control ASP.NET AJAX `ScriptManager` permite que el kit de herramientas de control funcione:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Otro panel contiene dos vínculos de JavaScript para establecer la opacidad de la sombra paralela: el vínculo menos reduce la opacidad de la sombra, el vínculo más lo incrementa.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

La función de JavaScript `changeOpacity()` debe buscar primero el control `DropShadowExtender` en la página. ASP.NET AJAX define el método de `$find()` para esa tarea exactamente. A continuación, el método `get_Opacity()` recupera la opacidad actual, el método `set_Opacity()` lo establece. Después, el código de JavaScript coloca el valor de la opacidad actual en el elemento `<label>`:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

[![se cambia la opacidad en el lado del cliente](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

La opacidad se cambia en el lado del cliente ([haga clic para ver la imagen de tamaño completo](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adjusting-the-z-index-of-a-dropshadow-vb.md)
