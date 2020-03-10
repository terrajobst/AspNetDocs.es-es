---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Crear una restricción de ruta personalizada (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther muestra cómo puede crear una restricción de ruta personalizada. Implementamos una restricción personalizada simple que impide que una ruta coincida con...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486793"
---
# <a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="15b0a-104">Crear una restricción de ruta personalizada (VB)</span><span class="sxs-lookup"><span data-stu-id="15b0a-104">Creating a Custom Route Constraint (VB)</span></span>

<span data-ttu-id="15b0a-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="15b0a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="15b0a-106">Stephen Walther muestra cómo puede crear una restricción de ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="15b0a-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="15b0a-107">Implementamos una restricción personalizada simple que impide que se haga coincidir una ruta cuando se realiza una solicitud de explorador desde un equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="15b0a-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="15b0a-108">El objetivo de este tutorial es mostrar cómo se puede crear una restricción de ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="15b0a-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="15b0a-109">Una restricción de ruta personalizada permite evitar que una ruta coincida a menos que coincida alguna condición personalizada.</span><span class="sxs-lookup"><span data-stu-id="15b0a-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="15b0a-110">En este tutorial, se crea una restricción de ruta localhost.</span><span class="sxs-lookup"><span data-stu-id="15b0a-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="15b0a-111">La restricción de ruta localhost solo coincide con las solicitudes realizadas desde el equipo local.</span><span class="sxs-lookup"><span data-stu-id="15b0a-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="15b0a-112">No se buscan coincidencias con las solicitudes remotas desde Internet.</span><span class="sxs-lookup"><span data-stu-id="15b0a-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="15b0a-113">Implemente una restricción de ruta personalizada implementando la interfaz IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="15b0a-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="15b0a-114">Se trata de una interfaz muy sencilla que describe un método único:</span><span class="sxs-lookup"><span data-stu-id="15b0a-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="15b0a-115">El método devuelve un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="15b0a-115">The method returns a Boolean value.</span></span> <span data-ttu-id="15b0a-116">Si devuelve false, la ruta asociada a la restricción no coincidirá con la solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="15b0a-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="15b0a-117">La restricción localhost está incluida en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="15b0a-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="15b0a-118">**Lista 1-LocalhostConstraint. VB**</span><span class="sxs-lookup"><span data-stu-id="15b0a-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="15b0a-119">La restricción de la lista 1 aprovecha la propiedad IsLocal expuesta por la clase HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="15b0a-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="15b0a-120">Esta propiedad devuelve true cuando la dirección IP de la solicitud es 127.0.0.1 o cuando la IP de la solicitud es la misma que la dirección IP del servidor.</span><span class="sxs-lookup"><span data-stu-id="15b0a-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="15b0a-121">Use una restricción personalizada en una ruta definida en el archivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="15b0a-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="15b0a-122">El archivo global. asax de la lista 2 usa la restricción localhost para evitar que alguien solicite una página de administración a menos que realice la solicitud desde el servidor local.</span><span class="sxs-lookup"><span data-stu-id="15b0a-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="15b0a-123">Por ejemplo, se producirá un error en una solicitud de/Admin/DeleteAll cuando se realice desde un servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="15b0a-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="15b0a-124">**Lista 2: global. asax**</span><span class="sxs-lookup"><span data-stu-id="15b0a-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="15b0a-125">La restricción localhost se usa en la definición de la ruta de administración.</span><span class="sxs-lookup"><span data-stu-id="15b0a-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="15b0a-126">Esta ruta no coincidirá con una solicitud de explorador remoto.</span><span class="sxs-lookup"><span data-stu-id="15b0a-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="15b0a-127">No obstante, tenga en cuentan que otras rutas definidas en global. asax pueden coincidir con la misma solicitud.</span><span class="sxs-lookup"><span data-stu-id="15b0a-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="15b0a-128">Es importante comprender que una restricción impide que una ruta determinada coincida con una solicitud y no con todas las rutas definidas en el archivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="15b0a-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="15b0a-129">Tenga en cuenta que la ruta predeterminada se ha comentado del archivo global. asax en la lista 2.</span><span class="sxs-lookup"><span data-stu-id="15b0a-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="15b0a-130">Si incluye la ruta predeterminada, la ruta predeterminada coincidiría con las solicitudes del controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="15b0a-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="15b0a-131">En ese caso, los usuarios remotos pueden seguir invocando acciones del controlador de administración aunque sus solicitudes no coincidan con la ruta de administrador.</span><span class="sxs-lookup"><span data-stu-id="15b0a-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="15b0a-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="15b0a-132">Previous</span></span>](creating-a-route-constraint-vb.md)
