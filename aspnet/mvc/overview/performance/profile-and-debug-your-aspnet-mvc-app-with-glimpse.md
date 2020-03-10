---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Generar perfiles y depurar la aplicación ASP.NET MVC con el fin de obtener | Microsoft Docs
author: Rick-Anderson
description: La perspectiva es una familia de paquetes de NuGet de código abierto que se encuentra en constante crecimiento y que proporciona información detallada sobre el rendimiento, la depuración y el diagnóstico de ASP.NET...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432901"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="af72b-103">Crear un perfil y depurar la aplicación de ASP.NET MVC con Glimpse</span><span class="sxs-lookup"><span data-stu-id="af72b-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="af72b-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="af72b-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="af72b-105">La visión es una creciente familia de paquetes NuGet de código abierto que proporcionan información detallada sobre rendimiento, depuración y diagnóstico para aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="af72b-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="af72b-106">Es trivial instalar, ligera, ultra rápida y muestra las métricas de rendimiento clave en la parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="af72b-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="af72b-107">Le permite profundizar en la aplicación cuando necesite averiguar lo que está ocurriendo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="af72b-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="af72b-108">El análisis proporciona una información muy valiosa que se recomienda para usarla durante todo el ciclo de desarrollo, incluido el entorno de prueba de Azure.</span><span class="sxs-lookup"><span data-stu-id="af72b-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="af72b-109">Aunque [Fiddler](http://www.telerik.com/fiddler) y las [herramientas de desarrollo de F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan una vista del lado cliente, el vistazo proporciona una vista detallada del servidor.</span><span class="sxs-lookup"><span data-stu-id="af72b-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="af72b-110">Este tutorial se centrará en el uso de los paquetes de ASP.NET MVC y EF, pero hay muchos otros paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="af72b-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="af72b-111">Siempre que sea posible, crearé un vínculo a los documentos de la [perspectiva](http://getglimpse.com/Docs/) que ayuda a mantener.</span><span class="sxs-lookup"><span data-stu-id="af72b-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="af72b-112">La visión es un proyecto de código abierto, que puede contribuir al código fuente y a los documentos.</span><span class="sxs-lookup"><span data-stu-id="af72b-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="af72b-113">Instalación de la visión</span><span class="sxs-lookup"><span data-stu-id="af72b-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="af72b-114">Habilitar la visión para localhost</span><span class="sxs-lookup"><span data-stu-id="af72b-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="af72b-115">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="af72b-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="af72b-116">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="af72b-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="af72b-117">Rutas</span><span class="sxs-lookup"><span data-stu-id="af72b-117">Routes</span></span>](#route)
- [<span data-ttu-id="af72b-118">Uso de la visión en Azure</span><span class="sxs-lookup"><span data-stu-id="af72b-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="af72b-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="af72b-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="af72b-120">Instalación de la visión</span><span class="sxs-lookup"><span data-stu-id="af72b-120">Installing Glimpse</span></span>

<span data-ttu-id="af72b-121">Puede instalar la visión desde la consola del administrador de paquetes NuGet o desde la consola de **Administración de paquetes Nuget** .</span><span class="sxs-lookup"><span data-stu-id="af72b-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="af72b-122">En esta demostración, instalaré los paquetes Mvc5 y EF6:</span><span class="sxs-lookup"><span data-stu-id="af72b-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalación del vistazo desde NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="af72b-124">Busque el *. EF.*</span><span class="sxs-lookup"><span data-stu-id="af72b-124">Search for *Glimpse.EF*</span></span>

![Vistazo a. EF desde la instalación de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="af72b-126">Al seleccionar **paquetes instalados**, puede ver los módulos dependientes que se han instalado:</span><span class="sxs-lookup"><span data-stu-id="af72b-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Se han instalado los paquetes de visión desde DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="af72b-128">Los siguientes comandos instalan los módulos de MVC5 y EF6 desde la consola del administrador de paquetes:</span><span class="sxs-lookup"><span data-stu-id="af72b-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="af72b-129">Habilitar la visión para localhost</span><span class="sxs-lookup"><span data-stu-id="af72b-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="af72b-130">Vaya a http://localhost:&lt;p ordenar #&gt;/Glimpse.axd y haga clic en el botón <strong>Activar</strong> .</span><span class="sxs-lookup"><span data-stu-id="af72b-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Página de introducción a axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="af72b-132">Si se muestra la barra de favoritos, puede arrastrar y colocar los botones de visión y agregarlos como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="af72b-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE con bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="af72b-134">Ahora puede navegar por la aplicación y se muestra la **pantalla de seguimiento** (HUD) en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="af72b-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Página Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="af72b-136">En la [página ver HUD](http://getglimpse.com/Docs/Heads-up-Display) se detalla la información de control de tiempo que se muestra anteriormente.</span><span class="sxs-lookup"><span data-stu-id="af72b-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="af72b-137">Los datos de rendimiento discretos que muestra el HUD pueden avisarle de un problema inmediatamente, antes de llegar al ciclo de pruebas.</span><span class="sxs-lookup"><span data-stu-id="af72b-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="af72b-138">Al hacer clic en la &quot;g&quot; en la esquina inferior derecha, se abre el panel de visión:</span><span class="sxs-lookup"><span data-stu-id="af72b-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Panel de visión](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="af72b-140">En la imagen anterior, se selecciona la [pestaña ejecución](http://getglimpse.com/Docs/Execution-Tab) , que muestra los detalles de tiempo de las acciones y los filtros de la canalización.</span><span class="sxs-lookup"><span data-stu-id="af72b-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="af72b-141">En la fase 6 de la canalización, puede ver que el [temporizador de filtro de detención del reloj](http://www.nuget.org/packages/StopWatch/) se ha iniciado.</span><span class="sxs-lookup"><span data-stu-id="af72b-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="af72b-142">Aunque el temporizador de peso claro puede proporcionar datos de perfil y control de tiempo útiles, se pierde todo el tiempo invertido en la autorización y en la representación de la vista.</span><span class="sxs-lookup"><span data-stu-id="af72b-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="af72b-143">Puede leer sobre mi temporizador en el [perfil y la hora de la aplicación ASP.NET MVC en Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="af72b-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="af72b-144">La página [pestañas](http://getglimpse.com/Docs/Tabs) proporciona vínculos a información detallada sobre cada pestaña.</span><span class="sxs-lookup"><span data-stu-id="af72b-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="af72b-145">La pestaña escala de tiempo</span><span class="sxs-lookup"><span data-stu-id="af72b-145">The Timeline tab</span></span>

<span data-ttu-id="af72b-146">He modificado el [tutorial de EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pendiente de Tom Dykstra con el siguiente cambio de código en el controlador de instructores:</span><span class="sxs-lookup"><span data-stu-id="af72b-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="af72b-147">El código anterior me permite pasar la cadena de consulta (`eager`) para controlar la carga diligente o explícita de los datos.</span><span class="sxs-lookup"><span data-stu-id="af72b-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="af72b-148">En la imagen siguiente, se usa la carga explícita y en la página control de tiempo se muestra cada inscripción cargada en el método de acción `Index`:</span><span class="sxs-lookup"><span data-stu-id="af72b-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="af72b-150">En el código siguiente, se especifica diligente y se captura cada inscripción después de que se llame a la vista `Index`:</span><span class="sxs-lookup"><span data-stu-id="af72b-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![se especifica diligente](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="af72b-152">Puede mantener el mouse sobre un segmento de tiempo para obtener información detallada sobre el tiempo:</span><span class="sxs-lookup"><span data-stu-id="af72b-152">You can hover over a time segment to get detailed timing information:</span></span>

![Mantenga el puntero para ver el tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="af72b-154">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="af72b-154">Model Binding</span></span>

<span data-ttu-id="af72b-155">La [pestaña enlace de modelos](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información para ayudarle a entender cómo se enlazan las variables de formulario y por qué algunos no están enlazados como cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="af72b-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="af72b-156">En la imagen siguiente se muestra el **?**</span><span class="sxs-lookup"><span data-stu-id="af72b-156">The image below shows the **?**</span></span> <span data-ttu-id="af72b-157">, en el que puede hacer clic en para abrir la página de ayuda de la característica.</span><span class="sxs-lookup"><span data-stu-id="af72b-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![vista de enlace de modelos de visión](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="af72b-159">Rutas</span><span class="sxs-lookup"><span data-stu-id="af72b-159">Routes</span></span>

 <span data-ttu-id="af72b-160">La pestaña ver rutas puede ayudarle a depurar y comprender el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="af72b-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="af72b-161">En la imagen siguiente, se selecciona la ruta del producto (y se muestra en verde, una Convención de visión).</span><span class="sxs-lookup"><span data-stu-id="af72b-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="af72b-162">también se muestran ![nombre del producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) restricciones de ruta, áreas y tokens de datos.</span><span class="sxs-lookup"><span data-stu-id="af72b-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="af72b-163">Para obtener más información, consulte las [rutas](http://getglimpse.com/Docs/Routes-Tab) y el [enrutamiento de atributos en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) .</span><span class="sxs-lookup"><span data-stu-id="af72b-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="af72b-164">Uso de la visión en Azure</span><span class="sxs-lookup"><span data-stu-id="af72b-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="af72b-165">La Directiva de seguridad predeterminada de la visualización solo permite mostrar datos del host local.</span><span class="sxs-lookup"><span data-stu-id="af72b-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="af72b-166">Puede cambiar esta directiva de seguridad para poder ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure).</span><span class="sxs-lookup"><span data-stu-id="af72b-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="af72b-167">Para entornos de prueba en Azure, agregue la marca resaltada a la parte inferior del archivo *Web. config* para habilitar el análisis:</span><span class="sxs-lookup"><span data-stu-id="af72b-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="af72b-168">Con este cambio solo, cualquier usuario puede ver los datos en un sitio remoto.</span><span class="sxs-lookup"><span data-stu-id="af72b-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="af72b-169">Considere la posibilidad de agregar el marcado anterior a un perfil de publicación para que solo se implemente un aplicado cuando se use ese perfil de publicación (por ejemplo, su perfil de prueba de Azure). Para restringir la consulta de datos, agregaremos el rol `canViewGlimpseData` y solo permitiremos a los usuarios de este rol ver datos de visión.</span><span class="sxs-lookup"><span data-stu-id="af72b-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="af72b-170">Quite los comentarios del archivo *GlimpseSecurityPolicy.CS* y cambie la llamada de [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) de `Administrator` al rol de `canViewGlimpseData`:</span><span class="sxs-lookup"><span data-stu-id="af72b-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="af72b-171">Seguridad: los datos enriquecidos que se proporcionan en la vista pueden exponer la seguridad de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="af72b-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="af72b-172">Microsoft no ha realizado una auditoría de seguridad de la perspectiva de uso en las aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="af72b-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="af72b-173">Para más información sobre cómo agregar roles, consulte el tutorial de [implementación de una aplicación Web de ASP.NET MVC 5 en Azure deploy con pertenencia, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="af72b-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="af72b-174">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="af72b-174">Additional Resources</span></span>

- [<span data-ttu-id="af72b-175">Implementación de una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure</span><span class="sxs-lookup"><span data-stu-id="af72b-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="af72b-176">[Visión](http://getglimpse.com/Docs/Configuration) de la página de documento sobre la configuración de pestañas, la Directiva de tiempo de ejecución, el registro y mucho más.</span><span class="sxs-lookup"><span data-stu-id="af72b-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
