---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: creación de una aplicación en tiempo real de alta frecuencia con Signalr 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr para proporcionar la funcionalidad de mensajería de alta frecuencia.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450121"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="eb998-103">Tutorial: creación de una aplicación en tiempo real de alta frecuencia con Signalr 2</span><span class="sxs-lookup"><span data-stu-id="eb998-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="eb998-104">En este tutorial se muestra cómo crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de mensajería de alta frecuencia.</span><span class="sxs-lookup"><span data-stu-id="eb998-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="eb998-105">En este caso, "mensajería de frecuencia alta" significa que el servidor envía actualizaciones a una velocidad fija.</span><span class="sxs-lookup"><span data-stu-id="eb998-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="eb998-106">Puede enviar hasta 10 mensajes por segundo.</span><span class="sxs-lookup"><span data-stu-id="eb998-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="eb998-107">La aplicación que cree muestra una forma que los usuarios pueden arrastrar.</span><span class="sxs-lookup"><span data-stu-id="eb998-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="eb998-108">El servidor actualiza la posición de la forma en todos los exploradores conectados para que coincida con la posición de la forma arrastrada mediante actualizaciones con tiempo.</span><span class="sxs-lookup"><span data-stu-id="eb998-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="eb998-109">Los conceptos presentados en este tutorial tienen aplicaciones en juegos en tiempo real y otras aplicaciones de simulación.</span><span class="sxs-lookup"><span data-stu-id="eb998-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="eb998-110">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="eb998-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb998-111">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="eb998-111">Set up the project</span></span>
> * <span data-ttu-id="eb998-112">Crear la aplicación base</span><span class="sxs-lookup"><span data-stu-id="eb998-112">Create the base application</span></span>
> * <span data-ttu-id="eb998-113">Asignar al centro cuando se inicia la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="eb998-114">Agregar el cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-114">Add the client</span></span>
> * <span data-ttu-id="eb998-115">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-115">Run the app</span></span>
> * <span data-ttu-id="eb998-116">Agregar el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-116">Add the client loop</span></span>
> * <span data-ttu-id="eb998-117">Agregar el bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="eb998-117">Add the server loop</span></span>
> * <span data-ttu-id="eb998-118">Agregar animaciones suaves</span><span class="sxs-lookup"><span data-stu-id="eb998-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="eb998-119">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="eb998-119">Prerequisites</span></span>

* <span data-ttu-id="eb998-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.</span><span class="sxs-lookup"><span data-stu-id="eb998-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="eb998-121">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="eb998-121">Set up the project</span></span>

<span data-ttu-id="eb998-122">En esta sección, creará el proyecto en Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="eb998-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="eb998-123">En esta sección se muestra cómo usar Visual Studio 2017 para crear una aplicación Web de ASP.NET vacía y agregar las bibliotecas Signalr y jQuery. UI.</span><span class="sxs-lookup"><span data-stu-id="eb998-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="eb998-124">En Visual Studio, cree una aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eb998-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Crear Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="eb998-126">En la ventana **New ASP.NET Web Application-MoveShapeDemo** , deje **vacío** seleccionado y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb998-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="eb998-127">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="eb998-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="eb998-128">En **Agregar nuevo elemento-MoveShapeDemo**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .</span><span class="sxs-lookup"><span data-stu-id="eb998-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="eb998-129">Asigne a la clase el nombre *MoveShapeHub* y agréguelo al proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="eb998-130">En este paso se crea el archivo de clase *MoveShapeHub.CS* .</span><span class="sxs-lookup"><span data-stu-id="eb998-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="eb998-131">Simultáneamente, agrega un conjunto de archivos de script y referencias de ensamblado que admiten Signalr en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="eb998-132">Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="eb998-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="eb998-133">En la **consola del administrador de paquetes**, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="eb998-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="eb998-134">El comando instala la biblioteca de jQuery UI.</span><span class="sxs-lookup"><span data-stu-id="eb998-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="eb998-135">Se usa para animar la forma.</span><span class="sxs-lookup"><span data-stu-id="eb998-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="eb998-136">En **Explorador de soluciones**, expanda el nodo scripts.</span><span class="sxs-lookup"><span data-stu-id="eb998-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Referencias de la biblioteca de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="eb998-138">Las bibliotecas de scripts para jQuery, jQueryUI y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="eb998-139">Crear la aplicación base</span><span class="sxs-lookup"><span data-stu-id="eb998-139">Create the base application</span></span>

