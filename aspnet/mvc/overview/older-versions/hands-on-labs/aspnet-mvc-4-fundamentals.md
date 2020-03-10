---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Aspectos básicos de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratorio práctico se basa en la tienda de música de MVC (controlador de vista de modelo), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484573"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="ea3c5-103">Conceptos básicos de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea3c5-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="ea3c5-104">por [equipo de grupos de web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ea3c5-105">Descargar el kit de aprendizaje de Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="ea3c5-106">Este laboratorio práctico se basa en la tienda de música de MVC (controlador de vista de modelo), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="ea3c5-107">En todo el laboratorio aprenderá la simplicidad, pero podrá usar estas tecnologías juntas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="ea3c5-108">Comenzará con una aplicación sencilla y la compilará hasta que tenga una aplicación Web de ASP.NET MVC 4 totalmente funcional.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="ea3c5-109">Este laboratorio funciona con ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="ea3c5-110">Si desea explorar la versión ASP.NET MVC 3 de la aplicación tutorial, puede encontrarla en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="ea3c5-111">Este laboratorio práctico supone que el desarrollador tiene experiencia en tecnologías de desarrollo web, como HTML y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="ea3c5-112">Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="ea3c5-113">El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="ea3c5-114">La aplicación Music Store</span><span class="sxs-lookup"><span data-stu-id="ea3c5-114">The Music Store application</span></span>

<span data-ttu-id="ea3c5-115">La aplicación Web de Music Store que se creará en este laboratorio consta de tres partes principales: compras, retirada y administración.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="ea3c5-116">Los visitantes podrán explorar álbumes por género, agregar álbumes a su carro, revisar su selección y, por último, proceder a la retirada para iniciar sesión y completar el pedido.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="ea3c5-117">Además, los administradores del almacén podrán administrar los álbumes disponibles, así como sus propiedades principales.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="ea3c5-118">![Pantallas de la tienda de música](aspnet-mvc-4-fundamentals/_static/image1.png "Pantallas de la tienda de música")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="ea3c5-119">*Pantallas de la tienda de música*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="ea3c5-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="ea3c5-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="ea3c5-121">La aplicación Music Store se generará con el **controlador de vista de modelo (MVC)** , un patrón arquitectónico que separa una aplicación en tres componentes principales:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="ea3c5-122">**Modelos**: los objetos de modelo son las partes de la aplicación que implementan la lógica del dominio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="ea3c5-123">A menudo, los objetos de modelo también recuperan y almacenan el estado del modelo en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="ea3c5-124">**Vistas:** Las vistas son los componentes que muestran la interfaz de usuario (UI) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="ea3c5-125">Normalmente, esta interfaz de usuario se crea a partir de los datos de modelo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="ea3c5-126">Un ejemplo sería la vista de edición de álbumes que muestra cuadros de texto y una lista desplegable basada en el estado actual de un objeto Album.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="ea3c5-127">**Controladores:** Los controladores son los componentes que controlan la interacción con el usuario, manipulan el modelo y, en última instancia, seleccionan una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="ea3c5-128">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="ea3c5-129">El patrón MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica de negocios y lógica de la interfaz de usuario), a la vez que proporciona un acoplamiento flexible entre estos elementos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="ea3c5-130">Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que le permite centrarse en un aspecto de la implementación a la vez.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="ea3c5-131">Además, el modelo de MVC facilita la prueba de las aplicaciones, fomentando también el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="ea3c5-132">El marco de **MVC de ASP.net** proporciona una alternativa al patrón de formularios Web Forms de ASP.net para crear aplicaciones web basadas en mvc de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="ea3c5-133">El marco de **MVC de ASP.net** es un marco de presentación ligero y muy comprobable que, al igual que con las aplicaciones basadas en formularios Web, se integra con las características de ASP.net existentes, como las páginas maestras y la autenticación basada en pertenencia, de modo que se obtiene toda la eficacia de .NET Framework principal.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="ea3c5-134">Esto resulta útil si ya está familiarizado con los formularios Web Forms de ASP.NET, ya que todas las bibliotecas que ya usa también están disponibles en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="ea3c5-135">Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también promueve el desarrollo paralelo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="ea3c5-136">Por ejemplo, un desarrollador puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y un tercer desarrollador puede centrarse en la lógica de negocios del modelo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="ea3c5-137">Objetivos</span><span class="sxs-lookup"><span data-stu-id="ea3c5-137">Objectives</span></span>

<span data-ttu-id="ea3c5-138">En este laboratorio práctico, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="ea3c5-139">Creación de una aplicación de ASP.NET MVC desde cero basada en el tutorial de la aplicación Music Store</span><span class="sxs-lookup"><span data-stu-id="ea3c5-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="ea3c5-140">Agregar controladores para controlar las direcciones URL en la Página principal del sitio y para examinar su funcionalidad principal</span><span class="sxs-lookup"><span data-stu-id="ea3c5-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="ea3c5-141">Agregar una vista para personalizar el contenido que se muestra junto con su estilo</span><span class="sxs-lookup"><span data-stu-id="ea3c5-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="ea3c5-142">Agregar clases de modelo para contener y administrar la lógica de datos y dominios</span><span class="sxs-lookup"><span data-stu-id="ea3c5-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="ea3c5-143">Usar modelo de modelo de vista para pasar información de las acciones del controlador a las plantillas de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="ea3c5-144">Exploración de la nueva plantilla de ASP.NET MVC 4 para aplicaciones de Internet</span><span class="sxs-lookup"><span data-stu-id="ea3c5-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ea3c5-145">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ea3c5-145">Prerequisites</span></span>

