---
ms.openlocfilehash: 93bda587eebc438e5da36b07cb7e4a37df8a91eb
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57062722"
---
# <a name="aspnet-core-response-caching-sample"></a><span data-ttu-id="b6522-101">Ejemplo de almacenamiento en caché de respuesta de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b6522-101">ASP.NET Core Response Caching Sample</span></span>

<span data-ttu-id="b6522-102">Este ejemplo ilustra el uso de ASP.NET Core [Middleware de almacenamiento en caché de respuestas](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="b6522-102">This sample illustrates the usage of ASP.NET Core [Response Caching Middleware](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).</span></span>

<span data-ttu-id="b6522-103">La aplicación responde con su página de índice, incluido un `Cache-Control` encabezado para configurar el comportamiento de almacenamiento en caché.</span><span class="sxs-lookup"><span data-stu-id="b6522-103">The app responds with its Index page, including a `Cache-Control` header to configure caching behavior.</span></span> <span data-ttu-id="b6522-104">La aplicación también establece la `Vary` encabezado para configurar la memoria caché para dar servicio a la respuesta solo si el `Accept-Encoding` encabezado de las solicitudes posteriores coincide con el de la solicitud original.</span><span class="sxs-lookup"><span data-stu-id="b6522-104">The app also sets the `Vary` header to configure the cache to serve the response only if the `Accept-Encoding` header of subsequent requests matches that from the original request.</span></span>

<span data-ttu-id="b6522-105">Al ejecutar el ejemplo, la página de índice se sirve desde la memoria caché cuando se almacenan y se almacena en caché durante 10 segundos.</span><span class="sxs-lookup"><span data-stu-id="b6522-105">When running the sample, the Index page is served from cache when stored and cached for up to 10 seconds.</span></span>
