---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: acceso a los modelos y a los datos | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 4 cubre los modelos y el acceso a los datos.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451021"
---
# <a name="part-4-models-and-data-access"></a><span data-ttu-id="b42a8-104">Parte 4: modelos y acceso a datos</span><span class="sxs-lookup"><span data-stu-id="b42a8-104">Part 4: Models and Data Access</span></span>

<span data-ttu-id="b42a8-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="b42a8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="b42a8-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="b42a8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="b42a8-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="b42a8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="b42a8-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="b42a8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="b42a8-109">La parte 4 cubre los modelos y el acceso a los datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-109">Part 4 covers Models and Data Access.</span></span>

<span data-ttu-id="b42a8-110">Hasta ahora, acabamos de pasar "datos ficticios" de nuestros controladores a nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="b42a8-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="b42a8-111">Ahora estamos preparados para enlazar una base de datos real.</span><span class="sxs-lookup"><span data-stu-id="b42a8-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="b42a8-112">En este tutorial, vamos a tratar cómo usar SQL Server Compact Edition (a menudo denominado SQL CE) como nuestro motor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="b42a8-113">SQL CE es una base de datos gratuita, incrustada y basada en archivos que no requiere ninguna instalación o configuración, lo que hace que sea realmente conveniente para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="b42a8-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="b42a8-114">Acceso a bases de datos con Entity Framework primero código</span><span class="sxs-lookup"><span data-stu-id="b42a8-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="b42a8-115">Usaremos la compatibilidad con Entity Framework (EF) que se incluye en los proyectos de ASP.NET MVC 3 para consultar y actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="b42a8-116">EF es una API de datos de asignación relacional de objetos (ORM) flexible que permite a los desarrolladores consultar y actualizar los datos almacenados en una base de datos de forma orientada a objetos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="b42a8-117">Entity Framework versión 4 admite un paradigma de desarrollo denominado primero código.</span><span class="sxs-lookup"><span data-stu-id="b42a8-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="b42a8-118">Code-First permite crear un objeto de modelo escribiendo clases simples (también conocidas como POCO a partir de objetos CLR "antiguos sin formato") y puede incluso crear la base de datos sobre la marcha desde las clases.</span><span class="sxs-lookup"><span data-stu-id="b42a8-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="b42a8-119">Cambios en las clases de modelo</span><span class="sxs-lookup"><span data-stu-id="b42a8-119">Changes to our Model Classes</span></span>

<span data-ttu-id="b42a8-120">En este tutorial se aprovechará la característica de creación de bases de datos en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b42a8-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="b42a8-121">Sin embargo, antes de hacerlo, vamos a realizar algunos cambios menores en las clases del modelo para agregar algunas cosas que usaremos más adelante.</span><span class="sxs-lookup"><span data-stu-id="b42a8-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="b42a8-122">Adición de las clases del modelo de intérprete</span><span class="sxs-lookup"><span data-stu-id="b42a8-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="b42a8-123">Nuestros álbumes se asociarán con artistas, por lo que agregaremos una clase de modelo simple para describir un artista.</span><span class="sxs-lookup"><span data-stu-id="b42a8-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="b42a8-124">Agregue una nueva clase a la carpeta models denominada Artist.cs con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="b42a8-125">Actualización de las clases del modelo</span><span class="sxs-lookup"><span data-stu-id="b42a8-125">Updating our Model Classes</span></span>

<span data-ttu-id="b42a8-126">Actualice la clase album como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="b42a8-127">A continuación, realice las siguientes actualizaciones en la clase Genre.</span><span class="sxs-lookup"><span data-stu-id="b42a8-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a><span data-ttu-id="b42a8-128">Adición de la carpeta de datos de\_de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b42a8-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="b42a8-129">Agregaremos una aplicación\_directorio de datos a nuestro proyecto para que contenga los archivos de base de datos de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b42a8-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="b42a8-130">Los datos de\_de la aplicación son un directorio especial en ASP.NET que ya tiene los permisos de acceso de seguridad correctos para el acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="b42a8-131">En el menú proyecto, seleccione Agregar carpeta ASP.NET y, a continuación,\_datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="b42a8-132">Crear una cadena de conexión en el archivo Web. config</span><span class="sxs-lookup"><span data-stu-id="b42a8-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="b42a8-133">Agregaremos algunas líneas al archivo de configuración del sitio web para que Entity Framework sepa cómo conectarse a nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="b42a8-134">Haga doble clic en el archivo Web. config que se encuentra en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b42a8-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="b42a8-135">Desplácese hasta la parte inferior de este archivo y agregue una sección &lt;connectionStrings&gt; directamente encima de la última línea, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="b42a8-136">Agregar una clase de contexto</span><span class="sxs-lookup"><span data-stu-id="b42a8-136">Adding a Context Class</span></span>

