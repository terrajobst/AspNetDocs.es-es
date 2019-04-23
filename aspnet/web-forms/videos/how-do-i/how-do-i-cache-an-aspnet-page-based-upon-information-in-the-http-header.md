---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[¿Cómo lo hago?:]  Caché de una página ASP.NET en función de la información en el encabezado HTTP | Microsoft Docs'
author: rick-anderson
description: En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET en función de la información de encabezado HTTP de la página. En primer lugar, el potencial encabe HTTP...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: 79c27f39793a4a3a94ea412838fb3844579e874d
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59404149"
---
# <a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a>[¿Cómo lo hago?:]  Caché de una página ASP.NET en función de la información en el encabezado HTTP

por [Chris Pels](https://twitter.com/chrispels)

En este vídeo Chris Pels muestra cómo mantener una página en la caché de resultados ASP.NET en función de la información de encabezado HTTP de la página. En primer lugar, se revisan los posibles valores del encabezado HTTP. A continuación, se crea una página de ejemplo y, a continuación, se usa la directiva OutputCache con el atributo VaryByHeader que contiene un valor "accept-language", un encabezado HTTP, para controlar el almacenamiento en caché según el idioma del explorador del usuario. La página de ejemplo se ve en Internet Explorer que se establece en inglés y, a continuación, en FireFox que está configurado para utilizar a francés. Por último, se describe la opción para mover la definición de la memoria caché para un CacheProfile en el archivo web.config.

[&#9654;Vea el vídeo (12 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
