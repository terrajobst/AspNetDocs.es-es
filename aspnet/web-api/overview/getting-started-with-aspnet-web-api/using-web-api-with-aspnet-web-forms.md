---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso de Web API con ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial con código paso a paso para agregar la API Web a una aplicación de formularios de ASP.NET para ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448537"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="fb634-103">Usar Web API con Formularios Web Forms de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fb634-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="fb634-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fb634-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fb634-105">Este tutorial le guiará por los pasos necesarios para agregar la API Web a una aplicación de formularios Web Forms tradicional de ASP.NET en ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="fb634-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="fb634-106">Información general</span><span class="sxs-lookup"><span data-stu-id="fb634-106">Overview</span></span>

<span data-ttu-id="fb634-107">Aunque ASP.NET Web API está empaquetado con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="fb634-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="fb634-108">Para usar Web API en una aplicación de formularios Web Forms, hay dos pasos principales:</span><span class="sxs-lookup"><span data-stu-id="fb634-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="fb634-109">Agregue un controlador de API Web que se derive de la clase **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="fb634-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="fb634-110">Agregue una tabla de rutas al método de **Inicio de\_** de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fb634-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="fb634-111">Crear un proyecto de formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="fb634-111">Create a Web Forms Project</span></span>

<span data-ttu-id="fb634-112">Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="fb634-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="fb634-113">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="fb634-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="fb634-114">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  .</span><span class="sxs-lookup"><span data-stu-id="fb634-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="fb634-115">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="fb634-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="fb634-116">En la lista de plantillas de proyecto, seleccione **ASP.NET Web Forms Application**.</span><span class="sxs-lookup"><span data-stu-id="fb634-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="fb634-117">Escriba un nombre para el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="fb634-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="fb634-118">Crear el modelo y el controlador</span><span class="sxs-lookup"><span data-stu-id="fb634-118">Create the Model and Controller</span></span>

<span data-ttu-id="fb634-119">En este tutorial se usan las mismas clases de modelo y controlador que en el tutorial de [Introducción](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="fb634-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="fb634-120">En primer lugar, agregue una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="fb634-120">First, add a model class.</span></span> <span data-ttu-id="fb634-121">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar clase**.</span><span class="sxs-lookup"><span data-stu-id="fb634-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="fb634-122">Asigne a la clase el nombre Product y agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="fb634-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="fb634-123">Después, agregue un controlador de API Web al proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para la API Web.</span><span class="sxs-lookup"><span data-stu-id="fb634-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="fb634-124">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="fb634-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="fb634-125">Seleccione **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="fb634-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="fb634-126">En **plantillas instaladas**, expanda  **C# visual** y seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="fb634-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="fb634-127">A continuación, en la lista de plantillas, seleccione **clase de controlador de API Web**.</span><span class="sxs-lookup"><span data-stu-id="fb634-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="fb634-128">Asigne al controlador el nombre "ProductsController" y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="fb634-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="fb634-129">El Asistente para **Agregar nuevo elemento** creará un archivo denominado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="fb634-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="fb634-130">Elimine los métodos incluidos en el asistente y agregue los siguientes métodos:</span><span class="sxs-lookup"><span data-stu-id="fb634-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="fb634-131">Para obtener más información sobre el código de este controlador, vea el tutorial de [Introducción](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="fb634-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="fb634-132">Agregar información de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="fb634-132">Add Routing Information</span></span>

<span data-ttu-id="fb634-133">A continuación, agregaremos una ruta de URI para que los URI del formulario &quot;&quot;/API/Products/se enruten al controlador.</span><span class="sxs-lookup"><span data-stu-id="fb634-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="fb634-134">En **Explorador de soluciones**, haga doble clic en global. asax para abrir el archivo de código subyacente global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="fb634-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="fb634-135">Agregue la siguiente instrucción **using** .</span><span class="sxs-lookup"><span data-stu-id="fb634-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="fb634-136">A continuación, agregue el siguiente código al método de **Inicio de\_** de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="fb634-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="fb634-137">Para obtener más información acerca de las tablas de enrutamiento, consulte [enrutamiento en ASP.net web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="fb634-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="fb634-138">Agregar AJAX del lado cliente</span><span class="sxs-lookup"><span data-stu-id="fb634-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="fb634-139">Eso es todo lo que necesita para crear una API Web a la que puedan acceder los clientes.</span><span class="sxs-lookup"><span data-stu-id="fb634-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="fb634-140">Ahora vamos a agregar una página HTML que usa jQuery para llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="fb634-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="fb634-141">Asegúrese de que la página maestra (por ejemplo, *site. Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="fb634-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="fb634-142">Abra el archivo default. aspx.</span><span class="sxs-lookup"><span data-stu-id="fb634-142">Open the file Default.aspx.</span></span> <span data-ttu-id="fb634-143">Reemplace el texto reutilizable que se encuentra en la sección contenido principal, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="fb634-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="fb634-144">A continuación, agregue una referencia al archivo de origen de jQuery en la sección `HeaderContent`:</span><span class="sxs-lookup"><span data-stu-id="fb634-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="fb634-145">Nota: puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **Explorador de soluciones** en la ventana del editor de código.</span><span class="sxs-lookup"><span data-stu-id="fb634-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="fb634-146">Debajo de la etiqueta de script de jQuery, agregue el siguiente bloque de script:</span><span class="sxs-lookup"><span data-stu-id="fb634-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="fb634-147">Cuando se carga el documento, este script realiza una solicitud AJAX a &quot;API/Products&quot;.</span><span class="sxs-lookup"><span data-stu-id="fb634-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="fb634-148">La solicitud devuelve una lista de productos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="fb634-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="fb634-149">El script agrega la información del producto a la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="fb634-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="fb634-150">Al ejecutar la aplicación, debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="fb634-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
