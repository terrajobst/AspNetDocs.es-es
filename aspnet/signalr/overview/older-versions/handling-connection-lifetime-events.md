---
uid: signalr/overview/older-versions/handling-connection-lifetime-events
title: Descripción y control de eventos de duración de la conexión en Signalr 1. x | Microsoft Docs
author: bradygaster
description: En este artículo se describe cómo usar los eventos expuestos por la API de hubs.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: e608e263-264d-448b-b0eb-6eeb77713b22
msc.legacyurl: /signalr/overview/older-versions/handling-connection-lifetime-events
msc.type: authoredcontent
ms.openlocfilehash: 2fb671e730a1d41c07b350bf1d64ac1d0b1be55c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431503"
---
# <a name="understanding-and-handling-connection-lifetime-events-in-signalr-1x"></a>Descripción y control de eventos de duración de la conexión en Signalr 1. x

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se proporciona información general sobre la conexión de Signalr, la reconexión y los eventos de desconexión que se pueden controlar, y el tiempo de espera y la configuración de KeepAlive que se pueden configurar.
> 
> En este artículo se da por supuesto que ya tiene cierto conocimiento de eventos Signalr y duración de la conexión. Para obtener una introducción a Signalr, consulte [signalr-Overview-introducción](index.md). Para obtener una lista de los eventos de duración de la conexión, consulte los siguientes recursos:
> 
> - [Cómo controlar los eventos de duración de la conexión en la clase hub](index.md)
> - [Cómo controlar los eventos de duración de la conexión en los clientes de JavaScript](index.md)
> - [Cómo controlar los eventos de duración de la conexión en los clientes de .NET](index.md)

## <a name="overview"></a>Información general

Este artículo contiene las siguientes secciones:

