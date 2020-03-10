---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Enrutamiento y selección de acciones en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446917"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="cfa3a-102">Enrutamiento y selección de acciones en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cfa3a-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="cfa3a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cfa3a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cfa3a-104">En este artículo se describe cómo ASP.NET Web API enruta una solicitud HTTP a una acción determinada en un controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="cfa3a-105">Para obtener información general de alto nivel sobre el enrutamiento, consulte [enrutamiento en ASP.net web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="cfa3a-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="cfa3a-106">En este artículo se examinan los detalles del proceso de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="cfa3a-107">Si crea un proyecto de API Web y observa que algunas solicitudes no se enrutan de la manera esperada, esperamos que este artículo le ayude.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="cfa3a-108">El enrutamiento tiene tres fases principales:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="cfa3a-109">Coincidencia del URI con una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="cfa3a-110">Seleccionar un controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-110">Selecting a controller.</span></span>
3. <span data-ttu-id="cfa3a-111">Seleccionar una acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-111">Selecting an action.</span></span>

<span data-ttu-id="cfa3a-112">Puede reemplazar algunas partes del proceso por sus propios comportamientos personalizados.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="cfa3a-113">En este artículo, se describe el comportamiento predeterminado.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="cfa3a-114">Al final, se indican los lugares en los que puede personalizar el comportamiento.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="cfa3a-115">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="cfa3a-115">Route Templates</span></span>

<span data-ttu-id="cfa3a-116">Una plantilla de ruta es similar a una ruta de acceso de URI, pero puede tener valores de marcador de posición, indicados con llaves:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="cfa3a-117">Al crear una ruta, puede proporcionar valores predeterminados para algunos o todos los marcadores de posición:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="cfa3a-118">También puede proporcionar restricciones, que restringen el modo en que un segmento URI puede coincidir con un marcador de posición:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="cfa3a-119">El marco de trabajo intenta hacer coincidir los segmentos de la ruta de acceso del URI con la plantilla.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="cfa3a-120">Los literales de la plantilla deben coincidir exactamente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="cfa3a-121">Un marcador de posición coincide con cualquier valor, a menos que se especifiquen restricciones.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="cfa3a-122">El marco de trabajo no coincide con otras partes del URI, como el nombre de host o los parámetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="cfa3a-123">El marco de trabajo selecciona la primera ruta de la tabla de rutas que coincide con el URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="cfa3a-124">Hay dos marcadores de posición especiales: "{Controller}" y "{Action}".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="cfa3a-125">"{Controller}" proporciona el nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="cfa3a-126">"{Action}" proporciona el nombre de la acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="cfa3a-127">En la API Web, la Convención habitual es omitir "{Action}".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="cfa3a-128">Valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="cfa3a-128">Defaults</span></span>

<span data-ttu-id="cfa3a-129">Si proporciona valores predeterminados, la ruta coincidirá con un URI al que les faltan esos segmentos.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="cfa3a-130">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="cfa3a-131">Los URI `http://localhost/api/products/all` y `http://localhost/api/products` coinciden con la ruta anterior.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="cfa3a-132">En el último URI, el segmento de `{category}` que falta tiene asignado el valor predeterminado `all`.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="cfa3a-133">Diccionario de ruta</span><span class="sxs-lookup"><span data-stu-id="cfa3a-133">Route Dictionary</span></span>

<span data-ttu-id="cfa3a-134">Si el marco de trabajo encuentra una coincidencia para un URI, crea un diccionario que contiene el valor de cada marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="cfa3a-135">Las claves son los nombres de marcador de posición, sin incluir las llaves.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="cfa3a-136">Los valores se toman de la ruta de acceso del URI o de los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="cfa3a-137">El diccionario se almacena en el objeto **IHttpRouteData** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="cfa3a-138">Durante esta fase de coincidencia de ruta, los marcadores de posición especiales "{Controller}" y "{Action}" se tratan igual que los demás marcadores de posición.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="cfa3a-139">Simplemente se almacenan en el diccionario con los demás valores.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="cfa3a-140">Un valor predeterminado puede tener el valor especial **RouteParameter. Optional**.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="cfa3a-141">Si se asigna este valor a un marcador de posición, el valor no se agrega al Diccionario de ruta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="cfa3a-142">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="cfa3a-143">Para la ruta de acceso de URI "API/Products", el Diccionario de ruta contendrá:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="cfa3a-144">controlador: "Products"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-144">controller: "products"</span></span>
- <span data-ttu-id="cfa3a-145">Categoría: "All"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-145">category: "all"</span></span>

