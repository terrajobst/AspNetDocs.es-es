---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: información general y archivo: > nuevo proyecto | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 1 cubre información general y > nuevo proyecto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451285"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="849c1-104">Parte 1: información general y archivo > nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="849c1-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="849c1-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="849c1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="849c1-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="849c1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="849c1-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="849c1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="849c1-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="849c1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="849c1-109">La parte 1 cubre información general y&gt;nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="849c1-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="849c1-110">Información general</span><span class="sxs-lookup"><span data-stu-id="849c1-110">Overview</span></span>

<span data-ttu-id="849c1-111">MVC Music Store es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Web Developer para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="849c1-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="849c1-112">Comenzaremos lentamente, por lo que la experiencia de desarrollo web de nivel principiante es correcta.</span><span class="sxs-lookup"><span data-stu-id="849c1-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="849c1-113">La aplicación que se va a crear es una tienda de música simple.</span><span class="sxs-lookup"><span data-stu-id="849c1-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="849c1-114">Hay tres partes principales en la aplicación: compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="849c1-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="849c1-115">Los visitantes pueden examinar álbumes por género:</span><span class="sxs-lookup"><span data-stu-id="849c1-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="849c1-116">Pueden ver un solo álbum y agregarlo a su carro:</span><span class="sxs-lookup"><span data-stu-id="849c1-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="849c1-117">Pueden revisar el carro y quitar los elementos que ya no deseen:</span><span class="sxs-lookup"><span data-stu-id="849c1-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="849c1-118">Al continuar con la desprotección, se les pedirá que inicien sesión o se registren para una cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="849c1-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="849c1-119">Después de crear una cuenta, puede completar el pedido rellenando la información de envío y pago.</span><span class="sxs-lookup"><span data-stu-id="849c1-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="849c1-120">Para simplificar las cosas, vamos a ejecutar una fantástica promoción: todo está disponible si escribe el código de promoción "gratis".</span><span class="sxs-lookup"><span data-stu-id="849c1-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="849c1-121">Después de la ordenación, verán una pantalla de confirmación simple:</span><span class="sxs-lookup"><span data-stu-id="849c1-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="849c1-122">Además de las páginas orientadas al cliente, también se creará una sección de administrador en la que se muestra una lista de álbumes en los que los administradores pueden crear, editar y eliminar álbumes:</span><span class="sxs-lookup"><span data-stu-id="849c1-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="849c1-123">1. archivo:&gt; nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="849c1-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="849c1-124">Instalación del software</span><span class="sxs-lookup"><span data-stu-id="849c1-124">Installing the software</span></span>

