---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Agregar un modelo (C#) | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a5f494eaa05bcfcd9d49873db728d71c1fd332c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434899"
---
# <a name="adding-a-model-c"></a><span data-ttu-id="c8dae-104">Agregar un modelo (C#)</span><span class="sxs-lookup"><span data-stu-id="c8dae-104">Adding a Model (C#)</span></span>

<span data-ttu-id="c8dae-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c8dae-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="c8dae-106">Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c8dae-106">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="c8dae-107">Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación.</span><span class="sxs-lookup"><span data-stu-id="c8dae-107">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="c8dae-108">Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="c8dae-108">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="c8dae-109">Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:</span><span class="sxs-lookup"><span data-stu-id="c8dae-109">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="c8dae-110">Requisitos previos de Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="c8dae-110">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="c8dae-111">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="c8dae-111">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="c8dae-112">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)</span><span class="sxs-lookup"><span data-stu-id="c8dae-112">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="c8dae-113">Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="c8dae-113">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="c8dae-114">Hay disponible un proyecto de Visual C# Web Developer con código fuente para acompañar este tema.</span><span class="sxs-lookup"><span data-stu-id="c8dae-114">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="c8dae-115">[Descargue la C# versión](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="c8dae-115">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="c8dae-116">Si prefiere Visual Basic, cambie a la [versión Visual Basic](../vb/adding-a-model.md) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c8dae-116">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/adding-a-model.md) of this tutorial.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="c8dae-117">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="c8dae-117">Adding a Model</span></span>

<span data-ttu-id="c8dae-118">En esta sección, agregará algunas clases para administrar películas en una base de datos de.</span><span class="sxs-lookup"><span data-stu-id="c8dae-118">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="c8dae-119">Estas clases serán la parte "modelo" de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c8dae-119">These classes will be the "model" part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="c8dae-120">Usará una .NET Framework tecnología de acceso a datos conocida como Entity Framework para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="c8dae-120">You'll use a .NET Framework data-access technology known as the Entity Framework to define and work with these model classes.</span></span> <span data-ttu-id="c8dae-121">El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*.</span><span class="sxs-lookup"><span data-stu-id="c8dae-121">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="c8dae-122">Code First permite crear objetos de modelo escribiendo clases simples.</span><span class="sxs-lookup"><span data-stu-id="c8dae-122">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="c8dae-123">(También se conocen como clases POCO, de "objetos CLR antiguos sin formato"). Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio.</span><span class="sxs-lookup"><span data-stu-id="c8dae-123">(These are also known as POCO classes, from "plain-old CLR objects.") You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="c8dae-124">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="c8dae-124">Adding Model Classes</span></span>

<span data-ttu-id="c8dae-125">En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="c8dae-125">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="c8dae-126">Asigne a la *clase* el nombre "Movie".</span><span class="sxs-lookup"><span data-stu-id="c8dae-126">Name the *class* "Movie".</span></span>

<span data-ttu-id="c8dae-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="c8dae-127">[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)</span></span>

<span data-ttu-id="c8dae-128">Agregue las cinco propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="c8dae-128">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="c8dae-129">Usaremos la clase `Movie` para representar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8dae-129">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="c8dae-130">Cada instancia de un objeto de `Movie` se corresponderá con una fila de una tabla de base de datos y cada propiedad de la clase `Movie` se asignará a una columna de la tabla.</span><span class="sxs-lookup"><span data-stu-id="c8dae-130">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="c8dae-131">En el mismo archivo, agregue la siguiente `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="c8dae-131">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="c8dae-132">La clase `MovieDBContext` representa el contexto de la base de datos Entity Framework Movie, que controla la captura, el almacenamiento y la actualización de instancias de clase `Movie` en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8dae-132">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="c8dae-133">El `MovieDBContext` deriva de la clase base `DbContext` proporcionada por el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c8dae-133">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span> <span data-ttu-id="c8dae-134">Para obtener más información sobre `DbContext` y `DbSet`, vea [mejoras de productividad para el Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="c8dae-134">For more information about `DbContext` and `DbSet`, see [Productivity Improvements for the Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).</span></span>

<span data-ttu-id="c8dae-135">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente instrucción `using` en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="c8dae-135">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="c8dae-136">A continuación se muestra el archivo *Movie.CS* completo.</span><span class="sxs-lookup"><span data-stu-id="c8dae-136">The complete *Movie.cs* file is shown below.</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a><span data-ttu-id="c8dae-137">Crear una cadena de conexión y trabajar con SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="c8dae-137">Creating a Connection String and Working with SQL Server Compact</span></span>

<span data-ttu-id="c8dae-138">La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8dae-138">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c8dae-139">Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará.</span><span class="sxs-lookup"><span data-stu-id="c8dae-139">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="c8dae-140">Para ello, agregue la información de conexión en el archivo *Web. config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8dae-140">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="c8dae-141">Abra el archivo *Web. config* raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c8dae-141">Open the application root *Web.config* file.</span></span> <span data-ttu-id="c8dae-142">(No el archivo *Web. config* en la carpeta *views* ). En la imagen siguiente se muestran los archivos *Web. config.* Abra el archivo *Web. config* en un círculo rojo.</span><span class="sxs-lookup"><span data-stu-id="c8dae-142">(Not the *Web.config* file in the *Views* folder.) The image below show both *Web.config* files; open the *Web.config* file circled in red.</span></span>

![](adding-a-model/_static/image4.png)

<span data-ttu-id="c8dae-143">Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="c8dae-143">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="c8dae-144">En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="c8dae-144">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

<span data-ttu-id="c8dae-145">Esta pequeña cantidad de código y XML es todo lo que necesita escribir para representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="c8dae-145">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="c8dae-146">A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.</span><span class="sxs-lookup"><span data-stu-id="c8dae-146">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c8dae-147">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="c8dae-147">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
