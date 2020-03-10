---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: registro y desprotección | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 9 cubre el registro y la desprotección.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 040bc0ccef889fb9a7c3d9b5ce88c75b7b754248
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450901"
---
# <a name="part-9-registration-and-checkout"></a><span data-ttu-id="163b3-104">Parte 9: registro y desprotección</span><span class="sxs-lookup"><span data-stu-id="163b3-104">Part 9: Registration and Checkout</span></span>

<span data-ttu-id="163b3-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="163b3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="163b3-106">El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="163b3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="163b3-107">MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="163b3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="163b3-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="163b3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="163b3-109">La parte 9 cubre el registro y la desprotección.</span><span class="sxs-lookup"><span data-stu-id="163b3-109">Part 9 covers Registration and Checkout.</span></span>

<span data-ttu-id="163b3-110">En esta sección, se creará un CheckoutController que recopilará la dirección del comprador y la información de pago.</span><span class="sxs-lookup"><span data-stu-id="163b3-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="163b3-111">Se requerirá a los usuarios que se registren en nuestro sitio antes de desprotegerlo, por lo que este controlador necesitará autorización.</span><span class="sxs-lookup"><span data-stu-id="163b3-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="163b3-112">Los usuarios navegarán hasta el proceso de desprotección desde el carro de la compra haciendo clic en el botón "desproteger".</span><span class="sxs-lookup"><span data-stu-id="163b3-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="163b3-113">Si el usuario no ha iniciado sesión, se le pedirá que lo indique.</span><span class="sxs-lookup"><span data-stu-id="163b3-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="163b3-114">Una vez que el inicio de sesión se realiza correctamente, se muestra la vista dirección y pago en el usuario.</span><span class="sxs-lookup"><span data-stu-id="163b3-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="163b3-115">Una vez que haya rellenado el formulario y enviado el pedido, se mostrará la pantalla de confirmación del pedido.</span><span class="sxs-lookup"><span data-stu-id="163b3-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="163b3-116">Al intentar ver un pedido que no existe o un pedido que no pertenece a, se mostrará la vista de error.</span><span class="sxs-lookup"><span data-stu-id="163b3-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="163b3-117">Migración del carro de la compra</span><span class="sxs-lookup"><span data-stu-id="163b3-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="163b3-118">Aunque el proceso de compra es anónimo, cuando el usuario hace clic en el botón de desprotección, se le pedirá que registre e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="163b3-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="163b3-119">Los usuarios esperarán que mantengan la información de su carro de la compra entre visitas, por lo que tendremos que asociar la información del carro de la compra con un usuario cuando completen el registro o el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="163b3-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="163b3-120">Esto es realmente muy sencillo, ya que nuestra clase ShoppingCart ya tiene un método que asociará todos los elementos del carro actual con un nombre de usuario.</span><span class="sxs-lookup"><span data-stu-id="163b3-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="163b3-121">Solo tendremos que llamar a este método cuando un usuario complete el registro o el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="163b3-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="163b3-122">Abra la clase **AccountController** que hemos agregado al configurar la pertenencia y la autorización.</span><span class="sxs-lookup"><span data-stu-id="163b3-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="163b3-123">Agregue una instrucción using que haga referencia a MvcMusicStore. Models y, después, agregue el siguiente método MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="163b3-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="163b3-124">A continuación, modifique la acción de inicio de sesión para llamar a MigrateShoppingCart una vez que se haya validado el usuario, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="163b3-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="163b3-125">Realice el mismo cambio en la acción registrar entrada inmediatamente después de que se haya creado correctamente la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="163b3-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="163b3-126">Es decir, ahora un carro de la compra anónimo se transferirá automáticamente a una cuenta de usuario al registrarse o iniciar sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="163b3-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="163b3-127">Crear CheckoutController</span><span class="sxs-lookup"><span data-stu-id="163b3-127">Creating the CheckoutController</span></span>

