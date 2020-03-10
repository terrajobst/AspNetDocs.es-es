---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inyección de dependencia de MVC 4 de ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Nota: en este laboratorio práctico se supone que tiene conocimientos básicos de los filtros de ASP.NET MVC y ASP.NET MVC 4. Si no ha usado los filtros de ASP.NET MVC 4 antes, Rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451825"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="c7b1b-104">Inserción de dependencias de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="c7b1b-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="c7b1b-105">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="c7b1b-106">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="c7b1b-107">Este laboratorio práctico supone que tiene conocimientos básicos de los filtros de **ASP.NET MVC** y **ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="c7b1b-108">Si no ha usado los **filtros de ASP.NET MVC 4** antes, le recomendamos que consulte el laboratorio práctico de los **filtros de acción personalizada de MVC de ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-109">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="c7b1b-110">El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Dependency inserciones](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="c7b1b-111">En el paradigma de **programación orientada a objetos** , los objetos funcionan juntos en un modelo de colaboración en el que hay colaboradores y consumidores.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="c7b1b-112">Naturalmente, este modelo de comunicación genera dependencias entre objetos y componentes, lo que resulta difícil de administrar cuando aumenta la complejidad.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="c7b1b-113">![Dependencias de clase y complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "Dependencias de clase y complejidad del modelo")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="c7b1b-114">*Dependencias de clase y complejidad del modelo*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="c7b1b-115">Probablemente ha oído hablar sobre el **patrón de fábrica** y la separación entre la interfaz y la implementación mediante servicios, donde los objetos de cliente suelen ser responsables de la ubicación del servicio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="c7b1b-116">El patrón de inserción de dependencias es una implementación concreta de la inversión de control.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="c7b1b-117">La **inversión de control (IOC)** significa que los objetos no crean otros objetos en los que se basan en su trabajo.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="c7b1b-118">En su lugar, obtienen los objetos que necesitan de un origen externo (por ejemplo, un archivo de configuración XML).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="c7b1b-119">La **inserción de dependencias (di)** significa que esto se hace sin la intervención del objeto, normalmente por parte de un componente de marco que pasa los parámetros de constructor y las propiedades de conjunto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="c7b1b-120">Modelo de diseño de inserción de dependencias (DI)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="c7b1b-121">En un nivel alto, el objetivo de la inserción de dependencias es que una clase de cliente (por ejemplo, *el jugador*) necesita algo que satisfaga una interfaz (por ejemplo, *iClub*).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="c7b1b-122">No le importa cuál sea el tipo concreto (por ejemplo *, WoodClub, IronClub, WedgeClub* o *PutterClub*), sino que otra persona pueda controlarlo (por ejemplo, un buen *Caddy*).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="c7b1b-123">La resolución de dependencias en ASP.NET MVC puede permitirle registrar la lógica de dependencia en otra parte (por ejemplo, un contenedor o una *bolsa de tréboles*).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="c7b1b-124">![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustración de inyección de dependencia")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="c7b1b-125">*Inserción de dependencias: analogía de golf*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="c7b1b-126">Las ventajas de usar el patrón de inserción de dependencias y la inversión de control son las siguientes:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="c7b1b-127">Reduce el acoplamiento de clases</span><span class="sxs-lookup"><span data-stu-id="c7b1b-127">Reduces class coupling</span></span>
- <span data-ttu-id="c7b1b-128">Aumenta la reutilización de código</span><span class="sxs-lookup"><span data-stu-id="c7b1b-128">Increases code reusing</span></span>
- <span data-ttu-id="c7b1b-129">Mejora el mantenimiento del código</span><span class="sxs-lookup"><span data-stu-id="c7b1b-129">Improves code maintainability</span></span>
- <span data-ttu-id="c7b1b-130">Mejora las pruebas de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="c7b1b-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-131">La inserción de dependencias a veces se compara con el patrón de diseño de fábrica abstracta, pero existe una pequeña diferencia entre ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="c7b1b-132">DI tiene un marco de trabajo en el que se resuelven las dependencias mediante una llamada a los generadores y los servicios registrados.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>

