---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Agregando un controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6b38d757d37374b14979f8a079a46158ff64f9c3
ms.sourcegitcommit: f774732a3960fca079438a88a5472c37cf7be08a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68810763"
---
# <a name="adding-a-controller"></a><span data-ttu-id="a6629-102">Agregar un controlador</span><span class="sxs-lookup"><span data-stu-id="a6629-102">Adding a Controller</span></span>

<span data-ttu-id="a6629-103">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="a6629-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="a6629-104">MVC representa el *controlador de vista de modelos*.</span><span class="sxs-lookup"><span data-stu-id="a6629-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="a6629-105">MVC es un patrón para desarrollar aplicaciones que están bien diseñadas, comprobables y fáciles de mantener.</span><span class="sxs-lookup"><span data-stu-id="a6629-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="a6629-106">Las aplicaciones basadas en MVC contienen:</span><span class="sxs-lookup"><span data-stu-id="a6629-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="a6629-107">**M** odelos: Clases que representan los datos de la aplicación y que usan la lógica de validación para aplicar las reglas de negocios para esos datos.</span><span class="sxs-lookup"><span data-stu-id="a6629-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="a6629-108">**V** istas: Archivos de plantilla que usa la aplicación para generar respuestas HTML dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="a6629-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="a6629-109">**C** ontroladores: Las clases que controlan las solicitudes entrantes del explorador, recuperan los datos del modelo y, a continuación, especifican plantillas de vista que devuelven una respuesta al explorador.</span><span class="sxs-lookup"><span data-stu-id="a6629-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="a6629-110">Trataremos todos estos conceptos de esta serie de tutoriales y le mostraremos cómo usarlos para compilar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a6629-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

<span data-ttu-id="a6629-111">Comencemos por crear una clase de controlador.</span><span class="sxs-lookup"><span data-stu-id="a6629-111">Let's begin by creating a controller class.</span></span> <span data-ttu-id="a6629-112">En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Controladores* y, a continuación, haga clic en **Agregar**y luego en **controlador**.</span><span class="sxs-lookup"><span data-stu-id="a6629-112">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](adding-a-controller/_static/image1.png)

<span data-ttu-id="a6629-113">En el cuadro de diálogo **Agregar scaffold** , haga clic en **controlador de MVC 5-vacío**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="a6629-113">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  

<span data-ttu-id="a6629-114">Asigne al nuevo controlador el nombre "HelloWorldController" y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="a6629-114">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Agregar controlador](adding-a-controller/_static/image3.png)

