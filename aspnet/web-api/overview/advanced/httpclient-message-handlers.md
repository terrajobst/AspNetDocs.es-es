---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes HttpClient en ASP.NET Web API - ASP.NET 4.x
author: MikeWasson
description: Crear controladores de mensajes personalizados para ASP.NET Web API en ASP.NET 4.x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: bd52396064cd7007ee17705ba86b02aaf27cb4f0
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59401730"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="a991e-103">Controladores de mensajes HttpClient en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a991e-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="a991e-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a991e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a991e-105">Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a991e-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="a991e-106">Normalmente, una serie de controladores de mensajes están conectados entre sí.</span><span class="sxs-lookup"><span data-stu-id="a991e-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="a991e-107">El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador.</span><span class="sxs-lookup"><span data-stu-id="a991e-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="a991e-108">En algún momento, la respuesta se crea y se vuelve a la cadena.</span><span class="sxs-lookup"><span data-stu-id="a991e-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="a991e-109">Este patrón se denomina un *delegar* controlador.</span><span class="sxs-lookup"><span data-stu-id="a991e-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="a991e-110">En el lado cliente, el **HttpClient** clase utiliza un controlador de mensajes para procesar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a991e-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="a991e-111">El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y se obtiene la respuesta del servidor.</span><span class="sxs-lookup"><span data-stu-id="a991e-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="a991e-112">Puede insertar controladores de mensajes personalizados en la canalización de cliente:</span><span class="sxs-lookup"><span data-stu-id="a991e-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="a991e-113">ASP.NET Web API también usa controladores de mensajes en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a991e-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="a991e-114">Para obtener más información, consulte [controladores de mensajes HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="a991e-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="a991e-115">Controladores de mensajes personalizado</span><span class="sxs-lookup"><span data-stu-id="a991e-115">Custom Message Handlers</span></span>

<span data-ttu-id="a991e-116">Para escribir un controlador de mensajes personalizado, que se derivan de **System.Net.Http.DelegatingHandler** e invalidar la **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="a991e-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="a991e-117">Esta es la firma de método:</span><span class="sxs-lookup"><span data-stu-id="a991e-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="a991e-118">El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="a991e-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="a991e-119">Una implementación típica hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a991e-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="a991e-120">Procesar el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="a991e-120">Process the request message.</span></span>
2. <span data-ttu-id="a991e-121">Llame a `base.SendAsync` para enviar la solicitud al controlador interno.</span><span class="sxs-lookup"><span data-stu-id="a991e-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="a991e-122">El controlador interno devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a991e-122">The inner handler returns a response message.</span></span> <span data-ttu-id="a991e-123">(Este paso es asíncrono).</span><span class="sxs-lookup"><span data-stu-id="a991e-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="a991e-124">Procesar la respuesta y devolverlo al autor de llamada.</span><span class="sxs-lookup"><span data-stu-id="a991e-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="a991e-125">El ejemplo siguiente muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud de salida:</span><span class="sxs-lookup"><span data-stu-id="a991e-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="a991e-126">La llamada a `base.SendAsync` es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="a991e-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="a991e-127">Si el controlador no hace ningún trabajo después de esta llamada, use la **await** palabra clave para reanudar la ejecución después de completarse el método.</span><span class="sxs-lookup"><span data-stu-id="a991e-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="a991e-128">El ejemplo siguiente muestra un controlador que registra los códigos de error.</span><span class="sxs-lookup"><span data-stu-id="a991e-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="a991e-129">El registro en sí mismo no es muy interesante, pero el ejemplo muestra cómo obtener la respuesta dentro del controlador.</span><span class="sxs-lookup"><span data-stu-id="a991e-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="a991e-130">Agregar controladores de mensajes a la canalización de cliente</span><span class="sxs-lookup"><span data-stu-id="a991e-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="a991e-131">Para agregar controladores personalizados para **HttpClient**, utilice el **HttpClientFactory.Create** método:</span><span class="sxs-lookup"><span data-stu-id="a991e-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="a991e-132">Se denominan controladores de mensajes en el orden en que se pasan en el **Create** método.</span><span class="sxs-lookup"><span data-stu-id="a991e-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="a991e-133">Porque los controladores están anidados, transmite el mensaje de respuesta en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="a991e-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="a991e-134">Es decir, el último controlador es el primero en recibir el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a991e-134">That is, the last handler is the first to get the response message.</span></span>
