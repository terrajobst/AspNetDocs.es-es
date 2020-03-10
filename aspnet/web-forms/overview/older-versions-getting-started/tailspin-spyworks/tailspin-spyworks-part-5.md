---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: lógica de negocios | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 5 agrega algunas lógica de negocios.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: c60eece9223c47304786d7b0d0ca4b11ac0572e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511549"
---
# <a name="part-5-business-logic"></a><span data-ttu-id="72c19-104">Parte 5: lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="72c19-104">Part 5: Business Logic</span></span>

<span data-ttu-id="72c19-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="72c19-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="72c19-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="72c19-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="72c19-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="72c19-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="72c19-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="72c19-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="72c19-109">La parte 5 agrega algunas lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="72c19-109">Part 5 adds some business logic.</span></span>

## <a id="_Toc260221671"></a><span data-ttu-id="72c19-110">Adición de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="72c19-110">Adding Some Business Logic</span></span>

<span data-ttu-id="72c19-111">Queremos que nuestra experiencia de compra esté disponible cada vez que alguien visite nuestro sitio Web.</span><span class="sxs-lookup"><span data-stu-id="72c19-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="72c19-112">Los visitantes podrán examinar y agregar elementos al carro de la compra, aunque no estén registrados o hayan iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="72c19-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="72c19-113">Cuando estén preparados para desprotegerlos, se les ofrecerá la opción de autenticación y, si aún no son miembros, podrán crear una cuenta.</span><span class="sxs-lookup"><span data-stu-id="72c19-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="72c19-114">Esto significa que se debe implementar la lógica para convertir el carro de la compra de un estado anónimo a un estado "usuario registrado".</span><span class="sxs-lookup"><span data-stu-id="72c19-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="72c19-115">Vamos a crear un directorio denominado "classes", luego haga clic con el botón derecho en la carpeta y cree un nuevo archivo "class" denominado MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="72c19-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="72c19-116">Como se mencionó anteriormente, extenderemos la clase que implementa la página MyShoppingCart. aspx y haremos esto con. La eficaz construcción "clase parcial" de NET.</span><span class="sxs-lookup"><span data-stu-id="72c19-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="72c19-117">La llamada generada para el archivo MyShoppingCart.aspx.cf tiene el siguiente aspecto.</span><span class="sxs-lookup"><span data-stu-id="72c19-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="72c19-118">Tenga en cuenta el uso de la palabra clave "Partial".</span><span class="sxs-lookup"><span data-stu-id="72c19-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="72c19-119">El archivo de clase que acabamos de generar tiene el siguiente aspecto.</span><span class="sxs-lookup"><span data-stu-id="72c19-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="72c19-120">Combinaremos nuestras implementaciones agregando también la palabra clave Partial a este archivo.</span><span class="sxs-lookup"><span data-stu-id="72c19-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="72c19-121">Nuestro nuevo archivo de clase ahora tiene este aspecto.</span><span class="sxs-lookup"><span data-stu-id="72c19-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="72c19-122">El primer método que agregaremos a nuestra clase es el método "AddItem".</span><span class="sxs-lookup"><span data-stu-id="72c19-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="72c19-123">Este es el método al que se llamará en última instancia cuando el usuario haga clic en los vínculos "Agregar a arte" en la lista de productos y en las páginas de detalles del producto.</span><span class="sxs-lookup"><span data-stu-id="72c19-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="72c19-124">Anexe lo siguiente a las instrucciones Using en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="72c19-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="72c19-125">Y agregue este método a la clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="72c19-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="72c19-126">Estamos usando LINQ to Entities para ver si el elemento ya está en el carro.</span><span class="sxs-lookup"><span data-stu-id="72c19-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="72c19-127">Si es así, se actualiza la cantidad de pedido del elemento; de lo contrario, se crea una nueva entrada para el elemento seleccionado.</span><span class="sxs-lookup"><span data-stu-id="72c19-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="72c19-128">Para llamar a este método, se implementará una página AddToCart. aspx que no solo clase este método, sino que se muestra la compra actual a = carro después de agregar el elemento.</span><span class="sxs-lookup"><span data-stu-id="72c19-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="72c19-129">Haga clic con el botón derecho en el nombre de la solución en el explorador de soluciones y agregue y una nueva página denominada AddToCart. aspx como hemos hecho anteriormente.</span><span class="sxs-lookup"><span data-stu-id="72c19-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="72c19-130">Aunque podríamos usar esta página para mostrar los resultados provisionales, como los problemas de baja existencias, etc., en nuestra implementación, la página no se representará realmente, sino que llamaremos a la lógica de "adición" y a la redirección.</span><span class="sxs-lookup"><span data-stu-id="72c19-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="72c19-131">Para ello, agregaremos el código siguiente a la página\_evento Load.</span><span class="sxs-lookup"><span data-stu-id="72c19-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="72c19-132">Tenga en cuenta que estamos recuperando el producto para agregarlo al carro de la compra desde un parámetro QueryString y llamando al método AddItem de nuestra clase.</span><span class="sxs-lookup"><span data-stu-id="72c19-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="72c19-133">Si no se produce ningún error, el control se pasa a la página SHoppingCart. aspx que se implementará a continuación.</span><span class="sxs-lookup"><span data-stu-id="72c19-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="72c19-134">Si se produce un error, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="72c19-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="72c19-135">En la actualidad todavía no hemos implementado un controlador de errores global, por lo que esta excepción no se controlaría en nuestra aplicación, pero solucionaremos esto en breve.</span><span class="sxs-lookup"><span data-stu-id="72c19-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="72c19-136">Tenga en cuenta también el uso de la instrucción Debug. FAIL () (disponible a través de `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="72c19-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="72c19-137">¿La aplicación se ejecuta en el depurador), este método mostrará un cuadro de diálogo detallado con información sobre el estado de las aplicaciones junto con el mensaje de error que se especifica.</span><span class="sxs-lookup"><span data-stu-id="72c19-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="72c19-138">Cuando se ejecuta en producción, se omite la instrucción Debug. FAIL ().</span><span class="sxs-lookup"><span data-stu-id="72c19-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="72c19-139">Observará en el código anterior a una llamada a un método en nuestros nombres de clase de carro de la compra "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="72c19-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="72c19-140">Agregue el código para implementar el método como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="72c19-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="72c19-141">Tenga en cuenta que también hemos agregado botones actualizar y desproteger y una etiqueta en la que se puede mostrar el carro "total".</span><span class="sxs-lookup"><span data-stu-id="72c19-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="72c19-142">Ahora podemos agregar elementos al carro de la compra, pero no hemos implementado la lógica para mostrar el carro una vez que se ha agregado un producto.</span><span class="sxs-lookup"><span data-stu-id="72c19-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="72c19-143">Por lo tanto, en la página MyShoppingCart. aspx agregaremos un control EntityDataSource y un control GridVire como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="72c19-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="72c19-144">Llame al formulario en el diseñador para que pueda hacer doble clic en el botón actualizar carro y generar el controlador de eventos Click que se especifica en la declaración en el marcado.</span><span class="sxs-lookup"><span data-stu-id="72c19-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="72c19-145">Implementaremos los detalles más adelante, pero esto nos permitirá compilar y ejecutar la aplicación sin errores.</span><span class="sxs-lookup"><span data-stu-id="72c19-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="72c19-146">Al ejecutar la aplicación y agregar un elemento al carro de la compra, verá esto.</span><span class="sxs-lookup"><span data-stu-id="72c19-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="72c19-147">Tenga en cuenta que hemos desviado la presentación de la cuadrícula "predeterminada" mediante la implementación de tres columnas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="72c19-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="72c19-148">El primero es un campo "enlazado" que se puede editar para la cantidad:</span><span class="sxs-lookup"><span data-stu-id="72c19-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="72c19-149">La siguiente es una columna "calculada" que muestra el total del artículo de la línea (el costo del artículo, la cantidad que se va a ordenar):</span><span class="sxs-lookup"><span data-stu-id="72c19-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="72c19-150">Por último, tenemos una columna personalizada que contiene un control CheckBox que el usuario usará para indicar que el elemento se debe quitar del gráfico de compras.</span><span class="sxs-lookup"><span data-stu-id="72c19-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="72c19-151">Como puede ver, la línea Order total está vacía, así que vamos a agregar cierta lógica para calcular el total del pedido.</span><span class="sxs-lookup"><span data-stu-id="72c19-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="72c19-152">Primero implementaremos un método "GetTotal" para nuestra clase MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="72c19-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="72c19-153">En el archivo MyShoppingCart.cs, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="72c19-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="72c19-154">Después, en la página\_controlador de eventos de carga, podremos llamar al método GetTotal.</span><span class="sxs-lookup"><span data-stu-id="72c19-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="72c19-155">Al mismo tiempo, agregaremos una prueba para ver si el carro de la compra está vacío y ajustar la presentación en consecuencia si es así.</span><span class="sxs-lookup"><span data-stu-id="72c19-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="72c19-156">Ahora, si el carro de la compra está vacío, obtenemos lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="72c19-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="72c19-157">Y, si no es así, vemos nuestro total.</span><span class="sxs-lookup"><span data-stu-id="72c19-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="72c19-158">Sin embargo, esta página no se ha completado todavía.</span><span class="sxs-lookup"><span data-stu-id="72c19-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="72c19-159">Necesitaremos lógica adicional para recalcular el carro de la compra mediante la eliminación de los elementos marcados para su eliminación y la determinación de los nuevos valores de cantidad, ya que el usuario puede haber cambiado algunos en la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="72c19-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="72c19-160">Permite agregar un método "RemoveItem" a nuestra clase de carro de la compra de MyShoppingCart.cs para controlar el caso en el que un usuario marca un elemento para su eliminación.</span><span class="sxs-lookup"><span data-stu-id="72c19-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="72c19-161">Ahora vamos a crear un método para controlar la circunstancia cuando un usuario simplemente cambia la calidad que se va a ordenar en GridView.</span><span class="sxs-lookup"><span data-stu-id="72c19-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="72c19-162">Con las características básicas de eliminación y actualización, podemos implementar la lógica que realmente actualiza el carro de la compra en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="72c19-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="72c19-163">(En MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="72c19-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="72c19-164">Observará que este método espera dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="72c19-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="72c19-165">Uno es el identificador del carro de la compra y el otro es una matriz de objetos de tipo definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="72c19-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="72c19-166">Para minimizar la dependencia de nuestra lógica en los detalles de la interfaz de usuario, se ha definido una estructura de datos que se puede usar para pasar los elementos del carro de la compra a nuestro código sin que nuestro método tenga que acceder directamente al control GridView.</span><span class="sxs-lookup"><span data-stu-id="72c19-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="72c19-167">En nuestro archivo MyShoppingCart.aspx.cs podemos usar esta estructura en el controlador de eventos Click del botón actualizar como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="72c19-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="72c19-168">Tenga en cuenta que, además de actualizar el carro, actualizaremos también el total del carro.</span><span class="sxs-lookup"><span data-stu-id="72c19-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="72c19-169">Tenga en cuenta que esta línea de código le interesa concretamente:</span><span class="sxs-lookup"><span data-stu-id="72c19-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="72c19-170">GetValues () es una función auxiliar especial que se implementará en MyShoppingCart.aspx.cs como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="72c19-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="72c19-171">Esto proporciona una manera limpia de tener acceso a los valores de los elementos enlazados en el control GridView.</span><span class="sxs-lookup"><span data-stu-id="72c19-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="72c19-172">Puesto que el control de la casilla "quitar elemento" no está enlazado, se tendrá acceso a él a través del método FindControl ().</span><span class="sxs-lookup"><span data-stu-id="72c19-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="72c19-173">En esta fase del desarrollo del proyecto, estamos preparando para implementar el proceso de desprotección.</span><span class="sxs-lookup"><span data-stu-id="72c19-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="72c19-174">Antes de hacerlo, vamos a usar Visual Studio para generar la base de datos de pertenencia y agregar un usuario al repositorio de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="72c19-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="72c19-175">[Anterior](tailspin-spyworks-part-4.md)
> [Siguiente](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="72c19-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
