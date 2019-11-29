---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: chat en tiempo real con Signalr 2 y MVC 5 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar ASP.NET Signalr 2 para crear una aplicación de chat en tiempo real. Puede Agregar Signalr a una aplicación de MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600483"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="d16bf-104">Tutorial: chat en tiempo real con Signalr 2 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="d16bf-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="d16bf-105">En este tutorial se muestra cómo usar ASP.NET Signalr 2 para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="d16bf-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="d16bf-106">Puede Agregar Signalr a una aplicación de MVC 5 y crear una vista de chat para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="d16bf-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="d16bf-107">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="d16bf-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d16bf-108">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="d16bf-108">Set up the project</span></span>
> * <span data-ttu-id="d16bf-109">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d16bf-109">Run the sample</span></span>
> * <span data-ttu-id="d16bf-110">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="d16bf-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="d16bf-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d16bf-111">Prerequisites</span></span>

* <span data-ttu-id="d16bf-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="d16bf-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d16bf-113">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="d16bf-113">Set up the Project</span></span>

<span data-ttu-id="d16bf-114">En esta sección se muestra cómo usar Visual Studio 2017 y Signalr 2 para crear una aplicación ASP.NET MVC 5 vacía, agregar la biblioteca de Signalr y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="d16bf-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="d16bf-115">En Visual Studio, cree una C# aplicación ASP.net cuyo destino sea .NET Framework 4,5, asígnele el nombre SignalRChat y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="d16bf-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Crear Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="d16bf-117">En **nueva aplicación Web de ASP.net-SignalRMvcChat**, seleccione **MVC** y, a continuación, seleccione **cambiar autenticación**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="d16bf-118">En **cambiar autenticación**, seleccione **sin autenticación** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Seleccione sin autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="d16bf-120">En **nueva aplicación Web de ASP.net-SignalRMvcChat**, seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="d16bf-121">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="d16bf-122">En **Agregar nuevo elemento-SignalRChat**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .</span><span class="sxs-lookup"><span data-stu-id="d16bf-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="d16bf-123">Asigne a la clase el nombre *ChatHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d16bf-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="d16bf-124">En este paso se crea el archivo de clase *ChatHub.CS* y se agrega un conjunto de archivos de script y referencias de ensamblado que admiten signalr en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d16bf-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="d16bf-125">Reemplace el código del nuevo archivo de clase *ChatHub.CS* con este código:</span><span class="sxs-lookup"><span data-stu-id="d16bf-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="d16bf-126">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .</span><span class="sxs-lookup"><span data-stu-id="d16bf-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="d16bf-127">Asigne a la nueva clase el nombre *Startup* y agréguela al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d16bf-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="d16bf-128">Reemplace el código del archivo de clase *Startup.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="d16bf-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="d16bf-129">En **Explorador de soluciones**, seleccione **controllers** > **HomeController.CS**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="d16bf-130">Agregue este método a *HomeController.CS*.</span><span class="sxs-lookup"><span data-stu-id="d16bf-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="d16bf-131">Este método devuelve la vista de **chat** que se crea en un paso posterior.</span><span class="sxs-lookup"><span data-stu-id="d16bf-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="d16bf-132">En **Explorador de soluciones**, haga clic con el botón derecho en **vistas** > **Inicio**y seleccione **Agregar** >  **vista**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="d16bf-133">En **Agregar vista**, asigne a la nueva vista el nombre **chat** y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="d16bf-134">Reemplace el contenido de **chat. cshtml** con este código:</span><span class="sxs-lookup"><span data-stu-id="d16bf-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="d16bf-135">En **Explorador de soluciones**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="d16bf-136">Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d16bf-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d16bf-137">Es posible que el administrador de paquetes haya instalado una versión posterior de los scripts de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d16bf-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="d16bf-138">Compruebe que las referencias de script en el bloque de código corresponden a las versiones de los archivos de script del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d16bf-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="d16bf-139">Haga referencia al script desde el bloque de código original:</span><span class="sxs-lookup"><span data-stu-id="d16bf-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="d16bf-140">Si no coinciden, actualice el archivo *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="d16bf-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="d16bf-141">En la barra de menús, seleccione **archivo** > **guardar todo**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="d16bf-142">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d16bf-142">Run the Sample</span></span>

1. <span data-ttu-id="d16bf-143">En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar el ejemplo en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="d16bf-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="d16bf-145">Cuando se abra el explorador, escriba un nombre para la identidad de chat.</span><span class="sxs-lookup"><span data-stu-id="d16bf-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="d16bf-146">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="d16bf-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="d16bf-147">En cada explorador, escriba un nombre único.</span><span class="sxs-lookup"><span data-stu-id="d16bf-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="d16bf-148">Ahora, agregue un comentario y seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="d16bf-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="d16bf-149">Repítalo en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="d16bf-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="d16bf-150">Los comentarios aparecen en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="d16bf-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d16bf-151">Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d16bf-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="d16bf-152">El centro difunde los comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="d16bf-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="d16bf-153">Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.</span><span class="sxs-lookup"><span data-stu-id="d16bf-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="d16bf-154">Vea cómo se ejecuta la aplicación de chat en tres exploradores diferentes.</span><span class="sxs-lookup"><span data-stu-id="d16bf-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="d16bf-155">Cuando Tom, Anand y Susan envían mensajes, todos los exploradores se actualizan en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="d16bf-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Los tres exploradores muestran el mismo historial de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="d16bf-157">En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="d16bf-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="d16bf-158">Hay un archivo de script denominado *hubs* que la biblioteca de signalr genera en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d16bf-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="d16bf-159">Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="d16bf-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs generados automáticamente en el nodo documentos de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="d16bf-161">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="d16bf-161">Examine the Code</span></span>

