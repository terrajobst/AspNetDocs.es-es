---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlar dinámicamente las animaciones UpdatePanel (VB) | Microsoft Docs
author: wenz
description: El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control. Para el contenido de...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599684"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="8f419-104">Controlar dinámicamente las animaciones UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="8f419-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>

<span data-ttu-id="8f419-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8f419-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8f419-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8f419-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="8f419-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="8f419-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8f419-108">Para el contenido de un UpdatePanel, existe un extensor especial que se basa en gran medida en el marco de animación: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="8f419-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="8f419-109">También puede funcionar junto con los desencadenadores UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="8f419-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="8f419-110">Información general del</span><span class="sxs-lookup"><span data-stu-id="8f419-110">Overview</span></span>

<span data-ttu-id="8f419-111">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="8f419-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8f419-112">Para el contenido de un `UpdatePanel`, existe un extensor especial que se basa en gran medida en el marco de animación: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="8f419-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="8f419-113">También puede trabajar junto con desencadenadores `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="8f419-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="8f419-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="8f419-114">Steps</span></span>

<span data-ttu-id="8f419-115">El primer paso es lo habitual para incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="8f419-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="8f419-116">La animación de este escenario se aplicará a una presentación de la hora actual.</span><span class="sxs-lookup"><span data-stu-id="8f419-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="8f419-117">Esta información se puede escribir en una etiqueta mediante el método `Page_Load()`, o (por simplicidad) se usa el siguiente código en línea:</span><span class="sxs-lookup"><span data-stu-id="8f419-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="8f419-118">Además, se crea un botón para desencadenar la actualización del tiempo:</span><span class="sxs-lookup"><span data-stu-id="8f419-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="8f419-119">A continuación, este código se coloca en la sección `<ContentTemplate>` de un elemento `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="8f419-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="8f419-120">El atributo `UpdateMode` del panel debe establecerse en `"Conditional"`, ya que solo los desencadenadores pueden actualizar el contenido del panel.</span><span class="sxs-lookup"><span data-stu-id="8f419-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="8f419-121">En la sección `<Triggers>` del `UpdatePanel`, se crea un desencadenador de postback asincrónico y se vincula al evento `Click` del botón.</span><span class="sxs-lookup"><span data-stu-id="8f419-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="8f419-122">Por lo tanto, si el usuario hace clic en el botón, se actualiza el `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="8f419-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="8f419-123">Este es el marcado del control `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="8f419-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="8f419-124">Por último, se debe configurar la `UpdatePanelAnimationExtender`: establezca el atributo `TargetControlID` en el identificador del panel y defina una animación dentro del objeto extender.</span><span class="sxs-lookup"><span data-stu-id="8f419-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="8f419-125">La difuminación en tiene sentido, lo que crea un atractivo visual interesante en la hora actualizada.</span><span class="sxs-lookup"><span data-stu-id="8f419-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="8f419-126">El marcado del extensor puede tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="8f419-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="8f419-127">Ejecute el archivo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="8f419-127">Run the file in the browser.</span></span> <span data-ttu-id="8f419-128">Siempre que se hace clic en el botón, la hora actual se muestra en el panel, siempre atenuada durante un segundo.</span><span class="sxs-lookup"><span data-stu-id="8f419-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="8f419-129">[![se difumina la hora actual](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f419-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="8f419-130">La hora actual se difumina en ([haga clic para ver la imagen de tamaño completo](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f419-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8f419-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="8f419-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
