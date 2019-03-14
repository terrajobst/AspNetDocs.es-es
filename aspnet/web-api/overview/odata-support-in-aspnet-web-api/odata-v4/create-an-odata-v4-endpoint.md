---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Crear un punto de conexión de OData v4 con ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para consultar y manipular conjuntos de datos mediante operaciones CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57042602"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="d9d08-104">Crear un punto de conexión de OData v4 con ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="d9d08-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="d9d08-105">Open Data Protocol (OData) es un protocolo de acceso a datos para la web.</span><span class="sxs-lookup"><span data-stu-id="d9d08-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="d9d08-106">OData proporciona una manera uniforme para consultar y manipular conjuntos de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="d9d08-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="d9d08-107">Es compatible con ASP.NET Web API v3 y v4 del protocolo.</span><span class="sxs-lookup"><span data-stu-id="d9d08-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="d9d08-108">Incluso puede tener un punto de conexión de v4 que se ejecuta en paralelo con un punto de conexión v3.</span><span class="sxs-lookup"><span data-stu-id="d9d08-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="d9d08-109">Este tutorial muestra cómo crear un punto de conexión de OData v4 que admite operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="d9d08-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d9d08-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="d9d08-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="d9d08-111">Web API 5.2</span><span class="sxs-lookup"><span data-stu-id="d9d08-111">Web API 5.2</span></span>
> - <span data-ttu-id="d9d08-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="d9d08-112">OData v4</span></span>
> - <span data-ttu-id="d9d08-113">Visual Studio 2017 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="d9d08-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="d9d08-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d9d08-114">Entity Framework 6</span></span>
> - <span data-ttu-id="d9d08-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="d9d08-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="d9d08-116">Versiones de tutoriales</span><span class="sxs-lookup"><span data-stu-id="d9d08-116">Tutorial versions</span></span>
>
> <span data-ttu-id="d9d08-117">Para la versión 3 de OData, consulte [creación de un extremo de OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d9d08-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d9d08-118">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9d08-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="d9d08-119">En Visual Studio, desde el **archivo** menú, seleccione **New** &gt; **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="d9d08-120">Expanda **instalado** &gt; **Visual C#**  &gt; **Web**y seleccione el **aplicación Web ASP.NET (.NET Framework)**  plantilla.</span><span class="sxs-lookup"><span data-stu-id="d9d08-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="d9d08-121">Denomine el proyecto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="d9d08-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="d9d08-122">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-122">Select **OK**.</span></span>



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="d9d08-123">Seleccione el **vacía** plantilla.</span><span class="sxs-lookup"><span data-stu-id="d9d08-123">Select the **Empty** template.</span></span> <span data-ttu-id="d9d08-124">En **agregar carpetas y referencias centrales para:**, seleccione **API Web**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="d9d08-125">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="d9d08-126">Instalar los paquetes de OData</span><span class="sxs-lookup"><span data-stu-id="d9d08-126">Install the OData packages</span></span>

<span data-ttu-id="d9d08-127">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d9d08-128">En la ventana de consola de administrador de paquetes, escriba:</span><span class="sxs-lookup"><span data-stu-id="d9d08-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="d9d08-129">Este comando instala los paquetes de OData NuGet más recientes.</span><span class="sxs-lookup"><span data-stu-id="d9d08-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d9d08-130">Incorporación de una clase de modelo</span><span class="sxs-lookup"><span data-stu-id="d9d08-130">Add a model class</span></span>

<span data-ttu-id="d9d08-131">Un *modelo* es un objeto que representa una entidad de datos en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9d08-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="d9d08-132">En el Explorador de soluciones, haga clic en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="d9d08-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d9d08-133">En el menú contextual, seleccione **agregar** &gt; **clase**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="d9d08-134">Por convención, las clases de modelo se colocan en la carpeta Models, pero no tiene que seguir esta convención en sus propios proyectos.</span><span class="sxs-lookup"><span data-stu-id="d9d08-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="d9d08-135">Asigne a la clase el nombre `Product`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-135">Name the class `Product`.</span></span> <span data-ttu-id="d9d08-136">En el archivo Product.cs, reemplace el código reutilizable, con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9d08-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="d9d08-137">El `Id` propiedad es la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="d9d08-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="d9d08-138">Los clientes pueden consultar las entidades por clave.</span><span class="sxs-lookup"><span data-stu-id="d9d08-138">Clients can query entities by key.</span></span> <span data-ttu-id="d9d08-139">Por ejemplo, para obtener el producto con el Id. de 5, el URI es `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="d9d08-140">El `Id` propiedad también será la clave principal de la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="d9d08-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="d9d08-141">Habilitar Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d9d08-141">Enable Entity Framework</span></span>

<span data-ttu-id="d9d08-142">Para este tutorial, vamos a usar Entity Framework (EF) Code First para crear la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="d9d08-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="d9d08-143">Web API OData no requiere EF.</span><span class="sxs-lookup"><span data-stu-id="d9d08-143">Web API OData does not require EF.</span></span> <span data-ttu-id="d9d08-144">Utilice cualquier capa de acceso a datos que se traduce las entidades de base de datos en modelos.</span><span class="sxs-lookup"><span data-stu-id="d9d08-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="d9d08-145">En primer lugar, instale el paquete de NuGet para EF.</span><span class="sxs-lookup"><span data-stu-id="d9d08-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="d9d08-146">En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d9d08-147">En la ventana de consola de administrador de paquetes, escriba:</span><span class="sxs-lookup"><span data-stu-id="d9d08-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="d9d08-148">Abra el archivo Web.config y agregue la siguiente sección dentro la **configuración** elemento, después el **configSections** elemento.</span><span class="sxs-lookup"><span data-stu-id="d9d08-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="d9d08-149">Esta configuración agrega una cadena de conexión para una base de datos LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d9d08-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="d9d08-150">Esta base de datos se usará cuando se ejecuta la aplicación localmente.</span><span class="sxs-lookup"><span data-stu-id="d9d08-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="d9d08-151">A continuación, agregue una clase denominada `ProductsContext` a la carpeta modelos:</span><span class="sxs-lookup"><span data-stu-id="d9d08-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="d9d08-152">En el constructor, `"name=ProductsContext"` proporciona el nombre de la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="d9d08-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="d9d08-153">Configurar el extremo de OData</span><span class="sxs-lookup"><span data-stu-id="d9d08-153">Configure the OData endpoint</span></span>

<span data-ttu-id="d9d08-154">Abra el archivo de aplicación\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="d9d08-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="d9d08-155">Agregue el siguiente **mediante** instrucciones:</span><span class="sxs-lookup"><span data-stu-id="d9d08-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="d9d08-156">A continuación, agregue el código siguiente a la **registrar** método:</span><span class="sxs-lookup"><span data-stu-id="d9d08-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="d9d08-157">Este código hace dos cosas:</span><span class="sxs-lookup"><span data-stu-id="d9d08-157">This code does two things:</span></span>

- <span data-ttu-id="d9d08-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="d9d08-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="d9d08-159">Agrega una ruta.</span><span class="sxs-lookup"><span data-stu-id="d9d08-159">Adds a route.</span></span>

<span data-ttu-id="d9d08-160">Un EDM es un modelo abstracto de los datos.</span><span class="sxs-lookup"><span data-stu-id="d9d08-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="d9d08-161">El EDM se usa para crear el documento de metadatos de servicio.</span><span class="sxs-lookup"><span data-stu-id="d9d08-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="d9d08-162">El **ODataConventionModelBuilder** clase crea un modelo EDM utilizando las convenciones de nomenclatura predeterminado.</span><span class="sxs-lookup"><span data-stu-id="d9d08-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="d9d08-163">Este enfoque requiere menos código.</span><span class="sxs-lookup"><span data-stu-id="d9d08-163">This approach requires the least code.</span></span> <span data-ttu-id="d9d08-164">Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el EDM agregando explícitamente las propiedades, claves y las propiedades de navegación.</span><span class="sxs-lookup"><span data-stu-id="d9d08-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="d9d08-165">Un *ruta* indica cómo enrutar las solicitudes HTTP al punto de conexión de API Web.</span><span class="sxs-lookup"><span data-stu-id="d9d08-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="d9d08-166">Para crear una ruta de OData v4, llame a la **MapODataServiceRoute** método de extensión.</span><span class="sxs-lookup"><span data-stu-id="d9d08-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="d9d08-167">Si la aplicación tiene varios puntos de conexión de OData, cree una ruta independiente para cada uno.</span><span class="sxs-lookup"><span data-stu-id="d9d08-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="d9d08-168">Asigne a cada ruta de un nombre de ruta único y un prefijo.</span><span class="sxs-lookup"><span data-stu-id="d9d08-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="d9d08-169">Agregar el controlador de OData</span><span class="sxs-lookup"><span data-stu-id="d9d08-169">Add the OData controller</span></span>

<span data-ttu-id="d9d08-170">Un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9d08-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="d9d08-171">Cree un controlador independiente para cada conjunto de entidades en el servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="d9d08-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="d9d08-172">En este tutorial, creará un controlador para el `Product` entidad.</span><span class="sxs-lookup"><span data-stu-id="d9d08-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="d9d08-173">En el Explorador de soluciones, haga clic en la carpeta Controllers y seleccione **agregar** &gt; **clase**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="d9d08-174">Asigne a la clase el nombre `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="d9d08-175">La versión de este tutorial para OData v3 utiliza el **Agregar controlador** scaffolding.</span><span class="sxs-lookup"><span data-stu-id="d9d08-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="d9d08-176">Actualmente, no hay ningún scaffolding para OData v4.</span><span class="sxs-lookup"><span data-stu-id="d9d08-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="d9d08-177">Reemplace el código reutilizable en ProductsController.cs por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="d9d08-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="d9d08-178">Utiliza el controlador del `ProductsContext` clase para tener acceso a la base de datos con EF.</span><span class="sxs-lookup"><span data-stu-id="d9d08-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="d9d08-179">Tenga en cuenta que el controlador invalida la **Dispose** método para desechar el **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="d9d08-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="d9d08-180">Es el punto de partida para el controlador.</span><span class="sxs-lookup"><span data-stu-id="d9d08-180">This is the starting point for the controller.</span></span> <span data-ttu-id="d9d08-181">A continuación, agregaremos métodos para todas las operaciones CRUD.</span><span class="sxs-lookup"><span data-stu-id="d9d08-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="d9d08-182">Consultar el conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="d9d08-182">Query the entity set</span></span>

<span data-ttu-id="d9d08-183">Agregue los siguientes métodos para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="d9d08-184">La versión sin parámetros de la `Get` método devuelve el conjunto completo de productos.</span><span class="sxs-lookup"><span data-stu-id="d9d08-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="d9d08-185">El `Get` método con un *clave* parámetro busca un producto por su clave (en este caso, el `Id` propiedad).</span><span class="sxs-lookup"><span data-stu-id="d9d08-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="d9d08-186">El **[EnableQuery]** atributo permite a los clientes modificar la consulta, mediante las opciones de consulta como $filter, $sort y $page.</span><span class="sxs-lookup"><span data-stu-id="d9d08-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="d9d08-187">Para obtener más información, consulte [que admiten opciones de consulta de OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="d9d08-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="d9d08-188">Agregar una entidad para el conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="d9d08-188">Add an entity to the entity set</span></span>

<span data-ttu-id="d9d08-189">Para permitir que los clientes agregar un nuevo producto a la base de datos, agregue el método siguiente a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="d9d08-190">Actualizar una entidad</span><span class="sxs-lookup"><span data-stu-id="d9d08-190">Update an entity</span></span>

<span data-ttu-id="d9d08-191">OData admite dos una semántica diferente para actualizar una entidad, PATCH y PUT.</span><span class="sxs-lookup"><span data-stu-id="d9d08-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="d9d08-192">REVISIÓN realiza una actualización parcial.</span><span class="sxs-lookup"><span data-stu-id="d9d08-192">PATCH performs a partial update.</span></span> <span data-ttu-id="d9d08-193">El cliente especifica las propiedades de actualización.</span><span class="sxs-lookup"><span data-stu-id="d9d08-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="d9d08-194">PUT reemplaza toda la entidad.</span><span class="sxs-lookup"><span data-stu-id="d9d08-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="d9d08-195">La desventaja de PUT es que el cliente debe enviar los valores de todas las propiedades de la entidad, incluidos los valores que no va a modificar.</span><span class="sxs-lookup"><span data-stu-id="d9d08-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="d9d08-196">El [especificación OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica que se prefiere la revisión.</span><span class="sxs-lookup"><span data-stu-id="d9d08-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="d9d08-197">En cualquier caso, este es el código para métodos PATCH y PUT:</span><span class="sxs-lookup"><span data-stu-id="d9d08-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="d9d08-198">En el caso de revisiones, el controlador utiliza la **Delta&lt;T&gt;**  tipo para realizar el seguimiento de los cambios.</span><span class="sxs-lookup"><span data-stu-id="d9d08-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="d9d08-199">Eliminar una entidad</span><span class="sxs-lookup"><span data-stu-id="d9d08-199">Delete an entity</span></span>

<span data-ttu-id="d9d08-200">Para permitir que los clientes eliminar un producto de la base de datos, agregue el método siguiente a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d9d08-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