<span data-ttu-id="a6629-116">Observe en **Explorador de soluciones** que se ha creado un nuevo archivo denominado *HelloWorldController.CS* y una nueva carpeta *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="a6629-116">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="a6629-117">El controlador está abierto en el IDE.</span><span class="sxs-lookup"><span data-stu-id="a6629-117">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="a6629-118">Reemplace el contenido del archivo por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6629-118">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="a6629-119">Los métodos de controlador devolverán una cadena de HTML como ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a6629-119">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="a6629-120">El controlador se denomina `HelloWorldController` y el primer método se denomina `Index`.</span><span class="sxs-lookup"><span data-stu-id="a6629-120">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="a6629-121">Vamos a invocarlo desde un explorador.</span><span class="sxs-lookup"><span data-stu-id="a6629-121">Let's invoke it from a browser.</span></span> <span data-ttu-id="a6629-122">Ejecute la aplicación (presione F5 o Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="a6629-122">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="a6629-123">En el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="a6629-123">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="a6629-124">(Por ejemplo, en la ilustración siguiente, `http://localhost:1234/HelloWorld.`es) la página en el explorador tendrá un aspecto similar al de la siguiente captura de pantalla.</span><span class="sxs-lookup"><span data-stu-id="a6629-124">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="a6629-125">En el método anterior, el código devolvió una cadena directamente.</span><span class="sxs-lookup"><span data-stu-id="a6629-125">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="a6629-126">Ha indicado al sistema que solo devuelva algún código HTML y lo hizo.</span><span class="sxs-lookup"><span data-stu-id="a6629-126">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="a6629-127">ASP.NET MVC invoca diferentes clases de controlador (y métodos de acción diferentes) en función de la dirección URL de entrada.</span><span class="sxs-lookup"><span data-stu-id="a6629-127">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="a6629-128">La lógica de enrutamiento de direcciones URL predeterminada usada por ASP.NET MVC usa un formato como este para determinar qué código debe invocar:</span><span class="sxs-lookup"><span data-stu-id="a6629-128">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="a6629-129">El formato para el enrutamiento se establece en el archivo inicio de la *aplicación\_/RouteConfig. CS* .</span><span class="sxs-lookup"><span data-stu-id="a6629-129">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="a6629-130">Al ejecutar la aplicación y no proporcionar ningún segmento de dirección URL, el valor predeterminado es el controlador de "Inicio" y el método de acción "índice" especificado en la sección de valores predeterminados del código anterior.</span><span class="sxs-lookup"><span data-stu-id="a6629-130">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="a6629-131">La primera parte de la dirección URL determina la clase de controlador que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a6629-131">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="a6629-132">Por lo tanto,/HelloWorld `HelloWorldController` se asigna a la clase.</span><span class="sxs-lookup"><span data-stu-id="a6629-132">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="a6629-133">La segunda parte de la dirección URL determina el método de acción de la clase que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a6629-133">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="a6629-134">Por lo tanto,/HelloWorld/index `Index` haría que el `HelloWorldController` método de la clase se ejecutara.</span><span class="sxs-lookup"><span data-stu-id="a6629-134">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="a6629-135">Tenga en cuenta que solo tuvimos que ir a */HelloWorld* y `Index` el método se usó de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a6629-135">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="a6629-136">Esto se debe a que un `Index` método denominado es el método predeterminado al que se llamará en un controlador si no se especifica uno explícitamente.</span><span class="sxs-lookup"><span data-stu-id="a6629-136">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="a6629-137">La tercera parte del segmento de dirección URL (`Parameters`) es para los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a6629-137">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="a6629-138">Veremos los datos de ruta más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="a6629-138">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="a6629-139">Vaya a `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="a6629-139">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="a6629-140">El `Welcome` método se ejecuta y devuelve la &quot;cadena. este es el método de acción de bienvenida... &quot;.</span><span class="sxs-lookup"><span data-stu-id="a6629-140">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="a6629-141">La asignación MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="a6629-141">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="a6629-142">Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción.</span><span class="sxs-lookup"><span data-stu-id="a6629-142">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="a6629-143">Todavía no ha usado el elemento `[Parameters]` de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a6629-143">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="a6629-144">Vamos a modificar ligeramente el ejemplo para que pueda pasar parte de la información de parámetros de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? name&amp;= Scott numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="a6629-144">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="a6629-145">Cambie el `Welcome` método para que incluya dos parámetros, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="a6629-145">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="a6629-146">Tenga en cuenta que el código C# usa la característica opcional-Parameter para indicar `numTimes` que el parámetro debe tener como valor predeterminado 1 si no se pasa ningún valor para ese parámetro.</span><span class="sxs-lookup"><span data-stu-id="a6629-146">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="a6629-147">Nota de seguridad: El código anterior usa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger la aplicación de entradas malintencionadas (es decir, JavaScript).</span><span class="sxs-lookup"><span data-stu-id="a6629-147">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="a6629-148">Para obtener más información, vea [Cómo: Proteja contra ataques de scripts en una aplicación web aplicando codificación HTML a las](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx)cadenas.</span><span class="sxs-lookup"><span data-stu-id="a6629-148">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>

 <span data-ttu-id="a6629-149">Ejecute la aplicación y vaya a la dirección URL del`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`ejemplo ().</span><span class="sxs-lookup"><span data-stu-id="a6629-149">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="a6629-150">Puede probar distintos valores para `name` y `numtimes` en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a6629-150">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="a6629-151">El [sistema de enlace de modelos de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta de la barra de direcciones a los parámetros del método.</span><span class="sxs-lookup"><span data-stu-id="a6629-151">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="a6629-152">En el ejemplo anterior, el segmento de dirección `Parameters`URL () no se usa `name` , `numTimes` y los parámetros y se pasan como [cadenas de consulta](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="a6629-152">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="a6629-153">El carácter comodín ?</span><span class="sxs-lookup"><span data-stu-id="a6629-153">The ?</span></span> <span data-ttu-id="a6629-154">(signo de interrogación) en la dirección URL anterior es un separador y las cadenas de consulta siguen.</span><span class="sxs-lookup"><span data-stu-id="a6629-154">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="a6629-155">El carácter &amp; separa las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a6629-155">The &amp; character separates query strings.</span></span>

<span data-ttu-id="a6629-156">Reemplace el método de bienvenida por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a6629-156">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="a6629-157">Ejecute la aplicación y escriba la siguiente dirección URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="a6629-157">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="a6629-158">Esta vez, el tercer segmento de dirección URL coincidió `ID.` con `Welcome` el parámetro de ruta, el`ID`método de acción contiene un parámetro () que `RegisterRoutes` coincidía con la especificación de la dirección URL en el método.</span><span class="sxs-lookup"><span data-stu-id="a6629-158">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="a6629-159">En las aplicaciones ASP.NET MVC, es más habitual pasar parámetros como datos de ruta (como hicimos con el identificador anterior) que pasarlos como cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a6629-159">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="a6629-160">También puede Agregar una ruta para pasar los `name` parámetros y `numtimes` in como datos de ruta en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a6629-160">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="a6629-161">En el *archivo\_Start\RouteConfig.CS* de la aplicación, agregue la ruta "Hello":</span><span class="sxs-lookup"><span data-stu-id="a6629-161">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="a6629-162">Ejecute la aplicación y vaya a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="a6629-162">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="a6629-163">Para muchas aplicaciones MVC, la ruta predeterminada funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="a6629-163">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="a6629-164">Más adelante en este tutorial aprenderá a pasar datos mediante el enlazador de modelos y no tendrá que modificar la ruta predeterminada para ello.</span><span class="sxs-lookup"><span data-stu-id="a6629-164">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="a6629-165">En estos ejemplos, el controlador ha estado realizando &quot;la&quot; parte VC de MVC, es decir, la vista y el controlador funcionan.</span><span class="sxs-lookup"><span data-stu-id="a6629-165">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="a6629-166">El controlador devuelve HTML directamente.</span><span class="sxs-lookup"><span data-stu-id="a6629-166">The controller is returning HTML directly.</span></span> <span data-ttu-id="a6629-167">Normalmente, no desea que los controladores devuelvan HTML directamente, ya que es muy engorroso para el código.</span><span class="sxs-lookup"><span data-stu-id="a6629-167">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="a6629-168">En su lugar, usaremos normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="a6629-168">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="a6629-169">Vamos a ver cómo podemos hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a6629-169">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6629-170">[Anterior](getting-started.md)
> [Siguiente](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="a6629-170">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
