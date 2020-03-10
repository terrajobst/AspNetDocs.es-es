---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introducción con Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Use ASP.NET Signalr para crear una aplicación de chat en tiempo real en una página HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505753"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="71317-103">Tutorial: Introducción con Signalr 1. x</span><span class="sxs-lookup"><span data-stu-id="71317-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="71317-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="71317-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="71317-105">En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="71317-106">Agregará Signalr a una aplicación Web ASP.NET vacía y creará una página HTML para enviar y mostrar mensajes.</span><span class="sxs-lookup"><span data-stu-id="71317-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="71317-107">Información general</span><span class="sxs-lookup"><span data-stu-id="71317-107">Overview</span></span>

<span data-ttu-id="71317-108">En este tutorial se presenta el desarrollo de Signalr al mostrar cómo crear una sencilla aplicación de chat basada en explorador.</span><span class="sxs-lookup"><span data-stu-id="71317-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="71317-109">Agregará la biblioteca de Signalr a una aplicación Web de ASP.NET vacía, creará una clase de concentrador para enviar mensajes a los clientes y creará una página HTML que permita a los usuarios enviar y recibir mensajes de chat.</span><span class="sxs-lookup"><span data-stu-id="71317-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="71317-110">Para ver un tutorial similar que muestra cómo crear una aplicación de chat en MVC 4 mediante una vista de MVC, vea [Introducción con signalr y MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="71317-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="71317-111">En este tutorial se usa la versión Release (1. x) de Signalr.</span><span class="sxs-lookup"><span data-stu-id="71317-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="71317-112">Para más información sobre los cambios entre Signalr 1. x y 2,0, consulte [actualización de proyectos de signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="71317-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="71317-113">Signalr es una biblioteca .NET de código abierto para compilar aplicaciones web que requieren interacción con el usuario activo o actualizaciones de datos en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="71317-114">Algunos ejemplos son las aplicaciones sociales, los juegos multiusuario, la colaboración empresarial y las aplicaciones de noticias, el tiempo o las actualizaciones financieras.</span><span class="sxs-lookup"><span data-stu-id="71317-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="71317-115">A menudo se denominan aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-115">These are often called real-time applications.</span></span>

<span data-ttu-id="71317-116">Signalr simplifica el proceso de creación de aplicaciones en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="71317-117">Incluye una biblioteca de servidores de ASP.NET y una biblioteca de cliente de JavaScript para facilitar la administración de las conexiones de cliente-servidor y la instalación de actualizaciones de contenido en los clientes.</span><span class="sxs-lookup"><span data-stu-id="71317-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="71317-118">Puede Agregar la biblioteca de Signalr a una aplicación de ASP.NET existente para obtener funcionalidad en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="71317-119">En el tutorial se muestran las siguientes tareas de desarrollo de Signalr:</span><span class="sxs-lookup"><span data-stu-id="71317-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="71317-120">Adición de la biblioteca de Signalr a una aplicación Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71317-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="71317-121">Crear una clase de concentrador para enviar contenido a los clientes.</span><span class="sxs-lookup"><span data-stu-id="71317-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="71317-122">Uso de la biblioteca de jQuery de Signalr en una página web para enviar mensajes y Mostrar actualizaciones desde el concentrador.</span><span class="sxs-lookup"><span data-stu-id="71317-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="71317-123">En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en un explorador.</span><span class="sxs-lookup"><span data-stu-id="71317-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="71317-124">Cada usuario nuevo puede publicar comentarios y ver los comentarios agregados una vez que el usuario se une al chat.</span><span class="sxs-lookup"><span data-stu-id="71317-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instancias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="71317-126">Sección</span><span class="sxs-lookup"><span data-stu-id="71317-126">Sections:</span></span>

- [<span data-ttu-id="71317-127">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="71317-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="71317-128">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="71317-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="71317-129">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="71317-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="71317-130">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="71317-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="71317-131">Configurar el proyecto</span><span class="sxs-lookup"><span data-stu-id="71317-131">Set up the Project</span></span>

<span data-ttu-id="71317-132">En esta sección se muestra cómo crear una aplicación Web de ASP.NET vacía, agregar Signalr y crear la aplicación de chat.</span><span class="sxs-lookup"><span data-stu-id="71317-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="71317-133">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="71317-133">Prerequisites:</span></span>

- <span data-ttu-id="71317-134">Visual Studio 2010 SP1 o 2012.</span><span class="sxs-lookup"><span data-stu-id="71317-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="71317-135">Si no tiene Visual Studio, consulte descargas de [ASP.net](https://www.asp.net/downloads) para obtener la herramienta gratuita de desarrollo de visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="71317-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="71317-136">[Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="71317-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="71317-137">En Visual Studio 2012, este instalador agrega nuevas características de ASP.NET, incluidas las plantillas de Signalr a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="71317-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="71317-138">Para Visual Studio 2010 SP1, no hay un instalador disponible, pero puede completar el tutorial mediante la instalación del paquete de NuGet Signalr como se describe en los pasos de configuración.</span><span class="sxs-lookup"><span data-stu-id="71317-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="71317-139">En los pasos siguientes se usa Visual Studio 2012 para crear una aplicación Web vacía de ASP.NET y agregar la biblioteca de Signalr:</span><span class="sxs-lookup"><span data-stu-id="71317-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="71317-140">En Visual Studio, cree una aplicación Web vacía de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="71317-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Crear Web vacío](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="71317-142">Para abrir la **consola del administrador de paquetes** , seleccione **herramientas | Administrador de paquetes NuGet | Consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="71317-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="71317-143">Escriba el siguiente comando en la ventana de la consola:</span><span class="sxs-lookup"><span data-stu-id="71317-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="71317-144">Este comando instala la versión más reciente de Signalr 1. x.</span><span class="sxs-lookup"><span data-stu-id="71317-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="71317-145">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar | Clase**.</span><span class="sxs-lookup"><span data-stu-id="71317-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="71317-146">Asigne a la nueva clase el nombre **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="71317-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="71317-147">En **Explorador de soluciones** expanda el nodo scripts.</span><span class="sxs-lookup"><span data-stu-id="71317-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="71317-148">Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="71317-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Referencias de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="71317-150">Reemplace el código de la clase **ChatHub** por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="71317-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="71317-151">En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="71317-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="71317-152">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **clase de aplicación global** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="71317-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Agregar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="71317-154">Agregue las siguientes instrucciones de `using` después de las instrucciones de `using` proporcionadas en la clase Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="71317-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="71317-155">Agregue la siguiente línea de código en el método `Application_Start` de la clase global para registrar la ruta predeterminada de los concentradores Signalr.</span><span class="sxs-lookup"><span data-stu-id="71317-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="71317-156">En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="71317-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="71317-157">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione página HTML y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="71317-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="71317-158">En **Explorador de soluciones**, haga clic con el botón secundario en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.</span><span class="sxs-lookup"><span data-stu-id="71317-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="71317-159">Reemplace el código predeterminado de la página HTML por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="71317-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="71317-160">**Guarde todo** para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="71317-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="71317-161">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="71317-161">Run the Sample</span></span>

1. <span data-ttu-id="71317-162">Presione F5 para ejecutar el proyecto en modo de depuración.</span><span class="sxs-lookup"><span data-stu-id="71317-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="71317-163">La página HTML se carga en una instancia del explorador y solicita un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="71317-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="71317-165">Escriba un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="71317-165">Enter a user name.</span></span>
3. <span data-ttu-id="71317-166">Copie la dirección URL de la línea de dirección del explorador y Úsela para abrir dos instancias más del explorador.</span><span class="sxs-lookup"><span data-stu-id="71317-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="71317-167">En cada instancia del explorador, escriba un nombre de usuario único.</span><span class="sxs-lookup"><span data-stu-id="71317-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="71317-168">En cada instancia del explorador, agregue un comentario y haga clic en **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="71317-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="71317-169">Los comentarios deben mostrarse en todas las instancias del explorador.</span><span class="sxs-lookup"><span data-stu-id="71317-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="71317-170">Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor.</span><span class="sxs-lookup"><span data-stu-id="71317-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="71317-171">El centro difunde los comentarios a todos los usuarios actuales.</span><span class="sxs-lookup"><span data-stu-id="71317-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="71317-172">Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.</span><span class="sxs-lookup"><span data-stu-id="71317-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="71317-173">En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en tres instancias del explorador, todas ellas actualizadas cuando una instancia envía un mensaje:</span><span class="sxs-lookup"><span data-stu-id="71317-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="71317-175">En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="71317-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="71317-176">Hay un archivo de script denominado **hubs** que la biblioteca de signalr genera dinámicamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="71317-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="71317-177">Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="71317-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de concentrador generado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="71317-179">Examinar el código</span><span class="sxs-lookup"><span data-stu-id="71317-179">Examine the Code</span></span>

<span data-ttu-id="71317-180">La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr: crear un concentrador como el objeto de coordinación principal en el servidor y usar la biblioteca de jQuery de Signalr para enviar y recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="71317-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="71317-181">Concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="71317-181">SignalR Hubs</span></span>

<span data-ttu-id="71317-182">En el ejemplo de código, la clase **ChatHub** se deriva de la clase **Microsoft. Aspnet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="71317-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="71317-183">Derivar de la clase **Hub** es una manera útil de compilar una aplicación signalr.</span><span class="sxs-lookup"><span data-stu-id="71317-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="71317-184">Puede crear métodos públicos en la clase de concentrador y, a continuación, acceder a esos métodos llamándolos desde scripts de jQuery en una página web.</span><span class="sxs-lookup"><span data-stu-id="71317-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="71317-185">En el código de chat, los clientes llaman al método **ChatHub. Send** para enviar un mensaje nuevo.</span><span class="sxs-lookup"><span data-stu-id="71317-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="71317-186">El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a **clients. All. broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="71317-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="71317-187">El método **send** muestra varios conceptos de Hub:</span><span class="sxs-lookup"><span data-stu-id="71317-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="71317-188">Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.</span><span class="sxs-lookup"><span data-stu-id="71317-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="71317-189">Use la propiedad dinámica **Microsoft. Aspnet. signalr. Hub. clients** para tener acceso a todos los clientes conectados a este concentrador.</span><span class="sxs-lookup"><span data-stu-id="71317-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="71317-190">Llame a una función de jQuery en el cliente (por ejemplo, la función `broadcastMessage`) para actualizar los clientes.</span><span class="sxs-lookup"><span data-stu-id="71317-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="71317-191">Signalr y jQuery</span><span class="sxs-lookup"><span data-stu-id="71317-191">SignalR and jQuery</span></span>

<span data-ttu-id="71317-192">En la página HTML del ejemplo de código se muestra cómo usar la biblioteca de jQuery de Signalr para comunicarse con un concentrador Signalr.</span><span class="sxs-lookup"><span data-stu-id="71317-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="71317-193">Las tareas esenciales del código consisten en declarar un proxy para que haga referencia al centro, declarando una función a la que el servidor puede llamar para enviar contenido a los clientes e iniciando una conexión para enviar mensajes al centro.</span><span class="sxs-lookup"><span data-stu-id="71317-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="71317-194">El código siguiente declara un proxy para un concentrador.</span><span class="sxs-lookup"><span data-stu-id="71317-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="71317-195">En jQuery, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas Camel.</span><span class="sxs-lookup"><span data-stu-id="71317-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="71317-196">En el ejemplo de código C# se hace referencia a la clase **ChatHub** de jQuery como **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="71317-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="71317-197">En el código siguiente se describe cómo crear una función de devolución de llamada en el script.</span><span class="sxs-lookup"><span data-stu-id="71317-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="71317-198">La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente.</span><span class="sxs-lookup"><span data-stu-id="71317-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="71317-199">Las dos líneas que codifican en HTML el contenido antes de mostrarse son opcionales y muestran una forma sencilla de evitar la inserción de scripts.</span><span class="sxs-lookup"><span data-stu-id="71317-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="71317-200">En el código siguiente se muestra cómo abrir una conexión con el centro.</span><span class="sxs-lookup"><span data-stu-id="71317-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="71317-201">El código inicia la conexión y, a continuación, la pasa una función para controlar el evento de clic en el botón **Enviar** de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="71317-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="71317-202">Este enfoque garantiza que la conexión se establece antes de que se ejecute el controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="71317-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="71317-203">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="71317-203">Next Steps</span></span>

<span data-ttu-id="71317-204">Ha aprendido que Signalr es un marco para compilar aplicaciones web en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="71317-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="71317-205">También ha aprendido varias tareas de desarrollo de Signalr: Cómo agregar Signalr a una aplicación de ASP.NET, cómo crear una clase de concentrador y cómo enviar y recibir mensajes desde el centro.</span><span class="sxs-lookup"><span data-stu-id="71317-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="71317-206">Puede hacer que la aplicación de ejemplo de este tutorial u otras aplicaciones de Signalr estén disponibles a través de Internet mediante su implementación en un proveedor de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="71317-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="71317-207">Microsoft ofrece hospedaje web gratuito para un máximo de 10 sitios web en una [cuenta de prueba gratuita de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="71317-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="71317-208">Para ver un tutorial sobre cómo implementar la aplicación Signalr de ejemplo, vea [publicar el ejemplo de signalr introducción como un sitio web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="71317-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="71317-209">Para obtener información detallada sobre cómo implementar un proyecto Web de Visual Studio en un sitio web de Windows Azure, vea [implementar una aplicación de ASP.net en un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="71317-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="71317-210">(Nota: actualmente no se admite el transporte de WebSocket para sitios web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="71317-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="71317-211">Cuando el transporte de WebSocket no está disponible, Signalr usa los otros transportes disponibles, tal y como se describe en la sección transportes del [tema Introducción a signalr](index.md).</span><span class="sxs-lookup"><span data-stu-id="71317-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="71317-212">Para obtener más información sobre los conceptos avanzados de los avances de Signalr, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:</span><span class="sxs-lookup"><span data-stu-id="71317-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="71317-213">Proyecto signalr</span><span class="sxs-lookup"><span data-stu-id="71317-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="71317-214">Github y ejemplos de signalr</span><span class="sxs-lookup"><span data-stu-id="71317-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="71317-215">Wiki de signalr</span><span class="sxs-lookup"><span data-stu-id="71317-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
