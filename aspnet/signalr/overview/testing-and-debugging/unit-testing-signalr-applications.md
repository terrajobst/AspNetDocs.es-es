---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Aplicaciones de Signalr de pruebas unitarias | Microsoft Docs
author: bradygaster
description: En este artículo se describe cómo usar las características de pruebas unitarias de Signalr 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467305"
---
# <a name="unit-testing-signalr-applications"></a><span data-ttu-id="841e8-103">Pruebas unitarias de aplicaciones de SignalR</span><span class="sxs-lookup"><span data-stu-id="841e8-103">Unit Testing SignalR Applications</span></span>

<span data-ttu-id="841e8-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="841e8-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="841e8-105">En este artículo se describe el uso de las características de pruebas unitarias de Signalr 2.</span><span class="sxs-lookup"><span data-stu-id="841e8-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="841e8-106">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="841e8-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="841e8-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="841e8-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="841e8-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="841e8-108">.NET 4.5</span></span>
> - <span data-ttu-id="841e8-109">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="841e8-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="841e8-110">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="841e8-110">Questions and comments</span></span>
>
> <span data-ttu-id="841e8-111">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="841e8-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="841e8-112">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="841e8-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="841e8-113">Aplicaciones de Signalr de pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="841e8-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="841e8-114">Puede usar las características de pruebas unitarias de Signalr 2 para crear pruebas unitarias para la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="841e8-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="841e8-115">Signalr 2 incluye la interfaz [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , que se puede usar para crear un objeto ficticio para simular los métodos de concentrador para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="841e8-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="841e8-116">En esta sección, agregará pruebas unitarias para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) con [xUnit.net](https://github.com/xunit/xunit) y [MOQ](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="841e8-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="841e8-117">XUnit.net se usará para controlar la prueba; MOQ se usará para crear un objeto [ficticio](http://en.wikipedia.org/wiki/Mock_object) para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="841e8-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="841e8-118">Si lo desea, puede usar otros marcos ficticios. [NSubstitute](http://nsubstitute.github.io/) también es una buena elección.</span><span class="sxs-lookup"><span data-stu-id="841e8-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="841e8-119">En este tutorial se muestra cómo configurar el objeto ficticio de dos maneras: en primer lugar, mediante un objeto `dynamic` (introducido en .NET Framework 4) y el segundo, mediante una interfaz.</span><span class="sxs-lookup"><span data-stu-id="841e8-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="841e8-120">Contenido</span><span class="sxs-lookup"><span data-stu-id="841e8-120">Contents</span></span>

<span data-ttu-id="841e8-121">Este tutorial contiene las siguientes secciones.</span><span class="sxs-lookup"><span data-stu-id="841e8-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="841e8-122">Pruebas unitarias con Dynamic</span><span class="sxs-lookup"><span data-stu-id="841e8-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="841e8-123">Pruebas unitarias por tipo</span><span class="sxs-lookup"><span data-stu-id="841e8-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="841e8-124">Pruebas unitarias con Dynamic</span><span class="sxs-lookup"><span data-stu-id="841e8-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="841e8-125">En esta sección, agregará una prueba unitaria para la aplicación creada en el [Introducción tutorial](../getting-started/tutorial-getting-started-with-signalr.md) con un objeto dinámico.</span><span class="sxs-lookup"><span data-stu-id="841e8-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="841e8-126">Instale la [extensión xUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="841e8-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="841e8-127">Complete el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md)o descargue la aplicación completada de la galería de [código de MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="841e8-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="841e8-128">Si usa la versión de descarga de la aplicación Introducción, abra la **consola del administrador de paquetes** y haga clic en **restaurar** para agregar el paquete de signalr al proyecto.</span><span class="sxs-lookup"><span data-stu-id="841e8-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Restaurar paquetes](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="841e8-130">Agregue un proyecto a la solución para la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="841e8-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="841e8-131">Haga clic con el botón derecho en la solución en **Explorador de soluciones** y seleccione **Agregar**, **nuevo proyecto..** .. En el **C#** nodo, seleccione el nodo **Windows** .</span><span class="sxs-lookup"><span data-stu-id="841e8-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="841e8-132">Seleccione **biblioteca de clases**.</span><span class="sxs-lookup"><span data-stu-id="841e8-132">Select **Class Library**.</span></span> <span data-ttu-id="841e8-133">Asigne al nuevo proyecto el nombre **TestLibrary** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="841e8-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Crear biblioteca de pruebas](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="841e8-135">Agregue una referencia en el proyecto de biblioteca de pruebas al proyecto SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="841e8-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="841e8-136">Haga clic con el botón derecho en el proyecto **TestLibrary** y seleccione **Agregar**, **referencia..** .. Seleccione el nodo **proyectos** en el nodo de la **solución** y compruebe **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="841e8-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="841e8-137">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="841e8-137">Click **OK**.</span></span>

    ![Agregar referencia de proyecto](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="841e8-139">Agregue los paquetes Signalr, MOQ y XUnit al proyecto **TestLibrary** .</span><span class="sxs-lookup"><span data-stu-id="841e8-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="841e8-140">En la **consola del administrador de paquetes**, establezca la lista desplegable **proyecto predeterminado** en **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="841e8-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="841e8-141">Ejecute los siguientes comandos en la ventana de la consola:</span><span class="sxs-lookup"><span data-stu-id="841e8-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalación de paquetes](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="841e8-143">Cree el archivo de prueba.</span><span class="sxs-lookup"><span data-stu-id="841e8-143">Create the test file.</span></span> <span data-ttu-id="841e8-144">Haga clic con el botón derecho en el proyecto **TestLibrary** y haga clic en **Agregar...** , **clase**.</span><span class="sxs-lookup"><span data-stu-id="841e8-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="841e8-145">Asigne a la nueva clase el nombre **tests.CS**.</span><span class="sxs-lookup"><span data-stu-id="841e8-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="841e8-146">Reemplace el contenido de Tests.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="841e8-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="841e8-147">En el código anterior, se crea un cliente de prueba mediante el `Mock` objeto de la biblioteca [MOQ](https://github.com/Moq/moq4) , de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en signalr 2,1, asigna `dynamic` para el parámetro de tipo). La interfaz de `IHubCallerConnectionContext` es el objeto proxy con el que se invocan métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="841e8-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="841e8-148">A continuación, se define la función `broadcastMessage` para el cliente ficticio, de modo que la clase `ChatHub` pueda llamarla.</span><span class="sxs-lookup"><span data-stu-id="841e8-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="841e8-149">A continuación, el motor de pruebas llama al método `Send` de la clase `ChatHub`, que a su vez llama a la función de `broadcastMessage` ficticia.</span><span class="sxs-lookup"><span data-stu-id="841e8-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="841e8-150">Para compilar la solución, presione **F6**.</span><span class="sxs-lookup"><span data-stu-id="841e8-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="841e8-151">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="841e8-151">Run the unit test.</span></span> <span data-ttu-id="841e8-152">En Visual Studio, seleccione **probar**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="841e8-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="841e8-153">En la ventana Explorador de pruebas, haga clic con el botón secundario en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="841e8-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="841e8-155">Compruebe que la prueba se superó comprobando el panel inferior en la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="841e8-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="841e8-156">La ventana mostrará que la prueba se ha superado.</span><span class="sxs-lookup"><span data-stu-id="841e8-156">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="841e8-158">Pruebas unitarias por tipo</span><span class="sxs-lookup"><span data-stu-id="841e8-158">Unit testing by type</span></span>

<span data-ttu-id="841e8-159">En esta sección, agregará una prueba para la aplicación creada en el [Introducción tutorial](../getting-started/tutorial-getting-started-with-signalr.md) mediante una interfaz que contiene el método que se va a probar.</span><span class="sxs-lookup"><span data-stu-id="841e8-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="841e8-160">Complete los pasos del 1-7 en el tutorial de [pruebas unitarias con Dynamic](#dynamic) .</span><span class="sxs-lookup"><span data-stu-id="841e8-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="841e8-161">Reemplace el contenido de Tests.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="841e8-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="841e8-162">En el código anterior, se crea una interfaz que define la firma del método `broadcastMessage` para el que el motor de pruebas creará un cliente ficticio.</span><span class="sxs-lookup"><span data-stu-id="841e8-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="841e8-163">A continuación, se crea un cliente ficticio mediante el `Mock` objeto, de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en signalr 2,1, asigna `dynamic` para el parámetro de tipo). La interfaz de `IHubCallerConnectionContext` es el objeto proxy con el que se invocan métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="841e8-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="841e8-164">A continuación, la prueba crea una instancia de `ChatHub`y, a continuación, crea una versión ficticia del método `broadcastMessage`, que a su vez se invoca llamando al método `Send` en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="841e8-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="841e8-165">Para compilar la solución, presione **F6**.</span><span class="sxs-lookup"><span data-stu-id="841e8-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="841e8-166">Ejecute la prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="841e8-166">Run the unit test.</span></span> <span data-ttu-id="841e8-167">En Visual Studio, seleccione **probar**, **Windows**, **Explorador de pruebas**.</span><span class="sxs-lookup"><span data-stu-id="841e8-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="841e8-168">En la ventana Explorador de pruebas, haga clic con el botón secundario en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.</span><span class="sxs-lookup"><span data-stu-id="841e8-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="841e8-170">Compruebe que la prueba se superó comprobando el panel inferior en la ventana del explorador de pruebas.</span><span class="sxs-lookup"><span data-stu-id="841e8-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="841e8-171">La ventana mostrará que la prueba se ha superado.</span><span class="sxs-lookup"><span data-stu-id="841e8-171">The window will show that the test passed.</span></span>

    ![Prueba superada](unit-testing-signalr-applications/_static/image8.png)
