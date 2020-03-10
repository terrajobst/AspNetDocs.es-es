---
uid: signalr/overview/performance/scaleout-in-signalr
title: Introducción a ampliación en Signalr | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14dc22f99a43b566903c59fb23b7d419350f4a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467785"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Introducción a la escalabilidad horizontal en SignalR

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

En general, hay dos formas de escalar una aplicación web: *escalar* verticalmente y *escalar*horizontalmente.

- Escalar verticalmente significa usar un servidor mayor (o una máquina virtual más grande) con más RAM, CPU, etc.
- Escalar horizontalmente significa agregar más servidores para controlar la carga.

El problema con el escalado vertical es que se alcanza rápidamente un límite en el tamaño de la máquina. Más allá de eso, debe escalar horizontalmente. Sin embargo, cuando se escala horizontalmente, los clientes se pueden enrutar a diferentes servidores. Un cliente que está conectado a un servidor no recibirá los mensajes enviados desde otro servidor.

![](scaleout-in-signalr/_static/image1.png)

Una solución consiste en reenviar mensajes entre servidores mediante un componente denominado *backplane*. Con un backplane habilitado, cada instancia de aplicación envía mensajes al plano posterior y el plano posterior los reenvía a las otras instancias de la aplicación. (En electrónica, un backplane es un grupo de conectores paralelos. De forma análoga, un backplane Signalr conecta varios servidores).

![](scaleout-in-signalr/_static/image2.png)

Signalr proporciona actualmente tres Backplanes:

- **Azure Service Bus**. Service Bus es una infraestructura de mensajería que permite a los componentes enviar mensajes de una manera débilmente acoplada.
- **Redis**. Redis es un almacén de clave-valor en memoria. Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.
- **SQL Server**. El backplane SQL Server escribe mensajes en las tablas SQL. El backplane usa Service Broker para la mensajería eficaz. Sin embargo, también funciona si Service Broker no está habilitado.

Si implementa su aplicación en Azure, considere la posibilidad de usar el backplane de Redis mediante [Azure Redis cache](https://azure.microsoft.com/services/cache/). Si va a realizar la implementación en su propia granja de servidores, tenga en cuenta los planes de SQL Server o de Redis.

Los temas siguientes contienen tutoriales paso a paso para cada backplane:

- [Escalabilidad horizontal de SignalR con Azure Service Bus](scaleout-with-windows-azure-service-bus.md)
- [Escalabilidad horizontal de SignalR con Redis](scaleout-with-redis.md)
- [Escalabilidad horizontal de SignalR con SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementación

En Signalr, cada mensaje se envía a través de un bus de mensajes. Un bus de mensajes implementa la interfaz [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , que proporciona una abstracción de publicación/suscripción. Los Backplanes funcionan reemplazando el valor predeterminado de **IMessageBus** por un bus diseñado para ese backplane. Por ejemplo, el bus de mensajes para Redis es [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)y usa el mecanismo [pub/sub](http://redis.io/topics/pubsub) de Redis para enviar y recibir mensajes.

Cada instancia de servidor se conecta al plano posterior a través del bus. Cuando se envía un mensaje, se dirige al backplane y el plano posterior lo envía a todos los servidores. Cuando un servidor recibe un mensaje del plano posterior, coloca el mensaje en su caché local. A continuación, el servidor entrega mensajes a los clientes desde su caché local.

Para cada conexión de cliente, se realiza un seguimiento del progreso del cliente en la lectura de la secuencia de mensajes mediante un cursor. (Un cursor representa una posición en el flujo de mensajes). Si un cliente se desconecta y se vuelve a conectar, pide al bus los mensajes que llegaron después del valor del cursor del cliente. Lo mismo sucede cuando una conexión utiliza un [sondeo largo](../getting-started/introduction-to-signalr.md#transports). Una vez completada una solicitud de sondeo largo, el cliente abre una nueva conexión y solicita los mensajes que llegaron después del cursor.

El mecanismo de cursor funciona aunque un cliente se enrute a un servidor diferente en la reconexión. El backplane es consciente de todos los servidores y no importa a qué servidor se conecta un cliente.

## <a name="limitations"></a>Limitaciones

Mediante el uso de un backplane, el rendimiento máximo de los mensajes es inferior al de los clientes que se comunican directamente con un solo nodo de servidor. Esto se debe a que el backplane reenvía cada mensaje a cada nodo, por lo que el plano posterior puede convertirse en un cuello de botella. El hecho de que esta limitación sea un problema depende de la aplicación. Por ejemplo, a continuación se muestran algunos escenarios típicos de Signalr:

- [Difusión del servidor](../getting-started/tutorial-server-broadcast-with-signalr.md) (por ejemplo, el tablero de cotizaciones): los Backplanes funcionan bien en este escenario, ya que el servidor controla la velocidad a la que se envían los mensajes.
- [Cliente a cliente](../getting-started/tutorial-getting-started-with-signalr.md) (por ejemplo, chat): en este escenario, el backplane podría ser un cuello de botella si el número de mensajes se escala con el número de clientes; es decir, si la tasa de mensajes aumenta proporcionalmente a medida que se unen más clientes.
- [Alta frecuencia en tiempo real](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): no se recomienda un backplane para este escenario.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitación del seguimiento de Signalr ampliación

Para habilitar el seguimiento de los Backplanes, agregue las siguientes secciones al archivo Web. config, en el elemento de **configuración** raíz:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