<span data-ttu-id="163b3-128">Haga clic con el botón derecho en la carpeta Controllers y agregue un nuevo controlador al proyecto denominado CheckoutController mediante la plantilla de controlador vacía.</span><span class="sxs-lookup"><span data-stu-id="163b3-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="163b3-129">En primer lugar, agregue el atributo Authorize por encima de la declaración de clase de controlador para requerir que los usuarios se registren antes de la desprotección:</span><span class="sxs-lookup"><span data-stu-id="163b3-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="163b3-130">*Nota: esto es similar al cambio realizado anteriormente en el StoreManagerController, pero en ese caso el atributo Authorize requiere que el usuario tenga un rol de administrador. En el controlador de desprotección, se requiere que el usuario inicie sesión pero no requiere que sean administradores.*</span><span class="sxs-lookup"><span data-stu-id="163b3-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="163b3-131">Por motivos de simplicidad, no trataremos la información de pago en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="163b3-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="163b3-132">En su lugar, se permite a los usuarios consultar mediante un código promocional.</span><span class="sxs-lookup"><span data-stu-id="163b3-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="163b3-133">Almacenaremos este código promocional mediante una constante denominada PromoCode.</span><span class="sxs-lookup"><span data-stu-id="163b3-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="163b3-134">Como en StoreController, se declarará un campo que contenga una instancia de la clase MusicStoreEntities, denominada storeDB.</span><span class="sxs-lookup"><span data-stu-id="163b3-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="163b3-135">Para hacer uso de la clase MusicStoreEntities, debemos agregar una instrucción using para el espacio de nombres MvcMusicStore. Models.</span><span class="sxs-lookup"><span data-stu-id="163b3-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="163b3-136">A continuación se muestra la parte superior del controlador de retirada.</span><span class="sxs-lookup"><span data-stu-id="163b3-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="163b3-137">CheckoutController tendrá las siguientes acciones de controlador:</span><span class="sxs-lookup"><span data-stu-id="163b3-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="163b3-138">**AddressAndPayment (get Method)** mostrará un formulario para permitir que el usuario escriba su información.</span><span class="sxs-lookup"><span data-stu-id="163b3-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="163b3-139">**AddressAndPayment (método post)** validará la entrada y procesará el pedido.</span><span class="sxs-lookup"><span data-stu-id="163b3-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="163b3-140">**Completar** se mostrará después de que un usuario haya finalizado correctamente el proceso de desprotección.</span><span class="sxs-lookup"><span data-stu-id="163b3-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="163b3-141">Esta vista incluirá el número de pedido del usuario, como confirmación.</span><span class="sxs-lookup"><span data-stu-id="163b3-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="163b3-142">En primer lugar, vamos a cambiar el nombre de la acción del controlador de índice (que se generó cuando se creó el controlador) a AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="163b3-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="163b3-143">Esta acción del controlador solo muestra el formulario de desprotección, por lo que no requiere información del modelo.</span><span class="sxs-lookup"><span data-stu-id="163b3-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="163b3-144">Nuestro método POST de AddressAndPayment seguirá el mismo patrón que usamos en StoreManagerController: intentará aceptar el envío del formulario y completar el pedido, y volverá a mostrar el formulario si se produce un error.</span><span class="sxs-lookup"><span data-stu-id="163b3-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="163b3-145">Después de validar que la entrada del formulario cumple nuestros requisitos de validación para un pedido, se comprobará el valor del formulario PromoCode directamente.</span><span class="sxs-lookup"><span data-stu-id="163b3-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="163b3-146">Suponiendo que todo sea correcto, guardaremos la información actualizada con el pedido, le indicaremos al objeto ShoppingCart que complete el proceso de pedido y le redirigirá a la acción completa.</span><span class="sxs-lookup"><span data-stu-id="163b3-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="163b3-147">Una vez finalizado correctamente el proceso de desprotección, se redirigirá a los usuarios a la acción de controlador completa.</span><span class="sxs-lookup"><span data-stu-id="163b3-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="163b3-148">Esta acción realizará una comprobación simple para validar que el pedido pertenece realmente al usuario que ha iniciado sesión antes de mostrar el número de pedido como una confirmación.</span><span class="sxs-lookup"><span data-stu-id="163b3-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="163b3-149">*Nota: la vista de error se creó automáticamente en la carpeta/Views/Shared cuando comenzó el proyecto.*</span><span class="sxs-lookup"><span data-stu-id="163b3-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="163b3-150">El código de CheckoutController completo es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="163b3-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="163b3-151">Adición de la vista AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="163b3-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="163b3-152">Ahora, vamos a crear la vista AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="163b3-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="163b3-153">Haga clic con el botón derecho en una de las acciones del controlador AddressAndPayment y agregue una vista denominada AddressAndPayment que esté fuertemente tipada como un orden y use la plantilla Edit, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="163b3-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="163b3-154">Esta vista hará uso de dos de las técnicas que hemos examinado durante la compilación de la vista StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="163b3-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="163b3-155">Usaremos HTML. EditorForModel () para mostrar campos de formulario para el modelo de pedido</span><span class="sxs-lookup"><span data-stu-id="163b3-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="163b3-156">Se aprovecharán las reglas de validación mediante una clase Order con atributos de validación</span><span class="sxs-lookup"><span data-stu-id="163b3-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="163b3-157">Comenzaremos por actualizar el código de formulario para usar HTML. EditorForModel (), seguido de un cuadro de texto adicional para el código de promoción.</span><span class="sxs-lookup"><span data-stu-id="163b3-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="163b3-158">A continuación se muestra el código completo de la vista AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="163b3-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="163b3-159">Definir reglas de validación para el pedido</span><span class="sxs-lookup"><span data-stu-id="163b3-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="163b3-160">Ahora que nuestra vista está configurada, se configurarán las reglas de validación para nuestro modelo de pedido, tal y como hicimos anteriormente para el modelo de álbumes.</span><span class="sxs-lookup"><span data-stu-id="163b3-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="163b3-161">Haga clic con el botón derecho en la carpeta modelos y agregue una clase denominada order.</span><span class="sxs-lookup"><span data-stu-id="163b3-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="163b3-162">Además de los atributos de validación que se usaron anteriormente para el álbum, también se usará una expresión regular para validar la dirección de correo electrónico del usuario.</span><span class="sxs-lookup"><span data-stu-id="163b3-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="163b3-163">Al intentar enviar el formulario con información que falta o no es válida, ahora se muestra un mensaje de error mediante la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="163b3-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="163b3-164">Bien, hemos realizado la mayor parte del trabajo duro para el proceso de desprotección; solo tenemos algunas probabilidades y acaban de finalizar.</span><span class="sxs-lookup"><span data-stu-id="163b3-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="163b3-165">Necesitamos agregar dos vistas simples, y es necesario encargarse de la entrega de la información del carro durante el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="163b3-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="163b3-166">Adición de la vista de desprotección completada</span><span class="sxs-lookup"><span data-stu-id="163b3-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="163b3-167">La vista de desprotección completa es bastante sencilla, ya que solo necesita mostrar el identificador de pedido.</span><span class="sxs-lookup"><span data-stu-id="163b3-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="163b3-168">Haga clic con el botón derecho en la acción de controlador completa y agregue una vista denominada completo que esté fuertemente tipada como un valor int.</span><span class="sxs-lookup"><span data-stu-id="163b3-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="163b3-169">Ahora actualizaremos el código de vista para mostrar el identificador de pedido, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="163b3-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="163b3-170">Actualización de la vista de errores</span><span class="sxs-lookup"><span data-stu-id="163b3-170">Updating The Error view</span></span>

<span data-ttu-id="163b3-171">La plantilla predeterminada incluye una vista de error en la carpeta vistas compartidas para que se pueda volver a usar en cualquier parte del sitio.</span><span class="sxs-lookup"><span data-stu-id="163b3-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="163b3-172">Esta vista de error contiene un error muy sencillo y no utiliza nuestro diseño del sitio, por lo que la actualizaremos.</span><span class="sxs-lookup"><span data-stu-id="163b3-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="163b3-173">Dado que se trata de una página de error genérica, el contenido es muy sencillo.</span><span class="sxs-lookup"><span data-stu-id="163b3-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="163b3-174">Incluiremos un mensaje y un vínculo para navegar a la página anterior en el historial si el usuario desea volver a intentar la acción.</span><span class="sxs-lookup"><span data-stu-id="163b3-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]

> [!div class="step-by-step"]
> <span data-ttu-id="163b3-175">[Anterior](mvc-music-store-part-8.md)
> [Siguiente](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="163b3-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
