---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usar DynamicPopulate con un control de usuario y JavaScriptC#() | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599151"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="d62fd-103">Usar DynamicPopulate con un control de usuario y JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="d62fd-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>

<span data-ttu-id="d62fd-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d62fd-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d62fd-105">[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d62fd-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="d62fd-106">El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="d62fd-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="d62fd-107">También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="d62fd-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="d62fd-108">Sin embargo, se debe tener especial cuidado cuando el extensor resida en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="d62fd-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="d62fd-109">Información general del</span><span class="sxs-lookup"><span data-stu-id="d62fd-109">Overview</span></span>

<span data-ttu-id="d62fd-110">El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="d62fd-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="d62fd-111">También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="d62fd-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="d62fd-112">Sin embargo, se debe tener especial cuidado cuando el extensor resida en un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="d62fd-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="d62fd-113">Pasos</span><span class="sxs-lookup"><span data-stu-id="d62fd-113">Steps</span></span>

<span data-ttu-id="d62fd-114">En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que llamará el control `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="d62fd-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="d62fd-115">El servicio Web implementa el método `getDate()` que espera un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web.</span><span class="sxs-lookup"><span data-stu-id="d62fd-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="d62fd-116">Este es el código (`DynamicPopulate.cs.asmx`de archivos), que recupera la fecha actual en uno de los tres formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d62fd-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="d62fd-117">En el paso siguiente, cree un nuevo control de usuario (archivo`.ascx`), indicado por la siguiente declaración en la primera línea:</span><span class="sxs-lookup"><span data-stu-id="d62fd-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="d62fd-118">Se usará un &lt;`label`elemento &gt; para mostrar los datos procedentes del servidor.</span><span class="sxs-lookup"><span data-stu-id="d62fd-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="d62fd-119">Además, en el archivo de control de usuario, usaremos tres botones de radio, cada uno de los cuales representa uno de los tres formatos de fecha posibles admitidos por el servicio Web.</span><span class="sxs-lookup"><span data-stu-id="d62fd-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="d62fd-120">Cuando el usuario hace clic en uno de los botones de radio, el explorador ejecutará código JavaScript que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="d62fd-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="d62fd-121">Este código tiene acceso a la `DynamicPopulateExtender` (no se preocupe todavía por el identificador extraño, esto se tratará más adelante) y desencadenará el rellenado dinámico con datos.</span><span class="sxs-lookup"><span data-stu-id="d62fd-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="d62fd-122">En el contexto del botón de radio actual, `this.value` hace referencia a su valor que es `format1`, `format2` o `format3` exactamente lo que espera el método Web.</span><span class="sxs-lookup"><span data-stu-id="d62fd-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="d62fd-123">Lo único que falta en el control de usuario es el `DynamicPopulateExtender` control que vincula los botones de radio al servicio Web.</span><span class="sxs-lookup"><span data-stu-id="d62fd-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="d62fd-124">También puede anotar el identificador extraño usado en el control: `mcd1$myDate` en lugar de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="d62fd-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="d62fd-125">Anteriormente, el código de JavaScript usaba `mcd1_dpe1` para tener acceso al `DynamicPopulateExtender` en lugar de `dpe1`. Esta estrategia de nomenclatura es un requisito especial al usar `DynamicPopulateExtender` dentro de un control de usuario.</span><span class="sxs-lookup"><span data-stu-id="d62fd-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="d62fd-126">Además, tiene que incrustar el control de usuario de una manera específica para que todo funcione.</span><span class="sxs-lookup"><span data-stu-id="d62fd-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="d62fd-127">Cree una nueva página de ASP.NET y registre un prefijo de etiqueta para el control de usuario que acaba de implementar:</span><span class="sxs-lookup"><span data-stu-id="d62fd-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="d62fd-128">Después, incluya el control ASP.NET de `ScriptManager` AJAX en la página nueva:</span><span class="sxs-lookup"><span data-stu-id="d62fd-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="d62fd-129">Por último, agregue el control de usuario a la página.</span><span class="sxs-lookup"><span data-stu-id="d62fd-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="d62fd-130">Solo tiene que establecer su `ID` atributo (y `runat="server"`, por supuesto), pero también tiene que establecerlo en un nombre específico: `mcd1` ya que este es el prefijo que se usa en el control de usuario para tener acceso a él mediante JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d62fd-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="d62fd-131">Y listo.</span><span class="sxs-lookup"><span data-stu-id="d62fd-131">And that's it!</span></span> <span data-ttu-id="d62fd-132">La página se comporta de la manera esperada: cuando un usuario hace clic en uno de los botones de radio, el control del kit de herramientas llama al servicio Web y muestra la fecha actual en el formato deseado.</span><span class="sxs-lookup"><span data-stu-id="d62fd-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="d62fd-133">[![de los botones de radio de un control de usuario](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d62fd-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="d62fd-134">Los botones de radio residen en un control de usuario ([haga clic para ver la imagen de tamaño completo](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d62fd-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d62fd-135">[Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Siguiente](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d62fd-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
