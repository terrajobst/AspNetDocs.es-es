---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-host ASP.NET Web API 1 (C#)-ASP.net 4. x
author: MikeWasson
description: Tutorial con código muestra cómo hospedar una API Web en una aplicación de consola.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421375"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="25abc-103">Self-host ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="25abc-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="25abc-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="25abc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="25abc-105">En este tutorial se muestra cómo hospedar una API Web en una aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="25abc-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="25abc-106">ASP.NET Web API no requiere IIS.</span><span class="sxs-lookup"><span data-stu-id="25abc-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="25abc-107">Puede hospedar una API Web en su propio proceso de host.</span><span class="sxs-lookup"><span data-stu-id="25abc-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="25abc-108">**Las nuevas aplicaciones deben usar OWIN para autohospedar la API Web.**</span><span class="sxs-lookup"><span data-stu-id="25abc-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="25abc-109">Consulte [uso de OWIN para Autohospedar ASP.net web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="25abc-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="25abc-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="25abc-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="25abc-111">API web 1</span><span class="sxs-lookup"><span data-stu-id="25abc-111">Web API 1</span></span>
> - <span data-ttu-id="25abc-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="25abc-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="25abc-113">Crear el proyecto de aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="25abc-113">Create the Console Application Project</span></span>

<span data-ttu-id="25abc-114">Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="25abc-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="25abc-115">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="25abc-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="25abc-116">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="25abc-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="25abc-117">En **Visual C#** , seleccione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="25abc-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="25abc-118">En la lista de plantillas de proyecto, seleccione **aplicación de consola**.</span><span class="sxs-lookup"><span data-stu-id="25abc-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="25abc-119">Asigne al proyecto el nombre &quot;SelfHost&quot; y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="25abc-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="25abc-120">Establecer la plataforma de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="25abc-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="25abc-121">Si usa Visual Studio 2010, cambie la versión de .NET Framework de destino a .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="25abc-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="25abc-122">(De forma predeterminada, la plantilla de proyecto tiene como destino [.NET Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)).</span><span class="sxs-lookup"><span data-stu-id="25abc-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="25abc-123">En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="25abc-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="25abc-124">En la lista desplegable **plataforma de destino** , cambie la versión de .NET Framework de destino a .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="25abc-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="25abc-125">Cuando se le pida que aplique el cambio, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="25abc-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="25abc-126">Instalar el administrador de paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="25abc-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="25abc-127">El administrador de paquetes NuGet es la forma más fácil de agregar los ensamblados de la API Web a un proyecto de non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="25abc-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="25abc-128">Para comprobar si el administrador de paquetes NuGet está instalado, haga clic en el menú **herramientas** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25abc-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="25abc-129">Si ve un elemento de menú denominado **Administrador de paquetes Nuget**, tiene el administrador de paquetes Nuget.</span><span class="sxs-lookup"><span data-stu-id="25abc-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="25abc-130">Para instalar el administrador de paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="25abc-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="25abc-131">Inicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25abc-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="25abc-132">En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="25abc-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="25abc-133">En el cuadro de diálogo **extensiones y actualizaciones** , seleccione **en línea**.</span><span class="sxs-lookup"><span data-stu-id="25abc-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="25abc-134">Si no ve "Administrador de paquetes NuGet", escriba "Administrador de paquetes Nuget" en el cuadro de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="25abc-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="25abc-135">Seleccione el administrador de paquetes NuGet y haga clic en **Descargar**.</span><span class="sxs-lookup"><span data-stu-id="25abc-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="25abc-136">Una vez finalizada la descarga, se le pedirá que instale.</span><span class="sxs-lookup"><span data-stu-id="25abc-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="25abc-137">Una vez finalizada la instalación, es posible que se le pida que reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25abc-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="25abc-138">Adición del paquete NuGet de Web API</span><span class="sxs-lookup"><span data-stu-id="25abc-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="25abc-139">Una vez instalado el administrador de paquetes NuGet, agregue el paquete de autohospedaje de la API Web al proyecto.</span><span class="sxs-lookup"><span data-stu-id="25abc-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="25abc-140">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="25abc-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="25abc-141">*Nota*: Si no ve este elemento de menú, asegúrese de que el administrador de paquetes NuGet esté instalado correctamente.</span><span class="sxs-lookup"><span data-stu-id="25abc-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="25abc-142">Seleccione **administrar paquetes NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="25abc-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="25abc-143">En el cuadro de diálogo **administrar paquetes** de **Internet, seleccione en línea**.</span><span class="sxs-lookup"><span data-stu-id="25abc-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="25abc-144">En el cuadro de búsqueda, escriba &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="25abc-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="25abc-145">Seleccione el paquete de host propio ASP.NET Web API y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="25abc-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="25abc-146">Una vez instalado el paquete, haga clic en **cerrar** para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="25abc-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="25abc-147">Asegúrese de instalar el paquete denominado Microsoft. AspNet. WebApi. SelfHost, no AspNetWebApi. SelfHost.</span><span class="sxs-lookup"><span data-stu-id="25abc-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="25abc-148">Crear el modelo y el controlador</span><span class="sxs-lookup"><span data-stu-id="25abc-148">Create the Model and Controller</span></span>

<span data-ttu-id="25abc-149">En este tutorial se usan las mismas clases de modelo y controlador que en el tutorial de [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="25abc-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="25abc-150">Agregue una clase pública denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="25abc-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="25abc-151">Agregue una clase pública denominada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="25abc-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="25abc-152">Derive esta clase de **System. Web. http. ApiController**.</span><span class="sxs-lookup"><span data-stu-id="25abc-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="25abc-153">Para obtener más información sobre el código de este controlador, vea el tutorial de [Introducción](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="25abc-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="25abc-154">Este controlador define tres acciones GET:</span><span class="sxs-lookup"><span data-stu-id="25abc-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="25abc-155">Identificador URI</span><span class="sxs-lookup"><span data-stu-id="25abc-155">URI</span></span> | <span data-ttu-id="25abc-156">Description</span><span class="sxs-lookup"><span data-stu-id="25abc-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="25abc-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="25abc-157">/api/products</span></span> | <span data-ttu-id="25abc-158">Obtiene una lista de todos los productos.</span><span class="sxs-lookup"><span data-stu-id="25abc-158">Get a list of all products.</span></span> |
| <span data-ttu-id="25abc-159">*identificador* de/API/Products/</span><span class="sxs-lookup"><span data-stu-id="25abc-159">/api/products/*id*</span></span> | <span data-ttu-id="25abc-160">Obtener un producto por identificador.</span><span class="sxs-lookup"><span data-stu-id="25abc-160">Get a product by ID.</span></span> |
| <span data-ttu-id="25abc-161">/API/Products/? categoría =*categoría*</span><span class="sxs-lookup"><span data-stu-id="25abc-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="25abc-162">Obtiene una lista de productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="25abc-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="25abc-163">Hospedar la API Web</span><span class="sxs-lookup"><span data-stu-id="25abc-163">Host the Web API</span></span>

<span data-ttu-id="25abc-164">Abra el archivo Program.cs y agregue las siguientes instrucciones Using:</span><span class="sxs-lookup"><span data-stu-id="25abc-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="25abc-165">Agregue el código siguiente a la clase **Program** .</span><span class="sxs-lookup"><span data-stu-id="25abc-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="25abc-166">Opta Agregar una reserva de espacio de nombres de URL HTTP</span><span class="sxs-lookup"><span data-stu-id="25abc-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="25abc-167">Esta aplicación escucha en `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="25abc-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="25abc-168">De forma predeterminada, la escucha en una dirección HTTP determinada requiere privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="25abc-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="25abc-169">Al ejecutar el tutorial, por lo tanto, puede obtener este error: "HTTP no pudo registrar la dirección URL http://+:8080/" hay dos maneras de evitar este error:</span><span class="sxs-lookup"><span data-stu-id="25abc-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="25abc-170">Ejecutar Visual Studio con permisos de administrador elevados, o</span><span class="sxs-lookup"><span data-stu-id="25abc-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="25abc-171">Use netsh. exe para conceder permisos de cuenta para reservar la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="25abc-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="25abc-172">Para usar netsh. exe, abra un símbolo del sistema con privilegios de administrador y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="25abc-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="25abc-173">donde *equipo\nombredeusuario* es su cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="25abc-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="25abc-174">Cuando haya terminado el autohospedado, asegúrese de eliminar la reserva:</span><span class="sxs-lookup"><span data-stu-id="25abc-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="25abc-175">Llamar a la API Web desde una aplicación clienteC#()</span><span class="sxs-lookup"><span data-stu-id="25abc-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="25abc-176">Vamos a escribir una aplicación de consola simple que llama a la API Web.</span><span class="sxs-lookup"><span data-stu-id="25abc-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="25abc-177">Agregue un nuevo proyecto de aplicación de consola a la solución:</span><span class="sxs-lookup"><span data-stu-id="25abc-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="25abc-178">En Explorador de soluciones, haga clic con el botón derecho en la solución y seleccione **Agregar nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="25abc-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="25abc-179">Cree una nueva aplicación de consola denominada &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="25abc-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="25abc-180">Use el administrador de paquetes NuGet para agregar el paquete de bibliotecas de ASP.NET Web API Core:</span><span class="sxs-lookup"><span data-stu-id="25abc-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="25abc-181">En el menú herramientas, seleccione **Administrador de paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="25abc-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="25abc-182">Seleccione **administrar paquetes NuGet para la solución**</span><span class="sxs-lookup"><span data-stu-id="25abc-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="25abc-183">En el cuadro de diálogo **administrar paquetes NuGet** , seleccione **en línea**.</span><span class="sxs-lookup"><span data-stu-id="25abc-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="25abc-184">En el cuadro de búsqueda, escriba &quot;Microsoft. AspNet. WebApi. Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="25abc-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="25abc-185">Seleccione el paquete de bibliotecas de cliente de API Web de Microsoft ASP.NET y haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="25abc-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="25abc-186">Agregue una referencia en ClientApp al proyecto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="25abc-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="25abc-187">En Explorador de soluciones, haga clic con el botón derecho en el proyecto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="25abc-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="25abc-188">Seleccione **Agregar referencia**.</span><span class="sxs-lookup"><span data-stu-id="25abc-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="25abc-189">En el cuadro de diálogo **Administrador de referencias** , en **solución**, seleccione **proyectos**.</span><span class="sxs-lookup"><span data-stu-id="25abc-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="25abc-190">Seleccione el proyecto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="25abc-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="25abc-191">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="25abc-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="25abc-192">Abra el archivo Client/Program. cs.</span><span class="sxs-lookup"><span data-stu-id="25abc-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="25abc-193">Agregue la siguiente instrucción **using** :</span><span class="sxs-lookup"><span data-stu-id="25abc-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="25abc-194">Agregue una instancia de **HttpClient** estática:</span><span class="sxs-lookup"><span data-stu-id="25abc-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="25abc-195">Agregue los siguientes métodos para enumerar todos los productos, mostrar un producto por identificador y mostrar los productos por categoría.</span><span class="sxs-lookup"><span data-stu-id="25abc-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="25abc-196">Cada uno de estos métodos sigue el mismo patrón:</span><span class="sxs-lookup"><span data-stu-id="25abc-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="25abc-197">Llame a **HttpClient. GetAsync** para enviar una solicitud GET al URI adecuado.</span><span class="sxs-lookup"><span data-stu-id="25abc-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="25abc-198">Llame a **HttpResponseMessage. EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="25abc-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="25abc-199">Este método produce una excepción si el estado de la respuesta HTTP es un código de error.</span><span class="sxs-lookup"><span data-stu-id="25abc-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="25abc-200">Llame a **ReadAsAsync&lt;t&gt;** para deserializar un tipo de CLR a partir de la respuesta http.</span><span class="sxs-lookup"><span data-stu-id="25abc-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="25abc-201">Este método es un método de extensión, que se define en **System .net. http. HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="25abc-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="25abc-202">Los métodos **GetAsync** y **ReadAsAsync** son asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="25abc-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="25abc-203">Devuelven objetos de **tarea** que representan la operación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="25abc-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="25abc-204">Al obtener la propiedad **resultado** , el subproceso se bloquea hasta que se completa la operación.</span><span class="sxs-lookup"><span data-stu-id="25abc-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="25abc-205">Para obtener más información sobre el uso de HttpClient, incluido cómo realizar llamadas sin bloqueo, consulte [llamar a una API Web desde un cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="25abc-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="25abc-206">Antes de llamar a estos métodos, establezca la propiedad BaseAddress en la instancia HttpClient en "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="25abc-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="25abc-207">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="25abc-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="25abc-208">Esto debería generar la siguiente información.</span><span class="sxs-lookup"><span data-stu-id="25abc-208">This should output the following.</span></span> <span data-ttu-id="25abc-209">(Recuerde ejecutar primero la aplicación SelfHost).</span><span class="sxs-lookup"><span data-stu-id="25abc-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
