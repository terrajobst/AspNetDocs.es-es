---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Agregar dinámicamente un panel Accordion (C#) | Microsoft Docs
author: wenz
description: El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Normalmente, los paneles se declaran...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497983"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a>Agregar dinámicamente un panel Accordion (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.

## <a name="overview"></a>Información general

El control Accordion en el kit de herramientas de control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.

## <a name="steps"></a>Pasos

El control Accordion expone todas las propiedades importantes en el código del lado servidor. Entre otras cosas, la propiedad `Panes` concede acceso a la colección de paneles que componen el Accordion. Cada panel es de tipo `AccordionPane`. Por lo tanto, es trivial crear este tipo de panel:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

La propiedad `HeaderContainer` de `AccordionPane` proporciona acceso a los controles ASP.NET dentro de la sección de encabezado del panel; la propiedad `ContentContainer` de `AccordionPane` hace lo mismo para la sección de contenido del panel. Esto permite que el código ASP.NET agregue contenido a los paneles:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Por último, los paneles se deben agregar a la colección de `Panes` de la Accordion:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

A continuación se muestra un código completo del lado servidor que agrega dos paneles a un control Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

El único elemento que falta es el propio Accordion, que depende de la presencia del control `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Para finalizar el ejemplo, las dos clases CSS a las que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

[![los datos del Accordion se agregaron dinámicamente mediante el código del lado servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Los datos en el Accordion se agregaron dinámicamente mediante el código del lado servidor ([haga clic para ver la imagen de tamaño completo](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-cs.md)
> [Siguiente](databinding-to-an-accordion-vb.md)
