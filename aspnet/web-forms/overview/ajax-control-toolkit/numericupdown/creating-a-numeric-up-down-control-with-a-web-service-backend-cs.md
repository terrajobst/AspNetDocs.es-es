---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Creación de un valor numérico arriba/abajo el Control con un back-end de Web Service (C#) | Microsoft Docs
author: wenz
description: En lugar de permitir que un usuario escriba un valor en una casilla de verificación, una numérica arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar c como más...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: b8160c6f5ac090e120e86f4273749b756857967e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59385714"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="ece40-103">Crear un control NumericUpDown con un back-end de servicio web (C#)</span><span class="sxs-lookup"><span data-stu-id="ece40-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="ece40-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ece40-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ece40-105">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ece40-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="ece40-106">En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar tan más cómodo.</span><span class="sxs-lookup"><span data-stu-id="ece40-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ece40-107">De forma predeterminada, el control NumericUpDown siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.</span><span class="sxs-lookup"><span data-stu-id="ece40-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="ece40-108">Información general</span><span class="sxs-lookup"><span data-stu-id="ece40-108">Overview</span></span>

<span data-ttu-id="ece40-109">En lugar de permitir que un usuario escriba un valor en una casilla de verificación, un valor numérico arriba/abajo control (que existe en Windows y otros sistemas operativos) podría resultar tan más cómodo.</span><span class="sxs-lookup"><span data-stu-id="ece40-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="ece40-110">De forma predeterminada, el `NumericUpDown` control siempre aumenta o disminuye un valor en 1, pero más flexibilidad demuestra que un servicio web.</span><span class="sxs-lookup"><span data-stu-id="ece40-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="ece40-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="ece40-111">Steps</span></span>

<span data-ttu-id="ece40-112">ASP.NET AJAX Control Toolkit contiene el `NumericUpDown` extensor que agrega automáticamente dos botones a un cuadro de texto: Uno para aumentar su valor, uno para reducirlo.</span><span class="sxs-lookup"><span data-stu-id="ece40-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="ece40-113">Sin embargo, el control también admite una llamada al servicio web (o una llamada al método de página).</span><span class="sxs-lookup"><span data-stu-id="ece40-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="ece40-114">Cada vez que el arriba o hacia abajo del botón se hace clic en, el código de JavaScript se conecta al servidor web de código y ejecuta un método no existe.</span><span class="sxs-lookup"><span data-stu-id="ece40-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="ece40-115">La firma del método es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="ece40-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="ece40-116">El `current` argumento es el valor actual en el cuadro de texto; el `tag` atributo es los datos de contexto adicional que pueden establecerse como una propiedad de la `NumericUpDown` extensor (pero no es necesario).</span><span class="sxs-lookup"><span data-stu-id="ece40-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="ece40-117">Para este ejemplo, el numérica arriba/abajo el control solo permitirá que los valores que son potencias de dos: 1, 2, 4, 8, 16, 32, 64 y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="ece40-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="ece40-118">Por lo tanto, el método que se ejecuta cuando el usuario desea aumentar el valor debe duplicar el valor antiguo; el otro método debe dividir el valor por dos.</span><span class="sxs-lookup"><span data-stu-id="ece40-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="ece40-119">Así que aquí es el servicio web completa:</span><span class="sxs-lookup"><span data-stu-id="ece40-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="ece40-120">Por último, cree una nueva página ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ece40-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="ece40-121">Como es habitual, necesita un `ScriptManager` (control), un `TextBox` control y un `NumericUpDownExtender` control.</span><span class="sxs-lookup"><span data-stu-id="ece40-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="ece40-122">Por último, tendrá que proporcionar la información del servicio web:</span><span class="sxs-lookup"><span data-stu-id="ece40-122">For the latter, you have to provide the web service information:</span></span>

- `ServiceDownMethod` <span data-ttu-id="ece40-123">nombre de la profundidad método web o método de página</span><span class="sxs-lookup"><span data-stu-id="ece40-123">name of the down web method or page method</span></span>
- `ServiceDownPath` <span data-ttu-id="ece40-124">ruta de acceso al servicio web con el método de servicio fuera de servicio; Omitir si está utilizando un método de página</span><span class="sxs-lookup"><span data-stu-id="ece40-124">path to the web service with the down service method; omit if you are using a page method</span></span>
- `ServiceUpMethod` <span data-ttu-id="ece40-125">nombre de la copia de método web o método de página</span><span class="sxs-lookup"><span data-stu-id="ece40-125">name of the up web method or page method</span></span>
- `ServiceUpPath` <span data-ttu-id="ece40-126">ruta de acceso al servicio web con el método de servicio arriba; Omitir si está utilizando un método de página</span><span class="sxs-lookup"><span data-stu-id="ece40-126">path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="ece40-127">Este es el marcado completo para la página:</span><span class="sxs-lookup"><span data-stu-id="ece40-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="ece40-128">Si ejecuta la página, observe cómo el valor en el cuadro de texto siempre duplica cuando haga clic en el botón superior y se reduce a la mitad al hacer clic en el botón inferior.</span><span class="sxs-lookup"><span data-stu-id="ece40-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


[![O<span data-ttu-id="ece40-129">aparecen solo números que sean la potencia de 2]</span><span class="sxs-lookup"><span data-stu-id="ece40-129">nly numbers that are a power of 2 appear]</span></span>(creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

<span data-ttu-id="ece40-130">Aparecen únicamente los números que sean la potencia de 2 ([haga clic aquí para ver imagen en tamaño completo](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ece40-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ece40-131">Siguiente</span><span class="sxs-lookup"><span data-stu-id="ece40-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
