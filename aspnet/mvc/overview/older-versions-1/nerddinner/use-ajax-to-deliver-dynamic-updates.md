---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar AJAX para ofrecer actualizaciones dinámicas | Microsoft Docs
author: microsoft
description: El paso 10 implementa la compatibilidad de los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, mediante un enfoque basado en Ajax integrado en los detalles de la cena...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486313"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="0ab9e-103">Usar AJAX para entregar actualizaciones dinámicas</span><span class="sxs-lookup"><span data-stu-id="0ab9e-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="0ab9e-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="0ab9e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="0ab9e-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="0ab9e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="0ab9e-106">Este es el paso 10 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="0ab9e-107">El paso 10 implementa la compatibilidad de los usuarios que han iniciado sesión en RSVP su interés en asistir a una cena, mediante un enfoque basado en Ajax integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="0ab9e-108">Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .</span><span class="sxs-lookup"><span data-stu-id="0ab9e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="0ab9e-109">NerdDinner paso 10: aceptación de habilitación de AJAX</span><span class="sxs-lookup"><span data-stu-id="0ab9e-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="0ab9e-110">Vamos a implementar ahora soporte técnico para que los usuarios que han iniciado sesión tengan su interés en asistir a una cena.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="0ab9e-111">Lo habilitaremos mediante un enfoque basado en AJAX integrado en la página de detalles de la cena.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="0ab9e-112">Que indica si el usuario es RSVP</span><span class="sxs-lookup"><span data-stu-id="0ab9e-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="0ab9e-113">Los usuarios pueden visitar la dirección URL de */Dinners/details/[id*] para ver los detalles de una cena determinada:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="0ab9e-114">El método de acción Details () se implementa de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="0ab9e-115">Nuestro primer paso para implementar la compatibilidad con RSVP será agregar un método auxiliar "IsUserRegistered (nombre de usuario)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente).</span><span class="sxs-lookup"><span data-stu-id="0ab9e-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="0ab9e-116">Este método auxiliar devuelve true o false en función de si el usuario está actualmente RSVP para la cena:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="0ab9e-117">A continuación, podemos agregar el código siguiente a nuestra plantilla de la vista details. aspx para mostrar un mensaje adecuado que indique si el usuario está registrado o no para el evento:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="0ab9e-118">Y ahora, cuando un usuario visita una cena registrada para ellos, verá este mensaje:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="0ab9e-119">Y cuando visiten una cena que no estén registradas, verán el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="0ab9e-120">Implementar el método de acción Register</span><span class="sxs-lookup"><span data-stu-id="0ab9e-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="0ab9e-121">Vamos a agregar ahora la funcionalidad necesaria para permitir a los usuarios confirmar una cena en la página de detalles.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="0ab9e-122">Para implementarlo, vamos a crear una nueva clase "RSVPController" haciendo clic con el botón derecho en el directorio \Controllers y eligiendo el comando de menú controlador de Add-&gt;.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="0ab9e-123">Implementaremos un método de acción "Register" dentro de la nueva clase RSVPController que toma un identificador para una cena como argumento, recupera el objeto cena adecuado, comprueba si el usuario que ha iniciado sesión está actualmente en la lista de usuarios que se han registrado para él, y si no agrega un objeto RSVP para ellos:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="0ab9e-124">Observe cómo se devuelve una cadena simple como salida del método de acción.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="0ab9e-125">Podríamos haber incrustado este mensaje en una plantilla de vista, pero dado que es tan pequeño, simplemente usaremos el método auxiliar Content () en la clase base del controlador y devolveremos un mensaje de cadena como el anterior.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="0ab9e-126">Llamar al método de acción RSVPForEvent mediante AJAX</span><span class="sxs-lookup"><span data-stu-id="0ab9e-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="0ab9e-127">Usaremos AJAX para invocar el método de acción Register de nuestra vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="0ab9e-128">La implementación de esto es bastante fácil.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="0ab9e-129">En primer lugar, vamos a agregar dos referencias de la biblioteca de scripts:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="0ab9e-130">La primera biblioteca hace referencia a la biblioteca de scripts del lado cliente de AJAX de ASP.NET básica.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="0ab9e-131">Este archivo tiene aproximadamente 24K de tamaño (comprimido) y contiene la funcionalidad básica de AJAX del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="0ab9e-132">La segunda biblioteca contiene funciones de utilidad que se integran con los métodos auxiliares de AJAX integrados de ASP.NET MVC (que usaremos en breve).</span><span class="sxs-lookup"><span data-stu-id="0ab9e-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="0ab9e-133">A continuación, podemos actualizar el código de la plantilla de vista que agregamos anteriormente para que en lugar de generar un mensaje "no se ha registrado para este evento", se represente un vínculo que, cuando se inserta, realiza una llamada AJAX que invoca nuestro método de acción RSVPForEvent en nuestro controlador de RSVP y RSVP al usuario:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="0ab9e-134">El método auxiliar Ajax. ActionLink () que se ha usado anteriormente está integrado en ASP.NET MVC y es similar al método auxiliar HTML. ActionLink (), salvo que en lugar de realizar una navegación estándar, realiza una llamada AJAX al método de acción cuando se hace clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="0ab9e-135">Anteriormente, se llama al método de acción "Register" en el controlador "RSVP" y se pasa DinnerID como el parámetro "ID".</span><span class="sxs-lookup"><span data-stu-id="0ab9e-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="0ab9e-136">El parámetro AjaxOptions final que se pasa indica que queremos tomar el contenido devuelto por el método de acción y actualizar el elemento HTML &lt;div&gt; en la página cuyo identificador es "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="0ab9e-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="0ab9e-137">Y ahora, cuando un usuario navega a una cena en la que no se ha registrado, verá un vínculo a RSVP para ella:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="0ab9e-138">Si hace clic en el vínculo "RSVP para este evento", realizará una llamada AJAX al método de acción Register en el controlador RSVP y, cuando se complete, verá un mensaje actualizado como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="0ab9e-139">El ancho de banda de red y el tráfico implicados al crear esta llamada AJAX es realmente ligero.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="0ab9e-140">Cuando el usuario hace clic en el vínculo "RSVP para este evento", se realiza una pequeña solicitud de red HTTP POST a la dirección URL de */Dinners/Register/1* que tiene el siguiente aspecto en la conexión:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="0ab9e-141">Y la respuesta del método de acción Register es simplemente:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="0ab9e-142">Esta llamada ligera es rápida y funcionará incluso a través de una red lenta.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="0ab9e-143">Agregar una animación de jQuery</span><span class="sxs-lookup"><span data-stu-id="0ab9e-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="0ab9e-144">La funcionalidad de AJAX que hemos implementado funciona bien y rápido.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="0ab9e-145">En ocasiones, es posible que esto suceda con rapidez, por lo que es posible que un usuario no tenga en cuenta que el vínculo RSVP se ha reemplazado por texto nuevo.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="0ab9e-146">Para que el resultado sea un poco más obvio, podemos agregar una animación simple para llamar la atención sobre el mensaje de actualización.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="0ab9e-147">La plantilla de proyecto ASP.NET MVC predeterminada incluye jQuery, una biblioteca JavaScript de código abierto excelente (y muy popular) que también es compatible con Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="0ab9e-148">jQuery proporciona varias características, entre las que se incluyen una buena biblioteca HTML de selección y efectos de DOM.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="0ab9e-149">Para usar jQuery, primero agregaremos una referencia de script.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="0ab9e-150">Como vamos a usar jQuery en una variedad de lugares dentro de nuestro sitio, agregaremos la referencia de script en nuestro archivo de página maestra site. Master para que todas las páginas puedan usarla.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="0ab9e-151">*Sugerencia: Asegúrese de que ha instalado la revisión de IntelliSense para JavaScript para VS 2008 SP1 que habilita la compatibilidad con IntelliSense más enriquecida para los archivos JavaScript (incluido jQuery). Puede descargarlo desde: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="0ab9e-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="0ab9e-152">El código escrito mediante JQuery suele usar un método JavaScript global "$ ()" que recupera uno o varios elementos HTML mediante un selector de CSS.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="0ab9e-153">Por ejemplo, *$ ("#rsvpmsg")* selecciona cualquier elemento HTML con el identificador de rsvpmsg, mientras que *$ (". Something")* seleccionaría todos los elementos con el nombre de clase CSS "algo".</span><span class="sxs-lookup"><span data-stu-id="0ab9e-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="0ab9e-154">También puede escribir consultas más avanzadas como "devolver todos los botones de radio seleccionados" mediante una consulta de selector como: *$ ("entrada [@type= radio] [@checked]")* .</span><span class="sxs-lookup"><span data-stu-id="0ab9e-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="0ab9e-155">Una vez seleccionados los elementos, puede llamar a métodos en ellos para realizar acciones, como ocultarlos: *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="0ab9e-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="0ab9e-156">En nuestro escenario de RSVP, vamos a definir una sencilla función de JavaScript denominada "AnimateRSVPMessage" que selecciona el&gt; "rsvpmsg" &lt;div y anima el tamaño del contenido de texto.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="0ab9e-157">El código siguiente inicia el texto pequeño y, a continuación, hace que aumente durante un período de 400 milisegundos:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="0ab9e-158">A continuación, podemos conectar esta función de JavaScript para que se llame después de que la llamada AJAX se complete correctamente pasando su nombre a nuestro método auxiliar de Ajax. ActionLink () (a través de la propiedad de evento AjaxOptions "Success"):</span><span class="sxs-lookup"><span data-stu-id="0ab9e-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="0ab9e-159">Y ahora, cuando se hace clic en el vínculo "RSVP para este evento" y nuestra llamada AJAX se completa correctamente, el mensaje de contenido que se devuelve se animará y aumentará de tamaño:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="0ab9e-160">Además de proporcionar un evento "Success", el objeto AjaxOptions expone eventos de inicio, de error y de finalización que puede controlar (junto con otras propiedades y opciones útiles).</span><span class="sxs-lookup"><span data-stu-id="0ab9e-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="0ab9e-161">Limpieza: refactorizar una vista parcial de RSVP</span><span class="sxs-lookup"><span data-stu-id="0ab9e-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="0ab9e-162">Nuestra plantilla de vista de detalles empieza a ser muy larga, lo que lo hará un poco más difícil de entender.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="0ab9e-163">Para ayudar a mejorar la legibilidad del código, vamos a crear una vista parcial (RSVPStatus. ascx) que encapsula todo el código de vista de RSVP de nuestra página de detalles.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="0ab9e-164">Para ello, haga clic con el botón derecho en la carpeta \Views\Dinners y, a continuación, elija el comando de menú Ver&gt;.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="0ab9e-165">Tendremos que tomar un objeto cena como ViewModel fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="0ab9e-166">A continuación, se puede copiar y pegar el contenido RSVP de la vista details. aspx.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="0ab9e-167">Una vez hecho esto, vamos a crear otra vista parcial: EditAndDeleteLinks. ascx, que encapsula el código de la vista de vínculos de edición y eliminación.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="0ab9e-168">También tendremos que tomar un objeto cena como ViewModel fuertemente tipado y copiar y pegar la lógica de edición y eliminación de la vista details. aspx.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="0ab9e-169">La plantilla de vista de detalles puede simplemente incluir dos llamadas al método html. RenderPartial () en la parte inferior:</span><span class="sxs-lookup"><span data-stu-id="0ab9e-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="0ab9e-170">Esto hace que el limpiador de código se lea y se mantenga.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="0ab9e-171">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="0ab9e-171">Next Step</span></span>

<span data-ttu-id="0ab9e-172">Ahora veamos cómo podemos usar AJAX aún más y agregar compatibilidad de asignación interactiva a nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="0ab9e-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0ab9e-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Siguiente](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="0ab9e-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
