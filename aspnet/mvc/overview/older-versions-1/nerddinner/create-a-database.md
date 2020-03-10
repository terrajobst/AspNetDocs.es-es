---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creacíón de una base de datos | Microsoft Docs
author: microsoft
description: En el paso 2 se muestran los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469303"
---
# <a name="create-a-database"></a><span data-ttu-id="ca287-103">Crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="ca287-103">Create a Database</span></span>

<span data-ttu-id="ca287-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ca287-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ca287-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="ca287-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="ca287-106">Este es el paso 2 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="ca287-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="ca287-107">En el paso 2 se muestran los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="ca287-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="ca287-108">Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="ca287-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="ca287-109">NerdDinner paso 2: crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="ca287-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="ca287-110">Usaremos una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="ca287-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="ca287-111">En los pasos siguientes se muestra cómo crear la base de datos con la edición gratuita de SQL Server Express (que puede instalar fácilmente mediante la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="ca287-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="ca287-112">Todo el código que escribiremos funciona con SQL Server Express y con el SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="ca287-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="ca287-113">Crear una nueva base de datos de SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="ca287-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="ca287-114">Comenzaremos haciendo clic con el botón derecho en nuestro proyecto web y, a continuación, seleccionaremos el comando de menú **agregar&gt;nuevo elemento** :</span><span class="sxs-lookup"><span data-stu-id="ca287-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="ca287-115">Se abrirá el cuadro de diálogo "Agregar nuevo elemento" de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ca287-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="ca287-116">Filtraremos por la categoría "Data" (datos) y seleccionaremos la plantilla de elemento "SQL Server Database":</span><span class="sxs-lookup"><span data-stu-id="ca287-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="ca287-117">Asignaremos el nombre "NerdDinner. MDF" y aceptaremos la base de datos de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="ca287-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="ca287-118">Visual Studio nos preguntará si queremos agregar este archivo al directorio de datos de \App\_(que es un directorio ya configurado con las ACL de seguridad de lectura y escritura):</span><span class="sxs-lookup"><span data-stu-id="ca287-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="ca287-119">Haremos clic en "sí" y la nueva base de datos se creará y se agregará a nuestro Explorador de soluciones:</span><span class="sxs-lookup"><span data-stu-id="ca287-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="ca287-120">Crear tablas dentro de nuestra base de datos</span><span class="sxs-lookup"><span data-stu-id="ca287-120">Creating Tables within our Database</span></span>

<span data-ttu-id="ca287-121">Ahora tenemos una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="ca287-121">We now have a new empty database.</span></span> <span data-ttu-id="ca287-122">Vamos a agregarle algunas tablas.</span><span class="sxs-lookup"><span data-stu-id="ca287-122">Let's add some tables to it.</span></span>

