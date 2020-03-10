---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Mostrar elementos de datos y detalles | Microsoft Docs
author: Erikre
description: En esta serie de tutoriales se muestran los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520813"
---
# <a name="display-data-items-and-details"></a><span data-ttu-id="fdfed-103">Mostrar elementos de datos y detalles</span><span class="sxs-lookup"><span data-stu-id="fdfed-103">Display data items and details</span></span>

<span data-ttu-id="fdfed-104">por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="fdfed-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="fdfed-105">Esta serie de tutoriales le enseña los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="fdfed-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="fdfed-106">En este tutorial, aprenderá a mostrar los elementos de datos y los detalles de los elementos de datos con los formularios Web Forms de ASP.NET y Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="fdfed-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="fdfed-107">Este tutorial se basa en el tutorial anterior "interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toy Store.</span><span class="sxs-lookup"><span data-stu-id="fdfed-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="fdfed-108">Después de completar este tutorial, verá productos en la página *ProductsList. aspx* y los detalles de un producto en la página *productdetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="fdfed-109">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="fdfed-109">You'll learn how to:</span></span>

- <span data-ttu-id="fdfed-110">Agregar un control de datos para mostrar los productos de la base de datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="fdfed-111">Conectar un control de datos a los datos seleccionados</span><span class="sxs-lookup"><span data-stu-id="fdfed-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="fdfed-112">Agregar un control de datos para mostrar los detalles del producto de la base de datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="fdfed-113">Recuperar un valor de la cadena de consulta y usar ese valor para limitar los datos que se recuperan de la base de datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="fdfed-114">Características introducidas en este tutorial:</span><span class="sxs-lookup"><span data-stu-id="fdfed-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="fdfed-115">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="fdfed-115">Model binding</span></span>
- <span data-ttu-id="fdfed-116">Proveedores de valores</span><span class="sxs-lookup"><span data-stu-id="fdfed-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="fdfed-117">Agregar un control de datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-117">Add a data control</span></span>

<span data-ttu-id="fdfed-118">Puede usar algunas opciones diferentes para enlazar datos a un control de servidor.</span><span class="sxs-lookup"><span data-stu-id="fdfed-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="fdfed-119">Entre las más comunes se incluyen:</span><span class="sxs-lookup"><span data-stu-id="fdfed-119">The most common include:</span></span>

* <span data-ttu-id="fdfed-120">Agregar un control de origen de datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-120">Adding a data source control</span></span>
* <span data-ttu-id="fdfed-121">Agregar código manualmente</span><span class="sxs-lookup"><span data-stu-id="fdfed-121">Adding code by hand</span></span>
* <span data-ttu-id="fdfed-122">Usar el enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="fdfed-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="fdfed-123">Usar un control de origen de datos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-123">Use a data source control to bind data</span></span>

<span data-ttu-id="fdfed-124">Agregar un control de origen de datos le permite vincular el control de origen de datos al control que muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="fdfed-125">Con este enfoque, puede, en lugar de hacerlo mediante programación, conectar controles del lado servidor a los orígenes de datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="fdfed-126">Código a mano para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-126">Code by hand to bind data</span></span>

<span data-ttu-id="fdfed-127">La codificación a mano implica:</span><span class="sxs-lookup"><span data-stu-id="fdfed-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="fdfed-128">Leer un valor</span><span class="sxs-lookup"><span data-stu-id="fdfed-128">Reading a value</span></span>
2. <span data-ttu-id="fdfed-129">Comprobando si es null</span><span class="sxs-lookup"><span data-stu-id="fdfed-129">Checking if it's null</span></span>
3. <span data-ttu-id="fdfed-130">Convertirlo en un tipo adecuado</span><span class="sxs-lookup"><span data-stu-id="fdfed-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="fdfed-131">Comprobando la conversión correcta</span><span class="sxs-lookup"><span data-stu-id="fdfed-131">Checking conversion success</span></span>
5. <span data-ttu-id="fdfed-132">Usar el valor de la consulta</span><span class="sxs-lookup"><span data-stu-id="fdfed-132">Using the value in the query</span></span> 

<span data-ttu-id="fdfed-133">Este enfoque le permite tener control total sobre la lógica de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="fdfed-134">Usar el enlace de modelos para enlazar datos</span><span class="sxs-lookup"><span data-stu-id="fdfed-134">Use model binding to bind data</span></span>

<span data-ttu-id="fdfed-135">El enlace de modelos permite enlazar los resultados con mucho menos código y ofrece la posibilidad de reutilizar la funcionalidad en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdfed-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="fdfed-136">Simplifica el trabajo con la lógica de acceso a datos centrada en el código y, al mismo tiempo, proporciona un marco de enlace de datos enriquecido.</span><span class="sxs-lookup"><span data-stu-id="fdfed-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="fdfed-137">Mostrar productos</span><span class="sxs-lookup"><span data-stu-id="fdfed-137">Display products</span></span>

