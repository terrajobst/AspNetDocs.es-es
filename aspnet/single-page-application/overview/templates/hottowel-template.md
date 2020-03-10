---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de paño de acceso frecuente | Microsoft Docs
author: madskristensen
description: Plantilla HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467143"
---
# <a name="hot-towel-template"></a><span data-ttu-id="adc02-103">Plantilla de Hot Towel</span><span class="sxs-lookup"><span data-stu-id="adc02-103">Hot Towel template</span></span>

<span data-ttu-id="adc02-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="adc02-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="adc02-105">La plantilla MVC de webtoallas está escrita por John Papa</span><span class="sxs-lookup"><span data-stu-id="adc02-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="adc02-106">Elija la versión que desea descargar:</span><span class="sxs-lookup"><span data-stu-id="adc02-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="adc02-107">Plantilla MVC de webtoallas para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="adc02-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="adc02-108">Plantilla MVC de webtoallas para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="adc02-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="adc02-109">El paño activo: porque no desea ir al SPA sin ninguno.</span><span class="sxs-lookup"><span data-stu-id="adc02-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="adc02-110">¿Quiere crear un SPA pero no puede decidir por dónde empezar?</span><span class="sxs-lookup"><span data-stu-id="adc02-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="adc02-111">Use el paño activo y, en segundos, tendrá un SPA y todas las herramientas que necesita para compilar.</span><span class="sxs-lookup"><span data-stu-id="adc02-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="adc02-112">El paño candente crea un excelente punto de partida para crear una aplicación de una sola página (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="adc02-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="adc02-113">De forma avanzada, proporciona una estructura modular para el código, ver la navegación, el enlace de datos, la administración de datos enriquecida y el estilo sencillo pero elegante.</span><span class="sxs-lookup"><span data-stu-id="adc02-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="adc02-114">El paño de acceso frecuente proporciona todo lo que necesita para crear un SPA, de modo que pueda centrarse en la aplicación, no en el establecimiento.</span><span class="sxs-lookup"><span data-stu-id="adc02-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="adc02-115">Obtenga más información sobre la creación de un SPA a partir de [vídeos, tutoriales y cursos de Pluralsight de John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="adc02-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="adc02-116">Estructura de aplicación</span><span class="sxs-lookup"><span data-stu-id="adc02-116">Application Structure</span></span>

<span data-ttu-id="adc02-117">El formato de WebPhone SPA proporciona una carpeta de aplicación que contiene los archivos JavaScript y HTML que definen su aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="adc02-118">Dentro de la carpeta de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="adc02-118">Inside the App folder:</span></span>

- <span data-ttu-id="adc02-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="adc02-119">durandal</span></span>
- <span data-ttu-id="adc02-120">servicios</span><span class="sxs-lookup"><span data-stu-id="adc02-120">services</span></span>
- <span data-ttu-id="adc02-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="adc02-121">viewmodels</span></span>
- <span data-ttu-id="adc02-122">vistas</span><span class="sxs-lookup"><span data-stu-id="adc02-122">views</span></span>

<span data-ttu-id="adc02-123">La carpeta de la aplicación contiene una colección de módulos.</span><span class="sxs-lookup"><span data-stu-id="adc02-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="adc02-124">Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos.</span><span class="sxs-lookup"><span data-stu-id="adc02-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="adc02-125">La carpeta views contiene el código HTML de la aplicación y la carpeta ViewModels contiene la lógica de presentación para las vistas (un patrón MVVM común).</span><span class="sxs-lookup"><span data-stu-id="adc02-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="adc02-126">La carpeta servicios es ideal para alojar cualquier servicio común que la aplicación pueda necesitar, como la recuperación de datos HTTP o la interacción del almacenamiento local.</span><span class="sxs-lookup"><span data-stu-id="adc02-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="adc02-127">Es común que varios ViewModels vuelvan a usar el código de los módulos de servicio.</span><span class="sxs-lookup"><span data-stu-id="adc02-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="adc02-128">Estructura de la aplicación del lado servidor ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="adc02-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="adc02-129">El paño activo se basa en la conocida y eficaz estructura de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="adc02-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="adc02-130">Inicio de\_de la aplicación</span><span class="sxs-lookup"><span data-stu-id="adc02-130">App\_Start</span></span>
- <span data-ttu-id="adc02-131">Contenido</span><span class="sxs-lookup"><span data-stu-id="adc02-131">Content</span></span>
- <span data-ttu-id="adc02-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="adc02-132">Controllers</span></span>
- <span data-ttu-id="adc02-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="adc02-133">Models</span></span>
- <span data-ttu-id="adc02-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="adc02-134">Scripts</span></span>
- <span data-ttu-id="adc02-135">Vistas</span><span class="sxs-lookup"><span data-stu-id="adc02-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="adc02-136">Bibliotecas destacadas</span><span class="sxs-lookup"><span data-stu-id="adc02-136">Featured Libraries</span></span>

- <span data-ttu-id="adc02-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="adc02-137">ASP.NET MVC</span></span>
- <span data-ttu-id="adc02-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="adc02-138">ASP.NET Web API</span></span>
- <span data-ttu-id="adc02-139">Optimización web de ASP.NET: Unión y minificación</span><span class="sxs-lookup"><span data-stu-id="adc02-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="adc02-140">[Multiplataforma: administración de datos](http://Breezejs.com) enriquecida</span><span class="sxs-lookup"><span data-stu-id="adc02-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="adc02-141">[Durandal. js](http://Durandaljs.com) : navegación y visualización de la composición</span><span class="sxs-lookup"><span data-stu-id="adc02-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="adc02-142">[Knockout. js](http://Knockoutjs.com) : enlaces de datos</span><span class="sxs-lookup"><span data-stu-id="adc02-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="adc02-143">Requerir la modularidad de [. js](http://requirejs.org) con AMD y optimización</span><span class="sxs-lookup"><span data-stu-id="adc02-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="adc02-144">[Tostador. js](http://jpapa.me/c7toastr) : mensajes emergentes</span><span class="sxs-lookup"><span data-stu-id="adc02-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="adc02-145">[Bootstrap de Twitter](https://twitter.github.com/bootstrap/) : estilo de CSS sólido</span><span class="sxs-lookup"><span data-stu-id="adc02-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="adc02-146">Instalación a través de la plantilla de Visual Studio 2012 Hot paño SPA</span><span class="sxs-lookup"><span data-stu-id="adc02-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="adc02-147">El paño activo se puede instalar como una plantilla de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="adc02-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="adc02-148">Simplemente haga clic en `File` | `New Project` y elija `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="adc02-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="adc02-149">A continuación, seleccione la plantilla "aplicación de una sola página de webtoallas" y ejecute.</span><span class="sxs-lookup"><span data-stu-id="adc02-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="adc02-150">Instalación mediante el paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="adc02-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="adc02-151">El paño de acceso frecuente es también un paquete de NuGet que aumenta un proyecto de MVC de ASP.NET vacío existente.</span><span class="sxs-lookup"><span data-stu-id="adc02-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="adc02-152">Solo tiene que instalar con Nuget y, después, ejecutar.</span><span class="sxs-lookup"><span data-stu-id="adc02-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="adc02-153">¿Cómo puedo compilar en el paño activo?</span><span class="sxs-lookup"><span data-stu-id="adc02-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="adc02-154">Simplemente comience a agregar código.</span><span class="sxs-lookup"><span data-stu-id="adc02-154">Simply start adding code!</span></span>

1. <span data-ttu-id="adc02-155">Agregue su propio código del lado servidor, preferiblemente Entity Framework y WebAPI (que realmente destaca con el. js)</span><span class="sxs-lookup"><span data-stu-id="adc02-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="adc02-156">Agregar vistas a la carpeta `App/views`</span><span class="sxs-lookup"><span data-stu-id="adc02-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="adc02-157">Agregar ViewModels a la carpeta `App/viewmodels`</span><span class="sxs-lookup"><span data-stu-id="adc02-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="adc02-158">Agregar enlaces de datos HTML y knockout a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="adc02-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="adc02-159">Actualice las rutas de navegación en `shell.js`</span><span class="sxs-lookup"><span data-stu-id="adc02-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="adc02-160">Tutorial de HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="adc02-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="adc02-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="adc02-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="adc02-162">index. cshtml es la ruta de inicio y la vista de la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="adc02-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="adc02-163">Contiene todas las etiquetas meta estándar, vínculos CSS y referencias de JavaScript que cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="adc02-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="adc02-164">El cuerpo contiene una sola `<div>` que es donde se colocará todo el contenido (las vistas) cuando se soliciten.</span><span class="sxs-lookup"><span data-stu-id="adc02-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="adc02-165">En el `@Scripts.Render` se usa require. js para ejecutar el punto de entrada del código de la aplicación, que se encuentra en el archivo de `main.js`.</span><span class="sxs-lookup"><span data-stu-id="adc02-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="adc02-166">Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="adc02-167">App/Main. js</span><span class="sxs-lookup"><span data-stu-id="adc02-167">App/main.js</span></span>

<span data-ttu-id="adc02-168">El archivo de `main.js` contiene el código que se ejecutará en cuanto se cargue la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="adc02-169">Aquí es donde desea definir las rutas de navegación, establecer las vistas de inicio y realizar cualquier instalación o arranque, como desbloquear los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="adc02-170">El archivo `main.js` define algunos de los módulos de Durandal para ayudar a iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="adc02-171">La instrucción define ayuda a resolver las dependencias de los módulos para que estén disponibles para la función.</span><span class="sxs-lookup"><span data-stu-id="adc02-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="adc02-172">En primer lugar, los mensajes de depuración están habilitados, que envían mensajes sobre qué eventos realiza la aplicación en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="adc02-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="adc02-173">El código app. Start indica a Durandal Framework que inicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="adc02-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="adc02-174">Las convenciones se establecen para que Durandal sepa todas las vistas y ViewModels se encuentran en las mismas carpetas con nombre, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="adc02-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="adc02-175">Por último, el `app.setRoot` inicia la carga del `shell` mediante una animación de `entrance` predefinida.</span><span class="sxs-lookup"><span data-stu-id="adc02-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="adc02-176">Vistas</span><span class="sxs-lookup"><span data-stu-id="adc02-176">Views</span></span>

<span data-ttu-id="adc02-177">Las vistas se encuentran en la carpeta `App/views`.</span><span class="sxs-lookup"><span data-stu-id="adc02-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="adc02-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="adc02-178">shell.html</span></span>

<span data-ttu-id="adc02-179">El `shell.html` contiene el diseño maestro para el código HTML.</span><span class="sxs-lookup"><span data-stu-id="adc02-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="adc02-180">Todas las demás vistas se crearán en algún lugar de la vista de `shell`.</span><span class="sxs-lookup"><span data-stu-id="adc02-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="adc02-181">El paño de acceso frecuente proporciona una `shell` con tres regiones de este tipo: un encabezado, un área de contenido y un pie de página.</span><span class="sxs-lookup"><span data-stu-id="adc02-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="adc02-182">Cada una de estas regiones se carga con contenido que forma otras vistas cuando se solicita.</span><span class="sxs-lookup"><span data-stu-id="adc02-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="adc02-183">Los enlaces de `compose` para el encabezado y el pie de página están codificados de forma rígida en el paño activo para que apunten a las vistas `nav` y `footer`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="adc02-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="adc02-184">El enlace de redacción de la sección `#content` se enlaza al elemento activo del módulo de `router`.</span><span class="sxs-lookup"><span data-stu-id="adc02-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="adc02-185">En otras palabras, al hacer clic en un vínculo de navegación, su vista correspondiente se carga en esta área.</span><span class="sxs-lookup"><span data-stu-id="adc02-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="adc02-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="adc02-186">nav.html</span></span>

<span data-ttu-id="adc02-187">El `nav.html` contiene los vínculos de navegación para el SPA.</span><span class="sxs-lookup"><span data-stu-id="adc02-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="adc02-188">Aquí es donde se puede colocar la estructura de menú, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="adc02-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="adc02-189">A menudo, esto está enlazado a datos (mediante Knockout) al módulo `router` para mostrar la navegación que ha definido en el `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="adc02-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="adc02-190">El knockout busca los atributos de enlace de datos y los enlaza al `shell` ViewModel para mostrar las rutas de navegación y mostrar un componente ProgressBar (mediante Twitter bootstrap) si el módulo de `router` está en medio de navegar de una vista a otra (vea `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="adc02-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="adc02-191">Home. html y details. html</span><span class="sxs-lookup"><span data-stu-id="adc02-191">home.html and details.html</span></span>

<span data-ttu-id="adc02-192">Estas vistas contienen HTML para las vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="adc02-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="adc02-193">Cuando se hace clic en el vínculo `home` del menú de la vista de `nav`, la vista de `home` se colocará en el área de contenido de la vista de `shell`.</span><span class="sxs-lookup"><span data-stu-id="adc02-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="adc02-194">Estas vistas se pueden aumentar o reemplazar por sus propias vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="adc02-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="adc02-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="adc02-195">footer.html</span></span>

<span data-ttu-id="adc02-196">El `footer.html` contiene el código HTML que aparece en el pie de página, en la parte inferior de la vista de `shell`.</span><span class="sxs-lookup"><span data-stu-id="adc02-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="adc02-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="adc02-197">ViewModels</span></span>

<span data-ttu-id="adc02-198">ViewModels se encuentran en la carpeta `App/viewmodels`.</span><span class="sxs-lookup"><span data-stu-id="adc02-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="adc02-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="adc02-199">shell.js</span></span>

<span data-ttu-id="adc02-200">El `shell` ViewModel contiene propiedades y funciones que se enlazan a la vista de `shell`.</span><span class="sxs-lookup"><span data-stu-id="adc02-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="adc02-201">A menudo, aquí es donde se encuentran los enlaces de navegación del menú (vea la lógica de `router.mapNav`).</span><span class="sxs-lookup"><span data-stu-id="adc02-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="adc02-202">Home. js y details. js</span><span class="sxs-lookup"><span data-stu-id="adc02-202">home.js and details.js</span></span>

<span data-ttu-id="adc02-203">Estos ViewModels contienen las propiedades y las funciones que se enlazan a la vista `home`.</span><span class="sxs-lookup"><span data-stu-id="adc02-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="adc02-204">también contiene la lógica de presentación de la vista y es el pegado entre los datos y la vista.</span><span class="sxs-lookup"><span data-stu-id="adc02-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="adc02-205">Servicios</span><span class="sxs-lookup"><span data-stu-id="adc02-205">Services</span></span>

<span data-ttu-id="adc02-206">Los servicios se encuentran en la carpeta app/Services.</span><span class="sxs-lookup"><span data-stu-id="adc02-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="adc02-207">Idealmente, los servicios futuros, como un módulo de servicio de datos, que es responsable de la obtención y publicación de datos remotos, podrían colocarse.</span><span class="sxs-lookup"><span data-stu-id="adc02-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="adc02-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="adc02-208">logger.js</span></span>

<span data-ttu-id="adc02-209">El paño de acceso frecuente proporciona un módulo de `logger` en la carpeta servicios.</span><span class="sxs-lookup"><span data-stu-id="adc02-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="adc02-210">El módulo `logger` es ideal para registrar mensajes en la consola y en el usuario en elementos emergentes emergentes.</span><span class="sxs-lookup"><span data-stu-id="adc02-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
