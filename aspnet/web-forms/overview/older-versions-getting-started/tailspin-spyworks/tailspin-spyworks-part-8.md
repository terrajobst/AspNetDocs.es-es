---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Parte 8: páginas finales, control de excepciones y conclusión | Microsoft Docs'
author: JoeStagner
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks. La parte 8 agrega una página de contacto, acerca de la página y la excepción...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474343"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="5a9c9-104">Parte 8: páginas finales, control de excepciones y conclusión</span><span class="sxs-lookup"><span data-stu-id="5a9c9-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>

<span data-ttu-id="5a9c9-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5a9c9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5a9c9-106">Tailspin Spyworks muestra cómo es extraordinariamente simple la creación de aplicaciones eficaces y escalables para la plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5a9c9-107">Muestra cómo usar las excelentes características nuevas de ASP.NET 4 para crear una tienda en línea, como compras, desprotección y administración.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5a9c9-108">En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5a9c9-109">La parte 8 agrega una página de contacto, about Page y control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="5a9c9-110">Esta es la conclusión de la serie.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-110">This is the conclusion of the series.</span></span>

## <a id="_Toc260221680"></a><span data-ttu-id="5a9c9-111">Página de contacto (envío de correo electrónico desde ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="5a9c9-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="5a9c9-112">Cree una nueva página denominada contactus. aspx</span><span class="sxs-lookup"><span data-stu-id="5a9c9-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="5a9c9-113">Con el diseñador, cree el siguiente formulario tomando nota especial para incluir ToolkitScriptManager y el control de editor de AjaxControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxControlToolkit.</span></span> <span data-ttu-id="5a9c9-114">.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="5a9c9-115">Haga doble clic en el botón "enviar" para generar un controlador de eventos de clic en el archivo de código subyacente e implementar un método para enviar la información de contacto como un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="5a9c9-116">Este código requiere que el archivo Web. config contenga una entrada en la sección de configuración que especifique el servidor SMTP que se va a usar para enviar correo.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="5a9c9-117">Página acerca de</span><span class="sxs-lookup"><span data-stu-id="5a9c9-117">About Page</span></span>

<span data-ttu-id="5a9c9-118">Cree una página denominada aboutUs. aspx y agregue el contenido que desee.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="5a9c9-119">Controlador de excepciones global</span><span class="sxs-lookup"><span data-stu-id="5a9c9-119">Global Exception Handler</span></span>

<span data-ttu-id="5a9c9-120">Por último, a lo largo de la aplicación hemos producido excepciones y existen circunstancias imprevistas que también causan excepciones no controladas en nuestra aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="5a9c9-121">Nunca queremos que se muestre una excepción no controlada en el visitante de un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="5a9c9-122">Además de ser una experiencia de usuario muy terrible, las excepciones no controladas también pueden ser un problema de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="5a9c9-123">Para solucionar este problema, implementaremos un controlador de excepciones global.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="5a9c9-124">Para ello, abra el archivo global. asax y observe el siguiente controlador de eventos generado previamente.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="5a9c9-125">Agregue código para implementar el controlador de errores de\_de la aplicación como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="5a9c9-126">Después, agregue una página denominada error. aspx a la solución y agregue este fragmento de código de marcado.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="5a9c9-127">Ahora, en la página\_controlador de eventos de carga Extraiga los mensajes de error del objeto de solicitud.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="5a9c9-128">Celebre</span><span class="sxs-lookup"><span data-stu-id="5a9c9-128">Conclusion</span></span>

<span data-ttu-id="5a9c9-129">Hemos visto que los WebForms de ASP.NET facilitan la creación de un sitio web sofisticado con acceso a bases de datos, pertenencia, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="5a9c9-130">con bastante rapidez.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-130">pretty quickly.</span></span>

<span data-ttu-id="5a9c9-131">Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear sus propias aplicaciones de WebForms de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5a9c9-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5a9c9-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="5a9c9-132">Previous</span></span>](tailspin-spyworks-part-7.md)
