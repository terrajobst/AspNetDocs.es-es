---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carro de la compra con actualizaciones de Ajax | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 8 abarca el carro de la compra con actualizaciones de Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433519"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="5b88c-104">Parte 8: carro de la compra con actualizaciones de Ajax</span><span class="sxs-lookup"><span data-stu-id="5b88c-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="5b88c-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="5b88c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="5b88c-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="5b88c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="5b88c-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="5b88c-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="5b88c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="5b88c-109">La parte 8 abarca el carro de la compra con actualizaciones de Ajax.</span><span class="sxs-lookup"><span data-stu-id="5b88c-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="5b88c-110">Permitiremos a los usuarios colocar álbumes en el carro sin registrarse, pero tendrán que registrarse como invitados para completar la desprotección.</span><span class="sxs-lookup"><span data-stu-id="5b88c-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="5b88c-111">El proceso de compra y retirada se divide en dos controladores: un controlador ShoppingCart que permite agregar elementos de forma anónima a un carro y un controlador de desprotección que controla el proceso de desprotección.</span><span class="sxs-lookup"><span data-stu-id="5b88c-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="5b88c-112">Comenzaremos con el carro de la compra en esta sección y, a continuación, compilaremos el proceso de desprotección en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b88c-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="5b88c-113">Adición de las clases de modelo Cart, order y OrderDetail</span><span class="sxs-lookup"><span data-stu-id="5b88c-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="5b88c-114">Nuestro carro de la compra y los procesos de retirada harán uso de algunas clases nuevas.</span><span class="sxs-lookup"><span data-stu-id="5b88c-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="5b88c-115">Haga clic con el botón secundario en la carpeta models y agregue una clase Cart (Cart.cs) con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b88c-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="5b88c-116">Esta clase es bastante similar a la de los demás que hemos usado hasta ahora, excepto el atributo [Key] de la propiedad RecordId.</span><span class="sxs-lookup"><span data-stu-id="5b88c-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="5b88c-117">Nuestros elementos del carro tendrán un identificador de cadena denominado CartID para permitir compras anónimas, pero la tabla incluye una clave principal de entero denominada RecordId.</span><span class="sxs-lookup"><span data-stu-id="5b88c-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="5b88c-118">Por Convención, Entity Framework Code-First espera que la clave principal de una tabla denominada Cart sea CartId o ID, pero se puede reemplazar fácilmente por las anotaciones o el código si se desea.</span><span class="sxs-lookup"><span data-stu-id="5b88c-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="5b88c-119">Este es un ejemplo de cómo podemos usar las convenciones simples en Entity Framework Code-First cuando nos acompañan, pero no están restringidos por ellos cuando no lo están.</span><span class="sxs-lookup"><span data-stu-id="5b88c-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="5b88c-120">A continuación, agregue una clase de pedido (Order.cs) con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b88c-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="5b88c-121">Esta clase realiza un seguimiento de la información de Resumen y entrega de un pedido.</span><span class="sxs-lookup"><span data-stu-id="5b88c-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="5b88c-122">**No se compilará todavía**, porque tiene una propiedad de navegación OrderDetails que depende de una clase que aún no se ha creado.</span><span class="sxs-lookup"><span data-stu-id="5b88c-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="5b88c-123">Vamos a corregir esto ahora agregando una clase denominada OrderDetail.cs, agregando el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b88c-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="5b88c-124">Haremos una última actualización a nuestra clase MusicStoreEntities para incluir DbSets que exponga las nuevas clases de modelo, incluido también un DbSet&lt;intérprete&gt;.</span><span class="sxs-lookup"><span data-stu-id="5b88c-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="5b88c-125">La clase MusicStoreEntities actualizada aparece como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5b88c-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="5b88c-126">Administrar la lógica de negocios de la cesta de la compra</span><span class="sxs-lookup"><span data-stu-id="5b88c-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="5b88c-127">A continuación, vamos a crear la clase ShoppingCart en la carpeta models.</span><span class="sxs-lookup"><span data-stu-id="5b88c-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="5b88c-128">El modelo ShoppingCart controla el acceso a los datos a la tabla Cart.</span><span class="sxs-lookup"><span data-stu-id="5b88c-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="5b88c-129">Además, administrará la lógica de negocios para agregar y quitar elementos del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="5b88c-130">Dado que no es necesario que los usuarios se suscriban a una cuenta solo para agregar elementos a su carro de la compra, se asignará a los usuarios un identificador único temporal (mediante un GUID o un identificador único global) cuando accedan al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="5b88c-131">Almacenaremos este identificador mediante la clase de sesión ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b88c-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="5b88c-132">*Nota: la sesión ASP.NET es un lugar adecuado para almacenar información específica del usuario que expirará después de abandonar el sitio. Aunque el uso incorrecto del estado de la sesión puede tener implicaciones en el rendimiento de los sitios de mayor tamaño, nuestro ligero uso funcionará bien con fines de demostración.*</span><span class="sxs-lookup"><span data-stu-id="5b88c-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="5b88c-133">La clase ShoppingCart expone los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="5b88c-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="5b88c-134">**AddToCart** toma un álbum como parámetro y lo agrega al carro del usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="5b88c-135">Dado que la tabla Cart realiza un seguimiento de la cantidad de cada álbum, incluye la lógica para crear una nueva fila si es necesario o simplemente incrementar la cantidad si el usuario ya ha pedido una copia del álbum.</span><span class="sxs-lookup"><span data-stu-id="5b88c-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="5b88c-136">**RemoveFromCart** toma un identificador de álbum y lo quita del carro del usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="5b88c-137">Si el usuario solo tenía una copia del álbum en el carro, se quita la fila.</span><span class="sxs-lookup"><span data-stu-id="5b88c-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="5b88c-138">**EmptyCart** quita todos los elementos del carro de la compra de un usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="5b88c-139">**GetCartItems** recupera una lista de CartItems para su presentación o procesamiento.</span><span class="sxs-lookup"><span data-stu-id="5b88c-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="5b88c-140">**GetCount** recupera el número total de álbumes que un usuario tiene en su carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="5b88c-141">**GetTotal** calcula el costo total de todos los elementos del carro.</span><span class="sxs-lookup"><span data-stu-id="5b88c-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="5b88c-142">**CreateOrder** convierte el carro de la compra en un pedido durante la fase de desprotección.</span><span class="sxs-lookup"><span data-stu-id="5b88c-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="5b88c-143">**GetCart** es un método estático que permite a nuestros controladores obtener un objeto Cart.</span><span class="sxs-lookup"><span data-stu-id="5b88c-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="5b88c-144">Usa el método **GetCartId** para controlar la lectura de CartId desde la sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="5b88c-145">El método GetCartId requiere HttpContextBase para que pueda leer el CartId del usuario de la sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="5b88c-146">Esta es la **clase ShoppingCart**completa:</span><span class="sxs-lookup"><span data-stu-id="5b88c-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="5b88c-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="5b88c-147">ViewModels</span></span>

