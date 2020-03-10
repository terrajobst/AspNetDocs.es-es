---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Signalr ampliación con Azure Service Bus | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr version 2 de este tema para la versión Signalr 1. x de este tema,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467737"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="02aff-103">Escalabilidad horizontal de SignalR con Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="02aff-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="02aff-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="02aff-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="02aff-105">En este tutorial, implementará una aplicación Signalr en un rol Web de Windows Azure, mediante el Service Bus backplane para distribuir los mensajes a cada instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="02aff-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="02aff-106">(También puede usar el backplane Service Bus con [Web Apps en Azure App Service](https://docs.microsoft.com/azure/app-service-web/)).</span><span class="sxs-lookup"><span data-stu-id="02aff-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="02aff-107">Requisitos previos:</span><span class="sxs-lookup"><span data-stu-id="02aff-107">Prerequisites:</span></span>

- <span data-ttu-id="02aff-108">Una cuenta de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="02aff-108">A Windows Azure account.</span></span>
- <span data-ttu-id="02aff-109">El [SDK de Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="02aff-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="02aff-110">Visual Studio 2012 o 2013</span><span class="sxs-lookup"><span data-stu-id="02aff-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="02aff-111">El backplane de Service Bus también es compatible con [Service Bus para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versión 1,1.</span><span class="sxs-lookup"><span data-stu-id="02aff-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="02aff-112">Sin embargo, no es compatible con la versión 1,0 de Service Bus para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="02aff-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="02aff-113">Precio</span><span class="sxs-lookup"><span data-stu-id="02aff-113">Pricing</span></span>

<span data-ttu-id="02aff-114">El backplane Service Bus usa temas para enviar mensajes.</span><span class="sxs-lookup"><span data-stu-id="02aff-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="02aff-115">Para obtener la información más reciente sobre los precios, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="02aff-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="02aff-116">En el momento de redactar este documento, puede enviar 1 millón mensajes al mes de menos de $1.</span><span class="sxs-lookup"><span data-stu-id="02aff-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="02aff-117">El backplane envía un mensaje de Service Bus para cada invocación de un método de concentrador de Signalr.</span><span class="sxs-lookup"><span data-stu-id="02aff-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="02aff-118">También hay algunos mensajes de control para las conexiones, las desconexiones, la Unión o el abandono de grupos, etc.</span><span class="sxs-lookup"><span data-stu-id="02aff-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="02aff-119">En la mayoría de las aplicaciones, la mayoría del tráfico de mensajes serán las invocaciones de método de concentrador.</span><span class="sxs-lookup"><span data-stu-id="02aff-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="02aff-120">Información general</span><span class="sxs-lookup"><span data-stu-id="02aff-120">Overview</span></span>

<span data-ttu-id="02aff-121">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="02aff-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="02aff-122">Use el Azure Portal de Windows para crear un nuevo espacio de nombres de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="02aff-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="02aff-123">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="02aff-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="02aff-124">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="02aff-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="02aff-125">[Microsoft. Aspnet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) o [Microsoft. Aspnet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="02aff-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="02aff-126">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="02aff-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="02aff-127">Agregue el código siguiente a Startup.cs para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="02aff-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="02aff-128">Este código configura el Backplane con los valores predeterminados para [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="02aff-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="02aff-129">Para obtener información sobre cómo cambiar estos valores, vea [rendimiento de signalr: métricas de ampliación](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="02aff-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="02aff-130">Para cada aplicación, elija un valor diferente para "YourAppName".</span><span class="sxs-lookup"><span data-stu-id="02aff-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="02aff-131">No use el mismo valor en varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="02aff-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="02aff-132">Creación de los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="02aff-132">Create the Azure Services</span></span>

<span data-ttu-id="02aff-133">Cree un servicio en la nube, como se describe en [creación e implementación de un servicio en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="02aff-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="02aff-134">Siga los pasos de la sección "Cómo: crear un servicio en la nube mediante creación rápida".</span><span class="sxs-lookup"><span data-stu-id="02aff-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="02aff-135">En este tutorial, no es necesario cargar un certificado.</span><span class="sxs-lookup"><span data-stu-id="02aff-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="02aff-136">Cree un nuevo espacio de nombres de Service Bus, como se describe en [uso de Service Bus temas/suscripciones](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="02aff-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="02aff-137">Siga los pasos de la sección "crear un espacio de nombres de servicio".</span><span class="sxs-lookup"><span data-stu-id="02aff-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="02aff-138">Asegúrese de seleccionar la misma región para el servicio en la nube y el espacio de nombres Service Bus.</span><span class="sxs-lookup"><span data-stu-id="02aff-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="02aff-139">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02aff-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="02aff-140">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="02aff-140">Start Visual Studio.</span></span> <span data-ttu-id="02aff-141">En el menú **Archivo**, haga clic en **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="02aff-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="02aff-142">En el cuadro de diálogo **nuevo proyecto** , expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="02aff-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="02aff-143">En **plantillas instaladas**, seleccione **nube** y, a continuación, seleccione **servicio en la nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="02aff-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="02aff-144">Mantenga el valor predeterminado .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="02aff-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="02aff-145">Asigne a la aplicación el nombre ChatService y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="02aff-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="02aff-146">En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , seleccione rol Web ASP.net.</span><span class="sxs-lookup"><span data-stu-id="02aff-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="02aff-147">Haga clic en el botón de flecha derecha ( **&gt;** ) para agregar el rol a la solución.</span><span class="sxs-lookup"><span data-stu-id="02aff-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="02aff-148">Mantenga el mouse sobre el nuevo rol, de modo que el icono de lápiz esté visible.</span><span class="sxs-lookup"><span data-stu-id="02aff-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="02aff-149">Haga clic en este icono para cambiar el nombre del rol.</span><span class="sxs-lookup"><span data-stu-id="02aff-149">Click this icon to rename the role.</span></span> <span data-ttu-id="02aff-150">Asigne el nombre "SignalRChat" al rol y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="02aff-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="02aff-151">En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione **MVC**y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="02aff-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="02aff-152">El Asistente para proyectos crea dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="02aff-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="02aff-153">ChatService: este proyecto es la aplicación de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="02aff-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="02aff-154">Define los roles de Azure y otras opciones de configuración.</span><span class="sxs-lookup"><span data-stu-id="02aff-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="02aff-155">SignalRChat: este proyecto es el proyecto de ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="02aff-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="02aff-156">Creación de la aplicación Signalr chat</span><span class="sxs-lookup"><span data-stu-id="02aff-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="02aff-157">Para crear la aplicación de chat, siga los pasos del tutorial [Introducción con signalr y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="02aff-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="02aff-158">Use NuGet para instalar las bibliotecas necesarias.</span><span class="sxs-lookup"><span data-stu-id="02aff-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="02aff-159">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="02aff-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="02aff-160">En la ventana de la **consola del administrador de paquetes** , escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="02aff-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="02aff-161">Use la opción `-ProjectName` para instalar los paquetes en el proyecto ASP.NET MVC, en lugar del proyecto de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="02aff-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="02aff-162">Configurar el backplane</span><span class="sxs-lookup"><span data-stu-id="02aff-162">Configure the Backplane</span></span>

<span data-ttu-id="02aff-163">En el archivo Startup.cs de la aplicación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="02aff-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="02aff-164">Ahora debe obtener la cadena de conexión de Service Bus.</span><span class="sxs-lookup"><span data-stu-id="02aff-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="02aff-165">En el Azure Portal, seleccione el espacio de nombres de Service Bus que ha creado y haga clic en el icono clave de acceso.</span><span class="sxs-lookup"><span data-stu-id="02aff-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="02aff-166">Copie la cadena de conexión en el portapapeles y, a continuación, péguela en la variable *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="02aff-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="02aff-167">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="02aff-167">Deploy to Azure</span></span>

<span data-ttu-id="02aff-168">En Explorador de soluciones, expanda la carpeta **roles** dentro del proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="02aff-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="02aff-169">Haga clic con el botón secundario en el rol SignalRChat y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="02aff-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="02aff-170">Seleccione la pestaña **configuración** . En **instancias** , seleccione 2.</span><span class="sxs-lookup"><span data-stu-id="02aff-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="02aff-171">También puede establecer el tamaño de la máquina virtual en **extra pequeño**.</span><span class="sxs-lookup"><span data-stu-id="02aff-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="02aff-172">Guarde los cambios.</span><span class="sxs-lookup"><span data-stu-id="02aff-172">Save the changes.</span></span>

<span data-ttu-id="02aff-173">En Explorador de soluciones, haga clic con el botón secundario en el proyecto ChatService.</span><span class="sxs-lookup"><span data-stu-id="02aff-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="02aff-174">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="02aff-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="02aff-175">Si esta es la primera vez que publica en Windows Azure, debe descargar las credenciales.</span><span class="sxs-lookup"><span data-stu-id="02aff-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="02aff-176">En el Asistente para **publicación** , haga clic en "iniciar sesión para descargar credenciales".</span><span class="sxs-lookup"><span data-stu-id="02aff-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="02aff-177">Esto le pedirá que inicie sesión en el Azure Portal de Windows y descargue un archivo de configuración de publicación.</span><span class="sxs-lookup"><span data-stu-id="02aff-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="02aff-178">Haga clic en **importar** y seleccione el archivo de configuración de publicación que ha descargado.</span><span class="sxs-lookup"><span data-stu-id="02aff-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="02aff-179">Haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="02aff-179">Click **Next**.</span></span> <span data-ttu-id="02aff-180">En el cuadro de diálogo **configuración de publicación** , en **servicio en la nube**, seleccione el servicio en la nube que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="02aff-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="02aff-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="02aff-181">Click **Publish**.</span></span> <span data-ttu-id="02aff-182">La implementación de la aplicación puede tardar unos minutos e iniciar las máquinas virtuales.</span><span class="sxs-lookup"><span data-stu-id="02aff-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="02aff-183">Ahora, al ejecutar la aplicación de chat, las instancias de rol se comunican a través de Azure Service Bus, mediante un tema Service Bus.</span><span class="sxs-lookup"><span data-stu-id="02aff-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="02aff-184">Un tema es una cola de mensajes que permite varios suscriptores.</span><span class="sxs-lookup"><span data-stu-id="02aff-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="02aff-185">El backplane crea automáticamente el tema y las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="02aff-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="02aff-186">Para ver la actividad de las suscripciones y los mensajes, abra el Azure Portal, seleccione el espacio de nombres Service Bus y haga clic en "temas".</span><span class="sxs-lookup"><span data-stu-id="02aff-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="02aff-187">La actividad del mensaje tarda unos minutos en aparecer en el panel.</span><span class="sxs-lookup"><span data-stu-id="02aff-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="02aff-188">Signalr administra la duración del tema.</span><span class="sxs-lookup"><span data-stu-id="02aff-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="02aff-189">Siempre que se implemente la aplicación, no intente eliminar manualmente los temas ni cambiar la configuración del tema.</span><span class="sxs-lookup"><span data-stu-id="02aff-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="02aff-190">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="02aff-190">Troubleshooting</span></span>

<span data-ttu-id="02aff-191">**System. InvalidOperationException "el único IsolationLevel admitido es ' IsolationLevel. serializable '".**</span><span class="sxs-lookup"><span data-stu-id="02aff-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="02aff-192">Este error puede producirse si el nivel de transacción de una operación se establece en un valor distinto de `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="02aff-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="02aff-193">Compruebe que no se realiza ninguna operación con otros niveles de transacción.</span><span class="sxs-lookup"><span data-stu-id="02aff-193">Verify that no operations are being performed with other transaction levels.</span></span>
