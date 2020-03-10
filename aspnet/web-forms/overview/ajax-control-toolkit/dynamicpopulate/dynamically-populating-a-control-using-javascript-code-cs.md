---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Rellenar dinámicamente un control mediante códigoC#JavaScript () | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430507"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="adb93-103">Rellenar dinámicamente un control mediante el código de JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="adb93-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>

<span data-ttu-id="adb93-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="adb93-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="adb93-105">[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="adb93-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="adb93-106">El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="adb93-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="adb93-107">También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="adb93-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="adb93-108">Información general</span><span class="sxs-lookup"><span data-stu-id="adb93-108">Overview</span></span>

<span data-ttu-id="adb93-109">El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="adb93-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="adb93-110">También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="adb93-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="adb93-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="adb93-111">Steps</span></span>

<span data-ttu-id="adb93-112">En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que llamará el control `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="adb93-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="adb93-113">El servicio Web implementa el método `getDate()` que espera un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web.</span><span class="sxs-lookup"><span data-stu-id="adb93-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="adb93-114">Este es el código (`DynamicPopulate.cs.asmx`de archivos), que recupera la fecha actual en uno de los tres formatos siguientes:</span><span class="sxs-lookup"><span data-stu-id="adb93-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="adb93-115">En el paso siguiente, cree un nuevo sitio de ASP.NET y comience con el control ScriptManager de AJAX de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="adb93-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="adb93-116">A continuación, agregue un control etiqueta (por ejemplo, mediante el control HTML del mismo nombre, o el control Web `<asp:Label />`) que posteriormente mostrará el resultado de la llamada al servicio Web.</span><span class="sxs-lookup"><span data-stu-id="adb93-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="adb93-117">A continuación, incluya un control de `DynamicPopulateExtender` y proporcione información del servicio Web, control de destino, pero no el nombre del control que desencadena el rellenado que se realizará más adelante, con JavaScript personalizado.</span><span class="sxs-lookup"><span data-stu-id="adb93-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="adb93-118">Ahora a la parte de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="adb93-118">Now to the JavaScript part.</span></span> <span data-ttu-id="adb93-119">La función `$find()`, definida por la biblioteca ASP.NET AJAX, devuelve una referencia a los objetos del lado servidor del kit de herramientas de control AJAX de ASP.NET, como `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="adb93-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="adb93-120">En el archivo actual, `$find("dpe")` devuelve una referencia al control `DynamicPopulateExtender` de la página.</span><span class="sxs-lookup"><span data-stu-id="adb93-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="adb93-121">Expone un método denominado `populate()` que desencadena el proceso de rellenado dinámico.</span><span class="sxs-lookup"><span data-stu-id="adb93-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="adb93-122">El método `populate()` requiere un argumento: la clave de contexto que actuará como argumento para el método Web `getDate()`.</span><span class="sxs-lookup"><span data-stu-id="adb93-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="adb93-123">Por ejemplo, `$find("dpe").populate("format1")` rellenaría la etiqueta con la fecha actual en formato de mes-día-año.</span><span class="sxs-lookup"><span data-stu-id="adb93-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="adb93-124">Para que el ejemplo sea un poco más flexible, el usuario puede elegir entre varios formatos de fecha.</span><span class="sxs-lookup"><span data-stu-id="adb93-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="adb93-125">Para cada uno de ellos, se muestra un botón de radio.</span><span class="sxs-lookup"><span data-stu-id="adb93-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="adb93-126">Una vez que el usuario hace clic en un botón de radio, el código JavaScript rellena dinámicamente la etiqueta con el formato de fecha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="adb93-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="adb93-127">Estos son los botones de radio siguientes:</span><span class="sxs-lookup"><span data-stu-id="adb93-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="adb93-128">Tenga en cuenta que, en el contexto de un botón de radio, la expresión de JavaScript `this.value` hace referencia al valor del botón actual, que es exactamente la misma información con la que puede trabajar el método `getDate()`.</span><span class="sxs-lookup"><span data-stu-id="adb93-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="adb93-129">[![un clic en el botón recupera la fecha del servidor, en el formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="adb93-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="adb93-130">Un clic en el botón recupera la fecha del servidor con el formato especificado ([haga clic para ver la imagen a tamaño completo](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="adb93-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="adb93-131">[Anterior](dynamically-populating-a-control-cs.md)
> [Siguiente](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="adb93-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
