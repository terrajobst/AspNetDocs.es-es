---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introducción a la programación web de ASP.NET mediante la sintaxis de Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: En este apéndice se ofrece información general sobre la programación con páginas Web ASP.NET en Visual Basic, mediante el sintaxis Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422665"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="d4c44-103">Introducción a la programación web de ASP.NET mediante la sintaxis Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d4c44-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="d4c44-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d4c44-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d4c44-105">En este artículo se proporciona información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor y Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4c44-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="d4c44-106">ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores Web.</span><span class="sxs-lookup"><span data-stu-id="d4c44-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="d4c44-107">**Lo que aprenderá**:</span><span class="sxs-lookup"><span data-stu-id="d4c44-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="d4c44-108">Las 8 mejores sugerencias de programación para empezar a programar ASP.NET Web Pages mediante sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="d4c44-109">Conceptos de programación básicos que necesitará.</span><span class="sxs-lookup"><span data-stu-id="d4c44-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="d4c44-110">Qué es el código del servidor de ASP.NET y el sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="d4c44-111">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="d4c44-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="d4c44-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="d4c44-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="d4c44-113">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="d4c44-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="d4c44-114">La mayoría de los ejemplos de uso de C#ASP.NET Web Pages con sintaxis Razor usar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="d4c44-115">Pero el sintaxis Razor también admite Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="d4c44-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="d4c44-116">Para programar una página web de ASP.NET en Visual Basic, cree una página web con una extensión de nombre de archivo *. vbhtml* y, a continuación, agregue Visual Basic código.</span><span class="sxs-lookup"><span data-stu-id="d4c44-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="d4c44-117">En este artículo se proporciona información general sobre cómo trabajar con el lenguaje de Visual Basic y la sintaxis para crear páginas web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4c44-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="d4c44-118">Las plantillas de sitio Web predeterminadas para Microsoft WebMatrix (**panadería**, **Galería fotográfica**y **sitio de inicio**, etc C# .) están disponibles en y Visual Basic versiones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="d4c44-119">Puede instalar las plantillas de Visual Basic como paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="d4c44-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="d4c44-120">Las plantillas de sitio web se instalan en la carpeta raíz de su sitio en una carpeta denominada *plantillas de Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="d4c44-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="d4c44-121">Las 8 mejores sugerencias de programación</span><span class="sxs-lookup"><span data-stu-id="d4c44-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="d4c44-122">En esta sección se enumeran algunas sugerencias que es absolutamente necesario saber a medida que empieza a escribir código de servidor de ASP.NET mediante el sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="d4c44-123">1. Agregue código a una página mediante el carácter @.</span><span class="sxs-lookup"><span data-stu-id="d4c44-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="d4c44-124">El carácter `@` inicia las expresiones insertadas, los bloques de una sola instrucción y los bloques de varias instrucciones:</span><span class="sxs-lookup"><span data-stu-id="d4c44-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="d4c44-125">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-125">The result displayed in a browser:</span></span>

