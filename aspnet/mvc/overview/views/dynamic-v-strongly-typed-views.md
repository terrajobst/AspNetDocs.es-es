---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vistas dinámicas frente a las Vistas fuertemente tipadas | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455686"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="c2496-103">Vistas dinámicas frente a las</span><span class="sxs-lookup"><span data-stu-id="c2496-103">Dynamic v.</span></span> <span data-ttu-id="c2496-104">vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="c2496-104">Strongly Typed Views</span></span>

<span data-ttu-id="c2496-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2496-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2496-106">Hay tres maneras de pasar información de un controlador a una vista en ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="c2496-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="c2496-107">Como un objeto de modelo fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="c2496-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="c2496-108">Como tipo dinámico (mediante @model dinámico)</span><span class="sxs-lookup"><span data-stu-id="c2496-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="c2496-109">Usar ViewBag</span><span class="sxs-lookup"><span data-stu-id="c2496-109">Using the ViewBag</span></span>

<span data-ttu-id="c2496-110">He escrito una aplicación sencilla de blog de MVC 3 para comparar y contrastar las vistas dinámicas y fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="c2496-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="c2496-111">El controlador empieza con una sencilla lista de blogs:</span><span class="sxs-lookup"><span data-stu-id="c2496-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="c2496-112">Haga clic con el botón derecho en el método IndexNotStonglyTyped () y agregue una vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="c2496-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="c2496-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2496-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="c2496-114">Asegúrese de que la casilla **crear una vista fuertemente tipada** no esté activada.</span><span class="sxs-lookup"><span data-stu-id="c2496-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="c2496-115">La vista resultante no contiene muchos:</span><span class="sxs-lookup"><span data-stu-id="c2496-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="c2496-116">Como estamos usando una vista dinámica y no fuertemente tipada, IntelliSense no nos ayuda.</span><span class="sxs-lookup"><span data-stu-id="c2496-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="c2496-117">A continuación se muestra el código completado:</span><span class="sxs-lookup"><span data-stu-id="c2496-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="c2496-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c2496-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="c2496-119">Ahora vamos a agregar una vista fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="c2496-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="c2496-120">Agregue el código siguiente al controlador:</span><span class="sxs-lookup"><span data-stu-id="c2496-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="c2496-121">Observe que es exactamente la misma vista de devolución (blog). Llame a como la vista sin establecimiento inflexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="c2496-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="c2496-122">Haga clic con el botón derecho dentro de *StonglyTypedIndex ()* y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="c2496-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="c2496-123">Esta vez, seleccione la clase de modelo de **blog** y seleccione **lista** como plantilla scaffolding.</span><span class="sxs-lookup"><span data-stu-id="c2496-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="c2496-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c2496-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="c2496-125">Dentro de la nueva plantilla de vista obtenemos compatibilidad con IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c2496-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="c2496-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c2496-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="c2496-127">El proyecto de c# se puede descargar [aquí](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="c2496-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
