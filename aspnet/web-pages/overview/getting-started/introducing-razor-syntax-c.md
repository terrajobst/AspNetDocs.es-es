---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introducción a la programación web de ASP.NET mediante la sintaxisC#de Razor () | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se ofrece información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor. ASP.NET es la tecnología de Microsoft para ejecutar Dynamic Web PA...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521209"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="94cfe-104">Introducción a la programación web de ASP.NET mediante la sintaxisC#Razor ()</span><span class="sxs-lookup"><span data-stu-id="94cfe-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>

<span data-ttu-id="94cfe-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="94cfe-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="94cfe-106">En este artículo se proporciona información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="94cfe-107">ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores Web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="94cfe-108">Este artículo se centra en el C# uso del lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="94cfe-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="94cfe-109">**Lo que aprenderá**:</span><span class="sxs-lookup"><span data-stu-id="94cfe-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="94cfe-110">Las 8 mejores sugerencias de programación para empezar a programar ASP.NET Web Pages mediante sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="94cfe-111">Conceptos de programación básicos que necesitará.</span><span class="sxs-lookup"><span data-stu-id="94cfe-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="94cfe-112">Qué es el código del servidor de ASP.NET y el sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="94cfe-113">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="94cfe-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="94cfe-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="94cfe-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="94cfe-115">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="94cfe-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="94cfe-116">Las 8 mejores sugerencias de programación</span><span class="sxs-lookup"><span data-stu-id="94cfe-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="94cfe-117">En esta sección se enumeran algunas sugerencias que es absolutamente necesario saber a medida que empieza a escribir código de servidor de ASP.NET mediante el sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="94cfe-118">El sintaxis Razor se basa en el C# lenguaje de programación, y es el lenguaje que se usa con más frecuencia con ASP.NET Web pages.</span><span class="sxs-lookup"><span data-stu-id="94cfe-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="94cfe-119">Sin embargo, el sintaxis Razor también admite el lenguaje de Visual Basic y todo lo que se ve también puede realizarse en Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="94cfe-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="94cfe-120">Para obtener más información, consulte el apéndice [Visual Basic lenguaje y la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="94cfe-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>

