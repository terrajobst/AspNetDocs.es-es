---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Signalr ampliación con Azure Service Bus (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449941"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="1d7ab-102">Escalabilidad horizontal de SignalR con Azure Service Bus (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1d7ab-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="1d7ab-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1d7ab-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="1d7ab-104">En este tutorial, implementará una aplicación Signalr en un rol Web de Windows Azure, mediante el Service Bus backplane para distribuir los mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="1d7ab-105">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-105">Prerequisites:</span></span>

- <span data-ttu-id="1d7ab-106">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-106">A Windows Azure account.</span></span>
- <span data-ttu-id="1d7ab-107">El [SDK de Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1d7ab-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="1d7ab-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-108">Visual Studio 2012.</span></span>

<span data-ttu-id="1d7ab-109">El backplane de Service Bus también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1,1.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="1d7ab-110">Sin embargo, no es compatible con la versión 1,0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="1d7ab-111">Precio</span><span class="sxs-lookup"><span data-stu-id="1d7ab-111">Pricing</span></span>

<span data-ttu-id="1d7ab-112">El backplane Service Bus usa temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="1d7ab-113">Para obtener la información más reciente sobre los precios, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="1d7ab-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="1d7ab-114">En el momento de redactar este documento, puede enviar 1 millón mensajes al mes de menos de $1.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="1d7ab-115">El backplane envía un mensaje de Service Bus para cada invocación de un método de concentrador de Signalr.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="1d7ab-116">También hay algunos mensajes de control para las conexiones, las desconexiones, la Unión o el abandono de grupos, etc.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="1d7ab-117">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="1d7ab-118">Información general</span><span class="sxs-lookup"><span data-stu-id="1d7ab-118">Overview</span></span>

<span data-ttu-id="1d7ab-119">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="1d7ab-120">Use el Azure Portal de Windows para crear un nuevo espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="1d7ab-121">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="1d7ab-122">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="1d7ab-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="1d7ab-123">Microsoft. AspNet. Signalr. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="1d7ab-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="1d7ab-124">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="1d7ab-125">Agregue el código siguiente a global. asax para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="1d7ab-126">Para cada aplicación, elija un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="1d7ab-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="1d7ab-127">No use el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="1d7ab-128">Creación de los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="1d7ab-128">Create the Azure Services</span></span>

<span data-ttu-id="1d7ab-129">Cree un servicio en la nube, como se describe en [creación e implementación de un servicio en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="1d7ab-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="1d7ab-130">Siga los pasos de la sección "Cómo: crear un servicio en la nube mediante creación rápida".</span><span class="sxs-lookup"><span data-stu-id="1d7ab-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="1d7ab-131">En este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="1d7ab-132">Cree un nuevo espacio de nombres de Service Bus, como se describe en [uso de Service Bus temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="1d7ab-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="1d7ab-133">Siga los pasos de la sección "crear un espacio de nombres de servicio".</span><span class="sxs-lookup"><span data-stu-id="1d7ab-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="1d7ab-134">Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="1d7ab-135">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d7ab-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="1d7ab-136">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-136">Start Visual Studio.</span></span> <span data-ttu-id="1d7ab-137">En el menú **Archivo**, haga clic en **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="1d7ab-138">En el cuadro de diálogo **nuevo proyecto** , expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="1d7ab-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="1d7ab-139">En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio en la nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="1d7ab-140">Mantenga el valor predeterminado .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="1d7ab-141">Asigne a la aplicación el nombre ChatService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="1d7ab-142">En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , seleccione rol Web ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="1d7ab-143">Haga clic en el botón de flecha derecha ( **&gt;** ) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="1d7ab-144">Mantenga el mouse sobre el nuevo rol, de modo que el icono de lápiz esté visible.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="1d7ab-145">Haga clic en este icono para cambiar el nombre del rol.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-145">Click this icon to rename the role.</span></span> <span data-ttu-id="1d7ab-146">Asigne el nombre "SignalRChat" al rol y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="1d7ab-147">En el Asistente para **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="1d7ab-148">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-148">Click **OK**.</span></span> <span data-ttu-id="1d7ab-149">El Asistente para proyectos crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="1d7ab-150">ChatService: este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="1d7ab-151">Define los roles de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="1d7ab-152">SignalRChat: este proyecto es el proyecto de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="1d7ab-153">Creación de la aplicación Signalr chat</span><span class="sxs-lookup"><span data-stu-id="1d7ab-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="1d7ab-154">Para crear la aplicación de chat, siga los pasos del tutorial [Introducción con signalr y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="1d7ab-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="1d7ab-155">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="1d7ab-156">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1d7ab-157">En la ventana de la **consola del administrador de paquetes** , escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="1d7ab-158">Use la opción `-ProjectName` para instalar los paquetes en el proyecto ASP.NET MVC, en lugar del proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="1d7ab-159">Configurar el backplane</span><span class="sxs-lookup"><span data-stu-id="1d7ab-159">Configure the Backplane</span></span>

<span data-ttu-id="1d7ab-160">En el archivo global. asax de la aplicación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d7ab-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="1d7ab-161">Ahora debe obtener la cadena de conexión de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="1d7ab-162">En el Azure Portal, seleccione el espacio de nombres de Service Bus que ha creado y haga clic en el icono clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="1d7ab-163">Copie la cadena de conexión en el portapapeles y, a continuación, péguela en la variable *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="1d7ab-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="1d7ab-164">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="1d7ab-164">Deploy to Azure</span></span>

<span data-ttu-id="1d7ab-165">En Explorador de soluciones, expanda la carpeta **roles** dentro del proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="1d7ab-166">Haga clic con el botón secundario en el rol SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="1d7ab-167">Seleccione la pestaña **configuración** . En **instancias** , seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="1d7ab-168">También puede establecer el tamaño de la máquina virtual en **extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="1d7ab-169">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-169">Save the changes.</span></span>

<span data-ttu-id="1d7ab-170">En Explorador de soluciones, haga clic con el botón secundario en el proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="1d7ab-171">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="1d7ab-172">Si esta es la primera vez que publica en Windows Azure, debe descargar las credenciales.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="1d7ab-173">En el Asistente para **publicación** , haga clic en "iniciar sesión para descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="1d7ab-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="1d7ab-174">Esto le pedirá que inicie sesión en el Azure Portal de Windows y descargue un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="1d7ab-175">Haga clic en **importar** y seleccione el archivo de configuración de publicación que ha descargado.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="1d7ab-176">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-176">Click **Next**.</span></span> <span data-ttu-id="1d7ab-177">En el cuadro de diálogo **configuración de publicación** , en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="1d7ab-178">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-178">Click **Publish**.</span></span> <span data-ttu-id="1d7ab-179">La implementación de la aplicación puede tardar unos minutos e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="1d7ab-180">Ahora, al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, mediante un tema Service Bus.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="1d7ab-181">Un tema es una cola de mensajes que permite varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="1d7ab-182">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="1d7ab-183">Para ver la actividad de las suscripciones y los mensajes, abra el Azure Portal, seleccione el espacio de nombres Service Bus y haga clic en "temas".</span><span class="sxs-lookup"><span data-stu-id="1d7ab-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="1d7ab-184">La actividad del mensaje tarda unos minutos en aparecer en el panel.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="1d7ab-185">Signalr administra la duración del tema.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="1d7ab-186">Siempre que se implemente la aplicación, no intente eliminar manualmente los temas ni cambiar la configuración del tema.</span><span class="sxs-lookup"><span data-stu-id="1d7ab-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