<span data-ttu-id="fdfed-138">En este tutorial, usará el enlace de modelos para enlazar datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="fdfed-139">Para configurar un control de datos para que use el enlace de modelos para seleccionar datos, establezca la propiedad `SelectMethod` del control en un nombre de método en el código de la página.</span><span class="sxs-lookup"><span data-stu-id="fdfed-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="fdfed-140">El control de datos llama al método en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="fdfed-141">No es necesario llamar explícitamente al método `DataBind`.</span><span class="sxs-lookup"><span data-stu-id="fdfed-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="fdfed-142">En **Explorador de soluciones**, Abra *productlist. aspx*.</span><span class="sxs-lookup"><span data-stu-id="fdfed-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="fdfed-143">Reemplace el marcado existente por este marcado:</span><span class="sxs-lookup"><span data-stu-id="fdfed-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="fdfed-144">Este código usa un control **ListView** denominado `productList` para mostrar los productos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="fdfed-145">Con las plantillas y los estilos, se define el modo en que el control **ListView** muestra los datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="fdfed-146">Es útil para los datos de cualquier estructura de repetición.</span><span class="sxs-lookup"><span data-stu-id="fdfed-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="fdfed-147">Aunque en este ejemplo de **ListView** solo se muestran los datos de la base de datos, también puede, sin código, permitir a los usuarios editar, insertar y eliminar datos, así como ordenar y paginar los datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="fdfed-148">Al establecer la propiedad `ItemType` en el control **ListView** , la expresión de enlace de datos `Item` está disponible y el control se vuelve fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="fdfed-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="fdfed-149">Como se mencionó en el tutorial anterior, puede seleccionar detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="fdfed-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Mostrar elementos de datos y detalles: IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="fdfed-151">También está utilizando el enlace de modelos para especificar un valor `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="fdfed-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="fdfed-152">Este valor (`GetProducts`) corresponde al método que agregará al código subyacente para mostrar los productos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="fdfed-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="fdfed-153">Agregar código para mostrar productos</span><span class="sxs-lookup"><span data-stu-id="fdfed-153">Add code to display products</span></span>

<span data-ttu-id="fdfed-154">En este paso, agregará código para rellenar el control **ListView** con datos de productos de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="fdfed-155">El código admite la visualización de todos los productos y productos de categoría individuales.</span><span class="sxs-lookup"><span data-stu-id="fdfed-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="fdfed-156">En **Explorador de soluciones**, haga clic con el botón secundario en *productlist. aspx* y seleccione **Ver código**.</span><span class="sxs-lookup"><span data-stu-id="fdfed-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="fdfed-157">Reemplace el código existente en el archivo *productlist.aspx.CS* por este:</span><span class="sxs-lookup"><span data-stu-id="fdfed-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="fdfed-158">Este código muestra el método `GetProducts` al que hace referencia la propiedad `ItemType` del control **ListView** en la página *productlist. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="fdfed-159">Para limitar los resultados a una categoría de base de datos específica, el código establece el valor `categoryId` del valor de la cadena de consulta que se pasa a la página *productlist. aspx* cuando se navega a la página *productlist. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="fdfed-160">La clase `QueryStringAttribute` del espacio de nombres `System.Web.ModelBinding` se utiliza para recuperar el valor de la variable de cadena de consulta `id`.</span><span class="sxs-lookup"><span data-stu-id="fdfed-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="fdfed-161">Esto indica al enlace de modelos que intente enlazar un valor de la cadena de consulta con el parámetro `categoryId` en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="fdfed-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="fdfed-162">Cuando se pasa una categoría válida como una cadena de consulta a la página, los resultados de la consulta se limitan a los productos de la base de datos que coinciden con el valor de `categoryId`.</span><span class="sxs-lookup"><span data-stu-id="fdfed-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="fdfed-163">Por ejemplo, si la dirección URL de la página *ProductsList. aspx* es esta:</span><span class="sxs-lookup"><span data-stu-id="fdfed-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="fdfed-164">En la página solo se muestran los productos en los que el `categoryId` es igual a `1`.</span><span class="sxs-lookup"><span data-stu-id="fdfed-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="fdfed-165">Se muestran todos los productos si no se incluye ninguna cadena de consulta cuando se llama a la página *productlist. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="fdfed-166">Los orígenes de los valores de estos métodos se conocen como *proveedores de valores* (como *QueryString*), y los atributos de parámetro que indican qué proveedor de valor se va a usar se conocen como *atributos de proveedor de valor* (como `id`).</span><span class="sxs-lookup"><span data-stu-id="fdfed-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="fdfed-167">ASP.NET incluye proveedores de valores y atributos correspondientes para todos los orígenes típicos de datos proporcionados por el usuario en una aplicación de formularios Web Forms, como la cadena de consulta, las cookies, los valores de formulario, los controles, el estado de la vista, el estado de la sesión y las propiedades del perfil.</span><span class="sxs-lookup"><span data-stu-id="fdfed-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="fdfed-168">También puede escribir proveedores de valores personalizados.</span><span class="sxs-lookup"><span data-stu-id="fdfed-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="fdfed-169">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="fdfed-169">Run the application</span></span>

