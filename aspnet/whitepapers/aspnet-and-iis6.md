---
uid: whitepapers/aspnet-and-iis6
title: Ejecución de ASP.NET 1,1 con IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Aunque Windows Server 2003 incluye tanto IIS 6,0 como ASP.NET 1,1, estos componentes están deshabilitados de forma predeterminada. En estas notas del producto se describe cómo habilitar IIS 6,0...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419851"
---
# <a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="ec281-104">Ejecutar ASP.NET 1.1 con IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="ec281-104">Running ASP.NET 1.1 with IIS 6.0</span></span>

> <span data-ttu-id="ec281-105">Aunque Windows Server 2003 incluye tanto IIS 6,0 como ASP.NET 1,1, estos componentes están deshabilitados de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ec281-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="ec281-106">En estas notas del producto se describe cómo habilitar IIS 6,0 y ASP.NET 1,1, y se recomiendan varias opciones de configuración para obtener el rendimiento óptimo de IIS y ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ec281-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="ec281-107">Se aplica a ASP.NET 1,1 e IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="ec281-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>

<span data-ttu-id="ec281-108">ASP.NET 1,1 se incluye con Windows Server 2003, que también incluye la versión más reciente de Internet Information Server (IIS) versión 6,0.</span><span class="sxs-lookup"><span data-stu-id="ec281-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="ec281-109">IIS 6,0 y ASP.NET 1,1 están diseñados para integrarse sin problemas y ASP.NET ahora tiene como valor predeterminado el nuevo modelo de proceso de trabajo de IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="ec281-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="ec281-110">ASP.NET 1,1 no está instalado de forma predeterminada</span><span class="sxs-lookup"><span data-stu-id="ec281-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="ec281-111">A diferencia de las versiones anteriores de los sistemas operativos de servidor de Microsoft, Internet Information Server (IIS) no está habilitado de forma predeterminada. ni es ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="ec281-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="ec281-112">Hay dos opciones para habilitar IIS:</span><span class="sxs-lookup"><span data-stu-id="ec281-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="ec281-113">Habilitar IIS, opción #1: Asistente para configurar el servidor</span><span class="sxs-lookup"><span data-stu-id="ec281-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="ec281-114">Windows Server 2003 envía un nuevo "Asistente para configurar el servidor" para ayudarle a configurar correctamente el servidor en el modo deseado.</span><span class="sxs-lookup"><span data-stu-id="ec281-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="ec281-115">Para iniciar el asistente: tenga en cuenta que, para ejecutar el asistente, debe haber iniciado sesión como administrador. vaya a: Inicio | Programas | Herramientas administrativas y seleccione "configurar el servidor".</span><span class="sxs-lookup"><span data-stu-id="ec281-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="ec281-116">Una vez seleccionado, debería ver la pantalla de apertura del Asistente para configurar el servidor:</span><span class="sxs-lookup"><span data-stu-id="ec281-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="ec281-117">Haga clic en "siguiente &gt;":</span><span class="sxs-lookup"><span data-stu-id="ec281-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="ec281-118">Haga clic en ' siguiente &gt;'</span><span class="sxs-lookup"><span data-stu-id="ec281-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="ec281-119">En esta pantalla deberá seleccionar ' servidor de aplicaciones (IIS, ASP.NET) ' como opciones para configurar.</span><span class="sxs-lookup"><span data-stu-id="ec281-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="ec281-120">Haga clic en "siguiente &gt;".</span><span class="sxs-lookup"><span data-stu-id="ec281-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="ec281-121">Después de seleccionar la opción para configurar el servidor como un servidor de aplicaciones, se mostrará esta pantalla en la que se le preguntará qué capacidades adicionales deben instalarse.</span><span class="sxs-lookup"><span data-stu-id="ec281-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="ec281-122">Ninguna opción está seleccionada de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ec281-122">Neither option is selected by default.</span></span> <span data-ttu-id="ec281-123">Para habilitar ASP.NET automáticamente, debe seleccionar "habilitar ASP". NET '.</span><span class="sxs-lookup"><span data-stu-id="ec281-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="ec281-124">Haga clic en "siguiente &gt;".</span><span class="sxs-lookup"><span data-stu-id="ec281-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="ec281-125">Esta pantalla muestra las opciones que se van a instalar.</span><span class="sxs-lookup"><span data-stu-id="ec281-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="ec281-126">Haga clic en "siguiente &gt;".</span><span class="sxs-lookup"><span data-stu-id="ec281-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="ec281-127">Verá esta pantalla mientras se instalan las opciones seleccionadas.</span><span class="sxs-lookup"><span data-stu-id="ec281-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="ec281-128">Es normal ver otros cuadros de diálogo que aparecen cuando se instalan los servicios.</span><span class="sxs-lookup"><span data-stu-id="ec281-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="ec281-129">Además, es posible que se le solicite la ubicación del CD de instalación de Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="ec281-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="ec281-130">Cuando termine, haga clic en "siguiente &gt;".</span><span class="sxs-lookup"><span data-stu-id="ec281-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="ec281-131">Haga clic en "finalizar": ahora, Windows Server 2003 está configurado para admitir IIS 6,0 y ASP.NET 1,1.</span><span class="sxs-lookup"><span data-stu-id="ec281-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="ec281-132">Habilitar IIS, opción #2: configurar manualmente IIS y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ec281-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="ec281-133">Si no desea usar el Asistente para configurar el servidor, puede instalar IIS 6,0 y ASP.NET 1,1 mediante la opción ' Agregar o quitar programas ' del panel de control.</span><span class="sxs-lookup"><span data-stu-id="ec281-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="ec281-134">En primer lugar, abra el panel de control:</span><span class="sxs-lookup"><span data-stu-id="ec281-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="ec281-135">A continuación, haga clic en "Agregar o quitar componentes de Windows", que abrirá el "Asistente para componentes de Windows":</span><span class="sxs-lookup"><span data-stu-id="ec281-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="ec281-136">Resalte y seleccione "servidor de aplicaciones" y, a continuación, haga clic en "detalles".</span><span class="sxs-lookup"><span data-stu-id="ec281-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="ec281-137">botón</span><span class="sxs-lookup"><span data-stu-id="ec281-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="ec281-138">Para instalar ASP.NET, Active "ASP. NET '.</span><span class="sxs-lookup"><span data-stu-id="ec281-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="ec281-139">Haga clic en "Aceptar" para volver al Asistente para componentes de Windows.</span><span class="sxs-lookup"><span data-stu-id="ec281-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="ec281-140">Haga clic en "siguiente &gt;" en el Asistente para componentes de Windows para empezar a instalar:</span><span class="sxs-lookup"><span data-stu-id="ec281-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="ec281-141">Es normal ver otros cuadros de diálogo que aparecen cuando se instalan los servicios.</span><span class="sxs-lookup"><span data-stu-id="ec281-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="ec281-142">Además, es posible que se le solicite la ubicación del CD de instalación de Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="ec281-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="ec281-143">Una vez completada la instalación, verá la última pantalla del Asistente para componentes de Windows:</span><span class="sxs-lookup"><span data-stu-id="ec281-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="ec281-144">IIS 6,0 y ASP.NET 1,1 ahora están configurados y disponibles.</span><span class="sxs-lookup"><span data-stu-id="ec281-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="ec281-145">Configuración recomendada</span><span class="sxs-lookup"><span data-stu-id="ec281-145">Recommended Settings</span></span>

