---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Uso de contadores de rendimiento de Signalr en un rol Web de Azure | Microsoft Docs
author: guardrex
description: Cómo instalar y usar los contadores de rendimiento de Signalr en un rol Web de Azure.
keywords: ASP. NET, signalr, contador de rendimiento, rol Web de Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467569"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="75b6f-104">Uso de contadores de rendimiento de Signalr en un rol Web de Azure</span><span class="sxs-lookup"><span data-stu-id="75b6f-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="75b6f-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="75b6f-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="75b6f-106">Los contadores de rendimiento de signalr se usan para supervisar el rendimiento de la aplicación en un rol Web de Azure.</span><span class="sxs-lookup"><span data-stu-id="75b6f-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="75b6f-107">Microsoft Azure Diagnostics captura los contadores.</span><span class="sxs-lookup"><span data-stu-id="75b6f-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="75b6f-108">Los contadores de rendimiento de Signalr se instalan en Azure con *signalr. exe*, la misma herramienta que se usa para aplicaciones independientes o locales.</span><span class="sxs-lookup"><span data-stu-id="75b6f-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="75b6f-109">Dado que los roles de Azure son transitorios, configure una aplicación para instalar y registrar los contadores de rendimiento de Signalr en el inicio.</span><span class="sxs-lookup"><span data-stu-id="75b6f-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75b6f-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="75b6f-110">Prerequisites</span></span>

