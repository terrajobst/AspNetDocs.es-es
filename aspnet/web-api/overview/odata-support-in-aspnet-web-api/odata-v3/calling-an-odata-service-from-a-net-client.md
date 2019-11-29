---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un clienteC#.net () | Microsoft Docs
author: MikeWasson
description: En este tutorial se muestra cómo llamar a un servicio de C# OData desde una aplicación cliente. Versiones de software usadas en el tutorial Visual Studio 2013 (funciona con objetos visuales...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600379"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="faad6-104">Llamar a un servicio OData desde un cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="faad6-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="faad6-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="faad6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="faad6-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="faad6-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="faad6-107">En este tutorial se muestra cómo llamar a un servicio de C# OData desde una aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="faad6-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="faad6-108">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="faad6-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="faad6-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funciona con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="faad6-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="faad6-110">Biblioteca cliente de Servicios de datos de WCF</span><span class="sxs-lookup"><span data-stu-id="faad6-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="faad6-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="faad6-111">Web API 2.</span></span> <span data-ttu-id="faad6-112">(El servicio OData de ejemplo se crea con Web API 2, pero la aplicación cliente no depende de la API Web).</span><span class="sxs-lookup"><span data-stu-id="faad6-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>

<span data-ttu-id="faad6-113">En este tutorial, veremos cómo crear una aplicación cliente que llama a un servicio de OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="faad6-114">El servicio OData expone las siguientes entidades:</span><span class="sxs-lookup"><span data-stu-id="faad6-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="faad6-115">En los artículos siguientes se describe cómo implementar el servicio OData en Web API.</span><span class="sxs-lookup"><span data-stu-id="faad6-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="faad6-116">(Sin embargo, no es necesario que los lea para comprender este tutorial).</span><span class="sxs-lookup"><span data-stu-id="faad6-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="faad6-117">Creación de un punto de conexión de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="faad6-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="faad6-118">Relaciones de entidad de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="faad6-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="faad6-119">Acciones de OData en Web API 2</span><span class="sxs-lookup"><span data-stu-id="faad6-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="faad6-120">Generar el proxy del servicio</span><span class="sxs-lookup"><span data-stu-id="faad6-120">Generate the Service Proxy</span></span>

<span data-ttu-id="faad6-121">El primer paso es generar un proxy de servicio.</span><span class="sxs-lookup"><span data-stu-id="faad6-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="faad6-122">El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="faad6-123">El proxy convierte las llamadas al método en solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="faad6-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="faad6-124">Abra el proyecto de servicio OData en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="faad6-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="faad6-125">Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express.</span><span class="sxs-lookup"><span data-stu-id="faad6-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="faad6-126">Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna.</span><span class="sxs-lookup"><span data-stu-id="faad6-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="faad6-127">Necesitará esta dirección al crear el proxy.</span><span class="sxs-lookup"><span data-stu-id="faad6-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="faad6-128">Después, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola.</span><span class="sxs-lookup"><span data-stu-id="faad6-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="faad6-129">La aplicación de consola será nuestra aplicación cliente de OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-129">The console application will be our OData client application.</span></span> <span data-ttu-id="faad6-130">(También puede Agregar el proyecto a la misma solución que el servicio).</span><span class="sxs-lookup"><span data-stu-id="faad6-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="faad6-131">Los pasos restantes hacen referencia al proyecto de consola.</span><span class="sxs-lookup"><span data-stu-id="faad6-131">The remaining steps refer the console project.</span></span>

