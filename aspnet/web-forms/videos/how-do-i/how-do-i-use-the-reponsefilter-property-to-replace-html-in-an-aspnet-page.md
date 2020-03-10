---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Cómo:] Usar la propiedad rerespuesta. Filter para reemplazar HTML en una página ASP.NET | Microsoft Docs'
author: rick-anderson
description: En este vídeo, Chris Pels muestra cómo usar la propiedad rerespuesta. Filter para interceptar y modificar el código HTML que se envía a una página. En primer lugar, se crea una página de ejemplo...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487999"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Cómo:] Usar la propiedad rerespuesta. Filter para reemplazar HTML en una página de ASP.NET

por [Chris Pels](https://twitter.com/chrispels)

En este vídeo, Chris Pels muestra cómo usar la propiedad rerespuesta. Filter para interceptar y modificar el código HTML que se envía a una página. En primer lugar, se crea una página de ejemplo con texto simple. A continuación, se crea una clase de secuencia personalizada que actúa como el flujo de reemplazo para la secuencia actual que se envía al explorador del usuario. En esa clase de secuencia personalizada, el contenido de la página se recupera de la secuencia, se modifica y, a continuación, se escribe en la secuencia de respuesta. En esta clase de secuencia personalizada, el método de escritura está personalizado para reemplazar el código HTML en la secuencia de respuesta base, lo que altera lo que se envía al explorador del usuario. Por último, la nueva clase Stream se asigna a la propiedad Response. Filter en la página\_evento Load, lo que proporciona el mecanismo para modificar el contenido de la página.

[&#9654;Ver vídeo (13 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