<span data-ttu-id="ec281-146">Al ejecutar ASP.NET 1,1 con IIS 6,0 hay varias opciones de configuración que se recomiendan para obtener el rendimiento óptimo de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="ec281-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="ec281-147">Configuración de límites de memoria de procesos de trabajo</span><span class="sxs-lookup"><span data-stu-id="ec281-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="ec281-148">Configurar el reciclaje de procesos de trabajo</span><span class="sxs-lookup"><span data-stu-id="ec281-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="ec281-149">Configuración de límites de memoria de procesos de trabajo</span><span class="sxs-lookup"><span data-stu-id="ec281-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="ec281-150">De forma predeterminada, IIS 6,0 no establece un límite en la cantidad de memoria que puede usar IIS.</span><span class="sxs-lookup"><span data-stu-id="ec281-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="ec281-151">ASP. La característica de caché de la red se basa en una limitación de memoria, por lo que la memoria caché puede quitar de forma proactiva los elementos no utilizados de la memoria.</span><span class="sxs-lookup"><span data-stu-id="ec281-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="ec281-152">Se recomienda que configure la característica reciclaje de memoria de IIS 6,0.</span><span class="sxs-lookup"><span data-stu-id="ec281-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="ec281-153">Para configurar este administrador de Internet Information Services abierto (Inicio | Programas | Herramientas administrativas | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="ec281-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="ec281-154">Una vez abierta, expanda la carpeta ' grupos de aplicaciones ':</span><span class="sxs-lookup"><span data-stu-id="ec281-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="ec281-155">Para cada grupo de aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="ec281-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="ec281-156">Haga clic con el botón derecho en el grupo de aplicaciones, por ejemplo, "DefaultAppPool" y seleccione "propiedades":</span><span class="sxs-lookup"><span data-stu-id="ec281-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="ec281-157">A continuación, habilite el reciclaje de memoria; para ello, haga clic en ' máximo de memoria usada (en megabytes): '.</span><span class="sxs-lookup"><span data-stu-id="ec281-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="ec281-158">El valor no debe ser mayor que la cantidad de memoria física (no virtual) del servidor, una buena aproximación es el 60% de la memoria física, es decir, para un servidor con 512 MB de memoria física, seleccione 310.</span><span class="sxs-lookup"><span data-stu-id="ec281-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="ec281-159">También se recomienda que el máximo no supere el valor de 800MB al usar un espacio de direcciones de 2 GB.</span><span class="sxs-lookup"><span data-stu-id="ec281-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="ec281-160">Si el espacio de direcciones de memoria del servidor es de 3 GB, el límite máximo de memoria para el proceso de trabajo puede ser tan alto como 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="ec281-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="ec281-161">Haga clic en "aplicar" y en "Aceptar" para salir del cuadro de diálogo Propiedades.</span><span class="sxs-lookup"><span data-stu-id="ec281-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="ec281-162">Repita este paso para todos los grupos de aplicaciones disponibles.</span><span class="sxs-lookup"><span data-stu-id="ec281-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="ec281-163">Configuración del reciclaje de trabajo</span><span class="sxs-lookup"><span data-stu-id="ec281-163">Configuring worker recycling</span></span>

