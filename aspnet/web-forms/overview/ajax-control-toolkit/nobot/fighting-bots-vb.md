---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Combatir bots (VB) | Microsoft Docs
author: wenz
description: Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario. El control NoBot en el ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606385"
---
# <a name="fighting-bots-vb"></a><span data-ttu-id="310f8-104">Combatir los bots (VB)</span><span class="sxs-lookup"><span data-stu-id="310f8-104">Fighting Bots (VB)</span></span>

<span data-ttu-id="310f8-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="310f8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="310f8-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="310f8-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="310f8-107">Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario.</span><span class="sxs-lookup"><span data-stu-id="310f8-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="310f8-108">El control NoBot en el kit de herramientas de control de AJAX de ASP.NET puede ayudar a combatir esos bots.</span><span class="sxs-lookup"><span data-stu-id="310f8-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="310f8-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="310f8-109">Overview</span></span>

<span data-ttu-id="310f8-110">Los bots automatizados transforman Weblogs y otros sitios web con correo no deseado, enviando formularios de comentarios sin intervención del usuario.</span><span class="sxs-lookup"><span data-stu-id="310f8-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="310f8-111">El control NoBot en el kit de herramientas de control de AJAX de ASP.NET puede ayudar a combatir esos bots.</span><span class="sxs-lookup"><span data-stu-id="310f8-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="310f8-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="310f8-112">Steps</span></span>

<span data-ttu-id="310f8-113">Un enfoque común para derrotar bots es usar la prueba de convertir pública totalmente automatizada CAPTCHAs para indicar a los equipos y a los seres humanos la diferencia.</span><span class="sxs-lookup"><span data-stu-id="310f8-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="310f8-114">Una prueba de convertir era originalmente una prueba en la que alguien necesitaba decidir si un asociado de comunicación es un usuario o un equipo.</span><span class="sxs-lookup"><span data-stu-id="310f8-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="310f8-115">En la web, un CAPTCHA normalmente se compone de una imagen con algunas letras distorsionadas.</span><span class="sxs-lookup"><span data-stu-id="310f8-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="310f8-116">La idea es que solo un usuario humano puede leer las letras de la imagen, mientras que los algoritmos de OCR no funcionarán.</span><span class="sxs-lookup"><span data-stu-id="310f8-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="310f8-117">Este enfoque tiene varias ventajas y desventajas, pero una explicación de esto está fuera del ámbito de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="310f8-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="310f8-118">No obstante, hay un control en el kit de herramientas de control de ASP.NET AJAX que proporciona un enfoque similar: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="310f8-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="310f8-119">Es más fácil de resolver que un CAPTCHA, pero es muy fácil de usar y tarifas muy bien en sitios web como blogs en los que se considera un éxito si la mayoría de los intentos de correo no deseado son derrotas, lo que puede hacer el control de `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="310f8-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="310f8-120">`NoBot` intercepta el PostBack del formulario Web ASP.NET actual si se cumple al menos una de estas condiciones:</span><span class="sxs-lookup"><span data-stu-id="310f8-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="310f8-121">El explorador no puede resolver un rompecabezas de JavaScript (por ejemplo, cuando se desactiva JavaScript)</span><span class="sxs-lookup"><span data-stu-id="310f8-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="310f8-122">El usuario envió el formulario a Fast</span><span class="sxs-lookup"><span data-stu-id="310f8-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="310f8-123">La dirección IP del cliente envió el formulario demasiado a menudo en un período de tiempo determinado.</span><span class="sxs-lookup"><span data-stu-id="310f8-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="310f8-124">Para comprobar estas condiciones, el control `NoBot` requiere estos atributos (todos ellos opcionales):</span><span class="sxs-lookup"><span data-stu-id="310f8-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="310f8-125">`ResponseMinimumDelaySeconds` cantidad mínima de segundos entre los postbacks</span><span class="sxs-lookup"><span data-stu-id="310f8-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="310f8-126">`CutoffWindowSeconds` intervalo de tiempo en el que los postbacks de una IP son medidas</span><span class="sxs-lookup"><span data-stu-id="310f8-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="310f8-127">`CutoffMaximumInstances` cantidad máxima de segundos por intervalo de tiempo</span><span class="sxs-lookup"><span data-stu-id="310f8-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="310f8-128">El marcado siguiente exige que transcurren al menos dos segundos entre los postbacks y que solo hay cinco postbacks o menos en un intervalo de 30 segundos:</span><span class="sxs-lookup"><span data-stu-id="310f8-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="310f8-129">Como es habitual, asegúrese de incluir el `ScriptManager` en la página para que se cargue la biblioteca de AJAX de ASP.NET y se pueda usar el kit de herramientas de control:</span><span class="sxs-lookup"><span data-stu-id="310f8-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="310f8-130">Dado que la mayoría de las comprobaciones que se `NoBot` realizar se producen en el lado servidor, debe comprobar el resultado de estas validaciones.</span><span class="sxs-lookup"><span data-stu-id="310f8-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="310f8-131">Esto se puede hacer llamando al método `IsValid()` de `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="310f8-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="310f8-132">Tiene un argumento (como `out` parámetro/`ByRef` parámetro) que es de tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="310f8-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="310f8-133">Su representación de cadena contiene la razón cuando se produce un error en la comprobación y `Valid` de otro modo.</span><span class="sxs-lookup"><span data-stu-id="310f8-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="310f8-134">El código siguiente genera un mensaje según el resultado de `NoBot`:</span><span class="sxs-lookup"><span data-stu-id="310f8-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="310f8-135">Por último, necesitará un formulario para enviar y un elemento etiqueta para generar el mensaje, y ya ha terminado.</span><span class="sxs-lookup"><span data-stu-id="310f8-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="310f8-136">Cuando ejecute este script y desactive JavaScript o envíe el formulario en los primeros dos segundos o envíe el formulario siete veces en un plazo de treinta segundos, recibirá un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="310f8-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="310f8-137">Sin embargo, use este control con prudencia, ya que solo unos 90-95% de los usuarios tienen activado JavaScript, por lo que el 5-10% de los usuarios producirán un error en la prueba de `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="310f8-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="310f8-138">[![este mensaje de error podría deberse a un bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="310f8-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="310f8-139">Este mensaje de error puede deberse a un bot ([hacer clic para ver la imagen de tamaño completo](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="310f8-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="310f8-140">Anterior</span><span class="sxs-lookup"><span data-stu-id="310f8-140">Previous</span></span>](fighting-bots-cs.md)