<span data-ttu-id="b42a8-137">Haga clic con el botón secundario en la carpeta modelos y agregue una nueva clase denominada MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="b42a8-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="b42a8-138">Esta clase representará el Entity Framework contexto de base de datos y controlará nuestras operaciones de creación, lectura, actualización y eliminación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="b42a8-139">A continuación se muestra el código de esta clase.</span><span class="sxs-lookup"><span data-stu-id="b42a8-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="b42a8-140">Es decir, no hay ninguna otra configuración, interfaces especiales, etc. Mediante la extensión de la clase base DbContext, nuestra clase MusicStoreEntities es capaz de controlar nuestras operaciones de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="b42a8-141">Ahora que hemos enlazado, vamos a agregar algunas propiedades más a las clases del modelo para aprovechar parte de la información adicional de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="b42a8-142">Agregar los datos del catálogo de tiendas</span><span class="sxs-lookup"><span data-stu-id="b42a8-142">Adding our store catalog data</span></span>

<span data-ttu-id="b42a8-143">Se aprovechará una característica de Entity Framework que agrega datos de "inicialización" a una base de datos recién creada.</span><span class="sxs-lookup"><span data-stu-id="b42a8-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="b42a8-144">Esto rellenará previamente el catálogo de tiendas con una lista de géneros, artistas y álbumes.</span><span class="sxs-lookup"><span data-stu-id="b42a8-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="b42a8-145">La descarga de MvcMusicStore-Assets. zip, que incluía los archivos de diseño del sitio usados anteriormente en este tutorial, tiene un archivo de clase con estos datos de inicialización, ubicado en una carpeta denominada Code.</span><span class="sxs-lookup"><span data-stu-id="b42a8-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="b42a8-146">Dentro de la carpeta Code/Models, busque el archivo SampleData.cs y colóquelo en la carpeta models en nuestro proyecto, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="b42a8-147">Ahora debemos agregar una línea de código para indicar Entity Framework sobre esa clase SampleData.</span><span class="sxs-lookup"><span data-stu-id="b42a8-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="b42a8-148">Haga doble clic en el archivo global. asax en la raíz del proyecto para abrirlo y agregue la siguiente línea al principio del método de inicio de la aplicación\_.</span><span class="sxs-lookup"><span data-stu-id="b42a8-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="b42a8-149">En este momento, hemos completado el trabajo necesario para configurar Entity Framework para nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="b42a8-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="b42a8-150">Consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="b42a8-150">Querying the Database</span></span>

