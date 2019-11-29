---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Ejecutar animaciones con código de cliente (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. La ejecución de la animación...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575471"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Ejecutar animaciones con código de cliente (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se puede desencadenar la ejecución de la animación mediante el código JavaScript del lado cliente personalizado.

## <a name="overview"></a>Información general del

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se puede desencadenar la ejecución de la animación mediante el código JavaScript del lado cliente personalizado.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Dentro del nodo de `<Animations>`, utilice `<OnClick>` para ejecutar las animaciones una vez que el usuario haga clic en el panel. Agregue dos animaciones para que se ejecuten en paralelo:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Para la demostración, esta animación (y cualquier otra animación creada con el kit de herramientas de control) se ejecuta mediante código JavaScript, una vez que se ejecuta la página. En primer lugar, es necesario tener acceso al control `AnimationExtender`. La biblioteca de ASP.NET AJAX proporciona la función `$find()` para esta tarea:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

El control `AnimationExtender` expone una API enriquecida, que incluye métodos con nombres idénticos a los controladores de eventos utilizados en el marcado XML: `OnClick()`, `OnLoad()`, etc. Por ejemplo, una llamada al método `OnClick()` ejecuta la animación dentro del elemento `<OnClick>` del control `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Este es el código JavaScript completo del lado cliente que emula el clic en el panel una vez que la página se ha cargado completamente. tenga en cuenta que se usa el nombre de la función `pageLoad()`, al que se llama mediante ASP.NET AJAX una vez que se han cargado la página y todas las bibliotecas de JavaScript incluidas.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![la animación se ejecuta inmediatamente, sin un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse ([haga clic para ver la imagen de tamaño completo](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](modifying-animations-from-the-server-side-vb.md)
> [Siguiente](changing-an-animation-using-client-side-code-vb.md)
