---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usar Inspector de página en MVC de ASP.NET | Microsoft Docs
author: rick-anderson
description: Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432457"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a><span data-ttu-id="8c574-104">Usar el Inspector de página en ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8c574-104">Using Page Inspector in ASP.NET MVC</span></span>

<span data-ttu-id="8c574-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="8c574-105">by Tim Ammann</span></span>

> <span data-ttu-id="8c574-106">Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="8c574-106">Page Inspector in Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="8c574-107">Seleccione cualquier elemento en el explorador integrado y Inspector de página resaltará al instante el origen y el código CSS del elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="8c574-108">Puede examinar cualquier vista de MVC, buscar rápidamente los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c574-108">You can browse any MVC view, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> [<span data-ttu-id="8c574-109">Ver el vídeo</span><span class="sxs-lookup"><span data-stu-id="8c574-109">Watch the Video</span></span>](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> <span data-ttu-id="8c574-110">En este tutorial se muestra cómo habilitar Modo de inspección y, a continuación, ubicar y editar rápidamente el marcado y CSS en el proyecto Web.</span><span class="sxs-lookup"><span data-stu-id="8c574-110">This tutorial shows how to enable Inspection Mode, and then quickly locate and edit markup and CSS within your web project.</span></span> <span data-ttu-id="8c574-111">En el tutorial se usa un proyecto de MVC, pero también puede usar Inspector de página para [formularios Web Forms](https://go.microsoft.com/?linkid=9802001) y otras aplicaciones ASP.net.</span><span class="sxs-lookup"><span data-stu-id="8c574-111">The tutorial uses an MVC Project, but you can also use Page Inspector for [Web Forms](https://go.microsoft.com/?linkid=9802001) and other ASP.NET applications.</span></span>
> 
> <span data-ttu-id="8c574-112">El tutorial consta de las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="8c574-112">The tutorial has the following sections:</span></span>
> 
> - [<span data-ttu-id="8c574-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8c574-113">Prerequisites</span></span>](#_1_prerequisites)
> - [<span data-ttu-id="8c574-114">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="8c574-114">Create a Web Application</span></span>](#_2_creating_a)
> - [<span data-ttu-id="8c574-115">Usar Inspector de página para ir a una vista</span><span class="sxs-lookup"><span data-stu-id="8c574-115">Use Page Inspector to Browse to a View</span></span>](#_3_using_page)
> - [<span data-ttu-id="8c574-116">Habilitar Modo de inspección</span><span class="sxs-lookup"><span data-stu-id="8c574-116">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> - [<span data-ttu-id="8c574-117">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="8c574-117">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> - [<span data-ttu-id="8c574-118">Modo de inspección y la ventana HTML</span><span class="sxs-lookup"><span data-stu-id="8c574-118">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> - [<span data-ttu-id="8c574-119">Vista previa de los cambios de CSS en la ventana estilos</span><span class="sxs-lookup"><span data-stu-id="8c574-119">Preview CSS Changes in the Styles window</span></span>](#_7_previewing_css)
> - [<span data-ttu-id="8c574-120">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="8c574-120">CSS Auto Sync</span></span>](#css_auto_sync)
> - [<span data-ttu-id="8c574-121">Usar el selector de colores de CSS</span><span class="sxs-lookup"><span data-stu-id="8c574-121">Using the CSS Color Picker</span></span>](#css_color_picker)
> - [<span data-ttu-id="8c574-122">Asignar elementos de página dinámicos a JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c574-122">Mapping Dynamic Page Elements to JavaScript</span></span>](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="8c574-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8c574-123">Prerequisites</span></span>

- <span data-ttu-id="8c574-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="8c574-124">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="8c574-125">Para obtener la versión más reciente de Inspector de página, use el [instalador de plataforma web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Windows Azure para .net 2,0.</span><span class="sxs-lookup"><span data-stu-id="8c574-125">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Windows Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="8c574-126">Inspector de página se incluye con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="8c574-126">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="8c574-127">La versión más reciente es 1,3.</span><span class="sxs-lookup"><span data-stu-id="8c574-127">The latest version is 1.3.</span></span> <span data-ttu-id="8c574-128">Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** en el menú **ayuda** .</span><span class="sxs-lookup"><span data-stu-id="8c574-128">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="8c574-129">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="8c574-129">Create a Web Application</span></span>

<span data-ttu-id="8c574-130">En primer lugar, cree una aplicación web que utilizará Inspector de página con.</span><span class="sxs-lookup"><span data-stu-id="8c574-130">First, create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="8c574-131">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8c574-131">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="8c574-132">A la izquierda, expanda **C#visualmente**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.net MVC4**.</span><span class="sxs-lookup"><span data-stu-id="8c574-132">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span>

![Nueva aplicación ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

<span data-ttu-id="8c574-134">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8c574-134">Click **OK**.</span></span>

<span data-ttu-id="8c574-135">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="8c574-135">In the **New ASP.NET MVC 4 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="8c574-136">Deje **Razor** como el motor de vista predeterminado.</span><span class="sxs-lookup"><span data-stu-id="8c574-136">Leave **Razor** as the default view engine.</span></span>

![Nuevo proyecto de ASP.NET MVC: aplicación de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

<span data-ttu-id="8c574-138">La aplicación se abre en la vista **código fuente** .</span><span class="sxs-lookup"><span data-stu-id="8c574-138">The application opens in **Source** view.</span></span>

![Nueva aplicación ASP.NET MVC en la vista Código fuente](using-page-inspector-in-aspnet-mvc/_static/image6.png)

<span data-ttu-id="8c574-140">Ahora que tiene una aplicación con la que trabajar, puede usar Inspector de página para examinarla y modificarla.</span><span class="sxs-lookup"><span data-stu-id="8c574-140">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a><span data-ttu-id="8c574-141">Usar Inspector de página para ir a una vista</span><span class="sxs-lookup"><span data-stu-id="8c574-141">Use Page Inspector to Browse to a View</span></span>

<span data-ttu-id="8c574-142">En Visual Studio 2012, puede hacer clic con el botón derecho en cualquier vista del proyecto, seleccionar **ver en inspector de página**y inspector de página desfigurará la ruta y mostrará la página.</span><span class="sxs-lookup"><span data-stu-id="8c574-142">In Visual Studio 2012, you can right-click any view in your project, select **View in Page Inspector**, and Page Inspector will figure out the route and display the page.</span></span>

<span data-ttu-id="8c574-143">En **Explorador de soluciones**, expanda la carpeta **vistas** y, a continuación, la carpeta **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="8c574-143">In **Solution Explorer**, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="8c574-144">Haga clic con el botón derecho en el archivo index. cshtml y elija **ver en inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="8c574-144">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

![Ver index. cshtml en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

<span data-ttu-id="8c574-146">De forma predeterminada, Inspector de página se acopla como una ventana en el lado izquierdo del entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c574-146">By default, Page Inspector is docked as a window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="8c574-147">Si lo prefiere, puede acoplarlo en otro lugar o desacoplar la ventana.</span><span class="sxs-lookup"><span data-stu-id="8c574-147">If you prefer, you can dock it elsewhere, or undock the window.</span></span> <span data-ttu-id="8c574-148">Consulte [Cómo: organizar y acoplar ventanas](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c574-148">See [How to: Arrange and Dock Windows](https://msdn.microsoft.com/library/z4y0hsax.aspx).</span></span>

<span data-ttu-id="8c574-149">En el panel superior de la ventana de Inspector de página se muestra la página actual en una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="8c574-149">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="8c574-150">En el panel inferior se muestra la página en formato HTML, junto con algunas pestañas que permiten inspeccionar distintos aspectos de la página.</span><span class="sxs-lookup"><span data-stu-id="8c574-150">The bottom pane shows the page in HTML markup, along with some tabs that let you inspect different aspects of the page.</span></span> <span data-ttu-id="8c574-151">El panel inferior es similar al [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="8c574-151">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span>

![Aplicación ASP.NET MVC en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

<span data-ttu-id="8c574-153">En este tutorial, usará las pestañas **HTML** y **styles** para navegar rápidamente y realizar cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8c574-153">In this tutorial, you will use the **HTML** and **Styles** tabs to navigate quickly and make changes to the application.</span></span>

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a><span data-ttu-id="8c574-154">Modo EnableInspection</span><span class="sxs-lookup"><span data-stu-id="8c574-154">EnableInspection Mode</span></span>

<span data-ttu-id="8c574-155">Para colocar Inspector de página en Modo de inspección, haga clic en el botón **inspeccionar** .</span><span class="sxs-lookup"><span data-stu-id="8c574-155">To put Page Inspector into Inspection Mode, click the **Inspect** button.</span></span> <span data-ttu-id="8c574-156">En Modo de inspección, cuando se mantiene presionado el puntero del mouse sobre cualquier parte de la página representada, se resalta el código fuente correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8c574-156">In Inspection Mode, when you hold the mouse pointer over any part of the rendered page, the corresponding source markup or code is highlighted.</span></span>

![Alternar Modo de inspección](using-page-inspector-in-aspnet-mvc/_static/image12.png)

<span data-ttu-id="8c574-158">Mueva el mouse sobre distintas partes de la página dentro de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-158">Now move your mouse over different parts of the page within Page Inspector.</span></span> <span data-ttu-id="8c574-159">Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:</span><span class="sxs-lookup"><span data-stu-id="8c574-159">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

<span data-ttu-id="8c574-161">A medida que mueve el puntero del mouse, Visual Studio resalta el sintaxis Razor correspondiente en el archivo de código fuente.</span><span class="sxs-lookup"><span data-stu-id="8c574-161">As you move the mouse pointer, Visual Studio highlights the corresponding Razor syntax in the source file.</span></span> <span data-ttu-id="8c574-162">Si el elemento HTML procede de otro archivo de código fuente, Visual Studio abre automáticamente el archivo.</span><span class="sxs-lookup"><span data-stu-id="8c574-162">If the HTML element comes from another source file, Visual Studio automatically opens the file.</span></span>

<span data-ttu-id="8c574-163">En Inspector de página, la pestaña **HTML** muestra el código HTML que se generó a partir del sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="8c574-163">In Page Inspector, the **HTML** tab shows the HTML that was generated from the Razor syntax.</span></span> <span data-ttu-id="8c574-164">A medida que mueve el puntero del mouse, se resaltan los elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="8c574-164">As you move the mouse pointer, the HTML elements are highlighted.</span></span> <span data-ttu-id="8c574-165">En la pestaña **estilos** se muestran las reglas de CSS para el elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-165">The **Styles** tab shows the CSS rules for the element.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="8c574-166">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="8c574-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="8c574-167">Inspector de página permite buscar el marcado cuya ubicación podría no ser obvia.</span><span class="sxs-lookup"><span data-stu-id="8c574-167">Page Inspector lets you find markup whose location might not be obvious.</span></span> <span data-ttu-id="8c574-168">Después, puede modificar el marcado y ver los cambios resultantes.</span><span class="sxs-lookup"><span data-stu-id="8c574-168">Then you can modify the markup and see the resulting changes.</span></span>

<span data-ttu-id="8c574-169">Para verlo, haga clic en **inspeccionar** y, a continuación, desplácese hasta la parte inferior de la página en la ventana de inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-169">To see this, click **Inspect** and then scroll to the bottom of the page in the Page Inspector window.</span></span>

<span data-ttu-id="8c574-170">Al colocar el puntero del mouse en el área de pie de página, Inspector de página abre el archivo \_layout. cshtml y resalta la sección de la página de diseño que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="8c574-170">When you move the mouse pointer into the footer area, Page Inspector opens the \_Layout.cshtml file and highlights the section of the layout page that you have selected.</span></span> <span data-ttu-id="8c574-171">Como puede ver, el pie de página se define en el archivo de diseño y no en la propia vista.</span><span class="sxs-lookup"><span data-stu-id="8c574-171">As you can see, the footer are is defined in the layout file, and not the view itself.</span></span>

![Pie de página](using-page-inspector-in-aspnet-mvc/_static/image16.png)

<span data-ttu-id="8c574-173">Ahora, mueva el puntero del mouse sobre la línea con <a id="a"> </a>el aviso de propiedad intelectual.</span><span class="sxs-lookup"><span data-stu-id="8c574-173">Now move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span> <span data-ttu-id="8c574-174">En la página \_layout. cshtml, se resalta la línea correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8c574-174">In the \_Layout.cshtml page, the corresponding line is highlighted.</span></span>

![Línea de copyright de pie de página resaltada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

<span data-ttu-id="8c574-176">Agregue texto al final de la línea en el archivo \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="8c574-176">Add some text to the end of the line in the \_Layout.cshtml file.</span></span>

<span data-ttu-id="8c574-177">&lt;p&gt;&amp;Copy; @DateTime.Now.Year de la aplicación ASP.NET MVC Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="8c574-177">&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET MVC Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="8c574-178">Ahora, presione Ctrl + Alt + entrar o haga clic en la barra de actualización para ver los resultados en la ventana del explorador Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-178">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![¡ Mi aplicación ASP.NET Rock!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

<span data-ttu-id="8c574-180">Es posible que haya pensado que el pie de página está definido en index. cshtml, pero que está en el \_layout. cshtml y Inspector de página lo ha encontrado.</span><span class="sxs-lookup"><span data-stu-id="8c574-180">You might have thought that the footer defined in Index.cshtml, but it turned out to be in the \_Layout.cshtml, and Page Inspector found it for you.</span></span>

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="8c574-181">Modo de inspección y la ventana HTML</span><span class="sxs-lookup"><span data-stu-id="8c574-181">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="8c574-182">A continuación, tendrá una vista rápida de la ventana HTML y cómo asigna los elementos.</span><span class="sxs-lookup"><span data-stu-id="8c574-182">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="8c574-183">Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="8c574-183">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="8c574-184">Haga clic en la parte superior de la página que dice "su logotipo aquí".</span><span class="sxs-lookup"><span data-stu-id="8c574-184">Click the top part of the page that says "Your logo here".</span></span> <span data-ttu-id="8c574-185">Está examinando un elemento determinado con más detalle, de modo que la presentación en la ventana del explorador ya no cambia a medida que mueve el puntero del mouse.</span><span class="sxs-lookup"><span data-stu-id="8c574-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="8c574-186">Ahora, mueva el puntero del mouse a la ventana **HTML** .</span><span class="sxs-lookup"><span data-stu-id="8c574-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="8c574-187">A medida que mueve el puntero del mouse, Inspector de página describe el elemento en la ventana **HTML** y resalta el elemento correspondiente en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="8c574-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Ventana HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

<span data-ttu-id="8c574-189">Como antes, Inspector de página abre el archivo \_layout. cshtml en una pestaña temporal. Haga clic en la ficha temporal \_layout. cshtml y el marcado correspondiente se resaltará en el encabezado &lt;&gt; sección:</span><span class="sxs-lookup"><span data-stu-id="8c574-189">As before, Page Inspector opens the \_Layout.cshtml file for you in a temporary tab. Click the \_Layout.cshtml temporary tab, and the corresponding markup will be highlighted in the &lt;header&gt; section for you:</span></span>

![Marcado resaltado](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="8c574-191">Vista previa de los cambios de CSS en la ventana estilos</span><span class="sxs-lookup"><span data-stu-id="8c574-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="8c574-192">A continuación, usará la ventana **estilos** de inspector de página para obtener una vista previa de los cambios en CSS.</span><span class="sxs-lookup"><span data-stu-id="8c574-192">Next, you will use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="8c574-193">Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="8c574-193">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="8c574-194">En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="8c574-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

<span data-ttu-id="8c574-196">Haga clic en la sección div. Content-wrapper una vez y, a continuación, mueva el puntero del mouse a la ventana **estilos** .</span><span class="sxs-lookup"><span data-stu-id="8c574-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="8c574-197">La ventana **estilos** muestra todas las reglas de CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-197">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="8c574-198">Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="8c574-198">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="8c574-199">Ahora desactive la casilla de la propiedad background-color.</span><span class="sxs-lookup"><span data-stu-id="8c574-199">Now clear the checkbox for the background-color property.</span></span>

![Borrar color de fondo](using-page-inspector-in-aspnet-mvc/_static/image28.png)

<span data-ttu-id="8c574-201">Observe cómo la vista previa de cambios se muestra al instante en la ventana del explorador Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-201">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="8c574-202">Vuelva a activar la casilla de verificación y, a continuación, haga doble clic en el valor de la propiedad y cámbielo a rojo.</span><span class="sxs-lookup"><span data-stu-id="8c574-202">Select the checkbox again, then double-click the property value and change it to red.</span></span> <span data-ttu-id="8c574-203">El cambio se muestra inmediatamente:</span><span class="sxs-lookup"><span data-stu-id="8c574-203">The change shows immediately:</span></span>

![Color de fondo rojo](using-page-inspector-in-aspnet-mvc/_static/image30.png)

<span data-ttu-id="8c574-205">La ventana **estilos** facilita la prueba y la vista previa de los cambios de CSS antes de confirmar los cambios en la propia hoja de estilos.</span><span class="sxs-lookup"><span data-stu-id="8c574-205">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="8c574-206">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="8c574-206">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="8c574-207">Esta característica requiere la versión 1,3 de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-207">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="8c574-208">La característica de sincronización automática de CSS le permite editar un archivo CSS directamente y ver los cambios inmediatamente en el explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-208">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="8c574-209">Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="8c574-209">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="8c574-210">En el explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="8c574-210">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="8c574-211">Haga clic en una vez para seleccionar este elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-211">Click once to select this element.</span></span>

<span data-ttu-id="8c574-212">La ventana **estilos** muestra todas las reglas de CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-212">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="8c574-213">Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="8c574-213">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="8c574-214">Haga clic en ". destacado. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="8c574-214">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="8c574-215">Inspector de página abre el archivo CSS que define este estilo (site. CSS) y resalta el estilo CSS correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8c574-215">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

<span data-ttu-id="8c574-216">Ahora, cambie el valor de `background-color` por "rojo".</span><span class="sxs-lookup"><span data-stu-id="8c574-216">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="8c574-217">El cambio aparece inmediatamente en el explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-217">The change appears immediately in the Page Inspector browser.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a><span data-ttu-id="8c574-218">Usar el selector de colores de CSS</span><span class="sxs-lookup"><span data-stu-id="8c574-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="8c574-219">El editor de CSS de Visual Studio 2012 tiene un selector de colores que facilita la elección y la inserción de colores.</span><span class="sxs-lookup"><span data-stu-id="8c574-219">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="8c574-220">El selector de colores incluye una paleta estándar de colores, admite los nombres de colores estándar, códigos hash, RGB, RGBA, HSL y HSLA, y mantiene una lista de los colores usados más recientemente en el documento.</span><span class="sxs-lookup"><span data-stu-id="8c574-220">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="8c574-221">En la sección anterior, cambió el valor de la propiedad `background-color`.</span><span class="sxs-lookup"><span data-stu-id="8c574-221">In the previous section, you changed the value of the `background-color` property.</span></span> <span data-ttu-id="8c574-222">Para invocar el selector de colores, coloque el punto de inserción después del nombre y el tipo de la propiedad **#** o **RGB (** .</span><span class="sxs-lookup"><span data-stu-id="8c574-222">To invoke the color picker, place the insertion point after the property name and type **#** or **rgb(**.</span></span>

![Barra de selector de colores de CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

<span data-ttu-id="8c574-224">Haga clic en un color para seleccionarlo o presione la tecla flecha abajo y, a continuación, use las teclas de flecha izquierda y derecha para atravesar los colores.</span><span class="sxs-lookup"><span data-stu-id="8c574-224">Click on a color to select it, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="8c574-225">Al visitar un color, se muestra una vista previa del valor hexadecimal correspondiente:</span><span class="sxs-lookup"><span data-stu-id="8c574-225">When you visit a color, the corresponding hex value is previewed:</span></span>

![vista previa del valor de propiedad de color de fondo](using-page-inspector-in-aspnet-mvc/_static/image38.png)

<span data-ttu-id="8c574-227">Si la barra de colores no tiene el color exacto que desea, puede usar la ventana emergente selector de colores.</span><span class="sxs-lookup"><span data-stu-id="8c574-227">If the color bar doesn't have the exact color you want, you can use the color picker pop-down.</span></span> <span data-ttu-id="8c574-228">Para abrirlo, haga clic en el botón de contenido adicional en el extremo derecho de la barra de colores o presione la flecha hacia abajo una o dos veces en el teclado.</span><span class="sxs-lookup"><span data-stu-id="8c574-228">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Cuadro emergente del selector de colores de CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

<span data-ttu-id="8c574-230">Haga clic en un color de la barra vertical de la derecha.</span><span class="sxs-lookup"><span data-stu-id="8c574-230">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="8c574-231">Esto muestra un degradado para ese color en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="8c574-231">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="8c574-232">Elija un color directamente en la barra vertical presionando entrar o haga clic en cualquier punto de la ventana principal para elegir con mayor precisión.</span><span class="sxs-lookup"><span data-stu-id="8c574-232">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="8c574-233">Si hay un color en la pantalla del equipo que desea utilizar (no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta Cuentagotas de la parte inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="8c574-233">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="8c574-234">También puede cambiar la opacidad de un color moviendo el control deslizante situado en la parte inferior del selector de colores.</span><span class="sxs-lookup"><span data-stu-id="8c574-234">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="8c574-235">Al hacerlo, se cambian los valores de color a los valores RGBA, ya que el formato RGBA puede representar la opacidad.</span><span class="sxs-lookup"><span data-stu-id="8c574-235">Doing so changes color values to RGBA values, because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="8c574-236">Después de elegir un color, presione entrar y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el archivo *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="8c574-236">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="8c574-237">Barra de actualización Inspector de página</span><span class="sxs-lookup"><span data-stu-id="8c574-237">The Page Inspector Update Bar</span></span>

<span data-ttu-id="8c574-238">Inspector de página detecta inmediatamente el cambio en el archivo *site. CSS* y muestra una alerta en una barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="8c574-238">Page Inspector immediately detects the change to the *Site.css* file and displays an alert in an update bar.</span></span>

![Barra de actualización](using-page-inspector-in-aspnet-mvc/_static/image42.png)

<span data-ttu-id="8c574-240">Para guardar todos los archivos y actualizar el explorador de Inspector de página, presione Ctrl + Alt + entrar o haga clic en la barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="8c574-240">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="8c574-241">El cambio en el color de resaltado aparece en el explorador.</span><span class="sxs-lookup"><span data-stu-id="8c574-241">The change in the highlight color appears in the browser.</span></span>

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a><span data-ttu-id="8c574-242">Asignar elementos de página dinámicos a JavaScript</span><span class="sxs-lookup"><span data-stu-id="8c574-242">Mapping Dynamic Page Elements to JavaScript</span></span>

<span data-ttu-id="8c574-243">En las aplicaciones web modernas, los elementos de la página se suelen generar dinámicamente con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8c574-243">In modern web applications, elements in the page are often generated dynamically with JavaScript.</span></span> <span data-ttu-id="8c574-244">Esto significa que no hay ningún marcado estático (HTML o Razor) que se corresponda con estos elementos de página.</span><span class="sxs-lookup"><span data-stu-id="8c574-244">That means there is no static markup (either HTML or Razor) that corresponds to these page elements.</span></span>

<span data-ttu-id="8c574-245">Con la versión 1,3, Inspector de página ahora pueden asignar elementos que se agregaron dinámicamente a la página de nuevo al código JavaScript correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8c574-245">With version 1.3, Page Inspector can now map items that were dynamically added to the page back to the corresponding JavaScript code.</span></span> <span data-ttu-id="8c574-246">Para demostrar esta característica, usaremos la [plantilla de aplicación de una sola página (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="8c574-246">To demonstrate this feature, we'll use the [Single Page Application (SPA) template](../../../single-page-application/overview/introduction/knockoutjs-template.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8c574-247">La plantilla SPA requiere la actualización [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .</span><span class="sxs-lookup"><span data-stu-id="8c574-247">The SPA template requires the [ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) update.</span></span>

<span data-ttu-id="8c574-248">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8c574-248">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="8c574-249">A la izquierda, expanda **C#visualmente**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.net MVC4**.</span><span class="sxs-lookup"><span data-stu-id="8c574-249">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET MVC4 Web Application**.</span></span> <span data-ttu-id="8c574-250">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8c574-250">Click **OK**.</span></span>

<span data-ttu-id="8c574-251">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de una sola página**.</span><span class="sxs-lookup"><span data-stu-id="8c574-251">In the **New ASP.NET MVC 4 Project** dialog, select **Single Page Application**.</span></span>

<span data-ttu-id="8c574-252">En Explorador de soluciones, expanda la carpeta **vistas** y, a continuación, la carpeta **Inicio** .</span><span class="sxs-lookup"><span data-stu-id="8c574-252">In Solution Explorer, expand the **Views** folder and then the **Home** folder.</span></span> <span data-ttu-id="8c574-253">Haga clic con el botón derecho en el archivo index. cshtml y elija **ver en inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="8c574-253">Right click the Index.cshtml file and choose **View in Page Inspector**.</span></span>

<span data-ttu-id="8c574-254">Lo primero que se muestra en Inspector de página explorador es una página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="8c574-254">The first thing that is displayed in the Page Inspector browser is a login page.</span></span> <span data-ttu-id="8c574-255">Haga clic en "registrarse" y cree un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="8c574-255">Click "Sign Up" and create a user name and password.</span></span> <span data-ttu-id="8c574-256">Una vez que se suscribe, la aplicación inicia sesión y crea una lista de tareas pendientes con algunos elementos de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="8c574-256">Once you sign up, the application logs you in and creates a to-do list with some sample items.</span></span>

<span data-ttu-id="8c574-257">Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="8c574-257">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span> <span data-ttu-id="8c574-258">En el explorador de Inspector de página, haga clic en uno de los elementos pendientes.</span><span class="sxs-lookup"><span data-stu-id="8c574-258">In the Page Inspector browser, click on one of the to-do items.</span></span> <span data-ttu-id="8c574-259">Observe que en lugar de resaltarse en azul, el elemento se resalta en color naranja, con "JS" junto al nombre del elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-259">Notice that instead of being highlighted in blue, the element is highlighted in orange, with "JS" next to the element name.</span></span> <span data-ttu-id="8c574-260">Esto indica que el elemento se ha creado dinámicamente a través de un script.</span><span class="sxs-lookup"><span data-stu-id="8c574-260">This indicates that the element was created dynamically through script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

<span data-ttu-id="8c574-261">Además, aparece un subrayado naranja en la pestaña **pila de llamadas** . Esto indica que el panel **pila de llamadas** tiene más información sobre el elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-261">In addition, an orange underline appears on the **Call Stack** tab. This indicates that the **Call Stack** pane has more information about the element.</span></span>

<span data-ttu-id="8c574-262">Haga clic en la pestaña **pila de llamadas** . El panel **pila de llamadas** muestra la pila de llamadas de la llamada de JavaScript que creó el elemento.</span><span class="sxs-lookup"><span data-stu-id="8c574-262">Click on the **Call Stack** tab. The **Call Stack** pane shows the call stack for the JavaScript call that created the element.</span></span> <span data-ttu-id="8c574-263">Las llamadas a bibliotecas externas como jQuery están contraídas, por lo que puede ver fácilmente las llamadas al script de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8c574-263">Calls to external libraries such as jQuery are collapsed, so that you can easily see the calls to your application script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

<span data-ttu-id="8c574-264">Para ver la pila completa, incluidas las llamadas a bibliotecas externas, puede expandir los nodos con la etiqueta "bibliotecas externas":</span><span class="sxs-lookup"><span data-stu-id="8c574-264">To see the full stack, including calls to external libraries, you can expand the nodes labeled "External Libraries":</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

<span data-ttu-id="8c574-265">Si hace clic en un elemento de la pila de llamadas, Visual Studio abre el archivo de código y resalta el script correspondiente.</span><span class="sxs-lookup"><span data-stu-id="8c574-265">If you click an item in the call stack, Visual Studio opens the code file and highlights the corresponding script.</span></span>

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a><span data-ttu-id="8c574-266">Consulte también</span><span class="sxs-lookup"><span data-stu-id="8c574-266">See Also</span></span>

<span data-ttu-id="8c574-267">[Introducción a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sitio web de ASP.net)</span><span class="sxs-lookup"><span data-stu-id="8c574-267">[Intro to ASP.NET MVC 4 with Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.net website)</span></span>

<span data-ttu-id="8c574-268">[Presentación de inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo de Channel 9)</span><span class="sxs-lookup"><span data-stu-id="8c574-268">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9 video)</span></span>

<span data-ttu-id="8c574-269">[Mensajes de error de inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="8c574-269">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