<span data-ttu-id="cfa3a-146">En el caso de "API/Products/Toys/123", sin embargo, el Diccionario de ruta contendrá:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="cfa3a-147">controlador: "Products"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-147">controller: "products"</span></span>
- <span data-ttu-id="cfa3a-148">Categoría: "juguetes"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-148">category: "toys"</span></span>
- <span data-ttu-id="cfa3a-149">ID.: "123"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-149">id: "123"</span></span>

<span data-ttu-id="cfa3a-150">Los valores predeterminados también pueden incluir un valor que no aparece en ninguna parte de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="cfa3a-151">Si la ruta coincide, ese valor se almacena en el diccionario.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="cfa3a-152">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="cfa3a-153">Si la ruta de acceso del URI es "API/root/8", el Diccionario contendrá dos valores:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="cfa3a-154">controlador: "Customers"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-154">controller: "customers"</span></span>
- <span data-ttu-id="cfa3a-155">ID.: "8"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="cfa3a-156">Selección de un controlador</span><span class="sxs-lookup"><span data-stu-id="cfa3a-156">Selecting a Controller</span></span>

<span data-ttu-id="cfa3a-157">El método **IHttpControllerSelector. SelectController** controla la selección del controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="cfa3a-158">Este método toma una instancia de **HttpRequestMessage** y devuelve un **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="cfa3a-159">La implementación predeterminada se proporciona mediante la clase **DefaultHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="cfa3a-160">Esta clase usa un algoritmo sencillo:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="cfa3a-161">Busque la clave "Controller" en el Diccionario de ruta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="cfa3a-162">Tome el valor de esta clave y anexe la cadena "Controller" para obtener el nombre del tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="cfa3a-163">Busque un controlador de API Web con este nombre de tipo.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="cfa3a-164">Por ejemplo, si el Diccionario de rutas contiene el par clave-valor "Controller" = "Products", el tipo de controlador es "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="cfa3a-165">Si no hay ningún tipo coincidente, o si hay varias coincidencias, el marco de trabajo devuelve un error al cliente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="cfa3a-166">En el paso 3, **DefaultHttpControllerSelector** usa la interfaz **IHttpControllerTypeResolver** para obtener la lista de tipos de controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="cfa3a-167">La implementación predeterminada de **IHttpControllerTypeResolver** devuelve todas las clases públicas que (a) implementan **IHttpController**, (b) no son abstractas y (c) tienen un nombre que termina en "Controller".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="cfa3a-168">Selección de acción</span><span class="sxs-lookup"><span data-stu-id="cfa3a-168">Action Selection</span></span>

