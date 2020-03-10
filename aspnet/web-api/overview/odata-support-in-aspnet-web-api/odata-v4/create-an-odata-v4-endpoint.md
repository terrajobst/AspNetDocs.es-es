---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Cree un punto de conexión de OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de consultar y manipular conjuntos de datos a través de operaciones CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484501"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="ee2f8-104">Cree un punto de conexión de OData V4 mediante ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ee2f8-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="ee2f8-105">El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="ee2f8-106">OData proporciona una manera uniforme de consultar y manipular conjuntos de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="ee2f8-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="ee2f8-107">ASP.NET Web API admite V3 y V4 del protocolo.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="ee2f8-108">Incluso puede tener un punto de conexión V4 que se ejecute en paralelo con un punto de conexión V3.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="ee2f8-109">En este tutorial se muestra cómo crear un punto de conexión de OData V4 que admita operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ee2f8-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="ee2f8-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="ee2f8-111">API Web 5,2</span><span class="sxs-lookup"><span data-stu-id="ee2f8-111">Web API 5.2</span></span>
> - <span data-ttu-id="ee2f8-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="ee2f8-112">OData v4</span></span>
> - <span data-ttu-id="ee2f8-113">Visual Studio 2017 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="ee2f8-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="ee2f8-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ee2f8-114">Entity Framework 6</span></span>
> - <span data-ttu-id="ee2f8-115">4\.7.2 .NET</span><span class="sxs-lookup"><span data-stu-id="ee2f8-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="ee2f8-116">Versiones del tutorial</span><span class="sxs-lookup"><span data-stu-id="ee2f8-116">Tutorial versions</span></span>
>
> <span data-ttu-id="ee2f8-117">Para la versión 3 de OData, consulte [creación de un punto de conexión de oData V3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="ee2f8-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="ee2f8-118">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee2f8-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="ee2f8-119">En Visual Studio, en el menú **archivo** , seleccione **nuevo** **proyecto**de &gt;.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="ee2f8-120">Expanda **instalado** &gt;  **C# visual** &gt; **Web**y seleccione la plantilla **aplicación Web de ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="ee2f8-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="ee2f8-121">Asigne al proyecto el nombre &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="ee2f8-122">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="ee2f8-123">Seleccione la plantilla **Vacía**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-123">Select the **Empty** template.</span></span> <span data-ttu-id="ee2f8-124">En **Agregar carpetas y referencias principales para:** , seleccione **Web API**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="ee2f8-125">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="ee2f8-126">Instalar los paquetes de OData</span><span class="sxs-lookup"><span data-stu-id="ee2f8-126">Install the OData packages</span></span>

<span data-ttu-id="ee2f8-127">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="ee2f8-128">En la ventana de la consola del administrador de paquetes, escriba:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="ee2f8-129">Este comando instala los paquetes de NuGet de OData más recientes.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="ee2f8-130">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="ee2f8-130">Add a model class</span></span>

<span data-ttu-id="ee2f8-131">Un *modelo* es un objeto que representa una entidad de datos en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="ee2f8-132">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="ee2f8-133">En el menú contextual, seleccione **agregar** &gt; **clase**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="ee2f8-134">Por Convención, las clases de modelo se colocan en la carpeta models, pero no es necesario seguir esta Convención en sus propios proyectos.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="ee2f8-135">Asigne a la clase el nombre `Product`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-135">Name the class `Product`.</span></span> <span data-ttu-id="ee2f8-136">En el archivo Product.cs, reemplace el código reutilizable por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="ee2f8-137">La propiedad `Id` es la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="ee2f8-138">Los clientes pueden consultar entidades por clave.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-138">Clients can query entities by key.</span></span> <span data-ttu-id="ee2f8-139">Por ejemplo, para obtener el producto con el identificador 5, el URI es `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="ee2f8-140">La propiedad `Id` también será la clave principal en la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="ee2f8-141">Habilitar Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ee2f8-141">Enable Entity Framework</span></span>

<span data-ttu-id="ee2f8-142">En este tutorial, usaremos el Code First de Entity Framework (EF) para crear la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="ee2f8-143">OData de Web API no requiere EF.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-143">Web API OData does not require EF.</span></span> <span data-ttu-id="ee2f8-144">Use cualquier capa de acceso a datos que pueda traducir entidades de base de datos en modelos.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="ee2f8-145">En primer lugar, instale el paquete NuGet para EF.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="ee2f8-146">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="ee2f8-147">En la ventana de la consola del administrador de paquetes, escriba:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="ee2f8-148">Abra el archivo Web. config y agregue la sección siguiente dentro del elemento de **configuración** , después del elemento **configSections** .</span><span class="sxs-lookup"><span data-stu-id="ee2f8-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="ee2f8-149">Esta configuración agrega una cadena de conexión para una base de datos de LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="ee2f8-150">Esta base de datos se usará al ejecutar la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="ee2f8-151">A continuación, agregue una clase denominada `ProductsContext` a la carpeta models:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="ee2f8-152">En el constructor, `"name=ProductsContext"` proporciona el nombre de la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="ee2f8-153">Configuración del punto de conexión de OData</span><span class="sxs-lookup"><span data-stu-id="ee2f8-153">Configure the OData endpoint</span></span>

<span data-ttu-id="ee2f8-154">Abra la aplicación de archivo\_Start/WebApiConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="ee2f8-155">Agregue las siguientes instrucciones **using** :</span><span class="sxs-lookup"><span data-stu-id="ee2f8-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="ee2f8-156">Después, agregue el código siguiente al método **Register** :</span><span class="sxs-lookup"><span data-stu-id="ee2f8-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="ee2f8-157">Este código hace dos cosas:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-157">This code does two things:</span></span>

- <span data-ttu-id="ee2f8-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="ee2f8-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="ee2f8-159">Agrega una ruta.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-159">Adds a route.</span></span>

<span data-ttu-id="ee2f8-160">Un EDM es un modelo abstracto de los datos.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="ee2f8-161">El EDM se usa para crear el documento de metadatos del servicio.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="ee2f8-162">La clase **ODataConventionModelBuilder** crea un EDM mediante las convenciones de nomenclatura predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="ee2f8-163">Este enfoque requiere el menos código.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-163">This approach requires the least code.</span></span> <span data-ttu-id="ee2f8-164">Si desea tener más control sobre el EDM, puede usar la clase **ODataModelBuilder** para crear el EDM agregando propiedades, claves y propiedades de navegación explícitamente.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="ee2f8-165">Una *ruta* indica a la API Web cómo enrutar las solicitudes HTTP al punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="ee2f8-166">Para crear una ruta de OData V4, llame al método de extensión **MapODataServiceRoute** .</span><span class="sxs-lookup"><span data-stu-id="ee2f8-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="ee2f8-167">Si la aplicación tiene varios puntos de conexión de OData, cree una ruta independiente para cada uno.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="ee2f8-168">Asigne a cada ruta un nombre y un prefijo de ruta únicos.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="ee2f8-169">Agregar el controlador OData</span><span class="sxs-lookup"><span data-stu-id="ee2f8-169">Add the OData controller</span></span>

<span data-ttu-id="ee2f8-170">Un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="ee2f8-171">Cree un controlador independiente para cada conjunto de entidades en el servicio OData.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="ee2f8-172">En este tutorial, creará un controlador para la entidad `Product`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="ee2f8-173">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** **clase**de &gt;.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="ee2f8-174">Asigne a la clase el nombre `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="ee2f8-175">La versión de este tutorial para OData V3 usa el scaffolding **Agregar controlador** .</span><span class="sxs-lookup"><span data-stu-id="ee2f8-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="ee2f8-176">Actualmente, no hay ningún scaffolding para OData V4.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="ee2f8-177">Reemplace el código reutilizable en ProductsController.cs con lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="ee2f8-178">El controlador utiliza la clase `ProductsContext` para tener acceso a la base de datos mediante EF.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="ee2f8-179">Tenga en cuenta que el controlador invalida el método **Dispose** para eliminar el **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="ee2f8-180">Este es el punto de partida para el controlador.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-180">This is the starting point for the controller.</span></span> <span data-ttu-id="ee2f8-181">A continuación, agregaremos métodos para todas las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="ee2f8-182">Consultar el conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="ee2f8-182">Query the entity set</span></span>

<span data-ttu-id="ee2f8-183">Agregue los métodos siguientes a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="ee2f8-184">La versión sin parámetros del método `Get` devuelve toda la colección de productos.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="ee2f8-185">El método `Get` con un parámetro *clave* busca un producto por su clave (en este caso, la propiedad `Id`).</span><span class="sxs-lookup"><span data-stu-id="ee2f8-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="ee2f8-186">El atributo **[EnableQuery]** permite a los clientes modificar la consulta mediante opciones de consulta como $filter, $sort y $Page.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="ee2f8-187">Para obtener más información, consulte [compatibilidad con las opciones de consulta de oData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="ee2f8-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="ee2f8-188">Agregar una entidad al conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="ee2f8-188">Add an entity to the entity set</span></span>

<span data-ttu-id="ee2f8-189">Para que los clientes puedan agregar un nuevo producto a la base de datos, agregue el método siguiente a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="ee2f8-190">Actualización de una entidad</span><span class="sxs-lookup"><span data-stu-id="ee2f8-190">Update an entity</span></span>

<span data-ttu-id="ee2f8-191">OData admite dos semánticas diferentes para actualizar una entidad, PATCH y PUT.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="ee2f8-192">PATCH realiza una actualización parcial.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-192">PATCH performs a partial update.</span></span> <span data-ttu-id="ee2f8-193">El cliente especifica solo las propiedades que se van a actualizar.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="ee2f8-194">PUT reemplaza toda la entidad.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="ee2f8-195">El inconveniente de PUT es que el cliente debe enviar valores para todas las propiedades de la entidad, incluidos los valores que no cambian.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="ee2f8-196">La [especificación de oData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica que se prefiere la revisión.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="ee2f8-197">En cualquier caso, este es el código de los métodos PATCH y PUT:</span><span class="sxs-lookup"><span data-stu-id="ee2f8-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="ee2f8-198">En el caso de la revisión, el controlador usa el tipo de **&gt;Delta&lt;t** para realizar el seguimiento de los cambios.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="ee2f8-199">Eliminación de una entidad</span><span class="sxs-lookup"><span data-stu-id="ee2f8-199">Delete an entity</span></span>

<span data-ttu-id="ee2f8-200">Para permitir que los clientes eliminen un producto de la base de datos, agregue el método siguiente a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="ee2f8-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
