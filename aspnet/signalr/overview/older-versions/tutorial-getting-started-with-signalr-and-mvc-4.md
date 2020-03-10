---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introducción con Signalr 1. x y MVC 4 | Microsoft Docs'
author: bradygaster
description: Use ASP.NET Signalr y ASP.NET MVC 4 para compilar una aplicación de chat en tiempo real.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468073"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="746f9-103">Tutorial: Introducción con Signalr 1. x y MVC 4</span><span class="sxs-lookup"><span data-stu-id="746f9-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="746f9-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="746f9-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="746f9-105">En este tutorial se muestra cómo usar ASP.NET Signalr para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="746f9-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="746f9-106">Agregará Signalr a una aplicación de MVC 4 y creará una vista de chat para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="746f9-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="746f9-107">Información general</span><span class="sxs-lookup"><span data-stu-id="746f9-107">Overview</span></span>

<span data-ttu-id="746f9-108">En este tutorial se presenta el desarrollo de aplicaciones web en tiempo real con ASP.NET Signalr y ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="746f9-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="746f9-109">En el tutorial se usa el mismo código de la aplicación de chat que [signalr introducción tutorial](tutorial-getting-started-with-signalr.md), pero se muestra cómo agregarlo a una aplicación de MVC 4 basada en la plantilla de Internet.</span><span class="sxs-lookup"><span data-stu-id="746f9-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="746f9-110">En este tema, aprenderá las siguientes tareas de desarrollo de Signalr:</span><span class="sxs-lookup"><span data-stu-id="746f9-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="746f9-111">Agregar la biblioteca de Signalr a una aplicación de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="746f9-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="746f9-112">Crear una clase de concentrador para enviar contenido a los clientes.</span><span class="sxs-lookup"><span data-stu-id="746f9-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="746f9-113">Uso de la biblioteca de jQuery de Signalr en una página web para enviar mensajes y Mostrar actualizaciones desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="746f9-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="746f9-114">En la captura de pantalla siguiente se muestra la aplicación de chat completada que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="746f9-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instancias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="746f9-116">Sección</span><span class="sxs-lookup"><span data-stu-id="746f9-116">Sections:</span></span>

