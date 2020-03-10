---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Enrutamiento en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 85862c094cc54365267b1f21e68d235a15519cda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449251"
---
# <a name="routing-in-aspnet-web-api"></a><span data-ttu-id="ae69c-102">Enrutar en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ae69c-102">Routing in ASP.NET Web API</span></span>

<span data-ttu-id="ae69c-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ae69c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ae69c-104">En este artículo se describe cómo ASP.NET Web API enruta las solicitudes HTTP a los controladores.</span><span class="sxs-lookup"><span data-stu-id="ae69c-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="ae69c-105">Si está familiarizado con ASP.NET MVC, el enrutamiento de API Web es muy similar al enrutamiento de MVC.</span><span class="sxs-lookup"><span data-stu-id="ae69c-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="ae69c-106">La principal diferencia es que la API Web usa el verbo HTTP, no la ruta de acceso del URI, para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="ae69c-106">The main difference is that Web API uses the HTTP verb, not the URI path, to select the action.</span></span> <span data-ttu-id="ae69c-107">También puede usar el enrutamiento de estilo MVC en Web API.</span><span class="sxs-lookup"><span data-stu-id="ae69c-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="ae69c-108">En este artículo no se asume ningún conocimiento de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ae69c-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>

## <a name="routing-tables"></a><span data-ttu-id="ae69c-109">Tablas de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="ae69c-109">Routing Tables</span></span>