<span data-ttu-id="849c1-125">Este tutorial comenzará por la creación de un nuevo proyecto de ASP.NET MVC 3 con Visual Web Developer 2010 Express gratuito (que es gratuito) y, a continuación, agregaremos características para crear una aplicación que funcione completamente.</span><span class="sxs-lookup"><span data-stu-id="849c1-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="849c1-126">A lo largo del proceso, trataremos el acceso a bases de datos, los escenarios de publicación de formularios, la validación de datos, el uso de páginas maestras para un diseño de página coherente, el uso de AJAX para la validación y actualización de páginas, el inicio de sesión de usuario</span><span class="sxs-lookup"><span data-stu-id="849c1-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="849c1-127">Puede seguir paso a paso, o bien puede descargar la aplicación completada desde [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="849c1-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="849c1-128">Puede usar Visual Studio 2010 SP1 o Visual Web Developer 2010 Express SP1 (una versión gratuita de Visual Studio 2010) para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="849c1-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="849c1-129">Usaremos el SQL Server Compact (también gratuito) para hospedar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="849c1-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="849c1-130">Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="849c1-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="849c1-131">[Requisitos previos de Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="849c1-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="849c1-132">[ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="849c1-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="849c1-133">[SQL Server Compact 4,0]: compatibilidad con el motor de tiempo de ejecución y las herramientas</span><span class="sxs-lookup"><span data-stu-id="849c1-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="849c1-134">Creación de un nuevo proyecto de ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="849c1-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="849c1-135">Comenzaremos seleccionando "nuevo proyecto" en el menú Archivo de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="849c1-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="849c1-136">Se abrirá el cuadro de diálogo nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="849c1-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="849c1-137">Seleccionaremos el grupo plantillas C# Web de Visual&gt; a la izquierda y, a continuación, elegiremos la plantilla "aplicación web MVC 3 ASP.net" en la columna central.</span><span class="sxs-lookup"><span data-stu-id="849c1-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="849c1-138">Asigne al proyecto el nombre MvcMusicStore y presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="849c1-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="849c1-139">Se mostrará un cuadro de diálogo secundario que nos permite realizar algunos valores específicos de MVC para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="849c1-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="849c1-140">Seleccione lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="849c1-140">Select the following:</span></span>

<span data-ttu-id="849c1-141">Plantilla de proyecto: seleccionar vacío</span><span class="sxs-lookup"><span data-stu-id="849c1-141">Project Template - select Empty</span></span>

<span data-ttu-id="849c1-142">Motor de vista: seleccione Razor</span><span class="sxs-lookup"><span data-stu-id="849c1-142">View Engine - select Razor</span></span>

<span data-ttu-id="849c1-143">Usar marcado semántico de HTML5: comprobado</span><span class="sxs-lookup"><span data-stu-id="849c1-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="849c1-144">Compruebe que la configuración sea la que se muestra a continuación y, a continuación, presione el botón Aceptar.</span><span class="sxs-lookup"><span data-stu-id="849c1-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="849c1-145">Se creará el proyecto.</span><span class="sxs-lookup"><span data-stu-id="849c1-145">This will create our project.</span></span> <span data-ttu-id="849c1-146">Echemos un vistazo a las carpetas que se han agregado a nuestra aplicación en el Explorador de soluciones del lado derecho.</span><span class="sxs-lookup"><span data-stu-id="849c1-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="849c1-147">La plantilla de MVC 3 vacía no está completamente vacía: agrega una estructura de carpetas básica:</span><span class="sxs-lookup"><span data-stu-id="849c1-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="849c1-148">ASP.NET MVC usa algunas convenciones de nomenclatura básicas para los nombres de carpeta:</span><span class="sxs-lookup"><span data-stu-id="849c1-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="849c1-149">**Carpeta**</span><span class="sxs-lookup"><span data-stu-id="849c1-149">**Folder**</span></span> | <span data-ttu-id="849c1-150">**Propósito**</span><span class="sxs-lookup"><span data-stu-id="849c1-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="849c1-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="849c1-151">**/Controllers**</span></span> | <span data-ttu-id="849c1-152">Los controladores responden a la entrada desde el explorador, deciden qué hacer con él y devuelven respuesta al usuario.</span><span class="sxs-lookup"><span data-stu-id="849c1-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="849c1-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="849c1-153">**/Views**</span></span> | <span data-ttu-id="849c1-154">Vistas que contienen nuestras plantillas de interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="849c1-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="849c1-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="849c1-155">**/Models**</span></span> | <span data-ttu-id="849c1-156">Los modelos contienen y manipulan datos</span><span class="sxs-lookup"><span data-stu-id="849c1-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="849c1-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="849c1-157">**/Content**</span></span> | <span data-ttu-id="849c1-158">Esta carpeta contiene nuestras imágenes, CSS y cualquier otro contenido estático.</span><span class="sxs-lookup"><span data-stu-id="849c1-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="849c1-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="849c1-159">**/Scripts**</span></span> | <span data-ttu-id="849c1-160">Esta carpeta contiene los archivos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="849c1-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="849c1-161">Estas carpetas se incluyen incluso en una aplicación ASP.NET MVC vacía porque el marco ASP.NET MVC usa de forma predeterminada un enfoque de "Convención de configuración" y realiza algunas suposiciones predeterminadas en función de las convenciones de nomenclatura de carpetas.</span><span class="sxs-lookup"><span data-stu-id="849c1-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="849c1-162">Por ejemplo, los controladores buscan vistas en la carpeta views de forma predeterminada sin tener que especificar explícitamente esto en el código.</span><span class="sxs-lookup"><span data-stu-id="849c1-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="849c1-163">El uso de las convenciones predeterminadas reduce la cantidad de código que necesita escribir y también puede facilitar a otros desarrolladores el conocimiento del proyecto.</span><span class="sxs-lookup"><span data-stu-id="849c1-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="849c1-164">Explicaremos estas convenciones más a medida que compilamos nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="849c1-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="849c1-165">Siguiente</span><span class="sxs-lookup"><span data-stu-id="849c1-165">Next</span></span>](mvc-music-store-part-2.md)
