---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Ejecutar varias animaciones al mismo tiempo (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Permite ejecutar Servera...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: fc11debd06c897c29db56d42167d483f5608cdf8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575313"
---
# <a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="6ff88-104">Ejecutar varias animaciones al mismo tiempo (VB)</span><span class="sxs-lookup"><span data-stu-id="6ff88-104">Executing Several Animations at The Same Time (VB)</span></span>

<span data-ttu-id="6ff88-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6ff88-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6ff88-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6ff88-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="6ff88-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="6ff88-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6ff88-108">Permite ejecutar varias animaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="6ff88-108">It allows to run several animations in a parallel fashion.</span></span>

## <a name="overview"></a><span data-ttu-id="6ff88-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="6ff88-109">Overview</span></span>

<span data-ttu-id="6ff88-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="6ff88-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6ff88-111">Permite ejecutar varias animaciones en paralelo.</span><span class="sxs-lookup"><span data-stu-id="6ff88-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="6ff88-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="6ff88-112">Steps</span></span>

<span data-ttu-id="6ff88-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="6ff88-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="6ff88-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="6ff88-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="6ff88-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="6ff88-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="6ff88-116">A continuación, agregue el `AnimationExtender` a la página, proporcionando un `ID`, el atributo `TargetControlID` y el `runat="server"`obligatorio:</span><span class="sxs-lookup"><span data-stu-id="6ff88-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="6ff88-117">Dentro del nodo de `<Animations>`, utilice `<OnLoad>` para ejecutar las animaciones una vez que la página se haya cargado por completo.</span><span class="sxs-lookup"><span data-stu-id="6ff88-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="6ff88-118">Por lo general, `<OnLoad>` solo acepta una animación.</span><span class="sxs-lookup"><span data-stu-id="6ff88-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="6ff88-119">El marco de animación permite combinar varias animaciones en una mediante el elemento `<Parallel>`.</span><span class="sxs-lookup"><span data-stu-id="6ff88-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="6ff88-120">Todas las animaciones de `<Parallel>` se ejecutan al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="6ff88-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="6ff88-121">A continuación se muestra un posible marcado para el control de `AnimationExtender`, que atenúa y cambia el tamaño del panel al mismo tiempo:</span><span class="sxs-lookup"><span data-stu-id="6ff88-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="6ff88-122">Y de hecho: al ejecutar este script, se muestra el panel y, a continuación, se cambia el tamaño (más de triplicar el ancho y la mitad de su altura) y se atenúa al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="6ff88-122">And indeed: when you run this script, the panel is displayed, then resizes (more than tripling its width and halving its height) and fades out at the same time.</span></span>

<span data-ttu-id="6ff88-123">[![el panel se atenúa y se cambia el tamaño (incluido su contenido, gracias al motor de representación del explorador)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6ff88-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="6ff88-124">El panel se atenúa y se cambia el tamaño (incluido su contenido, gracias al motor de representación del explorador) ([haga clic para ver la imagen de tamaño completo](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6ff88-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ff88-125">[Anterior](adding-animation-to-a-control-vb.md)
> [Siguiente](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6ff88-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
