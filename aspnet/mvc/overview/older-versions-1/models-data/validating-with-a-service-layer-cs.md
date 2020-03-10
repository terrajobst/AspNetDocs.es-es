---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validación con un nivel de servicioC#() | Microsoft Docs
author: StephenWalther
description: Obtenga información acerca de cómo trasladar la lógica de validación de las acciones del controlador y a un nivel de servicio independiente. En este tutorial, Stephen Walther explica cómo...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: da1a1c9cc79a452eb0d7597810e86f7bcf6cd179
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436609"
---
# <a name="validating-with-a-service-layer-c"></a><span data-ttu-id="17800-104">Validación con un nivel de servicio (C#)</span><span class="sxs-lookup"><span data-stu-id="17800-104">Validating with a Service Layer (C#)</span></span>

<span data-ttu-id="17800-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="17800-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="17800-106">Obtenga información acerca de cómo trasladar la lógica de validación de las acciones del controlador y a un nivel de servicio independiente.</span><span class="sxs-lookup"><span data-stu-id="17800-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="17800-107">En este tutorial, Stephen Walther explica cómo puede mantener una separación nítida de problemas aislando la capa de servicio de la capa de controlador.</span><span class="sxs-lookup"><span data-stu-id="17800-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>

<span data-ttu-id="17800-108">El objetivo de este tutorial es describir un método para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17800-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="17800-109">En este tutorial, aprenderá a trasladar la lógica de validación de los controladores y a un nivel de servicio independiente.</span><span class="sxs-lookup"><span data-stu-id="17800-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="17800-110">Descomponer problemas</span><span class="sxs-lookup"><span data-stu-id="17800-110">Separating Concerns</span></span>

<span data-ttu-id="17800-111">Al compilar una aplicación ASP.NET MVC, no debe colocar la lógica de base de datos dentro de las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="17800-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="17800-112">La combinación de la lógica de base de datos y del controlador hace que la aplicación sea más difícil de mantener con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="17800-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="17800-113">Se recomienda colocar toda la lógica de base de datos en una capa de repositorio independiente.</span><span class="sxs-lookup"><span data-stu-id="17800-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="17800-114">Por ejemplo, la lista 1 contiene un repositorio simple denominado ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="17800-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="17800-115">El repositorio del producto contiene todo el código de acceso a los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="17800-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="17800-116">La lista también incluye la interfaz IProductRepository que implementa el repositorio del producto.</span><span class="sxs-lookup"><span data-stu-id="17800-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="17800-117">**Lista 1: Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="17800-118">El controlador de la lista 2 usa la capa de repositorio en las acciones de índice () y Create ().</span><span class="sxs-lookup"><span data-stu-id="17800-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="17800-119">Tenga en cuenta que este controlador no contiene ninguna lógica de base de datos.</span><span class="sxs-lookup"><span data-stu-id="17800-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="17800-120">La creación de una capa de repositorio le permite mantener una separación limpia de los problemas.</span><span class="sxs-lookup"><span data-stu-id="17800-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="17800-121">Los controladores son responsables de la lógica de control de flujo de aplicación y el repositorio es responsable de la lógica de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="17800-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="17800-122">**Lista 2-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="17800-123">Creación de una capa de servicio</span><span class="sxs-lookup"><span data-stu-id="17800-123">Creating a Service Layer</span></span>