- [Terminología y escenarios de la duración de la conexión](#terminology)

    - [Conexiones de signalr, conexiones de transporte y conexiones físicas](#signalrvstransport)
    - [Escenarios de desconexión de transporte](#transportdisconnect)
    - [Escenarios de desconexión de cliente](#clientdisconnect)
    - [Escenarios de desconexión del servidor](#serverdisconnect)
- [Configuración de tiempo de espera y keepalive](#timeoutkeepalive)

    - [ConnectionTimeout](#connectiontimeout)
    - [DisconnectTimeout](#disconnecttimeout)
    - [KeepAlive](#keepalive)
    - [Cómo cambiar la configuración de tiempo de espera y keepalive](#changetimeout)
- [Cómo notificar a los usuarios las desconexiones](#notifydisconnect)
- [Cómo volver a conectar continuamente](#continuousreconnect)
- [Cómo desconectar un cliente en el código del servidor](#disconnectclientfromserver)

Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API. Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="terminology"></a>

## <a name="connection-lifetime-terminology-and-scenarios"></a>Terminología y escenarios de la duración de la conexión

El controlador de eventos `OnReconnected` de un concentrador Signalr puede ejecutarse directamente después de `OnConnected` pero no después de `OnDisconnected` para un cliente determinado. La razón por la que puede tener una reconexión sin una desconexión es que hay varias maneras en las que se usa la palabra "conexión" en Signalr.

<a id="signalrvstransport"></a>

### <a name="signalr-connections-transport-connections-and-physical-connections"></a>Conexiones de signalr, conexiones de transporte y conexiones físicas

En este artículo se diferenciarán las conexiones de *signalr*, *las conexiones de transporte*y *las conexiones físicas*:

- **Signalr Connection** hace referencia a una relación lógica entre un cliente y una dirección URL del servidor, que mantiene la API de signalr y que se identifica de forma única mediante un identificador de conexión. Signalr mantiene los datos sobre esta relación y se utiliza para establecer una conexión de transporte. La relación finaliza y el señalizador desecha los datos cuando el cliente llama al método `Stop` o se alcanza un límite de tiempo de espera mientras Signalr está intentando volver a establecer una conexión de transporte perdida.
- La **conexión de transporte** hace referencia a una relación lógica entre un cliente y un servidor, que se mantiene en una de las cuatro API de transporte: WebSockets, eventos enviados por el servidor, tiempo suficiente o sondeo largo. Signalr usa la API de transporte para crear una conexión de transporte, y la API de transporte depende de la existencia de una conexión de red física para crear la conexión de transporte. La conexión de transporte finaliza cuando Signalr la termina o cuando la API de transporte detecta que se ha interrumpido la conexión física.
- **Conexión física** hace referencia a los vínculos de red física: hilos, señales inalámbricas, enrutadores, etc., que facilitan la comunicación entre un equipo cliente y un equipo servidor. La conexión física debe estar presente con el fin de establecer una conexión de transporte, y se debe establecer una conexión de transporte para establecer una conexión Signalr. Sin embargo, interrumpir la conexión física no siempre finaliza inmediatamente la conexión de transporte o la conexión de Signalr, como se explicará más adelante en este tema.

En el diagrama siguiente, la conexión Signalr está representada por la capa hubs API y PersistentConnection API Signalr, la conexión de transporte está representada por la capa de transportes y la conexión física se representa mediante las líneas entre el servidor y los clientes de.

![Diagrama de arquitectura de signalr](handling-connection-lifetime-events/_static/image1.png)

Cuando se llama al método `Start` en un cliente de Signalr, se proporciona código de cliente de Signalr con toda la información necesaria para establecer una conexión física con un servidor. El código de cliente de signalr usa esta información para crear una solicitud HTTP y establecer una conexión física que usa uno de los cuatro métodos de transporte. Si se produce un error en la conexión de transporte o se produce un error en el servidor, la conexión de Signalr no desaparecerá inmediatamente porque el cliente todavía tiene la información necesaria para volver a establecer automáticamente una nueva conexión de transporte a la misma dirección URL de Signalr. En este escenario, no interviene la intervención de la aplicación de usuario y, cuando el código de cliente de Signalr establece una nueva conexión de transporte, no inicia una nueva conexión de Signalr. La continuidad de la conexión Signalr se refleja en el hecho de que el identificador de conexión, que se crea cuando se llama al método `Start`, no cambia.

El controlador de eventos `OnReconnected` en el concentrador se ejecuta cuando se restablece automáticamente una conexión de transporte después de haberse perdido. El controlador de eventos `OnDisconnected` se ejecuta al final de una conexión Signalr. Una conexión Signalr puede finalizar de cualquiera de las siguientes maneras:

- Si el cliente llama al método `Stop`, se envía un mensaje de detención al servidor y el cliente y el servidor finalizan la conexión de Signalr inmediatamente.
- Una vez que se pierde la conectividad entre el cliente y el servidor, el cliente intenta volver a conectarse y el servidor espera a que el cliente vuelva a conectarse. Si los intentos de reconexión no son correctos y el tiempo de espera de la desconexión finaliza, el cliente y el servidor finalizan la conexión de Signalr. El cliente deja de intentar la reconexión y el servidor desecha su representación de la conexión de Signalr.
- Si el cliente deja de ejecutarse sin tener la oportunidad de llamar al método `Stop`, el servidor espera a que el cliente vuelva a conectarse y, a continuación, finaliza la conexión de Signalr después del tiempo de espera de la desconexión.
- Si el servidor deja de ejecutarse, el cliente intenta volver a conectarse (vuelve a crear la conexión de transporte) y, a continuación, finaliza la conexión de Signalr después del tiempo de espera de la desconexión.

Cuando no hay ningún problema de conexión y la aplicación de usuario finaliza la conexión de Signalr llamando al método `Stop`, la conexión de Signalr y la conexión de transporte comienzan y terminan aproximadamente al mismo tiempo. En las secciones siguientes se describen con más detalle los otros escenarios.

<a id="transportdisconnect"></a>

### <a name="transport-disconnection-scenarios"></a>Escenarios de desconexión de transporte

Las conexiones físicas pueden ser lentas o podría haber interrupciones en la conectividad. Dependiendo de factores como la duración de la interrupción, es posible que se quite la conexión de transporte. Signalr intenta volver a establecer la conexión de transporte. A veces, la API de conexión de transporte detecta la interrupción y quita la conexión de transporte, y Signalr averigua inmediatamente que se ha perdido la conexión. En otros escenarios, ni la API de conexión de transporte ni Signalr se reconocen inmediatamente que se ha perdido la conectividad. En el caso de todos los transportes excepto el sondeo largo, el cliente de Signalr usa una función llamada *KeepAlive* para comprobar la pérdida de conectividad que la API de transporte no puede detectar. Para obtener información acerca de las conexiones de sondeo prolongado, vea [configuración de tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

Cuando una conexión está inactiva, el servidor envía periódicamente un paquete keepalive al cliente. A partir de la fecha en que se escribe este artículo, la frecuencia predeterminada es cada 10 segundos. Al escuchar estos paquetes, los clientes pueden saber si hay un problema de conexión. Si no se recibe un paquete keepalive cuando se espera, el cliente supone que hay problemas de conexión, como lentitud o interrupciones, después de un breve período de tiempo. Si el keepalive todavía no se recibe después de un tiempo más largo, el cliente asume que se ha quitado la conexión y comienza a intentar la reconexión.

En el diagrama siguiente se muestran los eventos de cliente y de servidor que se producen en un escenario típico cuando hay problemas con la conexión física que la API de transporte no reconoce inmediatamente. El diagrama se aplica a las siguientes circunstancias:

- El transporte es WebSockets, Forever o eventos enviados por el servidor.
- Hay distintos períodos de interrupción en la conexión de red física.
- La API de transporte no es consciente de las interrupciones, por lo que Signalr se basa en la funcionalidad keepalive para detectarlas.

![Desconexiones de transporte](handling-connection-lifetime-events/_static/image2.png)

Si el cliente entra en el modo de reconexión, pero no puede establecer una conexión de transporte dentro del límite de tiempo de espera de la desconexión, el servidor finaliza la conexión de Signalr. Cuando esto sucede, el servidor ejecuta el método de `OnDisconnected` del concentrador y pone en cola un mensaje de desconexión para enviar al cliente en caso de que el cliente administre la conexión más tarde. Si el cliente se vuelve a conectar, recibe el comando Disconnect y llama al método `Stop`. En este escenario, `OnReconnected` no se ejecuta cuando el cliente se vuelve a conectar y `OnDisconnected` no se ejecuta cuando el cliente llama a `Stop`. En el siguiente diagrama se ilustra este escenario.

![Interrupciones de transporte: tiempo de espera del servidor](handling-connection-lifetime-events/_static/image3.png)

Los eventos de duración de conexión de Signalr que se pueden producir en el cliente son los siguientes:

- `ConnectionSlow` evento de cliente.

    Se genera cuando se ha pasado una proporción preestablecida del tiempo de espera de KeepAlive desde que se recibió el último mensaje o el ping keepalive. El período de advertencia predeterminado de tiempo de espera de KeepAlive es 2/3 del tiempo de espera de KeepAlive. El tiempo de espera de KeepAlive es de 20 segundos, por lo que la advertencia se produce en unos 13 segundos.

    De forma predeterminada, el servidor envía pings de keepalive cada 10 segundos y el cliente comprueba los pings de keepalive cada 2 segundos (un tercio de la diferencia entre el valor de tiempo de espera de KeepAlive y el valor de advertencia de tiempo de espera de KeepAlive).

    Si la API de transporte es consciente de una desconexión, es posible que Signalr esté informado de la desconexión antes de que transcurra el período de advertencia de tiempo de espera de KeepAlive. En ese caso, no se produciría el evento `ConnectionSlow`, y Signalr irá directamente al evento `Reconnecting`.
- `Reconnecting` evento de cliente.

    Se genera cuando (a) la API de transporte detecta que se ha perdido la conexión o (b) el tiempo de espera de KeepAlive ha transcurrido desde que se recibió el último mensaje o el ping de KeepAlive. El código de cliente de Signalr comienza a intentar la reconexión. Puede controlar este evento si desea que la aplicación realice alguna acción cuando se pierde una conexión de transporte. El período predeterminado de tiempo de espera de KeepAlive es actualmente de 20 segundos.

    Si el código de cliente intenta llamar a un método de concentrador mientras Signalr está en modo de reconexión, Signalr intentará enviar el comando. En la mayoría de los casos, se producirá un error en estos intentos, pero en algunas circunstancias podrían realizarse correctamente. En el caso de los eventos enviados por el servidor, el marco Forever y los transportes de sondeo largos, Signalr usa dos canales de comunicación, uno que el cliente usa para enviar mensajes y otro que usa para recibir mensajes. El canal que se usa para recibir es el que se abre permanentemente y es el que se cierra cuando se interrumpe la conexión física. El canal que se usa para el envío sigue estando disponible, por lo que si se restaura la conectividad física, una llamada de método desde el cliente al servidor podría ser correcta antes de que se vuelva a establecer el canal de recepción. El valor devuelto no se recibirá hasta que Signalr vuelva a abrir el canal usado para recibir.
- `Reconnected` evento de cliente.

    Se genera cuando se restablece la conexión de transporte. Se ejecuta el controlador de eventos `OnReconnected` en el concentrador.
- `Closed` evento de cliente (evento de`disconnected` en JavaScript).

    Se genera cuando expira el tiempo de espera de la desconexión mientras el código de cliente de Signalr intenta volver a conectarse después de perder la conexión de transporte. El tiempo de espera de desconexión predeterminado es de 30 segundos. (Este evento también se genera cuando finaliza la conexión porque se llama al método `Stop`).

Las interrupciones de la conexión de transporte no detectadas por la API de transporte y no retrasan la recepción de ping de KeepAlive desde el servidor durante más tiempo que el período de advertencia de tiempo de espera de KeepAlive podría no provocar que se generen eventos de duración de la conexión.

Algunos entornos de red cierran deliberadamente las conexiones inactivas, y otra función de los paquetes keepalive es ayudar a evitar esto permitiendo que estas redes sepan que se está usando una conexión Signalr. En casos extremos, la frecuencia predeterminada de los pings keepalive podría no ser suficiente para evitar conexiones cerradas. En ese caso, puede configurar los pings keepalive para que se envíen con más frecuencia. Para obtener más información, vea [configuración de tiempo de espera y keepalive](#timeoutkeepalive) más adelante en este tema.

> [!NOTE] 
> 
> [!IMPORTANT]
> No se garantiza la secuencia de eventos que se describen aquí. Signalr realiza cada intento de generar eventos de duración de la conexión de forma predecible según este esquema, pero hay muchas variaciones de eventos de red y muchas maneras en las que los marcos de comunicaciones subyacentes, como las API de transporte, las controlan. Por ejemplo, el evento `Reconnected` podría no generarse cuando el cliente se vuelve a conectar o el controlador de `OnConnected` en el servidor puede ejecutarse cuando el intento de establecer una conexión no se realiza correctamente. En este tema solo se describen los efectos que normalmente se producirían en determinadas circunstancias típicas.

<a id="clientdisconnect"></a>

### <a name="client-disconnection-scenarios"></a>Escenarios de desconexión de cliente

En un cliente del explorador, el código de cliente de Signalr que mantiene una conexión de Signalr se ejecuta en el contexto de JavaScript de una página web. Este es el motivo por el que la conexión de Signalr tiene que finalizar al navegar de una página a otra, y eso es el motivo por el que tiene varias conexiones con varios identificadores de conexión si se conecta desde varias ventanas o pestañas del explorador. Cuando el usuario cierra una pestaña o ventana del explorador, o navega a una nueva página o actualiza la página, la conexión de Signalr finaliza inmediatamente porque el código de cliente de Signalr controla ese evento de explorador automáticamente y llama al método `Stop`. En estos escenarios, o en cualquier plataforma de cliente cuando la aplicación llama al método `Stop`, el controlador de eventos `OnDisconnected` se ejecuta inmediatamente en el servidor y el cliente genera el evento `Closed` (el evento se denomina `disconnected` en JavaScript).

Si una aplicación cliente o el equipo en el que se ejecuta se bloquea o entra en suspensión (por ejemplo, cuando el usuario cierra el equipo portátil), no se informa al servidor de lo que ha sucedido. En lo que se refiere al servidor, la pérdida del cliente podría deberse a una interrupción de la conectividad y el cliente podría estar intentando volver a conectarse. Por lo tanto, en estos casos, el servidor espera a proporcionar al cliente una oportunidad de volver a conectarse y `OnDisconnected` no se ejecuta hasta que expire el tiempo de espera de la desconexión (de forma predeterminada, 30 segundos). En el siguiente diagrama se ilustra este escenario.

![Error del equipo cliente](handling-connection-lifetime-events/_static/image4.png)

<a id="serverdisconnect"></a>

### <a name="server-disconnection-scenarios"></a>Escenarios de desconexión del servidor

Cuando un servidor se queda sin conexión, se reinicia, se produce un error, se recicla el dominio de la aplicación, etc.--el resultado puede ser similar a una conexión perdida, o la API de transporte y Signalr podrían saber inmediatamente que el servidor ha desaparecido, y es posible que Signalr intente volver a conectarse sin generar el evento `ConnectionSlow`. Si el cliente entra en el modo de reconexión y el servidor se recupera o se reinicia o se pone en línea un nuevo servidor antes de que expire el tiempo de espera de la desconexión, el cliente se volverá a conectar al servidor restaurado o nuevo. En ese caso, la conexión de Signalr continúa en el cliente y se genera el evento `Reconnected`. En el primer servidor, nunca se ejecuta `OnDisconnected` y, en el nuevo servidor, se ejecuta `OnReconnected` aunque nunca se ejecutó `OnConnected` para ese cliente en ese servidor antes. (El efecto es el mismo si el cliente se vuelve a conectar al mismo servidor después de un reinicio o un reciclaje de dominio de aplicación, porque cuando el servidor se reinicia no tiene memoria de actividad de conexión anterior). En el diagrama siguiente se da por supuesto que la API de transporte es consciente de la conexión perdida inmediatamente, por lo que no se genera el evento `ConnectionSlow`.

![Error y reconexión del servidor](handling-connection-lifetime-events/_static/image5.png)

Si un servidor no está disponible dentro del período de tiempo de espera de la desconexión, la conexión de Signalr finaliza. En este escenario, el evento de `Closed` (`disconnected` en los clientes de JavaScript) se genera en el cliente, pero nunca se llama a `OnDisconnected` en el servidor. En el diagrama siguiente se da por supuesto que la API de transporte no es consciente de la conexión perdida, por lo que se detecta mediante la funcionalidad keepalive de Signalr y se genera el evento `ConnectionSlow`.

![Error y tiempo de espera del servidor](handling-connection-lifetime-events/_static/image6.png)

<a id="timeoutkeepalive"></a>

## <a name="timeout-and-keepalive-settings"></a>Configuración de tiempo de espera y keepalive

Los valores predeterminados `ConnectionTimeout`, `DisconnectTimeout`y `KeepAlive` son adecuados para la mayoría de los escenarios, pero se pueden cambiar si su entorno tiene necesidades especiales. Por ejemplo, si el entorno de red cierra conexiones que están inactivas durante 5 segundos, es posible que tenga que reducir el valor de KeepAlive.

<a id="connectiontimeout"></a>

### <a name="connectiontimeout"></a>ConnectionTimeout

Esta configuración representa la cantidad de tiempo que se debe dejar abierta una conexión de transporte y esperar una respuesta antes de cerrarla y abrir una nueva conexión. El valor predeterminado es 110 segundos.

Esta configuración solo se aplica cuando la funcionalidad keepalive está deshabilitada, lo que normalmente se aplica solo al transporte de sondeo prolongado. En el diagrama siguiente se ilustra el efecto de esta configuración en una conexión de transporte de sondeo prolongado.

![Conexión de transporte de sondeo largo](handling-connection-lifetime-events/_static/image7.png)

<a id="disconnecttimeout"></a>

### <a name="disconnecttimeout"></a>DisconnectTimeout

Este valor representa la cantidad de tiempo que se esperará después de que se pierda una conexión de transporte antes de generar el evento `Disconnected`. El valor predeterminado es 30 segundos. Al establecer `DisconnectTimeout`, `KeepAlive` se establece automáticamente en 1/3 del valor de `DisconnectTimeout`.

<a id="keepalive"></a>

### <a name="keepalive"></a>KeepAlive

Esta configuración representa la cantidad de tiempo que hay que esperar antes de enviar un paquete keepalive a través de una conexión inactiva. El valor predeterminado es 10 segundos. Este valor no debe ser superior a 1/3 del valor de `DisconnectTimeout`.

Si desea establecer `DisconnectTimeout` y `KeepAlive`, establezca `KeepAlive` después de `DisconnectTimeout`. De lo contrario, se sobrescribirá el valor `KeepAlive` cuando `DisconnectTimeout` establezca automáticamente `KeepAlive` en 1/3 del valor de tiempo de espera.

Si desea deshabilitar la funcionalidad keepalive, establezca `KeepAlive` en NULL. La funcionalidad keepalive se deshabilita automáticamente para el transporte de sondeo prolongado.

<a id="changetimeout"></a>

### <a name="how-to-change-timeout-and-keepalive-settings"></a>Cómo cambiar la configuración de tiempo de espera y keepalive

Para cambiar los valores predeterminados de estos valores, establézcalo en `Application_Start` en el archivo *global. asax* , tal como se muestra en el ejemplo siguiente. Los valores que se muestran en el código de ejemplo son los mismos que los valores predeterminados.

[!code-csharp[Main](handling-connection-lifetime-events/samples/sample1.cs)]

<a id="notifydisconnect"></a>

## <a name="how-to-notify-the-user-about-disconnections"></a>Cómo notificar a los usuarios las desconexiones

En algunas aplicaciones, puede que desee mostrar un mensaje al usuario cuando hay problemas de conectividad. Tiene varias opciones sobre cómo y cuándo hacerlo. Los siguientes ejemplos de código son para un cliente de JavaScript que usa el proxy generado.

- Controle el evento `connectionSlow` para mostrar un mensaje tan pronto como Signalr tenga en cuenta los problemas de conexión, antes de que pase al modo de reconexión.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample2.js)]
- Controle el evento `reconnecting` para mostrar un mensaje cuando Signalr sea consciente de una desconexión y pase al modo de reconexión.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample3.js)]
- Controle el evento `disconnected` para mostrar un mensaje cuando se agote el tiempo de espera de un intento de reconexión. En este escenario, la única manera de volver a establecer una conexión con el servidor es reiniciar la conexión de Signalr llamando al método `Start`, que creará un nuevo identificador de conexión. En el ejemplo de código siguiente se usa una marca para asegurarse de que se emite la notificación solo después de un tiempo de espera de reconexión, no después de un fin normal a la conexión de Signalr que se produce al llamar al método `Stop`.

    [!code-javascript[Main](handling-connection-lifetime-events/samples/sample4.js)]

<a id="continuousreconnect"></a>

## <a name="how-to-continuously-reconnect"></a>Cómo volver a conectar continuamente

En algunas aplicaciones, es posible que desee volver a establecer automáticamente una conexión después de que se haya perdido y que se haya agotado el tiempo de espera para el intento de reconexión. Para ello, puede llamar al método `Start` desde el controlador de eventos de `Closed` (`disconnected` controlador de eventos en los clientes de JavaScript). Es posible que desee esperar un período de tiempo antes de llamar a `Start` para evitar hacerlo con demasiada frecuencia cuando el servidor o la conexión física no estén disponibles. El siguiente ejemplo de código es para un cliente de JavaScript que usa el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample5.js)]

Un problema potencial que se debe tener en cuenta en los clientes móviles es que los intentos de reconexión continua cuando el servidor o la conexión física no están disponibles pueden provocar una purga de la batería innecesaria.

<a id="disconnectclientfromserver"></a>

## <a name="how-to-disconnect-a-client-in-server-code"></a>Cómo desconectar un cliente en el código del servidor

Signalr versión 1.1.1 no tiene una API de servidor integrada para desconectar a los clientes. Hay [planes para agregar esta funcionalidad en el futuro](https://github.com/SignalR/SignalR/issues/2101). En la versión actual de Signalr, la manera más sencilla de desconectar un cliente del servidor consiste en implementar un método de desconexión en el cliente y llamar a ese método desde el servidor. En el ejemplo de código siguiente se muestra un método de desconexión para un cliente de JavaScript que usa el proxy generado.

[!code-javascript[Main](handling-connection-lifetime-events/samples/sample6.js)]

> [!WARNING]
> Seguridad: ninguno de los métodos de desconexión de los clientes ni de la API integrada propuesta abordará el escenario de clientes con piratería que ejecutan código malintencionado, ya que los clientes pueden volver a conectarse, o el código pirata podría quitar el método `stopClient` o cambiar lo que hace. El lugar adecuado para implementar la protección de denegación de servicio (DOS) con estado no está en el marco de trabajo ni en el nivel de servidor, sino en la infraestructura de front-end.
