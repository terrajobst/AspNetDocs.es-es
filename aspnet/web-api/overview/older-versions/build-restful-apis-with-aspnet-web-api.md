---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Cree API de RESTful con ASP.NET Web API ASP.NET 4. x
author: rick-anderson
description: 'Laboratorio práctico: Use Web API en ASP.NET 4. x para compilar una sencilla API de REST para una aplicación de Contact Manager.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504271"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="945a5-103">Compilación de API de RESTful con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="945a5-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="945a5-104">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="945a5-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="945a5-105">Laboratorio práctico: Use Web API en ASP.NET 4. x para compilar una sencilla API de REST para una aplicación de Contact Manager.</span><span class="sxs-lookup"><span data-stu-id="945a5-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="945a5-106">También creará un cliente para usar la API.</span><span class="sxs-lookup"><span data-stu-id="945a5-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="945a5-107">En los últimos años, se ha vuelto claro que HTTP no es solo para proporcionar páginas HTML.</span><span class="sxs-lookup"><span data-stu-id="945a5-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="945a5-108">También es una plataforma eficaz para la creación de API Web, con una serie de verbos (GET, POST, etc.) además de algunos conceptos simples, como los *URI* y los *encabezados*.</span><span class="sxs-lookup"><span data-stu-id="945a5-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="945a5-109">ASP.NET Web API es un conjunto de componentes que simplifican la programación HTTP.</span><span class="sxs-lookup"><span data-stu-id="945a5-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="945a5-110">Dado que se basa en el tiempo de ejecución de ASP.NET MVC, Web API controla automáticamente los detalles de transporte de bajo nivel de HTTP.</span><span class="sxs-lookup"><span data-stu-id="945a5-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="945a5-111">Al mismo tiempo, la API Web expone de forma natural el modelo de programación HTTP.</span><span class="sxs-lookup"><span data-stu-id="945a5-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="945a5-112">De hecho, un objetivo de Web API es *no* abstraer la realidad de http.</span><span class="sxs-lookup"><span data-stu-id="945a5-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="945a5-113">Como resultado, la API Web es flexible y fácil de ampliar.</span><span class="sxs-lookup"><span data-stu-id="945a5-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="945a5-114">El estilo arquitectónico REST ha demostrado ser una manera eficaz de aprovechar HTTP, aunque ciertamente no es el único enfoque válido para HTTP.</span><span class="sxs-lookup"><span data-stu-id="945a5-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="945a5-115">El administrador de contactos expondrá el RESTful para enumerar, agregar y quitar contactos, entre otros.</span><span class="sxs-lookup"><span data-stu-id="945a5-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="945a5-116">Este laboratorio requiere un conocimiento básico de HTTP, REST y supone que tiene conocimientos prácticos básicos de HTML, JavaScript y jQuery.</span><span class="sxs-lookup"><span data-stu-id="945a5-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="945a5-117">El sitio web de ASP.NET tiene un área dedicada al marco de ASP.NET Web API en [https://asp.net/web-api](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="945a5-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="945a5-118">Este sitio seguirá proporcionando información de última hora, ejemplos y noticias relacionados con Web API, por lo que debe comprobarlo con frecuencia si desea profundizar más en el arte de crear API Web personalizadas disponibles para prácticamente cualquier dispositivo o marco de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="945a5-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="945a5-119">ASP.NET Web API, similar a ASP.NET MVC 4, tiene una gran flexibilidad en cuanto a la separación del nivel de servicio de los controladores, lo que le permite usar fácilmente varios de los marcos de inserción de dependencias disponibles.</span><span class="sxs-lookup"><span data-stu-id="945a5-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="945a5-120">Hay un buen ejemplo en MSDN que muestra cómo usar Ninject para la inserción de dependencias en un proyecto ASP.NET Web API que puede descargar desde [aquí](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="945a5-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="945a5-121">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="945a5-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="945a5-122">Objetivos</span><span class="sxs-lookup"><span data-stu-id="945a5-122">Objectives</span></span>

<span data-ttu-id="945a5-123">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="945a5-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="945a5-124">Implementación de una API web RESTful</span><span class="sxs-lookup"><span data-stu-id="945a5-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="945a5-125">Llamar a la API desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="945a5-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="945a5-126">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="945a5-126">Prerequisites</span></span>

<span data-ttu-id="945a5-127">Para completar este laboratorio práctico, es necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="945a5-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="945a5-128">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="945a5-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="945a5-129">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="945a5-129">Setup</span></span>

<span data-ttu-id="945a5-130">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="945a5-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="945a5-131">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="945a5-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="945a5-132">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="945a5-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="945a5-133">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice A: usar fragmentos de código](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="945a5-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="945a5-134">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="945a5-134">Exercises</span></span>