<span data-ttu-id="ca287-123">Para ello, desplazaremos a la ventana de pestaña "Explorador de servidores" dentro de Visual Studio, lo que nos permite administrar las bases de datos y los servidores.</span><span class="sxs-lookup"><span data-stu-id="ca287-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="ca287-124">SQL Server Express las bases de datos almacenadas en la carpeta \App\_data de nuestra aplicación se mostrarán automáticamente en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="ca287-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="ca287-125">Opcionalmente, puede usar el icono "conectar con base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (tanto locales como remotas) a la lista también:</span><span class="sxs-lookup"><span data-stu-id="ca287-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="ca287-126">Agregaremos dos tablas a nuestra base de datos NerdDinner: una para almacenar nuestras cenas y la otra para realizar el seguimiento de las aceptaciones RSVP.</span><span class="sxs-lookup"><span data-stu-id="ca287-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="ca287-127">Podemos crear nuevas tablas haciendo clic con el botón derecho en la carpeta "tablas" de nuestra base de datos y eligiendo el comando de menú "Agregar nueva tabla":</span><span class="sxs-lookup"><span data-stu-id="ca287-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="ca287-128">Se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla.</span><span class="sxs-lookup"><span data-stu-id="ca287-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="ca287-129">En nuestra tabla "cenas", agregaremos 10 columnas de datos:</span><span class="sxs-lookup"><span data-stu-id="ca287-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="ca287-130">Queremos que la columna "DinnerID" sea una clave principal única para la tabla.</span><span class="sxs-lookup"><span data-stu-id="ca287-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="ca287-131">Para configurarlo, haga clic con el botón derecho en la columna "DinnerID" y elija el elemento de menú "establecer clave principal":</span><span class="sxs-lookup"><span data-stu-id="ca287-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="ca287-132">Además de convertir DinnerID en una clave principal, también queremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente a medida que se agregan nuevas filas de datos a la tabla (lo que significa que la primera fila de cena insertada tendrá un DinnerID de 1, la segunda fila insertada). tendrá un DinnerID de 2, etc.).</span><span class="sxs-lookup"><span data-stu-id="ca287-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="ca287-133">Para ello, seleccione la columna "DinnerID" y, a continuación, use el editor de "propiedades de columna" para establecer la propiedad "(is Identity)" en la columna en "Yes".</span><span class="sxs-lookup"><span data-stu-id="ca287-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="ca287-134">Usaremos los valores predeterminados de la identidad estándar (empieza en 1 y el incremento 1 en cada fila nueva cena):</span><span class="sxs-lookup"><span data-stu-id="ca287-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="ca287-135">Después, guardaremos la tabla escribiendo Ctrl-S o usando el comando **de menú guardar archivo&gt;** .</span><span class="sxs-lookup"><span data-stu-id="ca287-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="ca287-136">Esto le pedirá que asigne un nombre a la tabla.</span><span class="sxs-lookup"><span data-stu-id="ca287-136">This will prompt us to name the table.</span></span> <span data-ttu-id="ca287-137">Lo denominaremos "cenas":</span><span class="sxs-lookup"><span data-stu-id="ca287-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="ca287-138">La nueva tabla de cenas se mostrará en la base de datos en el explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="ca287-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="ca287-139">A continuación, repetiremos los pasos anteriores y crearemos una tabla "RSVP".</span><span class="sxs-lookup"><span data-stu-id="ca287-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="ca287-140">Esta tabla contiene tres columnas.</span><span class="sxs-lookup"><span data-stu-id="ca287-140">This table with have 3 columns.</span></span> <span data-ttu-id="ca287-141">La columna RsvpID se configurará como la clave principal y también se convertirá en una columna de identidad:</span><span class="sxs-lookup"><span data-stu-id="ca287-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="ca287-142">Lo guardaremos y le asignaremos el nombre "RSVP".</span><span class="sxs-lookup"><span data-stu-id="ca287-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="ca287-143">Configurar una relación de clave externa entre tablas</span><span class="sxs-lookup"><span data-stu-id="ca287-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="ca287-144">Ahora tenemos dos tablas en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="ca287-144">We now have two tables within our database.</span></span> <span data-ttu-id="ca287-145">Nuestro último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas, de modo que podamos asociar cada fila de cena con cero o más filas RSVP que se apliquen a ella.</span><span class="sxs-lookup"><span data-stu-id="ca287-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="ca287-146">Para ello, se configura la columna "DinnerID" de la tabla RSVP para que tenga una relación de clave externa con la columna "DinnerID" de la tabla "cenas".</span><span class="sxs-lookup"><span data-stu-id="ca287-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="ca287-147">Para ello, abriremos la tabla RSVP en el diseñador de tablas haciendo doble clic en ella en el explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="ca287-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="ca287-148">A continuación, seleccionaremos la columna "DinnerID" que contiene, hacemos clic con el botón derecho y elegiremos "relaciones..." comando del menú contextual:</span><span class="sxs-lookup"><span data-stu-id="ca287-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationships…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="ca287-149">Se abrirá un cuadro de diálogo que se puede usar para configurar las relaciones entre las tablas:</span><span class="sxs-lookup"><span data-stu-id="ca287-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="ca287-150">Haremos clic en el botón "agregar" para agregar una nueva relación al cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="ca287-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="ca287-151">Una vez que se ha agregado una relación, expandiremos el nodo de vista de árbol de las tablas y las especificaciones de columna en la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, hacer clic en el botón "..." situado a su derecha:</span><span class="sxs-lookup"><span data-stu-id="ca287-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="ca287-152">Al hacer clic en "..." el botón abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas participan en la relación, así como permitirnos asignar un nombre a la relación.</span><span class="sxs-lookup"><span data-stu-id="ca287-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="ca287-153">Cambiaremos la tabla de la clave principal a "cenas" y seleccionaremos la columna "DinnerID" dentro de la tabla cenas como clave principal.</span><span class="sxs-lookup"><span data-stu-id="ca287-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="ca287-154">La tabla RSVP será la tabla de clave externa y la RSVP. La columna DinnerID se asociará como clave externa:</span><span class="sxs-lookup"><span data-stu-id="ca287-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="ca287-155">Ahora, cada fila de la tabla RSVP se asociará a una fila de la tabla cena.</span><span class="sxs-lookup"><span data-stu-id="ca287-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="ca287-156">SQL Server mantendrá la integridad referencial para nosotros, y nos impedimos agregar una nueva fila RSVP si no señala a una fila cena válida.</span><span class="sxs-lookup"><span data-stu-id="ca287-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="ca287-157">También nos impedirá eliminar una fila de cena si todavía hay filas RSVP que hacen referencia a ella.</span><span class="sxs-lookup"><span data-stu-id="ca287-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="ca287-158">Agregar datos a las tablas</span><span class="sxs-lookup"><span data-stu-id="ca287-158">Adding Data to our Tables</span></span>

<span data-ttu-id="ca287-159">Vamos a terminar agregando algunos datos de ejemplo a nuestra tabla de cenas.</span><span class="sxs-lookup"><span data-stu-id="ca287-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="ca287-160">Podemos agregar datos a una tabla haciendo clic con el botón derecho en él en el Explorador de servidores y eligiendo el comando "Mostrar datos de tabla":</span><span class="sxs-lookup"><span data-stu-id="ca287-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="ca287-161">Agregaremos algunas filas de datos de cenas que podremos usar más adelante a medida que comenzamos a implementar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ca287-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="ca287-162">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="ca287-162">Next Step</span></span>

<span data-ttu-id="ca287-163">Hemos terminado de crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ca287-163">We've finished creating our database.</span></span> <span data-ttu-id="ca287-164">Vamos a crear ahora clases de modelo que se pueden usar para realizar consultas y actualizarlas.</span><span class="sxs-lookup"><span data-stu-id="ca287-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca287-165">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="ca287-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
