---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Agregar dinámicamente un panel Accordion (VB) | Microsoft Docs
author: wenz
description: El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Normalmente, los paneles se declaran...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607197"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Agregar dinámicamente un panel Accordion (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.

## <a name="overview"></a>Información general del

El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.

## <a name="steps"></a>Pasos

El control Accordion expone todas las propiedades importantes en el código del lado servidor. Entre otras cosas, la propiedad `Panes` concede acceso a la colección de paneles que componen el Accordion. Cada panel es de tipo `AccordionPane`. Por lo tanto, es trivial crear este tipo de panel:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

La propiedad `HeaderContainer` de `AccordionPane` proporciona acceso a los controles ASP.NET dentro de la sección de encabezado del panel; la propiedad `ContentContainer` de `AccordionPane` hace lo mismo para la sección de contenido del panel. Esto permite que el código ASP.NET agregue contenido a los paneles:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Por último, los paneles se deben agregar a la colección de `Panes` de la Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

A continuación se muestra un código completo del lado servidor que agrega dos paneles a un control Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

El único elemento que falta es el propio Accordion, que depende de la presencia del control `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Para finalizar el ejemplo, las dos clases CSS a las que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

[![los datos del Accordion se agregaron dinámicamente mediante el código del lado servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Los datos en el Accordion se agregaron dinámicamente mediante el código del lado servidor ([haga clic para ver la imagen de tamaño completo](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-vb.md)
