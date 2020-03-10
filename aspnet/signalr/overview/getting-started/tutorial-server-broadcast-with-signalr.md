---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutorial: difusión de servidores con Signalr 2 | Microsoft Docs'
author: tdykstra
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión del servidor.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14924109fff8db3e537e6bc08b6dc868792ee660
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431245"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="53c36-103">Tutorial: difusión de servidores con Signalr 2</span><span class="sxs-lookup"><span data-stu-id="53c36-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="53c36-104">En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión del servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="53c36-105">Difusión de servidor significa que el servidor inicia las comunicaciones enviadas a los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="53c36-106">La aplicación que creará en este tutorial simula un tablero de cotizaciones, un escenario típico para la funcionalidad de difusión de servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="53c36-107">Periódicamente, el servidor actualiza aleatoriamente los precios de las acciones y difunde las actualizaciones a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="53c36-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="53c36-108">En el explorador, los números y los símbolos de las columnas **cambiar** y **%** cambian dinámicamente en respuesta a las notificaciones del servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="53c36-109">Si abre más exploradores en la misma dirección URL, todos ellos muestran los mismos datos y los mismos cambios en los datos simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="53c36-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Crear Web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="53c36-111">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="53c36-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53c36-112">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="53c36-112">Create the project</span></span>
> * <span data-ttu-id="53c36-113">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-113">Set up the server code</span></span>
> * <span data-ttu-id="53c36-114">Examinar el código del servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-114">Examine the server code</span></span>
> * <span data-ttu-id="53c36-115">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-115">Set up the client code</span></span>
> * <span data-ttu-id="53c36-116">Examinar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-116">Examine the client code</span></span>
> * <span data-ttu-id="53c36-117">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="53c36-117">Test the application</span></span>
> * <span data-ttu-id="53c36-118">Habilite el registro</span><span class="sxs-lookup"><span data-stu-id="53c36-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53c36-119">Si no desea trabajar a través de los pasos necesarios para compilar la aplicación, puede instalar el paquete Signalr. Sample en un nuevo proyecto de aplicación Web ASP.NET vacío.</span><span class="sxs-lookup"><span data-stu-id="53c36-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="53c36-120">Si instala el paquete de NuGet sin realizar los pasos de este tutorial, debe seguir las instrucciones del archivo *README. txt* .</span><span class="sxs-lookup"><span data-stu-id="53c36-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="53c36-121">Para ejecutar el paquete, debe agregar una clase de inicio OWIN que llame al método `ConfigureSignalR` del paquete instalado.</span><span class="sxs-lookup"><span data-stu-id="53c36-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="53c36-122">Recibirá un error si no agrega la clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="53c36-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="53c36-123">Consulte la sección [instalación del ejemplo de StockTicker](#install-the-stockticker-sample) de este artículo.</span><span class="sxs-lookup"><span data-stu-id="53c36-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53c36-124">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="53c36-124">Prerequisites</span></span>

* <span data-ttu-id="53c36-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.</span><span class="sxs-lookup"><span data-stu-id="53c36-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="53c36-126">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="53c36-126">Create the project</span></span>

<span data-ttu-id="53c36-127">En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía.</span><span class="sxs-lookup"><span data-stu-id="53c36-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="53c36-128">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="53c36-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crear Web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="53c36-130">En la ventana **New ASP.NET Web Application-signalr. StockTicker** , deje **vacío** seleccionado y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="53c36-131">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-131">Set up the server code</span></span>

<span data-ttu-id="53c36-132">En esta sección, configurará el código que se ejecuta en el servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="53c36-133">Crear la clase stock</span><span class="sxs-lookup"><span data-stu-id="53c36-133">Create the Stock class</span></span>

<span data-ttu-id="53c36-134">Empiece por crear la clase del modelo de *existencias* que usará para almacenar y transmitir información acerca de una cotización.</span><span class="sxs-lookup"><span data-stu-id="53c36-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="53c36-135">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .</span><span class="sxs-lookup"><span data-stu-id="53c36-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="53c36-136">Asigne a la clase el nombre *stock* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="53c36-137">Reemplace el código del archivo *stock.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="53c36-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="53c36-138">Las dos propiedades que establecerá al crear acciones son `Symbol` (por ejemplo, MSFT para Microsoft) y `Price`.</span><span class="sxs-lookup"><span data-stu-id="53c36-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="53c36-139">Las demás propiedades dependen de cómo y cuándo se establece `Price`.</span><span class="sxs-lookup"><span data-stu-id="53c36-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="53c36-140">La primera vez que se establece `Price`, el valor se propaga a `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="53c36-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="53c36-141">Después, al establecer `Price`, la aplicación calcula los valores de las propiedades `Change` y `PercentChange` en función de la diferencia entre `Price` y `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="53c36-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="53c36-142">Creación de las clases StockTickerHub y StockTicker</span><span class="sxs-lookup"><span data-stu-id="53c36-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="53c36-143">Usará la API de Signalr hub para controlar la interacción entre el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="53c36-144">Una clase `StockTickerHub` que se deriva de la clase Signalr `Hub` controlará las conexiones de recepción y las llamadas a métodos de los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-144">A `StockTickerHub` class that derives from the SignalR `Hub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="53c36-145">También debe mantener los datos de stock y ejecutar un objeto de `Timer`.</span><span class="sxs-lookup"><span data-stu-id="53c36-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="53c36-146">El objeto `Timer` desencadenará periódicamente actualizaciones de precios independientes de las conexiones de cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="53c36-147">Estas funciones no se pueden colocar en una clase `Hub`, ya que los concentradores son transitorios.</span><span class="sxs-lookup"><span data-stu-id="53c36-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="53c36-148">La aplicación crea una instancia de clase `Hub` para cada tarea en el concentrador, como conexiones y llamadas desde el cliente al servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="53c36-149">Por lo tanto, el mecanismo que mantiene los datos de existencias, actualiza los precios y difunde las actualizaciones de precios tiene que ejecutarse en una clase independiente.</span><span class="sxs-lookup"><span data-stu-id="53c36-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="53c36-150">El nombre de la clase `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="53c36-150">You'll name the class `StockTicker`.</span></span>

![Difusión desde StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="53c36-152">Solo desea que una instancia de la clase `StockTicker` se ejecute en el servidor, por lo que tendrá que configurar una referencia de cada instancia de `StockTickerHub` a la instancia de `StockTicker` singleton.</span><span class="sxs-lookup"><span data-stu-id="53c36-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="53c36-153">La clase `StockTicker` tiene que difundir a los clientes porque tiene los datos bursátiles y desencadena actualizaciones, pero `StockTicker` no es una clase `Hub`.</span><span class="sxs-lookup"><span data-stu-id="53c36-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="53c36-154">La clase `StockTicker` tiene que obtener una referencia al objeto de contexto de conexión del concentrador de Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="53c36-155">Después, puede usar el objeto de contexto de conexión de Signalr para difundir a los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="53c36-156">Create StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="53c36-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="53c36-157">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="53c36-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="53c36-158">En **Agregar nuevo elemento-signalr. StockTicker**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .</span><span class="sxs-lookup"><span data-stu-id="53c36-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="53c36-159">Asigne a la clase el nombre *StockTickerHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="53c36-160">En este paso se crea el archivo de clase *StockTickerHub.CS* .</span><span class="sxs-lookup"><span data-stu-id="53c36-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="53c36-161">Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admite Signalr en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="53c36-162">Reemplace el código del archivo *StockTickerHub.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="53c36-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="53c36-163">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="53c36-163">Save the file.</span></span>

<span data-ttu-id="53c36-164">La aplicación usa la clase [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) para definir los métodos a los que los clientes pueden llamar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="53c36-165">Está definiendo un método: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="53c36-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="53c36-166">Cuando un cliente se conecta inicialmente al servidor, llamará a este método para obtener una lista de todas las acciones con sus precios actuales.</span><span class="sxs-lookup"><span data-stu-id="53c36-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="53c36-167">El método se puede ejecutar sincrónicamente y devolver `IEnumerable<Stock>` porque está devolviendo datos de la memoria.</span><span class="sxs-lookup"><span data-stu-id="53c36-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="53c36-168">Si el método tuviera que obtener los datos haciendo algo que implique la espera, como una búsqueda en la base de datos o una llamada de servicio Web, especificaría `Task<IEnumerable<Stock>>` como el valor devuelto para habilitar el procesamiento asincrónico.</span><span class="sxs-lookup"><span data-stu-id="53c36-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="53c36-169">Para obtener más información, consulte [Guía de la API de ASP.net signalr hubs-Server-when para ejecutar de forma asincrónica](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="53c36-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="53c36-170">El atributo `HubName` especifica cómo la aplicación hará referencia al centro en el código JavaScript en el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="53c36-171">El nombre predeterminado en el cliente si no se usa este atributo, es una versión camelCase del nombre de clase, que en este caso sería `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="53c36-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="53c36-172">Como verá más adelante cuando cree la clase `StockTicker`, la aplicación crea una instancia singleton de esa clase en su propiedad `Instance` estática.</span><span class="sxs-lookup"><span data-stu-id="53c36-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="53c36-173">Esa instancia singleton de `StockTicker` está en memoria independientemente de cuántos clientes se conectan o desconectan.</span><span class="sxs-lookup"><span data-stu-id="53c36-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="53c36-174">Esa instancia es lo que el método `GetAllStocks()` utiliza para devolver la información de existencias actual.</span><span class="sxs-lookup"><span data-stu-id="53c36-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="53c36-175">Crear StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="53c36-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="53c36-176">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .</span><span class="sxs-lookup"><span data-stu-id="53c36-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="53c36-177">Asigne a la clase el nombre *StockTicker* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="53c36-178">Reemplace el código del archivo *StockTicker.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="53c36-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="53c36-179">Dado que todos los subprocesos ejecutarán la misma instancia de código StockTicker, la clase StockTicker debe ser segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="53c36-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="53c36-180">Examinar el código del servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-180">Examine the server code</span></span>

<span data-ttu-id="53c36-181">Si examina el código del servidor, le ayudará a comprender el funcionamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53c36-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="53c36-182">Almacenar la instancia Singleton en un campo estático</span><span class="sxs-lookup"><span data-stu-id="53c36-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="53c36-183">El código inicializa el campo `_instance` estático que respalda la propiedad `Instance` con una instancia de la clase.</span><span class="sxs-lookup"><span data-stu-id="53c36-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="53c36-184">Dado que el constructor es privado, es la única instancia de la clase que puede crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53c36-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="53c36-185">La aplicación usa la [inicialización diferida](/dotnet/framework/performance/lazy-initialization) para el campo `_instance`.</span><span class="sxs-lookup"><span data-stu-id="53c36-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="53c36-186">No es por motivos de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="53c36-186">It's not for performance reasons.</span></span> <span data-ttu-id="53c36-187">Es para asegurarse de que la creación de la instancia es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="53c36-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="53c36-188">Cada vez que un cliente se conecta al servidor, una nueva instancia de la clase StockTickerHub que se ejecuta en un subproceso independiente obtiene la instancia singleton de StockTicker de la `StockTicker.Instance` propiedad estática, como se vio anteriormente en la clase `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="53c36-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="53c36-189">Almacenar datos bursátiles en un ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="53c36-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="53c36-190">El constructor inicializa la colección de `_stocks` con algunos datos bursátiles de ejemplo y `GetAllStocks` devuelve las existencias.</span><span class="sxs-lookup"><span data-stu-id="53c36-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="53c36-191">Como vimos anteriormente, `StockTickerHub.GetAllStocks`devuelve esta colección de acciones, que es un método de servidor de la clase `Hub` que los clientes pueden llamar.</span><span class="sxs-lookup"><span data-stu-id="53c36-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="53c36-192">La colección de acciones se define como un tipo [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) para la seguridad para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="53c36-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="53c36-193">Como alternativa, puede usar un objeto de [Diccionario](https://msdn.microsoft.com/library/xfhwa508.aspx) y bloquear explícitamente el Diccionario al realizar cambios en él.</span><span class="sxs-lookup"><span data-stu-id="53c36-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="53c36-194">En esta aplicación de ejemplo, es correcto almacenar los datos de la aplicación en memoria y perder los datos cuando la aplicación desecha la instancia de `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="53c36-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="53c36-195">En una aplicación real, se podía trabajar con un almacén de datos back-end como una base de datos.</span><span class="sxs-lookup"><span data-stu-id="53c36-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="53c36-196">Actualizar periódicamente los precios de las acciones</span><span class="sxs-lookup"><span data-stu-id="53c36-196">Periodically updating stock prices</span></span>

<span data-ttu-id="53c36-197">El constructor inicia un objeto `Timer` que llama periódicamente a métodos que actualizan los precios de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="53c36-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="53c36-198">`Timer` llama a `UpdateStockPrices`, que pasa null en el parámetro State.</span><span class="sxs-lookup"><span data-stu-id="53c36-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="53c36-199">Antes de actualizar los precios, la aplicación toma un bloqueo en el objeto de `_updateStockPricesLock`.</span><span class="sxs-lookup"><span data-stu-id="53c36-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="53c36-200">El código comprueba si otro subproceso ya está actualizando los precios y, a continuación, llama a `TryUpdateStockPrice` en cada acción de la lista.</span><span class="sxs-lookup"><span data-stu-id="53c36-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="53c36-201">El método `TryUpdateStockPrice` decide si se debe cambiar el precio de las acciones y cuánto se debe cambiar.</span><span class="sxs-lookup"><span data-stu-id="53c36-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="53c36-202">Si cambia el precio de las acciones, la aplicación llama a `BroadcastStockPrice` para difundir el cambio de precio de las acciones a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="53c36-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="53c36-203">La marca `_updatingStockPrices` designada como [volátil](https://msdn.microsoft.com/library/x13ttww7.aspx) para asegurarse de que es segura para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="53c36-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="53c36-204">En una aplicación real, el método `TryUpdateStockPrice` llamaría a un servicio web para buscar el precio.</span><span class="sxs-lookup"><span data-stu-id="53c36-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="53c36-205">En este código, la aplicación usa un generador de números aleatorios para realizar cambios de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="53c36-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="53c36-206">Obtención del contexto de Signalr para que la clase StockTicker pueda difundirse a los clientes</span><span class="sxs-lookup"><span data-stu-id="53c36-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="53c36-207">Dado que los cambios de precio se originan aquí en el objeto `StockTicker`, es el objeto el que necesita llamar a un método `updateStockPrice` en todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="53c36-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="53c36-208">En una clase `Hub`, tiene una API para llamar a métodos de cliente, pero `StockTicker` no se deriva de la clase `Hub` y no tiene una referencia a ningún objeto `Hub`.</span><span class="sxs-lookup"><span data-stu-id="53c36-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="53c36-209">Para difundir a los clientes conectados, la clase `StockTicker` tiene que obtener la instancia de contexto de Signalr para la clase `StockTickerHub` y utilizarla para llamar a métodos en los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="53c36-210">El código obtiene una referencia al contexto de Signalr cuando crea la instancia de la clase singleton, pasa esa referencia al constructor y el constructor lo coloca en la propiedad `Clients`.</span><span class="sxs-lookup"><span data-stu-id="53c36-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="53c36-211">Hay dos razones por las que desea obtener el contexto solo una vez: obtener el contexto es una tarea costosa y obtenerlo una vez que se asegura de que la aplicación conserva el orden previsto de mensajes enviados a los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="53c36-212">Obtener la propiedad `Clients` del contexto y colocarla en el `StockTickerClient` propiedad permite escribir código para llamar a métodos de cliente que tienen el mismo aspecto que en una clase `Hub`.</span><span class="sxs-lookup"><span data-stu-id="53c36-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="53c36-213">Por ejemplo, para difundir a todos los clientes, puede escribir `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="53c36-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="53c36-214">El método de `updateStockPrice` al que está llamando en `BroadcastStockPrice` todavía no existe.</span><span class="sxs-lookup"><span data-stu-id="53c36-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="53c36-215">Lo agregará más adelante cuando escriba código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="53c36-216">Puede hacer referencia a `updateStockPrice` aquí porque `Clients.All` es dinámico, lo que significa que la aplicación evaluará la expresión en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="53c36-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="53c36-217">Cuando se ejecuta esta llamada al método, Signalr enviará el nombre del método y el valor del parámetro al cliente, y si el cliente tiene un método denominado `updateStockPrice`, la aplicación llamará a ese método y le pasará el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="53c36-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="53c36-218">`Clients.All` significa enviar a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="53c36-219">Signalr ofrece otras opciones para especificar los clientes o grupos de clientes a los que se va a enviar.</span><span class="sxs-lookup"><span data-stu-id="53c36-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="53c36-220">Para obtener más información, vea [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="53c36-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="53c36-221">Registro de la ruta de Signalr</span><span class="sxs-lookup"><span data-stu-id="53c36-221">Register the SignalR route</span></span>

<span data-ttu-id="53c36-222">El servidor debe saber qué dirección URL se va a interceptar y dirigir a Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="53c36-223">Para ello, agregue una clase de inicio OWIN:</span><span class="sxs-lookup"><span data-stu-id="53c36-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="53c36-224">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="53c36-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="53c36-225">En **Agregar nuevo elemento-signalr. StockTicker** seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="53c36-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="53c36-226">Asigne a la clase el nombre *Startup* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="53c36-227">Reemplace el código predeterminado del archivo *Startup.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="53c36-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="53c36-228">Ya ha terminado de configurar el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="53c36-229">En la siguiente sección, configurará el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="53c36-230">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-230">Set up the client code</span></span>

<span data-ttu-id="53c36-231">En esta sección, configurará el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="53c36-232">Crear la página HTML y el archivo JavaScript</span><span class="sxs-lookup"><span data-stu-id="53c36-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="53c36-233">En la página HTML se mostrarán los datos y el archivo JavaScript organizará los datos.</span><span class="sxs-lookup"><span data-stu-id="53c36-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="53c36-234">Create StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="53c36-234">Create StockTicker.html</span></span>

<span data-ttu-id="53c36-235">En primer lugar, agregará el cliente HTML.</span><span class="sxs-lookup"><span data-stu-id="53c36-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="53c36-236">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="53c36-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="53c36-237">Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="53c36-238">Reemplace el código predeterminado del archivo *StockTicker. html* por este código:</span><span class="sxs-lookup"><span data-stu-id="53c36-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="53c36-239">El código HTML crea una tabla con cinco columnas, una fila de encabezado y una fila de datos con una sola celda que abarca las cinco columnas.</span><span class="sxs-lookup"><span data-stu-id="53c36-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="53c36-240">La fila de datos muestra "cargando..." momentáneamente cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53c36-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="53c36-241">El código de JavaScript quitará esa fila y agregará las filas de su ubicación con los datos de stock recuperados del servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="53c36-242">Las etiquetas de script especifican:</span><span class="sxs-lookup"><span data-stu-id="53c36-242">The script tags specify:</span></span>

    * <span data-ttu-id="53c36-243">Archivo de script de jQuery.</span><span class="sxs-lookup"><span data-stu-id="53c36-243">The jQuery script file.</span></span>

    * <span data-ttu-id="53c36-244">Archivo de script de Signalr Core.</span><span class="sxs-lookup"><span data-stu-id="53c36-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="53c36-245">Archivo de script de proxy de Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="53c36-246">Un archivo de script de StockTicker que creará más adelante.</span><span class="sxs-lookup"><span data-stu-id="53c36-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="53c36-247">La aplicación genera dinámicamente el archivo de script de los proxies de Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="53c36-248">Especifica la dirección URL "/signalr/hubs" y define métodos de proxy para los métodos de la clase hub, en este caso, para `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="53c36-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="53c36-249">Si lo prefiere, puede generar este archivo JavaScript manualmente mediante las [utilidades de signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="53c36-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="53c36-250">No olvide deshabilitar la creación dinámica de archivos en la llamada al método `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="53c36-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="53c36-251">En **Explorador de soluciones**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="53c36-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="53c36-252">Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="53c36-253">El administrador de paquetes instalará una versión posterior de los scripts de Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="53c36-254">Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="53c36-255">En **Explorador de soluciones**, haga clic con el botón secundario en *StockTicker. html*y seleccione **establecer como página principal**.</span><span class="sxs-lookup"><span data-stu-id="53c36-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="53c36-256">Crear StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="53c36-256">Create StockTicker.js</span></span>

<span data-ttu-id="53c36-257">Ahora cree el archivo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="53c36-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="53c36-258">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **archivo JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="53c36-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="53c36-259">Asigne al archivo el nombre *StockTicker* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="53c36-260">Agregue este código al archivo *StockTicker. js* :</span><span class="sxs-lookup"><span data-stu-id="53c36-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="53c36-261">Examinar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-261">Examine the client code</span></span>

<span data-ttu-id="53c36-262">Si examina el código de cliente, le ayudará a saber cómo interactúa el código de cliente con el código de servidor para que la aplicación funcione.</span><span class="sxs-lookup"><span data-stu-id="53c36-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="53c36-263">Inicio de la conexión</span><span class="sxs-lookup"><span data-stu-id="53c36-263">Starting the connection</span></span>

<span data-ttu-id="53c36-264">`$.connection` hace referencia a los servidores proxy de Signalr.</span><span class="sxs-lookup"><span data-stu-id="53c36-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="53c36-265">El código obtiene una referencia al proxy para la clase `StockTickerHub` y la coloca en la variable `ticker`.</span><span class="sxs-lookup"><span data-stu-id="53c36-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="53c36-266">El nombre del proxy es el nombre que estableció el atributo `HubName`:</span><span class="sxs-lookup"><span data-stu-id="53c36-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="53c36-267">Después de definir todas las variables y funciones, la última línea de código del archivo inicializa la conexión Signalr llamando a la función Signalr `start`.</span><span class="sxs-lookup"><span data-stu-id="53c36-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="53c36-268">La función `start` se ejecuta de forma asincrónica y devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="53c36-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="53c36-269">Puede llamar a la función Done para especificar la función a la que se llama cuando la aplicación finaliza la acción asincrónica.</span><span class="sxs-lookup"><span data-stu-id="53c36-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="53c36-270">Obtención de todas las acciones</span><span class="sxs-lookup"><span data-stu-id="53c36-270">Getting all the stocks</span></span>

<span data-ttu-id="53c36-271">La función `init` llama a la función `getAllStocks` en el servidor y usa la información que el servidor devuelve para actualizar la tabla stock.</span><span class="sxs-lookup"><span data-stu-id="53c36-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="53c36-272">Tenga en cuenta que, de forma predeterminada, tiene que usar alterne en el cliente aunque el nombre del método sea Pascal-Case en el servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="53c36-273">La regla alterne solo se aplica a los métodos, no a los objetos.</span><span class="sxs-lookup"><span data-stu-id="53c36-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="53c36-274">Por ejemplo, se hace referencia a `stock.Symbol` y `stock.Price`, no `stock.symbol` o `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="53c36-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="53c36-275">En el método `init`, la aplicación crea HTML para una fila de la tabla para cada objeto estándar recibido del servidor mediante una llamada a `formatStock` para dar formato a las propiedades del objeto `stock` y, a continuación, al llamar a `supplant` para reemplazar los marcadores de posición en la variable `rowTemplate` por los valores de propiedad del objeto `stock`.</span><span class="sxs-lookup"><span data-stu-id="53c36-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="53c36-276">El HTML resultante se anexa a continuación a la tabla stock.</span><span class="sxs-lookup"><span data-stu-id="53c36-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="53c36-277">Llame a `init` pasándolo como una función de `callback` que se ejecuta después de que finalice la función de `start` asincrónica.</span><span class="sxs-lookup"><span data-stu-id="53c36-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="53c36-278">Si llamó a `init` como una instrucción de JavaScript independiente después de llamar a `start`, la función produciría un error porque se ejecutaría inmediatamente sin esperar a que la función de inicio finalizara el establecimiento de la conexión.</span><span class="sxs-lookup"><span data-stu-id="53c36-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="53c36-279">En ese caso, la función de `init` intentaría llamar a la función de `getAllStocks` antes de que la aplicación establezca una conexión de servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="53c36-280">Obtención de precios de cotización actualizados</span><span class="sxs-lookup"><span data-stu-id="53c36-280">Getting updated stock prices</span></span>

<span data-ttu-id="53c36-281">Cuando el servidor cambia el precio de una cotización, llama al `updateStockPrice` en los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="53c36-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="53c36-282">La aplicación agrega la función a la propiedad client del proxy de `stockTicker` para que esté disponible para las llamadas del servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="53c36-283">La función `updateStockPrice` da formato a un objeto stock recibido del servidor en una fila de la tabla de la misma forma que en la función `init`.</span><span class="sxs-lookup"><span data-stu-id="53c36-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="53c36-284">En lugar de anexar la fila a la tabla, busca la fila actual de las acciones en la tabla y la reemplaza por la nueva.</span><span class="sxs-lookup"><span data-stu-id="53c36-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="53c36-285">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="53c36-285">Test the application</span></span>

<span data-ttu-id="53c36-286">Puede probar la aplicación para asegurarse de que funciona.</span><span class="sxs-lookup"><span data-stu-id="53c36-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="53c36-287">Verá que todas las ventanas del explorador muestran la tabla de acciones en directo con los precios de las acciones fluctuando.</span><span class="sxs-lookup"><span data-stu-id="53c36-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="53c36-288">En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="53c36-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Captura de pantalla del usuario que activa el modo de depuración y selecciona reproducir.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="53c36-290">Se abrirá una ventana del explorador que muestra la **tabla de acciones en directo**.</span><span class="sxs-lookup"><span data-stu-id="53c36-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="53c36-291">La tabla Stock muestra inicialmente el valor "cargando..." la línea, después, después de un breve período, la aplicación muestra los datos de existencias iniciales y, a continuación, los precios de las acciones comienzan a cambiar.</span><span class="sxs-lookup"><span data-stu-id="53c36-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="53c36-292">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="53c36-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="53c36-293">La pantalla de acciones inicial es igual que el primer explorador y los cambios se producen simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="53c36-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="53c36-294">Cierre todos los exploradores, abra un nuevo explorador y vaya a la misma dirección URL.</span><span class="sxs-lookup"><span data-stu-id="53c36-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="53c36-295">El objeto singleton de StockTicker continuó ejecutándose en el servidor.</span><span class="sxs-lookup"><span data-stu-id="53c36-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="53c36-296">La **tabla de acciones en directo** muestra que las existencias han ido a cambiar.</span><span class="sxs-lookup"><span data-stu-id="53c36-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="53c36-297">No se ve la tabla inicial con cero figuras de cambios.</span><span class="sxs-lookup"><span data-stu-id="53c36-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="53c36-298">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="53c36-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="53c36-299">Habilite el registro</span><span class="sxs-lookup"><span data-stu-id="53c36-299">Enable logging</span></span>

<span data-ttu-id="53c36-300">Signalr tiene una función de registro integrada que puede habilitar en el cliente para ayudar en la solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="53c36-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="53c36-301">En esta sección, habilitará el registro y verá ejemplos que muestran cómo los registros le indican cuál de los siguientes métodos de transporte señalizador está usando:</span><span class="sxs-lookup"><span data-stu-id="53c36-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="53c36-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), admitidos por IIS 8 y exploradores actuales.</span><span class="sxs-lookup"><span data-stu-id="53c36-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="53c36-303">[Eventos enviados por el servidor](http://en.wikipedia.org/wiki/Server-sent_events), admitidos por exploradores distintos de Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="53c36-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="53c36-304">El [marco Forever](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), compatible con Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="53c36-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="53c36-305">[Sondeo prolongado de Ajax](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), compatible con todos los exploradores.</span><span class="sxs-lookup"><span data-stu-id="53c36-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="53c36-306">Para una conexión determinada, Signalr elige el mejor método de transporte que admiten tanto el servidor como el cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="53c36-307">Abra *StockTicker. js*.</span><span class="sxs-lookup"><span data-stu-id="53c36-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="53c36-308">Agregue esta línea de código resaltada para habilitar el registro inmediatamente antes del código que inicializa la conexión al final del archivo:</span><span class="sxs-lookup"><span data-stu-id="53c36-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="53c36-309">Presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="53c36-310">Abra la ventana herramientas de desarrollo del explorador y seleccione la consola para ver los registros.</span><span class="sxs-lookup"><span data-stu-id="53c36-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="53c36-311">Es posible que tenga que actualizar la página para ver los registros de Signalr que negocian el método de transporte para una nueva conexión.</span><span class="sxs-lookup"><span data-stu-id="53c36-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="53c36-312">Si está ejecutando Internet Explorer 10 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="53c36-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="53c36-313">Si está ejecutando Internet Explorer 10 en Windows 7 (IIS 7,5), el método de transporte es **iframe**.</span><span class="sxs-lookup"><span data-stu-id="53c36-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="53c36-314">Si está ejecutando Firefox 19 en Windows 8 (IIS 8), el método de transporte es **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="53c36-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="53c36-315">En Firefox, instale el complemento de Firebug para obtener una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="53c36-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="53c36-316">Si está ejecutando Firefox 19 en Windows 7 (IIS 7,5), el método de transporte es eventos **enviados por el servidor** .</span><span class="sxs-lookup"><span data-stu-id="53c36-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="53c36-317">Instalación del ejemplo de StockTicker</span><span class="sxs-lookup"><span data-stu-id="53c36-317">Install the StockTicker sample</span></span>

<span data-ttu-id="53c36-318">[Microsoft. Aspnet. signalr. Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instala la aplicación StockTicker.</span><span class="sxs-lookup"><span data-stu-id="53c36-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="53c36-319">El paquete NuGet incluye más características que la versión simplificada que creó desde cero.</span><span class="sxs-lookup"><span data-stu-id="53c36-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="53c36-320">En esta sección del tutorial, instalará el paquete NuGet y revisará las nuevas características y el código que los implementa.</span><span class="sxs-lookup"><span data-stu-id="53c36-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53c36-321">Si instala el paquete sin realizar los pasos anteriores de este tutorial, debe agregar una clase de inicio OWIN al proyecto.</span><span class="sxs-lookup"><span data-stu-id="53c36-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="53c36-322">Este archivo README. txt para el paquete NuGet explica este paso.</span><span class="sxs-lookup"><span data-stu-id="53c36-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="53c36-323">Instalación del paquete de NuGet Signalr. Sample</span><span class="sxs-lookup"><span data-stu-id="53c36-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="53c36-324">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="53c36-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="53c36-325">En el **Administrador de paquetes NuGet: signalr. StockTicker**, seleccione **examinar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="53c36-326">En **origen del paquete**, seleccione **Nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="53c36-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="53c36-327">Escriba *signalr. Sample* en el cuadro de búsqueda y seleccione **Microsoft. Aspnet. signalr. Sample** > **instalar**.</span><span class="sxs-lookup"><span data-stu-id="53c36-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="53c36-328">En **Explorador de soluciones**, expanda la carpeta *signalr. Sample* .</span><span class="sxs-lookup"><span data-stu-id="53c36-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="53c36-329">La instalación del paquete Signalr. Sample creó la carpeta y su contenido.</span><span class="sxs-lookup"><span data-stu-id="53c36-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="53c36-330">En la carpeta *signalr. Sample* , haga clic con el botón secundario en *StockTicker. html*y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="53c36-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="53c36-331">La instalación de Signalr. ejemplo de paquete NuGet podría cambiar la versión de jQuery que tiene en la carpeta *scripts* .</span><span class="sxs-lookup"><span data-stu-id="53c36-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="53c36-332">El nuevo archivo *StockTicker. html* que el paquete instala en la carpeta *Signalr. Sample* estará sincronizado con la versión de jQuery que instala el paquete, pero si desea ejecutar de nuevo el archivo *StockTicker. html* original, es posible que tenga que actualizar la referencia de jQuery en la etiqueta de script en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="53c36-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="53c36-333">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="53c36-333">Run the application</span></span>

 <span data-ttu-id="53c36-334">La tabla que vio en la primera aplicación tenía características útiles.</span><span class="sxs-lookup"><span data-stu-id="53c36-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="53c36-335">La aplicación Full stock ticker muestra las nuevas características: una ventana con desplazamiento horizontal que muestra los datos y los valores bursátiles que cambian de color a medida que aumentan y disminuyen.</span><span class="sxs-lookup"><span data-stu-id="53c36-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="53c36-336">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53c36-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="53c36-337">Al ejecutar la aplicación por primera vez, el "mercado" se "cierra" y verá una tabla estática y una ventana de tablero de cotizaciones que no se desplazan.</span><span class="sxs-lookup"><span data-stu-id="53c36-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="53c36-338">Seleccione **abrir mercado**.</span><span class="sxs-lookup"><span data-stu-id="53c36-338">Select **Open Market**.</span></span>

    ![Captura de pantalla del tablero de cotizaciones activo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="53c36-340">El cuadro **tablero de cotizaciones en directo** comienza a desplazarse horizontalmente y el servidor empieza a difundir periódicamente los cambios de precio de las acciones de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="53c36-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="53c36-341">Cada vez que cambia el precio de las acciones, la aplicación actualiza la **tabla de acciones en directo** y el tablero de **cotizaciones en directo**.</span><span class="sxs-lookup"><span data-stu-id="53c36-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="53c36-342">Cuando el cambio de precio de una acción es positivo, la aplicación muestra las existencias con un fondo verde.</span><span class="sxs-lookup"><span data-stu-id="53c36-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="53c36-343">Cuando el cambio es negativo, la aplicación muestra los valores con un fondo rojo.</span><span class="sxs-lookup"><span data-stu-id="53c36-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="53c36-344">Seleccione **cerrar mercado**.</span><span class="sxs-lookup"><span data-stu-id="53c36-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="53c36-345">Se detiene la actualización de la tabla.</span><span class="sxs-lookup"><span data-stu-id="53c36-345">The table updates stop.</span></span>

    * <span data-ttu-id="53c36-346">El teletipo deja de desplazarse.</span><span class="sxs-lookup"><span data-stu-id="53c36-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="53c36-347">Seleccione **Restablecer**.</span><span class="sxs-lookup"><span data-stu-id="53c36-347">Select **Reset**.</span></span>

    * <span data-ttu-id="53c36-348">Se restablecen todos los datos bursátiles.</span><span class="sxs-lookup"><span data-stu-id="53c36-348">All stock data is reset.</span></span>

    * <span data-ttu-id="53c36-349">La aplicación restaura el estado inicial antes de que se inicien los cambios de precio.</span><span class="sxs-lookup"><span data-stu-id="53c36-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="53c36-350">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="53c36-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="53c36-351">Verá que los mismos datos se actualizan dinámicamente al mismo tiempo en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="53c36-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="53c36-352">Al seleccionar cualquiera de los controles, todos los exploradores responden de la misma manera al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="53c36-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="53c36-353">Presentación del tablero de cotizaciones en directo</span><span class="sxs-lookup"><span data-stu-id="53c36-353">Live Stock Ticker display</span></span>

<span data-ttu-id="53c36-354">La presentación del **tablero de cotizaciones en directo** es una lista sin ordenar de un elemento `<div>` con formato en una sola línea mediante estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="53c36-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="53c36-355">La aplicación inicializa y actualiza el tablero de la misma forma que la tabla: reemplazando los marcadores de posición en una cadena de plantilla `<li>` y agregando dinámicamente los elementos de `<li>` al elemento `<ul>`.</span><span class="sxs-lookup"><span data-stu-id="53c36-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="53c36-356">La aplicación incluye el desplazamiento mediante la función `animate` de jQuery para modificar el margen izquierdo de la lista sin ordenar en el `<div>`.</span><span class="sxs-lookup"><span data-stu-id="53c36-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="53c36-357">Signalr. Sample StockTicker. html</span><span class="sxs-lookup"><span data-stu-id="53c36-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="53c36-358">Código HTML del tablero de cotizaciones:</span><span class="sxs-lookup"><span data-stu-id="53c36-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="53c36-359">Signalr. Sample StockTicker. CSS</span><span class="sxs-lookup"><span data-stu-id="53c36-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="53c36-360">Código CSS del tablero de cotizaciones:</span><span class="sxs-lookup"><span data-stu-id="53c36-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="53c36-361">Signalr. Sample Signalr. StockTicker. js</span><span class="sxs-lookup"><span data-stu-id="53c36-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="53c36-362">Código jQuery que hace que se desplace:</span><span class="sxs-lookup"><span data-stu-id="53c36-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="53c36-363">Métodos adicionales en el servidor a los que el cliente puede llamar</span><span class="sxs-lookup"><span data-stu-id="53c36-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="53c36-364">Para agregar flexibilidad a la aplicación, hay métodos adicionales a los que puede llamar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="53c36-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="53c36-365">Signalr. Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="53c36-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="53c36-366">La clase `StockTickerHub` define cuatro métodos adicionales a los que el cliente puede llamar:</span><span class="sxs-lookup"><span data-stu-id="53c36-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="53c36-367">La aplicación llama a `OpenMarket`, `CloseMarket`y `Reset` en respuesta a los botones de la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="53c36-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="53c36-368">Demuestran el patrón de un cliente que desencadena un cambio en el estado propagado inmediatamente a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="53c36-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="53c36-369">Cada uno de estos métodos llama a un método en la clase `StockTicker` que provoca el cambio de estado del mercado y, a continuación, difunde el nuevo estado.</span><span class="sxs-lookup"><span data-stu-id="53c36-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="53c36-370">Signalr. Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="53c36-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="53c36-371">En la clase `StockTicker`, la aplicación mantiene el estado del mercado con una propiedad `MarketState` que devuelve un valor de enumeración `MarketState`:</span><span class="sxs-lookup"><span data-stu-id="53c36-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="53c36-372">Cada uno de los métodos que cambian el estado del mercado lo hacen dentro de un bloque de bloqueo porque la clase `StockTicker` tiene que ser segura para subprocesos:</span><span class="sxs-lookup"><span data-stu-id="53c36-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="53c36-373">Para asegurarse de que este código es seguro para subprocesos, el `_marketState` campo que respalda la propiedad `MarketState` designada `volatile`:</span><span class="sxs-lookup"><span data-stu-id="53c36-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="53c36-374">Los métodos `BroadcastMarketStateChange` y `BroadcastMarketReset` son similares al método BroadcastStockPrice que ya vio, salvo que llaman a métodos diferentes definidos en el cliente:</span><span class="sxs-lookup"><span data-stu-id="53c36-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="53c36-375">Funciones adicionales en el cliente a las que el servidor puede llamar</span><span class="sxs-lookup"><span data-stu-id="53c36-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="53c36-376">La función `updateStockPrice` ahora controla la tabla y la presentación del tablero, y utiliza `jQuery.Color` para los colores rojo y verde.</span><span class="sxs-lookup"><span data-stu-id="53c36-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="53c36-377">Las nuevas funciones de *signalr. StockTicker. js* habilitan y deshabilitan los botones en función del estado del mercado.</span><span class="sxs-lookup"><span data-stu-id="53c36-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="53c36-378">También detienen o inician el desplazamiento horizontal del **tablero de cotizaciones en directo** .</span><span class="sxs-lookup"><span data-stu-id="53c36-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="53c36-379">Dado que muchas funciones se agregan a `ticker.client`, la aplicación usa la [función Extend de jQuery](http://api.jquery.com/jQuery.extend/) para agregarlas.</span><span class="sxs-lookup"><span data-stu-id="53c36-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="53c36-380">Configuración adicional del cliente después de establecer la conexión</span><span class="sxs-lookup"><span data-stu-id="53c36-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="53c36-381">Una vez que el cliente establece la conexión, tiene que hacer algo más:</span><span class="sxs-lookup"><span data-stu-id="53c36-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="53c36-382">Averigüe si el mercado está abierto o cerrado para llamar a la función `marketOpened` o `marketClosed` adecuada.</span><span class="sxs-lookup"><span data-stu-id="53c36-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="53c36-383">Adjunte las llamadas de método de servidor a los botones.</span><span class="sxs-lookup"><span data-stu-id="53c36-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="53c36-384">Los métodos de servidor no se conectan a los botones hasta que la aplicación establece la conexión.</span><span class="sxs-lookup"><span data-stu-id="53c36-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="53c36-385">Es para que el código no pueda llamar a los métodos de servidor antes de que estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="53c36-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53c36-386">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="53c36-386">Additional resources</span></span>

<span data-ttu-id="53c36-387">En este tutorial ha aprendido a programar una aplicación Signalr que difunde mensajes del servidor a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="53c36-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="53c36-388">Ahora puede difundir mensajes periódicamente y en respuesta a las notificaciones de cualquier cliente.</span><span class="sxs-lookup"><span data-stu-id="53c36-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="53c36-389">Puede usar el concepto de instancia de singleton multiproceso para mantener el estado del servidor en escenarios de juegos en línea de varios jugadores.</span><span class="sxs-lookup"><span data-stu-id="53c36-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="53c36-390">Para ver un ejemplo, consulte [el juego del captador basado en signalr](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="53c36-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="53c36-391">Para ver tutoriales que muestran escenarios de comunicación punto a punto, consulte [Introducción con signalr](introduction-to-signalr.md) y [la actualización en tiempo real con signalr](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="53c36-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="53c36-392">Para obtener más información acerca de Signalr, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="53c36-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="53c36-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="53c36-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="53c36-394">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="53c36-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="53c36-395">GitHub y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="53c36-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="53c36-396">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="53c36-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="53c36-397">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="53c36-397">Next steps</span></span>

<span data-ttu-id="53c36-398">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="53c36-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53c36-399">Creó el proyecto</span><span class="sxs-lookup"><span data-stu-id="53c36-399">Created the project</span></span>
> * <span data-ttu-id="53c36-400">Configuración del código de servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-400">Set up the server code</span></span>
> * <span data-ttu-id="53c36-401">Examen del código de servidor</span><span class="sxs-lookup"><span data-stu-id="53c36-401">Examined the server code</span></span>
> * <span data-ttu-id="53c36-402">Configurar el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-402">Set up the client code</span></span>
> * <span data-ttu-id="53c36-403">Examina el código de cliente</span><span class="sxs-lookup"><span data-stu-id="53c36-403">Examined the client code</span></span>
> * <span data-ttu-id="53c36-404">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="53c36-404">Tested the application</span></span>
> * <span data-ttu-id="53c36-405">Registro habilitado</span><span class="sxs-lookup"><span data-stu-id="53c36-405">Enabled logging</span></span>

<span data-ttu-id="53c36-406">Avance al siguiente artículo para aprender a crear una aplicación web en tiempo real que use ASP.NET Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="53c36-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="53c36-407">Creación de una aplicación web en tiempo real con Signalr</span><span class="sxs-lookup"><span data-stu-id="53c36-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)
