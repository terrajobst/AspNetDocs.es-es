---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Signalr ampliación con SQL Server | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467743"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="7b195-103">Escalabilidad horizontal de SignalR con SQL Server</span><span class="sxs-lookup"><span data-stu-id="7b195-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="7b195-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7b195-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="7b195-105">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="7b195-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="7b195-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7b195-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7b195-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7b195-107">.NET 4.5</span></span>
> - <span data-ttu-id="7b195-108">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="7b195-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="7b195-109">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="7b195-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="7b195-110">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="7b195-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="7b195-111">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="7b195-111">Questions and comments</span></span>
>
> <span data-ttu-id="7b195-112">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="7b195-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7b195-113">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7b195-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="7b195-114">En este tutorial, usará SQL Server para distribuir los mensajes a través de una aplicación Signalr implementada en dos instancias de IIS independientes.</span><span class="sxs-lookup"><span data-stu-id="7b195-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="7b195-115">También puede ejecutar este tutorial en un único equipo de pruebas, pero para obtener el efecto completo, debe implementar la aplicación Signalr en dos o más servidores.</span><span class="sxs-lookup"><span data-stu-id="7b195-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="7b195-116">También debe instalar SQL Server en uno de los servidores o en un servidor dedicado independiente.</span><span class="sxs-lookup"><span data-stu-id="7b195-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="7b195-117">Otra opción consiste en ejecutar el tutorial con máquinas virtuales en Azure.</span><span class="sxs-lookup"><span data-stu-id="7b195-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="7b195-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="7b195-118">Prerequisites</span></span>

<span data-ttu-id="7b195-119">Microsoft SQL Server 2005 o posterior.</span><span class="sxs-lookup"><span data-stu-id="7b195-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="7b195-120">El backplane es compatible con las ediciones de escritorio y servidor de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7b195-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="7b195-121">No admite SQL Server Compact Edition o Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7b195-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="7b195-122">(Si la aplicación está hospedada en Azure, tenga en cuenta el Service Bus backplane en su lugar).</span><span class="sxs-lookup"><span data-stu-id="7b195-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="7b195-123">Información general</span><span class="sxs-lookup"><span data-stu-id="7b195-123">Overview</span></span>

