---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Compatibilidad con acciones de OData en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'En OData, las acciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones incluyen: implementar...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600345"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="efa0a-104">Compatibilidad de las acciones de OData en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="efa0a-104">Supporting OData Actions in ASP.NET Web API 2</span></span>

<span data-ttu-id="efa0a-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="efa0a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="efa0a-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="efa0a-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="efa0a-107">En OData, *las acciones* son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="efa0a-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="efa0a-108">Algunos usos de las acciones son:</span><span class="sxs-lookup"><span data-stu-id="efa0a-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="efa0a-109">Implementación de transacciones complejas.</span><span class="sxs-lookup"><span data-stu-id="efa0a-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="efa0a-110">Manipular varias entidades a la vez.</span><span class="sxs-lookup"><span data-stu-id="efa0a-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="efa0a-111">Permitir actualizaciones solo en determinadas propiedades de una entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="efa0a-112">Enviar información al servidor que no está definida en una entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="efa0a-113">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="efa0a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="efa0a-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="efa0a-114">Web API 2</span></span>
> - <span data-ttu-id="efa0a-115">Versión 3 de OData</span><span class="sxs-lookup"><span data-stu-id="efa0a-115">OData Version 3</span></span>
> - <span data-ttu-id="efa0a-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="efa0a-116">Entity Framework 6</span></span>

## <a name="example-rating-a-product"></a><span data-ttu-id="efa0a-117">Ejemplo: clasificación de un producto</span><span class="sxs-lookup"><span data-stu-id="efa0a-117">Example: Rating a Product</span></span>

<span data-ttu-id="efa0a-118">En este ejemplo, queremos que los usuarios puedan valorar los productos y, a continuación, exponer el promedio de las clasificaciones de cada producto.</span><span class="sxs-lookup"><span data-stu-id="efa0a-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="efa0a-119">En la base de datos, se almacenará una lista de clasificaciones con clave en los productos.</span><span class="sxs-lookup"><span data-stu-id="efa0a-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="efa0a-120">Este es el modelo que se puede usar para representar las clasificaciones en Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="efa0a-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="efa0a-121">Pero no queremos que los clientes PUBLIQUEn un objeto `ProductRating` en una colección "ratings".</span><span class="sxs-lookup"><span data-stu-id="efa0a-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="efa0a-122">De forma intuitiva, la clasificación está asociada a la colección de productos y el cliente solo debe publicar el valor de clasificación.</span><span class="sxs-lookup"><span data-stu-id="efa0a-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="efa0a-123">Por lo tanto, en lugar de usar las operaciones CRUD normales, se define una acción que un cliente puede invocar en un producto.</span><span class="sxs-lookup"><span data-stu-id="efa0a-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="efa0a-124">En la terminología de OData, la acción se *enlaza* a las entidades product.</span><span class="sxs-lookup"><span data-stu-id="efa0a-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="efa0a-125">Las acciones tienen efectos secundarios en el servidor.</span><span class="sxs-lookup"><span data-stu-id="efa0a-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="efa0a-126">Por esta razón, se invocan mediante solicitudes HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="efa0a-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="efa0a-127">Las acciones pueden tener parámetros y tipos devueltos, que se describen en los metadatos del servicio.</span><span class="sxs-lookup"><span data-stu-id="efa0a-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="efa0a-128">El cliente envía los parámetros en el cuerpo de la solicitud y el servidor envía el valor devuelto en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="efa0a-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="efa0a-129">Para invocar la acción "Rate Product", el cliente envía un POST a un URI como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="efa0a-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="efa0a-130">Los datos de la solicitud POST son simplemente la clasificación del producto:</span><span class="sxs-lookup"><span data-stu-id="efa0a-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="efa0a-131">Declare la acción en el Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="efa0a-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="efa0a-132">En la configuración de la API Web, agregue la acción a Entity Data Model (EDM):</span><span class="sxs-lookup"><span data-stu-id="efa0a-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="efa0a-133">Este código define "RateProduct" como una acción que se puede realizar en las entidades del producto.</span><span class="sxs-lookup"><span data-stu-id="efa0a-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="efa0a-134">También declara que la acción toma un parámetro **int** denominado "rating" y devuelve un valor **int** .</span><span class="sxs-lookup"><span data-stu-id="efa0a-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="efa0a-135">Agregar la acción al controlador</span><span class="sxs-lookup"><span data-stu-id="efa0a-135">Add the Action to the Controller</span></span>

<span data-ttu-id="efa0a-136">La acción "RateProduct" se enlaza a las entidades product.</span><span class="sxs-lookup"><span data-stu-id="efa0a-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="efa0a-137">Para implementar la acción, agregue un método denominado `RateProduct` al controlador Products:</span><span class="sxs-lookup"><span data-stu-id="efa0a-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="efa0a-138">Observe que el nombre de método coincide con el nombre de la acción en el EDM.</span><span class="sxs-lookup"><span data-stu-id="efa0a-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="efa0a-139">El método tiene dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="efa0a-139">The method has two parameters:</span></span>

