---
uid: signalr/overview/performance/scaleout-with-redis
title: Signalr ampliación con Redis | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467779"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="2ce48-103">Escalabilidad horizontal de SignalR con Redis</span><span class="sxs-lookup"><span data-stu-id="2ce48-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="2ce48-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2ce48-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="2ce48-105">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="2ce48-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="2ce48-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="2ce48-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="2ce48-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="2ce48-107">.NET 4.5</span></span>
> - <span data-ttu-id="2ce48-108">Signalr versión 2,4</span><span class="sxs-lookup"><span data-stu-id="2ce48-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="2ce48-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="2ce48-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="2ce48-110">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="2ce48-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="2ce48-111">Questions and comments</span></span>
>
> <span data-ttu-id="2ce48-112">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="2ce48-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="2ce48-113">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="2ce48-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="2ce48-114">En este tutorial, usará [Redis](http://redis.io/) para distribuir los mensajes a través de una aplicación signalr implementada en dos instancias de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="2ce48-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="2ce48-115">Redis es un almacén de clave-valor en memoria.</span><span class="sxs-lookup"><span data-stu-id="2ce48-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="2ce48-116">También admite un sistema de mensajería con un modelo de publicación/suscripción.</span><span class="sxs-lookup"><span data-stu-id="2ce48-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="2ce48-117">El backplane de ReDiS ReDiS usa la característica pub/sub para reenviar mensajes a otros servidores.</span><span class="sxs-lookup"><span data-stu-id="2ce48-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="2ce48-118">En este tutorial, usará tres servidores:</span><span class="sxs-lookup"><span data-stu-id="2ce48-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="2ce48-119">Dos servidores que ejecutan Windows, que se usarán para implementar una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ce48-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="2ce48-120">Un servidor que ejecuta Linux, que usará para ejecutar Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="2ce48-121">En el caso de las capturas de pantallas de este tutorial, usé Ubuntu 12,04 TLS.</span><span class="sxs-lookup"><span data-stu-id="2ce48-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="2ce48-122">Si no tiene tres servidores físicos para usar, puede crear máquinas virtuales en Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="2ce48-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="2ce48-123">Otra opción es crear máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="2ce48-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="2ce48-124">Aunque en este tutorial se usa la implementación de Redis oficial, también hay un [Puerto de Windows de Redis](https://github.com/MSOpenTech/redis) desde MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="2ce48-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="2ce48-125">La instalación y configuración son diferentes, pero, de lo contrario, los pasos son los mismos.</span><span class="sxs-lookup"><span data-stu-id="2ce48-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="2ce48-126">Signalr ampliación con Redis no admite clústeres de Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="2ce48-127">Información general</span><span class="sxs-lookup"><span data-stu-id="2ce48-127">Overview</span></span>

<span data-ttu-id="2ce48-128">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="2ce48-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="2ce48-129">Instale Redis e inicie el servidor de Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="2ce48-130">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="2ce48-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="2ce48-131">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="2ce48-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="2ce48-132">Microsoft. AspNet. Signalr. StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="2ce48-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="2ce48-133">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ce48-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="2ce48-134">Agregue el código siguiente a Startup.cs para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="2ce48-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="2ce48-135">Ubuntu en Hyper-V</span><span class="sxs-lookup"><span data-stu-id="2ce48-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="2ce48-136">Con Windows Hyper-V, puede crear fácilmente una máquina virtual de Ubuntu en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="2ce48-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="2ce48-137">Descargue el ISO de Ubuntu desde [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="2ce48-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="2ce48-138">En Hyper-V, agregue una nueva máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="2ce48-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="2ce48-139">En el paso **Conectar disco duro virtual** , seleccione **crear un disco duro virtual**.</span><span class="sxs-lookup"><span data-stu-id="2ce48-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="2ce48-140">En el paso **Opciones de instalación** , seleccione Archivo de **imagen (. ISO)** , haga clic en **examinar**y busque el ISO de instalación de Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="2ce48-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="2ce48-141">Instalación de Redis</span><span class="sxs-lookup"><span data-stu-id="2ce48-141">Install Redis</span></span>

<span data-ttu-id="2ce48-142">Siga los pasos descritos en [http://redis.io/download](http://redis.io/download) para descargar y compilar Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="2ce48-143">Esto crea los archivos binarios de Redis en el directorio de `src`.</span><span class="sxs-lookup"><span data-stu-id="2ce48-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="2ce48-144">De forma predeterminada, Redis no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="2ce48-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="2ce48-145">Para establecer una contraseña, edite el archivo de `redis.conf`, que se encuentra en el directorio raíz del código fuente.</span><span class="sxs-lookup"><span data-stu-id="2ce48-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="2ce48-146">(Hacer una copia de seguridad del archivo antes de editarlo) Agregue la siguiente directiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="2ce48-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="2ce48-147">Ahora inicie el servidor de Redis:</span><span class="sxs-lookup"><span data-stu-id="2ce48-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="2ce48-148">Abra el puerto 6379, que es el puerto predeterminado en el que escucha Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="2ce48-149">(Puede cambiar el número de puerto en el archivo de configuración).</span><span class="sxs-lookup"><span data-stu-id="2ce48-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="2ce48-150">Creación de la aplicación Signalr</span><span class="sxs-lookup"><span data-stu-id="2ce48-150">Create the SignalR Application</span></span>

<span data-ttu-id="2ce48-151">Cree una aplicación de Signalr siguiendo uno de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="2ce48-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="2ce48-152">Introducción con Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="2ce48-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="2ce48-153">Introducción con Signalr 2,0 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="2ce48-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="2ce48-154">A continuación, modificaremos la aplicación de chat para admitir ampliación con Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="2ce48-155">En primer lugar, agregue el `Microsoft.AspNet.SignalR.StackExchangeRedis` paquete NuGet al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ce48-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="2ce48-156">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="2ce48-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="2ce48-157">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="2ce48-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="2ce48-158">A continuación, abra el archivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="2ce48-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="2ce48-159">Agregue el código siguiente al método de **configuración** :</span><span class="sxs-lookup"><span data-stu-id="2ce48-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="2ce48-160">"servidor" es el nombre del servidor que ejecuta Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="2ce48-161">*Port* es el número de Puerto</span><span class="sxs-lookup"><span data-stu-id="2ce48-161">*port* is the port number</span></span>
- <span data-ttu-id="2ce48-162">"contraseña" es la contraseña que definió en el archivo Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="2ce48-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="2ce48-163">"AppName" es cualquier cadena.</span><span class="sxs-lookup"><span data-stu-id="2ce48-163">"AppName" is any string.</span></span> <span data-ttu-id="2ce48-164">Signalr crea un canal pub/sub de Redis con este nombre.</span><span class="sxs-lookup"><span data-stu-id="2ce48-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="2ce48-165">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2ce48-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="2ce48-166">Implementación y ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="2ce48-166">Deploy and Run the Application</span></span>

<span data-ttu-id="2ce48-167">Preparar las instancias de Windows Server para implementar la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ce48-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="2ce48-168">Agregue el rol de IIS.</span><span class="sxs-lookup"><span data-stu-id="2ce48-168">Add the IIS role.</span></span> <span data-ttu-id="2ce48-169">Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="2ce48-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="2ce48-170">Incluya también el servicio de administración de (que aparece en "herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="2ce48-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="2ce48-171">**Instale Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="2ce48-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="2ce48-172">Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="2ce48-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="2ce48-173">En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="2ce48-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="2ce48-174">Compruebe que el servicio de administración web se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="2ce48-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="2ce48-175">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="2ce48-175">If not, start the service.</span></span> <span data-ttu-id="2ce48-176">(Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).</span><span class="sxs-lookup"><span data-stu-id="2ce48-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="2ce48-177">De forma predeterminada, el servicio de administración web escucha en el puerto TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="2ce48-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="2ce48-178">En firewall de Windows, cree una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172.</span><span class="sxs-lookup"><span data-stu-id="2ce48-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="2ce48-179">Para obtener más información, consulte [configuración de reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="2ce48-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="2ce48-180">(Si hospeda las máquinas virtuales en Azure, puede hacerlo directamente en el Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="2ce48-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="2ce48-181">Consulte [configuración de puntos de conexión en una máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)).</span><span class="sxs-lookup"><span data-stu-id="2ce48-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="2ce48-182">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="2ce48-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="2ce48-183">En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="2ce48-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="2ce48-184">Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="2ce48-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="2ce48-185">Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro.</span><span class="sxs-lookup"><span data-stu-id="2ce48-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="2ce48-186">(Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).</span><span class="sxs-lookup"><span data-stu-id="2ce48-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="2ce48-187">Si tiene curiosidad por ver los mensajes que se envían a Redis, puede usar el cliente **de Redis-CLI** , que se instala con Redis.</span><span class="sxs-lookup"><span data-stu-id="2ce48-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
