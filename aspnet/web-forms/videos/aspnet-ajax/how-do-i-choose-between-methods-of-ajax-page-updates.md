---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Cómo:] ¿Elegir entre métodos de actualizaciones de páginas de AJAX? | Microsoft Docs'
author: JoeStagner
description: En este vídeo, Joe Stagner compara los dos métodos principales para realizar actualizaciones de páginas de estilo AJAX en una aplicación ASP.NET. El primer método es usar un...
ms.author: riande
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 56e3ebfbe0b5af4234791136725de79e38171cc1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78420157"
---
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[Cómo:] ¿Elegir entre métodos de actualizaciones de páginas de AJAX?

por [Joe Stagner](https://github.com/JoeStagner)

En este vídeo, Joe Stagner compara los dos métodos principales para realizar actualizaciones de páginas de estilo AJAX en una aplicación ASP.NET. El primer método es usar un UpdatePanel, donde no es necesario escribir código adicional en el lado cliente o en el servidor. La ventaja de usar UpdatePanel es que todo funciona automáticamente. La penalización es que, en el cliente, requiere una gran cantidad de datos que se van a incluir en la solicitud y respuesta de AJAX, y en el servidor requiere que se ejecute un ciclo de vida completo de la página. El segundo método consiste en usar devoluciones de llamada de red, donde es necesario escribir código adicional en el lado cliente y en el servidor. La ventaja de usar devoluciones de llamada de red es que en el cliente se requieren muy pocos datos para incluirse en la solicitud y respuesta de AJAX, y en el servidor solo requiere que se ejecute el método de servicio llamado. La penalización es el tiempo y el esfuerzo que se tarda en escribir el código necesario. Joe concluye el vídeo analizando lo que debe tener en cuenta al elegir entre los dos métodos principales de las actualizaciones de páginas de estilo AJAX. (Este vídeo usa el código del vídeo [How Do i Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video y [How Do i Client-Side devoluciones de llamada de red con ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) ).

[&#9654;Ver vídeo (11 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Anterior](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Siguiente](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
