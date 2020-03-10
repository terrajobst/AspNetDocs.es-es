---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Crear un nuevo proyecto de ASP.NET MVC | Microsoft Docs
author: microsoft
description: En el paso 1 se muestra cómo poner en marcha la estructura de la aplicación NerdDinner básica.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469243"
---
# <a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="8789a-103">Crear un proyecto de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8789a-103">Create a New ASP.NET MVC Project</span></span>

<span data-ttu-id="8789a-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8789a-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8789a-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="8789a-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8789a-106">Este es el paso 1 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="8789a-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="8789a-107">En el paso 1 se muestra cómo poner en marcha la estructura de la aplicación NerdDinner básica.</span><span class="sxs-lookup"><span data-stu-id="8789a-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="8789a-108">Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="8789a-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="8789a-109">NerdDinner paso 1: archivo-&gt;nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="8789a-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="8789a-110">Comenzaremos nuestra aplicación NerdDinner seleccionando el elemento **de menú Archivo-&gt;nuevo proyecto** en visual Studio 2008 o gratis Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="8789a-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="8789a-111">Se abrirá el cuadro de diálogo "nuevo proyecto".</span><span class="sxs-lookup"><span data-stu-id="8789a-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="8789a-112">Para crear una nueva aplicación ASP.NET MVC, seleccionaremos el nodo "Web" en el lado izquierdo del cuadro de diálogo y, a continuación, elegiremos la plantilla de proyecto "ASP.NET MVC Web Application" a la derecha:</span><span class="sxs-lookup"><span data-stu-id="8789a-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="8789a-113">*Importante: Asegúrese de que ha descargado e instalado ASP.NET MVC; de lo contrario, no se mostrará en el cuadro de diálogo nuevo proyecto. Puede usar la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) si aún no la ha instalado (ASP.NET MVC está disponible en la sección "plataforma web: entornos de&gt;y tiempos de ejecución").*</span><span class="sxs-lookup"><span data-stu-id="8789a-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="8789a-114">Asignaremos el nombre "NerdDinner" al nuevo proyecto y, a continuación, haremos clic en el botón "Aceptar" para crearlo.</span><span class="sxs-lookup"><span data-stu-id="8789a-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="8789a-115">Al hacer clic en "Aceptar", Visual Studio abrirá un cuadro de diálogo adicional en el que se le pregunta si quiere crear también un proyecto de prueba unitaria para la nueva aplicación.</span><span class="sxs-lookup"><span data-stu-id="8789a-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="8789a-116">Este proyecto de prueba unitaria nos permite crear pruebas automatizadas que comprueban la funcionalidad y el comportamiento de la aplicación (algo que veremos cómo hacerlo más adelante en este tutorial).</span><span class="sxs-lookup"><span data-stu-id="8789a-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="8789a-117">La lista desplegable "marco de prueba" del cuadro de diálogo anterior se rellena con todas las plantillas de proyecto de prueba unitaria de ASP.NET MVC disponibles en la máquina.</span><span class="sxs-lookup"><span data-stu-id="8789a-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="8789a-118">Las versiones se pueden descargar para NUnit, MBUnit y XUnit.</span><span class="sxs-lookup"><span data-stu-id="8789a-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="8789a-119">También se admite el marco de pruebas unitarias de Visual Studio integrado.</span><span class="sxs-lookup"><span data-stu-id="8789a-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="8789a-120">*Nota: el marco de pruebas unitarias de Visual Studio solo está disponible con Visual Studio 2008 Professional y versiones posteriores. Si usa VS 2008 Standard Edition o Visual Web Developer 2008 Express, tendrá que descargar e instalar las extensiones NUnit, MBUnit o XUnit para ASP.NET MVC para que se muestre este cuadro de diálogo. No se mostrará el cuadro de diálogo si no hay ningún marco de pruebas instalado.*</span><span class="sxs-lookup"><span data-stu-id="8789a-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="8789a-121">Usaremos el nombre predeterminado "NerdDinner. tests" para el proyecto de prueba que creamos y usaremos la opción de marco "prueba unitaria de Visual Studio".</span><span class="sxs-lookup"><span data-stu-id="8789a-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="8789a-122">Cuando hacemos clic en el botón "Aceptar", Visual Studio creará una solución para nosotros con dos proyectos en él: uno para nuestra aplicación web y otro para nuestras pruebas unitarias:</span><span class="sxs-lookup"><span data-stu-id="8789a-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="8789a-123">Examinar la estructura de directorios de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8789a-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="8789a-124">Al crear una nueva aplicación ASP.NET MVC con Visual Studio, se agrega automáticamente un número de archivos y directorios al proyecto:</span><span class="sxs-lookup"><span data-stu-id="8789a-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="8789a-125">Los proyectos de MVC de ASP.NET de forma predeterminada tienen seis directorios de nivel superior:</span><span class="sxs-lookup"><span data-stu-id="8789a-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="8789a-126">**Directorio**</span><span class="sxs-lookup"><span data-stu-id="8789a-126">**Directory**</span></span> | <span data-ttu-id="8789a-127">**Propósito**</span><span class="sxs-lookup"><span data-stu-id="8789a-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="8789a-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="8789a-128">**/Controllers**</span></span> | <span data-ttu-id="8789a-129">Dónde colocar las clases de controlador que controlan las solicitudes de dirección URL</span><span class="sxs-lookup"><span data-stu-id="8789a-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="8789a-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="8789a-130">**/Models**</span></span> | <span data-ttu-id="8789a-131">Dónde colocar las clases que representan y manipulan los datos</span><span class="sxs-lookup"><span data-stu-id="8789a-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="8789a-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="8789a-132">**/Views**</span></span> | <span data-ttu-id="8789a-133">Dónde colocar los archivos de plantilla de interfaz de usuario que son responsables de la representación de la salida</span><span class="sxs-lookup"><span data-stu-id="8789a-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="8789a-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="8789a-134">**/Scripts**</span></span> | <span data-ttu-id="8789a-135">Dónde colocar los archivos y scripts de la biblioteca de JavaScript (. js)</span><span class="sxs-lookup"><span data-stu-id="8789a-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="8789a-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="8789a-136">**/Content**</span></span> | <span data-ttu-id="8789a-137">Dónde se colocan los archivos de imagen y CSS, y otros contenidos no dinámicos o que no son de JavaScript</span><span class="sxs-lookup"><span data-stu-id="8789a-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="8789a-138">**/App\_datos**</span><span class="sxs-lookup"><span data-stu-id="8789a-138">**/App\_Data**</span></span> | <span data-ttu-id="8789a-139">Donde se almacenan los archivos de datos que se van a leer y escribir.</span><span class="sxs-lookup"><span data-stu-id="8789a-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="8789a-140">ASP.NET MVC no requiere esta estructura.</span><span class="sxs-lookup"><span data-stu-id="8789a-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="8789a-141">De hecho, los desarrolladores que trabajan en aplicaciones de gran tamaño suelen particionar la aplicación en varios proyectos para que sea más fácil de administrar (por ejemplo: las clases del modelo de datos van a menudo en un proyecto de biblioteca de clases independiente de la aplicación web).</span><span class="sxs-lookup"><span data-stu-id="8789a-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="8789a-142">La estructura predeterminada del proyecto, sin embargo, proporciona una buena Convención de directorio predeterminada que se puede usar para mantener la preocupación en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8789a-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="8789a-143">Al expandir el directorio/Controllers, veremos que Visual Studio agregó dos clases de controlador: HomeController y AccountController, de forma predeterminada al proyecto:</span><span class="sxs-lookup"><span data-stu-id="8789a-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="8789a-144">Al expandir el directorio/views, encontrará tres subdirectorios:/Home,/Account y/Shared, así como varios archivos de plantilla dentro de ellos también se agregaron al proyecto de forma predeterminada:</span><span class="sxs-lookup"><span data-stu-id="8789a-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="8789a-145">Cuando se expandan los directorios/Content y/scripts, encontrará un archivo site. CSS que se usa para aplicar estilo a todo el código HTML del sitio, así como a las bibliotecas de JavaScript que pueden habilitar la compatibilidad con ASP.NET AJAX y jQuery en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8789a-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="8789a-146">Al expandir el proyecto NerdDinner. tests, encontrará dos clases que contienen pruebas unitarias para las clases de controlador:</span><span class="sxs-lookup"><span data-stu-id="8789a-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="8789a-147">Estos archivos predeterminados agregados por Visual Studio nos proporcionan una estructura básica para una aplicación de trabajo: completa con Página principal, acerca de la página, Inicio de sesión de cuenta, cierre de sesión y páginas de registro, y una página de error no controlada (todos los cables conectados y listos para usar).</span><span class="sxs-lookup"><span data-stu-id="8789a-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="8789a-148">Ejecución de la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8789a-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="8789a-149">Para ejecutar el proyecto, puede elegir los elementos de menú Depurar **-&gt;iniciar depuración** o depurar **&gt;iniciar sin depurar** :</span><span class="sxs-lookup"><span data-stu-id="8789a-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="8789a-150">Se iniciará el ASP.NET Web-Server integrado que se incluye con Visual Studio y se ejecutará la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8789a-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="8789a-151">A continuación se muestra la Página principal de nuestro nuevo proyecto (URL: "/") cuando se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="8789a-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="8789a-152">Al hacer clic en la pestaña "acerca de", se muestra una página acerca de (URL: "/Home/About"):</span><span class="sxs-lookup"><span data-stu-id="8789a-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="8789a-153">Al hacer clic en el vínculo "iniciar sesión" en la parte superior derecha nos remite a una página de inicio de sesión (URL: "/Account/LogOn")</span><span class="sxs-lookup"><span data-stu-id="8789a-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="8789a-154">Si no tenemos una cuenta de inicio de sesión, podemos hacer clic en el vínculo Register (URL: "/Account/Register") para crear uno:</span><span class="sxs-lookup"><span data-stu-id="8789a-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="8789a-155">El código para implementar la funcionalidad anterior de inicio, acerca de y cierre de sesión/Registro se agregó de forma predeterminada al crear el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="8789a-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="8789a-156">La usaremos como punto de partida de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="8789a-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="8789a-157">Prueba de la aplicación NerdDinner</span><span class="sxs-lookup"><span data-stu-id="8789a-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="8789a-158">Si usamos la edición Professional o una versión posterior de Visual Studio 2008, podemos usar la compatibilidad integrada de IDE de pruebas unitarias en Visual Studio para probar el proyecto:</span><span class="sxs-lookup"><span data-stu-id="8789a-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="8789a-159">Al elegir una de las opciones anteriores, se abrirá el panel "Resultados de pruebas" en el IDE y nos proporcionará el estado Pass/Fail en las 27 pruebas unitarias incluidas en nuestro nuevo proyecto que cubren la funcionalidad integrada:</span><span class="sxs-lookup"><span data-stu-id="8789a-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="8789a-160">Más adelante en este tutorial hablaremos sobre las pruebas automatizadas y agregaremos pruebas unitarias adicionales que cubran la funcionalidad de la aplicación que implementamos.</span><span class="sxs-lookup"><span data-stu-id="8789a-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="8789a-161">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="8789a-161">Next Step</span></span>

<span data-ttu-id="8789a-162">Ahora tenemos una estructura de aplicación básica en su lugar.</span><span class="sxs-lookup"><span data-stu-id="8789a-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="8789a-163">Ahora vamos [a crear una base de datos para almacenar los datos de la aplicación](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="8789a-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8789a-164">[Anterior](introducing-the-nerddinner-tutorial.md)
> [Siguiente](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="8789a-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
