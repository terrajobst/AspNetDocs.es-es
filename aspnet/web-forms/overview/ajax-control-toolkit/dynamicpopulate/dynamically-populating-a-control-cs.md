---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
title: Rellenar dinámicamente unC#control () | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e1fec43e-1daf-49d2-b0c7-7f1b930455cc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 24f88e44e0f878127314774d4e8846f80133413e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430561"
---
# <a name="dynamically-populating-a-control-c"></a><span data-ttu-id="7f446-103">Rellenar un control dinámicamente (C#)</span><span class="sxs-lookup"><span data-stu-id="7f446-103">Dynamically Populating a Control (C#)</span></span>

<span data-ttu-id="7f446-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7f446-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7f446-105">[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7f446-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0CS.pdf)</span></span>

> <span data-ttu-id="7f446-106">El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="7f446-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>

## <a name="overview"></a><span data-ttu-id="7f446-107">Información general</span><span class="sxs-lookup"><span data-stu-id="7f446-107">Overview</span></span>

<span data-ttu-id="7f446-108">El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="7f446-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="7f446-109">En este tutorial se muestra cómo configurarlo.</span><span class="sxs-lookup"><span data-stu-id="7f446-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="7f446-110">Pasos</span><span class="sxs-lookup"><span data-stu-id="7f446-110">Steps</span></span>

<span data-ttu-id="7f446-111">En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que va a llamar `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="7f446-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="7f446-112">La clase de servicio Web requiere el atributo `ScriptService` que se define en `Microsoft.Web.Script.Services`; de lo contrario, ASP.NET AJAX no puede crear el proxy de JavaScript del lado cliente para el servicio Web que, a su vez, es necesario para `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="7f446-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="7f446-113">El método Web debe esperar un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web.</span><span class="sxs-lookup"><span data-stu-id="7f446-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="7f446-114">El siguiente servicio Web devuelve la fecha actual en un formato representado por el argumento `contextKey`:</span><span class="sxs-lookup"><span data-stu-id="7f446-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7f446-115">El servicio Web se guarda entonces como `DynamicPopulate.cs.asmx`.</span><span class="sxs-lookup"><span data-stu-id="7f446-115">The web service is then saved as `DynamicPopulate.cs.asmx`.</span></span> <span data-ttu-id="7f446-116">Como alternativa, puede implementar el método `getDate()` como un método de página dentro de la página ASP.NET real con el control `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="7f446-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="7f446-117">En el paso siguiente, cree un nuevo archivo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f446-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="7f446-118">Como siempre, el primer paso consiste en incluir el `ScriptManager` en la página actual para cargar la biblioteca de AJAX de ASP.NET y para que el kit de herramientas de control funcione:</span><span class="sxs-lookup"><span data-stu-id="7f446-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7f446-119">A continuación, agregue un control etiqueta (por ejemplo, con el control HTML del mismo nombre, o la &lt;`asp:Label` /&gt; control Web) que posteriormente mostrará el resultado de la llamada al servicio Web.</span><span class="sxs-lookup"><span data-stu-id="7f446-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample3.aspx)]

<span data-ttu-id="7f446-120">Un botón HTML (como control HTML, ya que no se requiere un postback en el servidor) se usará para desencadenar el rellenado dinámico:</span><span class="sxs-lookup"><span data-stu-id="7f446-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="7f446-121">Por último, necesitamos el control `DynamicPopulateExtender` para conectar las cosas.</span><span class="sxs-lookup"><span data-stu-id="7f446-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="7f446-122">Se establecerán los siguientes atributos (además de los obvios, `ID` y `runat`=`"server"`):</span><span class="sxs-lookup"><span data-stu-id="7f446-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="7f446-123">`TargetControlID` dónde colocar el resultado de la llamada al servicio Web</span><span class="sxs-lookup"><span data-stu-id="7f446-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="7f446-124">`ServicePath` ruta de acceso al servicio Web (omita si desea utilizar un método de página)</span><span class="sxs-lookup"><span data-stu-id="7f446-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="7f446-125">`ServiceMethod` nombre del método de la página o método Web</span><span class="sxs-lookup"><span data-stu-id="7f446-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="7f446-126">`ContextKey` la información de contexto que se va a enviar al servicio Web</span><span class="sxs-lookup"><span data-stu-id="7f446-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="7f446-127">`PopulateTriggerControlID` elemento que desencadena la llamada de servicio Web</span><span class="sxs-lookup"><span data-stu-id="7f446-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="7f446-128">`ClearContentsDuringUpdate` si se va a vaciar el elemento de destino durante la llamada al servicio Web</span><span class="sxs-lookup"><span data-stu-id="7f446-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="7f446-129">Como puede ver, el control requiere cierta información pero poner todo en marcha es bastante sencillo.</span><span class="sxs-lookup"><span data-stu-id="7f446-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="7f446-130">Este es el marcado del control `DynamicPopulateExtender` en el escenario actual:</span><span class="sxs-lookup"><span data-stu-id="7f446-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="7f446-131">Ejecute la página ASP.NET en el explorador y haga clic en el botón. recibirá la fecha actual en formato de mes-día-año.</span><span class="sxs-lookup"><span data-stu-id="7f446-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>

<span data-ttu-id="7f446-132">[![un clic en el botón recupera la fecha del servidor](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7f446-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-cs/_static/image2.png)](dynamically-populating-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7f446-133">Un clic en el botón recupera la fecha del servidor ([haga clic para ver la imagen de tamaño completo](dynamically-populating-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7f446-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7f446-134">Siguiente</span><span class="sxs-lookup"><span data-stu-id="7f446-134">Next</span></span>](dynamically-populating-a-control-using-javascript-code-cs.md)
