---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: agregar características | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 7 se agregan características adicionales, como la cuenta Revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521569"
---
# <a name="part-7-adding-features"></a><span data-ttu-id="0d845-104">Parte 7: agregar características</span><span class="sxs-lookup"><span data-stu-id="0d845-104">Part 7: Adding Features</span></span>

<span data-ttu-id="0d845-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0d845-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0d845-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="0d845-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0d845-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="0d845-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0d845-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0d845-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0d845-109">La parte 7 agrega características adicionales, como la revisión de la cuenta, las revisiones del producto y los "elementos populares" y los controles de usuario "también adquiridos".</span><span class="sxs-lookup"><span data-stu-id="0d845-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>

## <a id="_Toc260221673"></a><span data-ttu-id="0d845-110">Agregar características</span><span class="sxs-lookup"><span data-stu-id="0d845-110">Adding Features</span></span>

<span data-ttu-id="0d845-111">Aunque los usuarios pueden examinar nuestro catálogo, colocar los elementos en su carro de la compra y completar el proceso de retirada, hay una serie de características de soporte técnico que se incluirán para mejorar nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="0d845-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="0d845-112">Revisión de la cuenta (lista de pedidos colocados y ver detalles).</span><span class="sxs-lookup"><span data-stu-id="0d845-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="0d845-113">Agregue contenido específico del contexto a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="0d845-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="0d845-114">Agregue una característica para permitir que los usuarios revisen los productos en el catálogo.</span><span class="sxs-lookup"><span data-stu-id="0d845-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="0d845-115">Cree un control de usuario para mostrar los elementos populares y coloque ese control en la Página principal.</span><span class="sxs-lookup"><span data-stu-id="0d845-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="0d845-116">Cree un control de usuario "comprado también" y agréguelo a la página de detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="0d845-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="0d845-117">Agregue una página de contacto.</span><span class="sxs-lookup"><span data-stu-id="0d845-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="0d845-118">Agregue una página about.</span><span class="sxs-lookup"><span data-stu-id="0d845-118">Add an About Page.</span></span>
8. <span data-ttu-id="0d845-119">Error global</span><span class="sxs-lookup"><span data-stu-id="0d845-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="0d845-120">Revisión de la cuenta</span><span class="sxs-lookup"><span data-stu-id="0d845-120">Account Review</span></span>

<span data-ttu-id="0d845-121">En la carpeta "cuenta", cree dos páginas. aspx una denominada OrderList. aspx y la otra denominada OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="0d845-122">OrderList. aspx aprovechará los controles GridView y EntityDataSource de la misma forma que antes.</span><span class="sxs-lookup"><span data-stu-id="0d845-122">OrderList.aspx will leverage the GridView and EntityDataSource controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="0d845-123">EntityDataSource selecciona los registros de la tabla Orders filtrados en el nombre de usuario (vea WhereParameter), que se establece en una variable de sesión cuando el usuario inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="0d845-123">The EntityDataSource selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="0d845-124">Tenga en cuenta también estos parámetros en el HyperlinkField de GridView:</span><span class="sxs-lookup"><span data-stu-id="0d845-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="0d845-125">Estos parámetros especifican el vínculo a la vista de detalles de pedidos de cada producto que especifica el campo OrderID como parámetro QueryString en la página OrderDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="0d845-126">OrderDetails. aspx</span><span class="sxs-lookup"><span data-stu-id="0d845-126">OrderDetails.aspx</span></span>

