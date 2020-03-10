---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Crear un controlador (C#) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther muestra cómo puede Agregar un controlador a una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e3d0bae7f07410637c2b06c500d94a02c821f5c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437653"
---
# <a name="creating-a-controller-c"></a><span data-ttu-id="3cf39-103">Crear un controlador (C#)</span><span class="sxs-lookup"><span data-stu-id="3cf39-103">Creating a Controller (C#)</span></span>

<span data-ttu-id="3cf39-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="3cf39-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="3cf39-105">En este tutorial, Stephen Walther muestra cómo puede Agregar un controlador a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3cf39-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="3cf39-106">El objetivo de este tutorial es explicar cómo se pueden crear controladores ASP.NET MVC nuevos.</span><span class="sxs-lookup"><span data-stu-id="3cf39-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="3cf39-107">Aprenderá a crear controladores con la opción de menú Agregar controlador de Visual Studio y mediante la creación de un archivo de clase a mano.</span><span class="sxs-lookup"><span data-stu-id="3cf39-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="3cf39-108">Uso de la opción de menú Agregar controlador</span><span class="sxs-lookup"><span data-stu-id="3cf39-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="3cf39-109">La forma más fácil de crear un nuevo controlador es hacer clic con el botón derecho en la carpeta Controllers de la ventana de Explorador de soluciones de Visual Studio y seleccionar la opción de menú **Agregar, controlador** (vea la ilustración 1).</span><span class="sxs-lookup"><span data-stu-id="3cf39-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="3cf39-110">Al seleccionar esta opción de menú, se abre el cuadro de diálogo **Agregar controlador** (vea la figura 2).</span><span class="sxs-lookup"><span data-stu-id="3cf39-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="3cf39-111">[![el cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3cf39-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="3cf39-112">**Figura 01**: adición de un nuevo controlador ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="3cf39-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>

<span data-ttu-id="3cf39-113">[![el cuadro de diálogo nuevo proyecto](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3cf39-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="3cf39-114">**Figura 02**: cuadro de diálogo Agregar controlador ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="3cf39-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>

<span data-ttu-id="3cf39-115">Observe que la primera parte del nombre del controlador se resalta en el cuadro de diálogo **Agregar controlador** .</span><span class="sxs-lookup"><span data-stu-id="3cf39-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="3cf39-116">Cada nombre de controlador debe terminar con el *controlador*de sufijo.</span><span class="sxs-lookup"><span data-stu-id="3cf39-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="3cf39-117">Por ejemplo, puede crear un controlador denominado *ProductController* pero no un controlador denominado *Product*.</span><span class="sxs-lookup"><span data-stu-id="3cf39-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="3cf39-118">Si crea un controlador que no tiene el sufijo del *controlador* , no podrá invocar el controlador.</span><span class="sxs-lookup"><span data-stu-id="3cf39-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="3cf39-119">No haga esto--he perdido muchas horas de mi vida después de cometer este error.</span><span class="sxs-lookup"><span data-stu-id="3cf39-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="3cf39-120">**Lista 1-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="3cf39-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="3cf39-121">Siempre debe crear controladores en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="3cf39-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="3cf39-122">De lo contrario, infringirá las convenciones de ASP.NET MVC y otros desarrolladores tendrán un tiempo más difícil de entender su aplicación.</span><span class="sxs-lookup"><span data-stu-id="3cf39-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="3cf39-123">Métodos de acción de scaffolding</span><span class="sxs-lookup"><span data-stu-id="3cf39-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="3cf39-124">Al crear un controlador, tiene la opción de generar automáticamente métodos de acción de creación, actualización y detalles (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="3cf39-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="3cf39-125">Si selecciona esta opción, se genera la clase de controlador de la lista 2.</span><span class="sxs-lookup"><span data-stu-id="3cf39-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="3cf39-126">[![crear métodos de acción automáticamente](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3cf39-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="3cf39-127">**Figura 03**: creación automática de métodos de acción ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="3cf39-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>

<span data-ttu-id="3cf39-128">**Lista 2-Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="3cf39-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="3cf39-129">Estos métodos generados son métodos de código auxiliar.</span><span class="sxs-lookup"><span data-stu-id="3cf39-129">These generated methods are stub methods.</span></span> <span data-ttu-id="3cf39-130">Debe agregar la lógica real para crear, actualizar y mostrar los detalles de un cliente.</span><span class="sxs-lookup"><span data-stu-id="3cf39-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="3cf39-131">Sin embargo, los métodos de código auxiliar proporcionan un buen punto de partida.</span><span class="sxs-lookup"><span data-stu-id="3cf39-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="3cf39-132">Crear una clase de controlador</span><span class="sxs-lookup"><span data-stu-id="3cf39-132">Creating a Controller Class</span></span>

<span data-ttu-id="3cf39-133">El controlador ASP.NET MVC es simplemente una clase.</span><span class="sxs-lookup"><span data-stu-id="3cf39-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="3cf39-134">Si lo prefiere, puede omitir el práctico scaffolding del controlador de Visual Studio y crear una clase de controlador a mano.</span><span class="sxs-lookup"><span data-stu-id="3cf39-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="3cf39-135">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="3cf39-135">Follow these steps:</span></span>

1. <span data-ttu-id="3cf39-136">Haga clic con el botón derecho en la carpeta Controllers y seleccione la opción de menú **Agregar, nuevo elemento** y seleccione la plantilla de **clase** (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="3cf39-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="3cf39-137">Asigne a la nueva clase el nombre PersonController.cs y haga clic en el botón **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="3cf39-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="3cf39-138">Modifique el archivo de clase resultante para que la clase herede de la clase base System. Web. Mvc. Controller (vea la lista 3).</span><span class="sxs-lookup"><span data-stu-id="3cf39-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="3cf39-139">[![crear una nueva clase](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="3cf39-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="3cf39-140">**Figura 04**: creación de una nueva clase ([haga clic para ver la imagen de tamaño completo](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="3cf39-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>

<span data-ttu-id="3cf39-141">**Lista 3: Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="3cf39-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="3cf39-142">El controlador de la lista 3 expone una acción denominada index () que devuelve la cadena "Hola mundo!".</span><span class="sxs-lookup"><span data-stu-id="3cf39-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="3cf39-143">Para invocar esta acción del controlador, ejecute la aplicación y solicite una dirección URL similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="3cf39-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="3cf39-144">El Servidor de desarrollo de ASP.NET utiliza un número de Puerto aleatorio (por ejemplo, 40071).</span><span class="sxs-lookup"><span data-stu-id="3cf39-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="3cf39-145">Al escribir una dirección URL para invocar un controlador, deberá proporcionar el número de puerto correcto.</span><span class="sxs-lookup"><span data-stu-id="3cf39-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="3cf39-146">Puede determinar el número de puerto al mantener el mouse sobre el icono del Servidor de desarrollo de ASP.NET en el área de notificación de Windows (parte inferior derecha de la pantalla).</span><span class="sxs-lookup"><span data-stu-id="3cf39-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="3cf39-147">[Anterior](adding-dynamic-content-to-a-cached-page-cs.md)
> [Siguiente](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="3cf39-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