- <span data-ttu-id="efa0a-140">*key*: la clave del producto que se va a evaluar.</span><span class="sxs-lookup"><span data-stu-id="efa0a-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="efa0a-141">*parámetros*: un diccionario de valores de parámetro de acción.</span><span class="sxs-lookup"><span data-stu-id="efa0a-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="efa0a-142">Si utiliza las convenciones de enrutamiento predeterminadas, el parámetro de clave se debe denominar "Key".</span><span class="sxs-lookup"><span data-stu-id="efa0a-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="efa0a-143">También es importante incluir el atributo **[FromOdataUri]** , como se muestra.</span><span class="sxs-lookup"><span data-stu-id="efa0a-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="efa0a-144">Este atributo indica a la API Web que use las reglas de sintaxis de OData cuando analiza la clave del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="efa0a-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="efa0a-145">Use el Diccionario de *parámetros* para obtener los parámetros de acción:</span><span class="sxs-lookup"><span data-stu-id="efa0a-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="efa0a-146">Si el cliente envía los parámetros de acción en el formato correcto, el valor de **ModelState. IsValid** es true.</span><span class="sxs-lookup"><span data-stu-id="efa0a-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="efa0a-147">En ese caso, puede usar el diccionario **ODataActionParameters** para obtener los valores de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="efa0a-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="efa0a-148">En este ejemplo, la acción de `RateProduct` toma un parámetro único denominado "rating".</span><span class="sxs-lookup"><span data-stu-id="efa0a-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="efa0a-149">Metadatos de acción</span><span class="sxs-lookup"><span data-stu-id="efa0a-149">Action Metadata</span></span>

<span data-ttu-id="efa0a-150">Para ver los metadatos del servicio, envíe una solicitud GET a/OData/$metadata.</span><span class="sxs-lookup"><span data-stu-id="efa0a-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="efa0a-151">Esta es la parte de los metadatos que declara la acción `RateProduct`:</span><span class="sxs-lookup"><span data-stu-id="efa0a-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="efa0a-152">El elemento **FunctionImport** declara la acción.</span><span class="sxs-lookup"><span data-stu-id="efa0a-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="efa0a-153">La mayoría de los campos se explican por sí solos, pero hay dos que merece la pena mencionar:</span><span class="sxs-lookup"><span data-stu-id="efa0a-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="efa0a-154">**IsBindable** significa que la acción se puede invocar en la entidad de destino, al menos parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="efa0a-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="efa0a-155">**IsAlwaysBindable** significa que la acción siempre se puede invocar en la entidad de destino.</span><span class="sxs-lookup"><span data-stu-id="efa0a-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="efa0a-156">La diferencia es que algunas acciones siempre están disponibles para los clientes, pero otras acciones pueden depender del estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="efa0a-157">Por ejemplo, supongamos que define una acción de "compra".</span><span class="sxs-lookup"><span data-stu-id="efa0a-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="efa0a-158">Solo puede comprar un elemento que esté en existencias.</span><span class="sxs-lookup"><span data-stu-id="efa0a-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="efa0a-159">Si el elemento está agotado, un cliente no puede invocar esa acción.</span><span class="sxs-lookup"><span data-stu-id="efa0a-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="efa0a-160">Al definir el EDM, el método de **acción** crea una acción que se puede enlazar siempre:</span><span class="sxs-lookup"><span data-stu-id="efa0a-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="efa0a-161">Hablaré sobre acciones no siempre enlazables (también denominadas acciones *transitorias* ) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="efa0a-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="efa0a-162">Invocar la acción</span><span class="sxs-lookup"><span data-stu-id="efa0a-162">Invoking the Action</span></span>

<span data-ttu-id="efa0a-163">Ahora veamos cómo un cliente invocaría esta acción.</span><span class="sxs-lookup"><span data-stu-id="efa0a-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="efa0a-164">Supongamos que el cliente desea proporcionar una clasificación de 2 al producto con el identificador 4.</span><span class="sxs-lookup"><span data-stu-id="efa0a-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="efa0a-165">Este es un mensaje de solicitud de ejemplo, con el formato JSON para el cuerpo de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="efa0a-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="efa0a-166">Este es el mensaje de respuesta:</span><span class="sxs-lookup"><span data-stu-id="efa0a-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="efa0a-167">Enlazar una acción a un conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="efa0a-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="efa0a-168">En el ejemplo anterior, la acción se enlaza a una sola entidad: el cliente califica un único producto.</span><span class="sxs-lookup"><span data-stu-id="efa0a-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="efa0a-169">También puede enlazar una acción a una colección de entidades.</span><span class="sxs-lookup"><span data-stu-id="efa0a-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="efa0a-170">Solo tiene que realizar los cambios siguientes:</span><span class="sxs-lookup"><span data-stu-id="efa0a-170">Just make the following changes:</span></span>

