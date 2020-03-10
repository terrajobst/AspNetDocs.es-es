---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Acciones y funciones en OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: En OData, las acciones y las funciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. En este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: f5af94e93e5b7f2351d40febbf1a468d635c9db1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448063"
---
# <a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="3d08e-104">Acciones y funciones en OData V4 mediante ASP.NET Web API 2,2</span><span class="sxs-lookup"><span data-stu-id="3d08e-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>

<span data-ttu-id="3d08e-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d08e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3d08e-106">En OData, las acciones y las funciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="3d08e-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="3d08e-107">En este tutorial se muestra cómo agregar acciones y funciones a un punto de conexión de OData V4 mediante Web API 2,2.</span><span class="sxs-lookup"><span data-stu-id="3d08e-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="3d08e-108">El tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="3d08e-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3d08e-109">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3d08e-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="3d08e-110">API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="3d08e-110">Web API 2.2</span></span>
> - <span data-ttu-id="3d08e-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="3d08e-111">OData v4</span></span>
> - <span data-ttu-id="3d08e-112">Visual Studio 2013 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="3d08e-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="3d08e-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3d08e-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="3d08e-114">Versiones del tutorial</span><span class="sxs-lookup"><span data-stu-id="3d08e-114">Tutorial versions</span></span>
>
> <span data-ttu-id="3d08e-115">Para la versión 3 de OData, consulte [acciones de oData en ASP.net web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="3d08e-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="3d08e-116">La diferencia entre *las acciones* y las *funciones* es que las acciones pueden tener efectos secundarios y las funciones no.</span><span class="sxs-lookup"><span data-stu-id="3d08e-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="3d08e-117">Tanto las acciones como las funciones pueden devolver datos.</span><span class="sxs-lookup"><span data-stu-id="3d08e-117">Both actions and functions can return data.</span></span> <span data-ttu-id="3d08e-118">Algunos usos de las acciones son:</span><span class="sxs-lookup"><span data-stu-id="3d08e-118">Some uses for actions include:</span></span>

- <span data-ttu-id="3d08e-119">Transacciones complejas.</span><span class="sxs-lookup"><span data-stu-id="3d08e-119">Complex transactions.</span></span>
- <span data-ttu-id="3d08e-120">Manipular varias entidades a la vez.</span><span class="sxs-lookup"><span data-stu-id="3d08e-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="3d08e-121">Permitir actualizaciones solo en determinadas propiedades de una entidad.</span><span class="sxs-lookup"><span data-stu-id="3d08e-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="3d08e-122">Enviar datos que no son una entidad.</span><span class="sxs-lookup"><span data-stu-id="3d08e-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="3d08e-123">Las funciones son útiles para devolver información que no se corresponde directamente con una entidad o colección.</span><span class="sxs-lookup"><span data-stu-id="3d08e-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="3d08e-124">Una acción (o función) puede tener como destino una sola entidad o una colección.</span><span class="sxs-lookup"><span data-stu-id="3d08e-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="3d08e-125">En la terminología de OData, este es el *enlace*.</span><span class="sxs-lookup"><span data-stu-id="3d08e-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="3d08e-126">También puede tener &quot;funciones o acciones de&quot; sin enlazar, a las que se llama como operaciones estáticas en el servicio.</span><span class="sxs-lookup"><span data-stu-id="3d08e-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="3d08e-127">Ejemplo: agregar una acción</span><span class="sxs-lookup"><span data-stu-id="3d08e-127">Example: Adding an Action</span></span>

<span data-ttu-id="3d08e-128">Vamos a definir una acción para clasificar un producto.</span><span class="sxs-lookup"><span data-stu-id="3d08e-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="3d08e-129">Este tutorial se basa en el tutorial [creación de un punto de conexión de oData V4 mediante ASP.net web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="3d08e-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="3d08e-130">En primer lugar, agregue un modelo de `ProductRating` para representar las clasificaciones.</span><span class="sxs-lookup"><span data-stu-id="3d08e-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="3d08e-131">Agregue también un **DbSet** a la clase `ProductsContext`, para que EF cree una tabla de clasificación en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3d08e-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="3d08e-132">Agregar la acción al EDM</span><span class="sxs-lookup"><span data-stu-id="3d08e-132">Add the Action to the EDM</span></span>

<span data-ttu-id="3d08e-133">En WebApiConfig.cs, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3d08e-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="3d08e-134">El método **EntityTypeConfiguration. Action** agrega una acción a Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="3d08e-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="3d08e-135">El método **Parameter** especifica un parámetro con tipo para la acción.</span><span class="sxs-lookup"><span data-stu-id="3d08e-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="3d08e-136">Este código también establece el espacio de nombres para el EDM.</span><span class="sxs-lookup"><span data-stu-id="3d08e-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="3d08e-137">El espacio de nombres es importante porque el URI de la acción incluye el nombre completo de la acción:</span><span class="sxs-lookup"><span data-stu-id="3d08e-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="3d08e-138">En una configuración típica de IIS, el punto de esta dirección URL hará que IIS devuelva el error 404.</span><span class="sxs-lookup"><span data-stu-id="3d08e-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="3d08e-139">Puede resolver este paso agregando la siguiente sección al archivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="3d08e-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="3d08e-140">Agregar un método de controlador para la acción</span><span class="sxs-lookup"><span data-stu-id="3d08e-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="3d08e-141">Para habilitar la &quot;frecuencia&quot; acción, agregue el método siguiente a `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="3d08e-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="3d08e-142">Observe que el nombre del método coincide con el nombre de la acción.</span><span class="sxs-lookup"><span data-stu-id="3d08e-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="3d08e-143">El atributo **[HttpPost]** especifica que el método es un método post de http.</span><span class="sxs-lookup"><span data-stu-id="3d08e-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="3d08e-144">Para invocar la acción, el cliente envía una solicitud HTTP POST como la siguiente:</span><span class="sxs-lookup"><span data-stu-id="3d08e-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="3d08e-145">La tasa de &quot;&quot; acción está enlazada a las instancias del producto, por lo que el URI de la acción es el nombre completo de la acción anexado al URI de la entidad.</span><span class="sxs-lookup"><span data-stu-id="3d08e-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="3d08e-146">(Recuerde que establecemos el espacio de nombres EDM en &quot;ProductService&quot;, por lo que el nombre completo de la acción es &quot;ProductService. rate&quot;).</span><span class="sxs-lookup"><span data-stu-id="3d08e-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="3d08e-147">El cuerpo de la solicitud contiene los parámetros de acción como una carga JSON.</span><span class="sxs-lookup"><span data-stu-id="3d08e-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="3d08e-148">Web API convierte automáticamente la carga de JSON en un objeto **ODataActionParameters** , que es simplemente un diccionario de valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="3d08e-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="3d08e-149">Use este diccionario para tener acceso a los parámetros del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="3d08e-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="3d08e-150">Si el cliente envía los parámetros de acción en el formato incorrecto, el valor de **ModelState. IsValid** es false.</span><span class="sxs-lookup"><span data-stu-id="3d08e-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="3d08e-151">Active esta marca en el método de controlador y devuelva un error si **IsValid** es false.</span><span class="sxs-lookup"><span data-stu-id="3d08e-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="3d08e-152">Ejemplo: agregar una función</span><span class="sxs-lookup"><span data-stu-id="3d08e-152">Example: Adding a Function</span></span>

<span data-ttu-id="3d08e-153">Ahora vamos a agregar una función de OData que devuelve el producto más caro.</span><span class="sxs-lookup"><span data-stu-id="3d08e-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="3d08e-154">Como antes, el primer paso es agregar la función al EDM.</span><span class="sxs-lookup"><span data-stu-id="3d08e-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="3d08e-155">En WebApiConfig.cs, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="3d08e-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="3d08e-156">En este caso, la función se enlaza a la colección Products, en lugar de a instancias de producto individuales.</span><span class="sxs-lookup"><span data-stu-id="3d08e-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="3d08e-157">Los clientes invocan la función mediante el envío de una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="3d08e-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="3d08e-158">Este es el método de controlador para esta función:</span><span class="sxs-lookup"><span data-stu-id="3d08e-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="3d08e-159">Observe que el nombre del método coincide con el nombre de la función.</span><span class="sxs-lookup"><span data-stu-id="3d08e-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="3d08e-160">El atributo **[HttpGet]** especifica que el método es un método get de http.</span><span class="sxs-lookup"><span data-stu-id="3d08e-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="3d08e-161">Esta es la respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="3d08e-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="3d08e-162">Ejemplo: agregar una función sin enlazar</span><span class="sxs-lookup"><span data-stu-id="3d08e-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="3d08e-163">El ejemplo anterior era una función enlazada a una colección.</span><span class="sxs-lookup"><span data-stu-id="3d08e-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="3d08e-164">En el siguiente ejemplo, vamos a crear una función *sin enlazar* .</span><span class="sxs-lookup"><span data-stu-id="3d08e-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="3d08e-165">Se llama a las funciones sin enlazar como operaciones estáticas en el servicio.</span><span class="sxs-lookup"><span data-stu-id="3d08e-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="3d08e-166">La función de este ejemplo devolverá el impuesto de ventas para un código postal determinado.</span><span class="sxs-lookup"><span data-stu-id="3d08e-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="3d08e-167">En el archivo WebApiConfig, agregue la función al EDM:</span><span class="sxs-lookup"><span data-stu-id="3d08e-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="3d08e-168">Observe que se llama a la **función** directamente en **ODataModelBuilder**, en lugar del tipo de entidad o de la colección.</span><span class="sxs-lookup"><span data-stu-id="3d08e-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="3d08e-169">Esto indica al generador de modelos que la función está desenlazada.</span><span class="sxs-lookup"><span data-stu-id="3d08e-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="3d08e-170">Este es el método de controlador que implementa la función:</span><span class="sxs-lookup"><span data-stu-id="3d08e-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="3d08e-171">No importa en qué controlador de API Web se coloca este método.</span><span class="sxs-lookup"><span data-stu-id="3d08e-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="3d08e-172">Puede colocarlo en `ProductsController`o definir un controlador independiente.</span><span class="sxs-lookup"><span data-stu-id="3d08e-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="3d08e-173">El atributo **[ODataRoute]** define la plantilla de URI para la función.</span><span class="sxs-lookup"><span data-stu-id="3d08e-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="3d08e-174">Este es un ejemplo de solicitud de cliente:</span><span class="sxs-lookup"><span data-stu-id="3d08e-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="3d08e-175">La respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="3d08e-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
