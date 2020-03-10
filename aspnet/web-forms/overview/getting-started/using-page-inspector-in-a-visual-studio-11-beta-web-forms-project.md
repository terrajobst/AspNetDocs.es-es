---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Usar Inspector de página para Visual Studio 2012 en formularios Web Forms de ASP.NET | Microsoft Docs
author: rick-anderson
description: Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464947"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="81f30-104">Usar el Inspector de página para Visual Studio 2012 en los formularios Web Forms de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="81f30-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="81f30-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="81f30-105">by Tim Ammann</span></span>

> <span data-ttu-id="81f30-106">Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="81f30-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="81f30-107">Seleccione cualquier elemento en el explorador integrado y Inspector de página resaltará al instante el origen y el código CSS del elemento.</span><span class="sxs-lookup"><span data-stu-id="81f30-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="81f30-108">Puede examinar cualquier página de la aplicación, buscar rápidamente los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81f30-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="81f30-109">En este tutorial se muestra cómo habilitar Modo de inspección y, a continuación, ubicar y editar rápidamente reglas y texto de CSS en el proyecto Web.</span><span class="sxs-lookup"><span data-stu-id="81f30-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="81f30-110">En el tutorial se usa un proyecto de aplicación de formularios Web Forms, pero también puede usar Inspector de página para proyectos de sitios web y aplicaciones [MVC](https://go.microsoft.com/?linkid=9802002) .</span><span class="sxs-lookup"><span data-stu-id="81f30-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="81f30-111">El tutorial consta de las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="81f30-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="81f30-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="81f30-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="81f30-113">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="81f30-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="81f30-114">Usar Inspector de página para ver la aplicación</span><span class="sxs-lookup"><span data-stu-id="81f30-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="81f30-115">Habilitar Modo de inspección</span><span class="sxs-lookup"><span data-stu-id="81f30-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="81f30-116">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="81f30-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="81f30-117">Modo de inspección y la ventana HTML</span><span class="sxs-lookup"><span data-stu-id="81f30-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="81f30-118">Vista previa de los cambios de CSS en la ventana estilos</span><span class="sxs-lookup"><span data-stu-id="81f30-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="81f30-119">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="81f30-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="81f30-120">Usar el selector de colores de CSS</span><span class="sxs-lookup"><span data-stu-id="81f30-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="81f30-121">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="81f30-121">Prerequisites</span></span>

