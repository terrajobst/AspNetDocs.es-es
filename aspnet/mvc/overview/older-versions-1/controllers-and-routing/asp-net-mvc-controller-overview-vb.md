---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Información general sobre el controlador de MVC de ASP.NET (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther presenta los controladores de ASP.NET MVC. Aprenderá a crear nuevos controladores y a devolver distintos tipos de acciones...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f19e7dd7fc025de2e0c387db898d36623e790e6a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486925"
---
# <a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="e1065-104">Información general sobre el controlador de ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="e1065-104">ASP.NET MVC Controller Overview (VB)</span></span>

<span data-ttu-id="e1065-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e1065-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e1065-106">En este tutorial, Stephen Walther presenta los controladores de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1065-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="e1065-107">Aprenderá a crear nuevos controladores y a devolver distintos tipos de resultados de acciones.</span><span class="sxs-lookup"><span data-stu-id="e1065-107">You learn how to create new controllers and return different types of action results.</span></span>

<span data-ttu-id="e1065-108">En este tutorial se explora el tema de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acciones.</span><span class="sxs-lookup"><span data-stu-id="e1065-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="e1065-109">Después de completar este tutorial, comprenderá cómo se usan los controladores para controlar la manera en que un visitante interactúa con un sitio web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1065-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="e1065-110">Descripción de los controladores</span><span class="sxs-lookup"><span data-stu-id="e1065-110">Understanding Controllers</span></span>

<span data-ttu-id="e1065-111">Los controladores de MVC son responsables de responder a las solicitudes realizadas en un sitio web de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1065-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="e1065-112">Cada solicitud del explorador se asigna a un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="e1065-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="e1065-113">Por ejemplo, Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="e1065-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e1065-114">En este caso, se invoca un controlador denominado ProductController.</span><span class="sxs-lookup"><span data-stu-id="e1065-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="e1065-115">ProductController es responsable de generar la respuesta a la solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="e1065-116">Por ejemplo, el controlador podría devolver una vista determinada al explorador o el controlador podría redirigir al usuario a otro controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="e1065-117">La lista 1 contiene un controlador simple denominado ProductController.</span><span class="sxs-lookup"><span data-stu-id="e1065-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="e1065-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="e1065-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="e1065-119">Como puede ver en la lista 1, un controlador es simplemente una clase (un Visual Basic .NET o C# una clase).</span><span class="sxs-lookup"><span data-stu-id="e1065-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="e1065-120">Un controlador es una clase que se deriva de la clase base System. Web. Mvc. Controller.</span><span class="sxs-lookup"><span data-stu-id="e1065-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="e1065-121">Dado que un controlador hereda de esta clase base, un controlador hereda varios métodos útiles de forma gratuita (se describen estos métodos en un momento).</span><span class="sxs-lookup"><span data-stu-id="e1065-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="e1065-122">Descripción de las acciones de controlador</span><span class="sxs-lookup"><span data-stu-id="e1065-122">Understanding Controller Actions</span></span>

<span data-ttu-id="e1065-123">Un controlador expone las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-123">A controller exposes controller actions.</span></span> <span data-ttu-id="e1065-124">Una acción es un método en un controlador que se llama cuando se escribe una dirección URL determinada en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="e1065-125">Por ejemplo, Imagine que realiza una solicitud para la siguiente dirección URL:</span><span class="sxs-lookup"><span data-stu-id="e1065-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e1065-126">En este caso, se llama al método index () en la clase ProductController.</span><span class="sxs-lookup"><span data-stu-id="e1065-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="e1065-127">El método index () es un ejemplo de una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="e1065-128">Una acción de controlador debe ser un método público de una clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="e1065-129">De forma predeterminada, los métodos de Visual Basic.NET son métodos públicos.</span><span class="sxs-lookup"><span data-stu-id="e1065-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="e1065-130">Tenga en consideración que cualquier método público que agregue a una clase de controlador se expone automáticamente como una acción de controlador (debe tener cuidado con esto, ya que cualquier persona del universo puede invocar una acción de controlador simplemente escribiendo la dirección URL correcta en una barra de direcciones del explorador).</span><span class="sxs-lookup"><span data-stu-id="e1065-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="e1065-131">Hay algunos requisitos adicionales que debe cumplir una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="e1065-132">Un método utilizado como una acción de controlador no se puede sobrecargar.</span><span class="sxs-lookup"><span data-stu-id="e1065-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="e1065-133">Además, una acción de controlador no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="e1065-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="e1065-134">Aparte de eso, puede usar prácticamente cualquier método como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="e1065-135">Descripción de los resultados de la acción</span><span class="sxs-lookup"><span data-stu-id="e1065-135">Understanding Action Results</span></span>

<span data-ttu-id="e1065-136">Una acción de controlador devuelve algo llamado *resultado de acción*.</span><span class="sxs-lookup"><span data-stu-id="e1065-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="e1065-137">Un resultado de acción es lo que devuelve una acción del controlador en respuesta a una solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="e1065-138">El marco de MVC de ASP.NET admite varios tipos de resultados de acciones, entre los que se incluyen:</span><span class="sxs-lookup"><span data-stu-id="e1065-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="e1065-139">ViewResult: representa HTML y marcado.</span><span class="sxs-lookup"><span data-stu-id="e1065-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="e1065-140">EmptyResult: no representa ningún resultado.</span><span class="sxs-lookup"><span data-stu-id="e1065-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="e1065-141">RedirectResult: representa una redirección a una nueva dirección URL.</span><span class="sxs-lookup"><span data-stu-id="e1065-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="e1065-142">JsonResult: representa un resultado notación de objetos JavaScript que se puede usar en una aplicación AJAX.</span><span class="sxs-lookup"><span data-stu-id="e1065-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="e1065-143">JavaScriptResult: representa un script de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e1065-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="e1065-144">ContentResult: representa un resultado de texto.</span><span class="sxs-lookup"><span data-stu-id="e1065-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="e1065-145">FileContentResult: representa un archivo descargable (con el contenido binario).</span><span class="sxs-lookup"><span data-stu-id="e1065-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="e1065-146">FilePathResult: representa un archivo descargable (con una ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="e1065-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="e1065-147">FileStreamResult: representa un archivo descargable (con un flujo de archivo).</span><span class="sxs-lookup"><span data-stu-id="e1065-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="e1065-148">Todos estos resultados de acción heredan de la clase base ActionResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="e1065-149">En la mayoría de los casos, una acción de controlador devuelve un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="e1065-150">Por ejemplo, la acción del controlador de índice de la lista 2 devuelve un ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="e1065-151">**Lista 2-Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="e1065-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="e1065-152">Cuando una acción devuelve un ViewResult, se devuelve HTML al explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="e1065-153">El método index () de la lista 2 devuelve una vista denominada index al explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="e1065-154">Tenga en cuenta que la acción index () de la lista 2 no devuelve ViewResult ().</span><span class="sxs-lookup"><span data-stu-id="e1065-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="e1065-155">En su lugar, se llama al método View () de la clase base Controller.</span><span class="sxs-lookup"><span data-stu-id="e1065-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="e1065-156">Normalmente, no se devuelve directamente un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="e1065-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="e1065-157">En su lugar, se llama a uno de los métodos siguientes de la clase base del controlador:</span><span class="sxs-lookup"><span data-stu-id="e1065-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="e1065-158">View: devuelve el resultado de una acción de ViewResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="e1065-159">Redirect: devuelve el resultado de una acción de RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="e1065-160">RedirectToAction: devuelve el resultado de una acción RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="e1065-161">RedirectToRoute: devuelve el resultado de una acción RedirectToRouteResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="e1065-162">JSON: devuelve el resultado de una acción de JsonResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="e1065-163">JavaScriptResult: devuelve un JavaScriptResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="e1065-164">Contenido: devuelve el resultado de una acción de ContentResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="e1065-165">Archivo: devuelve FileContentResult, FilePathResult o FileStreamResult, dependiendo de los parámetros pasados al método.</span><span class="sxs-lookup"><span data-stu-id="e1065-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="e1065-166">Por lo tanto, si desea devolver una vista al explorador, llame al método View ().</span><span class="sxs-lookup"><span data-stu-id="e1065-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="e1065-167">Si desea redirigir al usuario de una acción de controlador a otra, llame al método RedirectToAction ().</span><span class="sxs-lookup"><span data-stu-id="e1065-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="e1065-168">Por ejemplo, la acción Details () de la lista 3 muestra una vista o redirige al usuario a la acción index (), dependiendo de si el parámetro ID tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="e1065-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="e1065-169">**Lista 3: CustomerController. VB**</span><span class="sxs-lookup"><span data-stu-id="e1065-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="e1065-170">El resultado de la acción ContentResult es especial.</span><span class="sxs-lookup"><span data-stu-id="e1065-170">The ContentResult action result is special.</span></span> <span data-ttu-id="e1065-171">Puede usar el resultado de la acción ContentResult para devolver el resultado de una acción como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="e1065-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="e1065-172">Por ejemplo, el método index () de la lista 4 devuelve un mensaje como texto sin formato y no como HTML.</span><span class="sxs-lookup"><span data-stu-id="e1065-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="e1065-173">**Lista 4: Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="e1065-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="e1065-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="e1065-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="e1065-175">System. Web. Mvc. Controller</span><span class="sxs-lookup"><span data-stu-id="e1065-175">System.Web.Mvc.Controller</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="e1065-176">Cuando se invoca la acción StatusController. index (), no se devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="e1065-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="e1065-177">En su lugar, el texto sin formato "Hola mundo!"</span><span class="sxs-lookup"><span data-stu-id="e1065-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="e1065-178">se devuelve al explorador.</span><span class="sxs-lookup"><span data-stu-id="e1065-178">is returned to the browser.</span></span>

<span data-ttu-id="e1065-179">Si una acción de controlador devuelve un resultado que no es un resultado de acción (por ejemplo, una fecha o un entero), el resultado se ajusta automáticamente en un ContentResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="e1065-180">Por ejemplo, cuando se invoca la acción index () de WorkController en la lista 5, la fecha se devuelve automáticamente como ContentResult.</span><span class="sxs-lookup"><span data-stu-id="e1065-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="e1065-181">**Lista 5-WorkController. VB**</span><span class="sxs-lookup"><span data-stu-id="e1065-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="e1065-182">La acción index () de la lista 5 devuelve un objeto DateTime.</span><span class="sxs-lookup"><span data-stu-id="e1065-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="e1065-183">El marco de MVC de ASP.NET convierte el objeto DateTime en una cadena y ajusta el valor DateTime en un ContentResult automáticamente.</span><span class="sxs-lookup"><span data-stu-id="e1065-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="e1065-184">El explorador recibe la fecha y la hora como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="e1065-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="e1065-185">Resumen</span><span class="sxs-lookup"><span data-stu-id="e1065-185">Summary</span></span>

<span data-ttu-id="e1065-186">El propósito de este tutorial fue presentar los conceptos de los controladores de ASP.NET MVC, las acciones de controlador y los resultados de acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="e1065-187">En la primera sección, ha aprendido a agregar nuevos controladores a un proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1065-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="e1065-188">A continuación, ha aprendido cómo los métodos públicos de un controlador se exponen al universo como acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="e1065-189">Por último, analizamos los diferentes tipos de resultados de acción que se pueden devolver desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="e1065-190">En concreto, hemos explicado cómo devolver ViewResult, RedirectToActionResult y ContentResult desde una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="e1065-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1065-191">[Anterior](creating-a-custom-route-constraint-cs.md)
> [Siguiente](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e1065-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
