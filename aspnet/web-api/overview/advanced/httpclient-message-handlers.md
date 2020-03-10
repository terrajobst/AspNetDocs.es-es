---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Controladores de mensajes HttpClient en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Cree controladores de mensajes personalizados para ASP.NET Web API en ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449281"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="f38e2-103">Controladores de mensajes HttpClient en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f38e2-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="f38e2-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f38e2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f38e2-105">Un *controlador de mensajes* es una clase que recibe una solicitud HTTP y devuelve una respuesta http.</span><span class="sxs-lookup"><span data-stu-id="f38e2-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="f38e2-106">Normalmente, una serie de controladores de mensajes se encadenan entre sí.</span><span class="sxs-lookup"><span data-stu-id="f38e2-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="f38e2-107">El primer controlador recibe una solicitud HTTP, realiza algún procesamiento y proporciona la solicitud al siguiente controlador.</span><span class="sxs-lookup"><span data-stu-id="f38e2-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="f38e2-108">En algún momento, se crea la respuesta y se realiza una copia de seguridad de la cadena.</span><span class="sxs-lookup"><span data-stu-id="f38e2-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="f38e2-109">Este patrón se denomina controlador de *delegación* .</span><span class="sxs-lookup"><span data-stu-id="f38e2-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="f38e2-110">En el lado del cliente, la clase **HttpClient** usa un controlador de mensajes para procesar solicitudes.</span><span class="sxs-lookup"><span data-stu-id="f38e2-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="f38e2-111">El controlador predeterminado es **HttpClientHandler**, que envía la solicitud a través de la red y obtiene la respuesta del servidor.</span><span class="sxs-lookup"><span data-stu-id="f38e2-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="f38e2-112">Puede insertar controladores de mensajes personalizados en la canalización de cliente:</span><span class="sxs-lookup"><span data-stu-id="f38e2-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="f38e2-113">ASP.NET Web API también utiliza controladores de mensajes en el lado servidor.</span><span class="sxs-lookup"><span data-stu-id="f38e2-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="f38e2-114">Para obtener más información, consulte [controladores de mensajes http](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="f38e2-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="f38e2-115">Controladores de mensajes personalizados</span><span class="sxs-lookup"><span data-stu-id="f38e2-115">Custom Message Handlers</span></span>

<span data-ttu-id="f38e2-116">Para escribir un controlador de mensajes personalizado, derive de **System .net. http. DelegatingHandler** e invalide el método **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="f38e2-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="f38e2-117">Esta es la firma del método:</span><span class="sxs-lookup"><span data-stu-id="f38e2-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="f38e2-118">El método toma un **HttpRequestMessage** como entrada y devuelve de forma asincrónica un **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="f38e2-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="f38e2-119">Una implementación típica hace lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f38e2-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="f38e2-120">Procesar el mensaje de solicitud.</span><span class="sxs-lookup"><span data-stu-id="f38e2-120">Process the request message.</span></span>
2. <span data-ttu-id="f38e2-121">Llame a `base.SendAsync` para enviar la solicitud al controlador interno.</span><span class="sxs-lookup"><span data-stu-id="f38e2-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="f38e2-122">El controlador interno devuelve un mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="f38e2-122">The inner handler returns a response message.</span></span> <span data-ttu-id="f38e2-123">(Este paso es asincrónico).</span><span class="sxs-lookup"><span data-stu-id="f38e2-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="f38e2-124">Procesa la respuesta y la devuelve al autor de la llamada.</span><span class="sxs-lookup"><span data-stu-id="f38e2-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="f38e2-125">En el ejemplo siguiente se muestra un controlador de mensajes que agrega un encabezado personalizado a la solicitud saliente:</span><span class="sxs-lookup"><span data-stu-id="f38e2-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="f38e2-126">La llamada a `base.SendAsync` es asincrónica.</span><span class="sxs-lookup"><span data-stu-id="f38e2-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="f38e2-127">Si el controlador realiza cualquier trabajo después de esta llamada, use la palabra clave **Await** para reanudar la ejecución después de que se complete el método.</span><span class="sxs-lookup"><span data-stu-id="f38e2-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="f38e2-128">En el ejemplo siguiente se muestra un controlador que registra códigos de error.</span><span class="sxs-lookup"><span data-stu-id="f38e2-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="f38e2-129">El registro en sí no es muy interesante, pero en el ejemplo se muestra cómo obtener la respuesta dentro del controlador.</span><span class="sxs-lookup"><span data-stu-id="f38e2-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="f38e2-130">Agregar controladores de mensajes a la canalización de cliente</span><span class="sxs-lookup"><span data-stu-id="f38e2-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="f38e2-131">Para agregar controladores personalizados a **HttpClient**, use el método **HttpClientFactory. Create** :</span><span class="sxs-lookup"><span data-stu-id="f38e2-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="f38e2-132">Se llama a los controladores de mensajes en el orden en que se pasan al método **Create** .</span><span class="sxs-lookup"><span data-stu-id="f38e2-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="f38e2-133">Dado que los controladores están anidados, el mensaje de respuesta se desplaza en la otra dirección.</span><span class="sxs-lookup"><span data-stu-id="f38e2-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="f38e2-134">Es decir, el último controlador es el primero en obtener el mensaje de respuesta.</span><span class="sxs-lookup"><span data-stu-id="f38e2-134">That is, the last handler is the first to get the response message.</span></span>
