---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: actualizaciones finales para la navegación y el diseño del sitio, conclusión | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 10 abarca las actualizaciones finales de navegación y...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433615"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="0e07a-104">Parte 10: actualizaciones finales para la navegación y el diseño del sitio, conclusión</span><span class="sxs-lookup"><span data-stu-id="0e07a-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="0e07a-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0e07a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0e07a-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="0e07a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0e07a-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="0e07a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0e07a-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="0e07a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0e07a-109">La parte 10 cubre las actualizaciones finales de la navegación y el diseño del sitio, conclusión.</span><span class="sxs-lookup"><span data-stu-id="0e07a-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="0e07a-110">Hemos completado toda la funcionalidad principal de nuestro sitio, pero todavía tenemos algunas características para agregar a la navegación del sitio, la Página principal y la página de exploración de la tienda.</span><span class="sxs-lookup"><span data-stu-id="0e07a-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="0e07a-111">Crear la vista parcial de resumen del carro de la compra</span><span class="sxs-lookup"><span data-stu-id="0e07a-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="0e07a-112">Queremos exponer el número de artículos en el carro de la compra del usuario en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="0e07a-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="0e07a-113">Esto se puede implementar fácilmente mediante la creación de una vista parcial que se agrega a nuestro sitio. Master.</span><span class="sxs-lookup"><span data-stu-id="0e07a-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="0e07a-114">Como se mostró anteriormente, el controlador ShoppingCart incluye un método de acción CartSummary que devuelve una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="0e07a-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="0e07a-115">Para crear la vista parcial CartSummary, haga clic con el botón derecho en la carpeta views/ShoppingCart y seleccione Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="0e07a-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="0e07a-116">Asigne a la vista el nombre CartSummary y active la casilla "crear una vista parcial" como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="0e07a-117">La vista parcial CartSummary es realmente sencilla, solo es un vínculo a la vista de índice ShoppingCart, que muestra el número de elementos del carro.</span><span class="sxs-lookup"><span data-stu-id="0e07a-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="0e07a-118">El código completo para CartSummary. cshtml es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e07a-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="0e07a-119">Podemos incluir una vista parcial en cualquier página del sitio, incluido el maestro del sitio, mediante el método html. RenderAction.</span><span class="sxs-lookup"><span data-stu-id="0e07a-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="0e07a-120">RenderAction requiere que se especifique el nombre de la acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="0e07a-121">Antes de agregarlo al diseño del sitio, también crearemos el menú género para que podamos hacer todas las actualizaciones de site. Master al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="0e07a-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="0e07a-122">Crear la vista parcial del menú género</span><span class="sxs-lookup"><span data-stu-id="0e07a-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="0e07a-123">Podemos facilitar a los usuarios la navegación por la tienda mediante la adición de un menú de género que enumera todos los géneros disponibles en nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="0e07a-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="0e07a-124">Seguiremos los mismos pasos también creará una vista parcial de GenreMenu y, a continuación, se pueden agregar ambos al maestro de sitio.</span><span class="sxs-lookup"><span data-stu-id="0e07a-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="0e07a-125">En primer lugar, agregue la siguiente acción del controlador GenreMenu a StoreController:</span><span class="sxs-lookup"><span data-stu-id="0e07a-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="0e07a-126">Esta acción devuelve una lista de géneros que se mostrarán en la vista parcial, que se creará a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="0e07a-127">*Nota: hemos agregado el atributo [ChildActionOnly] a esta acción del controlador, lo que indica que solo deseamos usar esta acción desde una vista parcial. Este atributo impedirá que se ejecute la acción de controlador examinando/Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es recomendable, ya que queremos asegurarnos de que las acciones del controlador se usen como se pretende. También devolvemos PartialView en lugar de View, lo que permite que el motor de vistas sepa que no debe usar el diseño de esta vista, ya que se incluye en otras vistas.*</span><span class="sxs-lookup"><span data-stu-id="0e07a-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="0e07a-128">Haga clic con el botón derecho en la acción del controlador GenreMenu y cree una vista parcial denominada GenreMenu que esté fuertemente tipada mediante la clase de datos de la vista Genre, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="0e07a-129">Actualice el código de vista de la vista parcial GenreMenu para mostrar los elementos mediante una lista sin ordenar como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="0e07a-130">Actualizar el diseño del sitio para mostrar nuestras vistas parciales</span><span class="sxs-lookup"><span data-stu-id="0e07a-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="0e07a-131">Podemos agregar nuestras vistas parciales al diseño del sitio (/Views/Shared/\_layout. cshtml) llamando a HTML. RenderAction ().</span><span class="sxs-lookup"><span data-stu-id="0e07a-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="0e07a-132">Los agregaremos en, así como algunas marcas adicionales para mostrarlas, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="0e07a-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="0e07a-133">Ahora, cuando ejecutemos la aplicación, veremos el género en el área de navegación izquierda y el Resumen de la cesta en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="0e07a-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="0e07a-134">Actualizar a la página de exploración de la tienda</span><span class="sxs-lookup"><span data-stu-id="0e07a-134">Update to the Store Browse page</span></span>