<span data-ttu-id="faad6-132">En Explorador de soluciones, haga clic con el botón secundario en **referencias** y seleccione **Agregar referencia de servicio**.</span><span class="sxs-lookup"><span data-stu-id="faad6-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="faad6-133">En el cuadro de diálogo **Agregar referencia de servicio** , escriba la dirección del servicio OData:</span><span class="sxs-lookup"><span data-stu-id="faad6-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="faad6-134">donde *Port* es el número de puerto.</span><span class="sxs-lookup"><span data-stu-id="faad6-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="faad6-135">En **espacio de nombres**, escriba "ProductService".</span><span class="sxs-lookup"><span data-stu-id="faad6-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="faad6-136">Esta opción define el espacio de nombres de la clase de proxy.</span><span class="sxs-lookup"><span data-stu-id="faad6-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="faad6-137">Haga clic en **Ir**.</span><span class="sxs-lookup"><span data-stu-id="faad6-137">Click **Go**.</span></span> <span data-ttu-id="faad6-138">Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.</span><span class="sxs-lookup"><span data-stu-id="faad6-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="faad6-139">Haga clic en **Aceptar** para agregar la clase de proxy a su proyecto.</span><span class="sxs-lookup"><span data-stu-id="faad6-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="faad6-140">Crear una instancia de la clase de proxy de servicio</span><span class="sxs-lookup"><span data-stu-id="faad6-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="faad6-141">Dentro de su método de `Main`, cree una nueva instancia de la clase de proxy, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="faad6-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="faad6-142">De nuevo, use el número de Puerto Real en el que se ejecuta el servicio.</span><span class="sxs-lookup"><span data-stu-id="faad6-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="faad6-143">Al implementar el servicio, usará el URI del servicio en directo.</span><span class="sxs-lookup"><span data-stu-id="faad6-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="faad6-144">No es necesario actualizar el proxy.</span><span class="sxs-lookup"><span data-stu-id="faad6-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="faad6-145">En el código siguiente se agrega un controlador de eventos que imprime los URI de solicitud en la ventana de la consola.</span><span class="sxs-lookup"><span data-stu-id="faad6-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="faad6-146">Este paso no es necesario, pero es interesante ver los URI de cada consulta.</span><span class="sxs-lookup"><span data-stu-id="faad6-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="faad6-147">Consultar el servicio</span><span class="sxs-lookup"><span data-stu-id="faad6-147">Query the Service</span></span>

<span data-ttu-id="faad6-148">El código siguiente obtiene la lista de productos del servicio OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="faad6-149">Tenga en cuenta que no es necesario escribir código para enviar la solicitud HTTP o analizar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="faad6-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="faad6-150">La clase de proxy hace esto automáticamente al enumerar la `Container.Products` colección en el bucle **foreach** .</span><span class="sxs-lookup"><span data-stu-id="faad6-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="faad6-151">Al ejecutar la aplicación, el resultado debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="faad6-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="faad6-152">Para obtener una entidad por identificador, utilice una cláusula `where`.</span><span class="sxs-lookup"><span data-stu-id="faad6-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="faad6-153">En el resto de este tema, no mostraré toda la función de `Main`, solo el código necesario para llamar al servicio.</span><span class="sxs-lookup"><span data-stu-id="faad6-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="faad6-154">Aplicar opciones de consulta</span><span class="sxs-lookup"><span data-stu-id="faad6-154">Apply Query Options</span></span>

<span data-ttu-id="faad6-155">OData define [las opciones de consulta](../supporting-odata-query-options.md) que se pueden usar para filtrar, ordenar, datos de página, etc.</span><span class="sxs-lookup"><span data-stu-id="faad6-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="faad6-156">En el proxy del servicio, puede aplicar estas opciones mediante varias expresiones LINQ.</span><span class="sxs-lookup"><span data-stu-id="faad6-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="faad6-157">En esta sección, mostraré breves ejemplos.</span><span class="sxs-lookup"><span data-stu-id="faad6-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="faad6-158">Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) en MSDN.</span><span class="sxs-lookup"><span data-stu-id="faad6-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="faad6-159">Filtrado ($filter)</span><span class="sxs-lookup"><span data-stu-id="faad6-159">Filtering ($filter)</span></span>

<span data-ttu-id="faad6-160">Para filtrar, use una cláusula `where`.</span><span class="sxs-lookup"><span data-stu-id="faad6-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="faad6-161">En el ejemplo siguiente se filtra por categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="faad6-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="faad6-162">Este código corresponde a la siguiente consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="faad6-163">Observe que el proxy convierte la cláusula `where` en una expresión `$filter` de OData.</span><span class="sxs-lookup"><span data-stu-id="faad6-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="faad6-164">Ordenación ($orderby)</span><span class="sxs-lookup"><span data-stu-id="faad6-164">Sorting ($orderby)</span></span>

