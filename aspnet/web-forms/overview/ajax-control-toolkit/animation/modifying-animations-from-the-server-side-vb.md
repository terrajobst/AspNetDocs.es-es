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
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575232"
---
# <a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="fe831-104">Modificar animaciones desde el lado servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="fe831-104">Modifying Animations From The Server Side (VB)</span></span>

<span data-ttu-id="fe831-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fe831-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fe831-106">[Descargar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="fe831-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="fe831-107">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="fe831-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fe831-108">También se pueden cambiar las animaciones en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="fe831-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="fe831-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="fe831-109">Overview</span></span>

<span data-ttu-id="fe831-110">El control de animación de ASP.NET AJAX control Toolkit no es solo un control, sino un marco de trabajo completo para agregar animaciones a un control.</span><span class="sxs-lookup"><span data-stu-id="fe831-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="fe831-111">También se pueden cambiar las animaciones en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="fe831-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="fe831-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="fe831-112">Steps</span></span>

<span data-ttu-id="fe831-113">En primer lugar, incluya el `ScriptManager` en la página; después, se carga la biblioteca de ASP.NET AJAX, lo que permite usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="fe831-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="fe831-114">La animación se aplicará a un panel de texto que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="fe831-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="fe831-115">En la clase CSS asociada del panel, defina un color de fondo agradable y establezca también un ancho fijo para el panel:</span><span class="sxs-lookup"><span data-stu-id="fe831-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="fe831-116">El resto del código se ejecuta en el lado servidor y no utiliza marcado; en su lugar, usa código para crear el control de `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="fe831-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="fe831-117">Sin embargo, el kit de herramientas de control no proporciona actualmente un acceso de API para crear las animaciones individuales.</span><span class="sxs-lookup"><span data-stu-id="fe831-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="fe831-118">No obstante, es posible establecer la propiedad animaciones del `AnimationExtender`en una cadena que contenga el marcado XML utilizado al asignar las animaciones mediante declaración.</span><span class="sxs-lookup"><span data-stu-id="fe831-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="fe831-119">Con el fin de crear el XML que no debe contener el elemento `<Animations>` podría usar la compatibilidad con XML del .NET Framework o, como en el código siguiente, simplemente proporcione la cadena:</span><span class="sxs-lookup"><span data-stu-id="fe831-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="fe831-120">Por último, agregue el control `AnimationExtender` a la página actual, dentro del elemento `<form runat="server">`, asegurándose de que la animación esté incluida y se ejecute:</span><span class="sxs-lookup"><span data-stu-id="fe831-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]

<span data-ttu-id="fe831-121">[![la animación se crea mediante código/VB del C#lado servidor](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fe831-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="fe831-122">La animación se crea mediante el código/VB C#del lado servidor ([haga clic para ver la imagen de tamaño completo](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fe831-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe831-123">[Anterior](triggering-an-animation-in-another-control-vb.md)
> [Siguiente](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fe831-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