<span data-ttu-id="0d845-127">Usaremos un control EntityDataSource para acceder a los pedidos y un objeto FormView para mostrar los datos de pedidos y otro EntityDataSource con un control GridView para mostrar todos los elementos de línea del pedido.</span><span class="sxs-lookup"><span data-stu-id="0d845-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="0d845-128">En el archivo de código subyacente (OrderDetails.aspx.cs) tenemos dos pequeños bits de mantenimiento.</span><span class="sxs-lookup"><span data-stu-id="0d845-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="0d845-129">En primer lugar, debemos asegurarnos de que OrderDetails siempre obtiene un OrderId.</span><span class="sxs-lookup"><span data-stu-id="0d845-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="0d845-130">También necesitamos calcular y mostrar el total del pedido a partir de los artículos de línea.</span><span class="sxs-lookup"><span data-stu-id="0d845-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="0d845-131">Página principal</span><span class="sxs-lookup"><span data-stu-id="0d845-131">The Home Page</span></span>

<span data-ttu-id="0d845-132">Vamos a agregar contenido estático a la página default. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="0d845-133">En primer lugar, crearé una carpeta de "contenido" y, dentro de ella, una carpeta de imágenes (y incluiré una imagen que se usará en la Página principal).</span><span class="sxs-lookup"><span data-stu-id="0d845-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="0d845-134">En el marcador de posición inferior de la página default. aspx, agregue el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="0d845-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="0d845-135">Revisiones del producto</span><span class="sxs-lookup"><span data-stu-id="0d845-135">Product Reviews</span></span>