<span data-ttu-id="eb998-140">En esta sección, creará una aplicación de explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-140">In this section, you create a browser application.</span></span> <span data-ttu-id="eb998-141">La aplicación envía la ubicación de la forma al servidor durante cada evento de movimiento del mouse.</span><span class="sxs-lookup"><span data-stu-id="eb998-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="eb998-142">El servidor difunde esta información a todos los demás clientes conectados en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="eb998-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="eb998-143">Puede obtener más información sobre esta aplicación en secciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="eb998-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="eb998-144">Abra el archivo *MoveShapeHub.CS* .</span><span class="sxs-lookup"><span data-stu-id="eb998-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="eb998-145">Reemplace el código del archivo *MoveShapeHub.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="eb998-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="eb998-146">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="eb998-146">Save the file.</span></span>

<span data-ttu-id="eb998-147">La clase `MoveShapeHub` es una implementación de un concentrador Signalr.</span><span class="sxs-lookup"><span data-stu-id="eb998-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="eb998-148">Como en el tutorial [Introducción con signalr](tutorial-getting-started-with-signalr.md) , el concentrador tiene un método al que los clientes llaman directamente.</span><span class="sxs-lookup"><span data-stu-id="eb998-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="eb998-149">En este caso, el cliente envía un objeto con las nuevas coordenadas X e y de la forma al servidor.</span><span class="sxs-lookup"><span data-stu-id="eb998-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="eb998-150">Esas coordenadas se difunden a todos los demás clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="eb998-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="eb998-151">Signalr serializa automáticamente este objeto mediante JSON.</span><span class="sxs-lookup"><span data-stu-id="eb998-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="eb998-152">La aplicación envía el objeto de `ShapeModel` al cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="eb998-153">Tiene miembros para almacenar la posición de la forma.</span><span class="sxs-lookup"><span data-stu-id="eb998-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="eb998-154">La versión del objeto en el servidor también tiene un miembro para realizar el seguimiento de los datos del cliente que se están almacenando.</span><span class="sxs-lookup"><span data-stu-id="eb998-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="eb998-155">Este objeto evita que el servidor envíe de nuevo los datos de un cliente a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="eb998-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="eb998-156">Este miembro usa el atributo `JsonIgnore` para impedir que la aplicación serialice los datos y los envíe de vuelta al cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="eb998-157">Asignar al centro cuando se inicia la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-157">Map to the hub when app starts</span></span>

