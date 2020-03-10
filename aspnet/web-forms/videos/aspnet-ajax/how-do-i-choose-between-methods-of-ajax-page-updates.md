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
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="d8406-105">[Cómo:] ¿Elegir entre métodos de actualizaciones de páginas de AJAX?</span><span class="sxs-lookup"><span data-stu-id="d8406-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>

<span data-ttu-id="d8406-106">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d8406-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="d8406-107">En este vídeo, Joe Stagner compara los dos métodos principales para realizar actualizaciones de páginas de estilo AJAX en una aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d8406-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="d8406-108">El primer método es usar un UpdatePanel, donde no es necesario escribir código adicional en el lado cliente o en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d8406-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="d8406-109">La ventaja de usar UpdatePanel es que todo funciona automáticamente.</span><span class="sxs-lookup"><span data-stu-id="d8406-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="d8406-110">La penalización es que, en el cliente, requiere una gran cantidad de datos que se van a incluir en la solicitud y respuesta de AJAX, y en el servidor requiere que se ejecute un ciclo de vida completo de la página.</span><span class="sxs-lookup"><span data-stu-id="d8406-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="d8406-111">El segundo método consiste en usar devoluciones de llamada de red, donde es necesario escribir código adicional en el lado cliente y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d8406-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="d8406-112">La ventaja de usar devoluciones de llamada de red es que en el cliente se requieren muy pocos datos para incluirse en la solicitud y respuesta de AJAX, y en el servidor solo requiere que se ejecute el método de servicio llamado.</span><span class="sxs-lookup"><span data-stu-id="d8406-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="d8406-113">La penalización es el tiempo y el esfuerzo que se tarda en escribir el código necesario.</span><span class="sxs-lookup"><span data-stu-id="d8406-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="d8406-114">Joe concluye el vídeo analizando lo que debe tener en cuenta al elegir entre los dos métodos principales de las actualizaciones de páginas de estilo AJAX.</span><span class="sxs-lookup"><span data-stu-id="d8406-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="d8406-115">(Este vídeo usa el código del vídeo [How Do i Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video y [How Do i Client-Side devoluciones de llamada de red con ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) ).</span><span class="sxs-lookup"><span data-stu-id="d8406-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="d8406-116">&#9654;Ver vídeo (11 minutos)</span><span class="sxs-lookup"><span data-stu-id="d8406-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="d8406-117">[Anterior](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Siguiente](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="d8406-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