* <span data-ttu-id="75b6f-111">Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="75b6f-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="75b6f-112">[Microsoft Azure SDK para Visual Studio](https://azure.microsoft.com/downloads/) **Nota: reinicie el equipo después de instalar el SDK.**</span><span class="sxs-lookup"><span data-stu-id="75b6f-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="75b6f-113">Microsoft Azure suscripción: para registrarse para obtener una cuenta de evaluación gratuita de Azure, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="75b6f-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="75b6f-114">Creación de una aplicación de rol Web de Azure que exponga los contadores de rendimiento de Signalr</span><span class="sxs-lookup"><span data-stu-id="75b6f-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="75b6f-115">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75b6f-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="75b6f-116">En Visual Studio, seleccione **Archivo** > **Nuevo** > **Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="75b6f-117">En el cuadro de diálogo **nuevo proyecto** , seleccione la categoría **nube** de  **C# visual** > a la izquierda y, a continuación, seleccione la plantilla servicio en la **nube de Azure** .</span><span class="sxs-lookup"><span data-stu-id="75b6f-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="75b6f-118">Asigne a la aplicación el nombre **SignalRPerfCounters** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nueva aplicación en la nube](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="75b6f-120">Si no ve la categoría de plantilla de **nube** o la plantilla de servicio en la **nube de Azure** , debe instalar la carga de trabajo de **desarrollo de Azure** para Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="75b6f-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="75b6f-121">Elija el vínculo **abrir instalador de Visual Studio** en la parte inferior izquierda del cuadro de diálogo **nuevo proyecto** para abrir instalador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75b6f-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="75b6f-122">Seleccione la carga de trabajo **desarrollo de Azure** y, después, elija **modificar** para empezar a instalar la carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="75b6f-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Carga de trabajo de desarrollo de Azure en Instalador de Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="75b6f-124">En el cuadro de diálogo **nuevo Microsoft Azure servicio en la nube** , seleccione **rol Web ASP.net** y seleccione el botón > para agregar el rol al proyecto.</span><span class="sxs-lookup"><span data-stu-id="75b6f-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="75b6f-125">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-125">Select **OK**.</span></span>

   ![Agregar rol Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="75b6f-127">En el cuadro de diálogo **nueva aplicación Web de ASP.net-WebRole1** , seleccione la plantilla **MVC** y, después, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Agregar MVC y API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="75b6f-129">En **Explorador de soluciones**, abra el archivo *Diagnostics. wadcfgx* en **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Explorador de soluciones Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="75b6f-131">Reemplace el contenido del archivo con la configuración siguiente y guarde el archivo:</span><span class="sxs-lookup"><span data-stu-id="75b6f-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="75b6f-132">Abra la **consola del administrador de paquetes** desde **herramientas** > **Administrador de paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="75b6f-133">Escriba los siguientes comandos para instalar la versión más reciente de Signalr y el paquete de utilidades de Signalr:</span><span class="sxs-lookup"><span data-stu-id="75b6f-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="75b6f-134">Configure la aplicación para instalar los contadores de rendimiento de Signalr en la instancia de rol cuando se inicie o se recicle.</span><span class="sxs-lookup"><span data-stu-id="75b6f-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="75b6f-135">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **WebRole1** y seleccione **Agregar** > **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="75b6f-136">Asigne a la nueva carpeta el nombre *Startup*.</span><span class="sxs-lookup"><span data-stu-id="75b6f-136">Name the new folder *Startup*.</span></span>

   ![Agregar carpeta de inicio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="75b6f-138">Copie el archivo *signalr. exe* (agregado con el paquete **Microsoft. Aspnet. Signalr. utils** ) desde \<carpeta del proyecto >/SignalRPerfCounters/Packages/Microsoft.Aspnet.SignalR.utils.\<versión >/Tools a la carpeta de *Inicio* que creó en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="75b6f-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="75b6f-139">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta de *Inicio* y seleccione **Agregar** > **elemento existente**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="75b6f-140">En el cuadro de diálogo que aparece, seleccione *signalr. exe* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Agregar signalr. exe al proyecto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="75b6f-142">Haga clic con el botón secundario en la carpeta de *Inicio* que creó.</span><span class="sxs-lookup"><span data-stu-id="75b6f-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="75b6f-143">Seleccione **Agregar** > **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="75b6f-144">Seleccione el nodo **General** , seleccione **archivo de texto**y asigne al nuevo elemento el nombre *SignalRPerfCounterInstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="75b6f-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="75b6f-145">Este archivo de comandos instalará los contadores de rendimiento de Signalr en el rol Web.</span><span class="sxs-lookup"><span data-stu-id="75b6f-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Crear archivo por lotes de instalación del contador de rendimiento de Signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="75b6f-147">Cuando Visual Studio crea el archivo *SignalRPerfCounterInstall. cmd* , se abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="75b6f-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="75b6f-148">Reemplace el contenido del archivo por el siguiente script y, a continuación, guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="75b6f-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="75b6f-149">Este script ejecuta *signalr. exe*, que agrega los contadores de rendimiento de signalr a la instancia de rol.</span><span class="sxs-lookup"><span data-stu-id="75b6f-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="75b6f-150">Seleccione el archivo *signalr. exe* en **Explorador de soluciones**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="75b6f-151">En las **propiedades**del archivo, establezca **Copiar en el directorio de salida** en **copiar siempre**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Establecer copiar en el directorio de salida en copiar siempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="75b6f-153">Repita el paso anterior para el archivo *SignalRPerfCounterInstall. cmd* .</span><span class="sxs-lookup"><span data-stu-id="75b6f-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="75b6f-154">Haga clic con el botón derecho en el archivo *SignalRPerfCounterInstall. cmd* y seleccione **abrir con**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="75b6f-155">En el cuadro de diálogo que aparece, seleccione **Editor binario** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Abrir con el Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="75b6f-157">En el Editor binario, seleccione los bytes iniciales del archivo y elimínelos.</span><span class="sxs-lookup"><span data-stu-id="75b6f-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="75b6f-158">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="75b6f-158">Save and close the file.</span></span>

    ![Eliminar los bytes iniciales](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="75b6f-160">Abra *ServiceDefinition. csdef* y agregue una tarea de inicio que ejecute el archivo *SignalrPerfCounterInstall. cmd* cuando se inicie el servicio:</span><span class="sxs-lookup"><span data-stu-id="75b6f-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="75b6f-161">Abra `Views/Shared/_Layout.cshtml` y quite el script de paquete de jQuery del final del archivo.</span><span class="sxs-lookup"><span data-stu-id="75b6f-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="75b6f-162">Agregue un cliente de JavaScript que llame continuamente al método `increment` en el servidor.</span><span class="sxs-lookup"><span data-stu-id="75b6f-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="75b6f-163">Abra `Views/Home/Index.cshtml` y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="75b6f-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="75b6f-164">Cree una nueva carpeta en el proyecto **WebRole1** denominado *hubs*.</span><span class="sxs-lookup"><span data-stu-id="75b6f-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="75b6f-165">Haga clic con el botón derecho en la carpeta *hubs* en **Explorador de soluciones** y seleccione **Agregar** > **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="75b6f-166">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la categoría **Web** > **signalr** y, a continuación, seleccione la plantilla de elemento **clase de concentrador de signalr (V2)** .</span><span class="sxs-lookup"><span data-stu-id="75b6f-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="75b6f-167">Asigne al nuevo concentrador el nombre *MyHub.CS* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Adición de la clase Signalr Hub a la carpeta hubs en el cuadro de diálogo Agregar nuevo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="75b6f-169">*MyHub.CS* se abrirá automáticamente en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="75b6f-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="75b6f-170">Reemplace el contenido por el código siguiente y, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="75b6f-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="75b6f-171">*[. Exe](signalr-connection-density-testing-with-crank.md)* es una herramienta de prueba de densidad de conexión que se proporciona con el código base de signalr.</span><span class="sxs-lookup"><span data-stu-id="75b6f-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="75b6f-172">Dado que el cigüeñal requiere una conexión persistente, agregue una al sitio para usarla al realizar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="75b6f-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="75b6f-173">Agregue una nueva carpeta al proyecto **WebRole1** denominado *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="75b6f-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="75b6f-174">Haga clic con el botón secundario en esta carpeta y seleccione **agregar** > **clase**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="75b6f-175">Asigne al nuevo archivo de clase el nombre *MyPersistentConnections.CS* y seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="75b6f-176">Visual Studio abrirá el archivo *MyPersistentConnections.CS* en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="75b6f-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="75b6f-177">Reemplace el contenido por el código siguiente y, a continuación, guarde y cierre el archivo:</span><span class="sxs-lookup"><span data-stu-id="75b6f-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="75b6f-178">Con la clase `Startup`, los objetos Signalr se inician cuando se inicia OWIN.</span><span class="sxs-lookup"><span data-stu-id="75b6f-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="75b6f-179">Abra o cree *Startup.CS* y reemplace el contenido por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="75b6f-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="75b6f-180">En el código anterior, el atributo `OwinStartup` marca esta clase para iniciar OWIN.</span><span class="sxs-lookup"><span data-stu-id="75b6f-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="75b6f-181">El método `Configuration` inicia Signalr.</span><span class="sxs-lookup"><span data-stu-id="75b6f-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="75b6f-182">Pruebe la aplicación en el Microsoft Azure Emulator presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="75b6f-183">Si encuentra un **FileLoadException** en **MapSignalR**, cambie las redirecciones de enlace en *Web. config* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="75b6f-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="75b6f-184">Espere aproximadamente un minuto.</span><span class="sxs-lookup"><span data-stu-id="75b6f-184">Wait about one minute.</span></span> <span data-ttu-id="75b6f-185">Abra la ventana de herramientas de Cloud Explorer en Visual Studio (**ver** > **Cloud Explorer**) y expanda la ruta de acceso `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="75b6f-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="75b6f-186">Haga doble clic en **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="75b6f-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="75b6f-187">Debería ver los contadores de Signalr en los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="75b6f-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="75b6f-188">Si no ve la tabla, es posible que tenga que volver a escribir sus credenciales de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75b6f-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="75b6f-189">Es posible que tenga que seleccionar el botón **Actualizar** para ver la tabla en **Cloud Explorer** o seleccionar el botón **Actualizar** de la ventana Abrir tabla para ver los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="75b6f-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Selección de la tabla de contadores de rendimiento de WAD en Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Mostrar los contadores recopilados en la tabla de contadores de rendimiento de WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="75b6f-192">Para probar la aplicación en la nube, actualice el archivo **ServiceConfiguration. Cloud. cscfg** y establezca la `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` en una cadena de conexión de Azure Storage cuenta válida.</span><span class="sxs-lookup"><span data-stu-id="75b6f-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="75b6f-193">Implemente la aplicación en la suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="75b6f-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="75b6f-194">Para más información sobre cómo implementar una aplicación en Azure, consulte [creación e implementación de un servicio en la nube](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="75b6f-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="75b6f-195">Espere unos minutos.</span><span class="sxs-lookup"><span data-stu-id="75b6f-195">Wait a few minutes.</span></span> <span data-ttu-id="75b6f-196">En **Cloud Explorer**, busque la cuenta de almacenamiento que configuró anteriormente y busque la tabla `WADPerformanceCountersTable` en ella.</span><span class="sxs-lookup"><span data-stu-id="75b6f-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="75b6f-197">Debería ver los contadores de Signalr en los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="75b6f-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="75b6f-198">Si no ve la tabla, es posible que tenga que volver a escribir sus credenciales de Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="75b6f-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="75b6f-199">Es posible que tenga que seleccionar el botón **Actualizar** para ver la tabla en **Cloud Explorer** o seleccionar el botón **Actualizar** de la ventana Abrir tabla para ver los datos de la tabla.</span><span class="sxs-lookup"><span data-stu-id="75b6f-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="75b6f-200">Especial gracias a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) para el contenido original que se usa en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="75b6f-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