<span data-ttu-id="ea3c5-146">Debe tener los siguientes elementos para completar este laboratorio:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="ea3c5-147">[Visual Studio 2012 Express para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lea [el Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ea3c5-148">Programa de instalación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-148">Setup</span></span>

<span data-ttu-id="ea3c5-149">**Instalar fragmentos de código**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="ea3c5-150">Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="ea3c5-151">Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="ea3c5-152">Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ea3c5-153">Ejercicios</span><span class="sxs-lookup"><span data-stu-id="ea3c5-153">Exercises</span></span>

<span data-ttu-id="ea3c5-154">Este laboratorio práctico se compone de los siguientes ejercicios:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="ea3c5-155">Ejercicio 1: creación de un proyecto de aplicación web MVC de MusicStore ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea3c5-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="ea3c5-156">Ejercicio 2: creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="ea3c5-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="ea3c5-157">Ejercicio 3: pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="ea3c5-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="ea3c5-158">Ejercicio 4: crear una vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="ea3c5-159">Ejercicio 5: crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="ea3c5-160">Ejercicio 6: uso de parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="ea3c5-161">Ejercicio 7: una nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea3c5-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="ea3c5-162">Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="ea3c5-163">Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="ea3c5-164">Tiempo estimado para completar este laboratorio: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="ea3c5-165">Ejercicio 1: creación de un proyecto de aplicación web MVC de MusicStore ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea3c5-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="ea3c5-166">En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como en su organización de carpetas principal.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="ea3c5-167">Además, aprenderá a agregar un nuevo controlador y a mostrar una cadena simple en la Página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="ea3c5-168">Tarea 1: creación del proyecto de aplicación web MVC de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea3c5-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="ea3c5-169">En esta tarea, creará un proyecto de aplicación MVC de ASP.NET vacío mediante la plantilla de Visual Studio MVC.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="ea3c5-170">Inicie **vs Express para web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="ea3c5-171">En el menú **Archivo**, haga clic en **Nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="ea3c5-172">En el cuadro de diálogo **nuevo proyecto** , seleccione el tipo de proyecto **aplicación web MVC 4 ASP.net** , que se encuentra en **Visual C#,** lista de plantillas **Web** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="ea3c5-173">Cambie el **nombre** a *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="ea3c5-174">Establezca la ubicación de la solución dentro de una nueva carpeta de **Inicio** en la carpeta de origen de este ejercicio, por ejemplo **[Your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="ea3c5-175">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-175">Click **OK**.</span></span>

    <span data-ttu-id="ea3c5-176">![Cuadro de diálogo Crear nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "Cuadro de diálogo Crear nuevo proyecto")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="ea3c5-177">*Cuadro de diálogo Crear nuevo proyecto*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="ea3c5-178">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla **básica** y asegúrese de que el **motor de vista** seleccionado sea **Razor**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="ea3c5-179">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-179">Click **OK**.</span></span>

    <span data-ttu-id="ea3c5-180">![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="ea3c5-181">*Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="ea3c5-182">Tarea 2: explorar la estructura de la solución</span><span class="sxs-lookup"><span data-stu-id="ea3c5-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="ea3c5-183">El marco de MVC de ASP.NET incluye una plantilla de proyecto de Visual Studio que le ayuda a crear aplicaciones Web compatibles con el patrón MVC.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="ea3c5-184">Esta plantilla crea una nueva aplicación web MVC ASP.NET con las carpetas, plantillas de elementos y entradas de archivo de configuración necesarias.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="ea3c5-185">En esta tarea, examinará la estructura de la solución para comprender los elementos implicados y sus relaciones.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="ea3c5-186">Las siguientes carpetas se incluyen en toda la aplicación ASP.NET MVC porque el marco ASP.NET MVC usa de forma predeterminada una Convención de &quot;sobre el enfoque de configuración&quot; y realiza algunas suposiciones predeterminadas en función de las convenciones de nomenclatura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="ea3c5-187">Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones del lado derecho:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="ea3c5-188">![ASP.NET estructura de carpetas de MVC en Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET estructura de carpetas de MVC en Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="ea3c5-189">*ASP.NET estructura de carpetas de MVC en Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="ea3c5-190">**Controladores**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-190">**Controllers**.</span></span> <span data-ttu-id="ea3c5-191">Esta carpeta contendrá las clases de controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="ea3c5-192">En una aplicación basada en MVC, los controladores son responsables de administrar la interacción con el usuario final, manipular el modelo y, en última instancia, elegir una vista para representar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="ea3c5-193">El marco de MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;(por ejemplo, HomeController, LoginController o ProductController).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="ea3c5-194">**Modelos**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-194">**Models**.</span></span> <span data-ttu-id="ea3c5-195">Esta carpeta se proporciona para las clases que representan el modelo de aplicación de la aplicación web MVC.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="ea3c5-196">Normalmente incluye código que define los objetos y la lógica para interactuar con el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="ea3c5-197">Normalmente, los objetos del modelo real estarán en bibliotecas de clases independientes.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="ea3c5-198">Sin embargo, cuando se crea una nueva aplicación, se pueden incluir clases y, a continuación, moverlas a bibliotecas de clases independientes en un punto posterior del ciclo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="ea3c5-199">**Vistas**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-199">**Views**.</span></span> <span data-ttu-id="ea3c5-200">Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="ea3c5-201">Las vistas usan archivos. aspx,. ascx,. cshtml y. Master, además de otros archivos relacionados con las vistas de representación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="ea3c5-202">La carpeta views contiene una carpeta para cada controlador. la carpeta se denomina con el prefijo del nombre del controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="ea3c5-203">Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta views contendrá una carpeta denominada Home.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="ea3c5-204">De forma predeterminada, cuando el marco de ASP.NET MVC carga una vista, busca un archivo. aspx con el nombre de vista solicitado en la carpeta Views\controllerName (**views [controllerName] [Action]. aspx**) o (**views [ControllerName] [Action]. cshtml**) para las vistas de Razor.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-205">Además de las carpetas enumeradas anteriormente, una aplicación web MVC utiliza el archivo **global. asax** para establecer los valores predeterminados de enrutamiento de URL global y usa el archivo **Web. config** para configurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="ea3c5-206">Tarea 3: agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="ea3c5-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="ea3c5-207">En las aplicaciones de ASP.NET que no usan el marco MVC, la interacción con el usuario se organiza en torno a las páginas y en torno a la generación y control de eventos de esas páginas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="ea3c5-208">Por el contrario, la interacción del usuario con aplicaciones ASP.NET MVC se organiza en torno a los controladores y sus métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="ea3c5-209">Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases a las que se hace referencia como controladores.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="ea3c5-210">Los controladores procesan las solicitudes entrantes, controlan las interacciones y los datos proporcionados por el usuario, ejecutan la lógica de aplicación adecuada y determinan la respuesta que se devuelve al cliente (muestran HTML, descargan un archivo, redirigen a una dirección URL diferente, etc.).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="ea3c5-211">En el caso de mostrar HTML, una clase de controlador normalmente llama a un componente de vista independiente para generar el marcado HTML para la solicitud.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="ea3c5-212">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="ea3c5-213">En esta tarea, agregará una clase de controlador que controlará las direcciones URL de la Página principal del sitio de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="ea3c5-214">Haga clic con el botón derecho en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, comando del **controlador** :</span><span class="sxs-lookup"><span data-stu-id="ea3c5-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="ea3c5-215">![Agregar un comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "Agregar un comando de controlador")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="ea3c5-216">*Agregar controlador (comando)*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="ea3c5-217">Aparece el cuadro de diálogo **Agregar controlador** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="ea3c5-218">Asigne al controlador el nombre *HomeController* y presione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="ea3c5-219">![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Cuadro de diálogo Agregar controlador")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="ea3c5-220">*Cuadro de diálogo Agregar controlador*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="ea3c5-221">El archivo **HomeController.CS** se crea en la carpeta **Controllers** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="ea3c5-222">Para que el **HomeController** devuelva una cadena en su acción de índice, reemplace el método de **Índice** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-223">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX1 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="ea3c5-224">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="ea3c5-225">En esta tarea, probará la aplicación en un explorador Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="ea3c5-226">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="ea3c5-227">El proyecto se compila y se inicia el servidor Web de IIS local.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="ea3c5-228">El servidor Web de IIS local abrirá automáticamente un explorador Web que apunte a la dirección URL del servidor Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="ea3c5-229">![Aplicación que se ejecuta en un explorador Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplicación que se ejecuta en un explorador Web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="ea3c5-230">*Aplicación que se ejecuta en un explorador Web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-231">El servidor Web de IIS local ejecutará el sitio web en un número de puerto libre aleatorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="ea3c5-232">En la ilustración anterior, el sitio se ejecuta en `http://localhost:50103/`, por lo que usa el puerto 50103.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="ea3c5-233">El número de puerto puede variar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-233">Your port number may vary.</span></span>
2. <span data-ttu-id="ea3c5-234">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="ea3c5-235">Ejercicio 2: creación de un controlador</span><span class="sxs-lookup"><span data-stu-id="ea3c5-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="ea3c5-236">En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar una funcionalidad sencilla de la aplicación Music Store.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="ea3c5-237">Ese controlador definirá métodos de acción para controlar cada una de las siguientes solicitudes específicas:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="ea3c5-238">Página de lista de los géneros musicales en la tienda de música</span><span class="sxs-lookup"><span data-stu-id="ea3c5-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="ea3c5-239">Página de exploración en la que se muestran todos los álbumes de música de un género determinado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="ea3c5-240">Una página de detalles que muestra información sobre un álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="ea3c5-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="ea3c5-241">Para el ámbito de este ejercicio, esas acciones simplemente devolverán una cadena en este momento.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="ea3c5-242">Tarea 1: agregar una nueva clase StoreController</span><span class="sxs-lookup"><span data-stu-id="ea3c5-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="ea3c5-243">En esta tarea, agregará un nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="ea3c5-244">Si aún no está abierto, inicie **VS Express para Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="ea3c5-245">En el menú **archivo** , elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="ea3c5-246">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02-CreatingAController\Begin**, seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="ea3c5-247">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="ea3c5-248">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ea3c5-249">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ea3c5-250">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ea3c5-251">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-252">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ea3c5-253">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ea3c5-254">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="ea3c5-255">Agregue el nuevo controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-255">Add the new controller.</span></span> <span data-ttu-id="ea3c5-256">Para ello, haga clic con el botón secundario en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, el comando **controlador** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="ea3c5-257">Cambie el **nombre del controlador** a *StoreController*y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="ea3c5-258">![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Cuadro de diálogo Agregar controlador")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="ea3c5-259">*Cuadro de diálogo Agregar controlador*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="ea3c5-260">Tarea 2: modificación de las acciones de StoreController</span><span class="sxs-lookup"><span data-stu-id="ea3c5-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="ea3c5-261">En esta tarea, modificará los métodos de controlador que se denominan **acciones**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="ea3c5-262">Las acciones son responsables de administrar las solicitudes de URL y de determinar el contenido que se debe devolver al explorador o al usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="ea3c5-263">La clase **StoreController** ya tiene un método de **Índice** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="ea3c5-264">La usará más adelante en este laboratorio para implementar la página en la que se enumeran todos los géneros de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="ea3c5-265">Por ahora, solo tiene que reemplazar el método de **Índice** por el código siguiente, que devuelve una cadena &quot;Hello de Store. index ()&quot;:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="ea3c5-266">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex2 StoreController index*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="ea3c5-267">Agregue los métodos **Browse** y **Details** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="ea3c5-268">Para ello, agregue el código siguiente al **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="ea3c5-269">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="ea3c5-270">Tarea 3: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="ea3c5-271">En esta tarea, probará la aplicación en un explorador Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="ea3c5-272">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-273">El proyecto se inicia en la página **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="ea3c5-274">Cambie la dirección URL para comprobar la implementación de cada acción.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="ea3c5-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-275">**/Store**.</span></span> <span data-ttu-id="ea3c5-276">Verá **&quot;Hola del&quot;Store. index ()** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="ea3c5-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-277">**/Store/Browse**.</span></span> <span data-ttu-id="ea3c5-278">Verá **&quot;Hola de Store. browse ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="ea3c5-279">**/Store/details**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-279">**/Store/Details**.</span></span> <span data-ttu-id="ea3c5-280">Verá **&quot;Hola en Store. Details ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="ea3c5-281">![Examinar StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Examinar StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="ea3c5-282">*Examinar/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="ea3c5-283">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="ea3c5-284">Ejercicio 3: pasar parámetros a un controlador</span><span class="sxs-lookup"><span data-stu-id="ea3c5-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="ea3c5-285">Hasta ahora, se han devuelto cadenas de constantes de los controladores.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="ea3c5-286">En este ejercicio aprenderá a pasar parámetros a un controlador mediante la dirección URL y la cadena de texto, y, a continuación, hará que las acciones del método respondan con texto al explorador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="ea3c5-287">Tarea 1: agregar el parámetro Genre a StoreController</span><span class="sxs-lookup"><span data-stu-id="ea3c5-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="ea3c5-288">En esta tarea, usará la **cadena** de consulta para enviar parámetros al método de acción de **exploración** en el **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="ea3c5-289">Si aún no está abierto, inicie **vs Express para web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="ea3c5-290">En el menú **archivo** , elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="ea3c5-291">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03-PassingParametersToAController\Begin**, seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="ea3c5-292">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="ea3c5-293">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ea3c5-294">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ea3c5-295">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ea3c5-296">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-297">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ea3c5-298">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ea3c5-299">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="ea3c5-300">Abra la clase **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-300">Open **StoreController** class.</span></span> <span data-ttu-id="ea3c5-301">Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="ea3c5-302">Cambie el método de **exploración** y agregue un parámetro de cadena para solicitar un género específico.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="ea3c5-303">ASP.NET MVC pasará automáticamente los parámetros QueryString o post de formulario denominados **Genre** a este método de acción cuando se invoque.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="ea3c5-304">Para ello, reemplace el método **Browse** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-305">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="ea3c5-306">Está usando el método de la utilidad **HttpUtility. HtmlEncode** para evitar que los usuarios inserten JavaScript en la vista con un vínculo como **/Store/Browse? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="ea3c5-307">Para obtener más información, visite [este artículo de MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="ea3c5-308">Tarea 2: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="ea3c5-309">En esta tarea, probará la aplicación en un explorador Web y usará el parámetro **Genre** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="ea3c5-310">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-311">El proyecto se inicia en la página **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="ea3c5-312">Cambie la dirección URL a */Store/Browse? Genre = disco* para comprobar que la acción recibe el parámetro Genre.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="ea3c5-313">![Exploración de StoreBrowseGenre = disco](aspnet-mvc-4-fundamentals/_static/image10.png "Exploración de StoreBrowseGenre = disco")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="ea3c5-314">*¿Examinar/Store/Browse? Género = disco*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="ea3c5-315">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="ea3c5-316">Tarea 3: agregar un parámetro de identificador incrustado en la dirección URL</span><span class="sxs-lookup"><span data-stu-id="ea3c5-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="ea3c5-317">En esta tarea, usará la **dirección URL** para pasar un parámetro de **identificador** al método de acción **Details** de **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="ea3c5-318">La Convención de enrutamiento predeterminada de MVC de ASP.NET consiste en tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado **ID**. Si el método de acción tiene un parámetro denominado ID, ASP.NET MVC pasará automáticamente el segmento de dirección URL como parámetro.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="ea3c5-319">En el almacén de direcciones URL **/detalles/5**, el **identificador** se interpretará como **5**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="ea3c5-320">Cambie el método de **detalles** de **StoreController**, agregando un parámetro **int** denominado **ID**. Para ello, reemplace el método **Details** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-321">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="ea3c5-322">Tarea 4: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="ea3c5-323">En esta tarea, probará la aplicación en un explorador Web y usará el parámetro **ID** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="ea3c5-324">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-325">El proyecto se inicia en la página **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="ea3c5-326">Cambie la dirección URL a */Store/details/5* para comprobar que la acción recibe el parámetro id.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="ea3c5-327">![Examinar StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Examinar StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="ea3c5-328">*Examinar/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="ea3c5-329">Ejercicio 4: crear una vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="ea3c5-330">Hasta ahora se han devuelto cadenas desde las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="ea3c5-331">Aunque es una manera útil de comprender cómo funcionan los controladores, no es cómo se compilan las aplicaciones Web reales.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="ea3c5-332">Las vistas son componentes que proporcionan un mejor enfoque para volver a generar HTML en el explorador con el uso de archivos de plantilla.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="ea3c5-333">En este ejercicio aprenderá a agregar una página maestra de diseño para configurar una plantilla para contenido HTML común, una hoja de estilos para mejorar la apariencia y el funcionamiento del sitio y, por último, una plantilla de vista para habilitar HomeController para que devuelva HTML.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="ea3c5-334">Tarea 1: modificación del archivo \_layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="ea3c5-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="ea3c5-335">El archivo **~/Views/Shared/\_layout. cshtml** permite configurar una plantilla para el HTML común que se usará en todo el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="ea3c5-336">En esta tarea, agregará una página maestra de diseño con un encabezado común con vínculos a la Página principal y al área de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="ea3c5-337">Si aún no está abierto, inicie **vs Express para web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="ea3c5-338">En el menú **archivo** , elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="ea3c5-339">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04-CreatingAView\Begin**, seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="ea3c5-340">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="ea3c5-341">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ea3c5-342">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ea3c5-343">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ea3c5-344">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-345">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ea3c5-346">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ea3c5-347">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="ea3c5-348">El archivo <strong>\_layout. cshtml</strong> contiene el diseño del contenedor HTML para todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="ea3c5-349">Incluye el&lt;elemento de <strong>&gt;HTML</strong> para la respuesta HTML, así como los elementos de <strong>&gt;de encabezado&lt;</strong> y <strong>&lt;cuerpo</strong> .&gt;</span><span class="sxs-lookup"><span data-stu-id="ea3c5-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="ea3c5-350"><strong>@RenderBody()</strong> en el cuerpo HTML identifican las regiones que las plantillas de vistas podrán rellenar con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="ea3c5-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="ea3c5-352">Agregue un encabezado común con vínculos a la Página principal y al área de almacenamiento en todas las páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="ea3c5-353">Para ello, agregue el siguiente código debajo de &lt;instrucción Body&gt;.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="ea3c5-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="ea3c5-355">Incluya un elemento div para representar la sección de cuerpo de cada página.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="ea3c5-356">Reemplace <strong>@RenderBody()</strong> por el siguiente código resaltadoC#: ()</span><span class="sxs-lookup"><span data-stu-id="ea3c5-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="ea3c5-357">¿Sabía que?</span><span class="sxs-lookup"><span data-stu-id="ea3c5-357">Did you know?</span></span> <span data-ttu-id="ea3c5-358">Visual Studio 2012 tiene fragmentos de código que facilitan la tarea de agregar código de uso común en HTML, archivos de código, etc.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="ea3c5-359">Para probarlo, escriba **&lt;div&gt;** y presione la **tecla TAB** dos veces para insertar una etiqueta **div** completa.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="ea3c5-360">Tarea 2: agregar una hoja de estilos CSS</span><span class="sxs-lookup"><span data-stu-id="ea3c5-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="ea3c5-361">La plantilla de proyecto vacía incluye un archivo CSS muy simplificado que solo incluye estilos que se usan para mostrar los mensajes de validación y formularios básicos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="ea3c5-362">Usará CSS y imágenes adicionales (potencialmente proporcionadas por un diseñador) para mejorar la apariencia y el funcionamiento del sitio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="ea3c5-363">En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="ea3c5-364">El archivo CSS y las imágenes se incluyen en la carpeta **Source\Assets\Content** de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="ea3c5-365">Para agregarlas a la aplicación, arrastre su contenido desde una ventana del **Explorador de Windows** hasta el **Explorador de soluciones** en Visual Studio Express para Web, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="ea3c5-366">![Arrastrar contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "Arrastrar contenido de estilo")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="ea3c5-367">*Arrastrar contenido de estilo*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="ea3c5-368">Aparecerá un cuadro de diálogo de advertencia en el que se le pide confirmación para reemplazar el archivo **site. CSS** y algunas imágenes existentes.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="ea3c5-369">Active **aplicar a todos los elementos** y haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="ea3c5-370">Tarea 3: agregar una plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="ea3c5-371">En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que utilizará la página maestra de diseño y la CSS agregada en este ejercicio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="ea3c5-372">Para usar una plantilla de vista al examinar la Página principal del sitio, primero debe indicar que, en lugar de devolver una cadena, el método de **Índice de HomeController** devolverá una **vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="ea3c5-373">Abra la clase **HomeController** y cambie su método de **Índice** para que devuelva un **ActionResult**y haga que devuelva la **vista ()** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="ea3c5-374">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="ea3c5-375">Ahora, debe agregar una plantilla de vista adecuada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="ea3c5-376">Para ello, **haga clic con el botón derecho en** el método de acción de **Índice** y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="ea3c5-377">Se abrirá el cuadro de diálogo **Agregar vista** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="ea3c5-378">![Agregar una vista desde el método index](aspnet-mvc-4-fundamentals/_static/image13.png "Agregar una vista desde el método index")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="ea3c5-379">*Agregar una vista desde el método index*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="ea3c5-380">Aparecerá el cuadro de diálogo **Agregar vista** para generar un archivo de plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="ea3c5-381">De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que la usará.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="ea3c5-382">Dado que usó el menú contextual **Agregar vista** en el método de acción de **Índice** dentro del HomeController, el cuadro de diálogo **Agregar vista** tiene el índice como nombre de vista predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="ea3c5-383">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-383">Click **Add**.</span></span>

    <span data-ttu-id="ea3c5-384">![Cuadro de diálogo Agregar vista](aspnet-mvc-4-fundamentals/_static/image14.png "Cuadro de diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="ea3c5-385">*Cuadro de diálogo Agregar vista*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="ea3c5-386">Visual Studio genera una plantilla de vista **index. cshtml** dentro de la carpeta **Views\Home** y, a continuación, la abre.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="ea3c5-387">![Vista de índice de inicio creada](aspnet-mvc-4-fundamentals/_static/image15.png "Vista de índice de inicio creada")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="ea3c5-388">*Vista de índice de inicio creada*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-389">el nombre y la ubicación del archivo **index. cshtml** es relevante y sigue las convenciones de nomenclatura predeterminadas de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="ea3c5-390">La carpeta \Views\**Home*\* coincide con el nombre del controlador (**Home** Controller).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="ea3c5-391">El nombre de la plantilla de vista (**Índice**) coincide con el método de acción del controlador que mostrará la vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="ea3c5-392">De este modo, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta Convención de nomenclatura para devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="ea3c5-393">La plantilla de vista generada se basa en la plantilla **\_layout. cshtml** que se ha definido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="ea3c5-394">Actualice la propiedad ViewBag. title a **Home**y cambie el contenido principal a **esta página principal**, como se muestra en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="ea3c5-395">Seleccione el proyecto **MvcMusicStore** en el explorador de soluciones y presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="ea3c5-396">Tarea 4: comprobación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-396">Task 4: Verification</span></span>

<span data-ttu-id="ea3c5-397">Para comprobar que ha realizado correctamente todos los pasos del ejercicio anterior, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="ea3c5-398">Con la aplicación abierta en un explorador, debe tener en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="ea3c5-399">El método de acción de índice de HomeController encontró y mostraba la plantilla de vista **\Views\Home\Index.cshtml** , aunque el código llamó a la **vista de devolución ()** , porque la plantilla de vista siguió la Convención de nomenclatura estándar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="ea3c5-400">En la Página principal se muestra el mensaje de bienvenida definido en la plantilla de vista **\Views\Home\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="ea3c5-401">La Página principal usa la plantilla **\_layout. cshtml** y, por tanto, el mensaje de bienvenida está incluido en el diseño HTML del sitio estándar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="ea3c5-402">![Vista de índice de inicio con el LayoutPage y estilo definidos](aspnet-mvc-4-fundamentals/_static/image16.png "Vista de índice de inicio con el LayoutPage y estilo definidos")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="ea3c5-403">*Vista de índice de inicio con el LayoutPage y estilo definidos*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="ea3c5-404">Ejercicio 5: crear un modelo de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="ea3c5-405">Hasta ahora, hizo que las vistas mostraran código HTML codificado, pero, para crear aplicaciones web dinámicas, la plantilla de vista debe recibir información del controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="ea3c5-406">Una técnica común que se usa para ese propósito es el patrón **ViewModel** , que permite a un controlador empaquetar toda la información necesaria para generar la respuesta HTML adecuada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="ea3c5-407">En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de esos géneros.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="ea3c5-408">También actualizará StoreController para usar el ViewModel creado y, por último, creará una nueva plantilla de vista que mostrará las propiedades mencionadas en la página.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="ea3c5-409">Tarea 1: creación de una clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="ea3c5-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="ea3c5-410">En esta tarea, creará una clase ViewModel que implementará el escenario de anuncio de género de tienda.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="ea3c5-411">Si aún no está abierto, inicie **vs Express para web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="ea3c5-412">En el menú **archivo** , elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="ea3c5-413">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05-CreatingAViewModel\Begin**, seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="ea3c5-414">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="ea3c5-415">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ea3c5-416">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ea3c5-417">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ea3c5-418">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-419">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ea3c5-420">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ea3c5-421">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="ea3c5-422">Cree una carpeta **ViewModels** para contener el ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="ea3c5-423">Para ello, haga clic con el botón derecho en el proyecto **MvcMusicStore** de nivel superior, seleccione **Agregar** y, a continuación, **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="ea3c5-424">![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "Agregar una nueva carpeta")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="ea3c5-425">*Agregar una nueva carpeta*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="ea3c5-426">Asigne a la carpeta el nombre *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="ea3c5-427">![Carpeta ViewModels en Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "Carpeta ViewModels en Explorador de soluciones")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="ea3c5-428">*Carpeta ViewModels en Explorador de soluciones*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="ea3c5-429">Cree una clase **ViewModel** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="ea3c5-430">Para ello, haga clic con el botón derecho en la carpeta **ViewModels** que ha creado recientemente, seleccione **Agregar** y, a continuación, **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="ea3c5-431">En **código**, elija el elemento de **clase** y asigne al archivo el nombre *StoreIndexViewModel.CS*y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="ea3c5-432">![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "Agregar una nueva clase")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="ea3c5-433">*Agregar una nueva clase*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-433">*Adding a new Class*</span></span>

    <span data-ttu-id="ea3c5-434">![Crear la clase StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Crear la clase StoreIndexViewModel")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="ea3c5-435">*Crear la clase StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="ea3c5-436">Tarea 2: agregar propiedades a la clase ViewModel</span><span class="sxs-lookup"><span data-stu-id="ea3c5-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="ea3c5-437">Hay dos parámetros que se pasarán desde StoreController a la plantilla de vista para generar la respuesta HTML esperada: el número de géneros en el almacén y una lista de esos géneros.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="ea3c5-438">En esta tarea, agregará esas dos propiedades a la clase **StoreIndexViewModel** : **NumberOfGenres** (un entero) y **géneros** (una lista de cadenas).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="ea3c5-439">Agregue las propiedades **NumberOfGenres** y **Genre** a la clase **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="ea3c5-440">Para ello, agregue las dos líneas siguientes a la definición de clase:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="ea3c5-441">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel Properties*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="ea3c5-442">La notación **{Get; Set;}** hace uso de C#la característica de propiedades implementadas automáticamente de.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="ea3c5-443">Proporciona las ventajas de una propiedad sin necesidad de declarar un campo de respaldo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="ea3c5-444">Tarea 3: actualización de StoreController para usar StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="ea3c5-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="ea3c5-445">La clase **StoreIndexViewModel** encapsula la información necesaria para pasar del método **index** de **StoreController**a una plantilla de vista con el fin de generar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="ea3c5-446">En esta tarea, actualizará el **StoreController** para usar **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="ea3c5-447">Abra la clase **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="ea3c5-448">![Apertura de la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Apertura de la clase StoreController")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="ea3c5-449">*Apertura de la clase StoreController*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="ea3c5-450">Para usar la clase **StoreIndexViewModel** de **StoreController**, agregue el siguiente espacio de nombres en la parte superior del código **StoreController** :</span><span class="sxs-lookup"><span data-stu-id="ea3c5-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="ea3c5-451">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel con ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="ea3c5-452">Cambie el método de acción de **Índice** de **StoreController**para que cree y rellene un objeto **StoreIndexViewModel** y, a continuación, lo pase a una plantilla de vista para generar una respuesta HTML con él.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-453">En Lab &quot;los modelos y el acceso a datos de ASP.NET MVC&quot; escribirá código que recupera la lista de géneros de almacén de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="ea3c5-454">En el código siguiente, creará una **lista** de géneros de datos ficticios que rellenará el **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="ea3c5-455">Después de crear y configurar el objeto **StoreIndexViewModel** , se pasará como argumento al método **View** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="ea3c5-456">Esto indica que la plantilla de vista utilizará ese objeto para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="ea3c5-457">Reemplace el método de **Índice** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-458">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreController index Method*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="ea3c5-459">Si no está familiarizado con C#, puede suponer que el uso de **var** significa que la variable **viewModel** está enlazada en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="ea3c5-460">Esto no es correcto: el C# compilador usa la inferencia de tipos basada en lo que se asigna a la variable para determinar que **viewModel** es de tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="ea3c5-461">Además, al compilar la variable de **viewModel** local como un tipo **StoreIndexViewModel** , obtiene la comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="ea3c5-462">Tarea 4: creación de una plantilla de vista que usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="ea3c5-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="ea3c5-463">En esta tarea, creará una plantilla de vista que usará un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="ea3c5-464">Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que el **cuadro de diálogo Agregar vista** Conozca la clase **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="ea3c5-465">Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="ea3c5-466">![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilar el proyecto")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="ea3c5-467">*Compilar el proyecto*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-467">*Building the project*</span></span>
2. <span data-ttu-id="ea3c5-468">Cree una nueva plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-468">Create a new View template.</span></span> <span data-ttu-id="ea3c5-469">Para ello, haga clic con el botón derecho en el método de **Índice** y seleccione **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="ea3c5-470">![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "Agregar una vista")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="ea3c5-471">*Agregar una vista*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-471">*Adding a View*</span></span>
3. <span data-ttu-id="ea3c5-472">Dado que el **cuadro de diálogo Agregar vista** se invocó desde **StoreController**, agregará la plantilla de vista de forma predeterminada en un archivo **\Views\Store\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="ea3c5-473">Active la casilla **crear una vista fuertemente tipada** y, a continuación, seleccione **StoreIndexViewModel** como **clase de modelo**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="ea3c5-474">Además, asegúrese de que el motor de vista seleccionado sea **Razor**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="ea3c5-475">Haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-475">Click **Add**.</span></span>

    <span data-ttu-id="ea3c5-476">![Cuadro de diálogo Agregar vista](aspnet-mvc-4-fundamentals/_static/image24.png "Cuadro de diálogo Agregar vista")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="ea3c5-477">*Cuadro de diálogo Agregar vista*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-477">*Add View Dialog*</span></span>

    <span data-ttu-id="ea3c5-478">El archivo de plantilla de vista **\Views\Store\Index.cshtml** se crea y se abre.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="ea3c5-479">En función de la información que se proporciona al cuadro de diálogo **Agregar vista** en el último paso, la plantilla de vista esperará una instancia de **StoreIndexViewModel** como los datos que se van a usar para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="ea3c5-480">Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="ea3c5-481">Tarea 5: actualización de la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="ea3c5-482">En esta tarea, actualizará la plantilla de vista creada en la última tarea para recuperar el número de géneros y sus nombres dentro de la página.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="ea3c5-483">Usará @ Syntax (a menudo conocido como &quot;códigos de código&quot;) para ejecutar código dentro de la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="ea3c5-484">En el archivo **index. cshtml** , dentro de la carpeta **Store** , reemplace su código por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="ea3c5-485">Recorra en iteración la lista género de **StoreIndexViewModel** y cree una lista HTML **&lt;UL&gt;** mediante un bucle **foreach** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="ea3c5-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="ea3c5-487">Presione **F5** para ejecutar la aplicación y examinar **/Store**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="ea3c5-488">Verá la lista de géneros pasados en el objeto **StoreIndexViewModel** de **StoreController** a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="ea3c5-489">![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "Vista que muestra una lista de géneros")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="ea3c5-490">*Vista que muestra una lista de géneros*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="ea3c5-491">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="ea3c5-492">Ejercicio 6: uso de parámetros en la vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="ea3c5-493">En el ejercicio 3 aprendió a pasar parámetros al controlador.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="ea3c5-494">En este ejercicio, obtendrá información sobre cómo usar esos parámetros en la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="ea3c5-495">Para ello, se introducirá primero en las clases de modelo que le ayudarán a administrar los datos y la lógica del dominio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="ea3c5-496">Además, aprenderá a crear vínculos a páginas dentro de la aplicación ASP.NET MVC sin preocuparse por cosas como la codificación de rutas de acceso URL.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="ea3c5-497">Tarea 1: agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="ea3c5-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="ea3c5-498">A diferencia de ViewModels, que se crean solo para pasar información del controlador a la vista, las clases de modelo se compilan para contener y administrar la lógica de los datos y del dominio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="ea3c5-499">En esta tarea, agregará dos clases de modelo para representar estos conceptos: **Genre** y **album**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="ea3c5-500">Si aún no está abierto, inicie **vs Express para web**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="ea3c5-501">En el menú **archivo** , elija **Abrir proyecto**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="ea3c5-502">En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06-UsingParametersInView\Begin**, seleccione **Begin. sln** y haga clic en **abrir**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="ea3c5-503">Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="ea3c5-504">Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ea3c5-505">Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ea3c5-506">En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ea3c5-507">Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ea3c5-508">Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ea3c5-509">Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ea3c5-510">Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="ea3c5-511">Agregue una clase de modelo **Genre** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="ea3c5-512">Para ello, haga clic con el botón secundario en la carpeta **modelos** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="ea3c5-513">En **código**, elija el elemento de **clase** y asigne al archivo el nombre *Genre.CS*y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="ea3c5-514">![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "Agregar una clase")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="ea3c5-515">*Agregar un nuevo elemento*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-515">*Adding a new item*</span></span>

    <span data-ttu-id="ea3c5-516">![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="ea3c5-517">*Agregar clase de modelo de género*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="ea3c5-518">Agregue una propiedad **Name** a la clase Genre.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="ea3c5-519">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-519">To do this, add the following code:</span></span>

    <span data-ttu-id="ea3c5-520">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 Genre*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="ea3c5-521">Siguiendo el mismo procedimiento que antes, agregue una clase **album** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="ea3c5-522">Para ello, haga clic con el botón secundario en la carpeta **modelos** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="ea3c5-523">En **código**, elija el elemento de **clase** y asigne al archivo el nombre *Album.CS*y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="ea3c5-524">Agregue dos propiedades a la clase Album: **género** y **título**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="ea3c5-525">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-525">To do this, add the following code:</span></span>

    <span data-ttu-id="ea3c5-526">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 album*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="ea3c5-527">Tarea 2: adición de un StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="ea3c5-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="ea3c5-528">Se usará una **StoreBrowseViewModel** en esta tarea para mostrar los álbumes que coinciden con un género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="ea3c5-529">En esta tarea, creará esta clase y, a continuación, agregará dos propiedades para controlar el **género** y la lista de su **álbum**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="ea3c5-530">Agregue una clase **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="ea3c5-531">Para ello, haga clic con el botón secundario en la carpeta **ViewModels** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="ea3c5-532">En **código**, elija el elemento de **clase** y asigne al archivo el nombre *StoreBrowseViewModel.CS*y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="ea3c5-533">Agregue una referencia a los modelos de la clase **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="ea3c5-534">Para ello, agregue lo siguiente mediante el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="ea3c5-535">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="ea3c5-536">Agregue dos propiedades a la clase **StoreBrowseViewModel** : **género** y **álbumes**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="ea3c5-537">Para ello, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-537">To do this, add the following code:</span></span>

    <span data-ttu-id="ea3c5-538">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="ea3c5-539">¿Qué es la **lista&lt;álbum&gt;** ?: esta definición usa el tipo de **&gt;de lista&lt;t** , donde **t** restringe el tipo al que pertenecen los elementos de esta **lista** , en este caso **album** (o cualquiera de sus descendientes).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="ea3c5-540">Esta capacidad de diseñar clases y métodos que aplazan la especificación de uno o más tipos hasta que se declara la clase o el método y el código de cliente crea una instancia C# del mismo, una característica del lenguaje denominada **genéricos**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="ea3c5-541">**List&lt;t&gt;** es el equivalente genérico del tipo **ArrayList** y está disponible en el espacio de nombres **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="ea3c5-542">Una de las ventajas de usar **genéricos** es que, puesto que se especifica el tipo, no es necesario que se ocupe de las operaciones de comprobación de tipos, como convertir los elementos en un **álbum** como haría con una **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="ea3c5-543">Tarea 3: uso del nuevo ViewModel en el StoreController</span><span class="sxs-lookup"><span data-stu-id="ea3c5-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="ea3c5-544">En esta tarea, modificará los métodos de acción **Browse** y **Details** de **StoreController**para usar el nuevo **StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="ea3c5-545">Agregue una referencia a la carpeta models en la clase **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="ea3c5-546">Para ello, expanda la carpeta **Controllers** en el **Explorador de soluciones** y abra la clase **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="ea3c5-547">A continuación, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-547">Then add the following code:</span></span>

    <span data-ttu-id="ea3c5-548">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="ea3c5-549">Reemplace el método de acción **Browse** para usar la clase **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="ea3c5-550">Creará un género y dos nuevos objetos álbumes con datos ficticios (en el siguiente laboratorio práctico usará datos reales de una base de datos).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="ea3c5-551">Para ello, reemplace el método **Browse** por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-552">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="ea3c5-553">Reemplace el método de acción **Details** para usar la clase **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="ea3c5-554">Creará un nuevo objeto de **álbum** que se va a devolver a la **vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="ea3c5-555">Para ello, reemplace el método **Details** con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="ea3c5-556">(Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="ea3c5-557">Tarea 4: agregar una plantilla de vista de exploración</span><span class="sxs-lookup"><span data-stu-id="ea3c5-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="ea3c5-558">En esta tarea, agregará una vista **examinar** para mostrar los álbumes encontrados para un género específico.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="ea3c5-559">Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el cuadro de diálogo **Agregar vista** Conozca la clase **ViewModel** que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="ea3c5-560">Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="ea3c5-561">Agregue una vista **examinar** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-561">Add a **Browse** View.</span></span> <span data-ttu-id="ea3c5-562">Para ello, haga clic con el botón secundario en el método de acción **Browse** de **StoreController** y haga clic en **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="ea3c5-563">En el cuadro de diálogo **Agregar vista** , compruebe que el nombre de la vista es **examinar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="ea3c5-564">Active la casilla **crear una vista fuertemente tipada** y seleccione **StoreBrowseViewModel** en la lista desplegable **clase de modelo** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="ea3c5-565">Deje los demás campos con su valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="ea3c5-566">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-566">Then click **Add**.</span></span>

    <span data-ttu-id="ea3c5-567">![Agregar una vista examinar](aspnet-mvc-4-fundamentals/_static/image29.png "Agregar una vista examinar")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="ea3c5-568">*Agregar una vista examinar*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="ea3c5-569">Modifique el objeto **Browse. cshtml** para mostrar la información del género, teniendo acceso al objeto **StoreBrowseViewModel** que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="ea3c5-570">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ea3c5-571">Tarea 5: ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="ea3c5-572">En esta tarea, probará que el método **Browse** recupera álbumes de la acción **Browse** Method.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="ea3c5-573">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-574">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-574">The project starts in the Home page.</span></span> <span data-ttu-id="ea3c5-575">Cambie la dirección URL a **/Store/Browse? Genre = disco** para comprobar que la acción devuelve dos álbumes.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="ea3c5-576">![Examinar álbumes en disco de almacén](aspnet-mvc-4-fundamentals/_static/image30.png "Examinar álbumes en disco de almacén")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="ea3c5-577">*Examinar álbumes en disco de almacén*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="ea3c5-578">Tarea 6: Mostrar información acerca de un álbum específico</span><span class="sxs-lookup"><span data-stu-id="ea3c5-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="ea3c5-579">En esta tarea, implementará la vista de **almacén y detalles** para mostrar información sobre un álbum específico.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="ea3c5-580">En este laboratorio práctico, todo lo que se muestra sobre el álbum ya está incluido en la plantilla de **vista** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="ea3c5-581">Por lo tanto, en lugar de crear una clase **StoreDetailsViewModel** , utilizará la plantilla **StoreBrowseViewModel** actual pasándole el álbum.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="ea3c5-582">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ea3c5-583">Agregue una nueva vista de **detalles** para el método de acción de **detalles** de **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="ea3c5-584">Para ello, haga clic con el botón secundario en el método **Details** de la clase **StoreController** y haga clic en **Agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="ea3c5-585">En el cuadro de diálogo **Agregar vista** , compruebe que el nombre de la **vista** sea **detalles**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="ea3c5-586">Active la casilla **crear una vista fuertemente tipada** y seleccione **álbum** en la lista desplegable **clase de modelo** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="ea3c5-587">Deje los demás campos con su valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="ea3c5-588">A continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-588">Then click **Add**.</span></span> <span data-ttu-id="ea3c5-589">Se creará y se abrirá un archivo **\Views\Store\Details.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="ea3c5-590">![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "Agregar una vista de detalles")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="ea3c5-591">*Agregar una vista de detalles*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="ea3c5-592">Modifique el archivo **details. cshtml** para mostrar la información del álbum, teniendo acceso al objeto de **álbum** que se pasa a la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="ea3c5-593">Para ello, reemplace el contenido por lo siguiente: (C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="ea3c5-594">Tarea 7: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="ea3c5-595">En esta tarea, probará que la vista de **detalles** recupera la información del álbum del método de **acción detalles** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="ea3c5-596">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-597">El proyecto se inicia en la página **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="ea3c5-598">Cambie la dirección URL a **/Store/details/5** para comprobar la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="ea3c5-599">![Detalles de la exploración de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "Detalles de la exploración de álbumes")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="ea3c5-600">*Examinar los detalles del álbum*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="ea3c5-601">Tarea 8: agregar vínculos entre páginas</span><span class="sxs-lookup"><span data-stu-id="ea3c5-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="ea3c5-602">En esta tarea, agregará un vínculo en la vista de almacén para tener un vínculo en cada nombre de género a la dirección URL de **/Store/Browse** adecuada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="ea3c5-603">De este modo, al hacer clic en un género, por **ejemplo,** se desplazará a **/Store/Browse? Genre =** dirección URL de disco.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="ea3c5-604">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ea3c5-605">Actualice la página de **Índice** para agregar un vínculo a la página de **exploración** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="ea3c5-606">Para ello, en el **Explorador de soluciones** expanda la carpeta **views** , la carpeta **Store** y haga doble clic en la página **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="ea3c5-607">Agregue un vínculo a la vista examinar que indica el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="ea3c5-608">Para ello, reemplace el siguiente código resaltado dentro de las etiquetas **&lt;li&gt;** : (C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="ea3c5-609">otro enfoque sería vincular directamente a la página, con un código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="ea3c5-610">&lt;a href =&quot;/Store/Browse? Genre =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="ea3c5-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="ea3c5-611">Aunque este enfoque funciona, depende de una cadena codificada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="ea3c5-612">Si posteriormente cambia el nombre del controlador, deberá cambiar esta instrucción manualmente.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="ea3c5-613">Una alternativa mejor es usar un método **auxiliar HTML** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="ea3c5-614">ASP.NET MVC incluye un método auxiliar HTML que está disponible para estas tareas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="ea3c5-615">El método auxiliar **HTML. ActionLink ()** facilita la compilación de HTML **&lt;un&gt;** vínculos, asegurándose de que las rutas de acceso de las direcciones URL están codificadas correctamente como dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="ea3c5-616">HTML. ActionLink tiene varias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="ea3c5-617">En este ejercicio usará una que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="ea3c5-618">Texto del vínculo, que mostrará el nombre del género</span><span class="sxs-lookup"><span data-stu-id="ea3c5-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="ea3c5-619">Nombre de acción del controlador (**examinar**)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="ea3c5-620">Valores de parámetro de la ruta, que especifican el nombre (**género**) y el valor (**nombre de género**)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="ea3c5-621">Tarea 9: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="ea3c5-622">En esta tarea, probará que cada género se muestra con un vínculo a la dirección URL de **/Store/Browse** adecuada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="ea3c5-623">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-624">El proyecto se inicia en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-624">The project starts in the Home page.</span></span> <span data-ttu-id="ea3c5-625">Cambie la dirección URL a **/Store** para comprobar que cada género se vincula a la dirección URL de **/Store/Browse** adecuada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="ea3c5-626">![Examinar géneros con vínculos a la página examinar](aspnet-mvc-4-fundamentals/_static/image33.png "Examinar géneros con vínculos a la página examinar")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="ea3c5-627">*Examinar géneros con vínculos a la página examinar*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="ea3c5-628">Tarea 10: usar la colección ViewModel dinámica para pasar valores</span><span class="sxs-lookup"><span data-stu-id="ea3c5-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="ea3c5-629">En esta tarea, aprenderá un método sencillo y eficaz para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="ea3c5-630">ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se puede asignar a cualquier valor dinámico y tener acceso también dentro de los controladores y las vistas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="ea3c5-631">Ahora usará la colección dinámica ViewBag para pasar una lista de &quot;**géneros destacan**&quot; desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="ea3c5-632">La vista de índice de almacén tendrá acceso a **ViewModel** y mostrará la información.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="ea3c5-633">Cierre el explorador si es necesario para volver a la ventana de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="ea3c5-634">Abra **StoreController.CS** y modifique el método **index** para crear una lista de géneros destacan en la colección ViewModel:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="ea3c5-635">También puede usar la sintaxis **ViewBag [&quot;destacan&quot;]** para tener acceso a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="ea3c5-636">El icono de estrella **&quot;&quot;destacan. png** se incluye en la carpeta **Source\Assets\Images** de este laboratorio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="ea3c5-637">Para agregarlo a la aplicación, arrastre su contenido desde una ventana del **Explorador de Windows** hasta el **Explorador de soluciones** en Visual Web Developer Express, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="ea3c5-638">![Agregar una imagen de estrella a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "Agregar una imagen de estrella a la solución")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="ea3c5-639">*Agregar una imagen de estrella a la solución*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="ea3c5-640">Abra la vista **Store/index. cshtml** y modifique el contenido.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="ea3c5-641">Leerá la propiedad &quot;destacan&quot; de la colección **ViewBag** y le preguntará si el nombre de género actual está en la lista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="ea3c5-642">En ese caso, mostrará un icono de estrella directamente al vínculo género.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="ea3c5-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="ea3c5-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="ea3c5-644">Tarea 11: ejecución de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ea3c5-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="ea3c5-645">En esta tarea, probará que el género destacan muestra un icono de estrella.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="ea3c5-646">Presione **F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ea3c5-647">El proyecto se inicia en la página **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="ea3c5-648">Cambie la dirección URL a **/Store** para comprobar que los géneros destacados tienen la etiqueta de respeto:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="ea3c5-649">![Examinar géneros con elementos destacan](aspnet-mvc-4-fundamentals/_static/image35.png "Examinar géneros con elementos destacan")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="ea3c5-650">*Examinar géneros con elementos destacan*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="ea3c5-651">Ejercicio 7: una nueva plantilla de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea3c5-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="ea3c5-652">En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4 y examinará las características más relevantes de la nueva plantilla.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="ea3c5-653">Tarea 1: exploración de la plantilla de aplicación de Internet de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea3c5-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="ea3c5-654">Si aún no está abierto, inicie **vs Express para web**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="ea3c5-655">Seleccione el **archivo | Nuevo |** Comando de menú proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="ea3c5-656">En el cuadro de diálogo **nuevo proyecto** , seleccione el **Visual C#| Plantilla Web** en el árbol del panel izquierdo y elija la **aplicación web MVC 4 ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ea3c5-657">**Asigne** al proyecto el nombre *MusicStore* y actualice el nombre de la **solución** a *Inicio*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="ea3c5-658">![Creación de un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Creación de un nuevo proyecto de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="ea3c5-659">*Creación de un nuevo proyecto de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="ea3c5-660">En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla de proyecto **aplicación de Internet** y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="ea3c5-661">Tenga en cuenta que puede seleccionar Razor o ASPX como motor de vista.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="ea3c5-662">![Creación de una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Creación de una nueva aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="ea3c5-663">*Creación de una nueva aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-664">Sintaxis Razor se ha introducido en ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="ea3c5-665">Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarios en un archivo, lo que permite un flujo de trabajo de codificación rápida y fluida.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="ea3c5-666">Razor aprovecha los conocimientos C#existentes del lenguaje/VB (u otros) y proporciona una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="ea3c5-667">Presione **F5** para ejecutar la solución y ver la plantilla renovada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="ea3c5-668">Puede consultar las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="ea3c5-669">**Plantillas de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-669">**Modern-style templates**</span></span>

        <span data-ttu-id="ea3c5-670">Las plantillas se han renovado, lo que proporciona más estilos de aspecto moderno.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="ea3c5-671">![Plantillas con estilo de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Plantillas con estilo de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="ea3c5-672">*Plantillas con estilo de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="ea3c5-673">**Representación adaptable**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="ea3c5-674">Desproteja el cambio de tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al nuevo tamaño de la ventana.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="ea3c5-675">Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="ea3c5-676">![Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador](aspnet-mvc-4-fundamentals/_static/image39.png "Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="ea3c5-677">*Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="ea3c5-678">Cierre el explorador para detener el depurador y volver a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="ea3c5-679">Ahora puede explorar la solución y consultar algunas de las nuevas características introducidas por ASP.NET MVC 4 en la plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="ea3c5-680">![ASP.NET MVC4-Internet-Application-Project-Template](aspnet-mvc-4-fundamentals/_static/image40.png "La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="ea3c5-681">*La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="ea3c5-682">**Marcado HTML5**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-682">**HTML5 markup**</span></span>

       <span data-ttu-id="ea3c5-683">Examine las vistas de plantilla para averiguar el nuevo marcado del tema; por ejemplo, abra la vista **About. cshtml** dentro de la carpeta **principal** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="ea3c5-684">![Nueva plantilla, con Razor y marcado HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nueva plantilla, con Razor y marcado HTML5")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="ea3c5-685">*Nueva plantilla, con Razor y marcado HTML5*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="ea3c5-686">**Bibliotecas de JavaScript incluidas**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="ea3c5-687">**jQuery**: jQuery simplifica el recorrido de documentos HTML, el control de eventos, la animación y las interacciones de Ajax.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="ea3c5-688">**jQuery UI**: esta biblioteca proporciona abstracciones para la interacción y animación de bajo nivel, efectos avanzados y widgets temáticos, que se basan en la biblioteca de JavaScript de jQuery.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="ea3c5-689">Puede obtener información sobre jQuery y jQuery UI en [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="ea3c5-690">**KnockoutJS**: la plantilla predeterminada de ASP.NET MVC 4 ahora incluye **KnockoutJS**, un marco de trabajo de JavaScript MVVM que le permite crear aplicaciones web enriquecidas y con una gran capacidad de respuesta mediante JavaScript y HTML.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="ea3c5-691">Al igual que en ASP.NET MVC 3, las bibliotecas de interfaz de usuario de jQuery y jQuery también se incluyen en ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="ea3c5-692">Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="ea3c5-693">**Modernizr**: esta biblioteca se ejecuta automáticamente, lo que hace que el sitio sea compatible con los exploradores más antiguos cuando se usan las tecnologías HTML5 y CSS3.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="ea3c5-694">Puede obtener más información acerca de la biblioteca Modernizr en este vínculo: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="ea3c5-695">**SimpleMembership incluido en la solución**</span><span class="sxs-lookup"><span data-stu-id="ea3c5-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="ea3c5-696">SimpleMembership se ha diseñado como un sustituto para el rol de ASP.NET anterior y el sistema de proveedor de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="ea3c5-697">Tiene muchas características nuevas que facilitan a los desarrolladores la protección de las páginas web de una manera más flexible.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="ea3c5-698">La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta OAuth, el inicio de sesión, la administración, etc.) y la seguridad Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="ea3c5-699">![SimpleMembership incluido en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluido en la solución")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="ea3c5-700">*SimpleMembership incluido en la solución*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="ea3c5-701">Obtenga más información sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="ea3c5-702">Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ea3c5-703">Resumen</span><span class="sxs-lookup"><span data-stu-id="ea3c5-703">Summary</span></span>

<span data-ttu-id="ea3c5-704">Al completar este laboratorio práctico, ha aprendido los aspectos básicos de ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="ea3c5-705">Los elementos básicos de una aplicación MVC y cómo interactúan</span><span class="sxs-lookup"><span data-stu-id="ea3c5-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="ea3c5-706">Cómo crear una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ea3c5-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="ea3c5-707">Cómo agregar y configurar Controladores para controlar los parámetros que se pasan a través de la dirección URL y QueryString</span><span class="sxs-lookup"><span data-stu-id="ea3c5-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="ea3c5-708">Cómo agregar una página maestra de diseño para configurar una plantilla para contenido HTML común, una hoja de estilos para mejorar la apariencia y el funcionamiento y una plantilla de vista para mostrar contenido HTML</span><span class="sxs-lookup"><span data-stu-id="ea3c5-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="ea3c5-709">Cómo usar el patrón ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica</span><span class="sxs-lookup"><span data-stu-id="ea3c5-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="ea3c5-710">Cómo usar los parámetros pasados a los controladores en la plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="ea3c5-711">Cómo agregar vínculos a páginas dentro de la aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ea3c5-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="ea3c5-712">Cómo agregar y usar propiedades dinámicas en una vista</span><span class="sxs-lookup"><span data-stu-id="ea3c5-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="ea3c5-713">Mejoras en las plantillas de proyecto de ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea3c5-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="ea3c5-714">Apéndice A: instalación de Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="ea3c5-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="ea3c5-715">Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="ea3c5-716">Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="ea3c5-717">Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="ea3c5-718">Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="ea3c5-719">Haga clic en **instalar ahora**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-719">Click on **Install Now**.</span></span> <span data-ttu-id="ea3c5-720">Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="ea3c5-721">Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="ea3c5-722">![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="ea3c5-723">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="ea3c5-724">Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceptación de los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="ea3c5-726">*Aceptación de los términos de licencia*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="ea3c5-727">Espere hasta que se complete el proceso de descarga e instalación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-727">Wait until the downloading and installation process completes.</span></span>

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="ea3c5-729">*Progreso de la instalación*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-729">*Installation progress*</span></span>
6. <span data-ttu-id="ea3c5-730">Cuando se complete la instalación, haga clic en **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-730">When the installation completes, click **Finish**.</span></span>

    ![Instalación completada](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="ea3c5-732">*Instalación completada*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-732">*Installation completed*</span></span>
7. <span data-ttu-id="ea3c5-733">Haga clic en **salir** para cerrar el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="ea3c5-734">Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para Web icono](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="ea3c5-736">*VS Express para Web icono*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="ea3c5-737">Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ea3c5-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="ea3c5-738">Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="ea3c5-739">Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure</span><span class="sxs-lookup"><span data-stu-id="ea3c5-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="ea3c5-740">Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-741">Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="ea3c5-742">Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="ea3c5-743">![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Iniciar sesión en Windows Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="ea3c5-744">*Iniciar sesión en Windows Azure Portal de administración*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="ea3c5-745">Haga clic en **nuevo** en la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="ea3c5-746">![Crear un nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image49.png "Crear un nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="ea3c5-747">*Crear un nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="ea3c5-748">Haga clic en **compute** | **sitio web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="ea3c5-749">A continuación, seleccione la opción **creación rápida** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="ea3c5-750">Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-751">Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="ea3c5-752">La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="ea3c5-753">No incluye los pasos para configurar una base de datos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="ea3c5-754">![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "Crear un nuevo sitio web mediante creación rápida")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="ea3c5-755">*Crear un nuevo sitio web mediante creación rápida*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="ea3c5-756">Espere hasta que se cree el nuevo **sitio web** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="ea3c5-757">Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="ea3c5-758">Compruebe que el nuevo sitio web funciona.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="ea3c5-759">![Ir al nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image51.png "Ir al nuevo sitio web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="ea3c5-760">*Ir al nuevo sitio web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="ea3c5-761">![Sitio web en ejecución](aspnet-mvc-4-fundamentals/_static/image52.png "Sitio web en ejecución")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="ea3c5-762">*Sitio web en ejecución*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-762">*Web site running*</span></span>
6. <span data-ttu-id="ea3c5-763">Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="ea3c5-764">![Abrir las páginas de administración de sitios web](aspnet-mvc-4-fundamentals/_static/image53.png "Abrir las páginas de administración de sitios web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="ea3c5-765">*Abrir las páginas de administración de sitios web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="ea3c5-766">En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-767">El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="ea3c5-768">El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="ea3c5-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="ea3c5-770">![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-fundamentals/_static/image54.png "Descargar el perfil de publicación del sitio web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="ea3c5-771">*Descargar el perfil de publicación del sitio web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="ea3c5-772">Descargue el archivo de Perfil de publicación en una ubicación conocida.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="ea3c5-773">Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="ea3c5-774">![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "Guardar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="ea3c5-775">*Guardar el archivo de Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="ea3c5-776">Tarea 2: configurar el servidor de base de datos</span><span class="sxs-lookup"><span data-stu-id="ea3c5-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="ea3c5-777">Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="ea3c5-778">Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="ea3c5-779">Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="ea3c5-780">Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="ea3c5-781">Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="ea3c5-782">Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="ea3c5-783">No cree todavía la base de datos, ya que se creará en una etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="ea3c5-784">![Panel de SQL Database Server](aspnet-mvc-4-fundamentals/_static/image56.png "Panel de SQL Database Server")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="ea3c5-785">*Panel de SQL Database Server*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="ea3c5-786">En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="ea3c5-787">Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-fundamentals/_static/image57.png).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="ea3c5-789">*Agregando la dirección IP del cliente*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="ea3c5-790">Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="ea3c5-792">*Confirmar cambios*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="ea3c5-793">Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ea3c5-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="ea3c5-794">Vuelva a la solución ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="ea3c5-795">En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="ea3c5-796">![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "Publicar la aplicación")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="ea3c5-797">*Publicar el sitio web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="ea3c5-798">Importe el perfil de publicación que guardó en la primera tarea.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="ea3c5-799">![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "Importar el perfil de publicación")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="ea3c5-800">*Importando Perfil de publicación*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="ea3c5-801">Haga clic en **validar conexión**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-801">Click **Validate Connection**.</span></span> <span data-ttu-id="ea3c5-802">Una vez finalizada la validación, haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ea3c5-803">La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="ea3c5-804">![Validando conexión](aspnet-mvc-4-fundamentals/_static/image62.png "Validando conexión")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="ea3c5-805">*Validando conexión*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-805">*Validating connection*</span></span>
4. <span data-ttu-id="ea3c5-806">En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="ea3c5-807">![Configuración de web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Configuración de web deploy")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="ea3c5-808">*Configuración de web deploy*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="ea3c5-809">Configure la conexión de base de datos de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="ea3c5-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="ea3c5-810">En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="ea3c5-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="ea3c5-811">En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="ea3c5-812">En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="ea3c5-813">Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="ea3c5-814">![Configuración de la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configuración de la cadena de conexión de destino")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="ea3c5-815">*Configuración de la cadena de conexión de destino*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="ea3c5-816">A continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-816">Then click **OK**.</span></span> <span data-ttu-id="ea3c5-817">Cuando se le pida que cree la base de datos, haga clic en **sí**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="ea3c5-818">![Crear la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "Crear la cadena de base de datos")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="ea3c5-819">*Crear la base de datos*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-819">*Creating the database*</span></span>
7. <span data-ttu-id="ea3c5-820">La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="ea3c5-821">Después, haga clic en **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-821">Then click **Next**.</span></span>

    <span data-ttu-id="ea3c5-822">![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Cadena de conexión que apunta a SQL Database")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="ea3c5-823">*Cadena de conexión que apunta a SQL Database*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="ea3c5-824">En la página **vista previa** , haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="ea3c5-825">![Publicación de la aplicación Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publicación de la aplicación Web")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="ea3c5-826">*Publicación de la aplicación Web*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="ea3c5-827">Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="ea3c5-828">![Aplicación publicada en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplicación publicada en Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="ea3c5-829">*Aplicación publicada en Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="ea3c5-830">Apéndice C: usar fragmentos de código</span><span class="sxs-lookup"><span data-stu-id="ea3c5-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="ea3c5-831">Con los fragmentos de código, tiene todo el código que necesita a su alcance.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="ea3c5-832">El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="ea3c5-833">![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="ea3c5-834">*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="ea3c5-835">***Para agregar un fragmento de código mediante el tecladoC# (solo)***</span><span class="sxs-lookup"><span data-stu-id="ea3c5-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="ea3c5-836">Coloque el cursor donde desea insertar el código.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="ea3c5-837">Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="ea3c5-838">Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="ea3c5-839">Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).</span><span class="sxs-lookup"><span data-stu-id="ea3c5-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="ea3c5-840">Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="ea3c5-841">![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-fundamentals/_static/image70.png "Comience a escribir el nombre del fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="ea3c5-842">*Comience a escribir el nombre del fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="ea3c5-843">![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-fundamentals/_static/image71.png "Presione TAB para seleccionar el fragmento de código resaltado")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="ea3c5-844">*Presione TAB para seleccionar el fragmento de código resaltado*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="ea3c5-845">![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-fundamentals/_static/image72.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="ea3c5-846">*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="ea3c5-847">***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="ea3c5-848">Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="ea3c5-849">Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="ea3c5-850">Seleccione el fragmento de código relevante de la lista haciendo clic en él.</span><span class="sxs-lookup"><span data-stu-id="ea3c5-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="ea3c5-851">![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-fundamentals/_static/image73.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="ea3c5-852">*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="ea3c5-853">![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-fundamentals/_static/image74.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")</span><span class="sxs-lookup"><span data-stu-id="ea3c5-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="ea3c5-854">*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*</span><span class="sxs-lookup"><span data-stu-id="ea3c5-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
