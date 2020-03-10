---
uid: signalr/overview/testing-and-debugging/troubleshooting
title: Solución de problemas de signalr | Microsoft Docs
author: bradygaster
description: En este artículo se describen los problemas comunes relacionados con el desarrollo de aplicaciones de Signalr.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 4b559e6c-4fb0-4a04-9812-45cf08ae5779
msc.legacyurl: /signalr/overview/testing-and-debugging/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: bcd273d839aed64ad2712eb503dd1942a2d4e355
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467437"
---
# <a name="signalr-troubleshooting"></a>Solución de problemas de SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este documento se describen los problemas comunes de la solución de problemas con Signalr.
>
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

Este documento contiene las siguientes secciones.

- [No se puede llamar a métodos entre el cliente y el servidor de forma silenciosa](#connection)
- [Configuración de WebSockets de IIS para ping/pong para detectar un cliente inactivo](#pong)
- [Otros problemas de conexión](#other)
- [Errores de compilación y de servidor](#server)
- [Problemas de Visual Studio](#vs)
- [Problemas de Internet Information Services](#iis)
- [Problemas de Microsoft Azure](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a>No se puede llamar a métodos entre el cliente y el servidor de forma silenciosa

En esta sección se describen las posibles causas de que se produzca un error en una llamada de método entre el cliente y el servidor sin un mensaje de error significativo. En una aplicación Signalr, el servidor no tiene información sobre los métodos que implementa el cliente; Cuando el servidor invoca un método de cliente, el nombre del método y los datos de parámetro se envían al cliente, y el método se ejecuta solo si existe en el formato especificado por el servidor. Si no se encuentra ningún método coincidente en el cliente, no sucede nada y no se genera ningún mensaje de error en el servidor.

Para investigar aún más los métodos de cliente que no se llaman, puede activar el registro antes de llamar al método Start en el concentrador para ver qué llamadas provienen del servidor. Para habilitar el registro en una aplicación de JavaScript, consulte [Cómo habilitar el registro del lado cliente (versión del cliente JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging). Para habilitar el registro en una aplicación cliente .NET, consulte [Cómo habilitar el registro del lado cliente (versión del cliente .net)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a>Método mal escrito, firma de método incorrecta o nombre de centro incorrecto

Si el nombre o la firma de un método llamado no coincide exactamente con un método adecuado en el cliente, se producirá un error en la llamada. Compruebe que el nombre del método al que llama el servidor coincide con el nombre del método en el cliente. Además, Signalr crea el proxy de concentrador mediante métodos con grafía Camel, como es adecuado en JavaScript, por lo que un método denominado `SendMessage` en el servidor se denominará `sendMessage` en el proxy del cliente. Si usa el atributo `HubName` en el código del lado servidor, compruebe que el nombre utilizado coincide con el nombre usado para crear el concentrador en el cliente. Si no usa el atributo `HubName`, compruebe que el nombre del concentrador en un cliente de JavaScript tiene mayúsculas y minúsculas Camel, como chatHub en lugar de ChatHub.

### <a name="duplicate-method-name-on-client"></a>Nombre de método duplicado en el cliente

Compruebe que no tiene un método duplicado en el cliente que se diferencie solo por el uso de mayúsculas y minúsculas. Si la aplicación cliente tiene un método denominado `sendMessage`, compruebe que tampoco hay también un método denominado `SendMessage`.

### <a name="missing-json-parser-on-the-client"></a>Falta el analizador JSON en el cliente

Signalr requiere que haya un analizador JSON presente para serializar las llamadas entre el servidor y el cliente. Si el cliente no tiene un analizador JSON integrado (por ejemplo, Internet Explorer 7), deberá incluir uno en la aplicación. Puede descargar el analizador JSON [aquí](http://nuget.org/packages/json2).

### <a name="mixing-hub-and-persistentconnection-syntax"></a>Combinación de la sintaxis de Hub y PersistentConnection

Signalr usa dos modelos de comunicación: hubs y PersistentConnections. La sintaxis para llamar a estos dos modelos de comunicación es diferente en el código de cliente. Si ha agregado un concentrador en el código del servidor, compruebe que todo el código de cliente utiliza la sintaxis de concentrador adecuada.

**Código de cliente de JavaScript que crea un PersistentConnection en un cliente de JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

**Código de cliente de JavaScript que crea un proxy de concentrador en un cliente de JavaScript**

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

**C#código de servidor que asigna una ruta a un PersistentConnection**

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

**C#código de servidor que asigna una ruta a un concentrador o a varios centros si tiene varias aplicaciones**

[!code-css[Main](troubleshooting/samples/sample4.css)]

### <a name="connection-started-before-subscriptions-are-added"></a>Conexión iniciada antes de agregar las suscripciones

Si se inicia la conexión del concentrador antes de que se agreguen al proxy los métodos a los que se puede llamar desde el servidor, no se recibirán los mensajes. El siguiente código de JavaScript no iniciará correctamente el concentrador:

**Código de cliente de JavaScript incorrecto que no permitirá la recepción de mensajes de concentradores**

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

En su lugar, agregue las suscripciones de método antes de llamar a Start:

**Código de cliente de JavaScript que agrega correctamente suscripciones a un centro**

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a>Falta el nombre de método en el proxy de concentrador

Compruebe que el método definido en el servidor está suscrito a en el cliente. Aunque el servidor define el método, todavía debe agregarse al proxy del cliente. Los métodos se pueden agregar al proxy de cliente de las maneras siguientes (tenga en cuenta que el método se agrega al miembro `client` del concentrador, no al centro directamente):

**Código de cliente de JavaScript que agrega métodos a un proxy de concentrador**

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a>Métodos centrales o concentradores no declarados como públicos

Para que sea visible en el cliente, la implementación y los métodos del concentrador se deben declarar como `public`.

### <a name="accessing-hub-from-a-different-application"></a>Acceder a la central desde una aplicación diferente

Solo se puede tener acceso a los concentradores de signalr a través de aplicaciones que implementan clientes de Signalr. Signalr no puede interoperar con otras bibliotecas de comunicación (como SOAP o servicios Web WCF). Si no hay ningún cliente de Signalr disponible para la plataforma de destino, no puede tener acceso directamente al punto de conexión del servidor.

### <a name="manually-serializing-data"></a>Serializar datos manualmente

Signalr usará automáticamente JSON para serializar los parámetros de método, por lo que no es necesario hacerlo usted mismo.

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a>Método de concentrador remoto no ejecutado en el cliente en la función OnDisconnection

Este comportamiento está diseñado así. Cuando se llama a `OnDisconnected`, el concentrador ya ha entrado en el estado `Disconnected`, lo que no permite llamar a más métodos de concentrador.

**C#código de servidor que ejecuta correctamente el código en el evento OnDisconnection**

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="ondisconnect-not-firing-at-consistent-times"></a>OnDisconnect no se activa en momentos coherentes

Este comportamiento está diseñado así. Cuando un usuario intenta salir de una página con una conexión de Signalr activa, el cliente de Signalr realizará el mejor intento de notificar al servidor que se detendrá la conexión de cliente. Si el intento de mejor esfuerzo del cliente de Signalr no alcanza el servidor, el servidor eliminará la conexión después de un `DisconnectTimeout` configurable más adelante, momento en el que se activará el evento de `OnDisconnected`. Si el intento de mejor esfuerzo del cliente de Signalr se realiza correctamente, el evento de `OnDisconnected` se activará inmediatamente.

Para obtener información sobre cómo establecer la configuración de `DisconnectTimeout`, consulte [controlar eventos de duración de la conexión: DisconnectTimeout](../guide-to-the-api/handling-connection-lifetime-events.md#disconnecttimeout).

### <a name="connection-limit-reached"></a>Se alcanzó el límite de conexiones

Cuando se usa la versión completa de IIS en un sistema operativo cliente como Windows 7, se impone un límite de 10 conexiones. Al utilizar un sistema operativo de cliente, use IIS Express en su lugar para evitar este límite.

### <a name="cross-domain-connection-not-set-up-properly"></a>La conexión entre dominios no está configurada correctamente

Si una conexión entre dominios (una conexión para la que la dirección URL de Signalr no está en el mismo dominio que la página de hospedaje) no está configurada correctamente, se puede producir un error en la conexión sin un mensaje de error. Para obtener información sobre cómo habilitar la comunicación entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a>La conexión con NTLM (Active Directory) no funciona en el cliente de .NET

Si la conexión no está configurada correctamente, se puede producir un error en una conexión en una aplicación cliente .NET que use la seguridad del dominio. Para usar Signalr en un entorno de dominio, establezca la propiedad de conexión necesaria como se indica a continuación:

**C#código de cliente que implementa credenciales de conexión**

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="pong"></a>

## <a name="configuring-iis-websockets-to-pingpong-to-detect-a-dead-client"></a>Configuración de WebSockets de IIS para ping/pong para detectar un cliente inactivo

Los servidores de signalr no saben si el cliente está inactivo o no y dependen de la notificación del WebSocket subyacente para los errores de conexión, es decir, la devolución de llamada de `OnClose`. Una solución a este problema consiste en configurar WebSockets de IIS para que haga ping/pong automáticamente. Esto garantiza que la conexión se cerrará si se interrumpe inesperadamente. Para obtener más información, consulte [esta entrada de stackoverflow](http://stackoverflow.com/questions/19502755/websocket-clients-state-not-changing-on-network-loss).

<a id="other"></a>

## <a name="other-connection-issues"></a>Otros problemas de conexión

En esta sección se describen las causas y soluciones de los síntomas o mensajes de error específicos que se producen durante una conexión.

### <a name="start-must-be-called-before-data-can-be-sent-error"></a>Error "se debe llamar a iniciar antes de enviar datos"

Este error se suele observar si el código hace referencia a objetos de Signalr antes de que se inicie la conexión. El wireup para los controladores y el like que llamarán a los métodos definidos en el servidor deben agregarse después de que se complete la conexión. Tenga en cuenta que la llamada a `Start` es asincrónica, por lo que el código después de la llamada se puede ejecutar antes de que se complete. La mejor manera de agregar controladores después de que una conexión se inicia por completo es colocarlos en una función de devolución de llamada que se pasa como un parámetro al método de Inicio:

**Código de cliente de JavaScript que agrega correctamente controladores de eventos que hacen referencia a objetos de Signalr**

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

Este error también se verá si se detiene una conexión mientras todavía se hace referencia a los objetos de Signalr.

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a>error "301 se ha cambiado permanentemente" o "302 se ha pasado temporalmente"

Este error puede aparecer si el proyecto contiene una carpeta llamada Signalr, que interferirá con el proxy creado automáticamente. Para evitar este error, no use una carpeta llamada `SignalR` en la aplicación o Active la generación automática de proxy. Vea [el proxy generado y lo que hace para](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) obtener más detalles.

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a>error "403 prohibido" en .NET o en el cliente de Silverlight

Este error puede producirse en entornos entre dominios en los que la comunicación entre dominios no se ha habilitado correctamente. Para obtener información sobre cómo habilitar la comunicación entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain). Para establecer una conexión entre dominios en un cliente de Silverlight, vea [conexiones entre dominios desde clientes de Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).

### <a name="404-not-found-error"></a>error "404 no encontrado"

Hay varias causas para este problema. Compruebe lo siguiente:

- **Referencia de dirección de proxy de concentrador no formateada correctamente:** Este error suele producirse si la referencia a la dirección del proxy del concentrador generada no tiene el formato correcto. Compruebe que la referencia a la dirección del concentrador se ha realizado correctamente. Vea [Cómo hacer referencia al proxy generado dinámicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obtener más información.
- **Agregar rutas a la aplicación antes de agregar la ruta del concentrador:** Si la aplicación utiliza otras rutas, compruebe que la primera ruta agregada es la llamada a `MapSignalR`.
- **Usar IIS 7 o 7,5 sin la actualización para direcciones URL sin extensión:** El uso de IIS 7 o 7,5 requiere una actualización para direcciones URL sin extensión para que el servidor pueda proporcionar acceso a las definiciones de concentrador en `/signalr/hubs`. La actualización se puede encontrar [aquí](https://support.microsoft.com/kb/980368).
- **Caché de IIS no actualizada o dañada:** Para comprobar que el contenido de la memoria caché no está actualizado, escriba el siguiente comando en una ventana de PowerShell para borrar la memoria caché:

    [!code-powershell[Main](troubleshooting/samples/sample11.ps1)]

### <a name="500-internal-server-error"></a>"500 error interno del servidor"

Se trata de un error muy genérico que podría tener una gran variedad de causas. Los detalles del error deben aparecer en el registro de eventos del servidor o pueden encontrarse a través de la depuración del servidor. Se puede obtener información de error más detallada si se activan los errores detallados en el servidor. Para obtener más información, vea [Cómo controlar errores en la clase hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).

Este error también aparece normalmente si un firewall o proxy no está configurado correctamente, lo que provoca que se vuelvan a escribir los encabezados de solicitud. La solución consiste en asegurarse de que el puerto 80 está habilitado en el firewall o el proxy.

### <a name="unexpected-response-code-500"></a>"Código de respuesta inesperado: 500"

Este error puede producirse si la versión de .NET Framework usada en la aplicación no coincide con la versión especificada en Web. config. La solución consiste en comprobar que .NET 4,5 se usa en la configuración de la aplicación y en el archivo Web. config.

### <a name="typeerror-lthubtypegt-is-undefined-error"></a>Error "TypeError: &lt;hubType&gt; no está definido"

Este error se producirá si la llamada a `MapSignalR` no se realiza correctamente. Consulte [Cómo registrar las opciones de middleware de signalr y configuración de signalr](../guide-to-the-api/hubs-api-guide-server.md#route) para obtener más información.

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a>El código de usuario no controló JsonSerializationException

Compruebe que los parámetros que envía a los métodos no incluyen tipos no serializables (como identificadores de archivo o conexiones de bases de datos). Si necesita usar miembros en un objeto del servidor que no desea que se envíe al cliente (ya sea por motivos de seguridad o por motivos de serialización), use el atributo `JSONIgnore`.

### <a name="protocol-error-unknown-transport-error"></a>Error "error de protocolo: transporte desconocido"

Este error puede producirse si el cliente no admite los transportes que utiliza Signalr. Consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports) para obtener información sobre los exploradores que se pueden usar con signalr.

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a>"Se ha deshabilitado la generación de proxy de concentrador de JavaScript".

Este error se producirá si se establece `DisableJavaScriptProxies` y se incluye también una referencia al proxy generado dinámicamente en `signalr/hubs`. Para obtener más información sobre la creación manual del proxy, consulte [el proxy generado y lo que hace](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a>"El identificador de conexión tiene el formato incorrecto" o "el error de identidad de usuario no puede cambiar durante una conexión de Signalr activa"

Este error puede aparecer si se utiliza la autenticación y el cliente se ha cerrado la sesión antes de que se detenga la conexión. La solución consiste en detener la conexión de Signalr antes de cerrar la sesión del cliente.

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a>"Error no detectado: Signalr: no se encontró jQuery. Asegúrese de que se hace referencia a jQuery antes del error del archivo Signalr. js.

El cliente de Signalr JavaScript requiere que jQuery se ejecute. Compruebe que la referencia a jQuery es correcta, que la ruta de acceso usada es válida y que la referencia a jQuery es anterior a la referencia a Signalr.

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a>"No se detectó el TypeError: no se puede leer la propiedad '&lt;la propiedad&gt;' de un error no definido '

Este error se produce porque no se hace referencia correctamente a jQuery o al proxy de los concentradores. Compruebe que la referencia a jQuery y el proxy de hubs es correcta, que la ruta de acceso usada es válida y que la referencia a jQuery es anterior a la referencia al proxy de los concentradores. La referencia predeterminada al proxy de los concentradores debe ser similar a la siguiente:

**Código HTML del lado cliente que hace referencia correctamente al proxy de los concentradores**

[!code-html[Main](troubleshooting/samples/sample12.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a>Error "RuntimeBinderException no controlado por el código de usuario"

Este error puede producirse cuando se utiliza la sobrecarga incorrecta de `Hub.On`. Si el método tiene un valor devuelto, el tipo de valor devuelto debe especificarse como un parámetro de tipo genérico:

**Método definido en el cliente (sin el proxy generado)**

[!code-html[Main](troubleshooting/samples/sample13.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a>El ID. de conexión es incoherente o se interrumpe la conexión entre cargas de páginas

Este comportamiento está diseñado así. Puesto que el objeto de concentrador se hospeda en el objeto de página, el concentrador se destruye cuando se actualiza la página. Una aplicación de varias páginas debe mantener la asociación entre los usuarios y los identificadores de conexión para que sean coherentes entre cargas de páginas. Los identificadores de conexión se pueden almacenar en el servidor en un objeto `ConcurrentDictionary` o en una base de datos.

### <a name="value-cannot-be-null-error"></a>Error "el valor no puede ser nulo"

Actualmente no se admiten los métodos del servidor con parámetros opcionales; Si se omite el parámetro opcional, se producirá un error en el método. Para obtener más información, vea [parámetros opcionales](https://github.com/SignalR/SignalR/issues/324).

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a>"Firefox no puede establecer una conexión con el servidor en &lt;dirección&gt;" error en Firebug

Este mensaje de error puede aparecer en Firebug si se produce un error en la negociación del transporte de WebSocket y en su lugar se usa otro transporte. Este comportamiento está diseñado así.

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a>"El certificado remoto no es válido según el procedimiento de validación" en la aplicación cliente de .NET

Si el servidor requiere certificados de cliente personalizados, puede Agregar un certificado X509 a la conexión antes de que se realice la solicitud. Agregue el certificado a la conexión mediante `Connection.AddClientCertificate`.

### <a name="connection-drops-after-authentication-times-out"></a>La conexión se interrumpe después del tiempo de espera de la autenticación

Este comportamiento está diseñado así. Las credenciales de autenticación no se pueden modificar mientras haya una conexión activa; para actualizar las credenciales, la conexión debe detenerse y reiniciarse.

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a>Se llama a alconnected dos veces al usar jQuery Mobile

la función `initializePage` de jQuery Mobile obliga a que se vuelvan a ejecutar los scripts de cada página, con lo que se crea una segunda conexión. Las soluciones para este problema incluyen:

- Incluya la referencia a jQuery Mobile antes del archivo de JavaScript.
- Deshabilite la función `initializePage` estableciendo `$.mobile.autoInitializePage = false`.
- Espere a que finalice la inicialización de la página antes de iniciar la conexión.

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a>Los mensajes se retrasan en las aplicaciones de Silverlight mediante eventos enviados del servidor

Los mensajes se retrasan cuando se usan eventos enviados por el servidor en Silverlight. Para forzar el uso del sondeo largo en su lugar, use lo siguiente al iniciar la conexión:

[!code-css[Main](troubleshooting/samples/sample14.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a>"Permiso denegado" con el protocolo de trama Forever

Este es un problema conocido, que se describe [aquí](https://github.com/SignalR/SignalR/issues/1963). Este síntoma puede aparecer con la biblioteca de JQuery más reciente. la solución alternativa es cambiar la aplicación a JQuery 1.8.2.

### <a name="invalidoperationexception-not-a-valid-web-socket-request"></a>"InvalidOperationException: no es una solicitud de socket web válida.

Este error puede producirse si se usa el protocolo WebSocket, pero el proxy de red está modificando los encabezados de solicitud. La solución consiste en configurar el proxy para permitir WebSocket en el puerto 80.

### <a name="exception-ltmethod-namegt-method-could-not-be-resolved-when-client-calls-method-on-server"></a>"Excepción: &lt;nombre del método&gt; método no se pudo resolver" cuando el cliente llama al método en el servidor

Este error puede deberse al uso de tipos de datos que no se pueden detectar en una carga JSON, como una matriz. La solución consiste en usar un tipo de datos que sea reconocible por JSON, como IList. Para obtener más información, consulte el [cliente .net no puede llamar a métodos de concentrador con parámetros de matriz](https://github.com/SignalR/SignalR/issues/2672).

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a>Errores de compilación y de servidor

 La siguiente sección contiene posibles soluciones para errores en tiempo de ejecución del compilador y del servidor.

### <a name="reference-to-hub-instance-is-null"></a>La referencia a la instancia de Hub es null

Dado que se crea una instancia de concentrador para cada conexión, no se puede crear una instancia de un concentrador en el código. Para llamar a métodos en un centro desde fuera del propio concentrador, consulte [cómo llamar a métodos de cliente y administrar grupos desde fuera de la clase hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para obtener una referencia al contexto del concentrador.

### <a name="httpcontextcurrentsession-is-null"></a>HTTPContext. Current. Session es null

Este comportamiento está diseñado así. Signalr no admite el estado de sesión ASP.NET, ya que al habilitar el estado de sesión se interrumpiría la mensajería dúplex.

### <a name="no-suitable-method-to-override"></a>No hay ningún método adecuado para invalidar

Es posible que vea este error si utiliza código de documentación o blogs más antiguos. Compruebe que no hace referencia a los nombres de los métodos que han cambiado o han quedado en desuso (como `OnConnectedAsync`).

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a>HostContextExtensions. WebSocketServerUrl es NULL.

Este comportamiento está diseñado así. Este miembro está en desuso y no debe usarse.

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a>"Una ruta denominada ' signalr. hubs ' ya está en el error de la colección de rutas.

Este error se verá si la aplicación llama a `MapSignalR` dos veces. Algunas aplicaciones de ejemplo llaman a `MapSignalR` directamente en la clase startup; otros realizan la llamada en una clase contenedora. Asegúrese de que la aplicación no realiza ambas tareas.

### <a name="websocket-is-not-used"></a>No se usa WebSocket

Si ha comprobado que el servidor y los clientes cumplen los requisitos de WebSocket (que se enumeran en el documento [plataformas admitidas](../getting-started/supported-platforms.md) ), deberá habilitar WebSocket en el servidor. [Aquí](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)encontrará instrucciones para hacerlo.

### <a name="connection-is-undefined"></a>$. la conexión no está definida

Este error indica que los scripts de una página no se están cargando correctamente, o bien que no se puede tener acceso al proxy del concentrador o que se está accediendo incorrectamente. Compruebe que las referencias de script de la página se corresponden con los scripts cargados en el proyecto y que se puede tener acceso a/signalr/hubs en un explorador cuando el servidor se está ejecutando.

### <a name="one-or-more-types-required-to-compile-a-dynamic-expression-cannot-be-found"></a>No se encuentran uno o más tipos necesarios para compilar una expresión dinámica

Este error indica que falta la biblioteca de `Microsoft.CSharp`. Agréguelo en la pestaña **ensamblados-&gt;Framework** .

### <a name="caller-state-cannot-be-accessed-from-clientscaller-in-visual-basic-or-in-a-strongly-typed-hub-conversion-from-type-taskof-object-to-type-string-is-not-valid-error"></a>No se puede tener acceso al estado del llamador desde los clientes. llamador en Visual Basic o en un concentrador fuertemente tipado; El error "conversión del tipo ' tarea (del objeto) ' al tipo ' cadena ' no es válido

Para tener acceso al estado del llamador en Visual Basic o en un concentrador fuertemente tipado, use la propiedad `Clients.CallerState` (introducida en Signalr 2,1) en lugar de `Clients.Caller`.

<a id="vs"></a>

## <a name="visual-studio-issues"></a>Problemas de Visual Studio

En esta sección se describen los problemas que se encuentran en Visual Studio.

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a>El nodo documentos de script no aparece en Explorador de soluciones

Algunos de nuestros tutoriales le dirigen al nodo "documentos de script" en Explorador de soluciones durante la depuración. Este nodo se genera mediante el depurador de JavaScript y solo aparecerá mientras se depuran los clientes del explorador en Internet Explorer. el nodo no aparecerá si se usan Chrome o Firefox. El depurador de JavaScript tampoco se ejecutará si se está ejecutando otro depurador del cliente, como el depurador de Silverlight.

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a>Signalr no funciona en Visual Studio 2008 o versiones anteriores

Este comportamiento está diseñado así. Signalr requiere .NET Framework 4 o posterior; Esto requiere que las aplicaciones de Signalr se desarrollen en Visual Studio 2010 o posterior. El componente de servidor de Signalr requiere .NET Framework 4,5.

<a id="iis"></a>

## <a name="iis-issues"></a>Problemas de IIS

Esta sección contiene problemas con Internet Information Services.

### <a name="signalr-works-on-visual-studio-development-server-but-not-in-iis"></a>Signalr funciona en el servidor de desarrollo de Visual Studio, pero no en IIS

Signalr es compatible con IIS 7,0 y 7,5, pero se debe agregar compatibilidad con direcciones URL sin extensión. Para agregar compatibilidad con direcciones URL sin extensión, vea [https://support.microsoft.com/kb/980368](https://support.microsoft.com/kb/980368)

Signalr requiere que ASP.NET esté instalado en el servidor (de forma predeterminada, ASP.NET no está instalado en IIS). Para instalar ASP.NET, consulte [descargas de ASP.net](https://www.asp.net/downloads).

<a id="azure"></a>

## <a name="microsoft-azure-issues"></a>Problemas de Microsoft Azure

Esta sección contiene problemas con Microsoft Azure.

### <a name="fileloadexception-when-hosting-signalr-in-an-azure-worker-role"></a>FileLoadException al hospedar Signalr en un rol de trabajo de Azure

El hospedaje Signalr en un rol de trabajo de Azure podría producir la excepción "no se pudo cargar el archivo o ensamblado ' Microsoft. Owin, version = 2.0.0.0". Se trata de un problema conocido con NuGet; Las redirecciones de enlace no se agregan automáticamente en los proyectos de rol de trabajo de Azure. Para solucionarlo, puede Agregar las redirecciones de enlace manualmente. Agregue las líneas siguientes al archivo `app.config` para el proyecto de rol de trabajo.

[!code-xml[Main](troubleshooting/samples/sample15.xml)]

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a>Los mensajes no se reciben a través del backplane de Azure después de modificar los nombres de los temas

Los temas usados por el backplane de Azure se mantienen internamente. no pretende ser configurables por el usuario.