<span data-ttu-id="94cfe-121">Puede encontrar más detalles sobre la mayoría de estas técnicas de programación más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="94cfe-122">1. Agregue código a una página mediante el carácter @.</span><span class="sxs-lookup"><span data-stu-id="94cfe-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="94cfe-123">El carácter `@` inicia las expresiones en línea, los bloques de instrucción única y los bloques de varias instrucciones:</span><span class="sxs-lookup"><span data-stu-id="94cfe-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="94cfe-124">Este es el aspecto de estas instrucciones cuando la página se ejecuta en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor: img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="94cfe-126">**Codificación HTML**</span><span class="sxs-lookup"><span data-stu-id="94cfe-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="94cfe-127">Al mostrar el contenido en una página mediante el carácter `@`, como en los ejemplos anteriores, ASP.NET codifica el resultado en HTML.</span><span class="sxs-lookup"><span data-stu-id="94cfe-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="94cfe-128">Esto reemplaza los caracteres HTML reservados (como `<` y `>` y `&`) con códigos que permiten mostrar los caracteres como caracteres en una página web en lugar de interpretarlos como etiquetas o entidades HTML.</span><span class="sxs-lookup"><span data-stu-id="94cfe-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="94cfe-129">Sin la codificación HTML, la salida del código del servidor podría no mostrarse correctamente y podría exponer una página a los riesgos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="94cfe-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="94cfe-130">Si el objetivo es generar el formato HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), vea la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="94cfe-131">Puede obtener más información sobre la codificación HTML en [trabajar con formularios](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="94cfe-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="94cfe-132">2. incluir los bloques de código entre llaves</span><span class="sxs-lookup"><span data-stu-id="94cfe-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="94cfe-133">Un *bloque de código* incluye una o varias instrucciones de código y está entre llaves.</span><span class="sxs-lookup"><span data-stu-id="94cfe-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="94cfe-134">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-134">The result displayed in a browser:</span></span>

![Razor: Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="94cfe-136">3. dentro de un bloque, finaliza cada instrucción de código con un punto y coma</span><span class="sxs-lookup"><span data-stu-id="94cfe-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="94cfe-137">Dentro de un bloque de código, cada instrucción de código completo debe terminar con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="94cfe-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="94cfe-138">Las expresiones insertadas no finalizan con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="94cfe-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="94cfe-139">4. Utilice variables para almacenar valores</span><span class="sxs-lookup"><span data-stu-id="94cfe-139">4. You use variables to store values</span></span>

<span data-ttu-id="94cfe-140">Puede almacenar valores en una *variable*, como cadenas, números y fechas, etc. Cree una nueva variable mediante la palabra clave `var`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="94cfe-141">Los valores de las variables se pueden insertar directamente en una página mediante `@`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="94cfe-142">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-142">The result displayed in a browser:</span></span>

![Razor: Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="94cfe-144">5. incluir valores de cadena literales entre comillas dobles</span><span class="sxs-lookup"><span data-stu-id="94cfe-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="94cfe-145">Una *cadena* es una secuencia de caracteres que se trata como texto.</span><span class="sxs-lookup"><span data-stu-id="94cfe-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="94cfe-146">Para especificar una cadena, debe encerrarla entre comillas dobles:</span><span class="sxs-lookup"><span data-stu-id="94cfe-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="94cfe-147">Si la cadena que desea mostrar contiene un carácter de barra diagonal inversa (`\`) o comillas dobles (`"`), use un *literal de cadena textual* que tenga como prefijo el operador `@`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="94cfe-148">(En C#, el carácter \ tiene un significado especial a menos que use un literal de cadena textual).</span><span class="sxs-lookup"><span data-stu-id="94cfe-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="94cfe-149">Para insertar comillas dobles, use un literal de cadena textual y repita las comillas:</span><span class="sxs-lookup"><span data-stu-id="94cfe-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="94cfe-150">Este es el resultado de usar estos dos ejemplos en una página:</span><span class="sxs-lookup"><span data-stu-id="94cfe-150">Here's the result of using both of these examples in a page:</span></span>

![Razor: Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="94cfe-152">Observe que el carácter `@` se usa para marcar los literales de cadena textual C# en y para marcar el código en las páginas de ASP.net.</span><span class="sxs-lookup"><span data-stu-id="94cfe-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>

### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="94cfe-153">6. el código distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="94cfe-153">6. Code is case sensitive</span></span>

<span data-ttu-id="94cfe-154">En C#, las palabras clave (como `var`, `true`y `if`) y los nombres de variable distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="94cfe-155">Las siguientes líneas de código crean dos variables diferentes, `lastName` y `LastName.`</span><span class="sxs-lookup"><span data-stu-id="94cfe-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="94cfe-156">Si declara una variable como `var lastName = "Smith";` e intenta hacer referencia a esa variable en la página como `@LastName`, obtendría el valor `"Jones"` en lugar de `"Smith"`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-156">If you declare a variable as `var lastName = "Smith";` and try to reference that variable in your page as `@LastName`, you would get the value `"Jones"` instead of `"Smith"`.</span></span>

> [!NOTE]
> <span data-ttu-id="94cfe-157">En Visual Basic, las palabras clave y las variables *no* distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>

### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="94cfe-158">7. gran parte de la codificación implica objetos</span><span class="sxs-lookup"><span data-stu-id="94cfe-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="94cfe-159">Un *objeto* representa un elemento que puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud Web, un mensaje de correo electrónico, un registro de cliente (fila de base de datos), etc. Los objetos tienen propiedades que describen sus características y que puede leer o cambiar &#8212; un objeto de cuadro de texto tiene una propiedad `Text` (entre otros), un objeto de solicitud tiene una propiedad `Url`, un mensaje de correo electrónico tiene una propiedad `From` y un objeto Customer tiene una propiedad `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="94cfe-160">Los objetos también tienen métodos que son los &quot;verbos&quot; pueden realizar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="94cfe-161">Entre los ejemplos se incluye el método de `Save` de un objeto de archivo, el método de `Rotate` de un objeto de imagen y el método `Send` de un objeto de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="94cfe-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="94cfe-162">A menudo, trabajará con el objeto `Request`, que le proporciona información como los valores de los cuadros de texto (campos de formulario) en la página, el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc. En el ejemplo siguiente se muestra cómo obtener acceso a las propiedades del objeto `Request` y cómo llamar al método `MapPath` del objeto `Request`, que proporciona la ruta de acceso absoluta de la página en el servidor:</span><span class="sxs-lookup"><span data-stu-id="94cfe-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="94cfe-163">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-163">The result displayed in a browser:</span></span>

![Razor: Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="94cfe-165">8. puede escribir código que tome decisiones</span><span class="sxs-lookup"><span data-stu-id="94cfe-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="94cfe-166">Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de las condiciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="94cfe-167">La forma más común de hacerlo es con la instrucción `if` (y la instrucción `else` opcional).</span><span class="sxs-lookup"><span data-stu-id="94cfe-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="94cfe-168">La instrucción `if(IsPost)` es una forma abreviada de escribir `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="94cfe-169">Junto con las instrucciones `if`, hay varias maneras de probar condiciones, repetir bloques de código, etc., que se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="94cfe-170">El resultado mostrado en un explorador (después de hacer clic en **submit**):</span><span class="sxs-lookup"><span data-stu-id="94cfe-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor: Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="94cfe-172">Métodos GET y POST de HTTP y la propiedad IsPost</span><span class="sxs-lookup"><span data-stu-id="94cfe-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="94cfe-173">El protocolo usado para páginas web (HTTP) admite un número muy limitado de métodos (verbos) que se usan para hacer solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="94cfe-174">Los dos más comunes son GET, que se usa para leer una página y POST, que se usa para enviar una página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="94cfe-175">En general, la primera vez que un usuario solicita una página, se solicita la página mediante GET.</span><span class="sxs-lookup"><span data-stu-id="94cfe-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="94cfe-176">Si el usuario rellena un formulario y, a continuación, hace clic en un botón Enviar, el explorador realiza una solicitud POST al servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="94cfe-177">En la programación web, a menudo resulta útil saber si se solicita una página como GET o como una entrada para que sepa cómo procesar la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="94cfe-178">En ASP.NET Web Pages, puede usar la propiedad `IsPost` para ver si una solicitud es GET o POST.</span><span class="sxs-lookup"><span data-stu-id="94cfe-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="94cfe-179">Si la solicitud es una publicación, la propiedad `IsPost` devolverá True y puede hacer cosas como leer los valores de los cuadros de texto de un formulario.</span><span class="sxs-lookup"><span data-stu-id="94cfe-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="94cfe-180">Muchos ejemplos que verá muestran cómo procesar la página de forma diferente en función del valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="94cfe-181">Un ejemplo de código simple</span><span class="sxs-lookup"><span data-stu-id="94cfe-181">A Simple Code Example</span></span>

<span data-ttu-id="94cfe-182">En este procedimiento se muestra cómo crear una página que muestra las técnicas básicas de programación.</span><span class="sxs-lookup"><span data-stu-id="94cfe-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="94cfe-183">En el ejemplo, creará una página que permite a los usuarios escribir dos números y, después, los agregará y mostrará el resultado.</span><span class="sxs-lookup"><span data-stu-id="94cfe-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="94cfe-184">En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="94cfe-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="94cfe-185">Copie el código y el marcado siguientes en la página, reemplazando todo lo que ya está en la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="94cfe-186">A continuación se indican algunas cosas que debe tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="94cfe-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="94cfe-187">El carácter `@` inicia el primer bloque de código de la página y precede a la variable `totalMessage` que está incrustada cerca de la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="94cfe-188">El bloque situado en la parte superior de la página se incluye entre llaves.</span><span class="sxs-lookup"><span data-stu-id="94cfe-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="94cfe-189">En el bloque de la parte superior, todas las líneas terminan con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="94cfe-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="94cfe-190">Las variables `total`, `num1`, `num2`y `totalMessage` almacenan varios números y una cadena.</span><span class="sxs-lookup"><span data-stu-id="94cfe-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="94cfe-191">El valor de cadena literal asignado a la variable `totalMessage` está entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="94cfe-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="94cfe-192">Dado que el código distingue entre mayúsculas y minúsculas, cuando la variable de `totalMessage` se usa cerca de la parte inferior de la página, su nombre debe coincidir exactamente con la variable en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="94cfe-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="94cfe-193">La expresión `num1.AsInt() + num2.AsInt()` muestra cómo trabajar con objetos y métodos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="94cfe-194">El método `AsInt` de cada variable convierte la cadena especificada por un usuario en un número (un entero) para que pueda realizar operaciones aritméticas en ella.</span><span class="sxs-lookup"><span data-stu-id="94cfe-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="94cfe-195">La etiqueta `<form>` incluye un atributo `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="94cfe-196">Esto especifica que cuando el usuario haga clic en **Agregar**, la página se enviará al servidor mediante el método http post.</span><span class="sxs-lookup"><span data-stu-id="94cfe-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="94cfe-197">Cuando se envía la página, la prueba `if(IsPost)` se evalúa como true y se ejecuta el código condicional, mostrando el resultado de sumar los números.</span><span class="sxs-lookup"><span data-stu-id="94cfe-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="94cfe-198">Guarde la página y ejecútela en un explorador.</span><span class="sxs-lookup"><span data-stu-id="94cfe-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="94cfe-199">(Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Escriba dos números enteros y, a continuación, haga clic en el botón **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="94cfe-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor: Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="94cfe-201">Conceptos básicos de la programación</span><span class="sxs-lookup"><span data-stu-id="94cfe-201">Basic Programming Concepts</span></span>

<span data-ttu-id="94cfe-202">En este artículo se proporciona información general sobre la programación web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="94cfe-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="94cfe-203">No es un examen exhaustivo, solo un paseo rápido por los conceptos de programación que usará con mayor frecuencia.</span><span class="sxs-lookup"><span data-stu-id="94cfe-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="94cfe-204">Incluso así, cubre casi todo lo que necesita para empezar a trabajar con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="94cfe-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="94cfe-205">Pero en primer lugar, un poco de conocimientos técnicos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="94cfe-206">La sintaxis de Razor, el código de servidor y ASP.NET</span><span class="sxs-lookup"><span data-stu-id="94cfe-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="94cfe-207">Sintaxis Razor es una sintaxis de programación sencilla para insertar código basado en servidor en una página web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="94cfe-208">En una página web que utiliza el sintaxis Razor, hay dos tipos de contenido: contenido del cliente y código del servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="94cfe-209">El contenido del cliente es el material que se usa en las páginas web: marcado HTML (elementos), información de estilo como CSS, quizás algún script de cliente como JavaScript y texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="94cfe-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="94cfe-210">Sintaxis Razor le permite agregar código de servidor a este contenido de cliente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="94cfe-211">Si hay código de servidor en la página, el servidor ejecuta primero ese código, antes de enviar la página al explorador.</span><span class="sxs-lookup"><span data-stu-id="94cfe-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="94cfe-212">Mediante la ejecución de en el servidor, el código puede realizar tareas que pueden ser mucho más complejas de utilizar solo el contenido del cliente, como el acceso a bases de datos basadas en el servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="94cfe-213">Lo más importante es que el código de servidor puede crear dinámicamente el contenido &#8212; del cliente y puede generar marcado HTML u otro contenido sobre la marcha y, a continuación, enviarlo al explorador junto con cualquier código HTML estático que pueda contener la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="94cfe-214">Desde la perspectiva del explorador, el contenido de cliente generado por el código del servidor no es diferente de ningún otro contenido de cliente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="94cfe-215">Como ya ha visto, el código de servidor necesario es bastante sencillo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="94cfe-216">Las páginas web de ASP.NET que incluyen la sintaxis Razor tienen una extensión de archivo especial ( *. cshtml* o *. vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="94cfe-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="94cfe-217">El servidor reconoce estas extensiones, ejecuta el código que está marcado con sintaxis Razor y, a continuación, envía la página al explorador.</span><span class="sxs-lookup"><span data-stu-id="94cfe-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="94cfe-218">¿Dónde encaja ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="94cfe-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="94cfe-219">Sintaxis Razor se basa en una tecnología de Microsoft llamada ASP.NET, que a su vez se basa en el marco de Microsoft .NET.</span><span class="sxs-lookup"><span data-stu-id="94cfe-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="94cfe-220">The.NET Framework es un marco de programación amplio y completo de Microsoft para desarrollar prácticamente cualquier tipo de aplicación de equipos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="94cfe-221">ASP.NET es la parte de la .NET Framework que está diseñada específicamente para la creación de aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="94cfe-222">Los desarrolladores han usado ASP.NET para crear muchos de los sitios web de mayor y mayor tráfico del mundo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="94cfe-223">(Siempre que vea la extensión de nombre de archivo *. aspx* como parte de la dirección URL de un sitio, sabrá que el sitio se escribió con ASP.net).</span><span class="sxs-lookup"><span data-stu-id="94cfe-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="94cfe-224">El sintaxis Razor le proporciona toda la eficacia de ASP.NET, pero con una sintaxis simplificada que es más fácil de aprender si es un principiante y que le permite ser más productivo si es un experto.</span><span class="sxs-lookup"><span data-stu-id="94cfe-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="94cfe-225">Aunque esta sintaxis es fácil de usar, su relación de familia con ASP.NET y el .NET Framework significa que, a medida que sus sitios web son más sofisticados, tiene la capacidad de los marcos de trabajo más grandes a su disposición.</span><span class="sxs-lookup"><span data-stu-id="94cfe-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor: Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="94cfe-227">**Clases e instancias**</span><span class="sxs-lookup"><span data-stu-id="94cfe-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="94cfe-228">El código de servidor ASP.NET usa objetos, que a su vez se basan en la idea de clases.</span><span class="sxs-lookup"><span data-stu-id="94cfe-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="94cfe-229">La clase es la definición o la plantilla de un objeto.</span><span class="sxs-lookup"><span data-stu-id="94cfe-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="94cfe-230">Por ejemplo, una aplicación podría contener una clase `Customer` que define las propiedades y los métodos que necesita cualquier objeto Customer.</span><span class="sxs-lookup"><span data-stu-id="94cfe-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="94cfe-231">Cuando la aplicación necesita trabajar con información de clientes real, crea una instancia de (o *crea instancias*) de un objeto de cliente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="94cfe-232">Cada cliente individual es una instancia independiente de la clase `Customer`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="94cfe-233">Cada instancia de admite las mismas propiedades y métodos, pero los valores de propiedad de cada instancia son normalmente diferentes, ya que cada objeto de cliente es único.</span><span class="sxs-lookup"><span data-stu-id="94cfe-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="94cfe-234">En un objeto Customer, la propiedad `LastName` podría ser "Smith"; en otro objeto de cliente, la propiedad `LastName` podría ser "Jones".</span><span class="sxs-lookup"><span data-stu-id="94cfe-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="94cfe-235">Del mismo modo, cualquier página web individual del sitio es un objeto `Page` que es una instancia de la clase `Page`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="94cfe-236">Un botón de la página es un objeto `Button` que es una instancia de la clase `Button`, etc.</span><span class="sxs-lookup"><span data-stu-id="94cfe-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="94cfe-237">Cada instancia tiene sus propias características, pero todas se basan en lo que se especifica en la definición de clase del objeto.</span><span class="sxs-lookup"><span data-stu-id="94cfe-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>

## <a name="basic-syntax"></a><span data-ttu-id="94cfe-238">Sintaxis básica</span><span class="sxs-lookup"><span data-stu-id="94cfe-238">Basic Syntax</span></span>

<span data-ttu-id="94cfe-239">Antes vio un ejemplo básico de cómo crear una página ASP.NET Web Pages y cómo puede agregar código de servidor al marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="94cfe-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="94cfe-240">Aquí aprenderá los conceptos básicos de la escritura de código de servidor ASP.NET mediante &#8212; el sintaxis Razor es decir, las reglas del lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="94cfe-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="94cfe-241">Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que Lee aquí le resultará familiar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="94cfe-242">Probablemente necesitará familiarizarse solo con el modo en que se agrega el código de servidor al marcado en los archivos *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="94cfe-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="94cfe-243">Combinar texto, marcado y código en bloques de código</span><span class="sxs-lookup"><span data-stu-id="94cfe-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="94cfe-244">En los bloques de código de servidor, a menudo desea que se genere texto o marcado (o ambos) en la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="94cfe-245">Si un bloque de código de servidor contiene texto que no es código y que, en su lugar, se debe representar como es, ASP.NET debe poder distinguir ese texto del código.</span><span class="sxs-lookup"><span data-stu-id="94cfe-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="94cfe-246">Existen varias formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-246">There are several ways to do this.</span></span>

- <span data-ttu-id="94cfe-247">Incluya el texto en un elemento HTML como `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="94cfe-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="94cfe-248">El elemento HTML puede incluir texto, elementos HTML adicionales y expresiones de código de servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="94cfe-249">Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), representa todo lo que incluye el elemento y su contenido tal como está en el explorador, resolviendo las expresiones de código de servidor a medida que van.</span><span class="sxs-lookup"><span data-stu-id="94cfe-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="94cfe-250">Use el operador `@:` o el elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="94cfe-251">El `@:` genera una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el elemento `<text>` incluye varias líneas para la salida.</span><span class="sxs-lookup"><span data-stu-id="94cfe-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="94cfe-252">Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.</span><span class="sxs-lookup"><span data-stu-id="94cfe-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="94cfe-253">Si desea generar varias líneas de texto o etiquetas HTML no coincidentes, puede preceder cada línea con `@:`, o puede incluir la línea en un elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="94cfe-254">Al igual que el operador de `@:`, las etiquetas de`<text>` se usan en ASP.NET para identificar el contenido de texto y nunca se representan en el resultado de la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="94cfe-255">En el primer ejemplo se repite el ejemplo anterior, pero se usa un solo par de etiquetas `<text>` para incluir el texto que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="94cfe-256">En el segundo ejemplo, las etiquetas `<text>` y `</text>` incluyen tres líneas, todas ellas tienen un texto no contenida y etiquetas HTML no coincidentes (`<br />`), junto con el código del servidor y las etiquetas HTML coincidentes.</span><span class="sxs-lookup"><span data-stu-id="94cfe-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="94cfe-257">De nuevo, también podría preceder cada línea individualmente con el operador `@:`; ambos métodos funcionan.</span><span class="sxs-lookup"><span data-stu-id="94cfe-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="94cfe-258">Cuando se genera texto como se muestra en esta &#8212; sección mediante un elemento HTML, el operador `@:` o el elemento &#8212; `<text>` ASP.net no codifica en HTML el resultado.</span><span class="sxs-lookup"><span data-stu-id="94cfe-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="94cfe-259">(Como se indicó anteriormente, ASP.NET codifica la salida de las expresiones de código de servidor y los bloques de código de servidor precedidos por `@`, excepto en los casos especiales que se indican en esta sección).</span><span class="sxs-lookup"><span data-stu-id="94cfe-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="94cfe-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="94cfe-260">Whitespace</span></span>

<span data-ttu-id="94cfe-261">Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:</span><span class="sxs-lookup"><span data-stu-id="94cfe-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="94cfe-262">Un salto de línea en una instrucción no tiene ningún efecto en la instrucción y puede ajustar las instrucciones para mejorar la legibilidad.</span><span class="sxs-lookup"><span data-stu-id="94cfe-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="94cfe-263">Las siguientes instrucciones son iguales:</span><span class="sxs-lookup"><span data-stu-id="94cfe-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="94cfe-264">Sin embargo, no se puede ajustar una línea en medio de un literal de cadena.</span><span class="sxs-lookup"><span data-stu-id="94cfe-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="94cfe-265">El ejemplo siguiente no funciona:</span><span class="sxs-lookup"><span data-stu-id="94cfe-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="94cfe-266">Para combinar una cadena larga que se ajusta a varias líneas como el código anterior, hay dos opciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="94cfe-267">Puede usar el operador de concatenación (`+`), que verá más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="94cfe-268">También puede usar el carácter `@` para crear un literal de cadena textual, como vimos anteriormente en este artículo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="94cfe-269">Puede romper los literales de cadena textual entre las líneas:</span><span class="sxs-lookup"><span data-stu-id="94cfe-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="94cfe-270">Comentarios de código (y marcado)</span><span class="sxs-lookup"><span data-stu-id="94cfe-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="94cfe-271">Los comentarios le permiten dejar notas por su cuenta o por otras personas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="94cfe-272">También le permiten deshabilitar (*comentar*) una sección de código o de marcado que no desea ejecutar, pero que desea mantener en la página por el momento.</span><span class="sxs-lookup"><span data-stu-id="94cfe-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="94cfe-273">Hay una sintaxis de comentario diferente para el código Razor y para el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="94cfe-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="94cfe-274">Al igual que con todo el código Razor, los comentarios de Razor se procesan (y luego se quitan) en el servidor antes de que la página se envíe al explorador.</span><span class="sxs-lookup"><span data-stu-id="94cfe-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="94cfe-275">Por lo tanto, la sintaxis de comentarios de Razor permite colocar comentarios en el código (o incluso en el marcado) que se pueden ver al editar el archivo, pero que los usuarios no ven, ni siquiera en el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="94cfe-276">En el caso de los comentarios de Razor ASP.NET, inicie el comentario con `@*` y termine con `*@`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="94cfe-277">El comentario puede estar en una o varias líneas:</span><span class="sxs-lookup"><span data-stu-id="94cfe-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="94cfe-278">Este es un comentario dentro de un bloque de código:</span><span class="sxs-lookup"><span data-stu-id="94cfe-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="94cfe-279">Este es el mismo bloque de código, con la línea de código comentada para que no se ejecute:</span><span class="sxs-lookup"><span data-stu-id="94cfe-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="94cfe-280">Dentro de un bloque de código, como alternativa al uso de la sintaxis de comentarios de Razor, puede usar la sintaxis de comentarios del lenguaje de programación que está C#usando, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94cfe-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="94cfe-281">En C#, los comentarios de una línea van precedidos de los caracteres `//` y los comentarios de varias líneas comienzan con `/*` y terminan con `*/`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="94cfe-282">(Al igual que con los C# comentarios de Razor, los comentarios no se representan en el explorador).</span><span class="sxs-lookup"><span data-stu-id="94cfe-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="94cfe-283">Para el marcado, como probablemente sepa, puede crear un Comentario HTML:</span><span class="sxs-lookup"><span data-stu-id="94cfe-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="94cfe-284">Los comentarios HTML comienzan con `<!--` caracteres y terminan con `-->`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="94cfe-285">Puede usar comentarios HTML para rodear no solo texto, sino también cualquier código HTML que desee conservar en la página, pero que no quiera que se represente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="94cfe-286">Este comentario HTML ocultará todo el contenido de las etiquetas y el texto que contienen:</span><span class="sxs-lookup"><span data-stu-id="94cfe-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="94cfe-287">A diferencia de los comentarios de Razor, los comentarios HTML *se* representan en la página y el usuario puede verlos viendo el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="94cfe-288">Razor tiene limitaciones en los bloques anidados C#de.</span><span class="sxs-lookup"><span data-stu-id="94cfe-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="94cfe-289">Para obtener más información [, C# consulte las variables con nombre y los bloques anidados generar código roto](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="94cfe-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="94cfe-290">variables</span><span class="sxs-lookup"><span data-stu-id="94cfe-290">Variables</span></span>

<span data-ttu-id="94cfe-291">Una variable es un objeto con nombre que se utiliza para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="94cfe-292">Puede asignar cualquier valor a las variables, pero el nombre debe comenzar por un carácter alfabético y no puede contener espacios en blanco ni caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="94cfe-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="94cfe-293">Variables y tipos de datos</span><span class="sxs-lookup"><span data-stu-id="94cfe-293">Variables and Data Types</span></span>

<span data-ttu-id="94cfe-294">Una variable puede tener un tipo de datos específico, que indica qué tipo de datos se almacenan en la variable.</span><span class="sxs-lookup"><span data-stu-id="94cfe-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="94cfe-295">Puede tener variables de cadena que almacenen valores de cadena (como &quot;Hello World&quot;), variables de entero que almacenan valores de número entero (como 3 o 79) y variables de fecha que almacenan valores de fecha en una variedad de formatos (como 4/12/2012 o marzo de 2009).</span><span class="sxs-lookup"><span data-stu-id="94cfe-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="94cfe-296">Y hay muchos otros tipos de datos que puede usar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="94cfe-297">Sin embargo, por lo general no es necesario especificar un tipo para una variable.</span><span class="sxs-lookup"><span data-stu-id="94cfe-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="94cfe-298">La mayoría de las veces, ASP.NET puede averiguar el tipo en función de cómo se usan los datos de la variable.</span><span class="sxs-lookup"><span data-stu-id="94cfe-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="94cfe-299">(En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).</span><span class="sxs-lookup"><span data-stu-id="94cfe-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="94cfe-300">Declare una variable mediante la palabra clave `var` (si no desea especificar un tipo) o con el nombre del tipo:</span><span class="sxs-lookup"><span data-stu-id="94cfe-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="94cfe-301">En el ejemplo siguiente se muestran algunos usos típicos de las variables en una página web:</span><span class="sxs-lookup"><span data-stu-id="94cfe-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="94cfe-302">Si combina los ejemplos anteriores en una página, verá que se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor: Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="94cfe-304">Conversión y prueba de tipos de datos</span><span class="sxs-lookup"><span data-stu-id="94cfe-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="94cfe-305">Aunque ASP.NET normalmente puede determinar un tipo de datos de forma automática, a veces no es posible.</span><span class="sxs-lookup"><span data-stu-id="94cfe-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="94cfe-306">Por lo tanto, es posible que necesite ayudar a ASP.NET a realizar una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="94cfe-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="94cfe-307">Incluso si no tiene que convertir tipos, a veces resulta útil probar para ver el tipo de datos con el que puede trabajar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="94cfe-308">El caso más común es que tenga que convertir una cadena en otro tipo, como un entero o una fecha.</span><span class="sxs-lookup"><span data-stu-id="94cfe-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="94cfe-309">En el ejemplo siguiente se muestra un caso típico en el que debe convertir una cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="94cfe-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="94cfe-310">Como regla, los datos proporcionados por el usuario se incluyen como cadenas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="94cfe-311">Incluso si ha solicitado que los usuarios escriban un número e incluso si han escrito un dígito, cuando se envía la entrada del usuario y lo lee en el código, los datos están en formato de cadena.</span><span class="sxs-lookup"><span data-stu-id="94cfe-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="94cfe-312">Por lo tanto, debe convertir la cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="94cfe-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="94cfe-313">En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, se producirá el siguiente error, porque ASP.NET no puede agregar dos cadenas:</span><span class="sxs-lookup"><span data-stu-id="94cfe-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="94cfe-314">*No se puede convertir implícitamente el tipo ' String ' a ' int '.*</span><span class="sxs-lookup"><span data-stu-id="94cfe-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="94cfe-315">Para convertir los valores en enteros, llame al método `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="94cfe-316">Si la conversión se realiza correctamente, puede Agregar los números.</span><span class="sxs-lookup"><span data-stu-id="94cfe-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="94cfe-317">En la tabla siguiente se enumeran algunos métodos de conversión y prueba comunes para las variables.</span><span class="sxs-lookup"><span data-stu-id="94cfe-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="94cfe-318"><strong>Método</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-318"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-319"><strong>Descripción</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-319"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-320"><strong>Ejemplo</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-320"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-321">Convierte una cadena que representa un número entero (como "593") en un entero.</span><span class="sxs-lookup"><span data-stu-id="94cfe-321">Converts a string that represents a whole number (like "593") to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-322">Convierte una cadena como &quot;true&quot; o &quot;falso&quot; a un tipo booleano.</span><span class="sxs-lookup"><span data-stu-id="94cfe-322">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-323">Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; a un número de punto flotante.</span><span class="sxs-lookup"><span data-stu-id="94cfe-323">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-324">Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; un número decimal.</span><span class="sxs-lookup"><span data-stu-id="94cfe-324">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="94cfe-325">(En ASP.NET, un número decimal es más preciso que un número de punto flotante).</span><span class="sxs-lookup"><span data-stu-id="94cfe-325">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-326">Convierte una cadena que representa un valor de fecha y hora en el tipo de `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="94cfe-326">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-327">Convierte cualquier otro tipo de datos en una cadena.</span><span class="sxs-lookup"><span data-stu-id="94cfe-327">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="94cfe-328">Operadores</span><span class="sxs-lookup"><span data-stu-id="94cfe-328">Operators</span></span>

<span data-ttu-id="94cfe-329">Un operador es una palabra clave o un carácter que indica a ASP.NET qué tipo de comando se debe realizar en una expresión.</span><span class="sxs-lookup"><span data-stu-id="94cfe-329">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="94cfe-330">El C# lenguaje (y el sintaxis Razor que se basa en él) admite muchos operadores, pero solo tiene que reconocer algunos para comenzar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-330">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="94cfe-331">En la tabla siguiente se resumen los operadores más comunes.</span><span class="sxs-lookup"><span data-stu-id="94cfe-331">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
    <span data-ttu-id="94cfe-332"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-332"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-333"><strong>Descripción</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-333"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-334"><strong>Ejemplos</strong></span><span class="sxs-lookup"><span data-stu-id="94cfe-334"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="94cfe-335">`+` `-` `*` `/`</span><span class="sxs-lookup"><span data-stu-id="94cfe-335">`+` `-` `*` `/`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-336">Operadores matemáticos usados en expresiones numéricas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-336">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-337">Asignación.</span><span class="sxs-lookup"><span data-stu-id="94cfe-337">Assignment.</span></span> <span data-ttu-id="94cfe-338">Asigna el valor del lado derecho de una instrucción al objeto del lado izquierdo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-338">Assigns the value on the right side of a statement to the object on the left side.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-339">Igualdad.</span><span class="sxs-lookup"><span data-stu-id="94cfe-339">Equality.</span></span> <span data-ttu-id="94cfe-340">Devuelve `true` si los valores son iguales.</span><span class="sxs-lookup"><span data-stu-id="94cfe-340">Returns `true` if the values are equal.</span></span> <span data-ttu-id="94cfe-341">(Observe la diferencia entre el operador `=` y el operador `==`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-341">(Notice the distinction between the `=` operator and the `==` operator.)</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-342">Desigualdad.</span><span class="sxs-lookup"><span data-stu-id="94cfe-342">Inequality.</span></span> <span data-ttu-id="94cfe-343">Devuelve `true` si los valores no son iguales.</span><span class="sxs-lookup"><span data-stu-id="94cfe-343">Returns `true` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-344">Menor que, mayor que, menor que o igual a, y mayor que o igual a.</span><span class="sxs-lookup"><span data-stu-id="94cfe-344">Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-345">Concatenación, que se usa para combinar cadenas.</span><span class="sxs-lookup"><span data-stu-id="94cfe-345">Concatenation, which is used to join strings.</span></span> <span data-ttu-id="94cfe-346">ASP.NET conoce la diferencia entre este operador y el operador de suma según el tipo de datos de la expresión.</span><span class="sxs-lookup"><span data-stu-id="94cfe-346">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="94cfe-347">`+=` `-=`</span><span class="sxs-lookup"><span data-stu-id="94cfe-347">`+=` `-=`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-348">Los operadores de incremento y decremento, que suman y restan 1 (respectivamente) de una variable.</span><span class="sxs-lookup"><span data-stu-id="94cfe-348">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-349">Semitono.</span><span class="sxs-lookup"><span data-stu-id="94cfe-349">Dot.</span></span> <span data-ttu-id="94cfe-350">Se usa para distinguir objetos y sus propiedades y métodos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-350">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-351">Paréntesis.</span><span class="sxs-lookup"><span data-stu-id="94cfe-351">Parentheses.</span></span> <span data-ttu-id="94cfe-352">Se utiliza para agrupar expresiones y pasar parámetros a métodos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-352">Used to group expressions and to pass parameters to methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-353">Corchetes.</span><span class="sxs-lookup"><span data-stu-id="94cfe-353">Brackets.</span></span> <span data-ttu-id="94cfe-354">Se utiliza para obtener acceso a los valores de matrices o colecciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-354">Used for accessing values in arrays or collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-355">Tampoco.</span><span class="sxs-lookup"><span data-stu-id="94cfe-355">Not.</span></span> <span data-ttu-id="94cfe-356">Invierte un valor `true` en `false` y viceversa.</span><span class="sxs-lookup"><span data-stu-id="94cfe-356">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="94cfe-357">Normalmente se utiliza como una forma abreviada de probar `false` (es decir, para no `true`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-357">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        <span data-ttu-id="94cfe-358">`&&` `||`</span><span class="sxs-lookup"><span data-stu-id="94cfe-358">`&&` `||`</span></span>
    :::column-end:::
    :::column:::
    <span data-ttu-id="94cfe-359">AND y OR lógicos, que se usan para vincular condiciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-359">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="94cfe-360">Trabajar con rutas de acceso de archivos y carpetas en el código</span><span class="sxs-lookup"><span data-stu-id="94cfe-360">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="94cfe-361">A menudo, trabajará con rutas de acceso de archivos y carpetas en el código.</span><span class="sxs-lookup"><span data-stu-id="94cfe-361">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="94cfe-362">A continuación se muestra un ejemplo de una estructura de carpetas físicas para un sitio Web tal y como podría aparecer en el equipo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="94cfe-362">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="94cfe-363">Estos son algunos detalles esenciales sobre las direcciones URL y las rutas de acceso:</span><span class="sxs-lookup"><span data-stu-id="94cfe-363">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="94cfe-364">Una dirección URL comienza con un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-364">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="94cfe-365">Una dirección URL corresponde a una ruta de acceso física en un equipo host.</span><span class="sxs-lookup"><span data-stu-id="94cfe-365">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="94cfe-366">Por ejemplo, `http://myserver` podría corresponder a la carpeta *C:\websites\mywebsite* del servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-366">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="94cfe-367">Una ruta de acceso virtual es una forma abreviada de representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa.</span><span class="sxs-lookup"><span data-stu-id="94cfe-367">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="94cfe-368">Incluye la parte de una dirección URL que sigue al nombre de dominio o de servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-368">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="94cfe-369">Al usar las rutas de acceso virtuales, puede trasladar el código a otro dominio o servidor sin tener que actualizar las rutas de acceso.</span><span class="sxs-lookup"><span data-stu-id="94cfe-369">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="94cfe-370">Este es un ejemplo para ayudarle a comprender las diferencias:</span><span class="sxs-lookup"><span data-stu-id="94cfe-370">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="94cfe-371">Dirección URL completa</span><span class="sxs-lookup"><span data-stu-id="94cfe-371">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="94cfe-372">Nombre de servidor</span><span class="sxs-lookup"><span data-stu-id="94cfe-372">Server name</span></span> | <span data-ttu-id="94cfe-373">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="94cfe-373">*mycompanyserver*</span></span> |
| <span data-ttu-id="94cfe-374">Ruta de acceso virtual</span><span class="sxs-lookup"><span data-stu-id="94cfe-374">Virtual path</span></span> | <span data-ttu-id="94cfe-375">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="94cfe-375">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="94cfe-376">Ruta de acceso física</span><span class="sxs-lookup"><span data-stu-id="94cfe-376">Physical path</span></span> | <span data-ttu-id="94cfe-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="94cfe-377">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="94cfe-378">La raíz virtual es/, al igual que la raíz de la unidad C: \.</span><span class="sxs-lookup"><span data-stu-id="94cfe-378">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="94cfe-379">(Las rutas de acceso de carpeta virtual siempre usan barras diagonales). La ruta de acceso virtual de una carpeta no tiene que tener el mismo nombre que la carpeta física; puede ser un alias.</span><span class="sxs-lookup"><span data-stu-id="94cfe-379">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="94cfe-380">(En servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta).</span><span class="sxs-lookup"><span data-stu-id="94cfe-380">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="94cfe-381">Cuando se trabaja con archivos y carpetas en el código, a veces es necesario hacer referencia a la ruta de acceso física y, en ocasiones, a una ruta de acceso virtual, en función de los objetos con los que esté trabajando.</span><span class="sxs-lookup"><span data-stu-id="94cfe-381">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="94cfe-382">ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el método `Server.MapPath` y el operador `~` y el método `Href`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-382">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="94cfe-383">Conversión de rutas de acceso virtuales a físicas: el método Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="94cfe-383">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="94cfe-384">El método `Server.MapPath` convierte una ruta de acceso virtual (como */default.cshtml*) en una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="94cfe-384">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="94cfe-385">Utilice este método siempre que necesite una ruta de acceso física completa.</span><span class="sxs-lookup"><span data-stu-id="94cfe-385">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="94cfe-386">Un ejemplo típico es al leer o escribir un archivo de texto o de imagen en el servidor Web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-386">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="94cfe-387">Normalmente, no se conoce la ruta de acceso física absoluta del sitio en el servidor de un sitio de hospedaje, por lo que este método puede convertir la ruta de acceso que se conoce (la ruta de acceso virtual) en la ruta de acceso correspondiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-387">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="94cfe-388">Pasa la ruta de acceso virtual a un archivo o una carpeta al método y devuelve la ruta de acceso física:</span><span class="sxs-lookup"><span data-stu-id="94cfe-388">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="94cfe-389">Referencia a la raíz virtual: el operador ~ y el método href</span><span class="sxs-lookup"><span data-stu-id="94cfe-389">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="94cfe-390">En un archivo *. cshtml* o *. vbhtml* , puede hacer referencia a la ruta de acceso de la raíz virtual mediante el operador `~`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-390">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="94cfe-391">Esto es muy útil porque puede mover páginas en un sitio y los vínculos que contienen a otras páginas no se romperán.</span><span class="sxs-lookup"><span data-stu-id="94cfe-391">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="94cfe-392">También es útil en caso de que se mueva el sitio web a una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-392">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="94cfe-393">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="94cfe-393">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="94cfe-394">Si el sitio web está `http://myserver/myapp`, aquí se muestra cómo ASP.NET tratará estas rutas de acceso cuando se ejecute la página:</span><span class="sxs-lookup"><span data-stu-id="94cfe-394">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="94cfe-395">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="94cfe-395">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="94cfe-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="94cfe-396">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="94cfe-397">(En realidad, no verá estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso fuera).</span><span class="sxs-lookup"><span data-stu-id="94cfe-397">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="94cfe-398">Puede usar el operador `~` en el código del servidor (como arriba) y en el marcado, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="94cfe-398">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="94cfe-399">En el marcado, se usa el operador `~` para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="94cfe-399">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="94cfe-400">Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todas las referencias de `~` a la ruta de acceso adecuada.</span><span class="sxs-lookup"><span data-stu-id="94cfe-400">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="94cfe-401">Lógica y bucles condicionales</span><span class="sxs-lookup"><span data-stu-id="94cfe-401">Conditional Logic and Loops</span></span>

<span data-ttu-id="94cfe-402">El código de servidor ASP.NET permite realizar tareas basadas en condiciones y escribir código que repite instrucciones un número específico de veces (es decir, código que ejecuta un bucle).</span><span class="sxs-lookup"><span data-stu-id="94cfe-402">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="94cfe-403">Condiciones de prueba</span><span class="sxs-lookup"><span data-stu-id="94cfe-403">Testing Conditions</span></span>

<span data-ttu-id="94cfe-404">Para probar una condición simple, use la instrucción `if`, que devuelve true o false en función de una prueba que especifique:</span><span class="sxs-lookup"><span data-stu-id="94cfe-404">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="94cfe-405">La palabra clave `if` inicia un bloque.</span><span class="sxs-lookup"><span data-stu-id="94cfe-405">The `if` keyword starts a block.</span></span> <span data-ttu-id="94cfe-406">La prueba real (condición) está entre paréntesis y devuelve true o false.</span><span class="sxs-lookup"><span data-stu-id="94cfe-406">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="94cfe-407">Las instrucciones que se ejecutan si la prueba es true se incluyen entre llaves.</span><span class="sxs-lookup"><span data-stu-id="94cfe-407">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="94cfe-408">Una instrucción `if` puede incluir un bloque de `else` que especifica las instrucciones que se ejecutarán si la condición es falsa:</span><span class="sxs-lookup"><span data-stu-id="94cfe-408">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="94cfe-409">Puede agregar varias condiciones mediante un bloque `else if`:</span><span class="sxs-lookup"><span data-stu-id="94cfe-409">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="94cfe-410">En este ejemplo, si la primera condición del bloque If no es true, se comprueba la condición `else if`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-410">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="94cfe-411">Si se cumple esta condición, se ejecutan las instrucciones del bloque `else if`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-411">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="94cfe-412">Si no se cumple ninguna de las condiciones, se ejecutan las instrucciones del bloque `else`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-412">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="94cfe-413">Puede agregar cualquier número de bloques else if y, a continuación, cerrar con un bloque `else` como el &quot;todo lo demás&quot; condición.</span><span class="sxs-lookup"><span data-stu-id="94cfe-413">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="94cfe-414">Para probar un gran número de condiciones, use un bloque `switch`:</span><span class="sxs-lookup"><span data-stu-id="94cfe-414">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="94cfe-415">El valor que se va a probar se encuentra entre paréntesis (en el ejemplo, la variable `weekday`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-415">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="94cfe-416">Cada prueba individual utiliza una instrucción `case` que termina con dos puntos (:).</span><span class="sxs-lookup"><span data-stu-id="94cfe-416">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="94cfe-417">Si el valor de una instrucción `case` coincide con el valor de prueba, se ejecuta el código de ese bloque de casos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-417">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="94cfe-418">Cierre cada instrucción case con una instrucción `break`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-418">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="94cfe-419">(Si olvida incluir break en cada bloque `case`, el código de la siguiente instrucción `case` se ejecutará también). Un bloque `switch` suele tener una instrucción `default` como el último caso para un &quot;todo lo demás&quot; opción que se ejecuta si no se cumple ninguno de los otros casos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-419">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="94cfe-420">El resultado de los dos últimos bloques condicionales mostrados en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-420">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="94cfe-422">Código de bucle</span><span class="sxs-lookup"><span data-stu-id="94cfe-422">Looping Code</span></span>

<span data-ttu-id="94cfe-423">A menudo es necesario ejecutar las mismas instrucciones varias veces.</span><span class="sxs-lookup"><span data-stu-id="94cfe-423">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="94cfe-424">Para ello, puede crear un bucle.</span><span class="sxs-lookup"><span data-stu-id="94cfe-424">You do this by looping.</span></span> <span data-ttu-id="94cfe-425">Por ejemplo, a menudo se ejecutan las mismas instrucciones para cada elemento en una colección de datos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-425">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="94cfe-426">Si sabe exactamente cuántas veces desea crear un bucle, puede usar un bucle `for`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-426">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="94cfe-427">Este tipo de bucle es especialmente útil para la acumulación o el recuento:</span><span class="sxs-lookup"><span data-stu-id="94cfe-427">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="94cfe-428">El bucle comienza con la palabra clave `for`, seguido de tres instrucciones entre paréntesis, cada una termina con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="94cfe-428">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="94cfe-429">Dentro de los paréntesis, la primera instrucción (`var i=10;`) crea un contador y lo inicializa en 10.</span><span class="sxs-lookup"><span data-stu-id="94cfe-429">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="94cfe-430">No tiene que asignar un nombre al contador &#8212; `i` puede usar cualquier variable.</span><span class="sxs-lookup"><span data-stu-id="94cfe-430">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="94cfe-431">Cuando se ejecuta el bucle `for`, el contador se incrementa automáticamente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-431">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="94cfe-432">La segunda instrucción (`i < 21;`) establece la condición de cuánto desea contar.</span><span class="sxs-lookup"><span data-stu-id="94cfe-432">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="94cfe-433">En este caso, desea que pase a un máximo de 20 (es decir, continúe mientras el contador sea inferior a 21).</span><span class="sxs-lookup"><span data-stu-id="94cfe-433">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="94cfe-434">La tercera instrucción (`i++`) utiliza un operador de incremento, que simplemente especifica que el contador debería tener 1 agregado cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="94cfe-434">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="94cfe-435">Dentro de las llaves está el código que se ejecutará para cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="94cfe-435">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="94cfe-436">El marcado crea un nuevo párrafo (elemento`<p>`) cada vez y agrega una línea a la salida, mostrando el valor de `i` (el contador).</span><span class="sxs-lookup"><span data-stu-id="94cfe-436">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="94cfe-437">Al ejecutar esta página, en el ejemplo se crean 11 líneas que muestran la salida, con el texto en cada línea que indica el número de elemento.</span><span class="sxs-lookup"><span data-stu-id="94cfe-437">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor: Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="94cfe-439">Si está trabajando con una colección o una matriz, a menudo utiliza un bucle `foreach`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-439">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="94cfe-440">Una colección es un grupo de objetos similares y el bucle `foreach` le permite llevar a cabo una tarea en cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="94cfe-440">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="94cfe-441">Este tipo de bucle es práctico para las colecciones, porque a diferencia de un bucle `for`, no tiene que incrementar el contador o establecer un límite.</span><span class="sxs-lookup"><span data-stu-id="94cfe-441">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="94cfe-442">En su lugar, el código de bucle `foreach` simplemente continúa a través de la colección hasta que termina.</span><span class="sxs-lookup"><span data-stu-id="94cfe-442">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="94cfe-443">Por ejemplo, el código siguiente devuelve los elementos de la colección `Request.ServerVariables`, que es un objeto que contiene información sobre el servidor Web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-443">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="94cfe-444">Usa un bucle `foreac` h para mostrar el nombre de cada elemento mediante la creación de un nuevo elemento `<li>` en una lista con viñetas HTML.</span><span class="sxs-lookup"><span data-stu-id="94cfe-444">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="94cfe-445">La palabra clave `foreach` va seguida de paréntesis en el que se declara una variable que representa un único elemento de la colección (en el ejemplo, `var item`), seguido de la palabra clave `in`, seguido de la colección a la que se desea crear un bucle.</span><span class="sxs-lookup"><span data-stu-id="94cfe-445">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="94cfe-446">En el cuerpo del bucle `foreach`, puede tener acceso al elemento actual mediante la variable que declaró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-446">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor: Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="94cfe-448">Para crear un bucle de uso más general, use la instrucción `while`:</span><span class="sxs-lookup"><span data-stu-id="94cfe-448">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="94cfe-449">Un bucle `while` comienza con la palabra clave `while`, seguido de paréntesis en el que se especifica cuánto tiempo continúa el bucle (aquí, siempre y cuando `countNum` sea inferior a 50), después el bloque que se va a repetir.</span><span class="sxs-lookup"><span data-stu-id="94cfe-449">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="94cfe-450">Los bucles suelen incrementar (agregar a) o disminuir (restar de) una variable o un objeto que se usa para el recuento.</span><span class="sxs-lookup"><span data-stu-id="94cfe-450">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="94cfe-451">En el ejemplo, el operador `+=` agrega 1 a `countNum` cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="94cfe-451">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="94cfe-452">(Para reducir una variable en un bucle que se recuenta, se usaría el operador de decremento `-=`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-452">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="94cfe-453">Objetos y colecciones</span><span class="sxs-lookup"><span data-stu-id="94cfe-453">Objects and Collections</span></span>

<span data-ttu-id="94cfe-454">Casi todo en un sitio web de ASP.NET es un objeto, incluida la propia página web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-454">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="94cfe-455">En esta sección se describen algunos objetos importantes con los que trabajará con frecuencia en el código.</span><span class="sxs-lookup"><span data-stu-id="94cfe-455">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="94cfe-456">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="94cfe-456">Page Objects</span></span>

<span data-ttu-id="94cfe-457">El objeto más básico de ASP.NET es la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-457">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="94cfe-458">Puede tener acceso directamente a las propiedades del objeto de página sin ningún objeto calificador.</span><span class="sxs-lookup"><span data-stu-id="94cfe-458">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="94cfe-459">En el código siguiente se obtiene la ruta de acceso del archivo de la página mediante el `Request` objeto de la página:</span><span class="sxs-lookup"><span data-stu-id="94cfe-459">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="94cfe-460">Para aclarar que está haciendo referencia a las propiedades y los métodos del objeto de página actual, puede usar opcionalmente la palabra clave `this` para representar el objeto de página en el código.</span><span class="sxs-lookup"><span data-stu-id="94cfe-460">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="94cfe-461">Este es el ejemplo de código anterior, con `this` agregados para representar la página:</span><span class="sxs-lookup"><span data-stu-id="94cfe-461">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="94cfe-462">Puede utilizar las propiedades del objeto `Page` para obtener una gran cantidad de información, como:</span><span class="sxs-lookup"><span data-stu-id="94cfe-462">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="94cfe-463">`Request`Operador</span><span class="sxs-lookup"><span data-stu-id="94cfe-463">`Request`.</span></span> <span data-ttu-id="94cfe-464">Como ya ha visto, se trata de una colección de información sobre la solicitud actual, incluido el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc.</span><span class="sxs-lookup"><span data-stu-id="94cfe-464">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="94cfe-465">`Response`Operador</span><span class="sxs-lookup"><span data-stu-id="94cfe-465">`Response`.</span></span> <span data-ttu-id="94cfe-466">Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor haya terminado de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="94cfe-466">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="94cfe-467">Por ejemplo, puede usar esta propiedad para escribir información en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="94cfe-467">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="94cfe-468">Objetos de colección (matrices y diccionarios)</span><span class="sxs-lookup"><span data-stu-id="94cfe-468">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="94cfe-469">Una *colección* es un grupo de objetos del mismo tipo, como una colección de objetos `Customer` de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-469">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="94cfe-470">ASP.NET contiene muchas colecciones integradas, como la `Request.Files` colección.</span><span class="sxs-lookup"><span data-stu-id="94cfe-470">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="94cfe-471">A menudo, trabajará con los datos de las colecciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-471">You'll often work with data in collections.</span></span> <span data-ttu-id="94cfe-472">Dos tipos de colección comunes son la *matriz* y el *Diccionario*.</span><span class="sxs-lookup"><span data-stu-id="94cfe-472">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="94cfe-473">Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente que contenga cada elemento:</span><span class="sxs-lookup"><span data-stu-id="94cfe-473">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="94cfe-474">Con las matrices, se declara un tipo de datos específico, como `string`, `int`o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-474">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="94cfe-475">Para indicar que la variable puede contener una matriz, agregue corchetes a la declaración (por ejemplo, `string[]` o `int[]`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-475">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="94cfe-476">Puede tener acceso a los elementos de una matriz mediante su posición (índice) o mediante la instrucción `foreach`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-476">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="94cfe-477">Los índices de matriz son de base &#8212; cero, es decir, el primer elemento está en la posición 0, el segundo elemento está en la posición 1, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-477">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="94cfe-478">Puede determinar el número de elementos de una matriz obteniendo su `Length` propiedad.</span><span class="sxs-lookup"><span data-stu-id="94cfe-478">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="94cfe-479">Para obtener la posición de un elemento específico en la matriz (para buscar en la matriz), use el método `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-479">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="94cfe-480">También puede hacer cosas como invertir el contenido de una matriz (el método `Array.Reverse`) u ordenar el contenido (el método `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-480">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="94cfe-481">La salida del código de la matriz de cadenas que se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="94cfe-481">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="94cfe-483">Un diccionario es una colección de pares clave-valor, donde se proporciona la clave (o nombre) para establecer o recuperar el valor correspondiente:</span><span class="sxs-lookup"><span data-stu-id="94cfe-483">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="94cfe-484">Para crear un diccionario, use la palabra clave `new` para indicar que está creando un nuevo objeto de diccionario.</span><span class="sxs-lookup"><span data-stu-id="94cfe-484">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="94cfe-485">Puede asignar un diccionario a una variable mediante la palabra clave `var`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-485">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="94cfe-486">Los tipos de datos de los elementos del diccionario se indican mediante corchetes angulares (`< >`).</span><span class="sxs-lookup"><span data-stu-id="94cfe-486">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="94cfe-487">Al final de la declaración, debe agregar un par de paréntesis, porque en realidad es un método que crea un nuevo diccionario.</span><span class="sxs-lookup"><span data-stu-id="94cfe-487">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="94cfe-488">Para agregar elementos al diccionario, puede llamar al método `Add` de la variable Dictionary (`myScores` en este caso) y, a continuación, especificar una clave y un valor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-488">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="94cfe-489">Como alternativa, puede usar corchetes para indicar la clave y realizar una asignación simple, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="94cfe-489">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="94cfe-490">Para obtener un valor del diccionario, especifique la clave entre corchetes:</span><span class="sxs-lookup"><span data-stu-id="94cfe-490">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="94cfe-491">Llamar a métodos con parámetros</span><span class="sxs-lookup"><span data-stu-id="94cfe-491">Calling Methods with Parameters</span></span>

<span data-ttu-id="94cfe-492">Tal como se ha leído anteriormente en este artículo, los objetos con los que programa pueden tener métodos.</span><span class="sxs-lookup"><span data-stu-id="94cfe-492">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="94cfe-493">Por ejemplo, un objeto de `Database` podría tener un método `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-493">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="94cfe-494">Muchos métodos también tienen uno o más parámetros.</span><span class="sxs-lookup"><span data-stu-id="94cfe-494">Many methods also have one or more parameters.</span></span> <span data-ttu-id="94cfe-495">Un *parámetro* es un valor que se pasa a un método para que el método pueda completar su tarea.</span><span class="sxs-lookup"><span data-stu-id="94cfe-495">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="94cfe-496">Por ejemplo, examine una declaración para el método `Request.MapPath`, que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="94cfe-496">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="94cfe-497">(La línea se ha ajustado para que sea más legible.</span><span class="sxs-lookup"><span data-stu-id="94cfe-497">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="94cfe-498">Recuerde que puede colocar los saltos de línea prácticamente en cualquier lugar excepto en las cadenas entre comillas).</span><span class="sxs-lookup"><span data-stu-id="94cfe-498">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="94cfe-499">Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="94cfe-499">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="94cfe-500">Los tres parámetros del método son `virtualPath`, `baseVirtualDir`y `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="94cfe-500">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="94cfe-501">(Observe que en la declaración, los parámetros se enumeran con los tipos de datos de los datos que aceptarán). Cuando llame a este método, debe proporcionar valores para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="94cfe-501">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="94cfe-502">El sintaxis Razor proporciona dos opciones para pasar parámetros a un método: *parámetros posicionales* y *parámetros con nombre*.</span><span class="sxs-lookup"><span data-stu-id="94cfe-502">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="94cfe-503">Para llamar a un método mediante parámetros posicionales, se pasan los parámetros en un orden estricto que se especifica en la declaración del método.</span><span class="sxs-lookup"><span data-stu-id="94cfe-503">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="94cfe-504">(Normalmente se conoce este orden leyendo la documentación del método). Debe seguir el orden y no puede omitir ninguno de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o `null` para un parámetro posicional para el que no tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-504">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="94cfe-505">En el ejemplo siguiente se da por supuesto que tiene una carpeta denominada *scripts* en el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="94cfe-505">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="94cfe-506">El código llama al método `Request.MapPath` y pasa los valores de los tres parámetros en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="94cfe-506">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="94cfe-507">A continuación, se muestra la ruta de acceso asignada resultante.</span><span class="sxs-lookup"><span data-stu-id="94cfe-507">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="94cfe-508">Cuando un método tiene muchos parámetros, puede mantener el código más legible mediante el uso de parámetros con nombre.</span><span class="sxs-lookup"><span data-stu-id="94cfe-508">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="94cfe-509">Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de un signo de dos puntos (:) y, a continuación, el valor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-509">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="94cfe-510">La ventaja de los parámetros con nombre es que puede pasarlos en cualquier orden que desee.</span><span class="sxs-lookup"><span data-stu-id="94cfe-510">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="94cfe-511">(Una desventaja es que la llamada al método no es tan compacta).</span><span class="sxs-lookup"><span data-stu-id="94cfe-511">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="94cfe-512">En el ejemplo siguiente se llama al mismo método que el anterior, pero se usan parámetros con nombre para proporcionar los valores:</span><span class="sxs-lookup"><span data-stu-id="94cfe-512">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="94cfe-513">Como puede ver, los parámetros se pasan en un orden diferente.</span><span class="sxs-lookup"><span data-stu-id="94cfe-513">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="94cfe-514">Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverán el mismo valor.</span><span class="sxs-lookup"><span data-stu-id="94cfe-514">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="94cfe-515">Control de errores</span><span class="sxs-lookup"><span data-stu-id="94cfe-515">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="94cfe-516">Instrucciones try-catch</span><span class="sxs-lookup"><span data-stu-id="94cfe-516">Try-Catch Statements</span></span>

<span data-ttu-id="94cfe-517">A menudo tendrá instrucciones en el código que podrían producir errores por motivos ajenos al control.</span><span class="sxs-lookup"><span data-stu-id="94cfe-517">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="94cfe-518">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="94cfe-518">For example:</span></span>

- <span data-ttu-id="94cfe-519">Si el código intenta crear o tener acceso a un archivo, puede producirse todo tipo de errores.</span><span class="sxs-lookup"><span data-stu-id="94cfe-519">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="94cfe-520">Es posible que el archivo que desea no exista, que esté bloqueado, que el código no tenga permisos, etc.</span><span class="sxs-lookup"><span data-stu-id="94cfe-520">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="94cfe-521">Del mismo modo, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, que la conexión a la base de datos se haya quitado, que los datos que se van a guardar no sean válidos, etc.</span><span class="sxs-lookup"><span data-stu-id="94cfe-521">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="94cfe-522">En términos de programación, estas situaciones se denominan *excepciones*.</span><span class="sxs-lookup"><span data-stu-id="94cfe-522">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="94cfe-523">Si el código encuentra una excepción, genera (lanza) un mensaje de error que, en el mejor de los usuarios, le molesta:</span><span class="sxs-lookup"><span data-stu-id="94cfe-523">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor: Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="94cfe-525">En situaciones en las que el código pueda encontrar excepciones y para evitar los mensajes de error de este tipo, puede usar `try/catch` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="94cfe-525">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="94cfe-526">En la instrucción `try`, ejecute el código que está comprobando.</span><span class="sxs-lookup"><span data-stu-id="94cfe-526">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="94cfe-527">En una o varias instrucciones de `catch`, puede buscar errores específicos (tipos específicos de excepciones) que puedan haberse producido.</span><span class="sxs-lookup"><span data-stu-id="94cfe-527">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="94cfe-528">Puede incluir tantas instrucciones `catch` como necesite para buscar los errores que espera.</span><span class="sxs-lookup"><span data-stu-id="94cfe-528">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="94cfe-529">Se recomienda evitar el uso del método `Response.Redirect` en `try/catch` instrucciones, ya que puede producir una excepción en la página.</span><span class="sxs-lookup"><span data-stu-id="94cfe-529">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="94cfe-530">En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="94cfe-530">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="94cfe-531">En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que se produzca una excepción.</span><span class="sxs-lookup"><span data-stu-id="94cfe-531">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="94cfe-532">El código incluye `catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto y `DirectoryNotFoundException`, que se produce si ASP.NET no puede incluso encontrar la carpeta.</span><span class="sxs-lookup"><span data-stu-id="94cfe-532">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="94cfe-533">(Puede quitar el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente).</span><span class="sxs-lookup"><span data-stu-id="94cfe-533">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="94cfe-534">Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior.</span><span class="sxs-lookup"><span data-stu-id="94cfe-534">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="94cfe-535">Sin embargo, la sección `try/catch` ayuda a evitar que el usuario vea estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="94cfe-535">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="94cfe-536">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="94cfe-536">Additional Resources</span></span>

<span data-ttu-id="94cfe-537">**Programar con Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="94cfe-537">**Programming with Visual Basic**</span></span>

[<span data-ttu-id="94cfe-538">Apéndice: lenguaje de Visual Basic y sintaxis</span><span class="sxs-lookup"><span data-stu-id="94cfe-538">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)

<span data-ttu-id="94cfe-539">**Documentación de referencia**</span><span class="sxs-lookup"><span data-stu-id="94cfe-539">**Reference Documentation**</span></span>

[<span data-ttu-id="94cfe-540">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="94cfe-540">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="94cfe-541">C#Módulo</span><span class="sxs-lookup"><span data-stu-id="94cfe-541">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
