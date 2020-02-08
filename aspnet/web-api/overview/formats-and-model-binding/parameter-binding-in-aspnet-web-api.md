---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Enlace de parámetros en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe el modo en que la API Web enlaza los parámetros y cómo personalizar el proceso de enlace en ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074909"
---
# <a name="parameter-binding-in-aspnet-web-api"></a><span data-ttu-id="ab9b3-103">Enlace de parámetros en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ab9b3-103">Parameter Binding in ASP.NET Web API</span></span>

<span data-ttu-id="ab9b3-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ab9b3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="ab9b3-105">En este artículo se describe cómo la API Web enlaza parámetros y cómo se puede personalizar el proceso de enlace.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-105">This article describes how Web API binds parameters, and how you can customize the binding process.</span></span> <span data-ttu-id="ab9b3-106">Cuando la API Web llama a un método en un controlador, debe establecer valores para los parámetros, un proceso llamado *enlace*.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-106">When Web API calls a method on a controller, it must set values for the parameters, a process called *binding*.</span></span>

<span data-ttu-id="ab9b3-107">De forma predeterminada, Web API usa las siguientes reglas para enlazar parámetros:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-107">By default, Web API uses the following rules to bind parameters:</span></span>

- <span data-ttu-id="ab9b3-108">Si el parámetro es un tipo "simple", la API Web intenta obtener el valor del URI.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-108">If the parameter is a "simple" type, Web API tries to get the value from the URI.</span></span> <span data-ttu-id="ab9b3-109">Los tipos simples incluyen los [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) de .net (**int**, **bool**, **Double**, etc.), más **TimeSpan**, **DateTime**, **GUID**, **decimal**y **String**, *además* de cualquier tipo con un convertidor de tipos que puede convertir de una cadena.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-109">Simple types include the .NET [primitive types](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **double**, and so forth), plus **TimeSpan**, **DateTime**, **Guid**, **decimal**, and **string**, *plus* any type with a type converter that can convert from a string.</span></span> <span data-ttu-id="ab9b3-110">(Más información sobre los convertidores de tipos más adelante).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-110">(More about type converters later.)</span></span>
- <span data-ttu-id="ab9b3-111">En el caso de los tipos complejos, la API Web intenta leer el valor del cuerpo del mensaje mediante un [formateador de tipo de medio](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-111">For complex types, Web API tries to read the value from the message body, using a [media-type formatter](media-formatters.md).</span></span>

<span data-ttu-id="ab9b3-112">Por ejemplo, a continuación se muestra un método de controlador de API Web típico:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-112">For example, here is a typical Web API controller method:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="ab9b3-113">El parámetro *ID* es un &quot;tipo de&quot; simple, por lo que la API Web intenta obtener el valor del URI de solicitud.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-113">The *id* parameter is a &quot;simple&quot; type, so Web API tries to get the value from the request URI.</span></span> <span data-ttu-id="ab9b3-114">El parámetro *Item* es un tipo complejo, por lo que la API Web usa un formateador de tipo de medio para leer el valor del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-114">The *item* parameter is a complex type, so Web API uses a media-type formatter to read the value from the request body.</span></span>

<span data-ttu-id="ab9b3-115">Para obtener un valor del URI, la API Web busca en los datos de ruta y en la cadena de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-115">To get a value from the URI, Web API looks in the route data and the URI query string.</span></span> <span data-ttu-id="ab9b3-116">Los datos de ruta se rellenan cuando el sistema de enrutamiento analiza el URI y lo hace coincidir con una ruta.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-116">The route data is populated when the routing system parses the URI and matches it to a route.</span></span> <span data-ttu-id="ab9b3-117">Para obtener más información, consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-117">For more information, see [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span>

<span data-ttu-id="ab9b3-118">En el resto de este artículo, mostraré cómo puede personalizar el proceso de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-118">In the rest of this article, I'll show how you can customize the model binding process.</span></span> <span data-ttu-id="ab9b3-119">Sin embargo, para los tipos complejos, considere la posibilidad de usar formateadores de tipo de medio siempre que sea posible.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-119">For complex types, however, consider using media-type formatters whenever possible.</span></span> <span data-ttu-id="ab9b3-120">Un principio clave de HTTP es que los recursos se envían en el cuerpo del mensaje, mediante la negociación de contenido para especificar la representación del recurso.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-120">A key principle of HTTP is that resources are sent in the message body, using content negotiation to specify the representation of the resource.</span></span> <span data-ttu-id="ab9b3-121">Los formateadores de tipo de medio se diseñaron para este fin exactamente.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-121">Media-type formatters were designed for exactly this purpose.</span></span>

## <a name="using-fromuri"></a><span data-ttu-id="ab9b3-122">Usar [FromUri]</span><span class="sxs-lookup"><span data-stu-id="ab9b3-122">Using [FromUri]</span></span>

<span data-ttu-id="ab9b3-123">Para forzar a la API Web a leer un tipo complejo del URI, agregue el atributo **[FromUri]** al parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-123">To force Web API to read a complex type from the URI, add the **[FromUri]** attribute to the parameter.</span></span> <span data-ttu-id="ab9b3-124">En el ejemplo siguiente se define un tipo de `GeoPoint`, junto con un método de controlador que obtiene el `GeoPoint` del URI.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-124">The following example defines a `GeoPoint` type, along with a controller method that gets the `GeoPoint` from the URI.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="ab9b3-125">El cliente puede poner los valores de latitud y longitud en la cadena de consulta y la API Web los usará para construir un `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-125">The client can put the Latitude and Longitude values in the query string and Web API will use them to construct a `GeoPoint`.</span></span> <span data-ttu-id="ab9b3-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-126">For example:</span></span>

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a><span data-ttu-id="ab9b3-127">Usar [FromBody]</span><span class="sxs-lookup"><span data-stu-id="ab9b3-127">Using [FromBody]</span></span>

<span data-ttu-id="ab9b3-128">Para forzar a la API Web a leer un tipo simple del cuerpo de la solicitud, agregue el atributo **[FromBody]** al parámetro:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-128">To force Web API to read a simple type from the request body, add the **[FromBody]** attribute to the parameter:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="ab9b3-129">En este ejemplo, Web API usará un formateador de tipo de medio para leer el valor del *nombre* del cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-129">In this example, Web API will use a media-type formatter to read the value of *name* from the request body.</span></span> <span data-ttu-id="ab9b3-130">Este es un ejemplo de solicitud de cliente.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-130">Here is an example client request.</span></span>

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

<span data-ttu-id="ab9b3-131">Cuando un parámetro tiene [FromBody], la API Web usa el encabezado Content-Type para seleccionar un formateador.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-131">When a parameter has [FromBody], Web API uses the Content-Type header to select a formatter.</span></span> <span data-ttu-id="ab9b3-132">En este ejemplo, el tipo de contenido es &quot;aplicación/&quot; JSON y el cuerpo de la solicitud es una cadena JSON sin formato (no un objeto JSON).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-132">In this example, the content type is &quot;application/json&quot; and the request body is a raw JSON string (not a JSON object).</span></span>

<span data-ttu-id="ab9b3-133">A lo sumo, se permite que un parámetro lea desde el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-133">At most one parameter is allowed to read from the message body.</span></span> <span data-ttu-id="ab9b3-134">Por lo tanto, esto no funcionará:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-134">So this will not work:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="ab9b3-135">La razón de esta regla es que el cuerpo de la solicitud puede almacenarse en un flujo no almacenado en búfer que solo se puede leer una vez.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-135">The reason for this rule is that the request body might be stored in a non-buffered stream that can only be read once.</span></span>

## <a name="type-converters"></a><span data-ttu-id="ab9b3-136">Convertidores de tipos</span><span class="sxs-lookup"><span data-stu-id="ab9b3-136">Type Converters</span></span>

<span data-ttu-id="ab9b3-137">Puede hacer que Web API trate una clase como un tipo simple (para que la API Web intente enlazarla desde el URI) mediante la creación de un **TypeConverter** y la conversión de una cadena.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-137">You can make Web API treat a class as a simple type (so that Web API will try to bind it from the URI) by creating a **TypeConverter** and providing a string conversion.</span></span>

<span data-ttu-id="ab9b3-138">En el código siguiente se muestra una `GeoPoint` clase que representa un punto geográfico, más un **TypeConverter** que convierte cadenas en instancias de `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-138">The following code shows a `GeoPoint` class that represents a geographical point, plus a **TypeConverter** that converts from strings to `GeoPoint` instances.</span></span> <span data-ttu-id="ab9b3-139">La clase `GeoPoint` está decorada con un atributo **[TypeConverter]** para especificar el convertidor de tipos.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-139">The `GeoPoint` class is decorated with a **[TypeConverter]** attribute to specify the type converter.</span></span> <span data-ttu-id="ab9b3-140">(Este ejemplo fue inspirado en la entrada de blog de Mike retrete [cómo enlazar a objetos personalizados en firmas de acción en MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-140">(This example was inspired by Mike Stall's blog post [How to bind to custom objects in action signatures in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="ab9b3-141">Ahora, la API web tratará `GeoPoint` como un tipo simple, lo que significa que intentará enlazar parámetros de `GeoPoint` desde el URI.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-141">Now Web API will treat `GeoPoint` as a simple type, meaning it will try to bind `GeoPoint` parameters from the URI.</span></span> <span data-ttu-id="ab9b3-142">No es necesario incluir **[FromUri]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-142">You don't need to include **[FromUri]** on the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="ab9b3-143">El cliente puede invocar el método con un URI como este:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-143">The client can invoke the method with a URI like this:</span></span>

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a><span data-ttu-id="ab9b3-144">Enlazadores modelo</span><span class="sxs-lookup"><span data-stu-id="ab9b3-144">Model Binders</span></span>

<span data-ttu-id="ab9b3-145">Una opción más flexible que un convertidor de tipos es crear un enlazador de modelos personalizado.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-145">A more flexible option than a type converter is to create a custom model binder.</span></span> <span data-ttu-id="ab9b3-146">Con un enlazador de modelos, tiene acceso a aspectos como la solicitud HTTP, la descripción de la acción y los valores sin formato de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-146">With a model binder, you have access to things like the HTTP request, the action description, and the raw values from the route data.</span></span>

<span data-ttu-id="ab9b3-147">Para crear un enlazador de modelos, implemente la interfaz **IModelBinder** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-147">To create a model binder, implement the **IModelBinder** interface.</span></span> <span data-ttu-id="ab9b3-148">Esta interfaz define un método único, **BindModel**:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-148">This interface defines a single method, **BindModel**:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

<span data-ttu-id="ab9b3-149">Este es un enlazador de modelos para objetos de `GeoPoint`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-149">Here is a model binder for `GeoPoint` objects.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="ab9b3-150">Un enlazador de modelos obtiene los valores de entrada sin formato de un *proveedor de valores*.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-150">A model binder gets raw input values from a *value provider*.</span></span> <span data-ttu-id="ab9b3-151">Este diseño separa dos funciones distintas:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-151">This design separates two distinct functions:</span></span>

- <span data-ttu-id="ab9b3-152">El proveedor de valores toma la solicitud HTTP y rellena un diccionario de pares clave-valor.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-152">The value provider takes the HTTP request and populates a dictionary of key-value pairs.</span></span>
- <span data-ttu-id="ab9b3-153">El enlazador de modelos usa este diccionario para rellenar el modelo.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-153">The model binder uses this dictionary to populate the model.</span></span>

<span data-ttu-id="ab9b3-154">El proveedor de valor predeterminado en Web API obtiene los valores de los datos de ruta y la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-154">The default value provider in Web API gets values from the route data and the query string.</span></span> <span data-ttu-id="ab9b3-155">Por ejemplo, si el URI es `http://localhost/api/values/1?location=48,-122`, el proveedor de valores crea los siguientes pares de clave-valor:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-155">For example, if the URI is `http://localhost/api/values/1?location=48,-122`, the value provider creates the following key-value pairs:</span></span>

- <span data-ttu-id="ab9b3-156">ID = &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="ab9b3-156">id = &quot;1&quot;</span></span>
- <span data-ttu-id="ab9b3-157">Location = &quot;48.122&quot;</span><span class="sxs-lookup"><span data-stu-id="ab9b3-157">location = &quot;48,122&quot;</span></span>

<span data-ttu-id="ab9b3-158">(Supongamos que la plantilla de ruta predeterminada, que es &quot;API/{Controller}/{ID}&quot;).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-158">(I'm assuming the default route template, which is &quot;api/{controller}/{id}&quot;.)</span></span>

<span data-ttu-id="ab9b3-159">El nombre del parámetro que se va a enlazar se almacena en la propiedad **ModelBindingContext. modelname** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-159">The name of the parameter to bind is stored in the **ModelBindingContext.ModelName** property.</span></span> <span data-ttu-id="ab9b3-160">El enlazador de modelos busca una clave con este valor en el diccionario.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-160">The model binder looks for a key with this value in the dictionary.</span></span> <span data-ttu-id="ab9b3-161">Si el valor existe y se puede convertir en un `GeoPoint`, el enlazador de modelos asigna el valor enlazado a la propiedad **ModelBindingContext. Model** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-161">If the value exists and can be converted into a `GeoPoint`, the model binder assigns the bound value to the **ModelBindingContext.Model** property.</span></span>

<span data-ttu-id="ab9b3-162">Observe que el enlazador de modelos no se limita a una conversión de tipo simple.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-162">Notice that the model binder is not limited to a simple type conversion.</span></span> <span data-ttu-id="ab9b3-163">En este ejemplo, el enlazador de modelos busca primero en una tabla de ubicaciones conocidas y, si se produce un error, usa la conversión de tipos.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-163">In this example, the model binder first looks in a table of known locations, and if that fails, it uses type conversion.</span></span>

<span data-ttu-id="ab9b3-164">**Establecer el enlazador de modelos**</span><span class="sxs-lookup"><span data-stu-id="ab9b3-164">**Setting the Model Binder**</span></span>

<span data-ttu-id="ab9b3-165">Hay varias maneras de establecer un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-165">There are several ways to set a model binder.</span></span> <span data-ttu-id="ab9b3-166">En primer lugar, puede Agregar un atributo **[ModelBinder]** al parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-166">First, you can add a **[ModelBinder]** attribute to the parameter.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

<span data-ttu-id="ab9b3-167">También puede Agregar un atributo **[ModelBinder]** al tipo.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-167">You can also add a **[ModelBinder]** attribute to the type.</span></span> <span data-ttu-id="ab9b3-168">Web API usará el enlazador de modelos especificado para todos los parámetros de ese tipo.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-168">Web API will use the specified model binder for all parameters of that type.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="ab9b3-169">Por último, puede Agregar un proveedor de enlazador de modelos a **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-169">Finally, you can add a model-binder provider to the **HttpConfiguration**.</span></span> <span data-ttu-id="ab9b3-170">Un proveedor de enlazador de modelos es simplemente una clase de generador que crea un enlazador de modelos.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-170">A model-binder provider is simply a factory class that creates a model binder.</span></span> <span data-ttu-id="ab9b3-171">Puede crear un proveedor derivando de la clase [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-171">You can create a provider by deriving from the [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) class.</span></span> <span data-ttu-id="ab9b3-172">Sin embargo, si el enlazador de modelos controla un solo tipo, es más fácil usar el **SimpleModelBinderProvider**integrado, que está diseñado para este fin.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-172">However, if your model binder handles a single type, it's easier to use the built-in **SimpleModelBinderProvider**, which is designed for this purpose.</span></span> <span data-ttu-id="ab9b3-173">El código siguiente muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-173">The following code shows how to do this.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

<span data-ttu-id="ab9b3-174">Con un proveedor de enlace de modelos, todavía necesita agregar el atributo **[ModelBinder]** al parámetro para indicar a la API Web que debe usar un enlazador de modelos y no un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-174">With a model-binding provider, you still need to add the **[ModelBinder]** attribute to the parameter, to tell Web API that it should use a model binder and not a media-type formatter.</span></span> <span data-ttu-id="ab9b3-175">Pero ahora no es necesario especificar el tipo de enlazador de modelos en el atributo:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-175">But now you don't need to specify the type of model binder in the attribute:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a><span data-ttu-id="ab9b3-176">Proveedores de valores</span><span class="sxs-lookup"><span data-stu-id="ab9b3-176">Value Providers</span></span>

<span data-ttu-id="ab9b3-177">He mencionado que un enlazador de modelos obtiene valores de un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-177">I mentioned that a model binder gets values from a value provider.</span></span> <span data-ttu-id="ab9b3-178">Para escribir un proveedor de valores personalizado, implemente la interfaz **IValueProvider** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-178">To write a custom value provider, implement the **IValueProvider** interface.</span></span> <span data-ttu-id="ab9b3-179">Este es un ejemplo que extrae los valores de las cookies de la solicitud:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-179">Here is an example that pulls values from the cookies in the request:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

<span data-ttu-id="ab9b3-180">También debe crear un generador de proveedores de valores derivando de la clase **ValueProviderFactory** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-180">You also need to create a value provider factory by deriving from the **ValueProviderFactory** class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

<span data-ttu-id="ab9b3-181">Agregue el generador de proveedores de valores a **HttpConfiguration** como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-181">Add the value provider factory to the **HttpConfiguration** as follows.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

<span data-ttu-id="ab9b3-182">La API Web crea todos los proveedores de valores, por lo que cuando un enlazador de modelos llama a **ValueProvider. GetValue**, el enlazador de modelos recibe el valor del primer proveedor de valor que puede generarlo.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-182">Web API composes all of the value providers, so when a model binder calls **ValueProvider.GetValue**, the model binder receives the value from the first value provider that is able to produce it.</span></span>

<span data-ttu-id="ab9b3-183">Como alternativa, puede establecer el generador de proveedores de valores en el nivel de parámetro mediante el atributo **ValueProvider** , como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-183">Alternatively, you can set the value provider factory at the parameter level by using the **ValueProvider** attribute, as follows:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

<span data-ttu-id="ab9b3-184">Esto indica a la API Web que use el enlace de modelos con el generador de proveedores de valores especificado y que no use ninguno de los demás proveedores de valores registrados.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-184">This tells Web API to use model binding with the specified value provider factory, and not to use any of the other registered value providers.</span></span>

## <a name="httpparameterbinding"></a><span data-ttu-id="ab9b3-185">HttpParameterBinding</span><span class="sxs-lookup"><span data-stu-id="ab9b3-185">HttpParameterBinding</span></span>

<span data-ttu-id="ab9b3-186">Los enlazadores de modelos son una instancia específica de un mecanismo más general.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-186">Model binders are a specific instance of a more general mechanism.</span></span> <span data-ttu-id="ab9b3-187">Si observa el atributo **[ModelBinder]** , verá que se deriva de la clase abstracta **ParameterBindingAttribute** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-187">If you look at the **[ModelBinder]** attribute, you will see that it derives from the abstract **ParameterBindingAttribute** class.</span></span> <span data-ttu-id="ab9b3-188">Esta clase define un método único, **GetBinding**, que devuelve un objeto **HttpParameterBinding** :</span><span class="sxs-lookup"><span data-stu-id="ab9b3-188">This class defines a single method, **GetBinding**, which returns an **HttpParameterBinding** object:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

<span data-ttu-id="ab9b3-189">Un **HttpParameterBinding** es responsable de enlazar un parámetro a un valor.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-189">An **HttpParameterBinding** is responsible for binding a parameter to a value.</span></span> <span data-ttu-id="ab9b3-190">En el caso de **[ModelBinder]** , el atributo devuelve una implementación de **HttpParameterBinding** que usa un **IModelBinder** para realizar el enlace real.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-190">In the case of **[ModelBinder]**, the attribute returns an **HttpParameterBinding** implementation that uses an **IModelBinder** to perform the actual binding.</span></span> <span data-ttu-id="ab9b3-191">También puede implementar su propio **HttpParameterBinding**.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-191">You can also implement your own **HttpParameterBinding**.</span></span>

<span data-ttu-id="ab9b3-192">Por ejemplo, supongamos que desea obtener ETags de los encabezados `if-match` y `if-none-match` de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-192">For example, suppose you want to get ETags from `if-match` and `if-none-match` headers in the request.</span></span> <span data-ttu-id="ab9b3-193">Comenzaremos definiendo una clase para representar ETags.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-193">We'll start by defining a class to represent ETags.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

<span data-ttu-id="ab9b3-194">También definiremos una enumeración para indicar si se va a obtener el valor ETag del encabezado `if-match` o del encabezado `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-194">We'll also define an enumeration to indicate whether to get the ETag from the `if-match` header or the `if-none-match` header.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

<span data-ttu-id="ab9b3-195">A continuación se muestra un **HttpParameterBinding** que obtiene el ETag del encabezado deseado y lo enlaza a un parámetro de tipo ETag:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-195">Here is an **HttpParameterBinding** that gets the ETag from the desired header and binds it to a parameter of type ETag:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

<span data-ttu-id="ab9b3-196">El método **ExecuteBindingAsync** realiza el enlace.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-196">The **ExecuteBindingAsync** method does the binding.</span></span> <span data-ttu-id="ab9b3-197">Dentro de este método, agregue el valor del parámetro enlazado al diccionario **ActionArgument** en **HttpActionContext**.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-197">Within this method, add the bound parameter value to the **ActionArgument** dictionary in the **HttpActionContext**.</span></span>

> [!NOTE]
> <span data-ttu-id="ab9b3-198">Si el método **ExecuteBindingAsync** lee el cuerpo del mensaje de solicitud, invalide la propiedad **WillReadBody** para que devuelva true.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-198">If your **ExecuteBindingAsync** method reads the body of the request message, override the **WillReadBody** property to return true.</span></span> <span data-ttu-id="ab9b3-199">El cuerpo de la solicitud puede ser un flujo no almacenado en búfer que solo se puede leer una vez, por lo que la API Web exige una regla que, como máximo, un enlace puede leer el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-199">The request body might be an unbuffered stream that can only be read once, so Web API enforces a rule that at most one binding can read the message body.</span></span>

<span data-ttu-id="ab9b3-200">Para aplicar un **HttpParameterBinding**personalizado, puede definir un atributo que se derive de **ParameterBindingAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-200">To apply a custom **HttpParameterBinding**, you can define an attribute that derives from **ParameterBindingAttribute**.</span></span> <span data-ttu-id="ab9b3-201">Por `ETagParameterBinding`, definiremos dos atributos, uno para `if-match` encabezados y otro para los encabezados de `if-none-match`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-201">For `ETagParameterBinding`, we'll define two attributes, one for `if-match` headers and one for `if-none-match` headers.</span></span> <span data-ttu-id="ab9b3-202">Ambos derivan de una clase base abstracta.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-202">Both derive from an abstract base class.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

<span data-ttu-id="ab9b3-203">A continuación se muestra un método de controlador que utiliza el atributo `[IfNoneMatch]`.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-203">Here is a controller method that uses the `[IfNoneMatch]` attribute.</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

<span data-ttu-id="ab9b3-204">Además de **ParameterBindingAttribute**, hay otro enlace para agregar un **HttpParameterBinding**personalizado.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-204">Besides **ParameterBindingAttribute**, there is another hook for adding a custom **HttpParameterBinding**.</span></span> <span data-ttu-id="ab9b3-205">En el objeto **HttpConfiguration** , la propiedad **ParameterBindingRules** es una colección de funciones anónimas de tipo (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span><span class="sxs-lookup"><span data-stu-id="ab9b3-205">On the **HttpConfiguration** object, the **ParameterBindingRules** property is a collection of anonymous functions of type (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**).</span></span> <span data-ttu-id="ab9b3-206">Por ejemplo, puede Agregar una regla que cualquier parámetro ETag en un método GET use `ETagParameterBinding` con `if-none-match`:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-206">For example, you could add a rule that any ETag parameter on a GET method uses `ETagParameterBinding` with `if-none-match`:</span></span>

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

<span data-ttu-id="ab9b3-207">La función debe devolver `null` para los parámetros en los que el enlace no es aplicable.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-207">The function should return `null` for parameters where the binding is not applicable.</span></span>

## <a name="iactionvaluebinder"></a><span data-ttu-id="ab9b3-208">IActionValueBinder</span><span class="sxs-lookup"><span data-stu-id="ab9b3-208">IActionValueBinder</span></span>

<span data-ttu-id="ab9b3-209">El proceso de enlace de parámetros completo se controla mediante un servicio conectable, **IActionValueBinder**.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-209">The entire parameter-binding process is controlled by a pluggable service, **IActionValueBinder**.</span></span> <span data-ttu-id="ab9b3-210">La implementación predeterminada de **IActionValueBinder** hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-210">The default implementation of **IActionValueBinder** does the following:</span></span>

1. <span data-ttu-id="ab9b3-211">Busque un **ParameterBindingAttribute** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-211">Look for a **ParameterBindingAttribute** on the parameter.</span></span> <span data-ttu-id="ab9b3-212">Esto incluye los atributos personalizados **[FromBody]** , **[FromUri]** y **[ModelBinder]** .</span><span class="sxs-lookup"><span data-stu-id="ab9b3-212">This includes **[FromBody]**, **[FromUri]**, and **[ModelBinder]**, or custom attributes.</span></span>
2. <span data-ttu-id="ab9b3-213">De lo contrario, busque en **HttpConfiguration. ParameterBindingRules** una función que devuelva un **HttpParameterBinding**que no sea NULL.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-213">Otherwise, look in **HttpConfiguration.ParameterBindingRules** for a function that returns a non-null **HttpParameterBinding**.</span></span>
3. <span data-ttu-id="ab9b3-214">De lo contrario, use las reglas predeterminadas descritas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-214">Otherwise, use the default rules that I described previously.</span></span> 

    - <span data-ttu-id="ab9b3-215">Si el tipo de parámetro es "simple" o tiene un convertidor de tipos, enlace desde el URI.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-215">If the parameter type is "simple"or has a type converter, bind from the URI.</span></span> <span data-ttu-id="ab9b3-216">Esto es equivalente a poner el atributo **[FromUri]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-216">This is equivalent to putting the **[FromUri]** attribute on the parameter.</span></span>
    - <span data-ttu-id="ab9b3-217">De lo contrario, intente leer el parámetro desde el cuerpo del mensaje.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-217">Otherwise, try to read the parameter from the message body.</span></span> <span data-ttu-id="ab9b3-218">Esto es equivalente a poner **[FromBody]** en el parámetro.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-218">This is equivalent to putting **[FromBody]** on the parameter.</span></span>

<span data-ttu-id="ab9b3-219">Si lo desea, puede reemplazar todo el servicio **IActionValueBinder** por una implementación personalizada.</span><span class="sxs-lookup"><span data-stu-id="ab9b3-219">If you wanted, you could replace the entire **IActionValueBinder** service with a custom implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab9b3-220">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ab9b3-220">Additional Resources</span></span>

[<span data-ttu-id="ab9b3-221">Ejemplo de enlace de parámetros personalizados</span><span class="sxs-lookup"><span data-stu-id="ab9b3-221">Custom Parameter Binding Sample</span></span>](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

<span data-ttu-id="ab9b3-222">Mike retrete escribió una buena serie de entradas de blog sobre el enlace de parámetros de la API Web:</span><span class="sxs-lookup"><span data-stu-id="ab9b3-222">Mike Stall wrote a good series of blog posts about Web API parameter binding:</span></span>

- [<span data-ttu-id="ab9b3-223">Cómo la API Web realiza el enlace de parámetros</span><span class="sxs-lookup"><span data-stu-id="ab9b3-223">How Web API does Parameter Binding</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [<span data-ttu-id="ab9b3-224">Enlace de parámetros de estilo de MVC para Web API</span><span class="sxs-lookup"><span data-stu-id="ab9b3-224">MVC Style parameter binding for Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [<span data-ttu-id="ab9b3-225">Cómo enlazar a objetos personalizados en firmas de acción en MVC/API Web</span><span class="sxs-lookup"><span data-stu-id="ab9b3-225">How to bind to custom objects in action signatures in MVC/Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [<span data-ttu-id="ab9b3-226">Creación de un proveedor de valores personalizado en Web API</span><span class="sxs-lookup"><span data-stu-id="ab9b3-226">How to create a custom value provider in Web API</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [<span data-ttu-id="ab9b3-227">Enlace de parámetros de la API Web en el capó</span><span class="sxs-lookup"><span data-stu-id="ab9b3-227">Web API Parameter binding under the hood</span></span>](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
