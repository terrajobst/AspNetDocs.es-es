---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: chat en tiempo real con Signalr 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Puede Agregar Signalr a una aplicación Web de ASP.NET vacía.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600463"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="7a6a6-104">Tutorial: chat en tiempo real con Signalr 2</span><span class="sxs-lookup"><span data-stu-id="7a6a6-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="7a6a6-105">En este tutorial se muestra cómo usar Signalr para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="7a6a6-106">Agrega Signalr a una aplicación Web ASP.NET vacía y crea una página HTML para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="7a6a6-107">En este tutorial ha:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a6a6-108">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="7a6a6-108">Set up the project</span></span>
> * <span data-ttu-id="7a6a6-109">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7a6a6-109">Run the sample</span></span>
> * <span data-ttu-id="7a6a6-110">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="7a6a6-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="7a6a6-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7a6a6-111">Prerequisites</span></span>

* <span data-ttu-id="7a6a6-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **desarrollo web y ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="7a6a6-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7a6a6-113">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="7a6a6-113">Set up the Project</span></span>

<span data-ttu-id="7a6a6-114">En esta sección se muestra cómo usar Visual Studio 2017 y Signalr 2 para crear una aplicación Web de ASP.NET vacía, agregar Signalr y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="7a6a6-115">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crear Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="7a6a6-117">En la ventana **New ASP.net Project-SignalRChat** , deje **vacío** seleccionado y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="7a6a6-118">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7a6a6-119">En **Agregar nuevo elemento-SignalRChat**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .</span><span class="sxs-lookup"><span data-stu-id="7a6a6-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="7a6a6-120">Asigne a la clase el nombre *ChatHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="7a6a6-121">En este paso se crea el archivo de clase *ChatHub.CS* y se agrega un conjunto de archivos de script y referencias de ensamblado que admiten signalr en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="7a6a6-122">Reemplace el código del nuevo archivo de clase *ChatHub.CS* con este código:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="7a6a6-123">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7a6a6-124">En **Agregar nuevo elemento-SignalRChat** , seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="7a6a6-125">Asigne a la clase el nombre *Startup* y agréguela al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="7a6a6-126">Reemplace el código predeterminado en la clase de *Inicio* con este código:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="7a6a6-127">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="7a6a6-128">Asigne un nombre al nuevo *Índice* de página y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="7a6a6-129">En **Explorador de soluciones**, haga clic con el botón secundario en la página HTML que creó y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="7a6a6-130">Reemplace el código predeterminado de la página HTML por este código:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="7a6a6-131">En **Explorador de soluciones**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="7a6a6-132">Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7a6a6-133">Es posible que el administrador de paquetes haya instalado una versión posterior de los scripts de Signalr.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="7a6a6-134">Compruebe que las referencias de script en el bloque de código corresponden a las versiones de los archivos de script del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="7a6a6-135">Haga referencia al script desde el bloque de código original:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="7a6a6-136">Si no coinciden, actualice el archivo *. html* .</span><span class="sxs-lookup"><span data-stu-id="7a6a6-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="7a6a6-137">En la barra de menús, seleccione **archivo** > **guardar todo**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="7a6a6-138">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7a6a6-138">Run the Sample</span></span>

1. <span data-ttu-id="7a6a6-139">En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar el ejemplo en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="7a6a6-141">Cuando se abra el explorador, escriba un nombre para la identidad de chat.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="7a6a6-142">Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="7a6a6-143">En cada explorador, escriba un nombre único.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="7a6a6-144">Ahora, agregue un comentario y seleccione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="7a6a6-145">Repítalo en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="7a6a6-146">Los comentarios aparecen en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7a6a6-147">Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="7a6a6-148">El centro difunde los comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="7a6a6-149">Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="7a6a6-150">Vea cómo se ejecuta la aplicación de chat en tres exploradores diferentes.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="7a6a6-151">Cuando Tom, Anand y Susan envían mensajes, todos los exploradores se actualizan en tiempo real:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Los tres exploradores muestran el mismo historial de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="7a6a6-153">En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="7a6a6-154">Hay un archivo de script denominado *hubs* que la biblioteca de signalr genera en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="7a6a6-155">Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs generados automáticamente en el nodo documentos de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="7a6a6-157">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="7a6a6-157">Examine the Code</span></span>

