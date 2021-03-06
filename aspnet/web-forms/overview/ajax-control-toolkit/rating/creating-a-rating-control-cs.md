---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Crear un control de clasificaciónC#() | Microsoft Docs
author: wenz
description: Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos. Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78496165"
---
# <a name="creating-a-rating-control-c"></a>Crear un control de clasificación (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)

> Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos. Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos el kit de herramientas de control para nuestra eliminación.

## <a name="overview"></a>Información general

Muchos sitios web, desde comercio electrónico hasta sitios de la comunidad, ofrecen a sus usuarios tarifas o artículos. Esto normalmente requiere cierto esfuerzo de codificación, pero tenemos el kit de herramientas de control para nuestra eliminación.

## <a name="steps"></a>Pasos

En primer lugar, necesita (al menos) dos tipos de imágenes: una para un elemento de calificación rellenado y otra para un elemento de clasificación vacío. Un elemento de clasificación suele ser una estrella o un smiley. En este escenario, encontrará tres archivos: Smiley. png y Empty. png y Smiley-done. png como parte de las descargas de código fuente de este tutorial.

A continuación, cree un nuevo archivo ASP.NET y empiece por agregarle un `ScriptManager` control:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

A continuación, agregue el control `Rating` de ASP.NET AJAX control Toolkit. Se deben establecer los siguientes atributos para este ejemplo:

- `CurrentRating` la clasificación inicial que se va a usar
- `MaxRating` la clasificación máxima
- `EmptyStarCssClass` la clase CSS que se va a usar cuando un elemento de clasificación (Star) esté vacío
- `FilledStarCssClass` la clase CSS que se va a usar cuando se rellena un elemento de clasificación (Star)
- `StarCssClass` la clase CSS que se va a usar para una estadística visible
- `WaitingStarCssClass` la clase CSS que se va a usar mientras se envía una clasificación por estrellas al servidor

Y este es el marcado que crea un control de clasificación con cinco elementos (sonrisais) de los que no se rellena inicialmente ninguno:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

Las tres clases CSS a las que se hace referencia ahora deben mostrar los archivos de imagen adecuados, lo que es fácil de usar con CSS:

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

Asegúrese de proporcionar el ancho y el alto de las tres imágenes; de lo contrario, la pantalla puede parecer un poco desordenado.

Por último, el resultado de la clasificación debe mostrarse al usuario (o, al menos, guardarse en una base de datos). Por tanto, agregue una etiqueta para la salida de un mensaje de texto y un botón de envío para devolver el formulario de clasificación al servidor:

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

En el código del lado servidor, acceda al control clasificación a través de su `ID` y, a continuación, acceda a su propiedad `CurrentRating`, que es el número de los elementos de clasificación seleccionados, en el ejemplo un valor entre 0 y 5.

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

Guarde la página y cargarla en el explorador. Al mantener el mouse sobre los elementos de clasificación (inicialmente vacíos), se produce un efecto de JavaScript: la clasificación cambia. Al hacer clic en el conjunto de estrellas, se conserva la clasificación actual. Por último, al enviar el formulario, el código del lado servidor genera la clasificación seleccionada.

[![crear un sistema de clasificación con código mínimo](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)

Creación de un sistema de clasificación con código mínimo ([haga clic para ver la imagen de tamaño completo](creating-a-rating-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](creating-a-rating-control-vb.md)
