---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupación y Minificar de recursos en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: microsoft
description: La Unión y minificación son maneras de agilizar su sitio. La agrupación permite combinar varios archivos de JavaScript (. js) o varias hojas de estilos en cascada (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516181"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a38c9-104">Agrupar y minificar recursos en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a38c9-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="a38c9-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a38c9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a38c9-106">La Unión y minificación son maneras de agilizar su sitio.</span><span class="sxs-lookup"><span data-stu-id="a38c9-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="a38c9-107">La agrupación permite combinar varios archivos de JavaScript ( *. js*) o varios archivos de hojas de estilos en cascada ( *. CSS*) para que se puedan descargar como una unidad, en lugar de de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="a38c9-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="a38c9-108">Minificación comprime el espacio en blanco y realiza otros tipos de compresión para que los archivos descargados sean lo más pequeños posible.</span><span class="sxs-lookup"><span data-stu-id="a38c9-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="a38c9-109">La versión RC de ASP.NET Web Pages 2 no admite la agrupación y la minificación porque el paquete que contiene los elementos necesarios todavía no está disponible en Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="a38c9-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="a38c9-110">Sentimos las molestias.</span><span class="sxs-lookup"><span data-stu-id="a38c9-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="a38c9-111">Se espera que el paquete esté disponible en la versión final de ASP.NET Web Pages 2 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="a38c9-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