<span data-ttu-id="c7b1b-133">Ahora que comprende el patrón de inserción de dependencias, aprenderá a lo largo de este laboratorio cómo aplicarlo en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="c7b1b-134">Empezará a usar la inserción de dependencias en los **Controladores** para incluir un servicio de acceso a bases de datos.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="c7b1b-135">A continuación, aplicará la inserción de dependencias a las **vistas** para consumir un servicio y Mostrar información.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="c7b1b-136">Por último, extenderá el DI a ASP.NET MVC 4 filters e insertará un filtro de acción personalizado en la solución.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="c7b1b-137">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="c7b1b-138">Integración de ASP.NET MVC 4 con Unity para la inserción de dependencias mediante paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="c7b1b-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="c7b1b-139">Usar la inserción de dependencias dentro de un controlador ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c7b1b-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="c7b1b-140">Usar la inserción de dependencias dentro de una vista de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7b1b-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="c7b1b-141">Usar la inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7b1b-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-142">En este laboratorio se usa el paquete de NuGet Unity. Mvc3 para la resolución de dependencias, pero es posible adaptar cualquier marco de inserción de dependencias para que funcione con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c7b1b-143">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c7b1b-143">Prerequisites</span></span>

<span data-ttu-id="c7b1b-144">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="c7b1b-145">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c7b1b-146">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="c7b1b-146">Setup</span></span>

<span data-ttu-id="c7b1b-147">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="c7b1b-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="c7b1b-148">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c7b1b-149">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c7b1b-150">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice B: usar fragmentos de código](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c7b1b-151">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="c7b1b-151">Exercises</span></span>

<span data-ttu-id="c7b1b-152">Este laboratorio práctico se compone de los siguientes ejercicios:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="c7b1b-153">Ejercicio 1: inyección de un controlador</span><span class="sxs-lookup"><span data-stu-id="c7b1b-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="c7b1b-154">Ejercicio 2: inserción de una vista</span><span class="sxs-lookup"><span data-stu-id="c7b1b-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="c7b1b-155">Ejercicio 3: inserción de filtros</span><span class="sxs-lookup"><span data-stu-id="c7b1b-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c7b1b-156">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c7b1b-157">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="c7b1b-158">Tiempo estimado para completar este laboratorio: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="c7b1b-159">Ejercicio 1: inyección de un controlador</span><span class="sxs-lookup"><span data-stu-id="c7b1b-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="c7b1b-160">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en los controladores de ASP.NET MVC mediante la integración de Unity con un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c7b1b-161">Por ese motivo, incluirá servicios en los controladores de MvcMusicStore para separar la lógica del acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="c7b1b-162">Los servicios crearán una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la ayuda de **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="c7b1b-163">Este enfoque le mostrará cómo generar aplicaciones menos acopladas, que son más flexibles y fáciles de mantener y probar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="c7b1b-164">También aprenderá a integrar ASP.NET MVC con Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="c7b1b-165">Acerca del servicio StoreManager</span><span class="sxs-lookup"><span data-stu-id="c7b1b-165">About StoreManager Service</span></span>

