---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de acción personalizada de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC proporciona filtros de acción para ejecutar la lógica de filtrado antes o después de llamar a un método de acción. Los filtros de acción son atributos personalizados...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468181"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="b8d78-104">Filtros de acción personalizada de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b8d78-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="b8d78-105">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b8d78-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b8d78-106">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="b8d78-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b8d78-107">ASP.NET MVC proporciona filtros de acción para ejecutar la lógica de filtrado antes o después de llamar a un método de acción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="b8d78-108">Los filtros de acción son atributos personalizados que proporcionan medios declarativos para agregar el comportamiento de acciones previas y posteriores a los métodos de acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="b8d78-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="b8d78-109">En este laboratorio práctico, creará un atributo de filtro de acción personalizado en la solución MvcMusicStore para detectar las solicitudes del controlador y registrar la actividad de un sitio en una tabla de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="b8d78-110">Podrá agregar el filtro de registro mediante la inyección a cualquier controlador o acción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="b8d78-111">Por último, verá la vista de registro que muestra la lista de visitantes.</span><span class="sxs-lookup"><span data-stu-id="b8d78-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="b8d78-112">Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="b8d78-113">Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="b8d78-114">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b8d78-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b8d78-115">El proyecto específico de este laboratorio está disponible en [filtros de acción personalizada de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="b8d78-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b8d78-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="b8d78-116">Objectives</span></span>

<span data-ttu-id="b8d78-117">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="b8d78-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b8d78-118">Crear un atributo de filtro de acción personalizado para extender las capacidades de filtrado</span><span class="sxs-lookup"><span data-stu-id="b8d78-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="b8d78-119">Aplicar un atributo de filtro personalizado mediante inyección a un nivel específico</span><span class="sxs-lookup"><span data-stu-id="b8d78-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="b8d78-120">Registrar un filtro de acción personalizado globalmente</span><span class="sxs-lookup"><span data-stu-id="b8d78-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b8d78-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b8d78-121">Prerequisites</span></span>

<span data-ttu-id="b8d78-122">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="b8d78-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b8d78-123">[Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).</span><span class="sxs-lookup"><span data-stu-id="b8d78-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b8d78-124">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="b8d78-124">Setup</span></span>

<span data-ttu-id="b8d78-125">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="b8d78-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="b8d78-126">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8d78-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b8d78-127">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="b8d78-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b8d78-128">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b8d78-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b8d78-129">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="b8d78-129">Exercises</span></span>