<span data-ttu-id="faad6-165">Para ordenar, use una cláusula `orderby`.</span><span class="sxs-lookup"><span data-stu-id="faad6-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="faad6-166">En el ejemplo siguiente se ordena por precio, de mayor a menor.</span><span class="sxs-lookup"><span data-stu-id="faad6-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="faad6-167">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="faad6-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="faad6-168">Paginación del lado cliente ($skip y $top)</span><span class="sxs-lookup"><span data-stu-id="faad6-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="faad6-169">En el caso de los conjuntos de entidades de gran tamaño, es posible que el cliente desee limitar el número de resultados.</span><span class="sxs-lookup"><span data-stu-id="faad6-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="faad6-170">Por ejemplo, un cliente podría mostrar 10 entradas a la vez.</span><span class="sxs-lookup"><span data-stu-id="faad6-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="faad6-171">Esto se denomina *paginación del lado cliente*.</span><span class="sxs-lookup"><span data-stu-id="faad6-171">This is called *client-side paging*.</span></span> <span data-ttu-id="faad6-172">(También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados). Para realizar la paginación del lado cliente, use los métodos **SKIP** y **Take** de Linq.</span><span class="sxs-lookup"><span data-stu-id="faad6-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="faad6-173">En el ejemplo siguiente se omiten los primeros 40 resultados y se toman los 10 siguientes.</span><span class="sxs-lookup"><span data-stu-id="faad6-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="faad6-174">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="faad6-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="faad6-175">Select ($select) y Expand ($expand)</span><span class="sxs-lookup"><span data-stu-id="faad6-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="faad6-176">Para incluir entidades relacionadas, use el método `DataServiceQuery<t>.Expand`.</span><span class="sxs-lookup"><span data-stu-id="faad6-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="faad6-177">Por ejemplo, para incluir el `Supplier` para cada `Product`:</span><span class="sxs-lookup"><span data-stu-id="faad6-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="faad6-178">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="faad6-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="faad6-179">Para cambiar la forma de la respuesta, use la cláusula LINQ **Select** .</span><span class="sxs-lookup"><span data-stu-id="faad6-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="faad6-180">En el ejemplo siguiente se obtiene solo el nombre de cada producto, sin ninguna otra propiedad.</span><span class="sxs-lookup"><span data-stu-id="faad6-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="faad6-181">Esta es la solicitud de OData correspondiente:</span><span class="sxs-lookup"><span data-stu-id="faad6-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="faad6-182">Una cláusula SELECT puede incluir entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="faad6-182">A select clause can include related entities.</span></span> <span data-ttu-id="faad6-183">En ese caso, no llame a **Expand**; en este caso, el proxy incluye automáticamente la expansión.</span><span class="sxs-lookup"><span data-stu-id="faad6-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="faad6-184">En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.</span><span class="sxs-lookup"><span data-stu-id="faad6-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="faad6-185">Esta es la solicitud de OData correspondiente.</span><span class="sxs-lookup"><span data-stu-id="faad6-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="faad6-186">Tenga en cuenta que incluye la opción **$Expand** .</span><span class="sxs-lookup"><span data-stu-id="faad6-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="faad6-187">Para obtener más información sobre $select y $expand, consulte [uso de $Select, $Expand y $Value en Web API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="faad6-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="faad6-188">Agregar una nueva entidad</span><span class="sxs-lookup"><span data-stu-id="faad6-188">Add a New Entity</span></span>

<span data-ttu-id="faad6-189">Para agregar una nueva entidad a un conjunto de entidades, llame a `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="faad6-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="faad6-190">Por ejemplo, `AddToProducts` agrega un nuevo `Product` al conjunto de entidades `Products`.</span><span class="sxs-lookup"><span data-stu-id="faad6-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="faad6-191">Al generar el proxy, WCF Data Services crea automáticamente estos métodos **AddTo** fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="faad6-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="faad6-192">Para agregar un vínculo entre dos entidades, use los métodos **AddLink** y **SetLink** .</span><span class="sxs-lookup"><span data-stu-id="faad6-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="faad6-193">El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.</span><span class="sxs-lookup"><span data-stu-id="faad6-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="faad6-194">Use **AddLink** cuando la propiedad de navegación sea una colección.</span><span class="sxs-lookup"><span data-stu-id="faad6-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="faad6-195">En este ejemplo, vamos a agregar un producto a la colección de `Products` del proveedor.</span><span class="sxs-lookup"><span data-stu-id="faad6-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="faad6-196">Use **SetLink** cuando la propiedad de navegación sea una sola entidad.</span><span class="sxs-lookup"><span data-stu-id="faad6-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="faad6-197">En este ejemplo, estamos estableciendo la propiedad `Supplier` en el producto.</span><span class="sxs-lookup"><span data-stu-id="faad6-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="faad6-198">Actualización/revisión</span><span class="sxs-lookup"><span data-stu-id="faad6-198">Update / Patch</span></span>

<span data-ttu-id="faad6-199">Para actualizar una entidad, llame al método **UpdateObject** .</span><span class="sxs-lookup"><span data-stu-id="faad6-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="faad6-200">La actualización se realiza cuando se llama a **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="faad6-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="faad6-201">De forma predeterminada, WCF envía una solicitud HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="faad6-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="faad6-202">La opción **PatchOnUpdate** indica a WCF que envíe una revisión http en su lugar.</span><span class="sxs-lookup"><span data-stu-id="faad6-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="faad6-203">¿Por qué PATCH y MERGE?</span><span class="sxs-lookup"><span data-stu-id="faad6-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="faad6-204">La especificación HTTP 1,1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no definió ningún método http con la semántica "actualización parcial".</span><span class="sxs-lookup"><span data-stu-id="faad6-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="faad6-205">Para admitir las actualizaciones parciales, la especificación de OData definía el método MERGE.</span><span class="sxs-lookup"><span data-stu-id="faad6-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="faad6-206">En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definía el método patch para actualizaciones parciales.</span><span class="sxs-lookup"><span data-stu-id="faad6-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="faad6-207">Puede leer parte del historial de esta entrada de [blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el blog de WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="faad6-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="faad6-208">En la actualidad, la revisión es preferible a la combinación.</span><span class="sxs-lookup"><span data-stu-id="faad6-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="faad6-209">El controlador OData creado por el scaffolding de API Web admite ambos métodos.</span><span class="sxs-lookup"><span data-stu-id="faad6-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>

<span data-ttu-id="faad6-210">Si desea reemplazar toda la entidad (semántica de PUT), especifique la opción **ReplaceOnUpdate** .</span><span class="sxs-lookup"><span data-stu-id="faad6-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="faad6-211">Esto hace que WCF envíe una solicitud HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="faad6-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="faad6-212">Eliminar una entidad</span><span class="sxs-lookup"><span data-stu-id="faad6-212">Delete an Entity</span></span>

<span data-ttu-id="faad6-213">Para eliminar una entidad, llame a **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="faad6-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="faad6-214">Invocar una acción de OData</span><span class="sxs-lookup"><span data-stu-id="faad6-214">Invoke an OData Action</span></span>

<span data-ttu-id="faad6-215">En OData, [las acciones](odata-actions.md) son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades.</span><span class="sxs-lookup"><span data-stu-id="faad6-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="faad6-216">Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos.</span><span class="sxs-lookup"><span data-stu-id="faad6-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="faad6-217">Todavía puede invocar una acción de OData mediante el método **Execute** genérico.</span><span class="sxs-lookup"><span data-stu-id="faad6-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="faad6-218">Sin embargo, tendrá que conocer los tipos de datos de los parámetros y el valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="faad6-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="faad6-219">Por ejemplo, la acción `RateProduct` toma el parámetro denominado "rating" de tipo `Int32` y devuelve una `double`.</span><span class="sxs-lookup"><span data-stu-id="faad6-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="faad6-220">En el código siguiente se muestra cómo invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="faad6-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="faad6-221">Para obtener más información, consulte[llamar a operaciones y acciones de servicio](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="faad6-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="faad6-222">Una opción consiste en extender la clase de **contenedor** para proporcionar un método fuertemente tipado que invoca la acción:</span><span class="sxs-lookup"><span data-stu-id="faad6-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