<span data-ttu-id="0d845-136">En primer lugar, vamos a agregar un botón con un vínculo a un formulario que se puede usar para escribir una revisión del producto.</span><span class="sxs-lookup"><span data-stu-id="0d845-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="0d845-137">Tenga en cuenta que vamos a pasar el ProductID en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0d845-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="0d845-138">A continuación, vamos a agregar la página denominada ReviewAdd. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="0d845-139">Esta página usará el kit de herramientas de control de ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="0d845-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="0d845-140">Si no lo ha hecho todavía, puede descargarlo de [DevExpress](http://devexpress.com/act) y encontrará instrucciones sobre cómo configurar el kit de herramientas para su uso con Visual Studio aquí [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="0d845-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="0d845-141">En el modo de diseño, arrastre los controles y validadores desde el cuadro de herramientas y genere un formulario como el que aparece a continuación.</span><span class="sxs-lookup"><span data-stu-id="0d845-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="0d845-142">El marcado tendrá un aspecto similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="0d845-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="0d845-143">Ahora que podemos especificar revisiones, muestre esas revisiones en la página del producto.</span><span class="sxs-lookup"><span data-stu-id="0d845-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="0d845-144">Agregue este marcado a la página ProductDetails. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="0d845-145">La ejecución de la aplicación ahora y la navegación a un producto muestra la información del producto, incluidas las opiniones de los clientes.</span><span class="sxs-lookup"><span data-stu-id="0d845-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="0d845-146">Control de elementos populares (creación de controles de usuario)</span><span class="sxs-lookup"><span data-stu-id="0d845-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="0d845-147">Con el fin de aumentar las ventas en el sitio web, agregaremos un par de características a los productos más populares o relacionados.</span><span class="sxs-lookup"><span data-stu-id="0d845-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="0d845-148">La primera de estas características será una lista del producto más popular en el catálogo de productos.</span><span class="sxs-lookup"><span data-stu-id="0d845-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="0d845-149">Vamos a crear un "control de usuario" para mostrar los elementos más vendidos en la Página principal de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d845-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="0d845-150">Dado que se trata de un control, podemos usarlo en cualquier página arrastrando y colocando el control en el diseñador de Visual Studio en cualquier página que desee.</span><span class="sxs-lookup"><span data-stu-id="0d845-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="0d845-151">En el explorador de soluciones de Visual Studio, haga clic con el botón derecho en el nombre de la solución y cree un nuevo directorio denominado "controles".</span><span class="sxs-lookup"><span data-stu-id="0d845-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="0d845-152">Aunque no es necesario hacerlo, le ayudaremos a mantener el proyecto organizado creando todos nuestros controles de usuario en el directorio "controles".</span><span class="sxs-lookup"><span data-stu-id="0d845-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="0d845-153">Haga clic con el botón derecho en la carpeta controles y elija "nuevo elemento":</span><span class="sxs-lookup"><span data-stu-id="0d845-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="0d845-154">Especifique un nombre para el control "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="0d845-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="0d845-155">Tenga en cuenta que la extensión de archivo para los controles de usuario es. ascx no. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="0d845-156">El control de usuario de los elementos más populares se definirá como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="0d845-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="0d845-157">Aquí vamos a usar un método que aún no se ha usado en esta aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d845-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="0d845-158">Usamos el control Repeater y, en lugar de usar un control de origen de datos, enlazamos el control Repeater a los resultados de una consulta LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="0d845-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="0d845-159">En el código subyacente de nuestro control, hacemos esto como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="0d845-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="0d845-160">Tenga en cuenta también esta línea importante en la parte superior del marcado del control.</span><span class="sxs-lookup"><span data-stu-id="0d845-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="0d845-161">Dado que los elementos más populares no cambiarán de un minuto a minuto, podemos agregar una directiva Aching para mejorar el rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d845-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="0d845-162">Esta directiva hará que el código de controles solo se ejecute cuando expire la salida almacenada en caché del control.</span><span class="sxs-lookup"><span data-stu-id="0d845-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="0d845-163">De lo contrario, se usará la versión almacenada en caché de la salida del control.</span><span class="sxs-lookup"><span data-stu-id="0d845-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="0d845-164">Ahora todo lo que tenemos que hacer es incluir nuestro nuevo control en la página default. aspx.</span><span class="sxs-lookup"><span data-stu-id="0d845-164">Now all we have to do is include our new control in our Default.aspx page.</span></span>

<span data-ttu-id="0d845-165">Use las opciones de arrastrar y colocar para colocar una instancia del control en la columna abrir del formulario predeterminado.</span><span class="sxs-lookup"><span data-stu-id="0d845-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="0d845-166">Ahora, cuando se ejecuta la aplicación, la Página principal muestra los elementos más populares.</span><span class="sxs-lookup"><span data-stu-id="0d845-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="0d845-167">Control "también adquirido" (controles de usuario con parámetros)</span><span class="sxs-lookup"><span data-stu-id="0d845-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="0d845-168">El segundo control de usuario que crearemos llevará a cabo una venta por debajo del siguiente nivel agregando la especificidad del contexto.</span><span class="sxs-lookup"><span data-stu-id="0d845-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="0d845-169">La lógica para calcular los elementos principales "comprados también" no es trivial.</span><span class="sxs-lookup"><span data-stu-id="0d845-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="0d845-170">El control "también adquirido" seleccionará los registros de OrderDetails (previamente comprados) para el ProductID seleccionado actualmente y tomará el OrderIDs para cada orden único que se encuentre.</span><span class="sxs-lookup"><span data-stu-id="0d845-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="0d845-171">A continuación, seleccionaremos los productos de todos los pedidos y sumaremos las cantidades compradas.</span><span class="sxs-lookup"><span data-stu-id="0d845-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="0d845-172">Ordenaremos los productos por esa suma de cantidad y mostraremos los cinco elementos principales.</span><span class="sxs-lookup"><span data-stu-id="0d845-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="0d845-173">Dada la complejidad de esta lógica, se implementará este algoritmo como un procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="0d845-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="0d845-174">El T-SQL para el procedimiento almacenado es el siguiente.</span><span class="sxs-lookup"><span data-stu-id="0d845-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="0d845-175">Tenga en cuenta que este procedimiento almacenado (SelectPurchasedWithProducts) existía en la base de datos cuando lo incluimos en nuestra aplicación y, cuando generamos el Entity Data Model especificamos que, además de las tablas y vistas que necesitábamos, el Entity Data Model debe incluir este procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="0d845-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="0d845-176">Para tener acceso al procedimiento almacenado desde el Entity Data Model necesitamos importar la función.</span><span class="sxs-lookup"><span data-stu-id="0d845-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="0d845-177">Haga doble clic en el Entity Data Model en el explorador de soluciones para abrirlo en el diseñador y abra el explorador de modelos, haga clic con el botón derecho en el diseñador y seleccione "agregar importación de función".</span><span class="sxs-lookup"><span data-stu-id="0d845-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="0d845-178">Si lo hace, se abrirá este cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="0d845-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="0d845-179">Rellene los campos como se muestra arriba, seleccionando "SelectPurchasedWithProducts" y usando el nombre del procedimiento para el nombre de la función importada.</span><span class="sxs-lookup"><span data-stu-id="0d845-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="0d845-180">Haga clic en "Aceptar".</span><span class="sxs-lookup"><span data-stu-id="0d845-180">Click "Ok".</span></span>

<span data-ttu-id="0d845-181">Una vez hecho esto, podemos simplemente programar con el procedimiento almacenado, como podríamos hacer con cualquier otro elemento del modelo.</span><span class="sxs-lookup"><span data-stu-id="0d845-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="0d845-182">Por lo tanto, en la carpeta "controles", cree un nuevo control de usuario denominado AlsoPurchased. ascx.</span><span class="sxs-lookup"><span data-stu-id="0d845-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="0d845-183">El marcado de este control resultará muy familiar al control PopularItems.</span><span class="sxs-lookup"><span data-stu-id="0d845-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="0d845-184">La diferencia más importante es que no almacena en caché la salida, ya que el elemento que se va a representar será diferente según el producto.</span><span class="sxs-lookup"><span data-stu-id="0d845-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="0d845-185">ProductId será una "propiedad" para el control.</span><span class="sxs-lookup"><span data-stu-id="0d845-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="0d845-186">En el controlador de eventos de preprocesamiento del control, se EED hacer tres cosas.</span><span class="sxs-lookup"><span data-stu-id="0d845-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="0d845-187">Asegúrese de que ProductID está establecido.</span><span class="sxs-lookup"><span data-stu-id="0d845-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="0d845-188">Compruebe si hay algún producto que se haya comprado con el actual.</span><span class="sxs-lookup"><span data-stu-id="0d845-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="0d845-189">Genere algunos elementos según se determine en #2.</span><span class="sxs-lookup"><span data-stu-id="0d845-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="0d845-190">Tenga en cuenta lo fácil que es llamar al procedimiento almacenado a través del modelo.</span><span class="sxs-lookup"><span data-stu-id="0d845-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="0d845-191">Después de determinar que hay "también adquirido", podemos simplemente enlazar el repetidor a los resultados devueltos por la consulta.</span><span class="sxs-lookup"><span data-stu-id="0d845-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="0d845-192">Si no había ningún elemento "también adquirido", simplemente mostraremos otros elementos populares de nuestro catálogo.</span><span class="sxs-lookup"><span data-stu-id="0d845-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="0d845-193">Para ver los elementos "comprados también", abra la página ProductDetails. aspx y arrastre el control AlsoPurchased desde el explorador de soluciones para que aparezca en esta posición en el marcado.</span><span class="sxs-lookup"><span data-stu-id="0d845-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="0d845-194">Al hacerlo, se creará una referencia al control en la parte superior de la página ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="0d845-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="0d845-195">Dado que el control de usuario AlsoPurchased requiere un número ProductId, estableceremos la propiedad ProductID del control mediante una instrucción eval en el elemento de modelo de datos actual de la página.</span><span class="sxs-lookup"><span data-stu-id="0d845-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="0d845-196">Al compilar y ejecutar ahora y buscar un producto, vemos los elementos "comprados también".</span><span class="sxs-lookup"><span data-stu-id="0d845-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="0d845-197">[Anterior](tailspin-spyworks-part-6.md)
> [Siguiente](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="0d845-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