- <span data-ttu-id="81f30-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="81f30-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="81f30-123">Para obtener la versión más reciente de Inspector de página, use el [instalador de plataforma web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Azure para .net 2,0.</span><span class="sxs-lookup"><span data-stu-id="81f30-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="81f30-124">Inspector de página se incluye con Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="81f30-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="81f30-125">La versión más reciente es 1,3.</span><span class="sxs-lookup"><span data-stu-id="81f30-125">The latest version is 1.3.</span></span> <span data-ttu-id="81f30-126">Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** en el menú **ayuda** .</span><span class="sxs-lookup"><span data-stu-id="81f30-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="81f30-127">Crear una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="81f30-127">Create a Web Application</span></span>

<span data-ttu-id="81f30-128">En primer lugar, creará una aplicación web que utilizará Inspector de página con.</span><span class="sxs-lookup"><span data-stu-id="81f30-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="81f30-129">En Visual Studio, elija **archivo** &gt; **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="81f30-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="81f30-130">A la izquierda, expanda **visualmente C#** , seleccione **Web**y, a continuación, seleccione **ASP.NET Web Forms Application**.</span><span class="sxs-lookup"><span data-stu-id="81f30-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Nueva aplicación de formularios Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="81f30-132">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="81f30-132">Click **OK**.</span></span>

<span data-ttu-id="81f30-133">La aplicación se abre en la vista **código fuente** .</span><span class="sxs-lookup"><span data-stu-id="81f30-133">The application opens in **Source** view.</span></span>

![Nueva aplicación de formularios Web Forms en la vista Código fuente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="81f30-135">Ahora que tiene una aplicación con la que trabajar, puede usar Inspector de página para examinarla y modificarla.</span><span class="sxs-lookup"><span data-stu-id="81f30-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="81f30-136">Usar Inspector de página para ver la aplicación</span><span class="sxs-lookup"><span data-stu-id="81f30-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="81f30-137">A continuación, verá la aplicación con Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="81f30-138">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y elija **ver en inspector de página**.</span><span class="sxs-lookup"><span data-stu-id="81f30-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Ver en Inspector de páginas](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="81f30-140">De forma predeterminada, cuando Inspector de página se inicia por primera vez, se acopla como una ventana estrecha en el lado izquierdo del entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81f30-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="81f30-141">Déjelo acoplado en el lado izquierdo y establézcalo en un ancho que le resulte cómodo, o bien acoplelo en una de las áreas de herramientas de la parte superior, inferior o derecha:</span><span class="sxs-lookup"><span data-stu-id="81f30-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Inspector de página posiciones de acoplamiento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="81f30-143">Si desacopla la ventana de Inspector de página, puede colocarla fuera de Visual Studio, o incluso en un segundo monitor si tiene una.</span><span class="sxs-lookup"><span data-stu-id="81f30-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="81f30-144">Sin embargo, para presionar ALT + TAB entre Inspector de página y Visual Studio cuando la ventana de Inspector de página está desacoplada, vaya a **herramientas** &gt; **opciones** &gt; **entorno** &gt; **pestañas y ventanas**y, en **pestañas**, desactive la casilla de verificación las ventanas de herramientas **flotantes permanecen siempre en la parte superior de la ventana principal**:</span><span class="sxs-lookup"><span data-stu-id="81f30-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Desactive la casilla flotante ventanas de herramientas en ALT + TAB entre Visual Studio y la ventana de Inspector de página desacoplada.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="81f30-146">En el panel superior de la ventana de Inspector de página se muestra la página actual en una ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="81f30-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="81f30-147">En el panel inferior se muestra la página en formato HTML a la izquierda y algunas de las pestañas de la derecha que permiten inspeccionar distintos aspectos de la página.</span><span class="sxs-lookup"><span data-stu-id="81f30-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="81f30-148">El panel inferior es similar al [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="81f30-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="81f30-149">(Sin embargo, a diferencia de las herramientas de desarrollo, puede usar Inspector de página directamente en Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="81f30-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="81f30-151">En este tutorial, usará el panel Explorador de Inspector de página y las pestañas **HTML** y **estilos** para ayudarle a navegar rápidamente y realizar cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="81f30-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="81f30-152">Habilitar Modo de inspección</span><span class="sxs-lookup"><span data-stu-id="81f30-152">Enable Inspection Mode</span></span>

<span data-ttu-id="81f30-153">A continuación, verá cómo funciona el Modo de inspección de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="81f30-154">En la ventana de Inspector de página, haga clic en el botón **inspeccionar** .</span><span class="sxs-lookup"><span data-stu-id="81f30-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="81f30-156">Para ver el modo de inspección en acción, mueva el mouse sobre diferentes partes de la página en la Inspector de página ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="81f30-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="81f30-157">Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:</span><span class="sxs-lookup"><span data-stu-id="81f30-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="81f30-159">Cuando mueva el puntero del mouse, tenga en cuenta que</span><span class="sxs-lookup"><span data-stu-id="81f30-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="81f30-160">El contenido de la vista **código fuente** cambia para mostrar el marcado correspondiente al elemento seleccionado en la página.</span><span class="sxs-lookup"><span data-stu-id="81f30-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="81f30-161">Se resalta el marcado pertinente.</span><span class="sxs-lookup"><span data-stu-id="81f30-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="81f30-162">Si el origen está en otro archivo, ese archivo se abre en la vista de código fuente con el marcado correspondiente resaltado.</span><span class="sxs-lookup"><span data-stu-id="81f30-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="81f30-163">El marcado que se muestra en la pestaña **HTML** en inspector de página también cambia para que se corresponda con el elemento seleccionado en la página.</span><span class="sxs-lookup"><span data-stu-id="81f30-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="81f30-164">En la pestaña **HTML** , se describe el marcado pertinente.</span><span class="sxs-lookup"><span data-stu-id="81f30-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="81f30-165">En la pestaña **estilos** se muestran las reglas de CSS relevantes para la selección actual.</span><span class="sxs-lookup"><span data-stu-id="81f30-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="81f30-166">Usar Inspector de página para realizar cambios en el marcado</span><span class="sxs-lookup"><span data-stu-id="81f30-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="81f30-167">Ahora verá cómo puede usar Inspector de página para buscar y realizar cambios en el marcado o en el texto cuya ubicación podría no ser obvia inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="81f30-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="81f30-168">Coloque Inspector de página en Modo de inspección y, a continuación, desplácese hasta la parte inferior de la Página principal.</span><span class="sxs-lookup"><span data-stu-id="81f30-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="81f30-169">En cuanto se escribe el área de pie de página, Inspector de página abre el archivo de diseño *site. Master* en la vista **código fuente** en una pestaña temporal situada a la derecha de las demás pestañas y resalta la sección de la página maestra que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="81f30-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="81f30-170">Esto muestra cómo Inspector de página puede buscar y mostrar el contenido de una página que podría proceder realmente de un archivo diferente al que abrió originalmente.</span><span class="sxs-lookup"><span data-stu-id="81f30-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Resaltado de pie de página en Modo de inspección](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="81f30-172">En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la línea con <a id="a"> </a>el aviso de propiedad intelectual.</span><span class="sxs-lookup"><span data-stu-id="81f30-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="81f30-173">En la página *site. Master* , se resalta la línea correspondiente.</span><span class="sxs-lookup"><span data-stu-id="81f30-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Línea de copyright de pie de página resaltada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="81f30-175">Agregue texto al final de la línea en el archivo *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="81f30-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="81f30-176">&lt;p&gt;&amp;Copy; &lt;%: DateTime. Now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="81f30-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="81f30-177">Ahora, presione Ctrl + Alt + entrar o haga clic en la barra de actualización para ver los resultados en la ventana del explorador Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![¡ Mi aplicación ASP.NET Rock!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="81f30-179">Es posible que haya pensado que el pie de página se encontraba en la página *default. aspx* , pero que se encontraba en la página de diseño maestra y inspector de página lo encontró.</span><span class="sxs-lookup"><span data-stu-id="81f30-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="81f30-180">Modo de inspección y la ventana HTML</span><span class="sxs-lookup"><span data-stu-id="81f30-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="81f30-181">A continuación, tendrá una vista rápida de la ventana HTML y cómo asigna los elementos.</span><span class="sxs-lookup"><span data-stu-id="81f30-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="81f30-182">Coloque Inspector de página en Modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="81f30-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="81f30-184">Haga clic en la parte superior de la página que dice "su logotipo aquí".</span><span class="sxs-lookup"><span data-stu-id="81f30-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="81f30-185">Está examinando un elemento determinado con más detalle, de modo que la presentación en la ventana del explorador ya no cambia a medida que mueve el puntero del mouse.</span><span class="sxs-lookup"><span data-stu-id="81f30-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="81f30-186">Ahora, mueva el puntero del mouse a la ventana **HTML** .</span><span class="sxs-lookup"><span data-stu-id="81f30-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="81f30-187">A medida que mueve el puntero del mouse, Inspector de página describe el elemento en la ventana **HTML** y resalta el elemento correspondiente en la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="81f30-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Ventana HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="81f30-189">Como antes, Inspector de página abre el archivo *site. Master* en una pestaña temporal. Haga clic en la pestaña site. Master y el marcado correspondiente se resalta en el encabezado de &lt;&gt; sección:</span><span class="sxs-lookup"><span data-stu-id="81f30-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Marcado resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="81f30-191">Vista previa de los cambios de CSS en la ventana estilos</span><span class="sxs-lookup"><span data-stu-id="81f30-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="81f30-192">A continuación, verá cómo puede utilizar la ventana **estilos** de inspector de página para obtener una vista previa de los cambios en CSS.</span><span class="sxs-lookup"><span data-stu-id="81f30-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="81f30-193">Haga clic en el botón **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="81f30-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="81f30-194">En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="81f30-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Mantener el mouse sobre los elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="81f30-196">Haga clic en la sección div. Content-wrapper una vez y, a continuación, mueva el puntero del mouse a la ventana **estilos** .</span><span class="sxs-lookup"><span data-stu-id="81f30-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="81f30-197">En el selector de clases. destacado. Content-wrapper, desactive y active la casilla de la propiedad background-color.</span><span class="sxs-lookup"><span data-stu-id="81f30-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Borrar color de fondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="81f30-199">Observe cómo la vista previa de cambios se muestra al instante en la ventana del explorador Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="81f30-200">Vuelva a activar la casilla de verificación y, a continuación, haga doble clic en el valor de la propiedad y cámbielo a `red`.</span><span class="sxs-lookup"><span data-stu-id="81f30-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="81f30-201">El cambio se muestra inmediatamente:</span><span class="sxs-lookup"><span data-stu-id="81f30-201">The change shows immediately:</span></span>

![Color de fondo rojo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="81f30-203">La ventana **estilos** facilita la prueba y la vista previa de los cambios de CSS antes de confirmar los cambios en la propia hoja de estilos.</span><span class="sxs-lookup"><span data-stu-id="81f30-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="81f30-204">Sincronización automática de CSS</span><span class="sxs-lookup"><span data-stu-id="81f30-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="81f30-205">Esta característica requiere la versión 1,3 de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="81f30-206">La característica de sincronización automática de CSS le permite editar un archivo CSS directamente y ver los cambios inmediatamente en el explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="81f30-207">Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="81f30-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="81f30-208">En el explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .</span><span class="sxs-lookup"><span data-stu-id="81f30-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="81f30-209">Haga clic en una vez para seleccionar este elemento.</span><span class="sxs-lookup"><span data-stu-id="81f30-209">Click once to select this element.</span></span>

<span data-ttu-id="81f30-210">La ventana **estilos** muestra todas las reglas de CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="81f30-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="81f30-211">Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="81f30-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="81f30-212">Haga clic en ". destacado. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="81f30-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="81f30-213">Inspector de página abre el archivo CSS que define este estilo (site. CSS) y resalta el estilo CSS correspondiente.</span><span class="sxs-lookup"><span data-stu-id="81f30-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Archivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="81f30-215">Ahora, cambie el valor de `background-color` por "rojo".</span><span class="sxs-lookup"><span data-stu-id="81f30-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="81f30-216">El cambio aparece inmediatamente en el explorador de Inspector de página.</span><span class="sxs-lookup"><span data-stu-id="81f30-216">The change appears immediately in the Page Inspector browser.</span></span>

![Explorador Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="81f30-218">Usar el selector de colores de CSS</span><span class="sxs-lookup"><span data-stu-id="81f30-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="81f30-219">A continuación, aprenderá a usar Inspector de página para buscar y cambiar rápidamente el CSS del texto resaltado en la aplicación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="81f30-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="81f30-220">En este ejemplo, ha decidido que no le gusta el resaltado azul y desea cambiarlo a otro color.</span><span class="sxs-lookup"><span data-stu-id="81f30-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="81f30-221">Haga clic en el botón **inspeccionar** .</span><span class="sxs-lookup"><span data-stu-id="81f30-221">Click the **Inspect** button.</span></span>

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="81f30-223">En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre el texto "vídeos, tutoriales y muestras" resaltado para que aparezca la etiqueta de "marca" de CSS.</span><span class="sxs-lookup"><span data-stu-id="81f30-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Mantener el puntero sobre el elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="81f30-225">Haga clic en el texto para seleccionarlo.</span><span class="sxs-lookup"><span data-stu-id="81f30-225">Click the text to select it.</span></span> <span data-ttu-id="81f30-226">El selector de marcas de CSS correspondiente aparece en la parte inferior de la ventana **estilos** .</span><span class="sxs-lookup"><span data-stu-id="81f30-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![Selector de marcas en la ventana estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="81f30-228">Haga clic en el selector de marcas.</span><span class="sxs-lookup"><span data-stu-id="81f30-228">Click the mark selector.</span></span> <span data-ttu-id="81f30-229">Se abrirá el archivo *site. CSS* de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="81f30-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="81f30-230">Haga clic en la pestaña site. CSS y se resalta el CSS correspondiente para el selector:</span><span class="sxs-lookup"><span data-stu-id="81f30-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![Selector de marcas en la hoja de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="81f30-232">Seleccione y quite la línea con la propiedad background-color.</span><span class="sxs-lookup"><span data-stu-id="81f30-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="81f30-233">Ahora usará el nuevo selector de colores CSS de Visual Studio 2012 para elegir un nuevo color para la propiedad **marcar** fondo-color.</span><span class="sxs-lookup"><span data-stu-id="81f30-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="81f30-234">Usar el selector de colores CSS de Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="81f30-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="81f30-235">El editor de CSS de Visual Studio 2012 tiene un selector de colores que facilita la elección y la inserción de colores.</span><span class="sxs-lookup"><span data-stu-id="81f30-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="81f30-236">Tiene una barra de colores simple y un selector "emergente" que ofrece un mayor control.</span><span class="sxs-lookup"><span data-stu-id="81f30-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="81f30-237">El selector de colores incluye una paleta estándar de colores, admite los nombres de colores estándar, códigos hash, RGB, RGBA, HSL y HSLA, y mantiene una lista de los colores usados más recientemente en el documento.</span><span class="sxs-lookup"><span data-stu-id="81f30-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="81f30-238">En la línea donde se encuentra la propiedad background-color, escriba "BC" y presione la flecha abajo una vez.</span><span class="sxs-lookup"><span data-stu-id="81f30-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="81f30-239">Al escribir el primer carácter de cada palabra en una propiedad separada por guiones como "background-color", IntelliSense filtra la lista para que solo se muestren las propiedades que coinciden con:</span><span class="sxs-lookup"><span data-stu-id="81f30-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valores filtrados de IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="81f30-241">Ahora escriba un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="81f30-241">Now type a colon.</span></span> <span data-ttu-id="81f30-242">Cuando lo haga, se insertará el nombre completo de la propiedad de color de fondo.</span><span class="sxs-lookup"><span data-stu-id="81f30-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="81f30-243">Escriba **#** o **RGB (** y aparecerá la barra de selector de colores:</span><span class="sxs-lookup"><span data-stu-id="81f30-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![Barra de selector de colores de CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="81f30-245">Para ver cómo funciona la barra del selector de colores, haga clic en sus colores con el puntero del mouse o presione la tecla flecha abajo y, a continuación, use las teclas de flecha izquierda y derecha para recorrer los colores.</span><span class="sxs-lookup"><span data-stu-id="81f30-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="81f30-246">Al visitar un color, se muestra una vista previa del valor correspondiente de la propiedad de color de fondo:</span><span class="sxs-lookup"><span data-stu-id="81f30-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![vista previa del valor de propiedad de color de fondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="81f30-248">En este momento, puede presionar entrar para seleccionar el valor y, a continuación, un punto y coma (;) para completar la entrada de CSS.</span><span class="sxs-lookup"><span data-stu-id="81f30-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="81f30-249">Por ahora, vaya a la sección siguiente para que pueda ver cómo funciona la ventana emergente del selector de colores.</span><span class="sxs-lookup"><span data-stu-id="81f30-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="81f30-250">Usar la ventana emergente del selector de colores</span><span class="sxs-lookup"><span data-stu-id="81f30-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="81f30-251">Si la barra de colores no tiene el color exacto que está buscando, puede usar la ventana emergente selector de colores.</span><span class="sxs-lookup"><span data-stu-id="81f30-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="81f30-252">Para abrirlo, haga clic en el botón de contenido adicional en el extremo derecho de la barra de colores o presione la flecha hacia abajo una o dos veces en el teclado.</span><span class="sxs-lookup"><span data-stu-id="81f30-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Cuadro emergente del selector de colores de CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="81f30-254">Haga clic en un color de la barra vertical de la derecha.</span><span class="sxs-lookup"><span data-stu-id="81f30-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="81f30-255">Esto muestra un degradado para ese color en la ventana principal.</span><span class="sxs-lookup"><span data-stu-id="81f30-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="81f30-256">Elija un color directamente en la barra vertical presionando entrar o haga clic en cualquier punto de la ventana principal para elegir con mayor precisión.</span><span class="sxs-lookup"><span data-stu-id="81f30-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="81f30-257">Si hay un color en la pantalla del equipo que desea utilizar (no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta Cuentagotas de la parte inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="81f30-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="81f30-258">También puede cambiar la opacidad de un color moviendo el control deslizante situado en la parte inferior del selector de colores.</span><span class="sxs-lookup"><span data-stu-id="81f30-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="81f30-259">Al hacerlo, se cambian los valores de color a los valores RGBA porque el formato RGBA puede representar la opacidad.</span><span class="sxs-lookup"><span data-stu-id="81f30-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="81f30-260">Después de elegir un color, presione entrar y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el archivo *site. CSS* .</span><span class="sxs-lookup"><span data-stu-id="81f30-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="81f30-261">Barra de actualización Inspector de página</span><span class="sxs-lookup"><span data-stu-id="81f30-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="81f30-262">Inspector de página detecta inmediatamente el cambio en el archivo *site. CSS* (o en cualquier archivo de la aplicación) y muestra una alerta en una barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="81f30-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra de actualización](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="81f30-264">Para guardar todos los archivos y actualizar el explorador de Inspector de página, presione Ctrl + Alt + entrar o haga clic en la barra de actualización.</span><span class="sxs-lookup"><span data-stu-id="81f30-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="81f30-265">El cambio en el color de resaltado aparece en el explorador:</span><span class="sxs-lookup"><span data-stu-id="81f30-265">The change in the highlight color appears in the browser:</span></span>

![Color de resaltado cambiado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="81f30-267">Tenga en cuenta que se ha actualizado de forma cómoda el Inspector de página explorador directamente desde el entorno de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81f30-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="81f30-268">El uso de Inspector de página en lugar de un explorador externo le permite permanecer en el editor al desarrollar sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="81f30-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="81f30-269">Consulte también</span><span class="sxs-lookup"><span data-stu-id="81f30-269">See Also</span></span>

<span data-ttu-id="81f30-270">[Presentación de inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo de Channel 9)</span><span class="sxs-lookup"><span data-stu-id="81f30-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="81f30-271">[Mensajes de error de inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="81f30-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
