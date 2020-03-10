---
uid: signalr/overview/advanced/dependency-injection
title: Inserción de dependencias en Signalr | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 52978b10b6c131ac8eff4535216cc60b43fdf3de
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431869"
---
# <a name="dependency-injection-in-signalr"></a><span data-ttu-id="b2732-103">Inserción de dependencias en SignalR</span><span class="sxs-lookup"><span data-stu-id="b2732-103">Dependency Injection in SignalR</span></span>

<span data-ttu-id="b2732-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b2732-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b2732-105">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="b2732-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="b2732-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b2732-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b2732-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b2732-107">.NET 4.5</span></span>
> - <span data-ttu-id="b2732-108">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="b2732-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b2732-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="b2732-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="b2732-110">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b2732-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="b2732-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="b2732-111">Questions and comments</span></span>
>
> <span data-ttu-id="b2732-112">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="b2732-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b2732-113">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b2732-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="b2732-114">La inserción de dependencias es una manera de quitar las dependencias codificadas de forma rígida entre objetos, lo que facilita reemplazar las dependencias de un objeto, ya sea para las pruebas (mediante objetos ficticios) o para cambiar el comportamiento en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b2732-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="b2732-115">En este tutorial se muestra cómo realizar la inserción de dependencias en los concentradores de Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2732-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="b2732-116">También se muestra cómo usar los contenedores de IoC con Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2732-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="b2732-117">Un contenedor de IoC es un marco general para la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b2732-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="b2732-118">¿Qué es la inserción de dependencias?</span><span class="sxs-lookup"><span data-stu-id="b2732-118">What is Dependency Injection?</span></span>

<span data-ttu-id="b2732-119">Omita esta sección si ya está familiarizado con la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="b2732-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="b2732-120">La *inserción de dependencias* (di) es un patrón en el que los objetos no son responsables de crear sus propias dependencias.</span><span class="sxs-lookup"><span data-stu-id="b2732-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="b2732-121">Este es un ejemplo sencillo para motivar DI.</span><span class="sxs-lookup"><span data-stu-id="b2732-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="b2732-122">Supongamos que tiene un objeto que necesita registrar los mensajes.</span><span class="sxs-lookup"><span data-stu-id="b2732-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="b2732-123">Puede definir una interfaz de registro:</span><span class="sxs-lookup"><span data-stu-id="b2732-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="b2732-124">En el objeto, puede crear una `ILogger` para registrar los mensajes:</span><span class="sxs-lookup"><span data-stu-id="b2732-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="b2732-125">Esto funciona, pero no es el mejor diseño.</span><span class="sxs-lookup"><span data-stu-id="b2732-125">This works, but it's not the best design.</span></span> <span data-ttu-id="b2732-126">Si desea reemplazar `FileLogger` por otra implementación `ILogger`, tendrá que modificar `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="b2732-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="b2732-127">Suponiendo que muchos otros objetos usan `FileLogger`, tendrá que cambiarlos todos.</span><span class="sxs-lookup"><span data-stu-id="b2732-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="b2732-128">O bien, si decide crear `FileLogger` un singleton, también tendrá que realizar cambios en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b2732-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="b2732-129">Un mejor enfoque consiste en "Insertar" un `ILogger` en el objeto, por ejemplo, mediante un argumento de constructor:</span><span class="sxs-lookup"><span data-stu-id="b2732-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="b2732-130">Ahora, el objeto no es responsable de seleccionar el `ILogger` que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="b2732-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="b2732-131">Puede cambiar `ILogger` implementaciones sin cambiar los objetos que dependen de ella.</span><span class="sxs-lookup"><span data-stu-id="b2732-131">You can switch `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="b2732-132">Este patrón se denomina [inyección de constructor](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="b2732-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="b2732-133">Otro patrón es la inyección del establecedor, donde se establece la dependencia a través de un método o una propiedad de establecedor.</span><span class="sxs-lookup"><span data-stu-id="b2732-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="b2732-134">Inserción de dependencias simple en Signalr</span><span class="sxs-lookup"><span data-stu-id="b2732-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="b2732-135">Considere la aplicación de chat del tutorial [Introducción con signalr](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="b2732-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="b2732-136">Esta es la clase de concentrador de esa aplicación:</span><span class="sxs-lookup"><span data-stu-id="b2732-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="b2732-137">Supongamos que desea almacenar mensajes de chat en el servidor antes de enviarlos.</span><span class="sxs-lookup"><span data-stu-id="b2732-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="b2732-138">Podría definir una interfaz que abstrae esta funcionalidad y usar DI para insertar la interfaz en la clase `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="b2732-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="b2732-139">El único problema es que una aplicación Signalr no crea centros directamente; Signalr los crea automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b2732-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="b2732-140">De forma predeterminada, Signalr espera que una clase de concentrador tenga un constructor sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="b2732-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="b2732-141">Sin embargo, puede registrar fácilmente una función para crear instancias de concentrador y usar esta función para realizar la inserción.</span><span class="sxs-lookup"><span data-stu-id="b2732-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="b2732-142">Registre la función llamando a **host global. DependencyResolver. Register**.</span><span class="sxs-lookup"><span data-stu-id="b2732-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="b2732-143">Ahora Signalr invocará esta función anónima siempre que necesite crear una instancia de `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="b2732-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="b2732-144">Contenedores de IoC</span><span class="sxs-lookup"><span data-stu-id="b2732-144">IoC Containers</span></span>

