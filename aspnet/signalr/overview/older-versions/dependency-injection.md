---
uid: signalr/overview/older-versions/dependency-injection
title: Inserción de dependencias en Signalr 1. x | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: de838ab6b3a299eb1e5ebeb9fa3c583478ce3e56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431545"
---
# <a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="ca11f-102">Inserción de dependencias en SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="ca11f-102">Dependency Injection in SignalR 1.x</span></span>

<span data-ttu-id="ca11f-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="ca11f-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="ca11f-104">La inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, lo que facilita reemplazar las dependencias de un objeto, ya sea para las pruebas (mediante objetos ficticios) o para cambiar el comportamiento en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ca11f-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="ca11f-105">En este tutorial se muestra cómo realizar la inserción de dependencias en los concentradores de Signalr.</span><span class="sxs-lookup"><span data-stu-id="ca11f-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="ca11f-106">También se muestra cómo usar los contenedores de IoC con Signalr.</span><span class="sxs-lookup"><span data-stu-id="ca11f-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="ca11f-107">Un contenedor de IoC es un marco general para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="ca11f-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="ca11f-108">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="ca11f-108">What is Dependency Injection?</span></span>

<span data-ttu-id="ca11f-109">Omita esta sección si ya está familiarizado con la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="ca11f-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="ca11f-110">La *inserción de dependencias* (di) es un patrón en el que los objetos no son responsables de crear sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="ca11f-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="ca11f-111">Este es un ejemplo sencillo para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="ca11f-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="ca11f-112">Supongamos que tiene un objeto que necesita registrar los mensajes.</span><span class="sxs-lookup"><span data-stu-id="ca11f-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="ca11f-113">Puede definir una interfaz de registro:</span><span class="sxs-lookup"><span data-stu-id="ca11f-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="ca11f-114">En el objeto, puede crear una `ILogger` para registrar los mensajes:</span><span class="sxs-lookup"><span data-stu-id="ca11f-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="ca11f-115">Esto funciona, pero no es el mejor diseño.</span><span class="sxs-lookup"><span data-stu-id="ca11f-115">This works, but it's not the best design.</span></span> <span data-ttu-id="ca11f-116">Si desea reemplazar `FileLogger` por otra implementación `ILogger`, tendrá que modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="ca11f-117">Suponiendo que muchos otros objetos usan `FileLogger`, tendrá que cambiarlos todos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="ca11f-118">O bien, si decide crear `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ca11f-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="ca11f-119">Un mejor enfoque consiste en "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="ca11f-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="ca11f-120">Ahora, el objeto no es responsable de seleccionar el `ILogger` que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="ca11f-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="ca11f-121">Puede cambiar `ILogger` implementaciones sin cambiar los objetos que dependen de ella.</span><span class="sxs-lookup"><span data-stu-id="ca11f-121">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="ca11f-122">Este patrón se denomina [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="ca11f-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="ca11f-123">Otro patrón es la inyección del establecedor, donde se establece la dependencia a través de un método o una propiedad de establecedor.</span><span class="sxs-lookup"><span data-stu-id="ca11f-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="ca11f-124">Inserción de dependencias simple en Signalr</span><span class="sxs-lookup"><span data-stu-id="ca11f-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="ca11f-125">Considere la aplicación de chat del tutorial [Introducción con signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="ca11f-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="ca11f-126">Esta es la clase de concentrador de esa aplicación:</span><span class="sxs-lookup"><span data-stu-id="ca11f-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="ca11f-127">Supongamos que desea almacenar mensajes de chat en el servidor antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="ca11f-128">Podría definir una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en la clase `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="ca11f-129">El único problema es que una aplicación Signalr no crea centros directamente; Signalr los crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="ca11f-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="ca11f-130">De forma predeterminada, Signalr espera que una clase de concentrador tenga un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="ca11f-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="ca11f-131">Sin embargo, puede registrar fácilmente una función para crear instancias de concentrador y usar esta función para realizar la inserción.</span><span class="sxs-lookup"><span data-stu-id="ca11f-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="ca11f-132">Registre la función llamando a **host global. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="ca11f-133">Ahora Signalr invocará esta función anónima siempre que necesite crear una instancia de `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="ca11f-134">Contenedores de IoC</span><span class="sxs-lookup"><span data-stu-id="ca11f-134">IoC Containers</span></span>

