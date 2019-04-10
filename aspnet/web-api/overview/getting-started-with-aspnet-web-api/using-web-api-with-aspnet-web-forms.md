---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso de API Web con ASP.NET Web Forms, ASP.NET 4.x
author: MikeWasson
description: Tutorial con el código paso a paso para agregar la API Web a una aplicación de formularios de ASP.NET para ASP.NET 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422582"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="f7788-103">Usar Web API con Formularios Web Forms de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7788-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="f7788-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f7788-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f7788-105">Este tutorial le guiará por los pasos para agregar la API Web a una aplicación de formularios Web Forms de ASP.NET tradicional en ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="f7788-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="f7788-106">Información general</span><span class="sxs-lookup"><span data-stu-id="f7788-106">Overview</span></span>

<span data-ttu-id="f7788-107">Aunque ASP.NET Web API se empaqueta con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="f7788-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="f7788-108">Para usar la API Web en una aplicación de formularios Web Forms, hay dos pasos principales:</span><span class="sxs-lookup"><span data-stu-id="f7788-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="f7788-109">Agregar un controlador de API Web que se deriva de la **ApiController** clase.</span><span class="sxs-lookup"><span data-stu-id="f7788-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="f7788-110">Agregar una tabla de rutas para el **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="f7788-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="f7788-111">Crear un proyecto de Web Forms</span><span class="sxs-lookup"><span data-stu-id="f7788-111">Create a Web Forms Project</span></span>

<span data-ttu-id="f7788-112">Inicie Visual Studio y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="f7788-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="f7788-113">O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f7788-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="f7788-114">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="f7788-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="f7788-115">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="f7788-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="f7788-116">En la lista de plantillas de proyecto, seleccione **aplicación de ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="f7788-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="f7788-117">Escriba un nombre para el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f7788-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="f7788-118">Crear el modelo y controlador</span><span class="sxs-lookup"><span data-stu-id="f7788-118">Create the Model and Controller</span></span>

<span data-ttu-id="f7788-119">Este tutorial usa las mismas clases de modelo y controlador como el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="f7788-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="f7788-120">En primer lugar, agregue una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="f7788-120">First, add a model class.</span></span> <span data-ttu-id="f7788-121">En **el Explorador de soluciones**, haga clic en el proyecto y seleccione **Agregar clase**.</span><span class="sxs-lookup"><span data-stu-id="f7788-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="f7788-122">Nombre de la clase de producto y agregue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="f7788-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="f7788-123">A continuación, agregue un controlador Web API al proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para API Web.</span><span class="sxs-lookup"><span data-stu-id="f7788-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="f7788-124">En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="f7788-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f7788-125">Seleccione **Agregar nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f7788-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="f7788-126">En **plantillas instaladas**, expanda **Visual C#** y seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="f7788-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="f7788-127">A continuación, en la lista de plantillas, seleccione **clase de controlador de Web API**.</span><span class="sxs-lookup"><span data-stu-id="f7788-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="f7788-128">Asigne al controlador el "ProductsController" y haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="f7788-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="f7788-129">El **Agregar nuevo elemento** asistente creará un archivo denominado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="f7788-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="f7788-130">Elimine los métodos que incluye el asistente y agregue los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="f7788-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="f7788-131">Para obtener más información sobre el código de este controlador, consulte el [Introducción](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="f7788-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="f7788-132">Agregar información de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="f7788-132">Add Routing Information</span></span>

<span data-ttu-id="f7788-133">A continuación, vamos a agregar una ruta URI así que esa URI del formulario &quot;/API/productos/&quot; se enrutan al controlador.</span><span class="sxs-lookup"><span data-stu-id="f7788-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="f7788-134">En **el Explorador de soluciones**, haga doble clic en Global.asax para abrir el archivo de código subyacente Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="f7788-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="f7788-135">Agregue el siguiente **mediante** instrucción.</span><span class="sxs-lookup"><span data-stu-id="f7788-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="f7788-136">A continuación, agregue el código siguiente a la **aplicación\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="f7788-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="f7788-137">Para obtener más información acerca de las tablas de enrutamientos, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f7788-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="f7788-138">Agregar cliente AJAX</span><span class="sxs-lookup"><span data-stu-id="f7788-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="f7788-139">Eso es todo lo que necesita para crear una API web que pueden tener acceso los clientes.</span><span class="sxs-lookup"><span data-stu-id="f7788-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="f7788-140">Ahora vamos a agregar una página HTML que usa jQuery para llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="f7788-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="f7788-141">Asegúrese de que la página maestra (por ejemplo, *Site.Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="f7788-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="f7788-142">Abra el archivo Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="f7788-142">Open the file Default.aspx.</span></span> <span data-ttu-id="f7788-143">Reemplace el texto reutilizable que se encuentra en la sección de contenido principal, como se muestra:</span><span class="sxs-lookup"><span data-stu-id="f7788-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="f7788-144">A continuación, agregue una referencia al archivo de código fuente de jQuery en la `HeaderContent` sección:</span><span class="sxs-lookup"><span data-stu-id="f7788-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="f7788-145">Nota: Puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **el Explorador de soluciones** en la ventana del editor de código.</span><span class="sxs-lookup"><span data-stu-id="f7788-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="f7788-146">A continuación de la etiqueta de script de jQuery, agregue el siguiente bloque de script:</span><span class="sxs-lookup"><span data-stu-id="f7788-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="f7788-147">Cuando se carga el documento, este script realiza una solicitud de AJAX para &quot;api/products&quot;.</span><span class="sxs-lookup"><span data-stu-id="f7788-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="f7788-148">La solicitud devuelve una lista de productos en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="f7788-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="f7788-149">El script agrega la información del producto a la tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="f7788-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="f7788-150">Al ejecutar la aplicación, debe ver así:</span><span class="sxs-lookup"><span data-stu-id="f7788-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