<span data-ttu-id="b2732-145">El código anterior está bien para casos sencillos.</span><span class="sxs-lookup"><span data-stu-id="b2732-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="b2732-146">Pero tiene que escribir esto:</span><span class="sxs-lookup"><span data-stu-id="b2732-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="b2732-147">En una aplicación compleja con muchas dependencias, es posible que tenga que escribir gran parte de este código de "conexión".</span><span class="sxs-lookup"><span data-stu-id="b2732-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="b2732-148">Este código puede ser difícil de mantener, especialmente si las dependencias están anidadas.</span><span class="sxs-lookup"><span data-stu-id="b2732-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="b2732-149">También es difícil de hacer pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="b2732-149">It is also hard to unit test.</span></span>

<span data-ttu-id="b2732-150">Una solución consiste en usar un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="b2732-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="b2732-151">Un contenedor de IoC es un componente de software que es responsable de administrar las dependencias. Los tipos se registran con el contenedor y, a continuación, se usa el contenedor para crear objetos.</span><span class="sxs-lookup"><span data-stu-id="b2732-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="b2732-152">El contenedor averigua automáticamente las relaciones de dependencia.</span><span class="sxs-lookup"><span data-stu-id="b2732-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="b2732-153">Muchos contenedores de IoC también permiten controlar aspectos como la duración y el ámbito de los objetos.</span><span class="sxs-lookup"><span data-stu-id="b2732-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="b2732-154">"IoC" representa "inversion of control", que es un patrón general en el que un marco llama al código de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b2732-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="b2732-155">Un contenedor de IoC crea los objetos automáticamente, lo que "invierte" el flujo de control habitual.</span><span class="sxs-lookup"><span data-stu-id="b2732-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>

## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="b2732-156">Uso de contenedores de IoC en Signalr</span><span class="sxs-lookup"><span data-stu-id="b2732-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="b2732-157">Probablemente, la aplicación de chat es demasiado simple para beneficiarse de un contenedor de IoC.</span><span class="sxs-lookup"><span data-stu-id="b2732-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="b2732-158">En su lugar, echemos un vistazo al ejemplo [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) .</span><span class="sxs-lookup"><span data-stu-id="b2732-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="b2732-159">El ejemplo de StockTicker define dos clases principales:</span><span class="sxs-lookup"><span data-stu-id="b2732-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="b2732-160">`StockTickerHub`: la clase de concentrador, que administra las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="b2732-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="b2732-161">`StockTicker`: singleton que contiene los precios de las acciones y los actualiza periódicamente.</span><span class="sxs-lookup"><span data-stu-id="b2732-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="b2732-162">`StockTickerHub` contiene una referencia al `StockTicker` singleton, mientras que `StockTicker` contiene una referencia a **IHubConnectionContext** para el `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="b2732-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="b2732-163">Utiliza esta interfaz para comunicarse con instancias de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="b2732-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="b2732-164">(Para obtener más información, consulte [difusión de servidor con ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md)).</span><span class="sxs-lookup"><span data-stu-id="b2732-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="b2732-165">Podemos usar un contenedor de IoC para desenredar estas dependencias de un bit.</span><span class="sxs-lookup"><span data-stu-id="b2732-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="b2732-166">En primer lugar, vamos a simplificar las clases `StockTickerHub` y `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="b2732-167">En el código siguiente, he comentado las partes que no necesitamos.</span><span class="sxs-lookup"><span data-stu-id="b2732-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="b2732-168">Quite el constructor sin parámetros de `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="b2732-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="b2732-169">En su lugar, usaremos siempre DI para crear el centro.</span><span class="sxs-lookup"><span data-stu-id="b2732-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="b2732-170">Para StockTicker, quite la instancia singleton.</span><span class="sxs-lookup"><span data-stu-id="b2732-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="b2732-171">Más adelante, usaremos el contenedor de IoC para controlar la duración del StockTicker.</span><span class="sxs-lookup"><span data-stu-id="b2732-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="b2732-172">Además, haga que el constructor sea público.</span><span class="sxs-lookup"><span data-stu-id="b2732-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="b2732-173">A continuación, se puede refactorizar el código mediante la creación de una interfaz para `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="b2732-174">Usaremos esta interfaz para desacoplar el `StockTickerHub` de la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="b2732-175">Visual Studio facilita este tipo de refactorización.</span><span class="sxs-lookup"><span data-stu-id="b2732-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="b2732-176">Abra el archivo StockTicker.cs, haga clic con el botón derecho en la declaración de clase `StockTicker` y seleccione **refactorizar** ... **Extraer interfaz**.</span><span class="sxs-lookup"><span data-stu-id="b2732-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="b2732-177">En el cuadro de diálogo **Extraer interfaz** , haga clic en **seleccionar todo**.</span><span class="sxs-lookup"><span data-stu-id="b2732-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="b2732-178">Deje los demás valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="b2732-178">Leave the other defaults.</span></span> <span data-ttu-id="b2732-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b2732-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="b2732-180">Visual Studio crea una nueva interfaz denominada `IStockTicker`y también cambia `StockTicker` para derivar de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="b2732-181">Abra el archivo IStockTicker.cs y cambie la interfaz a **público**.</span><span class="sxs-lookup"><span data-stu-id="b2732-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="b2732-182">En la clase `StockTickerHub`, cambie las dos instancias de `StockTicker` a `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="b2732-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="b2732-183">Crear una interfaz de `IStockTicker` no es estrictamente necesario, pero quería mostrar cómo DI puede ayudar a reducir el acoplamiento entre los componentes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b2732-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="b2732-184">Agregar la biblioteca Ninject</span><span class="sxs-lookup"><span data-stu-id="b2732-184">Add the Ninject Library</span></span>

