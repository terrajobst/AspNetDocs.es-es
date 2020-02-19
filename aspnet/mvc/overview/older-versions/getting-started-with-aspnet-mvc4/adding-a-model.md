---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Agregar un modelo | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457809"
---
# <a name="adding-a-model"></a><span data-ttu-id="8d4b9-104">Agregar un modelo</span><span class="sxs-lookup"><span data-stu-id="8d4b9-104">Adding a Model</span></span>

<span data-ttu-id="8d4b9-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8d4b9-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="8d4b9-106">[Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="8d4b9-107">Es más seguro, mucho más fácil de seguir y demuestra más características.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="8d4b9-108">En esta sección, agregará algunas clases para administrar películas en una base de datos de.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8d4b9-109">Estas clases serán el modelo de &quot;&quot; parte de la aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="8d4b9-110">Usará una .NET Framework tecnología de acceso a datos conocida como [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir y trabajar con estas clases de modelo.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="8d4b9-111">El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="8d4b9-112">Code First permite crear objetos de modelo escribiendo clases simples.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="8d4b9-113">(También se conocen como clases POCO, desde &quot;objetos CLR antiguos sin formato.&quot;) Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="8d4b9-114">Agregar clases de modelo</span><span class="sxs-lookup"><span data-stu-id="8d4b9-114">Adding Model Classes</span></span>

<span data-ttu-id="8d4b9-115">En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="8d4b9-116">Escriba el nombre de *clase* &quot;película&quot;.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="8d4b9-117">Agregue las cinco propiedades siguientes a la clase `Movie`:</span><span class="sxs-lookup"><span data-stu-id="8d4b9-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="8d4b9-118">Usaremos la clase `Movie` para representar películas en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="8d4b9-119">Cada instancia de un objeto de `Movie` se corresponderá con una fila de una tabla de base de datos y cada propiedad de la clase `Movie` se asignará a una columna de la tabla.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="8d4b9-120">En el mismo archivo, agregue la siguiente `MovieDBContext` clase:</span><span class="sxs-lookup"><span data-stu-id="8d4b9-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="8d4b9-121">La clase `MovieDBContext` representa el contexto de la base de datos Entity Framework Movie, que controla la captura, el almacenamiento y la actualización de instancias de clase `Movie` en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="8d4b9-122">El `MovieDBContext` deriva de la clase base `DbContext` proporcionada por el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="8d4b9-123">Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente instrucción `using` en la parte superior del archivo:</span><span class="sxs-lookup"><span data-stu-id="8d4b9-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="8d4b9-124">A continuación se muestra el archivo *Movie.CS* completo.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="8d4b9-125">(Se han quitado varias instrucciones Using que no son necesarias).</span><span class="sxs-lookup"><span data-stu-id="8d4b9-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="8d4b9-126">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="8d4b9-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="8d4b9-127">La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="8d4b9-128">Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="8d4b9-129">Para ello, agregue la información de conexión en el archivo *Web. config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="8d4b9-130">Abra el archivo *Web. config* raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="8d4b9-131">(No el archivo *Web. config* en la carpeta *views* ). Abra el archivo *Web. config* que se describe en rojo.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="8d4b9-132">Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="8d4b9-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="8d4b9-133">En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="8d4b9-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="8d4b9-134">Esta pequeña cantidad de código y XML es todo lo que necesita escribir para representar y almacenar los datos de la película en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="8d4b9-135">A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.</span><span class="sxs-lookup"><span data-stu-id="8d4b9-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8d4b9-136">[Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="8d4b9-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
