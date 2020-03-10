---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Visualización de vídeo en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo mostrar vídeo en un ASP.NET Web Pages con sintaxis Razor página.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510385"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="3c419-103">Mostrar vídeo en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="3c419-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="3c419-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3c419-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3c419-105">En este artículo se explica cómo usar un reproductor de vídeo (multimedia) en un sitio web de ASP.NET Web Pages (Razor) para permitir a los usuarios ver los vídeos almacenados en el sitio.</span><span class="sxs-lookup"><span data-stu-id="3c419-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="3c419-106">ASP.NET Web Pages con sintaxis Razor le permite reproducir vídeos de Flash ( *. SWF*), Media Player ( *. wmv*) y Silverlight ( *. xap*).</span><span class="sxs-lookup"><span data-stu-id="3c419-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="3c419-107">Temas que se abordarán:</span><span class="sxs-lookup"><span data-stu-id="3c419-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3c419-108">Cómo elegir un reproductor de vídeo.</span><span class="sxs-lookup"><span data-stu-id="3c419-108">How to choose a video player.</span></span>
> - <span data-ttu-id="3c419-109">Cómo agregar vídeo a una página web.</span><span class="sxs-lookup"><span data-stu-id="3c419-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="3c419-110">Cómo establecer los atributos del reproductor de vídeo.</span><span class="sxs-lookup"><span data-stu-id="3c419-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="3c419-111">Estas son las características de las páginas de Razor ASP.NET presentadas en el artículo:</span><span class="sxs-lookup"><span data-stu-id="3c419-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="3c419-112">Aplicación auxiliar de `Video`.</span><span class="sxs-lookup"><span data-stu-id="3c419-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3c419-113">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3c419-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3c419-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="3c419-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="3c419-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="3c419-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="3c419-116">Este tutorial también funciona con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="3c419-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="3c419-117">Introducción</span><span class="sxs-lookup"><span data-stu-id="3c419-117">Introduction</span></span>

<span data-ttu-id="3c419-118">Es posible que desee mostrar un vídeo en el sitio.</span><span class="sxs-lookup"><span data-stu-id="3c419-118">You might want to display a video on your site.</span></span> <span data-ttu-id="3c419-119">Una forma de hacerlo es vincular a un sitio que ya tiene el vídeo, como YouTube.</span><span class="sxs-lookup"><span data-stu-id="3c419-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="3c419-120">Si desea insertar un vídeo desde estos sitios directamente en sus propias páginas, normalmente puede obtener el formato HTML del sitio y, a continuación, copiarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="3c419-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="3c419-121">Por ejemplo, en el ejemplo siguiente se muestra cómo insertar un vídeo de YouTube:</span><span class="sxs-lookup"><span data-stu-id="3c419-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="3c419-122">Si desea reproducir un vídeo que se encuentra en su propio sitio web (no en un sitio de uso compartido de vídeo público), no se puede vincular directamente a él mediante el marcado insertado como este.</span><span class="sxs-lookup"><span data-stu-id="3c419-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="3c419-123">Sin embargo, puede reproducir vídeos desde su sitio mediante la aplicación auxiliar de `Video`, que representa un reproductor multimedia directamente en una página.</span><span class="sxs-lookup"><span data-stu-id="3c419-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="3c419-124">Elección de un reproductor de vídeo</span><span class="sxs-lookup"><span data-stu-id="3c419-124">Choosing a Video Player</span></span>

<span data-ttu-id="3c419-125">Hay muchos formatos para los archivos de vídeo y cada formato normalmente requiere un reproductor diferente y una manera diferente de configurar el reproductor.</span><span class="sxs-lookup"><span data-stu-id="3c419-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="3c419-126">En las páginas de Razor ASP.NET, puede reproducir un vídeo en una página web con la aplicación auxiliar de `Video`.</span><span class="sxs-lookup"><span data-stu-id="3c419-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="3c419-127">La aplicación auxiliar de `Video` simplifica el proceso de insertar vídeos en una página web, ya que genera automáticamente los elementos HTML `object` y `embed` que se usan normalmente para agregar vídeo a la página.</span><span class="sxs-lookup"><span data-stu-id="3c419-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="3c419-128">La aplicación auxiliar de `Video` admite los siguientes reproductores multimedia:</span><span class="sxs-lookup"><span data-stu-id="3c419-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="3c419-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="3c419-129">Adobe Flash</span></span>
- <span data-ttu-id="3c419-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="3c419-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="3c419-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="3c419-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="3c419-132">El reproductor de Flash</span><span class="sxs-lookup"><span data-stu-id="3c419-132">The Flash Player</span></span>

