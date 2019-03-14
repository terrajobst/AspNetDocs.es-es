---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57061712"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="c39a4-103">Áreas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c39a4-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="c39a4-104">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c39a4-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c39a4-105">Las áreas son una característica de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).</span><span class="sxs-lookup"><span data-stu-id="c39a4-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="c39a4-106">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`, o bien a un elemento `page` de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="c39a4-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="c39a4-107">Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core en grupos funcionales más pequeños, cada uno con su propio conjunto de Razor Pages, controladores, vistas y modelos.</span><span class="sxs-lookup"><span data-stu-id="c39a4-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="c39a4-108">Un área es en realidad una estructura dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="c39a4-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="c39a4-109">En un proyecto web ASP.NET Core, los componentes lógicos como Páginas, Modelo, Vista y Controlador se mantienen en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="c39a4-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="c39a4-110">El runtime de ASP.NET Core usa convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="c39a4-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="c39a4-111">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="c39a4-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="c39a4-112">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como las de finalización de la compra, facturación y búsqueda.</span><span class="sxs-lookup"><span data-stu-id="c39a4-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="c39a4-113">Cada una de estas unidades tiene un área propia para controlar vistas, controladores, Razor Pages y modelos.</span><span class="sxs-lookup"><span data-stu-id="c39a4-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="c39a4-114">Considere el uso de áreas en un proyecto en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c39a4-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="c39a4-115">La aplicación está formada por varios componentes funcionales generales que se pueden separar de forma lógica.</span><span class="sxs-lookup"><span data-stu-id="c39a4-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="c39a4-116">Le interesa dividir la aplicación para que se pueda trabajar en cada área funcional de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="c39a4-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="c39a4-117">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c39a4-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c39a4-118">En el ejemplo de descarga se proporciona una aplicación básica para probar las áreas.</span><span class="sxs-lookup"><span data-stu-id="c39a4-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="c39a4-119">Áreas para controladores con vistas</span><span class="sxs-lookup"><span data-stu-id="c39a4-119">Areas for controllers with views</span></span>

