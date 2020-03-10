---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creando páginas de ayuda para ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: En este tutorial con código se muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448621"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="545ee-103">Crear páginas de ayuda para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="545ee-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="545ee-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="545ee-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="545ee-105">En este tutorial con código se muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="545ee-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="545ee-106">Al crear una API Web, a menudo resulta útil crear una página de ayuda, de modo que otros desarrolladores sepan cómo llamar a la API.</span><span class="sxs-lookup"><span data-stu-id="545ee-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="545ee-107">Puede crear toda la documentación de forma manual, pero es mejor generarla lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="545ee-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="545ee-108">Para facilitar esta tarea, ASP.NET Web API proporciona una biblioteca para la generación automática de páginas de ayuda en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="545ee-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="545ee-109">Creación de páginas de ayuda de API</span><span class="sxs-lookup"><span data-stu-id="545ee-109">Creating API Help Pages</span></span>

<span data-ttu-id="545ee-110">Instale [ASP.net and Web Tools actualización 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="545ee-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="545ee-111">Esta actualización integra las páginas de ayuda en la plantilla de proyecto de Web API.</span><span class="sxs-lookup"><span data-stu-id="545ee-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="545ee-112">A continuación, cree un nuevo proyecto de ASP.NET MVC 4 y seleccione la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="545ee-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="545ee-113">La plantilla de proyecto crea un controlador de API de ejemplo denominado `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="545ee-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="545ee-114">La plantilla también crea las páginas de ayuda de la API.</span><span class="sxs-lookup"><span data-stu-id="545ee-114">The template also creates the API help pages.</span></span> <span data-ttu-id="545ee-115">Todos los archivos de código de la página de ayuda se colocan en la carpeta áreas del proyecto.</span><span class="sxs-lookup"><span data-stu-id="545ee-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="545ee-116">Al ejecutar la aplicación, la Página principal contiene un vínculo a la página de ayuda de la API.</span><span class="sxs-lookup"><span data-stu-id="545ee-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="545ee-117">En la Página principal, la ruta de acceso relativa es/Help.</span><span class="sxs-lookup"><span data-stu-id="545ee-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="545ee-118">Este vínculo le lleva a una página de Resumen de la API.</span><span class="sxs-lookup"><span data-stu-id="545ee-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="545ee-119">La vista de MVC de esta página se define en areas/HelpPage/views/help/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="545ee-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="545ee-120">Puede editar esta página para modificar el diseño, la introducción, el título, los estilos, etc.</span><span class="sxs-lookup"><span data-stu-id="545ee-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="545ee-121">La parte principal de la página es una tabla de API, agrupadas por controlador.</span><span class="sxs-lookup"><span data-stu-id="545ee-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="545ee-122">Las entradas de la tabla se generan dinámicamente, mediante la interfaz **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="545ee-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="545ee-123">(Más adelante hablaré sobre esta interfaz). Si agrega un nuevo controlador de API, la tabla se actualiza automáticamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="545ee-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="545ee-124">La columna "API" muestra el método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="545ee-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="545ee-125">La columna "Descripción" contiene documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="545ee-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="545ee-126">Inicialmente, la documentación es simplemente texto de marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="545ee-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="545ee-127">En la siguiente sección, le mostraré cómo agregar documentación de comentarios XML.</span><span class="sxs-lookup"><span data-stu-id="545ee-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="545ee-128">Cada API tiene un vínculo a una página con información más detallada, incluidos los cuerpos de solicitud y respuesta de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="545ee-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="545ee-129">Agregar páginas de ayuda a un proyecto existente</span><span class="sxs-lookup"><span data-stu-id="545ee-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="545ee-130">Puede agregar páginas de ayuda a un proyecto de API Web existente mediante el administrador de paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="545ee-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="545ee-131">Esta opción es útil para empezar a partir de una plantilla de proyecto diferente de la plantilla de "Web API".</span><span class="sxs-lookup"><span data-stu-id="545ee-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="545ee-132">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="545ee-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="545ee-133">En la ventana de la [consola del administrador de paquetes](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , escriba uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="545ee-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="545ee-134">Para una **C#** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="545ee-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="545ee-135">Para una aplicación **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="545ee-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="545ee-136">Hay dos paquetes, uno para C# y otro para Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="545ee-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="545ee-137">Asegúrese de usar el que coincida con su proyecto.</span><span class="sxs-lookup"><span data-stu-id="545ee-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="545ee-138">Este comando instala los ensamblados necesarios y agrega las vistas de MVC para las páginas de ayuda (ubicadas en la carpeta areas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="545ee-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="545ee-139">Tendrá que agregar manualmente un vínculo a la página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="545ee-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="545ee-140">El URI es/Help.</span><span class="sxs-lookup"><span data-stu-id="545ee-140">The URI is /Help.</span></span> <span data-ttu-id="545ee-141">Para crear un vínculo en una vista de Razor, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="545ee-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="545ee-142">Además, asegúrese de registrar áreas.</span><span class="sxs-lookup"><span data-stu-id="545ee-142">Also, make sure to register areas.</span></span> <span data-ttu-id="545ee-143">En el archivo global. asax, agregue el siguiente código al método de **Inicio de\_** de la aplicación, si no está ya presente:</span><span class="sxs-lookup"><span data-stu-id="545ee-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="545ee-144">Adición de documentación de API</span><span class="sxs-lookup"><span data-stu-id="545ee-144">Adding API Documentation</span></span>

<span data-ttu-id="545ee-145">De forma predeterminada, las páginas de ayuda de tienen cadenas de marcador de posición para la documentación de.</span><span class="sxs-lookup"><span data-stu-id="545ee-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="545ee-146">Puede usar los [comentarios de documentación XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para crear la documentación.</span><span class="sxs-lookup"><span data-stu-id="545ee-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="545ee-147">Para habilitar esta característica, abra las áreas de archivo/HelpPage/App\_Start/HelpPageConfig. CS y quite la marca de comentario de la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="545ee-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="545ee-148">Ahora, habilite la documentación XML.</span><span class="sxs-lookup"><span data-stu-id="545ee-148">Now enable XML documentation.</span></span> <span data-ttu-id="545ee-149">En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="545ee-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="545ee-150">Seleccione la página **compilación** .</span><span class="sxs-lookup"><span data-stu-id="545ee-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="545ee-151">En **salida**, compruebe el **archivo de documentación XML**.</span><span class="sxs-lookup"><span data-stu-id="545ee-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="545ee-152">En el cuadro de edición, escriba "App\_Data/XmlDocument. xml".</span><span class="sxs-lookup"><span data-stu-id="545ee-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="545ee-153">A continuación, abra el código del controlador de API de `ValuesController`, que se define en/Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="545ee-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="545ee-154">Agregue algunos comentarios de documentación a los métodos del controlador.</span><span class="sxs-lookup"><span data-stu-id="545ee-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="545ee-155">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="545ee-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="545ee-156">Sugerencia: Si coloca el símbolo de intercalación en la línea que está encima del método y escribe tres barras diagonales, Visual Studio inserta automáticamente los elementos XML.</span><span class="sxs-lookup"><span data-stu-id="545ee-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="545ee-157">Después, puede rellenar los espacios en blanco.</span><span class="sxs-lookup"><span data-stu-id="545ee-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="545ee-158">Ahora vuelva a compilar y ejecutar la aplicación y vaya a las páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="545ee-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="545ee-159">Las cadenas de documentación deben aparecer en la tabla de API.</span><span class="sxs-lookup"><span data-stu-id="545ee-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="545ee-160">La página de ayuda Lee las cadenas del archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="545ee-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="545ee-161">(Al implementar la aplicación, asegúrese de implementar el archivo XML).</span><span class="sxs-lookup"><span data-stu-id="545ee-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="545ee-162">Una mirada al interior</span><span class="sxs-lookup"><span data-stu-id="545ee-162">Under the Hood</span></span>

<span data-ttu-id="545ee-163">Las páginas de ayuda se compilan sobre la clase **ApiExplorer** , que forma parte del marco de la API Web.</span><span class="sxs-lookup"><span data-stu-id="545ee-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="545ee-164">La clase **ApiExplorer** proporciona materias primas para crear una página de ayuda.</span><span class="sxs-lookup"><span data-stu-id="545ee-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="545ee-165">Para cada API, **ApiExplorer** contiene una **ApiDescription** que describe la API.</span><span class="sxs-lookup"><span data-stu-id="545ee-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="545ee-166">Para este propósito, una "API" se define como la combinación del método HTTP y el URI relativo.</span><span class="sxs-lookup"><span data-stu-id="545ee-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="545ee-167">Por ejemplo, estas son algunas API distintas:</span><span class="sxs-lookup"><span data-stu-id="545ee-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="545ee-168">OBTENER/api/Products</span><span class="sxs-lookup"><span data-stu-id="545ee-168">GET /api/Products</span></span>
- <span data-ttu-id="545ee-169">GET /api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="545ee-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="545ee-170">POST/api/Products</span><span class="sxs-lookup"><span data-stu-id="545ee-170">POST /api/Products</span></span>

<span data-ttu-id="545ee-171">Si una acción de controlador admite varios métodos HTTP, el **ApiExplorer** trata cada método como una API distinta.</span><span class="sxs-lookup"><span data-stu-id="545ee-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="545ee-172">Para ocultar una API de **ApiExplorer**, agregue el atributo **ApiExplorerSettings** a la acción y establezca *IgnoreApi* en true.</span><span class="sxs-lookup"><span data-stu-id="545ee-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="545ee-173">También puede Agregar este atributo al controlador para excluir todo el controlador.</span><span class="sxs-lookup"><span data-stu-id="545ee-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="545ee-174">La clase ApiExplorer obtiene las cadenas de documentación de la interfaz **IDocumentationProvider** .</span><span class="sxs-lookup"><span data-stu-id="545ee-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="545ee-175">Como vimos anteriormente, la biblioteca de páginas de ayuda proporciona un **IDocumentationProvider** que obtiene documentación de cadenas de documentación XML.</span><span class="sxs-lookup"><span data-stu-id="545ee-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="545ee-176">El código se encuentra en/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="545ee-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="545ee-177">Puede obtener documentación de otro origen escribiendo su propio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="545ee-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="545ee-178">Para conectarlo, llame al método de extensión **SetDocumentationProvider** , definido en **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="545ee-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="545ee-179">**ApiExplorer** llama automáticamente a la interfaz **IDocumentationProvider** para obtener cadenas de documentación para cada API.</span><span class="sxs-lookup"><span data-stu-id="545ee-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="545ee-180">Los almacena en la propiedad **Documentation** de los objetos **ApiDescription** y **ApiParameterDescription** .</span><span class="sxs-lookup"><span data-stu-id="545ee-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="545ee-181">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="545ee-181">Next Steps</span></span>

<span data-ttu-id="545ee-182">No está limitado a las páginas de ayuda que se muestran aquí.</span><span class="sxs-lookup"><span data-stu-id="545ee-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="545ee-183">De hecho, **ApiExplorer** no se limita a crear páginas de ayuda.</span><span class="sxs-lookup"><span data-stu-id="545ee-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="545ee-184">Yao Huangt ha escrito algunas excelentes entradas de blog para que pueda pensar de la caja:</span><span class="sxs-lookup"><span data-stu-id="545ee-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="545ee-185">Agregar un cliente de prueba simple a ASP.NET Web API página de ayuda</span><span class="sxs-lookup"><span data-stu-id="545ee-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="545ee-186">Cómo funciona ASP.NET Web API página de ayuda en servicios autohospedados</span><span class="sxs-lookup"><span data-stu-id="545ee-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="545ee-187">Generación en tiempo de diseño de la página de ayuda (o cliente) para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="545ee-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="545ee-188">Personalizaciones de la página de ayuda avanzada</span><span class="sxs-lookup"><span data-stu-id="545ee-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
