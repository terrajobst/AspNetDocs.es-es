---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[Cómo:]  Almacenar en caché una página de ASP.NET basada en la información del encabezado HTTP | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo mantener una página en la caché de resultados de ASP.NET basándose en la información del encabezado HTTP de la página. En primer lugar, el posible encabe HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506977"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="ec5b8-104">[Cómo:]  Almacenar en caché una página de ASP.NET basada en la información del encabezado HTTP</span><span class="sxs-lookup"><span data-stu-id="ec5b8-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>

<span data-ttu-id="ec5b8-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ec5b8-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ec5b8-106">En este vídeo, Chris Pels muestra cómo mantener una página en la caché de resultados de ASP.NET basándose en la información del encabezado HTTP de la página.</span><span class="sxs-lookup"><span data-stu-id="ec5b8-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="ec5b8-107">En primer lugar, se revisan los posibles valores de encabezado HTTP.</span><span class="sxs-lookup"><span data-stu-id="ec5b8-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="ec5b8-108">A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "Accept-Language", un encabezado HTTP, para controlar el almacenamiento en caché en función del idioma del explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="ec5b8-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="ec5b8-109">La página de ejemplo se ve en Internet Explorer, que se establece en inglés y, a continuación, en FireFox, que se establece para usar francés.</span><span class="sxs-lookup"><span data-stu-id="ec5b8-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="ec5b8-110">Por último, se describe la opción para trasladar la definición de caché a un CacheProfile en el archivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="ec5b8-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="ec5b8-111">&#9654;Ver vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="ec5b8-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
