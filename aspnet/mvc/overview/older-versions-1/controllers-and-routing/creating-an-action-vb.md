---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Crear una acción (VB) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar una nueva acción a un controlador MVC de ASP.NET. Obtenga información sobre los requisitos para que un método sea una acción.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: b1b53bea899deecef203551b23c087944e3990ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470161"
---
# <a name="creating-an-action-vb"></a><span data-ttu-id="6edfe-104">Crear una acción (VB)</span><span class="sxs-lookup"><span data-stu-id="6edfe-104">Creating an Action (VB)</span></span>

<span data-ttu-id="6edfe-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="6edfe-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="6edfe-106">Obtenga información sobre cómo agregar una nueva acción a un controlador MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6edfe-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="6edfe-107">Obtenga información sobre los requisitos para que un método sea una acción.</span><span class="sxs-lookup"><span data-stu-id="6edfe-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="6edfe-108">El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="6edfe-109">Conocerá los requisitos de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="6edfe-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="6edfe-110">También aprenderá a evitar que un método se exponga como una acción.</span><span class="sxs-lookup"><span data-stu-id="6edfe-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="6edfe-111">Agregar una acción a un controlador</span><span class="sxs-lookup"><span data-stu-id="6edfe-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="6edfe-112">Agregue una nueva acción a un controlador agregando un nuevo método al controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="6edfe-113">Por ejemplo, el controlador de la lista 1 contiene una acción denominada index () y una acción denominada SayHello ().</span><span class="sxs-lookup"><span data-stu-id="6edfe-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="6edfe-114">Ambos métodos se exponen como acciones.</span><span class="sxs-lookup"><span data-stu-id="6edfe-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="6edfe-115">**Lista 1-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="6edfe-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="6edfe-116">Para que se exponga al universo como una acción, un método debe cumplir ciertos requisitos:</span><span class="sxs-lookup"><span data-stu-id="6edfe-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="6edfe-117">El método debe ser público.</span><span class="sxs-lookup"><span data-stu-id="6edfe-117">The method must be public.</span></span>
- <span data-ttu-id="6edfe-118">El método no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="6edfe-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="6edfe-119">El método no puede ser un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="6edfe-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="6edfe-120">El método no puede ser un constructor, un captador o un establecedor.</span><span class="sxs-lookup"><span data-stu-id="6edfe-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="6edfe-121">El método no puede tener tipos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="6edfe-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="6edfe-122">El método no es un método de la clase base del controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="6edfe-123">El método no puede contener parámetros **ref** ni **out** .</span><span class="sxs-lookup"><span data-stu-id="6edfe-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="6edfe-124">Observe que no hay restricciones en el tipo de valor devuelto de una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="6edfe-125">Una acción de controlador puede devolver una cadena, un valor DateTime, una instancia de la clase Random o void.</span><span class="sxs-lookup"><span data-stu-id="6edfe-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="6edfe-126">El marco de MVC de ASP.NET convertirá cualquier tipo de valor devuelto que no sea un resultado de acción en una cadena y represente la cadena en el explorador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="6edfe-127">Al agregar cualquier método que no infrinja estos requisitos en un controlador, el método se expone como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="6edfe-128">Tenga cuidado aquí.</span><span class="sxs-lookup"><span data-stu-id="6edfe-128">Be careful here.</span></span> <span data-ttu-id="6edfe-129">Cualquier persona conectada a Internet puede invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="6edfe-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="6edfe-130">Por ejemplo, no cree una acción del controlador DeleteMyWebsite ().</span><span class="sxs-lookup"><span data-stu-id="6edfe-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="6edfe-131">Impedir que se invoque un método público</span><span class="sxs-lookup"><span data-stu-id="6edfe-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="6edfe-132">Si tiene que crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, puede evitar que se invoque el método mediante el &lt;atributo de&gt; que no es de acción.</span><span class="sxs-lookup"><span data-stu-id="6edfe-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="6edfe-133">Por ejemplo, el controlador de la lista 2 contiene un método público denominado CompanySecrets () que se decora con el &lt;atributo de&gt; no Action.</span><span class="sxs-lookup"><span data-stu-id="6edfe-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="6edfe-134">**Lista 2-Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="6edfe-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="6edfe-135">Si intenta invocar la acción del controlador CompanySecrets () escribiendo/Work/CompanySecrets en la barra de direcciones del explorador, obtendrá el mensaje de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="6edfe-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="6edfe-136">[![invocar un método que no es de acción](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6edfe-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="6edfe-137">**Figura 01**: invocación de un método de no acción ([haga clic para ver la imagen de tamaño completo](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="6edfe-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6edfe-138">[Anterior](creating-a-controller-vb.md)
> [Siguiente](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6edfe-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
