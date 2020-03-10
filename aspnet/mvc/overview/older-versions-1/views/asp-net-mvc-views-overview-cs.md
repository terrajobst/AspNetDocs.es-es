---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Información general sobre las vistasC#de MVC de ASP.net () | Microsoft Docs
author: StephenWalther
description: ¿Qué es una vista de ASP.NET MVC y en qué se diferencia de una página HTML? En este tutorial, Stephen Walther le presenta las vistas y demuestra cómo puede...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485857"
---
# <a name="aspnet-mvc-views-overview-c"></a><span data-ttu-id="b83e6-104">Información general sobre las vistas de ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="b83e6-104">ASP.NET MVC Views Overview (C#)</span></span>

<span data-ttu-id="b83e6-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b83e6-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b83e6-106">¿Qué es una vista de ASP.NET MVC y en qué se diferencia de una página HTML?</span><span class="sxs-lookup"><span data-stu-id="b83e6-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="b83e6-107">En este tutorial, Stephen Walther le presenta las vistas y muestra cómo puede aprovechar las ventajas de ver datos y aplicaciones auxiliares HTML dentro de una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="b83e6-108">El propósito de este tutorial es proporcionar una breve introducción a las vistas de ASP.NET MVC, ver datos y aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b83e6-109">Al final de este tutorial, debe comprender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="b83e6-110">Descripción de vistas</span><span class="sxs-lookup"><span data-stu-id="b83e6-110">Understanding Views</span></span>

<span data-ttu-id="b83e6-111">En el caso de las páginas ASP.NET o Active Server, ASP.NET MVC no incluye nada que se corresponda directamente con una página.</span><span class="sxs-lookup"><span data-stu-id="b83e6-111">For ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="b83e6-112">En una aplicación ASP.NET MVC, no hay una página en disco que se corresponda con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="b83e6-113">Lo más parecido a una página en una aplicación ASP.NET MVC es algo denominado *vista*.</span><span class="sxs-lookup"><span data-stu-id="b83e6-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="b83e6-114">En una aplicación ASP.NET MVC, las solicitudes del explorador entrante se asignan a las acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="b83e6-115">Una acción del controlador puede devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-115">A controller action might return a view.</span></span> <span data-ttu-id="b83e6-116">Sin embargo, una acción de controlador podría realizar algún otro tipo de acción, como redirigirle a otra acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="b83e6-117">La lista 1 contiene un controlador simple denominado HomeController.</span><span class="sxs-lookup"><span data-stu-id="b83e6-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="b83e6-118">El HomeController expone dos acciones de controlador denominadas index () y Details ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="b83e6-119">**Lista 1-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="b83e6-119">**Listing 1 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

<span data-ttu-id="b83e6-120">Puede invocar la primera acción, la acción index (), escribiendo la siguiente dirección URL en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="b83e6-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="b83e6-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="b83e6-121">/Home/Index</span></span>

<span data-ttu-id="b83e6-122">Puede invocar la segunda acción, la acción Details (), escribiendo esta dirección en el explorador:</span><span class="sxs-lookup"><span data-stu-id="b83e6-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="b83e6-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="b83e6-123">/Home/Details</span></span>

<span data-ttu-id="b83e6-124">La acción index () devuelve una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-124">The Index() action returns a view.</span></span> <span data-ttu-id="b83e6-125">La mayoría de las acciones que cree devolverán vistas.</span><span class="sxs-lookup"><span data-stu-id="b83e6-125">Most actions that you create will return views.</span></span> <span data-ttu-id="b83e6-126">Sin embargo, una acción puede devolver otros tipos de resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="b83e6-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="b83e6-127">Por ejemplo, la acción Details () devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción index ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="b83e6-128">La acción index () contiene la siguiente línea de código:</span><span class="sxs-lookup"><span data-stu-id="b83e6-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="b83e6-129">Vista ();</span><span class="sxs-lookup"><span data-stu-id="b83e6-129">View();</span></span>

<span data-ttu-id="b83e6-130">Esta línea de código devuelve una vista que se debe encontrar en la siguiente ruta de acceso del servidor Web:</span><span class="sxs-lookup"><span data-stu-id="b83e6-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="b83e6-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="b83e6-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="b83e6-132">La ruta de acceso a la vista se deduce del nombre del controlador y el nombre de la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="b83e6-133">Si lo prefiere, puede ser explícito acerca de la vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="b83e6-134">La siguiente línea de código devuelve una vista denominada Fred:</span><span class="sxs-lookup"><span data-stu-id="b83e6-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="b83e6-135">Vista (Fred);</span><span class="sxs-lookup"><span data-stu-id="b83e6-135">View( Fred );</span></span>

