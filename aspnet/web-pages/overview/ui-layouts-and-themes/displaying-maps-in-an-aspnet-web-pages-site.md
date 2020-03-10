---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualización de mapas en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este artículo se explica cómo mostrar mapas interactivos en páginas en un sitio web de ASP.NET Web Pages (Razor) en función de los servicios de asignación proporcionados por Bing, Google, MA...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518725"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="f1d13-103">Visualización de mapas en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f1d13-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="f1d13-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f1d13-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f1d13-105">En este artículo se explica cómo mostrar mapas interactivos en páginas en un sitio web de ASP.NET Web Pages (Razor) en función de los servicios de asignación proporcionados por Bing, Google, MapQuest y Yahoo.</span><span class="sxs-lookup"><span data-stu-id="f1d13-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="f1d13-106">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="f1d13-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f1d13-107">Cómo generar una asignación basada en una dirección.</span><span class="sxs-lookup"><span data-stu-id="f1d13-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="f1d13-108">Cómo generar una asignación basada en las coordenadas de latitud y longitud.</span><span class="sxs-lookup"><span data-stu-id="f1d13-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="f1d13-109">Cómo registrar una cuenta de desarrollador de Bing Maps y obtener una clave para usarla con mapas de Bing.</span><span class="sxs-lookup"><span data-stu-id="f1d13-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="f1d13-110">Esta es la característica ASP.NET presentada en el artículo:</span><span class="sxs-lookup"><span data-stu-id="f1d13-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="f1d13-111">Aplicación auxiliar de `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f1d13-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f1d13-112">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f1d13-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f1d13-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f1d13-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f1d13-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="f1d13-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="f1d13-115">Este tutorial también funciona con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="f1d13-115">This tutorial also works with WebMatrix 3.</span></span>

<span data-ttu-id="f1d13-116">En las páginas web, puede mostrar las asignaciones en una página mediante `Maps` aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1d13-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="f1d13-117">Puede generar asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud.</span><span class="sxs-lookup"><span data-stu-id="f1d13-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="f1d13-118">La clase `Maps` le permite llamar a motores de mapas populares, como Bing, Google, MapQuest y Yahoo.</span><span class="sxs-lookup"><span data-stu-id="f1d13-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="f1d13-119">Los pasos para agregar la asignación a una página son los mismos independientemente de los motores de mapa a los que llame.</span><span class="sxs-lookup"><span data-stu-id="f1d13-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="f1d13-120">Solo tiene que agregar una referencia de archivo JavaScript que haga que los métodos disponibles muestren el mapa y, a continuación, llamar a los métodos de la aplicación auxiliar `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f1d13-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="f1d13-121">Puede elegir un servicio de asignación basado en el `Maps` método auxiliar que use.</span><span class="sxs-lookup"><span data-stu-id="f1d13-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="f1d13-122">Puede usar cualquiera de estos:</span><span class="sxs-lookup"><span data-stu-id="f1d13-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="f1d13-123">Instalación de las piezas necesarias</span><span class="sxs-lookup"><span data-stu-id="f1d13-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="f1d13-124">Para mostrar las asignaciones, necesita estos elementos:</span><span class="sxs-lookup"><span data-stu-id="f1d13-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="f1d13-125">Aplicación auxiliar de `Maps`.</span><span class="sxs-lookup"><span data-stu-id="f1d13-125">The `Maps` helper.</span></span> <span data-ttu-id="f1d13-126">Esta aplicación auxiliar está en la versión 2 de la biblioteca de aplicaciones auxiliares Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1d13-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="f1d13-127">Si aún no ha agregado la biblioteca, puede instalarla en el sitio como un paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="f1d13-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="f1d13-128">Para obtener más información, consulte [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="f1d13-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="f1d13-129">(En la galería, busque el paquete de `microsoft-web-helpers`).</span><span class="sxs-lookup"><span data-stu-id="f1d13-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="f1d13-130">Biblioteca jQuery.</span><span class="sxs-lookup"><span data-stu-id="f1d13-130">The jQuery library.</span></span> <span data-ttu-id="f1d13-131">Algunas de las plantillas de sitio de WebMatrix ya incluyen bibliotecas de jQuery en sus carpetas de *scripts* .</span><span class="sxs-lookup"><span data-stu-id="f1d13-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="f1d13-132">Si no tiene estas bibliotecas, puede descargar la biblioteca de jQuery más reciente directamente desde el sitio de [jQuery.org](http://jQuery.org) .</span><span class="sxs-lookup"><span data-stu-id="f1d13-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="f1d13-133">También puede crear un sitio nuevo mediante una plantilla (por ejemplo, la plantilla del **sitio de inicio** ) y, a continuación, copiar los archivos de jQuery desde ese sitio al sitio actual.</span><span class="sxs-lookup"><span data-stu-id="f1d13-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="f1d13-134">Por último, si desea utilizar mapas de Bing, primero debe crear una cuenta (gratuita) y obtener una clave.</span><span class="sxs-lookup"><span data-stu-id="f1d13-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="f1d13-135">Para obtener una clave, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="f1d13-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="f1d13-136">Cree una cuenta en la [cuenta de desarrollador de Bing Maps](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1d13-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="f1d13-137">También debe tener un cuenta de Microsoft (Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="f1d13-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="f1d13-138">Puede especificar que desea utilizar la clave para **evaluar y probar**.</span><span class="sxs-lookup"><span data-stu-id="f1d13-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="f1d13-139">Si va a probar la función de asignación en su propio equipo mediante WebMatrix y IIS Express, vaya al área de trabajo del **sitio** y anote la dirección URL del sitio (por ejemplo, `http://localhost:50408`, aunque es probable que el número de Puerto sea diferente).</span><span class="sxs-lookup"><span data-stu-id="f1d13-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="f1d13-140">Puede usar esta dirección *localhost* como sitio al registrarse.</span><span class="sxs-lookup"><span data-stu-id="f1d13-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="f1d13-141">Una vez que se haya registrado para una cuenta, vaya al centro de cuentas de Bing Maps y haga clic en **crear o ver claves**:</span><span class="sxs-lookup"><span data-stu-id="f1d13-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![asignación-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="f1d13-143">Grabe la clave que Bing crea.</span><span class="sxs-lookup"><span data-stu-id="f1d13-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="f1d13-144">Crear una asignación basada en una dirección (mediante Google)</span><span class="sxs-lookup"><span data-stu-id="f1d13-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="f1d13-145">En el ejemplo siguiente se muestra cómo crear una página que representa una asignación basada en una dirección.</span><span class="sxs-lookup"><span data-stu-id="f1d13-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="f1d13-146">En este ejemplo se muestra cómo usar Google Maps.</span><span class="sxs-lookup"><span data-stu-id="f1d13-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="f1d13-147">Cree un archivo denominado *MapAddress. cshtml* en la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="f1d13-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="f1d13-148">En esta página se generará una asignación basada en una dirección que se le pase.</span><span class="sxs-lookup"><span data-stu-id="f1d13-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="f1d13-149">Copie el siguiente código en el archivo, sobrescribiendo el contenido existente.</span><span class="sxs-lookup"><span data-stu-id="f1d13-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="f1d13-150">Tenga en cuenta las siguientes características de la página:</span><span class="sxs-lookup"><span data-stu-id="f1d13-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="f1d13-151">El elemento `<script>` del elemento `<head>`.</span><span class="sxs-lookup"><span data-stu-id="f1d13-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="f1d13-152">En el ejemplo, el elemento `<script>` hace referencia al archivo *jQuery-1.6.4. min. js* , que es una versión reducida (comprimida) de la biblioteca de jQuery, versión 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="f1d13-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="f1d13-153">Tenga en cuenta que la referencia supone que el archivo *. js* está en la carpeta *scripts* del sitio.</span><span class="sxs-lookup"><span data-stu-id="f1d13-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="f1d13-154">Si usa una versión diferente de la biblioteca de jQuery, asegúrese de que está apuntando correctamente a esa versión.</span><span class="sxs-lookup"><span data-stu-id="f1d13-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="f1d13-155">La llamada a la `@Maps.GetGoogleHtml` en el cuerpo de la página.</span><span class="sxs-lookup"><span data-stu-id="f1d13-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="f1d13-156">Para asignar una dirección, debe pasar una cadena de dirección.</span><span class="sxs-lookup"><span data-stu-id="f1d13-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="f1d13-157">Los métodos para los demás motores de mapa funcionan de manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="f1d13-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="f1d13-158">Ejecute la página y escriba una dirección.</span><span class="sxs-lookup"><span data-stu-id="f1d13-158">Run the page and enter an address.</span></span> <span data-ttu-id="f1d13-159">La página muestra un mapa, basado en Google Maps, que muestra la ubicación que especificó.</span><span class="sxs-lookup"><span data-stu-id="f1d13-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![asignación-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="f1d13-161">Crear una asignación basada en las coordenadas de latitud y longitud (mediante Bing)</span><span class="sxs-lookup"><span data-stu-id="f1d13-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="f1d13-162">En este ejemplo se muestra cómo crear una asignación basada en coordenadas.</span><span class="sxs-lookup"><span data-stu-id="f1d13-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="f1d13-163">En este ejemplo se muestra cómo usar Bing Maps y cómo incluir la clave de Bing.</span><span class="sxs-lookup"><span data-stu-id="f1d13-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="f1d13-164">(Puede crear una asignación basada en coordenadas utilizando también los otros motores de mapa, sin usar una clave de Bing).</span><span class="sxs-lookup"><span data-stu-id="f1d13-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="f1d13-165">Cree un archivo denominado *MapCoordinates. cshtml* en la raíz del sitio y reemplace el contenido existente por el siguiente código y marcado:</span><span class="sxs-lookup"><span data-stu-id="f1d13-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="f1d13-166">Reemplace `your-key-here` por la clave de Bing Maps que ha generado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f1d13-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="f1d13-167">Ejecute la página *MapCoordinates. cshtml* , escriba las coordenadas de latitud y longitud y, a continuación, haga clic en el **mapa** .</span><span class="sxs-lookup"><span data-stu-id="f1d13-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="f1d13-168">.</span><span class="sxs-lookup"><span data-stu-id="f1d13-168">button.</span></span> <span data-ttu-id="f1d13-169">(Si no conoce ninguna de las coordenadas, intente lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="f1d13-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="f1d13-170">Se trata de una ubicación en el campus de Microsoft Redmond).</span><span class="sxs-lookup"><span data-stu-id="f1d13-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="f1d13-171">Latitud: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="f1d13-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="f1d13-172">Longitud:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="f1d13-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="f1d13-173">La página se muestra con las coordenadas especificadas.</span><span class="sxs-lookup"><span data-stu-id="f1d13-173">The page is displayed using the coordinates that you specified.</span></span>

     ![asignación-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f1d13-175">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f1d13-175">Additional Resources</span></span>

[<span data-ttu-id="f1d13-176">Referencia de la API de Microsoft. Maps</span><span class="sxs-lookup"><span data-stu-id="f1d13-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
