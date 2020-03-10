---
uid: webhooks/receiving/handlers
title: Controladores de webhooks de ASP.NET | Microsoft Docs
author: rick-anderson
description: Cómo administrar las solicitudes en webhooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518035"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="a0a65-103">Controladores de webhooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a0a65-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="a0a65-104">Una vez que un receptor de webhook ha validado las solicitudes de webhooks, están listas para ser procesadas por el código de usuario.</span><span class="sxs-lookup"><span data-stu-id="a0a65-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="a0a65-105">Aquí es donde se incorporan los *Controladores* .</span><span class="sxs-lookup"><span data-stu-id="a0a65-105">This is where *handlers* come in.</span></span> <span data-ttu-id="a0a65-106">Los controladores derivan de la interfaz [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) pero normalmente usan la clase [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) en lugar de derivar directamente de la interfaz.</span><span class="sxs-lookup"><span data-stu-id="a0a65-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="a0a65-107">Una solicitud de webhook se puede procesar mediante uno o varios controladores.</span><span class="sxs-lookup"><span data-stu-id="a0a65-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="a0a65-108">Se llama a los controladores en orden en función de su propiedad *Order* respectiva, pasando de menor a mayor, donde Order es un entero simple (se sugiere que esté entre 1 y 100):</span><span class="sxs-lookup"><span data-stu-id="a0a65-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propiedades de orden de controlador de webhook](_static/Handlers.png)

<span data-ttu-id="a0a65-110">Un controlador puede establecer opcionalmente la propiedad *Response* en WebHookHandlerContext, lo que hará que se detenga el procesamiento y se devuelva la respuesta como respuesta HTTP al webhook.</span><span class="sxs-lookup"><span data-stu-id="a0a65-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="a0a65-111">En el caso anterior, no se llamará al controlador C porque tiene un orden superior al de B y B establece la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a0a65-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="a0a65-112">La configuración de la respuesta normalmente solo es pertinente para los webhooks en los que la respuesta puede llevar información a la API de origen.</span><span class="sxs-lookup"><span data-stu-id="a0a65-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="a0a65-113">Esto es así, por ejemplo, el caso con webhooks de demora donde la respuesta se devuelve al canal del que procede el webhook.</span><span class="sxs-lookup"><span data-stu-id="a0a65-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="a0a65-114">Los controladores pueden establecer la propiedad del receptor si solo quieren recibir webhooks de ese receptor determinado.</span><span class="sxs-lookup"><span data-stu-id="a0a65-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="a0a65-115">Si no establecen el receptor, se les llama para todos ellos.</span><span class="sxs-lookup"><span data-stu-id="a0a65-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="a0a65-116">Otro uso común de una respuesta es usar una respuesta 410 que no se ha *pasado* para indicar que el webhook ya no está activo y no se deben enviar más solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a0a65-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="a0a65-117">De forma predeterminada, todos los receptores de webhook llamarán a un controlador.</span><span class="sxs-lookup"><span data-stu-id="a0a65-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="a0a65-118">Sin embargo, si la propiedad *Receiver* está establecida en el nombre de un controlador, ese controlador solo recibirá las solicitudes de webhook de ese receptor.</span><span class="sxs-lookup"><span data-stu-id="a0a65-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="a0a65-119">Procesamiento de un webhook</span><span class="sxs-lookup"><span data-stu-id="a0a65-119">Processing a WebHook</span></span>

<span data-ttu-id="a0a65-120">Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de webhook.</span><span class="sxs-lookup"><span data-stu-id="a0a65-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="a0a65-121">Los datos, normalmente el cuerpo de la solicitud HTTP, están disponibles en la propiedad de *datos* .</span><span class="sxs-lookup"><span data-stu-id="a0a65-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="a0a65-122">El tipo de los datos es normalmente datos de formulario JSON o HTML, pero se puede convertir a un tipo más específico si se desea.</span><span class="sxs-lookup"><span data-stu-id="a0a65-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="a0a65-123">Por ejemplo, los webhooks personalizados generados por webhooks de ASP.NET se pueden convertir al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="a0a65-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="a0a65-124">Procesamiento en cola</span><span class="sxs-lookup"><span data-stu-id="a0a65-124">Queued Processing</span></span>

<span data-ttu-id="a0a65-125">La mayoría de los remitentes de webhook volverán a enviar un webhook si no se genera una respuesta en unos pocos segundos.</span><span class="sxs-lookup"><span data-stu-id="a0a65-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="a0a65-126">Esto significa que el controlador debe completar el procesamiento dentro de ese intervalo de tiempo para que no se vuelva a llamar.</span><span class="sxs-lookup"><span data-stu-id="a0a65-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="a0a65-127">Si el procesamiento tarda más o se controla mejor por separado, el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) se puede usar para enviar la solicitud de webhook a una cola, por ejemplo [Azure Storage cola](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="a0a65-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="a0a65-128">Aquí se proporciona un esquema de una implementación de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) :</span><span class="sxs-lookup"><span data-stu-id="a0a65-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