<span data-ttu-id="17800-124">Por lo tanto, la lógica de control de flujo de la aplicación pertenece a un controlador y la lógica de acceso a datos pertenece a un repositorio.</span><span class="sxs-lookup"><span data-stu-id="17800-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="17800-125">En ese caso, ¿dónde se coloca la lógica de validación?</span><span class="sxs-lookup"><span data-stu-id="17800-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="17800-126">Una opción consiste en colocar la lógica de validación en un *nivel de servicio*.</span><span class="sxs-lookup"><span data-stu-id="17800-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="17800-127">Una capa de servicio es una capa adicional en una aplicación ASP.NET MVC que media la comunicación entre un controlador y un nivel de repositorio.</span><span class="sxs-lookup"><span data-stu-id="17800-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="17800-128">El nivel de servicio contiene la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="17800-128">The service layer contains business logic.</span></span> <span data-ttu-id="17800-129">En concreto, contiene la lógica de validación.</span><span class="sxs-lookup"><span data-stu-id="17800-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="17800-130">Por ejemplo, la capa de servicio de producto de la lista 3 tiene un método CreateProduct ().</span><span class="sxs-lookup"><span data-stu-id="17800-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="17800-131">El método CreateProduct () llama al método ValidateProduct () para validar un nuevo producto antes de pasar el producto al repositorio del producto.</span><span class="sxs-lookup"><span data-stu-id="17800-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="17800-132">**Lista 3: Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="17800-133">El controlador de producto se ha actualizado en la lista 4 para usar el nivel de servicio en lugar de la capa de repositorio.</span><span class="sxs-lookup"><span data-stu-id="17800-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="17800-134">El nivel de controlador se comunica con el nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="17800-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="17800-135">El nivel de servicio se comunica con la capa de repositorio.</span><span class="sxs-lookup"><span data-stu-id="17800-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="17800-136">Cada capa tiene una responsabilidad independiente.</span><span class="sxs-lookup"><span data-stu-id="17800-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="17800-137">**Lista 4: Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="17800-138">Observe que el servicio de producto se crea en el constructor del controlador del producto.</span><span class="sxs-lookup"><span data-stu-id="17800-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="17800-139">Cuando se crea el servicio de producto, el Diccionario de estado del modelo se pasa al servicio.</span><span class="sxs-lookup"><span data-stu-id="17800-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="17800-140">El servicio de producto utiliza el estado del modelo para devolver los mensajes de error de validación al controlador.</span><span class="sxs-lookup"><span data-stu-id="17800-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="17800-141">Desacoplamiento del nivel de servicio</span><span class="sxs-lookup"><span data-stu-id="17800-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="17800-142">No se ha podido aislar el controlador y las capas de servicio en un sentido.</span><span class="sxs-lookup"><span data-stu-id="17800-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="17800-143">El controlador y los niveles de servicio se comunican a través del estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="17800-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="17800-144">En otras palabras, el nivel de servicio tiene una dependencia en una característica determinada del marco ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17800-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="17800-145">Queremos aislar el nivel de servicio de la capa de controlador tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="17800-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="17800-146">En teoría, deberíamos poder usar el nivel de servicio con cualquier tipo de aplicación y no solo una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17800-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="17800-147">Por ejemplo, en el futuro, es posible que deseemos compilar un front-end de WPF para nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="17800-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="17800-148">Deberíamos encontrar una manera de quitar la dependencia en el estado del modelo MVC de ASP.NET de nuestro nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="17800-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="17800-149">En la lista 5, el nivel de servicio se ha actualizado para que ya no use el estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="17800-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="17800-150">En su lugar, utiliza cualquier clase que implemente la interfaz IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="17800-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="17800-151">**Lista 5-Models\ProductService.cs (desacoplado)**</span><span class="sxs-lookup"><span data-stu-id="17800-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="17800-152">La interfaz IValidationDictionary se define en la lista 6.</span><span class="sxs-lookup"><span data-stu-id="17800-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="17800-153">Esta interfaz simple tiene un único método y una propiedad única.</span><span class="sxs-lookup"><span data-stu-id="17800-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="17800-154">**Listado 6-Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="17800-155">La clase de la lista 7, denominada la clase ModelStateWrapper, implementa la interfaz IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="17800-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="17800-156">Puede crear una instancia de la clase ModelStateWrapper pasando un diccionario de estado del modelo al constructor.</span><span class="sxs-lookup"><span data-stu-id="17800-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="17800-157">**Listado 7-Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="17800-158">Por último, el controlador actualizado en la lista 8 usa ModelStateWrapper al crear el nivel de servicio en su constructor.</span><span class="sxs-lookup"><span data-stu-id="17800-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="17800-159">**Lista 8-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="17800-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="17800-160">El uso de la interfaz IValidationDictionary y la clase ModelStateWrapper nos permite aislar completamente nuestro nivel de servicio de nuestra capa de controlador.</span><span class="sxs-lookup"><span data-stu-id="17800-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="17800-161">El nivel de servicio ya no depende del estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="17800-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="17800-162">Puede pasar cualquier clase que implemente la interfaz IValidationDictionary a la capa de servicio.</span><span class="sxs-lookup"><span data-stu-id="17800-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="17800-163">Por ejemplo, una aplicación de WPF podría implementar la interfaz IValidationDictionary con una clase de colección simple.</span><span class="sxs-lookup"><span data-stu-id="17800-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="17800-164">Resumen</span><span class="sxs-lookup"><span data-stu-id="17800-164">Summary</span></span>

<span data-ttu-id="17800-165">El objetivo de este tutorial era analizar un enfoque para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="17800-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="17800-166">En este tutorial, ha aprendido a trasladar toda la lógica de validación de los controladores y a un nivel de servicio independiente.</span><span class="sxs-lookup"><span data-stu-id="17800-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="17800-167">También ha aprendido a aislar el nivel de servicio de la capa de controlador mediante la creación de una clase ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="17800-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="17800-168">[Anterior](validating-with-the-idataerrorinfo-interface-cs.md)
> [Siguiente](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="17800-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
