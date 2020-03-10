---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Cómo:] ¿Usar el UpdateMode condicional de UpdatePanel? | Microsoft Docs'
author: JoeStagner
description: El UpdatePanel de AJAX de ASP.NET incluye una propiedad UpdateMode que se puede establecer en ' Always ' o ' Conditional '. El valor predeterminado es siempre, en cuyo caso el UpdatePan...
ms.author: riande
ms.date: 08/01/2007
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: c05d4f262d56dfba858443b830d72ff0520b65d7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510127"
---
# <a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="18e7c-105">[Cómo:] ¿Usar el UpdateMode condicional de UpdatePanel?</span><span class="sxs-lookup"><span data-stu-id="18e7c-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>

<span data-ttu-id="18e7c-106">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="18e7c-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="18e7c-107">El UpdatePanel de AJAX de ASP.NET incluye una propiedad UpdateMode que se puede establecer en ' Always ' o ' Conditional '.</span><span class="sxs-lookup"><span data-stu-id="18e7c-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="18e7c-108">El valor predeterminado es siempre, en cuyo caso el UpdatePanel siempre actualizará su contenido durante un postback asincrónico.</span><span class="sxs-lookup"><span data-stu-id="18e7c-108">The default is Always, in which case the UpdatePanel will always update its content during an asynchronous postback.</span></span> <span data-ttu-id="18e7c-109">En este vídeo se explica cómo se puede establecer el valor de UpdateMode en Conditional, en cuyo caso el UpdatePanel solo actualizará su contenido cuando nuestro código de servidor llame a su método Update.</span><span class="sxs-lookup"><span data-stu-id="18e7c-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="18e7c-110">Esto le permite usar lógica condicional en el C# código o Visual Basic para determinar si UpdatePanel actualizará su contenido durante el PostBack asincrónico actual.</span><span class="sxs-lookup"><span data-stu-id="18e7c-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="18e7c-111">&#9654;Ver vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="18e7c-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="18e7c-112">[Anterior](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Siguiente](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="18e7c-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
