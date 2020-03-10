---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 en un rol de trabajo de Azure-ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: hospede ASP.NET Web API en un rol de trabajo de Azure, usando OWIN para autohospedar el marco de API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448411"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a><span data-ttu-id="8b7b9-103">Host ASP.NET Web API 2 en un rol de trabajo de Azure</span><span class="sxs-lookup"><span data-stu-id="8b7b9-103">Host ASP.NET Web API 2 in an Azure Worker Role</span></span>

<span data-ttu-id="8b7b9-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8b7b9-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8b7b9-105">En este tutorial se muestra cómo hospedar ASP.NET Web API en un rol de trabajo de Azure, usando OWIN para autohospedar el marco de API Web.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-105">This tutorial shows how to host ASP.NET Web API in an Azure Worker Role, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="8b7b9-106">[Open Web Interface for .net](http://owin.org/) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-106">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="8b7b9-107">OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS, por ejemplo, dentro de un rol de trabajo de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-107">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
>
> <span data-ttu-id="8b7b9-108">En este tutorial, usará el paquete Microsoft. Owin. host. HttpListener, que proporciona un servidor HTTP que se usa para aplicaciones OWIN de autohospedar.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-108">In this tutorial, you'll use the Microsoft.Owin.Host.HttpListener package, which provides an HTTP server that be used to self-host OWIN applications.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8b7b9-109">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="8b7b9-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="8b7b9-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8b7b9-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="8b7b9-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="8b7b9-111">Web API 2</span></span>
> - [<span data-ttu-id="8b7b9-112">SDK de Azure para .NET 2,3</span><span class="sxs-lookup"><span data-stu-id="8b7b9-112">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="8b7b9-113">Creación de un proyecto de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="8b7b9-113">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="8b7b9-114">Inicie Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-114">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="8b7b9-115">Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-115">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="8b7b9-116">En el menú **archivo** , haga clic en **nuevo**y, a continuación, haga clic en **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-116">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="8b7b9-117">En **plantillas instaladas**, en C#visual, haga clic en **nube** y, a continuación, haga clic en **servicio en la nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-117">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="8b7b9-118">Asigne al proyecto el nombre "AzureApp" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-118">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="8b7b9-119">En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , haga doble clic en **rol de trabajo**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-119">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="8b7b9-120">Deje el nombre predeterminado ("WorkerRole1").</span><span class="sxs-lookup"><span data-stu-id="8b7b9-120">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="8b7b9-121">En este paso se agrega un rol de trabajo a la solución.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-121">This step adds a worker role to the solution.</span></span> <span data-ttu-id="8b7b9-122">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-122">Click **OK**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="8b7b9-123">La solución de Visual Studio que se crea contiene dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-123">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="8b7b9-124">&quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-124">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="8b7b9-125">&quot;WorkerRole1&quot; contiene el código para el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-125">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="8b7b9-126">En general, una aplicación de Azure puede contener varios roles, aunque en este tutorial se usa un único rol.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-126">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="8b7b9-127">Incorporación de la API Web y los paquetes OWIN</span><span class="sxs-lookup"><span data-stu-id="8b7b9-127">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="8b7b9-128">En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-128">From the **Tools** menu, click **NuGet Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="8b7b9-129">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-129">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="8b7b9-130">Agregar un punto de conexión HTTP</span><span class="sxs-lookup"><span data-stu-id="8b7b9-130">Add an HTTP Endpoint</span></span>

<span data-ttu-id="8b7b9-131">En Explorador de soluciones, expanda el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-131">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="8b7b9-132">Expanda el nodo roles, haga clic con el botón secundario en WorkerRole1 y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-132">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="8b7b9-133">Haga clic en **Extremos** y, a continuación, en **Agregar extremo**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-133">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="8b7b9-134">En la lista desplegable **Protocolo** , seleccione "http".</span><span class="sxs-lookup"><span data-stu-id="8b7b9-134">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="8b7b9-135">En **puerto público** y **puerto privado**, escriba 80.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-135">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="8b7b9-136">Estos números de puerto pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-136">These port numbers can be different.</span></span> <span data-ttu-id="8b7b9-137">El puerto público es lo que los clientes usan cuando envían una solicitud al rol.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-137">The public port is what clients use when they send a request to the role.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="8b7b9-138">Configuración de la API Web para el autohospedaje</span><span class="sxs-lookup"><span data-stu-id="8b7b9-138">Configure Web API for Self-Host</span></span>

<span data-ttu-id="8b7b9-139">En Explorador de soluciones, haga clic con el botón derecho en el proyecto WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-139">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="8b7b9-140">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-140">Name the class `Startup`.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="8b7b9-141">Reemplace todo el código reutilizable de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-141">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="8b7b9-142">Incorporación de un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="8b7b9-142">Add a Web API Controller</span></span>

<span data-ttu-id="8b7b9-143">A continuación, agregue una clase de controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-143">Next, add a Web API controller class.</span></span> <span data-ttu-id="8b7b9-144">Haga clic con el botón derecho en el proyecto WorkerRole1 y seleccione **agregar** / **clase**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-144">Right-click the WorkerRole1 project and select **Add** / **Class**.</span></span> <span data-ttu-id="8b7b9-145">Asigne a la clase el nombre TestController.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-145">Name the class TestController.</span></span> <span data-ttu-id="8b7b9-146">Reemplace todo el código reutilizable de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-146">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="8b7b9-147">Para simplificar, este controlador solo define dos métodos GET que devuelven texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-147">For simplicity, this controller just defines two GET methods that return plain text.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="8b7b9-148">Inicio del host OWIN</span><span class="sxs-lookup"><span data-stu-id="8b7b9-148">Start the OWIN Host</span></span>

<span data-ttu-id="8b7b9-149">Abra el archivo WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-149">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="8b7b9-150">Esta clase define el código que se ejecuta cuando se inicia y se detiene el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-150">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="8b7b9-151">Agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-151">Add the following using statement:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="8b7b9-152">Agregue un miembro **IDisposable** a la clase `WorkerRole`:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-152">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

<span data-ttu-id="8b7b9-153">En el método `OnStart`, agregue el código siguiente para iniciar el host:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-153">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

<span data-ttu-id="8b7b9-154">El método **webapp. Start** inicia el host OWIN.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-154">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="8b7b9-155">El nombre de la clase `Startup` es un parámetro de tipo para el método.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-155">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="8b7b9-156">Por Convención, el host llamará al método `Configure` de esta clase.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-156">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="8b7b9-157">Invalide el `OnStop` para desechar la instancia de la *aplicación\_* :</span><span class="sxs-lookup"><span data-stu-id="8b7b9-157">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="8b7b9-158">Este es el código completo de WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-158">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

<span data-ttu-id="8b7b9-159">Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-159">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="8b7b9-160">En función de la configuración del firewall, es posible que deba permitir el emulador a través del firewall.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-160">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="8b7b9-161">Si recibe una excepción como la siguiente, consulte [esta entrada de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para obtener una solución alternativa.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-161">If you get an exception like the following, please see [this blog post](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) for a workaround.</span></span> <span data-ttu-id="8b7b9-162">"No se pudo cargar el archivo o ensamblado ' Microsoft. Owin, version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' o una de sus dependencias.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-162">"Could not load file or assembly 'Microsoft.Owin, Version=2.0.2.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies.</span></span> <span data-ttu-id="8b7b9-163">La definición del manifiesto del ensamblado encontrado no coincide con la referencia de ensamblado.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-163">The located assembly's manifest definition does not match the assembly reference.</span></span> <span data-ttu-id="8b7b9-164">(Excepción de HRESULT: 0x80131040) "</span><span class="sxs-lookup"><span data-stu-id="8b7b9-164">(Exception from HRESULT: 0x80131040)"</span></span>

<span data-ttu-id="8b7b9-165">El emulador de proceso asigna una dirección IP local al extremo.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-165">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="8b7b9-166">Puede encontrar la dirección IP visualizando la interfaz de usuario del emulador de proceso.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-166">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="8b7b9-167">Haga clic con el botón derecho en el icono del emulador en el área de notificación de la barra de tareas y seleccione **Mostrar interfaz de usuario del emulador de proceso**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-167">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

<span data-ttu-id="8b7b9-168">Busque la dirección IP en implementaciones de servicio, implementación [id], detalles del servicio.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-168">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="8b7b9-169">Abra un explorador Web y vaya a la<em>dirección</em>http:///Test/1, donde <em>Dirección</em> es la dirección IP asignada por el emulador de proceso. por ejemplo, `http://127.0.0.1:80/test/1`.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-169">Open a web browser and navigate to http://<em>address</em>/test/1, where <em>address</em> is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80/test/1`.</span></span> <span data-ttu-id="8b7b9-170">Debería ver la respuesta del controlador de API Web:</span><span class="sxs-lookup"><span data-stu-id="8b7b9-170">You should see the response from the Web API controller:</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="8b7b9-171">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="8b7b9-171">Deploy to Azure</span></span>

<span data-ttu-id="8b7b9-172">En este paso, debe tener una cuenta de Azure.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-172">For this step, you must have an Azure account.</span></span> <span data-ttu-id="8b7b9-173">Si aún no tiene una, puede crear una cuenta de evaluación gratuita en un par de minutos.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-173">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8b7b9-174">Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="8b7b9-174">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="8b7b9-175">En Explorador de soluciones, haga clic con el botón secundario en el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-175">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="8b7b9-176">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-176">Select **Publish**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="8b7b9-177">Si no ha iniciado sesión en su cuenta de Azure, haga clic en **iniciar sesión**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-177">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

<span data-ttu-id="8b7b9-178">Una vez iniciada la sesión, elija una suscripción y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-178">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

<span data-ttu-id="8b7b9-179">Escriba un nombre para el servicio en la nube y elija una región.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-179">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="8b7b9-180">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-180">Click **Create**.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="8b7b9-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-181">Click **Publish**.</span></span>

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

<span data-ttu-id="8b7b9-182">La ventana registro de actividad de Azure muestra el progreso de la implementación.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-182">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="8b7b9-183">Cuando se implemente la aplicación, vaya a http://appname.cloudapp.net/test/1.</span><span class="sxs-lookup"><span data-stu-id="8b7b9-183">When the app is deployed, browse to http://appname.cloudapp.net/test/1.</span></span>

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a><span data-ttu-id="8b7b9-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8b7b9-184">Additional Resources</span></span>

- [<span data-ttu-id="8b7b9-185">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="8b7b9-185">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [<span data-ttu-id="8b7b9-186">Proyecto Katana en GitHub</span><span class="sxs-lookup"><span data-stu-id="8b7b9-186">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana)
