---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Presentación de ASP.NET Web Pages: creación de un diseño coherente | Microsoft Docs'
author: Rick-Anderson
description: En este tutorial se muestra cómo usar diseños para crear una apariencia coherente de las páginas de un sitio que usa ASP.NET Web Pages. Se supone que ha completado la...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422749"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="e567b-104">Presentación de ASP.NET Web Pages: creación de un diseño coherente</span><span class="sxs-lookup"><span data-stu-id="e567b-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>

<span data-ttu-id="e567b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e567b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e567b-106">En este tutorial se muestra cómo usar *diseños* para crear una apariencia coherente de las páginas de un sitio que usa ASP.NET Web pages.</span><span class="sxs-lookup"><span data-stu-id="e567b-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="e567b-107">Se supone que ha completado la serie a través [de la eliminación de datos de la base de datos en ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="e567b-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="e567b-108">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="e567b-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e567b-109">Qué es una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-109">What a layout page is.</span></span>
> - <span data-ttu-id="e567b-110">Cómo combinar páginas de diseño con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="e567b-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="e567b-111">Cómo pasar valores a una página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-111">How to pass values to a layout page.</span></span>

## <a name="about-layouts"></a><span data-ttu-id="e567b-112">Acerca de los diseños</span><span class="sxs-lookup"><span data-stu-id="e567b-112">About Layouts</span></span>

<span data-ttu-id="e567b-113">Las páginas que ha creado hasta ahora se han completado, páginas independientes.</span><span class="sxs-lookup"><span data-stu-id="e567b-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="e567b-114">Todos ellos pertenecen al mismo sitio, pero no tienen ningún elemento común o una apariencia estándar.</span><span class="sxs-lookup"><span data-stu-id="e567b-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="e567b-115">La mayoría de los sitios tienen una apariencia y un diseño coherentes.</span><span class="sxs-lookup"><span data-stu-id="e567b-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="e567b-116">Por ejemplo, si va al sitio de [Microsoft.com/web](https://www.microsoft.com/web/) y observa que las páginas se adhieren a un diseño general y a un tema visual:</span><span class="sxs-lookup"><span data-stu-id="e567b-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Página del sitio de Microsoft.com/web que muestra el diseño del encabezado, el área de navegación, el área de contenido y el pie de página](layouts/_static/image1.png)

<span data-ttu-id="e567b-118">Una manera *ineficaz* de crear este diseño sería definir un encabezado, una barra de navegación y un pie de página por separado en cada una de las páginas.</span><span class="sxs-lookup"><span data-stu-id="e567b-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="e567b-119">Se va a duplicar el mismo marcado cada vez.</span><span class="sxs-lookup"><span data-stu-id="e567b-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="e567b-120">Si desea cambiar algo (por ejemplo, actualizar el pie de página), tendría que cambiar cada página por separado.</span><span class="sxs-lookup"><span data-stu-id="e567b-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="e567b-121">Aquí es donde se encuentran *las páginas de diseño* .</span><span class="sxs-lookup"><span data-stu-id="e567b-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="e567b-122">En ASP.NET Web Pages, puede definir una página de diseño que proporcione un contenedor global para las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="e567b-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="e567b-123">Por ejemplo, la página de diseño puede contener el encabezado, el área de navegación y el pie de página.</span><span class="sxs-lookup"><span data-stu-id="e567b-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="e567b-124">La página de diseño incluye un marcador de posición donde va el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e567b-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="e567b-125">Después, puede definir páginas de contenido individuales que contengan el marcado y el código solo para esa página.</span><span class="sxs-lookup"><span data-stu-id="e567b-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="e567b-126">Las páginas de contenido no tienen que ser páginas HTML completas; ni siquiera tienen que tener un elemento `<body>`.</span><span class="sxs-lookup"><span data-stu-id="e567b-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="e567b-127">También tienen una línea de código que indica a ASP.NET en qué página de diseño desea mostrar el contenido.</span><span class="sxs-lookup"><span data-stu-id="e567b-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="e567b-128">Esta es una imagen que muestra aproximadamente cómo funciona esta relación:</span><span class="sxs-lookup"><span data-stu-id="e567b-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagrama conceptual que muestra dos páginas de contenido y una página de diseño en la que se ajustan](layouts/_static/image2.png)

