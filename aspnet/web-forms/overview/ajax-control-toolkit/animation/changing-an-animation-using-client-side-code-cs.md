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
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="7f341-104">Cambiar una animación con código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="7f341-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="7f341-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7f341-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7f341-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7f341-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="7f341-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="7f341-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7f341-108">También se puede cambiar la animación mediante el código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="7f341-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="7f341-109">Información general</span><span class="sxs-lookup"><span data-stu-id="7f341-109">Overview</span></span>

<span data-ttu-id="7f341-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="7f341-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7f341-111">También se puede cambiar la animación mediante el código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="7f341-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="7f341-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="7f341-112">Steps</span></span>

<span data-ttu-id="7f341-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="7f341-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="7f341-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="7f341-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="7f341-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="7f341-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="7f341-116">La animación real se inicia mediante un botón HTML:</span><span class="sxs-lookup"><span data-stu-id="7f341-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="7f341-117">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="7f341-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="7f341-118">Tenga en cuenta que no hay ningún nodo `<Animations>` en el control `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7f341-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="7f341-119">El código JavaScript personalizado se usa para proporcionar las animaciones que se van a usar con el control.</span><span class="sxs-lookup"><span data-stu-id="7f341-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="7f341-120">Al igual que con la API de servidor de `AnimationExtender`, no hay ninguna manera fácil de asignar una animación al extensor.</span><span class="sxs-lookup"><span data-stu-id="7f341-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="7f341-121">Sin embargo, el extensor expone varios métodos para leer y escribir animaciones registradas con los distintos eventos (`OnClick`, `OnLoad`, etc.).</span><span class="sxs-lookup"><span data-stu-id="7f341-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="7f341-122">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="7f341-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="7f341-123">El formato del valor devuelto de las funciones de `get_*()` y el formato del argumento para las funciones de `set_*()` es una cadena JSON, que proporciona una representación de objeto de lo que sería el marcado XML.</span><span class="sxs-lookup"><span data-stu-id="7f341-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="7f341-124">Actualmente, no hay ninguna manera de pasar un objeto en, pero es posible leer un objeto de una animación determinada (métodos`get_OnXXXBehavior()`).</span><span class="sxs-lookup"><span data-stu-id="7f341-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="7f341-125">A continuación se muestra una cadena JSON (sin las comillas delimitadoras con el formato correcto) que representa una animación desencadenada por el botón, pero animando el panel mediante el cambio de tamaño y su difuminación al mismo tiempo:</span><span class="sxs-lookup"><span data-stu-id="7f341-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="7f341-126">El siguiente código de JavaScript asigna este descriptor de comandos JSON a la animación `OnClick` del extensor actual y lo ejecuta:</span><span class="sxs-lookup"><span data-stu-id="7f341-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

<span data-ttu-id="7f341-127">[![la animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7f341-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="7f341-128">La animación se ejecuta inmediatamente, sin un clic del mouse (y con muy poco marcado) ([haga clic para ver la imagen de tamaño completo](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7f341-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7f341-129">[Anterior](executing-animations-using-client-side-code-cs.md)
> [Siguiente](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7f341-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