<span data-ttu-id="b8d78-130">Este laboratorio práctico se compone de los siguientes ejercicios:</span><span class="sxs-lookup"><span data-stu-id="b8d78-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b8d78-131">Ejercicio 1: registro de acciones</span><span class="sxs-lookup"><span data-stu-id="b8d78-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="b8d78-132">Ejercicio 2: administración de varios filtros de acción</span><span class="sxs-lookup"><span data-stu-id="b8d78-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="b8d78-133">Tiempo estimado para completar este laboratorio: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b8d78-134">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="b8d78-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b8d78-135">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="b8d78-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="b8d78-136">Ejercicio 1: registro de acciones</span><span class="sxs-lookup"><span data-stu-id="b8d78-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="b8d78-137">En este ejercicio, obtendrá información sobre cómo crear un filtro de registro de acciones personalizado mediante el uso de proveedores de filtros de ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b8d78-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="b8d78-138">Para ello, aplicará un filtro de registro al sitio MusicStore que registrará todas las actividades en los controladores seleccionados.</span><span class="sxs-lookup"><span data-stu-id="b8d78-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="b8d78-139">El filtro extenderá **ActionFilterAttributeClass** e invalidará el método **OnActionExecuting** para que detecte cada solicitud y, a continuación, realice las acciones de registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="b8d78-140">La clase ASP.NET MVC **ActionExecutingContext** proporcionará la información de contexto sobre las solicitudes HTTP, la ejecución de métodos, resultados y parámetros **.**</span><span class="sxs-lookup"><span data-stu-id="b8d78-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="b8d78-141">ASP.NET MVC 4 también tiene proveedores de filtros predeterminados que puede usar sin crear un filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="b8d78-142">ASP.NET MVC 4 proporciona los siguientes tipos de filtros:</span><span class="sxs-lookup"><span data-stu-id="b8d78-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="b8d78-143">Filtro de **autorización** , que toma decisiones de seguridad sobre si se debe ejecutar un método de acción, como la realización de la autenticación o la validación de las propiedades de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="b8d78-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="b8d78-144">Filtro de **acción** , que incluye la ejecución del método de acción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="b8d78-145">Este filtro puede realizar un procesamiento adicional, como proporcionar datos adicionales al método de acción, inspeccionar el valor devuelto o cancelar la ejecución del método de acción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="b8d78-146">Filtro de **resultados** , que ajusta la ejecución del objeto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="b8d78-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="b8d78-147">Este filtro puede realizar un procesamiento adicional del resultado, como la modificación de la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="b8d78-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="b8d78-148">Filtro de **excepciones** , que se ejecuta si se produce una excepción no controlada en alguna parte del método de acción, empezando por los filtros de autorización y finalizando con la ejecución del resultado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="b8d78-149">Los filtros de excepciones se pueden usar para tareas como registrar o mostrar una página de error.</span><span class="sxs-lookup"><span data-stu-id="b8d78-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="b8d78-150">Para obtener más información acerca de los proveedores de filtros, visite este vínculo de MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="b8d78-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="b8d78-151">Acerca de la característica de registro de aplicaciones de almacén de música MVC</span><span class="sxs-lookup"><span data-stu-id="b8d78-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="b8d78-152">Esta solución de almacén de música tiene una nueva tabla de modelo de datos para el registro del sitio, **ActionLog**, con los siguientes campos: nombre del controlador que recibió una solicitud, denominada acción, dirección IP del cliente y marca de tiempo.</span><span class="sxs-lookup"><span data-stu-id="b8d78-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="b8d78-153">![Modelo de datos. Tabla ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de datos. Tabla ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="b8d78-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="b8d78-154">*Modelo de datos: tabla ActionLog*</span><span class="sxs-lookup"><span data-stu-id="b8d78-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="b8d78-155">La solución proporciona una vista de MVC de ASP.NET para el registro de acciones que se puede encontrar en **MvcMusicStores/views/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="b8d78-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="b8d78-156">![Vista del registro de acciones](aspnet-mvc-4-custom-action-filters/_static/image2.png "Vista del registro de acciones")</span><span class="sxs-lookup"><span data-stu-id="b8d78-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="b8d78-157">*Vista del registro de acciones*</span><span class="sxs-lookup"><span data-stu-id="b8d78-157">*Action Log view*</span></span>

