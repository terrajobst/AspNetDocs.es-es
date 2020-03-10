---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalación de una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe cómo instalar una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor). Una aplicación auxiliar es un componente reutilizable que incluye código y marcado a por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518647"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="106af-104">Instalación de una aplicación auxiliar en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="106af-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="106af-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="106af-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="106af-106">En este artículo se describe cómo instalar una aplicación auxiliar en un sitio web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="106af-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="106af-107">Una *aplicación auxiliar* es un componente reutilizable que incluye código y marcado para realizar una tarea que podría ser tediosa o compleja.</span><span class="sxs-lookup"><span data-stu-id="106af-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="106af-108">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="106af-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="106af-109">Cómo instalar una aplicación auxiliar en un sitio web creado mediante WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="106af-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="106af-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="106af-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="106af-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="106af-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="106af-112">Información general de las aplicaciones auxiliares</span><span class="sxs-lookup"><span data-stu-id="106af-112">Overview of Helpers</span></span>

<span data-ttu-id="106af-113">Algunas tareas que los usuarios suelen querer realizar en páginas Web requieren mucho código o requieren conocimientos adicionales.</span><span class="sxs-lookup"><span data-stu-id="106af-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="106af-114">Entre los ejemplos se incluye la presentación de un gráfico para los datos; colocar un botón "seguir" de Twitter en una página; enviar correo electrónico desde su sitio web; recortar o cambiar el tamaño de las imágenes; usar PayPal para su sitio.</span><span class="sxs-lookup"><span data-stu-id="106af-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="106af-115">Para facilitar la creación de estos tipos de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*.</span><span class="sxs-lookup"><span data-stu-id="106af-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="106af-116">Las aplicaciones auxiliares son componentes que se instalan para un sitio y que permiten realizar tareas típicas usando solo una línea o dos de código Razor.</span><span class="sxs-lookup"><span data-stu-id="106af-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="106af-117">ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas.</span><span class="sxs-lookup"><span data-stu-id="106af-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="106af-118">Sin embargo, hay muchas aplicaciones auxiliares disponibles en paquetes (complementos) que se proporcionan mediante el administrador de paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="106af-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="106af-119">NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se ocupa de todos los detalles de la instalación.</span><span class="sxs-lookup"><span data-stu-id="106af-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="106af-120">Instalación de una aplicación auxiliar en WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="106af-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="106af-121">En WebMatrix 3, haga clic en el botón **NuGet** .</span><span class="sxs-lookup"><span data-stu-id="106af-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Cuadro de diálogo Galería de NuGet en WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="106af-123">Se inicia el administrador de paquetes NuGet y se muestran los paquetes disponibles.</span><span class="sxs-lookup"><span data-stu-id="106af-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="106af-124">En el cuadro de búsqueda, escriba una palabra clave para el ayudante que desea instalar.</span><span class="sxs-lookup"><span data-stu-id="106af-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Cuadro de diálogo Galería de NuGet en WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="106af-126">Seleccione el paquete y, a continuación, haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="106af-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="106af-127">Haga clic en **sí** cuando se le pregunte si desea instalar el paquete e indicar que acepta los términos.</span><span class="sxs-lookup"><span data-stu-id="106af-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="106af-128">Si es la primera vez que instala una aplicación auxiliar, NuGet crea carpetas en el sitio web para el código que constituye el ayudante.</span><span class="sxs-lookup"><span data-stu-id="106af-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="106af-129">Para desinstalar una aplicación auxiliar, haga clic en el botón **Galería** , haga clic en la pestaña **instalado** y seleccione el paquete que desea desinstalar.</span><span class="sxs-lookup"><span data-stu-id="106af-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="106af-130">Instalación de la aplicación auxiliar de Twitter</span><span class="sxs-lookup"><span data-stu-id="106af-130">Installing the Twitter helper</span></span>

<span data-ttu-id="106af-131">La versión más reciente de la API de Twitter no es compatible con la aplicación auxiliar de Twitter que se instala a través de NuGet.</span><span class="sxs-lookup"><span data-stu-id="106af-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="106af-132">En su lugar, consulte el tema [aplicación auxiliar de Twitter con WebMatrix](twitter-helper.md) para obtener información sobre cómo configurar la aplicación auxiliar de Twitter en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="106af-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="106af-133">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="106af-133">Additional Resources</span></span>

[<span data-ttu-id="106af-134">Introducción a ASP.NET Web Pages 2: aspectos básicos de la programación</span><span class="sxs-lookup"><span data-stu-id="106af-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="106af-135">Aplicación auxiliar de Twitter con WebMatrix</span><span class="sxs-lookup"><span data-stu-id="106af-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
