---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: ASP.NET Web Pages de programación (Razor) con Visual Studio | Microsoft Docs
author: Rick-Anderson
description: En este apéndice se explica cómo puede usar Visual Studio 2010 o Visual Web Developer 2010 Express para programar ASP.NET Web Pages con la sintaxis Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514297"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a><span data-ttu-id="5b151-103">ASP.NET Web Pages de programación (Razor) con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b151-103">Programming ASP.NET Web Pages (Razor) Using Visual Studio</span></span>

<span data-ttu-id="5b151-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5b151-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5b151-105">En este artículo se explica cómo puede usar Visual Studio o Visual Web Developer Express para programar ASP.NET Web Pages (Razor) sitios Web.</span><span class="sxs-lookup"><span data-stu-id="5b151-105">This article explains how you can use Visual Studio or Visual Web Developer Express to program ASP.NET Web Pages (Razor) websites.</span></span>
>
> <span data-ttu-id="5b151-106">Temas que se abordarán</span><span class="sxs-lookup"><span data-stu-id="5b151-106">What you'll learn</span></span>
>
> - <span data-ttu-id="5b151-107">Lo que necesita para instalar (en caso de que haya algo) para trabajar con ASP.NET Web Pages en su versión de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-107">What you need to install (if anything) to work with ASP.NET Web Pages in your version of Visual Studio.</span></span>
> - <span data-ttu-id="5b151-108">Cómo agregar compatibilidad para ASP.NET Web Pages a Visual Web Developer 2010 Express.</span><span class="sxs-lookup"><span data-stu-id="5b151-108">How to add support for ASP.NET Web Pages to Visual Web Developer 2010 Express.</span></span>
> - <span data-ttu-id="5b151-109">Cómo usar las características de Visual Studio para trabajar con las páginas de Razor de ASP.NET, incluidas IntelliSense y el depurador.</span><span class="sxs-lookup"><span data-stu-id="5b151-109">How to use features in Visual Studio to work with ASP.NET Razor pages, including IntelliSense and the debugger.</span></span>
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5b151-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="5b151-110">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="5b151-111">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5b151-111">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="5b151-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5b151-112">Visual Studio 2013</span></span>
> - <span data-ttu-id="5b151-113">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="5b151-113">WebMatrix 3</span></span>
>
>
> <span data-ttu-id="5b151-114">Este tutorial también funciona con ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="5b151-114">This tutorial also works with ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010, and WebMatrix 2.</span></span>

