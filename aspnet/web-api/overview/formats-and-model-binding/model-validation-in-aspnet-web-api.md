---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validación del modelo en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Información general sobre la validación de modelos en ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448933"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="5752b-103">Validación del modelo en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5752b-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="5752b-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5752b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5752b-105">En este artículo se muestra cómo anotar los modelos, usar las anotaciones para la validación de datos y controlar los errores de validación en la API Web.</span><span class="sxs-lookup"><span data-stu-id="5752b-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="5752b-106">Cuando un cliente envía datos a la API Web, a menudo desea validar los datos antes de realizar cualquier procesamiento.</span><span class="sxs-lookup"><span data-stu-id="5752b-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="5752b-107">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="5752b-107">Data Annotations</span></span>

<span data-ttu-id="5752b-108">En ASP.NET Web API, puede usar atributos del espacio de nombres [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) para establecer reglas de validación para las propiedades del modelo.</span><span class="sxs-lookup"><span data-stu-id="5752b-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="5752b-109">Considere el modelo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5752b-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="5752b-110">Si ha usado la validación de modelos en ASP.NET MVC, debería resultar familiar.</span><span class="sxs-lookup"><span data-stu-id="5752b-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="5752b-111">El atributo **required** indica que la propiedad `Name` no debe ser null.</span><span class="sxs-lookup"><span data-stu-id="5752b-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="5752b-112">El atributo de **intervalo** indica que `Weight` debe estar entre cero y 999.</span><span class="sxs-lookup"><span data-stu-id="5752b-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="5752b-113">Supongamos que un cliente envía una solicitud POST con la siguiente representación JSON:</span><span class="sxs-lookup"><span data-stu-id="5752b-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="5752b-114">Puede ver que el cliente no incluyó la propiedad `Name`, que está marcada como requerida.</span><span class="sxs-lookup"><span data-stu-id="5752b-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="5752b-115">Cuando Web API convierte el archivo JSON en una instancia de `Product`, valida el `Product` con los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="5752b-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="5752b-116">En la acción del controlador, puede comprobar si el modelo es válido:</span><span class="sxs-lookup"><span data-stu-id="5752b-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="5752b-117">La validación del modelo no garantiza que los datos del cliente sean seguros.</span><span class="sxs-lookup"><span data-stu-id="5752b-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="5752b-118">Podría ser necesaria una validación adicional en otras capas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5752b-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="5752b-119">(Por ejemplo, el nivel de datos puede exigir restricciones Foreign Key). El tutorial [uso de Web API con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora algunos de estos problemas.</span><span class="sxs-lookup"><span data-stu-id="5752b-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="5752b-120">**"Superexponer"** : la subexposición se produce cuando el cliente abandona algunas propiedades.</span><span class="sxs-lookup"><span data-stu-id="5752b-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="5752b-121">Por ejemplo, supongamos que el cliente envía lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5752b-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="5752b-122">Aquí, el cliente no especificó valores para `Price` o `Weight`.</span><span class="sxs-lookup"><span data-stu-id="5752b-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="5752b-123">El formateador JSON asigna un valor predeterminado de cero a las propiedades que faltan.</span><span class="sxs-lookup"><span data-stu-id="5752b-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="5752b-124">El estado del modelo es válido, porque cero es un valor válido para estas propiedades.</span><span class="sxs-lookup"><span data-stu-id="5752b-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="5752b-125">El hecho de que se trata de un problema depende del escenario.</span><span class="sxs-lookup"><span data-stu-id="5752b-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="5752b-126">Por ejemplo, en una operación de actualización, es posible que desee distinguir entre "cero" y "sin establecer".</span><span class="sxs-lookup"><span data-stu-id="5752b-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="5752b-127">Para obligar a los clientes a establecer un valor, haga que la propiedad acepte valores NULL y establezca el atributo **required** :</span><span class="sxs-lookup"><span data-stu-id="5752b-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="5752b-128">**"Sobreexposición"** : un cliente también puede enviar *más* datos de los esperados.</span><span class="sxs-lookup"><span data-stu-id="5752b-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="5752b-129">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5752b-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="5752b-130">Aquí, el archivo JSON incluye una propiedad ("color") que no existe en el modelo de `Product`.</span><span class="sxs-lookup"><span data-stu-id="5752b-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="5752b-131">En este caso, el formateador JSON simplemente omite este valor.</span><span class="sxs-lookup"><span data-stu-id="5752b-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="5752b-132">(El formateador XML hace lo mismo). La publicación en exceso produce problemas si el modelo tiene propiedades que desea que sean de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="5752b-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="5752b-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5752b-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="5752b-134">No quiere que los usuarios actualicen la propiedad `IsAdmin` y se eleven a los administradores.</span><span class="sxs-lookup"><span data-stu-id="5752b-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="5752b-135">La estrategia más segura es usar una clase de modelo que coincida exactamente con la que el cliente puede enviar:</span><span class="sxs-lookup"><span data-stu-id="5752b-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="5752b-136">La entrada de blog de Brad Wilson "[validación de entrada frente a validación de modelo en ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" tiene una buena explicación de la publicación en exceso y la publicación en exceso.</span><span class="sxs-lookup"><span data-stu-id="5752b-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="5752b-137">Aunque la publicación es sobre ASP.NET MVC 2, los problemas siguen siendo pertinentes para la API Web.</span><span class="sxs-lookup"><span data-stu-id="5752b-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="5752b-138">Control de errores de validación</span><span class="sxs-lookup"><span data-stu-id="5752b-138">Handling Validation Errors</span></span>

<span data-ttu-id="5752b-139">La API Web no devuelve automáticamente un error al cliente cuando se produce un error de validación.</span><span class="sxs-lookup"><span data-stu-id="5752b-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="5752b-140">Depende de la acción del controlador comprobar el estado del modelo y responder de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="5752b-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="5752b-141">También puede crear un filtro de acción para comprobar el estado del modelo antes de que se invoque la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="5752b-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="5752b-142">El código siguiente muestra un ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5752b-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="5752b-143">Si se produce un error en la validación del modelo, este filtro devuelve una respuesta HTTP que contiene los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="5752b-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="5752b-144">En ese caso, no se invoca la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="5752b-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="5752b-145">Para aplicar este filtro a todos los controladores de API Web, agregue una instancia del filtro a la colección **HttpConfiguration. filters** durante la configuración:</span><span class="sxs-lookup"><span data-stu-id="5752b-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="5752b-146">Otra opción consiste en establecer el filtro como un atributo en los controladores o acciones de controlador individuales:</span><span class="sxs-lookup"><span data-stu-id="5752b-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
