---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Seguimiento de la información de los visitantes (análisis) de un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Una vez que haya llegado el sitio web, puede que desee analizar el tráfico del sitio Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421459"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="95a7e-103">Seguimiento de la información de los visitantes (análisis) de un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="95a7e-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="95a7e-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="95a7e-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="95a7e-105">En este artículo se describe cómo usar una aplicación auxiliar para agregar análisis de sitios web a páginas en un sitio web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="95a7e-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="95a7e-106">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="95a7e-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="95a7e-107">Cómo enviar información sobre el tráfico de su sitio web a un proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="95a7e-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="95a7e-108">Estas son las características de programación de ASP.NET que se introdujeron en el artículo:</span><span class="sxs-lookup"><span data-stu-id="95a7e-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="95a7e-109">Aplicación auxiliar de `Analytics`.</span><span class="sxs-lookup"><span data-stu-id="95a7e-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="95a7e-110">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="95a7e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="95a7e-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="95a7e-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="95a7e-112">Biblioteca de aplicaciones auxiliares Web de ASP.NET (paquete NuGet)</span><span class="sxs-lookup"><span data-stu-id="95a7e-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="95a7e-113">Analytics es un término general para la tecnología que mide el tráfico en su sitio web para que pueda entender cómo usan el sitio los usuarios.</span><span class="sxs-lookup"><span data-stu-id="95a7e-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="95a7e-114">Muchos servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros.</span><span class="sxs-lookup"><span data-stu-id="95a7e-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="95a7e-115">La forma en que el análisis funciona es que se suscribe a una cuenta con el proveedor de análisis, donde se registra el sitio del que desea realizar un seguimiento. El proveedor le envía un fragmento de código JavaScript que incluye un identificador o un código de seguimiento para su cuenta.</span><span class="sxs-lookup"><span data-stu-id="95a7e-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="95a7e-116">Agregue el fragmento de código de JavaScript a las páginas web del sitio del que desea realizar un seguimiento. (Normalmente, el fragmento de código de Analytics se agrega a un pie de página o a una página de diseño u otro marcado HTML que aparece en todas las páginas del sitio). Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual al proveedor de análisis, que registra varios detalles sobre la página.</span><span class="sxs-lookup"><span data-stu-id="95a7e-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="95a7e-117">Si desea ver las estadísticas del sitio, inicie sesión en el sitio web del proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="95a7e-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="95a7e-118">A continuación, puede ver todo tipo de informes sobre el sitio, como:</span><span class="sxs-lookup"><span data-stu-id="95a7e-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="95a7e-119">El número de vistas de página para páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="95a7e-119">The number of page views for individual pages.</span></span> <span data-ttu-id="95a7e-120">Esto le indica aproximadamente el número de personas que visitan el sitio y qué páginas del sitio son las más populares.</span><span class="sxs-lookup"><span data-stu-id="95a7e-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="95a7e-121">Cuánto tiempo gastan las personas en páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="95a7e-121">How long people spend on specific pages.</span></span> <span data-ttu-id="95a7e-122">Esto puede indicar si la Página principal mantiene el interés de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="95a7e-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="95a7e-123">En qué sitios se encontraban los usuarios antes de visitar el sitio.</span><span class="sxs-lookup"><span data-stu-id="95a7e-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="95a7e-124">Esto le ayudará a comprender si el tráfico procede de vínculos, de búsquedas, etc.</span><span class="sxs-lookup"><span data-stu-id="95a7e-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="95a7e-125">Cuando los usuarios visitan su sitio y cuánto tiempo permanecen.</span><span class="sxs-lookup"><span data-stu-id="95a7e-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="95a7e-126">En qué países se encuentran los visitantes.</span><span class="sxs-lookup"><span data-stu-id="95a7e-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="95a7e-127">Qué exploradores y sistemas operativos están usando los visitantes.</span><span class="sxs-lookup"><span data-stu-id="95a7e-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="95a7e-129">Usar una aplicación auxiliar para agregar análisis a una página</span><span class="sxs-lookup"><span data-stu-id="95a7e-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="95a7e-130">ASP.NET Web Pages incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`y `Analytics.GetStatCounterHtml`) que facilitan la administración de los fragmentos de código de JavaScript que se usan para el análisis.</span><span class="sxs-lookup"><span data-stu-id="95a7e-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="95a7e-131">En lugar de averiguar cómo y dónde colocar el código de JavaScript, lo único que tiene que hacer es agregar el ayudante a una página.</span><span class="sxs-lookup"><span data-stu-id="95a7e-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="95a7e-132">La única información que debe proporcionar es el nombre de la cuenta, el identificador o el código de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="95a7e-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="95a7e-133">(Para StatCounter, también tiene que proporcionar algunos valores adicionales).</span><span class="sxs-lookup"><span data-stu-id="95a7e-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="95a7e-134">En este procedimiento, creará una página de diseño que utiliza la aplicación auxiliar `GetGoogleHtml`.</span><span class="sxs-lookup"><span data-stu-id="95a7e-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="95a7e-135">Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede usar esa cuenta en su lugar y realizar pequeños ajustes según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="95a7e-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="95a7e-136">Al crear una cuenta de análisis, debe registrar la dirección URL del sitio del que desea realizar el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="95a7e-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="95a7e-137">Si está probando todo en el equipo local, no realizará el seguimiento del tráfico real (el único tráfico es), por lo que no podrá registrar y ver las estadísticas del sitio.</span><span class="sxs-lookup"><span data-stu-id="95a7e-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="95a7e-138">Pero este procedimiento muestra cómo agregar una aplicación auxiliar de análisis a una página.</span><span class="sxs-lookup"><span data-stu-id="95a7e-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="95a7e-139">Al publicar el sitio, el sitio activo enviará información al proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="95a7e-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="95a7e-140">Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="95a7e-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="95a7e-141">Cree una cuenta con Google Analytics y registre el nombre de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="95a7e-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="95a7e-142">Cree una página de diseño denominada *Analytics. cshtml* y agregue el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="95a7e-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="95a7e-143">Debe realizar la llamada a la aplicación auxiliar de `Analytics` en el cuerpo de la página web (antes de la etiqueta de `</body>`).</span><span class="sxs-lookup"><span data-stu-id="95a7e-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="95a7e-144">De lo contrario, el explorador no ejecutará el script.</span><span class="sxs-lookup"><span data-stu-id="95a7e-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="95a7e-145">Si usa un proveedor de análisis diferente, utilice en su lugar una de las aplicaciones auxiliares siguientes:</span><span class="sxs-lookup"><span data-stu-id="95a7e-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="95a7e-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="95a7e-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="95a7e-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="95a7e-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="95a7e-148">Reemplace `myaccount` por el nombre de la cuenta, el identificador o el código de seguimiento que creó en el paso 1.</span><span class="sxs-lookup"><span data-stu-id="95a7e-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="95a7e-149">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="95a7e-149">Run the page in the browser.</span></span> <span data-ttu-id="95a7e-150">(Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla).</span><span class="sxs-lookup"><span data-stu-id="95a7e-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="95a7e-151">En el explorador, vea el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="95a7e-151">In the browser, view the page source.</span></span> <span data-ttu-id="95a7e-152">Podrá ver el código de análisis representado:</span><span class="sxs-lookup"><span data-stu-id="95a7e-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="95a7e-153">Inicie sesión en el sitio de Google Analytics y examine las estadísticas de su sitio.</span><span class="sxs-lookup"><span data-stu-id="95a7e-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="95a7e-154">Si está ejecutando la página en un sitio activo, verá una entrada que registra la visita a la página.</span><span class="sxs-lookup"><span data-stu-id="95a7e-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="95a7e-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="95a7e-155">Additional Resources</span></span>

- [<span data-ttu-id="95a7e-156">Sitio de Google Analytics</span><span class="sxs-lookup"><span data-stu-id="95a7e-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="95a7e-157">Sitio de Yahoo! Web Analytics</span><span class="sxs-lookup"><span data-stu-id="95a7e-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="95a7e-158">Sitio de StatCounter</span><span class="sxs-lookup"><span data-stu-id="95a7e-158">StatCounter site</span></span>](http://statcounter.com/)
