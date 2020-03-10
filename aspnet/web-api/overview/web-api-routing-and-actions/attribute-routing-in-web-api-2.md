---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Enrutamiento de atributos en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7da1805d8a7066e82743dc9bd7e024cc9813ee89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447001"
---
# <a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="43b33-102">Enrutamiento de atributos en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="43b33-102">Attribute Routing in ASP.NET Web API 2</span></span>

<span data-ttu-id="43b33-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="43b33-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="43b33-104">El *enrutamiento* es el modo en que la API Web coincide con un URI de una acción.</span><span class="sxs-lookup"><span data-stu-id="43b33-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="43b33-105">Web API 2 admite un nuevo tipo de enrutamiento, denominado *enrutamiento de atributos*.</span><span class="sxs-lookup"><span data-stu-id="43b33-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="43b33-106">Como implica el nombre, el enrutamiento de atributos utiliza atributos para definir rutas.</span><span class="sxs-lookup"><span data-stu-id="43b33-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="43b33-107">El enrutamiento de atributos le proporciona más control sobre los URI de la API Web.</span><span class="sxs-lookup"><span data-stu-id="43b33-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="43b33-108">Por ejemplo, puede crear fácilmente URI que describen jerarquías de recursos.</span><span class="sxs-lookup"><span data-stu-id="43b33-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="43b33-109">El estilo anterior de enrutamiento, denominado enrutamiento basado en convenciones, sigue siendo totalmente compatible.</span><span class="sxs-lookup"><span data-stu-id="43b33-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="43b33-110">De hecho, puede combinar ambas técnicas en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="43b33-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="43b33-111">En este tema se muestra cómo habilitar el enrutamiento de atributos y se describen las distintas opciones para el enrutamiento de atributos.</span><span class="sxs-lookup"><span data-stu-id="43b33-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="43b33-112">Para ver un tutorial de un extremo a otro que usa el enrutamiento de atributos, consulte [creación de una API de REST con enrutamiento de atributos en Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="43b33-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43b33-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="43b33-113">Prerequisites</span></span>

<span data-ttu-id="43b33-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="43b33-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="43b33-115">También puede usar el administrador de paquetes NuGet para instalar los paquetes necesarios.</span><span class="sxs-lookup"><span data-stu-id="43b33-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="43b33-116">En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="43b33-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="43b33-117">Escriba el siguiente comando en la ventana de la consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="43b33-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="43b33-118">¿Por qué el enrutamiento de atributos?</span><span class="sxs-lookup"><span data-stu-id="43b33-118">Why Attribute Routing?</span></span>

<span data-ttu-id="43b33-119">La primera versión de la API Web usó el enrutamiento *basado en convenciones* .</span><span class="sxs-lookup"><span data-stu-id="43b33-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="43b33-120">En ese tipo de enrutamiento, se definen una o varias plantillas de ruta, que básicamente son cadenas con parámetros.</span><span class="sxs-lookup"><span data-stu-id="43b33-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="43b33-121">Cuando el marco de trabajo recibe una solicitud, coincide con el URI en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="43b33-122">(Para obtener más información sobre el enrutamiento basado en convenciones, consulte [enrutamiento en ASP.net web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="43b33-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="43b33-123">Una ventaja del enrutamiento basado en convenciones es que las plantillas se definen en un único lugar y las reglas de enrutamiento se aplican de forma coherente en todos los controladores.</span><span class="sxs-lookup"><span data-stu-id="43b33-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="43b33-124">Desafortunadamente, el enrutamiento basado en convenciones dificulta la compatibilidad con determinados patrones de URI que son comunes en las API de RESTful.</span><span class="sxs-lookup"><span data-stu-id="43b33-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="43b33-125">Por ejemplo, los recursos suelen contener recursos secundarios: los clientes tienen pedidos, las películas tienen actores, los libros tienen autores, etc.</span><span class="sxs-lookup"><span data-stu-id="43b33-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="43b33-126">Es natural crear identificadores URI que reflejen estas relaciones:</span><span class="sxs-lookup"><span data-stu-id="43b33-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="43b33-127">Este tipo de URI es difícil de crear mediante el enrutamiento basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="43b33-128">Aunque se puede hacer, los resultados no se escalan bien si tiene muchos controladores o tipos de recursos.</span><span class="sxs-lookup"><span data-stu-id="43b33-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="43b33-129">Con el enrutamiento de atributos, es trivial definir una ruta para este URI.</span><span class="sxs-lookup"><span data-stu-id="43b33-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="43b33-130">Simplemente agregue un atributo a la acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="43b33-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="43b33-131">Estos son otros patrones que facilita el enrutamiento de atributos.</span><span class="sxs-lookup"><span data-stu-id="43b33-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="43b33-132">**Control de versiones de API**</span><span class="sxs-lookup"><span data-stu-id="43b33-132">**API versioning**</span></span>

<span data-ttu-id="43b33-133">En este ejemplo, "/API/v1/Products" se enrutaría a un controlador diferente que "/API/V2/Products".</span><span class="sxs-lookup"><span data-stu-id="43b33-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="43b33-134">**Segmentos de URI sobrecargados**</span><span class="sxs-lookup"><span data-stu-id="43b33-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="43b33-135">En este ejemplo, "1" es un número de pedido, pero "pendiente" se asigna a una colección.</span><span class="sxs-lookup"><span data-stu-id="43b33-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="43b33-136">**Varios tipos de parámetro**</span><span class="sxs-lookup"><span data-stu-id="43b33-136">**Multiple parameter types**</span></span>

<span data-ttu-id="43b33-137">En este ejemplo, "1" es un número de pedido, pero "2013/06/16" especifica una fecha.</span><span class="sxs-lookup"><span data-stu-id="43b33-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="43b33-138">Habilitar el enrutamiento de atributos</span><span class="sxs-lookup"><span data-stu-id="43b33-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="43b33-139">Para habilitar el enrutamiento de atributos, llame a **MapHttpAttributeRoutes** durante la configuración.</span><span class="sxs-lookup"><span data-stu-id="43b33-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="43b33-140">Este método de extensión se define en la clase **System. Web. http. HttpConfigurationExtensions** .</span><span class="sxs-lookup"><span data-stu-id="43b33-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="43b33-141">El enrutamiento de atributos se puede combinar con [el enrutamiento basado en convenciones](routing-in-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="43b33-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="43b33-142">Para definir las rutas basadas en convenciones, llame al método **MapHttpRoute** .</span><span class="sxs-lookup"><span data-stu-id="43b33-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="43b33-143">Para obtener más información sobre la configuración de la API Web, consulte [configuración de ASP.net web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="43b33-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="43b33-144">Nota: migración desde web API 1</span><span class="sxs-lookup"><span data-stu-id="43b33-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="43b33-145">Antes de la API Web 2, las plantillas de proyecto de la API Web generaban código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="43b33-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="43b33-146">Si el enrutamiento de atributos está habilitado, este código producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="43b33-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="43b33-147">Si actualiza un proyecto de API Web existente para usar el enrutamiento de atributos, asegúrese de actualizar este código de configuración a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="43b33-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="43b33-148">Para obtener más información, consulte Configuración de la [API Web con hospedaje de ASP.net](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="43b33-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>

<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="43b33-149">Agregar atributos de ruta</span><span class="sxs-lookup"><span data-stu-id="43b33-149">Adding Route Attributes</span></span>

<span data-ttu-id="43b33-150">Este es un ejemplo de una ruta definida mediante un atributo:</span><span class="sxs-lookup"><span data-stu-id="43b33-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="43b33-151">La cadena &quot;customers/{customerId}/Orders&quot; es la plantilla URI de la ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="43b33-152">Web API intenta hacer coincidir el URI de solicitud con la plantilla.</span><span class="sxs-lookup"><span data-stu-id="43b33-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="43b33-153">En este ejemplo, "Customers" y "Orders" son segmentos literales y "{customerId}" es un parámetro de variable.</span><span class="sxs-lookup"><span data-stu-id="43b33-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="43b33-154">Los siguientes URI coincidirían con esta plantilla:</span><span class="sxs-lookup"><span data-stu-id="43b33-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="43b33-155">Puede restringir la coincidencia mediante [restricciones](#constraints), descritas más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="43b33-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="43b33-156">Observe que el parámetro &quot;{customerId}&quot; de la plantilla de ruta coincide con el nombre del parámetro *customerId* en el método.</span><span class="sxs-lookup"><span data-stu-id="43b33-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="43b33-157">Cuando la API Web invoca la acción del controlador, intenta enlazar los parámetros de ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="43b33-158">Por ejemplo, si el URI es `http://example.com/customers/1/orders`, Web API intenta enlazar el valor "1" al parámetro *customerId* en la acción.</span><span class="sxs-lookup"><span data-stu-id="43b33-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="43b33-159">Una plantilla de URI puede tener varios parámetros:</span><span class="sxs-lookup"><span data-stu-id="43b33-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="43b33-160">Los métodos de controlador que no tienen un atributo de ruta usan el enrutamiento basado en convenciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="43b33-161">De este modo, puede combinar ambos tipos de enrutamiento en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="43b33-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="43b33-162">HTTP Methods</span><span class="sxs-lookup"><span data-stu-id="43b33-162">HTTP Methods</span></span>

<span data-ttu-id="43b33-163">Web API también selecciona acciones basadas en el método HTTP de la solicitud (GET, POST, etc.).</span><span class="sxs-lookup"><span data-stu-id="43b33-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="43b33-164">De forma predeterminada, Web API busca una coincidencia sin distinción entre mayúsculas y minúsculas con el inicio del nombre del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="43b33-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="43b33-165">Por ejemplo, un método de controlador denominado `PutCustomers` coincide con una solicitud PUT de HTTP.</span><span class="sxs-lookup"><span data-stu-id="43b33-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="43b33-166">Puede invalidar esta Convención decorando el método con cualquiera de los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="43b33-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="43b33-167">**HttpDelete**</span><span class="sxs-lookup"><span data-stu-id="43b33-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="43b33-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="43b33-168">**[HttpGet]**</span></span>
- <span data-ttu-id="43b33-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="43b33-169">**[HttpHead]**</span></span>
- <span data-ttu-id="43b33-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="43b33-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="43b33-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="43b33-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="43b33-172">**HttpPost**</span><span class="sxs-lookup"><span data-stu-id="43b33-172">**[HttpPost]**</span></span>
- <span data-ttu-id="43b33-173">**HttpPut**</span><span class="sxs-lookup"><span data-stu-id="43b33-173">**[HttpPut]**</span></span>

<span data-ttu-id="43b33-174">En el ejemplo siguiente se asigna el método CreateBook a las solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="43b33-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="43b33-175">Para todos los demás métodos HTTP, incluidos los métodos no estándar, use el atributo **AcceptVerbs** , que toma una lista de métodos http.</span><span class="sxs-lookup"><span data-stu-id="43b33-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="43b33-176">Prefijos de ruta</span><span class="sxs-lookup"><span data-stu-id="43b33-176">Route Prefixes</span></span>

<span data-ttu-id="43b33-177">A menudo, todas las rutas de un controlador se inician con el mismo prefijo.</span><span class="sxs-lookup"><span data-stu-id="43b33-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="43b33-178">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="43b33-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="43b33-179">Puede establecer un prefijo común para todo un controlador mediante el atributo **[RoutePrefix]** :</span><span class="sxs-lookup"><span data-stu-id="43b33-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="43b33-180">Use una tilde (~) en el atributo del método para invalidar el prefijo de ruta:</span><span class="sxs-lookup"><span data-stu-id="43b33-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="43b33-181">El prefijo de ruta puede incluir parámetros:</span><span class="sxs-lookup"><span data-stu-id="43b33-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="43b33-182">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="43b33-182">Route Constraints</span></span>

<span data-ttu-id="43b33-183">Las restricciones de ruta permiten restringir el modo en que se buscan coincidencias con los parámetros de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="43b33-184">La sintaxis general es &quot;{parámetro: Constraint}&quot;.</span><span class="sxs-lookup"><span data-stu-id="43b33-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="43b33-185">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="43b33-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="43b33-186">Aquí, la primera ruta solo se seleccionará si el identificador de &quot;&quot; segmento del URI es un entero.</span><span class="sxs-lookup"><span data-stu-id="43b33-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="43b33-187">De lo contrario, se elegirá la segunda ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="43b33-188">En la tabla siguiente se enumeran las restricciones que se admiten.</span><span class="sxs-lookup"><span data-stu-id="43b33-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="43b33-189">Restricción</span><span class="sxs-lookup"><span data-stu-id="43b33-189">Constraint</span></span> | <span data-ttu-id="43b33-190">Description</span><span class="sxs-lookup"><span data-stu-id="43b33-190">Description</span></span> | <span data-ttu-id="43b33-191">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="43b33-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="43b33-192">alpha</span><span class="sxs-lookup"><span data-stu-id="43b33-192">alpha</span></span> | <span data-ttu-id="43b33-193">Coincide con caracteres del alfabeto latino en mayúsculas o minúsculas (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="43b33-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="43b33-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="43b33-194">{x:alpha}</span></span> |
| <span data-ttu-id="43b33-195">bool</span><span class="sxs-lookup"><span data-stu-id="43b33-195">bool</span></span> | <span data-ttu-id="43b33-196">Coincide con un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="43b33-196">Matches a Boolean value.</span></span> | <span data-ttu-id="43b33-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="43b33-197">{x:bool}</span></span> |
| <span data-ttu-id="43b33-198">datetime</span><span class="sxs-lookup"><span data-stu-id="43b33-198">datetime</span></span> | <span data-ttu-id="43b33-199">Coincide con un valor **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="43b33-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="43b33-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="43b33-200">{x:datetime}</span></span> |
| <span data-ttu-id="43b33-201">decimal</span><span class="sxs-lookup"><span data-stu-id="43b33-201">decimal</span></span> | <span data-ttu-id="43b33-202">Coincide con un valor decimal.</span><span class="sxs-lookup"><span data-stu-id="43b33-202">Matches a decimal value.</span></span> | <span data-ttu-id="43b33-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="43b33-203">{x:decimal}</span></span> |
| <span data-ttu-id="43b33-204">double</span><span class="sxs-lookup"><span data-stu-id="43b33-204">double</span></span> | <span data-ttu-id="43b33-205">Coincide con un valor de punto flotante de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="43b33-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="43b33-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="43b33-206">{x:double}</span></span> |
| <span data-ttu-id="43b33-207">float</span><span class="sxs-lookup"><span data-stu-id="43b33-207">float</span></span> | <span data-ttu-id="43b33-208">Coincide con un valor de punto flotante de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="43b33-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="43b33-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="43b33-209">{x:float}</span></span> |
| <span data-ttu-id="43b33-210">guid</span><span class="sxs-lookup"><span data-stu-id="43b33-210">guid</span></span> | <span data-ttu-id="43b33-211">Coincide con un valor de GUID.</span><span class="sxs-lookup"><span data-stu-id="43b33-211">Matches a GUID value.</span></span> | <span data-ttu-id="43b33-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="43b33-212">{x:guid}</span></span> |
| <span data-ttu-id="43b33-213">int</span><span class="sxs-lookup"><span data-stu-id="43b33-213">int</span></span> | <span data-ttu-id="43b33-214">Coincide con un valor entero de 32 bits.</span><span class="sxs-lookup"><span data-stu-id="43b33-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="43b33-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="43b33-215">{x:int}</span></span> |
| <span data-ttu-id="43b33-216">longitud</span><span class="sxs-lookup"><span data-stu-id="43b33-216">length</span></span> | <span data-ttu-id="43b33-217">Coincide con una cadena con la longitud especificada o dentro de un intervalo de longitud especificado.</span><span class="sxs-lookup"><span data-stu-id="43b33-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="43b33-218">{x:length (6)} {x:length (1, 20)}</span><span class="sxs-lookup"><span data-stu-id="43b33-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="43b33-219">long</span><span class="sxs-lookup"><span data-stu-id="43b33-219">long</span></span> | <span data-ttu-id="43b33-220">Coincide con un valor entero de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="43b33-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="43b33-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="43b33-221">{x:long}</span></span> |
| <span data-ttu-id="43b33-222">max</span><span class="sxs-lookup"><span data-stu-id="43b33-222">max</span></span> | <span data-ttu-id="43b33-223">Coincide con un entero con un valor máximo.</span><span class="sxs-lookup"><span data-stu-id="43b33-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="43b33-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="43b33-224">{x:max(10)}</span></span> |
| <span data-ttu-id="43b33-225">longitud</span><span class="sxs-lookup"><span data-stu-id="43b33-225">maxlength</span></span> | <span data-ttu-id="43b33-226">Coincide con una cadena con una longitud máxima.</span><span class="sxs-lookup"><span data-stu-id="43b33-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="43b33-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="43b33-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="43b33-228">min</span><span class="sxs-lookup"><span data-stu-id="43b33-228">min</span></span> | <span data-ttu-id="43b33-229">Coincide con un entero con un valor mínimo.</span><span class="sxs-lookup"><span data-stu-id="43b33-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="43b33-230">{x:min(10)}</span><span class="sxs-lookup"><span data-stu-id="43b33-230">{x:min(10)}</span></span> |
| <span data-ttu-id="43b33-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="43b33-231">minlength</span></span> | <span data-ttu-id="43b33-232">Coincide con una cadena con una longitud mínima.</span><span class="sxs-lookup"><span data-stu-id="43b33-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="43b33-233">{x:MinLength (10)}</span><span class="sxs-lookup"><span data-stu-id="43b33-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="43b33-234">range</span><span class="sxs-lookup"><span data-stu-id="43b33-234">range</span></span> | <span data-ttu-id="43b33-235">Coincide con un entero dentro de un intervalo de valores.</span><span class="sxs-lookup"><span data-stu-id="43b33-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="43b33-236">{x:Range (10, 50)}</span><span class="sxs-lookup"><span data-stu-id="43b33-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="43b33-237">regex</span><span class="sxs-lookup"><span data-stu-id="43b33-237">regex</span></span> | <span data-ttu-id="43b33-238">Coincide con una expresión regular.</span><span class="sxs-lookup"><span data-stu-id="43b33-238">Matches a regular expression.</span></span> | <span data-ttu-id="43b33-239">{x:regex (^ \d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="43b33-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="43b33-240">Observe que algunas de las restricciones, como &quot;min&quot;, toman argumentos entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="43b33-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="43b33-241">Puede aplicar varias restricciones a un parámetro, separadas por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="43b33-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="43b33-242">Restricciones de ruta personalizadas</span><span class="sxs-lookup"><span data-stu-id="43b33-242">Custom Route Constraints</span></span>

<span data-ttu-id="43b33-243">Puede crear restricciones de ruta personalizadas implementando la interfaz **IHttpRouteConstraint** .</span><span class="sxs-lookup"><span data-stu-id="43b33-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="43b33-244">Por ejemplo, la restricción siguiente restringe un parámetro a un valor entero distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="43b33-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="43b33-245">En el código siguiente se muestra cómo registrar la restricción:</span><span class="sxs-lookup"><span data-stu-id="43b33-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="43b33-246">Ahora puede aplicar la restricción en las rutas:</span><span class="sxs-lookup"><span data-stu-id="43b33-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="43b33-247">También puede reemplazar toda la clase **DefaultInlineConstraintResolver** implementando la interfaz **IInlineConstraintResolver** .</span><span class="sxs-lookup"><span data-stu-id="43b33-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="43b33-248">Al hacerlo, se reemplazarán todas las restricciones integradas, a menos que la implementación de **IInlineConstraintResolver** las agregue específicamente.</span><span class="sxs-lookup"><span data-stu-id="43b33-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="43b33-249">Parámetros de URI opcionales y valores predeterminados</span><span class="sxs-lookup"><span data-stu-id="43b33-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="43b33-250">Puede crear un parámetro de URI opcional si agrega un signo de interrogación al parámetro Route.</span><span class="sxs-lookup"><span data-stu-id="43b33-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="43b33-251">Si un parámetro de ruta es opcional, debe definir un valor predeterminado para el parámetro de método.</span><span class="sxs-lookup"><span data-stu-id="43b33-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="43b33-252">En este ejemplo, `/api/books/locale/1033` y `/api/books/locale` devuelven el mismo recurso.</span><span class="sxs-lookup"><span data-stu-id="43b33-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="43b33-253">También puede especificar un valor predeterminado dentro de la plantilla de ruta, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="43b33-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="43b33-254">Esto es prácticamente igual que el ejemplo anterior, pero hay una pequeña diferencia de comportamiento cuando se aplica el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="43b33-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="43b33-255">En el primer ejemplo ("{LCID: int?}"), el valor predeterminado de 1033 se asigna directamente al parámetro del método, por lo que el parámetro tendrá este valor exacto.</span><span class="sxs-lookup"><span data-stu-id="43b33-255">In the first example ("{lcid:int?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="43b33-256">En el segundo ejemplo ("{LCID: int = 1033}"), el valor predeterminado de "1033" pasa por el proceso de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="43b33-256">In the second example ("{lcid:int=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="43b33-257">El enlazador de modelos predeterminado convertirá "1033" en el valor numérico 1033.</span><span class="sxs-lookup"><span data-stu-id="43b33-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="43b33-258">Sin embargo, podría conectar un enlazador de modelos personalizado, que podría hacer algo diferente.</span><span class="sxs-lookup"><span data-stu-id="43b33-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="43b33-259">(En la mayoría de los casos, a menos que tenga enlazadores de modelos personalizados en la canalización, las dos formas serán equivalentes).</span><span class="sxs-lookup"><span data-stu-id="43b33-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="43b33-260">Nombres de ruta</span><span class="sxs-lookup"><span data-stu-id="43b33-260">Route Names</span></span>

<span data-ttu-id="43b33-261">En Web API, cada ruta tiene un nombre.</span><span class="sxs-lookup"><span data-stu-id="43b33-261">In Web API, every route has a name.</span></span> <span data-ttu-id="43b33-262">Los nombres de ruta son útiles para generar vínculos, por lo que puede incluir un vínculo en una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="43b33-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="43b33-263">Para especificar el nombre de la ruta, establezca la propiedad **nombre** en el atributo.</span><span class="sxs-lookup"><span data-stu-id="43b33-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="43b33-264">En el ejemplo siguiente se muestra cómo establecer el nombre de la ruta y cómo usar el nombre de la ruta al generar un vínculo.</span><span class="sxs-lookup"><span data-stu-id="43b33-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="43b33-265">Orden de ruta</span><span class="sxs-lookup"><span data-stu-id="43b33-265">Route Order</span></span>

<span data-ttu-id="43b33-266">Cuando el marco intenta hacer coincidir un URI con una ruta, evalúa las rutas en un orden determinado.</span><span class="sxs-lookup"><span data-stu-id="43b33-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="43b33-267">Para especificar el orden, establezca la propiedad **Order** en el atributo Route.</span><span class="sxs-lookup"><span data-stu-id="43b33-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="43b33-268">Primero se evalúan los valores inferiores.</span><span class="sxs-lookup"><span data-stu-id="43b33-268">Lower values are evaluated first.</span></span> <span data-ttu-id="43b33-269">El valor de orden predeterminado es cero.</span><span class="sxs-lookup"><span data-stu-id="43b33-269">The default order value is zero.</span></span>

<span data-ttu-id="43b33-270">A continuación se indica cómo se determina el orden total:</span><span class="sxs-lookup"><span data-stu-id="43b33-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="43b33-271">Compare la propiedad **Order** del atributo Route.</span><span class="sxs-lookup"><span data-stu-id="43b33-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="43b33-272">Examine cada segmento del URI en la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="43b33-273">Para cada segmento, orden de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="43b33-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="43b33-274">Segmentos literales.</span><span class="sxs-lookup"><span data-stu-id="43b33-274">Literal segments.</span></span>
    2. <span data-ttu-id="43b33-275">Parámetros de ruta con restricciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="43b33-276">Parámetros de ruta sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="43b33-277">Segmentos de parámetros comodín con restricciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="43b33-278">Segmentos de parámetros comodín sin restricciones.</span><span class="sxs-lookup"><span data-stu-id="43b33-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="43b33-279">En el caso de una empate, las rutas se ordenan por una comparación de cadenas ordinales sin distinción entre mayúsculas y minúsculas ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="43b33-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="43b33-280">A continuación se muestra un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="43b33-280">Here is an example.</span></span> <span data-ttu-id="43b33-281">Supongamos que define el siguiente controlador:</span><span class="sxs-lookup"><span data-stu-id="43b33-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="43b33-282">Estas rutas se ordenan de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="43b33-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="43b33-283">pedidos y detalles</span><span class="sxs-lookup"><span data-stu-id="43b33-283">orders/details</span></span>
2. <span data-ttu-id="43b33-284">orders/{id}</span><span class="sxs-lookup"><span data-stu-id="43b33-284">orders/{id}</span></span>
3. <span data-ttu-id="43b33-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="43b33-285">orders/{customerName}</span></span>
4. <span data-ttu-id="43b33-286">Orders/{\*Date}</span><span class="sxs-lookup"><span data-stu-id="43b33-286">orders/{\*date}</span></span>
5. <span data-ttu-id="43b33-287">pedidos/pendientes</span><span class="sxs-lookup"><span data-stu-id="43b33-287">orders/pending</span></span>

<span data-ttu-id="43b33-288">Observe que "details" es un segmento literal y aparece antes que "{ID}", pero "Pending" aparece en último lugar porque la propiedad **Order** es 1.</span><span class="sxs-lookup"><span data-stu-id="43b33-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="43b33-289">(En este ejemplo se da por supuesto que no hay clientes con el nombre "details" o "Pending".</span><span class="sxs-lookup"><span data-stu-id="43b33-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="43b33-290">En general, intente evitar rutas ambiguas.</span><span class="sxs-lookup"><span data-stu-id="43b33-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="43b33-291">En este ejemplo, una plantilla de ruta mejor para `GetByCustomer` es "Customers/{customerName}")</span><span class="sxs-lookup"><span data-stu-id="43b33-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
