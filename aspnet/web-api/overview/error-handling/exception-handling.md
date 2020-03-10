---
uid: web-api/overview/error-handling/exception-handling
title: Control de excepciones en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504703"
---
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="3df40-102">Control de excepciones en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="3df40-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="3df40-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3df40-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3df40-104">En este artículo se describe el control de errores y excepciones en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="3df40-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="3df40-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="3df40-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="3df40-106">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="3df40-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="3df40-107">Registrar filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="3df40-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="3df40-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="3df40-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="3df40-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="3df40-109">HttpResponseException</span></span>

<span data-ttu-id="3df40-110">¿Qué ocurre si un controlador de API Web produce una excepción no detectada?</span><span class="sxs-lookup"><span data-stu-id="3df40-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="3df40-111">De forma predeterminada, la mayoría de las excepciones se traducen en una respuesta HTTP con el código de estado 500, error interno del servidor.</span><span class="sxs-lookup"><span data-stu-id="3df40-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="3df40-112">El tipo **HttpResponseException** es un caso especial.</span><span class="sxs-lookup"><span data-stu-id="3df40-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="3df40-113">Esta excepción devuelve cualquier código de Estado HTTP que especifique en el constructor de la excepción.</span><span class="sxs-lookup"><span data-stu-id="3df40-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="3df40-114">Por ejemplo, el método siguiente devuelve 404, no encontrado, si el parámetro *ID* no es válido.</span><span class="sxs-lookup"><span data-stu-id="3df40-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="3df40-115">Para obtener más control sobre la respuesta, también puede crear el mensaje de respuesta completo e incluirlo con el **HttpResponseException:**</span><span class="sxs-lookup"><span data-stu-id="3df40-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="3df40-116">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="3df40-116">Exception Filters</span></span>

<span data-ttu-id="3df40-117">Puede personalizar el modo en que la API Web controla las excepciones escribiendo un *filtro de excepciones*.</span><span class="sxs-lookup"><span data-stu-id="3df40-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="3df40-118">Se ejecuta un filtro de excepción cuando un método de controlador produce una excepción no controlada que *no* es una excepción **HttpResponseException** .</span><span class="sxs-lookup"><span data-stu-id="3df40-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="3df40-119">El tipo **HttpResponseException** es un caso especial, ya que está diseñado específicamente para devolver una respuesta http.</span><span class="sxs-lookup"><span data-stu-id="3df40-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="3df40-120">Los filtros de excepción implementan la interfaz **System. Web. http. filters. IExceptionFilter** .</span><span class="sxs-lookup"><span data-stu-id="3df40-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="3df40-121">La manera más sencilla de escribir un filtro de excepción es derivar de la clase **System. Web. http. filters. ExceptionFilterAttribute** e invalidar **el método de excepción.**</span><span class="sxs-lookup"><span data-stu-id="3df40-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="3df40-122">Los filtros de excepción en ASP.NET Web API son similares a los de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3df40-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="3df40-123">Sin embargo, se declaran en un espacio de nombres independiente y una función por separado.</span><span class="sxs-lookup"><span data-stu-id="3df40-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="3df40-124">En concreto, la clase **HandleErrorAttribute** que se usa en MVC no controla las excepciones iniciadas por los controladores de API Web.</span><span class="sxs-lookup"><span data-stu-id="3df40-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="3df40-125">Este es un filtro que convierte las excepciones **NotImplementedException** en el código de estado http 501, no implementado:</span><span class="sxs-lookup"><span data-stu-id="3df40-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="3df40-126">La propiedad **Response** del objeto **HttpActionExecutedContext** contiene el mensaje de respuesta HTTP que se enviará al cliente.</span><span class="sxs-lookup"><span data-stu-id="3df40-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="3df40-127">Registrar filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="3df40-127">Registering Exception Filters</span></span>