<span data-ttu-id="e567b-130">Esta interacción es fácil de entender cuando se ve en acción.</span><span class="sxs-lookup"><span data-stu-id="e567b-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="e567b-131">En este tutorial, cambiará las páginas de películas para usar un diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="e567b-132">Agregar una página de diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-132">Adding a Layout Page</span></span>

<span data-ttu-id="e567b-133">Comenzaremos por crear una página de diseño que defina un diseño de página típico con un encabezado, un pie de página y un área para el contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e567b-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="e567b-134">En el sitio de WebPagesMovies, agregue una página CSHTML denominada *\_layout. CSHTML*.</span><span class="sxs-lookup"><span data-stu-id="e567b-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="e567b-135">El carácter de subrayado inicial (`_`) es significativo.</span><span class="sxs-lookup"><span data-stu-id="e567b-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="e567b-136">Si el nombre de una página comienza con un carácter de subrayado, ASP.NET no enviará directamente esa página al explorador.</span><span class="sxs-lookup"><span data-stu-id="e567b-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="e567b-137">Esta Convención le permite definir las páginas necesarias para su sitio, pero que los usuarios no deben poder solicitar directamente.</span><span class="sxs-lookup"><span data-stu-id="e567b-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="e567b-138">Reemplace el contenido de la página por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e567b-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="e567b-139">Como puede ver, este marcado es simplemente HTML que usa `<div>` elementos para definir tres secciones en la página más un elemento `<div>` más para contener las tres secciones.</span><span class="sxs-lookup"><span data-stu-id="e567b-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="e567b-140">El pie de página contiene un poco de código Razor: `@DateTime.Now.Year`, que representará el año actual en esa ubicación de la página.</span><span class="sxs-lookup"><span data-stu-id="e567b-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="e567b-141">Observe que hay un vínculo a una hoja de estilos denominada *movies. CSS*.</span><span class="sxs-lookup"><span data-stu-id="e567b-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="e567b-142">La hoja de estilos es donde se definirán los detalles del diseño físico de los elementos.</span><span class="sxs-lookup"><span data-stu-id="e567b-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="e567b-143">Lo creará en un momento.</span><span class="sxs-lookup"><span data-stu-id="e567b-143">You'll create that in a moment.</span></span>

<span data-ttu-id="e567b-144">La única característica inusual en esta *\_página layout. cshtml* es la línea de `@Render.Body()`.</span><span class="sxs-lookup"><span data-stu-id="e567b-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="e567b-145">Este es el marcador de posición al que irá el contenido cuando este diseño se combine con otra página.</span><span class="sxs-lookup"><span data-stu-id="e567b-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="e567b-146">Agregar un archivo. CSS</span><span class="sxs-lookup"><span data-stu-id="e567b-146">Adding a .css File</span></span>

<span data-ttu-id="e567b-147">La mejor manera de definir la disposición real (es decir, el aspecto) de los elementos de la página es usar reglas de hoja de estilos en cascada (CSS).</span><span class="sxs-lookup"><span data-stu-id="e567b-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="e567b-148">Por lo tanto, creará un archivo *. CSS* con las reglas para el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="e567b-149">En WebMatrix, seleccione la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="e567b-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="e567b-150">Después, en la pestaña **archivos** de la cinta de opciones, haga clic en la flecha situada debajo del botón **nuevo** y, a continuación, haga clic en **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="e567b-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![La opción "nueva carpeta" en nuevo en la cinta de opciones.](layouts/_static/image3.png)

<span data-ttu-id="e567b-152">Asigne el nombre *estilos*a la nueva carpeta.</span><span class="sxs-lookup"><span data-stu-id="e567b-152">Name the new folder *Styles*.</span></span>

![Asignar un nombre a la nueva carpeta ' styles '](layouts/_static/image4.png)

<span data-ttu-id="e567b-154">Dentro de la nueva carpeta de *estilos* , cree un archivo denominado *movies. CSS*.</span><span class="sxs-lookup"><span data-stu-id="e567b-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Crear un nuevo archivo movies. CSS](layouts/_static/image5.png)

