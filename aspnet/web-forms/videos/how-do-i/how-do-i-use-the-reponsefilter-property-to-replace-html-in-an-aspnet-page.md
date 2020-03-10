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
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="a47a7-104">[Cómo:] Usar la propiedad rerespuesta. Filter para reemplazar HTML en una página de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a47a7-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="a47a7-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="a47a7-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="a47a7-106">En este vídeo, Chris Pels muestra cómo usar la propiedad rerespuesta. Filter para interceptar y modificar el código HTML que se envía a una página.</span><span class="sxs-lookup"><span data-stu-id="a47a7-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="a47a7-107">En primer lugar, se crea una página de ejemplo con texto simple.</span><span class="sxs-lookup"><span data-stu-id="a47a7-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="a47a7-108">A continuación, se crea una clase de secuencia personalizada que actúa como el flujo de reemplazo para la secuencia actual que se envía al explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="a47a7-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="a47a7-109">En esa clase de secuencia personalizada, el contenido de la página se recupera de la secuencia, se modifica y, a continuación, se escribe en la secuencia de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a47a7-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="a47a7-110">En esta clase de secuencia personalizada, el método de escritura está personalizado para reemplazar el código HTML en la secuencia de respuesta base, lo que altera lo que se envía al explorador del usuario.</span><span class="sxs-lookup"><span data-stu-id="a47a7-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="a47a7-111">Por último, la nueva clase Stream se asigna a la propiedad Response. Filter en la página\_evento Load, lo que proporciona el mecanismo para modificar el contenido de la página.</span><span class="sxs-lookup"><span data-stu-id="a47a7-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="a47a7-112">&#9654;Ver vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="a47a7-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