<span data-ttu-id="b2732-185">Hay muchos contenedores de IoC de código abierto para .NET.</span><span class="sxs-lookup"><span data-stu-id="b2732-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="b2732-186">En este tutorial, usaremos [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="b2732-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="b2732-187">(Otras bibliotecas populares incluyen el ejemplo de la [Windsor](http://www.castleproject.org/), [Spring.net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity)y [StructureMap](http://docs.structuremap.net)).</span><span class="sxs-lookup"><span data-stu-id="b2732-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="b2732-188">Use el administrador de paquetes NuGet para instalar la [biblioteca Ninject](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="b2732-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="b2732-189">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet** > **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="b2732-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="b2732-190">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b2732-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="b2732-191">Reemplazar el solucionador de dependencias de Signalr</span><span class="sxs-lookup"><span data-stu-id="b2732-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="b2732-192">Para usar Ninject en Signalr, cree una clase que derive de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b2732-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="b2732-193">Esta clase invalida los métodos **GetService** y **GetServices** de **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="b2732-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="b2732-194">Signalr llama a estos métodos para crear varios objetos en tiempo de ejecución, incluidas las instancias de Hub, así como varios servicios utilizados internamente por Signalr.</span><span class="sxs-lookup"><span data-stu-id="b2732-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="b2732-195">El método **GetService** crea una única instancia de un tipo.</span><span class="sxs-lookup"><span data-stu-id="b2732-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="b2732-196">Invalide este método para llamar al método **TryGet** del kernel de Ninject.</span><span class="sxs-lookup"><span data-stu-id="b2732-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="b2732-197">Si ese método devuelve null, revierte al solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b2732-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="b2732-198">El método **GetServices** crea una colección de objetos de un tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="b2732-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="b2732-199">Invalide este método para concatenar los resultados de Ninject con los resultados del solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b2732-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="b2732-200">Configuración de enlaces de Ninject</span><span class="sxs-lookup"><span data-stu-id="b2732-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="b2732-201">Ahora usaremos Ninject para declarar enlaces de tipo.</span><span class="sxs-lookup"><span data-stu-id="b2732-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="b2732-202">Abra la clase Startup.cs de la aplicación (que creó manualmente según las instrucciones del paquete en `readme.txt`, o que se creó agregando la autenticación al proyecto).</span><span class="sxs-lookup"><span data-stu-id="b2732-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="b2732-203">En el método `Startup.Configuration`, cree el contenedor Ninject, que Ninject llama al *kernel*.</span><span class="sxs-lookup"><span data-stu-id="b2732-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="b2732-204">Cree una instancia de la resolución de dependencias personalizada:</span><span class="sxs-lookup"><span data-stu-id="b2732-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="b2732-205">Cree un enlace para `IStockTicker` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="b2732-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="b2732-206">Este código dice dos cosas.</span><span class="sxs-lookup"><span data-stu-id="b2732-206">This code is saying two things.</span></span> <span data-ttu-id="b2732-207">En primer lugar, cada vez que la aplicación necesita un `IStockTicker`, el kernel debe crear una instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="b2732-208">En segundo lugar, la clase `StockTicker` debe crearse como un objeto singleton.</span><span class="sxs-lookup"><span data-stu-id="b2732-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="b2732-209">Ninject creará una instancia del objeto y devolverá la misma instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="b2732-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="b2732-210">Cree un enlace para **IHubConnectionContext** como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="b2732-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="b2732-211">Este código crea una función anónima que devuelve un **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="b2732-211">This code creates an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="b2732-212">El método **WhenInjectedInto** indica a Ninject que use esta función solo al crear instancias de `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="b2732-213">La razón es que Signalr crea las instancias de **IHubConnectionContext** internamente y no queremos invalidar el modo en que signalr las crea.</span><span class="sxs-lookup"><span data-stu-id="b2732-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="b2732-214">Esta función solo se aplica a la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="b2732-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="b2732-215">Pase la resolución de dependencia al método **MapSignalR** agregando una configuración de concentrador:</span><span class="sxs-lookup"><span data-stu-id="b2732-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="b2732-216">Actualice el método startup. ConfigureSignalR en la clase de inicio del ejemplo con el nuevo parámetro:</span><span class="sxs-lookup"><span data-stu-id="b2732-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="b2732-217">Ahora Signalr usará el solucionador especificado en **MapSignalR**, en lugar del solucionador predeterminado.</span><span class="sxs-lookup"><span data-stu-id="b2732-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="b2732-218">Esta es la lista de código completa para `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="b2732-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="b2732-219">Para ejecutar la aplicación StockTicker en Visual Studio, presione F5.</span><span class="sxs-lookup"><span data-stu-id="b2732-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="b2732-220">En la ventana del explorador, vaya a `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="b2732-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="b2732-221">La aplicación tiene exactamente la misma funcionalidad que antes.</span><span class="sxs-lookup"><span data-stu-id="b2732-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="b2732-222">(Para obtener una descripción, consulte [difusión de servidor con ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md)). No hemos cambiado el comportamiento. basta con hacer que el código sea más fácil de probar, mantener y desarrollar.</span><span class="sxs-lookup"><span data-stu-id="b2732-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
