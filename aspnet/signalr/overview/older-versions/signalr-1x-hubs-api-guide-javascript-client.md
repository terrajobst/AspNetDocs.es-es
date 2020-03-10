---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: 'Guía de la API signalr 1. x hubs: cliente JavaScript | Microsoft Docs'
author: bradygaster
description: En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 1,1 en clientes de JavaScript, como exploradores y la tienda Windows (WinJS) aplic...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431107"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a>Guía de la API SignalR 1.x Hubs - Cliente JavaScript

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se proporciona una introducción al uso de la API de hubs para Signalr versión 1,1 en los clientes de JavaScript, como los exploradores y las aplicaciones de la tienda Windows (WinJS).
> 
> Signalr hubs API le permite realizar llamadas a procedimiento remoto (RPC) desde un servidor a clientes conectados y desde clientes al servidor. En el código del servidor, se definen los métodos a los que pueden llamar los clientes y se llama a los métodos que se ejecutan en el cliente. En el código de cliente, se definen los métodos a los que se puede llamar desde el servidor y se llama a los métodos que se ejecutan en el servidor. Signalr se encarga de todas las conexiones de cliente a servidor.
> 
> Signalr también ofrece una API de nivel inferior denominada conexiones persistentes. Para obtener una introducción a Signalr, hubs y conexiones persistentes, o para ver un tutorial que muestra cómo crear una aplicación Signalr completa, consulte [signalr-introducción](../getting-started/index.md).

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [El proxy generado y lo que hace para usted](#genproxy)

    - [Cuándo usar el proxy generado](#cantusegenproxy)
- [Configuración de cliente](#clientsetup)

    - [Cómo hacer referencia al proxy generado dinámicamente](#dynamicproxy)
    - [Cómo crear un archivo físico para el proxy generado por Signalr](#manualproxy)
- [Cómo establecer una conexión](#establishconnection)

    - [$. Connection. Hub es el mismo objeto que $. hubConnection () crea](#connequivalence)
    - [Ejecución asincrónica del método Start](#asyncstart)
- [Cómo establecer una conexión entre dominios](#crossdomain)
- [Cómo configurar la conexión](#configureconnection)

    - [Cómo especificar parámetros de cadena de consulta](#querystring)
    - [Cómo especificar el método de transporte](#transport)
- [Cómo obtener un proxy para una clase de concentrador](#getproxy)
- [Cómo definir métodos en el cliente a los que el servidor puede llamar](#callclient)
- [Cómo llamar a métodos de servidor desde el cliente](#callserver)
- [Cómo controlar los eventos de duración de la conexión](#connectionlifetime)
- [Cómo controlar errores](#handleerrors)
- [Cómo habilitar el registro del lado cliente](#logging)

Para obtener documentación sobre cómo programar el servidor o los clientes de .NET, vea los siguientes recursos:

- [Guía de API de signalr hubs-servidor](../guide-to-the-api/hubs-api-guide-server.md)
- [Guía de la API de signalr hubs: cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Los vínculos a los temas de referencia de la API se redirigen a la versión .NET 4,5 de la API. Si usa .NET 4, vea [la versión .net 4 de los temas de la API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>El proxy generado y lo que hace para usted

Puede programar un cliente de JavaScript para que se comunique con un servicio de Signalr con o sin un proxy que Signalr genera automáticamente. Lo que hace el proxy es simplificar la sintaxis del código que se utiliza para conectarse, escribir métodos a los que llama el servidor y llamar a métodos en el servidor.

Al escribir código para llamar a métodos de servidor, el proxy generado le permite utilizar la sintaxis que parece que estaba ejecutando una función local: puede escribir `serverMethod(arg1, arg2)` en lugar de `invoke('serverMethod', arg1, arg2)`. La sintaxis de proxy generada también permite un error de lado cliente inmediato e inteligible si se escribe un nombre de método de servidor invariable. Y si crea manualmente el archivo que define los proxies, también puede obtener compatibilidad con IntelliSense para escribir código que llame a métodos de servidor.

Por ejemplo, supongamos que tiene la siguiente clase de concentrador en el servidor:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

En los siguientes ejemplos de código se muestra el aspecto del código JavaScript para invocar el método `NewContosoChatMessage` en el servidor y recibir invocaciones del método `addContosoChatMessageToPage` desde el servidor.

**Con el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sin el proxy generado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Cuándo usar el proxy generado

Si desea registrar varios controladores de eventos para un método de cliente al que el servidor llama, no puede usar el proxy generado. De lo contrario, puede elegir usar el proxy generado o no según sus preferencias de codificación. Si decide no utilizarlo, no es necesario que haga referencia a la dirección URL de "signalr/hubs" en un elemento de `script` en el código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuración del cliente

Un cliente de JavaScript requiere referencias a jQuery y al archivo JavaScript de Signalr Core. La versión de jQuery debe ser 1.6.4 o versiones posteriores, como 1.7.2, 1.8.2 o 1.9.1. Si decide usar el proxy generado, también necesitará una referencia al archivo JavaScript del proxy generado por Signalr. En el ejemplo siguiente se muestra el aspecto de las referencias en una página HTML que usa el proxy generado.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Estas referencias se deben incluir en este orden: primero jQuery, Signalr Core después de eso y los proxies de Signalr en último lugar.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Cómo hacer referencia al proxy generado dinámicamente

En el ejemplo anterior, la referencia al proxy generado por Signalr es el código de JavaScript generado dinámicamente, no un archivo físico. Signalr crea el código JavaScript del proxy sobre la marcha y lo atiende al cliente en respuesta a la dirección URL "/signalr/hubs". Si especificó una dirección URL base diferente para las conexiones de Signalr en el servidor en el método de `MapHubs`, la dirección URL del archivo de proxy generado dinámicamente es su dirección URL personalizada con "/hubs" anexado.

> [!NOTE]
> En el caso de los clientes JavaScript de Windows 8 (tienda Windows), use el archivo de proxy físico en lugar de uno generado dinámicamente. Para obtener más información, vea [Cómo crear un archivo físico para el proxy generado por signalr](#manualproxy) más adelante en este tema.

En una vista de Razor de ASP.NET MVC 4, use la tilde para hacer referencia a la raíz de la aplicación en la referencia del archivo proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Para obtener más información sobre el uso de Signalr en MVC 4, vea [Introducción con signalr y MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

En una vista de Razor de ASP.NET MVC 3, use `Url.Content` para la referencia del archivo proxy:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

En una aplicación de formularios Web Forms ASP.NET, use `ResolveClientUrl` para la referencia de archivo de servidores proxy o regístrela mediante el ScriptManager mediante la ruta de acceso relativa de la raíz de la aplicación (empezando por una tilde):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como norma general, use el mismo método para especificar la dirección URL "/signalr/hubs" que se usa para los archivos CSS o JavaScript. Si especifica una dirección URL sin usar una tilde, en algunos escenarios la aplicación funcionará correctamente al realizar la prueba en Visual Studio con IIS Express pero se producirá un error 404 al implementar en el IIS completo. Para obtener más información, vea **resolver referencias a recursos de nivel de raíz** en [servidores Web en Visual Studio para proyectos Web de ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) en el sitio de MSDN.

Al ejecutar un proyecto web en Visual Studio 2012 en modo de depuración, y si usa Internet Explorer como explorador, puede ver el archivo de proxy en **Explorador de soluciones** en **documentos de script**, como se muestra en la siguiente ilustración.

![Archivo proxy generado por JavaScript en Explorador de soluciones](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Para ver el contenido del archivo, haga doble clic en **hubs**. Si no usa Visual Studio 2012 e Internet Explorer, o si no está en modo de depuración, también puede obtener el contenido del archivo si va a la dirección URL "/signalR/hubs". Por ejemplo, si el sitio se ejecuta en `http://localhost:56699`, vaya a `http://localhost:56699/SignalR/hubs` en el explorador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Cómo crear un archivo físico para el proxy generado por Signalr

Como alternativa al proxy generado dinámicamente, puede crear un archivo físico que tenga el código proxy y hacer referencia a ese archivo. Tal vez desee hacerlo para controlar el comportamiento de almacenamiento en caché o de agrupación, o para obtener IntelliSense cuando se codifican llamadas a métodos de servidor.

Para crear un archivo proxy, realice los pasos siguientes:

1. Instale el paquete NuGet [Microsoft. Aspnet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Abra un símbolo del sistema y vaya a la carpeta *Tools* que contiene el archivo signalr. exe. La carpeta Tools se encuentra en la siguiente ubicación:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Escriba el comando siguiente:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    La ruta de acceso al *archivo. dll* es normalmente la carpeta *bin* en la carpeta del proyecto.

    Este comando crea un archivo denominado *Server. js* en la misma carpeta que *signalr. exe*.
4. Coloque el archivo *Server. js* en una carpeta adecuada del proyecto, cambie el nombre según corresponda a la aplicación y agregue una referencia a él en lugar de la referencia "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Cómo establecer una conexión

Antes de establecer una conexión, tiene que crear un objeto de conexión, crear un proxy y registrar controladores de eventos para los métodos a los que se puede llamar desde el servidor. Cuando el proxy y los controladores de eventos estén configurados, establezca la conexión llamando al método `start`.

Si usa el proxy generado, no tiene que crear el objeto de conexión en su propio código porque el código proxy generado lo hace por usted.

<a id="nogenconnection"></a>

**Establecer una conexión (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Establecer una conexión (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Normalmente se registran los controladores de eventos antes de llamar al método `start` para establecer la conexión. Si desea registrar algunos controladores de eventos después de establecer la conexión, puede hacerlo, pero debe registrar al menos uno de los controladores de eventos antes de llamar al método `start`. Una razón para esto es que puede haber muchos concentradores en una aplicación, pero no querrá desencadenar el evento de `OnConnected` en cada concentrador si solo va a usar uno de ellos. Cuando se establece la conexión, la presencia de un método de cliente en el proxy de un concentrador es lo que indica a Signalr que desencadene el evento `OnConnected`. Si no registra ningún controlador de eventos antes de llamar al método `start`, podrá invocar métodos en el concentrador, pero no se llamará al método `OnConnected` del concentrador y no se invocará ningún método de cliente desde el servidor.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub es el mismo objeto que $. hubConnection () crea

Como puede ver en los ejemplos, al utilizar el proxy generado, `$.connection.hub` hace referencia al objeto de conexión. Este es el mismo objeto que se obtiene al llamar a `$.hubConnection()` cuando no se usa el proxy generado. El código proxy generado crea la conexión mediante la ejecución de la siguiente instrucción:

![Crear una conexión en el archivo de proxy generado](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Cuando use el proxy generado, puede hacer algo con `$.connection.hub` que puede hacer con un objeto de conexión cuando no está utilizando el proxy generado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Ejecución asincrónica del método Start

El método `start` se ejecuta de forma asincrónica. Devuelve un [objeto de jQuery diferido](http://api.jquery.com/category/deferred-object/), lo que significa que puede agregar funciones de devolución de llamada llamando a métodos como `pipe`, `done`y `fail`. Si tiene código que desea ejecutar una vez establecida la conexión, como una llamada a un método de servidor, coloque ese código en una función de devolución de llamada o llámelo desde una función de devolución de llamada. El método de devolución de llamada de `.done` se ejecuta una vez establecida la conexión y después de que se complete la ejecución de cualquier código que tenga en el método de control de eventos `OnConnected` en el servidor.

Si coloca la instrucción "ahora conectada" del ejemplo anterior como la siguiente línea de código después de la llamada al método `start` (no en una devolución de llamada `.done`), la línea de `console.log` se ejecutará antes de establecer la conexión, como se muestra en el ejemplo siguiente:

![Manera incorrecta de escribir código que se ejecute una vez establecida la conexión](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Cómo establecer una conexión entre dominios

Normalmente, si el explorador carga una página desde `http://contoso.com`, la conexión de Signalr está en el mismo dominio, en `http://contoso.com/signalr`. Si la página de `http://contoso.com` establece una conexión con `http://fabrikam.com/signalr`, es una conexión entre dominios. Por motivos de seguridad, las conexiones entre dominios están deshabilitadas de forma predeterminada. Para establecer una conexión entre dominios, asegúrese de que las conexiones entre dominios estén habilitadas en el servidor y especifique la dirección URL de conexión cuando cree el objeto de conexión. Signalr usará la tecnología adecuada para las conexiones entre dominios, como [JSONP](http://en.wikipedia.org/wiki/JSONP) o [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

En el servidor, habilite las conexiones entre dominios seleccionando esa opción al llamar al método `MapHubs`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

En el cliente, especifique la dirección URL cuando cree el objeto de conexión (sin el proxy generado) o antes de llamar al método Start (con el proxy generado).

**Código de cliente que especifica una conexión entre dominios (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Código de cliente que especifica una conexión entre dominios (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Al usar el constructor `$.hubConnection`, no tiene que incluir `signalr` en la dirección URL porque se agrega automáticamente (a menos que especifique `useDefaultUrl` como `false`).

Puede crear varias conexiones a distintos puntos de conexión.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - No establezca `jQuery.support.cors` en true en el código.
> 
>     ![No establezca jQuery. support. CORS en true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     Signalr controla el uso de JSONP o CORS. Si se establece `jQuery.support.cors` en true, se deshabilita JSONP porque hace que Signalr asuma que el explorador es compatible con CORS.
> - Cuando se conecta a una dirección URL de localhost, Internet Explorer 10 no la considerará una conexión entre dominios, por lo que la aplicación funcionará localmente con IE 10 aunque no haya habilitado las conexiones entre dominios en el servidor.
> - Para obtener información sobre el uso de conexiones entre dominios con Internet Explorer 9, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obtener información sobre el uso de conexiones entre dominios con Chrome, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - El código de ejemplo utiliza la dirección URL "/signalr" predeterminada para conectarse al servicio Signalr. Para obtener información sobre cómo especificar una dirección URL base diferente, consulte [ASP.net signalr hubs API Guide-Server-The/SIGNALR URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Cómo configurar la conexión

Antes de establecer una conexión, puede especificar parámetros de cadena de consulta o especificar el método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Cómo especificar parámetros de cadena de consulta

Si desea enviar datos al servidor cuando el cliente se conecta, puede agregar parámetros de cadena de consulta al objeto de conexión. En los siguientes ejemplos se muestra cómo establecer un parámetro de cadena de consulta en el código de cliente.

**Establecer un valor de cadena de consulta antes de llamar al método Start (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Establecer un valor de cadena de consulta antes de llamar al método Start (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

En el ejemplo siguiente se muestra cómo leer un parámetro de cadena de consulta en el código del servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Cómo especificar el método de transporte

Como parte del proceso de conexión, un cliente de Signalr normalmente negocia con el servidor para determinar el mejor transporte compatible con el servidor y el cliente. Si ya sabe qué transporte desea usar, puede omitir este proceso de negociación especificando el método de transporte al llamar al método `start`.

**Código de cliente que especifica el método de transporte (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**Código de cliente que especifica el método de transporte (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Como alternativa, puede especificar varios métodos de transporte en el orden en el que desea que Signalr los pruebe:

**Código de cliente que especifica un esquema de reserva de transporte personalizado (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**Código de cliente que especifica un esquema de reserva de transporte personalizado (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Puede usar los siguientes valores para especificar el método de transporte:

- WebSockets
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

En los siguientes ejemplos se muestra cómo averiguar qué método de transporte está utilizando una conexión.

**Código de cliente que muestra el método de transporte utilizado por una conexión (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**Código de cliente que muestra el método de transporte utilizado por una conexión (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Para obtener información sobre cómo comprobar el método de transporte en el código del servidor, consulte [ASP.net signalr hubs API Guide-Server: How to get Information about the Client from the Context Property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Para obtener más información sobre los transportes y los respaldos, vea [Introducción a signalr-transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Cómo obtener un proxy para una clase de concentrador

Cada objeto de conexión que se crea encapsula información sobre una conexión a un servicio de Signalr que contiene una o más clases de concentrador. Para comunicarse con una clase de concentrador, se usa un objeto proxy que se crea (si no se usa el proxy generado) o que se genera automáticamente.

En el cliente, el nombre del proxy es una versión con mayúsculas y minúsculas Camel del nombre de la clase de concentrador. Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.

**Clase hub en el servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Si decora la clase hub con un atributo `HubName`, use el nombre exacto sin cambiar mayúsculas de minúsculas.

**Clase hub en el servidor con el atributo HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Obtener una referencia al proxy de cliente generado para el concentrador**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Crear proxy de cliente para la clase de concentrador (sin el proxy generado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Cómo definir métodos en el cliente a los que el servidor puede llamar

Para definir un método al que el servidor pueda llamar desde un concentrador, agregue un controlador de eventos al proxy del concentrador mediante la propiedad `client` del proxy generado o llame al método `on` si no está utilizando el proxy generado. Los parámetros pueden ser objetos complejos.

Agregue el controlador de eventos antes de llamar al método `start` para establecer la conexión. (Si desea agregar controladores de eventos después de llamar al método `start`, vea la nota de [cómo establecer una conexión](#establishconnection) anteriormente en este documento y use la sintaxis que se muestra para definir un método sin usar el proxy generado).

La coincidencia de nombres de método no distingue mayúsculas de minúsculas. Por ejemplo, `Clients.All.addContosoChatMessageToPage` en el servidor ejecutará `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`o `addcontosochatmessagetopage` en el cliente.

**Define el método en el cliente (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Manera alternativa de definir el método en el cliente (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Define el método en el cliente (sin el proxy generado o al agregar después de llamar al método Start)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Código de servidor que llama al método de cliente**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

En los ejemplos siguientes se incluye un objeto complejo como parámetro de método.

**Define el método en el cliente que toma un objeto complejo (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Define el método en el cliente que toma un objeto complejo (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Código de servidor que define el objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Código de servidor que llama al método de cliente mediante un objeto complejo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Cómo llamar a métodos de servidor desde el cliente

Para llamar a un método de servidor desde el cliente, use la propiedad `server` del proxy generado o el método `invoke` en el proxy de concentrador si no está utilizando el proxy generado. El valor devuelto o los parámetros pueden ser objetos complejos.

Pase una versión en mayúsculas y minúsculas Camel del nombre del método en el concentrador. Signalr realiza este cambio automáticamente para que el código JavaScript pueda ajustarse a las convenciones de JavaScript.

En los siguientes ejemplos se muestra cómo llamar a un método de servidor que no tiene un valor devuelto y cómo llamar a un método de servidor que tiene un valor devuelto.

**Método de servidor sin atributo HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Código de servidor que define el objeto complejo pasado en un parámetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Si decora el método de concentrador con un atributo `HubMethodName`, use ese nombre sin cambiar mayúsculas de minúsculas.

**Método de servidor** con un atributo HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

En los ejemplos anteriores se muestra cómo llamar a un método de servidor que no tiene ningún valor devuelto. En los siguientes ejemplos se muestra cómo llamar a un método de servidor que tiene un valor devuelto.

**Código de servidor para un método que tiene un valor devuelto**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**La clase stock utilizada para el** valor devuelto

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Código de cliente que invoca el método de servidor (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Código de cliente que invoca el método de servidor (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Cómo controlar los eventos de duración de la conexión

Signalr proporciona los siguientes eventos de duración de conexión que puede controlar:

- `starting`: se genera antes de que se envíen datos a través de la conexión.
- `received`: se genera cuando se reciben datos en la conexión. Proporciona los datos recibidos.
- `connectionSlow`: se genera cuando el cliente detecta una conexión de eliminación lenta o con frecuencia.
- `reconnecting`: se genera cuando se inicia la reconexión del transporte subyacente.
- `reconnected`: se genera cuando se vuelve a conectar el transporte subyacente.
- `stateChanged`: se genera cuando cambia el estado de la conexión. Proporciona el estado anterior y el nuevo estado (conexión, conexión, reconexión o desconexión).
- `disconnected`: se genera cuando se desconecta la conexión.

Por ejemplo, si desea mostrar mensajes de advertencia cuando hay problemas de conexión que pueden provocar retrasos perceptibles, controle el evento `connectionSlow`.

**Controlar el evento connectionSlow (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Controlar el evento connectionSlow (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Para obtener más información, consulte [Descripción y control de eventos de duración de conexión en signalr](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Cómo controlar errores

El cliente de Signalr JavaScript proporciona un evento `error` al que se puede Agregar un controlador. También puede utilizar el método FAIL para agregar un controlador para los errores resultantes de una invocación de método de servidor.

Si no se habilitan explícitamente los mensajes de error detallados en el servidor, el objeto de excepción que Signalr devuelve después de un error contiene información mínima sobre el error. Por ejemplo, si se produce un error en una llamada a `newContosoChatMessage`, el mensaje de error del objeto de error contiene "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`". no se recomienda el envío de mensajes de error detallados a los clientes en producción por motivos de seguridad, pero si desea habilitar mensajes de error detallados para la solución de problemas, use el siguiente código en el servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

En el ejemplo siguiente se muestra cómo agregar un controlador para el evento de error.

**Agregar un controlador de errores (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Agregar un controlador de errores (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

En el ejemplo siguiente se muestra cómo controlar un error desde una invocación de método.

**Controlar un error desde una invocación de método (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Controlar un error desde una invocación de método (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Si se produce un error en la invocación de un método, también se produce el evento `error`, por lo que se ejecutaría el código en el controlador del método `error` y en la devolución de llamada del método `.fail`.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Cómo habilitar el registro del lado cliente

Para habilitar el registro del lado cliente en una conexión, establezca la propiedad `logging` en el objeto de conexión antes de llamar al método `start` para establecer la conexión.

**Habilitar el registro (con el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Habilitar el registro (sin el proxy generado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Para ver los registros, abra las herramientas de desarrollo del explorador y vaya a la pestaña consola. Para ver un tutorial en el que se muestran instrucciones paso a paso y capturas de pantalla que muestran cómo hacerlo, consulte [difusión del servidor con ASP.net signalr-habilitar el registro](index.md).