<span data-ttu-id="eb998-158">A continuación, configure la asignación al concentrador cuando se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb998-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="eb998-159">En Signalr 2, la adición de una clase de inicio OWIN crea la asignación.</span><span class="sxs-lookup"><span data-stu-id="eb998-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="eb998-160">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="eb998-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="eb998-161">En **Agregar nuevo elemento-MoveShapeDemo** , seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.</span><span class="sxs-lookup"><span data-stu-id="eb998-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="eb998-162">Asigne a la clase el nombre *Startup* y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb998-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="eb998-163">Reemplace el código predeterminado del archivo *Startup.CS* por este código:</span><span class="sxs-lookup"><span data-stu-id="eb998-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="eb998-164">La clase de inicio OWIN llama a `MapSignalR` cuando la aplicación ejecuta el método `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="eb998-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="eb998-165">La aplicación agrega la clase al proceso de inicio de OWIN mediante el `OwinStartup` atributo de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="eb998-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="eb998-166">Agregar el cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-166">Add the client</span></span>

<span data-ttu-id="eb998-167">Agregue la página HTML para el cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="eb998-168">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="eb998-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="eb998-169">Asigne un nombre **predeterminado** a la página y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="eb998-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="eb998-170">En **Explorador de soluciones**, haga clic con el botón secundario en *default. html* y seleccione **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="eb998-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="eb998-171">Reemplace el código predeterminado en el archivo *default. html* por este código:</span><span class="sxs-lookup"><span data-stu-id="eb998-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="eb998-172">En **Explorador de soluciones**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="eb998-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="eb998-173">Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eb998-174">El administrador de paquetes instala una versión posterior de los scripts de Signalr.</span><span class="sxs-lookup"><span data-stu-id="eb998-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="eb998-175">Actualice las referencias de script en el bloque de código para que correspondan a las versiones de los archivos de script del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="eb998-176">Este código HTML y JavaScript crea un `div` rojo denominado `shape`.</span><span class="sxs-lookup"><span data-stu-id="eb998-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="eb998-177">Habilita el comportamiento de arrastre de la forma mediante la biblioteca de jQuery y usa el evento `drag` para enviar la posición de la forma al servidor.</span><span class="sxs-lookup"><span data-stu-id="eb998-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="eb998-178">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-178">Run the app</span></span>

<span data-ttu-id="eb998-179">Puede ejecutar la aplicación para se'e que funcione.</span><span class="sxs-lookup"><span data-stu-id="eb998-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="eb998-180">Al arrastrar la forma alrededor de una ventana del explorador, la forma también se mueve en los otros exploradores.</span><span class="sxs-lookup"><span data-stu-id="eb998-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="eb998-181">En la barra de herramientas, active la **depuración de script** y, a continuación, seleccione el botón reproducir para ejecutar la aplicación en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="eb998-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Captura de pantalla del usuario que activa el modo de depuración y selecciona reproducir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="eb998-183">Se abre una ventana del explorador con la forma roja en la esquina superior derecha.</span><span class="sxs-lookup"><span data-stu-id="eb998-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="eb998-184">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="eb998-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="eb998-185">Abra otro explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="eb998-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="eb998-186">Arrastre la forma en una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="eb998-187">A continuación se muestra la forma en la otra ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="eb998-188">Mientras que la aplicación funciona con este método, no es un modelo de programación recomendado.</span><span class="sxs-lookup"><span data-stu-id="eb998-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="eb998-189">No hay ningún límite superior para el número de mensajes que se envían.</span><span class="sxs-lookup"><span data-stu-id="eb998-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="eb998-190">Como resultado, los clientes y el servidor se sobrecargan con los mensajes y la degradación del rendimiento.</span><span class="sxs-lookup"><span data-stu-id="eb998-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="eb998-191">Además, la aplicación muestra una animación separada en el cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="eb998-192">Esta animación irregular se produce porque la forma se mueve al instante por cada método.</span><span class="sxs-lookup"><span data-stu-id="eb998-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="eb998-193">Es mejor si la forma se mueve sin problemas a cada nueva ubicación.</span><span class="sxs-lookup"><span data-stu-id="eb998-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="eb998-194">A continuación, aprenderá a corregir esos problemas.</span><span class="sxs-lookup"><span data-stu-id="eb998-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="eb998-195">Agregar el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-195">Add the client loop</span></span>

<span data-ttu-id="eb998-196">El envío de la ubicación de la forma en cada evento de movimiento del mouse crea una cantidad innecesaria de tráfico de red.</span><span class="sxs-lookup"><span data-stu-id="eb998-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="eb998-197">La aplicación debe limitar los mensajes del cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="eb998-198">Utilice la función `setInterval` de JavaScript para configurar un bucle que envíe la nueva información de posición al servidor a una velocidad fija.</span><span class="sxs-lookup"><span data-stu-id="eb998-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="eb998-199">Este bucle es una representación básica de un "bucle de juego".</span><span class="sxs-lookup"><span data-stu-id="eb998-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="eb998-200">Se trata de una función llamada repetidamente que impulsa toda la funcionalidad de un juego.</span><span class="sxs-lookup"><span data-stu-id="eb998-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="eb998-201">Reemplace el código de cliente en el archivo *default. html* por este código:</span><span class="sxs-lookup"><span data-stu-id="eb998-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="eb998-202">Tendrá que reemplazar las referencias de script de nuevo.</span><span class="sxs-lookup"><span data-stu-id="eb998-202">You have to replace the script references again.</span></span> <span data-ttu-id="eb998-203">Deben coincidir con las versiones de los scripts del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eb998-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="eb998-204">Este nuevo código agrega la función `updateServerModel`.</span><span class="sxs-lookup"><span data-stu-id="eb998-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="eb998-205">Se llama en una frecuencia fija.</span><span class="sxs-lookup"><span data-stu-id="eb998-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="eb998-206">La función envía los datos de la posición al servidor cada vez que la marca `moved` indica que hay nuevos datos de posición que se van a enviar.</span><span class="sxs-lookup"><span data-stu-id="eb998-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="eb998-207">Seleccione el botón reproducir para iniciar la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="eb998-208">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="eb998-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="eb998-209">Abra otro explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="eb998-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="eb998-210">Arrastre la forma en una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="eb998-211">A continuación se muestra la forma en la otra ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="eb998-212">Dado que la aplicación limita el número de mensajes que se envían al servidor, la animación no aparecerá como Smooth en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="eb998-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="eb998-213">Agregar el bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="eb998-213">Add the server loop</span></span>

<span data-ttu-id="eb998-214">En la aplicación actual, los mensajes enviados desde el servidor al cliente salen con tanta frecuencia como se reciben.</span><span class="sxs-lookup"><span data-stu-id="eb998-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="eb998-215">Este tráfico de red presenta un problema similar al que vemos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="eb998-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="eb998-216">La aplicación puede enviar mensajes con más frecuencia de lo que son necesarios.</span><span class="sxs-lookup"><span data-stu-id="eb998-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="eb998-217">La conexión se puede saturar como resultado.</span><span class="sxs-lookup"><span data-stu-id="eb998-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="eb998-218">En esta sección se describe cómo actualizar el servidor para agregar un temporizador que limite la velocidad de los mensajes salientes.</span><span class="sxs-lookup"><span data-stu-id="eb998-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="eb998-219">Reemplace el contenido de `MoveShapeHub.cs` por este código:</span><span class="sxs-lookup"><span data-stu-id="eb998-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="eb998-220">Seleccione el botón reproducir para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb998-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="eb998-221">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="eb998-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="eb998-222">Abra otro explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="eb998-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="eb998-223">Arrastre la forma en una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="eb998-224">Este código expande el cliente para agregar la clase `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="eb998-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="eb998-225">La nueva clase limita los mensajes salientes mediante la `Timer` clase de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="eb998-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="eb998-226">Es conveniente saber que el propio concentrador es transitorio.</span><span class="sxs-lookup"><span data-stu-id="eb998-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="eb998-227">Se crea cada vez que se necesita.</span><span class="sxs-lookup"><span data-stu-id="eb998-227">It's created every time it's needed.</span></span> <span data-ttu-id="eb998-228">Por lo tanto, la aplicación crea el `Broadcaster` como un singleton.</span><span class="sxs-lookup"><span data-stu-id="eb998-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="eb998-229">Utiliza la inicialización diferida para aplazar la creación del `Broadcaster`hasta que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="eb998-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="eb998-230">Esto garantiza que la aplicación crea la primera instancia de concentrador completamente antes de iniciar el temporizador.</span><span class="sxs-lookup"><span data-stu-id="eb998-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="eb998-231">La llamada a la función de `UpdateShape` de los clientes se mueve fuera del método de `UpdateModel` del centro.</span><span class="sxs-lookup"><span data-stu-id="eb998-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="eb998-232">Ya no se llama inmediatamente cuando la aplicación recibe los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="eb998-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="eb998-233">En su lugar, la aplicación envía los mensajes a los clientes a una velocidad de 25 llamadas por segundo.</span><span class="sxs-lookup"><span data-stu-id="eb998-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="eb998-234">El proceso se administra mediante el temporizador de `_broadcastLoop` desde dentro de la clase `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="eb998-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="eb998-235">Por último, en lugar de llamar directamente al método de cliente desde el concentrador, la clase `Broadcaster` debe obtener una referencia al concentrador de `_hubContext` operativo actualmente.</span><span class="sxs-lookup"><span data-stu-id="eb998-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="eb998-236">Obtiene la referencia con el `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="eb998-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="eb998-237">Agregar animaciones suaves</span><span class="sxs-lookup"><span data-stu-id="eb998-237">Add smooth animation</span></span>

