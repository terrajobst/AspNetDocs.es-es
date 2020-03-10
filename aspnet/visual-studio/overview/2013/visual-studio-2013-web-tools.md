---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio práctico: Visual Studio 2013 herramientas web | Microsoft Docs'
author: rick-anderson
description: Visual Studio es un entorno de desarrollo excelente para. Proyectos de Windows y Web basados en .net. Incluye un eficaz editor de texto que se puede usar fácilmente para...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505009"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="d002c-104">Laboratorio práctico: Visual Studio 2013 herramientas web</span><span class="sxs-lookup"><span data-stu-id="d002c-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="d002c-105">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="d002c-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="d002c-106">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="d002c-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="d002c-107">Visual Studio es un entorno de desarrollo excelente para. Proyectos de Windows y Web basados en .net.</span><span class="sxs-lookup"><span data-stu-id="d002c-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="d002c-108">Incluye un eficaz editor de texto que se puede usar fácilmente para editar archivos independientes sin un proyecto.</span><span class="sxs-lookup"><span data-stu-id="d002c-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="d002c-109">Visual Studio mantiene un árbol de análisis completo mientras edita cada archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="d002c-110">Esto permite que Visual Studio proporcione acciones de finalización automática y basadas en documentos sin parangón, a la vez que hace que la experiencia de desarrollo sea mucho más rápida y agradable.</span><span class="sxs-lookup"><span data-stu-id="d002c-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="d002c-111">Estas características son especialmente eficaces en documentos HTML y CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="d002c-112">Toda esta potencia también está disponible para las extensiones, lo que facilita la ampliación de los editores con nuevas y eficaces características que se adaptan a sus necesidades.</span><span class="sxs-lookup"><span data-stu-id="d002c-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="d002c-113">Web Essentials es una colección de mejoras (principalmente) relacionadas con la Web de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="d002c-114">Incluye muchas finalizaciones de IntelliSense nuevas (especialmente para CSS), nuevas características de vínculo del explorador, JSHint automáticas para archivos JavaScript, nuevas advertencias para HTML y CSS, y muchas otras características que son esenciales para el desarrollo web moderno.</span><span class="sxs-lookup"><span data-stu-id="d002c-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="d002c-115">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="d002c-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="d002c-116">Información general</span><span class="sxs-lookup"><span data-stu-id="d002c-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="d002c-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="d002c-117">Objectives</span></span>

<span data-ttu-id="d002c-118">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="d002c-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="d002c-119">Usar las nuevas características del editor HTML incluidas en Web Essentials, como fragmentos de código HTML5 enriquecidos y codificación Zen</span><span class="sxs-lookup"><span data-stu-id="d002c-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="d002c-120">Usar las nuevas características del editor de CSS incluidas en Web Essentials como la información sobre herramientas del selector de colores y la matriz del explorador</span><span class="sxs-lookup"><span data-stu-id="d002c-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="d002c-121">Usar las nuevas características del editor de JavaScript incluidas en Web Essentials como extraer a archivo e IntelliSense para todos los elementos HTML</span><span class="sxs-lookup"><span data-stu-id="d002c-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="d002c-122">Intercambio de datos entre el explorador y Visual Studio mediante el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="d002c-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="d002c-123">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d002c-123">Prerequisites</span></span>

<span data-ttu-id="d002c-124">Para completar este laboratorio práctico, es necesario lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d002c-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="d002c-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o superior</span><span class="sxs-lookup"><span data-stu-id="d002c-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="d002c-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="d002c-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="d002c-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="d002c-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="d002c-128">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="d002c-128">Setup</span></span>

<span data-ttu-id="d002c-129">Con el fin de ejecutar los ejercicios de este laboratorio práctico, deberá configurar el entorno primero.</span><span class="sxs-lookup"><span data-stu-id="d002c-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="d002c-130">Abra una ventana del explorador de Windows y vaya a la carpeta de **origen** del laboratorio.</span><span class="sxs-lookup"><span data-stu-id="d002c-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="d002c-131">Haga clic con el botón secundario en **setup. cmd** y seleccione **Ejecutar como administrador** para iniciar el proceso de instalación que configurará el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="d002c-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="d002c-132">Si se muestra el cuadro de diálogo control de cuentas de usuario, confirme la acción para continuar.</span><span class="sxs-lookup"><span data-stu-id="d002c-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="d002c-133">Asegúrese de que ha comprobado todas las dependencias de este laboratorio antes de ejecutar el programa de instalación.</span><span class="sxs-lookup"><span data-stu-id="d002c-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="d002c-134">Usar los fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="d002c-134">Using the Code Snippets</span></span>

<span data-ttu-id="d002c-135">En todo el documento de laboratorio, se le indicará que inserte bloques de código.</span><span class="sxs-lookup"><span data-stu-id="d002c-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="d002c-136">Para su comodidad, la mayor parte de este código se proporciona como fragmentos de Visual Studio Code, a los que puede acceder desde Visual Studio 2013 para evitar tener que agregarlo manualmente.</span><span class="sxs-lookup"><span data-stu-id="d002c-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="d002c-137">Cada ejercicio va acompañado de una solución de inicio que se encuentra en la carpeta **Inicio** del ejercicio que le permite seguir cada ejercicio con independencia de los demás.</span><span class="sxs-lookup"><span data-stu-id="d002c-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="d002c-138">Tenga en cuenta que los fragmentos de código que se agregan durante un ejercicio no se encuentran en estas soluciones de inicio y puede que no funcionen hasta que haya completado el ejercicio.</span><span class="sxs-lookup"><span data-stu-id="d002c-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="d002c-139">Dentro del código fuente de un ejercicio, también encontrará una carpeta **final** que contiene una solución de Visual Studio con el código que es el resultado de completar los pasos del ejercicio correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d002c-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="d002c-140">Puede usar estas soluciones como guía si necesita ayuda adicional a medida que trabaja en este laboratorio práctico.</span><span class="sxs-lookup"><span data-stu-id="d002c-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="d002c-141">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="d002c-141">Exercises</span></span>

