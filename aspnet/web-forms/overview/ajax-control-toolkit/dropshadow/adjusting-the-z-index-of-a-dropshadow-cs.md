---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajustar el índice Z de una sombra (C#) | Microsoft Docs
author: wenz
description: El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Sin embargo, a veces esta sombra entra en conflicto con otros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497485"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Ajustar el índice Z de un control DropShadow (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Sin embargo, a veces esta sombra entra en conflicto con otros controles, por ejemplo, el control de menú ASP.NET. Cuando se muestra una entrada de menú, aparece detrás de la sombra paralela.

## <a name="overview"></a>Información general

El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela. Sin embargo, a veces esta sombra entra en conflicto con otros controles, por ejemplo, el control de menú ASP.NET. Cuando se muestra una entrada de menú, aparece detrás de la sombra paralela.

## <a name="steps"></a>Pasos

El código comienza con el panel en sí, con un texto suficiente para que el panel contenga texto suficiente para que el efecto sea visible:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Otro panel se coloca directamente antes del panel de `panelShadow`. Contiene un menú con orientación horizontal para que las entradas de menú aparezcan encima (o en lugar de hacerlo en) en el panel de `dropShadow`):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

A continuación, se agrega el `DropShadowExtender` para ampliar el panel de `panelShadow` con un efecto de sombra paralela:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Por último, el control ASP.NET AJAX `ScriptManager` permite que el kit de herramientas de control funcione:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Al ejecutar este script, las entradas de menú aparecen debajo del panel. Sin embargo, el menú usa la clase CSS `panel` donde solo tiene que definir dos cosas para hacer que los elementos aparezcan delante del otro panel:

- Posición relativa
- Un índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Después, el control de `DropShadowExtender` no entra en conflicto con el control de menú.

[![antes de: la entrada de menú no está visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Antes: la entrada de menú no está visible ([haga clic para ver la imagen de tamaño completo](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))

[![después: aparece la entrada de menú](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Después: aparece la entrada de menú ([haga clic para ver la imagen de tamaño completo](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Siguiente](manipulating-dropshadow-properties-from-client-code-cs.md)