<span data-ttu-id="d16bf-162">La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr.</span><span class="sxs-lookup"><span data-stu-id="d16bf-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="d16bf-163">Muestra cómo crear un centro.</span><span class="sxs-lookup"><span data-stu-id="d16bf-163">It shows you how to create a hub.</span></span> <span data-ttu-id="d16bf-164">El servidor utiliza ese concentrador como el objeto de coordinación principal.</span><span class="sxs-lookup"><span data-stu-id="d16bf-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="d16bf-165">El concentrador usa la biblioteca de jQuery de Signalr para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="d16bf-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="d16bf-166">Concentradores de signalr en ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="d16bf-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="d16bf-167">En el ejemplo de código, la clase `ChatHub` se deriva de la clase `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="d16bf-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="d16bf-168">La derivación de la clase `Hub` es una manera útil de compilar una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="d16bf-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="d16bf-169">Puede crear métodos públicos en la clase de concentrador y, a continuación, tener acceso a esos métodos llamándolos desde scripts en una página web.</span><span class="sxs-lookup"><span data-stu-id="d16bf-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="d16bf-170">En el código de chat, los clientes llaman al método `ChatHub.Send` para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="d16bf-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="d16bf-171">El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="d16bf-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="d16bf-172">El método `Send` muestra varios conceptos de Hub:</span><span class="sxs-lookup"><span data-stu-id="d16bf-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="d16bf-173">Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.</span><span class="sxs-lookup"><span data-stu-id="d16bf-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="d16bf-174">Use la propiedad dinámica `Microsoft.AspNet.SignalR.Hub.Clients` para comunicarse con todos los clientes conectados a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="d16bf-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="d16bf-175">Llame a una función en el cliente (como la función `addNewMessageToPage`) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="d16bf-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="d16bf-176">Signalr y jQuery chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="d16bf-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="d16bf-177">El archivo de vista de *chat. cshtml* en el ejemplo de código muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador de signalr.</span><span class="sxs-lookup"><span data-stu-id="d16bf-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="d16bf-178">El código lleva a cabo muchas tareas importantes.</span><span class="sxs-lookup"><span data-stu-id="d16bf-178">The code carries out many important tasks.</span></span> <span data-ttu-id="d16bf-179">Crea una referencia al proxy generado automáticamente para el concentrador, declara una función a la que el servidor puede llamar para insertar contenido en los clientes e inicia una conexión para enviar mensajes al centro.</span><span class="sxs-lookup"><span data-stu-id="d16bf-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="d16bf-180">En JavaScript, la referencia a la clase de servidor y sus miembros está en camelCase.</span><span class="sxs-lookup"><span data-stu-id="d16bf-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="d16bf-181">En el ejemplo de código C# se hace referencia a la clase `ChatHub` en JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="d16bf-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="d16bf-182">En este bloque de código, se crea una función de devolución de llamada en el script.</span><span class="sxs-lookup"><span data-stu-id="d16bf-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="d16bf-183">La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente.</span><span class="sxs-lookup"><span data-stu-id="d16bf-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="d16bf-184">La llamada opcional a la función `htmlEncode` muestra una forma de codificar en HTML el contenido del mensaje antes de mostrarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="d16bf-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="d16bf-185">Es una manera de evitar la inyección de scripts.</span><span class="sxs-lookup"><span data-stu-id="d16bf-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="d16bf-186">Este código abre una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="d16bf-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="d16bf-187">Este enfoque garantiza que se establece una conexión antes de que se ejecute el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="d16bf-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="d16bf-188">El código inicia la conexión y, a continuación, le pasa una función para controlar el evento de clic en el botón **Enviar** de la página de chat.</span><span class="sxs-lookup"><span data-stu-id="d16bf-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="d16bf-189">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="d16bf-189">Get the code</span></span>

[<span data-ttu-id="d16bf-190">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d16bf-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="d16bf-191">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d16bf-191">Additional resources</span></span>

<span data-ttu-id="d16bf-192">Para obtener más información acerca de Signalr, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="d16bf-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="d16bf-193">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="d16bf-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="d16bf-194">GitHub y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="d16bf-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="d16bf-195">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="d16bf-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="d16bf-196">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d16bf-196">Next steps</span></span>

<span data-ttu-id="d16bf-197">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="d16bf-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d16bf-198">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="d16bf-198">Set up the project</span></span>
> * <span data-ttu-id="d16bf-199">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="d16bf-199">Ran the sample</span></span>
> * <span data-ttu-id="d16bf-200">Examinó el código</span><span class="sxs-lookup"><span data-stu-id="d16bf-200">Examined the code</span></span>

<span data-ttu-id="d16bf-201">Vaya al siguiente artículo para obtener información sobre cómo crear una aplicación web que use ASP.NET Signalr 2 para proporcionar funcionalidad de mensajería de alta frecuencia.</span><span class="sxs-lookup"><span data-stu-id="d16bf-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d16bf-202">Aplicación web con mensajería de alta frecuencia</span><span class="sxs-lookup"><span data-stu-id="d16bf-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)