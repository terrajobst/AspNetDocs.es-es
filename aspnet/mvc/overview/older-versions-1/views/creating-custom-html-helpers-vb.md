---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Crear aplicaciones auxiliares HTML personalizadas (VB) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC. Aprovechando la aplicación auxiliar HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593885"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="f60a2-104">Crear asistentes de HTML personalizados (VB)</span><span class="sxs-lookup"><span data-stu-id="f60a2-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="f60a2-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f60a2-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f60a2-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="f60a2-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="f60a2-107">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="f60a2-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="f60a2-108">Al aprovechar las aplicaciones auxiliares HTML, puede reducir la cantidad de tipos de etiquetas HTML tediosas que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="f60a2-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="f60a2-109">El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares HTML personalizadas que puede usar en las vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="f60a2-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="f60a2-110">Al aprovechar las aplicaciones auxiliares HTML, puede reducir la cantidad de tipos de etiquetas HTML tediosas que debe realizar para crear una página HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="f60a2-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="f60a2-111">En la primera parte de este tutorial, se describen algunas de las aplicaciones auxiliares HTML existentes incluidas con el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f60a2-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="f60a2-112">A continuación, se describen dos métodos para crear aplicaciones auxiliares HTML personalizadas: se explica cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método compartido y la creación de un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="f60a2-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="f60a2-113">Descripción de las aplicaciones auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="f60a2-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="f60a2-114">Una aplicación auxiliar HTML es simplemente un método que devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="f60a2-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="f60a2-115">La cadena puede representar cualquier tipo de contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="f60a2-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="f60a2-116">Por ejemplo, puede usar aplicaciones auxiliares HTML para representar etiquetas HTML estándar, como HTML `<input>` y etiquetas de `<img>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="f60a2-117">También puede usar aplicaciones auxiliares HTML para representar contenido más complejo, como una franja de pestañas o una tabla HTML de datos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f60a2-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="f60a2-118">El marco de MVC de ASP.NET incluye el siguiente conjunto de aplicaciones auxiliares estándar de HTML (esta no es una lista completa):</span><span class="sxs-lookup"><span data-stu-id="f60a2-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="f60a2-119">HTML. ActionLink ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-119">Html.ActionLink()</span></span>
- <span data-ttu-id="f60a2-120">HTML. BeginForm ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-120">Html.BeginForm()</span></span>
- <span data-ttu-id="f60a2-121">HTML. CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-121">Html.CheckBox()</span></span>
- <span data-ttu-id="f60a2-122">HTML. DropDownList ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-122">Html.DropDownList()</span></span>
- <span data-ttu-id="f60a2-123">HTML. EndForm ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-123">Html.EndForm()</span></span>
- <span data-ttu-id="f60a2-124">HTML. Hidden ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-124">Html.Hidden()</span></span>
- <span data-ttu-id="f60a2-125">HTML. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-125">Html.ListBox()</span></span>
- <span data-ttu-id="f60a2-126">HTML. Password ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-126">Html.Password()</span></span>
- <span data-ttu-id="f60a2-127">HTML. RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-127">Html.RadioButton()</span></span>
- <span data-ttu-id="f60a2-128">HTML. TextArea ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-128">Html.TextArea()</span></span>
- <span data-ttu-id="f60a2-129">HTML. TextBox ()</span><span class="sxs-lookup"><span data-stu-id="f60a2-129">Html.TextBox()</span></span>

<span data-ttu-id="f60a2-130">Por ejemplo, considere el formulario de la lista 1.</span><span class="sxs-lookup"><span data-stu-id="f60a2-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="f60a2-131">Este formulario se representa con la ayuda de dos de las aplicaciones auxiliares de HTML estándar (vea la ilustración 1).</span><span class="sxs-lookup"><span data-stu-id="f60a2-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="f60a2-132">Este formulario utiliza los métodos auxiliares `Html.BeginForm()` y `Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="f60a2-133">[![página representada con aplicaciones auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f60a2-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="f60a2-134">**Figura 01**: página representada con aplicaciones auxiliares HTML ([haga clic para ver la imagen de tamaño completo](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f60a2-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="f60a2-135">**Lista 1: `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="f60a2-136">El método auxiliar `Html.BeginForm()` se usa para crear las etiquetas de `<form>` HTML de apertura y cierre.</span><span class="sxs-lookup"><span data-stu-id="f60a2-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="f60a2-137">Observe que se llama al método `Html.BeginForm()` dentro de una instrucción using.</span><span class="sxs-lookup"><span data-stu-id="f60a2-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="f60a2-138">La instrucción using garantiza que la etiqueta de `<form>` se cierre al final del bloque using.</span><span class="sxs-lookup"><span data-stu-id="f60a2-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="f60a2-139">Si lo prefiere, en lugar de crear un bloque Using, puede llamar al método auxiliar HTML. EndForm () para cerrar la etiqueta `<form>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="f60a2-140">Use cualquier enfoque para crear una etiqueta de `<form>` de apertura y cierre que le parezca más intuitiva.</span><span class="sxs-lookup"><span data-stu-id="f60a2-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="f60a2-141">Los métodos auxiliares de `Html.TextBox()` se usan en la enumeración 1 para representar etiquetas HTML `<input>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="f60a2-142">Si selecciona ver código fuente en el explorador, verá el origen HTML en la lista 2.</span><span class="sxs-lookup"><span data-stu-id="f60a2-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="f60a2-143">Observe que el origen contiene etiquetas HTML estándar.</span><span class="sxs-lookup"><span data-stu-id="f60a2-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f60a2-144">Tenga en cuenta que la aplicación auxiliar `Html.TextBox()`-HTML se representa con `<%= %>` etiquetas en lugar de `<% %>` etiquetas.</span><span class="sxs-lookup"><span data-stu-id="f60a2-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="f60a2-145">Si no incluye el signo igual, no se representa nada en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f60a2-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="f60a2-146">El marco de MVC de ASP.NET contiene un pequeño conjunto de aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="f60a2-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="f60a2-147">Lo más probable es que necesite extender el marco de MVC con aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f60a2-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="f60a2-148">En el resto de este tutorial, aprenderá dos métodos de creación de aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f60a2-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="f60a2-149">**Lista 2: `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="f60a2-150">Crear aplicaciones auxiliares HTML con métodos compartidos</span><span class="sxs-lookup"><span data-stu-id="f60a2-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="f60a2-151">La forma más fácil de crear una aplicación auxiliar HTML es crear un método compartido que devuelva una cadena.</span><span class="sxs-lookup"><span data-stu-id="f60a2-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="f60a2-152">Imagine, por ejemplo, que decide crear una nueva aplicación auxiliar HTML que representa una etiqueta de `<label>` HTML.</span><span class="sxs-lookup"><span data-stu-id="f60a2-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="f60a2-153">Puede usar la clase en la lista 2 para representar un `<label>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="f60a2-154">**Lista 2: `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="f60a2-155">No hay nada especial sobre la clase en la lista 2.</span><span class="sxs-lookup"><span data-stu-id="f60a2-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="f60a2-156">El método `Label()` simplemente devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="f60a2-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="f60a2-157">La vista de índice modificada de la lista 3 utiliza el `LabelHelper` para representar etiquetas HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="f60a2-158">Observe que la vista incluye una directiva de `<%@ imports %>` que importa el espacio de nombres application1. helpers.</span><span class="sxs-lookup"><span data-stu-id="f60a2-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="f60a2-159">**Lista 2: `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="f60a2-160">Crear aplicaciones auxiliares HTML con métodos de extensión</span><span class="sxs-lookup"><span data-stu-id="f60a2-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="f60a2-161">Si desea crear aplicaciones auxiliares HTML que funcionen igual que las aplicaciones auxiliares estándar de HTML incluidas en el marco de MVC de ASP.NET, debe crear métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="f60a2-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="f60a2-162">Los métodos de extensión permiten agregar nuevos métodos a una clase existente.</span><span class="sxs-lookup"><span data-stu-id="f60a2-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="f60a2-163">Al crear un método auxiliar HTML, se agregan nuevos métodos a la clase `HtmlHelper` representada por la propiedad HTML de una vista.</span><span class="sxs-lookup"><span data-stu-id="f60a2-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="f60a2-164">El módulo Visual Basic de la lista 3 agrega un método de extensión denominado `Label()` a la clase `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="f60a2-165">Hay un par de cosas que debe tener en cuenta sobre este módulo.</span><span class="sxs-lookup"><span data-stu-id="f60a2-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="f60a2-166">En primer lugar, tenga en cuenta que el módulo está decorado con el atributo `<Extension()>`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="f60a2-167">Para usar este atributo, debe importar el espacio de nombres `System.Runtime.CompilerServices`</span><span class="sxs-lookup"><span data-stu-id="f60a2-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="f60a2-168">En segundo lugar, observe que el primer parámetro del método `Label()` representa la clase `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="f60a2-169">El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.</span><span class="sxs-lookup"><span data-stu-id="f60a2-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="f60a2-170">**Lista 3: `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="f60a2-171">Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en IntelliSense de Visual Studio como todos los demás métodos de una clase (vea la figura 2).</span><span class="sxs-lookup"><span data-stu-id="f60a2-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="f60a2-172">La única diferencia es que los métodos de extensión aparecen con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).</span><span class="sxs-lookup"><span data-stu-id="f60a2-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="f60a2-173">[![mediante el método de extensión html. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f60a2-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="f60a2-174">**Figura 02**: uso del método de extensión html. Label () ([haga clic para ver la imagen de tamaño completo](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f60a2-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="f60a2-175">La vista de índice modificada de la lista 4 usa el método de extensión html. Label () para representar todas las etiquetas de &lt;etiqueta&gt;.</span><span class="sxs-lookup"><span data-stu-id="f60a2-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="f60a2-176">**Lista 4: `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f60a2-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="f60a2-177">Resumen</span><span class="sxs-lookup"><span data-stu-id="f60a2-177">Summary</span></span>

<span data-ttu-id="f60a2-178">En este tutorial, ha aprendido dos métodos de creación de aplicaciones auxiliares HTML personalizadas.</span><span class="sxs-lookup"><span data-stu-id="f60a2-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="f60a2-179">En primer lugar, aprendió a crear una aplicación auxiliar HTML de `Label()` personalizada mediante la creación de un método compartido que devuelva una cadena.</span><span class="sxs-lookup"><span data-stu-id="f60a2-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="f60a2-180">A continuación, aprendió a crear un método auxiliar HTML personalizado `Label()` mediante la creación de un método de extensión en la clase `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="f60a2-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="f60a2-181">En este tutorial, me hemos centrado en la creación de un método auxiliar HTML muy sencillo.</span><span class="sxs-lookup"><span data-stu-id="f60a2-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="f60a2-182">Observe que una aplicación auxiliar HTML puede ser tan complicada como desee.</span><span class="sxs-lookup"><span data-stu-id="f60a2-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="f60a2-183">Puede compilar aplicaciones auxiliares HTML que representen contenido enriquecido, como vistas de árbol, menús o tablas de datos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f60a2-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f60a2-184">[Anterior](asp-net-mvc-views-overview-vb.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f60a2-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
