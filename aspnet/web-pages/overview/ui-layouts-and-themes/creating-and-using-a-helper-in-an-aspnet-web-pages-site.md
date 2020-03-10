---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Crear y usar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo crear una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado en perf...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454309"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="5bf47-104">Crear y usar una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="5bf47-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="5bf47-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5bf47-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5bf47-106">En este artículo se describe cómo crear una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="5bf47-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="5bf47-107">Una *aplicación auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser tediosa o compleja.</span><span class="sxs-lookup"><span data-stu-id="5bf47-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="5bf47-108">**Lo que aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="5bf47-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="5bf47-109">Cómo crear y usar una aplicación auxiliar simple.</span><span class="sxs-lookup"><span data-stu-id="5bf47-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="5bf47-110">Estas son las características de ASP.NET presentadas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="5bf47-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="5bf47-111">Sintaxis de `@helper`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5bf47-112">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="5bf47-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="5bf47-113">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="5bf47-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="5bf47-114">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="5bf47-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="5bf47-115">Información general de las aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="5bf47-115">Overview of Helpers</span></span>

<span data-ttu-id="5bf47-116">Si necesita realizar las mismas tareas en diferentes páginas de su sitio, puede usar una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="5bf47-117">ASP.NET Web Pages incluye una serie de aplicaciones auxiliares y hay muchas más que puede descargar e instalar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="5bf47-118">(Una lista de las aplicaciones auxiliares integradas en ASP.NET Web Pages se muestra en la [referencia rápida](https://go.microsoft.com/fwlink/?LinkId=202907)de la API de ASP.net). Si ninguna de las aplicaciones auxiliares existentes satisface sus necesidades, puede crear su propia aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="5bf47-119">Una aplicación auxiliar le permite usar un bloque de código común en varias páginas.</span><span class="sxs-lookup"><span data-stu-id="5bf47-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="5bf47-120">Supongamos que, en la página, a menudo desea crear un elemento de nota que se diferencia de los párrafos normales.</span><span class="sxs-lookup"><span data-stu-id="5bf47-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="5bf47-121">Quizás la nota se crea como un elemento `<div>` con el estilo de cuadro con un borde.</span><span class="sxs-lookup"><span data-stu-id="5bf47-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="5bf47-122">En lugar de agregar este mismo marcado a una página cada vez que desea mostrar una nota, puede empaquetar el marcado como una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="5bf47-123">Después, puede insertar la nota con una sola línea de código en cualquier lugar en que la necesite.</span><span class="sxs-lookup"><span data-stu-id="5bf47-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="5bf47-124">El uso de una aplicación auxiliar como esta hace que el código de cada una de las páginas sea más sencillo y fácil de leer.</span><span class="sxs-lookup"><span data-stu-id="5bf47-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="5bf47-125">También facilita el mantenimiento del sitio, porque si necesita cambiar el aspecto de las notas, puede cambiar el marcado en un solo lugar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="5bf47-126">Crear una aplicación auxiliar</span><span class="sxs-lookup"><span data-stu-id="5bf47-126">Creating a Helper</span></span>

<span data-ttu-id="5bf47-127">En este procedimiento se muestra cómo crear la aplicación auxiliar que crea la nota, tal y como se describe aquí.</span><span class="sxs-lookup"><span data-stu-id="5bf47-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="5bf47-128">Este es un ejemplo sencillo, pero el ayudante personalizado puede incluir cualquier marcado y el código de ASP.NET que necesite.</span><span class="sxs-lookup"><span data-stu-id="5bf47-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="5bf47-129">En la carpeta raíz del sitio web, cree una carpeta denominada *App\_Code*.</span><span class="sxs-lookup"><span data-stu-id="5bf47-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="5bf47-130">Se trata de un nombre de carpeta reservado en ASP.NET donde puede colocar código para componentes como aplicaciones auxiliares.</span><span class="sxs-lookup"><span data-stu-id="5bf47-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="5bf47-131">En la carpeta de *código del\_* de la aplicación, cree un nuevo archivo *. cshtml* y asígnele el nombre mis *aplicaciones. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5bf47-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="5bf47-132">Reemplace el contenido existente por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5bf47-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="5bf47-133">El código usa la sintaxis de `@helper` para declarar un nuevo ayudante denominado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="5bf47-134">Esta aplicación auxiliar concreta le permite pasar un parámetro denominado `content` que puede contener una combinación de texto y marcado.</span><span class="sxs-lookup"><span data-stu-id="5bf47-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="5bf47-135">La aplicación auxiliar inserta la cadena en el cuerpo de la nota mediante el `@content` variable.</span><span class="sxs-lookup"><span data-stu-id="5bf47-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="5bf47-136">Tenga en cuenta que el nombre del archivo es me *helper. cshtml*, pero el ayudante se denomina `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="5bf47-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="5bf47-137">Puede incluir varias aplicaciones auxiliares personalizadas en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="5bf47-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="5bf47-138">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="5bf47-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="5bf47-139">Usar el ayudante en una página</span><span class="sxs-lookup"><span data-stu-id="5bf47-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="5bf47-140">En la carpeta raíz, cree un nuevo archivo en blanco denominado *TestHelper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5bf47-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="5bf47-141">Agregue el código siguiente al archivo:</span><span class="sxs-lookup"><span data-stu-id="5bf47-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="5bf47-142">Para llamar al ayudante que creó, utilice `@` seguido del nombre de archivo donde el ayudante es, un punto y, a continuación, el nombre de la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="5bf47-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="5bf47-143">(Si tiene varias carpetas en la carpeta de *código del\_* de la aplicación, puede usar la sintaxis `@FolderName.FileName.HelperName` para llamar a la aplicación auxiliar en cualquier nivel de carpeta anidada).</span><span class="sxs-lookup"><span data-stu-id="5bf47-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="5bf47-144">El texto que agregue entre comillas entre paréntesis es el texto que el ayudante mostrará como parte de la nota en la Página Web.</span><span class="sxs-lookup"><span data-stu-id="5bf47-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="5bf47-145">Guarde la página y ejecútela en un explorador.</span><span class="sxs-lookup"><span data-stu-id="5bf47-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="5bf47-146">El ayudante genera el elemento de nota justo donde llamó a la aplicación auxiliar: entre los dos párrafos.</span><span class="sxs-lookup"><span data-stu-id="5bf47-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Captura de pantalla que muestra la página en el explorador y cómo el ayudante generó el marcado que coloca un cuadro alrededor del texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="5bf47-148">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5bf47-148">Additional Resources</span></span>

<span data-ttu-id="5bf47-149">[Menú horizontal como aplicación auxiliar de Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="5bf47-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="5bf47-150">En esta entrada de blog de Mike Pope se muestra cómo crear un menú horizontal como ayudante mediante marcado, CSS y código.</span><span class="sxs-lookup"><span data-stu-id="5bf47-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="5bf47-151">Uso de [HTML5 en ASP.NET Web pages aplicaciones auxiliares para WebMatrix y ASP.net MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="5bf47-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="5bf47-152">En esta entrada de blog de Sam Abraham se muestra una aplicación auxiliar que representa un elemento de `Canvas` HTML5.</span><span class="sxs-lookup"><span data-stu-id="5bf47-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="5bf47-153">[La diferencia entre @Helpers y @Functions en WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="5bf47-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="5bf47-154">En esta entrada de blog, Mike Salmuered describe `@helper` sintaxis y `@function` sintaxis y Cuándo usar cada una.</span><span class="sxs-lookup"><span data-stu-id="5bf47-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
