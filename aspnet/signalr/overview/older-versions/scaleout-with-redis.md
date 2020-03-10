---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Signalr ampliación con REDIS (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431209"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="a1d72-102">Escalabilidad horizontal de SignalR con Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="a1d72-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="a1d72-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="a1d72-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="a1d72-104">En este tutorial, usará [Redis](http://redis.io/) para distribuir los mensajes a través de una aplicación signalr implementada en dos instancias de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="a1d72-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="a1d72-105">Redis es un almacén de clave-valor en memoria.</span><span class="sxs-lookup"><span data-stu-id="a1d72-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="a1d72-106">También admite un sistema de mensajería con un modelo de publicación/suscripción.</span><span class="sxs-lookup"><span data-stu-id="a1d72-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="a1d72-107">El backplane de ReDiS ReDiS usa la característica pub/sub para reenviar mensajes a otros servidores.</span><span class="sxs-lookup"><span data-stu-id="a1d72-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="a1d72-108">En este tutorial, usará tres servidores:</span><span class="sxs-lookup"><span data-stu-id="a1d72-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="a1d72-109">Dos servidores que ejecutan Windows, que se usarán para implementar una aplicación de Signalr.</span><span class="sxs-lookup"><span data-stu-id="a1d72-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="a1d72-110">Un servidor que ejecuta Linux, que usará para ejecutar Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="a1d72-111">En el caso de las capturas de pantallas de este tutorial, usé Ubuntu 12,04 TLS.</span><span class="sxs-lookup"><span data-stu-id="a1d72-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="a1d72-112">Si no tiene tres servidores físicos para usar, puede crear máquinas virtuales en Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="a1d72-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="a1d72-113">Otra opción es crear máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="a1d72-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="a1d72-114">Aunque en este tutorial se usa la implementación de Redis oficial, también hay un [Puerto de Windows de Redis](https://github.com/MSOpenTech/redis) desde MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="a1d72-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="a1d72-115">La instalación y configuración son diferentes, pero, de lo contrario, los pasos son los mismos.</span><span class="sxs-lookup"><span data-stu-id="a1d72-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a1d72-116">Signalr ampliación con Redis no admite clústeres de Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="a1d72-117">Información general</span><span class="sxs-lookup"><span data-stu-id="a1d72-117">Overview</span></span>

<span data-ttu-id="a1d72-118">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="a1d72-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="a1d72-119">Instale Redis e inicie el servidor de Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="a1d72-120">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a1d72-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="a1d72-121">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="a1d72-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="a1d72-122">Microsoft. AspNet. Signalr. Redis</span><span class="sxs-lookup"><span data-stu-id="a1d72-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="a1d72-123">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="a1d72-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="a1d72-124">Agregue el código siguiente a global. asax para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="a1d72-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="a1d72-125">Ubuntu en Hyper-V</span><span class="sxs-lookup"><span data-stu-id="a1d72-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="a1d72-126">Con Windows Hyper-V, puede crear fácilmente una máquina virtual de Ubuntu en Windows Server.</span><span class="sxs-lookup"><span data-stu-id="a1d72-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="a1d72-127">Descargue el ISO de Ubuntu desde [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="a1d72-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="a1d72-128">En Hyper-V, agregue una nueva máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="a1d72-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="a1d72-129">En el paso **Conectar disco duro virtual** , seleccione **crear un disco duro virtual**.</span><span class="sxs-lookup"><span data-stu-id="a1d72-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="a1d72-130">En el paso **Opciones de instalación** , seleccione Archivo de **imagen (. ISO)** , haga clic en **examinar**y busque el ISO de instalación de Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="a1d72-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="a1d72-131">Instalación de Redis</span><span class="sxs-lookup"><span data-stu-id="a1d72-131">Install Redis</span></span>

<span data-ttu-id="a1d72-132">Siga los pasos descritos en [http://redis.io/download](http://redis.io/download) para descargar y compilar Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="a1d72-133">Esto crea los archivos binarios de Redis en el directorio de `src`.</span><span class="sxs-lookup"><span data-stu-id="a1d72-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="a1d72-134">De forma predeterminada, Redis no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="a1d72-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="a1d72-135">Para establecer una contraseña, edite el archivo de `redis.conf`, que se encuentra en el directorio raíz del código fuente.</span><span class="sxs-lookup"><span data-stu-id="a1d72-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="a1d72-136">(Hacer una copia de seguridad del archivo antes de editarlo) Agregue la siguiente directiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="a1d72-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="a1d72-137">Ahora inicie el servidor de Redis:</span><span class="sxs-lookup"><span data-stu-id="a1d72-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="a1d72-138">Abra el puerto 6379, que es el puerto predeterminado en el que escucha Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="a1d72-139">(Puede cambiar el número de puerto en el archivo de configuración).</span><span class="sxs-lookup"><span data-stu-id="a1d72-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="a1d72-140">Creación de la aplicación Signalr</span><span class="sxs-lookup"><span data-stu-id="a1d72-140">Create the SignalR Application</span></span>

<span data-ttu-id="a1d72-141">Cree una aplicación de Signalr siguiendo uno de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="a1d72-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="a1d72-142">Introducción con Signalr</span><span class="sxs-lookup"><span data-stu-id="a1d72-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="a1d72-143">Introducción con Signalr y MVC 4</span><span class="sxs-lookup"><span data-stu-id="a1d72-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="a1d72-144">A continuación, modificaremos la aplicación de chat para admitir ampliación con Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="a1d72-145">En primer lugar, agregue el paquete NuGet Signalr. Redis al proyecto.</span><span class="sxs-lookup"><span data-stu-id="a1d72-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="a1d72-146">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="a1d72-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a1d72-147">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="a1d72-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="a1d72-148">A continuación, abra el archivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="a1d72-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="a1d72-149">Agregue el código siguiente al método de **Inicio de\_** de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a1d72-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="a1d72-150">"servidor" es el nombre del servidor que ejecuta Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="a1d72-151">*Port* es el número de Puerto</span><span class="sxs-lookup"><span data-stu-id="a1d72-151">*port* is the port number</span></span>
- <span data-ttu-id="a1d72-152">"contraseña" es la contraseña que definió en el archivo Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="a1d72-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="a1d72-153">"AppName" es cualquier cadena.</span><span class="sxs-lookup"><span data-stu-id="a1d72-153">"AppName" is any string.</span></span> <span data-ttu-id="a1d72-154">Signalr crea un canal pub/sub de Redis con este nombre.</span><span class="sxs-lookup"><span data-stu-id="a1d72-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="a1d72-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a1d72-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="a1d72-156">Implementación y ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="a1d72-156">Deploy and Run the Application</span></span>

<span data-ttu-id="a1d72-157">Preparar las instancias de Windows Server para implementar la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="a1d72-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="a1d72-158">Agregue el rol de IIS.</span><span class="sxs-lookup"><span data-stu-id="a1d72-158">Add the IIS role.</span></span> <span data-ttu-id="a1d72-159">Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a1d72-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="a1d72-160">Incluya también el servicio de administración de (que aparece en "herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="a1d72-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="a1d72-161">**Instale Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="a1d72-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="a1d72-162">Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="a1d72-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="a1d72-163">En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="a1d72-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="a1d72-164">Compruebe que el servicio de administración web se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="a1d72-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="a1d72-165">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="a1d72-165">If not, start the service.</span></span> <span data-ttu-id="a1d72-166">(Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).</span><span class="sxs-lookup"><span data-stu-id="a1d72-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="a1d72-167">De forma predeterminada, el servicio de administración web escucha en el puerto TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="a1d72-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="a1d72-168">En firewall de Windows, cree una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172.</span><span class="sxs-lookup"><span data-stu-id="a1d72-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="a1d72-169">Para obtener más información, consulte [configuración de reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a1d72-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="a1d72-170">(Si hospeda las máquinas virtuales en Azure, puede hacerlo directamente en el Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="a1d72-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="a1d72-171">Consulte [configuración de puntos de conexión en una máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)).</span><span class="sxs-lookup"><span data-stu-id="a1d72-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="a1d72-172">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a1d72-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="a1d72-173">En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="a1d72-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="a1d72-174">Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="a1d72-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="a1d72-175">Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro.</span><span class="sxs-lookup"><span data-stu-id="a1d72-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="a1d72-176">(Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).</span><span class="sxs-lookup"><span data-stu-id="a1d72-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="a1d72-177">Si tiene curiosidad por ver los mensajes que se envían a Redis, puede usar el cliente **de Redis-CLI** , que se instala con Redis.</span><span class="sxs-lookup"><span data-stu-id="a1d72-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
