---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción con formularios Web Forms de ASP.NET 4,7 y Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458347"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="e0af8-103">Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e0af8-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>

<span data-ttu-id="e0af8-104">[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="e0af8-104">[Download Wingtip Toys Sample Project (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="e0af8-105">En esta serie de tutoriales se muestra cómo crear una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e0af8-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="e0af8-106">Introducción</span><span class="sxs-lookup"><span data-stu-id="e0af8-106">Introduction</span></span>

<span data-ttu-id="e0af8-107">Esta serie de tutoriales le guía en la creación de una aplicación de formularios Web Forms ASP.NET con Visual Studio 2017 y ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="e0af8-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="e0af8-108">Creará una aplicación llamada **Wingtip Toys** -un sitio web de escaparate simplificado que vende artículos en línea.</span><span class="sxs-lookup"><span data-stu-id="e0af8-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="e0af8-109">Durante la serie, se resaltan las nuevas características de ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="e0af8-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="e0af8-110">Audiencia de destino</span><span class="sxs-lookup"><span data-stu-id="e0af8-110">Target audience</span></span>

<span data-ttu-id="e0af8-111">Los desarrolladores nuevos en ASP.NET Web Forms son la audiencia de destino de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="e0af8-112">Debería tener algún conocimiento en las siguientes áreas:</span><span class="sxs-lookup"><span data-stu-id="e0af8-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="e0af8-113">Programación orientada a objetos (OOP) e idiomas</span><span class="sxs-lookup"><span data-stu-id="e0af8-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="e0af8-114">Desarrollo web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="e0af8-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="e0af8-115">Bases de datos relacionales</span><span class="sxs-lookup"><span data-stu-id="e0af8-115">Relational databases</span></span>
- <span data-ttu-id="e0af8-116">Arquitectura de n niveles</span><span class="sxs-lookup"><span data-stu-id="e0af8-116">N-tier architecture</span></span>

<span data-ttu-id="e0af8-117">Para revisar estas áreas, considere la posibilidad de estudiar el siguiente contenido:</span><span class="sxs-lookup"><span data-stu-id="e0af8-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="e0af8-118">Introducción a Visual C#</span><span class="sxs-lookup"><span data-stu-id="e0af8-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="e0af8-119">[Desarrollo web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, jQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="e0af8-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="e0af8-120">Base de datos relacional</span><span class="sxs-lookup"><span data-stu-id="e0af8-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="e0af8-121">Arquitectura de varios niveles</span><span class="sxs-lookup"><span data-stu-id="e0af8-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="e0af8-122">Características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e0af8-122">Application features</span></span>

<span data-ttu-id="e0af8-123">Las características de formularios Web Forms de ASP.NET presentadas en esta serie incluyen:</span><span class="sxs-lookup"><span data-stu-id="e0af8-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="e0af8-124">Proyecto de aplicación web (no proyecto de sitio web)</span><span class="sxs-lookup"><span data-stu-id="e0af8-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="e0af8-125">formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="e0af8-125">Web Forms</span></span>
- <span data-ttu-id="e0af8-126">Páginas maestras, configuración</span><span class="sxs-lookup"><span data-stu-id="e0af8-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="e0af8-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="e0af8-127">Bootstrap</span></span>
- <span data-ttu-id="e0af8-128">Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="e0af8-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="e0af8-129">Validación de solicitudes</span><span class="sxs-lookup"><span data-stu-id="e0af8-129">Request Validation</span></span>
- <span data-ttu-id="e0af8-130">Controles de datos fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="e0af8-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="e0af8-131">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="e0af8-131">Model Binding</span></span>
- <span data-ttu-id="e0af8-132">Anotaciones de datos</span><span class="sxs-lookup"><span data-stu-id="e0af8-132">Data Annotations</span></span>
- <span data-ttu-id="e0af8-133">Proveedores de valores</span><span class="sxs-lookup"><span data-stu-id="e0af8-133">Value Providers</span></span>
- <span data-ttu-id="e0af8-134">SSL y OAuth</span><span class="sxs-lookup"><span data-stu-id="e0af8-134">SSL and OAuth</span></span>
- <span data-ttu-id="e0af8-135">ASP.NET Identity, configuración y autorización</span><span class="sxs-lookup"><span data-stu-id="e0af8-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="e0af8-136">Validación discreta</span><span class="sxs-lookup"><span data-stu-id="e0af8-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="e0af8-137">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="e0af8-137">Routing</span></span>
- <span data-ttu-id="e0af8-138">Control de errores de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e0af8-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="e0af8-139">Escenarios y tareas de la aplicación</span><span class="sxs-lookup"><span data-stu-id="e0af8-139">Application scenarios and tasks</span></span>

<span data-ttu-id="e0af8-140">Las tareas de la serie de tutoriales incluyen:</span><span class="sxs-lookup"><span data-stu-id="e0af8-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="e0af8-141">Crear, revisar y ejecutar un proyecto nuevo</span><span class="sxs-lookup"><span data-stu-id="e0af8-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="e0af8-142">Crear una estructura de base de datos</span><span class="sxs-lookup"><span data-stu-id="e0af8-142">Creating a database structure</span></span>
- <span data-ttu-id="e0af8-143">Inicializar y propagar una base de datos</span><span class="sxs-lookup"><span data-stu-id="e0af8-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="e0af8-144">Personalización de la interfaz de usuario con estilos, gráficos y una página maestra</span><span class="sxs-lookup"><span data-stu-id="e0af8-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="e0af8-145">Agregar páginas y navegación</span><span class="sxs-lookup"><span data-stu-id="e0af8-145">Adding pages and navigation</span></span>
- <span data-ttu-id="e0af8-146">Mostrar detalles del menú y datos del producto</span><span class="sxs-lookup"><span data-stu-id="e0af8-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="e0af8-147">Creación de un carro de la compra</span><span class="sxs-lookup"><span data-stu-id="e0af8-147">Creating a shopping cart</span></span>
- <span data-ttu-id="e0af8-148">Adición de compatibilidad con SSL y OAuth</span><span class="sxs-lookup"><span data-stu-id="e0af8-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="e0af8-149">Agregar un método de pago</span><span class="sxs-lookup"><span data-stu-id="e0af8-149">Adding a payment method</span></span>
- <span data-ttu-id="e0af8-150">Incluir un rol de administrador y un usuario en la aplicación</span><span class="sxs-lookup"><span data-stu-id="e0af8-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="e0af8-151">Restringir el acceso a páginas y carpetas específicas</span><span class="sxs-lookup"><span data-stu-id="e0af8-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="e0af8-152">Carga de un archivo en la aplicación Web</span><span class="sxs-lookup"><span data-stu-id="e0af8-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="e0af8-153">Implementar la validación de entrada</span><span class="sxs-lookup"><span data-stu-id="e0af8-153">Implementing input validation</span></span>
- <span data-ttu-id="e0af8-154">Registro de rutas para la aplicación Web</span><span class="sxs-lookup"><span data-stu-id="e0af8-154">Registering routes for the web application</span></span>
- <span data-ttu-id="e0af8-155">Implementar el control de errores y el registro de errores</span><span class="sxs-lookup"><span data-stu-id="e0af8-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="e0af8-156">Información general</span><span class="sxs-lookup"><span data-stu-id="e0af8-156">Overview</span></span>

<span data-ttu-id="e0af8-157">Esta serie de tutoriales está destinada a usuarios familiarizados con los conceptos de programación, pero que son nuevos en ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e0af8-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="e0af8-158">Si ya está familiarizado con los formularios Web Forms de ASP.NET, esta serie puede ayudarle a conocer las nuevas características de ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="e0af8-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="e0af8-159">Para los lectores que no estén familiarizados con los conceptos de programación y los formularios Web Forms de ASP.NET, consulte los tutoriales de formularios Web Forms adicionales que se proporcionan en la sección [Introducción](../../../index.md) del sitio Web de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="e0af8-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="e0af8-160">El ASP.NET 4,5 proporcionado en esta serie de tutoriales incluye las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="e0af8-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="e0af8-161">Una interfaz de usuario sencilla para crear proyectos que ofrece [compatibilidad con muchos marcos de trabajo de ASP.net](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC y Web API).</span><span class="sxs-lookup"><span data-stu-id="e0af8-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="e0af8-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un diseño, la configuración y el marco de diseño con capacidad de respuesta.</span><span class="sxs-lookup"><span data-stu-id="e0af8-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="e0af8-163">[ASP.net Identity](../../../../identity/index.md), un nuevo sistema de pertenencia a ASP.net que funciona igual en todos los marcos de ASP.net y funciona con software de hospedaje web que no sea IIS.</span><span class="sxs-lookup"><span data-stu-id="e0af8-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="e0af8-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e0af8-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="e0af8-165">Una actualización del Entity Framework le permite:</span><span class="sxs-lookup"><span data-stu-id="e0af8-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="e0af8-166">Recuperar y manipular datos como objetos fuertemente tipados</span><span class="sxs-lookup"><span data-stu-id="e0af8-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="e0af8-167">Acceder a los datos de forma asincrónica</span><span class="sxs-lookup"><span data-stu-id="e0af8-167">Access data asynchronously</span></span>
  - <span data-ttu-id="e0af8-168">Control de errores de conexión transitorios</span><span class="sxs-lookup"><span data-stu-id="e0af8-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="e0af8-169">Instrucciones SQL de registro</span><span class="sxs-lookup"><span data-stu-id="e0af8-169">Log SQL statements</span></span>

<span data-ttu-id="e0af8-170">Para obtener una lista completa de características de ASP.NET 4,5, consulte [ASP.net and Web Tools para obtener Visual Studio 2013 notas de la versión](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="e0af8-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="e0af8-171">Aplicación de ejemplo Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="e0af8-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="e0af8-172">Las capturas de pantallas siguientes proceden de la aplicación de formularios Web Forms ASP.NET que se crea en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="e0af8-173">Al ejecutar la aplicación en Visual Studio, aparece la siguiente página principal Web.</span><span class="sxs-lookup"><span data-stu-id="e0af8-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Wingtip Toys: página predeterminada](introduction-and-overview/_static/image1.png)

<span data-ttu-id="e0af8-175">Puede registrarse como usuario nuevo o iniciar sesión como un usuario existente.</span><span class="sxs-lookup"><span data-stu-id="e0af8-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="e0af8-176">La navegación superior tiene vínculos a las categorías de producto y sus productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e0af8-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="e0af8-177">Si selecciona **productos**, se muestran todos los productos disponibles.</span><span class="sxs-lookup"><span data-stu-id="e0af8-177">If you select **Products**, all available products are displayed.</span></span> 

![Wingtip Toys-productos](introduction-and-overview/_static/image2.png)

<span data-ttu-id="e0af8-179">Si selecciona un producto específico, se mostrarán los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="e0af8-179">If you select a specific product, product details are displayed.</span></span>

![Wingtip Toys: detalles del producto](introduction-and-overview/_static/image3.png)

<span data-ttu-id="e0af8-181">Como usuario, puede registrarse e iniciar sesión con la funcionalidad predeterminada de la plantilla de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e0af8-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="e0af8-182">En este tutorial también se explica cómo iniciar sesión con una cuenta existente de gmail.</span><span class="sxs-lookup"><span data-stu-id="e0af8-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="e0af8-183">Además, puede iniciar sesión como administrador para agregar y quitar productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e0af8-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys: Inicio de sesión](introduction-and-overview/_static/image4.png)

<span data-ttu-id="e0af8-185">Una vez que haya iniciado sesión como usuario, puede Agregar productos al carro de la compra y desprotegerlos con PayPal.</span><span class="sxs-lookup"><span data-stu-id="e0af8-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="e0af8-186">La aplicación de ejemplo está diseñada para trabajar en el espacio aislado del desarrollador de PayPal.</span><span class="sxs-lookup"><span data-stu-id="e0af8-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="e0af8-187">No tiene lugar ninguna transacción Money real.</span><span class="sxs-lookup"><span data-stu-id="e0af8-187">No actual money transaction takes place.</span></span>

![Wingtip Toys: carro de la compra](introduction-and-overview/_static/image5.png)

<span data-ttu-id="e0af8-189">PayPal confirma la información de la cuenta, el pedido y el pago.</span><span class="sxs-lookup"><span data-stu-id="e0af8-189">PayPal confirms your account, order, and payment information.</span></span>

![Wingtip Toys-PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="e0af8-191">Después de volver de PayPal, puede revisar y completar el pedido.</span><span class="sxs-lookup"><span data-stu-id="e0af8-191">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys: revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="e0af8-193">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="e0af8-193">Prerequisites</span></span>

<span data-ttu-id="e0af8-194">Antes de empezar, asegúrese de que está instalado el siguiente software en el equipo:</span><span class="sxs-lookup"><span data-stu-id="e0af8-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="e0af8-195">[Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e0af8-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="e0af8-196">El .NET Framework se instala automáticamente.</span><span class="sxs-lookup"><span data-stu-id="e0af8-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="e0af8-197">En esta serie de tutoriales se usa Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="e0af8-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="e0af8-198">Puede usar o Microsoft Visual Studio 2017 para completar esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="e0af8-199">Tenga en cuenta lo siguiente sobre Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e0af8-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="e0af8-200">Microsoft Visual Studio 2017 y Microsoft Visual Studio Community 2017 se conocen como *Visual Studio* a lo largo de esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="e0af8-201">Visual Studio 2017 se instala junto a las versiones anteriores ya instaladas.</span><span class="sxs-lookup"><span data-stu-id="e0af8-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="e0af8-202">Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2017 y seguir abriendo en versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="e0af8-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="e0af8-203">La primera vez que inicia Visual Studio, se supone que ha seleccionado la configuración de *desarrollo web* .</span><span class="sxs-lookup"><span data-stu-id="e0af8-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="e0af8-204">Para obtener más información, consulte [Cómo: seleccionar la configuración del entorno de desarrollo web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="e0af8-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="e0af8-205">Después de instalar los requisitos previos, está listo para empezar a crear el proyecto web presentado en esta serie de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="e0af8-206">Descarga de la aplicación de ejemplo</span><span class="sxs-lookup"><span data-stu-id="e0af8-206">Download the sample application</span></span>

 <span data-ttu-id="e0af8-207">Puede descargar la aplicación de ejemplo completada en cualquier momento desde el sitio de ejemplos de MSDN:</span><span class="sxs-lookup"><span data-stu-id="e0af8-207">You can download the completed sample application at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="e0af8-208">[Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="e0af8-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="e0af8-209">Esta descarga tiene los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="e0af8-209">This download has the following items:</span></span>

- <span data-ttu-id="e0af8-210">La aplicación de ejemplo en la carpeta *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="e0af8-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="e0af8-211">Los recursos usados para crear la aplicación de ejemplo en la carpeta *WingtipToys-assets* de la carpeta *WingtipToys* .</span><span class="sxs-lookup"><span data-stu-id="e0af8-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="e0af8-212">La descarga es un archivo *. zip* .</span><span class="sxs-lookup"><span data-stu-id="e0af8-212">The download is a *.zip* file.</span></span> <span data-ttu-id="e0af8-213">Para ver el proyecto completado que crea esta serie de tutoriales, busque y *C#* seleccione la carpeta en el archivo. zip.</span><span class="sxs-lookup"><span data-stu-id="e0af8-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="e0af8-214">Guarde la C# carpeta en la carpeta que utiliza para trabajar con proyectos de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0af8-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="e0af8-215">De forma predeterminada, la carpeta proyectos de Visual Studio 2017 es:</span><span class="sxs-lookup"><span data-stu-id="e0af8-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="e0af8-216"><strong>C:\Users&#92;</strong>  <strong><em>&lt;nombredeusuario&gt;</em></strong> <strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="e0af8-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="e0af8-217">Cambie el nombre ***C#*** de la carpeta a ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="e0af8-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="e0af8-218">Si ya tiene una carpeta llamada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre de la carpeta existente antes de *C#* cambiar el nombre de la carpeta a *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="e0af8-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="e0af8-219">Para ejecutar el proyecto completado, abra la carpeta *WingtipToys* y haga doble clic en el archivo *WingtipToys. sln* .</span><span class="sxs-lookup"><span data-stu-id="e0af8-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="e0af8-220">Visual Studio 2017 abre el proyecto.</span><span class="sxs-lookup"><span data-stu-id="e0af8-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="e0af8-221">A continuación, haga clic con el botón secundario en el archivo *default. aspx* en **Explorador de soluciones** y seleccione **ver en el explorador**.</span><span class="sxs-lookup"><span data-stu-id="e0af8-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="e0af8-222">Tome una prueba de formularios Web Forms de ASP.NET para revisar el contenido</span><span class="sxs-lookup"><span data-stu-id="e0af8-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="e0af8-223">Después de completar la serie de tutoriales, realice una prueba para probar sus conocimientos y reforzar los conceptos clave.</span><span class="sxs-lookup"><span data-stu-id="e0af8-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="e0af8-224">Cada pregunta proporciona una explicación y vínculos a instrucciones adicionales.</span><span class="sxs-lookup"><span data-stu-id="e0af8-224">Each question provides an explanation and links to additional guidance.</span></span>

* [<span data-ttu-id="e0af8-225">Cuestionario de formularios Web Forms ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e0af8-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="e0af8-226">Comentarios y soporte técnico del tutorial</span><span class="sxs-lookup"><span data-stu-id="e0af8-226">Tutorial support and comments</span></span>

<span data-ttu-id="e0af8-227">Si tiene preguntas y comentarios, use las secciones p y a que se incluyen en la página de ejemplo [Introducción con ASP.net 4,5 Web Forms y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="e0af8-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="e0af8-228">Los comentarios de esta serie de tutoriales son bienvenidos.</span><span class="sxs-lookup"><span data-stu-id="e0af8-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="e0af8-229">Cuando se actualiza esta serie de tutoriales, se hace todo lo posible para tener en cuenta las correcciones o sugerencias para mejorar.</span><span class="sxs-lookup"><span data-stu-id="e0af8-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="e0af8-230">Si se produce un error, los mensajes de error correspondientes podrían ser confusos, sin ninguna explicación adecuada sobre cómo corregirlo.</span><span class="sxs-lookup"><span data-stu-id="e0af8-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="e0af8-231">Para obtener ayuda, puede consultar los [foros de ASP.net](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="e0af8-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="e0af8-232">Otra buena fuente es la sección preguntas y respuestas de la página de ejemplo [Introducción con los formularios Web de ASP.net 4,5 y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).</span><span class="sxs-lookup"><span data-stu-id="e0af8-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="e0af8-233">Siguiente</span><span class="sxs-lookup"><span data-stu-id="e0af8-233">Next</span></span>](create-the-project.md)
