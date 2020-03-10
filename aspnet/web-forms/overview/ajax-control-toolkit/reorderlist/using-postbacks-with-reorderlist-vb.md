---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usar postbacks con ReorderList (VB) | Microsoft Docs
author: wenz
description: El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordena la lista, se muestra un pedido...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78445933"
---
# <a name="using-postbacks-with-reorderlist-vb"></a>Usar postbacks con ReorderList (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> El control ReorderList en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordene la lista, un postback informará al servidor del cambio.

## <a name="overview"></a>Información general

El control `ReorderList` en el kit de herramientas de control de AJAX proporciona una lista que el usuario puede reordenar mediante arrastrar y colocar. Cada vez que se reordene la lista, un postback informará al servidor del cambio.

## <a name="steps"></a>Pasos

Hay varios orígenes de datos posibles para el control `ReorderList`. Uno es usar un control `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Para enlazar este XML a un `ReorderList` control y habilitar los postbacks, se deben establecer los siguientes atributos:

- `DataSourceID`: el identificador del origen de datos
- `SortOrderField`: propiedad por la que se va a ordenar
- `AllowReorder`: si se permite al usuario cambiar el orden de los elementos de la lista
- `PostBackOnReorder`: si se va a crear un postback cada vez que se reorganice la lista

Este es el marcado adecuado para el control:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

En el control `ReorderList`, los datos específicos del origen de datos se pueden enlazar mediante el método `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

En una posición arbitraria de la página, una etiqueta contendrá la información cuando se haya producido la última reordenación:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Esta etiqueta se rellena con texto en el código del lado servidor, controlando el PostBack:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Por último, para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, se debe colocar el control `ScriptManager` en la página:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

[![cada reordenación desencadena un postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Cada reordenación desencadena un postback ([haga clic para ver la imagen de tamaño completo](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Siguiente](drag-and-drop-via-reorderlist-vb.md)