- [<span data-ttu-id="746f9-117">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="746f9-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="746f9-118">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="746f9-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="746f9-119">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="746f9-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="746f9-120">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="746f9-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="746f9-121">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="746f9-121">Set up the Project</span></span>

<span data-ttu-id="746f9-122">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="746f9-122">Prerequisites:</span></span>

- <span data-ttu-id="746f9-123">Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="746f9-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="746f9-124">Si no tiene Visual Studio, consulte descargas de [ASP.net](https://www.asp.net/downloads) para obtener la herramienta gratuita de desarrollo de visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="746f9-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="746f9-125">Para Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="746f9-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="746f9-126">En esta sección se muestra cómo crear una aplicación ASP.NET MVC 4, agregar la biblioteca Signalr y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="746f9-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="746f9-127">En Visual Studio, cree una aplicación ASP.NET MVC 4, asígnele el nombre SignalRChat y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="746f9-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="746f9-128">En VS 2010, seleccione **.NET Framework 4** en el control desplegable versión de Framework.</span><span class="sxs-lookup"><span data-stu-id="746f9-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="746f9-129">El código de signalr se ejecuta en las versiones 4 y 4,5 de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="746f9-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Crear web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="746f9-131">Seleccione la plantilla aplicación de Internet, desactive la opción para **crear un proyecto de prueba unitaria**y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="746f9-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Crear sitio de Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="746f9-133">Abra las **herramientas > el administrador de paquetes NuGet > consola del administrador de paquetes** y ejecute el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="746f9-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="746f9-134">Este paso agrega al proyecto un conjunto de archivos de script y referencias de ensamblado que habilitan la funcionalidad de Signalr.</span><span class="sxs-lookup"><span data-stu-id="746f9-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="746f9-135">En **Explorador de soluciones** expanda la carpeta scripts.</span><span class="sxs-lookup"><span data-stu-id="746f9-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="746f9-136">Tenga en cuenta que las bibliotecas de scripts de Signalr se han agregado al proyecto.</span><span class="sxs-lookup"><span data-stu-id="746f9-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Referencias de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="746f9-138">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar | Nueva carpeta**y agregue una nueva carpeta denominada **hubs**.</span><span class="sxs-lookup"><span data-stu-id="746f9-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="746f9-139">Haga clic con el botón secundario en la carpeta **hubs** , haga clic en **Agregar |** Y cree una nueva C# clase denominada **ChatHub.CS**.</span><span class="sxs-lookup"><span data-stu-id="746f9-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="746f9-140">Usará esta clase como concentrador de servidor de Signalr que envía mensajes a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="746f9-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="746f9-141">Si usa Visual Studio 2012 y ha instalado la [actualización ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), puede usar la nueva plantilla de elemento signalr para crear la clase hub.</span><span class="sxs-lookup"><span data-stu-id="746f9-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="746f9-142">Para ello, haga clic con el botón secundario en la carpeta **hubs** , haga clic en **Agregar | Nuevo elemento**, seleccione **clase de concentrador de signalr (V1)** y asigne a la clase el nombre **ChatHub.CS**.</span><span class="sxs-lookup"><span data-stu-id="746f9-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="746f9-143">Reemplace el código de la clase **ChatHub** por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="746f9-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="746f9-144">Abra el archivo **global. asax** del proyecto y agregue una llamada al método `RouteTable.Routes.MapHubs();` como la primera línea de código en el método `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="746f9-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="746f9-145">Este código registra la ruta predeterminada para los concentradores de Signalr y se debe llamar antes de registrar otras rutas.</span><span class="sxs-lookup"><span data-stu-id="746f9-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="746f9-146">El método `Application_Start` completado es similar al del ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="746f9-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="746f9-147">Edite la clase `HomeController` que se encuentra en **Controllers/HomeController. CS** y agregue el método siguiente a la clase.</span><span class="sxs-lookup"><span data-stu-id="746f9-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="746f9-148">Este método devuelve la vista de **chat** que creará en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="746f9-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="746f9-149">Haga clic con el botón derecho en el método `Chat` que acaba de crear y haga clic en **Agregar vista** para crear un nuevo archivo de vista.</span><span class="sxs-lookup"><span data-stu-id="746f9-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="746f9-150">En el cuadro de diálogo **Agregar vista** , asegúrese de que la casilla está activada para **usar un diseño o una página maestra** (desactive las demás casillas) y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="746f9-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="746f9-152">Edite el nuevo archivo de vista denominado **chat. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="746f9-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="746f9-153">Después de la etiqueta &lt;H2&gt;, pegue la siguiente sección de &lt;div&gt; y el bloque de código `@section scripts` en la página.</span><span class="sxs-lookup"><span data-stu-id="746f9-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="746f9-154">Este script permite a la página enviar mensajes de chat y mostrar mensajes del servidor.</span><span class="sxs-lookup"><span data-stu-id="746f9-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="746f9-155">El código completo de la vista de chat aparece en el siguiente bloque de código.</span><span class="sxs-lookup"><span data-stu-id="746f9-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="746f9-156">Al agregar Signalr y otras bibliotecas de scripts al proyecto de Visual Studio, el administrador de paquetes puede instalar versiones de los scripts que son más recientes que las versiones que se muestran en este tema.</span><span class="sxs-lookup"><span data-stu-id="746f9-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="746f9-157">Asegúrese de que las referencias de script en el código coinciden con las versiones de las bibliotecas de scripts instaladas en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="746f9-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="746f9-158">**Guarde todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="746f9-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="746f9-159">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="746f9-159">Run the Sample</span></span>

1. <span data-ttu-id="746f9-160">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="746f9-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="746f9-161">En la línea de dirección del explorador, anexe **/Home/chat** a la dirección URL de la página predeterminada del proyecto.</span><span class="sxs-lookup"><span data-stu-id="746f9-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="746f9-162">La página de chat se carga en una instancia del explorador y solicita un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="746f9-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="746f9-164">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="746f9-164">Enter a user name.</span></span>
4. <span data-ttu-id="746f9-165">Copie la dirección URL de la línea de dirección del explorador y Úsela para abrir dos instancias más del explorador.</span><span class="sxs-lookup"><span data-stu-id="746f9-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="746f9-166">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="746f9-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="746f9-167">En cada instancia del explorador, agregue un comentario y haga clic en **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="746f9-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="746f9-168">Los comentarios deben mostrarse en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="746f9-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="746f9-169">Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="746f9-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="746f9-170">El centro difunde los comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="746f9-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="746f9-171">Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.</span><span class="sxs-lookup"><span data-stu-id="746f9-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="746f9-172">En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="746f9-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="746f9-174">En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="746f9-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="746f9-175">Este nodo está visible en modo de depuración si usa Internet Explorer como explorador.</span><span class="sxs-lookup"><span data-stu-id="746f9-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="746f9-176">Hay un archivo de script denominado **hubs** que la biblioteca de signalr genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="746f9-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="746f9-177">Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="746f9-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="746f9-178">Si utiliza un explorador que no sea Internet Explorer, también puede tener acceso al archivo de **concentradores** dinámicos desplazándose directamente a él, por ejemplo http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="746f9-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script de concentrador generado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="746f9-180">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="746f9-180">Examine the Code</span></span>

<span data-ttu-id="746f9-181">La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr: crear un concentrador como el objeto de coordinación principal en el servidor y usar la biblioteca de jQuery de Signalr para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="746f9-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="746f9-182">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="746f9-182">SignalR Hubs</span></span>

<span data-ttu-id="746f9-183">En el ejemplo de código, la clase **ChatHub** se deriva de la clase **Microsoft. Aspnet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="746f9-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="746f9-184">Derivar de la clase **Hub** es una manera útil de compilar una aplicación signalr.</span><span class="sxs-lookup"><span data-stu-id="746f9-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="746f9-185">Puede crear métodos públicos en la clase de concentrador y, a continuación, acceder a esos métodos llamándolos desde scripts de jQuery en una página web.</span><span class="sxs-lookup"><span data-stu-id="746f9-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="746f9-186">En el código de chat, los clientes llaman al método **ChatHub. Send** para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="746f9-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="746f9-187">El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a **clients. All. addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="746f9-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="746f9-188">El método **send** muestra varios conceptos de Hub:</span><span class="sxs-lookup"><span data-stu-id="746f9-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="746f9-189">Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.</span><span class="sxs-lookup"><span data-stu-id="746f9-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="746f9-190">Use la propiedad **Microsoft. Aspnet. signalr. Hub. clients** para tener acceso a todos los clientes conectados a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="746f9-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="746f9-191">Llame a una función de jQuery en el cliente (por ejemplo, la función `addNewMessageToPage`) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="746f9-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="746f9-192">Signalr y jQuery</span><span class="sxs-lookup"><span data-stu-id="746f9-192">SignalR and jQuery</span></span>

<span data-ttu-id="746f9-193">El archivo de vista de **chat. cshtml** en el ejemplo de código muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador de signalr.</span><span class="sxs-lookup"><span data-stu-id="746f9-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="746f9-194">Las tareas esenciales del código son la creación de una referencia al proxy generado automáticamente para el concentrador, la declaración de una función a la que el servidor puede llamar para insertar contenido en los clientes y el inicio de una conexión para enviar mensajes al centro.</span><span class="sxs-lookup"><span data-stu-id="746f9-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="746f9-195">El código siguiente declara un proxy para un concentrador.</span><span class="sxs-lookup"><span data-stu-id="746f9-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="746f9-196">En jQuery, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas Camel.</span><span class="sxs-lookup"><span data-stu-id="746f9-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="746f9-197">En el ejemplo de código C# se hace referencia a la clase **ChatHub** de jQuery como **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="746f9-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="746f9-198">Si desea hacer referencia a la clase `ChatHub` en jQuery con la grafía Pascal convencional tal como lo C#haría en, edite el archivo de clase ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="746f9-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="746f9-199">Agregue una instrucción `using` para hacer referencia al espacio de nombres `Microsoft.AspNet.SignalR.Hubs`.</span><span class="sxs-lookup"><span data-stu-id="746f9-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="746f9-200">A continuación, agregue el atributo `HubName` a la clase `ChatHub`, por ejemplo `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="746f9-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="746f9-201">Por último, actualice la referencia de jQuery a la clase `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="746f9-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="746f9-202">En el código siguiente se muestra cómo crear una función de devolución de llamada en el script.</span><span class="sxs-lookup"><span data-stu-id="746f9-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="746f9-203">La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente.</span><span class="sxs-lookup"><span data-stu-id="746f9-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="746f9-204">La llamada opcional a la función `htmlEncode` muestra una forma de codificar en HTML el contenido del mensaje antes de mostrarlo en la página, como una manera de evitar la inserción de scripts.</span><span class="sxs-lookup"><span data-stu-id="746f9-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="746f9-205">En el código siguiente se muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="746f9-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="746f9-206">El código inicia la conexión y, a continuación, le pasa una función para controlar el evento de clic en el botón **Enviar** de la página de chat.</span><span class="sxs-lookup"><span data-stu-id="746f9-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="746f9-207">Este enfoque garantiza que la conexión se establece antes de que se ejecute el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="746f9-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="746f9-208">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="746f9-208">Next Steps</span></span>

<span data-ttu-id="746f9-209">Ha aprendido que Signalr es un marco para compilar aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="746f9-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="746f9-210">También ha aprendido varias tareas de desarrollo de Signalr: Cómo agregar Signalr a una aplicación de ASP.NET, cómo crear una clase de concentrador y cómo enviar y recibir mensajes desde el centro.</span><span class="sxs-lookup"><span data-stu-id="746f9-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="746f9-211">Para obtener más información sobre los conceptos avanzados de los avances de Signalr, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:</span><span class="sxs-lookup"><span data-stu-id="746f9-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="746f9-212">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="746f9-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="746f9-213">Github y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="746f9-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="746f9-214">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="746f9-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
