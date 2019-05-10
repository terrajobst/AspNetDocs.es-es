---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Modelo de validación en ASP.NET Web API - ASP.NET 4.x
author: MikeWasson
description: Información general sobre la validación de modelos en ASP.NET Web API para ASP.NET 4.x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112825"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="b55a2-103">Validación de modelos en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b55a2-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="b55a2-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b55a2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b55a2-105">En este artículo se muestra cómo anotar los modelos, utilice las anotaciones para la validación de datos y controlar los errores de validación de la API web.</span><span class="sxs-lookup"><span data-stu-id="b55a2-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="b55a2-106">Cuando un cliente envía datos a la API web, a menudo desea validar los datos antes de realizar cualquier procesamiento.</span><span class="sxs-lookup"><span data-stu-id="b55a2-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="b55a2-107">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="b55a2-107">Data Annotations</span></span>

<span data-ttu-id="b55a2-108">En ASP.NET Web API, puede usar los atributos de la [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) espacio de nombres para establecer reglas de validación para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="b55a2-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="b55a2-109">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b55a2-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="b55a2-110">Si ha usado la validación del modelo en ASP.NET MVC, debería resultar familiar.</span><span class="sxs-lookup"><span data-stu-id="b55a2-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="b55a2-111">El **requiere** atributos indica que el `Name` propiedad no debe ser null.</span><span class="sxs-lookup"><span data-stu-id="b55a2-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="b55a2-112">El **intervalo** atributo dice que `Weight` debe estar comprendido entre 0 y 999.</span><span class="sxs-lookup"><span data-stu-id="b55a2-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="b55a2-113">Suponga que un cliente envía una solicitud POST con la siguiente representación JSON:</span><span class="sxs-lookup"><span data-stu-id="b55a2-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="b55a2-114">Puede ver que el cliente no incluía el `Name` propiedad, que está marcada como necesaria.</span><span class="sxs-lookup"><span data-stu-id="b55a2-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="b55a2-115">Cuando la API Web convierte de JSON en un `Product` instancia, valida el `Product` frente a los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="b55a2-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="b55a2-116">En la acción del controlador, puede comprobar si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="b55a2-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="b55a2-117">Validación del modelo no garantiza que los datos del cliente están seguros.</span><span class="sxs-lookup"><span data-stu-id="b55a2-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="b55a2-118">Puede ser necesaria una validación adicional en otros niveles de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b55a2-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="b55a2-119">(Por ejemplo, la capa de datos podría aplicar restricciones de clave externa). El tutorial [usar Web API con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora algunos de estos problemas.</span><span class="sxs-lookup"><span data-stu-id="b55a2-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="b55a2-120">**"Debajo del registro"**: Debajo del registro se produce cuando el cliente deja fuera algunas propiedades.</span><span class="sxs-lookup"><span data-stu-id="b55a2-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="b55a2-121">Por ejemplo, suponga que el cliente envía la siguiente:</span><span class="sxs-lookup"><span data-stu-id="b55a2-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="b55a2-122">En este caso, el cliente no especificó valores para `Price` o `Weight`.</span><span class="sxs-lookup"><span data-stu-id="b55a2-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="b55a2-123">El formateador JSON asigna un valor predeterminado de cero para las propiedades que faltan.</span><span class="sxs-lookup"><span data-stu-id="b55a2-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="b55a2-124">El estado del modelo es válido, porque cero es un valor válido para estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="b55a2-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="b55a2-125">Si se trata de un problema depende de su escenario.</span><span class="sxs-lookup"><span data-stu-id="b55a2-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="b55a2-126">Por ejemplo, en una operación de actualización, es posible que desee distinguir entre "cero" y "sin establecer".</span><span class="sxs-lookup"><span data-stu-id="b55a2-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="b55a2-127">Para obligar a los clientes para establecer un valor, que la propiedad que acepta valores NULL y establezca el **necesario** atributo:</span><span class="sxs-lookup"><span data-stu-id="b55a2-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="b55a2-128">**"Exceso de publicación"**: También puede enviar un cliente *más* datos de lo previsto.</span><span class="sxs-lookup"><span data-stu-id="b55a2-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="b55a2-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b55a2-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="b55a2-130">En este caso, el JSON que incluye una propiedad ("Color") que no existe en el `Product` modelo.</span><span class="sxs-lookup"><span data-stu-id="b55a2-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="b55a2-131">En este caso, el formateador JSON simplemente omite este valor.</span><span class="sxs-lookup"><span data-stu-id="b55a2-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="b55a2-132">(El formateador XML hace lo mismo.) Publicación excesiva provoca problemas si el modelo tiene propiedades que pretende ser de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="b55a2-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="b55a2-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b55a2-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="b55a2-134">No desea que los usuarios actualizar la `IsAdmin` propiedad y elevar sus privilegios a los administradores.</span><span class="sxs-lookup"><span data-stu-id="b55a2-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="b55a2-135">La estrategia más segura consiste en utilizar una clase de modelo que coincida exactamente con lo que el cliente tiene permiso para enviar:</span><span class="sxs-lookup"><span data-stu-id="b55a2-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="b55a2-136">Entrada de blog de Brad Wilson "[frente a la validación de entrada. Modelo de validación en ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tiene una excelente explicación de debajo del registro y la publicación excesiva.</span><span class="sxs-lookup"><span data-stu-id="b55a2-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="b55a2-137">Aunque es la entrada de blog sobre ASP.NET MVC 2, los problemas continúan siendo relevantes para la API Web.</span><span class="sxs-lookup"><span data-stu-id="b55a2-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="b55a2-138">Control de errores de validación</span><span class="sxs-lookup"><span data-stu-id="b55a2-138">Handling Validation Errors</span></span>

<span data-ttu-id="b55a2-139">API Web no automáticamente devolverá un error al cliente cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="b55a2-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="b55a2-140">Es la acción del controlador para comprobar el estado del modelo y responder según corresponda.</span><span class="sxs-lookup"><span data-stu-id="b55a2-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="b55a2-141">También puede crear un filtro de acción para comprobar el estado del modelo antes de invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="b55a2-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="b55a2-142">El código siguiente muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b55a2-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="b55a2-143">Si se produce un error de validación del modelo, este filtro devuelve una respuesta HTTP que contiene los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="b55a2-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="b55a2-144">En ese caso, no se invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="b55a2-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="b55a2-145">Para aplicar este filtro a todos los controladores Web API, agregue una instancia del filtro para el **HttpConfiguration.Filters** colección durante la configuración:</span><span class="sxs-lookup"><span data-stu-id="b55a2-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="b55a2-146">Otra opción consiste en establecer el filtro como un atributo en controladores individuales o las acciones de controlador:</span><span class="sxs-lookup"><span data-stu-id="b55a2-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