<span data-ttu-id="7a6a6-158">La aplicación SignalRChat muestra dos tareas básicas de desarrollo de Signalr.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="7a6a6-159">Muestra cómo crear un centro.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-159">It shows you how to create a hub.</span></span> <span data-ttu-id="7a6a6-160">El servidor utiliza ese concentrador como el objeto de coordinación principal.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="7a6a6-161">El concentrador usa la biblioteca de jQuery de Signalr para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="7a6a6-162">Concentradores de signalr en ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="7a6a6-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="7a6a6-163">En el ejemplo de código anterior, la clase `ChatHub` se deriva de la clase `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="7a6a6-164">La derivación de la clase `Hub` es una manera útil de compilar una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="7a6a6-165">Puede crear métodos públicos en la clase de concentrador y, a continuación, usar esos métodos llamándolos desde scripts en una página web.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="7a6a6-166">En el código de chat, los clientes llaman al método `ChatHub.Send` para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="7a6a6-167">A continuación, el concentrador envía el mensaje a todos los clientes mediante una llamada a `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="7a6a6-168">El método `Send` muestra varios conceptos de Hub:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="7a6a6-169">Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="7a6a6-170">Use la propiedad dinámica `Microsoft.AspNet.SignalR.Hub.Clients` para comunicarse con todos los clientes conectados a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="7a6a6-171">Llame a una función en el cliente (como la función `broadcastMessage`) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="7a6a6-172">Signalr y jQuery en index. html</span><span class="sxs-lookup"><span data-stu-id="7a6a6-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="7a6a6-173">En la página *index. html* del ejemplo de código se muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador signalr.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="7a6a6-174">El código lleva a cabo muchas tareas importantes.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-174">The code carries out many important tasks.</span></span> <span data-ttu-id="7a6a6-175">Declara un proxy para que haga referencia al concentrador, declara una función a la que el servidor puede llamar para insertar contenido en los clientes e inicia una conexión para enviar mensajes al centro.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="7a6a6-176">En JavaScript, la referencia a la clase de servidor y sus miembros deben ser camelCase.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="7a6a6-177">En el ejemplo de código C# se hace referencia a la clase *ChatHub* en JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="7a6a6-178">En este bloque de código, se crea una función de devolución de llamada en el script.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="7a6a6-179">La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="7a6a6-180">Las dos líneas que codifican en HTML el contenido antes de mostrarse son opcionales y muestran una buena manera de evitar la inserción de scripts.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="7a6a6-181">Este código abre una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="7a6a6-182">Este enfoque garantiza que el código establezca una conexión antes de que se ejecute el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="7a6a6-183">El código inicia la conexión y, a continuación, la pasa una función para controlar el evento de clic en el botón **Enviar** de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="7a6a6-184">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="7a6a6-184">Get the code</span></span>

[<span data-ttu-id="7a6a6-185">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="7a6a6-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="7a6a6-186">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7a6a6-186">Additional resources</span></span>

<span data-ttu-id="7a6a6-187">Para obtener más información acerca de Signalr, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="7a6a6-188">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="7a6a6-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="7a6a6-189">Github y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="7a6a6-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="7a6a6-190">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="7a6a6-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="7a6a6-191">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="7a6a6-191">Next steps</span></span>

<span data-ttu-id="7a6a6-192">En este tutorial:</span><span class="sxs-lookup"><span data-stu-id="7a6a6-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7a6a6-193">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="7a6a6-193">Set up the project</span></span>
> * <span data-ttu-id="7a6a6-194">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="7a6a6-194">Ran the sample</span></span>
> * <span data-ttu-id="7a6a6-195">Examinó el código</span><span class="sxs-lookup"><span data-stu-id="7a6a6-195">Examined the code</span></span>

<span data-ttu-id="7a6a6-196">Avance al siguiente artículo para aprender a usar Signalr y MVC 5.</span><span class="sxs-lookup"><span data-stu-id="7a6a6-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7a6a6-197">Signalr 2 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="7a6a6-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)