<span data-ttu-id="5b88c-148">Nuestro controlador de carro de la compra deberá comunicar información compleja a sus vistas, lo que no se asigna correctamente a los objetos del modelo.</span><span class="sxs-lookup"><span data-stu-id="5b88c-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="5b88c-149">No queremos modificar nuestros modelos para que se adapten a nuestras vistas; Las clases de modelo deben representar nuestro dominio, no la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="5b88c-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="5b88c-150">Una solución sería pasar la información a nuestras vistas mediante la clase ViewBag, como hicimos con la información de la lista desplegable del administrador de la tienda, pero pasar mucha información a través de ViewBag es difícil de administrar.</span><span class="sxs-lookup"><span data-stu-id="5b88c-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="5b88c-151">Una solución a esto es usar el patrón *ViewModel* .</span><span class="sxs-lookup"><span data-stu-id="5b88c-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="5b88c-152">Al usar este patrón, creamos clases fuertemente tipadas que están optimizadas para nuestros escenarios de vista específicos y que exponen propiedades para los valores dinámicos y el contenido que necesitan nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="5b88c-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="5b88c-153">A continuación, las clases de controlador pueden rellenar y pasar estas clases optimizadas para vistas a nuestra plantilla de vista que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="5b88c-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="5b88c-154">Esto permite la seguridad de tipos, la comprobación en tiempo de compilación y el IntelliSense del editor dentro de las plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="5b88c-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="5b88c-155">Crearemos dos modelos de vista para su uso en nuestro controlador de carro de la compra: el ShoppingCartViewModel contendrá el contenido del carro de la compra del usuario y ShoppingCartRemoveViewModel se usará para mostrar información de confirmación cuando un usuario quite algo desde el carro.</span><span class="sxs-lookup"><span data-stu-id="5b88c-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="5b88c-156">Vamos a crear una nueva carpeta ViewModels en la raíz de nuestro proyecto para mantener las cosas organizadas.</span><span class="sxs-lookup"><span data-stu-id="5b88c-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="5b88c-157">Haga clic con el botón derecho en el proyecto, seleccione Agregar o nueva carpeta.</span><span class="sxs-lookup"><span data-stu-id="5b88c-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="5b88c-158">Asigne a la carpeta el nombre ViewModels.</span><span class="sxs-lookup"><span data-stu-id="5b88c-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="5b88c-159">A continuación, agregue la clase ShoppingCartViewModel en la carpeta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="5b88c-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="5b88c-160">Tiene dos propiedades: una lista de elementos del carro y un valor decimal para contener el precio total de todos los artículos del carro.</span><span class="sxs-lookup"><span data-stu-id="5b88c-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="5b88c-161">Ahora, agregue ShoppingCartRemoveViewModel a la carpeta ViewModels, con las cuatro propiedades siguientes.</span><span class="sxs-lookup"><span data-stu-id="5b88c-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="5b88c-162">Controlador de carro de la compra</span><span class="sxs-lookup"><span data-stu-id="5b88c-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="5b88c-163">El controlador de carro de la compra tiene tres propósitos principales: agregar elementos a un carro, quitar elementos del carro y ver los elementos del carro.</span><span class="sxs-lookup"><span data-stu-id="5b88c-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="5b88c-164">Usará las tres clases que acabamos de crear: ShoppingCartViewModel, ShoppingCartRemoveViewModel y ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="5b88c-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="5b88c-165">Como en StoreController y StoreManagerController, agregaremos un campo que contenga una instancia de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="5b88c-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="5b88c-166">Agregue un nuevo controlador de carro de la compra al proyecto mediante la plantilla de controlador vacía.</span><span class="sxs-lookup"><span data-stu-id="5b88c-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="5b88c-167">Este es el controlador ShoppingCart completo.</span><span class="sxs-lookup"><span data-stu-id="5b88c-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="5b88c-168">Las acciones index y Add Controller deben tener un aspecto muy familiar.</span><span class="sxs-lookup"><span data-stu-id="5b88c-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="5b88c-169">Las acciones de controlador Remove y CartSummary controlan dos casos especiales, que trataremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="5b88c-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="5b88c-170">Actualizaciones de AJAX con jQuery</span><span class="sxs-lookup"><span data-stu-id="5b88c-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="5b88c-171">A continuación, crearemos una página de índice de carro de la compra fuertemente tipada a ShoppingCartViewModel y utilizará la plantilla de vista de lista con el mismo método que antes.</span><span class="sxs-lookup"><span data-stu-id="5b88c-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="5b88c-172">Sin embargo, en lugar de usar un elemento HTML. ActionLink para quitar elementos del carro, usaremos jQuery para "conectar" el evento de clic de todos los vínculos de esta vista que tengan la clase HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="5b88c-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="5b88c-173">En lugar de publicar el formulario, este controlador de eventos de clic solo realizará una devolución de llamada AJAX a nuestra acción del controlador RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="5b88c-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="5b88c-174">RemoveFromCart devuelve un resultado serializado de JSON, que la devolución de llamada de jQuery analiza y realiza cuatro actualizaciones rápidas en la página con jQuery:</span><span class="sxs-lookup"><span data-stu-id="5b88c-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="5b88c-175">Quita el álbum eliminado de la lista.</span><span class="sxs-lookup"><span data-stu-id="5b88c-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="5b88c-176">Actualiza el número de carros en el encabezado.</span><span class="sxs-lookup"><span data-stu-id="5b88c-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="5b88c-177">Muestra un mensaje de actualización al usuario</span><span class="sxs-lookup"><span data-stu-id="5b88c-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="5b88c-178">Actualiza el precio total del carro</span><span class="sxs-lookup"><span data-stu-id="5b88c-178">Updates the cart total price</span></span>