<span data-ttu-id="ca11f-135">El código anterior está bien para casos sencillos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="ca11f-136">Pero tiene que escribir esto:</span><span class="sxs-lookup"><span data-stu-id="ca11f-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="ca11f-137">En una aplicación compleja con muchas dependencias, es posible que tenga que escribir gran parte de este código de "conexión".</span><span class="sxs-lookup"><span data-stu-id="ca11f-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="ca11f-138">Este código puede ser difícil de mantener, especialmente si las dependencias están anidadas.</span><span class="sxs-lookup"><span data-stu-id="ca11f-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="ca11f-139">También es difícil de hacer pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="ca11f-139">It is also hard to unit test.</span></span>

<span data-ttu-id="ca11f-140">Una solución consiste en usar un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="ca11f-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="ca11f-141">Un contenedor de IoC es un componente de software que es responsable de administrar las dependencias. Los tipos se registran con el contenedor y, a continuación, se usa el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="ca11f-142">El contenedor averigua automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="ca11f-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="ca11f-143">Muchos contenedores de IoC también permiten controlar aspectos como la duración y el ámbito de los objetos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="ca11f-144">"IoC" representa "inversion of control", que es un patrón general en el que un marco llama al código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ca11f-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="ca11f-145">Un contenedor de IoC crea los objetos automáticamente, lo que "invierte" el flujo de control habitual.</span><span class="sxs-lookup"><span data-stu-id="ca11f-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="ca11f-146">Uso de contenedores de IoC en Signalr</span><span class="sxs-lookup"><span data-stu-id="ca11f-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="ca11f-147">Probablemente, la aplicación de chat es demasiado simple para beneficiarse de un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="ca11f-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="ca11f-148">En su lugar, echemos un vistazo al ejemplo [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="ca11f-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="ca11f-149">El ejemplo de StockTicker define dos clases principales:</span><span class="sxs-lookup"><span data-stu-id="ca11f-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="ca11f-150">`StockTickerHub`: la clase de concentrador, que administra las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="ca11f-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="ca11f-151">`StockTicker`: singleton que contiene los precios de las acciones y los actualiza periódicamente.</span><span class="sxs-lookup"><span data-stu-id="ca11f-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="ca11f-152">`StockTickerHub` contiene una referencia al `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a **IHubConnectionContext** para el `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="ca11f-153">Utiliza esta interfaz para comunicarse con instancias de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="ca11f-154">(Para obtener más información, consulte [difusión de servidor con ASP.net signalr](index.md)).</span><span class="sxs-lookup"><span data-stu-id="ca11f-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="ca11f-155">Podemos usar un contenedor de IoC para desenredar estas dependencias de un bit.</span><span class="sxs-lookup"><span data-stu-id="ca11f-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="ca11f-156">En primer lugar, vamos a simplificar las clases `StockTickerHub` y `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="ca11f-157">En el código siguiente, he comentado las partes que no necesitamos.</span><span class="sxs-lookup"><span data-stu-id="ca11f-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="ca11f-158">Quite el constructor sin parámetros de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="ca11f-159">En su lugar, usaremos siempre DI para crear el centro.</span><span class="sxs-lookup"><span data-stu-id="ca11f-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="ca11f-160">Para StockTicker, quite la instancia singleton.</span><span class="sxs-lookup"><span data-stu-id="ca11f-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="ca11f-161">Más adelante, usaremos el contenedor de IoC para controlar la duración del StockTicker.</span><span class="sxs-lookup"><span data-stu-id="ca11f-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="ca11f-162">Además, haga que el constructor sea público.</span><span class="sxs-lookup"><span data-stu-id="ca11f-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="ca11f-163">A continuación, se puede refactorizar el código mediante la creación de una interfaz para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="ca11f-164">Usaremos esta interfaz para desacoplar el `StockTickerHub` de la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="ca11f-165">Visual Studio facilita este tipo de refactorización.</span><span class="sxs-lookup"><span data-stu-id="ca11f-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="ca11f-166">Abra el archivo StockTicker.cs, haga clic con el botón derecho en la declaración de clase `StockTicker` y seleccione **refactorizar** ... **Extraer interfaz**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="ca11f-167">En el cuadro de diálogo **Extraer interfaz** , haga clic en **seleccionar todo**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="ca11f-168">Deje los demás valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="ca11f-168">Leave the other defaults.</span></span> <span data-ttu-id="ca11f-169">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="ca11f-170">Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` para derivar de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="ca11f-171">Abra el archivo IStockTicker.cs y cambie la interfaz a **público**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="ca11f-172">En la clase `StockTickerHub`, cambie las dos instancias de `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="ca11f-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="ca11f-173">Crear una interfaz de `IStockTicker` no es estrictamente necesario, pero quería mostrar cómo DI puede ayudar a reducir el acoplamiento entre los componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ca11f-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="ca11f-174">Agregar la biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="ca11f-174">Add the Ninject Library</span></span>