<span data-ttu-id="e567b-156">Reemplace el contenido del nuevo archivo *. CSS* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e567b-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="e567b-157">No tendremos en cuenta las siguientes reglas de CSS, excepto para anotar dos cosas.</span><span class="sxs-lookup"><span data-stu-id="e567b-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="e567b-158">Una es que, además de establecer fuentes y tamaños, las reglas utilizan el posicionamiento absoluto para establecer la ubicación del encabezado, el pie de página y el área de contenido principal.</span><span class="sxs-lookup"><span data-stu-id="e567b-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="e567b-159">Si no está familiarizado con la posición en CSS, puede leer el tutorial de [posicionamiento de CSS](http://www.w3schools.com/css/css_positioning.asp) en el sitio de w3schools.</span><span class="sxs-lookup"><span data-stu-id="e567b-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="e567b-160">Lo que hay que tener en cuenta es que, en la parte inferior, se han copiado las reglas de estilo que se definieron originalmente individualmente en el archivo *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="e567b-161">Estas reglas se usaron en la [Introducción a la visualización de datos mediante ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para que la aplicación auxiliar de `WebGrid` pueda representar el marcado que agregó bandas a la tabla.</span><span class="sxs-lookup"><span data-stu-id="e567b-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="e567b-162">(Si va a utilizar un archivo *. CSS* para las definiciones de estilo, también puede colocar las reglas de estilo para todo el sitio en ella).</span><span class="sxs-lookup"><span data-stu-id="e567b-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="e567b-163">Actualizar el archivo de películas para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="e567b-164">Ahora puede actualizar los archivos existentes en el sitio para usar el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="e567b-165">Abra el archivo *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="e567b-166">En la parte superior, como la primera línea de código, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e567b-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="e567b-167">La página ahora empieza de esta manera:</span><span class="sxs-lookup"><span data-stu-id="e567b-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="e567b-168">Esta línea de código indica a ASP.NET que, cuando se ejecuta la página de *películas* , debe combinarse con el archivo *\_layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="e567b-169">Dado que el archivo *movies. cshtml* ahora usa una página de diseño, puede quitar el marcado de la página *movies. cshtml* que se encarga de la *\_archivo layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="e567b-170">Saque el `<!DOCTYPE>`, `<html>`y `<body>` etiquetas de apertura y cierre.</span><span class="sxs-lookup"><span data-stu-id="e567b-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="e567b-171">Saque todo el elemento `<head>` y su contenido, que incluye las reglas de estilo de la cuadrícula, ya que ahora tiene esas reglas en un archivo *. CSS* .</span><span class="sxs-lookup"><span data-stu-id="e567b-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="e567b-172">Mientras esté en él, cambie el elemento de `<h1>` existente a un elemento `<h2>`; ya tiene un elemento `<h1>` en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="e567b-173">Cambie el texto de la `<h2>` a "list Movies".</span><span class="sxs-lookup"><span data-stu-id="e567b-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="e567b-174">Normalmente, no tendría que realizar estos tipos de cambios en una página de contenido.</span><span class="sxs-lookup"><span data-stu-id="e567b-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="e567b-175">Al iniciar el sitio con una página de diseño, se crean páginas de contenido sin todos estos elementos para comenzar.</span><span class="sxs-lookup"><span data-stu-id="e567b-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="e567b-176">Sin embargo, en este caso, va a convertir una página independiente en una que usa un diseño, por lo que hay un poco de limpieza.</span><span class="sxs-lookup"><span data-stu-id="e567b-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="e567b-177">Cuando haya terminado, la página *movies. cshtml* tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="e567b-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="e567b-178">Probar el diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-178">Testing the Layout</span></span>

<span data-ttu-id="e567b-179">Ahora puede ver el aspecto del diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="e567b-180">En WebMatrix, haga clic con el botón derecho en la página *movies. cshtml* y seleccione **iniciar en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="e567b-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="e567b-181">Cuando el explorador muestra la página, se parece a esta página:</span><span class="sxs-lookup"><span data-stu-id="e567b-181">When the browser displays the page, it looks like this page:</span></span>

![Página de películas representada mediante un diseño](layouts/_static/image6.png)

<span data-ttu-id="e567b-183">ASP.NET ha combinado el contenido de la página movies. cshtml en la página *\_layout. cshtml* en la que el método de `RenderBody` es.</span><span class="sxs-lookup"><span data-stu-id="e567b-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="e567b-184">Y, por supuesto, la página *\_layout. cshtml* hace referencia a un archivo *. CSS* que define el aspecto de la página.</span><span class="sxs-lookup"><span data-stu-id="e567b-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="e567b-185">Actualización de la página AddMovie para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="e567b-186">La ventaja real de los diseños es que puede usarlos para todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="e567b-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="e567b-187">Abra la página *AddMovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="e567b-188">Es posible que recuerde que en la página *AddMovie. cshtml* tenía algunas reglas CSS para definir la apariencia de los mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="e567b-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="e567b-189">Puesto que ahora tiene un archivo *. CSS* para su sitio, puede trasladar esas reglas al archivo *. CSS* .</span><span class="sxs-lookup"><span data-stu-id="e567b-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="e567b-190">Quítelos del archivo *AddMovie. cshtml* y agréguelos a la parte inferior del archivo *movies. CSS* .</span><span class="sxs-lookup"><span data-stu-id="e567b-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="e567b-191">Va a mover las siguientes reglas:</span><span class="sxs-lookup"><span data-stu-id="e567b-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="e567b-192">Ahora realice el mismo tipo de cambios en *AddMovie. cshtml* que hizo para *movies. cshtml* : agregue `Layout="~/_Layout.cshtml;` y quite el marcado HTML que ahora es extraño.</span><span class="sxs-lookup"><span data-stu-id="e567b-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="e567b-193">Cambie el elemento `<h1>` por `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="e567b-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="e567b-194">Cuando haya terminado, la página tendrá un aspecto similar al de este ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e567b-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="e567b-195">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="e567b-195">Run the page.</span></span> <span data-ttu-id="e567b-196">Ahora tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="e567b-196">Now it looks like this illustration:</span></span>

![Página ' Agregar películas ' representada mediante un diseño](layouts/_static/image7.png)

<span data-ttu-id="e567b-198">Quiere realizar cambios similares en las páginas del sitio: *EditMovie. cshtml* y *DeleteMovie. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="e567b-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="e567b-199">Sin embargo, antes de hacerlo, puede realizar otro cambio en el diseño que lo hace un poco más flexible.</span><span class="sxs-lookup"><span data-stu-id="e567b-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="e567b-200">Pasar información de título a la página de diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="e567b-201">La página *\_layout. cshtml* que ha creado tiene un elemento `<title>` que se establece en "mi sitio de película".</span><span class="sxs-lookup"><span data-stu-id="e567b-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="e567b-202">La mayoría de los exploradores muestran el contenido de este elemento como texto en una pestaña:</span><span class="sxs-lookup"><span data-stu-id="e567b-202">Most browsers display the content of this element as the text on a tab:</span></span>

![El título de la &lt;de la página&gt; elemento que se muestra en una pestaña del explorador](layouts/_static/image8.png)

<span data-ttu-id="e567b-204">Esta información de título es genérica.</span><span class="sxs-lookup"><span data-stu-id="e567b-204">This title information is generic.</span></span> <span data-ttu-id="e567b-205">Supongamos que desea que el texto del título sea más específico en la página actual.</span><span class="sxs-lookup"><span data-stu-id="e567b-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="e567b-206">(Los motores de búsqueda también usan el texto del título para determinar el contenido de la página). Puede pasar información de una página de contenido como *movies. cshtml* o *AddMovie. cshtml* a la página de diseño y, a continuación, utilizar esa información para personalizar lo que representa la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="e567b-207">Vuelva a abrir la página *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="e567b-208">En el código de la parte superior, agregue la siguiente línea:</span><span class="sxs-lookup"><span data-stu-id="e567b-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="e567b-209">El objeto `Page` está disponible en todas las páginas *. cshtml* y es para este fin, es decir, para compartir información entre una página y su diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="e567b-210">Abra la página *\_layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-210">Open the *\_Layout.cshtml* page.</span></span> <span data-ttu-id="e567b-211">Cambie el elemento `<title>` para que tenga el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="e567b-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="e567b-212">Este código representa todo lo que hay en la propiedad `Page.Title` derecha en esa ubicación de la página.</span><span class="sxs-lookup"><span data-stu-id="e567b-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="e567b-213">Ejecute la página *movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="e567b-214">Esta vez, la pestaña del explorador muestra lo que pasó como el valor de `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="e567b-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Una pestaña del explorador que muestra el título creado dinámicamente](layouts/_static/image9.png)

