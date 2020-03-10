---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Guía de la API de ASP.NET Signalr hubsC#-servidor () | Microsoft Docs
author: bradygaster
description: En este documento se proporciona una introducción a la programación del lado del servidor de ASP.NET Signalr hubs API para Signalr versión 2, con ejemplos de código que muestran...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c681b104b15bfc4a04587c7abf685dcf20def2ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431371"
---
# <a name="aspnet-signalr-hubs-api-guide---server-c"></a>Guía de la API de ASP.NET Signalr hubsC#-servidor ()

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se proporciona una introducción a la programación del lado del servidor de ASP.NET Signalr hubs API para Signalr versión 2, con ejemplos de código que muestran opciones comunes.
> 
> Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente. En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor. Signalr se encarga de todas las conexiones de cliente a servidor.
> 
> Signalr también ofrece una API de nivel inferior denominada conexiones persistentes. Para ver una introducción a Signalr, hubs y conexiones persistentes, consulte [Introducción a signalr 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Signalr versión 2
>   
> 
> 
> ## <a name="topic-versions"></a>Versiones de tema
> 
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Cómo registrar middleware Signalr](#route)

    - [La dirección URL de/signalr](#signalrurl)
    - [Configuración de las opciones de Signalr](#options)
- [Cómo crear y usar clases de Hub](#hubclass)

    - [Duración del objeto de concentrador](#transience)
    - [Grafía Camel de nombres de concentrador en clientes de JavaScript](#hubnames)
    - [Varios centros](#multiplehubs)
    - [Concentradores fuertemente tipados](#stronglytypedhubs)
- [Cómo definir métodos en la clase de concentrador a los que los clientes pueden llamar](#hubmethods)

    - [Grafía Camel de nombres de métodos en clientes de JavaScript](#methodnames)
    - [Cuándo ejecutar de forma asincrónica](#asyncmethods)
    - [Definir sobrecargas](#overloads)
    - [Informar del progreso de las invocaciones de método del concentrador](#progress)
- [Cómo llamar a métodos de cliente desde la clase hub](#callfromhub)

    - [Selección de los clientes que recibirán la RPC](#selectingclients)
    - [No hay validación en tiempo de compilación para los nombres de método](#dynamicmethodnames)
    - [Coincidencia de nombres de método que no distinguen mayúsculas de minúsculas](#caseinsensitive)
    - [Ejecución asincrónica](#asyncclient)
- [Cómo administrar la pertenencia a grupos desde la clase hub](#groupsfromhub)

    - [Ejecución asincrónica de métodos Add y Remove](#asyncgroupmethods)
    - [Persistencia de pertenencia a grupos](#grouppersistence)
    - [Grupos de usuario único](#singleusergroups)
- [Cómo controlar los eventos de duración de la conexión en la clase hub](#connectionlifetime)

    - [Cuando se llama a alconnected, OnDisconnection y OnReconnected](#onreconnected)
    - [Estado del autor de la llamada no rellenado](#nocallerstate)
- [Cómo obtener información sobre el cliente desde la propiedad de contexto](#contextproperty)
- [Cómo pasar el estado entre los clientes y la clase de concentrador](#passstate)
- [Cómo controlar errores en la clase hub](#handleErrors)
- [Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase hub](#callfromoutsidehub)

    - [Llamar a métodos de cliente](#callingclientsoutsidehub)
    - [Administrar la pertenencia a grupos](#managinggroupsoutsidehub)
- [Cómo habilitar el seguimiento](#tracing)
- [Personalización de la canalización de hubs](#hubpipeline)

Para obtener documentación sobre cómo programar clientes, consulte los siguientes recursos:

- [Guía de API de signalr hubs: cliente JavaScript](hubs-api-guide-javascript-client.md)
- [Guía de la API de signalr hubs: cliente .NET](hubs-api-guide-net-client.md)

Los componentes de servidor de Signalr 2 solo están disponibles en .NET 4,5. Los servidores que ejecutan .NET 4,0 deben usar Signalr v1. x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Cómo registrar middleware Signalr

Para definir la ruta que los clientes usarán para conectarse al centro, llame al método `MapSignalR` cuando se inicie la aplicación. `MapSignalR` es un [método de extensión](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) para la clase `OwinExtensions`. En el ejemplo siguiente se muestra cómo definir la ruta de los concentradores de Signalr mediante una clase de inicio OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

Si va a agregar la funcionalidad de Signalr a una aplicación de ASP.NET MVC, asegúrese de que la ruta de Signalr se agrega antes que las demás rutas. Para obtener más información, vea [Tutorial: introducción con signalr 2 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>La dirección URL de/signalr

De forma predeterminada, la dirección URL de ruta que los clientes usarán para conectarse al centro es "/signalr". (No confunda esta dirección URL con la dirección URL "/signalr/hubs", que es para el archivo JavaScript generado automáticamente. Para obtener más información sobre el proxy generado, consulte [Guía de API de signalr hubs: cliente JavaScript: el proxy generado y lo que hace para usted](hubs-api-guide-javascript-client.md#genproxy).

Puede haber circunstancias extraordinarias que hagan que esta dirección URL base no sea utilizable para Signalr. por ejemplo, si tiene una carpeta en el proyecto denominada *signalr* y no desea cambiar el nombre. En ese caso, puede cambiar la dirección URL base, como se muestra en los ejemplos siguientes (reemplace "/signalr" en el código de ejemplo por la dirección URL que desee).

**Código de servidor que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (con el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Código de cliente de JavaScript que especifica la dirección URL (sin el proxy generado)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Código de cliente .NET que especifica la dirección URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Configuración de las opciones de Signalr

Las sobrecargas del método `MapSignalR` permiten especificar una dirección URL personalizada, una resolución de dependencia personalizada y las siguientes opciones:

- Habilite las llamadas entre dominios mediante CORS o JSONP desde los clientes del explorador.

    Normalmente, si el explorador carga una página desde `http://contoso.com`, la conexión de Signalr está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` establece una conexión con `http://fabrikam.com/signalr`, es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para obtener más información, consulte Guía de la [API de ASP.net signalr hubs: cliente JavaScript: Cómo establecer una conexión entre dominios](hubs-api-guide-javascript-client.md#crossdomain).
- Habilitar mensajes de error detallados.

    Cuando se producen errores, el comportamiento predeterminado de Signalr es enviar a los clientes un mensaje de notificación sin detalles sobre lo que ha sucedido. No se recomienda enviar información de error detallada a los clientes en producción, ya que es posible que los usuarios malintencionados puedan usar la información en ataques contra la aplicación. Para solucionar problemas, puede usar esta opción para habilitar temporalmente informes de errores más informativos.
- Deshabilite los archivos de proxy de JavaScript generados automáticamente.

    De forma predeterminada, se genera un archivo JavaScript con servidores proxy para las clases de centro como respuesta a la dirección URL "/signalr/hubs". Si no desea usar los servidores proxy de JavaScript, o si desea generar este archivo manualmente y hacer referencia a un archivo físico en sus clientes, puede usar esta opción para deshabilitar la generación de proxy. Para obtener más información, consulte [Guía de API de signalr hubs: cliente JavaScript: Cómo crear un archivo físico para el proxy generado por signalr](hubs-api-guide-javascript-client.md#manualproxy).

En el ejemplo siguiente se muestra cómo especificar la dirección URL de conexión de Signalr y estas opciones en una llamada al método `MapSignalR`. Para especificar una dirección URL personalizada, reemplace "/signalr" en el ejemplo por la dirección URL que desea usar.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Cómo crear y usar clases de Hub

Para crear un concentrador, cree una clase que derive de [Microsoft. Aspnet. signalr. Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). En el ejemplo siguiente se muestra una clase de concentrador simple para una aplicación de chat.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

En este ejemplo, un cliente conectado puede llamar al método `NewContosoChatMessage` y, cuando lo hace, los datos recibidos se difunden a todos los clientes conectados.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Duración del objeto de concentrador

No se crea una instancia de la clase hub ni se llama a sus métodos desde su propio código en el servidor; todo lo que hace la canalización de hubs de Signalr. Signalr crea una instancia nueva de la clase hub cada vez que necesita controlar una operación de concentrador, como cuando un cliente se conecta, desconecta o realiza una llamada al método en el servidor.

Dado que las instancias de la clase hub son transitorias, no puede usarlas para mantener el estado de una llamada de método a la siguiente. Cada vez que el servidor recibe una llamada al método desde un cliente, una nueva instancia de la clase del centro procesa el mensaje. Para mantener el estado a través de varias conexiones y llamadas a métodos, use algún otro método, como una base de datos, una variable estática en la clase hub o una clase diferente que no se derive de `Hub`. Si conserva los datos en memoria, mediante un método como una variable estática en la clase hub, los datos se perderán cuando se recicle el dominio de aplicación.

Si desea enviar mensajes a los clientes desde su propio código que se ejecuta fuera de la clase de concentrador, no puede crear una instancia de la clase de concentrador, pero puede hacerlo obteniendo una referencia al objeto de contexto de Signalr para la clase de centro. Para obtener más información, consulte [llamar a métodos de cliente y administrar grupos desde fuera de la clase de concentrador](#callfromoutsidehub) más adelante en este tema.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Grafía Camel de nombres de concentrador en clientes de JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a los concentradores mediante una versión con grafía Camel del nombre de clase. Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript. El ejemplo anterior se denominaría `contosoChatHub` en el código de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Si desea especificar un nombre diferente para que lo usen los clientes, agregue el atributo `HubName`. Cuando se usa un atributo `HubName`, no se cambia el nombre a mayúsculas y minúsculas Camel en los clientes de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Varios centros

Puede definir varias clases de concentrador en una aplicación. Al hacerlo, la conexión se comparte pero los grupos son independientes:

- Todos los clientes usarán la misma dirección URL para establecer una conexión Signalr con su servicio ("/signalr" o su dirección URL personalizada si especificó una) y esa conexión se utiliza para todos los concentradores definidos por el servicio.

    No hay ninguna diferencia de rendimiento para varios concentradores en comparación con la definición de todas las funciones del concentrador en una sola clase.
- Todos los concentradores obtienen la misma información de solicitud HTTP.

    Dado que todos los concentradores comparten la misma conexión, la única información de la solicitud HTTP que obtiene el servidor es lo que se incluye en la solicitud HTTP original que establece la conexión de Signalr. Si usa la solicitud de conexión para pasar información desde el cliente al servidor mediante la especificación de una cadena de consulta, no puede proporcionar cadenas de consulta diferentes a diferentes concentradores. Todos los concentradores recibirán la misma información.
- El archivo de proxies de JavaScript generado contendrá servidores proxy para todos los concentradores en un archivo.

    Para obtener información sobre los servidores proxy de JavaScript, consulte [Guía de API de signalr hubs: cliente JavaScript: el proxy generado y lo que hace para usted](hubs-api-guide-javascript-client.md#genproxy).
- Los grupos se definen dentro de los concentradores.

    En Signalr puede definir grupos con nombre para difundirlos a subconjuntos de clientes conectados. Los grupos se mantienen por separado para cada concentrador. Por ejemplo, un grupo denominado "administradores" incluiría un conjunto de clientes para la clase `ContosoChatHub` y el mismo nombre de grupo haría referencia a un conjunto de clientes diferente para la clase `StockTickerHub`.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Concentradores fuertemente tipados

Para definir una interfaz para los métodos de concentrador a los que puede hacer referencia el cliente (y habilitar IntelliSense en los métodos de concentrador), derive su centro de `Hub<T>` (introducido en Signalr 2,1) en lugar de `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Cómo definir métodos en la clase de concentrador a los que los clientes pueden llamar

Para exponer un método en el concentrador al que desea llamar desde el cliente, declare un método público, tal como se muestra en los ejemplos siguientes.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Puede especificar un tipo de valor devuelto y parámetros, incluidos tipos complejos y matrices, como haría en cualquier C# método. Los datos que se reciben en parámetros o se devuelven al autor de la llamada se comunican entre el cliente y el servidor mediante JSON, y Signalr controla el enlace de objetos complejos y matrices de objetos de forma automática.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Grafía Camel de nombres de métodos en clientes de JavaScript

De forma predeterminada, los clientes de JavaScript hacen referencia a métodos de concentrador mediante una versión con mayúsculas y minúsculas Camel del nombre del método. Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Si desea especificar un nombre diferente para que lo usen los clientes, agregue el atributo `HubMethodName`.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Cuándo ejecutar de forma asincrónica

Si el método va a ser de ejecución prolongada o tiene que hacer un trabajo que implique la espera, como una búsqueda en la base de datos o una llamada de servicio Web, haga que el método del concentrador sea asincrónico devolviendo una [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (en lugar de `void` Return) o una [tarea&lt;t&gt;](https://msdn.microsoft.com/library/dd321424.aspx) objeto (en lugar de `T` tipo de valor devuelto). Cuando se devuelve un objeto de `Task` desde el método, Signalr espera a que se complete el `Task` y, a continuación, devuelve el resultado desencapsulado al cliente, por lo que no hay ninguna diferencia en la forma de codificar la llamada al método en el cliente.

Al convertir un método de concentrador en asincrónico se evita el bloqueo de la conexión cuando se usa el transporte de WebSocket. Cuando un método de concentrador se ejecuta sincrónicamente y el transporte es WebSocket, las siguientes invocaciones de métodos en el centro desde el mismo cliente se bloquean hasta que se completa el método de concentrador.

En el ejemplo siguiente se muestra el mismo método codificado para ejecutarse de forma sincrónica o asincrónica, seguido del código de cliente de JavaScript que funciona para llamar a cualquier versión.

**Sincrónica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asincrónica**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Para obtener más información sobre cómo usar métodos asincrónicos en ASP.NET 4,5, vea [uso de métodos asincrónicos en ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definir sobrecargas

Si desea definir sobrecargas para un método, el número de parámetros de cada sobrecarga debe ser diferente. Si diferencia una sobrecarga simplemente especificando distintos tipos de parámetro, la clase del concentrador se compilará, pero el servicio Signalr producirá una excepción en tiempo de ejecución cuando los clientes intenten llamar a una de las sobrecargas.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Informar del progreso de las invocaciones de método del concentrador

Signalr 2,1 agrega compatibilidad con el [patrón de informes de progreso](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introducido en .net 4,5. Para implementar los informes de progreso, defina un parámetro `IProgress<T>` para el método de concentrador al que pueda acceder el cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Al escribir un método de servidor de ejecución prolongada, es importante usar un patrón de programación asincrónico como Async/Await en lugar de bloquear el subproceso del concentrador.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Cómo llamar a métodos de cliente desde la clase hub

Para llamar a métodos de cliente desde el servidor, use la propiedad `Clients` en un método de la clase hub. En el ejemplo siguiente se muestra el código de servidor que llama a `addNewMessageToPage` en todos los clientes conectados y el código de cliente que define el método en un cliente de JavaScript.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

Invocar un método de cliente es una operación asincrónica y devuelve un `Task`. Use `await`:

* Para asegurarse de que el mensaje se envía sin errores. 
* Para habilitar la detección y el control de errores en un bloque try-catch.

**Cliente JavaScript que usa el proxy generado**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

No se puede obtener un valor devuelto de un método de cliente; la sintaxis como `int x = Clients.All.add(1,1)` no funciona.

Puede especificar tipos complejos y matrices para los parámetros. En el ejemplo siguiente se pasa un tipo complejo al cliente en un parámetro de método.

**Código de servidor que llama a un método de cliente mediante un objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Selección de los clientes que recibirán la RPC

La propiedad clients devuelve un objeto [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) que proporciona varias opciones para especificar qué clientes recibirán la RPC:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Solo el cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Todos los clientes, excepto el cliente que llama.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    En este ejemplo se llama a `addContosoChatMessageToPage` en el cliente que llama y tiene el mismo efecto que el uso de `Clients.Caller`.
- Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Todos los clientes conectados en un grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Todos los clientes conectados en un grupo especificado, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Todos los clientes conectados en un grupo especificado, excepto el cliente que llama.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Un usuario específico, identificado por userId.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    De forma predeterminada, es `IPrincipal.Identity.Name`, pero esto se puede cambiar [registrando una implementación de IUserIdProvider con el host global](mapping-users-to-connections.md#IUserIdProvider).
- Todos los clientes y grupos de una lista de identificadores de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Una lista de grupos.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Un usuario por nombre.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Una lista de nombres de usuario (introducida en Signalr 2,1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>No hay validación en tiempo de compilación para los nombres de método

El nombre de método que especifique se interpreta como un objeto dinámico, lo que significa que no hay ninguna validación de IntelliSense o de tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, Signalr envía el nombre del método y los valores de parámetro al cliente, y si el cliente tiene un método que coincide con el nombre, se llama a ese método y se le pasan los valores de parámetro. Si no se encuentra ningún método coincidente en el cliente, no se produce ningún error. Para obtener información sobre el formato de los datos que Signalr transmite al cliente en segundo plano cuando se llama a un método de cliente, consulte [Introducción a signalr](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Coincidencia de nombres de método que no distinguen mayúsculas de minúsculas

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` en el servidor ejecutará `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`o `addContosoChatMessageToPage` en el cliente.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Ejecución asincrónica

El método que se llama se ejecuta de forma asincrónica. Cualquier código que venga después de una llamada de método a un cliente se ejecutará inmediatamente sin esperar a que Signalr termine de transmitir datos a los clientes a menos que especifique que las siguientes líneas de código deben esperar a que se complete el método. En el ejemplo de código siguiente se muestra cómo ejecutar dos métodos de cliente secuencialmente.

**Usar Await (.NET 4,5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Si usa `await` para esperar hasta que un método de cliente finaliza antes de que se ejecute la siguiente línea de código, eso no significa que los clientes reciban realmente el mensaje antes de que se ejecute la siguiente línea de código. La "finalización" de una llamada al método de cliente solo significa que Signalr ha hecho todo lo necesario para enviar el mensaje. Si necesita comprobar que los clientes recibieron el mensaje, debe programar ese mecanismo. Por ejemplo, puede codificar un método de `MessageReceived` en el concentrador y, en el método de `addContosoChatMessageToPage` en el cliente, podría llamar a `MessageReceived` después de hacer todo el trabajo que necesita hacer en el cliente. En `MessageReceived` en el concentrador puede realizar cualquier trabajo que dependa de la recepción y el procesamiento reales del cliente de la llamada al método original.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Cómo usar una variable de cadena como nombre del método

Si desea invocar un método de cliente utilizando una variable de cadena como nombre del método, convierta `Clients.All` (o `Clients.Others`, `Clients.Caller`, etc.) a `IClientProxy` y, a continuación, llame a [Invoke (MethodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Cómo administrar la pertenencia a grupos desde la clase hub

Los grupos de Signalr proporcionan un método para difundir mensajes a subconjuntos específicos de clientes conectados. Un grupo puede tener cualquier número de clientes y un cliente puede ser miembro de cualquier número de grupos.

Para administrar la pertenencia a grupos, use los métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) y [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) proporcionados por la propiedad `Groups` de la clase hub. En el ejemplo siguiente se muestran los métodos `Groups.Add` y `Groups.Remove` usados en métodos de concentrador a los que llama el código de cliente, seguidos del código de cliente de JavaScript que los llama.

**Servidor**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Cliente JavaScript que usa el proxy generado**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

No tiene que crear grupos explícitamente. En efecto, se crea automáticamente un grupo la primera vez que se especifica su nombre en una llamada a `Groups.Add`y se elimina cuando se quita la última conexión de la pertenencia a él.

No hay ninguna API para obtener una lista de pertenencia a grupos o una lista de grupos. Signalr envía mensajes a clientes y grupos basados en un [modelo pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), y el servidor no mantiene listas de grupos o pertenencias a grupos. Esto ayuda a maximizar la escalabilidad, ya que cada vez que se agrega un nodo a una granja de servidores Web, cualquier Estado que mantenga Signalr debe propagarse al nuevo nodo.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Ejecución asincrónica de métodos Add y Remove

Los métodos `Groups.Add` y `Groups.Remove` se ejecutan de forma asincrónica. Si desea agregar un cliente a un grupo y enviar inmediatamente un mensaje al cliente mediante el grupo, debe asegurarse de que el método `Groups.Add` finaliza primero. En el ejemplo de código siguiente se muestra cómo hacerlo.

**Agregar un cliente a un grupo y, a continuación, mensajería de ese cliente**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Persistencia de pertenencia a grupos

Signalr realiza un seguimiento de las conexiones, no de los usuarios, por lo que si desea que un usuario esté en el mismo grupo cada vez que el usuario establece una conexión, debe llamar a `Groups.Add` cada vez que el usuario establezca una nueva conexión.

Después de una pérdida temporal de conectividad, a veces Signalr puede restaurar la conexión automáticamente. En ese caso, Signalr está restaurando la misma conexión, sin establecer una conexión nueva, por lo que la pertenencia al grupo del cliente se restaura automáticamente. Esto es posible incluso cuando la interrupción temporal es el resultado de un reinicio o un error del servidor, ya que el estado de conexión de cada cliente, incluida la pertenencia a grupos, es de ida y vuelta al cliente. Si un servidor deja de funcionar y se reemplaza por un nuevo servidor antes de que se agote el tiempo de espera de la conexión, un cliente puede volver a conectarse automáticamente al nuevo servidor y volver a inscribirse en los grupos de los que es miembro.

Cuando una conexión no se puede restaurar automáticamente después de una pérdida de conectividad, o cuando se agota el tiempo de espera de la conexión o cuando el cliente se desconecta (por ejemplo, cuando un explorador navega a una nueva página), se pierden las pertenencias a grupos. La próxima vez que el usuario se conecte será una nueva conexión. Para mantener la pertenencia a grupos cuando el mismo usuario establece una nueva conexión, la aplicación tiene que realizar el seguimiento de las asociaciones entre usuarios y grupos, y restaurar la pertenencia a grupos cada vez que un usuario establece una nueva conexión.

Para obtener más información acerca de las conexiones y las reconexiones, vea [Cómo controlar los eventos de duración de la conexión en la clase de concentrador](#connectionlifetime) más adelante en este tema.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupos de usuario único

Las aplicaciones que usan Signalr normalmente tienen que realizar un seguimiento de las asociaciones entre los usuarios y las conexiones para saber qué usuario ha enviado un mensaje y qué usuarios deben recibir un mensaje. Los grupos se usan en uno de los dos patrones de uso frecuente para hacerlo.

- Grupos de usuario único.

    Puede especificar el nombre de usuario como el nombre del grupo y agregar el identificador de conexión actual al grupo cada vez que el usuario se conecta o se vuelve a conectar. Para enviar mensajes al usuario que envía al grupo. Un inconveniente de este método es que el grupo no proporciona una manera de averiguar si el usuario está en línea o sin conexión.
- Realizar un seguimiento de las asociaciones entre los nombres de usuario y los identificadores de conexión.

    Puede almacenar una asociación entre cada nombre de usuario y uno o varios identificadores de conexión de un diccionario o una base de datos, y actualizar los datos almacenados cada vez que el usuario se conecta o desconecta. Para enviar mensajes al usuario, especifique los identificadores de conexión. Una desventaja de este método es que toma más memoria.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Cómo controlar los eventos de duración de la conexión en la clase hub

Los motivos típicos para controlar los eventos de duración de la conexión son realizar un seguimiento de si un usuario está conectado o no, y realizar un seguimiento de la asociación entre los nombres de usuario y los identificadores de conexión. Para ejecutar su propio código cuando los clientes se conectan o desconectan, invalide los métodos virtuales `OnConnected`, `OnDisconnected`y `OnReconnected` de la clase hub, tal y como se muestra en el ejemplo siguiente.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Cuando se llama a alconnected, OnDisconnection y OnReconnected

Cada vez que un explorador navega a una nueva página, debe establecerse una nueva conexión, lo que significa que Signalr ejecutará el método `OnDisconnected` seguido del método `OnConnected`. Signalr siempre crea un nuevo identificador de conexión cuando se establece una nueva conexión.

Se llama al método `OnReconnected` cuando se ha producido una interrupción temporal en la conectividad que Signalr puede recuperar automáticamente de, por ejemplo, cuando un cable se desconecta temporalmente y se vuelve a conectar antes de que se agote el tiempo de espera de la conexión. Se llama al método `OnDisconnected` cuando el cliente está desconectado y Signalr no puede volver a conectarse automáticamente, como cuando un explorador navega a una nueva página. Por lo tanto, una posible secuencia de eventos para un cliente determinado es `OnConnected`, `OnReconnected``OnDisconnected`; o `OnConnected`, `OnDisconnected`. No verá la secuencia `OnConnected`, `OnDisconnected``OnReconnected` para una conexión determinada.

No se llama al método `OnDisconnected` en algunos escenarios, como cuando un servidor deja de funcionar o se recicla el dominio de la aplicación. Cuando otro servidor entra en línea o el dominio de la aplicación completa su reciclaje, algunos clientes pueden volver a conectarse y activar el evento `OnReconnected`.

Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Estado del autor de la llamada no rellenado

Se llama a los métodos de controlador de eventos de duración de conexión desde el servidor, lo que significa que cualquier Estado que se coloque en el objeto de `state` en el cliente no se rellenará en la propiedad `Caller` del servidor. Para obtener información sobre el objeto `state` y la propiedad `Caller`, vea [Cómo pasar el estado entre clientes y la clase de concentrador](#passstate) más adelante en este tema.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Cómo obtener información sobre el cliente desde la propiedad de contexto

Para obtener información sobre el cliente, use la propiedad `Context` de la clase hub. La propiedad `Context` devuelve un objeto [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) que proporciona acceso a la siguiente información:

- El identificador de conexión del cliente que realiza la llamada.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    El identificador de conexión es un GUID que se asigna mediante Signalr (no puede especificar el valor en su propio código). Hay un identificador de conexión para cada conexión y todos los concentradores usan el mismo identificador de conexión si tiene varios centros en la aplicación.
- Datos de encabezado HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    También puede obtener encabezados HTTP de `Context.Headers`. La razón para varias referencias a lo mismo es que `Context.Headers` se creó en primer lugar, la propiedad `Context.Request` se agregó más tarde y `Context.Headers` se conserva por compatibilidad con versiones anteriores.
- Datos de cadena de consulta.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    También puede obtener datos de cadena de consulta de `Context.QueryString`.

    La cadena de consulta que se obtiene en esta propiedad es la que se usó con la solicitud HTTP que estableció la conexión de Signalr. Puede agregar parámetros de cadena de consulta en el cliente mediante la configuración de la conexión, que es una manera cómoda de pasar datos sobre el cliente desde el cliente al servidor. En el ejemplo siguiente se muestra una manera de agregar una cadena de consulta en un cliente de JavaScript cuando se usa el proxy generado.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Para obtener más información sobre la configuración de parámetros de cadena de consulta, vea las guías de API de los clientes de [JavaScript](hubs-api-guide-javascript-client.md) y [.net](hubs-api-guide-net-client.md) .

    Puede encontrar el método de transporte que se usa para la conexión en los datos de la cadena de consulta, junto con otros valores utilizados internamente por Signalr:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    El valor de `transportMethod` será "WebSockets", "serverSentEvents", "foreverFrame" o "longPolling". Tenga en cuenta que si comprueba este valor en el método de control de eventos `OnConnected`, en algunos escenarios podría obtener inicialmente un valor de transporte que no sea el método de transporte negociado final para la conexión. En ese caso, el método producirá una excepción y se llamará de nuevo más tarde cuando se establezca el método de transporte final.
- Propias.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    También puede obtener cookies de `Context.RequestCookies`.
- información del usuario.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- El objeto HttpContext de la solicitud:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Utilice este método en lugar de obtener `HttpContext.Current` para obtener el objeto de `HttpContext` para la conexión de Signalr.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Cómo pasar el estado entre los clientes y la clase de concentrador

El proxy de cliente proporciona un `state` objeto en el que puede almacenar los datos que desea que se transmitan al servidor con cada llamada al método. En el servidor, puede tener acceso a estos datos en la propiedad `Clients.Caller` en métodos de concentrador a los que llaman los clientes. La propiedad `Clients.Caller` no se rellena para los métodos de controlador de eventos de duración de conexión `OnConnected`, `OnDisconnected`y `OnReconnected`.

La creación o la actualización de datos en el objeto `state` y la propiedad `Clients.Caller` funciona en ambas direcciones. Puede actualizar los valores en el servidor y devolverlos al cliente.

En el ejemplo siguiente se muestra el código de cliente de JavaScript que almacena el estado de la transmisión al servidor con cada llamada al método.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

En el ejemplo siguiente se muestra el código equivalente en un cliente .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

En la clase hub, puede tener acceso a estos datos en la propiedad `Clients.Caller`. En el ejemplo siguiente se muestra código que recupera el estado al que se hace referencia en el ejemplo anterior.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Este mecanismo para conservar el estado no está pensado para grandes cantidades de datos, ya que todo lo que se coloca en la propiedad `state` o `Clients.Caller` se recorre en ida y vuelta con cada invocación de método. Resulta útil para los elementos más pequeños, como los nombres de usuario o los contadores.

En VB.NET o en un concentrador fuertemente tipado, no se puede tener acceso al objeto de estado del llamador a través de `Clients.Caller`; en su lugar, use `Clients.CallerState` (introducida en Signalr 2,1):

**Usar CallerState enC#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Usar CallerState en Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Cómo controlar errores en la clase hub

Para controlar los errores que se producen en los métodos de clase de concentrador, primero debe asegurarse de que "observa" cualquier excepción de las operaciones asincrónicas (como la invocación de métodos de cliente) mediante el uso de `await`. A continuación, use uno o varios de los métodos siguientes:

- Ajuste el código del método en los bloques try-catch y registre el objeto de excepción. Con fines de depuración, puede enviar la excepción al cliente, pero no se recomienda por motivos de seguridad que envíen información detallada a los clientes de producción.
- Cree un módulo de canalización de hubs que controle el método [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) . En el ejemplo siguiente se muestra un módulo de canalización que registra los errores, seguido del código de Startup.cs que inserta el módulo en la canalización de los concentradores.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Use la clase `HubException` (introducida en Signalr 2). Este error se puede producir desde cualquier invocación del concentrador. El constructor `HubError` toma un mensaje de cadena y un objeto para almacenar los datos de error adicionales. Signalr realizará la serialización automática de la excepción y la enviará al cliente, donde se utilizará para rechazar o producir un error en la invocación del método de concentrador.

    Los ejemplos de código siguientes muestran cómo iniciar una `HubException` durante una invocación del concentrador y cómo controlar la excepción en los clientes de JavaScript y .NET.

    **Código de servidor que muestra la clase HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Código de cliente de JavaScript que muestra la respuesta a la generación de un HubException en un centro**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Código de cliente .NET que muestra la respuesta a la generación de un HubException en un centro**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Para obtener más información acerca de los módulos de canalización de concentrador, consulte [Personalización de la canalización de hubs](#hubpipeline) más adelante en este tema.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Cómo habilitar el seguimiento

Para habilitar el seguimiento del lado servidor, agregue un elemento System. Diagnostics al archivo Web. config, tal como se muestra en este ejemplo:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Al ejecutar la aplicación en Visual Studio, puede ver los registros en la ventana **resultados** .

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase hub

Para llamar a métodos de cliente desde una clase diferente de la clase de concentrador, obtenga una referencia al objeto de contexto de Signalr para el concentrador y úselo para llamar a métodos en el cliente o administrar grupos.

En el siguiente ejemplo `StockTicker` clase se obtiene el objeto de contexto, se almacena en una instancia de la clase, se almacena la instancia de la clase en una propiedad estática y se usa el contexto de la instancia de la clase singleton para llamar al método `updateStockPrice` en los clientes que están conectados a un concentrador denominado `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Si necesita usar el contexto varias veces en un objeto de larga duración, obtenga la referencia una vez y guárdela en lugar de volver a obtenerla cada vez. Obtener el contexto una vez garantiza que Signalr envía mensajes a los clientes en la misma secuencia en la que los métodos del concentrador realizan las invocaciones de métodos de cliente. Para ver un tutorial que muestra cómo usar el contexto de Signalr para un concentrador, consulte [difusión del servidor con ASP.net signalr](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Llamar a métodos de cliente

Puede especificar qué clientes recibirán la RPC, pero tiene menos opciones que al llamar desde una clase de concentrador. La razón es que el contexto no está asociado a una llamada determinada de un cliente, por lo que no están disponibles los métodos que requieren conocimientos del identificador de conexión actual, como `Clients.Others`, o `Clients.Caller`o `Clients.OthersInGroup`. Están disponibles las siguientes opciones:

- Todos los clientes conectados.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Un cliente específico identificado por el identificador de conexión.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Todos los clientes conectados, excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Todos los clientes conectados en un grupo especificado.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Todos los clientes conectados en un grupo especificado excepto los clientes especificados, identificados por el identificador de conexión.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

Si está llamando a la clase que no es de Hub desde los métodos de la clase hub, puede pasar el identificador de conexión actual y usarlo con `Clients.Client`, `Clients.AllExcept`o `Clients.Group` para simular `Clients.Caller`, `Clients.Others`o `Clients.OthersInGroup`. En el ejemplo siguiente, la clase `MoveShapeHub` pasa el identificador de conexión a la clase `Broadcaster` para que la clase `Broadcaster` pueda simular `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Administrar la pertenencia a grupos

Para administrar grupos, tiene las mismas opciones que en una clase de Hub.

- Agregar un cliente a un grupo

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Quitar un cliente de un grupo

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Personalización de la canalización de hubs

Signalr le permite insertar su propio código en la canalización del concentrador. En el ejemplo siguiente se muestra un módulo de canalización de concentrador personalizado que registra cada llamada de método entrante recibida del cliente y la llamada al método de salida invocada en el cliente:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

El siguiente código del archivo *Startup.CS* registra el módulo para ejecutarse en la canalización del concentrador:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Hay muchos métodos diferentes que puede invalidar. Para obtener una lista completa, consulte [métodos de HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