<span data-ttu-id="b42a8-151">Ahora vamos a actualizar nuestro StoreController de modo que, en lugar de usar "datos ficticios", llame a nuestra base de datos para consultar toda su información.</span><span class="sxs-lookup"><span data-stu-id="b42a8-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="b42a8-152">Comenzaremos declarando un campo en el **StoreController** para que contenga una instancia de la clase MusicStoreEntities, denominada storeDB:</span><span class="sxs-lookup"><span data-stu-id="b42a8-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="b42a8-153">Actualizar el índice de almacén para consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="b42a8-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="b42a8-154">El Entity Framework mantiene la clase MusicStoreEntities y expone una propiedad de colección para cada tabla de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="b42a8-155">Vamos a actualizar la acción de índice de StoreController para recuperar todos los géneros de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="b42a8-156">Anteriormente lo hicimos mediante la codificación de datos de cadena.</span><span class="sxs-lookup"><span data-stu-id="b42a8-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="b42a8-157">Ahora podemos usar la Entity Framework colección generes context:</span><span class="sxs-lookup"><span data-stu-id="b42a8-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="b42a8-158">No es necesario realizar ningún cambio en nuestra plantilla de vista, ya que seguimos devolviendo el mismo StoreIndexViewModel que hemos devuelto antes de que solo se devuelvan datos activos de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="b42a8-159">Al ejecutar el proyecto de nuevo y visitar la dirección URL "/Store", ahora veremos una lista de todos los géneros de nuestra base de datos:</span><span class="sxs-lookup"><span data-stu-id="b42a8-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="b42a8-160">Actualización de la exploración y los detalles del almacén para usar datos activos</span><span class="sxs-lookup"><span data-stu-id="b42a8-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="b42a8-161">Con el método de acción/Store/Browse? Genre = *[some-Genre]* , buscamos un género por nombre.</span><span class="sxs-lookup"><span data-stu-id="b42a8-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="b42a8-162">Solo se espera un resultado, ya que nunca tenemos dos entradas para el mismo nombre de género y, por tanto, podemos usar. Extensión Single () en LINQ para consultar el objeto de género adecuado como este (no lo escriba todavía):</span><span class="sxs-lookup"><span data-stu-id="b42a8-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="b42a8-163">El método único toma una expresión lambda como parámetro, que especifica que se desea un objeto de género único, de modo que su nombre coincida con el valor que hemos definido.</span><span class="sxs-lookup"><span data-stu-id="b42a8-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="b42a8-164">En el caso anterior, vamos a cargar un objeto de género único con un valor de nombre que coincide con disco.</span><span class="sxs-lookup"><span data-stu-id="b42a8-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="b42a8-165">Sacaremos provecho de una característica Entity Framework que nos permite indicar otras entidades relacionadas que queremos cargar también cuando se recupera el objeto Genre.</span><span class="sxs-lookup"><span data-stu-id="b42a8-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="b42a8-166">Esta característica se denomina modelado de resultados de consultas y nos permite reducir el número de veces que necesitamos tener acceso a la base de datos para recuperar toda la información que necesitamos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="b42a8-167">Queremos realizar una captura previa de los álbumes para el género que recuperemos, por lo que actualizaremos la consulta para incluirla en los géneros. incluya ("álbumes") para indicar que también queremos álbumes relacionados.</span><span class="sxs-lookup"><span data-stu-id="b42a8-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="b42a8-168">Esto es más eficaz, ya que recuperará los datos del género y del álbum en una única solicitud de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="b42a8-169">Con las explicaciones fuera del camino, este es el aspecto de nuestra acción de controlador de exploración actualizada:</span><span class="sxs-lookup"><span data-stu-id="b42a8-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="b42a8-170">Ahora podemos actualizar la vista de exploración del almacén para mostrar los álbumes que están disponibles en cada género.</span><span class="sxs-lookup"><span data-stu-id="b42a8-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="b42a8-171">Abra la plantilla de vista (que se encuentra en/Views/Store/Browse.cshtml) y agregue una lista con viñetas de álbumes como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="b42a8-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="b42a8-172">La ejecución de la aplicación y la navegación a/Store/Browse? Genre = jazz muestra que los resultados se están extrayendo de la base de datos y muestran todos los álbumes del género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="b42a8-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="b42a8-173">Realizaremos el mismo cambio en la dirección URL de/Store/Details/[id] y reemplazaremos los datos ficticios por una consulta de base de datos que cargue un álbum cuyo identificador coincida con el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="b42a8-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="b42a8-174">La ejecución de la aplicación y el desplazamiento a/Store/Details/1 muestra que ahora se extraen los resultados de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b42a8-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="b42a8-175">Ahora que la página de detalles de la tienda está configurada para mostrar un álbum por el identificador del álbum, vamos a actualizar la vista **examinar** para vincularla a la vista detalles.</span><span class="sxs-lookup"><span data-stu-id="b42a8-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="b42a8-176">Usaremos HTML. ActionLink, exactamente como hicimos para vincular el índice de almacén al de la tienda al final de la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="b42a8-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="b42a8-177">A continuación se muestra el código fuente completo de la vista examinar.</span><span class="sxs-lookup"><span data-stu-id="b42a8-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="b42a8-178">Ahora podemos examinar desde nuestra página de la tienda hasta una página de género, en la que se enumeran los álbumes disponibles y haciendo clic en un álbum en el que se pueden ver los detalles de ese álbum.</span><span class="sxs-lookup"><span data-stu-id="b42a8-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b42a8-179">[Anterior](mvc-music-store-part-3.md)
> [Siguiente](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="b42a8-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
