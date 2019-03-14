---
title: Introducción a SignalR de ASP.NET Core
author: bradygaster
description: En este tutorial, creará una aplicación de chat en la que se usa SignalR de ASP.NET Core.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr
ms.openlocfilehash: 53ec924c2d7b4fac227be0c0bf24d93476528167
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029032"
---
# <a name="tutorial-get-started-with-aspnet-core-signalr"></a><span data-ttu-id="d7d95-103">Tutorial: Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7d95-103">Tutorial: Get started with ASP.NET Core SignalR</span></span>

<span data-ttu-id="d7d95-104">En este tutorial se describen los conceptos básicos de la creación de una aplicación en tiempo real con SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-104">This tutorial teaches the basics of building a real-time app using SignalR.</span></span> <span data-ttu-id="d7d95-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="d7d95-105">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7d95-106">Cree un proyecto web.</span><span class="sxs-lookup"><span data-stu-id="d7d95-106">Create a web project.</span></span>
> * <span data-ttu-id="d7d95-107">Agregar la biblioteca cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-107">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d7d95-108">Crear un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-108">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d7d95-109">Configurar el proyecto para usar SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-109">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d7d95-110">Agregar código que envía mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="d7d95-110">Add code that sends messages from any client to all connected clients.</span></span>

<span data-ttu-id="d7d95-111">Al final, tendrá una aplicación de chat funcional:</span><span class="sxs-lookup"><span data-stu-id="d7d95-111">At the end, you'll have a working chat app:</span></span>

