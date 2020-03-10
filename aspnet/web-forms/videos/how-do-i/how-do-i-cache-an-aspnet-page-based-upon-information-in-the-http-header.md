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
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[Cómo:]  Almacenar en caché una página de ASP.NET basada en la información del encabezado HTTP

por [Chris Pels](https://twitter.com/chrispels)

En este vídeo, Chris Pels muestra cómo mantener una página en la caché de resultados de ASP.NET basándose en la información del encabezado HTTP de la página. En primer lugar, se revisan los posibles valores de encabezado HTTP. A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "Accept-Language", un encabezado HTTP, para controlar el almacenamiento en caché en función del idioma del explorador del usuario. La página de ejemplo se ve en Internet Explorer, que se establece en inglés y, a continuación, en FireFox, que se establece para usar francés. Por último, se describe la opción para trasladar la definición de caché a un CacheProfile en el archivo Web. config.

[&#9654;Ver vídeo (12 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