<span data-ttu-id="7b195-124">Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.</span><span class="sxs-lookup"><span data-stu-id="7b195-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="7b195-125">Cree una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="7b195-125">Create a new empty database.</span></span> <span data-ttu-id="7b195-126">El backplane creará las tablas necesarias en esta base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b195-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="7b195-127">Agregue estos paquetes de NuGet a la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7b195-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="7b195-128">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="7b195-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="7b195-129">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="7b195-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="7b195-130">Cree una aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="7b195-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="7b195-131">Agregue el código siguiente a Startup.cs para configurar el backplane:</span><span class="sxs-lookup"><span data-stu-id="7b195-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="7b195-132">Este código configura el Backplane con los valores predeterminados para [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) y [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="7b195-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="7b195-133">Para obtener información sobre cómo cambiar estos valores, vea [rendimiento de signalr: métricas de ampliación](signalr-performance.md#scaleout_metrics).</span><span class="sxs-lookup"><span data-stu-id="7b195-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="7b195-134">Configurar la base de datos</span><span class="sxs-lookup"><span data-stu-id="7b195-134">Configure the Database</span></span>

<span data-ttu-id="7b195-135">Decida si la aplicación usará la autenticación de Windows o la autenticación de SQL Server para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b195-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="7b195-136">Sin importar, asegúrese de que el usuario de base de datos tiene permisos para iniciar sesión, crear esquemas y crear tablas.</span><span class="sxs-lookup"><span data-stu-id="7b195-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="7b195-137">Cree una nueva base de datos para que la use el backplane.</span><span class="sxs-lookup"><span data-stu-id="7b195-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="7b195-138">Puede asignar cualquier nombre a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7b195-138">You can give the database any name.</span></span> <span data-ttu-id="7b195-139">No es necesario crear ninguna tabla en la base de datos; el backplane creará las tablas necesarias.</span><span class="sxs-lookup"><span data-stu-id="7b195-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="7b195-140">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="7b195-140">Enable Service Broker</span></span>

<span data-ttu-id="7b195-141">Se recomienda habilitar Service Broker para la base de datos de backplane.</span><span class="sxs-lookup"><span data-stu-id="7b195-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="7b195-142">Service Broker proporciona compatibilidad nativa para mensajería y puesta en cola en SQL Server, lo que permite que el backplane reciba las actualizaciones de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="7b195-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="7b195-143">(Sin embargo, el backplane también funciona sin Service Broker).</span><span class="sxs-lookup"><span data-stu-id="7b195-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="7b195-144">Para comprobar si Service Broker está habilitado, consulte la columna **is\_broker\_Enabled** en la vista de catálogo **Sys. Databases** .</span><span class="sxs-lookup"><span data-stu-id="7b195-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="7b195-145">Para habilitar Service Broker, use la siguiente consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="7b195-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="7b195-146">Si esta consulta aparece en interbloqueo, asegúrese de que no haya ninguna aplicación conectada a la base de BD.</span><span class="sxs-lookup"><span data-stu-id="7b195-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="7b195-147">Si ha habilitado el seguimiento, los seguimientos también mostrarán si Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="7b195-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="7b195-148">Creación de una aplicación Signalr</span><span class="sxs-lookup"><span data-stu-id="7b195-148">Create a SignalR Application</span></span>

<span data-ttu-id="7b195-149">Cree una aplicación de Signalr siguiendo uno de estos tutoriales:</span><span class="sxs-lookup"><span data-stu-id="7b195-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="7b195-150">Introducción con Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="7b195-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="7b195-151">Introducción con Signalr 2,0 y MVC 5</span><span class="sxs-lookup"><span data-stu-id="7b195-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="7b195-152">A continuación, modificaremos la aplicación de chat para admitir ampliación con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7b195-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="7b195-153">En primer lugar, agregue el paquete NuGet Signalr. SqlServer al proyecto.</span><span class="sxs-lookup"><span data-stu-id="7b195-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="7b195-154">En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="7b195-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7b195-155">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="7b195-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="7b195-156">A continuación, abra el archivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="7b195-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="7b195-157">Agregue el código siguiente al método **Configure** :</span><span class="sxs-lookup"><span data-stu-id="7b195-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="7b195-158">Implementación y ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="7b195-158">Deploy and Run the Application</span></span>

<span data-ttu-id="7b195-159">Preparar las instancias de Windows Server para implementar la aplicación Signalr.</span><span class="sxs-lookup"><span data-stu-id="7b195-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="7b195-160">Agregue el rol de IIS.</span><span class="sxs-lookup"><span data-stu-id="7b195-160">Add the IIS role.</span></span> <span data-ttu-id="7b195-161">Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="7b195-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="7b195-162">Incluya también el servicio de administración de (que aparece en "herramientas de administración").</span><span class="sxs-lookup"><span data-stu-id="7b195-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="7b195-163">**Instale Web Deploy 3,0.**</span><span class="sxs-lookup"><span data-stu-id="7b195-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="7b195-164">Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="7b195-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="7b195-165">En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0</span><span class="sxs-lookup"><span data-stu-id="7b195-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="7b195-166">Compruebe que el servicio de administración web se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="7b195-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="7b195-167">Si no es así, inicie el servicio.</span><span class="sxs-lookup"><span data-stu-id="7b195-167">If not, start the service.</span></span> <span data-ttu-id="7b195-168">(Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).</span><span class="sxs-lookup"><span data-stu-id="7b195-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="7b195-169">Por último, abra el puerto 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="7b195-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="7b195-170">Este es el puerto que utiliza la herramienta de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="7b195-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="7b195-171">Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7b195-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="7b195-172">En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="7b195-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="7b195-173">Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="7b195-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="7b195-174">Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro.</span><span class="sxs-lookup"><span data-stu-id="7b195-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="7b195-175">(Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).</span><span class="sxs-lookup"><span data-stu-id="7b195-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="7b195-176">Después de ejecutar la aplicación, puede ver que Signalr ha creado tablas automáticamente en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="7b195-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="7b195-177">Signalr administra las tablas.</span><span class="sxs-lookup"><span data-stu-id="7b195-177">SignalR manages the tables.</span></span> <span data-ttu-id="7b195-178">Siempre que se implemente la aplicación, no elimine filas, modifique la tabla, etc.</span><span class="sxs-lookup"><span data-stu-id="7b195-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