<span data-ttu-id="b83e6-136">Cuando se ejecuta esta línea de código, se devuelve una vista desde la ruta de acceso siguiente:</span><span class="sxs-lookup"><span data-stu-id="b83e6-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="b83e6-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="b83e6-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b83e6-138">Si tiene previsto crear pruebas unitarias para la aplicación ASP.NET MVC, se recomienda ser explícita sobre los nombres de vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="b83e6-139">De este modo, puede crear una prueba unitaria para comprobar que una acción de controlador devolvió la vista esperada.</span><span class="sxs-lookup"><span data-stu-id="b83e6-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="b83e6-140">Agregar contenido a una vista</span><span class="sxs-lookup"><span data-stu-id="b83e6-140">Adding Content to a View</span></span>

<span data-ttu-id="b83e6-141">Una vista es un documento HTML estándar (X) que puede contener scripts.</span><span class="sxs-lookup"><span data-stu-id="b83e6-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="b83e6-142">Los scripts se usan para agregar contenido dinámico a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="b83e6-143">Por ejemplo, la vista de la lista 2 muestra la fecha y la hora actuales.</span><span class="sxs-lookup"><span data-stu-id="b83e6-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="b83e6-144">**Lista 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b83e6-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

<span data-ttu-id="b83e6-145">Observe que el cuerpo de la página HTML de la lista 2 contiene el siguiente script:</span><span class="sxs-lookup"><span data-stu-id="b83e6-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="b83e6-146">&lt;% Response. Write (DateTime. Now);%&gt;</span><span class="sxs-lookup"><span data-stu-id="b83e6-146">&lt;% Response.Write(DateTime.Now);%&gt;</span></span>

<span data-ttu-id="b83e6-147">Use los delimitadores de script &lt;% y%&gt; para marcar el principio y el final de un script.</span><span class="sxs-lookup"><span data-stu-id="b83e6-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="b83e6-148">Este script está escrito en C#.</span><span class="sxs-lookup"><span data-stu-id="b83e6-148">This script is written in C#.</span></span> <span data-ttu-id="b83e6-149">Muestra la fecha y hora actuales llamando al método Response. Write () para representar el contenido en el explorador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="b83e6-150">Los delimitadores de script &lt;% y%&gt; se pueden usar para ejecutar una o más instrucciones.</span><span class="sxs-lookup"><span data-stu-id="b83e6-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="b83e6-151">Dado que llama a Response. Write () con tanta frecuencia, Microsoft le proporciona un acceso directo para llamar al método Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="b83e6-152">La vista de la lista 3 usa los delimitadores &lt;% = y%&gt; como un acceso directo para llamar a Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="b83e6-153">**Lista 3: Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="b83e6-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

<span data-ttu-id="b83e6-154">Puede usar cualquier lenguaje .NET para generar contenido dinámico en una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="b83e6-155">Normalmente, usará Visual Basic .NET o C# para escribir los controladores y las vistas.</span><span class="sxs-lookup"><span data-stu-id="b83e6-155">Normally, you'll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="b83e6-156">Usar aplicaciones auxiliares HTML para generar el contenido de la vista</span><span class="sxs-lookup"><span data-stu-id="b83e6-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="b83e6-157">Para que sea más fácil agregar contenido a una vista, puede aprovechar algo denominado *aplicación auxiliar HTML*.</span><span class="sxs-lookup"><span data-stu-id="b83e6-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="b83e6-158">Una aplicación auxiliar HTML, normalmente, es un método que genera una cadena.</span><span class="sxs-lookup"><span data-stu-id="b83e6-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="b83e6-159">Puede usar aplicaciones auxiliares HTML para generar elementos HTML estándar como cuadros de texto, vínculos, listas desplegables y cuadros de lista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="b83e6-160">Por ejemplo, la vista de la lista 4 aprovecha las ventajas de tres aplicaciones auxiliares HTML: las aplicaciones auxiliares (), el cuadro de texto () y la contraseña () para generar un formulario de inicio de sesión (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="b83e6-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="b83e6-161">**Lista 4: \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b83e6-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