<span data-ttu-id="5b151-115">Puede programar páginas Web ASP.NET con sintaxis Razor mediante WebMatrix o muchos otros editores de código.</span><span class="sxs-lookup"><span data-stu-id="5b151-115">You can program ASP.NET Web pages with Razor syntax using WebMatrix or many other code editors.</span></span> <span data-ttu-id="5b151-116">También puede usar Microsoft Visual Studio, que es un entorno de desarrollo integrado (IDE) con todas las características que proporciona un eficaz conjunto de herramientas para crear muchos tipos de aplicaciones (no solo sitios web).</span><span class="sxs-lookup"><span data-stu-id="5b151-116">You can also use Microsoft Visual Studio which is a full-featured integrated development environment (IDE) that provides a powerful set of tools for creating many types of applications (not just websites).</span></span> <span data-ttu-id="5b151-117">Para trabajar con las páginas de Razor para ASP.NET, puede usar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="5b151-117">To work with ASP.NET Razor pages, you can use [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span>

<span data-ttu-id="5b151-118">Dos características especialmente útiles que Visual Studio proporciona para la programación con páginas web de Razor para ASP.NET son:</span><span class="sxs-lookup"><span data-stu-id="5b151-118">Two particularly useful features that Visual Studio provides for programming with ASP.NET Razor web pages are:</span></span>

- <span data-ttu-id="5b151-119">*IntelliSense*.</span><span class="sxs-lookup"><span data-stu-id="5b151-119">*IntelliSense*.</span></span> <span data-ttu-id="5b151-120">La característica IntelliSense integrada en Visual Studio es más completa que IntelliSense en WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="5b151-120">The IntelliSense feature built into Visual Studio is more comprehensive than IntelliSense in WebMatrix.</span></span>
- <span data-ttu-id="5b151-121">*Depurador*.</span><span class="sxs-lookup"><span data-stu-id="5b151-121">*Debugger*.</span></span> <span data-ttu-id="5b151-122">El depurador permite solucionar problemas del código al detener un programa mientras se ejecuta, examinar variables y recorrer el código línea a línea.</span><span class="sxs-lookup"><span data-stu-id="5b151-122">The debugger lets you troubleshoot your code by stopping a program while it's running, examining variables, and stepping through the code line by line.</span></span>

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a><span data-ttu-id="5b151-123">Usar Visual Studio con versiones diferentes de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="5b151-123">Using Visual Studio with Different Versions of ASP.NET Web Pages</span></span>

<span data-ttu-id="5b151-124">Para desarrollar aplicaciones Web de ASP.NET en Visual Studio 2017, instale la carga de trabajo de **desarrollo de ASP.net y Web** .</span><span class="sxs-lookup"><span data-stu-id="5b151-124">To develop ASP.NET web apps in Visual Studio 2017, install the **ASP.NET and web development** workload.</span></span>

<span data-ttu-id="5b151-125">Visual Studio 2012 y Visual Studio 2013 incluyen compatibilidad con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5b151-125">Visual Studio 2012 and Visual Studio 2013 include support for ASP.NET Web Pages.</span></span> <span data-ttu-id="5b151-126">(Los paquetes necesarios para admitir ASP.NET Web Pages se instalan al instalar Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="5b151-126">(The packages that are required in order to support ASP.NET Web Pages are installed when you install Visual Studio.)</span></span>

<span data-ttu-id="5b151-127">Visual Studio 2010 no incluye compatibilidad de forma predeterminada para ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="5b151-127">Visual Studio 2010 does not include support by default for ASP.NET Web Pages.</span></span> <span data-ttu-id="5b151-128">Para usar ASP.NET Web Pages con Visual Studio 2010, debe instalar el paquete ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b151-128">To use ASP.NET Web Pages with Visual Studio 2010, you must install the ASP.NET MVC package.</span></span> <span data-ttu-id="5b151-129">Para obtener ASP.NET Web Pages 2, instale ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5b151-129">To get ASP.NET Web Pages 2, you install ASP.NET MVC 4.</span></span>

<span data-ttu-id="5b151-130">En la tabla siguiente se resume la compatibilidad con ASP.NET Web Pages en diferentes versiones de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-130">The following table summarizes the support for ASP.NET Web Pages in different versions of Visual Studio.</span></span>

|  | <span data-ttu-id="5b151-131">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5b151-131">Visual Studio 2010</span></span> | <span data-ttu-id="5b151-132">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5b151-132">Visual Studio 2012</span></span> | <span data-ttu-id="5b151-133">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5b151-133">Visual Studio 2013</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5b151-134">**ASP.NET Web Pages 2**</span><span class="sxs-lookup"><span data-stu-id="5b151-134">**ASP.NET Web Pages 2**</span></span> | <span data-ttu-id="5b151-135">Instalación de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="5b151-135">Install ASP.NET MVC 4</span></span> | <span data-ttu-id="5b151-136">Inclusión</span><span class="sxs-lookup"><span data-stu-id="5b151-136">(Included)</span></span> | <span data-ttu-id="5b151-137">Inclusión</span><span class="sxs-lookup"><span data-stu-id="5b151-137">(Included)</span></span> |
| <span data-ttu-id="5b151-138">**ASP.NET Web Pages 3**</span><span class="sxs-lookup"><span data-stu-id="5b151-138">**ASP.NET Web Pages 3**</span></span> |  | <span data-ttu-id="5b151-139">Actualización de ASP.NET Web Pages 3 a NuGet</span><span class="sxs-lookup"><span data-stu-id="5b151-139">Update to ASP.NET Web Pages 3 through NuGet</span></span> | <span data-ttu-id="5b151-140">Inclusión</span><span class="sxs-lookup"><span data-stu-id="5b151-140">(Included)</span></span> |

<span data-ttu-id="5b151-141">Para trabajar con Visual Studio 2010, consulte [instalación de la compatibilidad con ASP.NET Web pages en visual studio 2010](#vs2010support).</span><span class="sxs-lookup"><span data-stu-id="5b151-141">To work with Visual Studio 2010, see [Installing Support for ASP.NET Web Pages in Visual Studio 2010](#vs2010support).</span></span>

## <a name="launching-visual-studio-from-webmatrix"></a><span data-ttu-id="5b151-142">Inicio de Visual Studio desde WebMatrix</span><span class="sxs-lookup"><span data-stu-id="5b151-142">Launching Visual Studio from WebMatrix</span></span>

<span data-ttu-id="5b151-143">Si ha iniciado un proyecto en WebMatrix y desea cambiar a Visual Studio, WebMatrix proporciona un botón para abrir fácilmente el proyecto en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-143">If you have started a project in WebMatrix and want to switch to Visual Studio, WebMatrix provides a button to easily open the project in Visual Studio.</span></span> <span data-ttu-id="5b151-144">Para que este botón esté habilitado, debe tener Visual Studio instalado en el equipo.</span><span class="sxs-lookup"><span data-stu-id="5b151-144">You must have Visual Studio installed on your computer for this button to be enabled.</span></span> <span data-ttu-id="5b151-145">En la imagen siguiente se muestra el botón de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="5b151-145">The following image shows the button in WebMatrix.</span></span>

![iniciar Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

<span data-ttu-id="5b151-147">Al hacer clic en el botón, el proyecto se abre en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-147">When you click the button, the project is opened in Visual Studio.</span></span> <span data-ttu-id="5b151-148">Puede alternar entre WebMatrix y Visual Studio sin problemas.</span><span class="sxs-lookup"><span data-stu-id="5b151-148">You can switch back and forth between WebMatrix and Visual Studio without any problems.</span></span> <span data-ttu-id="5b151-149">Se le notificará si algún archivo ha cambiado en el otro entorno y debe volver a cargarse para obtener los cambios más recientes.</span><span class="sxs-lookup"><span data-stu-id="5b151-149">You will be notified if any files have changed in the other environment and need to be reloaded to get the latest changes.</span></span>

## <a name="creating-aspnet-razor-site-in-visual-studio"></a><span data-ttu-id="5b151-150">Creación de un sitio de ASP.NET Razor en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b151-150">Creating ASP.NET Razor Site in Visual Studio</span></span>

<span data-ttu-id="5b151-151">Para crear un sitio web de Razor ASP.NET en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5b151-151">To create an ASP.NET Razor website in Visual Studio:</span></span>

1. <span data-ttu-id="5b151-152">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-152">Open Visual Studio.</span></span>
2. <span data-ttu-id="5b151-153">En el menú **archivo** , haga clic en **nuevo sitio web**.</span><span class="sxs-lookup"><span data-stu-id="5b151-153">In the **File** menu, click **New Web Site**.</span></span>

    ![crear nuevo sitio web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. <span data-ttu-id="5b151-155">En el cuadro de diálogo **nuevo sitio web** , seleccione el idioma que desea usar C# (visual o Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="5b151-155">In the **New Web Site** dialog box, select the language to use (Visual C# or Visual Basic).</span></span>
4. <span data-ttu-id="5b151-156">Seleccione la plantilla **sitio web de ASP.net (Razor)** .</span><span class="sxs-lookup"><span data-stu-id="5b151-156">Select the **ASP.NET Web Site (Razor)** template.</span></span>

    ![sitio de Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. <span data-ttu-id="5b151-158">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="5b151-158">Click **OK**.</span></span>

<span data-ttu-id="5b151-159">El nuevo proyecto existe y se rellena con algunas páginas Web predeterminadas para ayudarle a empezar.</span><span class="sxs-lookup"><span data-stu-id="5b151-159">Your new project exists and is populated with some default web pages to help you get started.</span></span>

### <a name="using-intellisense"></a><span data-ttu-id="5b151-160">Using IntelliSense</span><span class="sxs-lookup"><span data-stu-id="5b151-160">Using IntelliSense</span></span>

<span data-ttu-id="5b151-161">Ahora que ha creado un sitio, puede ver cómo funciona IntelliSense en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-161">Now that you've created a site, you can see how IntelliSense works in Visual Studio.</span></span>

1. <span data-ttu-id="5b151-162">En el sitio web que acaba de crear, abra la página *default. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="5b151-162">In the website you just created, open the *Default.cshtml* page.</span></span>
2. <span data-ttu-id="5b151-163">Después de la `<h3>` etiquetas en la página, escriba `@ServerInfo.` (incluido el punto).</span><span class="sxs-lookup"><span data-stu-id="5b151-163">After the `<h3>` tags in the page, type `@ServerInfo.` (including the dot).</span></span> <span data-ttu-id="5b151-164">Observe cómo IntelliSense muestra los métodos disponibles para la aplicación auxiliar de `ServerInfo` en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="5b151-164">Notice how IntelliSense displays the available methods for the `ServerInfo` helper in a drop-down list.</span></span>

    ![IntelliSense](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. <span data-ttu-id="5b151-166">Seleccione el método de `GetHtml` de la lista y, a continuación, presione Entrar.</span><span class="sxs-lookup"><span data-stu-id="5b151-166">Select the `GetHtml` method from the list and then press Enter.</span></span> <span data-ttu-id="5b151-167">IntelliSense rellena automáticamente el método.</span><span class="sxs-lookup"><span data-stu-id="5b151-167">IntelliSense automatically fills in the method.</span></span> <span data-ttu-id="5b151-168">(Como con cualquier método de C#, debe agregar `()` caracteres después del método). El código completado para el método `GetHtml` es similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5b151-168">(As with any method in C#, you must add `()` characters after the method.) The completed code for the `GetHtml` method looks like the following example:</span></span>

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. <span data-ttu-id="5b151-169">Presione Ctrl + F5 para ejecutar la página.</span><span class="sxs-lookup"><span data-stu-id="5b151-169">Press Ctrl+F5 to run the page.</span></span> <span data-ttu-id="5b151-170">Este es el aspecto de la página cuando se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="5b151-170">This is what the page looks like when displayed in a browser:</span></span>

    ![página predeterminada en el explorador](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. <span data-ttu-id="5b151-172">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="5b151-172">Close the browser.</span></span>

### <a name="using-the-debugger"></a><span data-ttu-id="5b151-173">Usar el depurador</span><span class="sxs-lookup"><span data-stu-id="5b151-173">Using the Debugger</span></span>

1. <span data-ttu-id="5b151-174">En la parte superior de la página *default. cshtml* , después de la línea que comienza con `Page.Title`, agregue la siguiente línea de código:</span><span class="sxs-lookup"><span data-stu-id="5b151-174">At the top of the *Default.cshtml* page, after the line that begins with `Page.Title`, add the following line of code:</span></span>

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. <span data-ttu-id="5b151-175">En el margen gris del editor a la izquierda del código, haga clic junto a esta nueva línea para agregar un *punto de interrupción*.</span><span class="sxs-lookup"><span data-stu-id="5b151-175">In the gray margin of the editor to the left of the code, click next to this new line in order to add a *breakpoint*.</span></span> <span data-ttu-id="5b151-176">Un punto de interrupción es un marcador que indica al depurador que detenga la ejecución del programa en ese momento para que pueda ver lo que está ocurriendo.</span><span class="sxs-lookup"><span data-stu-id="5b151-176">A breakpoint is a marker that tells the debugger to stop running the program at that point so you can see what's happening.</span></span>

    ![establecer punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. <span data-ttu-id="5b151-178">Quite la llamada al método `ServerInfo.GetHtml` y agregue una llamada a la variable `@myTime` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="5b151-178">Remove the call to the `ServerInfo.GetHtml` method, and add a call to the `@myTime` variable in its place.</span></span> <span data-ttu-id="5b151-179">Esta llamada muestra el valor de hora actual devuelto por la nueva línea de código.</span><span class="sxs-lookup"><span data-stu-id="5b151-179">This call displays the current time value that's returned by the new line of code.</span></span>
4. <span data-ttu-id="5b151-180">Presione F5 para ejecutar la página en el depurador.</span><span class="sxs-lookup"><span data-stu-id="5b151-180">Press F5 to run the page in the debugger.</span></span> <span data-ttu-id="5b151-181">La página se detiene en el punto de interrupción que estableció.</span><span class="sxs-lookup"><span data-stu-id="5b151-181">The page stops on the breakpoint that you set.</span></span> <span data-ttu-id="5b151-182">En la imagen siguiente se muestra el aspecto de la página en el editor con el punto de interrupción (en amarillo).</span><span class="sxs-lookup"><span data-stu-id="5b151-182">The following image shows what the page looks like in the editor with the breakpoint (in yellow).</span></span>

    ![depurar punto de interrupción](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. <span data-ttu-id="5b151-184">En la barra de herramientas Depurar, haga clic en el botón **paso a paso por instrucciones** (o presione F11) para ejecutar la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="5b151-184">In the Debug toolbar, click the **Step Into** button (or press F11) to run the next line of code.</span></span> <span data-ttu-id="5b151-185">Cada vez que haga clic en este botón, avanzará la ejecución a la siguiente línea de código.</span><span class="sxs-lookup"><span data-stu-id="5b151-185">Each time you click this button, you advance the execution to the next line of code.</span></span>

    ![Botón paso a paso por instrucciones](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. <span data-ttu-id="5b151-187">Examine el valor de la variable `myTime` manteniendo el puntero del mouse sobre él o inspeccionando los valores mostrados en las ventanas **variables locales** y **pila de llamadas** .</span><span class="sxs-lookup"><span data-stu-id="5b151-187">Examine the value of the `myTime` variable by holding your mouse pointer over it or by inspecting the values displayed in the **Locals** and **Call Stack** windows.</span></span> <span data-ttu-id="5b151-188">Visual Studio muestra el valor de la variable.</span><span class="sxs-lookup"><span data-stu-id="5b151-188">Visual Studio display the value of the variable.</span></span>

    ![Mostrar valor de tiempo](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. <span data-ttu-id="5b151-190">Cuando haya terminado de examinar la variable y recorrer el código, presione F5 para continuar con la ejecución de la página sin detener en cada línea.</span><span class="sxs-lookup"><span data-stu-id="5b151-190">When you're done examining the variable and stepping through code, press F5 to continue running the page without stopping at each line.</span></span> <span data-ttu-id="5b151-191">Cuando haya terminado de recorrer todo el código, el explorador muestra la página.</span><span class="sxs-lookup"><span data-stu-id="5b151-191">When you've finished stepping through all the code, the browser displays the page.</span></span>

<span data-ttu-id="5b151-192">Para obtener más información sobre el depurador y sobre cómo depurar código en Visual Studio, vea [Tutorial: depurar páginas web en Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b151-192">To learn more about the debugger and about how to debug code in Visual Studio, see [Walkthrough: Debugging Web Pages in Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).</span></span>

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a><span data-ttu-id="5b151-193">Usar Razor en proyectos de MVC de ASP.NET con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b151-193">Using Razor in ASP.NET MVC projects with Visual Studio</span></span>

<span data-ttu-id="5b151-194">La sintaxis Razor también se usa en los proyectos de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b151-194">The Razor syntax is also used extensively in ASP.NET MVC projects.</span></span> <span data-ttu-id="5b151-195">MVC es un método eficaz basado en patrones para crear sitios web dinámicos.</span><span class="sxs-lookup"><span data-stu-id="5b151-195">MVC is a powerful, patterns-based way to build dynamic websites.</span></span> <span data-ttu-id="5b151-196">Si el sitio de ASP.NET Web Pages resulta difícil de mantener, puede considerar la posibilidad de convertirlo en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b151-196">If your ASP.NET Web Pages site becomes difficult to maintain, you might want to consider converting it to an ASP.NET MVC application.</span></span> <span data-ttu-id="5b151-197">Para obtener un ejemplo de cómo crear una aplicación MVC, vea [Introducción con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5b151-197">For an example of creating an MVC application, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a><span data-ttu-id="5b151-198">Instalación de compatibilidad para ASP.NET Web Pages en Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5b151-198">Installing Support for ASP.NET Web Pages in Visual Studio 2010</span></span>

<span data-ttu-id="5b151-199">En esta sección se muestra cómo instalar Visual Web Developer Express 2010 y las herramientas de ASP.NET Web Pages para Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b151-199">This section shows how to install Visual Web Developer Express 2010 and the ASP.NET Web Pages Tools for Visual Studio.</span></span>

1. <span data-ttu-id="5b151-200">Si aún no tiene el instalador de plataforma web, descárguelo de la siguiente dirección URL:</span><span class="sxs-lookup"><span data-stu-id="5b151-200">If you don't already have the Web Platform Installer, download it from the following URL:</span></span>

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. <span data-ttu-id="5b151-201">Ejecute el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="5b151-201">Run the Web Platform Installer.</span></span>
3. <span data-ttu-id="5b151-202">Haga clic en la pestaña **productos** .</span><span class="sxs-lookup"><span data-stu-id="5b151-202">Click the **Products** tab.</span></span>

    ![Pestaña productos de WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. <span data-ttu-id="5b151-204">Busque **ASP.NET MVC 4** (para ASP.NET Web Pages 2) y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="5b151-204">Search for **ASP.NET MVC 4** (for ASP.NET Web Pages 2) and then click **Add**.</span></span> <span data-ttu-id="5b151-205">Estos productos incluyen herramientas de Visual Studio para compilar sitios web de Razor ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b151-205">These products include Visual Studio tools for building ASP.NET Razor websites.</span></span>

    ![Opciones de instalación de WebPi](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. <span data-ttu-id="5b151-207">Haga clic en **instalar** para completar la instalación.</span><span class="sxs-lookup"><span data-stu-id="5b151-207">Click **Install** to complete the installation.</span></span>
