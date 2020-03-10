---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Crear un control numérico de arriba y abajo con un back-endC#de servicio Web () | Microsoft Docs
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría demostrar como más c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509017"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="9b262-103">Crear un control NumericUpDown con un back-end de servicio web (C#)</span><span class="sxs-lookup"><span data-stu-id="9b262-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="9b262-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9b262-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9b262-105">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9b262-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="9b262-106">En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo.</span><span class="sxs-lookup"><span data-stu-id="9b262-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="9b262-107">De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="9b262-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="9b262-108">Información general</span><span class="sxs-lookup"><span data-stu-id="9b262-108">Overview</span></span>

<span data-ttu-id="9b262-109">En lugar de permitir que un usuario escriba un valor en una casilla, un control numérico de arriba y abajo (que existe en Windows y otros sistemas operativos) podría resultar más cómodo.</span><span class="sxs-lookup"><span data-stu-id="9b262-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="9b262-110">De forma predeterminada, el control `NumericUpDown` siempre aumenta o disminuye un valor en 1, pero un servicio Web demuestra una mayor flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="9b262-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="9b262-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="9b262-111">Steps</span></span>

<span data-ttu-id="9b262-112">El kit de herramientas de control de ASP.NET AJAX contiene el extensor `NumericUpDown`, que agrega automáticamente dos botones a un cuadro de texto: uno para aumentar su valor, uno para reducirlo.</span><span class="sxs-lookup"><span data-stu-id="9b262-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="9b262-113">Sin embargo, el control también admite una llamada de servicio Web (o una llamada al método de página).</span><span class="sxs-lookup"><span data-stu-id="9b262-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="9b262-114">Cada vez que se hace clic en el botón arriba o abajo, el código JavaScript se conecta al servidor Web y ejecuta un método allí.</span><span class="sxs-lookup"><span data-stu-id="9b262-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="9b262-115">La firma del método es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="9b262-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="9b262-116">El `current` argumento es el valor actual del cuadro de texto; el atributo `tag` es datos de contexto adicionales que se pueden establecer como una propiedad del extensor `NumericUpDown` (pero no es necesario).</span><span class="sxs-lookup"><span data-stu-id="9b262-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="9b262-117">En este ejemplo, el control numérico de arriba y abajo solo permitirá valores que sean potencias de dos: 1, 2, 4, 8, 16, 32, 64, etc.</span><span class="sxs-lookup"><span data-stu-id="9b262-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="9b262-118">Por lo tanto, el método ejecutado cuando el usuario desea aumentar el valor debe doblar el valor anterior; el otro método debe dividir el valor por dos.</span><span class="sxs-lookup"><span data-stu-id="9b262-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="9b262-119">Este es el servicio web completo:</span><span class="sxs-lookup"><span data-stu-id="9b262-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="9b262-120">Por último, cree una nueva página de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9b262-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="9b262-121">Como de costumbre, necesita un control `ScriptManager`, un control `TextBox` y un control `NumericUpDownExtender`.</span><span class="sxs-lookup"><span data-stu-id="9b262-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="9b262-122">En este último caso, tendrá que proporcionar la información del servicio Web:</span><span class="sxs-lookup"><span data-stu-id="9b262-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="9b262-123">`ServiceDownMethod` nombre del método Web o del método de página inactivo</span><span class="sxs-lookup"><span data-stu-id="9b262-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="9b262-124">`ServiceDownPath` ruta de acceso al servicio Web con el método de servicio inactivo; omitir si se usa un método de página</span><span class="sxs-lookup"><span data-stu-id="9b262-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="9b262-125">`ServiceUpMethod` nombre del método Web o del método de página up</span><span class="sxs-lookup"><span data-stu-id="9b262-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="9b262-126">`ServiceUpPath` ruta de acceso al servicio Web con el método de servicio up; omitir si se usa un método de página</span><span class="sxs-lookup"><span data-stu-id="9b262-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="9b262-127">Este es el marcado completo de la página:</span><span class="sxs-lookup"><span data-stu-id="9b262-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="9b262-128">Si ejecuta la página, observe que el valor del cuadro de texto siempre se duplica al hacer clic en el botón superior y se reduce al hacer clic en el botón inferior.</span><span class="sxs-lookup"><span data-stu-id="9b262-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="9b262-129">[![solo aparecen los números que son una potencia de 2](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9b262-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="9b262-130">Solo aparecen los números que son una potencia de 2 ([haga clic para ver la imagen de tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9b262-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9b262-131">Siguiente</span><span class="sxs-lookup"><span data-stu-id="9b262-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