<span data-ttu-id="ae69c-110">En ASP.NET Web API, un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae69c-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="ae69c-111">Los métodos públicos del controlador se denominan *métodos de acción* o simplemente *acciones*.</span><span class="sxs-lookup"><span data-stu-id="ae69c-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="ae69c-112">Cuando el marco de la API Web recibe una solicitud, enruta la solicitud a una acción.</span><span class="sxs-lookup"><span data-stu-id="ae69c-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="ae69c-113">Para determinar la acción que se va a invocar, el marco de trabajo utiliza una *tabla de enrutamiento*.</span><span class="sxs-lookup"><span data-stu-id="ae69c-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="ae69c-114">La plantilla de proyecto de Visual Studio para Web API crea una ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="ae69c-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="ae69c-115">Esta ruta se define en el archivo *WebApiConfig.CS* , que se coloca en el directorio de inicio de la *aplicación\_* :</span><span class="sxs-lookup"><span data-stu-id="ae69c-115">This route is defined in the *WebApiConfig.cs* file, which is placed in the *App\_Start* directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="ae69c-116">Para obtener más información sobre la clase `WebApiConfig`, vea [configurar ASP.net web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ae69c-116">For more information about the `WebApiConfig` class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="ae69c-117">Si Self-host Web API, debe establecer la tabla de enrutamiento directamente en el objeto `HttpSelfHostConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="ae69c-117">If you self-host Web API, you must set the routing table directly on the `HttpSelfHostConfiguration` object.</span></span> <span data-ttu-id="ae69c-118">Para obtener más información, consulte [Autohospedar una API Web](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ae69c-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="ae69c-119">Cada entrada de la tabla de enrutamiento contiene una *plantilla de ruta*.</span><span class="sxs-lookup"><span data-stu-id="ae69c-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="ae69c-120">La plantilla de ruta predeterminada para Web API es &quot;API/{Controller}/{ID}&quot;.</span><span class="sxs-lookup"><span data-stu-id="ae69c-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="ae69c-121">En esta plantilla, &quot;API&quot; es un segmento de ruta de acceso literal, y {Controller} y {ID} son variables de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="ae69c-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="ae69c-122">Cuando el marco de la API Web recibe una solicitud HTTP, intenta hacer coincidir el URI con una de las plantillas de ruta de la tabla de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="ae69c-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="ae69c-123">Si no coincide ninguna ruta, el cliente recibe un error 404.</span><span class="sxs-lookup"><span data-stu-id="ae69c-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="ae69c-124">Por ejemplo, los siguientes URI coinciden con la ruta predeterminada:</span><span class="sxs-lookup"><span data-stu-id="ae69c-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="ae69c-125">/api/contacts</span><span class="sxs-lookup"><span data-stu-id="ae69c-125">/api/contacts</span></span>
- <span data-ttu-id="ae69c-126">/api/contacts/1</span><span class="sxs-lookup"><span data-stu-id="ae69c-126">/api/contacts/1</span></span>
- <span data-ttu-id="ae69c-127">/api/products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="ae69c-127">/api/products/gizmo1</span></span>

<span data-ttu-id="ae69c-128">Sin embargo, el URI siguiente no coincide porque carece del segmento&quot; de &quot;API:</span><span class="sxs-lookup"><span data-stu-id="ae69c-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="ae69c-129">/contacts/1</span><span class="sxs-lookup"><span data-stu-id="ae69c-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="ae69c-130">La razón para usar "API" en la ruta es evitar colisiones con el enrutamiento de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ae69c-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="ae69c-131">De este modo, puede tener &quot;/Contacts&quot; ir a un controlador MVC y &quot;/API/Contacts&quot; ir a un controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="ae69c-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="ae69c-132">Por supuesto, si no le gusta esta Convención, puede cambiar la tabla de ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ae69c-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="ae69c-133">Una vez que se encuentra una ruta coincidente, la API Web selecciona el controlador y la acción:</span><span class="sxs-lookup"><span data-stu-id="ae69c-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="ae69c-134">Para buscar el controlador, la API Web agrega &quot;controlador&quot; al valor de la variable *{Controller}* .</span><span class="sxs-lookup"><span data-stu-id="ae69c-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="ae69c-135">Para encontrar la acción, la API Web examina el verbo HTTP y busca una acción cuyo nombre comienza con ese nombre de verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae69c-135">To find the action, Web API looks at the HTTP verb, and then looks for an action whose name begins with that HTTP verb name.</span></span> <span data-ttu-id="ae69c-136">Por ejemplo, con una solicitud GET, la API Web busca una acción con el prefijo &quot;Get&quot;, como &quot;GetContact&quot; o &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="ae69c-136">For example, with a GET request, Web API looks for an action prefixed with &quot;Get&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="ae69c-137">Esta Convención solo se aplica a los verbos GET, POST, PUT, DELETE, HEAD, OPTIONs y PATCH.</span><span class="sxs-lookup"><span data-stu-id="ae69c-137">This convention applies only to GET, POST, PUT, DELETE, HEAD, OPTIONS, and PATCH verbs.</span></span> <span data-ttu-id="ae69c-138">Puede habilitar otros verbos HTTP mediante el uso de atributos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ae69c-138">You can enable other HTTP verbs by using attributes on your controller.</span></span> <span data-ttu-id="ae69c-139">Veremos un ejemplo más adelante.</span><span class="sxs-lookup"><span data-stu-id="ae69c-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="ae69c-140">Otras variables de marcador de posición de la plantilla de ruta, como *{ID},* se asignan a los parámetros de acción.</span><span class="sxs-lookup"><span data-stu-id="ae69c-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="ae69c-141">Veamos un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ae69c-141">Let's look at an example.</span></span> <span data-ttu-id="ae69c-142">Supongamos que define el siguiente controlador:</span><span class="sxs-lookup"><span data-stu-id="ae69c-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="ae69c-143">A continuación se indican algunas posibles solicitudes HTTP, junto con la acción que se invoca para cada una:</span><span class="sxs-lookup"><span data-stu-id="ae69c-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="ae69c-144">Verbo HTTP</span><span class="sxs-lookup"><span data-stu-id="ae69c-144">HTTP Verb</span></span> | <span data-ttu-id="ae69c-145">Ruta de acceso de URI</span><span class="sxs-lookup"><span data-stu-id="ae69c-145">URI Path</span></span> | <span data-ttu-id="ae69c-146">Acción</span><span class="sxs-lookup"><span data-stu-id="ae69c-146">Action</span></span> | <span data-ttu-id="ae69c-147">Parámetro</span><span class="sxs-lookup"><span data-stu-id="ae69c-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ae69c-148">GET</span><span class="sxs-lookup"><span data-stu-id="ae69c-148">GET</span></span> | <span data-ttu-id="ae69c-149">API/productos</span><span class="sxs-lookup"><span data-stu-id="ae69c-149">api/products</span></span> | <span data-ttu-id="ae69c-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="ae69c-150">GetAllProducts</span></span> | <span data-ttu-id="ae69c-151">*ninguna*</span><span class="sxs-lookup"><span data-stu-id="ae69c-151">*(none)*</span></span> |
| <span data-ttu-id="ae69c-152">GET</span><span class="sxs-lookup"><span data-stu-id="ae69c-152">GET</span></span> | <span data-ttu-id="ae69c-153">API/productos/4</span><span class="sxs-lookup"><span data-stu-id="ae69c-153">api/products/4</span></span> | <span data-ttu-id="ae69c-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="ae69c-154">GetProductById</span></span> | <span data-ttu-id="ae69c-155">4</span><span class="sxs-lookup"><span data-stu-id="ae69c-155">4</span></span> |
| <span data-ttu-id="ae69c-156">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="ae69c-156">DELETE</span></span> | <span data-ttu-id="ae69c-157">API/productos/4</span><span class="sxs-lookup"><span data-stu-id="ae69c-157">api/products/4</span></span> | <span data-ttu-id="ae69c-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="ae69c-158">DeleteProduct</span></span> | <span data-ttu-id="ae69c-159">4</span><span class="sxs-lookup"><span data-stu-id="ae69c-159">4</span></span> |
| <span data-ttu-id="ae69c-160">EXPONER</span><span class="sxs-lookup"><span data-stu-id="ae69c-160">POST</span></span> | <span data-ttu-id="ae69c-161">API/productos</span><span class="sxs-lookup"><span data-stu-id="ae69c-161">api/products</span></span> | <span data-ttu-id="ae69c-162">*(sin coincidencia)*</span><span class="sxs-lookup"><span data-stu-id="ae69c-162">*(no match)*</span></span> |  |

<span data-ttu-id="ae69c-163">Observe que el segmento *{ID}* del URI, si está presente, se asigna al parámetro *ID* de la acción.</span><span class="sxs-lookup"><span data-stu-id="ae69c-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="ae69c-164">En este ejemplo, el controlador define dos métodos GET, uno con un parámetro *ID* y otro sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="ae69c-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="ae69c-165">Además, tenga en cuenta que se producirá un error en la solicitud POST porque el controlador no define un método &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="ae69c-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="ae69c-166">Variaciones de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="ae69c-166">Routing Variations</span></span>

<span data-ttu-id="ae69c-167">En la sección anterior se describe el mecanismo de enrutamiento básico para ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="ae69c-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="ae69c-168">En esta sección se describen algunas variaciones.</span><span class="sxs-lookup"><span data-stu-id="ae69c-168">This section describes some variations.</span></span>

### <a name="http-verbs"></a><span data-ttu-id="ae69c-169">verbos HTTP</span><span class="sxs-lookup"><span data-stu-id="ae69c-169">HTTP verbs</span></span>

<span data-ttu-id="ae69c-170">En lugar de usar la Convención de nomenclatura para los verbos HTTP, se puede especificar explícitamente el verbo HTTP para una acción si se decora el método de acción con uno de los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="ae69c-170">Instead of using the naming convention for HTTP verbs, you can explicitly specify the HTTP verb for an action by decorating the action method with one of the following attributes:</span></span>

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

<span data-ttu-id="ae69c-171">En el ejemplo siguiente, el método `FindProduct` se asigna a las solicitudes GET:</span><span class="sxs-lookup"><span data-stu-id="ae69c-171">In the following example, the `FindProduct` method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="ae69c-172">Para permitir varios verbos HTTP para una acción, o para permitir verbos HTTP distintos de GET, PUT, POST, DELETE, HEAD, OPTIONs y PATCH, utilice el atributo `[AcceptVerbs]`, que toma una lista de verbos HTTP.</span><span class="sxs-lookup"><span data-stu-id="ae69c-172">To allow multiple HTTP verbs for an action, or to allow HTTP verbs other than GET, PUT, POST, DELETE, HEAD, OPTIONS, and PATCH, use the `[AcceptVerbs]` attribute, which takes a list of HTTP verbs.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="ae69c-173">Enrutamiento por nombre de acción</span><span class="sxs-lookup"><span data-stu-id="ae69c-173">Routing by Action Name</span></span>

<span data-ttu-id="ae69c-174">Con la plantilla de enrutamiento predeterminada, la API Web usa el verbo HTTP para seleccionar la acción.</span><span class="sxs-lookup"><span data-stu-id="ae69c-174">With the default routing template, Web API uses the HTTP verb to select the action.</span></span> <span data-ttu-id="ae69c-175">Sin embargo, también puede crear una ruta en la que el nombre de la acción se incluya en el URI:</span><span class="sxs-lookup"><span data-stu-id="ae69c-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="ae69c-176">En esta plantilla de ruta, el parámetro *{Action}* nombra el método de acción en el controlador.</span><span class="sxs-lookup"><span data-stu-id="ae69c-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="ae69c-177">Con este estilo de enrutamiento, use atributos para especificar los verbos HTTP permitidos.</span><span class="sxs-lookup"><span data-stu-id="ae69c-177">With this style of routing, use attributes to specify the allowed HTTP verbs.</span></span> <span data-ttu-id="ae69c-178">Por ejemplo, suponga que el controlador tiene el método siguiente:</span><span class="sxs-lookup"><span data-stu-id="ae69c-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="ae69c-179">En este caso, una solicitud GET de "API/products/details/1" se asignaría al método `Details`.</span><span class="sxs-lookup"><span data-stu-id="ae69c-179">In this case, a GET request for "api/products/details/1" would map to the `Details` method.</span></span> <span data-ttu-id="ae69c-180">Este estilo de enrutamiento es similar a ASP.NET MVC y puede ser adecuado para una API de estilo RPC.</span><span class="sxs-lookup"><span data-stu-id="ae69c-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="ae69c-181">Puede invalidar el nombre de la acción mediante el atributo `[ActionName]`.</span><span class="sxs-lookup"><span data-stu-id="ae69c-181">You can override the action name by using the `[ActionName]` attribute.</span></span> <span data-ttu-id="ae69c-182">En el ejemplo siguiente, hay dos acciones que se asignan a &quot;API/Products/thumbnail/*ID*. Uno admite GET y el otro admite POST:</span><span class="sxs-lookup"><span data-stu-id="ae69c-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="ae69c-183">No acciones</span><span class="sxs-lookup"><span data-stu-id="ae69c-183">Non-Actions</span></span>

<span data-ttu-id="ae69c-184">Para evitar que un método se invoque como una acción, use el atributo `[NonAction]`.</span><span class="sxs-lookup"><span data-stu-id="ae69c-184">To prevent a method from getting invoked as an action, use the `[NonAction]` attribute.</span></span> <span data-ttu-id="ae69c-185">Esto indica al marco de trabajo que el método no es una acción, aunque de otro modo coincidiría con las reglas de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="ae69c-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="ae69c-186">Información adicional</span><span class="sxs-lookup"><span data-stu-id="ae69c-186">Further Reading</span></span>

<span data-ttu-id="ae69c-187">En este tema se proporciona una vista de alto nivel del enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="ae69c-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="ae69c-188">Para obtener más información, consulte [enrutamiento y selección de acciones](routing-and-action-selection.md), que describe exactamente cómo el marco de trabajo coincide con un URI de una ruta, selecciona un controlador y, a continuación, selecciona la acción que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="ae69c-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