<span data-ttu-id="eb998-238">La aplicación está casi finalizada, pero podríamos mejorarla.</span><span class="sxs-lookup"><span data-stu-id="eb998-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="eb998-239">La aplicación mueve la forma en el cliente en respuesta a los mensajes del servidor.</span><span class="sxs-lookup"><span data-stu-id="eb998-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="eb998-240">En lugar de establecer la posición de la forma en la nueva ubicación proporcionada por el servidor, use la función `animate` de la biblioteca de interfaz de usuario de JQuery.</span><span class="sxs-lookup"><span data-stu-id="eb998-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="eb998-241">Puede mover la forma suavemente entre su posición actual y la nueva.</span><span class="sxs-lookup"><span data-stu-id="eb998-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="eb998-242">Actualice el método de `updateShape` del cliente en el archivo *default. html* para que tenga un aspecto similar al código resaltado:</span><span class="sxs-lookup"><span data-stu-id="eb998-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="eb998-243">Seleccione el botón reproducir para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb998-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="eb998-244">Copie la dirección URL de la página.</span><span class="sxs-lookup"><span data-stu-id="eb998-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="eb998-245">Abra otro explorador y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="eb998-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="eb998-246">Arrastre la forma en una de las ventanas del explorador.</span><span class="sxs-lookup"><span data-stu-id="eb998-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="eb998-247">El movimiento de la forma en la otra ventana aparece menos entrecortado.</span><span class="sxs-lookup"><span data-stu-id="eb998-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="eb998-248">La aplicación interpola su movimiento a lo largo del tiempo, en lugar de establecerse una vez por cada mensaje entrante.</span><span class="sxs-lookup"><span data-stu-id="eb998-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="eb998-249">Este código mueve la forma de la ubicación anterior al nuevo.</span><span class="sxs-lookup"><span data-stu-id="eb998-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="eb998-250">El servidor proporciona la posición de la forma en el curso del intervalo de animación.</span><span class="sxs-lookup"><span data-stu-id="eb998-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="eb998-251">En este caso, eso es 100 milisegundos.</span><span class="sxs-lookup"><span data-stu-id="eb998-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="eb998-252">La aplicación borra cualquier animación anterior que se ejecute en la forma antes de que se inicie la nueva animación.</span><span class="sxs-lookup"><span data-stu-id="eb998-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="eb998-253">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="eb998-253">Get the code</span></span>

