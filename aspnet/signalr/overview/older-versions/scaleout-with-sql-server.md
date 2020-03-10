---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Signalr ampliación con SQL Server (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431131"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="d4194-102">Escalabilidad horizontal de SignalR con SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d4194-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>

<span data-ttu-id="d4194-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d4194-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="d4194-104">En este tutorial, usará SQL Server para distribuir los mensajes a través de una aplicación Signalr implementada en dos instancias de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="d4194-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="d4194-105">También puede ejecutar este tutorial en un único equipo de pruebas, pero para obtener el efecto completo, debe implementar la aplicación Signalr en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="d4194-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="d4194-106">También debe instalar SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="d4194-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="d4194-107">Otra opción consiste en ejecutar el tutorial con máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="d4194-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="d4194-108">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d4194-108">Prerequisites</span></span>

<span data-ttu-id="d4194-109">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d4194-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="d4194-110">El backplane es compatible con las ediciones de escritorio y servidor de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d4194-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="d4194-111">No admite SQL Server Compact Edition o Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d4194-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="d4194-112">(Si la aplicación está hospedada en Azure, tenga en cuenta el Service Bus backplane en su lugar).</span><span class="sxs-lookup"><span data-stu-id="d4194-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="d4194-113">Información general</span><span class="sxs-lookup"><span data-stu-id="d4194-113">Overview</span></span>

<span data-ttu-id="d4194-114">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="d4194-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d4194-115">Cree una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="d4194-115">Create a new empty database.</span></span> <span data-ttu-id="d4194-116">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="d4194-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="d4194-117">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d4194-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d4194-118">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="d4194-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d4194-119">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="d4194-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="d4194-120">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="d4194-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="d4194-121">Agregue el código siguiente a global. asax para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="d4194-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="d4194-122">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="d4194-122">Configure the Database</span></span>

<span data-ttu-id="d4194-123">Decida si la aplicación usará la autenticación de Windows o la autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d4194-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="d4194-124">Sin importar, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="d4194-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="d4194-125">Cree una nueva base de datos para que la use el backplane.</span><span class="sxs-lookup"><span data-stu-id="d4194-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="d4194-126">Puede asignar cualquier nombre a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d4194-126">You can give the database any name.</span></span> <span data-ttu-id="d4194-127">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="d4194-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="d4194-128">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="d4194-128">Enable Service Broker</span></span>

<span data-ttu-id="d4194-129">Se recomienda habilitar Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="d4194-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="d4194-130">Service Broker proporciona compatibilidad nativa para mensajería y puesta en cola en SQL Server, lo que permite que el backplane reciba las actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="d4194-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="d4194-131">(Sin embargo, el backplane también funciona sin Service Broker).</span><span class="sxs-lookup"><span data-stu-id="d4194-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="d4194-132">Para comprobar si Service Broker está habilitado, consulte la columna **is\_broker\_Enabled** en la vista de catálogo **Sys. Databases** .</span><span class="sxs-lookup"><span data-stu-id="d4194-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="d4194-133">Para habilitar Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="d4194-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="d4194-134">Si esta consulta aparece en interbloqueo, asegúrese de que no haya ninguna aplicación conectada a la base de BD.</span><span class="sxs-lookup"><span data-stu-id="d4194-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="d4194-135">Si ha habilitado el seguimiento, los seguimientos también mostrarán si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="d4194-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="d4194-136">Creación de una aplicación Signalr</span><span class="sxs-lookup"><span data-stu-id="d4194-136">Create a SignalR Application</span></span>

<span data-ttu-id="d4194-137">Cree una aplicación de Signalr siguiendo uno de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="d4194-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d4194-138">Introducción con Signalr</span><span class="sxs-lookup"><span data-stu-id="d4194-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d4194-139">Introducción con Signalr y MVC 4</span><span class="sxs-lookup"><span data-stu-id="d4194-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d4194-140">A continuación, modificaremos la aplicación de chat para admitir ampliación con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d4194-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="d4194-141">En primer lugar, agregue el paquete NuGet Signalr. SqlServer al proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4194-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="d4194-142">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="d4194-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d4194-143">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="d4194-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="d4194-144">A continuación, abra el archivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="d4194-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="d4194-145">Agregue el código siguiente al método de **Inicio de\_** de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d4194-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d4194-146">Implementación y ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="d4194-146">Deploy and Run the Application</span></span>

<span data-ttu-id="d4194-147">Preparar las instancias de Windows Server para implementar la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="d4194-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d4194-148">Agregue el rol de IIS.</span><span class="sxs-lookup"><span data-stu-id="d4194-148">Add the IIS role.</span></span> <span data-ttu-id="d4194-149">Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d4194-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="d4194-150">Incluya también el servicio de administración de (que aparece en "herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="d4194-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="d4194-151">**Instale Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="d4194-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d4194-152">Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="d4194-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d4194-153">En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="d4194-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="d4194-154">Compruebe que el servicio de administración web se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="d4194-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d4194-155">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="d4194-155">If not, start the service.</span></span> <span data-ttu-id="d4194-156">(Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).</span><span class="sxs-lookup"><span data-stu-id="d4194-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d4194-157">Por último, abra el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="d4194-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="d4194-158">Este es el puerto que utiliza la herramienta de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="d4194-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="d4194-159">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d4194-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d4194-160">En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="d4194-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d4194-161">Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d4194-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d4194-162">Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro.</span><span class="sxs-lookup"><span data-stu-id="d4194-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d4194-163">(Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).</span><span class="sxs-lookup"><span data-stu-id="d4194-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="d4194-164">Después de ejecutar la aplicación, puede ver que Signalr ha creado tablas automáticamente en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d4194-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="d4194-165">Signalr administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="d4194-165">SignalR manages the tables.</span></span> <span data-ttu-id="d4194-166">Siempre que se implemente la aplicación, no elimine filas, modifique la tabla, etc.</span><span class="sxs-lookup"><span data-stu-id="d4194-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
