---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Información general sobre el enrutamientoC#de MVC de ASP.net () | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo el marco de ASP.NET MVC asigna las solicitudes del explorador a las acciones de controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437707"
---
# <a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="99c2b-103">Información general sobre el enrutamiento de ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="99c2b-103">ASP.NET MVC Routing Overview (C#)</span></span>

<span data-ttu-id="99c2b-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="99c2b-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="99c2b-105">En este tutorial, Stephen Walther muestra cómo el marco de ASP.NET MVC asigna las solicitudes del explorador a las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="99c2b-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="99c2b-106">En este tutorial, se presenta una característica importante de cada aplicación de ASP.NET MVC denominada *enrutamiento de ASP.net*.</span><span class="sxs-lookup"><span data-stu-id="99c2b-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="99c2b-107">El módulo de enrutamiento ASP.NET es responsable de asignar las solicitudes entrantes del explorador a determinadas acciones del controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="99c2b-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="99c2b-108">Al final de este tutorial, comprenderá cómo la tabla de rutas estándar asigna las solicitudes a las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="99c2b-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="99c2b-109">Uso de la tabla de rutas predeterminada</span><span class="sxs-lookup"><span data-stu-id="99c2b-109">Using the Default Route Table</span></span>

<span data-ttu-id="99c2b-110">Al crear una nueva aplicación ASP.NET MVC, la aplicación ya está configurada para usar el enrutamiento ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99c2b-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="99c2b-111">El enrutamiento de ASP.NET se configura en dos lugares.</span><span class="sxs-lookup"><span data-stu-id="99c2b-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="99c2b-112">En primer lugar, se habilita el enrutamiento ASP.NET en el archivo de configuración Web de la aplicación (archivo Web. config).</span><span class="sxs-lookup"><span data-stu-id="99c2b-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="99c2b-113">Hay cuatro secciones en el archivo de configuración que son relevantes para el enrutamiento: la sección System. Web. httpModules, la sección System. Web. httpHandlers, la sección System. WebServer. modules y la sección System. WebServer. Handlers.</span><span class="sxs-lookup"><span data-stu-id="99c2b-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="99c2b-114">Tenga cuidado de no eliminar estas secciones porque sin estas secciones el enrutamiento dejará de funcionar.</span><span class="sxs-lookup"><span data-stu-id="99c2b-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="99c2b-115">En segundo lugar, y lo que es más importante, se crea una tabla de rutas en el archivo global. asax de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="99c2b-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="99c2b-116">El archivo global. asax es un archivo especial que contiene controladores de eventos para los eventos de ciclo de vida de la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99c2b-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="99c2b-117">La tabla de rutas se crea durante el evento de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="99c2b-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="99c2b-118">El archivo de la lista 1 contiene el archivo global. asax predeterminado para una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="99c2b-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="99c2b-119">**Lista 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="99c2b-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="99c2b-120">Cuando se inicia por primera vez una aplicación MVC, se llama al método Start () de la aplicación\_.</span><span class="sxs-lookup"><span data-stu-id="99c2b-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="99c2b-121">Este método, a su vez, llama al método RegisterRoutes ().</span><span class="sxs-lookup"><span data-stu-id="99c2b-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="99c2b-122">El método RegisterRoutes () crea la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="99c2b-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="99c2b-123">La tabla de rutas predeterminada contiene una única ruta (denominada predeterminada).</span><span class="sxs-lookup"><span data-stu-id="99c2b-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="99c2b-124">La ruta predeterminada asigna el primer segmento de una dirección URL a un nombre de controlador, el segundo segmento de una dirección URL a una acción de controlador y el tercer segmento a un parámetro denominado **ID**.</span><span class="sxs-lookup"><span data-stu-id="99c2b-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="99c2b-125">Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador Web:</span><span class="sxs-lookup"><span data-stu-id="99c2b-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="99c2b-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="99c2b-126">/Home/Index/3</span></span>

<span data-ttu-id="99c2b-127">La ruta predeterminada asigna esta dirección URL a los parámetros siguientes:</span><span class="sxs-lookup"><span data-stu-id="99c2b-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="99c2b-128">controlador = Inicio</span><span class="sxs-lookup"><span data-stu-id="99c2b-128">controller = Home</span></span>

- <span data-ttu-id="99c2b-129">Action = índice</span><span class="sxs-lookup"><span data-stu-id="99c2b-129">action = Index</span></span>

- <span data-ttu-id="99c2b-130">ID. = 3</span><span class="sxs-lookup"><span data-stu-id="99c2b-130">id = 3</span></span>

<span data-ttu-id="99c2b-131">Al solicitar la dirección URL/Home/Index/3, se ejecuta el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="99c2b-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="99c2b-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="99c2b-132">HomeController.Index(3)</span></span>

