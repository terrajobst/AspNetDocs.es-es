---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introducción a ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469801"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="52192-104">Introducción a ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="52192-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="52192-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="52192-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="52192-106">Una versión actualizada si este tutorial está disponible [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="52192-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="52192-107">En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="52192-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="52192-108">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="52192-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="52192-109">Creará una aplicación web simple que lee y escribe en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="52192-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="52192-110">Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="52192-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="52192-111">Vamos a crear nuestra primera aplicación web MVC de ASP.NET con [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="52192-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="52192-112">Crearemos una pequeña aplicación de lista de películas que nos permitirá crear y enumerar películas.</span><span class="sxs-lookup"><span data-stu-id="52192-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="52192-113">Lo que creará</span><span class="sxs-lookup"><span data-stu-id="52192-113">What You'll Build</span></span>

<span data-ttu-id="52192-114">A continuación se muestran dos capturas de pantallas de la aplicación que va a compilar.</span><span class="sxs-lookup"><span data-stu-id="52192-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="52192-115">Tendrá una tabla sencilla de películas con varias columnas.</span><span class="sxs-lookup"><span data-stu-id="52192-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="52192-116">[![lista de películas: Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="52192-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="52192-117">Y tendrá un formulario de creación para que podamos agregar películas a la lista.</span><span class="sxs-lookup"><span data-stu-id="52192-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="52192-118">[![crear una película: Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="52192-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="52192-119">Habilidades que aprenderá</span><span class="sxs-lookup"><span data-stu-id="52192-119">Skills You'll Learn</span></span>

<span data-ttu-id="52192-120">Este tutorial le enseñará los conceptos básicos de la creación de una aplicación web MVC de ASP.NET con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52192-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="52192-121">Aprenderá lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="52192-121">You'll learn:</span></span>

- <span data-ttu-id="52192-122">Cómo crear un nuevo proyecto de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="52192-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="52192-123">Cómo crear una nueva base de datos con SQL Server</span><span class="sxs-lookup"><span data-stu-id="52192-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="52192-124">Cómo crear controladores y vistas de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="52192-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="52192-125">Cómo recuperar y Mostrar datos</span><span class="sxs-lookup"><span data-stu-id="52192-125">How to retrieve and display data</span></span>
- <span data-ttu-id="52192-126">Cómo editar datos y habilitar la validación de datos</span><span class="sxs-lookup"><span data-stu-id="52192-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="52192-127">Cómo actualizar el esquema de la base de datos</span><span class="sxs-lookup"><span data-stu-id="52192-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="52192-128">Primeros pasos</span><span class="sxs-lookup"><span data-stu-id="52192-128">Get Started</span></span>

<span data-ttu-id="52192-129">Para empezar, ejecute Visual Web Developer 2010 Express (lo llamaremos "vWD existente" desde ahora) y seleccione nuevo proyecto en la pantalla Inicio.</span><span class="sxs-lookup"><span data-stu-id="52192-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="52192-130">Visual Web Developer es un IDE o un entorno de desarrollo integrado.</span><span class="sxs-lookup"><span data-stu-id="52192-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="52192-131">Del mismo modo que usa Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="52192-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="52192-132">Hay una barra de herramientas en la parte superior que muestra varias opciones disponibles, así como el menú que también podría haber usado para seleccionar archivo | Nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="52192-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="52192-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="52192-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="52192-134">Crear la primera aplicación</span><span class="sxs-lookup"><span data-stu-id="52192-134">Creating Your First Application</span></span>

<span data-ttu-id="52192-135">Puede crear aplicaciones mediante Visual Basic o visual C#.</span><span class="sxs-lookup"><span data-stu-id="52192-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="52192-136">Por ahora, seleccione visual C# a la izquierda y, a continuación, elija "aplicación web MVC 2 de ASP.net".</span><span class="sxs-lookup"><span data-stu-id="52192-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="52192-137">Asigne el nombre "películas" al proyecto y haga clic en Aceptar.</span><span class="sxs-lookup"><span data-stu-id="52192-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="52192-138">[![nuevo proyecto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="52192-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="52192-139">En el lado derecho está el Explorador de soluciones que muestra todos los archivos y carpetas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52192-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="52192-140">La ventana grande del centro es donde se edita el código y se dedica la mayor parte del tiempo.</span><span class="sxs-lookup"><span data-stu-id="52192-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="52192-141">Visual Studio usó una plantilla predeterminada para el proyecto ASP.NET MVC que acaba de crear, por lo que tiene una aplicación en funcionamiento en este momento sin hacer nada.</span><span class="sxs-lookup"><span data-stu-id="52192-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="52192-142">Este es un sencillo "Hola mundo!</span><span class="sxs-lookup"><span data-stu-id="52192-142">This is a simple "Hello World!</span></span> <span data-ttu-id="52192-143">Project, y es un buen punto de partida para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52192-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="52192-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="52192-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="52192-145">Seleccione el botón "reproducir" en la barra de herramientas.</span><span class="sxs-lookup"><span data-stu-id="52192-145">Select the "play" button to the toolbar.</span></span>

![Iniciar depuración](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="52192-147">Es una flecha verde que apunta a la derecha que compilará el programa e iniciará la aplicación en un explorador Web.</span><span class="sxs-lookup"><span data-stu-id="52192-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="52192-148">*Nota: en su lugar, puede presionar F5 en el teclado o seleccionar depurar-&gt;iniciar depuración desde el menú "depurar".*</span><span class="sxs-lookup"><span data-stu-id="52192-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="52192-149">Esto hará que Visual Web Developer inicie un servidor Web de desarrollo y ejecute la aplicación web (no es necesario realizar ninguna configuración o pasos manuales para habilitarlo).</span><span class="sxs-lookup"><span data-stu-id="52192-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="52192-150">A continuación, iniciará un explorador y lo configurará para examinar la Página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="52192-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="52192-151">Observe que la barra de direcciones del explorador indica "localhost", y no algo como example.com.</span><span class="sxs-lookup"><span data-stu-id="52192-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="52192-152">Esto se debe a que localhost siempre apunta a su propio equipo local, que en este caso está ejecutando la aplicación que acabamos de crear.</span><span class="sxs-lookup"><span data-stu-id="52192-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="52192-153">[Página principal de ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="52192-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="52192-154">De forma predeterminada, esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica.</span><span class="sxs-lookup"><span data-stu-id="52192-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="52192-155">Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso.</span><span class="sxs-lookup"><span data-stu-id="52192-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="52192-156">Cierre el explorador y permita cambiar parte del código.</span><span class="sxs-lookup"><span data-stu-id="52192-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="52192-157">Siguiente</span><span class="sxs-lookup"><span data-stu-id="52192-157">Next</span></span>](getting-started-with-mvc-part2.md)
