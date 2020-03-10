---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Introducción al tutorial de NerdDinner | Microsoft Docs
author: shanselman
description: La mejor manera de aprender un nuevo marco es crear algo con él. En este tutorial se explica cómo crear una aplicación pequeña, pero completa, con ASP.NE...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 154cfe6694cf723c0a1f8e33bfdb42c97594518f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468937"
---
# <a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="ccf6b-104">Introducción al tutorial de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ccf6b-104">Introducing the NerdDinner Tutorial</span></span>

<span data-ttu-id="ccf6b-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="ccf6b-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="ccf6b-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="ccf6b-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="ccf6b-107">La mejor manera de aprender un nuevo marco es crear algo con él.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="ccf6b-108">En este tutorial se explica cómo crear una aplicación pequeña, pero completa, que use ASP.NET MVC 1, y se presentan algunos de los conceptos principales que hay detrás.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="ccf6b-109">Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="ccf6b-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-tutorial"></a><span data-ttu-id="ccf6b-110">Tutorial de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ccf6b-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="ccf6b-111">La mejor manera de aprender un nuevo marco es crear algo con él.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="ccf6b-112">En este tutorial se explica cómo crear una aplicación pequeña, pero completa, con ASP.NET MVC, y se presentan algunos de los conceptos principales que hay detrás.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="ccf6b-113">La aplicación que se va a compilar se denomina "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="ccf6b-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="ccf6b-114">NerdDinner proporciona a los usuarios una manera fácil de buscar y organizar las cenas en línea:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="ccf6b-115">NerdDinner permite a los usuarios registrados crear, editar y eliminar cenas.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="ccf6b-116">Aplica un conjunto coherente de reglas de validación y de negocios a través de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="ccf6b-117">Los visitantes pueden usar un mapa basado en AJAX para buscar las siguientes cenas que se mantienen cerca de ellas:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="ccf6b-118">Al hacer clic en una cena, se le llevará a una página de detalles donde podrá obtener más información al respecto:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="ccf6b-119">Si están interesados en asistir a la cena, pueden iniciar sesión o registrarse en el sitio:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="ccf6b-120">Después, pueden hacer clic en un vínculo RSVP basado en AJAX para asistir al evento:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="ccf6b-121">Implementación de NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ccf6b-121">Implementing NerdDinner</span></span>

<span data-ttu-id="ccf6b-122">Vamos a comenzar nuestra aplicación NerdDinner con el comando archivo-&gt;nuevo proyecto dentro de Visual Studio para crear un nuevo proyecto de ASP.NET MVC de marca.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="ccf6b-123">A continuación, agregaremos funcionalidades y características de forma incremental.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="ccf6b-124">A lo largo del proceso, trataremos:</span><span class="sxs-lookup"><span data-stu-id="ccf6b-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="ccf6b-125">Cómo crear un nuevo proyecto de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ccf6b-125">How to create a new ASP.NET MVC Project</span></span>](create-a-new-aspnet-mvc-project.md)
2. [<span data-ttu-id="ccf6b-126">Cómo crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="ccf6b-126">How to create a database</span></span>](create-a-database.md)
3. [<span data-ttu-id="ccf6b-127">Cómo crear un modelo con validaciones de reglas de negocios</span><span class="sxs-lookup"><span data-stu-id="ccf6b-127">How to build a model with business rule validations</span></span>](build-a-model-with-business-rule-validations.md)
4. [<span data-ttu-id="ccf6b-128">Cómo usar controladores y vistas para implementar una interfaz de usuario de lista/detalles</span><span class="sxs-lookup"><span data-stu-id="ccf6b-128">How to use controllers and views to implement a listing/details UI</span></span>](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
5. [<span data-ttu-id="ccf6b-129">Cómo proporcionar compatibilidad con la entrada de formulario de datos CRUD (creación, lectura, actualización y eliminación)</span><span class="sxs-lookup"><span data-stu-id="ccf6b-129">How to provide CRUD (create, read, update, delete) data form entry support</span></span>](provide-crud-create-read-update-delete-data-form-entry-support.md)
6. [<span data-ttu-id="ccf6b-130">Cómo usar ViewData e implementar clases ViewModel</span><span class="sxs-lookup"><span data-stu-id="ccf6b-130">How to use ViewData and implement ViewModel classes</span></span>](use-viewdata-and-implement-viewmodel-classes.md)
7. [<span data-ttu-id="ccf6b-131">Cómo volver a usar la interfaz de usuario con páginas maestras y parciales</span><span class="sxs-lookup"><span data-stu-id="ccf6b-131">How to re-use UI using master pages and partials</span></span>](re-use-ui-using-master-pages-and-partials.md)
8. [<span data-ttu-id="ccf6b-132">Cómo implementar la paginación de datos eficaz</span><span class="sxs-lookup"><span data-stu-id="ccf6b-132">How to implement efficient data paging</span></span>](implement-efficient-data-paging.md)
9. [<span data-ttu-id="ccf6b-133">Cómo proteger las aplicaciones mediante la autenticación y la autorización</span><span class="sxs-lookup"><span data-stu-id="ccf6b-133">How to secure applications using authentication and authorization</span></span>](secure-applications-using-authentication-and-authorization.md)
10. [<span data-ttu-id="ccf6b-134">Cómo usar AJAX para ofrecer actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="ccf6b-134">How to use AJAX to deliver dynamic updates</span></span>](use-ajax-to-deliver-dynamic-updates.md)
11. [<span data-ttu-id="ccf6b-135">Cómo usar AJAX para implementar escenarios de asignación</span><span class="sxs-lookup"><span data-stu-id="ccf6b-135">How to use AJAX to implement mapping scenarios</span></span>](use-ajax-to-implement-mapping-scenarios.md)
12. [<span data-ttu-id="ccf6b-136">Cómo habilitar las pruebas unitarias automatizadas</span><span class="sxs-lookup"><span data-stu-id="ccf6b-136">How to enable automated unit testing</span></span>](enable-automated-unit-testing.md)

<span data-ttu-id="ccf6b-137">Puede crear su propia copia de NerdDinner desde cero completando cada paso que se tutorial en este capítulo.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="ccf6b-138">Como alternativa, puede descargar una versión completada del código fuente aquí: [NerdDinner en github](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="ccf6b-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="ccf6b-139">También puede descargar opcionalmente [una versión en PDF gratuita de este tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) si desea leer el tutorial sin conexión.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="ccf6b-140">Puede usar Visual Studio 2008 o Visual Web Developer 2008 Express gratuito para compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="ccf6b-141">Puede usar SQL Server o el SQL Server Express gratuito para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="ccf6b-142">Puede instalar ASP.NET MVC, Visual Web Developer 2008 Express y SQL Server Express (todo gratis) mediante la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="ccf6b-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="ccf6b-143">Ahora vamos a empezar....</span><span class="sxs-lookup"><span data-stu-id="ccf6b-143">Now let's get started....</span></span>

<span data-ttu-id="ccf6b-144">Ahora que hemos analizado qué es NerdDinner, vamos a acumular nuestras mangas y a escribir código.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="ccf6b-145">Comenzaremos usando el archivo&gt;nuevo proyecto dentro de Visual Studio para crear la aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="ccf6b-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ccf6b-146">Siguiente</span><span class="sxs-lookup"><span data-stu-id="ccf6b-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
