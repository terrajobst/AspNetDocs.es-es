---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipular las propiedades de la sombra de laC#sombra desde el código de cliente () | Microsoft Docs
author: wenz
description: Personalización de la interfaz de edición de DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574096"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="7d0cc-103">Manipular propiedades DropShadow desde el código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="7d0cc-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>

<span data-ttu-id="7d0cc-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7d0cc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7d0cc-105">[Descargar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7d0cc-105">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="7d0cc-106">El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7d0cc-107">Las propiedades de este extensor también se pueden cambiar mediante el código JavaScript del cliente.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-107">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="7d0cc-108">Información general del</span><span class="sxs-lookup"><span data-stu-id="7d0cc-108">Overview</span></span>

<span data-ttu-id="7d0cc-109">El control de sombra en el kit de herramientas de control de AJAX extiende un panel con una sombra paralela.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="7d0cc-110">Las propiedades de este extensor también se pueden cambiar mediante el código JavaScript del cliente.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7d0cc-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="7d0cc-111">Steps</span></span>

<span data-ttu-id="7d0cc-112">El código comienza con un panel que contiene algunas líneas de texto:</span><span class="sxs-lookup"><span data-stu-id="7d0cc-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="7d0cc-113">La clase CSS asociada proporciona al panel un color de fondo agradable:</span><span class="sxs-lookup"><span data-stu-id="7d0cc-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="7d0cc-114">El `DropShadowExtender` se agrega para extender el panel con un efecto de sombra paralela, que se establece en 50%:</span><span class="sxs-lookup"><span data-stu-id="7d0cc-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="7d0cc-115">Después, el control ASP.NET AJAX `ScriptManager` permite que el kit de herramientas de control funcione:</span><span class="sxs-lookup"><span data-stu-id="7d0cc-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="7d0cc-116">Otro panel contiene dos vínculos de JavaScript para establecer la opacidad de la sombra paralela: el vínculo menos reduce la opacidad de la sombra, el vínculo más lo incrementa.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="7d0cc-117">La función de JavaScript `changeOpacity()` debe buscar primero el control `DropShadowExtender` en la página.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="7d0cc-118">ASP.NET AJAX define el método de `$find()` para esa tarea exactamente.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="7d0cc-119">A continuación, el método `get_Opacity()` recupera la opacidad actual, el método `set_Opacity()` lo establece.</span><span class="sxs-lookup"><span data-stu-id="7d0cc-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="7d0cc-120">Después, el código de JavaScript coloca el valor de la opacidad actual en el elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="7d0cc-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

<span data-ttu-id="7d0cc-121">[![se cambia la opacidad en el lado del cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7d0cc-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="7d0cc-122">La opacidad se cambia en el lado del cliente ([haga clic para ver la imagen de tamaño completo](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7d0cc-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d0cc-123">[Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Siguiente](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7d0cc-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