<span data-ttu-id="d002c-142">Este laboratorio práctico incluye los siguientes ejercicios:</span><span class="sxs-lookup"><span data-stu-id="d002c-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="d002c-143">Trabajar con vínculos del explorador y Web Essentials</span><span class="sxs-lookup"><span data-stu-id="d002c-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="d002c-144">Aprovechar las ventajas de los fragmentos de código e IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d002c-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="d002c-145">Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas.</span><span class="sxs-lookup"><span data-stu-id="d002c-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="d002c-146">Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="d002c-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="d002c-147">En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** .</span><span class="sxs-lookup"><span data-stu-id="d002c-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="d002c-148">Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.</span><span class="sxs-lookup"><span data-stu-id="d002c-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="d002c-149">Ejercicio 1: trabajar con vínculos de explorador y Web Essentials</span><span class="sxs-lookup"><span data-stu-id="d002c-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="d002c-150">**Web Essentials** es una extensión de Visual Studio que agrega una gran variedad de características útiles para el desarrollo web moderno, principalmente centradas en facilitar la experiencia de desarrollo web mucho más rápida y agradable.</span><span class="sxs-lookup"><span data-stu-id="d002c-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="d002c-151">Puede instalar Web Essentials desde la galería de extensiones en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="d002c-152">El **vínculo del explorador** es una característica nueva incluida en Visual Studio 2013 que proporciona un canal entre el IDE de Visual Studio y cualquier explorador abierto para intercambiar datos entre la aplicación web y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="d002c-153">Web Essentials amplía el vínculo del explorador con herramientas para manipular el modelo de objetos DOM y los estilos CSS de las páginas web directamente desde el explorador.</span><span class="sxs-lookup"><span data-stu-id="d002c-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="d002c-154">En este ejercicio, explorará algunas de las características compatibles con **Web Essentials** y el **vínculo del explorador** para mejorar una sencilla página de cuestionario.</span><span class="sxs-lookup"><span data-stu-id="d002c-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="d002c-155">Tarea 1: ejecutar el proyecto en varios exploradores</span><span class="sxs-lookup"><span data-stu-id="d002c-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="d002c-156">En esta tarea, configurará la aplicación web para que se ejecute en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="d002c-157">Abra **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d002c-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="d002c-158">En el menú **archivo** , seleccione **abrir | Proyecto o solución..** . y vaya a **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** en la carpeta de **origen** del laboratorio (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="d002c-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="d002c-159">Seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="d002c-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="d002c-160">En la barra de herramientas de Visual Studio, expanda el menú del explorador y seleccione **examinar con..** ..</span><span class="sxs-lookup"><span data-stu-id="d002c-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="d002c-161">![Opción de menú explorar con](visual-studio-2013-web-tools/_static/image1.png "Examinar con... en el menú del explorador")</span><span class="sxs-lookup"><span data-stu-id="d002c-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="d002c-162">*Opción de menú explorar con*</span><span class="sxs-lookup"><span data-stu-id="d002c-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="d002c-163">En el cuadro de diálogo **explorar con** , seleccione **Google Chrome** e **Internet Explorer** manteniendo presionada la tecla **Ctrl** y haga clic en **establecer como predeterminado**.</span><span class="sxs-lookup"><span data-stu-id="d002c-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="d002c-164">![Cuadro de diálogo explorar con](visual-studio-2013-web-tools/_static/image2.png "Cuadro de diálogo explorar con")</span><span class="sxs-lookup"><span data-stu-id="d002c-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="d002c-165">*Seleccionar varios exploradores predeterminados*</span><span class="sxs-lookup"><span data-stu-id="d002c-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="d002c-166">Google Chrome e Internet Explorer deben aparecer ahora como exploradores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="d002c-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="d002c-167">Haga clic en **Cancelar** para cerrar el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="d002c-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="d002c-168">![Google Chrome e Internet Explorer como exploradores predeterminados](visual-studio-2013-web-tools/_static/image3.png "Exploradores predeterminados de Google Chrome e Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="d002c-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="d002c-169">*Google Chrome e Internet Explorer como exploradores predeterminados*</span><span class="sxs-lookup"><span data-stu-id="d002c-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-170">Después de configurar los exploradores predeterminados, se selecciona la opción **varios exploradores** en el menú del explorador.</span><span class="sxs-lookup"><span data-stu-id="d002c-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="d002c-171">![Varios exploradores](visual-studio-2013-web-tools/_static/image4.png "Varios exploradores")</span><span class="sxs-lookup"><span data-stu-id="d002c-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="d002c-172">Presione **CTRL** + **F5** para ejecutar la aplicación sin depuración.</span><span class="sxs-lookup"><span data-stu-id="d002c-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="d002c-173">Cuando se abran las ventanas del explorador, coloque una de ellas por encima de la otra para ver las actualizaciones en ambos exploradores simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="d002c-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="d002c-174">Los exploradores deben mostrar una página web con un rectángulo azul claro.</span><span class="sxs-lookup"><span data-stu-id="d002c-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="d002c-175">![Colocar un explorador sobre el otro](visual-studio-2013-web-tools/_static/image5.png "Colocar un explorador sobre el otro")</span><span class="sxs-lookup"><span data-stu-id="d002c-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="d002c-176">*Colocar un explorador sobre el otro*</span><span class="sxs-lookup"><span data-stu-id="d002c-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="d002c-177">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-177">Do not close the browsers.</span></span> <span data-ttu-id="d002c-178">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="d002c-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="d002c-179">Tarea 2: uso de código Zen para crear elementos HTML</span><span class="sxs-lookup"><span data-stu-id="d002c-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="d002c-180">La **codificación de Zen** es un complemento de editor para la codificación y edición de HTML de alta velocidad, XML, XSL (o cualquier otro formato de código estructurado).</span><span class="sxs-lookup"><span data-stu-id="d002c-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="d002c-181">El núcleo de este complemento es un eficaz motor de abreviaturas que permite expandir expresiones, similares a los selectores de CSS, en código HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="d002c-182">La codificación Zen es una forma rápida de escribir HTML mediante una sintaxis de selector de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="d002c-183">En este ejercicio, usará la característica de codificación Zen proporcionada por Web Essentials para generar los botones HTML que representan las opciones de la pregunta.</span><span class="sxs-lookup"><span data-stu-id="d002c-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="d002c-184">Vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="d002c-185">Abra el archivo **index. cshtml** que se encuentra en la carpeta **views** | **Home** .</span><span class="sxs-lookup"><span data-stu-id="d002c-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="d002c-186">Reemplace el **&lt;!--todo: agregar opciones aquí--&gt;** comentario con el código siguiente y presione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="d002c-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="d002c-187">El código debe expandirse a HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="d002c-188">![HTML expandido](visual-studio-2013-web-tools/_static/image6.png "HTML expandido")</span><span class="sxs-lookup"><span data-stu-id="d002c-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="d002c-189">*HTML expandido*</span><span class="sxs-lookup"><span data-stu-id="d002c-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-190">Para más información sobre la sintaxis de codificación de Zen, consulte el [artículo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)siguiente.</span><span class="sxs-lookup"><span data-stu-id="d002c-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="d002c-191">Haga clic en el botón **Actualizar exploradores vinculados** para actualizar los dos exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="d002c-192">![Actualizar exploradores vinculados](visual-studio-2013-web-tools/_static/image7.png "Actualizar exploradores vinculados")</span><span class="sxs-lookup"><span data-stu-id="d002c-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="d002c-193">*Actualizar exploradores vinculados*</span><span class="sxs-lookup"><span data-stu-id="d002c-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="d002c-194">![Internet Explorer-Página actualizada con cuatro botones](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-Página actualizada con cuatro botones")</span><span class="sxs-lookup"><span data-stu-id="d002c-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="d002c-195">*Internet Explorer-Página actualizada con cuatro botones*</span><span class="sxs-lookup"><span data-stu-id="d002c-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="d002c-196">![Google Chrome-Página actualizada con cuatro botones](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-Página actualizada con cuatro botones")</span><span class="sxs-lookup"><span data-stu-id="d002c-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="d002c-197">*Google Chrome-Página actualizada con cuatro botones*</span><span class="sxs-lookup"><span data-stu-id="d002c-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="d002c-198">Vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="d002c-199">Ha agregado los botones a la página, pero todavía necesita agregar una pregunta de plantilla.</span><span class="sxs-lookup"><span data-stu-id="d002c-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="d002c-200">Para ello, utilizará una nueva característica de Web Essentials denominada de **Lorem ipsum generator**.</span><span class="sxs-lookup"><span data-stu-id="d002c-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="d002c-201">Busque el elemento **div** con el atributo de **clase** **delante**.</span><span class="sxs-lookup"><span data-stu-id="d002c-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="d002c-202">Agregue el código siguiente como primer elemento secundario del **div**y presione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="d002c-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="d002c-203">El código debe expandirse a HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="d002c-204">![Lorem Ipsum generado automáticamente](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum generado automáticamente")</span><span class="sxs-lookup"><span data-stu-id="d002c-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="d002c-205">*Lorem Ipsum generado automáticamente*</span><span class="sxs-lookup"><span data-stu-id="d002c-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-206">Como parte de la codificación de Zen, ahora puede generar código Lorem ipsum directamente en el editor HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="d002c-207">Simplemente escriba **Lorem** y pulse **Tab** y se insertará un texto de 30 palabras Lorem ipsum.</span><span class="sxs-lookup"><span data-stu-id="d002c-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="d002c-208">P. ej.,</span><span class="sxs-lookup"><span data-stu-id="d002c-208">E.g.</span></span> <span data-ttu-id="d002c-209">*lorem10* inserta 10 palabras ipsum ipsum.</span><span class="sxs-lookup"><span data-stu-id="d002c-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="d002c-210">Agregará un logotipo en la parte superior de la pregunta mediante otra característica nueva en Web Essentials denominada generador de **píxeles Lorem**.</span><span class="sxs-lookup"><span data-stu-id="d002c-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="d002c-211">Agregue el código siguiente como primer elemento secundario del elemento **div** con **Container** como valor de **clase** y presione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="d002c-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="d002c-212">El código debe expandirse a HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="d002c-213">![Lorem pixel generado automáticamente](visual-studio-2013-web-tools/_static/image11.png "Lorem pixel generado automáticamente")</span><span class="sxs-lookup"><span data-stu-id="d002c-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="d002c-214">*Lorem pixel generado automáticamente*</span><span class="sxs-lookup"><span data-stu-id="d002c-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-215">Como parte de la codificación de Zen, también puede generar código Lorem píxel directamente en el editor HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="d002c-216">Simplemente escriba **PIX-200 x 200-Animals** y presione la **tecla TAB** y se insertará una etiqueta **IMG** con una imagen de 200 x 200 de un animal.</span><span class="sxs-lookup"><span data-stu-id="d002c-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="d002c-217">Para obtener más información, consulte [Lorem pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="d002c-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="d002c-218">Haga clic en el botón **Actualizar exploradores vinculados** para actualizar los dos exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="d002c-219">![Internet Explorer: imagen y texto generados automáticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer: imagen y texto generados automáticamente")</span><span class="sxs-lookup"><span data-stu-id="d002c-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="d002c-220">*Internet Explorer: imagen y texto generados automáticamente*</span><span class="sxs-lookup"><span data-stu-id="d002c-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="d002c-221">![Google Chrome: imagen y texto generados automáticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome: imagen y texto generados automáticamente")</span><span class="sxs-lookup"><span data-stu-id="d002c-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="d002c-222">*Google Chrome: imagen y texto generados automáticamente*</span><span class="sxs-lookup"><span data-stu-id="d002c-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-223">Dado que la imagen se selecciona aleatoriamente al agregar el fragmento de código, la imagen que se muestra en los exploradores puede ser diferente.</span><span class="sxs-lookup"><span data-stu-id="d002c-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="d002c-224">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-224">Do not close the browsers.</span></span> <span data-ttu-id="d002c-225">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="d002c-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="d002c-226">Tarea 3: actualizar una propiedad de estilo</span><span class="sxs-lookup"><span data-stu-id="d002c-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="d002c-227">En esta tarea, usará la característica de **modo inspeccionar** del vínculo del explorador para detectar la ubicación exacta en la que se genera el elemento DOM específico y, a continuación, actualizar la propiedad color de ese elemento mediante un selector de colores proporcionado por Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="d002c-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="d002c-228">En el explorador de Internet Explorer, presione **CTRL** + **Alt** + **I** habilitar el modo de inspección.</span><span class="sxs-lookup"><span data-stu-id="d002c-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="d002c-229">Mueva el puntero sobre el borde azul claro y haga clic en.</span><span class="sxs-lookup"><span data-stu-id="d002c-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="d002c-230">![Mover el puntero sobre el borde de color azul claro](visual-studio-2013-web-tools/_static/image14.png "Mover el puntero sobre el borde de color azul claro")</span><span class="sxs-lookup"><span data-stu-id="d002c-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="d002c-231">*Mover el puntero sobre el borde de color azul claro*</span><span class="sxs-lookup"><span data-stu-id="d002c-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="d002c-232">Vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="d002c-233">Observe cómo el elemento HTML que seleccionó en el explorador también está seleccionado en el editor HTML de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="d002c-234">![Elemento HTML seleccionado en el editor HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML seleccionado en el editor HTML de Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="d002c-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="d002c-235">*Elemento HTML seleccionado en el editor HTML de Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="d002c-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="d002c-236">Ahora se actualizará la clase CSS **frontal** para cambiar el estilo del elemento seleccionado.</span><span class="sxs-lookup"><span data-stu-id="d002c-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="d002c-237">Para ello, presione **CTRL** \*\* + para\*\* abrir el cuadro de búsqueda **navegar a** .</span><span class="sxs-lookup"><span data-stu-id="d002c-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="d002c-238">Escriba **site. CSS** y presione **entrar** para abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="d002c-239">![Abriendo archivo site. CSS](visual-studio-2013-web-tools/_static/image16.png "Abriendo archivo site. CSS")</span><span class="sxs-lookup"><span data-stu-id="d002c-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="d002c-240">*Abriendo archivo site. CSS*</span><span class="sxs-lookup"><span data-stu-id="d002c-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="d002c-241">Presione **CTRL** + **F** y escriba **. Flip-container. Front** para buscar el selector de CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="d002c-242">Haga clic en el cuadrado azul claro en la propiedad border de la clase para abrir el selector de colores.</span><span class="sxs-lookup"><span data-stu-id="d002c-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="d002c-243">![Abrir el selector de colores](visual-studio-2013-web-tools/_static/image17.png "Abrir el selector de colores")</span><span class="sxs-lookup"><span data-stu-id="d002c-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="d002c-244">*Abrir el selector de colores*</span><span class="sxs-lookup"><span data-stu-id="d002c-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="d002c-245">Expanda el selector de colores haciendo clic en el botón de contenido adicional y seleccione un nuevo color.</span><span class="sxs-lookup"><span data-stu-id="d002c-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="d002c-246">![Expandir el selector de colores](visual-studio-2013-web-tools/_static/image18.png "Expandir el selector de colores")</span><span class="sxs-lookup"><span data-stu-id="d002c-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="d002c-247">*Expandir el selector de colores*</span><span class="sxs-lookup"><span data-stu-id="d002c-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="d002c-248">Presione **CTRL** + **Alt** + **entrar** para actualizar los exploradores vinculados.</span><span class="sxs-lookup"><span data-stu-id="d002c-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="d002c-249">Cambie a Internet Explorer y observe cómo ha cambiado el color del borde.</span><span class="sxs-lookup"><span data-stu-id="d002c-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="d002c-250">![Internet Explorer-color actualizado del borde](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-color actualizado del borde")</span><span class="sxs-lookup"><span data-stu-id="d002c-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="d002c-251">*Internet Explorer-color actualizado del borde*</span><span class="sxs-lookup"><span data-stu-id="d002c-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="d002c-252">Cambie a Google Chrome y observe cómo ha cambiado el color del borde.</span><span class="sxs-lookup"><span data-stu-id="d002c-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="d002c-253">![Google Chrome: color de borde actualizado](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: color de borde actualizado")</span><span class="sxs-lookup"><span data-stu-id="d002c-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="d002c-254">*Google Chrome: color de borde actualizado*</span><span class="sxs-lookup"><span data-stu-id="d002c-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="d002c-255">Vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="d002c-256">Vaya al final del archivo **site. CSS** y presione **Ctrl** + **F** para buscar el selector **. BTN** .</span><span class="sxs-lookup"><span data-stu-id="d002c-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="d002c-257">Observe que la propiedad **-webkit-border-radius** está subrayada en verde.</span><span class="sxs-lookup"><span data-stu-id="d002c-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="d002c-258">![propiedad-webkit-border-radius del selector BTN](visual-studio-2013-web-tools/_static/image21.png "propiedad-webkit-border-radius del selector BTN")</span><span class="sxs-lookup"><span data-stu-id="d002c-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="d002c-259">*propiedad-webkit-border-radius del selector BTN*</span><span class="sxs-lookup"><span data-stu-id="d002c-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="d002c-260">Colocar el símbolo de intercalación en la propiedad **-webkit-border-radius** .</span><span class="sxs-lookup"><span data-stu-id="d002c-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="d002c-261">Debe aparecer una línea azul debajo de la primera letra de la primera palabra de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="d002c-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="d002c-262">Esta es la **etiqueta inteligente**.</span><span class="sxs-lookup"><span data-stu-id="d002c-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="d002c-263">Presione **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="d002c-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="d002c-264">para abrir el menú sugerencias y haga clic en **Agregar propiedad estándar que falta (Border-RADIUS)** .</span><span class="sxs-lookup"><span data-stu-id="d002c-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="d002c-265">![Agregar sugerencia de propiedad estándar que falta](visual-studio-2013-web-tools/_static/image22.png "Agregar sugerencia de propiedad estándar que falta")</span><span class="sxs-lookup"><span data-stu-id="d002c-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="d002c-266">*Agregar sugerencia de propiedad estándar que falta*</span><span class="sxs-lookup"><span data-stu-id="d002c-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="d002c-267">La propiedad **border-radius** se agrega automáticamente a la regla CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="d002c-268">![Falta la propiedad estándar agregada](visual-studio-2013-web-tools/_static/image23.png "Falta la propiedad estándar agregada")</span><span class="sxs-lookup"><span data-stu-id="d002c-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="d002c-269">*Falta la propiedad estándar agregada*</span><span class="sxs-lookup"><span data-stu-id="d002c-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="d002c-270">Mueva el puntero sobre la propiedad **border-radius** para mostrar la **información sobre herramientas**de la matriz de explorador.</span><span class="sxs-lookup"><span data-stu-id="d002c-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="d002c-271">La **información sobre herramientas** de la matriz del explorador muestra la disponibilidad de la propiedad en cada explorador.</span><span class="sxs-lookup"><span data-stu-id="d002c-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="d002c-272">![Información sobre herramientas de la matriz de explorador](visual-studio-2013-web-tools/_static/image24.png "Información sobre herramientas de la matriz de explorador")</span><span class="sxs-lookup"><span data-stu-id="d002c-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="d002c-273">*Información sobre herramientas de la matriz de explorador*</span><span class="sxs-lookup"><span data-stu-id="d002c-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="d002c-274">Observe que el valor de la propiedad **border-radius** todavía está subrayado.</span><span class="sxs-lookup"><span data-stu-id="d002c-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="d002c-275">Mueva el puntero sobre el valor para ver el mensaje de advertencia.</span><span class="sxs-lookup"><span data-stu-id="d002c-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="d002c-276">![ADVERTENCIA de valor de propiedad border-radius](visual-studio-2013-web-tools/_static/image25.png "ADVERTENCIA de valor de propiedad border-radius")</span><span class="sxs-lookup"><span data-stu-id="d002c-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="d002c-277">*ADVERTENCIA de valor de propiedad border-radius*</span><span class="sxs-lookup"><span data-stu-id="d002c-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="d002c-278">Quite la unidad del valor de propiedad **border-radius** como se sugiere en la información sobre herramientas.</span><span class="sxs-lookup"><span data-stu-id="d002c-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="d002c-279">Como **border-radius** es la propiedad estándar para definir el modo en que se redondean las esquinas de los bordes redondeados, puede quitar la propiedad **-webkit-border-radius** y el valor de la regla de CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="d002c-280">Coloque el símbolo de intercalación en la propiedad de **ajuste automático de texto** y observe que la etiqueta inteligente también aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="d002c-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="d002c-281">Abra el menú y haga clic en **Agregar información específica del proveedor que falta**.</span><span class="sxs-lookup"><span data-stu-id="d002c-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="d002c-282">![Agregar sugerencia de detalles de proveedor que faltan](visual-studio-2013-web-tools/_static/image26.png "Agregar sugerencia de detalles de proveedor que faltan")</span><span class="sxs-lookup"><span data-stu-id="d002c-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="d002c-283">*Agregar sugerencia de detalles de proveedor que faltan*</span><span class="sxs-lookup"><span data-stu-id="d002c-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="d002c-284">La propiedad **-MS-Word-Wrap** se agrega automáticamente a la regla CSS.</span><span class="sxs-lookup"><span data-stu-id="d002c-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="d002c-285">![Propiedad específica del proveedor agregada](visual-studio-2013-web-tools/_static/image27.png "Propiedad específica del proveedor agregada")</span><span class="sxs-lookup"><span data-stu-id="d002c-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="d002c-286">*Propiedad específica del proveedor agregada*</span><span class="sxs-lookup"><span data-stu-id="d002c-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="d002c-287">Tarea 4: actualización del código HTML desde el explorador</span><span class="sxs-lookup"><span data-stu-id="d002c-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="d002c-288">En esta tarea, usará la característica de **modo de diseño** del vínculo del explorador para editar el objeto DOM desde el explorador y transferir los cambios al archivo de código fuente HTML en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d002c-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="d002c-289">En Google Chrome, presione **CTRL** + **Alt** + **D** para habilitar el modo de diseño.</span><span class="sxs-lookup"><span data-stu-id="d002c-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="d002c-290">Mueva el puntero sobre la etiqueta **Lorem ipsum dolor sit amet** y haga clic en.</span><span class="sxs-lookup"><span data-stu-id="d002c-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="d002c-291">![Editar la pregunta](visual-studio-2013-web-tools/_static/image28.png "Editar la pregunta")</span><span class="sxs-lookup"><span data-stu-id="d002c-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="d002c-292">*Editar la pregunta*</span><span class="sxs-lookup"><span data-stu-id="d002c-292">*Editing the question*</span></span>
3. <span data-ttu-id="d002c-293">Debería aparecer un cursor.</span><span class="sxs-lookup"><span data-stu-id="d002c-293">A cursor should appear.</span></span> <span data-ttu-id="d002c-294">Reemplace el texto original por el *que aparece cuando escribo una pregunta más larga*y, a continuación, presione **ESC** para salir del modo de diseño.</span><span class="sxs-lookup"><span data-stu-id="d002c-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="d002c-295">![Pregunta modificada](visual-studio-2013-web-tools/_static/image29.png "Pregunta modificada")</span><span class="sxs-lookup"><span data-stu-id="d002c-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="d002c-296">*Pregunta modificada*</span><span class="sxs-lookup"><span data-stu-id="d002c-296">*Question edited*</span></span>
4. <span data-ttu-id="d002c-297">Vuelva a Visual Studio y Abra **index. cshtml**, si aún no está abierto.</span><span class="sxs-lookup"><span data-stu-id="d002c-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="d002c-298">Observe que se ha actualizado el texto interno del elemento **&lt;p&gt;** .</span><span class="sxs-lookup"><span data-stu-id="d002c-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="d002c-299">![Pregunta actualizada en la página HTML](visual-studio-2013-web-tools/_static/image30.png "Pregunta actualizada en la página HTML")</span><span class="sxs-lookup"><span data-stu-id="d002c-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="d002c-300">*Pregunta actualizada en la página HTML*</span><span class="sxs-lookup"><span data-stu-id="d002c-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="d002c-301">Tarea 5: revisar las advertencias relacionadas con SEO</span><span class="sxs-lookup"><span data-stu-id="d002c-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="d002c-302">La **optimización del motor de búsqueda** (SEO) es el proceso de crear una clasificación de sitio web superior en la lista de resultados de un motor de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="d002c-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="d002c-303">Cuanto mayor sea el número de sitios y más constantemente se muestren, más visitantes obtendrá el sitio de ese motor de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="d002c-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="d002c-304">Web Essentials incorpora una herramienta de análisis que examina HTML, notifica los problemas encontrados y proporciona ayuda para corregirlos.</span><span class="sxs-lookup"><span data-stu-id="d002c-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="d002c-305">Vaya al menú **Ver** y haga clic en **lista de errores** para abrir la ventana de **lista de errores** .</span><span class="sxs-lookup"><span data-stu-id="d002c-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="d002c-306">![Lista de errores en el menú Ver](visual-studio-2013-web-tools/_static/image31.png "Lista de errores en el menú Ver")</span><span class="sxs-lookup"><span data-stu-id="d002c-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="d002c-307">*Lista de errores en el menú Ver*</span><span class="sxs-lookup"><span data-stu-id="d002c-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="d002c-308">Observe que hay una advertencia de SEO que notifica que falta un **&lt;etiqueta meta&gt;** de la descripción de la página.</span><span class="sxs-lookup"><span data-stu-id="d002c-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="d002c-309">Haga doble clic en la entrada de advertencia de SEO para corregirlo.</span><span class="sxs-lookup"><span data-stu-id="d002c-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="d002c-310">![Lista de errores (ventana)](visual-studio-2013-web-tools/_static/image32.png "Lista de errores (ventana)")</span><span class="sxs-lookup"><span data-stu-id="d002c-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="d002c-311">*Lista de errores (ventana)*</span><span class="sxs-lookup"><span data-stu-id="d002c-311">*Error List window*</span></span>
3. <span data-ttu-id="d002c-312">En el cuadro de diálogo **Web Essentials** , haga clic en **sí** para insertar una descripción &lt;etiqueta meta&gt;.</span><span class="sxs-lookup"><span data-stu-id="d002c-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="d002c-313">![Web Essentials (cuadro de diálogo)](visual-studio-2013-web-tools/_static/image33.png "Web Essentials (cuadro de diálogo)")</span><span class="sxs-lookup"><span data-stu-id="d002c-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="d002c-314">*Web Essentials (cuadro de diálogo)*</span><span class="sxs-lookup"><span data-stu-id="d002c-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="d002c-315">Se abre el editor de **\_layout. cshtml** y la etiqueta **&lt;meta&gt;** se agrega automáticamente a la sección de **encabezado** del archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="d002c-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="d002c-316">![Etiqueta meta agregada automáticamente en _Layout página](visual-studio-2013-web-tools/_static/image34.png "Etiqueta meta agregada automáticamente en _Layout página")</span><span class="sxs-lookup"><span data-stu-id="d002c-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="d002c-317">*Etiqueta meta agregada automáticamente a \_página de diseño*</span><span class="sxs-lookup"><span data-stu-id="d002c-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="d002c-318">Cambie el valor del atributo **Content** a *GeekQuiz* y guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="d002c-319">Ejercicio 2: sacar partido de los fragmentos de código e IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d002c-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="d002c-320">Con Web Essentials, el editor HTML se ha ampliado con funcionalidad adicional.</span><span class="sxs-lookup"><span data-stu-id="d002c-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="d002c-321">En este ejercicio, verá algunas características nuevas que son útiles al desarrollar aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="d002c-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="d002c-322">Tarea 1: uso de IntelliSense en documentos HTML</span><span class="sxs-lookup"><span data-stu-id="d002c-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="d002c-323">La primera característica nueva que verá en esta tarea se denomina **IntelliSense dinámico**.</span><span class="sxs-lookup"><span data-stu-id="d002c-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="d002c-324">IntelliSense dinámico Lee otras etiquetas y atributos para deducir los posibles identificadores que se usarán.</span><span class="sxs-lookup"><span data-stu-id="d002c-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="d002c-325">En esta tarea, creará un nuevo elemento de formulario HTML que contiene una etiqueta y un campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="d002c-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="d002c-326">A continuación, agregará un atributo **for** a la etiqueta para enlazarlo a la entrada y verá sugerencias de IntelliSense basadas en los identificadores de las entradas en el ámbito.</span><span class="sxs-lookup"><span data-stu-id="d002c-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="d002c-327">Abra **Visual Studio Express 2013 para web** y la solución **Begin. sln** que se encuentra en la carpeta **source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** .</span><span class="sxs-lookup"><span data-stu-id="d002c-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="d002c-328">Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="d002c-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="d002c-329">En **Explorador de soluciones**, abra el archivo **index. cshtml** que se encuentra en la carpeta **views** | **Home** .</span><span class="sxs-lookup"><span data-stu-id="d002c-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="d002c-330">Agregue el siguiente formulario dentro de la **sección&lt;&gt;** elemento.</span><span class="sxs-lookup"><span data-stu-id="d002c-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="d002c-331">(Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *formulario*)</span><span class="sxs-lookup"><span data-stu-id="d002c-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="d002c-332">La etiqueta de entrada debe ir precedida de una etiqueta con alguna Descripción del campo.</span><span class="sxs-lookup"><span data-stu-id="d002c-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="d002c-333">Agregue la siguiente etiqueta delante de la etiqueta de entrada.</span><span class="sxs-lookup"><span data-stu-id="d002c-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="d002c-334">El atributo **for** de una **etiqueta de&lt;&gt;** especifica a qué elemento de formulario está enlazada una etiqueta.</span><span class="sxs-lookup"><span data-stu-id="d002c-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="d002c-335">El valor del atributo debe ser igual al identificador del elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="d002c-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="d002c-336">Agregue el atributo **for** al elemento **&lt;etiqueta&gt;** .</span><span class="sxs-lookup"><span data-stu-id="d002c-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="d002c-337">Como se muestra en la ilustración siguiente, el nombre de &quot;&quot; valor aparece en el cuadro IntelliSense, en función del identificador de los elementos que se encuentran dentro del mismo ámbito (el **formulario de&lt;envolvente&gt;** ).</span><span class="sxs-lookup"><span data-stu-id="d002c-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="d002c-338">![Mostrar el identificador en IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Mostrar el identificador en IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="d002c-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="d002c-339">*Mostrar el identificador en IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="d002c-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="d002c-340">Elimine el **formulario&lt;** recientemente agregado&gt;elemento y su contenido.</span><span class="sxs-lookup"><span data-stu-id="d002c-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="d002c-341">Tarea 2: usar fragmentos de código HTML</span><span class="sxs-lookup"><span data-stu-id="d002c-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="d002c-342">HTML5 presentó más de 25 etiquetas semánticas nuevas.</span><span class="sxs-lookup"><span data-stu-id="d002c-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="d002c-343">Visual Studio ya tenía compatibilidad con IntelliSense para estas etiquetas, pero Visual Studio 2013 agiliza y facilita la escritura de marcado agregando nuevos fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="d002c-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="d002c-344">Aunque estas etiquetas no son complicadas, incluyen algunos matices pequeños, como agregar las reservas de códec correctas para la etiqueta de *audio* .</span><span class="sxs-lookup"><span data-stu-id="d002c-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="d002c-345">En esta tarea, verá los fragmentos de código HTML para la etiqueta de audio.</span><span class="sxs-lookup"><span data-stu-id="d002c-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="d002c-346">En el archivo **index. cshtml** , escriba **&lt;AUD** dentro de la **sección&lt;&gt;** elemento, tal y como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="d002c-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="d002c-347">![Insertar un elemento de audio](visual-studio-2013-web-tools/_static/image36.png "Insertar un elemento de audio")</span><span class="sxs-lookup"><span data-stu-id="d002c-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="d002c-348">*Insertar un elemento de audio*</span><span class="sxs-lookup"><span data-stu-id="d002c-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="d002c-349">Presione la **tecla TAB** dos veces y observe cómo se agrega el código siguiente en la página y el cursor se coloca en el atributo **src** del primer origen.</span><span class="sxs-lookup"><span data-stu-id="d002c-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="d002c-350">Al presionar la tecla **Tab** dos veces, se inserta el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="d002c-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="d002c-351">El fragmento de código de audio muestra el uso estándar de la etiqueta de *audio* , con dos archivos de origen para mejorar la compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="d002c-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="d002c-352">Elimine la segunda línea y actualice el origen de la primera línea con el siguiente vínculo a WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="d002c-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="d002c-353">El código resultante se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="d002c-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="d002c-354">El archivo de código fuente *KatanaProject. mp3* se usa como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="d002c-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="d002c-355">Puede usar otro origen si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="d002c-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="d002c-356">Presione **CTRL** + **S** para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="d002c-357">Presione **CTRL** + **F5** para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d002c-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="d002c-358">Observe que se ha agregado un reproductor de audio a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d002c-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="d002c-359">![Reproductor de audio en Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Reproductor de audio en Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="d002c-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="d002c-360">*Reproductor de audio en Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="d002c-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="d002c-361">![Reproductor de audio en Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Reproductor de audio en Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="d002c-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="d002c-362">*Reproductor de audio en Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="d002c-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="d002c-363">No cierre los exploradores.</span><span class="sxs-lookup"><span data-stu-id="d002c-363">Do not close the browsers.</span></span> <span data-ttu-id="d002c-364">Los usará en la siguiente tarea.</span><span class="sxs-lookup"><span data-stu-id="d002c-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="d002c-365">Tarea 3: usar IntelliSense en documentos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="d002c-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="d002c-366">Con Web Essentials 2013, hojas de estilos y páginas HTML, se genera una lista de identificadores y nombres de clase.</span><span class="sxs-lookup"><span data-stu-id="d002c-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="d002c-367">En esta tarea, aprenderá cómo esas listas mejoran la compatibilidad con IntelliSense para JavaScript en Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="d002c-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="d002c-368">En el archivo **index. cshtml** , agregue el código siguiente para definir una etiqueta de **script** para el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d002c-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="d002c-369">Agregue el código siguiente dentro de la etiqueta de **script** para definir la función de devolución de llamada Ready.</span><span class="sxs-lookup"><span data-stu-id="d002c-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="d002c-370">(Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="d002c-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="d002c-371">Coloque el símbolo de intercalación en la etiqueta de **script** y presione **Ctrl** +  **.**</span><span class="sxs-lookup"><span data-stu-id="d002c-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="d002c-372">para abrir el menú de sugerencias.</span><span class="sxs-lookup"><span data-stu-id="d002c-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="d002c-373">Haga clic en **extraer a archivo**.</span><span class="sxs-lookup"><span data-stu-id="d002c-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="d002c-374">![Sugerencia de extracción a archivo de JavaScript](visual-studio-2013-web-tools/_static/image39.png "Sugerencia de extracción a archivo de JavaScript")</span><span class="sxs-lookup"><span data-stu-id="d002c-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="d002c-375">*Sugerencia de extracción a archivo de JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d002c-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="d002c-376">En la ventana **Guardar como** , seleccione la carpeta **scripts** , asigne al archivo el nombre **init. js** y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="d002c-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="d002c-377">![Guardar como ventana](visual-studio-2013-web-tools/_static/image40.png "Guardar como ventana")</span><span class="sxs-lookup"><span data-stu-id="d002c-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="d002c-378">*Guardar como ventana*</span><span class="sxs-lookup"><span data-stu-id="d002c-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-379">Se crea el archivo **init. js** y el contenido del script se mueve al archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="d002c-380">![Archivo init. js creado con el contenido incluido](visual-studio-2013-web-tools/_static/image41.png "Archivo init. js creado con el contenido incluido")</span><span class="sxs-lookup"><span data-stu-id="d002c-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="d002c-381">*Archivo init. js creado con el contenido incluido*</span><span class="sxs-lookup"><span data-stu-id="d002c-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="d002c-382">Abra el archivo **index. cshtml** y compruebe que la etiqueta de script se ha reemplazado por una referencia al archivo **init. js** .</span><span class="sxs-lookup"><span data-stu-id="d002c-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="d002c-383">![Referencia HTML de init. js](visual-studio-2013-web-tools/_static/image42.png "Referencia HTML de init. js")</span><span class="sxs-lookup"><span data-stu-id="d002c-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="d002c-384">*Referencia HTML de init. js*</span><span class="sxs-lookup"><span data-stu-id="d002c-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="d002c-385">Vaya al **Explorador de soluciones** y observe que el archivo **init. js** se incluyó automáticamente en la solución.</span><span class="sxs-lookup"><span data-stu-id="d002c-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="d002c-386">![Archivo init. js incluido en la solución](visual-studio-2013-web-tools/_static/image43.png "Archivo init. js incluido en la solución")</span><span class="sxs-lookup"><span data-stu-id="d002c-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="d002c-387">*Archivo init. js incluido en la solución*</span><span class="sxs-lookup"><span data-stu-id="d002c-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="d002c-388">Vuelva al archivo **init. js** para actualizar la devolución de llamada de función **preparada** .</span><span class="sxs-lookup"><span data-stu-id="d002c-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="d002c-389">Dentro de la definición de devolución de llamada de función que se pasa a *Ready*, agregue el código siguiente para obtener todos los elementos de un atributo de clase específico.</span><span class="sxs-lookup"><span data-stu-id="d002c-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="d002c-390">Presione **CTRL** + **espacio** entre comillas dentro de la llamada a la función **getElementsByClassName** .</span><span class="sxs-lookup"><span data-stu-id="d002c-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="d002c-391">![Mostrar IntelliSense para la función getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Mostrar IntelliSense para la función getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="d002c-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="d002c-392">*Mostrar IntelliSense para la función getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="d002c-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-393">Observe que IntelliSense muestra las clases definidas en las hojas de estilos del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d002c-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="d002c-394">Reemplace la línea que ha creado con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="d002c-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="d002c-395">Coloque el cursor después de **au** dentro de las comillas en la función **getElementsByTagName** y presione **Ctrl** + **espacio**.</span><span class="sxs-lookup"><span data-stu-id="d002c-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="d002c-396">![Mostrar IntelliSense para el método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Mostrar IntelliSense para el método getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="d002c-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="d002c-397">*Mostrar IntelliSense para el método getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="d002c-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="d002c-398">Seleccione **&quot;&quot;de audio** de la lista y presione **entrar**.</span><span class="sxs-lookup"><span data-stu-id="d002c-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="d002c-399">El resultado se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="d002c-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="d002c-400">![Recuperación de elementos de audio](visual-studio-2013-web-tools/_static/image46.png "Recuperación de elementos de audio")</span><span class="sxs-lookup"><span data-stu-id="d002c-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="d002c-401">*Recuperación de elementos de audio*</span><span class="sxs-lookup"><span data-stu-id="d002c-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="d002c-402">En **Explorador de soluciones**, haga clic con el botón derecho en el archivo **init. js** de la carpeta **scripts** y seleccione **minimizar JavaScript files** en el menú **Web Essentials** .</span><span class="sxs-lookup"><span data-stu-id="d002c-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="d002c-403">![Archivos de minimizar JavaScript](visual-studio-2013-web-tools/_static/image47.png "Archivos JavaScript de minimizar")</span><span class="sxs-lookup"><span data-stu-id="d002c-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="d002c-404">*Archivos de minimizar JavaScript*</span><span class="sxs-lookup"><span data-stu-id="d002c-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="d002c-405">Cuando se le pida que habilite minificación automática cuando el archivo de código fuente cambie, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="d002c-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="d002c-406">![Habilitar la advertencia automática de minificación](visual-studio-2013-web-tools/_static/image48.png "Habilitar la advertencia automática de minificación")</span><span class="sxs-lookup"><span data-stu-id="d002c-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="d002c-407">*Habilitar la advertencia automática de minificación*</span><span class="sxs-lookup"><span data-stu-id="d002c-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="d002c-408">**Init. min. js** se crea y se agrega como una dependencia del archivo **init. js** .</span><span class="sxs-lookup"><span data-stu-id="d002c-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="d002c-409">![Archivo init. min. js creado](visual-studio-2013-web-tools/_static/image49.png "Archivo init. min. js creado")</span><span class="sxs-lookup"><span data-stu-id="d002c-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="d002c-410">*Archivo init. min. js creado*</span><span class="sxs-lookup"><span data-stu-id="d002c-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="d002c-411">Abra el archivo **init. min. js** y observe que el archivo es reducida.</span><span class="sxs-lookup"><span data-stu-id="d002c-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="d002c-412">![Contenido del archivo init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenido del archivo init. min. js")</span><span class="sxs-lookup"><span data-stu-id="d002c-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="d002c-413">*Contenido del archivo init. min. js*</span><span class="sxs-lookup"><span data-stu-id="d002c-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="d002c-414">En el archivo **init. js** , agregue el siguiente código debajo de la llamada de la función **getElementsByTagName** para reproducir todos los elementos de audio.</span><span class="sxs-lookup"><span data-stu-id="d002c-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="d002c-415">(Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="d002c-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="d002c-416">Haga clic en **CTRL** + **S** para guardar el archivo.</span><span class="sxs-lookup"><span data-stu-id="d002c-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="d002c-417">Puesto que el archivo reducida ya está abierto, verá un cuadro de diálogo que indica que el archivo se modificó fuera del editor de código fuente.</span><span class="sxs-lookup"><span data-stu-id="d002c-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="d002c-418">Haga clic en **Sí**.</span><span class="sxs-lookup"><span data-stu-id="d002c-418">Click **Yes**.</span></span>

    <span data-ttu-id="d002c-419">![Microsoft Visual Studio ADVERTENCIA](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio ADVERTENCIA")</span><span class="sxs-lookup"><span data-stu-id="d002c-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="d002c-420">*Microsoft Visual Studio ADVERTENCIA*</span><span class="sxs-lookup"><span data-stu-id="d002c-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="d002c-421">Vuelva al archivo **init. min. js** para comprobar que el archivo se actualizó con el código nuevo.</span><span class="sxs-lookup"><span data-stu-id="d002c-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="d002c-422">![Archivo init. min. js actualizado](visual-studio-2013-web-tools/_static/image52.png "Archivo init. min. js actualizado")</span><span class="sxs-lookup"><span data-stu-id="d002c-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="d002c-423">*Archivo init. min. js actualizado*</span><span class="sxs-lookup"><span data-stu-id="d002c-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="d002c-424">Haga clic en el botón **actualizar vínculo del explorador** .</span><span class="sxs-lookup"><span data-stu-id="d002c-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="d002c-425">Una vez actualizados los dos exploradores, los reproductores de audio que vio en la tarea anterior comenzarán a reproducirse automáticamente.</span><span class="sxs-lookup"><span data-stu-id="d002c-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="d002c-426">![Reproductor de audio incluido en la vista](visual-studio-2013-web-tools/_static/image53.png "Reproductor de audio incluido en la vista")</span><span class="sxs-lookup"><span data-stu-id="d002c-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="d002c-427">*Reproductor de audio incluido en la vista*</span><span class="sxs-lookup"><span data-stu-id="d002c-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="d002c-428">Resumen</span><span class="sxs-lookup"><span data-stu-id="d002c-428">Summary</span></span>

<span data-ttu-id="d002c-429">Al completar este laboratorio práctico, ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="d002c-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="d002c-430">Usar las nuevas características del editor HTML incluidas en Web Essentials, como fragmentos de código HTML5 enriquecidos y codificación Zen</span><span class="sxs-lookup"><span data-stu-id="d002c-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="d002c-431">Usar las nuevas características del editor de CSS incluidas en Web Essentials como la información sobre herramientas del selector de colores y la matriz del explorador</span><span class="sxs-lookup"><span data-stu-id="d002c-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="d002c-432">Usar las nuevas características del editor de JavaScript incluidas en Web Essentials como extraer a archivo e IntelliSense para todos los elementos HTML</span><span class="sxs-lookup"><span data-stu-id="d002c-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="d002c-433">Intercambio de datos entre el explorador y Visual Studio mediante el vínculo del explorador</span><span class="sxs-lookup"><span data-stu-id="d002c-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
