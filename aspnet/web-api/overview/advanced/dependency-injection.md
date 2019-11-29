---
uid: web-api/overview/advanced/dependency-injection
title: Inserción de dependencias en ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: En este tutorial se muestra cómo insertar dependencias en el controlador de ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: f9c212af92168ac02644625b9aa8ec1bef329cab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600420"
---
# <a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="fdba7-103">Inserción de dependencias en ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="fdba7-103">Dependency Injection in ASP.NET Web API 2</span></span>

<span data-ttu-id="fdba7-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fdba7-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="fdba7-105">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="fdba7-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="fdba7-106">En este tutorial se muestra cómo insertar dependencias en el controlador de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="fdba7-106">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fdba7-107">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="fdba7-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="fdba7-108">API Web 2</span><span class="sxs-lookup"><span data-stu-id="fdba7-108">Web API 2</span></span>
> - [<span data-ttu-id="fdba7-109">Bloque de aplicaciones de Unity</span><span class="sxs-lookup"><span data-stu-id="fdba7-109">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="fdba7-110">Entity Framework 6 (la versión 5 también funciona)</span><span class="sxs-lookup"><span data-stu-id="fdba7-110">Entity Framework 6 (version 5 also works)</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="fdba7-111">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="fdba7-111">What is Dependency Injection?</span></span>

<span data-ttu-id="fdba7-112">Una *dependencia* es cualquier objeto requerido por otro objeto.</span><span class="sxs-lookup"><span data-stu-id="fdba7-112">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="fdba7-113">Por ejemplo, es habitual definir un [repositorio](http://martinfowler.com/eaaCatalog/repository.html) que controle el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="fdba7-113">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="fdba7-114">Vamos a ilustrar con un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="fdba7-114">Let's illustrate with an example.</span></span> <span data-ttu-id="fdba7-115">En primer lugar, definiremos un modelo de dominio:</span><span class="sxs-lookup"><span data-stu-id="fdba7-115">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="fdba7-116">A continuación se muestra una clase de repositorio simple que almacena elementos en una base de datos mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fdba7-116">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="fdba7-117">Ahora vamos a definir un controlador de API Web que admita solicitudes GET para entidades `Product`.</span><span class="sxs-lookup"><span data-stu-id="fdba7-117">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="fdba7-118">(He salido de POST y otros métodos de simplicidad). Este es un primer intento:</span><span class="sxs-lookup"><span data-stu-id="fdba7-118">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="fdba7-119">Tenga en cuenta que la clase del controlador depende de `ProductRepository`y de que el controlador cree la instancia de `ProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="fdba7-119">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="fdba7-120">Sin embargo, es una buena idea codificar la dependencia de esta manera, por varias razones.</span><span class="sxs-lookup"><span data-stu-id="fdba7-120">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="fdba7-121">Si desea reemplazar `ProductRepository` por una implementación diferente, también debe modificar la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-121">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="fdba7-122">Si el `ProductRepository` tiene dependencias, debe configurarlas dentro del controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-122">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="fdba7-123">En un proyecto grande con varios controladores, el código de configuración se vuelve disperso en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fdba7-123">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="fdba7-124">Es difícil realizar pruebas unitarias, ya que el controlador está codificado de forma rígida para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fdba7-124">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="fdba7-125">En el caso de una prueba unitaria, debe usar un repositorio ficticio o de código auxiliar, lo que no es posible con el diseño actual.</span><span class="sxs-lookup"><span data-stu-id="fdba7-125">For a unit test, you should use a mock or stub repository, which is not possible with the current design.</span></span>

<span data-ttu-id="fdba7-126">Podemos resolver estos problemas *insertando* el repositorio en el controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-126">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="fdba7-127">En primer lugar, refactorice la clase `ProductRepository` en una interfaz:</span><span class="sxs-lookup"><span data-stu-id="fdba7-127">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="fdba7-128">A continuación, proporcione el `IProductRepository` como un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="fdba7-128">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="fdba7-129">En este ejemplo se usa la [inserción de constructores](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="fdba7-129">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="fdba7-130">También puede usar la *inyección de establecedor*, donde se establece la dependencia a través de un método o propiedad de establecedor.</span><span class="sxs-lookup"><span data-stu-id="fdba7-130">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="fdba7-131">Pero ahora hay un problema, porque la aplicación no crea el controlador directamente.</span><span class="sxs-lookup"><span data-stu-id="fdba7-131">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="fdba7-132">La API Web crea el controlador cuando enruta la solicitud y la API Web no sabe nada sobre `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="fdba7-132">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="fdba7-133">Aquí es donde entra en el solucionador de dependencias de la API Web.</span><span class="sxs-lookup"><span data-stu-id="fdba7-133">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="fdba7-134">La resolución de dependencias de la API Web</span><span class="sxs-lookup"><span data-stu-id="fdba7-134">The Web API Dependency Resolver</span></span>

<span data-ttu-id="fdba7-135">Web API define la interfaz **objeto idependencyresolver** para resolver las dependencias.</span><span class="sxs-lookup"><span data-stu-id="fdba7-135">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="fdba7-136">Esta es la definición de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="fdba7-136">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="fdba7-137">La interfaz **IDependencyScope** tiene dos métodos:</span><span class="sxs-lookup"><span data-stu-id="fdba7-137">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="fdba7-138">**GetService** crea una instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="fdba7-138">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="fdba7-139">**GetServices** crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="fdba7-139">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="fdba7-140">El método **objeto idependencyresolver** hereda **IDependencyScope** y agrega el método **BeginScope** .</span><span class="sxs-lookup"><span data-stu-id="fdba7-140">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="fdba7-141">Hablaremos sobre los ámbitos más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="fdba7-141">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="fdba7-142">Cuando la API Web crea una instancia del controlador, llama primero a **objeto idependencyresolver. GetService**, pasando el tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-142">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="fdba7-143">Puede usar este enlace de extensibilidad para crear el controlador y resolver las dependencias.</span><span class="sxs-lookup"><span data-stu-id="fdba7-143">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="fdba7-144">Si **GetService** devuelve null, Web API busca un constructor sin parámetros en la clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-144">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="fdba7-145">Resolución de dependencias con el contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="fdba7-145">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="fdba7-146">Aunque podría escribir una implementación completa de **objeto idependencyresolver** desde cero, la interfaz está diseñada realmente para actuar como puente entre la API Web y los contenedores de IOC existentes.</span><span class="sxs-lookup"><span data-stu-id="fdba7-146">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="fdba7-147">Un contenedor de IoC es un componente de software que es responsable de administrar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="fdba7-147">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="fdba7-148">Los tipos se registran con el contenedor y, a continuación, se usa el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="fdba7-148">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="fdba7-149">El contenedor averigua automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="fdba7-149">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="fdba7-150">Muchos contenedores de IoC también permiten controlar aspectos como la duración y el ámbito de los objetos.</span><span class="sxs-lookup"><span data-stu-id="fdba7-150">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="fdba7-151">"IoC" representa "inversion of control", que es un patrón general en el que un marco llama al código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdba7-151">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="fdba7-152">Un contenedor de IoC crea los objetos automáticamente, lo que "invierte" el flujo de control habitual.</span><span class="sxs-lookup"><span data-stu-id="fdba7-152">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

<span data-ttu-id="fdba7-153">En este tutorial, usaremos [Unity](https://msdn.microsoft.com/library/ff647202.aspx) en Microsoft patterns &amp; Practices.</span><span class="sxs-lookup"><span data-stu-id="fdba7-153">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="fdba7-154">(Otras bibliotecas populares incluyen el ejemplo de el [Windsor Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/)y [StructureMap](http://structuremap.github.io/documentation/)). Puede usar el administrador de paquetes NuGet para instalar Unity.</span><span class="sxs-lookup"><span data-stu-id="fdba7-154">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="fdba7-155">En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="fdba7-155">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="fdba7-156">En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="fdba7-156">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="fdba7-157">Esta es una implementación de **objeto idependencyresolver** que contiene un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="fdba7-157">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="fdba7-158">Si el método **GetService** no puede resolver un tipo, debe devolver **null**.</span><span class="sxs-lookup"><span data-stu-id="fdba7-158">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="fdba7-159">Si el método **GetServices** no puede resolver un tipo, debe devolver un objeto de colección vacío.</span><span class="sxs-lookup"><span data-stu-id="fdba7-159">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="fdba7-160">No inicie excepciones para tipos desconocidos.</span><span class="sxs-lookup"><span data-stu-id="fdba7-160">Don't throw exceptions for unknown types.</span></span>

## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="fdba7-161">Configuración de la resolución de dependencias</span><span class="sxs-lookup"><span data-stu-id="fdba7-161">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="fdba7-162">Establezca la resolución de dependencias en la propiedad **DependencyResolver** del objeto **HttpConfiguration** global.</span><span class="sxs-lookup"><span data-stu-id="fdba7-162">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="fdba7-163">En el código siguiente se registra la interfaz de `IProductRepository` con Unity y, a continuación, se crea un `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="fdba7-163">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="fdba7-164">Ámbito de dependencia y duración del controlador</span><span class="sxs-lookup"><span data-stu-id="fdba7-164">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="fdba7-165">Los controladores se crean por solicitud.</span><span class="sxs-lookup"><span data-stu-id="fdba7-165">Controllers are created per request.</span></span> <span data-ttu-id="fdba7-166">Para administrar la duración de los objetos, **objeto idependencyresolver** usa el concepto de un *ámbito*.</span><span class="sxs-lookup"><span data-stu-id="fdba7-166">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="fdba7-167">El solucionador de dependencias asociado al objeto **HttpConfiguration** tiene ámbito global.</span><span class="sxs-lookup"><span data-stu-id="fdba7-167">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="fdba7-168">Cuando Web API crea un controlador, llama a **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="fdba7-168">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="fdba7-169">Este método devuelve un **IDependencyScope** que representa un ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="fdba7-169">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="fdba7-170">A continuación, la API Web llama a **GetService** en el ámbito secundario para crear el controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-170">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="fdba7-171">Cuando se completa la solicitud, la API Web llama a **Dispose** en el ámbito secundario.</span><span class="sxs-lookup"><span data-stu-id="fdba7-171">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="fdba7-172">Use el método **Dispose** para eliminar las dependencias del controlador.</span><span class="sxs-lookup"><span data-stu-id="fdba7-172">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="fdba7-173">La forma de implementar **BeginScope** depende del contenedor de IOC.</span><span class="sxs-lookup"><span data-stu-id="fdba7-173">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="fdba7-174">Para Unity, el ámbito corresponde a un contenedor secundario:</span><span class="sxs-lookup"><span data-stu-id="fdba7-174">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="fdba7-175">La mayoría de los contenedores de IoC tienen equivalentes similares.</span><span class="sxs-lookup"><span data-stu-id="fdba7-175">Most IoC containers have similar equivalents.</span></span>