<span data-ttu-id="ec281-164">De forma predeterminada, IIS 6,0 se configura para reciclar el proceso de trabajo cada 29 horas.</span><span class="sxs-lookup"><span data-stu-id="ec281-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="ec281-165">Esto es un poco agresivo para una aplicación que ejecuta ASP.NET y se recomienda que el reciclaje automático del proceso de trabajo esté deshabilitado.</span><span class="sxs-lookup"><span data-stu-id="ec281-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="ec281-166">Para deshabilitar el reciclaje automático de procesos de trabajo, abra primero Internet Information Services Manager (Start | Programas | Herramientas administrativas | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="ec281-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="ec281-167">Una vez abierta, expanda la carpeta ' grupos de aplicaciones ':</span><span class="sxs-lookup"><span data-stu-id="ec281-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="ec281-168">Para cada grupo de aplicaciones:</span><span class="sxs-lookup"><span data-stu-id="ec281-168">For each application pool:</span></span>

1. <span data-ttu-id="ec281-169">Haga clic con el botón derecho en el grupo de aplicaciones, por ejemplo, "DefaultAppPool" y seleccione "propiedades":</span><span class="sxs-lookup"><span data-stu-id="ec281-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="ec281-170">Desactive la casilla ' reciclar el proceso de trabajo (en minutos): ':</span><span class="sxs-lookup"><span data-stu-id="ec281-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="ec281-171">Haga clic en "aplicar" y en "Aceptar" para salir del cuadro de diálogo Propiedades.</span><span class="sxs-lookup"><span data-stu-id="ec281-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="ec281-172">Repita este paso para todos los grupos de aplicaciones disponibles.</span><span class="sxs-lookup"><span data-stu-id="ec281-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="ec281-173">Conceder acceso de escritura al sistema de archivos</span><span class="sxs-lookup"><span data-stu-id="ec281-173">Granting write access to the file system</span></span>

<span data-ttu-id="ec281-174">Si la aplicación necesita acceso de escritura al sistema de archivos y usa NTFS, tendrá que modificar una lista de Access Control (ACL) en la carpeta o el archivo para conceder acceso a ASP.NET a.</span><span class="sxs-lookup"><span data-stu-id="ec281-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="ec281-175">Por ejemplo, para conceder acceso de escritura de ASP.NET a c:\inetpub\wwwroot, abra primero el explorador y navegue hasta el directorio:</span><span class="sxs-lookup"><span data-stu-id="ec281-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="ec281-176">A continuación, haga clic con el botón derecho en el directorio, por ejemplo, ' wwwroot ' y seleccione Propiedades.</span><span class="sxs-lookup"><span data-stu-id="ec281-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="ec281-177">Después de abrir el cuadro de diálogo Propiedades, seleccione la pestaña ' seguridad ':</span><span class="sxs-lookup"><span data-stu-id="ec281-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="ec281-178">El directorio c:\inetpub\wwwroot\ es un directorio especial en el que el grupo especial IIS 6,0 ' IIS\_WPG ' ya tiene concedido el permiso leer &amp; ejecutar, mostrar el contenido de la carpeta y leer.</span><span class="sxs-lookup"><span data-stu-id="ec281-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="ec281-179">Sin embargo, para conceder permiso de escritura, debe hacer clic en la casilla permitir para escritura:</span><span class="sxs-lookup"><span data-stu-id="ec281-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="ec281-180">IIS 6,0 ahora tiene permiso de escritura en esta carpeta.</span><span class="sxs-lookup"><span data-stu-id="ec281-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="ec281-181">Para conceder permisos de escritura en otras carpetas, siga estos pasos: tenga en cuenta que es posible que tenga que agregar el grupo de IIS\_WPG si aún no existe.</span><span class="sxs-lookup"><span data-stu-id="ec281-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="ec281-182">Conceder permiso de escritura a IIS\_WPG permitirá que cualquier aplicación ASP.NET escriba en este directorio.</span><span class="sxs-lookup"><span data-stu-id="ec281-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="ec281-183">Compatibilidad con la autenticación integrada con SQL Server</span><span class="sxs-lookup"><span data-stu-id="ec281-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="ec281-184">La autenticación integrada permite a SQL Server aprovechar la autenticación de Windows NT para validar SQL Server cuentas de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="ec281-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="ec281-185">Esto permite al usuario omitir el proceso de inicio de sesión de SQL Server estándar.</span><span class="sxs-lookup"><span data-stu-id="ec281-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="ec281-186">Con este enfoque, un usuario de red puede tener acceso a una base de datos de SQL Server sin proporcionar una identificación de inicio de sesión independiente o una contraseña porque SQL Server obtiene la información de usuario y contraseña del proceso de seguridad de red de Windows NT.</span><span class="sxs-lookup"><span data-stu-id="ec281-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="ec281-187">La elección de la autenticación integrada para aplicaciones ASP.NET es una buena opción, ya que ninguna de las credenciales se almacenan en la cadena de conexión de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ec281-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="ec281-188">En su lugar, la cadena de conexión utilizada para conectarse a SQL tendrá el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="ec281-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="ec281-189">Esta cadena de conexión indica a SQL Server que use las credenciales de Windows de la aplicación que intenta tener acceso a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec281-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="ec281-190">En el caso de ASP.NET/IIS 6, sería una cuenta en el grupo de IIS\_WPG.</span><span class="sxs-lookup"><span data-stu-id="ec281-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="ec281-191">Para habilitar la autenticación integrada entre SQL Server y ASP.NET, primero deberá asegurarse de que SQL Server está configurado para la autenticación integrada o la autenticación de modo mixto. Consulte con su DBA para determinar esto.</span><span class="sxs-lookup"><span data-stu-id="ec281-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="ec281-192">Si SQL Server está en uno de estos dos modos, puede utilizar la autenticación integrada.</span><span class="sxs-lookup"><span data-stu-id="ec281-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="ec281-193">Abra el administrador de SQL Server Enterprise (Inicio | Programas | Microsoft SQL Server | Administrador corporativo), seleccione el servidor adecuado y expanda la carpeta seguridad:</span><span class="sxs-lookup"><span data-stu-id="ec281-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="ec281-194">Si no aparece el grupo ' BUILTINT\IIS\_WPG ', haga clic con el botón derecho en inicios de sesión y seleccione ' nuevo login ':</span><span class="sxs-lookup"><span data-stu-id="ec281-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="ec281-195">En el cuadro de texto "nombre:", escriba "[nombre del servidor/dominio] \IIS\_WPG" o haga clic en el botón de puntos suspensivos para abrir el selector de usuarios o grupos de Windows NT:</span><span class="sxs-lookup"><span data-stu-id="ec281-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="ec281-196">Seleccione el grupo de IIS\_WPG del equipo actual y haga clic en "agregar" y en Aceptar para cerrar el selector.</span><span class="sxs-lookup"><span data-stu-id="ec281-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="ec281-197">A continuación, debe establecer también la base de datos predeterminada y los permisos para tener acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ec281-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="ec281-198">Para establecer la base de datos predeterminada, elija en la lista desplegable; por ejemplo, se selecciona Northwind:</span><span class="sxs-lookup"><span data-stu-id="ec281-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="ec281-199">A continuación, haga clic en la pestaña acceso a base de datos:</span><span class="sxs-lookup"><span data-stu-id="ec281-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="ec281-200">Haga clic en la casilla permitir para cada base de datos a la que desea permitir el acceso.</span><span class="sxs-lookup"><span data-stu-id="ec281-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="ec281-201">También tendrá que seleccionar roles de base de datos, comprobando que el propietario de la base de datos\_debe asegurarse de que el inicio de sesión tenga todos los permisos necesarios para administrar y usar la base de datos seleccionada.</span><span class="sxs-lookup"><span data-stu-id="ec281-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="ec281-202">Haga clic en Aceptar para salir del cuadro de diálogo de propiedades.</span><span class="sxs-lookup"><span data-stu-id="ec281-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="ec281-203">La aplicación ASP.NET ya está configurada para admitir la autenticación integrada de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec281-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="ec281-204">No ejecute ASP.NET 1,0 en el modo nativo de IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="ec281-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="ec281-205">ASP.NET 1,0 en IIS 6,0 solo se admite en el modo de compatibilidad de IIS 5.</span><span class="sxs-lookup"><span data-stu-id="ec281-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="ec281-206">Para configurar ASP.NET 1,0 para que se ejecute en el modo de compatibilidad de IIS 5,0, abra Administrador de servicios Internet, haga clic con el botón derecho en sitios web y seleccione Propiedades:</span><span class="sxs-lookup"><span data-stu-id="ec281-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="ec281-207">Cambie a la pestaña servicio y seleccione? ¿Ejecutar el servicio WWW en el modo de aislamiento de IIS 5,0?:</span><span class="sxs-lookup"><span data-stu-id="ec281-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
