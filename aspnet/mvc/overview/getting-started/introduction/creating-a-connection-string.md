---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Crear una cadena de conexión y trabajar con SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 20781ad760d3a0e4559ec4c7e18528f3686dcc02
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456522"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="3ca89-102">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="3ca89-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="3ca89-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3ca89-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="3ca89-104">Crear una cadena de conexión y trabajar con SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="3ca89-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="3ca89-105">La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3ca89-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="3ca89-106">Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará.</span><span class="sxs-lookup"><span data-stu-id="3ca89-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="3ca89-107">En realidad, no tiene que especificar la base de datos que se va a usar, Entity Framework usará de forma predeterminada [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="3ca89-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="3ca89-108">En esta sección, agregaremos explícitamente una cadena de conexión en el archivo *Web. config* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ca89-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="3ca89-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="3ca89-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="3ca89-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) es una versión ligera de la SQL Server Express motor de base de datos que se inicia a petición y se ejecuta en modo de usuario.</span><span class="sxs-lookup"><span data-stu-id="3ca89-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="3ca89-111">LocalDB se ejecuta en un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como archivos *. MDF* .</span><span class="sxs-lookup"><span data-stu-id="3ca89-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="3ca89-112">Normalmente, los archivos de base de datos LocalDB se guardan en la carpeta *App\_Data* de un proyecto Web.</span><span class="sxs-lookup"><span data-stu-id="3ca89-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="3ca89-113">No se recomienda el uso de SQL Server Express en aplicaciones Web de producción.</span><span class="sxs-lookup"><span data-stu-id="3ca89-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="3ca89-114">En concreto, LocalDB no debe usarse para producción con una aplicación web porque no está diseñado para funcionar con IIS.</span><span class="sxs-lookup"><span data-stu-id="3ca89-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="3ca89-115">Sin embargo, una base de datos de LocalDB se puede migrar fácilmente a SQL Server o SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="3ca89-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="3ca89-116">En Visual Studio 2017, LocalDB se instala de forma predeterminada con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ca89-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="3ca89-117">De forma predeterminada, la Entity Framework busca una cadena de conexión denominada igual que la clase de contexto del objeto (`MovieDBContext` para este proyecto).</span><span class="sxs-lookup"><span data-stu-id="3ca89-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="3ca89-118">Para obtener más información, vea [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ca89-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="3ca89-119">Abra el archivo *Web. config* raíz de la aplicación que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3ca89-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="3ca89-120">(No el archivo *Web. config* en la carpeta *views* ).</span><span class="sxs-lookup"><span data-stu-id="3ca89-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="3ca89-121">Busque el elemento `<connectionStrings>`:</span><span class="sxs-lookup"><span data-stu-id="3ca89-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="3ca89-122">Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="3ca89-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="3ca89-123">En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:</span><span class="sxs-lookup"><span data-stu-id="3ca89-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="3ca89-124">Las dos cadenas de conexión son muy similares.</span><span class="sxs-lookup"><span data-stu-id="3ca89-124">The two connection strings are very similar.</span></span> <span data-ttu-id="3ca89-125">La primera cadena de conexión se denomina `DefaultConnection` y se utiliza para que la base de datos de pertenencia controle quién puede tener acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3ca89-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="3ca89-126">La cadena de conexión que ha agregado especifica una base de datos LocalDB denominada *Movie. MDF* ubicada en la carpeta *App\_Data* .</span><span class="sxs-lookup"><span data-stu-id="3ca89-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="3ca89-127">No usaremos la base de datos de pertenencia en este tutorial. para obtener más información sobre la pertenencia, la autenticación y la seguridad, consulte mi tutorial [creación de una aplicación de ASP.NET MVC con autenticación y base de datos SQL e implementación en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="3ca89-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="3ca89-128">El nombre de la cadena de conexión debe coincidir con el nombre de la clase [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .</span><span class="sxs-lookup"><span data-stu-id="3ca89-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="3ca89-129">En realidad, no es necesario agregar la cadena de conexión de `MovieDBContext`.</span><span class="sxs-lookup"><span data-stu-id="3ca89-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="3ca89-130">Si no especifica una cadena de conexión, Entity Framework creará una base de datos LocalDB en el directorio usuarios con el nombre completo de la clase [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (en este caso `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="3ca89-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="3ca89-131">Puede asignar a la base de datos el nombre que desee, siempre y cuando tenga el *. Sufijo MDF* .</span><span class="sxs-lookup"><span data-stu-id="3ca89-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="3ca89-132">Por ejemplo, podríamos asignar un nombre a la base de datos *. MDF*.</span><span class="sxs-lookup"><span data-stu-id="3ca89-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="3ca89-133">A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.</span><span class="sxs-lookup"><span data-stu-id="3ca89-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3ca89-134">[Anterior](adding-a-model.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="3ca89-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