<span data-ttu-id="5b88c-179">Dado que el escenario de eliminación está siendo controlado por una devolución de llamada de Ajax en la vista de índice, no se necesita una vista adicional para la acción RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="5b88c-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="5b88c-180">Este es el código completo de la vista/ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="5b88c-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="5b88c-181">Para probar esto, es necesario poder agregar elementos al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="5b88c-182">Actualizaremos la vista de detalles de la **tienda** para incluir un botón "agregar al carro".</span><span class="sxs-lookup"><span data-stu-id="5b88c-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="5b88c-183">Aunque estamos en él, podemos incluir parte de la información adicional del álbum que hemos agregado desde la última vez que se actualizó esta vista: género, intérprete, precio y carátula del álbum.</span><span class="sxs-lookup"><span data-stu-id="5b88c-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="5b88c-184">El código de vista de detalles del almacén actualizado aparece como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="5b88c-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="5b88c-185">Ahora podemos hacer clic en la tienda y probar agregar y quitar álbumes en el carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="5b88c-186">Ejecute la aplicación y vaya al índice de la tienda.</span><span class="sxs-lookup"><span data-stu-id="5b88c-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="5b88c-187">A continuación, haga clic en un género para ver una lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="5b88c-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="5b88c-188">Al hacer clic en el título de un álbum, se muestra la vista de detalles del álbum actualizada, incluido el botón "agregar al carro".</span><span class="sxs-lookup"><span data-stu-id="5b88c-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="5b88c-189">Al hacer clic en el botón "agregar al carro", se muestra nuestra vista de índice de carro de la compra con la lista de resumen del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="5b88c-190">Después de cargar el carro de la compra, puede hacer clic en el vínculo quitar del carro para ver la actualización de Ajax en el carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="5b88c-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="5b88c-191">Hemos creado un carro de la compra que funciona, lo que permite a los usuarios no registrados agregar elementos a su carro.</span><span class="sxs-lookup"><span data-stu-id="5b88c-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="5b88c-192">En la sección siguiente, se les permitirá registrar y completar el proceso de desprotección.</span><span class="sxs-lookup"><span data-stu-id="5b88c-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b88c-193">[Anterior](mvc-music-store-part-7.md)
> [Siguiente](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="5b88c-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
