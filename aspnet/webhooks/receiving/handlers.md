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
# <a name="aspnet-webhooks-handlers"></a>Controladores de webhooks de ASP.NET

Una vez que un receptor de webhook ha validado las solicitudes de webhooks, están listas para ser procesadas por el código de usuario. Aquí es donde se incorporan los *Controladores* . Los controladores derivan de la interfaz [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) pero normalmente usan la clase [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) en lugar de derivar directamente de la interfaz.

Una solicitud de webhook se puede procesar mediante uno o varios controladores. Se llama a los controladores en orden en función de su propiedad *Order* respectiva, pasando de menor a mayor, donde Order es un entero simple (se sugiere que esté entre 1 y 100):

![Diagrama de propiedades de orden de controlador de webhook](_static/Handlers.png)

Un controlador puede establecer opcionalmente la propiedad *Response* en WebHookHandlerContext, lo que hará que se detenga el procesamiento y se devuelva la respuesta como respuesta HTTP al webhook. En el caso anterior, no se llamará al controlador C porque tiene un orden superior al de B y B establece la respuesta.

La configuración de la respuesta normalmente solo es pertinente para los webhooks en los que la respuesta puede llevar información a la API de origen. Esto es así, por ejemplo, el caso con webhooks de demora donde la respuesta se devuelve al canal del que procede el webhook. Los controladores pueden establecer la propiedad del receptor si solo quieren recibir webhooks de ese receptor determinado. Si no establecen el receptor, se les llama para todos ellos.

Otro uso común de una respuesta es usar una respuesta 410 que no se ha *pasado* para indicar que el webhook ya no está activo y no se deben enviar más solicitudes.

De forma predeterminada, todos los receptores de webhook llamarán a un controlador. Sin embargo, si la propiedad *Receiver* está establecida en el nombre de un controlador, ese controlador solo recibirá las solicitudes de webhook de ese receptor.

## <a name="processing-a-webhook"></a>Procesamiento de un webhook

Cuando se llama a un controlador, obtiene un [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contiene información sobre la solicitud de webhook. Los datos, normalmente el cuerpo de la solicitud HTTP, están disponibles en la propiedad de *datos* .

El tipo de los datos es normalmente datos de formulario JSON o HTML, pero se puede convertir a un tipo más específico si se desea. Por ejemplo, los webhooks personalizados generados por webhooks de ASP.NET se pueden convertir al tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) como se indica a continuación:

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

  ## <a name="queued-processing"></a>Procesamiento en cola

La mayoría de los remitentes de webhook volverán a enviar un webhook si no se genera una respuesta en unos pocos segundos. Esto significa que el controlador debe completar el procesamiento dentro de ese intervalo de tiempo para que no se vuelva a llamar.

Si el procesamiento tarda más o se controla mejor por separado, el [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) se puede usar para enviar la solicitud de webhook a una cola, por ejemplo [Azure Storage cola](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Aquí se proporciona un esquema de una implementación de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) :

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