![Razor: img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d4c44-127">**Codificación HTML**</span><span class="sxs-lookup"><span data-stu-id="d4c44-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="d4c44-128">Al mostrar el contenido en una página mediante el carácter `@`, como en los ejemplos anteriores, ASP.NET codifica el resultado en HTML.</span><span class="sxs-lookup"><span data-stu-id="d4c44-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="d4c44-129">Esto reemplaza los caracteres HTML reservados (como `<` y `>` y `&`) con códigos que permiten mostrar los caracteres como caracteres en una página web en lugar de interpretarlos como etiquetas o entidades HTML.</span><span class="sxs-lookup"><span data-stu-id="d4c44-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="d4c44-130">Sin la codificación HTML, la salida del código del servidor podría no mostrarse correctamente y podría exponer una página a los riesgos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="d4c44-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="d4c44-131">Si el objetivo es generar el formato HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), vea la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d4c44-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="d4c44-132">Puede obtener más información sobre la codificación HTML en [trabajar con formularios HTML en ASP.NET Web pages sitios](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="d4c44-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="d4c44-133">2. delimite los bloques de código con código... Código final</span><span class="sxs-lookup"><span data-stu-id="d4c44-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="d4c44-134">Un bloque de código incluye una o varias instrucciones de código y se encuentra entre las palabras clave `Code` y `End Code`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="d4c44-135">Coloque la palabra clave de `Code` de apertura inmediatamente después &#8212; del carácter de `@` no puede haber espacio en blanco entre ellas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="d4c44-136">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-136">The result displayed in a browser:</span></span>

![Razor: Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="d4c44-138">3. dentro de un bloque, finaliza cada instrucción de código con un salto de línea.</span><span class="sxs-lookup"><span data-stu-id="d4c44-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="d4c44-139">En un bloque de código Visual Basic, cada instrucción finaliza con un salto de línea.</span><span class="sxs-lookup"><span data-stu-id="d4c44-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="d4c44-140">(Más adelante en el artículo verá una forma de ajustar una instrucción de código larga en varias líneas si es necesario).</span><span class="sxs-lookup"><span data-stu-id="d4c44-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="d4c44-141">4. Utilice variables para almacenar valores</span><span class="sxs-lookup"><span data-stu-id="d4c44-141">4. You use variables to store values</span></span>

<span data-ttu-id="d4c44-142">Puede almacenar valores en una *variable*, como cadenas, números y fechas, etc. Cree una nueva variable mediante la palabra clave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="d4c44-143">Los valores de las variables se pueden insertar directamente en una página mediante `@`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="d4c44-144">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-144">The result displayed in a browser:</span></span>

![Razor: Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="d4c44-146">5. incluir valores de cadena literales entre comillas dobles</span><span class="sxs-lookup"><span data-stu-id="d4c44-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="d4c44-147">Una *cadena* es una secuencia de caracteres que se trata como texto.</span><span class="sxs-lookup"><span data-stu-id="d4c44-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="d4c44-148">Para especificar una cadena, debe encerrarla entre comillas dobles:</span><span class="sxs-lookup"><span data-stu-id="d4c44-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="d4c44-149">Para insertar comillas dobles en un valor de cadena, inserte dos caracteres de comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="d4c44-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="d4c44-150">Si desea que el carácter de comillas dobles aparezca una vez en el resultado de la página, escríbalo como `""` dentro de la cadena entrecomillada y, si desea que aparezca dos veces, escríbalo como `""""` dentro de la cadena entrecomillada.</span><span class="sxs-lookup"><span data-stu-id="d4c44-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="d4c44-151">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-151">The result displayed in a browser:</span></span>

![Razor: Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="d4c44-153">6. Visual Basic código no distingue entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="d4c44-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="d4c44-154">El lenguaje Visual Basic no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="d4c44-155">En cualquier caso, se pueden escribir palabras clave de programación (como `Dim`, `If`y `True`) y nombres de variables (como `myString`o `subTotal`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="d4c44-156">Las siguientes líneas de código asignan un valor a la variable `lastname` con un nombre en minúsculas y, a continuación, envían el valor de la variable a la página con un nombre en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="d4c44-157">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="d4c44-159">7. gran parte de la codificación implica trabajar con objetos</span><span class="sxs-lookup"><span data-stu-id="d4c44-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="d4c44-160">Un objeto representa un elemento que puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud Web, un mensaje de correo electrónico, un registro de cliente (fila de base de datos), etc. Los objetos tienen propiedades que describen sus &#8212; características. un objeto de cuadro de texto tiene una propiedad `Text`, un objeto de solicitud tiene una propiedad `Url`, un mensaje de correo electrónico tiene una propiedad `From` y un objeto Customer tiene una propiedad `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="d4c44-161">Los objetos también tienen métodos que son los &quot;verbos&quot; pueden realizar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="d4c44-162">Entre los ejemplos se incluye el método de `Save` de un objeto de archivo, el método de `Rotate` de un objeto de imagen y el método `Send` de un objeto de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="d4c44-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="d4c44-163">A menudo, trabajará con el objeto `Request`, que le proporciona información como los valores de los campos de formulario en la página (cuadros de texto, etc.), el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc. En este ejemplo se muestra cómo obtener acceso a las propiedades del objeto `Request` y cómo llamar al método `MapPath` del objeto `Request`, que proporciona la ruta de acceso absoluta de la página en el servidor:</span><span class="sxs-lookup"><span data-stu-id="d4c44-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="d4c44-164">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-164">The result displayed in a browser:</span></span>

![Razor: Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="d4c44-166">8. puede escribir código que tome decisiones</span><span class="sxs-lookup"><span data-stu-id="d4c44-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="d4c44-167">Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de las condiciones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="d4c44-168">La forma más común de hacerlo es con la instrucción `If` (y la instrucción `Else` opcional).</span><span class="sxs-lookup"><span data-stu-id="d4c44-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="d4c44-169">La instrucción `If IsPost` es una forma abreviada de escribir `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="d4c44-170">Junto con las instrucciones `If`, hay varias maneras de probar condiciones, repetir bloques de código, etc., que se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d4c44-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="d4c44-171">El resultado mostrado en un explorador (después de hacer clic en **submit**):</span><span class="sxs-lookup"><span data-stu-id="d4c44-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor: Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="d4c44-173">**Métodos GET y POST de HTTP y la propiedad IsPost**</span><span class="sxs-lookup"><span data-stu-id="d4c44-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="d4c44-174">El protocolo usado para páginas web (HTTP) admite un número muy limitado de métodos (&quot;verbos&quot;) que se usan para hacer solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="d4c44-175">Los dos más comunes son GET, que se usa para leer una página y POST, que se usa para enviar una página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="d4c44-176">En general, la primera vez que un usuario solicita una página, se solicita la página mediante GET.</span><span class="sxs-lookup"><span data-stu-id="d4c44-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="d4c44-177">Si el usuario rellena un formulario y, a continuación, hace clic en **Enviar**, el explorador realiza una solicitud post al servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="d4c44-178">En la programación web, a menudo resulta útil saber si se solicita una página como GET o como una entrada para que sepa cómo procesar la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="d4c44-179">En ASP.NET Web Pages, puede usar la propiedad `IsPost` para ver si una solicitud es GET o POST.</span><span class="sxs-lookup"><span data-stu-id="d4c44-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="d4c44-180">Si la solicitud es una publicación, la propiedad `IsPost` devolverá True y puede hacer cosas como leer los valores de los cuadros de texto de un formulario.</span><span class="sxs-lookup"><span data-stu-id="d4c44-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="d4c44-181">Muchos ejemplos que verá muestran cómo procesar la página de forma diferente en función del valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="d4c44-182">Un ejemplo de código simple</span><span class="sxs-lookup"><span data-stu-id="d4c44-182">A Simple Code Example</span></span>

<span data-ttu-id="d4c44-183">En este procedimiento se muestra cómo crear una página que muestra las técnicas básicas de programación.</span><span class="sxs-lookup"><span data-stu-id="d4c44-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="d4c44-184">En el ejemplo, creará una página que permite a los usuarios escribir dos números y, después, los agregará y mostrará el resultado.</span><span class="sxs-lookup"><span data-stu-id="d4c44-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="d4c44-185">En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="d4c44-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="d4c44-186">Copie el código y el marcado siguientes en la página, reemplazando todo lo que ya está en la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="d4c44-187">A continuación se indican algunas cosas que debe tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="d4c44-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="d4c44-188">El carácter `@` inicia el primer bloque de código de la página y precede a la variable `totalMessage` incrustada cerca de la parte inferior.</span><span class="sxs-lookup"><span data-stu-id="d4c44-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="d4c44-189">El bloque situado en la parte superior de la página se incluye en `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="d4c44-190">Las variables `total`, `num1`, `num2`y `totalMessage` almacenan varios números y una cadena.</span><span class="sxs-lookup"><span data-stu-id="d4c44-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="d4c44-191">El valor de cadena literal asignado a la variable `totalMessage` está entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="d4c44-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="d4c44-192">Dado que Visual Basic código no distingue entre mayúsculas y minúsculas, cuando la variable de `totalMessage` se usa cerca de la parte inferior de la página, su nombre solo debe coincidir con la ortografía de la declaración de variable en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="d4c44-193">No importa el uso de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="d4c44-194">La expresión `num1.AsInt()` + `num2.AsInt()` muestra cómo trabajar con objetos y métodos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="d4c44-195">El método `AsInt` de cada variable convierte la cadena especificada por un usuario en un número entero (un entero) que se puede Agregar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="d4c44-196">La etiqueta `<form>` incluye un atributo `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="d4c44-197">Esto especifica que cuando el usuario haga clic en **Agregar**, la página se enviará al servidor mediante el método http post.</span><span class="sxs-lookup"><span data-stu-id="d4c44-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="d4c44-198">Cuando se envía la página, el código `If IsPost` se evalúa como true y se ejecuta el código condicional, lo que muestra el resultado de sumar los números.</span><span class="sxs-lookup"><span data-stu-id="d4c44-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="d4c44-199">Guarde la página y ejecútela en un explorador.</span><span class="sxs-lookup"><span data-stu-id="d4c44-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="d4c44-200">(Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Escriba dos números enteros y, a continuación, haga clic en el botón **Agregar** .</span><span class="sxs-lookup"><span data-stu-id="d4c44-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor: Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="d4c44-202">Lenguaje Visual Basic y sintaxis</span><span class="sxs-lookup"><span data-stu-id="d4c44-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="d4c44-203">Antes vio un ejemplo básico de cómo crear una página web de ASP.NET y cómo agregar código de servidor al marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="d4c44-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="d4c44-204">Aquí aprenderá los conceptos básicos del uso de Visual Basic para escribir código del servidor de ASP.NET mediante &#8212; el sintaxis Razor es decir, las reglas del lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="d4c44-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="d4c44-205">Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que Lee aquí le resultará familiar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="d4c44-206">Probablemente necesitará familiarizarse solo con cómo se agrega el código WebMatrix al marcado en los archivos *. vbhtml* .</span><span class="sxs-lookup"><span data-stu-id="d4c44-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="d4c44-207">Combinar texto, marcado y código en bloques de código</span><span class="sxs-lookup"><span data-stu-id="d4c44-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="d4c44-208">En los bloques de código de servidor, a menudo querrá Mostrar texto y marcado en la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="d4c44-209">Si un bloque de código de servidor contiene texto que no es código y que, en su lugar, se debe representar como es, ASP.NET debe poder distinguir ese texto del código.</span><span class="sxs-lookup"><span data-stu-id="d4c44-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="d4c44-210">Existen varias formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="d4c44-210">There are several ways to do this.</span></span>

- <span data-ttu-id="d4c44-211">Incluya el texto en un elemento de bloque HTML, como `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="d4c44-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="d4c44-212">El elemento HTML puede incluir texto, elementos HTML adicionales y expresiones de código de servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="d4c44-213">Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), representa todo el elemento y su contenido tal como está en el explorador (y resuelve las expresiones de código de servidor).</span><span class="sxs-lookup"><span data-stu-id="d4c44-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="d4c44-214">Use el operador `@:` o el elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="d4c44-215">El `@:` genera una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el elemento `<text>` incluye varias líneas para la salida.</span><span class="sxs-lookup"><span data-stu-id="d4c44-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="d4c44-216">Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.</span><span class="sxs-lookup"><span data-stu-id="d4c44-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="d4c44-217">En el ejemplo siguiente se repite el ejemplo anterior, pero se usa un solo par de etiquetas `<text>` para incluir el texto que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="d4c44-218">En el ejemplo siguiente, las etiquetas `<text>` y `</text>` incluyen tres líneas, todas ellas tienen un texto no contenida y etiquetas HTML no coincidentes (`<br />`), junto con el código del servidor y las etiquetas HTML coincidentes.</span><span class="sxs-lookup"><span data-stu-id="d4c44-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="d4c44-219">De nuevo, también podría preceder cada línea individualmente con el operador `@:`; ambos métodos funcionan.</span><span class="sxs-lookup"><span data-stu-id="d4c44-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="d4c44-220">Cuando se genera texto como se muestra en esta &#8212; sección mediante un elemento HTML, el operador `@:` o el elemento &#8212; `<text>` ASP.net no codifica en HTML el resultado.</span><span class="sxs-lookup"><span data-stu-id="d4c44-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="d4c44-221">(Como se indicó anteriormente, ASP.NET codifica la salida de las expresiones de código de servidor y los bloques de código de servidor precedidos por `@`, excepto en los casos especiales que se indican en esta sección).</span><span class="sxs-lookup"><span data-stu-id="d4c44-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="d4c44-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="d4c44-222">Whitespace</span></span>

<span data-ttu-id="d4c44-223">Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:</span><span class="sxs-lookup"><span data-stu-id="d4c44-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="d4c44-224">Dividir instrucciones largas en varias líneas</span><span class="sxs-lookup"><span data-stu-id="d4c44-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="d4c44-225">Puede dividir una instrucción de código larga en varias líneas mediante el carácter de subrayado `_` (que en Visual Basic se denomina el *carácter de continuación*) después de cada línea de código.</span><span class="sxs-lookup"><span data-stu-id="d4c44-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="d4c44-226">Para dividir una instrucción en la línea siguiente, al final de la línea agregue un espacio y, a continuación, el carácter de continuación.</span><span class="sxs-lookup"><span data-stu-id="d4c44-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="d4c44-227">Continúe con la instrucción en la línea siguiente.</span><span class="sxs-lookup"><span data-stu-id="d4c44-227">Continue the statement on the next line.</span></span> <span data-ttu-id="d4c44-228">Puede incluir instrucciones en tantas líneas como necesite para mejorar la legibilidad.</span><span class="sxs-lookup"><span data-stu-id="d4c44-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="d4c44-229">Las siguientes instrucciones son iguales:</span><span class="sxs-lookup"><span data-stu-id="d4c44-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="d4c44-230">Sin embargo, no se puede ajustar una línea en medio de un literal de cadena.</span><span class="sxs-lookup"><span data-stu-id="d4c44-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="d4c44-231">El ejemplo siguiente no funciona:</span><span class="sxs-lookup"><span data-stu-id="d4c44-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="d4c44-232">Para combinar una cadena larga que se ajusta a varias líneas como el código anterior, deberá usar el *operador de concatenación* (`&`), que verá más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="d4c44-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="d4c44-233">Comentarios de código</span><span class="sxs-lookup"><span data-stu-id="d4c44-233">Code comments</span></span>

<span data-ttu-id="d4c44-234">Los comentarios le permiten dejar notas por su cuenta o por otras personas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="d4c44-235">Sintaxis Razor comentarios llevan el prefijo `@*` y terminan con `*@`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="d4c44-236">Dentro de los bloques de código puede utilizar los comentarios de sintaxis Razor, o bien usar el carácter de comentario de Visual Basic ordinario, que es una comilla simple (`'`) con prefijo en cada línea.</span><span class="sxs-lookup"><span data-stu-id="d4c44-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="d4c44-237">variables</span><span class="sxs-lookup"><span data-stu-id="d4c44-237">Variables</span></span>

<span data-ttu-id="d4c44-238">Una variable es un objeto con nombre que se utiliza para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="d4c44-239">Puede asignar cualquier valor a las variables, pero el nombre debe comenzar por un carácter alfabético y no puede contener espacios en blanco ni caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="d4c44-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="d4c44-240">En Visual Basic, como vimos anteriormente, no importa el caso de las letras de un nombre de variable.</span><span class="sxs-lookup"><span data-stu-id="d4c44-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="d4c44-241">Variables y tipos de datos</span><span class="sxs-lookup"><span data-stu-id="d4c44-241">Variables and data types</span></span>

<span data-ttu-id="d4c44-242">Una variable puede tener un tipo de datos específico, que indica qué tipo de datos se almacenan en la variable.</span><span class="sxs-lookup"><span data-stu-id="d4c44-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="d4c44-243">Puede tener variables de cadena que almacenen valores de cadena (como &quot;Hello World&quot;), variables de entero que almacenan valores de número entero (como 3 o 79) y variables de fecha que almacenan valores de fecha en una variedad de formatos (como 4/12/2012 o marzo de 2009).</span><span class="sxs-lookup"><span data-stu-id="d4c44-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="d4c44-244">Y hay muchos otros tipos de datos que puede usar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="d4c44-245">Sin embargo, no es necesario especificar un tipo para una variable.</span><span class="sxs-lookup"><span data-stu-id="d4c44-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="d4c44-246">En la mayoría de los casos, ASP.NET puede averiguar el tipo en función de cómo se usan los datos de la variable.</span><span class="sxs-lookup"><span data-stu-id="d4c44-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="d4c44-247">(En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).</span><span class="sxs-lookup"><span data-stu-id="d4c44-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="d4c44-248">Para declarar una variable sin especificar un tipo, utilice `Dim` más el nombre de la variable (por ejemplo, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="d4c44-249">Para declarar una variable con un tipo, utilice `Dim` más el nombre de la variable, seguido de `As` y del nombre de tipo (por ejemplo, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="d4c44-250">En el ejemplo siguiente se muestran algunas expresiones en línea que usan las variables en una página web.</span><span class="sxs-lookup"><span data-stu-id="d4c44-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="d4c44-251">Resultado mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-251">The result displayed in a browser:</span></span>

![Razor: Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="d4c44-253">Conversión y prueba de tipos de datos</span><span class="sxs-lookup"><span data-stu-id="d4c44-253">Converting and testing data types</span></span>

<span data-ttu-id="d4c44-254">Aunque ASP.NET normalmente puede determinar un tipo de datos de forma automática, a veces no es posible.</span><span class="sxs-lookup"><span data-stu-id="d4c44-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="d4c44-255">Por lo tanto, es posible que necesite ayudar a ASP.NET a realizar una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="d4c44-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="d4c44-256">Incluso si no tiene que convertir tipos, a veces resulta útil probar para ver el tipo de datos con el que puede trabajar.</span><span class="sxs-lookup"><span data-stu-id="d4c44-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="d4c44-257">El caso más común es que tenga que convertir una cadena en otro tipo, como un entero o una fecha.</span><span class="sxs-lookup"><span data-stu-id="d4c44-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="d4c44-258">En el ejemplo siguiente se muestra un caso típico en el que debe convertir una cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="d4c44-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="d4c44-259">Como regla, los datos proporcionados por el usuario se incluyen como cadenas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="d4c44-260">Incluso si ha solicitado al usuario que escriba un número e incluso si ha escrito un dígito, cuando se envía la entrada del usuario y lo lee en el código, los datos están en formato de cadena.</span><span class="sxs-lookup"><span data-stu-id="d4c44-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="d4c44-261">Por lo tanto, debe convertir la cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="d4c44-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="d4c44-262">En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, se producirá el siguiente error, porque ASP.NET no puede agregar dos cadenas:</span><span class="sxs-lookup"><span data-stu-id="d4c44-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="d4c44-263">Para convertir los valores en enteros, llame al método `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="d4c44-264">Si la conversión se realiza correctamente, puede Agregar los números.</span><span class="sxs-lookup"><span data-stu-id="d4c44-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="d4c44-265">En la tabla siguiente se enumeran algunos métodos de conversión y prueba comunes para las variables.</span><span class="sxs-lookup"><span data-stu-id="d4c44-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="d4c44-266"><strong>Método</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-267"><strong>Descripción</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-268"><strong>Ejemplo</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-269">Convierte una cadena que representa un número entero (como &quot;593&quot;) en un entero.</span><span class="sxs-lookup"><span data-stu-id="d4c44-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-270">Convierte una cadena como &quot;true&quot; o &quot;falso&quot; a un tipo booleano.</span><span class="sxs-lookup"><span data-stu-id="d4c44-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-271">Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; a un número de punto flotante.</span><span class="sxs-lookup"><span data-stu-id="d4c44-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-272">Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; un número decimal.</span><span class="sxs-lookup"><span data-stu-id="d4c44-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="d4c44-273">(En ASP.NET, un número decimal es más preciso que un número de punto flotante).</span><span class="sxs-lookup"><span data-stu-id="d4c44-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-274">Convierte una cadena que representa un valor de fecha y hora en el tipo de `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4c44-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-275">Convierte cualquier otro tipo de datos en una cadena.</span><span class="sxs-lookup"><span data-stu-id="d4c44-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="d4c44-276">Operadores</span><span class="sxs-lookup"><span data-stu-id="d4c44-276">Operators</span></span>

<span data-ttu-id="d4c44-277">Un operador es una palabra clave o un carácter que indica a ASP.NET qué tipo de comando se debe realizar en una expresión.</span><span class="sxs-lookup"><span data-stu-id="d4c44-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="d4c44-278">Visual Basic admite muchos operadores, pero solo necesita reconocer algunos para empezar a desarrollar páginas web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4c44-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="d4c44-279">En la tabla siguiente se resumen los operadores más comunes.</span><span class="sxs-lookup"><span data-stu-id="d4c44-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="d4c44-280"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-281"><strong>Descripción</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-282"><strong>Ejemplos</strong></span><span class="sxs-lookup"><span data-stu-id="d4c44-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-283">Operadores matemáticos usados en expresiones numéricas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-284">Asignación e igualdad.</span><span class="sxs-lookup"><span data-stu-id="d4c44-284">Assignment and equality.</span></span> <span data-ttu-id="d4c44-285">Dependiendo del contexto, asigna el valor en el lado derecho de una instrucción al objeto en el lado izquierdo o comprueba si los valores son iguales.</span><span class="sxs-lookup"><span data-stu-id="d4c44-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-286">Desigualdad.</span><span class="sxs-lookup"><span data-stu-id="d4c44-286">Inequality.</span></span> <span data-ttu-id="d4c44-287">Devuelve `True` si los valores no son iguales.</span><span class="sxs-lookup"><span data-stu-id="d4c44-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-288">Menor que, mayor que, menor o igual que y mayor o igual que.</span><span class="sxs-lookup"><span data-stu-id="d4c44-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-289">Concatenación, que se usa para combinar cadenas.</span><span class="sxs-lookup"><span data-stu-id="d4c44-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-290">Los operadores de incremento y decremento, que suman y restan 1 (respectivamente) de una variable.</span><span class="sxs-lookup"><span data-stu-id="d4c44-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-291">Semitono.</span><span class="sxs-lookup"><span data-stu-id="d4c44-291">Dot.</span></span> <span data-ttu-id="d4c44-292">Se usa para distinguir objetos y sus propiedades y métodos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-293">Paréntesis.</span><span class="sxs-lookup"><span data-stu-id="d4c44-293">Parentheses.</span></span> <span data-ttu-id="d4c44-294">Se utiliza para agrupar expresiones, para pasar parámetros a métodos y para obtener acceso a miembros de matrices y colecciones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-295">Tampoco.</span><span class="sxs-lookup"><span data-stu-id="d4c44-295">Not.</span></span> <span data-ttu-id="d4c44-296">Invierte un valor true a false y viceversa.</span><span class="sxs-lookup"><span data-stu-id="d4c44-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="d4c44-297">Normalmente se utiliza como una forma abreviada de probar `False` (es decir, para no `True`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="d4c44-298">AND y OR lógicos, que se usan para vincular condiciones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="d4c44-299">Trabajar con rutas de acceso de archivos y carpetas en el código</span><span class="sxs-lookup"><span data-stu-id="d4c44-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="d4c44-300">A menudo, trabajará con rutas de acceso de archivos y carpetas en el código.</span><span class="sxs-lookup"><span data-stu-id="d4c44-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="d4c44-301">A continuación se muestra un ejemplo de una estructura de carpetas físicas para un sitio Web tal y como podría aparecer en el equipo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="d4c44-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="d4c44-302">Estos son algunos detalles esenciales sobre las direcciones URL y las rutas de acceso:</span><span class="sxs-lookup"><span data-stu-id="d4c44-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="d4c44-303">Una dirección URL comienza con un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="d4c44-304">Una dirección URL corresponde a una ruta de acceso física en un equipo host.</span><span class="sxs-lookup"><span data-stu-id="d4c44-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="d4c44-305">Por ejemplo, `http://myserver` podría corresponder a la carpeta *C:\websites\mywebsite* del servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="d4c44-306">Una ruta de acceso virtual es una forma abreviada de representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa.</span><span class="sxs-lookup"><span data-stu-id="d4c44-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="d4c44-307">Incluye la parte de una dirección URL que sigue al nombre de dominio o de servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="d4c44-308">Al usar las rutas de acceso virtuales, puede trasladar el código a otro dominio o servidor sin tener que actualizar las rutas de acceso.</span><span class="sxs-lookup"><span data-stu-id="d4c44-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="d4c44-309">Este es un ejemplo para ayudarle a comprender las diferencias:</span><span class="sxs-lookup"><span data-stu-id="d4c44-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="d4c44-310">Dirección URL completa</span><span class="sxs-lookup"><span data-stu-id="d4c44-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="d4c44-311">Nombre de servidor</span><span class="sxs-lookup"><span data-stu-id="d4c44-311">Server name</span></span> | <span data-ttu-id="d4c44-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="d4c44-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="d4c44-313">Ruta de acceso virtual</span><span class="sxs-lookup"><span data-stu-id="d4c44-313">Virtual path</span></span> | <span data-ttu-id="d4c44-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d4c44-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="d4c44-315">Ruta de acceso física</span><span class="sxs-lookup"><span data-stu-id="d4c44-315">Physical path</span></span> | <span data-ttu-id="d4c44-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="d4c44-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="d4c44-317">La raíz virtual es/, al igual que la raíz de la unidad C: \.</span><span class="sxs-lookup"><span data-stu-id="d4c44-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="d4c44-318">(Las rutas de acceso de carpeta virtual siempre usan barras diagonales). La ruta de acceso virtual de una carpeta no tiene que tener el mismo nombre que la carpeta física; puede ser un alias.</span><span class="sxs-lookup"><span data-stu-id="d4c44-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="d4c44-319">(En servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta).</span><span class="sxs-lookup"><span data-stu-id="d4c44-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="d4c44-320">Cuando se trabaja con archivos y carpetas en el código, a veces es necesario hacer referencia a la ruta de acceso física y, en ocasiones, a una ruta de acceso virtual, en función de los objetos con los que esté trabajando.</span><span class="sxs-lookup"><span data-stu-id="d4c44-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="d4c44-321">ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el método `Server.MapPath` y el operador `~` y el método `Href`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="d4c44-322">Conversión de rutas de acceso virtuales a físicas: el método Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="d4c44-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="d4c44-323">El método `Server.MapPath` convierte una ruta de acceso virtual (como */default.cshtml*) en una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="d4c44-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="d4c44-324">Utilice este método siempre que necesite una ruta de acceso física completa.</span><span class="sxs-lookup"><span data-stu-id="d4c44-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="d4c44-325">Un ejemplo típico es al leer o escribir un archivo de texto o de imagen en el servidor Web.</span><span class="sxs-lookup"><span data-stu-id="d4c44-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="d4c44-326">Normalmente, no se conoce la ruta de acceso física absoluta del sitio en el servidor de un sitio de hospedaje, por lo que este método puede convertir la ruta de acceso que se conoce (la ruta de acceso virtual) en la ruta de acceso correspondiente en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="d4c44-327">Pasa la ruta de acceso virtual a un archivo o una carpeta al método y devuelve la ruta de acceso física:</span><span class="sxs-lookup"><span data-stu-id="d4c44-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="d4c44-328">Referencia a la raíz virtual: el operador ~ y el método href</span><span class="sxs-lookup"><span data-stu-id="d4c44-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="d4c44-329">En un archivo *. cshtml* o *. vbhtml* , puede hacer referencia a la ruta de acceso de la raíz virtual mediante el operador `~`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="d4c44-330">Esto es muy útil porque puede mover páginas en un sitio y los vínculos que contienen a otras páginas no se romperán.</span><span class="sxs-lookup"><span data-stu-id="d4c44-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="d4c44-331">También es útil en caso de que se mueva el sitio web a una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="d4c44-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="d4c44-332">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="d4c44-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="d4c44-333">Si el sitio web está `http://myserver/myapp`, aquí se muestra cómo ASP.NET tratará estas rutas de acceso cuando se ejecute la página:</span><span class="sxs-lookup"><span data-stu-id="d4c44-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="d4c44-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="d4c44-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="d4c44-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="d4c44-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="d4c44-336">(En realidad, no verá estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso fuera).</span><span class="sxs-lookup"><span data-stu-id="d4c44-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="d4c44-337">Puede usar el operador `~` en el código del servidor (como arriba) y en el marcado, como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="d4c44-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="d4c44-338">En el marcado, se usa el operador `~` para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="d4c44-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="d4c44-339">Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todas las referencias de `~` a la ruta de acceso adecuada.</span><span class="sxs-lookup"><span data-stu-id="d4c44-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="d4c44-340">Lógica y bucles condicionales</span><span class="sxs-lookup"><span data-stu-id="d4c44-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="d4c44-341">El código de servidor ASP.NET permite realizar tareas basadas en condiciones y escribir código que repite instrucciones un número específico de veces, es decir, código que ejecuta un bucle).</span><span class="sxs-lookup"><span data-stu-id="d4c44-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="d4c44-342">Condiciones de prueba</span><span class="sxs-lookup"><span data-stu-id="d4c44-342">Testing conditions</span></span>

<span data-ttu-id="d4c44-343">Para probar una condición simple, use la instrucción `If...Then`, que devuelve `True` o `False` en función de una prueba que especifique:</span><span class="sxs-lookup"><span data-stu-id="d4c44-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="d4c44-344">La palabra clave `If` inicia un bloque.</span><span class="sxs-lookup"><span data-stu-id="d4c44-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="d4c44-345">La prueba real (condición) sigue a la palabra clave `If` y devuelve true o false.</span><span class="sxs-lookup"><span data-stu-id="d4c44-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="d4c44-346">La instrucción `If` finaliza con `Then`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="d4c44-347">Las instrucciones que se ejecutarán si la prueba es true se incluyen `If` y `End If`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="d4c44-348">Una instrucción `If` puede incluir un bloque de `Else` que especifica las instrucciones que se ejecutarán si la condición es falsa:</span><span class="sxs-lookup"><span data-stu-id="d4c44-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="d4c44-349">Si una instrucción `If` inicia un bloque de código, no tiene que utilizar las instrucciones de `Code...End Code` normales para incluir los bloques.</span><span class="sxs-lookup"><span data-stu-id="d4c44-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="d4c44-350">Simplemente puede Agregar `@` al bloque y funcionará.</span><span class="sxs-lookup"><span data-stu-id="d4c44-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="d4c44-351">Este enfoque funciona con `If` así como con otras palabras clave de programación de Visual Basic que van seguidas de bloques de código, incluidos `For`, `For Each`, `Do While`, etc.</span><span class="sxs-lookup"><span data-stu-id="d4c44-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="d4c44-352">Puede agregar varias condiciones mediante uno o varios bloques de `ElseIf`:</span><span class="sxs-lookup"><span data-stu-id="d4c44-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="d4c44-353">En este ejemplo, si la primera condición del bloque `If` no es true, se comprueba la condición de `ElseIf`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="d4c44-354">Si se cumple esta condición, se ejecutan las instrucciones del bloque `ElseIf`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="d4c44-355">Si no se cumple ninguna de las condiciones, se ejecutan las instrucciones del bloque `Else`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="d4c44-356">Puede agregar cualquier número de bloques de `ElseIf` y, a continuación, cerrar con un bloque de `Else` como &quot;todo lo demás&quot; condición.</span><span class="sxs-lookup"><span data-stu-id="d4c44-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="d4c44-357">Para probar un gran número de condiciones, use un bloque `Select Case`:</span><span class="sxs-lookup"><span data-stu-id="d4c44-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="d4c44-358">El valor que se va a probar se encuentra entre paréntesis (en el ejemplo, la variable Weekday).</span><span class="sxs-lookup"><span data-stu-id="d4c44-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="d4c44-359">Cada prueba individual utiliza una instrucción `Case` que muestra un valor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="d4c44-360">Si el valor de una instrucción `Case` coincide con el valor de prueba, se ejecuta el código de ese bloque de `Case`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="d4c44-361">El resultado de los dos últimos bloques condicionales mostrados en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="d4c44-363">Código de bucle</span><span class="sxs-lookup"><span data-stu-id="d4c44-363">Looping code</span></span>

<span data-ttu-id="d4c44-364">A menudo es necesario ejecutar las mismas instrucciones varias veces.</span><span class="sxs-lookup"><span data-stu-id="d4c44-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="d4c44-365">Para ello, puede crear un bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-365">You do this by looping.</span></span> <span data-ttu-id="d4c44-366">Por ejemplo, a menudo se ejecutan las mismas instrucciones para cada elemento en una colección de datos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="d4c44-367">Si sabe exactamente cuántas veces desea crear un bucle, puede usar un bucle `For`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="d4c44-368">Este tipo de bucle es especialmente útil para la acumulación o el recuento:</span><span class="sxs-lookup"><span data-stu-id="d4c44-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="d4c44-369">El bucle comienza con la palabra clave `For`, seguido de tres elementos:</span><span class="sxs-lookup"><span data-stu-id="d4c44-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="d4c44-370">Inmediatamente después de la instrucción `For`, se declara una variable de contador (no es necesario usar `Dim`) y, a continuación, se indica el intervalo, como en `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="d4c44-371">Esto significa que la variable `i` comenzará a contar en 10 y continuará hasta que alcance 20 (inclusive).</span><span class="sxs-lookup"><span data-stu-id="d4c44-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="d4c44-372">Entre las instrucciones `For` y `Next` se encuentra el contenido del bloque.</span><span class="sxs-lookup"><span data-stu-id="d4c44-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="d4c44-373">Puede contener una o varias instrucciones de código que se ejecutan con cada bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="d4c44-374">La instrucción `Next i` finaliza el bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="d4c44-375">Incrementa el contador e inicia la siguiente iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="d4c44-376">La línea de código entre las líneas `For` y `Next` contiene el código que se ejecuta para cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="d4c44-377">El marcado crea un nuevo párrafo (elemento`<p>`) cada vez y agrega una línea a la salida, mostrando el valor de i (el contador).</span><span class="sxs-lookup"><span data-stu-id="d4c44-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="d4c44-378">Al ejecutar esta página, en el ejemplo se crean 11 líneas que muestran la salida, con el texto en cada línea que indica el número de elemento.</span><span class="sxs-lookup"><span data-stu-id="d4c44-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor: Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="d4c44-380">Si está trabajando con una colección o una matriz, a menudo utiliza un bucle `For Each`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="d4c44-381">Una colección es un grupo de objetos similares y el bucle `For Each` le permite llevar a cabo una tarea en cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="d4c44-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="d4c44-382">Este tipo de bucle es práctico para las colecciones, porque a diferencia de un bucle `For`, no tiene que incrementar el contador o establecer un límite.</span><span class="sxs-lookup"><span data-stu-id="d4c44-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="d4c44-383">En su lugar, el código de bucle `For Each` simplemente continúa a través de la colección hasta que termina.</span><span class="sxs-lookup"><span data-stu-id="d4c44-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="d4c44-384">En este ejemplo se devuelven los elementos de la colección `Request.ServerVariables` (que contiene información sobre el servidor Web).</span><span class="sxs-lookup"><span data-stu-id="d4c44-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="d4c44-385">Usa un bucle `For Each` para mostrar el nombre de cada elemento mediante la creación de un nuevo elemento `<li>` en una lista con viñetas HTML.</span><span class="sxs-lookup"><span data-stu-id="d4c44-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="d4c44-386">La palabra clave `For Each` va seguida de una variable que representa un único elemento de la colección (en el ejemplo, `myItem`), seguido de la palabra clave `In`, seguida de la colección a la que desea recorrer el bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="d4c44-387">En el cuerpo del bucle `For Each`, puede tener acceso al elemento actual mediante la variable que declaró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d4c44-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor: Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="d4c44-389">Para crear un bucle de uso más general, use la instrucción `Do While`:</span><span class="sxs-lookup"><span data-stu-id="d4c44-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="d4c44-390">Este bucle comienza con la palabra clave `Do While`, seguida de una condición, seguida del bloque que se va a repetir.</span><span class="sxs-lookup"><span data-stu-id="d4c44-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="d4c44-391">Los bucles suelen incrementar (agregar a) o disminuir (restar de) una variable o un objeto que se usa para el recuento.</span><span class="sxs-lookup"><span data-stu-id="d4c44-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="d4c44-392">En el ejemplo, el operador `+=` agrega 1 al valor de una variable cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="d4c44-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="d4c44-393">(Para reducir una variable en un bucle que se repite, se usaría el operador de decremento `-=`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="d4c44-394">Objetos y colecciones</span><span class="sxs-lookup"><span data-stu-id="d4c44-394">Objects and Collections</span></span>

<span data-ttu-id="d4c44-395">Casi todo en un sitio web de ASP.NET es un objeto, incluida la propia página web.</span><span class="sxs-lookup"><span data-stu-id="d4c44-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="d4c44-396">En esta sección se describen algunos objetos importantes con los que trabajará con frecuencia en el código.</span><span class="sxs-lookup"><span data-stu-id="d4c44-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="d4c44-397">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="d4c44-397">Page objects</span></span>

<span data-ttu-id="d4c44-398">El objeto más básico de ASP.NET es la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="d4c44-399">Puede tener acceso directamente a las propiedades del objeto de página sin ningún objeto calificador.</span><span class="sxs-lookup"><span data-stu-id="d4c44-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="d4c44-400">En el código siguiente se obtiene la ruta de acceso del archivo de la página mediante el `Request` objeto de la página:</span><span class="sxs-lookup"><span data-stu-id="d4c44-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="d4c44-401">Puede utilizar las propiedades del objeto `Page` para obtener una gran cantidad de información, como:</span><span class="sxs-lookup"><span data-stu-id="d4c44-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="d4c44-402">`Request`Operador</span><span class="sxs-lookup"><span data-stu-id="d4c44-402">`Request`.</span></span> <span data-ttu-id="d4c44-403">Como ya ha visto, se trata de una colección de información sobre la solicitud actual, incluido el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc.</span><span class="sxs-lookup"><span data-stu-id="d4c44-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="d4c44-404">`Response`Operador</span><span class="sxs-lookup"><span data-stu-id="d4c44-404">`Response`.</span></span> <span data-ttu-id="d4c44-405">Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor haya terminado de ejecutarse.</span><span class="sxs-lookup"><span data-stu-id="d4c44-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="d4c44-406">Por ejemplo, puede usar esta propiedad para escribir información en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d4c44-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="d4c44-407">Objetos de colección (matrices y diccionarios)</span><span class="sxs-lookup"><span data-stu-id="d4c44-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="d4c44-408">Una colección es un grupo de objetos del mismo tipo, como una colección de objetos `Customer` de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="d4c44-409">ASP.NET contiene muchas colecciones integradas, como la `Request.Files` colección.</span><span class="sxs-lookup"><span data-stu-id="d4c44-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="d4c44-410">A menudo, trabajará con los datos de las colecciones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-410">You'll often work with data in collections.</span></span> <span data-ttu-id="d4c44-411">Dos tipos de colección comunes son la *matriz* y el *Diccionario*.</span><span class="sxs-lookup"><span data-stu-id="d4c44-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="d4c44-412">Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente que contenga cada elemento:</span><span class="sxs-lookup"><span data-stu-id="d4c44-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="d4c44-413">Con las matrices, se declara un tipo de datos específico, como `String`, `Integer`o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="d4c44-414">Para indicar que la variable puede contener una matriz, agregue paréntesis al nombre de la variable en la declaración (como `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="d4c44-415">Puede tener acceso a los elementos de una matriz mediante su posición (índice) o mediante la instrucción `For Each`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="d4c44-416">Los índices de matriz son de base &#8212; cero, es decir, el primer elemento está en la posición 0, el segundo elemento está en la posición 1, y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="d4c44-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="d4c44-417">Puede determinar el número de elementos de una matriz obteniendo su `Length` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d4c44-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="d4c44-418">Para obtener la posición de un elemento específico en la matriz (es decir, para buscar en la matriz), use el método `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="d4c44-419">También puede hacer cosas como invertir el contenido de una matriz (el método `Array.Reverse`) u ordenar el contenido (el método `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="d4c44-420">La salida del código de la matriz de cadenas que se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="d4c44-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="d4c44-422">Un diccionario es una colección de pares clave-valor, donde se proporciona la clave (o nombre) para establecer o recuperar el valor correspondiente:</span><span class="sxs-lookup"><span data-stu-id="d4c44-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="d4c44-423">Para crear un diccionario, use la palabra clave `New` para indicar que está creando un nuevo objeto `Dictionary`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="d4c44-424">Puede asignar un diccionario a una variable mediante la palabra clave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="d4c44-425">Se indican los tipos de datos de los elementos del diccionario mediante paréntesis (`( )`).</span><span class="sxs-lookup"><span data-stu-id="d4c44-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="d4c44-426">Al final de la declaración, debe agregar otro par de paréntesis, porque en realidad es un método que crea un nuevo diccionario.</span><span class="sxs-lookup"><span data-stu-id="d4c44-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="d4c44-427">Para agregar elementos al diccionario, puede llamar al método `Add` de la variable Dictionary (`myScores` en este caso) y, a continuación, especificar una clave y un valor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="d4c44-428">Como alternativa, puede usar paréntesis para indicar la clave y realizar una asignación simple, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d4c44-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="d4c44-429">Para obtener un valor del diccionario, especifique la clave entre paréntesis:</span><span class="sxs-lookup"><span data-stu-id="d4c44-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="d4c44-430">Llamar a métodos con parámetros</span><span class="sxs-lookup"><span data-stu-id="d4c44-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="d4c44-431">Como vimos anteriormente en este artículo, los objetos con los que programa tienen métodos.</span><span class="sxs-lookup"><span data-stu-id="d4c44-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="d4c44-432">Por ejemplo, un objeto de `Database` podría tener un método `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="d4c44-433">Muchos métodos también tienen uno o más parámetros.</span><span class="sxs-lookup"><span data-stu-id="d4c44-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="d4c44-434">Un *parámetro* es un valor que se pasa a un método para que el método pueda completar su tarea.</span><span class="sxs-lookup"><span data-stu-id="d4c44-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="d4c44-435">Por ejemplo, examine una declaración para el método `Request.MapPath`, que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="d4c44-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="d4c44-436">Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="d4c44-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="d4c44-437">Los tres parámetros del método son `virtualPath`, `baseVirtualDir`y `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="d4c44-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="d4c44-438">(Observe que en la declaración, los parámetros se enumeran con los tipos de datos de los datos que aceptarán). Cuando llame a este método, debe proporcionar valores para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="d4c44-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="d4c44-439">Cuando se usa Visual Basic con el sintaxis Razor, tiene dos opciones para pasar parámetros a un método: *parámetros posicionales* o *parámetros con nombre*.</span><span class="sxs-lookup"><span data-stu-id="d4c44-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="d4c44-440">Para llamar a un método mediante parámetros posicionales, se pasan los parámetros en un orden estricto que se especifica en la declaración del método.</span><span class="sxs-lookup"><span data-stu-id="d4c44-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="d4c44-441">(Normalmente se conoce este orden leyendo la documentación del método). Debe seguir el orden y no puede omitir ninguno de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o null para un parámetro posicional para el que no tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="d4c44-442">En el ejemplo siguiente se da por supuesto que tiene una carpeta denominada *scripts* en el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="d4c44-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="d4c44-443">El código llama al método `Request.MapPath` y pasa los valores de los tres parámetros en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="d4c44-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="d4c44-444">A continuación, se muestra la ruta de acceso asignada resultante.</span><span class="sxs-lookup"><span data-stu-id="d4c44-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="d4c44-445">Cuando hay muchos parámetros para un método, puede mantener el código más limpio y más legible mediante el uso de parámetros con nombre.</span><span class="sxs-lookup"><span data-stu-id="d4c44-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="d4c44-446">Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de `:=` y, a continuación, proporcione el valor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="d4c44-447">Una ventaja de los parámetros con nombre es que puede agregarlos en cualquier orden que desee.</span><span class="sxs-lookup"><span data-stu-id="d4c44-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="d4c44-448">(Una desventaja es que la llamada al método no es tan compacta).</span><span class="sxs-lookup"><span data-stu-id="d4c44-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="d4c44-449">En el ejemplo siguiente se llama al mismo método que el anterior, pero se usan parámetros con nombre para proporcionar los valores:</span><span class="sxs-lookup"><span data-stu-id="d4c44-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="d4c44-450">Como puede ver, los parámetros se pasan en un orden diferente.</span><span class="sxs-lookup"><span data-stu-id="d4c44-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="d4c44-451">Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverán el mismo valor.</span><span class="sxs-lookup"><span data-stu-id="d4c44-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="d4c44-452">Control de errores</span><span class="sxs-lookup"><span data-stu-id="d4c44-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="d4c44-453">Instrucciones try-catch</span><span class="sxs-lookup"><span data-stu-id="d4c44-453">Try-Catch statements</span></span>

<span data-ttu-id="d4c44-454">A menudo tendrá instrucciones en el código que podrían producir errores por motivos ajenos al control.</span><span class="sxs-lookup"><span data-stu-id="d4c44-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="d4c44-455">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d4c44-455">For example:</span></span>

- <span data-ttu-id="d4c44-456">Si el código intenta abrir, crear, leer o escribir un archivo, puede producirse todo tipo de errores.</span><span class="sxs-lookup"><span data-stu-id="d4c44-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="d4c44-457">Es posible que el archivo que desea no exista, que esté bloqueado, que el código no tenga permisos, etc.</span><span class="sxs-lookup"><span data-stu-id="d4c44-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="d4c44-458">Del mismo modo, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, que la conexión a la base de datos se haya quitado, que los datos que se van a guardar no sean válidos, etc.</span><span class="sxs-lookup"><span data-stu-id="d4c44-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="d4c44-459">En términos de programación, estas situaciones se denominan *excepciones*.</span><span class="sxs-lookup"><span data-stu-id="d4c44-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="d4c44-460">Si el código encuentra una excepción, genera (lanza) un mensaje de error que, en el mejor de los usuarios, es molesto.</span><span class="sxs-lookup"><span data-stu-id="d4c44-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor: Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="d4c44-462">En situaciones en las que el código pueda encontrar excepciones y para evitar los mensajes de error de este tipo, puede usar `Try/Catch` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="d4c44-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="d4c44-463">En la instrucción `Try`, ejecute el código que está comprobando.</span><span class="sxs-lookup"><span data-stu-id="d4c44-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="d4c44-464">En una o varias instrucciones de `Catch`, puede buscar errores específicos (tipos específicos de excepciones) que puedan haberse producido.</span><span class="sxs-lookup"><span data-stu-id="d4c44-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="d4c44-465">Puede incluir tantas instrucciones `Catch` como necesite para buscar los errores que espera.</span><span class="sxs-lookup"><span data-stu-id="d4c44-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="d4c44-466">Se recomienda evitar el uso del método `Response.Redirect` en `Try/Catch` instrucciones, ya que puede producir una excepción en la página.</span><span class="sxs-lookup"><span data-stu-id="d4c44-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="d4c44-467">En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="d4c44-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="d4c44-468">En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que se produzca una excepción.</span><span class="sxs-lookup"><span data-stu-id="d4c44-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="d4c44-469">El código incluye `Catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto y `DirectoryNotFoundException`, que se produce si ASP.NET no puede incluso encontrar la carpeta.</span><span class="sxs-lookup"><span data-stu-id="d4c44-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="d4c44-470">(Puede quitar el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente).</span><span class="sxs-lookup"><span data-stu-id="d4c44-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="d4c44-471">Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior.</span><span class="sxs-lookup"><span data-stu-id="d4c44-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="d4c44-472">Sin embargo, la sección `Try/Catch` ayuda a evitar que el usuario vea estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="d4c44-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="d4c44-473">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d4c44-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="d4c44-474">Documentación de referencia</span><span class="sxs-lookup"><span data-stu-id="d4c44-474">Reference Documentation</span></span>

- [<span data-ttu-id="d4c44-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d4c44-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="d4c44-476">Visual Basic (lenguaje)</span><span class="sxs-lookup"><span data-stu-id="d4c44-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