<span data-ttu-id="3df40-128">Hay varias formas de registrar un filtro de excepción API web:</span><span class="sxs-lookup"><span data-stu-id="3df40-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="3df40-129">Por acción</span><span class="sxs-lookup"><span data-stu-id="3df40-129">By action</span></span>
- <span data-ttu-id="3df40-130">Por controlador</span><span class="sxs-lookup"><span data-stu-id="3df40-130">By controller</span></span>
- <span data-ttu-id="3df40-131">Globalmente</span><span class="sxs-lookup"><span data-stu-id="3df40-131">Globally</span></span>

<span data-ttu-id="3df40-132">Para aplicar el filtro a una acción concreta, agregue el filtro como un atributo a la acción:</span><span class="sxs-lookup"><span data-stu-id="3df40-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="3df40-133">Para aplicar el filtro a todas las acciones de un controlador, agregue el filtro como un atributo a la clase de controlador:</span><span class="sxs-lookup"><span data-stu-id="3df40-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="3df40-134">Para aplicar el filtro globalmente a todos los controladores de la API Web, agregue una instancia del filtro a la colección **GlobalConfiguration. Configuration. filters** .</span><span class="sxs-lookup"><span data-stu-id="3df40-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="3df40-135">Los filtros de excepción de esta colección se aplican a cualquier acción de controlador de API web.</span><span class="sxs-lookup"><span data-stu-id="3df40-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="3df40-136">Si usa la plantilla de proyecto "aplicación web MVC 4 de ASP.NET" para crear el proyecto, coloque el código de configuración de la API Web dentro de la clase `WebApiConfig`, que se encuentra en la carpeta app\_Inicio:</span><span class="sxs-lookup"><span data-stu-id="3df40-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="3df40-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="3df40-137">HttpError</span></span>

<span data-ttu-id="3df40-138">El objeto **HttpError** proporciona una manera coherente de devolver información de error en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="3df40-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="3df40-139">En el ejemplo siguiente se muestra cómo devolver el código de Estado HTTP 404 (no encontrado) con un **HttpError** en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="3df40-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="3df40-140">**CreateErrorResponse** es un método de extensión definido en la clase **System .net. http. HttpRequestMessageExtensions** .</span><span class="sxs-lookup"><span data-stu-id="3df40-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="3df40-141">Internamente, **CreateErrorResponse** crea una instancia de **HttpError** y, a continuación, crea un **HttpResponseMessage** que contiene **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="3df40-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="3df40-142">En este ejemplo, si el método se realiza correctamente, devuelve el producto en la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3df40-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="3df40-143">Pero si no se encuentra el producto solicitado, la respuesta HTTP contiene un **HttpError** en el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3df40-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="3df40-144">La respuesta podría ser similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="3df40-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="3df40-145">Observe que **HttpError** se serializó a JSON en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="3df40-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="3df40-146">Una ventaja de usar **HttpError** es que pasa por el mismo proceso [de](../formats-and-model-binding/content-negotiation.md) serialización y negociación de contenido que cualquier otro modelo con establecimiento inflexible de tipos.</span><span class="sxs-lookup"><span data-stu-id="3df40-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="3df40-147">HttpError y validación del modelo</span><span class="sxs-lookup"><span data-stu-id="3df40-147">HttpError and Model Validation</span></span>

<span data-ttu-id="3df40-148">Para la validación del modelo, puede pasar el estado del modelo a **CreateErrorResponse**para incluir los errores de validación en la respuesta:</span><span class="sxs-lookup"><span data-stu-id="3df40-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="3df40-149">Este ejemplo puede devolver la siguiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="3df40-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="3df40-150">Para obtener más información sobre la validación de modelos, vea [validación de modelos en ASP.net web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3df40-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="3df40-151">Uso de HttpError con HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="3df40-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="3df40-152">En los ejemplos anteriores se devuelve un mensaje **HttpResponseMessage** desde la acción del controlador, pero también puede usar **HttpResponseException** para devolver una **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="3df40-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="3df40-153">Esto le permite devolver un modelo fuertemente tipado en el caso de éxito normal y, al mismo tiempo, devolver **HttpError** si se produce un error:</span><span class="sxs-lookup"><span data-stu-id="3df40-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
