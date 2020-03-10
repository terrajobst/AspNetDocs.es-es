---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: archivo: > nuevo proyecto | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 1 se tratan información general y archivo o proyecto nuevo.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516541"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="0673c-104">Parte 1: archivo: > nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="0673c-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="0673c-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0673c-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0673c-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="0673c-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0673c-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="0673c-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0673c-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0673c-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0673c-109">En la parte 1 se tratan información general y archivo o proyecto nuevo.</span><span class="sxs-lookup"><span data-stu-id="0673c-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="0673c-110">Visión</span><span class="sxs-lookup"><span data-stu-id="0673c-110">Overview</span></span>

<span data-ttu-id="0673c-111">Este tutorial es una introducción a los WebForms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0673c-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="0673c-112">Comenzaremos lentamente, por lo que la experiencia de desarrollo web de nivel principiante es correcta.</span><span class="sxs-lookup"><span data-stu-id="0673c-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="0673c-113">La aplicación que se va a crear es una tienda en línea sencilla.</span><span class="sxs-lookup"><span data-stu-id="0673c-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="0673c-114">Los visitantes pueden examinar productos por Categoría:</span><span class="sxs-lookup"><span data-stu-id="0673c-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="0673c-115">Pueden ver un solo producto y agregarlo a su carro:</span><span class="sxs-lookup"><span data-stu-id="0673c-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="0673c-116">Pueden revisar el carro y quitar los elementos que ya no deseen:</span><span class="sxs-lookup"><span data-stu-id="0673c-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="0673c-117">Al continuar con la desprotección, se les pedirá que</span><span class="sxs-lookup"><span data-stu-id="0673c-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="0673c-118">Después de la ordenación, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="0673c-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="0673c-119">Comenzaremos por crear un nuevo proyecto de WebForms de ASP.NET en Visual Studio 2010 y agregaremos características de forma incremental para crear una aplicación totalmente operativa.</span><span class="sxs-lookup"><span data-stu-id="0673c-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="0673c-120">A lo largo del proceso, veremos el acceso a la base de datos, las vistas de lista y cuadrícula, las páginas de actualización de datos, la validación de datos, el uso de páginas maestras para un diseño de página coherente, AJAX, validación, pertenencia a usuarios, etc.</span><span class="sxs-lookup"><span data-stu-id="0673c-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="0673c-121">Puede seguir paso a paso, o puede descargar la aplicación completada desde [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="0673c-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="0673c-122">Puede usar Visual Studio 2010 o la versión gratuita de Visual Web Developer 2010 desde [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="0673c-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="0673c-123">Para compilar la aplicación, puede usar SQL Server o el SQL Server Express gratuito para hospedar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0673c-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="0673c-124">Archivo/nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="0673c-124">File / New Project</span></span>

<span data-ttu-id="0673c-125">Comenzaremos seleccionando el nuevo proyecto en el menú Archivo de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0673c-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="0673c-126">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="0673c-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="0673c-127">Seleccionaremos el grupo de C# plantillas de Visual/Web a la izquierda y, a continuación, elegiremos la plantilla "aplicación Web de ASP.net" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="0673c-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="0673c-128">Asigne al proyecto el nombre TailspinSpyworks y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="0673c-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="0673c-129">Se creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="0673c-129">This will create our project.</span></span> <span data-ttu-id="0673c-130">Echemos un vistazo a las carpetas que se incluyen en nuestra aplicación en el Explorador de soluciones del lado derecho.</span><span class="sxs-lookup"><span data-stu-id="0673c-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="0673c-131">La solución vacía no está completamente vacía: agrega una estructura de carpetas básica:</span><span class="sxs-lookup"><span data-stu-id="0673c-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="0673c-132">Tenga en cuenta las convenciones implementadas por la plantilla de proyecto predeterminada de ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="0673c-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="0673c-133">La carpeta "cuenta" implementa una interfaz de usuario básica para ASP. Subsistema de pertenencia de la red.</span><span class="sxs-lookup"><span data-stu-id="0673c-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="0673c-134">La carpeta "scripts" actúa como repositorio para los archivos JavaScript del lado cliente y los archivos principales de jQuery. js están disponibles de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0673c-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="0673c-135">La carpeta "Styles" se usa para organizar los objetos visuales del sitio web (hojas de estilos CSS)</span><span class="sxs-lookup"><span data-stu-id="0673c-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="0673c-136">Cuando se presiona F5 para ejecutar la aplicación y se representa la página default. aspx, vemos lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="0673c-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="0673c-137">Nuestra primera mejora de la aplicación será reemplazar el archivo Style. CSS de la plantilla WebForms predeterminada por las clases CSS y los archivos de imagen asociados que van a representar el asthetics visual que queremos para nuestra aplicación Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0673c-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="0673c-138">Después de hacerlo, la página default. aspx se representa como esta.</span><span class="sxs-lookup"><span data-stu-id="0673c-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="0673c-139">Observe los vínculos de imagen en la parte superior derecha de la página y los elementos de menú que se han agregado a la página maestra.</span><span class="sxs-lookup"><span data-stu-id="0673c-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="0673c-140">Solo los vínculos "Inicio de sesión" y "cuenta" apuntan a las páginas que existen (generadas por la plantilla predeterminada) y el resto de las páginas que se van a implementar a medida que se compila la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0673c-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="0673c-141">También vamos a reubicar la página maestra en el directorio de estilos.</span><span class="sxs-lookup"><span data-stu-id="0673c-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="0673c-142">Aunque esto es solo una preferencia, puede simplificar las cosas si decidimos que la aplicación "se pueda enmascarar" en el futuro.</span><span class="sxs-lookup"><span data-stu-id="0673c-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="0673c-143">Después de hacerlo, deberá cambiar las referencias de la página maestra en todos los archivos. aspx generados por las páginas de WebForms predeterminadas de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0673c-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0673c-144">Siguiente</span><span class="sxs-lookup"><span data-stu-id="0673c-144">Next</span></span>](tailspin-spyworks-part-2.md)
