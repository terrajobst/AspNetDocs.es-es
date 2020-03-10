---
uid: signalr/overview/getting-started/introduction-to-signalr
title: Introducción a Signalr | Microsoft Docs
author: bradygaster
description: En este artículo se describe qué es Signalr y algunas de las soluciones que se diseñaron para su creación.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 0fab5e35-8c1f-43d4-8635-b8aba8766a71
msc.legacyurl: /signalr/overview/getting-started/introduction-to-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8dbc31a5c8d59fa55dc5b513c1a51d24d18a685f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431101"
---
# <a name="introduction-to-signalr"></a>Introducción a SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe qué es Signalr y algunas de las soluciones que se diseñaron para su creación. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](https://stackoverflow.com/questions/tagged/signalr).

## <a name="what-is-signalr"></a>¿Qué es Signalr?

ASP.NET Signalr es una biblioteca para desarrolladores de ASP.NET que simplifica el proceso de agregar funcionalidad web en tiempo real a las aplicaciones. La funcionalidad web en tiempo real es la posibilidad de que el código de servidor Inserte el contenido en los clientes conectados al instante a medida que esté disponible, en lugar de que el servidor espere a que un cliente solicite datos nuevos.

Signalr se puede usar para agregar cualquier tipo de funcionalidad web "en tiempo real" a la aplicación ASP.NET. Aunque el chat se usa a menudo como ejemplo, puede hacer mucho más. Cada vez que un usuario actualiza una página web para ver los nuevos datos o la página implementa un [sondeo prolongado](http://en.wikipedia.org/wiki/Push_technology#Long_polling) para recuperar nuevos datos, es un candidato para usar signalr. Algunos ejemplos son los paneles y las aplicaciones de supervisión, las aplicaciones de colaboración (por ejemplo, la edición simultánea de documentos), las actualizaciones del progreso del trabajo y los formularios en tiempo real.

Signalr también permite nuevos tipos de aplicaciones web que requieren actualizaciones de alta frecuencia del servidor, por ejemplo, juegos en tiempo real.

Signalr proporciona una API sencilla para crear llamadas a procedimiento remoto (RPC) de servidor a cliente que llaman a funciones de JavaScript en exploradores cliente (y otras plataformas cliente) desde código .NET del lado servidor. Signalr también incluye API para la administración de conexiones (por ejemplo, eventos de conexión y desconexión) y agrupar conexiones.

![Invocar métodos con Signalr](introduction-to-signalr/_static/image1.png)

Signalr controla la administración de conexiones automáticamente y le permite difundir mensajes a todos los clientes conectados simultáneamente, como un salón de chat. También puede enviar mensajes a clientes específicos. La conexión entre el cliente y el servidor es persistente, a diferencia de una conexión HTTP clásica, que se restablece para cada comunicación.

Signalr admite la funcionalidad de "instalación de servidor", en la que el código de servidor puede llamar al código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC), en lugar del modelo de solicitud-respuesta común en la web en la actualidad.

Las aplicaciones de signalr pueden escalarse horizontalmente a miles de clientes mediante proveedores de escalado horizontal integrados y de terceros.