<span data-ttu-id="cfa3a-169">Después de seleccionar el controlador, el marco de trabajo selecciona la acción llamando al método **IHttpActionSelector. SelectAction** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="cfa3a-170">Este método toma un **HttpControllerContext** y devuelve un **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="cfa3a-171">La implementación predeterminada se proporciona mediante la clase **ApiControllerActionSelector** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="cfa3a-172">Para seleccionar una acción, tiene un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="cfa3a-173">Método HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="cfa3a-174">El marcador de posición "{Action}" en la plantilla de ruta, si está presente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="cfa3a-175">Parámetros de las acciones en el controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="cfa3a-176">Antes de examinar el algoritmo de selección, necesitamos comprender algunas cosas sobre las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="cfa3a-177">**¿Qué métodos del controlador se consideran "acciones"?**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="cfa3a-178">Al seleccionar una acción, el marco solo busca métodos de instancia públicos en el controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="cfa3a-179">Además, excluye los métodos ["Special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (constructores, eventos, sobrecargas de operador, etc.) y los métodos heredados de la clase **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="cfa3a-180">**Métodos HTTP.**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-180">**HTTP Methods.**</span></span> <span data-ttu-id="cfa3a-181">El marco de trabajo solo elige las acciones que coinciden con el método HTTP de la solicitud, que se determina de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="cfa3a-182">Puede especificar el método HTTP con un atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**o **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="cfa3a-183">De lo contrario, si el nombre del método de controlador comienza por "Get", "post", "put", "Delete", "Head", "Options" o "patch", entonces por Convención la acción admite ese método HTTP.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="cfa3a-184">Si ninguno de los anteriores, el método admite POST.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="cfa3a-185">**Enlaces de parámetros.**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-185">**Parameter Bindings.**</span></span> <span data-ttu-id="cfa3a-186">Un enlace de parámetros es cómo Web API crea un valor para un parámetro.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="cfa3a-187">Esta es la regla predeterminada para el enlace de parámetros:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="cfa3a-188">Los tipos simples se toman del URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="cfa3a-189">Los tipos complejos se toman del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="cfa3a-190">Entre los tipos simples se incluyen todos los [.NET Framework tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive), además de **DateTime**, **decimal**, **GUID**, **String**y **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="cfa3a-191">Para cada acción, a lo sumo un parámetro puede leer el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="cfa3a-192">Es posible invalidar las reglas de enlace predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="cfa3a-193">Consulte [enlace de parámetros de WebAPI en el capó](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="cfa3a-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="cfa3a-194">Con ese fondo, este es el algoritmo de selección de acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="cfa3a-195">Cree una lista de todas las acciones del controlador que coincidan con el método de solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="cfa3a-196">Si el Diccionario de rutas tiene una entrada de "acción", quite las acciones cuyo nombre no coincida con este valor.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="cfa3a-197">Intente hacer coincidir los parámetros de acción con el URI, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="cfa3a-198">Para cada acción, obtenga una lista de los parámetros que son de tipo simple, donde el enlace obtiene el parámetro del URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="cfa3a-199">Excluya los parámetros opcionales.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="cfa3a-200">En esta lista, intente encontrar una coincidencia para cada nombre de parámetro, ya sea en el Diccionario de ruta o en la cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="cfa3a-201">Las coincidencias no distinguen mayúsculas de minúsculas y no dependen del orden de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="cfa3a-202">Seleccione una acción en la que todos los parámetros de la lista tengan una coincidencia en el URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="cfa3a-203">Si hay más de una acción que cumpla estos criterios, seleccione la que tenga más coincidencias de parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="cfa3a-204">Omitir acciones con el atributo **[no Action]** .</span><span class="sxs-lookup"><span data-stu-id="cfa3a-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="cfa3a-205">El paso #3 es probablemente el más confuso.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="cfa3a-206">La idea básica es que un parámetro puede obtener su valor desde el URI, desde el cuerpo de la solicitud o desde un enlace personalizado.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="cfa3a-207">En el caso de los parámetros que proceden del URI, queremos asegurarnos de que el URI contiene realmente un valor para ese parámetro, ya sea en la ruta de acceso (a través del Diccionario de rutas) o en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="cfa3a-208">Por ejemplo, considere la siguiente acción:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="cfa3a-209">El parámetro *ID* se enlaza al URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="cfa3a-210">Por lo tanto, esta acción solo puede coincidir con un URI que contenga un valor para "ID", ya sea en el Diccionario de ruta o en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="cfa3a-211">Los parámetros opcionales son una excepción, porque son opcionales.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="cfa3a-212">Para un parámetro opcional, es correcto si el enlace no puede obtener el valor del URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="cfa3a-213">Los tipos complejos son una excepción por una razón diferente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="cfa3a-214">Un tipo complejo solo puede enlazar con el URI a través de un enlace personalizado.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="cfa3a-215">Pero en ese caso, el marco no puede saber de antemano si el parámetro se enlaza a un URI determinado.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="cfa3a-216">Para averiguarlo, debe invocar el enlace.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="cfa3a-217">El objetivo del algoritmo de selección es seleccionar una acción de la descripción estática antes de invocar cualquier enlace.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="cfa3a-218">Por lo tanto, los tipos complejos se excluyen del algoritmo de coincidencia.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="cfa3a-219">Una vez seleccionada la acción, se invocan todos los enlaces de parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="cfa3a-220">Resumen:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-220">Summary:</span></span>

- <span data-ttu-id="cfa3a-221">La acción debe coincidir con el método HTTP de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="cfa3a-222">El nombre de la acción debe coincidir con la entrada "acción" del Diccionario de rutas, si está presente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="cfa3a-223">Para cada parámetro de la acción, si el parámetro se toma del URI, el nombre del parámetro debe encontrarse en el Diccionario de ruta o en la cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="cfa3a-224">(Se excluyen los parámetros y parámetros opcionales con tipos complejos).</span><span class="sxs-lookup"><span data-stu-id="cfa3a-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="cfa3a-225">Intente buscar coincidencias con el mayor número de parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="cfa3a-226">La mejor coincidencia podría ser un método sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="cfa3a-227">Ejemplo extendido</span><span class="sxs-lookup"><span data-stu-id="cfa3a-227">Extended Example</span></span>

<span data-ttu-id="cfa3a-228">Route</span><span class="sxs-lookup"><span data-stu-id="cfa3a-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="cfa3a-229">Controlador:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="cfa3a-230">Solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="cfa3a-231">Coincidencia de rutas</span><span class="sxs-lookup"><span data-stu-id="cfa3a-231">Route Matching</span></span>

<span data-ttu-id="cfa3a-232">El URI coincide con la ruta denominada "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="cfa3a-233">El Diccionario de rutas contiene las siguientes entradas:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="cfa3a-234">controlador: "Products"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-234">controller: "products"</span></span>
- <span data-ttu-id="cfa3a-235">ID.: "1"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-235">id: "1"</span></span>

<span data-ttu-id="cfa3a-236">El Diccionario de rutas no contiene los parámetros de la cadena de consulta, "version" y "details", pero todavía se tendrán en cuenta durante la selección de la acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="cfa3a-237">Selección de controlador</span><span class="sxs-lookup"><span data-stu-id="cfa3a-237">Controller Selection</span></span>

<span data-ttu-id="cfa3a-238">Desde la entrada "Controller" en el Diccionario de ruta, el tipo de controlador es `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="cfa3a-239">Selección de acción</span><span class="sxs-lookup"><span data-stu-id="cfa3a-239">Action Selection</span></span>

<span data-ttu-id="cfa3a-240">La solicitud HTTP es una solicitud GET.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="cfa3a-241">Las acciones de controlador que admiten GET son `GetAll`, `GetById`y `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="cfa3a-242">El Diccionario de rutas no contiene una entrada para "Action", por lo que no es necesario que coincida con el nombre de la acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="cfa3a-243">A continuación, intentamos hacer coincidir los nombres de parámetro para las acciones, solo en las acciones GET.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="cfa3a-244">Acción</span><span class="sxs-lookup"><span data-stu-id="cfa3a-244">Action</span></span> | <span data-ttu-id="cfa3a-245">Parámetros que deben coincidir</span><span class="sxs-lookup"><span data-stu-id="cfa3a-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="cfa3a-246">ninguna</span><span class="sxs-lookup"><span data-stu-id="cfa3a-246">none</span></span> |
| `GetById` | <span data-ttu-id="cfa3a-247">"id"</span><span class="sxs-lookup"><span data-stu-id="cfa3a-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="cfa3a-248">Name</span><span class="sxs-lookup"><span data-stu-id="cfa3a-248">"name"</span></span> |

<span data-ttu-id="cfa3a-249">Observe que no se tiene en cuenta el parámetro *version* de `GetById`, porque es un parámetro opcional.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="cfa3a-250">El método `GetAll` coincide trivialmente.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="cfa3a-251">El método `GetById` también coincide, porque el Diccionario de rutas contiene "ID".</span><span class="sxs-lookup"><span data-stu-id="cfa3a-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="cfa3a-252">El método `FindProductsByName` no coincide.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="cfa3a-253">El método `GetById` gana, porque coincide con un parámetro, frente a ningún parámetro de `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="cfa3a-254">El método se invoca con los siguientes valores de parámetro:</span><span class="sxs-lookup"><span data-stu-id="cfa3a-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="cfa3a-255">*ID.* = 1</span><span class="sxs-lookup"><span data-stu-id="cfa3a-255">*id* = 1</span></span>
- <span data-ttu-id="cfa3a-256">*versión* = 1,5</span><span class="sxs-lookup"><span data-stu-id="cfa3a-256">*version* = 1.5</span></span>

<span data-ttu-id="cfa3a-257">Tenga en cuenta que aunque la *versión* no se utilizó en el algoritmo de selección, el valor del parámetro procede de la cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="cfa3a-258">Puntos de extensión</span><span class="sxs-lookup"><span data-stu-id="cfa3a-258">Extension Points</span></span>

<span data-ttu-id="cfa3a-259">La API Web proporciona puntos de extensión para algunas partes del proceso de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="cfa3a-260">Interfaz</span><span class="sxs-lookup"><span data-stu-id="cfa3a-260">Interface</span></span> | <span data-ttu-id="cfa3a-261">Description</span><span class="sxs-lookup"><span data-stu-id="cfa3a-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cfa3a-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="cfa3a-263">Selecciona el controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-263">Selects the controller.</span></span> |
| <span data-ttu-id="cfa3a-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="cfa3a-265">Obtiene la lista de tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-265">Gets the list of controller types.</span></span> <span data-ttu-id="cfa3a-266">**DefaultHttpControllerSelector** elige el tipo de controlador de esta lista.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="cfa3a-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="cfa3a-268">Obtiene la lista de ensamblados del proyecto.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="cfa3a-269">La interfaz **IHttpControllerTypeResolver** usa esta lista para encontrar los tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="cfa3a-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="cfa3a-271">Crea nuevas instancias de controlador.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="cfa3a-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="cfa3a-273">Selecciona la acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-273">Selects the action.</span></span> |
| <span data-ttu-id="cfa3a-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="cfa3a-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="cfa3a-275">Invoca la acción.</span><span class="sxs-lookup"><span data-stu-id="cfa3a-275">Invokes the action.</span></span> |

<span data-ttu-id="cfa3a-276">Para proporcionar su propia implementación para cualquiera de estas interfaces, use la colección de **servicios** en el objeto **HttpConfiguration** :</span><span class="sxs-lookup"><span data-stu-id="cfa3a-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
