---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Acciones y funciones en OData v4 con ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: En OData, acciones y funciones son una manera de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Este tutorial se muestra cómo...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 45b84ec4ee76e83ece99bf6841c28e13c3ab7728
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063232"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="8eaca-104">Acciones y funciones en OData v4 con ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="8eaca-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="8eaca-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8eaca-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8eaca-106">En OData, acciones y funciones son una manera de agregar comportamientos de lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="8eaca-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="8eaca-107">Este tutorial muestra cómo agregar funciones y acciones a un extremo OData v4, con Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="8eaca-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="8eaca-108">El tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="8eaca-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8eaca-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="8eaca-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="8eaca-110">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="8eaca-110">Web API 2.2</span></span>
> - <span data-ttu-id="8eaca-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="8eaca-111">OData v4</span></span>
> - <span data-ttu-id="8eaca-112">Visual Studio 2013 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="8eaca-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="8eaca-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8eaca-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="8eaca-114">Versiones de tutoriales</span><span class="sxs-lookup"><span data-stu-id="8eaca-114">Tutorial versions</span></span>
>
> <span data-ttu-id="8eaca-115">Para OData versión 3, vea [acciones de OData en ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="8eaca-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="8eaca-116">La diferencia entre *acciones* y *funciones* es que las acciones pueden tener efectos secundarios y las funciones no lo hacen.</span><span class="sxs-lookup"><span data-stu-id="8eaca-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="8eaca-117">Acciones y funciones pueden devolver datos.</span><span class="sxs-lookup"><span data-stu-id="8eaca-117">Both actions and functions can return data.</span></span> <span data-ttu-id="8eaca-118">Algunos usos de las acciones incluyen:</span><span class="sxs-lookup"><span data-stu-id="8eaca-118">Some uses for actions include:</span></span>

- <span data-ttu-id="8eaca-119">Transacciones complejas.</span><span class="sxs-lookup"><span data-stu-id="8eaca-119">Complex transactions.</span></span>
- <span data-ttu-id="8eaca-120">Manipular varias entidades a la vez.</span><span class="sxs-lookup"><span data-stu-id="8eaca-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="8eaca-121">Permitir actualizaciones únicamente a determinadas propiedades de una entidad.</span><span class="sxs-lookup"><span data-stu-id="8eaca-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="8eaca-122">Envío de datos que no es una entidad.</span><span class="sxs-lookup"><span data-stu-id="8eaca-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="8eaca-123">Las funciones son útiles para devolver información que no corresponden directamente a una entidad o colección.</span><span class="sxs-lookup"><span data-stu-id="8eaca-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="8eaca-124">Una acción (o función) puede tener como destino una sola entidad o una colección.</span><span class="sxs-lookup"><span data-stu-id="8eaca-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="8eaca-125">En la terminología de OData, se trata la *enlace*.</span><span class="sxs-lookup"><span data-stu-id="8eaca-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="8eaca-126">También puede tener &quot;independiente&quot; las acciones o funciones, que se conocen como operaciones estáticas en el servicio.</span><span class="sxs-lookup"><span data-stu-id="8eaca-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="8eaca-127">Ejemplo: Agregar una acción</span><span class="sxs-lookup"><span data-stu-id="8eaca-127">Example: Adding an Action</span></span>

<span data-ttu-id="8eaca-128">Vamos a definir una acción para valorar un producto.</span><span class="sxs-lookup"><span data-stu-id="8eaca-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="8eaca-129">Este tutorial se basa en el tutorial [crear OData v4 extremo mediante ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="8eaca-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="8eaca-130">En primer lugar, agregue un `ProductRating` modelo para representar las clasificaciones.</span><span class="sxs-lookup"><span data-stu-id="8eaca-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="8eaca-131">Agregue también un **DbSet** a la `ProductsContext` clase, por lo que EF creará una tabla de clasificación en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8eaca-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="8eaca-132">Agregar la acción al EDM</span><span class="sxs-lookup"><span data-stu-id="8eaca-132">Add the Action to the EDM</span></span>

<span data-ttu-id="8eaca-133">En WebApiConfig.cs, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="8eaca-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="8eaca-134">El **EntityTypeConfiguration.Action** método agrega una acción para el entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="8eaca-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="8eaca-135">El **parámetro** método especifica un parámetro con tipo para la acción.</span><span class="sxs-lookup"><span data-stu-id="8eaca-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="8eaca-136">Este código también establece el espacio de nombres para el EDM.</span><span class="sxs-lookup"><span data-stu-id="8eaca-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="8eaca-137">El espacio de nombres es importante porque el URI para la acción incluye el nombre de acción completa:</span><span class="sxs-lookup"><span data-stu-id="8eaca-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="8eaca-138">En una configuración típica de IIS, el punto en esta dirección URL hará que IIS devuelva el error 404.</span><span class="sxs-lookup"><span data-stu-id="8eaca-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="8eaca-139">Para resolver este problema agregando la siguiente sección al archivo Web.Config:</span><span class="sxs-lookup"><span data-stu-id="8eaca-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="8eaca-140">Agregar un método de controlador para la acción</span><span class="sxs-lookup"><span data-stu-id="8eaca-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="8eaca-141">Para habilitar el &quot;tasa&quot; acción, agregue el siguiente método a `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="8eaca-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="8eaca-142">Tenga en cuenta que el nombre del método coincide con el nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="8eaca-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="8eaca-143">El **[HttpPost]** atributo especifica el método es un método HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8eaca-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="8eaca-144">Para invocar la acción, el cliente envía una solicitud HTTP POST similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8eaca-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="8eaca-145">El &quot;tasa&quot; acción está enlazada a las instancias de Product, por lo que el URI para la acción es el nombre de acción completa anexado a la URI de la entidad.</span><span class="sxs-lookup"><span data-stu-id="8eaca-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="8eaca-146">(Recuerde que configuramos el espacio de nombres EDM &quot;ProductService&quot;, por lo que es el nombre de acción completa &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="8eaca-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="8eaca-147">El cuerpo de la solicitud contiene los parámetros de acción como una carga JSON.</span><span class="sxs-lookup"><span data-stu-id="8eaca-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="8eaca-148">API Web convierte automáticamente la carga de JSON para un **ODataActionParameters** objeto, que es más que un diccionario de valores de parámetro.</span><span class="sxs-lookup"><span data-stu-id="8eaca-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="8eaca-149">Utilice este diccionario para tener acceso a los parámetros del método de controlador.</span><span class="sxs-lookup"><span data-stu-id="8eaca-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="8eaca-150">Si el cliente envía los parámetros de acción en el problema de formato, el valor de **ModelState.IsValid** es false.</span><span class="sxs-lookup"><span data-stu-id="8eaca-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="8eaca-151">Active esta marca en el método de controlador y devolverá un error si **IsValid** es false.</span><span class="sxs-lookup"><span data-stu-id="8eaca-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="8eaca-152">Ejemplo: Agregar una función</span><span class="sxs-lookup"><span data-stu-id="8eaca-152">Example: Adding a Function</span></span>

<span data-ttu-id="8eaca-153">Ahora vamos a agregar una función de OData que devuelve el producto más costoso.</span><span class="sxs-lookup"><span data-stu-id="8eaca-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="8eaca-154">Como antes, el primer paso es agregar la función en el EDM.</span><span class="sxs-lookup"><span data-stu-id="8eaca-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="8eaca-155">En WebApiConfig.cs, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="8eaca-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="8eaca-156">En este caso, la función está enlazada a la colección de productos, en lugar de las instancias individuales de producto.</span><span class="sxs-lookup"><span data-stu-id="8eaca-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="8eaca-157">Los clientes de invocan la función mediante el envío de una solicitud GET:</span><span class="sxs-lookup"><span data-stu-id="8eaca-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="8eaca-158">Este es el método de controlador para esta función:</span><span class="sxs-lookup"><span data-stu-id="8eaca-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="8eaca-159">Tenga en cuenta que el nombre del método coincide con el nombre de función.</span><span class="sxs-lookup"><span data-stu-id="8eaca-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="8eaca-160">El **[HttpGet]** atributo especifica el método es un método HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="8eaca-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="8eaca-161">Esta es la respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="8eaca-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="8eaca-162">Ejemplo: Adición de una función sin enlazar</span><span class="sxs-lookup"><span data-stu-id="8eaca-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="8eaca-163">El ejemplo anterior era una función enlazada a una colección.</span><span class="sxs-lookup"><span data-stu-id="8eaca-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="8eaca-164">En el ejemplo siguiente, crearemos un *independiente* función.</span><span class="sxs-lookup"><span data-stu-id="8eaca-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="8eaca-165">Las funciones no enlazadas se denominan como operaciones estáticas en el servicio.</span><span class="sxs-lookup"><span data-stu-id="8eaca-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="8eaca-166">La función en este ejemplo devolverá los impuestos por un código postal determinado.</span><span class="sxs-lookup"><span data-stu-id="8eaca-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="8eaca-167">En el archivo WebApiConfig, agregue la función en el EDM:</span><span class="sxs-lookup"><span data-stu-id="8eaca-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="8eaca-168">Tenga en cuenta que estamos llamando a **función** directamente en el **ODataModelBuilder**, en lugar del tipo de entidad o colección.</span><span class="sxs-lookup"><span data-stu-id="8eaca-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="8eaca-169">Esto indica que el generador de modelos que la función es independiente.</span><span class="sxs-lookup"><span data-stu-id="8eaca-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="8eaca-170">Este es el método de controlador que implementa la función:</span><span class="sxs-lookup"><span data-stu-id="8eaca-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="8eaca-171">No importa qué controlador de API Web Coloque este método.</span><span class="sxs-lookup"><span data-stu-id="8eaca-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="8eaca-172">Podría poner en `ProductsController`, o definir un controlador aparte.</span><span class="sxs-lookup"><span data-stu-id="8eaca-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="8eaca-173">El **[ODataRoute]** atributo define la plantilla URI para la función.</span><span class="sxs-lookup"><span data-stu-id="8eaca-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="8eaca-174">Este es un ejemplo de solicitud de cliente:</span><span class="sxs-lookup"><span data-stu-id="8eaca-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="8eaca-175">La respuesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="8eaca-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
