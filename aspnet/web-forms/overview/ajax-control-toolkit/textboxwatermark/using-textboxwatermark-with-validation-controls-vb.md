---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usar TextBoxWatermark con controles de validación (VB) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro,...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 141cae26c9e52be510e2a5a8f816cbac977dcf3d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74597228"
---
# <a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="b3356-104">Usar TextBoxWatermark con los controles de validación (VB)</span><span class="sxs-lookup"><span data-stu-id="b3356-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>

<span data-ttu-id="b3356-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b3356-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b3356-106">[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b3356-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="b3356-107">El control TextBoxWatermark en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro.</span><span class="sxs-lookup"><span data-stu-id="b3356-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="b3356-108">Cuando un usuario hace clic en el cuadro, se vacía.</span><span class="sxs-lookup"><span data-stu-id="b3356-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="b3356-109">Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado.</span><span class="sxs-lookup"><span data-stu-id="b3356-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="b3356-110">Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.</span><span class="sxs-lookup"><span data-stu-id="b3356-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="b3356-111">Información general del</span><span class="sxs-lookup"><span data-stu-id="b3356-111">Overview</span></span>

<span data-ttu-id="b3356-112">El control `TextBoxWatermark` en el kit de herramientas de control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro.</span><span class="sxs-lookup"><span data-stu-id="b3356-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="b3356-113">Cuando un usuario hace clic en el cuadro, se vacía.</span><span class="sxs-lookup"><span data-stu-id="b3356-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="b3356-114">Si el usuario deja el cuadro sin escribir texto, vuelve a aparecer el texto rellenado.</span><span class="sxs-lookup"><span data-stu-id="b3356-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="b3356-115">Esto puede entrar en conflicto con los controles de validación de ASP.NET en la misma página, pero estos problemas pueden solucionarse.</span><span class="sxs-lookup"><span data-stu-id="b3356-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="b3356-116">Pasos</span><span class="sxs-lookup"><span data-stu-id="b3356-116">Steps</span></span>

<span data-ttu-id="b3356-117">La configuración básica del ejemplo es la siguiente: un control de `TextBox` se marca mediante un control `TextBoxWatermarkExtender`.</span><span class="sxs-lookup"><span data-stu-id="b3356-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="b3356-118">Un botón desencadena un postback y se usará más adelante para desencadenar los controles de validación en la página.</span><span class="sxs-lookup"><span data-stu-id="b3356-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="b3356-119">Además, se requiere un control `ScriptManager` para inicializar ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="b3356-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="b3356-120">Ahora, agregue un control `RequiredFieldValidator` que compruebe si hay texto en el campo cuando se envía el formulario.</span><span class="sxs-lookup"><span data-stu-id="b3356-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="b3356-121">La propiedad `InitialValue` del validador debe establecerse en el mismo valor que se utiliza en el control de `TextBoxWatermarkExtender`: cuando se envía el formulario, el valor de un cuadro de texto sin modificar es el valor de marca de agua que contiene:</span><span class="sxs-lookup"><span data-stu-id="b3356-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="b3356-122">Sin embargo, hay un problema con este enfoque: Si el cliente deshabilita JavaScript, el campo de texto no se rellena previamente con el texto de marca de agua, por lo que el `RequiredFieldValidator` no desencadena un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b3356-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="b3356-123">Por lo tanto, se requiere un segundo control `RequiredFieldValidator` que compruebe si hay un cuadro de texto vacío (omitiendo el atributo `InitialValue`).</span><span class="sxs-lookup"><span data-stu-id="b3356-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="b3356-124">Dado que ambos validadores usan `Display`=`"Dynamic"`, el usuario final no puede distinguir de la apariencia visual de los dos validadores que se ha desencadenado; en su lugar, parece que solo uno de ellos.</span><span class="sxs-lookup"><span data-stu-id="b3356-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="b3356-125">Por último, agregue algún código del lado servidor para generar el texto en el campo si ningún validador emitió un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="b3356-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="b3356-126">[![el validador se queja de que no hay texto en el campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b3356-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="b3356-127">El validador se queja de que no hay texto en el campo ([haga clic para ver la imagen de tamaño completo](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b3356-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b3356-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="b3356-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