<span data-ttu-id="e567b-216">Si lo desea, vea el origen de la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="e567b-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="e567b-217">Puede ver que el elemento `<title>` se representa como `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="e567b-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="e567b-218">**El objeto Page**</span><span class="sxs-lookup"><span data-stu-id="e567b-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="e567b-219">Una característica útil de `Page` es que es un objeto dinámico: la propiedad `Title` no es un nombre fijo o reservado.</span><span class="sxs-lookup"><span data-stu-id="e567b-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="e567b-220">Puede utilizar *cualquier* nombre para un valor del objeto `Page`.</span><span class="sxs-lookup"><span data-stu-id="e567b-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="e567b-221">Por ejemplo, puede que le resulte tan fácil pasar el título mediante una propiedad denominada `Page.CurrentName` o `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="e567b-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="e567b-222">La única restricción es que el nombre debe seguir las reglas normales para las propiedades a las que se puede asignar un nombre.</span><span class="sxs-lookup"><span data-stu-id="e567b-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="e567b-223">(Por ejemplo, el nombre no puede contener un espacio).</span><span class="sxs-lookup"><span data-stu-id="e567b-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="e567b-224">Puede pasar cualquier número de valores mediante el `Page` objeto.</span><span class="sxs-lookup"><span data-stu-id="e567b-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="e567b-225">Si desea pasar información de películas a la página de diseño, puede pasar valores mediante algo como `Page.MovieTitle` y `Page.Genre` y `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="e567b-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="e567b-226">(O cualquier otro nombre que se haya inventado para almacenar la información). El único requisito, que es probablemente obvio, es que tiene que usar los mismos nombres en la página de contenido y en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="e567b-227">La información que se pasa mediante el objeto `Page` no se limita al texto que se va a mostrar en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="e567b-228">Puede pasar un valor a la página de diseño y, a continuación, el código de la página de diseño puede utilizar el valor para decidir si se va a mostrar una sección de la página, el archivo *. CSS* que se va a usar, etc.</span><span class="sxs-lookup"><span data-stu-id="e567b-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="e567b-229">Los valores que se pasan en el objeto `Page` son como cualquier otro valor que se utilice en el código.</span><span class="sxs-lookup"><span data-stu-id="e567b-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="e567b-230">Es simplemente que los valores se originan en la página de contenido y se pasan a la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>