[<span data-ttu-id="eb998-254">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="eb998-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="eb998-255">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="eb998-255">Additional resources</span></span>

<span data-ttu-id="eb998-256">El paradigma de comunicación que acaba de aprender es útil para desarrollar juegos en línea y otras simulaciones, como [el juego del captador creado con signalr](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="eb998-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="eb998-257">Para obtener más información acerca de Signalr, consulte los siguientes recursos:</span><span class="sxs-lookup"><span data-stu-id="eb998-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="eb998-258">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="eb998-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="eb998-259">GitHub y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="eb998-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="eb998-260">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="eb998-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="eb998-261">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="eb998-261">Next steps</span></span>

<span data-ttu-id="eb998-262">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="eb998-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb998-263">Configuración del proyecto</span><span class="sxs-lookup"><span data-stu-id="eb998-263">Set up the project</span></span>
> * <span data-ttu-id="eb998-264">Creación de la aplicación base</span><span class="sxs-lookup"><span data-stu-id="eb998-264">Created the base application</span></span>
> * <span data-ttu-id="eb998-265">Asignado al centro cuando se inicia la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="eb998-266">Agregado el cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-266">Added the client</span></span>
> * <span data-ttu-id="eb998-267">Ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb998-267">Ran the app</span></span>
> * <span data-ttu-id="eb998-268">Agregado el bucle de cliente</span><span class="sxs-lookup"><span data-stu-id="eb998-268">Added the client loop</span></span>
> * <span data-ttu-id="eb998-269">Se agregó el bucle de servidor</span><span class="sxs-lookup"><span data-stu-id="eb998-269">Added the server loop</span></span>
> * <span data-ttu-id="eb998-270">Animación suave agregada</span><span class="sxs-lookup"><span data-stu-id="eb998-270">Added smooth animation</span></span>

<span data-ttu-id="eb998-271">Avance al siguiente artículo para aprender a crear una aplicación web que usa ASP.NET Signalr 2 para proporcionar la funcionalidad de difusión de servidor.</span><span class="sxs-lookup"><span data-stu-id="eb998-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="eb998-272">Signalr 2 y difusión de servidor</span><span class="sxs-lookup"><span data-stu-id="eb998-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)