<span data-ttu-id="c39a4-120">Una aplicación web ASP.NET Core típica en la que se usen áreas, controladores y vistas contiene lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c39a4-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="c39a4-121">Una [estructura de carpetas de área](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="c39a4-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="c39a4-122">Controladores decorados con el atributo [&lbrack;Area&rbrack;](#attribute) para asociar el controlador el área: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c39a4-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="c39a4-123">La [ruta de área agregada al inicio](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="c39a4-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="c39a4-124">Estructura de carpetas de área</span><span class="sxs-lookup"><span data-stu-id="c39a4-124">Area folder structure</span></span>
<span data-ttu-id="c39a4-125">Considere una aplicación que tiene dos grupos lógicos, *Productos* y *Servicios*.</span><span class="sxs-lookup"><span data-stu-id="c39a4-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="c39a4-126">Mediante las áreas, la estructura de carpetas sería similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="c39a4-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="c39a4-127">Nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="c39a4-127">Project name</span></span>
  * <span data-ttu-id="c39a4-128">Áreas</span><span class="sxs-lookup"><span data-stu-id="c39a4-128">Areas</span></span>
    * <span data-ttu-id="c39a4-129">Productos</span><span class="sxs-lookup"><span data-stu-id="c39a4-129">Products</span></span>
      * <span data-ttu-id="c39a4-130">Controladores</span><span class="sxs-lookup"><span data-stu-id="c39a4-130">Controllers</span></span>
        * <span data-ttu-id="c39a4-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="c39a4-131">HomeController.cs</span></span>
        * <span data-ttu-id="c39a4-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="c39a4-132">ManageController.cs</span></span>
      * <span data-ttu-id="c39a4-133">Vistas</span><span class="sxs-lookup"><span data-stu-id="c39a4-133">Views</span></span>
        * <span data-ttu-id="c39a4-134">Página principal</span><span class="sxs-lookup"><span data-stu-id="c39a4-134">Home</span></span>
          * <span data-ttu-id="c39a4-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c39a4-135">Index.cshtml</span></span>
        * <span data-ttu-id="c39a4-136">Administrar</span><span class="sxs-lookup"><span data-stu-id="c39a4-136">Manage</span></span>
          * <span data-ttu-id="c39a4-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c39a4-137">Index.cshtml</span></span>
          * <span data-ttu-id="c39a4-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="c39a4-138">About.cshtml</span></span>
    * <span data-ttu-id="c39a4-139">Servicios</span><span class="sxs-lookup"><span data-stu-id="c39a4-139">Services</span></span>
      * <span data-ttu-id="c39a4-140">Controladores</span><span class="sxs-lookup"><span data-stu-id="c39a4-140">Controllers</span></span>
        * <span data-ttu-id="c39a4-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="c39a4-141">HomeController.cs</span></span>
      * <span data-ttu-id="c39a4-142">Vistas</span><span class="sxs-lookup"><span data-stu-id="c39a4-142">Views</span></span>
        * <span data-ttu-id="c39a4-143">Página principal</span><span class="sxs-lookup"><span data-stu-id="c39a4-143">Home</span></span>
          * <span data-ttu-id="c39a4-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c39a4-144">Index.cshtml</span></span>

<span data-ttu-id="c39a4-145">Aunque el diseño anterior es típico cuando se usan áreas, solo los archivos de vista tienen que usar esta estructura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="c39a4-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="c39a4-146">La detección de vista busca un archivo de vista de área coincidente en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="c39a4-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="c39a4-147">La ubicación de las carpetas que no son de vista, como *Controllers* y *Models*, **no** importa.</span><span class="sxs-lookup"><span data-stu-id="c39a4-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="c39a4-148">Por ejemplo, las carpetas *Controllers* y *Models* no son necesarias.</span><span class="sxs-lookup"><span data-stu-id="c39a4-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="c39a4-149">El contenido de *Controllers* y *Models* es código que se compila en un archivo .dll.</span><span class="sxs-lookup"><span data-stu-id="c39a4-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="c39a4-150">El contenido de *Views* no se compila hasta que se haya realizado una solicitud a esa vista.</span><span class="sxs-lookup"><span data-stu-id="c39a4-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="c39a4-151">Asociación del controlador a un área</span><span class="sxs-lookup"><span data-stu-id="c39a4-151">Associate the controller with an Area</span></span>

<span data-ttu-id="c39a4-152">Los controladores de área se designan con el atributo [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute):</span><span class="sxs-lookup"><span data-stu-id="c39a4-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="c39a4-153">Adición de la ruta de área</span><span class="sxs-lookup"><span data-stu-id="c39a4-153">Add Area route</span></span>

<span data-ttu-id="c39a4-154">Las rutas de área normalmente usan el enrutamiento convencional en lugar del enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="c39a4-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="c39a4-155">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="c39a4-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="c39a4-156">En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="c39a4-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="c39a4-157">Se puede `{area:...}` usar como un token en las plantillas de ruta, si el espacio de direcciones URL es uniforme en todas las áreas:</span><span class="sxs-lookup"><span data-stu-id="c39a4-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="c39a4-158">En el código anterior, `exists` aplica la restricción de que la ruta debe coincidir con un área.</span><span class="sxs-lookup"><span data-stu-id="c39a4-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="c39a4-159">El uso de `{area:...}` es el mecanismo menos complicado para agregar enrutamiento a las áreas.</span><span class="sxs-lookup"><span data-stu-id="c39a4-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="c39a4-160">En el código siguiente se usa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> para crear dos rutas de área con nombre:</span><span class="sxs-lookup"><span data-stu-id="c39a4-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="c39a4-161">Cuando use `MapAreaRoute` con ASP.NET Core 2.2, vea [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="c39a4-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="c39a4-162">Para más información, vea [Enrutamiento de áreas](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="c39a4-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="c39a4-163">Generación de vínculos con áreas</span><span class="sxs-lookup"><span data-stu-id="c39a4-163">Link Generation with Areas</span></span>

<span data-ttu-id="c39a4-164">En el código siguiente de la [descarga de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) se muestra la generación de vínculos con el área especificada:</span><span class="sxs-lookup"><span data-stu-id="c39a4-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="c39a4-165">Los vínculos generados con el código anterior son válidos en cualquier parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c39a4-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="c39a4-166">En la descarga de ejemplo se incluye una [vista parcial](xref:mvc/views/partial) que contiene los vínculos anteriores y los mismos vínculos sin especificar el área.</span><span class="sxs-lookup"><span data-stu-id="c39a4-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="c39a4-167">En el [archivo de diseño]() se hace referencia a la vista parcial, por lo que en todas las páginas de la aplicación se muestran los vínculos generados.</span><span class="sxs-lookup"><span data-stu-id="c39a4-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="c39a4-168">Los vínculos generados sin especificar el área solo son válidos cuando se les hace referencia desde una página en el mismo área y controlador.</span><span class="sxs-lookup"><span data-stu-id="c39a4-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="c39a4-169">Cuando no se especifica el área o el controlador, el enrutamiento depende de los valores de *ambiente*.</span><span class="sxs-lookup"><span data-stu-id="c39a4-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="c39a4-170">Los valores de ruta actuales de la solicitud actual se consideran valores de ambiente para la generación de vínculos.</span><span class="sxs-lookup"><span data-stu-id="c39a4-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="c39a4-171">En muchos casos de la aplicación de ejemplo, el uso de valores de ambiente genera vínculos incorrectos.</span><span class="sxs-lookup"><span data-stu-id="c39a4-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="c39a4-172">Para más información, vea [Enrutar a acciones de controlador](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="c39a4-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="c39a4-173">Diseño Compartido para áreas con el archivo _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="c39a4-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="c39a4-174">Para compartir un diseño común para toda la aplicación, mueva el archivo *_ViewStart.cshtml* a la carpeta raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c39a4-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="c39a4-175">Cambio de la carpeta de área predeterminada donde se almacenan las vistas</span><span class="sxs-lookup"><span data-stu-id="c39a4-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="c39a4-176">En el código siguiente se cambia la carpeta de área predeterminada de `"Areas"` a `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="c39a4-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="c39a4-177">Publicación de áreas</span><span class="sxs-lookup"><span data-stu-id="c39a4-177">Publishing Areas</span></span>

<span data-ttu-id="c39a4-178">Todos los archivos `*.cshtml` y `wwwroot/**` se publican en la salida cuando se incluye `<Project Sdk="Microsoft.NET.Sdk.Web">` en el archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c39a4-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