<span data-ttu-id="e567b-231">Abra la página *AddMovie. cshtml* y agregue una línea en la parte superior del código que proporciona un título para la página *AddMovie. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e567b-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="e567b-232">Ejecute la página *AddMovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e567b-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="e567b-233">Verá el nuevo título allí:</span><span class="sxs-lookup"><span data-stu-id="e567b-233">You see the new title there:</span></span>

![Una pestaña del explorador que muestra el título "agregar películas" creado dinámicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="e567b-235">Actualizar las páginas restantes para usar el diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="e567b-236">Ahora puede finalizar las páginas restantes del sitio para que usen el nuevo diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="e567b-237">Abra *EditMovie. cshtml* y *DeleteMovie. cshtml* a su vez y realice los mismos cambios en cada uno.</span><span class="sxs-lookup"><span data-stu-id="e567b-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="e567b-238">Agregue la línea de código que se vincula a la página de diseño:</span><span class="sxs-lookup"><span data-stu-id="e567b-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="e567b-239">Agregue una línea para establecer el título de la página:</span><span class="sxs-lookup"><span data-stu-id="e567b-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="e567b-240">O bien</span><span class="sxs-lookup"><span data-stu-id="e567b-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="e567b-241">Quitar todo el marcado HTML extraño: básicamente, deje solo los bits que se encuentran dentro del elemento `<body>` (más el bloque de código en la parte superior).</span><span class="sxs-lookup"><span data-stu-id="e567b-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="e567b-242">Cambie el elemento `<h1>` para que sea un elemento `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="e567b-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="e567b-243">Cuando haya realizado estos cambios, pruebe cada uno de ellos y asegúrese de que se muestra correctamente y de que el título es correcto.</span><span class="sxs-lookup"><span data-stu-id="e567b-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="e567b-244">Consideraciones sobre las páginas de diseño</span><span class="sxs-lookup"><span data-stu-id="e567b-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="e567b-245">En este tutorial, ha creado una *\_página layout. cshtml* y ha usado el método `RenderBody` para combinar el contenido de otra página.</span><span class="sxs-lookup"><span data-stu-id="e567b-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="e567b-246">Este es el patrón básico para usar diseños en páginas Web.</span><span class="sxs-lookup"><span data-stu-id="e567b-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="e567b-247">Las páginas de diseño tienen características adicionales que no tratamos aquí.</span><span class="sxs-lookup"><span data-stu-id="e567b-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="e567b-248">Por ejemplo, puede anidar las páginas de diseño; una página de diseño puede, a su vez, hacer referencia a otra.</span><span class="sxs-lookup"><span data-stu-id="e567b-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="e567b-249">Los diseños anidados pueden ser útiles si está trabajando con subsecciones de un sitio que requiere diseños diferentes.</span><span class="sxs-lookup"><span data-stu-id="e567b-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="e567b-250">También puede usar métodos adicionales (por ejemplo, `RenderSection`) para configurar secciones con nombre en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="e567b-251">La combinación de las páginas de diseño y los archivos *. CSS* es eficaz.</span><span class="sxs-lookup"><span data-stu-id="e567b-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="e567b-252">Como verá en la siguiente serie de tutoriales, en WebMatrix, puede crear un sitio basado en una *plantilla*, lo que le ofrece un sitio que tiene una funcionalidad pregenerada.</span><span class="sxs-lookup"><span data-stu-id="e567b-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="e567b-253">Las plantillas hacen un uso correcto de las páginas de diseño y CSS para crear sitios que tienen un aspecto excelente y que tienen características como menús.</span><span class="sxs-lookup"><span data-stu-id="e567b-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="e567b-254">Esta es una captura de pantalla de la Página principal de un sitio basado en una plantilla, que muestra las características que usan páginas de diseño y CSS:</span><span class="sxs-lookup"><span data-stu-id="e567b-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Diseño creado por plantilla de sitio de WebMatrix que muestra el encabezado, el área de navegación, el área de contenido, la sección opcional y los vínculos de inicio de sesión](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="e567b-256">Lista completa de la página de película (actualizada para usar una página de diseño)</span><span class="sxs-lookup"><span data-stu-id="e567b-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="e567b-257">Completar la lista de páginas para agregar una página de película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e567b-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="e567b-258">Completar la lista de páginas para la página eliminar película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e567b-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="e567b-259">Completar la lista de páginas de la página Editar película (actualizada para el diseño)</span><span class="sxs-lookup"><span data-stu-id="e567b-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="e567b-260">Próxima</span><span class="sxs-lookup"><span data-stu-id="e567b-260">Coming Up Next</span></span>

<span data-ttu-id="e567b-261">En el siguiente tutorial, aprenderá a publicar su sitio en Internet para que todos los usuarios puedan verlo.</span><span class="sxs-lookup"><span data-stu-id="e567b-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e567b-262">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e567b-262">Additional Resources</span></span>

- <span data-ttu-id="e567b-263">[Creación de una apariencia coherente](https://go.microsoft.com/fwlink/?LinkID=202891) : un artículo que proporciona información más detallada sobre cómo trabajar con diseños.</span><span class="sxs-lookup"><span data-stu-id="e567b-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="e567b-264">También se describe cómo pasar un valor a una página de diseño que muestra u oculta parte del contenido.</span><span class="sxs-lookup"><span data-stu-id="e567b-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="e567b-265">[Páginas de diseño anidadas con Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) : blogs de Mike salmuera un ejemplo de cómo anidar páginas de diseño.</span><span class="sxs-lookup"><span data-stu-id="e567b-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="e567b-266">(Incluye una descarga de las páginas).</span><span class="sxs-lookup"><span data-stu-id="e567b-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e567b-267">[Anterior](deleting-data.md)
> [Siguiente](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="e567b-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
