---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creación de un punto de conexión de OData V3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de estructurar los datos, consultar los datos y manipular los datos...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448225"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="f4514-104">Creación de un punto de conexión de OData V3 con Web API 2</span><span class="sxs-lookup"><span data-stu-id="f4514-104">Creating an OData v3 Endpoint with Web API 2</span></span>

<span data-ttu-id="f4514-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f4514-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f4514-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="f4514-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="f4514-107">El [Open Data Protocol](http://www.odata.org/) (OData) es un protocolo de acceso a datos para la Web.</span><span class="sxs-lookup"><span data-stu-id="f4514-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="f4514-108">OData proporciona una manera uniforme de estructurar los datos, consultar los datos y manipular el conjunto de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).</span><span class="sxs-lookup"><span data-stu-id="f4514-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="f4514-109">OData admite los formatos AtomPub (XML) y JSON.</span><span class="sxs-lookup"><span data-stu-id="f4514-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="f4514-110">OData también define una manera de exponer metadatos sobre los datos.</span><span class="sxs-lookup"><span data-stu-id="f4514-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="f4514-111">Los clientes pueden usar los metadatos para detectar la información de tipo y las relaciones del conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="f4514-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="f4514-112">ASP.NET Web API facilita la creación de un punto de conexión de OData para un conjunto de datos.</span><span class="sxs-lookup"><span data-stu-id="f4514-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="f4514-113">Puede controlar exactamente las operaciones de OData que admite el extremo.</span><span class="sxs-lookup"><span data-stu-id="f4514-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="f4514-114">Puede hospedar varios puntos de conexión de OData, junto con puntos de conexión que no son de OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="f4514-115">Tiene control total sobre el modelo de datos, la lógica de negocios de back-end y el nivel de datos.</span><span class="sxs-lookup"><span data-stu-id="f4514-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4514-116">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f4514-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="f4514-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f4514-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="f4514-118">API Web 2</span><span class="sxs-lookup"><span data-stu-id="f4514-118">Web API 2</span></span>
> - <span data-ttu-id="f4514-119">Versión 3 de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-119">OData Version 3</span></span>
> - <span data-ttu-id="f4514-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f4514-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="f4514-121">Proxy de depuración web de Fiddler (opcional)</span><span class="sxs-lookup"><span data-stu-id="f4514-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="f4514-122">Se ha agregado compatibilidad con la API Web de OData en [ASP.net and Web Tools actualización 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="f4514-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="f4514-123">Sin embargo, en este tutorial se usa la técnica scaffolding que se agregó en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f4514-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>

<span data-ttu-id="f4514-124">En este tutorial, creará un punto de conexión de OData sencillo que los clientes pueden consultar.</span><span class="sxs-lookup"><span data-stu-id="f4514-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="f4514-125">También creará un C# cliente para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="f4514-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="f4514-126">Después de completar este tutorial, el siguiente conjunto de tutoriales muestra cómo agregar más funcionalidad, incluidas las relaciones de entidad, las acciones y el $expand/$select.</span><span class="sxs-lookup"><span data-stu-id="f4514-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="f4514-127">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4514-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="f4514-128">Agregar un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="f4514-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="f4514-129">Agregar un controlador OData</span><span class="sxs-lookup"><span data-stu-id="f4514-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="f4514-130">Agregar el EDM y la ruta</span><span class="sxs-lookup"><span data-stu-id="f4514-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="f4514-131">Inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="f4514-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="f4514-132">Exploración del punto de conexión de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="f4514-133">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="f4514-134">Crear el proyecto de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4514-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="f4514-135">En este tutorial, creará un punto de conexión de OData que admita las operaciones CRUD básicas.</span><span class="sxs-lookup"><span data-stu-id="f4514-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="f4514-136">El punto de conexión expondrá un único recurso, una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="f4514-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="f4514-137">En los tutoriales posteriores se agregarán más características.</span><span class="sxs-lookup"><span data-stu-id="f4514-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="f4514-138">Inicie Visual Studio y seleccione **nuevo proyecto** en la página de inicio.</span><span class="sxs-lookup"><span data-stu-id="f4514-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="f4514-139">O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f4514-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="f4514-140">En el panel **plantillas** , seleccione **plantillas instaladas** y expanda C# el nodo visual.</span><span class="sxs-lookup"><span data-stu-id="f4514-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="f4514-141">En **Visual C#** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="f4514-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="f4514-142">Seleccione **la plantilla aplicación Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="f4514-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="f4514-143">En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** .</span><span class="sxs-lookup"><span data-stu-id="f4514-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="f4514-144">En &quot;agregar carpetas y referencias principales para...&quot;, Compruebe la **API Web**.</span><span class="sxs-lookup"><span data-stu-id="f4514-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="f4514-145">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f4514-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="f4514-146">Agregar un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="f4514-146">Add an Entity Model</span></span>

<span data-ttu-id="f4514-147">Un *modelo* es un objeto que representa los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4514-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="f4514-148">En este tutorial, se necesita un modelo que represente un producto.</span><span class="sxs-lookup"><span data-stu-id="f4514-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="f4514-149">El modelo corresponde a nuestro tipo de entidad OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="f4514-150">En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models.</span><span class="sxs-lookup"><span data-stu-id="f4514-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="f4514-151">En el menú contextual, seleccione **Agregar** y, después, haga clic en **Clase**.</span><span class="sxs-lookup"><span data-stu-id="f4514-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="f4514-152">En el cuadro de diálogo **Agregar nuevo** elemento, asigne a la clase el nombre &quot;&quot;de producto.</span><span class="sxs-lookup"><span data-stu-id="f4514-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="f4514-153">Por Convención, las clases de modelo se colocan en la carpeta models.</span><span class="sxs-lookup"><span data-stu-id="f4514-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="f4514-154">No tiene que seguir esta Convención en sus propios proyectos, pero la usaremos para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="f4514-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>

<span data-ttu-id="f4514-155">En el archivo Product.cs, agregue la siguiente definición de clase:</span><span class="sxs-lookup"><span data-stu-id="f4514-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="f4514-156">La propiedad ID será la clave de entidad.</span><span class="sxs-lookup"><span data-stu-id="f4514-156">The ID property will be the entity key.</span></span> <span data-ttu-id="f4514-157">Los clientes pueden consultar los productos por identificador.</span><span class="sxs-lookup"><span data-stu-id="f4514-157">Clients can query products by ID.</span></span> <span data-ttu-id="f4514-158">Este campo también sería la clave principal en la base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="f4514-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="f4514-159">Compile el proyecto ahora.</span><span class="sxs-lookup"><span data-stu-id="f4514-159">Build the project now.</span></span> <span data-ttu-id="f4514-160">En el paso siguiente, usaremos algunos scaffolding de Visual Studio que usa la reflexión para buscar el tipo de producto.</span><span class="sxs-lookup"><span data-stu-id="f4514-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="f4514-161">Agregar un controlador OData</span><span class="sxs-lookup"><span data-stu-id="f4514-161">Add an OData Controller</span></span>

<span data-ttu-id="f4514-162">Un *controlador* es una clase que controla las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="f4514-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="f4514-163">Debe definir un controlador independiente para cada conjunto de entidades en el servicio OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="f4514-164">En este tutorial, vamos a crear un único controlador.</span><span class="sxs-lookup"><span data-stu-id="f4514-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="f4514-165">En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers.</span><span class="sxs-lookup"><span data-stu-id="f4514-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="f4514-166">Seleccione **Agregar** y, luego, **Controlador**.</span><span class="sxs-lookup"><span data-stu-id="f4514-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="f4514-167">En el cuadro de diálogo **Agregar scaffold** , seleccione &quot;controlador OData de Web API 2 con acciones, con Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="f4514-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="f4514-168">En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="f4514-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="f4514-169">Active la casilla &quot;usar las acciones de controlador Async&quot;.</span><span class="sxs-lookup"><span data-stu-id="f4514-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="f4514-170">En la lista desplegable **modelo** , seleccione la clase product.</span><span class="sxs-lookup"><span data-stu-id="f4514-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="f4514-171">Haga clic en el botón **nuevo contexto de datos...** .</span><span class="sxs-lookup"><span data-stu-id="f4514-171">Click the **New data context...** button.</span></span> <span data-ttu-id="f4514-172">Deje el nombre predeterminado para el tipo de contexto de datos y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="f4514-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="f4514-173">Haga clic en agregar en el cuadro de diálogo Agregar controlador para agregar el controlador.</span><span class="sxs-lookup"><span data-stu-id="f4514-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="f4514-174">Nota: Si recibe un mensaje de error que indica &quot;se produjo un error al obtener el tipo...&quot;, asegúrese de que ha compilado el proyecto de Visual Studio después de haber agregado la clase product.</span><span class="sxs-lookup"><span data-stu-id="f4514-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="f4514-175">El scaffolding usa la reflexión para buscar la clase.</span><span class="sxs-lookup"><span data-stu-id="f4514-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="f4514-176">El scaffolding agrega dos archivos de código al proyecto:</span><span class="sxs-lookup"><span data-stu-id="f4514-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="f4514-177">Products.cs define el controlador de API Web que implementa el punto de conexión de OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="f4514-178">ProductServiceContext.cs proporciona métodos para consultar la base de datos subyacente mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f4514-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="f4514-179">Agregar el EDM y la ruta</span><span class="sxs-lookup"><span data-stu-id="f4514-179">Add the EDM and Route</span></span>

<span data-ttu-id="f4514-180">En Explorador de soluciones, expanda la carpeta app\_Start y abra el archivo denominado WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="f4514-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="f4514-181">Esta clase contiene el código de configuración de la API Web.</span><span class="sxs-lookup"><span data-stu-id="f4514-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="f4514-182">Reemplace este código por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4514-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="f4514-183">Este código hace dos cosas:</span><span class="sxs-lookup"><span data-stu-id="f4514-183">This code does two things:</span></span>

- <span data-ttu-id="f4514-184">Crea un Entity Data Model (EDM) para el extremo de OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="f4514-185">Agrega una ruta para el extremo.</span><span class="sxs-lookup"><span data-stu-id="f4514-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="f4514-186">Un EDM es un modelo abstracto de los datos.</span><span class="sxs-lookup"><span data-stu-id="f4514-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="f4514-187">El EDM se usa para crear el documento de metadatos y definir los URI para el servicio.</span><span class="sxs-lookup"><span data-stu-id="f4514-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="f4514-188">**ODataConventionModelBuilder** crea un EDM mediante un conjunto de convenciones de nomenclatura predeterminadas EDM.</span><span class="sxs-lookup"><span data-stu-id="f4514-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="f4514-189">Este enfoque requiere el menos código.</span><span class="sxs-lookup"><span data-stu-id="f4514-189">This approach requires the least code.</span></span> <span data-ttu-id="f4514-190">Si desea tener más control sobre el EDM, puede usar la clase **ODataModelBuilder** para crear el EDM agregando propiedades, claves y propiedades de navegación explícitamente.</span><span class="sxs-lookup"><span data-stu-id="f4514-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="f4514-191">El método **EntitySet** agrega un conjunto de entidades al EDM:</span><span class="sxs-lookup"><span data-stu-id="f4514-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="f4514-192">La cadena "Products" define el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="f4514-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="f4514-193">El nombre del controlador debe coincidir con el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="f4514-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="f4514-194">En este tutorial, el conjunto de entidades se denomina "Products" y el controlador se denomina `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="f4514-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="f4514-195">Si ha llamado al conjunto de entidades "ProductSet", deberá asignar un nombre al controlador `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="f4514-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="f4514-196">Tenga en cuenta que un punto de conexión puede tener varios conjuntos de entidades.</span><span class="sxs-lookup"><span data-stu-id="f4514-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="f4514-197">Llame a **EntitySet&lt;t&gt;** para cada conjunto de entidades y, a continuación, defina un controlador correspondiente.</span><span class="sxs-lookup"><span data-stu-id="f4514-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="f4514-198">El método **MapODataRoute** agrega una ruta para el punto de conexión de oData.</span><span class="sxs-lookup"><span data-stu-id="f4514-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="f4514-199">El primer parámetro es un nombre descriptivo para la ruta.</span><span class="sxs-lookup"><span data-stu-id="f4514-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="f4514-200">Los clientes de su servicio no ven este nombre.</span><span class="sxs-lookup"><span data-stu-id="f4514-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="f4514-201">El segundo parámetro es el prefijo URI del extremo.</span><span class="sxs-lookup"><span data-stu-id="f4514-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="f4514-202">Dado este código, el URI del conjunto de entidades Products es http://<em>hostname</em>/OData/Products.</span><span class="sxs-lookup"><span data-stu-id="f4514-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="f4514-203">La aplicación puede tener más de un punto de conexión de OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="f4514-204">Para cada punto de conexión, llame a <strong>MapODataRoute</strong> y proporcione un nombre de ruta único y un prefijo de identificador URI único.</span><span class="sxs-lookup"><span data-stu-id="f4514-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="f4514-205">Inicialización de la base de datos (opcional)</span><span class="sxs-lookup"><span data-stu-id="f4514-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="f4514-206">En este paso, usará Entity Framework para inicializar la base de datos con algunos datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="f4514-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="f4514-207">Este paso es opcional, pero le permite probar el punto de conexión de OData de inmediato.</span><span class="sxs-lookup"><span data-stu-id="f4514-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="f4514-208">En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="f4514-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="f4514-209">En la ventana Package Manager Console, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f4514-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="f4514-210">Esto agrega una carpeta denominada Migrations y un archivo de código denominado Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="f4514-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="f4514-211">Abra este archivo y agregue el código siguiente al método `Configuration.Seed`.</span><span class="sxs-lookup"><span data-stu-id="f4514-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="f4514-212">En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="f4514-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="f4514-213">Estos comandos generan código que crea la base de datos y, a continuación, ejecutan ese código.</span><span class="sxs-lookup"><span data-stu-id="f4514-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="f4514-214">Exploración del punto de conexión de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="f4514-215">En esta sección, usaremos el [proxy de depuración web de Fiddler](http://www.fiddler2.com) para enviar solicitudes al punto de conexión y examinar los mensajes de respuesta.</span><span class="sxs-lookup"><span data-stu-id="f4514-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="f4514-216">Esto le ayudará a comprender las capacidades de un punto de conexión de OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="f4514-217">En Visual Studio, presione F5 para iniciar la depuración.</span><span class="sxs-lookup"><span data-stu-id="f4514-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="f4514-218">De forma predeterminada, Visual Studio abre el explorador para `http://localhost:*port*`, donde *Port* es el número de puerto configurado en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4514-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="f4514-219">Puede cambiar el número de puerto en la configuración del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f4514-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="f4514-220">En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="f4514-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="f4514-221">En la ventana Propiedades, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="f4514-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="f4514-222">Escriba el número de puerto en **dirección URL del proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f4514-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="f4514-223">Documento de servicio</span><span class="sxs-lookup"><span data-stu-id="f4514-223">Service Document</span></span>

<span data-ttu-id="f4514-224">El *documento de servicio* contiene una lista de los conjuntos de entidades para el punto de conexión de oData.</span><span class="sxs-lookup"><span data-stu-id="f4514-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="f4514-225">Para obtener el documento de servicio, envíe una solicitud GET al URI raíz del servicio.</span><span class="sxs-lookup"><span data-stu-id="f4514-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="f4514-226">Con Fiddler, escriba el siguiente URI en la pestaña **compositor** : `http://localhost:port/odata/`, donde *Port* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="f4514-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="f4514-227">Haga clic en el botón **Ejecutar** .</span><span class="sxs-lookup"><span data-stu-id="f4514-227">Click the **Execute** button.</span></span> <span data-ttu-id="f4514-228">Fiddler envía una solicitud HTTP GET a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f4514-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="f4514-229">Debería ver la respuesta en la lista de sesiones Web.</span><span class="sxs-lookup"><span data-stu-id="f4514-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="f4514-230">Si todo funciona, el código de estado será 200.</span><span class="sxs-lookup"><span data-stu-id="f4514-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="f4514-231">Haga doble clic en la respuesta en la lista sesiones Web para ver los detalles del mensaje de respuesta en la pestaña inspectores.</span><span class="sxs-lookup"><span data-stu-id="f4514-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="f4514-232">El mensaje de respuesta HTTP sin formato debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="f4514-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="f4514-233">De forma predeterminada, la API Web devuelve el documento de servicio en formato AtomPub.</span><span class="sxs-lookup"><span data-stu-id="f4514-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="f4514-234">Para solicitar JSON, agregue el siguiente encabezado a la solicitud HTTP:</span><span class="sxs-lookup"><span data-stu-id="f4514-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="f4514-235">Ahora la respuesta HTTP contiene una carga JSON:</span><span class="sxs-lookup"><span data-stu-id="f4514-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="f4514-236">Documento de metadatos de servicio</span><span class="sxs-lookup"><span data-stu-id="f4514-236">Service Metadata Document</span></span>

<span data-ttu-id="f4514-237">El *documento de metadatos del servicio* describe el modelo de datos del servicio, utilizando un lenguaje XML denominado lenguaje de definición de esquemas conceptuales (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f4514-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="f4514-238">El documento de metadatos muestra la estructura de los datos en el servicio y se puede usar para generar el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="f4514-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="f4514-239">Para obtener el documento de metadatos, envíe una solicitud GET a `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="f4514-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="f4514-240">Estos son los metadatos del punto de conexión que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="f4514-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="f4514-241">Conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="f4514-241">Entity Set</span></span>

<span data-ttu-id="f4514-242">Para obtener el conjunto de entidades Products, envíe una solicitud GET a `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="f4514-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="f4514-243">Entity</span><span class="sxs-lookup"><span data-stu-id="f4514-243">Entity</span></span>

<span data-ttu-id="f4514-244">Para obtener un producto individual, envíe una solicitud GET a `http://localhost:port/odata/Products(1)`, donde "1" es el identificador del producto.</span><span class="sxs-lookup"><span data-stu-id="f4514-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="f4514-245">Formatos de serialización de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-245">OData Serialization Formats</span></span>

<span data-ttu-id="f4514-246">OData admite varios formatos de serialización:</span><span class="sxs-lookup"><span data-stu-id="f4514-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="f4514-247">Atom pub (XML)</span><span class="sxs-lookup"><span data-stu-id="f4514-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="f4514-248">JSON "Light" (introducido en OData V3)</span><span class="sxs-lookup"><span data-stu-id="f4514-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="f4514-249">JSON "detallado" (OData V2)</span><span class="sxs-lookup"><span data-stu-id="f4514-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="f4514-250">De forma predeterminada, Web API usa el formato "Light" de AtomPubJSON.</span><span class="sxs-lookup"><span data-stu-id="f4514-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="f4514-251">Para obtener el formato AtomPub, establezca el encabezado Accept en "Application/Atom + XML".</span><span class="sxs-lookup"><span data-stu-id="f4514-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="f4514-252">A continuación se proporciona un cuerpo de respuesta de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f4514-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="f4514-253">Puede ver una desventaja obvia del formato Atom: es mucho más detallado que la luz JSON.</span><span class="sxs-lookup"><span data-stu-id="f4514-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="f4514-254">Sin embargo, si tiene un cliente que entienda AtomPub, el cliente podría preferir ese formato a través de JSON.</span><span class="sxs-lookup"><span data-stu-id="f4514-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="f4514-255">Esta es la versión JSON Light de la misma entidad:</span><span class="sxs-lookup"><span data-stu-id="f4514-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="f4514-256">El formato JSON Light se presentó en la versión 3 del protocolo OData.</span><span class="sxs-lookup"><span data-stu-id="f4514-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="f4514-257">Para mantener la compatibilidad con versiones anteriores, un cliente puede solicitar el formato JSON anterior "detallado".</span><span class="sxs-lookup"><span data-stu-id="f4514-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="f4514-258">Para solicitar JSON detallado, establezca el encabezado Accept en `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="f4514-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="f4514-259">Esta es la versión detallada:</span><span class="sxs-lookup"><span data-stu-id="f4514-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="f4514-260">Este formato transmite más metadatos en el cuerpo de la respuesta, que puede Agregar una sobrecarga considerable sobre una sesión completa.</span><span class="sxs-lookup"><span data-stu-id="f4514-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="f4514-261">Además, agrega un nivel de direccionamiento indirecto ajustando el objeto en una propiedad denominada "d".</span><span class="sxs-lookup"><span data-stu-id="f4514-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4514-262">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f4514-262">Next Steps</span></span>

- [<span data-ttu-id="f4514-263">Agregar relaciones de entidad</span><span class="sxs-lookup"><span data-stu-id="f4514-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="f4514-264">Agregar acciones de OData</span><span class="sxs-lookup"><span data-stu-id="f4514-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="f4514-265">Llamar al servicio de OData desde un cliente .NET</span><span class="sxs-lookup"><span data-stu-id="f4514-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