<span data-ttu-id="c7b1b-166">El almacén de música de MVC proporcionado en la solución de inicio ahora incluye un servicio que administra los datos del controlador de almacenamiento denominados **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="c7b1b-167">A continuación encontrará la implementación del servicio de tienda.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="c7b1b-168">Tenga en cuenta que todos los métodos devuelven entidades del modelo.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="c7b1b-169">**StoreController** de la solución Begin ahora consume **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="c7b1b-170">Todas las referencias de datos se han quitado de **StoreController**y ahora es posible modificar el proveedor de acceso a datos actual sin cambiar ningún método que consuma **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="c7b1b-171">Encontrará más abajo que la implementación de **StoreController** tiene una dependencia con **StoreService** dentro del constructor de clase.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-172">La dependencia introducida en este ejercicio está relacionada con la **inversión de control** (IOC).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="c7b1b-173">El constructor de la clase **StoreController** recibe un parámetro de tipo **IStoreService** , que es esencial para realizar llamadas de servicio desde dentro de la clase.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="c7b1b-174">Sin embargo, **StoreController** no implementa el constructor predeterminado (sin parámetros) que cualquier controlador debe tener para trabajar con ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="c7b1b-175">Para resolver la dependencia, el controlador debe crearse mediante un generador abstracto (una clase que devuelve cualquier objeto del tipo especificado).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="c7b1b-176">Obtendrá un error cuando la clase intente crear el StoreController sin enviar el objeto de servicio, ya que no se ha declarado ningún constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="c7b1b-177">Tarea 1: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c7b1b-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="c7b1b-178">En esta tarea, ejecutará la aplicación de inicio, que incluye el servicio en el controlador de almacenamiento que separa el acceso a los datos de la lógica de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="c7b1b-179">Al ejecutar la aplicación, recibirá una excepción, ya que el servicio del controlador no se pasa como parámetro de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="c7b1b-180">Abra la solución de **Inicio** que se encuentra en **Source\Ex01-injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="c7b1b-181">Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c7b1b-182">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c7b1b-183">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c7b1b-184">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c7b1b-185">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c7b1b-186">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c7b1b-187">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="c7b1b-188">Presione **Ctrl + F5** para ejecutar la aplicación sin depuración.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="c7b1b-189">Obtendrá el mensaje de error &quot;**no hay ningún constructor sin parámetros definido para este objeto**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="c7b1b-190">![Error al ejecutar la aplicación de inicio de ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Error al ejecutar la aplicación de inicio de ASP.NET MVC")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="c7b1b-191">*Error al ejecutar la aplicación de inicio de ASP.NET MVC*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="c7b1b-192">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-192">Close the browser.</span></span>

<span data-ttu-id="c7b1b-193">En los pasos siguientes, trabajará en la solución Music Store para insertar la dependencia que este controlador necesita.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="c7b1b-194">Tarea 2: inclusión de Unity en la solución MvcMusicStore</span><span class="sxs-lookup"><span data-stu-id="c7b1b-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="c7b1b-195">En esta tarea, incluirá el paquete de NuGet **Unity. Mvc3** en la solución.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-196">El paquete Unity. Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="c7b1b-197">Unity es un contenedor de inserción de dependencias ligera y extensible con compatibilidad opcional para la intercepción de instancias y tipos.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="c7b1b-198">Es un contenedor de uso general para su uso en cualquier tipo de aplicación .NET.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="c7b1b-199">Proporciona todas las características comunes que se encuentran en los mecanismos de inserción de dependencias, entre las que se incluyen la creación de objetos, la abstracción de los requisitos mediante la especificación de dependencias en tiempo de ejecución y flexibilidad, al aplazar la configuración del componente en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>

