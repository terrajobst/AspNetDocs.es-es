---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Cómo:] Controlar el almacenamiento en caché de una página de ASP.NET en función de la información personalizada | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo controlar los criterios para almacenar en caché una página de ASP.NET en función de la información personalizada. Se crea una página de ejemplo y, a continuación, la
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506773"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="671a2-104">[Cómo:] Controlar el almacenamiento en caché de una página de ASP.NET en función de la información personalizada</span><span class="sxs-lookup"><span data-stu-id="671a2-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="671a2-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="671a2-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="671a2-106">En este vídeo, Chris Pels muestra cómo controlar los criterios para almacenar en caché una página de ASP.NET en función de la información personalizada.</span><span class="sxs-lookup"><span data-stu-id="671a2-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="671a2-107">Se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByCustom que contiene un valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="671a2-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="671a2-108">A continuación, se invalida el método GetVaryCustomByString () en el módulo global. asax, que proporciona el control del atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="671a2-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="671a2-109">En ese método se devuelve una cadena que identifica de forma única la versión almacenada en caché de la página.</span><span class="sxs-lookup"><span data-stu-id="671a2-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="671a2-110">Por último, hay una explicación sobre cómo se puede usar el almacenamiento en caché con un valor personalizado de varias maneras para un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="671a2-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="671a2-111">&#9654;Ver vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="671a2-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