![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="d7d95-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="d7d95-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

[!INCLUDE [|Prerequisites](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="d7d95-114">Creación de un proyecto web</span><span class="sxs-lookup"><span data-stu-id="d7d95-114">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7d95-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7d95-115">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d7d95-116">En el menú, seleccione **Archivo > Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-116">From the menu, select **File > New Project**.</span></span>

* <span data-ttu-id="d7d95-117">En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado > Visual C# > Web > Aplicación web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-117">In the **New Project** dialog, select **Installed > Visual C# > Web > ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d7d95-118">Asigne al proyecto el nombre *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-118">Name the project *SignalRChat*.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-dialog.png)

* <span data-ttu-id="d7d95-120">Seleccione **Aplicación web** para crear un proyecto en el que se use Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d7d95-120">Select **Web Application** to create a project that uses Razor Pages.</span></span>

* <span data-ttu-id="d7d95-121">Seleccione una plataforma de destino de **.NET Core**, seleccione **ASP.NET Core 2.2** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-121">Select a target framework of **.NET Core**, select **ASP.NET Core 2.2**, and click **OK**.</span></span>

  ![Cuadro de diálogo Nuevo proyecto en Visual Studio](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7d95-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7d95-123">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d7d95-124">Abra el [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) en la carpeta en la que vaya a crear la del proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="d7d95-124">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) to the folder in which the new project folder will be created.</span></span>

* <span data-ttu-id="d7d95-125">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7d95-125">Run the following commands:</span></span>

   ```console
   dotnet new webapp -o SignalRChat
   code -r SignalRChat
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7d95-126">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d7d95-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7d95-127">En el menú, seleccione **Archivo > Nueva solución**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-127">From the menu, select **File > New Solution**.</span></span>

* <span data-ttu-id="d7d95-128">Seleccione **.NET Core > Aplicación > Aplicación web ASP.NET Core** (no seleccione **Aplicación web de ASP.NET Core (MVC)**).</span><span class="sxs-lookup"><span data-stu-id="d7d95-128">Select **.NET Core > App > ASP.NET Core Web App** (Don't select **ASP.NET Core Web App (MVC)**).</span></span>

* <span data-ttu-id="d7d95-129">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-129">Select **Next**.</span></span>

* <span data-ttu-id="d7d95-130">Asigne el nombre *SignalRChat* al proyecto y, después, haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-130">Name the project *SignalRChat*, and then select **Create**.</span></span>

---

## <a name="add-the-signalr-client-library"></a><span data-ttu-id="d7d95-131">Agregar la biblioteca cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="d7d95-131">Add the SignalR client library</span></span>

<span data-ttu-id="d7d95-132">La biblioteca de servidor de SignalR se incluye en el metapaquete `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="d7d95-132">The SignalR server library is included in the `Microsoft.AspNetCore.App` metapackage.</span></span> <span data-ttu-id="d7d95-133">La biblioteca cliente de JavaScript no se incluye automáticamente en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d7d95-133">The JavaScript client library isn't automatically included in the project.</span></span> <span data-ttu-id="d7d95-134">En este tutorial, usará el Administrador de bibliotecas (LibMan) para obtener la biblioteca cliente de *unpkg*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-134">For this tutorial, you use Library Manager (LibMan) to get the client library from *unpkg*.</span></span> <span data-ttu-id="d7d95-135">unpkg es una red de entrega de contenido (CDN) que puede entregar todo lo que encuentre en npm, el administrador de paquetes de Node.js.</span><span class="sxs-lookup"><span data-stu-id="d7d95-135">unpkg is a content delivery network (CDN)) that can deliver anything found in npm, the Node.js package manager.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7d95-136">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7d95-136">Visual Studio</span></span>](#tab/visual-studio/)

* <span data-ttu-id="d7d95-137">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **Client-Side Library** (Biblioteca del lado cliente).</span><span class="sxs-lookup"><span data-stu-id="d7d95-137">In **Solution Explorer**, right-click the project, and select **Add** > **Client-Side Library**.</span></span>

* <span data-ttu-id="d7d95-138">En el cuadro de diálogo **Add Client-Side Library** (Agregar biblioteca del lado cliente), en **Proveedor**, seleccione **unpkg**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-138">In the **Add Client-Side Library** dialog, for **Provider** select **unpkg**.</span></span> 

* <span data-ttu-id="d7d95-139">En **Biblioteca**, escriba `@aspnet/signalr@1` y seleccione la versión más reciente que no sea una versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="d7d95-139">For **Library**, enter `@aspnet/signalr@1`, and select the latest version that isn't preview.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de la biblioteca](signalr/_static/libman1.png)

* <span data-ttu-id="d7d95-141">Seleccione **Choose specific files** (Elegir archivos específicos), expanda la carpeta *dist/browser* y seleccione *signalr.js* y *signalr.min.js*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-141">Select **Choose specific files**, expand the *dist/browser* folder, and select *signalr.js* and *signalr.min.js*.</span></span>

* <span data-ttu-id="d7d95-142">Establezca **Ubicación de destino** en *wwwroot/lib/signalr/* y seleccione **Instalar**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-142">Set **Target Location** to *wwwroot/lib/signalr/*, and select **Install**.</span></span>

  ![Cuadro de diálogo Add Client-Side Library (Agregar biblioteca del lado cliente): selección de archivos y destino](signalr/_static/libman2.png)

  <span data-ttu-id="d7d95-144">LibMan crea una carpeta *wwwroot/lib/signalr* y copia en ella los archivos seleccionados.</span><span class="sxs-lookup"><span data-stu-id="d7d95-144">LibMan creates a *wwwroot/lib/signalr* folder and copies the selected files to it.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7d95-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7d95-145">Visual Studio Code</span></span>](#tab/visual-studio-code/)

* <span data-ttu-id="d7d95-146">En el terminal integrado, ejecute el comando siguiente para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7d95-146">In the integrated terminal, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d7d95-147">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7d95-147">Run the following command to get the SignalR client library by using LibMan.</span></span> <span data-ttu-id="d7d95-148">Puede que deba esperar unos segundos antes de ver la salida.</span><span class="sxs-lookup"><span data-stu-id="d7d95-148">You might have to wait a few seconds before seeing output.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d7d95-149">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7d95-149">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d7d95-150">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="d7d95-150">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d7d95-151">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-151">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d7d95-152">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="d7d95-152">Copy only the specified files.</span></span>

  <span data-ttu-id="d7d95-153">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-153">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7d95-154">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d7d95-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7d95-155">En el **terminal**, ejecute el siguiente comando para instalar LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7d95-155">In the **Terminal**, run the following command to install LibMan.</span></span>

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* <span data-ttu-id="d7d95-156">Vaya a la carpeta de proyecto (la que contiene el archivo *SignalRChat.csproj*).</span><span class="sxs-lookup"><span data-stu-id="d7d95-156">Navigate to the project folder (the one that contains the *SignalRChat.csproj* file).</span></span>

* <span data-ttu-id="d7d95-157">Ejecute el comando siguiente para obtener la biblioteca cliente de SignalR con LibMan.</span><span class="sxs-lookup"><span data-stu-id="d7d95-157">Run the following command to get the SignalR client library by using LibMan.</span></span>

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot/lib/signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  <span data-ttu-id="d7d95-158">Los parámetros especifican las opciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="d7d95-158">The parameters specify the following options:</span></span>
  * <span data-ttu-id="d7d95-159">Use el proveedor unpkg.</span><span class="sxs-lookup"><span data-stu-id="d7d95-159">Use the unpkg provider.</span></span>
  * <span data-ttu-id="d7d95-160">Copie los archivos en el destino *wwwroot/lib/signalr*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-160">Copy files to the *wwwroot/lib/signalr* destination.</span></span>
  * <span data-ttu-id="d7d95-161">Copie solo los archivos especificados.</span><span class="sxs-lookup"><span data-stu-id="d7d95-161">Copy only the specified files.</span></span>

  <span data-ttu-id="d7d95-162">La salida se parece al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-162">The output looks like the following example:</span></span>

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot/lib/signalr"
  ```

---

## <a name="create-a-signalr-hub"></a><span data-ttu-id="d7d95-163">Creación de un concentrador de SignalR</span><span class="sxs-lookup"><span data-stu-id="d7d95-163">Create a SignalR hub</span></span>

<span data-ttu-id="d7d95-164">Un *concentrador* es una clase que actúa como una canalización general que controla la comunicación entre el cliente y el servidor.</span><span class="sxs-lookup"><span data-stu-id="d7d95-164">A *hub* is a class that serves as a high-level pipeline that handles client-server communication.</span></span>

* <span data-ttu-id="d7d95-165">En la carpeta del proyecto SignalRChat, cree una carpeta *Hubs*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-165">In the SignalRChat project folder, create a *Hubs* folder.</span></span>

* <span data-ttu-id="d7d95-166">En la carpeta *Hubs*, cree un archivo *ChatHub.cs* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-166">In the *Hubs* folder, create a *ChatHub.cs* file with the following code:</span></span>

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  <span data-ttu-id="d7d95-167">La clase `ChatHub` hereda de la clase `Hub` de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-167">The `ChatHub` class inherits from the SignalR `Hub` class.</span></span> <span data-ttu-id="d7d95-168">La clase `Hub` administra las conexiones, los grupos y la mensajería.</span><span class="sxs-lookup"><span data-stu-id="d7d95-168">The `Hub` class manages connections, groups, and messaging.</span></span>

  <span data-ttu-id="d7d95-169">Puede llamarse al método `SendMessage` mediante un cliente conectado para enviar un mensaje a todos los clientes.</span><span class="sxs-lookup"><span data-stu-id="d7d95-169">The `SendMessage` method can be called by a connected client to send a message to all clients.</span></span> <span data-ttu-id="d7d95-170">El código de cliente de JavaScript que llama al método se muestra más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="d7d95-170">JavaScript client code that calls the method is shown later in the tutorial.</span></span> <span data-ttu-id="d7d95-171">El código de SignalR es asincrónico para proporcionar la máxima escalabilidad.</span><span class="sxs-lookup"><span data-stu-id="d7d95-171">SignalR code is asynchronous to provide maximum scalability.</span></span>

## <a name="configure-signalr"></a><span data-ttu-id="d7d95-172">Configuración de SignalR</span><span class="sxs-lookup"><span data-stu-id="d7d95-172">Configure SignalR</span></span>

<span data-ttu-id="d7d95-173">El servidor de SignalR se debe configurar para que pase las solicitudes de SignalR a SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-173">The SignalR server must be configured to pass SignalR requests to SignalR.</span></span>

* <span data-ttu-id="d7d95-174">Agregue el código resaltado siguiente al archivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d7d95-174">Add the following highlighted code to the *Startup.cs* file.</span></span>

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  <span data-ttu-id="d7d95-175">Estos cambios agregan SignalR al sistema de inserción de dependencias de ASP.NET Core y a la canalización de software intermedio.</span><span class="sxs-lookup"><span data-stu-id="d7d95-175">These changes add SignalR to the ASP.NET Core dependency injection system and the middleware pipeline.</span></span>

## <a name="add-signalr-client-code"></a><span data-ttu-id="d7d95-176">Adición del código de cliente de SignalR</span><span class="sxs-lookup"><span data-stu-id="d7d95-176">Add SignalR client code</span></span>

* <span data-ttu-id="d7d95-177">Reemplace el contenido de *Pages/Index.cshtml* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-177">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  <span data-ttu-id="d7d95-178">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d7d95-178">The preceding code:</span></span>

  * <span data-ttu-id="d7d95-179">Crea cuadros de texto para el nombre y el mensaje de texto, y un botón de envío.</span><span class="sxs-lookup"><span data-stu-id="d7d95-179">Creates text boxes for name and message text, and a submit button.</span></span>
  * <span data-ttu-id="d7d95-180">Crea una lista con `id="messagesList"` para mostrar los mensajes que se reciben desde el concentrador SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-180">Creates a list with `id="messagesList"` for displaying messages that are received from the SignalR hub.</span></span>
  * <span data-ttu-id="d7d95-181">Incluye las referencias de script en SignalR y el código de aplicación de *chat.js* que se va a crear en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="d7d95-181">Includes script references to SignalR and the *chat.js* application code that you create in the next step.</span></span>

* <span data-ttu-id="d7d95-182">En la carpeta *wwwroot/js*, cree un archivo *chat.js* con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-182">In the *wwwroot/js* folder, create a *chat.js* file with the following code:</span></span>

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  <span data-ttu-id="d7d95-183">El código anterior:</span><span class="sxs-lookup"><span data-stu-id="d7d95-183">The preceding code:</span></span>

  * <span data-ttu-id="d7d95-184">Crea e inicia una conexión.</span><span class="sxs-lookup"><span data-stu-id="d7d95-184">Creates and starts a connection.</span></span>
  * <span data-ttu-id="d7d95-185">Agrega al botón de envío un controlador que envía mensajes al concentrador.</span><span class="sxs-lookup"><span data-stu-id="d7d95-185">Adds to the submit button a handler that sends messages to the hub.</span></span>
  * <span data-ttu-id="d7d95-186">Agrega al objeto de conexión un controlador que recibe mensajes desde el concentrador y los agrega a la lista.</span><span class="sxs-lookup"><span data-stu-id="d7d95-186">Adds to the connection object a handler that receives messages from the hub and adds them to the list.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="d7d95-187">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="d7d95-187">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d7d95-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d7d95-188">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d7d95-189">Presione **CTRL+F5** para ejecutar la aplicación sin depurar.</span><span class="sxs-lookup"><span data-stu-id="d7d95-189">Press **CTRL+F5** to run the app without debugging.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="d7d95-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d7d95-190">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="d7d95-191">En el terminal integrado, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d7d95-191">In the integrated terminal, run the following command:</span></span>

  ```console
  dotnet run -p SignalRChat.csproj
  ```
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="d7d95-192">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="d7d95-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="d7d95-193">En el menú, seleccione **Ejecutar > Iniciar sin depurar**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-193">From the menu, select **Run > Start Without Debugging**.</span></span>

---

* <span data-ttu-id="d7d95-194">Copie la dirección URL de la barra de direcciones, abra otra instancia o pestaña del explorador, y pegue la dirección URL en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="d7d95-194">Copy the URL from the address bar, open another browser instance or tab, and paste the URL in the address bar.</span></span>

* <span data-ttu-id="d7d95-195">Elija cualquier explorador, escriba un nombre y un mensaje, y haga clic en el botón **Enviar mensaje**.</span><span class="sxs-lookup"><span data-stu-id="d7d95-195">Choose either browser, enter a name and message, and select the **Send Message** button.</span></span>

  <span data-ttu-id="d7d95-196">El nombre y el mensaje se muestran en ambas páginas al instante.</span><span class="sxs-lookup"><span data-stu-id="d7d95-196">The name and message are displayed on both pages instantly.</span></span>

  ![Aplicación de ejemplo de SignalR](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> <span data-ttu-id="d7d95-198">Si la aplicación no funciona, abra las herramientas para desarrolladores del explorador (F12) y vaya a la consola.</span><span class="sxs-lookup"><span data-stu-id="d7d95-198">If the app doesn't work, open your browser developer tools (F12) and go to the console.</span></span> <span data-ttu-id="d7d95-199">Es posible que vea errores relacionados con el código HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d7d95-199">You might see errors related to your HTML and JavaScript code.</span></span> <span data-ttu-id="d7d95-200">Por ejemplo, suponga que coloca *signalr.js* en una carpeta distinta a la indicada.</span><span class="sxs-lookup"><span data-stu-id="d7d95-200">For example, suppose you put *signalr.js* in a different folder than directed.</span></span> <span data-ttu-id="d7d95-201">En ese caso, la referencia a ese archivo no funcionará y verá un error 404 en la consola.</span><span class="sxs-lookup"><span data-stu-id="d7d95-201">In that case the reference to that file won't work and you'll see a 404 error in the console.</span></span>
> <span data-ttu-id="d7d95-202">![Error: signalr.js no encontrado](signalr/_static/f12-console.png)</span><span class="sxs-lookup"><span data-stu-id="d7d95-202">![signalr.js not found error](signalr/_static/f12-console.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7d95-203">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d7d95-203">Next steps</span></span>

<span data-ttu-id="d7d95-204">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="d7d95-204">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7d95-205">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="d7d95-205">Create a web app project.</span></span>
> * <span data-ttu-id="d7d95-206">Agregar la biblioteca cliente de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-206">Add the SignalR client library.</span></span>
> * <span data-ttu-id="d7d95-207">Crear un concentrador de SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-207">Create a SignalR hub.</span></span>
> * <span data-ttu-id="d7d95-208">Configurar el proyecto para usar SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d95-208">Configure the project to use SignalR.</span></span>
> * <span data-ttu-id="d7d95-209">Agregar código que usa el concentrador para enviar mensajes desde cualquier cliente a todos los clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="d7d95-209">Add code that uses the hub to send messages from any client to all connected clients.</span></span>

<span data-ttu-id="d7d95-210">Para obtener más información sobre SignalR, vea la introducción:</span><span class="sxs-lookup"><span data-stu-id="d7d95-210">To learn more about SignalR, see the introduction:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d7d95-211">Introducción a SignalR de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7d95-211">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
