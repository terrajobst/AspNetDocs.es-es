---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animación según una condición (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Si una animación es...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 476b0cf80fa7c04cd8b8f9a92060ddabb9d14c13
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484171"
---
# <a name="animation-depending-on-a-condition-c"></a>Animación según una condición (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. El hecho de que una animación se ejecute o no también puede depender de una condición en forma de código JavaScript.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el obligatorio `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo. En lugar de una de las animaciones normales, el elemento `<Condition>` entra en juego. El código JavaScript proporcionado como el valor del atributo `ConditionScript` se ejecuta en tiempo de ejecución. Si se evalúa como true, se ejecuta la animación; de lo contrario, no. El marcado siguiente proporciona dos animaciones, cada una de las cuales se ejecuta en el 50% de los casos de forma aleatoria. Dado que solo puede haber una animación dentro de `<OnLoad>`, las dos animaciones `<Condition>` se unen juntas mediante el elemento `<Sequence>`:

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

Tenga en cuenta que el signo menor que (`<`) en el atributo `ConditionScript` debe ser un carácter de escape (). Al ejecutar este script, no se ejecuta ninguna animación, o bien uno de los dos, o ambos.

[![el panel se atenúa sin cambiar de tamaño, por lo que la segunda animación se ejecuta, la primera no](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)

El panel se atenúa sin cambiar el tamaño, por lo que la segunda animación se ejecuta, pero la primera no ([hacer clic para ver la imagen de tamaño completo](animation-depending-on-a-condition-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-cs.md)
> [Siguiente](picking-one-animation-out-of-a-list-cs.md)
