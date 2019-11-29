---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: creación de un controlador de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600039"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="b01aa-102">Parte 3: creación de un controlador de administración</span><span class="sxs-lookup"><span data-stu-id="b01aa-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="b01aa-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b01aa-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b01aa-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="b01aa-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="b01aa-105">Agregar un controlador de administración</span><span class="sxs-lookup"><span data-stu-id="b01aa-105">Add an Admin Controller</span></span>

<span data-ttu-id="b01aa-106">En esta sección, agregaremos un controlador de API Web que admita operaciones CRUD (creación, lectura, actualización y eliminación) en productos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="b01aa-107">El controlador usará Entity Framework para comunicarse con el nivel de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="b01aa-108">Solo los administradores podrán usar este controlador.</span><span class="sxs-lookup"><span data-stu-id="b01aa-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="b01aa-109">Los clientes tendrán acceso a los productos a través de otro controlador.</span><span class="sxs-lookup"><span data-stu-id="b01aa-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="b01aa-110">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="b01aa-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b01aa-111">Seleccione **Agregar** y después **controlador**.</span><span class="sxs-lookup"><span data-stu-id="b01aa-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="b01aa-112">En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="b01aa-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="b01aa-113">En **plantilla**, seleccione &quot;controlador de API con acciones de lectura y escritura con Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="b01aa-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="b01aa-114">En **clase de modelo**, seleccione "Product (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="b01aa-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="b01aa-115">En **contexto de datos**, seleccione "&lt;nuevo contexto de datos&gt;".</span><span class="sxs-lookup"><span data-stu-id="b01aa-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="b01aa-116">Si la lista desplegable **clase de modelo** no muestra ninguna clase de modelo, asegúrese de que compiló el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b01aa-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="b01aa-117">Entity Framework usa la reflexión, por lo que necesita el ensamblado compilado.</span><span class="sxs-lookup"><span data-stu-id="b01aa-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="b01aa-118">Al seleccionar "&lt;nuevo contexto de datos&gt;", se abrirá el cuadro de diálogo **nuevo contexto de datos** .</span><span class="sxs-lookup"><span data-stu-id="b01aa-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="b01aa-119">Asigne un nombre al `ProductStore.Models.OrdersContext`de contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="b01aa-120">Haga clic en **Aceptar** para descartar el cuadro de diálogo **nuevo contexto de datos** .</span><span class="sxs-lookup"><span data-stu-id="b01aa-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="b01aa-121">En el cuadro de diálogo **Agregar controlador** , haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="b01aa-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="b01aa-122">Esto es lo que se ha agregado al proyecto:</span><span class="sxs-lookup"><span data-stu-id="b01aa-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="b01aa-123">Una clase denominada `OrdersContext` que se deriva de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="b01aa-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="b01aa-124">Esta clase proporciona el pegado entre los modelos POCO y la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="b01aa-125">Un controlador de API Web denominado `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="b01aa-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="b01aa-126">Este controlador admite operaciones CRUD en instancias de `Product`.</span><span class="sxs-lookup"><span data-stu-id="b01aa-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="b01aa-127">Usa la clase `OrdersContext` para comunicarse con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b01aa-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="b01aa-128">Una nueva cadena de conexión de base de datos en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="b01aa-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="b01aa-129">Abra el archivo OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="b01aa-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="b01aa-130">Observe que el constructor especifica el nombre de la cadena de conexión de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="b01aa-131">Este nombre hace referencia a la cadena de conexión que se ha agregado a Web. config.</span><span class="sxs-lookup"><span data-stu-id="b01aa-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="b01aa-132">Agregue las propiedades siguientes a la clase `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="b01aa-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="b01aa-133">Un **DbSet** representa un conjunto de entidades que se pueden consultar.</span><span class="sxs-lookup"><span data-stu-id="b01aa-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="b01aa-134">Esta es la lista completa de la clase `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="b01aa-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="b01aa-135">La clase `AdminController` define cinco métodos que implementan la funcionalidad CRUD básica.</span><span class="sxs-lookup"><span data-stu-id="b01aa-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="b01aa-136">Cada método corresponde a un URI que el cliente puede invocar:</span><span class="sxs-lookup"><span data-stu-id="b01aa-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="b01aa-137">Método de controlador</span><span class="sxs-lookup"><span data-stu-id="b01aa-137">Controller Method</span></span> | <span data-ttu-id="b01aa-138">Descripción</span><span class="sxs-lookup"><span data-stu-id="b01aa-138">Description</span></span> | <span data-ttu-id="b01aa-139">URI</span><span class="sxs-lookup"><span data-stu-id="b01aa-139">URI</span></span> | <span data-ttu-id="b01aa-140">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="b01aa-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b01aa-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="b01aa-141">GetProducts</span></span> | <span data-ttu-id="b01aa-142">Obtiene todos los productos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-142">Gets all products.</span></span> | <span data-ttu-id="b01aa-143">API/productos</span><span class="sxs-lookup"><span data-stu-id="b01aa-143">api/products</span></span> | <span data-ttu-id="b01aa-144">GET</span><span class="sxs-lookup"><span data-stu-id="b01aa-144">GET</span></span> |
| <span data-ttu-id="b01aa-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="b01aa-145">GetProduct</span></span> | <span data-ttu-id="b01aa-146">Busca un producto por identificador.</span><span class="sxs-lookup"><span data-stu-id="b01aa-146">Finds a product by ID.</span></span> | <span data-ttu-id="b01aa-147">API/productos/*ID.*</span><span class="sxs-lookup"><span data-stu-id="b01aa-147">api/products/*id*</span></span> | <span data-ttu-id="b01aa-148">GET</span><span class="sxs-lookup"><span data-stu-id="b01aa-148">GET</span></span> |
| <span data-ttu-id="b01aa-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="b01aa-149">PutProduct</span></span> | <span data-ttu-id="b01aa-150">Actualiza un producto.</span><span class="sxs-lookup"><span data-stu-id="b01aa-150">Updates a product.</span></span> | <span data-ttu-id="b01aa-151">API/productos/*ID.*</span><span class="sxs-lookup"><span data-stu-id="b01aa-151">api/products/*id*</span></span> | <span data-ttu-id="b01aa-152">PUT</span><span class="sxs-lookup"><span data-stu-id="b01aa-152">PUT</span></span> |
| <span data-ttu-id="b01aa-153">Postproducto</span><span class="sxs-lookup"><span data-stu-id="b01aa-153">PostProduct</span></span> | <span data-ttu-id="b01aa-154">Crea un nuevo producto.</span><span class="sxs-lookup"><span data-stu-id="b01aa-154">Creates a new product.</span></span> | <span data-ttu-id="b01aa-155">API/productos</span><span class="sxs-lookup"><span data-stu-id="b01aa-155">api/products</span></span> | <span data-ttu-id="b01aa-156">Exponer</span><span class="sxs-lookup"><span data-stu-id="b01aa-156">POST</span></span> |
| <span data-ttu-id="b01aa-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="b01aa-157">DeleteProduct</span></span> | <span data-ttu-id="b01aa-158">Elimina un producto.</span><span class="sxs-lookup"><span data-stu-id="b01aa-158">Deletes a product.</span></span> | <span data-ttu-id="b01aa-159">API/productos/*ID.*</span><span class="sxs-lookup"><span data-stu-id="b01aa-159">api/products/*id*</span></span> | <span data-ttu-id="b01aa-160">SUPRIMIR</span><span class="sxs-lookup"><span data-stu-id="b01aa-160">DELETE</span></span> |

<span data-ttu-id="b01aa-161">Cada método llama a `OrdersContext` para consultar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="b01aa-162">Los métodos que modifican la colección (PUT, POST y DELETE) llaman a `db.SaveChanges` para conservar los cambios en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="b01aa-163">Los controladores se crean por solicitud HTTP y, a continuación, se eliminan, por lo que es necesario conservar los cambios antes de que se devuelva un método.</span><span class="sxs-lookup"><span data-stu-id="b01aa-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="b01aa-164">Agregar un inicializador de base de datos</span><span class="sxs-lookup"><span data-stu-id="b01aa-164">Add a Database Initializer</span></span>

<span data-ttu-id="b01aa-165">Entity Framework tiene una característica agradable que le permite rellenar la base de datos al iniciarse y volver a crear automáticamente la base de datos cada vez que cambien los modelos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="b01aa-166">Esta característica es útil durante el desarrollo, porque siempre tiene algunos datos de prueba, aunque cambie los modelos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="b01aa-167">En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos y cree una nueva clase denominada `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="b01aa-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="b01aa-168">Pegue la siguiente implementación:</span><span class="sxs-lookup"><span data-stu-id="b01aa-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="b01aa-169">Al heredar de la clase **DropCreateDatabaseIfModelChanges** , estamos indicando Entity Framework a quitar la base de datos siempre que se modifiquen las clases del modelo.</span><span class="sxs-lookup"><span data-stu-id="b01aa-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="b01aa-170">Cuando Entity Framework crea (o vuelve a crear) la base de datos, llama al método de **inicialización** para rellenar las tablas.</span><span class="sxs-lookup"><span data-stu-id="b01aa-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="b01aa-171">Usamos el método **SEED** para agregar algunos productos de ejemplo más un orden de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b01aa-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="b01aa-172">Esta característica es ideal para las pruebas, pero no se usa la clase **DropCreateDatabaseIfModelChanges** en producción,, porque se podrían perder los datos si alguien cambia una clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="b01aa-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="b01aa-173">A continuación, abra global. asax y agregue el código siguiente al método de **Inicio de\_** de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b01aa-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="b01aa-174">Enviar una solicitud al controlador</span><span class="sxs-lookup"><span data-stu-id="b01aa-174">Send a Request to the Controller</span></span>

<span data-ttu-id="b01aa-175">En este momento, no hemos escrito ningún código de cliente, pero puede invocar la API Web mediante un explorador Web o una herramienta de depuración HTTP como [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="b01aa-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="b01aa-176">En Visual Studio, presione F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="b01aa-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="b01aa-177">El explorador Web se abrirá en `http://localhost:*portnum*/`, donde *portnum* es un número de puerto.</span><span class="sxs-lookup"><span data-stu-id="b01aa-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="b01aa-178">Envíe una solicitud HTTP a "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="b01aa-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="b01aa-179">Es posible que la primera solicitud tarde en completarse, porque Entity Framework necesita crear e inicializar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b01aa-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="b01aa-180">La respuesta debe ser similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="b01aa-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="b01aa-181">[Anterior](using-web-api-with-entity-framework-part-2.md)
> [Siguiente](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="b01aa-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
