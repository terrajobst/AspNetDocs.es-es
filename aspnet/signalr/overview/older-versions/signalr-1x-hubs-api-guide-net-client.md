---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: 'Guía de la API de ASP.NET Signalr hubs: cliente .NET (Signalr 1. x) | Microsoft Docs'
author: bradygaster
description: En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de .NET, como la tienda Windows (WinRT), WPF, Silverlight y los inconvenientes...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 2b22b53c405a865f91b04e677f60b82dd46dbf9b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505975"
---
# <a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>Guía de la API de ASP.NET Signalr hubs: cliente .NET (Signalr 1. x)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 2 en clientes de .NET, como la tienda Windows (WinRT), WPF, Silverlight y aplicaciones de consola.
> 
> Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente. En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor. Signalr se encarga de todas las conexiones de cliente a servidor.
> 
> Signalr también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a Signalr, hubs y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación Signalr completa, consulte [signalr-introducción](../getting-started/index.md).

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Configuración de cliente](#clientsetup)
- [Cómo establecer una conexión](#establishconnection)

    - [Conexiones entre dominios desde clientes de Silverlight](#slcrossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPF](#maxconnections)
    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
    - [Cómo especificar encabezados HTTP](#httpheaders)
    - [Cómo especificar certificados de cliente](#clientcertificate)
- [Cómo crear el proxy de concentrador](#proxy)
- [Cómo definir métodos en el cliente a los que el servidor puede llamar](#callclient)

    - [Métodos sin parámetros](#clientmethodswithoutparms)
    - [Métodos con parámetros, especificar tipos de parámetro](#clientmethodswithparmtypes)
    - [Métodos con parámetros, especificar objetos dinámicos para los parámetros](#clientmethodswithdynamparms)
    - [Cómo quitar un controlador](#removehandler)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar los eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)
- [Ejemplos de código de la aplicación de consola, Silverlight y WPF para los métodos de cliente a los que el servidor puede llamar](#wpfsl)

Para obtener un ejemplo de proyectos de cliente .NET, vea los siguientes recursos:

- [Gustavo-Armenta/signalr-samples](https://github.com/gustavo-armenta/SignalR-Samples) en github.com (WinRT, Silverlight, ejemplos de aplicación de consola).
- [DamianEdwards/signalr-MoveShapeDemo/MoveShape. Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) en github.com (ejemplo de WPF).
- [Signalr/Microsoft. Aspnet. signalr. Client. samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) en github.com (ejemplo de aplicación de consola).

Para obtener documentación sobre cómo programar el servidor o los clientes de JavaScript, consulte los siguientes recursos:

- [Guía de API de signalr hubs-servidor](../guide-to-the-api/hubs-api-guide-server.md)
- [Guía de API de signalr hubs: cliente JavaScript](../guide-to-the-api/hubs-api-guide-javascript-client.md)

Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API. Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuración del cliente

Instale el paquete NuGet [Microsoft. Aspnet. signalr. Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) (no el paquete [Microsoft. Aspnet. signalr](http://nuget.org/packages/microsoft.aspnet.signalr) ). Este paquete es compatible con los clientes de WinRT, Silverlight, WPF, aplicación de consola y Windows Phone, tanto para .NET 4 como para .NET 4,5.

Si la versión de Signalr que tiene en el cliente es diferente de la versión que tiene en el servidor, Signalr suele ser capaz de adaptarse a la diferencia. Por ejemplo, si se lanza la versión 2,0 de Signalr y se instala en el servidor, el servidor admitirá clientes que tengan 1.1. x instalado, así como clientes que tengan instalado 2,0. Si la diferencia entre la versión del servidor y la versión del cliente es demasiado grande, Signalr inicia una excepción `InvalidOperationException` cuando el cliente intenta establecer una conexión. El mensaje de error es "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Para poder establecer una conexión, tiene que crear un objeto de `HubConnection` y crear un proxy. Para establecer la conexión, llame al método `Start` en el objeto `HubConnection`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> En el caso de los clientes de JavaScript, debe registrar al menos un controlador de eventos antes de llamar al método `Start` para establecer la conexión. Esto no es necesario para los clientes de .NET. En el caso de los clientes de JavaScript, el código de proxy generado crea automáticamente servidores proxy para todos los concentradores que existen en el servidor, y el registro de un controlador es la forma en que se indican los concentradores que el cliente intenta usar. Pero para un cliente .NET se crean proxies de concentrador de forma manual, por lo que Signalr supone que va a usar cualquier centro para el que cree un proxy.

El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

El método `Start` se ejecuta de forma asincrónica. Para asegurarse de que las líneas de código posteriores no se ejecutan hasta que se establece la conexión, use `await` en un método asincrónico ASP.NET 4,5 o `.Wait()` en un método sincrónico. No use `.Wait()` en un cliente de WinRT.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

La clase `HubConnection` es segura para la ejecución de subprocesos.

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Conexiones entre dominios desde clientes de Silverlight

Para obtener información sobre cómo habilitar las conexiones entre dominios desde clientes de Silverlight, consulte [hacer que un servicio esté disponible a través](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)de los límites del dominio.

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar cualquiera de las siguientes opciones:

- Límite de conexiones simultáneas.
- Parámetros de cadena de consulta.
- Método de transporte.
- Encabezados HTTP.
- Certificados de cliente.

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>Cómo establecer el número máximo de conexiones simultáneas en los clientes de WPF

En los clientes de WPF, es posible que tenga que aumentar el número máximo de conexiones simultáneas de su valor predeterminado de 2. El valor recomendado es 10.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

Para obtener más información, consulte [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión. En el ejemplo siguiente se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de Signalr normalmente negocia con el servidor para determinar el mejor transporte compatible con el servidor y el cliente. Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación. Para especificar el método de transporte, pase un objeto de transporte al método de inicio. En el ejemplo siguiente se muestra cómo especificar el método de transporte en el código de cliente.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

El espacio de nombres [Microsoft. Aspnet. signalr. Client. transportes](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) incluye las siguientes clases que se pueden utilizar para especificar el transporte.

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (disponible solo cuando el servidor y el cliente usan .net 4,5).
- [Transporte](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) automático (elige automáticamente el mejor transporte que admiten tanto el cliente como el servidor. Este es el transporte predeterminado. Pasarlo al método `Start` tiene el mismo efecto que no pasar nada.)

El transporte ForeverFrame no se incluye en esta lista porque solo lo usan los exploradores.

Para obtener información sobre cómo comprobar el método de transporte en el código del servidor, consulte [ASP.net signalr hubs API Guide-Server: How to get Information about the Client from the Context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Para obtener más información sobre los transportes y los respaldos, vea [Introducción a signalr-transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>Cómo especificar encabezados HTTP

Para establecer los encabezados HTTP, utilice la propiedad `Headers` en el objeto de conexión. En el ejemplo siguiente se muestra cómo agregar un encabezado HTTP.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>Cómo especificar certificados de cliente

Para agregar certificados de cliente, use el método `AddClientCertificate` en el objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>Cómo crear el proxy de concentrador

Para definir métodos en el cliente a los que puede llamar un concentrador desde el servidor e invocar métodos en un concentrador en el servidor, cree un proxy para el concentrador llamando a `CreateHubProxy` en el objeto de conexión. La cadena que se pasa a `CreateHubProxy` es el nombre de la clase del concentrador o el nombre especificado por el atributo `HubName` si se usó uno en el servidor. La coincidencia de nombres no distingue mayúsculas y minúsculas.

**Clase hub en el servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Crear proxy de cliente para la clase de concentrador**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

Si decora la clase hub con un atributo `HubName`, use ese nombre.

**Clase hub en el servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**Crear proxy de cliente para la clase de concentrador**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

El objeto proxy es seguro para subprocesos. De hecho, si llama a `HubConnection.CreateHubProxy` varias veces con el mismo `hubName`, obtiene el mismo objeto de `IHubProxy` almacenado en memoria caché.

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente a los que el servidor puede llamar

Para definir un método al que pueda llamar el servidor, use el método `On` del proxy para registrar un controlador de eventos.

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.UpdateStockPrice` en el servidor ejecutará `updateStockPrice`, `updatestockprice`o `UpdateStockPrice` en el cliente.

Las distintas plataformas de cliente tienen requisitos diferentes para escribir el código de método para actualizar la interfaz de usuario. Los ejemplos que se muestran son para clientes de WinRT (Windows Store .NET). Los ejemplos de WPF, Silverlight y aplicación de consola se proporcionan en [una sección independiente, más adelante en este tema](#wpfsl).

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>Métodos sin parámetros

Si el método que está controlando no tiene parámetros, use la sobrecarga no genérica del método `On`:

**Método de cliente de llamada de código de servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**Código de cliente de WinRT para el método al que se llama desde el servidor sin parámetros ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros que especifican los tipos de parámetro

Si el método que está controlando tiene parámetros, especifique los tipos de los parámetros como tipos genéricos del método `On`. Existen sobrecargas genéricas del método `On` para que pueda especificar hasta 8 parámetros (4 en Windows Phone 7). En el ejemplo siguiente, se envía un parámetro al método `UpdateStockPrice`.

**Método de cliente de llamada de código de servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**La clase stock utilizada para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**Código de cliente de WinRT para un método llamado desde el servidor con un parámetro ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, especificar objetos dinámicos para los parámetros

Como alternativa a especificar parámetros como tipos genéricos del método `On`, puede especificar parámetros como objetos dinámicos:

**Método de cliente de llamada de código de servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**La clase stock utilizada para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**Código de cliente de WinRT para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro ([vea los ejemplos de WPF y Silverlight más adelante en este tema](#wpfsl))**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>Cómo quitar un controlador

Para quitar un controlador, llame a su método `Dispose`.

**Código de cliente para un método llamado desde el servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**Código de cliente para quitar el controlador**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método en el servidor, use el método `Invoke` en el proxy de concentrador.

Si el método de servidor no tiene ningún valor devuelto, use la sobrecarga no genérica del método `Invoke`.

**Código de servidor para un método que no tiene ningún valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**Código de cliente que llama a un método que no tiene ningún valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

Si el método de servidor tiene un valor devuelto, especifique el tipo de valor devuelto como tipo genérico del método `Invoke`.

**Código de servidor para un método que tiene un valor devuelto y toma un parámetro de tipo complejo.**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**La clase stock utilizada para el parámetro y el valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método asincrónico ASP.NET 4,5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**Código de cliente que llama a un método que tiene un valor devuelto y toma un parámetro de tipo complejo, en un método sincrónico**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

El método `Invoke` se ejecuta de forma asincrónica y devuelve un objeto `Task`. Si no especifica `await` o `.Wait()`, la siguiente línea de código se ejecutará antes de que finalice la ejecución del método invocado.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar los eventos de duración de la conexión

Signalr proporciona los siguientes eventos de duración de conexión que puede controlar:

- `Received`: se genera cuando se reciben datos en la conexión. Proporciona los datos recibidos.
- `ConnectionSlow`: se genera cuando el cliente detecta una conexión de eliminación lenta o con frecuencia.
- `Reconnecting`: se genera cuando se inicia la reconexión del transporte subyacente.
- `Reconnected`: se genera cuando se vuelve a conectar el transporte subyacente.
- `StateChanged`: se genera cuando cambia el estado de la conexión. Proporciona el estado anterior y el nuevo estado. Para obtener información sobre los valores de estado de la conexión, consulte [enumeración ConnectionState](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).
- `Closed`: se genera cuando se desconecta la conexión.

Por ejemplo, si desea mostrar mensajes de advertencia sobre errores que no son graves pero causan problemas de conexión intermitentes, como la lentitud o la caída frecuente de la conexión, controle el evento `ConnectionSlow`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](../guide-to-the-api/handling-connection-lifetime-events.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar errores

Si no se habilitan explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que Signalr devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si se produce un error en una llamada a `newContosoChatMessage`, el mensaje de error del objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`". no se recomienda el envío de mensajes de error detallados a los clientes en producción por motivos de seguridad, pero si desea habilitar mensajes de error detallados para la solución de problemas, use el siguiente código en el servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

Para controlar los errores generados por Signalr, puede Agregar un controlador para el evento `Error` en el objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

Para controlar los errores de las invocaciones de método, ajuste el código en un bloque try-catch.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro del lado cliente, establezca las propiedades `TraceLevel` y `TraceWriter` en el objeto de conexión.

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>Ejemplos de código de la aplicación de consola, Silverlight y WPF para los métodos de cliente a los que el servidor puede llamar

Los ejemplos de código mostrados anteriormente para definir métodos de cliente a los que puede llamar el servidor se aplican a los clientes de WinRT. En los siguientes ejemplos se muestra el código equivalente para los clientes de la aplicación de consola, Silverlight y WPF.

### <a name="methods-without-parameters"></a>Métodos sin parámetros

**Código de cliente de WPF para el método al que se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**Código de cliente de Silverlight para el método al que se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**Código de cliente de aplicación de consola para el método al que se llama desde el servidor sin parámetros**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>Métodos con parámetros que especifican los tipos de parámetro

**Código de cliente de WPF para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método llamado desde el servidor con un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>Métodos con parámetros, especificar objetos dinámicos para los parámetros

**Código de cliente de WPF para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**Código de cliente de Silverlight para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**Código de cliente de aplicación de consola para un método llamado desde el servidor con un parámetro, utilizando un objeto dinámico para el parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
