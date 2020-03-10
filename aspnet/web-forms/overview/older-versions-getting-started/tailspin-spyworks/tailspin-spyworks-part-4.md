---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: enumeración de productos | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. En la parte 4 se describe la lista de productos con el contr de GridView...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457285"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="d3d78-104">Parte 4: lista de productos</span><span class="sxs-lookup"><span data-stu-id="d3d78-104">Part 4: Listing Products</span></span>

<span data-ttu-id="d3d78-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d3d78-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d3d78-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="d3d78-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d3d78-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="d3d78-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d3d78-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="d3d78-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d3d78-109">En la parte 4 se describe la lista de productos con el control GridView.</span><span class="sxs-lookup"><span data-stu-id="d3d78-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="d3d78-110">Enumerar productos con el control GridView</span><span class="sxs-lookup"><span data-stu-id="d3d78-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="d3d78-111">Comencemos a implementar nuestra página de ProductsList. aspx haciendo clic con el botón derecho en la solución y seleccionando "agregar" y "nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="d3d78-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="d3d78-112">Elija "formulario web usando la página maestra" y escriba un nombre de página de ProductsList. aspx ".</span><span class="sxs-lookup"><span data-stu-id="d3d78-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="d3d78-113">Haga clic en "agregar".</span><span class="sxs-lookup"><span data-stu-id="d3d78-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="d3d78-114">A continuación, elija la carpeta "Styles" donde colocamos la página site. Master y selecciónela en la ventana "contenido de la carpeta".</span><span class="sxs-lookup"><span data-stu-id="d3d78-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="d3d78-115">Haga clic en "Aceptar" para crear la página.</span><span class="sxs-lookup"><span data-stu-id="d3d78-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="d3d78-116">Nuestra base de datos se rellena con datos de productos, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="d3d78-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="d3d78-117">Después de crear la página, volveremos a usar un origen de datos de entidad para acceder a los datos del producto, pero en esta instancia necesitamos seleccionar las entidades Product y necesitamos restringir los elementos que se devuelven solo a los de la categoría seleccionada.</span><span class="sxs-lookup"><span data-stu-id="d3d78-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="d3d78-118">Para ello, indicaremos a EntityDataSource que genere automáticamente la cláusula WHERE y especificaremos WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="d3d78-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="d3d78-119">Recordará que cuando creamos los elementos de menú en nuestro "menú de categoría de producto", creamos dinámicamente el vínculo agregando el CategoryID a la cadena de incorporación de cada vínculo.</span><span class="sxs-lookup"><span data-stu-id="d3d78-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="d3d78-120">Se indicará al origen de datos de la entidad que derive el parámetro WHERE de ese parámetro QueryString.</span><span class="sxs-lookup"><span data-stu-id="d3d78-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="d3d78-121">A continuación, configuraremos el control ListView para mostrar una lista de productos.</span><span class="sxs-lookup"><span data-stu-id="d3d78-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="d3d78-122">Para crear una experiencia de compra óptima, compactaremos varias características concisas en cada producto individual mostrado en nuestro ListVew.</span><span class="sxs-lookup"><span data-stu-id="d3d78-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="d3d78-123">El nombre del producto será un vínculo a la vista de detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="d3d78-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="d3d78-124">Se mostrará el precio del producto.</span><span class="sxs-lookup"><span data-stu-id="d3d78-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="d3d78-125">Se mostrará una imagen del producto y seleccionaremos dinámicamente la imagen en un directorio de imágenes de catálogo en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="d3d78-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="d3d78-126">Se incluirá un vínculo para agregar inmediatamente el producto específico al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="d3d78-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="d3d78-127">Este es el marcado de la instancia del control ListView.</span><span class="sxs-lookup"><span data-stu-id="d3d78-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="d3d78-128">Vamos a crear dinámicamente varios vínculos para cada producto mostrado.</span><span class="sxs-lookup"><span data-stu-id="d3d78-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="d3d78-129">Además, antes de probar la nueva página, es necesario crear la estructura de directorios para las imágenes del catálogo de productos como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="d3d78-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="d3d78-130">Una vez que se pueda acceder a nuestras imágenes de producto, podemos probar nuestra página de lista de productos.</span><span class="sxs-lookup"><span data-stu-id="d3d78-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="d3d78-131">En la Página principal del sitio, haga clic en uno de los vínculos de la lista de categorías.</span><span class="sxs-lookup"><span data-stu-id="d3d78-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="d3d78-132">Ahora es necesario implementar la página ProductDetails. aspx y la funcionalidad AddToCart.</span><span class="sxs-lookup"><span data-stu-id="d3d78-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="d3d78-133">Use el&gt;de archivo nuevo para crear un nombre de página ProductDetails. aspx mediante la página maestra del sitio como hicimos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d3d78-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="d3d78-134">De nuevo, usaremos un control EntityDataSource para tener acceso al registro del producto específico en la base de datos y usaremos un control ASP.NET FormView para mostrar los datos del producto como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="d3d78-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="d3d78-135">No se preocupe si el formato le parece un poco divertido.</span><span class="sxs-lookup"><span data-stu-id="d3d78-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="d3d78-136">El marcado anterior deja espacio en el diseño de la pantalla para un par de características que se implementarán más adelante.</span><span class="sxs-lookup"><span data-stu-id="d3d78-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="d3d78-137">El carro de la compra representa la lógica más compleja de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="d3d78-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="d3d78-138">Para empezar, use el&gt;de archivo nuevo para crear una página denominada MyShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="d3d78-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="d3d78-139">Tenga en cuenta que no estamos eligiendo el nombre ShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="d3d78-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="d3d78-140">Nuestra base de datos contiene una tabla denominada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="d3d78-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="d3d78-141">Cuando se genera una Entity Data Model se creó una clase para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d3d78-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="d3d78-142">Por lo tanto, el Entity Data Model genera una clase de entidad denominada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="d3d78-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="d3d78-143">Podríamos editar el modelo para que podamos usar ese nombre para nuestra implementación de la cesta de la compra o ampliarlo según nuestras necesidades, pero optaremos por simplemente seleccionar un nombre que evite el conflicto.</span><span class="sxs-lookup"><span data-stu-id="d3d78-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="d3d78-144">También merece la pena tener en cuenta que vamos a crear un carro de la compra sencillo e insertar la lógica del carro de la compra con la pantalla del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="d3d78-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="d3d78-145">También podemos optar por implementar el carro de la compra en un nivel de negocio completamente independiente.</span><span class="sxs-lookup"><span data-stu-id="d3d78-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3d78-146">[Anterior](tailspin-spyworks-part-3.md)
> [Siguiente](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d3d78-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
