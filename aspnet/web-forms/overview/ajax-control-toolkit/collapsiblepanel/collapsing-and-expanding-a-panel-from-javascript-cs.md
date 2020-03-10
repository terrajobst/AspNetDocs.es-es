---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Contraer y expandir un panel desde JavaScript (C#) | Microsoft Docs
author: wenz
description: El control CollapsiblePanel de ASP.NET AJAX control Toolkit amplía un panel y le proporciona la capacidad de contraer su contenido y expandirlo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497719"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Contraer y expandir un panel desde JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> El control CollapsiblePanel en el kit de herramientas de control de AJAX de ASP.NET amplía un panel y le proporciona la capacidad de contraer su contenido y expandirlo de nuevo. Estas dos acciones también se pueden desencadenar desde el código JavaScript personalizado.

## <a name="overview"></a>Información general

El control CollapsiblePanel en el kit de herramientas de control de AJAX de ASP.NET amplía un panel y le proporciona la capacidad de contraer su contenido y expandirlo de nuevo. Estas dos acciones también se pueden desencadenar desde el código JavaScript personalizado.

## <a name="steps"></a>Pasos

En primer lugar, cree una nueva página de ASP.NET e incluya el `ScriptManager` en el elemento `<form>`. Esto carga la biblioteca de ASP.NET AJAX necesaria para el control Toolkit:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

A continuación, cree un panel con texto para que se pueda visualizar el efecto de contraer o expandir:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Como puede ver, el panel hace referencia a una clase CSS que se muestra aquí (y básicamente define un color de fondo y el ancho del panel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

El control `CollapsiblePanelExtender` requiere el atributo `TargetControlID` para que el kit de herramientas sepa qué panel se va a contraer o expandir según lo solicitado:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Desafortunadamente, el extensor no expone actualmente una API específica para contraer o expandir el panel, pero sí algunos métodos no documentados. En primer lugar, agregue tres botones HTML a la página que desencadenarán el JavaScript del lado cliente para contraer o expandir el contenido del panel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

En el código de JavaScript del lado cliente (iniciado con `<script type="text/javascript">`), debe usarse el método `$find()` para tener acceso al `CollapsiblePanelExtender`. `$find("cpe")` devolverá una referencia a él. Desde allí, los métodos específicos resolverán la tarea a mano.

El método para abrir (expandir) el panel se denomina `_doOpen()`; el código siguiente implementa la función de `doOpen()` llamada cuando se hace clic en el primer botón:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Para cerrar o contraer el panel, es necesario ejecutar el método `_doClose()`. Por tanto, cuando el usuario hace clic en el segundo botón, se llama al siguiente código JavaScript:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

El tercer botón alterna el estado del panel: de contraído a expandido y viceversa. El `CollapsiblePanelExtender` expone el método de `toggle()` que hace exactamente esto: invierte el estado del panel. Sin embargo, también hay otro enfoque (que usa internamente el método `toggle()`): el método `get_Collapsed()` de la `CollapsiblePanelExtender()` nos indica si el panel está contraído o no. En función del valor devuelto de esta función, el panel se expande (método`_doOpen()`) o contraído (`_doClose()`):

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

[![el tercer botón cambia el estado del panel: de contraído a expandido y atrás](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

El tercer botón cambia el estado del panel: de contraído a expandido y hacia atrás ([haga clic para ver la imagen de tamaño completo](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](collapsing-and-expanding-a-panel-from-javascript-vb.md)