<span data-ttu-id="945a5-135">Este laboratorio práctico incluye el siguiente ejercicio:</span><span class="sxs-lookup"><span data-stu-id="945a5-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="945a5-136">Ejercicio 1: creación de una API Web de solo lectura</span><span class="sxs-lookup"><span data-stu-id="945a5-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="945a5-137">Ejercicio 2: creación de una API Web de lectura/escritura</span><span class="sxs-lookup"><span data-stu-id="945a5-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="945a5-138">Ejercicio 3: usar la API Web desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="945a5-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="945a5-139">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="945a5-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="945a5-140">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="945a5-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="945a5-141">Tiempo estimado para completar este laboratorio: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="945a5-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="945a5-142">Ejercicio 1: creación de una API Web de solo lectura</span><span class="sxs-lookup"><span data-stu-id="945a5-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="945a5-143">En este ejercicio, implementará los métodos GET de solo lectura para el administrador de contactos.</span><span class="sxs-lookup"><span data-stu-id="945a5-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="945a5-144">Tarea 1: creación del proyecto de API</span><span class="sxs-lookup"><span data-stu-id="945a5-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="945a5-145">En esta tarea, usará las nuevas plantillas de proyecto Web de ASP.NET para crear una aplicación Web de API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="945a5-146">Ejecute **Visual Studio 2012 Express para web**. para ello, vaya a **inicio** y escriba **vs Express para web** Presione **entrar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="945a5-147">En el menú **archivo** , seleccione **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="945a5-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="945a5-148">Seleccione el **Visual C# |** Tipo de proyecto web en la vista de árbol tipo de proyecto y, a continuación, seleccione el tipo de proyecto **aplicación web MVC 4 ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="945a5-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="945a5-149">Establezca el **nombre** del proyecto en *ContactManager* y el **nombre** de la solución en *Inicio*; a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="945a5-150">![Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0")</span><span class="sxs-lookup"><span data-stu-id="945a5-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="945a5-151">*Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0*</span><span class="sxs-lookup"><span data-stu-id="945a5-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="945a5-152">En el cuadro de diálogo tipo de proyecto de ASP.NET MVC 4, seleccione el tipo de proyecto **API Web** .</span><span class="sxs-lookup"><span data-stu-id="945a5-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="945a5-153">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-153">Click **OK**.</span></span>

    <span data-ttu-id="945a5-154">![Especificación del tipo de proyecto de API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Especificación del tipo de proyecto de API Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="945a5-155">*Especificación del tipo de proyecto de API Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="945a5-156">Tarea 2: creación de los controladores de API de Contact Manager</span><span class="sxs-lookup"><span data-stu-id="945a5-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="945a5-157">En esta tarea, creará las clases de controlador en las que residirán los métodos de la API.</span><span class="sxs-lookup"><span data-stu-id="945a5-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="945a5-158">Elimine el archivo denominado **ValuesController.CS** dentro de la carpeta **Controllers** del proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="945a5-159">Haga clic con el botón derecho en la carpeta **Controllers** del proyecto y seleccione **Agregar | Controlador** del menú contextual.</span><span class="sxs-lookup"><span data-stu-id="945a5-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="945a5-160">![Agregar un nuevo controlador al proyecto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Agregar un nuevo controlador al proyecto")</span><span class="sxs-lookup"><span data-stu-id="945a5-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="945a5-161">*Agregar un nuevo controlador al proyecto*</span><span class="sxs-lookup"><span data-stu-id="945a5-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="945a5-162">En el cuadro de diálogo **Agregar controlador** que aparece, seleccione **controlador de API vacío** en el menú plantilla.</span><span class="sxs-lookup"><span data-stu-id="945a5-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="945a5-163">Asigne a la clase de controlador el nombre **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="945a5-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="945a5-164">A continuación, haga clic en **Agregar.**</span><span class="sxs-lookup"><span data-stu-id="945a5-164">Then, click **Add.**</span></span>

    <span data-ttu-id="945a5-165">![Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="945a5-166">*Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="945a5-167">Agregue el código siguiente a **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="945a5-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="945a5-168">(Fragmento de código: *API Web Lab-EX01-Get API Method*)</span><span class="sxs-lookup"><span data-stu-id="945a5-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="945a5-169">Presione **F5** para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="945a5-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="945a5-170">Debe aparecer la Página principal predeterminada de un proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="945a5-171">![La Página principal predeterminada de una aplicación ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "La Página principal predeterminada de una aplicación ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="945a5-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="945a5-172">*La Página principal predeterminada de una aplicación ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="945a5-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="945a5-173">En la ventana de Internet Explorer, presione la tecla **F12** para abrir la ventana **herramientas de desarrollo** .</span><span class="sxs-lookup"><span data-stu-id="945a5-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="945a5-174">Haga clic en la pestaña **red** y, a continuación, haga clic en el botón **Iniciar captura** para empezar a capturar el tráfico de red en la ventana.</span><span class="sxs-lookup"><span data-stu-id="945a5-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="945a5-175">![Apertura de la pestaña red e inicio de la captura de red](build-restful-apis-with-aspnet-web-api/_static/image6.png "Apertura de la pestaña red e inicio de la captura de red")</span><span class="sxs-lookup"><span data-stu-id="945a5-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="945a5-176">*Apertura de la pestaña red e inicio de la captura de red*</span><span class="sxs-lookup"><span data-stu-id="945a5-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="945a5-177">Anexe la dirección URL de la barra de direcciones del explorador con **/API/Contact** y presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="945a5-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="945a5-178">Los detalles de la transmisión aparecerán en la ventana captura de red.</span><span class="sxs-lookup"><span data-stu-id="945a5-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="945a5-179">Tenga en cuenta que el tipo MIME de la respuesta es **Application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="945a5-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="945a5-180">Esto muestra cómo el formato de salida predeterminado es JSON.</span><span class="sxs-lookup"><span data-stu-id="945a5-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="945a5-181">![Visualización de la salida de la solicitud de API Web en la vista de red](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualización de la salida de la solicitud de API Web en la vista de red")</span><span class="sxs-lookup"><span data-stu-id="945a5-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="945a5-182">*Visualización de la salida de la solicitud de API Web en la vista de red*</span><span class="sxs-lookup"><span data-stu-id="945a5-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-183">El comportamiento predeterminado de Internet Explorer 10 en este momento será preguntar si el usuario desea guardar o abrir el flujo resultante de la llamada a la API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="945a5-184">La salida será un archivo de texto que contenga el resultado JSON de la llamada URL de la API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="945a5-185">No cancele el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="945a5-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="945a5-186">Haga clic en el botón **ir a vista detallada** para ver más detalles sobre la respuesta de esta llamada de API.</span><span class="sxs-lookup"><span data-stu-id="945a5-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="945a5-187">![Cambiar a la vista detallada](build-restful-apis-with-aspnet-web-api/_static/image8.png "Cambiar a la vista de detalles")</span><span class="sxs-lookup"><span data-stu-id="945a5-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="945a5-188">*Cambiar a la vista detallada*</span><span class="sxs-lookup"><span data-stu-id="945a5-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="945a5-189">Haga clic en la pestaña **cuerpo de respuesta** para ver el texto de respuesta JSON real.</span><span class="sxs-lookup"><span data-stu-id="945a5-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="945a5-190">![Visualización del texto de salida JSON en el monitor de red](build-restful-apis-with-aspnet-web-api/_static/image9.png "Visualización del texto de salida JSON en el monitor de red")</span><span class="sxs-lookup"><span data-stu-id="945a5-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="945a5-191">*Visualización del texto de salida JSON en el monitor de red*</span><span class="sxs-lookup"><span data-stu-id="945a5-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="945a5-192">Tarea 3: crear los modelos de contacto y aumentar el controlador de contactos</span><span class="sxs-lookup"><span data-stu-id="945a5-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="945a5-193">En esta tarea, creará las clases de controlador en las que residirán los métodos de la API.</span><span class="sxs-lookup"><span data-stu-id="945a5-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="945a5-194">Haga clic con el botón derecho en la carpeta **modelos** y seleccione **Agregar | Clase...** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="945a5-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="945a5-195">![Agregar un nuevo modelo a la aplicación Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Agregar un nuevo modelo a la aplicación Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="945a5-196">*Agregar un nuevo modelo a la aplicación Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="945a5-197">En el cuadro de diálogo **Agregar nuevo elemento** , asigne al nuevo archivo el nombre **Contact.CS** y haga clic en **Agregar.**</span><span class="sxs-lookup"><span data-stu-id="945a5-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="945a5-198">![Crear el nuevo archivo de clase de contacto](build-restful-apis-with-aspnet-web-api/_static/image11.png "Crear el nuevo archivo de clase de contacto")</span><span class="sxs-lookup"><span data-stu-id="945a5-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="945a5-199">*Crear el nuevo archivo de clase de contacto*</span><span class="sxs-lookup"><span data-stu-id="945a5-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="945a5-200">Agregue el siguiente código resaltado a la clase **Contact** .</span><span class="sxs-lookup"><span data-stu-id="945a5-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="945a5-201">(Fragmento de código- *Web API Lab-EX01-Contact (clase*))</span><span class="sxs-lookup"><span data-stu-id="945a5-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="945a5-202">En la clase **ContactController** , seleccione la palabra **cadena** en la definición de método del método **Get** y escriba la palabra *Contact*.</span><span class="sxs-lookup"><span data-stu-id="945a5-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="945a5-203">Una vez escrita la palabra, aparecerá un indicador al principio de la palabra **contacto**.</span><span class="sxs-lookup"><span data-stu-id="945a5-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="945a5-204">Mantenga presionada la tecla **Ctrl** y presione la tecla punto (.) o haga clic en el icono con el mouse para abrir el cuadro de diálogo de asistencia en el editor de código y rellene automáticamente la directiva **using** para el espacio de nombres Models.</span><span class="sxs-lookup"><span data-stu-id="945a5-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Usar la asistencia de IntelliSense para las declaraciones de espacio de nombres](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="945a5-206">*Usar la asistencia de IntelliSense para las declaraciones de espacio de nombres*</span><span class="sxs-lookup"><span data-stu-id="945a5-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="945a5-207">Modifique el código para el método **Get** para que devuelva una matriz de instancias de modelo de contacto.</span><span class="sxs-lookup"><span data-stu-id="945a5-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="945a5-208">(Fragmento de código: *API Web Lab-EX01: devolver una lista de contactos*)</span><span class="sxs-lookup"><span data-stu-id="945a5-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="945a5-209">Presione **F5** para depurar la aplicación web en el explorador.</span><span class="sxs-lookup"><span data-stu-id="945a5-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="945a5-210">Para ver los cambios realizados en la salida de respuesta de la API, realice los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="945a5-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="945a5-211">Una vez que se abra el explorador, presione **F12** si las herramientas de desarrollo no están abiertas todavía.</span><span class="sxs-lookup"><span data-stu-id="945a5-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="945a5-212">Haga clic en la pestaña **red** .</span><span class="sxs-lookup"><span data-stu-id="945a5-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="945a5-213">Presione el botón **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="945a5-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="945a5-214">Agregue el sufijo de dirección URL **/API/Contact** a la dirección URL en la barra de direcciones y presione la tecla **entrar** .</span><span class="sxs-lookup"><span data-stu-id="945a5-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="945a5-215">Presione el botón **ir a vista detallada** .</span><span class="sxs-lookup"><span data-stu-id="945a5-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="945a5-216">Seleccione la pestaña **cuerpo de respuesta** . Debería ver una cadena JSON que representa el formato serializado de una matriz de instancias de contacto.</span><span class="sxs-lookup"><span data-stu-id="945a5-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="945a5-217">![Salida serializada de JSON de una llamada de método de API Web compleja](build-restful-apis-with-aspnet-web-api/_static/image13.png "Salida serializada de JSON de una llamada de método de API Web compleja")</span><span class="sxs-lookup"><span data-stu-id="945a5-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="945a5-218">*Salida serializada de JSON de una llamada de método de API Web compleja*</span><span class="sxs-lookup"><span data-stu-id="945a5-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="945a5-219">Tarea 4: extracción de la funcionalidad en un nivel de servicio</span><span class="sxs-lookup"><span data-stu-id="945a5-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="945a5-220">Esta tarea mostrará cómo extraer funcionalidad en un nivel de servicio para facilitar a los desarrolladores la separación de su funcionalidad de servicio del nivel de controlador, lo que permite la reutilización de los servicios que realmente realizan el trabajo.</span><span class="sxs-lookup"><span data-stu-id="945a5-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="945a5-221">Cree una nueva carpeta en la raíz de la solución y asígnele el nombre **servicios**.</span><span class="sxs-lookup"><span data-stu-id="945a5-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="945a5-222">Para ello, haga clic con el botón derecho en proyecto **ContactManager** , seleccione **Agregar** | **nueva carpeta**, asígnele el nombre *servicios*.</span><span class="sxs-lookup"><span data-stu-id="945a5-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="945a5-223">![Creando carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creando carpeta de servicios")</span><span class="sxs-lookup"><span data-stu-id="945a5-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="945a5-224">*Creando carpeta de servicios*</span><span class="sxs-lookup"><span data-stu-id="945a5-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="945a5-225">Haga clic con el botón derecho en la carpeta **servicios** y seleccione **Agregar | Clase...** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="945a5-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="945a5-226">![Agregar una nueva clase a la carpeta servicios](build-restful-apis-with-aspnet-web-api/_static/image15.png "Agregar una nueva clase a la carpeta servicios")</span><span class="sxs-lookup"><span data-stu-id="945a5-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="945a5-227">*Agregar una nueva clase a la carpeta servicios*</span><span class="sxs-lookup"><span data-stu-id="945a5-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="945a5-228">Cuando aparezca el cuadro de diálogo **Agregar nuevo elemento** , asigne a la nueva clase el nombre **ContactRepository** y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="945a5-229">![Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos](build-restful-apis-with-aspnet-web-api/_static/image16.png "Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos")</span><span class="sxs-lookup"><span data-stu-id="945a5-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="945a5-230">*Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos*</span><span class="sxs-lookup"><span data-stu-id="945a5-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="945a5-231">Agregue una directiva using al archivo **ContactRepository.CS** para incluir el espacio de nombres de los modelos.</span><span class="sxs-lookup"><span data-stu-id="945a5-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="945a5-232">Agregue el siguiente código resaltado al archivo **ContactRepository.CS** para implementar el método GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="945a5-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="945a5-233">(Fragmento de código- *Web API Lab-EX01-repositorio del contacto*)</span><span class="sxs-lookup"><span data-stu-id="945a5-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="945a5-234">Abra el archivo **ContactController.CS** si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="945a5-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="945a5-235">Agregue la siguiente instrucción using a la sección de declaración de espacio de nombres del archivo.</span><span class="sxs-lookup"><span data-stu-id="945a5-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="945a5-236">Agregue el siguiente código resaltado a la clase **ContactController.CS** para agregar un campo privado que represente la instancia del repositorio, de modo que el resto de los miembros de clase pueda usar la implementación del servicio.</span><span class="sxs-lookup"><span data-stu-id="945a5-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="945a5-237">(Fragmento de código- *Web API Lab-EX01-Contact Controller*)</span><span class="sxs-lookup"><span data-stu-id="945a5-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="945a5-238">Cambie el método **Get** para que use el servicio de repositorio de contactos.</span><span class="sxs-lookup"><span data-stu-id="945a5-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="945a5-239">(Fragmento de código: *API Web Lab-EX01: devolver una lista de contactos a través del repositorio*)</span><span class="sxs-lookup"><span data-stu-id="945a5-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="945a5-240">Coloque un punto de interrupción en la definición del método **Get** de **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="945a5-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="945a5-241">![Agregar puntos de interrupción al controlador de contacto](build-restful-apis-with-aspnet-web-api/_static/image17.png "Agregar puntos de interrupción al controlador de contacto")</span><span class="sxs-lookup"><span data-stu-id="945a5-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="945a5-242">*Agregar puntos de interrupción al controlador de contacto*</span><span class="sxs-lookup"><span data-stu-id="945a5-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="945a5-243">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="945a5-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="945a5-244">Cuando se abra el explorador, presione **F12** para abrir las herramientas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="945a5-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="945a5-245">Haga clic en la pestaña **red** .</span><span class="sxs-lookup"><span data-stu-id="945a5-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="945a5-246">Haga clic en el botón **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="945a5-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="945a5-247">Anexe la dirección URL en la barra de direcciones con el sufijo **/API/Contact** y presione **entrar** para cargar el controlador de API.</span><span class="sxs-lookup"><span data-stu-id="945a5-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="945a5-248">Visual Studio 2012 debería interrumpir una vez que el método **Get** comience la ejecución.</span><span class="sxs-lookup"><span data-stu-id="945a5-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="945a5-249">![Interrumpir dentro del método get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Interrumpir dentro del método get")</span><span class="sxs-lookup"><span data-stu-id="945a5-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="945a5-250">*Interrumpir dentro del método get*</span><span class="sxs-lookup"><span data-stu-id="945a5-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="945a5-251">Presione **F5** para continuar.</span><span class="sxs-lookup"><span data-stu-id="945a5-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="945a5-252">Vuelva a Internet Explorer si aún no está en el foco.</span><span class="sxs-lookup"><span data-stu-id="945a5-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="945a5-253">Observe la ventana de captura de red.</span><span class="sxs-lookup"><span data-stu-id="945a5-253">Note the network capture window.</span></span>

    <span data-ttu-id="945a5-254">![Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="945a5-255">*Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="945a5-256">Haga clic en el botón **ir a vista detallada** .</span><span class="sxs-lookup"><span data-stu-id="945a5-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="945a5-257">Haga clic en la pestaña **cuerpo de respuesta** . tenga en cuenta la salida JSON de la llamada de API y cómo representa los dos contactos recuperados por el nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="945a5-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="945a5-258">![Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo](build-restful-apis-with-aspnet-web-api/_static/image20.png "Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo")</span><span class="sxs-lookup"><span data-stu-id="945a5-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="945a5-259">*Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo*</span><span class="sxs-lookup"><span data-stu-id="945a5-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="945a5-260">Ejercicio 2: creación de una API Web de lectura/escritura</span><span class="sxs-lookup"><span data-stu-id="945a5-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="945a5-261">En este ejercicio, implementará los métodos POST y PUT para el administrador de contactos con el fin de habilitarlo con características de edición de datos.</span><span class="sxs-lookup"><span data-stu-id="945a5-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="945a5-262">Tarea 1: apertura del proyecto de API Web</span><span class="sxs-lookup"><span data-stu-id="945a5-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="945a5-263">En esta tarea, preparará la mejora del proyecto de API Web creado en el ejercicio 1 para que pueda aceptar datos proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="945a5-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="945a5-264">Ejecute **Visual Studio 2012 Express para web**. para ello, vaya a **inicio** y escriba **vs Express para web** Presione **entrar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="945a5-265">Abra la carpeta **Begin** Solution ubicada en **source/Ex02-ReadWriteWebAPI/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="945a5-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="945a5-266">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="945a5-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="945a5-267">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="945a5-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="945a5-268">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="945a5-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="945a5-269">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="945a5-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="945a5-270">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="945a5-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="945a5-271">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="945a5-272">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="945a5-273">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="945a5-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="945a5-274">Abra el archivo **Services/ContactRepository. CS** .</span><span class="sxs-lookup"><span data-stu-id="945a5-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="945a5-275">Tarea 2: agregar características de persistencia de datos a la implementación del repositorio de contactos</span><span class="sxs-lookup"><span data-stu-id="945a5-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="945a5-276">En esta tarea, ampliará la clase ContactRepository del proyecto de API Web creado en el ejercicio 1 para que pueda conservar y aceptar la entrada del usuario y las nuevas instancias de contacto.</span><span class="sxs-lookup"><span data-stu-id="945a5-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="945a5-277">Agregue la siguiente constante a la clase **ContactRepository** para representar el nombre del nombre de la clave del elemento de la caché del servidor Web más adelante en este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="945a5-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="945a5-278">Agregue un constructor a **ContactRepository** que contenga el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="945a5-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="945a5-279">(Fragmento de código- *Web API Lab-Ex02-constructor de repositorio de contactos*)</span><span class="sxs-lookup"><span data-stu-id="945a5-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="945a5-280">Modifique el código para el método **GetAllContacts** como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="945a5-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="945a5-281">(Fragmento de código- *Web API Lab-Ex02-obtener todos los contactos*)</span><span class="sxs-lookup"><span data-stu-id="945a5-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="945a5-282">Este ejemplo se usa para fines de demostración y utilizará la memoria caché del servidor web como medio de almacenamiento, de modo que los valores estarán disponibles para varios clientes simultáneamente, en lugar de usar un mecanismo de almacenamiento de sesión o una duración de almacenamiento de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="945a5-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="945a5-283">Puede usar Entity Framework, el almacenamiento XML o cualquier otra variedad en lugar de la memoria caché del servidor Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="945a5-284">Implemente un nuevo método denominado **SaveContact** en la clase **ContactRepository** para realizar el trabajo de guardar un contacto.</span><span class="sxs-lookup"><span data-stu-id="945a5-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="945a5-285">El método **SaveContact** debe tomar un único parámetro **Contact** y devolver un valor booleano que indique si se ha realizado correctamente o no.</span><span class="sxs-lookup"><span data-stu-id="945a5-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="945a5-286">(Fragmento de código- *Web API Lab-Ex02-implementando el método SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="945a5-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="945a5-287">Ejercicio 3: usar la API Web desde un cliente HTML</span><span class="sxs-lookup"><span data-stu-id="945a5-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="945a5-288">En este ejercicio, creará un cliente HTML para llamar a la API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="945a5-289">Este cliente facilitará el intercambio de datos con la API Web mediante JavaScript y mostrará los resultados en un explorador Web mediante el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="945a5-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="945a5-290">Tarea 1: modificar la vista de índice para proporcionar una GUI para mostrar contactos</span><span class="sxs-lookup"><span data-stu-id="945a5-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="945a5-291">En esta tarea, modificará la vista de índice predeterminada de la aplicación web para admitir el requisito de mostrar la lista de contactos existentes en un explorador HTML.</span><span class="sxs-lookup"><span data-stu-id="945a5-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="945a5-292">Abra **Visual Studio 2012 Express para web** si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="945a5-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="945a5-293">Abra la carpeta **Begin** Solution ubicada en **source/Ex03-ConsumingWebAPI/Begin/** .</span><span class="sxs-lookup"><span data-stu-id="945a5-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="945a5-294">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="945a5-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="945a5-295">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="945a5-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="945a5-296">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="945a5-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="945a5-297">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="945a5-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="945a5-298">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="945a5-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="945a5-299">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="945a5-300">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="945a5-301">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="945a5-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="945a5-302">Abra el archivo **index. cshtml** que se encuentra en la carpeta **views/Home** .</span><span class="sxs-lookup"><span data-stu-id="945a5-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="945a5-303">Reemplace el código HTML dentro del elemento div con el **cuerpo** del identificador para que sea similar al código siguiente.</span><span class="sxs-lookup"><span data-stu-id="945a5-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="945a5-304">Agregue el siguiente código JavaScript en la parte inferior del archivo para realizar la solicitud HTTP a la API Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="945a5-305">Abra el archivo **ContactController.CS** si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="945a5-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="945a5-306">Coloque un punto de interrupción en el método **Get** de la clase **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="945a5-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="945a5-307">![Colocar un punto de interrupción en el método get del controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Colocar un punto de interrupción en el método get del controlador de API")</span><span class="sxs-lookup"><span data-stu-id="945a5-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="945a5-308">*Colocar un punto de interrupción en el método get del controlador de API*</span><span class="sxs-lookup"><span data-stu-id="945a5-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="945a5-309">Presione **F5** para ejecutar el proyecto.</span><span class="sxs-lookup"><span data-stu-id="945a5-309">Press **F5** to run the project.</span></span> <span data-ttu-id="945a5-310">El explorador cargará el documento HTML.</span><span class="sxs-lookup"><span data-stu-id="945a5-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-311">Asegúrese de que está explorando la dirección URL raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="945a5-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="945a5-312">Una vez que se carga la página y se ejecuta JavaScript, se alcanza el punto de interrupción y la ejecución del código se pausará en el controlador.</span><span class="sxs-lookup"><span data-stu-id="945a5-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="945a5-313">![Depurar en las llamadas a la API Web mediante VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Depurar en las llamadas a la API Web mediante VS Express para Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="945a5-314">*Depuración en la llamada de la API Web con Visual Studio 2012 Express for Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="945a5-315">Quite el punto de interrupción y presione **F5** o el botón **continuar** de la barra de herramientas de depuración para continuar cargando la vista en el explorador.</span><span class="sxs-lookup"><span data-stu-id="945a5-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="945a5-316">Una vez que se complete la llamada a la API Web, debería ver los contactos devueltos por la llamada a la API Web que se muestra como elementos de lista en el explorador.</span><span class="sxs-lookup"><span data-stu-id="945a5-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="945a5-317">![Resultados de la llamada API mostrada en el explorador como elementos de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "Resultados de la llamada API mostrada en el explorador como elementos de lista")</span><span class="sxs-lookup"><span data-stu-id="945a5-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="945a5-318">*Resultados de la llamada API mostrada en el explorador como elementos de lista*</span><span class="sxs-lookup"><span data-stu-id="945a5-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="945a5-319">Detenga la depuración.</span><span class="sxs-lookup"><span data-stu-id="945a5-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="945a5-320">Tarea 2: modificación de la vista de índice para proporcionar una GUI para crear contactos</span><span class="sxs-lookup"><span data-stu-id="945a5-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="945a5-321">En esta tarea, seguirá modificando la vista de índice de la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="945a5-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="945a5-322">Se agregará un formulario a la página HTML que capturará los datos proporcionados por el usuario y los enviará a la API Web para crear un nuevo contacto, y se creará un nuevo método de controlador de API Web para recopilar la fecha de la GUI.</span><span class="sxs-lookup"><span data-stu-id="945a5-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="945a5-323">Abra el archivo **ContactController.CS** .</span><span class="sxs-lookup"><span data-stu-id="945a5-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="945a5-324">Agregue un nuevo método a la clase de controlador denominada **post** tal y como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="945a5-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="945a5-325">(Fragmento de código- *Web API Lab-Ex03-post (método*))</span><span class="sxs-lookup"><span data-stu-id="945a5-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="945a5-326">Abra el archivo **index. cshtml** en Visual Studio si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="945a5-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="945a5-327">Agregue el código HTML siguiente al archivo justo después de la lista sin ordenar que agregó en la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="945a5-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="945a5-328">En el elemento script situado en la parte inferior del documento, agregue el siguiente código resaltado para controlar los eventos de clic de botón, que publicarán los datos en la API Web mediante una llamada HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="945a5-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="945a5-329">En **ContactController.CS**, coloque un punto de interrupción en el método **post** .</span><span class="sxs-lookup"><span data-stu-id="945a5-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="945a5-330">Presione **F5** para ejecutar la aplicación en el explorador.</span><span class="sxs-lookup"><span data-stu-id="945a5-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="945a5-331">Una vez que la página se cargue en el explorador, escriba un nuevo nombre y un identificador de contacto y haga clic en el botón **Guardar** .</span><span class="sxs-lookup"><span data-stu-id="945a5-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="945a5-332">![Documento HTML del cliente cargado en el explorador](build-restful-apis-with-aspnet-web-api/_static/image24.png "Documento HTML del cliente cargado en el explorador")</span><span class="sxs-lookup"><span data-stu-id="945a5-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="945a5-333">*Documento HTML del cliente cargado en el explorador*</span><span class="sxs-lookup"><span data-stu-id="945a5-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="945a5-334">Cuando la ventana del depurador se interrumpa en el método **post** , eche un vistazo a las propiedades del parámetro **Contact** .</span><span class="sxs-lookup"><span data-stu-id="945a5-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="945a5-335">Los valores deben coincidir con los datos que especificó en el formulario.</span><span class="sxs-lookup"><span data-stu-id="945a5-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="945a5-336">![El objeto de contacto que se envía a la API Web desde el cliente.](build-restful-apis-with-aspnet-web-api/_static/image25.png "El objeto de contacto que se envía a la API Web desde el cliente.")</span><span class="sxs-lookup"><span data-stu-id="945a5-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="945a5-337">*El objeto de contacto que se envía a la API Web desde el cliente.*</span><span class="sxs-lookup"><span data-stu-id="945a5-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="945a5-338">Recorra el método en el depurador hasta que se haya creado la variable de **respuesta** .</span><span class="sxs-lookup"><span data-stu-id="945a5-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="945a5-339">Al realizar la inspección en la ventana **variables locales** del depurador, verá que se han establecido todas las propiedades.</span><span class="sxs-lookup"><span data-stu-id="945a5-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="945a5-340">![La respuesta siguiente a la creación en el depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "La respuesta siguiente a la creación en el depurador")</span><span class="sxs-lookup"><span data-stu-id="945a5-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="945a5-341">*La respuesta siguiente a la creación en el depurador*</span><span class="sxs-lookup"><span data-stu-id="945a5-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="945a5-342">Si presiona **F5** o hace clic en **continuar** en el depurador, la solicitud se completará.</span><span class="sxs-lookup"><span data-stu-id="945a5-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="945a5-343">Una vez que vuelva al explorador, el nuevo contacto se ha agregado a la lista de contactos almacenados por la implementación de **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="945a5-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="945a5-344">![El explorador refleja la creación correcta de la nueva instancia de contacto](build-restful-apis-with-aspnet-web-api/_static/image27.png "El explorador refleja la creación correcta de la nueva instancia de contacto")</span><span class="sxs-lookup"><span data-stu-id="945a5-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="945a5-345">*El explorador refleja la creación correcta de la nueva instancia de contacto*</span><span class="sxs-lookup"><span data-stu-id="945a5-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="945a5-346">Además, puede implementar esta aplicación en Azure después del [Apéndice C: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="945a5-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="945a5-347">Resumen</span><span class="sxs-lookup"><span data-stu-id="945a5-347">Summary</span></span>

<span data-ttu-id="945a5-348">Este laboratorio le ha incorporado el nuevo marco de ASP.NET Web API y la implementación de las API Web de RESTful mediante el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="945a5-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="945a5-349">Desde aquí, puede crear un nuevo repositorio que facilite la persistencia de los datos mediante cualquier número de mecanismos y conecte el servicio en lugar del simple proporcionado como ejemplo en este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="945a5-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="945a5-350">Web API admite varias características adicionales, como la habilitación de la comunicación desde clientes no HTML escritos en cualquier lenguaje que admita HTTP y JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="945a5-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="945a5-351">También es posible hospedar una API Web fuera de una aplicación web típica, así como la capacidad de crear sus propios formatos de serialización.</span><span class="sxs-lookup"><span data-stu-id="945a5-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="945a5-352">El sitio web de ASP.NET tiene un área dedicada al marco de ASP.NET Web API en [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="945a5-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="945a5-353">Este sitio seguirá proporcionando información de última hora, ejemplos y noticias relacionados con Web API, por lo que debe comprobarlo con frecuencia si desea profundizar más en el arte de crear API Web personalizadas disponibles para prácticamente cualquier dispositivo o marco de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="945a5-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="945a5-354">Apéndice A: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="945a5-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="945a5-355">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="945a5-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="945a5-356">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="945a5-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="945a5-357">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="945a5-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="945a5-358">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="945a5-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="945a5-359">Para agregar un fragmento de código mediante el tecladoC# (solo)</span><span class="sxs-lookup"><span data-stu-id="945a5-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="945a5-360">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="945a5-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="945a5-361">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="945a5-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="945a5-362">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="945a5-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="945a5-363">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="945a5-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="945a5-364">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="945a5-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="945a5-365">![Comience a escribir el nombre del fragmento de código.](build-restful-apis-with-aspnet-web-api/_static/image29.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="945a5-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="945a5-366">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="945a5-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="945a5-367">![Presione TAB para seleccionar el fragmento de código resaltado](build-restful-apis-with-aspnet-web-api/_static/image30.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="945a5-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="945a5-368">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="945a5-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="945a5-369">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](build-restful-apis-with-aspnet-web-api/_static/image31.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="945a5-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="945a5-370">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="945a5-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="945a5-371">Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)</span><span class="sxs-lookup"><span data-stu-id="945a5-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="945a5-372">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="945a5-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="945a5-373">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="945a5-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="945a5-374">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="945a5-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="945a5-375">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](build-restful-apis-with-aspnet-web-api/_static/image32.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="945a5-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="945a5-376">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="945a5-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="945a5-377">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](build-restful-apis-with-aspnet-web-api/_static/image33.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="945a5-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="945a5-378">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="945a5-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="945a5-379">Apéndice B: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="945a5-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="945a5-380">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="945a5-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="945a5-381">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="945a5-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="945a5-382">Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="945a5-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="945a5-383">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="945a5-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="945a5-384">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="945a5-384">Click on **Install Now**.</span></span> <span data-ttu-id="945a5-385">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="945a5-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="945a5-386">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="945a5-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="945a5-387">![Instalar Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="945a5-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="945a5-388">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="945a5-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="945a5-389">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="945a5-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="945a5-391">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="945a5-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="945a5-392">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="945a5-392">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="945a5-394">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="945a5-394">*Installation progress*</span></span>
6. <span data-ttu-id="945a5-395">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-395">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="945a5-397">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="945a5-397">*Installation completed*</span></span>
7. <span data-ttu-id="945a5-398">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="945a5-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="945a5-399">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="945a5-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="945a5-401">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="945a5-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="945a5-402">Apéndice C: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="945a5-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="945a5-403">Este apéndice le mostrará cómo crear un nuevo sitio web desde Azure portal y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Azure.</span><span class="sxs-lookup"><span data-stu-id="945a5-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="945a5-404">Tarea 1: creación de un nuevo sitio web desde Azure portal</span><span class="sxs-lookup"><span data-stu-id="945a5-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="945a5-405">Vaya a [Azure portal de administración](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.</span><span class="sxs-lookup"><span data-stu-id="945a5-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-406">Con Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico.</span><span class="sxs-lookup"><span data-stu-id="945a5-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="945a5-407">Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="945a5-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="945a5-408">![Iniciar sesión en Windows Azure Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Iniciar sesión en Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="945a5-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="945a5-409">*Iniciar sesión en el portal*</span><span class="sxs-lookup"><span data-stu-id="945a5-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="945a5-410">Haga clic en **nuevo** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="945a5-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="945a5-411">![Crear un nuevo sitio web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Crear un nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="945a5-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="945a5-412">*Crear un nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="945a5-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="945a5-413">Haga clic en **compute** | **sitio web**.</span><span class="sxs-lookup"><span data-stu-id="945a5-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="945a5-414">A continuación, seleccione la opción **creación rápida** .</span><span class="sxs-lookup"><span data-stu-id="945a5-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="945a5-415">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.</span><span class="sxs-lookup"><span data-stu-id="945a5-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-416">Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="945a5-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="945a5-417">La opción creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="945a5-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="945a5-418">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="945a5-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="945a5-419">![Crear un nuevo sitio web mediante creación rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Crear un nuevo sitio web mediante creación rápida")</span><span class="sxs-lookup"><span data-stu-id="945a5-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="945a5-420">*Crear un nuevo sitio web mediante creación rápida*</span><span class="sxs-lookup"><span data-stu-id="945a5-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="945a5-421">Espere hasta que se cree el nuevo **sitio web** .</span><span class="sxs-lookup"><span data-stu-id="945a5-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="945a5-422">Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** .</span><span class="sxs-lookup"><span data-stu-id="945a5-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="945a5-423">Compruebe que el nuevo sitio web funciona.</span><span class="sxs-lookup"><span data-stu-id="945a5-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="945a5-424">![Ir al nuevo sitio web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Ir al nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="945a5-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="945a5-425">*Ir al nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="945a5-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="945a5-426">![Sitio web en ejecución](build-restful-apis-with-aspnet-web-api/_static/image43.png "Sitio web en ejecución")</span><span class="sxs-lookup"><span data-stu-id="945a5-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="945a5-427">*Sitio web en ejecución*</span><span class="sxs-lookup"><span data-stu-id="945a5-427">*Web site running*</span></span>
6. <span data-ttu-id="945a5-428">Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="945a5-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="945a5-429">![Abrir las páginas de administración de sitios web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Abrir las páginas de administración de sitios web")</span><span class="sxs-lookup"><span data-stu-id="945a5-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="945a5-430">*Abrir las páginas de administración de sitios web*</span><span class="sxs-lookup"><span data-stu-id="945a5-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="945a5-431">En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .</span><span class="sxs-lookup"><span data-stu-id="945a5-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-432">El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="945a5-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="945a5-433">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos.</span><span class="sxs-lookup"><span data-stu-id="945a5-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="945a5-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en Azure.</span><span class="sxs-lookup"><span data-stu-id="945a5-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="945a5-435">![Descargar el perfil de publicación del sitio web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Descargar el perfil de publicación del sitio web")</span><span class="sxs-lookup"><span data-stu-id="945a5-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="945a5-436">*Descargar el perfil de publicación del sitio web*</span><span class="sxs-lookup"><span data-stu-id="945a5-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="945a5-437">Descargue el archivo de Perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="945a5-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="945a5-438">Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="945a5-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="945a5-439">![Guardar el archivo de Perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image46.png "Guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="945a5-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="945a5-440">*Guardar el archivo de Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="945a5-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="945a5-441">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="945a5-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="945a5-442">Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="945a5-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="945a5-443">Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="945a5-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="945a5-444">Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="945a5-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="945a5-445">Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**.</span><span class="sxs-lookup"><span data-stu-id="945a5-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="945a5-446">Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="945a5-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="945a5-447">Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="945a5-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="945a5-448">No cree todavía la base de datos, ya que se creará en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="945a5-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="945a5-449">![Panel de SQL Database Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "Panel de SQL Database Server")</span><span class="sxs-lookup"><span data-stu-id="945a5-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="945a5-450">*Panel de SQL Database Server*</span><span class="sxs-lookup"><span data-stu-id="945a5-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="945a5-451">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor.</span><span class="sxs-lookup"><span data-stu-id="945a5-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="945a5-452">Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](build-restful-apis-with-aspnet-web-api/_static/image48.png).</span><span class="sxs-lookup"><span data-stu-id="945a5-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Agregando la dirección IP del cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="945a5-454">*Agregando la dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="945a5-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="945a5-455">Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="945a5-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="945a5-457">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="945a5-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="945a5-458">Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="945a5-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="945a5-459">Vuelva a la solución ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="945a5-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="945a5-460">En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="945a5-461">![Publicar la aplicación](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="945a5-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="945a5-462">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="945a5-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="945a5-463">Importe el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="945a5-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="945a5-464">![Importar el perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="945a5-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="945a5-465">*Importando Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="945a5-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="945a5-466">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="945a5-466">Click **Validate Connection**.</span></span> <span data-ttu-id="945a5-467">Una vez finalizada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="945a5-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="945a5-468">La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.</span><span class="sxs-lookup"><span data-stu-id="945a5-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="945a5-469">![Validando conexión](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validando conexión")</span><span class="sxs-lookup"><span data-stu-id="945a5-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="945a5-470">*Validando conexión*</span><span class="sxs-lookup"><span data-stu-id="945a5-470">*Validating connection*</span></span>
4. <span data-ttu-id="945a5-471">En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="945a5-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="945a5-472">![Configuración de web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuración de web deploy")</span><span class="sxs-lookup"><span data-stu-id="945a5-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="945a5-473">*Configuración de web deploy*</span><span class="sxs-lookup"><span data-stu-id="945a5-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="945a5-474">Configure la conexión de base de datos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="945a5-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="945a5-475">En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="945a5-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="945a5-476">En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="945a5-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="945a5-477">En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="945a5-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="945a5-478">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="945a5-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="945a5-479">![Configuración de la cadena de conexión de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="945a5-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="945a5-480">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="945a5-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="945a5-481">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-481">Then click **OK**.</span></span> <span data-ttu-id="945a5-482">Cuando se le pida que cree la base de datos, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="945a5-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="945a5-483">![Crear la base de datos](build-restful-apis-with-aspnet-web-api/_static/image56.png "Crear la cadena de base de datos")</span><span class="sxs-lookup"><span data-stu-id="945a5-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="945a5-484">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="945a5-484">*Creating the database*</span></span>
7. <span data-ttu-id="945a5-485">La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada.</span><span class="sxs-lookup"><span data-stu-id="945a5-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="945a5-486">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="945a5-486">Then click **Next**.</span></span>

    <span data-ttu-id="945a5-487">![Cadena de conexión que apunta a SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Cadena de conexión que apunta a SQL Database")</span><span class="sxs-lookup"><span data-stu-id="945a5-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="945a5-488">*Cadena de conexión que apunta a SQL Database*</span><span class="sxs-lookup"><span data-stu-id="945a5-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="945a5-489">En la página **vista previa** , haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="945a5-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="945a5-490">![Publicación de la aplicación Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publicación de la aplicación Web")</span><span class="sxs-lookup"><span data-stu-id="945a5-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="945a5-491">*Publicación de la aplicación Web*</span><span class="sxs-lookup"><span data-stu-id="945a5-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="945a5-492">Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.</span><span class="sxs-lookup"><span data-stu-id="945a5-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="945a5-493">![Aplicación publicada en Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplicación publicada en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="945a5-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="945a5-494">*Aplicación publicada en Azure*</span><span class="sxs-lookup"><span data-stu-id="945a5-494">*Application published to Azure*</span></span>
