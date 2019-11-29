---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Usar postbacks con ReorderList (C#) | Microsoft Docs
author: wenz
description: El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordena la lista, se muestra un pedido...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611394"
---
# <a name="using-postbacks-with-reorderlist-c"></a>Usar postbacks con ReorderList (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordene la lista, un postback informará al servidor del cambio.

## <a name="overview"></a>Información general del

El control `ReorderList` en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordene la lista, un postback informará al servidor del cambio.

## <a name="steps"></a>Pasos

Hay varios orígenes de datos posibles para el control `ReorderList`. Uno es usar un control `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Para enlazar este XML a un `ReorderList` control y habilitar los postbacks, se deben establecer los siguientes atributos:

- `DataSourceID`: el identificador del origen de datos
- `SortOrderField`: propiedad por la que se va a ordenar
- `AllowReorder`: si se permite al usuario cambiar el orden de los elementos de la lista
- `PostBackOnReorder`: si se va a crear un postback cada vez que se reorganice la lista

Este es el marcado adecuado para el control:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

En el control `ReorderList`, los datos específicos del origen de datos se pueden enlazar mediante el método `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

En una posición arbitraria de la página, una etiqueta contendrá la información cuando se haya producido la última reordenación:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Esta etiqueta se rellena con texto en el código del lado servidor, controlando el PostBack:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Por último, para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, se debe colocar el control `ScriptManager` en la página:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

[![cada reordenación desencadena un postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Cada reordenación desencadena un postback ([haga clic para ver la imagen de tamaño completo](using-postbacks-with-reorderlist-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](drag-and-drop-via-reorderlist-cs.md)