<span data-ttu-id="efa0a-171">En el EDM, agregue la acción a la propiedad de **colección** de la entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="efa0a-172">En el método de controlador, omita el parámetro *key* .</span><span class="sxs-lookup"><span data-stu-id="efa0a-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="efa0a-173">Ahora el cliente invoca la acción en el conjunto de entidades Products:</span><span class="sxs-lookup"><span data-stu-id="efa0a-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="efa0a-174">Acciones con parámetros de colección</span><span class="sxs-lookup"><span data-stu-id="efa0a-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="efa0a-175">Las acciones pueden tener parámetros que toman una colección de valores.</span><span class="sxs-lookup"><span data-stu-id="efa0a-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="efa0a-176">En el EDM, use **CollectionParameter&lt;t&gt;** para declarar el parámetro.</span><span class="sxs-lookup"><span data-stu-id="efa0a-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="efa0a-177">Esto declara un parámetro denominado "ratings" que toma una colección de valores **int** .</span><span class="sxs-lookup"><span data-stu-id="efa0a-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="efa0a-178">En el método de controlador, todavía se obtiene el valor del parámetro del objeto **ODataActionParameters** , pero ahora el valor es un valor **int&lt;int&gt;** :</span><span class="sxs-lookup"><span data-stu-id="efa0a-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="efa0a-179">Acciones transitorias</span><span class="sxs-lookup"><span data-stu-id="efa0a-179">Transient Actions</span></span>

<span data-ttu-id="efa0a-180">En el ejemplo "RateProduct", los usuarios siempre pueden clasificar un producto, por lo que la acción siempre está disponible.</span><span class="sxs-lookup"><span data-stu-id="efa0a-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="efa0a-181">Pero algunas acciones dependen del estado de la entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="efa0a-182">Por ejemplo, en un servicio de alquiler de vídeo, la acción "desproteger" no siempre está disponible.</span><span class="sxs-lookup"><span data-stu-id="efa0a-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="efa0a-183">(Depende de si hay una copia de ese vídeo disponible). Este tipo de acción se denomina acción *transitoria* .</span><span class="sxs-lookup"><span data-stu-id="efa0a-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="efa0a-184">En los metadatos del servicio, una acción transitoria tiene **IsAlwaysBindable** igual a false.</span><span class="sxs-lookup"><span data-stu-id="efa0a-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="efa0a-185">Ese es realmente el valor predeterminado, por lo que los metadatos tendrán el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="efa0a-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="efa0a-186">Este es el motivo por el que esto es importante: Si una acción es transitoria, el servidor debe indicar al cliente si la acción está disponible.</span><span class="sxs-lookup"><span data-stu-id="efa0a-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="efa0a-187">Para ello, incluye un vínculo a la acción en la entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="efa0a-188">Este es un ejemplo de una entidad de película:</span><span class="sxs-lookup"><span data-stu-id="efa0a-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="efa0a-189">La propiedad "#CheckOut" contiene un vínculo a la acción de desprotección.</span><span class="sxs-lookup"><span data-stu-id="efa0a-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="efa0a-190">Si la acción no está disponible, el servidor omite el vínculo.</span><span class="sxs-lookup"><span data-stu-id="efa0a-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="efa0a-191">Para declarar una acción transitoria en el EDM, llame al método **TransientAction** :</span><span class="sxs-lookup"><span data-stu-id="efa0a-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="efa0a-192">Además, debe proporcionar una función que devuelva un vínculo de acción para una entidad determinada.</span><span class="sxs-lookup"><span data-stu-id="efa0a-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="efa0a-193">Establezca esta función llamando a **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="efa0a-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="efa0a-194">Puede escribir la función como una expresión lambda:</span><span class="sxs-lookup"><span data-stu-id="efa0a-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="efa0a-195">Si la acción está disponible, la expresión lambda devuelve un vínculo a la acción.</span><span class="sxs-lookup"><span data-stu-id="efa0a-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="efa0a-196">El serializador de OData incluye este vínculo cuando serializa la entidad.</span><span class="sxs-lookup"><span data-stu-id="efa0a-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="efa0a-197">Cuando la acción no está disponible, la función devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="efa0a-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="efa0a-198">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="efa0a-198">Additional Resources</span></span>

[<span data-ttu-id="efa0a-199">Ejemplo de acciones de OData</span><span class="sxs-lookup"><span data-stu-id="efa0a-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