1. <span data-ttu-id="c7b1b-200">Instale el paquete de NuGet de **Unity. Mvc3** en el proyecto **MvcMusicStore** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="c7b1b-201">Para ello, abra la **consola del administrador de paquetes** en **Ver** | **otras ventanas**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="c7b1b-202">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-202">Run the following command.</span></span>

    <span data-ttu-id="c7b1b-203">PMC</span><span class="sxs-lookup"><span data-stu-id="c7b1b-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="c7b1b-204">![Instalación del paquete de NuGet de Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalación del paquete de NuGet de Unity. Mvc3")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="c7b1b-205">*Instalación del paquete de NuGet de Unity. Mvc3*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="c7b1b-206">Una vez instalado el paquete **Unity. Mvc3** , explore los archivos y las carpetas que agrega automáticamente para simplificar la configuración de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="c7b1b-207">![Paquete Unity. Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Paquete Unity. Mvc3 instalado")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="c7b1b-208">*Paquete Unity. Mvc3 instalado*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a><span data-ttu-id="c7b1b-209">Tarea 3: registro de Unity en la aplicación Global.asax.cs Inicio\_</span><span class="sxs-lookup"><span data-stu-id="c7b1b-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="c7b1b-210">En esta tarea, actualizará la **aplicación\_método Start** ubicado en **global.asax.CS** para llamar al inicializador de arranque de Unity y, a continuación, actualizar el archivo de programa previo que registra el servicio y el controlador que usará para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="c7b1b-211">Ahora, enlazará el programa previo, que es el archivo que inicializa el contenedor de Unity y la resolución de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="c7b1b-212">Para ello, Abra **global.asax.CS** y agregue el siguiente código resaltado en el método de inicio de la **aplicación\_** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="c7b1b-213">(Fragmento de código- *ASP.net de inserción de dependencias-EX01-Initialize Unity*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="c7b1b-214">Abra el archivo **Bootstrapper.CS** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="c7b1b-215">Incluya los siguientes espacios de nombres: **MvcMusicStore. Services** y **MusicStore. Controllers**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="c7b1b-216">(Fragmento de código- *ASP.net inserciones de dependencia Lab-EX01-Bootstrapper agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="c7b1b-217">Reemplace el contenido del método **BuildUnityContainer** por el código siguiente que registra el controlador de almacén y el servicio de almacén.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="c7b1b-218">(Fragmento de código- *ASP.net de inserción de dependencias-EX01-registrar controlador y servicio de almacén*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c7b1b-219">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c7b1b-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="c7b1b-220">En esta tarea, ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="c7b1b-221">Presione **F5** para ejecutar la aplicación, la aplicación debe cargarse ahora sin mostrar ningún mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="c7b1b-222">![Ejecución de la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "Ejecución de la aplicación con la inserción de dependencias")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="c7b1b-223">*Ejecución de la aplicación con la inserción de dependencias*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="c7b1b-224">Vaya a **/Store**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-224">Browse to **/Store**.</span></span> <span data-ttu-id="c7b1b-225">Esto invocará a **StoreController**, que ahora se crea con **Unity**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="c7b1b-226">![Almacén de música de MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Almacén de música de MVC")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="c7b1b-227">*Almacén de música de MVC*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="c7b1b-228">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-228">Close the browser.</span></span>

<span data-ttu-id="c7b1b-229">En los siguientes ejercicios aprenderá a ampliar el ámbito de inserción de dependencias para usarlo dentro de las vistas y los filtros de acción de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="c7b1b-230">Ejercicio 2: inserción de una vista</span><span class="sxs-lookup"><span data-stu-id="c7b1b-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="c7b1b-231">En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="c7b1b-232">Para ello, llamará a un servicio personalizado dentro de la vista examinar del almacén, que mostrará un mensaje y una imagen a continuación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="c7b1b-233">A continuación, integrará el proyecto con Unity y creará una resolución de dependencia personalizada para insertar las dependencias.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="c7b1b-234">Tarea 1: creación de una vista que consume un servicio</span><span class="sxs-lookup"><span data-stu-id="c7b1b-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="c7b1b-235">En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="c7b1b-236">El servicio está en un servicio de mensajería simple incluido en esta solución.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="c7b1b-237">Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex02-injecting View\Begin**</span><span class="sxs-lookup"><span data-stu-id="c7b1b-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="c7b1b-238">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c7b1b-239">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c7b1b-240">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c7b1b-241">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c7b1b-242">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c7b1b-243">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c7b1b-244">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c7b1b-245">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c7b1b-246">Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c7b1b-247">Incluya las clases **MessageService.CS** y **IMessageService.CS** que se encuentran en la carpeta **source \Assets** en **/Services**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="c7b1b-248">Para ello, haga clic con el botón derecho en la carpeta **servicios** y seleccione **Agregar elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="c7b1b-249">Vaya a la ubicación de los archivos e inclúyalo.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="c7b1b-250">![Adición del servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Adición del servicio de mensajes y la interfaz de servicio")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="c7b1b-251">*Adición del servicio de mensajes y la interfaz de servicio*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="c7b1b-252">La interfaz **IMessageService** define dos propiedades implementadas por la clase **MessageService** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="c7b1b-253">Estas propiedades-**Message** y **ImageUrl**: almacenan el mensaje y la dirección URL de la imagen que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="c7b1b-254">Cree la carpeta **/pages** en la carpeta raíz del proyecto y, a continuación, agregue la clase existente **MyBasePage.CS** desde **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="c7b1b-255">La página base de la que se va a heredar tiene la estructura siguiente.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="c7b1b-256">![Carpeta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "Carpeta Pages")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="c7b1b-257">Abra la vista **examinar. cshtml** de la carpeta **/views/Store** y haga que se herede de **MyBasePage.CS**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="c7b1b-258">En la vista **examinar** , agregue una llamada a **MessageService** para mostrar una imagen y un mensaje recuperado por el servicio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="c7b1b-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="c7b1b-260">Tarea 2: incluir un solucionador de dependencias personalizado y un activador de página de vista personalizada</span><span class="sxs-lookup"><span data-stu-id="c7b1b-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="c7b1b-261">En la tarea anterior, insertó una nueva dependencia dentro de una vista para realizar una llamada de servicio dentro de ella.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="c7b1b-262">Ahora, resolverá esa dependencia implementando las interfaces de inyección de dependencia de ASP.NET MVC **IViewPageActivator** y **objeto idependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="c7b1b-263">Incluirá en la solución una implementación de **objeto idependencyresolver** que se ocupará de la recuperación del servicio mediante Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="c7b1b-264">A continuación, incluirá otra implementación personalizada de la interfaz **IViewPageActivator** que resolverá la creación de las vistas.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b1b-265">Dado que ASP.NET MVC 3, la implementación de la inserción de dependencias ha simplificado las interfaces para registrar los servicios.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="c7b1b-266">**Objeto idependencyresolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="c7b1b-267">**-** La interfaz objeto idependencyresolver reemplaza el IMvcServiceLocator anterior.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="c7b1b-268">Los implementadores de objeto idependencyresolver deben devolver una instancia del servicio o una colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="c7b1b-269">**-La interfaz IViewPageActivator** proporciona un control más preciso sobre cómo se crean instancias de las páginas de vista a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="c7b1b-270">Las clases que implementan la interfaz **IViewPageActivator** pueden crear instancias de vista mediante la información de contexto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. <span data-ttu-id="c7b1b-271">Cree la carpeta/**factorías** en la carpeta raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="c7b1b-272">Incluya **CustomViewPageActivator.CS** en la solución de **/sources/assets/** en la carpeta **factorías** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="c7b1b-273">Para ello, haga clic con el botón derecho en la carpeta **/factories** , seleccione **Agregar | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.CS**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="c7b1b-274">Esta clase implementa la interfaz **IViewPageActivator** para contener el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="c7b1b-275">**CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="c7b1b-276">Incluya el archivo **UnityDependencyResolver.CS** de **/sources/assets** en la carpeta **/factories**</span><span class="sxs-lookup"><span data-stu-id="c7b1b-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="c7b1b-277">Para ello, haga clic con el botón derecho en la carpeta **/factories** , seleccione **Agregar | Elemento existente** y, a continuación, seleccione el archivo **UnityDependencyResolver.CS** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="c7b1b-278">La clase **UnityDependencyResolver** es un DependencyResolver personalizado para Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="c7b1b-279">Cuando no se puede encontrar un servicio dentro del contenedor de Unity, el solucionador base es invocated.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="c7b1b-280">En la tarea siguiente, ambas implementaciones se registrarán para permitir que el modelo Conozca la ubicación de los servicios y las vistas.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="c7b1b-281">Tarea 3: registro de la inserción de dependencias en el contenedor de Unity</span><span class="sxs-lookup"><span data-stu-id="c7b1b-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="c7b1b-282">En esta tarea, colocará todos los elementos anteriores juntos para que la inserción de dependencias funcione.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="c7b1b-283">Hasta ahora la solución tiene los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="c7b1b-284">Una vista de **exploración** que hereda de **MyBaseClass** y consume **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="c7b1b-285">Clase intermedia-**MyBaseClass**: que tiene la inserción de dependencias declarada para la interfaz de servicio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="c7b1b-286">Un servicio- **MessageService** -y su interfaz **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="c7b1b-287">Una resolución de dependencia personalizada para Unity- **UnityDependencyResolver** , que se ocupa de la recuperación del servicio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="c7b1b-288">Una página de vista Activator- **CustomViewPageActivator** : que crea la página.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="c7b1b-289">Para insertar la vista **examinar** , ahora registrará la resolución de dependencia personalizada en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="c7b1b-290">Abra el archivo **Bootstrapper.CS** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="c7b1b-291">Registre una instancia de **MessageService** en el contenedor de Unity para inicializar el servicio:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="c7b1b-292">(Fragmento de código- *ASP.net de inserción de dependencias-Ex02-registrar servicio de mensajes*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="c7b1b-293">Agregue una referencia al espacio de nombres **MvcMusicStore. factorías** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="c7b1b-294">(Fragmento de código- *ASP.net de inserción de dependencias Lab-Ex02-factorías espacio de nombres*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="c7b1b-295">Registre **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="c7b1b-296">(Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex02-Register CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="c7b1b-297">Reemplace la resolución de dependencia predeterminada de ASP.NET MVC 4 por una instancia de **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="c7b1b-298">Para ello, reemplace el contenido del método **Initialize** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-298">To do this, replace **Initialize** method content with the following code:</span></span>

    <span data-ttu-id="c7b1b-299">(Fragmento de código- *ASP.net de inserción de dependencias laboratorio-Ex02-solucionador de dependencias de actualización*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="c7b1b-300">ASP.NET MVC proporciona una clase de solucionador de dependencias predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="c7b1b-301">Para trabajar con resoluciones de dependencia personalizadas como la que hemos creado para Unity, este solucionador debe reemplazarse.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="c7b1b-302">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c7b1b-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="c7b1b-303">En esta tarea, ejecutará la aplicación para comprobar que el explorador de la tienda consume el servicio y muestra la imagen y el mensaje recuperado:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="c7b1b-304">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c7b1b-305">Haga clic en **Rock** en el menú géneros y vea cómo se insertó **MessageService** en la vista y cargó el mensaje de bienvenida y la imagen.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="c7b1b-306">En este ejemplo, vamos a entrar en &quot;**Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="c7b1b-307">![Almacén de música de MVC: ver inyección](aspnet-mvc-4-dependency-injection/_static/image10.png "Almacén de música de MVC: ver inyección")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="c7b1b-308">*Almacén de música de MVC: ver inyección*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="c7b1b-309">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="c7b1b-310">Ejercicio 3: inserción de filtros de acción</span><span class="sxs-lookup"><span data-stu-id="c7b1b-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="c7b1b-311">En los **filtros de acción personalizada** del laboratorio práctico anterior, ha trabajado con la personalización de filtros y la inserción.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="c7b1b-312">En este ejercicio, obtendrá información sobre cómo insertar filtros con la inserción de dependencias mediante el contenedor Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="c7b1b-313">Para ello, agregará a la solución Music Store un filtro de acción personalizado que realizará el seguimiento de la actividad del sitio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="c7b1b-314">Tarea 1: incluir el filtro de seguimiento en la solución</span><span class="sxs-lookup"><span data-stu-id="c7b1b-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="c7b1b-315">En esta tarea, incluirá en la tienda de música un filtro de acción personalizado para realizar un seguimiento de los eventos.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="c7b1b-316">Como los conceptos de filtros de acción personalizados ya se tratan en el laboratorio anterior &quot;filtros de acción personalizados&quot;, solo se incluye la clase de filtro de la carpeta activos de este laboratorio y, a continuación, se crea un proveedor de filtro para Unity:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="c7b1b-317">Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex03-inyectando acción Filter\Begin** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="c7b1b-318">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c7b1b-319">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c7b1b-320">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c7b1b-321">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c7b1b-322">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c7b1b-323">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c7b1b-324">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c7b1b-325">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="c7b1b-326">Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="c7b1b-327">Incluya el archivo **TraceActionFilter.CS** de **/sources/assets** en la carpeta **/Filters**</span><span class="sxs-lookup"><span data-stu-id="c7b1b-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="c7b1b-328">Este filtro de acción personalizada realiza el seguimiento de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="c7b1b-329">Puede comprobar &quot;los filtros de acciones locales y dinámicas de ASP.NET MVC 4&quot; laboratorio para obtener más referencia.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="c7b1b-330">Agregue la clase vacía **FilterProvider.CS** al proyecto en la carpeta **/Filters.**</span><span class="sxs-lookup"><span data-stu-id="c7b1b-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="c7b1b-331">Agregue los espacios de nombres **System. Web. Mvc** y **Microsoft. Practices. Unity** en **FilterProvider.CS**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="c7b1b-332">(Fragmento de código- *ASP.net de inserción de dependencias-Ex03-filtro agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="c7b1b-333">Haga que la clase herede de la interfaz **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="c7b1b-334">Agregue una propiedad **IUnityContainer** en la clase **FilterProvider** y, a continuación, cree un constructor de clase para asignar el contenedor.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="c7b1b-335">(Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Filter Provider constructor*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="c7b1b-336">El constructor de clase de proveedor de filtros no está creando un **nuevo** objeto dentro de.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="c7b1b-337">El contenedor se pasa como parámetro y Unity resuelve la dependencia.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="c7b1b-338">En la clase **FilterProvider** , implemente el método **GetFilters** desde la interfaz **IFilterProvider** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="c7b1b-339">(Fragmento de código- *ASP.net de inserción de dependencias-Ex03-proveedor de filtros GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="c7b1b-340">Tarea 2: registro y habilitación del filtro</span><span class="sxs-lookup"><span data-stu-id="c7b1b-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="c7b1b-341">En esta tarea, habilitará el seguimiento del sitio.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="c7b1b-342">Para ello, registrará el filtro en el método **Bootstrapper.CS BuildUnityContainer** para iniciar el seguimiento:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="c7b1b-343">Abra el **archivo Web. config** que se encuentra en la raíz del proyecto y habilite seguimiento de seguimiento en el grupo System. Web.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="c7b1b-344">Abra **Bootstrapper.CS** en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="c7b1b-345">Agregue una referencia al espacio de nombres **MvcMusicStore. filters** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="c7b1b-346">(Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Bootstrapper agregar espacios de nombres*)</span><span class="sxs-lookup"><span data-stu-id="c7b1b-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="c7b1b-347">Seleccione el método **BuildUnityContainer** y registre el filtro en el contenedor de Unity.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="c7b1b-348">Tendrá que registrar el proveedor de filtro, así como el filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="c7b1b-349">(Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Register FilterProvider y actionfilter (* )</span><span class="sxs-lookup"><span data-stu-id="c7b1b-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="c7b1b-350">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="c7b1b-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="c7b1b-351">En esta tarea, ejecutará la aplicación y comprobará que el filtro de acción personalizada está realizando el seguimiento de la actividad:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="c7b1b-352">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="c7b1b-353">Haga clic en **Rock** en el menú géneros.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="c7b1b-354">Si lo desea, puede ir a más géneros.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="c7b1b-355">![Tienda de música](aspnet-mvc-4-dependency-injection/_static/image11.png "Tienda de música")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="c7b1b-356">*Tienda de música*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-356">*Music Store*</span></span>
3. <span data-ttu-id="c7b1b-357">Vaya a **/Trace.axd** para ver la página seguimiento de la aplicación y, a continuación, haga clic en **Ver detalles**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="c7b1b-358">![Registro de seguimiento de la aplicación](aspnet-mvc-4-dependency-injection/_static/image12.png "Registro de seguimiento de la aplicación")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="c7b1b-359">*Registro de seguimiento de la aplicación*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-359">*Application Trace Log*</span></span>

    <span data-ttu-id="c7b1b-360">![Seguimiento de la aplicación-detalles de la solicitud](aspnet-mvc-4-dependency-injection/_static/image13.png "Seguimiento de la aplicación-detalles de la solicitud")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="c7b1b-361">*Seguimiento de la aplicación-detalles de la solicitud*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="c7b1b-362">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-362">Close the browser.</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c7b1b-363">Resumen</span><span class="sxs-lookup"><span data-stu-id="c7b1b-363">Summary</span></span>

<span data-ttu-id="c7b1b-364">Al completar este laboratorio práctico, ha aprendido a usar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity con un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="c7b1b-365">Para lograrlo, ha usado la inserción de dependencias en los controladores, las vistas y los filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="c7b1b-366">Se trataron los siguientes conceptos:</span><span class="sxs-lookup"><span data-stu-id="c7b1b-366">The following concepts were covered:</span></span>

- <span data-ttu-id="c7b1b-367">Características de inserción de dependencias de MVC 4 de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7b1b-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="c7b1b-368">Integración de Unity mediante el paquete NuGet de Unity. Mvc3</span><span class="sxs-lookup"><span data-stu-id="c7b1b-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="c7b1b-369">Inserción de dependencias en controladores</span><span class="sxs-lookup"><span data-stu-id="c7b1b-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="c7b1b-370">Inserción de dependencias en vistas</span><span class="sxs-lookup"><span data-stu-id="c7b1b-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="c7b1b-371">Inserción de dependencias de filtros de acción</span><span class="sxs-lookup"><span data-stu-id="c7b1b-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c7b1b-372">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="c7b1b-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c7b1b-373">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c7b1b-374">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c7b1b-375">Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-375">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c7b1b-376">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c7b1b-377">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-377">Click on **Install Now**.</span></span> <span data-ttu-id="c7b1b-378">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c7b1b-379">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c7b1b-380">![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="c7b1b-381">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="c7b1b-382">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="c7b1b-384">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="c7b1b-385">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-385">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="c7b1b-387">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-387">*Installation progress*</span></span>
6. <span data-ttu-id="c7b1b-388">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-388">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="c7b1b-390">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-390">*Installation completed*</span></span>
7. <span data-ttu-id="c7b1b-391">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c7b1b-392">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="c7b1b-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="c7b1b-394">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="c7b1b-395">Apéndice B: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="c7b1b-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="c7b1b-396">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c7b1b-397">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c7b1b-398">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="c7b1b-399">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="c7b1b-400">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="c7b1b-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="c7b1b-401">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c7b1b-402">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c7b1b-403">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c7b1b-404">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="c7b1b-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c7b1b-405">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="c7b1b-406">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-dependency-injection/_static/image20.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="c7b1b-407">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="c7b1b-408">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-dependency-injection/_static/image21.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="c7b1b-409">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="c7b1b-410">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-dependency-injection/_static/image22.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="c7b1b-411">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="c7b1b-412">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="c7b1b-413">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="c7b1b-414">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="c7b1b-415">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="c7b1b-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="c7b1b-416">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-dependency-injection/_static/image23.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="c7b1b-417">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="c7b1b-418">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-dependency-injection/_static/image24.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="c7b1b-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="c7b1b-419">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="c7b1b-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