<span data-ttu-id="3c419-133">El reproductor de `Flash` de la aplicación auxiliar de `Video` le permite reproducir vídeos Flash (archivos *. SWF* ) en una página web.</span><span class="sxs-lookup"><span data-stu-id="3c419-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="3c419-134">Como mínimo, debe proporcionar una ruta de acceso al archivo de vídeo.</span><span class="sxs-lookup"><span data-stu-id="3c419-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="3c419-135">Si no especifica nada excepto la ruta de acceso, el reproductor usa los valores predeterminados establecidos por la versión actual de Flash.</span><span class="sxs-lookup"><span data-stu-id="3c419-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="3c419-136">La configuración predeterminada típica es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="3c419-136">Typical default settings are:</span></span>

- <span data-ttu-id="3c419-137">El vídeo se muestra con el ancho y el alto predeterminados y sin un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="3c419-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="3c419-138">El vídeo se reproduce automáticamente cuando se carga la página.</span><span class="sxs-lookup"><span data-stu-id="3c419-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="3c419-139">El vídeo se repite continuamente hasta que se detiene explícitamente.</span><span class="sxs-lookup"><span data-stu-id="3c419-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="3c419-140">El vídeo se escala para mostrar todo el vídeo, en lugar de recortar el vídeo para ajustarse a un tamaño específico.</span><span class="sxs-lookup"><span data-stu-id="3c419-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="3c419-141">El vídeo se reproduce en una ventana.</span><span class="sxs-lookup"><span data-stu-id="3c419-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="3c419-142">El reproductor de MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="3c419-142">The MediaPlayer Player</span></span>

<span data-ttu-id="3c419-143">El reproductor de `MediaPlayer` de la aplicación auxiliar de `Video` le permite reproducir vídeos de Windows Media (archivos *. wmv* ), audio de Windows Media (archivos *. WMA* ) y MP3 (archivos *. mp3* ) en una página web.</span><span class="sxs-lookup"><span data-stu-id="3c419-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="3c419-144">Debe incluir la ruta de acceso del archivo multimedia que se va a reproducir. todos los demás parámetros son opcionales.</span><span class="sxs-lookup"><span data-stu-id="3c419-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="3c419-145">Si especifica solo una ruta de acceso, el reproductor utiliza la configuración predeterminada establecida por la versión actual de MediaPlayer, como:</span><span class="sxs-lookup"><span data-stu-id="3c419-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="3c419-146">El vídeo se muestra con el ancho y el alto predeterminados.</span><span class="sxs-lookup"><span data-stu-id="3c419-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="3c419-147">El vídeo se reproduce automáticamente cuando se carga la página.</span><span class="sxs-lookup"><span data-stu-id="3c419-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="3c419-148">El vídeo se reproduce una vez (no se repite).</span><span class="sxs-lookup"><span data-stu-id="3c419-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="3c419-149">El reproductor muestra el conjunto completo de controles en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3c419-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="3c419-150">El vídeo se reproduce en una ventana.</span><span class="sxs-lookup"><span data-stu-id="3c419-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="3c419-151">El reproductor de Silverlight</span><span class="sxs-lookup"><span data-stu-id="3c419-151">The Silverlight Player</span></span>