<span data-ttu-id="ca11f-175">Hay muchos contenedores de IoC de código abierto para .NET.</span><span class="sxs-lookup"><span data-stu-id="ca11f-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="ca11f-176">En este tutorial, usaremos [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="ca11f-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="ca11f-177">(Otras bibliotecas populares incluyen el ejemplo de la [Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)y [StructureMap](http://docs.structuremap.net)).</span><span class="sxs-lookup"><span data-stu-id="ca11f-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="ca11f-178">Use el administrador de paquetes NuGet para instalar la [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="ca11f-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="ca11f-179">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet** > **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-179">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="ca11f-180">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ca11f-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="ca11f-181">Reemplazar el solucionador de dependencias de Signalr</span><span class="sxs-lookup"><span data-stu-id="ca11f-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="ca11f-182">Para usar Ninject en Signalr, cree una clase que derive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="ca11f-183">Esta clase invalida los métodos **GetService** y **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="ca11f-184">Signalr llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias de Hub, así como varios servicios utilizados internamente por Signalr.</span><span class="sxs-lookup"><span data-stu-id="ca11f-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="ca11f-185">El método **GetService** crea una única instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="ca11f-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="ca11f-186">Invalide este método para llamar al método **TryGet** del kernel de Ninject.</span><span class="sxs-lookup"><span data-stu-id="ca11f-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="ca11f-187">Si ese método devuelve null, revierte al solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ca11f-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="ca11f-188">El método **GetServices** crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="ca11f-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="ca11f-189">Invalide este método para concatenar los resultados de Ninject con los resultados del solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ca11f-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="ca11f-190">Configuración de enlaces de Ninject</span><span class="sxs-lookup"><span data-stu-id="ca11f-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="ca11f-191">Ahora usaremos Ninject para declarar enlaces de tipo.</span><span class="sxs-lookup"><span data-stu-id="ca11f-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="ca11f-192">Abra el archivo RegisterHubs.cs.</span><span class="sxs-lookup"><span data-stu-id="ca11f-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="ca11f-193">En el método `RegisterHubs.Start`, cree el contenedor Ninject, que Ninject llama al *kernel*.</span><span class="sxs-lookup"><span data-stu-id="ca11f-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="ca11f-194">Cree una instancia de la resolución de dependencias personalizada:</span><span class="sxs-lookup"><span data-stu-id="ca11f-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="ca11f-195">Cree un enlace para `IStockTicker` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ca11f-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="ca11f-196">Este código dice dos cosas.</span><span class="sxs-lookup"><span data-stu-id="ca11f-196">This code is saying two things.</span></span> <span data-ttu-id="ca11f-197">En primer lugar, cada vez que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="ca11f-198">En segundo lugar, la clase `StockTicker` debe crearse como un objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="ca11f-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="ca11f-199">Ninject creará una instancia del objeto y devolverá la misma instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="ca11f-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="ca11f-200">Cree un enlace para **IHubConnectionContext** como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="ca11f-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="ca11f-201">Este código crea una función anónima que devuelve un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="ca11f-201">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="ca11f-202">El método **WhenInjectedInto** indica a Ninject que use esta función solo al crear instancias de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="ca11f-203">La razón es que Signalr crea las instancias de **IHubConnectionContext** internamente y no queremos invalidar el modo en que signalr las crea.</span><span class="sxs-lookup"><span data-stu-id="ca11f-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="ca11f-204">Esta función solo se aplica a la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="ca11f-205">Pase el solucionador de dependencias en el método **MapHubs** :</span><span class="sxs-lookup"><span data-stu-id="ca11f-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="ca11f-206">Ahora Signalr usará el solucionador especificado en **MapHubs**, en lugar del solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ca11f-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="ca11f-207">Esta es la lista de código completa para `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="ca11f-208">Para ejecutar la aplicación StockTicker en Visual Studio, presione F5.</span><span class="sxs-lookup"><span data-stu-id="ca11f-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="ca11f-209">En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="ca11f-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="ca11f-210">La aplicación tiene exactamente la misma funcionalidad que antes.</span><span class="sxs-lookup"><span data-stu-id="ca11f-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="ca11f-211">(Para obtener una descripción, consulte [difusión de servidor con ASP.net signalr](index.md)). No hemos cambiado el comportamiento. basta con hacer que el código sea más fácil de probar, mantener y desarrollar.</span><span class="sxs-lookup"><span data-stu-id="ca11f-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