<span data-ttu-id="b83e6-162">[![el cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b83e6-162">[![The New Project dialog box](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="b83e6-163">**Figura 01**: formulario de inicio de sesión estándar ([haga clic para ver la imagen de tamaño completo](asp-net-mvc-views-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b83e6-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="b83e6-164">Se llama a todos los métodos auxiliares HTML en la propiedad HTML de la vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="b83e6-165">Por ejemplo, si se representa un cuadro de texto, se llama al método html. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="b83e6-166">Tenga en cuenta que usa los delimitadores de script &lt;% = y%&gt; al llamar a las aplicaciones auxiliares HTML. TextBox () y HTML. Password ().</span><span class="sxs-lookup"><span data-stu-id="b83e6-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="b83e6-167">Estas aplicaciones auxiliares simplemente devuelven una cadena.</span><span class="sxs-lookup"><span data-stu-id="b83e6-167">These helpers simply return a string.</span></span> <span data-ttu-id="b83e6-168">Debe llamar a Response. Write () para representar la cadena en el explorador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="b83e6-169">El uso de métodos auxiliares HTML es opcional.</span><span class="sxs-lookup"><span data-stu-id="b83e6-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="b83e6-170">Facilitan su vida reduciendo la cantidad de HTML y el script que necesita escribir.</span><span class="sxs-lookup"><span data-stu-id="b83e6-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="b83e6-171">La vista de la lista 5 representa el mismo formulario exacto que la vista de la lista 4 sin usar aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="b83e6-172">**Lista 5: \Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="b83e6-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

<span data-ttu-id="b83e6-173">También tiene la opción de crear sus propias aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="b83e6-174">Por ejemplo, puede crear un método auxiliar GridView () que muestre automáticamente un conjunto de registros de base de datos en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="b83e6-175">Exploramos este tema en el tutorial **creación de aplicaciones auxiliares HTML personalizadas**.</span><span class="sxs-lookup"><span data-stu-id="b83e6-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="b83e6-176">Usar los datos de vista para pasar datos a una vista</span><span class="sxs-lookup"><span data-stu-id="b83e6-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="b83e6-177">Utilice los datos de vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="b83e6-178">Considere la visualización de datos como un paquete que se envía a través del correo.</span><span class="sxs-lookup"><span data-stu-id="b83e6-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="b83e6-179">Todos los datos pasados desde un controlador a una vista se deben enviar mediante este paquete.</span><span class="sxs-lookup"><span data-stu-id="b83e6-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="b83e6-180">Por ejemplo, el controlador de la lista 6 agrega un mensaje para ver los datos.</span><span class="sxs-lookup"><span data-stu-id="b83e6-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="b83e6-181">**Listado 6-ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="b83e6-181">**Listing 6 - ProductController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

<span data-ttu-id="b83e6-182">La propiedad ViewData del controlador representa una colección de pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="b83e6-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="b83e6-183">En la enumeración 6, el método index () agrega un elemento a la colección de datos de vista denominada Message con el valor Hola mundo!.</span><span class="sxs-lookup"><span data-stu-id="b83e6-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="b83e6-184">Cuando el método index () devuelve la vista, los datos de la vista se pasan automáticamente a la vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="b83e6-185">La vista de la lista 7 recupera el mensaje de los datos de la vista y representa el mensaje en el explorador.</span><span class="sxs-lookup"><span data-stu-id="b83e6-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="b83e6-186">**Listado 7: \Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="b83e6-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

<span data-ttu-id="b83e6-187">Tenga en cuenta que la vista aprovecha el método auxiliar HTML. encode () HTML al representar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="b83e6-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="b83e6-188">La aplicación auxiliar html html. encode () codifica caracteres especiales como &lt; y &gt; en caracteres que se pueden mostrar de forma segura en una página web.</span><span class="sxs-lookup"><span data-stu-id="b83e6-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="b83e6-189">Siempre que represente el contenido que un usuario envía a un sitio web, debe codificar el contenido para evitar ataques por inyección de código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b83e6-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="b83e6-190">(Dado que hemos creado el mensaje en ProductController, realmente no es necesario codificar el mensaje.</span><span class="sxs-lookup"><span data-stu-id="b83e6-190">(Because we created the message ourselves in the ProductController, we don't really need to encode the message.</span></span> <span data-ttu-id="b83e6-191">Sin embargo, es un buen hábito llamar siempre al método html. encode () al mostrar el contenido recuperado de los datos de vista en una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="b83e6-192">En el listado 7, aprovechamos los datos de la vista para pasar un mensaje de cadena simple de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="b83e6-193">También puede usar ver datos para pasar otros tipos de datos, como una colección de registros de base de datos, de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="b83e6-194">Por ejemplo, si desea mostrar el contenido de la tabla de la base de datos Products en una vista, pasaría la colección de registros de base de datos en los datos de la vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="b83e6-195">También tiene la opción de pasar datos de vista fuertemente tipados de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="b83e6-196">Exploramos este tema en el tutorial **Descripción de las vistas y los datos de vista fuertemente tipados**.</span><span class="sxs-lookup"><span data-stu-id="b83e6-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="b83e6-197">Resumen</span><span class="sxs-lookup"><span data-stu-id="b83e6-197">Summary</span></span>

<span data-ttu-id="b83e6-198">En este tutorial se ha proporcionado una breve introducción a las vistas de MVC de ASP.NET, los datos de vista y las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="b83e6-199">En la primera sección, aprendió a agregar nuevas vistas al proyecto.</span><span class="sxs-lookup"><span data-stu-id="b83e6-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="b83e6-200">Aprendió que debe agregar una vista a la carpeta correcta para poder llamarla desde un controlador determinado.</span><span class="sxs-lookup"><span data-stu-id="b83e6-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="b83e6-201">A continuación, se describe el tema de las aplicaciones auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="b83e6-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="b83e6-202">Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente contenido HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="b83e6-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="b83e6-203">Por último, aprendió a sacar provecho de los datos de vista para pasar datos de un controlador a una vista.</span><span class="sxs-lookup"><span data-stu-id="b83e6-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b83e6-204">Siguiente</span><span class="sxs-lookup"><span data-stu-id="b83e6-204">Next</span></span>](creating-custom-html-helpers-cs.md)
