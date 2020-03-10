---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validación de la entrada del usuario en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo validar la información que obtiene de los usuarios &mdash; es decir, para asegurarse de que los usuarios escriben información válida en formularios HTML en un AS...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454303"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="2e5c1-103">Validación de la entrada del usuario en sitios de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="2e5c1-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="2e5c1-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2e5c1-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2e5c1-105">En este artículo se describe cómo validar la información que obtiene de los usuarios &mdash; es, para asegurarse de que los usuarios escriben información válida en formularios HTML en un sitio de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="2e5c1-106">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="2e5c1-107">Cómo comprobar que la entrada de un usuario coincide con los criterios de validación que se definen.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="2e5c1-108">Cómo determinar si se han superado todas las pruebas de validación.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="2e5c1-109">Cómo mostrar los errores de validación (y cómo darles formato).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="2e5c1-110">Cómo validar los datos que no proceden directamente de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="2e5c1-111">Estos son los conceptos de programación de ASP.NET que se incluyen en el artículo:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="2e5c1-112">Aplicación auxiliar de `Validation`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="2e5c1-113">Los métodos `Html.ValidationSummary` y `Html.ValidationMessage`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2e5c1-114">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="2e5c1-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2e5c1-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="2e5c1-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="2e5c1-116">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="2e5c1-117">Este artículo contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="2e5c1-118">Información general sobre la validación de entradas de usuario</span><span class="sxs-lookup"><span data-stu-id="2e5c1-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="2e5c1-119">Validación de los datos proporcionados por el usuario</span><span class="sxs-lookup"><span data-stu-id="2e5c1-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="2e5c1-120">Agregar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="2e5c1-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="2e5c1-121">Dar formato a errores de validación</span><span class="sxs-lookup"><span data-stu-id="2e5c1-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="2e5c1-122">Validar datos que no proceden directamente de los usuarios</span><span class="sxs-lookup"><span data-stu-id="2e5c1-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="2e5c1-123">Información general sobre la validación de entradas de usuario</span><span class="sxs-lookup"><span data-stu-id="2e5c1-123">Overview of User Input Validation</span></span>

<span data-ttu-id="2e5c1-124">Si pide a los usuarios que escriban información en una página (por ejemplo, en un formulario), es importante asegurarse de que los valores que especifican son válidos.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="2e5c1-125">Por ejemplo, no desea procesar un formulario en el que falta información crítica.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="2e5c1-126">Cuando los usuarios escriben valores en un formulario HTML, los valores que especifican son cadenas.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="2e5c1-127">En muchos casos, los valores que necesita son otros tipos de datos, como enteros o fechas.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="2e5c1-128">Por lo tanto, también tiene que asegurarse de que los valores especificados por los usuarios se pueden convertir correctamente en los tipos de datos adecuados.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="2e5c1-129">También puede tener determinadas restricciones en los valores.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="2e5c1-130">Incluso si los usuarios escriben correctamente un entero, por ejemplo, puede que tenga que asegurarse de que el valor está dentro de un intervalo determinado.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Errores de validación que usan clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="2e5c1-132">**Importante** La validación de los datos proporcionados por el usuario también es importante para la seguridad.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="2e5c1-133">Al restringir los valores que los usuarios pueden especificar en los formularios, se reduce la posibilidad de que alguien pueda especificar un valor que pueda poner en peligro la seguridad de su sitio.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="2e5c1-134">Validar los datos introducidos por el usuario</span><span class="sxs-lookup"><span data-stu-id="2e5c1-134">Validating User Input</span></span>