<span data-ttu-id="b8d78-158">Con esta estructura determinada, todo el trabajo se centrará en la solicitud del controlador de interrupción y en la realización del registro mediante el filtrado personalizado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="b8d78-159">Tarea 1: creación de un filtro personalizado para detectar la solicitud de un controlador</span><span class="sxs-lookup"><span data-stu-id="b8d78-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="b8d78-160">En esta tarea creará una clase de atributo de filtro personalizado que contendrá la lógica de registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="b8d78-161">Para ello, extenderá ASP.NET MVC **ActionFilterAttribute** Class e implementará la interfaz **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="b8d78-162">**ActionFilterAttribute** es la clase base para todos los filtros de atributos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="b8d78-163">Proporciona los métodos siguientes para ejecutar una lógica específica después y antes de la ejecución de la acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="b8d78-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="b8d78-164">**OnActionExecuting**(ActionExecutingContext filterContext): justo antes de llamar al método de acción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="b8d78-165">**OnActionExecuted**(ActionExecutedContext filterContext): después de llamar al método de acción y antes de que se ejecute el resultado (antes de que se represente la vista).</span><span class="sxs-lookup"><span data-stu-id="b8d78-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b8d78-166">**OnResultExecuting**(ResultExecutingContext filterContext): justo antes de que se ejecute el resultado (antes de que se represente la vista).</span><span class="sxs-lookup"><span data-stu-id="b8d78-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="b8d78-167">**OnResultExecuted**(ResultExecutedContext filterContext): después de que se ejecute el resultado (después de que se represente la vista).</span><span class="sxs-lookup"><span data-stu-id="b8d78-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="b8d78-168">Al invalidar cualquiera de estos métodos en una clase derivada, puede ejecutar su propio código de filtrado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="b8d78-169">Abra la solución de **Inicio** que se encuentra en la carpeta **\Source\Ex01-LoggingActions\Begin** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="b8d78-170">Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="b8d78-171">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b8d78-172">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="b8d78-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b8d78-173">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b8d78-174">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8d78-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b8d78-175">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8d78-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b8d78-176">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="b8d78-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="b8d78-177">Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b8d78-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b8d78-178">Agregue una nueva C# clase a la carpeta **Filters** y asígnele el nombre *CustomActionFilter.CS*.</span><span class="sxs-lookup"><span data-stu-id="b8d78-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="b8d78-179">Esta carpeta almacenará todos los filtros personalizados.</span><span class="sxs-lookup"><span data-stu-id="b8d78-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="b8d78-180">Abra **CustomActionFilter.CS** y agregue una referencia a los espacios de nombres **System. Web. Mvc** y **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="b8d78-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="b8d78-181">(Fragmento de código- *ASP.NET MVC 4 Custom Action filters-EX1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b8d78-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="b8d78-182">Herede la clase **CustomActionFilter** de **ActionFilterAttribute** y, a continuación, haga que la clase **CustomActionFilter** implemente la interfaz **IActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="b8d78-183">Haga que la clase **CustomActionFilter** invalide el método **OnActionExecuting** y agregue la lógica necesaria para registrar la ejecución del filtro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="b8d78-184">Para ello, agregue el siguiente código resaltado dentro de la clase **CustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="b8d78-185">(Fragmento de código- *ASP.NET MVC 4 Custom Action filters-EX1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="b8d78-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="b8d78-186">El método **OnActionExecuting** usa **Entity Framework** para agregar un nuevo registro ActionLog.</span><span class="sxs-lookup"><span data-stu-id="b8d78-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="b8d78-187">Crea y rellena una nueva instancia de la entidad con la información de contexto de **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="b8d78-188">Puede obtener más información acerca de la clase **ControllerContext** en [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8d78-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="b8d78-189">Tarea 2: inyección de un interceptor de código en la clase de controlador de almacén</span><span class="sxs-lookup"><span data-stu-id="b8d78-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="b8d78-190">En esta tarea, agregará el filtro personalizado mediante su inserción en todas las clases de controlador y acciones de controlador que se registrarán.</span><span class="sxs-lookup"><span data-stu-id="b8d78-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="b8d78-191">Para este ejercicio, la clase de controlador de almacenamiento tendrá un registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="b8d78-192">El método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado se ejecuta cuando se llama a un elemento insertado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="b8d78-193">También es posible interceptar un método de controlador específico.</span><span class="sxs-lookup"><span data-stu-id="b8d78-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="b8d78-194">Abra **StoreController** en **MvcMusicStore\Controllers** y agregue una referencia al espacio de nombres **Filters** :</span><span class="sxs-lookup"><span data-stu-id="b8d78-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="b8d78-195">Inserte el filtro personalizado **CustomActionFilter** en la clase **StoreController** agregando el atributo **[CustomActionFilter]** antes de la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="b8d78-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="b8d78-196">Cuando se inserta un filtro en una clase de controlador, también se insertan todas sus acciones.</span><span class="sxs-lookup"><span data-stu-id="b8d78-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="b8d78-197">Si desea aplicar el filtro solo para un conjunto de acciones, tendría que insertar **[CustomActionFilter]** en cada uno de ellos:</span><span class="sxs-lookup"><span data-stu-id="b8d78-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b8d78-198">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b8d78-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="b8d78-199">En esta tarea, probará que el filtro de registro funciona.</span><span class="sxs-lookup"><span data-stu-id="b8d78-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="b8d78-200">Iniciará la aplicación y visitará la tienda y, a continuación, comprobará las actividades registradas.</span><span class="sxs-lookup"><span data-stu-id="b8d78-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="b8d78-201">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="b8d78-202">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro:</span><span class="sxs-lookup"><span data-stu-id="b8d78-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="b8d78-203">![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image3.png "Estado del seguimiento de registro antes de la actividad de la página")</span><span class="sxs-lookup"><span data-stu-id="b8d78-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8d78-204">*Estado del seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="b8d78-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="b8d78-205">De forma predeterminada, siempre mostrará un elemento que se genera al recuperar los géneros existentes para el menú.</span><span class="sxs-lookup"><span data-stu-id="b8d78-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="b8d78-206">Por motivos de simplicidad, limpiaremos la tabla **ActionLog** cada vez que se ejecute la aplicación, de modo que solo se mostrarán los registros de la comprobación de cada tarea determinada.</span><span class="sxs-lookup"><span data-stu-id="b8d78-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="b8d78-207">Es posible que tenga que quitar el siguiente código de la **sesión\_método Start** (en la clase **global. asax** ) para guardar un registro histórico para todas las acciones ejecutadas en el controlador de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="b8d78-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="b8d78-208">Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="b8d78-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="b8d78-209">Vaya a **/ActionLog** y, si el registro está vacío, presione **F5** para actualizar la página.</span><span class="sxs-lookup"><span data-stu-id="b8d78-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="b8d78-210">Compruebe que se ha realizado el seguimiento de las visitas:</span><span class="sxs-lookup"><span data-stu-id="b8d78-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="b8d78-211">![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image4.png "Registro de acciones con actividad registrada")</span><span class="sxs-lookup"><span data-stu-id="b8d78-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8d78-212">*Registro de acciones con actividad registrada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="b8d78-213">Ejercicio 2: administración de varios filtros de acción</span><span class="sxs-lookup"><span data-stu-id="b8d78-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="b8d78-214">En este ejercicio, agregará un segundo filtro de acción personalizada a la clase StoreController y definirá el orden específico en el que se ejecutarán ambos filtros.</span><span class="sxs-lookup"><span data-stu-id="b8d78-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="b8d78-215">A continuación, actualizará el código para registrar el filtro globalmente.</span><span class="sxs-lookup"><span data-stu-id="b8d78-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="b8d78-216">Hay diferentes opciones que hay que tener en cuenta al definir el orden de ejecución de los filtros.</span><span class="sxs-lookup"><span data-stu-id="b8d78-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="b8d78-217">Por ejemplo, la propiedad order y el ámbito filters:</span><span class="sxs-lookup"><span data-stu-id="b8d78-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="b8d78-218">Puede definir un **ámbito** para cada uno de los filtros; por ejemplo, puede establecer el ámbito de todos los filtros de acción para que se ejecuten en el **ámbito del controlador**y todos los filtros de autorización para ejecutarse en el **ámbito global**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="b8d78-219">Los ámbitos tienen un orden de ejecución definido.</span><span class="sxs-lookup"><span data-stu-id="b8d78-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="b8d78-220">Además, cada filtro de acción tiene una propiedad Order que se usa para determinar el orden de ejecución en el ámbito del filtro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="b8d78-221">Para obtener más información sobre el orden de ejecución de los filtros de acción personalizados, visite este artículo de MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="b8d78-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="b8d78-222">Tarea 1: crear un filtro de acción personalizado nuevo</span><span class="sxs-lookup"><span data-stu-id="b8d78-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="b8d78-223">En esta tarea, creará un nuevo filtro de acción personalizada para insertarlo en la clase StoreController y aprenderá a administrar el orden de ejecución de los filtros.</span><span class="sxs-lookup"><span data-stu-id="b8d78-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="b8d78-224">Abra la solución de **Inicio** que se encuentra en la carpeta **\Source\Ex02-ManagingMultipleActionFilters\Begin** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="b8d78-225">De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="b8d78-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="b8d78-226">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b8d78-227">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="b8d78-228">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="b8d78-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="b8d78-229">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="b8d78-230">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8d78-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b8d78-231">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b8d78-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b8d78-232">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="b8d78-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="b8d78-233">Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="b8d78-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="b8d78-234">Agregue una nueva C# clase a la carpeta **Filters** y asígnele el nombre *MyNewCustomActionFilter.CS*</span><span class="sxs-lookup"><span data-stu-id="b8d78-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="b8d78-235">Abra **MyNewCustomActionFilter.CS** y agregue una referencia a **System. Web. Mvc** y el espacio de nombres **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="b8d78-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="b8d78-236">(Fragmento de código- *ASP.NET MVC 4 Custom Action filters-Ex2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="b8d78-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="b8d78-237">Reemplace la declaración de clase predeterminada por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b8d78-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="b8d78-238">(Fragmento de código- *ASP.NET MVC 4 Custom Action filters-Ex2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="b8d78-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="b8d78-239">Este filtro de acción personalizada es casi el mismo que el que creó en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="b8d78-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="b8d78-240">La principal diferencia es que tiene la *&quot;registrada por&quot;* atributo actualizado con el nombre de esta nueva clase para identificar qué filtro registró el registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="b8d78-241">Tarea 2: insertar un nuevo interceptor de código en la clase StoreController</span><span class="sxs-lookup"><span data-stu-id="b8d78-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="b8d78-242">En esta tarea, agregará un nuevo filtro personalizado a la clase StoreController y ejecutará la solución para comprobar el modo en que ambos filtros funcionan juntos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="b8d78-243">Abra la clase **StoreController** que se encuentra en **MvcMusicStore\Controllers** e inserte el nuevo filtro personalizado **MyNewCustomActionFilter** en **StoreController** clase como se muestra en el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b8d78-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="b8d78-244">Ahora, ejecute la aplicación para ver cómo funcionan estos dos filtros de acción personalizados.</span><span class="sxs-lookup"><span data-stu-id="b8d78-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="b8d78-245">Para ello, presione **F5** y espere hasta que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b8d78-246">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b8d78-247">![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image5.png "Estado del seguimiento de registro antes de la actividad de la página")</span><span class="sxs-lookup"><span data-stu-id="b8d78-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8d78-248">*Estado del seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="b8d78-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b8d78-249">Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="b8d78-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b8d78-250">Compruebe esta hora; se realizó un seguimiento de las visitas dos veces: una vez para cada uno de los filtros de acción personalizada que agregó en la clase **StorageController** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="b8d78-251">![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image6.png "Registro de acciones con actividad registrada")</span><span class="sxs-lookup"><span data-stu-id="b8d78-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8d78-252">*Registro de acciones con actividad registrada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b8d78-253">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="b8d78-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="b8d78-254">Tarea 3: administrar el orden de los filtros</span><span class="sxs-lookup"><span data-stu-id="b8d78-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="b8d78-255">En esta tarea, aprenderá a administrar el orden de ejecución de los filtros mediante la propiedad order.</span><span class="sxs-lookup"><span data-stu-id="b8d78-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="b8d78-256">Abra la clase **StoreController** que se encuentra en **MvcMusicStore\Controllers** y especifique la propiedad **Order** en ambos filtros, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="b8d78-257">Ahora, Compruebe cómo se ejecutan los filtros según el valor de su propiedad order.</span><span class="sxs-lookup"><span data-stu-id="b8d78-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="b8d78-258">Observará que el filtro con el menor valor de orden (**CustomActionFilter**) es el primero que se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="b8d78-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="b8d78-259">Presione **F5** y espere hasta que se inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="b8d78-260">Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="b8d78-261">![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image7.png "Estado del seguimiento de registro antes de la actividad de la página")</span><span class="sxs-lookup"><span data-stu-id="b8d78-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="b8d78-262">*Estado del seguimiento de registro antes de la actividad de la página*</span><span class="sxs-lookup"><span data-stu-id="b8d78-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="b8d78-263">Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="b8d78-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="b8d78-264">Compruebe que, en este momento, se realizó un seguimiento de las visitas por los filtros "valor de orden: registros de **CustomActionFilter** " en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="b8d78-265">![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image8.png "Registro de acciones con actividad registrada")</span><span class="sxs-lookup"><span data-stu-id="b8d78-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8d78-266">*Registro de acciones con actividad registrada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="b8d78-267">Ahora, actualizará el valor de orden de los filtros y comprobará cómo cambia el orden de registro.</span><span class="sxs-lookup"><span data-stu-id="b8d78-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="b8d78-268">En la clase **StoreController** , actualice el valor de orden de los filtros, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="b8d78-269">Vuelva a ejecutar la aplicación presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="b8d78-270">Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="b8d78-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="b8d78-271">Compruebe que esta vez, los registros creados por el filtro **MyNewCustomActionFilter** aparecen en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="b8d78-272">![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image9.png "Registro de acciones con actividad registrada")</span><span class="sxs-lookup"><span data-stu-id="b8d78-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8d78-273">*Registro de acciones con actividad registrada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="b8d78-274">Tarea 4: registro de filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="b8d78-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="b8d78-275">En esta tarea, actualizará la solución para registrar el nuevo filtro (**MyNewCustomActionFilter**) como filtro global.</span><span class="sxs-lookup"><span data-stu-id="b8d78-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="b8d78-276">Al hacerlo, se desencadenarán todas las acciones realizadas en la aplicación y no solo en las StoreController de la tarea anterior.</span><span class="sxs-lookup"><span data-stu-id="b8d78-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="b8d78-277">En la clase **StoreController** , quite el atributo **[MyNewCustomActionFilter]** y la propiedad order de **[CustomActionFilter]** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="b8d78-278">Debería tener este aspecto:</span><span class="sxs-lookup"><span data-stu-id="b8d78-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="b8d78-279">Abra el archivo **global. asax** y busque el método de **Inicio de\_** de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="b8d78-280">Tenga en cuenta que cada vez que se inicia la aplicación, se registran los filtros globales mediante una llamada al método **RegisterGlobalFilters** dentro de la clase **FilterConfig** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="b8d78-281">![Registrar filtros globales en global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrar filtros globales en global. asax")</span><span class="sxs-lookup"><span data-stu-id="b8d78-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="b8d78-282">*Registrar filtros globales en global. asax*</span><span class="sxs-lookup"><span data-stu-id="b8d78-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="b8d78-283">Abra el archivo **FilterConfig.CS** en la carpeta de inicio de la **aplicación\_** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="b8d78-284">Agregar una referencia a mediante System. Web. Mvc; usar MvcMusicStore. filters; System.IO.</span><span class="sxs-lookup"><span data-stu-id="b8d78-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="b8d78-285">Actualice el método **RegisterGlobalFilters** agregando el filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="b8d78-286">Para ello, agregue el código resaltado:</span><span class="sxs-lookup"><span data-stu-id="b8d78-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="b8d78-287">Ejecute la aplicación presionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="b8d78-288">Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.</span><span class="sxs-lookup"><span data-stu-id="b8d78-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="b8d78-289">Compruebe que ahora **[MyNewCustomActionFilter]** se está insertando en HomeController y ActionLogController.</span><span class="sxs-lookup"><span data-stu-id="b8d78-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="b8d78-290">![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image11.png "Registro de acciones con actividad registrada")</span><span class="sxs-lookup"><span data-stu-id="b8d78-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="b8d78-291">*Registro de acciones con actividad global registrada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="b8d78-292">Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b8d78-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b8d78-293">Resumen</span><span class="sxs-lookup"><span data-stu-id="b8d78-293">Summary</span></span>

