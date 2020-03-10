---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para autohospedar ASP.NET Web API ASP.NET 4. x
author: rick-anderson
description: Tutorial con código que muestra cómo hospedar ASP.NET Web API en una aplicación de consola.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448333"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="73377-103">Usar OWIN para autohospedar ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="73377-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="73377-104">En este tutorial se muestra cómo hospedar ASP.NET Web API en una aplicación de consola mediante OWIN para autohospedar el marco de la API Web.</span><span class="sxs-lookup"><span data-stu-id="73377-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="73377-105">[Open Web Interface for .net](http://owin.org) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="73377-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="73377-106">OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS.</span><span class="sxs-lookup"><span data-stu-id="73377-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73377-107">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="73377-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="73377-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="73377-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="73377-109">API Web 5.2.7</span><span class="sxs-lookup"><span data-stu-id="73377-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="73377-110">Puede encontrar el código fuente completo de este tutorial en [github.com/ASPNET/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="73377-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="73377-111">Creación de una aplicación de consola</span><span class="sxs-lookup"><span data-stu-id="73377-111">Create a console application</span></span>

<span data-ttu-id="73377-112">En el menú **archivo** , haga clic en **nuevo**y seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="73377-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="73377-113">En **instalado**, en **Visual C#** , seleccione **escritorio de Windows** y, a continuación, seleccione **aplicación de consola (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="73377-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="73377-114">Asigne al proyecto el nombre "OwinSelfhostSample" y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="73377-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="73377-115">Incorporación de la API Web y los paquetes OWIN</span><span class="sxs-lookup"><span data-stu-id="73377-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="73377-116">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="73377-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="73377-117">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="73377-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="73377-118">Se instalará el paquete de WebAPI OWIN selfhost y todos los paquetes OWIN necesarios.</span><span class="sxs-lookup"><span data-stu-id="73377-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="73377-119">Configuración de la API Web para el autohospedaje</span><span class="sxs-lookup"><span data-stu-id="73377-119">Configure Web API for self-host</span></span>

<span data-ttu-id="73377-120">En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="73377-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="73377-121">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="73377-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="73377-122">Reemplace todo el código reutilizable de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73377-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="73377-123">Incorporación de un controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="73377-123">Add a Web API controller</span></span>

<span data-ttu-id="73377-124">A continuación, agregue una clase de controlador de API Web.</span><span class="sxs-lookup"><span data-stu-id="73377-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="73377-125">En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="73377-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="73377-126">Asigne a la clase el nombre `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="73377-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="73377-127">Reemplace todo el código reutilizable de este archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73377-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="73377-128">Inicio del host OWIN y solicitud con HttpClient</span><span class="sxs-lookup"><span data-stu-id="73377-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="73377-129">Reemplace todo el código reutilizable en el archivo Program.cs por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="73377-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="73377-130">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="73377-130">Run the application</span></span>

<span data-ttu-id="73377-131">Para ejecutar la aplicación, presione F5 en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73377-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="73377-132">La salida debe tener un aspecto parecido al siguiente:</span><span class="sxs-lookup"><span data-stu-id="73377-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="73377-133">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="73377-133">Additional resources</span></span>

[<span data-ttu-id="73377-134">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="73377-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="73377-135">ASP.NET Web API host en un rol de trabajo de Azure</span><span class="sxs-lookup"><span data-stu-id="73377-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
