---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modificar animaciones desde el lado servidor (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Las animaciones también pueden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: ebc311d1a931ad611d9556799c94440d41a9cf49
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483871"
---
# <a name="modifying-animations-from-the-server-side-vb"></a>Modificar animaciones desde el lado servidor (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)

> El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se pueden cambiar las animaciones en el lado servidor.

## <a name="overview"></a>Información general

El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. También se pueden cambiar las animaciones en el lado servidor.

## <a name="steps"></a>Pasos

En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

La animación se aplicará a un panel de texto que tiene el siguiente aspecto:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

El resto del código se ejecuta en el lado servidor y no utiliza marcado; en su lugar, usa código para crear el control de `AnimationExtender`:

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

Sin embargo, el kit de herramientas de control no proporciona actualmente un acceso de API para crear las animaciones individuales. No obstante, es posible establecer la propiedad animaciones del `AnimationExtender`en una cadena que contenga el marcado XML utilizado al asignar las animaciones mediante declaración. Con el fin de crear el XML que no debe contener el elemento `<Animations>` podría usar la compatibilidad con XML del .NET Framework o, como en el código siguiente, simplemente proporcione la cadena:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

Por último, agregue el control `AnimationExtender` a la página actual, dentro del elemento `<form runat="server">`, asegurándose de que la animación esté incluida y se ejecute:

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

[![la animación se crea mediante código/VB del C#lado servidor](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)

La animación se crea mediante el código/VB C#del lado servidor ([haga clic para ver la imagen de tamaño completo](modifying-animations-from-the-server-side-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](triggering-an-animation-in-another-control-vb.md)
> [Siguiente](executing-animations-using-client-side-code-vb.md)