<span data-ttu-id="b8d78-294">Al completar este laboratorio práctico, ha aprendido a extender un filtro de acción para ejecutar acciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="b8d78-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="b8d78-295">También ha aprendido a insertar cualquier filtro en los controladores de páginas.</span><span class="sxs-lookup"><span data-stu-id="b8d78-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="b8d78-296">Se usaron los siguientes conceptos:</span><span class="sxs-lookup"><span data-stu-id="b8d78-296">The following concepts were used:</span></span>

- <span data-ttu-id="b8d78-297">Cómo crear filtros de acción personalizados con la clase ActionFilterAttribute MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b8d78-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="b8d78-298">Inserción de filtros en controladores ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b8d78-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="b8d78-299">Cómo administrar el orden de los filtros mediante la propiedad Order</span><span class="sxs-lookup"><span data-stu-id="b8d78-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="b8d78-300">Cómo registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="b8d78-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b8d78-301">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="b8d78-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b8d78-302">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b8d78-303">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="b8d78-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b8d78-304">Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b8d78-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b8d78-305">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b8d78-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b8d78-306">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-306">Click on **Install Now**.</span></span> <span data-ttu-id="b8d78-307">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="b8d78-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b8d78-308">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b8d78-309">![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b8d78-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b8d78-310">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b8d78-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b8d78-311">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="b8d78-313">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="b8d78-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b8d78-314">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-314">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="b8d78-316">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="b8d78-316">*Installation progress*</span></span>
6. <span data-ttu-id="b8d78-317">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-317">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="b8d78-319">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="b8d78-319">*Installation completed*</span></span>
7. <span data-ttu-id="b8d78-320">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="b8d78-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b8d78-321">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="b8d78-323">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="b8d78-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b8d78-324">Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b8d78-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b8d78-325">Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8d78-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b8d78-326">Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure</span><span class="sxs-lookup"><span data-stu-id="b8d78-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b8d78-327">Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.</span><span class="sxs-lookup"><span data-stu-id="b8d78-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8d78-328">Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico.</span><span class="sxs-lookup"><span data-stu-id="b8d78-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b8d78-329">Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b8d78-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b8d78-330">![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Iniciar sesión en Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="b8d78-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b8d78-331">*Iniciar sesión en Windows Azure Portal de administración*</span><span class="sxs-lookup"><span data-stu-id="b8d78-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b8d78-332">Haga clic en **nuevo** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b8d78-333">![Crear un nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Crear un nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="b8d78-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b8d78-334">*Crear un nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b8d78-335">Haga clic en **compute** | **sitio web**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b8d78-336">A continuación, seleccione la opción **creación rápida** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="b8d78-337">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8d78-338">Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="b8d78-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b8d78-339">La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="b8d78-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b8d78-340">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b8d78-341">![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Crear un nuevo sitio web mediante creación rápida")</span><span class="sxs-lookup"><span data-stu-id="b8d78-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b8d78-342">*Crear un nuevo sitio web mediante creación rápida*</span><span class="sxs-lookup"><span data-stu-id="b8d78-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b8d78-343">Espere hasta que se cree el nuevo **sitio web** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b8d78-344">Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b8d78-345">Compruebe que el nuevo sitio web funciona.</span><span class="sxs-lookup"><span data-stu-id="b8d78-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b8d78-346">![Ir al nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Ir al nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="b8d78-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b8d78-347">*Ir al nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b8d78-348">![Sitio web en ejecución](aspnet-mvc-4-custom-action-filters/_static/image21.png "Sitio web en ejecución")</span><span class="sxs-lookup"><span data-stu-id="b8d78-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="b8d78-349">*Sitio web en ejecución*</span><span class="sxs-lookup"><span data-stu-id="b8d78-349">*Web site running*</span></span>
6. <span data-ttu-id="b8d78-350">Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="b8d78-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b8d78-351">![Abrir las páginas de administración de sitios web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Abrir las páginas de administración de sitios web")</span><span class="sxs-lookup"><span data-stu-id="b8d78-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b8d78-352">*Abrir las páginas de administración de sitios web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b8d78-353">En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .</span><span class="sxs-lookup"><span data-stu-id="b8d78-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8d78-354">El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b8d78-355">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b8d78-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b8d78-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b8d78-357">![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Descargar el perfil de publicación del sitio web")</span><span class="sxs-lookup"><span data-stu-id="b8d78-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b8d78-358">*Descargar el perfil de publicación del sitio web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b8d78-359">Descargue el archivo de Perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="b8d78-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b8d78-360">Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8d78-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b8d78-361">![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image24.png "Guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="b8d78-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b8d78-362">*Guardar el archivo de Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="b8d78-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b8d78-363">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="b8d78-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b8d78-364">Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8d78-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b8d78-365">Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="b8d78-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b8d78-366">Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b8d78-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b8d78-367">Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b8d78-368">Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b8d78-369">Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="b8d78-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b8d78-370">No cree todavía la base de datos, ya que se creará en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="b8d78-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b8d78-371">![Panel de SQL Database Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "Panel de SQL Database Server")</span><span class="sxs-lookup"><span data-stu-id="b8d78-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b8d78-372">*Panel de SQL Database Server*</span><span class="sxs-lookup"><span data-stu-id="b8d78-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b8d78-373">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor.</span><span class="sxs-lookup"><span data-stu-id="b8d78-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b8d78-374">Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-custom-action-filters/_static/image26.png).</span><span class="sxs-lookup"><span data-stu-id="b8d78-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="b8d78-376">*Agregando la dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="b8d78-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b8d78-377">Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="b8d78-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="b8d78-379">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="b8d78-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b8d78-380">Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="b8d78-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b8d78-381">Vuelva a la solución ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b8d78-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b8d78-382">En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b8d78-383">![Publicar la aplicación](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="b8d78-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="b8d78-384">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="b8d78-385">Importe el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="b8d78-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b8d78-386">![Importar el perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="b8d78-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b8d78-387">*Importando Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="b8d78-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="b8d78-388">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-388">Click **Validate Connection**.</span></span> <span data-ttu-id="b8d78-389">Una vez finalizada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b8d78-390">La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.</span><span class="sxs-lookup"><span data-stu-id="b8d78-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b8d78-391">![Validando conexión](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validando conexión")</span><span class="sxs-lookup"><span data-stu-id="b8d78-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="b8d78-392">*Validando conexión*</span><span class="sxs-lookup"><span data-stu-id="b8d78-392">*Validating connection*</span></span>
4. <span data-ttu-id="b8d78-393">En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b8d78-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b8d78-394">![Configuración de web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuración de web deploy")</span><span class="sxs-lookup"><span data-stu-id="b8d78-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b8d78-395">*Configuración de web deploy*</span><span class="sxs-lookup"><span data-stu-id="b8d78-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b8d78-396">Configure la conexión de base de datos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="b8d78-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b8d78-397">En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="b8d78-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b8d78-398">En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="b8d78-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b8d78-399">En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="b8d78-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b8d78-400">Escriba un nuevo nombre de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b8d78-400">Type a new database name.</span></span>

     <span data-ttu-id="b8d78-401">![Configuración de la cadena de conexión de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="b8d78-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b8d78-402">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="b8d78-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b8d78-403">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-403">Then click **OK**.</span></span> <span data-ttu-id="b8d78-404">Cuando se le pida que cree la base de datos, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b8d78-405">![Crear la base de datos](aspnet-mvc-4-custom-action-filters/_static/image34.png "Crear la cadena de base de datos")</span><span class="sxs-lookup"><span data-stu-id="b8d78-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="b8d78-406">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="b8d78-406">*Creating the database*</span></span>
7. <span data-ttu-id="b8d78-407">La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b8d78-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b8d78-408">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-408">Then click **Next**.</span></span>

    <span data-ttu-id="b8d78-409">![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Cadena de conexión que apunta a SQL Database")</span><span class="sxs-lookup"><span data-stu-id="b8d78-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b8d78-410">*Cadena de conexión que apunta a SQL Database*</span><span class="sxs-lookup"><span data-stu-id="b8d78-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b8d78-411">En la página **vista previa** , haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b8d78-412">![Publicación de la aplicación Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publicación de la aplicación Web")</span><span class="sxs-lookup"><span data-stu-id="b8d78-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="b8d78-413">*Publicación de la aplicación Web*</span><span class="sxs-lookup"><span data-stu-id="b8d78-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="b8d78-414">Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.</span><span class="sxs-lookup"><span data-stu-id="b8d78-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b8d78-415">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="b8d78-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b8d78-416">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="b8d78-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b8d78-417">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="b8d78-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b8d78-418">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="b8d78-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b8d78-419">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="b8d78-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b8d78-420">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="b8d78-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b8d78-421">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="b8d78-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b8d78-422">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="b8d78-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b8d78-423">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="b8d78-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b8d78-424">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="b8d78-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b8d78-425">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="b8d78-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b8d78-426">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-custom-action-filters/_static/image38.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="b8d78-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b8d78-427">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="b8d78-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="b8d78-428">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-custom-action-filters/_static/image39.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="b8d78-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b8d78-429">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="b8d78-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b8d78-430">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-custom-action-filters/_static/image40.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="b8d78-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b8d78-431">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="b8d78-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b8d78-432">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="b8d78-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b8d78-433">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="b8d78-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b8d78-434">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="b8d78-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b8d78-435">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="b8d78-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b8d78-436">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-custom-action-filters/_static/image41.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="b8d78-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b8d78-437">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="b8d78-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b8d78-438">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-custom-action-filters/_static/image42.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="b8d78-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b8d78-439">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="b8d78-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