<span data-ttu-id="0e07a-135">La página de exploración del almacén es funcional, pero no se ve muy bien.</span><span class="sxs-lookup"><span data-stu-id="0e07a-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="0e07a-136">Podemos actualizar la página para mostrar los álbumes en un mejor diseño; para ello, actualice el código de vista (que se encuentra en/Views/Store/Browse.cshtml) de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="0e07a-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="0e07a-137">Aquí estamos haciendo uso de URL. Action en lugar de HTML. ActionLink para que podamos aplicar formato especial al vínculo para incluir la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="0e07a-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="0e07a-138">*Nota: se muestra una portada de álbum genérica para estos álbumes. Esta información se almacena en la base de datos y se puede modificar a través del administrador de la tienda. Le agradecemos que agregue su propia ilustración.*</span><span class="sxs-lookup"><span data-stu-id="0e07a-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="0e07a-139">Ahora, cuando examinemos un género, veremos que los álbumes se muestran en una cuadrícula con la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="0e07a-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="0e07a-140">Actualización de la Página principal para mostrar los principales álbumes de ventas</span><span class="sxs-lookup"><span data-stu-id="0e07a-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="0e07a-141">Queremos incluir nuestros principales álbumes de ventas en la Página principal para aumentar las ventas.</span><span class="sxs-lookup"><span data-stu-id="0e07a-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="0e07a-142">Realizaremos algunas actualizaciones en el HomeController para controlar eso y agregar también algunos gráficos adicionales.</span><span class="sxs-lookup"><span data-stu-id="0e07a-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="0e07a-143">En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase album para que EntityFramework sepa que están asociadas.</span><span class="sxs-lookup"><span data-stu-id="0e07a-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="0e07a-144">Las últimas líneas de la clase **album** deben tener ahora el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="0e07a-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="0e07a-145">*Nota: Esto requerirá agregar una instrucción using para traer el espacio de nombres System. Collections. Generic.*</span><span class="sxs-lookup"><span data-stu-id="0e07a-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="0e07a-146">En primer lugar, vamos a agregar un campo storeDB y las instrucciones MvcMusicStore. Models con, como en nuestros otros controladores.</span><span class="sxs-lookup"><span data-stu-id="0e07a-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="0e07a-147">A continuación, agregaremos el método siguiente al HomeController, que consulta nuestra base de datos para encontrar los álbumes principales de ventas según OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="0e07a-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="0e07a-148">Se trata de un método privado, ya que no queremos que esté disponible como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="0e07a-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="0e07a-149">Lo incluimos en el HomeController por simplicidad, pero le recomendamos que mueva la lógica de negocios a clases de servicio independientes, según corresponda.</span><span class="sxs-lookup"><span data-stu-id="0e07a-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="0e07a-150">Con eso en su lugar, podemos actualizar la acción del controlador de índice para consultar los 5 álbumes principales y devolverlos a la vista.</span><span class="sxs-lookup"><span data-stu-id="0e07a-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="0e07a-151">El código completo para el HomeController actualizado es como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="0e07a-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="0e07a-152">Por último, deberá actualizar nuestra vista de índice principal para que pueda mostrar una lista de álbumes; para ello, actualice el tipo de modelo y agregue la lista de álbumes a la parte inferior.</span><span class="sxs-lookup"><span data-stu-id="0e07a-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="0e07a-153">Tomaremos esta oportunidad para agregar también un encabezado y una sección de promoción a la página.</span><span class="sxs-lookup"><span data-stu-id="0e07a-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="0e07a-154">Ahora, cuando ejecutemos la aplicación, veremos nuestra página principal actualizada con los principales álbumes de ventas y nuestro mensaje promocional.</span><span class="sxs-lookup"><span data-stu-id="0e07a-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="0e07a-155">Conclusión</span><span class="sxs-lookup"><span data-stu-id="0e07a-155">Conclusion</span></span>

<span data-ttu-id="0e07a-156">Hemos visto que ASP.NET MVC facilita la creación de un sitio web sofisticado con acceso a bases de datos, pertenencia, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="0e07a-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="0e07a-157">con bastante rapidez.</span><span class="sxs-lookup"><span data-stu-id="0e07a-157">pretty quickly.</span></span> <span data-ttu-id="0e07a-158">Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear sus propias aplicaciones ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0e07a-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0e07a-159">Anterior</span><span class="sxs-lookup"><span data-stu-id="0e07a-159">Previous</span></span>](mvc-music-store-part-9.md)
