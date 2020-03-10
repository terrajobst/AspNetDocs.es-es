---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creación de direcciones URL legibles en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se describe el enrutamiento en un sitio web de ASP.NET Web Pages (Razor) y cómo esto le permite usar direcciones URL más legibles y mejores para SEO. Qué desea...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509911"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7ac4d-104">Creación de direcciones URL legibles en sitios ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7ac4d-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="7ac4d-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7ac4d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7ac4d-106">En este artículo se describe el enrutamiento en un sitio web de ASP.NET Web Pages (Razor) y cómo esto le permite usar direcciones URL más legibles y mejores para SEO.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="7ac4d-107">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7ac4d-108">Cómo usa ASP.NET el enrutamiento para permitir el uso de direcciones URL más legibles y de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7ac4d-109">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="7ac4d-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7ac4d-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7ac4d-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7ac4d-111">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="7ac4d-112">Acerca del enrutamiento</span><span class="sxs-lookup"><span data-stu-id="7ac4d-112">About Routing</span></span>

<span data-ttu-id="7ac4d-113">Las direcciones URL de las páginas de su sitio pueden afectar al grado de funcionamiento del sitio.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="7ac4d-114">Una dirección URL &quot;&quot; descriptivos puede facilitar a los usuarios el uso del sitio.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="7ac4d-115">También puede ayudar con la optimización del motor de búsqueda (SEO) para el sitio.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="7ac4d-116">Los sitios web de ASP.NET incluyen la capacidad de usar direcciones URL descriptivas automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="7ac4d-117">ASP.NET permite crear direcciones URL significativas que describen las acciones del usuario en lugar de apuntar simplemente a un archivo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="7ac4d-118">Tenga en cuenta estas direcciones URL para un blog ficticio:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="7ac4d-119">Compare esas direcciones URL con las siguientes:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="7ac4d-120">En el primer par, un usuario tendría que saber que el blog se muestra mediante la página *blog. cshtml* y, a continuación, tendría que crear una cadena de consulta que obtenga la categoría o el intervalo de fechas correctos.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="7ac4d-121">El segundo conjunto de ejemplos es mucho más fácil de entender y crear.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="7ac4d-122">Las direcciones URL del primer ejemplo también señalan directamente a un archivo específico (*blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7ac4d-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="7ac4d-123">Si por alguna razón el blog se moviera a otra carpeta del servidor, o si el blog se reescribió para usar una página diferente, los vínculos no funcionarán correctamente.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="7ac4d-124">El segundo conjunto de direcciones URL no apunta a una página específica, por lo que, aunque la implementación o la ubicación del blog cambien, las direcciones URL seguirán siendo válidas.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="7ac4d-125">En ASP.NET Web Pages, puede crear direcciones URL más descriptivas, como las de los ejemplos anteriores, porque ASP.NET usa el *enrutamiento*.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="7ac4d-126">El enrutamiento crea una asignación lógica desde una dirección URL a una página (o páginas) que puede satisfacer la solicitud.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="7ac4d-127">Dado que la asignación es lógica (no física, a un archivo específico), el enrutamiento proporciona una gran flexibilidad en la forma de definir las direcciones URL para el sitio.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="7ac4d-128">Cómo funciona el enrutamiento</span><span class="sxs-lookup"><span data-stu-id="7ac4d-128">How Routing Works</span></span>

<span data-ttu-id="7ac4d-129">Cuando ASP.NET procesa una solicitud, lee la dirección URL para determinar cómo enrutarla.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="7ac4d-130">ASP.NET intenta hacer coincidir los segmentos individuales de la dirección URL con los archivos del disco, de izquierda a derecha.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="7ac4d-131">Si hay una coincidencia, todo lo restante en la dirección URL se pasa a la página como información de la *ruta de acceso*.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="7ac4d-132">Imagine que alguien realiza una solicitud con esta dirección URL:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="7ac4d-133">La búsqueda tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-133">The search goes like this:</span></span>

1. <span data-ttu-id="7ac4d-134">¿Hay un archivo con la ruta de acceso y el nombre de */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7ac4d-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="7ac4d-135">Si es así, ejecute esa página y no le pase ninguna información.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="7ac4d-136">De lo contrario...</span><span class="sxs-lookup"><span data-stu-id="7ac4d-136">Otherwise ...</span></span>
2. <span data-ttu-id="7ac4d-137">¿Hay un archivo con la ruta de acceso y el nombre de */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7ac4d-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="7ac4d-138">Si es así, ejecute esa página y pase el valor `c` a ella.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="7ac4d-139">En caso contrario,...</span><span class="sxs-lookup"><span data-stu-id="7ac4d-139">Otherwise …</span></span>
3. <span data-ttu-id="7ac4d-140">¿Hay un archivo con la ruta de acceso y el nombre de */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7ac4d-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="7ac4d-141">Si es así, ejecute esa página y pase el valor `b/c` a ella.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="7ac4d-142">Si la búsqueda no encuentra coincidencias exactas para los archivos *. cshtml* en sus carpetas especificadas, ASP.net continúa buscando estos archivos a su vez:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="7ac4d-143">*/a/b/c/default.cshtml* (sin información de ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="7ac4d-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="7ac4d-144">*/a/b/c/index.cshtml* (sin información de ruta de acceso).</span><span class="sxs-lookup"><span data-stu-id="7ac4d-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="7ac4d-145">Para ser claros, las solicitudes de páginas específicas (es decir, las solicitudes que incluyen la extensión de nombre de archivo *. cshtml* ) funcionan del mismo modo que cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="7ac4d-146">Una solicitud como `http://www.contoso.com/a/b.cshtml` ejecutará la página *b. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7ac4d-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="7ac4d-147">Dentro de una página, puede obtener la información de la ruta de acceso a través de la propiedad `UrlData` de la página, que es un diccionario.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="7ac4d-148">Imagine que tiene un archivo llamado *ViewCustomers. cshtml* y que el sitio obtiene esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="7ac4d-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="7ac4d-149">Tal y como se describe en las reglas anteriores, la solicitud irá a la página.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="7ac4d-150">Dentro de la página, puede usar código similar al siguiente para obtener y mostrar la información de la ruta de acceso (en este caso, el valor &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="7ac4d-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="7ac4d-151">Dado que el enrutamiento no implica nombres de archivo completos, puede haber ambigüedad si tiene páginas con el mismo nombre pero con extensiones de nombre de archivo diferentes (por ejemplo, la *página. cshtml* y la *página. html*).</span><span class="sxs-lookup"><span data-stu-id="7ac4d-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="7ac4d-152">Con el fin de evitar problemas con el enrutamiento, es mejor asegurarse de que no tiene páginas en el sitio cuyos nombres solo difieren en su extensión.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7ac4d-153">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7ac4d-153">Additional Resources</span></span>

<span data-ttu-id="7ac4d-154">[WebMatrix: direcciones URL, UrlData y enrutamiento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="7ac4d-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="7ac4d-155">En esta entrada de blog, Mike enlazód, se proporcionan algunos detalles adicionales sobre cómo funciona el enrutamiento en ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7ac4d-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