Los proveedores integrados incluyen:
* [Service Bus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3)
* [SQL Server](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
* [Redis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.Redis)

Los proveedores de terceros incluyen:
* [NCache](https://www.alachisoft.com/ncache/asp-net-core-signalr.html).

Signalr es de código abierto, accesible a través de [GitHub](https://github.com/signalr).

## <a name="signalr-and-websocket"></a>Signalr y WebSocket

Signalr usa el nuevo transporte de WebSocket cuando está disponible y recurre a los transportes más antiguos cuando sea necesario. Aunque podría escribir la aplicación con WebSocket directamente, el uso de Signalr significa que una gran parte de la funcionalidad adicional que tendría que implementar ya se realiza automáticamente. Lo que es más importante, esto significa que puede codificar la aplicación para aprovechar las ventajas de WebSocket sin tener que preocuparse por crear una ruta de acceso de código independiente para clientes más antiguos. Signalr también evita tener que preocuparse de las actualizaciones de WebSocket, ya que Signalr se actualiza para admitir los cambios en el transporte subyacente, lo que proporciona a la aplicación una interfaz coherente entre las distintas versiones de WebSocket.

<a id="transports"></a>

## <a name="transports-and-fallbacks"></a>Transportes y reservas

Signalr es una abstracción en algunos de los transportes necesarios para realizar el trabajo en tiempo real entre el cliente y el servidor. Una conexión Signalr se inicia como HTTP y, después, se promueve a una conexión WebSocket si está disponible. WebSocket es el transporte ideal para Signalr, ya que hace el uso más eficaz de la memoria del servidor, tiene la latencia más baja y tiene las características más subyacentes (por ejemplo, comunicación dúplex completa entre el cliente y el servidor), pero también tiene el más estricto requisitos: WebSocket requiere que el servidor use Windows Server 2012 o Windows 8 y .NET Framework 4,5. Si no se cumplen estos requisitos, Signalr intentará usar otros transportes para realizar sus conexiones.

### <a name="html-5-transports"></a>Transportes HTML 5

Estos transportes dependen de la compatibilidad con [HTML 5](http://en.wikipedia.org/wiki/HTML5). Si el explorador del cliente no admite el estándar HTML 5, se utilizarán transportes antiguos.

- **WebSocket** (si el servidor y el explorador indican que pueden admitir WebSocket). WebSocket es el único transporte que establece una verdadera conexión persistente y bidireccional entre el cliente y el servidor. Sin embargo, WebSocket también tiene los requisitos más estrictos; es totalmente compatible solo en las versiones más recientes de Microsoft Internet Explorer, Google Chrome y Mozilla Firefox, y solo tiene una implementación parcial en otros exploradores, como opera y Safari.
- **Los eventos enviados**por el servidor, también conocidos como EventSource (si el explorador admite eventos enviados del servidor, que básicamente son todos los exploradores, excepto Internet Explorer).

### <a name="comet-transports"></a>Comet (transportes)

Los transportes siguientes se basan en el modelo de aplicación Web [Comet](http://en.wikipedia.org/wiki/Comet_(programming)) , en el que un explorador u otro cliente mantiene una solicitud HTTP de larga duración, que el servidor puede usar para insertar datos en el cliente sin que el cliente lo solicite específicamente.

- **Siempre el marco** (solo para Internet Explorer). El marco Forever crea un IFrame oculto que realiza una solicitud a un extremo en el servidor que no se completa. A continuación, el servidor envía continuamente el script al cliente que se ejecuta inmediatamente, lo que proporciona una conexión en tiempo real unidireccional entre el servidor y el cliente. La conexión del cliente al servidor utiliza una conexión independiente entre la conexión entre el servidor y el cliente, y como una solicitud HTTP estándar, se crea una nueva conexión para cada fragmento de datos que se debe enviar.
- **Sondeo largo de Ajax**. El sondeo prolongado no crea una conexión persistente, sino que sondea el servidor con una solicitud que permanece abierta hasta que el servidor responde, momento en el que se cierra la conexión, y se solicita una nueva conexión inmediatamente. Esto puede introducir cierta latencia mientras se restablece la conexión.

Para obtener más información sobre qué transportes se admiten con qué configuraciones, consulte [plataformas compatibles](supported-platforms.md).

### <a name="transport-selection-process"></a>Proceso de selección de transporte

En la lista siguiente se muestran los pasos que Signalr usa para decidir qué transporte usar.

1. Si el explorador es Internet Explorer 8 o una versión anterior, se usa el sondeo largo.
2. Si JSONP está configurado (es decir, el parámetro `jsonp` se establece en `true` cuando se inicia la conexión), se usa el sondeo largo.
3. Si se realiza una conexión entre dominios (es decir, si el punto de conexión de Signalr no está en el mismo dominio que la página de hospedaje), se usará WebSocket si se cumplen los siguientes criterios:

   - El cliente es compatible con CORS (uso compartido de recursos entre orígenes). Para más información sobre qué clientes admiten CORS, consulte [CORS en Caniuse.com](http://www.caniuse.com/CORS).
   - El cliente es compatible con WebSocket
   - El servidor admite WebSocket

     Si no se cumple alguno de estos criterios, se usará un sondeo largo. Para obtener más información sobre las conexiones entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).
4. Si JSONP no está configurado y la conexión no es entre dominios, se usará WebSocket si tanto el cliente como el servidor lo admiten.
5. Si el cliente o el servidor no admiten WebSocket, se usarán los eventos enviados del servidor si están disponibles.
6. Si los eventos enviados del servidor no están disponibles, se intentará siempre el marco.
7. Si siempre se produce un error en el marco, se usa un sondeo largo.

<a id="MonitoringTransports"></a>
### <a name="monitoring-transports"></a>Supervisar transportes

Puede determinar qué transporte usa la aplicación habilitando el registro en el centro y abriendo la ventana de la consola en el explorador.

Para habilitar el registro de los eventos de su concentrador en un explorador, agregue el siguiente comando a la aplicación cliente:

`$.connection.hub.logging = true;`

- En Internet Explorer, presione F12 para abrir las herramientas de desarrollo y haga clic en la pestaña consola.

    ![Consola de en Microsoft Internet Explorer](introduction-to-signalr/_static/image2.png)
- En Chrome, abra la consola presionando Ctrl + Mayús + J.

    ![Consola de Google Chrome](introduction-to-signalr/_static/image3.png)

Con la consola abierta y el registro habilitado, podrá ver qué transporte está utilizando Signalr.

![Consola de Internet Explorer que muestra el transporte de WebSocket](introduction-to-signalr/_static/image4.png)

### <a name="specifying-a-transport"></a>Especificación de un transporte

La negociación de un transporte requiere cierto tiempo y recursos de cliente/servidor. Si se conocen las capacidades del cliente, se puede especificar un transporte cuando se inicia la conexión del cliente. El siguiente fragmento de código muestra cómo iniciar una conexión mediante el transporte de sondeo largo de Ajax, como se utilizaría si se sabe que el cliente no admitía ningún otro protocolo:

`connection.start({ transport: 'longPolling' });`

Puede especificar un orden de reserva si desea que un cliente pruebe transportes específicos en orden. En el fragmento de código siguiente se muestra cómo probar WebSocket y, por lo tanto, se produce un error, pasando directamente a un sondeo largo.

`connection.start({ transport: ['webSockets','longPolling'] });`

Las constantes de cadena para especificar transportes se definen de la siguiente manera:

- `webSockets`
- `foreverFrame`
- `serverSentEvents`
- `longPolling`

## <a name="connections-and-hubs"></a>Conexiones y concentradores

Signalr API contiene dos modelos para la comunicación entre clientes y servidores: conexiones y concentradores persistentes.

Una conexión representa un extremo simple para el envío de mensajes de un solo destinatario, agrupados o de difusión. La API de conexión persistente (representada en el código de .NET por la clase PersistentConnection) proporciona al desarrollador acceso directo al Protocolo de comunicación de bajo nivel que Signalr expone. El uso del modelo de comunicación de conexiones resultará familiar a los desarrolladores que hayan usado API basadas en conexión como Windows Communication Foundation.

Un concentrador es una canalización de alto nivel que se basa en la API de conexión que permite que el cliente y el servidor llamen a métodos entre sí directamente. Signalr controla la distribución a través de los límites de la máquina como si se requiera, lo que permite a los clientes llamar a métodos en el servidor con la misma facilidad que los métodos locales y viceversa. El uso del modelo de comunicación de hubs le resultará familiar a los desarrolladores que hayan usado API de invocación remota como .NET Remoting. El uso de un concentrador también le permite pasar parámetros fuertemente tipados a métodos, habilitando el enlace de modelos.

### <a name="architecture-diagram"></a>Diagrama de la arquitectura

En el diagrama siguiente se muestra la relación entre los concentradores, las conexiones persistentes y las tecnologías subyacentes que se usan para los transportes.

![Diagrama de arquitectura de signalr que muestra API, transportes y clientes](introduction-to-signalr/_static/image5.png)

### <a name="how-hubs-work"></a>Cómo funcionan los concentradores

Cuando el código del lado servidor llama a un método en el cliente, se envía un paquete a través del transporte activo que contiene el nombre y los parámetros del método al que se va a llamar (cuando se envía un objeto como un parámetro de método, se serializa mediante JSON). Después, el cliente hace coincidir el nombre del método con los métodos definidos en el código del lado cliente. Si hay una coincidencia, el método de cliente se ejecutará con los datos del parámetro deserializado.

La llamada al método se puede supervisar mediante herramientas como [Fiddler.](http://fiddler2.com/) En la imagen siguiente se muestra una llamada de método enviada desde un servidor de Signalr a un cliente de explorador Web en el panel de registros de Fiddler. La llamada al método se envía desde un centro denominado `MoveShapeHub`y el método que se invoca se llama `updateShape`.

![Vista del registro de Fiddler que muestra el tráfico de Signalr](introduction-to-signalr/_static/image6.png)

En este ejemplo, el nombre del concentrador se identifica con el parámetro `H`; el nombre del método se identifica con el parámetro `M`, y los datos que se envían al método se identifican con el parámetro `A`. La aplicación que generó este mensaje se crea en el tutorial en [tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) .

### <a name="choosing-a-communication-model"></a>Elegir un modelo de comunicación

La mayoría de las aplicaciones deben usar la API de hubs. La API de conexiones se podría usar en las siguientes circunstancias:

- Es necesario especificar el formato del mensaje real enviado.
- El desarrollador prefiere trabajar con un modelo de mensajería y distribución en lugar de un modelo de invocación remoto.
- Una aplicación existente que utiliza un modelo de mensajería se traslada a usar Signalr.
