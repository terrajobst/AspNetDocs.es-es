---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: pertenencia a ASP.NET | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 6 agrega la pertenencia a ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454885"
---
# <a name="part-6-aspnet-membership"></a><span data-ttu-id="b9877-104">Parte 6: pertenencia a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9877-104">Part 6: ASP.NET Membership</span></span>

<span data-ttu-id="b9877-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="b9877-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="b9877-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="b9877-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="b9877-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="b9877-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="b9877-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="b9877-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="b9877-109">La parte 6 agrega la pertenencia a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b9877-109">Part 6 adds ASP.NET Membership.</span></span>

## <a id="_Toc260221672"></a><span data-ttu-id="b9877-110">Trabajar con pertenencia a ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b9877-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="b9877-111">Haga clic en seguridad</span><span class="sxs-lookup"><span data-stu-id="b9877-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="b9877-112">Asegúrese de usar la autenticación de formularios.</span><span class="sxs-lookup"><span data-stu-id="b9877-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="b9877-113">Use el vínculo "crear usuario" para crear un par de usuarios.</span><span class="sxs-lookup"><span data-stu-id="b9877-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="b9877-114">Cuando haya terminado, consulte la ventana de Explorador de soluciones y actualice la vista.</span><span class="sxs-lookup"><span data-stu-id="b9877-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="b9877-115">Tenga en cuenta que ASPNETDB. Se ha creado el estado de MDF.</span><span class="sxs-lookup"><span data-stu-id="b9877-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="b9877-116">Este archivo contiene las tablas que admiten los servicios principales de ASP.NET, como la pertenencia.</span><span class="sxs-lookup"><span data-stu-id="b9877-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="b9877-117">Ahora podemos empezar a implementar el proceso de desprotección.</span><span class="sxs-lookup"><span data-stu-id="b9877-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="b9877-118">Empiece por crear una página CheckOut. aspx.</span><span class="sxs-lookup"><span data-stu-id="b9877-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="b9877-119">La página CheckOut. aspx solo debe estar disponible para los usuarios que hayan iniciado sesión, por lo que restringiremos el acceso a los usuarios que han iniciado sesión y redirigirá a los usuarios que no hayan iniciado sesión en la página de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="b9877-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="b9877-120">Para ello, agregaremos lo siguiente a la sección de configuración del archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="b9877-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="b9877-121">La plantilla para aplicaciones de formularios Web Forms de ASP.NET agregó automáticamente una sección de autenticación al archivo Web. config y estableció la página de inicio de sesión predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b9877-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="b9877-122">Se debe modificar el archivo de código subyacente login. aspx para migrar un carro de la compra anónimo cuando el usuario inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="b9877-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="b9877-123">Cambie la página\_evento de carga como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="b9877-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="b9877-124">Después, agregue un controlador de eventos "Loggedon" como este para establecer el nombre de sesión en el usuario que acaba de iniciar sesión y cambiar el identificador de sesión temporal en el carro de la compra por el del usuario mediante una llamada al método MigrateCart de nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="b9877-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="b9877-125">(Se implementa en el archivo. CS)</span><span class="sxs-lookup"><span data-stu-id="b9877-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="b9877-126">Implemente el método MigrateCart () como este.</span><span class="sxs-lookup"><span data-stu-id="b9877-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="b9877-127">En Checkout. aspx, usaremos una EntityDataSource y un GridView en nuestra página de extracción, como hicimos en nuestra página de carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="b9877-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="b9877-128">Tenga en cuenta que el control GridView especifica un controlador de eventos "OnDataBound" denominado mi lista\_RowDataBound, así que vamos a implementar ese controlador de eventos como este.</span><span class="sxs-lookup"><span data-stu-id="b9877-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="b9877-129">Este método mantiene un total actualizado del carro de la compra, ya que cada fila está enlazada y actualiza la fila inferior de GridView.</span><span class="sxs-lookup"><span data-stu-id="b9877-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="b9877-130">En esta fase hemos implementado una presentación de "revisión" del pedido que se va a colocar.</span><span class="sxs-lookup"><span data-stu-id="b9877-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="b9877-131">Vamos a controlar un escenario de carro vacío agregando algunas líneas de código a nuestra página\_evento de carga:</span><span class="sxs-lookup"><span data-stu-id="b9877-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="b9877-132">Cuando el usuario haga clic en el botón "enviar", ejecutaremos el siguiente código en el controlador de eventos Click del botón Enviar.</span><span class="sxs-lookup"><span data-stu-id="b9877-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="b9877-133">La "carne" del proceso de envío de pedidos se va a implementar en el método SubmitOrder () de nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="b9877-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="b9877-134">SubmitOrder:</span><span class="sxs-lookup"><span data-stu-id="b9877-134">SubmitOrder will:</span></span>

- <span data-ttu-id="b9877-135">Tome todos los elementos de línea en el carro de la compra y úselos para crear un nuevo registro de pedido y los registros OrderDetails asociados.</span><span class="sxs-lookup"><span data-stu-id="b9877-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="b9877-136">Calcular la fecha de envío.</span><span class="sxs-lookup"><span data-stu-id="b9877-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="b9877-137">Borre el carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="b9877-137">Clear the shopping cart.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="b9877-138">Para los fines de esta aplicación de ejemplo, calcularemos una fecha de envío simplemente agregando dos días a la fecha actual.</span><span class="sxs-lookup"><span data-stu-id="b9877-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="b9877-139">Ejecutar la aplicación ahora nos permitirá probar el proceso de compra desde el principio hasta el final.</span><span class="sxs-lookup"><span data-stu-id="b9877-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b9877-140">[Anterior](tailspin-spyworks-part-5.md)
> [Siguiente](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="b9877-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