<span data-ttu-id="99c2b-133">La ruta predeterminada incluye los valores predeterminados para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="99c2b-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="99c2b-134">Si no proporciona un controlador, el parámetro Controller tiene como valor predeterminado el valor **Home**.</span><span class="sxs-lookup"><span data-stu-id="99c2b-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="99c2b-135">Si no proporciona una acción, el parámetro de acción tiene como valor predeterminado el valor de **Índice**.</span><span class="sxs-lookup"><span data-stu-id="99c2b-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="99c2b-136">Por último, si no proporciona un identificador, el parámetro ID tiene como valor predeterminado una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="99c2b-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="99c2b-137">Echemos un vistazo a algunos ejemplos de cómo la ruta predeterminada asigna las direcciones URL a las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="99c2b-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="99c2b-138">Imagine que escribe la siguiente dirección URL en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="99c2b-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="99c2b-139">/Home</span><span class="sxs-lookup"><span data-stu-id="99c2b-139">/Home</span></span>

<span data-ttu-id="99c2b-140">Debido a los valores predeterminados de los parámetros de ruta predeterminados, al escribir esta dirección URL se llamará al método index () de la clase HomeController de la lista 2.</span><span class="sxs-lookup"><span data-stu-id="99c2b-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="99c2b-141">**Lista 2-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="99c2b-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="99c2b-142">En la lista 2, la clase HomeController incluye un método denominado index () que acepta un único parámetro denominado ID. La dirección URL/Home hace que se llame al método index () con una cadena vacía como valor del parámetro id.</span><span class="sxs-lookup"><span data-stu-id="99c2b-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="99c2b-143">Debido a la manera en que el marco de MVC invoca las acciones del controlador, la dirección URL/Home también coincide con el método index () de la clase HomeController de la lista 3.</span><span class="sxs-lookup"><span data-stu-id="99c2b-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="99c2b-144">**Lista 3-HomeController.cs (acción de índice sin parámetro)**</span><span class="sxs-lookup"><span data-stu-id="99c2b-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="99c2b-145">El método index () de la lista 3 no acepta ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="99c2b-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="99c2b-146">La dirección URL/Home hará que se llame a este método index ().</span><span class="sxs-lookup"><span data-stu-id="99c2b-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="99c2b-147">La dirección URL/Home/Index/3 también invoca este método (se omite el identificador).</span><span class="sxs-lookup"><span data-stu-id="99c2b-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="99c2b-148">La dirección URL/Home también coincide con el método index () de la clase HomeController en la lista 4.</span><span class="sxs-lookup"><span data-stu-id="99c2b-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="99c2b-149">**Lista 4-HomeController.cs (acción de índice con parámetro que acepta valores NULL)**</span><span class="sxs-lookup"><span data-stu-id="99c2b-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="99c2b-150">En la lista 4, el método index () tiene un parámetro entero.</span><span class="sxs-lookup"><span data-stu-id="99c2b-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="99c2b-151">Dado que el parámetro es un parámetro que acepta valores NULL (puede tener el valor null), se puede llamar al índice () sin generar un error.</span><span class="sxs-lookup"><span data-stu-id="99c2b-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="99c2b-152">Por último, al invocar el método index () en la lista 5 con la URL/Home se produce una excepción, ya que el parámetro id *no es* un parámetro que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="99c2b-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="99c2b-153">Si intenta invocar el método index (), obtendrá el error que se muestra en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="99c2b-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="99c2b-154">**Lista 5-HomeController.cs (acción de índice con el parámetro id)**</span><span class="sxs-lookup"><span data-stu-id="99c2b-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

<span data-ttu-id="99c2b-155">[![invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="99c2b-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="99c2b-156">**Figura 01**: invocación de una acción de controlador que espera un valor de parámetro ([haga clic para ver la imagen de tamaño completo](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="99c2b-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="99c2b-157">La dirección URL/Home/Index/3, por otro lado, funciona perfectamente con la acción del controlador de índice en la lista 5.</span><span class="sxs-lookup"><span data-stu-id="99c2b-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="99c2b-158">La solicitud/Home/Index/3 hace que se llame al método index () con un parámetro de identificador que tiene el valor 3.</span><span class="sxs-lookup"><span data-stu-id="99c2b-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="99c2b-159">Resumen</span><span class="sxs-lookup"><span data-stu-id="99c2b-159">Summary</span></span>

<span data-ttu-id="99c2b-160">El objetivo de este tutorial era proporcionarle una breve introducción al enrutamiento de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="99c2b-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="99c2b-161">Hemos examinado la tabla de rutas predeterminada que obtiene con una nueva aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="99c2b-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="99c2b-162">Ha aprendido cómo la ruta predeterminada asigna las direcciones URL a las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="99c2b-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="99c2b-163">Siguiente</span><span class="sxs-lookup"><span data-stu-id="99c2b-163">Next</span></span>](understanding-action-filters-cs.md)