<span data-ttu-id="2e5c1-135">En ASP.NET Web Pages 2, puede usar la aplicación auxiliar de `Validator` para probar los datos proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="2e5c1-136">El enfoque básico consiste en hacer lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="2e5c1-137">Determine qué elementos de entrada (campos) desea validar.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="2e5c1-138">Normalmente, se validan los valores de `<input>` elementos de un formulario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="2e5c1-139">Sin embargo, se recomienda validar todas las entradas, incluso las que proceden de un elemento restringido como una lista de `<select>`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="2e5c1-140">Esto ayuda a asegurarse de que los usuarios no omiten los controles en una página y envían un formulario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="2e5c1-141">En el código de página, agregue comprobaciones de validación individuales para cada elemento de entrada mediante métodos de la aplicación auxiliar de `Validation`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="2e5c1-142">Para comprobar los campos obligatorios, utilice `Validation.RequireField(field, [error message])` (para un campo individual) o `Validation.RequireFields(field1, field2, ...))` (para obtener una lista de campos).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="2e5c1-143">Para otros tipos de validación, use `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="2e5c1-144">Por `ValidationType`, puede usar estas opciones:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="2e5c1-145">Cuando se envíe la página, compruebe si la validación se ha superado comprobando `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="2e5c1-146">Si hay algún error de validación, se omite el procesamiento de páginas normal.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="2e5c1-147">Por ejemplo, si el propósito de la página es actualizar una base de datos, no lo hace hasta que se hayan corregido todos los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="2e5c1-148">Si hay errores de validación, muestre los mensajes de error en el marcado de la página mediante `Html.ValidationSummary` o `Html.ValidationMessage`, o ambos.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="2e5c1-149">En el ejemplo siguiente se muestra una página que muestra estos pasos.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="2e5c1-150">Para ver cómo funciona la validación, ejecute esta página y realice deliberadamente errores.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="2e5c1-151">Por ejemplo, este es el aspecto de la página si se olvida de escribir un nombre de curso, si escribe un y especifica una fecha no válida:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Errores de validación en la página representada](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="2e5c1-153">Agregar la validación del lado cliente</span><span class="sxs-lookup"><span data-stu-id="2e5c1-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="2e5c1-154">De forma predeterminada, los datos proporcionados por el usuario se validan después de que los usuarios envíen la página; es decir, la validación se realiza en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="2e5c1-155">Una desventaja de este enfoque es que los usuarios no saben que han realizado un error hasta después de enviar la página.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="2e5c1-156">Si un formulario es largo o complejo, los errores de informes solo después de que se envíe la página pueden ser inadecuados para el usuario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="2e5c1-157">Puede Agregar compatibilidad para realizar la validación en el script de cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="2e5c1-158">En ese caso, la validación se realiza a medida que los usuarios trabajan en el explorador.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="2e5c1-159">Por ejemplo, supongamos que especifica que un valor debe ser un entero.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="2e5c1-160">Si un usuario escribe un valor no entero, el error se indica en cuanto el usuario deja el campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="2e5c1-161">Los usuarios obtienen comentarios inmediatos, lo que resulta cómodo para ellos.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="2e5c1-162">La validación basada en el cliente también puede reducir el número de veces que el usuario tiene que enviar el formulario para corregir varios errores.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="2e5c1-163">Incluso si utiliza la validación del lado cliente, la validación siempre se realiza también en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="2e5c1-164">La realización de la validación en el código del servidor es una medida de seguridad, en caso de que los usuarios omitan la validación basada en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>

1. <span data-ttu-id="2e5c1-165">Registre las siguientes bibliotecas de JavaScript en la página:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="2e5c1-166">Dos de las bibliotecas se pueden cargar desde una red de entrega de contenido (CDN), por lo que no es necesario que las tenga en el equipo o servidor.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="2e5c1-167">Sin embargo, debe tener una copia local de *jQuery. Validate. discreto. js*.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="2e5c1-168">Si aún no está trabajando con una plantilla de WebMatrix (como el **sitio inicial** ) que incluye la biblioteca, cree un sitio de páginas web basado en el **sitio de inicio**.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="2e5c1-169">A continuación, copie el archivo *. js* en el sitio actual.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="2e5c1-170">En el marcado, para cada elemento que se está validando, agregue una llamada a `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="2e5c1-171">Este método emite atributos que se usan en la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="2e5c1-172">(En lugar de emitir código JavaScript real, el método emite atributos como `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="2e5c1-173">Estos atributos admiten la validación discreta del cliente que usa jQuery para realizar el trabajo.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="2e5c1-174">En la página siguiente se muestra cómo agregar características de validación de cliente al ejemplo mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="2e5c1-175">No todas las comprobaciones de validación se ejecutan en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="2e5c1-176">En concreto, la validación de tipo de datos (entero, fecha, etc.) no se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="2e5c1-177">Las siguientes comprobaciones funcionan tanto en el cliente como en el servidor:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="2e5c1-178">En este ejemplo, la prueba de una fecha válida no funcionará en el código de cliente.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="2e5c1-179">Sin embargo, la prueba se realizará en el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="2e5c1-180">Dar formato a errores de validación</span><span class="sxs-lookup"><span data-stu-id="2e5c1-180">Formatting Validation Errors</span></span>

<span data-ttu-id="2e5c1-181">Puede controlar cómo se muestran los errores de validación definiendo las clases CSS que tienen los siguientes nombres reservados:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="2e5c1-182">`field-validation-error`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-182">`field-validation-error`.</span></span> <span data-ttu-id="2e5c1-183">Define la salida del método `Html.ValidationMessage` cuando se muestra un error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="2e5c1-184">`field-validation-valid`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-184">`field-validation-valid`.</span></span> <span data-ttu-id="2e5c1-185">Define la salida del método `Html.ValidationMessage` cuando no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="2e5c1-186">`input-validation-error`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-186">`input-validation-error`.</span></span> <span data-ttu-id="2e5c1-187">Define cómo se representan los elementos de `<input>` cuando hay un error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="2e5c1-188">(Por ejemplo, puede usar esta clase para establecer el color de fondo de una &lt;entrada&gt; elemento en un color diferente si su valor no es válido). Esta clase CSS solo se usa durante la validación de cliente (en ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="2e5c1-189">`input-validation-valid`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-189">`input-validation-valid`.</span></span> <span data-ttu-id="2e5c1-190">Define la apariencia de los elementos de `<input>` cuando no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="2e5c1-191">`validation-summary-errors`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-191">`validation-summary-errors`.</span></span> <span data-ttu-id="2e5c1-192">Define la salida del método `Html.ValidationSummary` se muestra una lista de errores.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="2e5c1-193">`validation-summary-valid`Operador</span><span class="sxs-lookup"><span data-stu-id="2e5c1-193">`validation-summary-valid`.</span></span> <span data-ttu-id="2e5c1-194">Define la salida del método `Html.ValidationSummary` cuando no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="2e5c1-195">En el siguiente bloque de `<style>` se muestran reglas para las condiciones de error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="2e5c1-196">Si incluye este bloque de estilo en las páginas de ejemplo de anteriormente en el artículo, la presentación del error tendrá el aspecto siguiente:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Errores de validación que usan clases de estilo CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="2e5c1-198">Si no está utilizando la validación de cliente en ASP.NET Web Pages 2, las clases CSS para los elementos `<input>` (`input-validation-error` y `input-validation-valid` no tienen ningún efecto.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>

### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="2e5c1-199">Presentación de errores estáticos y dinámicos</span><span class="sxs-lookup"><span data-stu-id="2e5c1-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="2e5c1-200">Las reglas de CSS se encuentran en pares, como `validation-summary-errors` y `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="2e5c1-201">Estos pares permiten definir reglas para ambas condiciones: una condición de error y una condición "normal" (sin errores).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="2e5c1-202">Es importante comprender que el marcado de la presentación de errores siempre se representa, incluso si no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="2e5c1-203">Por ejemplo, si una página tiene un método `Html.ValidationSummary` en el marcado, el origen de la página contendrá el marcado siguiente incluso cuando se solicite la página por primera vez:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="2e5c1-204">En otras palabras, el método `Html.ValidationSummary` siempre representa un elemento `<div>` y una lista, incluso si la lista de errores está vacía.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="2e5c1-205">Del mismo modo, el método `Html.ValidationMessage` siempre representa un elemento `<span>` como un marcador de posición para un error de campo individual, incluso si no hay ningún error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="2e5c1-206">En algunas situaciones, mostrar un mensaje de error puede hacer que la página se retransmita y puede hacer que los elementos de la página se desplacen.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="2e5c1-207">Las reglas de CSS que acaban en `-valid` permiten definir un diseño que puede ayudar a evitar este problema.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="2e5c1-208">Por ejemplo, puede definir `field-validation-error` y `field-validation-valid` tienen el mismo tamaño fijo.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="2e5c1-209">De este modo, el área de visualización del campo es estática y no cambiará el flujo de la página si se muestra un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="2e5c1-210">Validar datos que no proceden directamente de los usuarios</span><span class="sxs-lookup"><span data-stu-id="2e5c1-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="2e5c1-211">A veces, es necesario validar la información que no procede directamente de un formulario HTML.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="2e5c1-212">Un ejemplo típico es una página en la que se pasa un valor en una cadena de consulta, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="2e5c1-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="2e5c1-213">En este caso, querrá asegurarse de que el valor que se pasa a la página (aquí, 1022 para el valor de `classid`) es válido.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="2e5c1-214">No se puede usar directamente la aplicación auxiliar de `Validation` para realizar esta validación.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="2e5c1-215">Sin embargo, puede usar otras características del sistema de validación, como la capacidad de mostrar mensajes de error de validación.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="2e5c1-216">**Importante** Valide siempre los valores que obtiene de *cualquier* origen, incluidos los valores de campo de formulario, los valores de cadena de consulta y los valores de cookie.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="2e5c1-217">Es fácil para los usuarios cambiar estos valores (quizás con fines malintencionados).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="2e5c1-218">Por lo tanto, debe comprobar estos valores para proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-218">So you must check these values in order to protect your application.</span></span>

<span data-ttu-id="2e5c1-219">En el ejemplo siguiente se muestra cómo se puede validar un valor que se pasa en una cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="2e5c1-220">El código prueba que el valor no está vacío y es un entero.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="2e5c1-221">Tenga en cuenta que la prueba se realiza cuando la solicitud no es un envío de formulario (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="2e5c1-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="2e5c1-222">Esta prueba se pasaría la primera vez que se solicita la página, pero no cuando la solicitud es un envío de formulario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="2e5c1-223">Para mostrar este error, puede Agregar el error a la lista de errores de validación mediante una llamada a `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="2e5c1-224">Si la página contiene una llamada al método `Html.ValidationSummary`, el error se muestra allí, como si se tratara de un error de validación de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="2e5c1-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="2e5c1-225">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2e5c1-225">Additional Resources</span></span>

[<span data-ttu-id="2e5c1-226">Trabajar con formularios HTML en sitios de ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="2e5c1-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
