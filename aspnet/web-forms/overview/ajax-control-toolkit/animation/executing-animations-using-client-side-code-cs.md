---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: Ejecutar animaciones mediante el código del lado cliente (C#) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. La ejecución de la animación...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: b6ba1553b9c8c51d5d6ae1679e53f9cc1d17b769
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599655"
---
# <a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="92e3c-104">Ejecutar animaciones con código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="92e3c-104">Executing Animations Using Client-Side Code (C#)</span></span>

<span data-ttu-id="92e3c-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="92e3c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="92e3c-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="92e3c-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="92e3c-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="92e3c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="92e3c-108">También se puede desencadenar la ejecución de la animación mediante el código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="92e3c-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="92e3c-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="92e3c-109">Overview</span></span>

<span data-ttu-id="92e3c-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="92e3c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="92e3c-111">También se puede desencadenar la ejecución de la animación mediante el código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="92e3c-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="92e3c-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="92e3c-112">Steps</span></span>

<span data-ttu-id="92e3c-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="92e3c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="92e3c-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="92e3c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="92e3c-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="92e3c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="92e3c-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="92e3c-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="92e3c-117">Dentro del nodo de `<Animations>`, utilice `<OnClick>` para ejecutar las animaciones una vez que el usuario haga clic en el panel.</span><span class="sxs-lookup"><span data-stu-id="92e3c-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="92e3c-118">Agregue dos animaciones para que se ejecuten en paralelo:</span><span class="sxs-lookup"><span data-stu-id="92e3c-118">Add two animations to be executed in parallel:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="92e3c-119">Para la demostración, esta animación (y cualquier otra animación creada con el kit de herramientas de control) se ejecuta mediante código JavaScript, una vez que se ejecuta la página.</span><span class="sxs-lookup"><span data-stu-id="92e3c-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="92e3c-120">En primer lugar, es necesario tener acceso al control `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="92e3c-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="92e3c-121">La biblioteca de ASP.NET AJAX proporciona la función `$find()` para esta tarea:</span><span class="sxs-lookup"><span data-stu-id="92e3c-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="92e3c-122">El control `AnimationExtender` expone una API enriquecida, que incluye métodos con nombres idénticos a los controladores de eventos utilizados en el marcado XML: `OnClick()`, `OnLoad()`, etc.</span><span class="sxs-lookup"><span data-stu-id="92e3c-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="92e3c-123">Por ejemplo, una llamada al método `OnClick()` ejecuta la animación dentro del elemento `<OnClick>` del control `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="92e3c-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="92e3c-124">Este es el código JavaScript completo del lado cliente que emula el clic en el panel una vez que la página se ha cargado completamente. tenga en cuenta que se usa el nombre de la función `pageLoad()`, al que se llama mediante ASP.NET AJAX una vez que se han cargado la página y todas las bibliotecas de JavaScript incluidas.</span><span class="sxs-lookup"><span data-stu-id="92e3c-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]

<span data-ttu-id="92e3c-125">[![la animación se ejecuta inmediatamente, sin un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92e3c-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="92e3c-126">La animación se ejecuta inmediatamente, sin un clic del mouse ([haga clic para ver la imagen de tamaño completo](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92e3c-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="92e3c-127">[Anterior](modifying-animations-from-the-server-side-cs.md)
> [Siguiente](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="92e3c-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
