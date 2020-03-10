---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Crear una aplicación cliente de OData V4C#() | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448123"
---
# <a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="086c0-102">Crear una aplicación de cliente de OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="086c0-102">Create an OData v4 Client App (C#)</span></span>

<span data-ttu-id="086c0-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="086c0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="086c0-104">En el tutorial anterior, creó un servicio OData básico que admite operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="086c0-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="086c0-105">Ahora vamos a crear un cliente para el servicio.</span><span class="sxs-lookup"><span data-stu-id="086c0-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="086c0-106">Inicie una nueva instancia de Visual Studio y cree un nuevo proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="086c0-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="086c0-107">En el cuadro de diálogo **nuevo proyecto** , seleccione **instalado** &gt; **plantillas** &gt;  **C# visual** &gt; **escritorio de Windows**y seleccione la plantilla aplicación de **consola** .</span><span class="sxs-lookup"><span data-stu-id="086c0-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="086c0-108">Asigne al proyecto el nombre &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="086c0-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="086c0-109">También puede Agregar la aplicación de consola a la misma solución de Visual Studio que contiene el servicio OData.</span><span class="sxs-lookup"><span data-stu-id="086c0-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>

## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="086c0-110">Instalación del generador de código de cliente de OData</span><span class="sxs-lookup"><span data-stu-id="086c0-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="086c0-111">En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="086c0-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="086c0-112">Seleccione **en línea** &gt; **Galería de Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="086c0-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="086c0-113">En el cuadro de búsqueda, busque &quot;generador de código de cliente de OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="086c0-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="086c0-114">Haga clic en **Descargar** para instalar el VSIX.</span><span class="sxs-lookup"><span data-stu-id="086c0-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="086c0-115">Es posible que se le pida que reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="086c0-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="086c0-116">Ejecutar el servicio de OData localmente</span><span class="sxs-lookup"><span data-stu-id="086c0-116">Run the OData Service Locally</span></span>

<span data-ttu-id="086c0-117">Ejecute el proyecto ProductService desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="086c0-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="086c0-118">De forma predeterminada, Visual Studio inicia un explorador en la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="086c0-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="086c0-119">Anote el URI; lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="086c0-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="086c0-120">Deje la aplicación en ejecución.</span><span class="sxs-lookup"><span data-stu-id="086c0-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="086c0-121">Si coloca ambos proyectos en la misma solución, asegúrese de ejecutar el proyecto ProductService sin depuración.</span><span class="sxs-lookup"><span data-stu-id="086c0-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="086c0-122">En el paso siguiente, tendrá que mantener el servicio en ejecución mientras modifica el proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="086c0-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>

## <a name="generate-the-service-proxy"></a><span data-ttu-id="086c0-123">Generar el proxy del servicio</span><span class="sxs-lookup"><span data-stu-id="086c0-123">Generate the Service Proxy</span></span>

<span data-ttu-id="086c0-124">El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio OData.</span><span class="sxs-lookup"><span data-stu-id="086c0-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="086c0-125">El proxy convierte las llamadas al método en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="086c0-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="086c0-126">Va a crear la clase de proxy mediante la ejecución de una [plantilla T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="086c0-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="086c0-127">Haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="086c0-127">Right-click the project.</span></span> <span data-ttu-id="086c0-128">Seleccione **Agregar** &gt; **Nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="086c0-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="086c0-129">En el cuadro de diálogo **Agregar nuevo elemento** , seleccione  **C# elementos visuales** &gt; **código** &gt; **cliente de oData**.</span><span class="sxs-lookup"><span data-stu-id="086c0-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="086c0-130">Asigne a la plantilla el nombre &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="086c0-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="086c0-131">Haga clic en **Agregar** y haga clic en la advertencia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="086c0-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="086c0-132">En este momento, obtendrá un error, que se puede omitir.</span><span class="sxs-lookup"><span data-stu-id="086c0-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="086c0-133">Visual Studio ejecuta automáticamente la plantilla, pero la plantilla necesita algunas opciones de configuración en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="086c0-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="086c0-134">Abra el archivo ProductClient. OData. config. En el elemento `Parameter`, pegue el URI del proyecto ProductService (paso anterior).</span><span class="sxs-lookup"><span data-stu-id="086c0-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="086c0-135">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="086c0-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="086c0-136">Vuelva a ejecutar la plantilla.</span><span class="sxs-lookup"><span data-stu-id="086c0-136">Run the template again.</span></span> <span data-ttu-id="086c0-137">En Explorador de soluciones, haga clic con el botón derecho en el archivo ProductClient.tt y seleccione **Ejecutar herramienta personalizada**.</span><span class="sxs-lookup"><span data-stu-id="086c0-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="086c0-138">La plantilla crea un archivo de código denominado ProductClient.cs que define el proxy.</span><span class="sxs-lookup"><span data-stu-id="086c0-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="086c0-139">Al desarrollar la aplicación, si cambia el punto de conexión de OData, vuelva a ejecutar la plantilla para actualizar el proxy.</span><span class="sxs-lookup"><span data-stu-id="086c0-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="086c0-140">Usar el proxy de servicio para llamar al servicio OData</span><span class="sxs-lookup"><span data-stu-id="086c0-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="086c0-141">Abra el archivo Program.cs y reemplace el código reutilizable con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="086c0-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="086c0-142">Reemplace el valor de *serviceUri* por el URI del servicio anterior.</span><span class="sxs-lookup"><span data-stu-id="086c0-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="086c0-143">Al ejecutar la aplicación, debe generar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="086c0-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
