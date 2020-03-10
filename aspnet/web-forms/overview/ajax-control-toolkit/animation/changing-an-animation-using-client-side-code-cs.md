---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Cambiar una animación mediante el código del lado clienteC#() | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. La animación también puede...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484057"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Cambiar una animación con código de cliente (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se puede cambiar la animación mediante el código JavaScript del lado cliente personalizado.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se puede cambiar la animación mediante el código JavaScript del lado cliente personalizado.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

La animación real se inicia mediante un botón HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Tenga en cuenta que no hay ningún nodo `<Animations>` en el control `AnimationExtender`. El código JavaScript personalizado se usa para proporcionar las animaciones que se van a usar con el control.

Al igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil de asignar una animación al extensor. Sin embargo, el extensor expone varios métodos para leer y escribir animaciones registradas con los distintos eventos (`OnClick`, `OnLoad`, etc.). A continuación se muestran algunos ejemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

El formato del valor devuelto de las funciones de `get_*()` y el formato del argumento para las funciones de `set_*()` es una cadena JSON, que proporciona una representación de objeto de lo que sería el marcado XML. Actualmente, no hay ninguna manera de pasar un objeto en, pero es posible leer un objeto de una animación determinada (métodos`get_OnXXXBehavior()`).

A continuación se muestra una cadena JSON (sin las comillas delimitadoras con el formato correcto) que representa una animación desencadenada por el botón, pero animando el panel mediante el cambio de tamaño y su difuminación al mismo tiempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

El siguiente código de JavaScript asigna este descriptor de comandos JSON a la animación `OnClick` del extensor actual y lo ejecuta:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

[![la animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado) ([haga clic para ver la imagen de tamaño completo](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-cs.md)
> [Siguiente](animating-an-updatepanel-control-cs.md)
