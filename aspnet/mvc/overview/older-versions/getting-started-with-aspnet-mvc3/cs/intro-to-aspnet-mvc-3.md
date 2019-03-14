---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introducción a ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 53306318ab1a782d3605876aac53ec903d053269
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049332"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="28253-103">Introducción a ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="28253-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="28253-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="28253-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> > [!NOTE]
> > <span data-ttu-id="28253-105">Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="28253-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="28253-106">Es más seguro y mucho más fácil de seguir y se muestran las características más.</span><span class="sxs-lookup"><span data-stu-id="28253-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="28253-107">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="28253-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="28253-108">Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación.</span><span class="sxs-lookup"><span data-stu-id="28253-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="28253-109">Puede instalar todos ellos haciendo clic en el siguiente vínculo: [Instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="28253-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="28253-110">Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:</span><span class="sxs-lookup"><span data-stu-id="28253-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="28253-111">Requisitos previos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="28253-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="28253-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="28253-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="28253-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)</span><span class="sxs-lookup"><span data-stu-id="28253-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="28253-114">Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [Requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="28253-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="28253-115">Un proyecto de Visual Web Developer con código fuente de C# está disponible como acompañamiento de este tema.</span><span class="sxs-lookup"><span data-stu-id="28253-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="28253-116">[Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="28253-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="28253-117">Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="28253-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="28253-118">¿Qué va a crear</span><span class="sxs-lookup"><span data-stu-id="28253-118">What You'll Build</span></span>

<span data-ttu-id="28253-119">Implementará una aplicación simple de la lista de películas que admite la creación, edición y la lista de películas de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="28253-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="28253-120">A continuación se muestran dos capturas de pantalla de la aplicación que se va a crear.</span><span class="sxs-lookup"><span data-stu-id="28253-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="28253-121">Incluye una página que muestra una lista de películas de una base de datos:</span><span class="sxs-lookup"><span data-stu-id="28253-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="28253-123">La aplicación también le permite agregar, editar y eliminar las películas, así como ver detalles acerca de los individuales.</span><span class="sxs-lookup"><span data-stu-id="28253-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="28253-124">Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos están correctos.</span><span class="sxs-lookup"><span data-stu-id="28253-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="28253-125">Habilidades que aprenderá</span><span class="sxs-lookup"><span data-stu-id="28253-125">Skills You'll Learn</span></span>

<span data-ttu-id="28253-126">Aquí es lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="28253-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="28253-127">Cómo crear un nuevo proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="28253-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="28253-128">Cómo crear controladores y vistas de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="28253-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="28253-129">Cómo crear una nueva base de datos mediante el paradigma de Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="28253-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="28253-130">Cómo recuperar y mostrar datos.</span><span class="sxs-lookup"><span data-stu-id="28253-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="28253-131">Cómo editar los datos y habilitar la validación de datos.</span><span class="sxs-lookup"><span data-stu-id="28253-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="28253-132">Introducción</span><span class="sxs-lookup"><span data-stu-id="28253-132">Getting Started</span></span>

<span data-ttu-id="28253-133">Comience ejecutando Visual Web Developer 2010 Express ("Visual Web Developer" para abreviar) y seleccione **nuevo proyecto** desde el **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="28253-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="28253-134">Visual Web Developer es un entorno de desarrollo integrado o IDE.</span><span class="sxs-lookup"><span data-stu-id="28253-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="28253-135">Al igual que utiliza Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="28253-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="28253-136">En Visual Web Developer, hay una barra de herramientas en la parte superior que muestra diversas opciones disponibles para usted.</span><span class="sxs-lookup"><span data-stu-id="28253-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="28253-137">También hay un menú que proporciona otra manera de realizar las tareas en el IDE.</span><span class="sxs-lookup"><span data-stu-id="28253-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="28253-138">(Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde el **iniciar** página, puede usar el menú y seleccione **archivo** &gt; **denuevoproyecto**.)</span><span class="sxs-lookup"><span data-stu-id="28253-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="28253-139">Crear su primera aplicación</span><span class="sxs-lookup"><span data-stu-id="28253-139">Creating Your First Application</span></span>

<span data-ttu-id="28253-140">Puede crear aplicaciones con Visual Basic o Visual C# como lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="28253-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="28253-141">Seleccione Visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="28253-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="28253-142">Nombre del proyecto "MvcMovie" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="28253-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="28253-143">(Si lo prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="28253-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="28253-144">En el **nuevo proyecto de ASP.NET MVC 3** cuadro de diálogo, seleccione **aplicación de Internet**.</span><span class="sxs-lookup"><span data-stu-id="28253-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="28253-145">Comprobar **uso HTML5 marcado** y dejar **Razor** como el motor de vistas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="28253-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="28253-146">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="28253-146">Click **OK**.</span></span> <span data-ttu-id="28253-147">Visual Web Developer se usa una plantilla predeterminada para el proyecto de ASP.NET MVC que acaba de crear, por lo que tiene ahora una aplicación de trabajo sin hacer nada!</span><span class="sxs-lookup"><span data-stu-id="28253-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="28253-148">Se trata de un simple "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="28253-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="28253-149">proyecto y, de un buen lugar para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28253-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="28253-150">En el menú **Depurar**, seleccione **Iniciar depuración**.</span><span class="sxs-lookup"><span data-stu-id="28253-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="28253-151">Observe que el método abreviado de teclado para iniciar la depuración F5.</span><span class="sxs-lookup"><span data-stu-id="28253-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="28253-152">F5 hace que Visual Web Developer iniciar un servidor web de desarrollo y ejecutar la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="28253-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="28253-153">A continuación, Visual Web Developer inicia un explorador y se abre la página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28253-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="28253-154">Tenga en cuenta que la barra de direcciones del explorador dice `localhost` y no algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="28253-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="28253-155">Eso es porque `localhost` siempre apunta a su propio equipo local, que en este caso, se ejecuta la aplicación que acaba de crear.</span><span class="sxs-lookup"><span data-stu-id="28253-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="28253-156">Cuando Visual Web Developer ejecuta un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="28253-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="28253-157">En la imagen siguiente, el número de puerto aleatorio es 43246.</span><span class="sxs-lookup"><span data-stu-id="28253-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="28253-158">Al ejecutar la aplicación, probablemente verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="28253-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="28253-159">Desde el comienzo esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica.</span><span class="sxs-lookup"><span data-stu-id="28253-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="28253-160">El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso.</span><span class="sxs-lookup"><span data-stu-id="28253-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="28253-161">Cierre el explorador y vamos a cambiar algo de código.</span><span class="sxs-lookup"><span data-stu-id="28253-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="28253-162">Siguiente</span><span class="sxs-lookup"><span data-stu-id="28253-162">Next</span></span>](adding-a-controller.md)