<span data-ttu-id="3c419-152">El reproductor de `Silverlight` de la aplicación auxiliar de `Video` le permite reproducir Windows Media Video (archivos *. wmv* ), Windows Media Audio (archivos *. WMA* ) y MP3 (archivos *. mp3* ).</span><span class="sxs-lookup"><span data-stu-id="3c419-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="3c419-153">Debe establecer el parámetro Path para que señale a un paquete de aplicación basado en Silverlight (archivo *. xap* ).</span><span class="sxs-lookup"><span data-stu-id="3c419-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="3c419-154">También debe establecer los parámetros de ancho y alto.</span><span class="sxs-lookup"><span data-stu-id="3c419-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="3c419-155">Todos los demás parámetros son opcionales.</span><span class="sxs-lookup"><span data-stu-id="3c419-155">All other parameters are optional.</span></span> <span data-ttu-id="3c419-156">Cuando se usa el reproductor de Silverlight para vídeo, si solo se establecen los parámetros necesarios, el reproductor de Silverlight muestra el vídeo sin un color de fondo.</span><span class="sxs-lookup"><span data-stu-id="3c419-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="3c419-157">En caso de que aún no conozca Silverlight: el archivo *. xap* es un archivo comprimido que contiene instrucciones de diseño en un archivo *. Xaml* , código administrado en ensamblados y recursos opcionales.</span><span class="sxs-lookup"><span data-stu-id="3c419-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="3c419-158">Puede crear un archivo *. xap* en Visual Studio como un proyecto de aplicación de Silverlight.</span><span class="sxs-lookup"><span data-stu-id="3c419-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="3c419-159">El reproductor de vídeo `Silverlight` usa la configuración que se proporciona para el reproductor y la configuración que se proporciona en el archivo *. xap* .</span><span class="sxs-lookup"><span data-stu-id="3c419-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="3c419-160">Tipos de MIME</span><span class="sxs-lookup"><span data-stu-id="3c419-160">MIME Types</span></span>
> 
> <span data-ttu-id="3c419-161">Cuando un explorador descarga un archivo, el explorador se asegura de que el tipo de archivo coincide con el tipo MIME especificado para el documento que se está representando.</span><span class="sxs-lookup"><span data-stu-id="3c419-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="3c419-162">El tipo MIME es el tipo de contenido o el tipo de medio de un archivo.</span><span class="sxs-lookup"><span data-stu-id="3c419-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="3c419-163">La aplicación auxiliar de `Video` usa los siguientes tipos MIME:</span><span class="sxs-lookup"><span data-stu-id="3c419-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="3c419-164">Reproducir vídeos Flash (. SWF)</span><span class="sxs-lookup"><span data-stu-id="3c419-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="3c419-165">En este procedimiento se muestra cómo reproducir un vídeo Flash denominado *sample. SWF*.</span><span class="sxs-lookup"><span data-stu-id="3c419-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="3c419-166">En el procedimiento se supone que tiene una carpeta denominada *media* en el sitio y que el archivo *. SWF* está en esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="3c419-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="3c419-167">Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha hecho.</span><span class="sxs-lookup"><span data-stu-id="3c419-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="3c419-168">En el sitio web, agregue una página y asígnele el nombre *FlashVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c419-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="3c419-169">Agregue el siguiente marcado a la página:</span><span class="sxs-lookup"><span data-stu-id="3c419-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="3c419-170">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="3c419-170">Run the page in a browser.</span></span> <span data-ttu-id="3c419-171">(Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Se muestra la página y el vídeo se reproduce automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3c419-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="3c419-172">![impresión](10-working-with-video/_static/image1.jpg "ch08_video -1. jpg")</span><span class="sxs-lookup"><span data-stu-id="3c419-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="3c419-173">Puede establecer el parámetro `quality` para un vídeo Flash en `low`, `autolow`, `autohigh`, `medium`, `high`y `best`:</span><span class="sxs-lookup"><span data-stu-id="3c419-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="3c419-174">Puede cambiar el vídeo Flash para que se reproduzca con un tamaño específico mediante el parámetro `scale`, que puede establecer en lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3c419-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="3c419-175">`showall`Operador</span><span class="sxs-lookup"><span data-stu-id="3c419-175">`showall`.</span></span> <span data-ttu-id="3c419-176">Esto hace que todo el vídeo sea visible mientras mantiene la relación de aspecto original.</span><span class="sxs-lookup"><span data-stu-id="3c419-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="3c419-177">Sin embargo, es posible que acabe con bordes en cada lado.</span><span class="sxs-lookup"><span data-stu-id="3c419-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="3c419-178">`noorder`Operador</span><span class="sxs-lookup"><span data-stu-id="3c419-178">`noorder`.</span></span> <span data-ttu-id="3c419-179">Esto escala el vídeo mientras mantiene la relación de aspecto original, pero puede que se recorte.</span><span class="sxs-lookup"><span data-stu-id="3c419-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="3c419-180">`exactfit`Operador</span><span class="sxs-lookup"><span data-stu-id="3c419-180">`exactfit`.</span></span> <span data-ttu-id="3c419-181">Esto hace que todo el vídeo sea visible sin conservar la relación de aspecto original, pero puede producirse una distorsión.</span><span class="sxs-lookup"><span data-stu-id="3c419-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="3c419-182">Si no especifica un parámetro `scale`, todo el vídeo será visible y la relación de aspecto original se mantendrá sin recortes.</span><span class="sxs-lookup"><span data-stu-id="3c419-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="3c419-183">En el ejemplo siguiente se muestra cómo usar el parámetro `scale`:</span><span class="sxs-lookup"><span data-stu-id="3c419-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="3c419-184">Flash Player admite una configuración de modo de vídeo denominada `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="3c419-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="3c419-185">Puede establecerlo en `window`, `opaque`y `transparent`.</span><span class="sxs-lookup"><span data-stu-id="3c419-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="3c419-186">De forma predeterminada, la `windowMode` se establece en `window`, que muestra el vídeo en una ventana independiente de la Página Web.</span><span class="sxs-lookup"><span data-stu-id="3c419-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="3c419-187">El valor `opaque` oculta todo lo que hay detrás del vídeo en la Página Web.</span><span class="sxs-lookup"><span data-stu-id="3c419-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="3c419-188">El valor `transparent` permite que el fondo de la página web se muestre a través del vídeo, suponiendo que cualquier parte del vídeo es transparente.</span><span class="sxs-lookup"><span data-stu-id="3c419-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="3c419-189">Reproducir vídeos de MediaPlayer ( *. wmv*)</span><span class="sxs-lookup"><span data-stu-id="3c419-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="3c419-190">En el procedimiento siguiente se muestra cómo reproducir un vídeo de Windows Media denominado *sample. wmv* que se encuentra en la carpeta *multimedia* .</span><span class="sxs-lookup"><span data-stu-id="3c419-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="3c419-191">Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="3c419-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="3c419-192">Cree una nueva página denominada *MediaPlayerVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c419-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="3c419-193">Agregue el siguiente marcado a la página:</span><span class="sxs-lookup"><span data-stu-id="3c419-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="3c419-194">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="3c419-194">Run the page in a browser.</span></span> <span data-ttu-id="3c419-195">El vídeo se carga y se reproduce automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3c419-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="3c419-196">![impresión](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")</span><span class="sxs-lookup"><span data-stu-id="3c419-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="3c419-197">Puede establecer `playCount` en un entero que indique el número de veces que se reproducirá el vídeo automáticamente:</span><span class="sxs-lookup"><span data-stu-id="3c419-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="3c419-198">El parámetro `uiMode` permite especificar qué controles se muestran en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="3c419-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="3c419-199">Puede establecer `uiMode` en `invisible`, `none`, `mini`o `full`.</span><span class="sxs-lookup"><span data-stu-id="3c419-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="3c419-200">Si no especifica un parámetro de `uiMode`, el vídeo se mostrará con la ventana de estado, la barra de búsqueda, los botones de control y los controles de volumen, además de la ventana de vídeo.</span><span class="sxs-lookup"><span data-stu-id="3c419-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="3c419-201">Estos controles también se mostrarán si usa el reproductor para reproducir un archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="3c419-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="3c419-202">A continuación se muestra un ejemplo de cómo usar el parámetro `uiMode`:</span><span class="sxs-lookup"><span data-stu-id="3c419-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="3c419-203">De forma predeterminada, el audio está encendido cuando el vídeo se reproduce.</span><span class="sxs-lookup"><span data-stu-id="3c419-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="3c419-204">Puede silenciar el audio estableciendo el parámetro `mute` en true:</span><span class="sxs-lookup"><span data-stu-id="3c419-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="3c419-205">Puede controlar el nivel de audio del vídeo de MediaPlayer si establece el parámetro `volume` en un valor entre 0 y 100.</span><span class="sxs-lookup"><span data-stu-id="3c419-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="3c419-206">El valor predeterminado es 50.</span><span class="sxs-lookup"><span data-stu-id="3c419-206">The default value is 50.</span></span> <span data-ttu-id="3c419-207">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="3c419-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="3c419-208">Reproducir vídeos de Silverlight</span><span class="sxs-lookup"><span data-stu-id="3c419-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="3c419-209">Este procedimiento muestra cómo reproducir el vídeo contenido en una página de Silverlight *. xap* que se encuentra en una carpeta denominada *media*.</span><span class="sxs-lookup"><span data-stu-id="3c419-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="3c419-210">Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="3c419-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="3c419-211">Cree una nueva página denominada *SilverlightVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3c419-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="3c419-212">Agregue el siguiente marcado a la página:</span><span class="sxs-lookup"><span data-stu-id="3c419-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="3c419-213">Ejecute la página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="3c419-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="3c419-214">![impresión](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")</span><span class="sxs-lookup"><span data-stu-id="3c419-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3c419-215">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3c419-215">Additional Resources</span></span>

<span data-ttu-id="3c419-216">[Información general de Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="3c419-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="3c419-217">Atributos de etiqueta de INCRUSTAción y objeto Flash</span><span class="sxs-lookup"><span data-stu-id="3c419-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="3c419-218">[Etiquetas de parámetros del SDK de Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="3c419-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
