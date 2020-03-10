---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introducción con OWIN y Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472447"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="862ed-102">Introducción a OWIN y Katana</span><span class="sxs-lookup"><span data-stu-id="862ed-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="862ed-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="862ed-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="862ed-104">[Open Web Interface for .net (OWIN)](http://owin.org/) define una abstracción entre los servidores Web .net y las aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="862ed-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="862ed-105">Al desacoplar el servidor Web de la aplicación, OWIN facilita la creación de middleware para el desarrollo web de .NET.</span><span class="sxs-lookup"><span data-stu-id="862ed-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="862ed-106">Además, OWIN facilita el traslado de aplicaciones web a otros hosts&#8212;, por ejemplo, autohospedado en un servicio de Windows u otro proceso.</span><span class="sxs-lookup"><span data-stu-id="862ed-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="862ed-107">OWIN es una especificación propiedad de la comunidad, no una implementación.</span><span class="sxs-lookup"><span data-stu-id="862ed-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="862ed-108">El proyecto Katana es un conjunto de componentes OWIN de código abierto desarrollados por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="862ed-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="862ed-109">Para obtener información general sobre OWIN y Katana, vea [información general de Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="862ed-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="862ed-110">En este artículo, entraremos en el código para comenzar.</span><span class="sxs-lookup"><span data-stu-id="862ed-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="862ed-111">En este tutorial se usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), pero también puede usar Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="862ed-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="862ed-112">Algunos de los pasos son diferentes en Visual Studio 2012, que se indican a continuación.</span><span class="sxs-lookup"><span data-stu-id="862ed-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="862ed-113">Hospedar OWIN en IIS</span><span class="sxs-lookup"><span data-stu-id="862ed-113">Host OWIN in IIS</span></span>

<span data-ttu-id="862ed-114">En esta sección, hospedaremos OWIN en IIS.</span><span class="sxs-lookup"><span data-stu-id="862ed-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="862ed-115">Esta opción ofrece la flexibilidad y la composición de una canalización OWIN junto con el conjunto de características de IIS.</span><span class="sxs-lookup"><span data-stu-id="862ed-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="862ed-116">Con esta opción, la aplicación OWIN se ejecuta en la canalización de solicitudes ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="862ed-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="862ed-117">En primer lugar, cree un nuevo proyecto de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="862ed-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="862ed-118">(En Visual Studio 2012, use el tipo de proyecto aplicación Web vacía de ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="862ed-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="862ed-119">En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** .</span><span class="sxs-lookup"><span data-stu-id="862ed-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="862ed-120">Agregar paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="862ed-120">Add NuGet Packages</span></span>

<span data-ttu-id="862ed-121">A continuación, agregue los paquetes de NuGet necesarios.</span><span class="sxs-lookup"><span data-stu-id="862ed-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="862ed-122">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="862ed-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="862ed-123">En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="862ed-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="862ed-124">Agregar una clase de inicio</span><span class="sxs-lookup"><span data-stu-id="862ed-124">Add a Startup Class</span></span>

<span data-ttu-id="862ed-125">A continuación, agregue una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="862ed-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="862ed-126">En Explorador de soluciones, haga clic con el botón derecho en el proyecto, seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="862ed-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="862ed-127">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **clase de inicio Owin**.</span><span class="sxs-lookup"><span data-stu-id="862ed-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="862ed-128">Para obtener más información sobre la configuración de la clase startup, vea [detección de clases de inicio de OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="862ed-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="862ed-129">Agregue el código siguiente al método `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="862ed-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="862ed-130">Este código agrega una parte simple de middleware a la canalización OWIN, implementada como una función que recibe una instancia de **Microsoft. OWIN. IOwinContext** .</span><span class="sxs-lookup"><span data-stu-id="862ed-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="862ed-131">Cuando el servidor recibe una solicitud HTTP, la canalización OWIN invoca el middleware.</span><span class="sxs-lookup"><span data-stu-id="862ed-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="862ed-132">El middleware establece el tipo de contenido de la respuesta y escribe el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="862ed-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="862ed-133">La plantilla de la clase de inicio OWIN está disponible en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="862ed-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="862ed-134">Si usa Visual Studio 2012, solo tiene que agregar una nueva clase vacía denominada `Startup1`y pegar en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="862ed-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="862ed-135">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="862ed-135">Run the Application</span></span>

<span data-ttu-id="862ed-136">Presione F5 para comenzar la depuración.</span><span class="sxs-lookup"><span data-stu-id="862ed-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="862ed-137">Visual Studio abrirá una ventana del explorador para `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="862ed-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="862ed-138">La página debería tener un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="862ed-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="862ed-139">Servicio OWIN de autohospedaje en una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="862ed-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="862ed-140">Es fácil convertir esta aplicación del hospedaje de IIS en un proceso personalizado.</span><span class="sxs-lookup"><span data-stu-id="862ed-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="862ed-141">Con el hospedaje de IIS, IIS actúa como el servidor HTTP y como el proceso que hospeda el servicio.</span><span class="sxs-lookup"><span data-stu-id="862ed-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="862ed-142">Con el autohospedaje, la aplicación crea el proceso y usa la clase **HttpListener** como servidor http.</span><span class="sxs-lookup"><span data-stu-id="862ed-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="862ed-143">En Visual Studio, cree una nueva aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="862ed-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="862ed-144">En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="862ed-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="862ed-145">Agregue al proyecto una clase de `Startup1` de la parte 1 de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="862ed-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="862ed-146">No es necesario modificar esta clase.</span><span class="sxs-lookup"><span data-stu-id="862ed-146">You don't need to modify this class.</span></span>

<span data-ttu-id="862ed-147">Implemente el método de `Main` de la aplicación como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="862ed-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="862ed-148">Al ejecutar la aplicación de consola, el servidor empieza a escuchar `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="862ed-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="862ed-149">Si navega a esta dirección en un explorador Web, verá la página "Hello World".</span><span class="sxs-lookup"><span data-stu-id="862ed-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="862ed-150">Agregar diagnósticos OWIN</span><span class="sxs-lookup"><span data-stu-id="862ed-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="862ed-151">El paquete Microsoft. Owin. Diagnostics contiene software intermedio que detecta las excepciones no controladas y muestra una página HTML con los detalles del error.</span><span class="sxs-lookup"><span data-stu-id="862ed-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="862ed-152">Esta página funciona de manera muy similar a la página de error ASP.NET, que a veces se denomina "[pantalla amarilla de muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="862ed-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="862ed-153">Al igual que YSOD, la página de error Katana es útil durante el desarrollo, pero se recomienda deshabilitarla en modo de producción.</span><span class="sxs-lookup"><span data-stu-id="862ed-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="862ed-154">Para instalar el paquete de diagnósticos en el proyecto, escriba el siguiente comando en la ventana de la consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="862ed-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="862ed-155">Cambie el código del método `Startup1.Configuration` como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="862ed-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="862ed-156">Ahora, use CTRL + F5 para ejecutar la aplicación sin depuración, de modo que Visual Studio no se interrumpa en la excepción.</span><span class="sxs-lookup"><span data-stu-id="862ed-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="862ed-157">La aplicación se comporta igual que antes, hasta que se navega a `http://localhost/fail`, momento en el que la aplicación produce la excepción.</span><span class="sxs-lookup"><span data-stu-id="862ed-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="862ed-158">El middleware de la página de error detectará la excepción y mostrará una página HTML con información sobre el error.</span><span class="sxs-lookup"><span data-stu-id="862ed-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="862ed-159">Puede hacer clic en las fichas para ver las variables de la pila, la cadena de consulta, las cookies, el encabezado de solicitud y el entorno OWIN.</span><span class="sxs-lookup"><span data-stu-id="862ed-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="862ed-160">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="862ed-160">Next Steps</span></span>

- [<span data-ttu-id="862ed-161">Detección de la clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="862ed-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="862ed-162">Usar OWIN para autohospedar ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="862ed-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="862ed-163">Usar OWIN para autohospedar Signalr</span><span class="sxs-lookup"><span data-stu-id="862ed-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