<span data-ttu-id="fdfed-170">Ejecute la aplicación ahora para ver todos los productos o los productos de una categoría.</span><span class="sxs-lookup"><span data-stu-id="fdfed-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="fdfed-171">Presione **F5** mientras está en Visual Studio para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdfed-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="fdfed-172">El explorador se abre y muestra la página *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="fdfed-173">Seleccione **automóviles** en el menú de navegación categoría de producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="fdfed-174">La página *productlist. aspx* muestra solo los productos de categoría **Cars** .</span><span class="sxs-lookup"><span data-stu-id="fdfed-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="fdfed-175">Más adelante en este tutorial, se mostrarán los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Mostrar elementos de datos y detalles: automóviles](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="fdfed-177">Seleccione **productos** en el menú de navegación de la parte superior.</span><span class="sxs-lookup"><span data-stu-id="fdfed-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="fdfed-178">Una vez más, se muestra la página *productlist. aspx* , pero esta vez muestra toda la lista de productos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Mostrar elementos de datos y detalles: productos](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="fdfed-180">Cierre el explorador y vuelva a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fdfed-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="fdfed-181">Agregar un control de datos para mostrar los detalles del producto</span><span class="sxs-lookup"><span data-stu-id="fdfed-181">Add a data control to display product details</span></span>

<span data-ttu-id="fdfed-182">A continuación, modificará el marcado en la página *productdetails. aspx* que agregó en el tutorial anterior para mostrar información específica del producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="fdfed-183">En **Explorador de soluciones**, Abra *productdetails. aspx*.</span><span class="sxs-lookup"><span data-stu-id="fdfed-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="fdfed-184">Reemplace el marcado existente por este marcado:</span><span class="sxs-lookup"><span data-stu-id="fdfed-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="fdfed-185">Este código usa un control **FormView** para mostrar detalles concretos del producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="fdfed-186">Este marcado usa métodos como los métodos que se usan para Mostrar datos en la página *productlist. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="fdfed-187">El control **FormView** se utiliza para mostrar un único registro a la vez desde un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="fdfed-188">Cuando se usa el control **FormView** , se crean plantillas para mostrar y editar valores enlazados a datos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="fdfed-189">Estas plantillas contienen controles, expresiones de enlace y formatos que definen la apariencia y la funcionalidad del formulario.</span><span class="sxs-lookup"><span data-stu-id="fdfed-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="fdfed-190">La conexión del marcado anterior a la base de datos requiere código adicional.</span><span class="sxs-lookup"><span data-stu-id="fdfed-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="fdfed-191">En **Explorador de soluciones**, haga clic con el botón secundario en *productdetails. aspx* y, a continuación, haga clic en **Ver código**.</span><span class="sxs-lookup"><span data-stu-id="fdfed-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="fdfed-192">Se muestra el archivo *productdetails.aspx.CS* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="fdfed-193">Reemplace el código existente con este código:</span><span class="sxs-lookup"><span data-stu-id="fdfed-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="fdfed-194">Este código comprueba un valor de cadena de consulta "`productID`".</span><span class="sxs-lookup"><span data-stu-id="fdfed-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="fdfed-195">Si se encuentra un valor de cadena de consulta válido, se muestra el producto correspondiente.</span><span class="sxs-lookup"><span data-stu-id="fdfed-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="fdfed-196">Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="fdfed-197">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="fdfed-197">Run the application</span></span>

<span data-ttu-id="fdfed-198">Ahora puede ejecutar la aplicación para ver un producto individual que se muestra según el ID. de producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="fdfed-199">Presione **F5** mientras está en Visual Studio para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fdfed-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="fdfed-200">El explorador se abre y muestra la página *default. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="fdfed-201">Seleccione **barcos** en el menú de navegación categoría.</span><span class="sxs-lookup"><span data-stu-id="fdfed-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="fdfed-202">Se muestra la página *productlist. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="fdfed-203">Seleccione el **barco de papel** en la lista de productos.</span><span class="sxs-lookup"><span data-stu-id="fdfed-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="fdfed-204">Se muestra la página *productdetails. aspx* .</span><span class="sxs-lookup"><span data-stu-id="fdfed-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Mostrar elementos de datos y detalles: productos](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="fdfed-206">Cierre el explorador.</span><span class="sxs-lookup"><span data-stu-id="fdfed-206">Close the browser.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fdfed-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="fdfed-207">Additional resources</span></span>

[<span data-ttu-id="fdfed-208">Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="fdfed-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="fdfed-209">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="fdfed-209">Next steps</span></span>

<span data-ttu-id="fdfed-210">En este tutorial, ha agregado marcado y código para mostrar los productos y los detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="fdfed-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="fdfed-211">Ha aprendido acerca de los controles de datos fuertemente tipados, el enlace de modelos y los proveedores de valores.</span><span class="sxs-lookup"><span data-stu-id="fdfed-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="fdfed-212">En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="fdfed-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="fdfed-213">[Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="fdfed-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
