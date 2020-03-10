---
uid: signalr/overview/performance/signalr-performance
title: Rendimiento de signalr | Microsoft Docs
author: bradygaster
description: Rendimiento de SignalR
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: b8a44f4c924c94cdfff1ce7630539b45fe269bbf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449863"
---
# <a name="signalr-performance"></a>Rendimiento de SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo diseñar, medir y mejorar el rendimiento en una aplicación de Signalr.
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

Para obtener una presentación reciente sobre el rendimiento y el escalado de Signalr, consulte [escalado del Web en tiempo real con ASP.net signalr](https://channel9.msdn.com/Events/Build/2013/3-502).

Este tema contiene las siguientes secciones:

- [Consideraciones de diseño](#design)
- [Ajuste del rendimiento del servidor de Signalr](#tuning)
- [Solución de problemas de rendimiento](#troubleshooting)
- [Usar contadores de rendimiento de Signalr](#perfcounters)
- [Usar otros contadores de rendimiento](#othercounters)
- [Otros recursos](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Consideraciones de diseño

En esta sección se describen los patrones que se pueden implementar durante el diseño de una aplicación Signalr, para asegurarse de que el rendimiento no se obstaculiza generando tráfico de red innecesario.

### <a name="throttling-message-frequency"></a>Frecuencia del mensaje de limitación

Incluso en una aplicación que envía mensajes a una frecuencia alta (por ejemplo, una aplicación de juegos en tiempo real), la mayoría de las aplicaciones no necesitan enviar más de unos pocos mensajes por segundo. Para reducir la cantidad de tráfico que genera cada cliente, se puede implementar un bucle de mensajes que pone en cola y envía mensajes con una frecuencia superior a una velocidad fija (es decir, se enviará un número determinado de mensajes cada segundo, si hay mensajes en ese momento en tervalo que se va a enviar). Para obtener una aplicación de ejemplo que limita los mensajes a una determinada tasa (desde el cliente y el servidor), consulte [la alta frecuencia en tiempo real con signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Reducir el tamaño de los mensajes

Puede reducir el tamaño de un mensaje de Signalr reduciendo el tamaño de los objetos serializados. En el código del servidor, si va a enviar un objeto que contiene propiedades que no es necesario transmitir, evite que esas propiedades se serialicen mediante el `JsonIgnore` atributo. Los nombres de las propiedades también se almacenan en el mensaje; los nombres de las propiedades se pueden acortar mediante el atributo `JsonProperty`. En el ejemplo de código siguiente se muestra cómo excluir una propiedad de la que se va a enviar al cliente y cómo acortar los nombres de propiedad:

**Código de servidor .NET que muestra el atributo JsonIgnore para excluir los datos que se envían al cliente y el atributo JsonProperty para reducir el tamaño del mensaje**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Con el fin de conservar la legibilidad y el mantenimiento en el código de cliente, los nombres de propiedad abreviados se pueden reasignar a nombres descriptivos después de recibir el mensaje. En el ejemplo de código siguiente se muestra una posible manera de volver a asignar nombres abreviados a más largos, mediante la definición de un contrato de mensaje (asignación) y el uso de la función `reMap` para aplicar el contrato a la clase de mensaje optimizada:

**Código JavaScript del lado cliente que reasigna nombres de propiedad abreviados a nombres legibles para el usuario.**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Los nombres también se pueden acortar en los mensajes del cliente al servidor, mediante el mismo método.

Reducir la superficie de memoria (es decir, la cantidad de memoria utilizada para el mensaje) del objeto de mensaje también puede mejorar el rendimiento. Por ejemplo, si el intervalo completo de un `int` no es necesario, se puede usar en su lugar un `short` o `byte`.

Puesto que los mensajes se almacenan en el bus de mensajes en la memoria del servidor, la reducción del tamaño de los mensajes también puede solucionar problemas de memoria del servidor.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Ajuste del rendimiento del servidor de Signalr

Se pueden usar las siguientes opciones de configuración para optimizar el servidor y obtener un mejor rendimiento en una aplicación de Signalr. Para obtener información general sobre cómo mejorar el rendimiento en una aplicación de ASP.NET, vea [mejorar el rendimiento de ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).

**Opciones de configuración de signalr**

- **DefaultMessageBufferSize**: de forma predeterminada, signalr conserva 1000 mensajes en memoria por cada concentrador por conexión. Si se usan mensajes grandes, se pueden crear problemas de memoria que se pueden mitigar reduciendo este valor. Esta configuración se puede establecer en el controlador de eventos `Application_Start` en una aplicación ASP.NET o en el método `Configuration` de una clase de inicio OWIN en una aplicación autohospedada. En el ejemplo siguiente se muestra cómo reducir este valor para reducir la superficie de memoria de la aplicación con el fin de reducir la cantidad de memoria del servidor usada:

    **Código de servidor .NET en Startup.cs para reducir el tamaño del búfer de mensajes predeterminado**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Opciones de configuración de IIS**

- Número **máximo de solicitudes simultáneas por aplicación**: el aumento del número de solicitudes de IIS simultáneas aumentará los recursos del servidor disponibles para atender las solicitudes. El valor predeterminado es 5000; para aumentar este valor, ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- **ApplicationPool QueueLength**: este es el número máximo de solicitudes que http. sys pone en cola para el grupo de aplicaciones. Cuando la cola está llena, las nuevas solicitudes reciben una respuesta 503 "servicio no disponible". El valor predeterminado es 1000.

    Acortar la longitud de la cola para el proceso de trabajo en el grupo de aplicaciones que hospeda la aplicación conservará los recursos de memoria. Para obtener más información, vea [Administración, ajuste y configuración de grupos de aplicaciones](https://technet.microsoft.com/library/cc745955.aspx).

**Opciones de configuración de ASP.NET**

En esta sección se incluyen las opciones de configuración que se pueden establecer en el archivo de `aspnet.config`. Este archivo se encuentra en una de las dos ubicaciones, en función de la plataforma:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Entre las opciones de ASP.NET que pueden mejorar el rendimiento de Signalr se incluyen las siguientes:

- **Número máximo de solicitudes simultáneas por CPU**: el aumento de este valor puede aliviar los cuellos de botella de rendimiento. Para aumentar este valor, agregue la siguiente opción de configuración al archivo `aspnet.config`:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Límite**de la cola de solicitudes: cuando el número total de conexiones supera el valor de `maxConcurrentRequestsPerCPU`, ASP.net iniciará las solicitudes de limitación mediante una cola. Para aumentar el tamaño de la cola, puede aumentar el valor de `requestQueueLimit`. Para ello, agregue la siguiente opción de configuración al nodo `processModel` en `config/machine.config` (en lugar de `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Solución de problemas de rendimiento

En esta sección se describen las formas de encontrar cuellos de botella de rendimiento en la aplicación.

### <a name="verifying-that-websocket-is-being-used"></a>Comprobando que se está usando WebSocket

Aunque Signalr puede utilizar una variedad de transportes para la comunicación entre el cliente y el servidor, WebSocket ofrece una ventaja significativa en el rendimiento y debe usarse si el cliente y el servidor lo admiten. Para determinar si el cliente y el servidor cumplen los requisitos de WebSocket, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports). Para determinar qué transporte se usa en la aplicación, puede usar las herramientas de desarrollo del explorador y examinar los registros para ver qué transporte se utiliza para la conexión. Para obtener información sobre el uso de las herramientas de desarrollo del explorador en Internet Explorer y Chrome, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Usar contadores de rendimiento de Signalr

En esta sección se describe cómo habilitar y usar los contadores de rendimiento de Signalr, que se encuentran en el paquete de `Microsoft.AspNet.SignalR.Utils`.

### <a name="installing-signalrexe"></a>Instalando signalr. exe

Los contadores de rendimiento se pueden agregar al servidor mediante una utilidad denominada Signalr. exe. Para instalar esta utilidad, siga estos pasos:

1. En Visual Studio, seleccione **herramientas** > **Administrador de paquetes Nuget** > **administrar paquetes Nuget para la solución** .
2. Busque **signalr. utils**y seleccione instalar.

    ![](signalr-performance/_static/image1.png)
3. Acepte el contrato de licencia para instalar el paquete.
4. Signalr. exe se instalará en `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalación de contadores de rendimiento con Signalr. exe

Para instalar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Para quitar los contadores de rendimiento de Signalr, ejecute Signalr. exe en un símbolo del sistema con privilegios elevados con el siguiente parámetro:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Contadores de rendimiento de signalr

El paquete de utilidades instala los siguientes contadores de rendimiento. Los contadores "total" miden el número de eventos desde el último reinicio del grupo de aplicaciones o del servidor.

**Métricas de conexión**

Las métricas siguientes miden los eventos de duración de la conexión que se producen. Para obtener más información, vea [Descripción y control de eventos de duración](../guide-to-the-api/handling-connection-lifetime-events.md)de la conexión.

- **Conexiones conectadas**
- **Conexiones reconectadas**
- **Conexiones desconectadas**
- **Conexiones actuales**

**Métricas de mensajes**

Las métricas siguientes miden el tráfico de mensajes generado por Signalr.

- **Total de mensajes de conexión recibidos**
- **Total de mensajes de conexión enviados**
- **Mensajes de conexión recibidos/seg.**
- **Mensajes de conexión enviados/seg.**

**Métricas del bus de mensajes**

Las métricas siguientes miden el tráfico a través del bus de mensajes interno de Signalr, la cola en la que se colocan todos los mensajes entrantes y salientes de Signalr. Un mensaje se **publica** cuando se envía o se emite. Un **suscriptor** en este contexto es una suscripción en el bus de mensajes; debe ser igual al número de clientes más el propio servidor. Un **trabajador asignado** es un componente que envía datos a las conexiones activas. un **trabajador ocupado** es aquel que está enviando un mensaje de forma activa.

- **Total de mensajes de bus de mensajes recibidos**
- **Mensajes de bus de mensajes recibidos/seg.**
- **Total de mensajes de bus de mensajes publicados**
- **Mensajes de bus de mensajes publicados/s**
- **Suscriptores de bus de mensajes actuales**
- **Total de suscriptores de Message bus**
- **Suscriptores de Message bus/s**
- **Trabajos asignados del bus de mensajes**
- **Trabajadores ocupados del bus de mensajes**
- **Temas del bus de mensajes actuales**

**Métricas de error**

Las métricas siguientes miden los errores generados por el tráfico de mensajes de Signalr. Los errores de **resolución de concentrador** se producen cuando no se puede resolver un método de concentrador o concentrador. Los errores de **invocación de concentrador** son excepciones que se producen al invocar un método de concentrador. Los errores de **transporte** son errores de conexión producidos durante una solicitud o respuesta http.

- **Errores: todos los totales**
- **Errores: todos/seg.**
- **Errores: total de resolución de concentrador**
- **Errores: resolución de concentrador/s**
- **Errores: total de invocación de concentrador**
- **Errores: invocación de concentrador/s**
- **Errores: total de transporte**
- **Errores: transporte/s**

<a id="scaleout_metrics"></a>

**Métricas de ampliación**

Las métricas siguientes miden el tráfico y los errores generados por el proveedor de ampliación. Un **flujo** en este contexto es una unidad de escalado que usa el proveedor ampliación. se trata de una tabla si se usa SQL Server, un tema si se usa Service Bus y una suscripción si se usa Redis. Cada flujo garantiza operaciones de lectura y escritura ordenadas; un flujo único es un cuello de botella de escala posible, por lo que se puede aumentar el número de flujos para ayudar a reducir el cuello de botella. Si se usan varias secuencias, Signalr distribuirá automáticamente los mensajes (de partición) a través de estos flujos de forma que se asegure de que los mensajes enviados desde cualquier conexión determinada estén en orden.

La configuración [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) controla la longitud de la cola de envío de ampliación mantenida por signalr. Si se establece en un valor mayor que 0, todos los mensajes de una cola de envío se enviarán de uno en uno al backplane de mensajería configurado. Si el tamaño de la cola supera la longitud configurada, las llamadas subsiguientes a SEND producirán un error inmediatamente con una [excepción InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) hasta que el número de mensajes de la cola sea menor que el valor de de nuevo. La puesta en cola está deshabilitada de forma predeterminada porque los Backplanes implementados generalmente tienen su propia cola o control de flujo en su lugar. En el caso de SQL Server, la agrupación de conexiones limita de forma eficaz el número de envíos que se están realizando en un momento dado.

De forma predeterminada, solo se usa una secuencia para SQL Server y Redis, se usan cinco flujos para Service Bus y la puesta en cola está deshabilitada, pero esta configuración se puede cambiar a través de la configuración en SQL Server y Service Bus:

**Código de servidor .NET para configurar el recuento de tablas y la longitud de cola para SQL Server backplane**

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

**Código de servidor .NET para configurar el número de temas y la longitud de la cola para Service Bus backplane**

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

Una secuencia de **almacenamiento en búfer** es aquella que ha entrado en un estado de error. Cuando la secuencia se encuentra en el estado FAULTED, se producirá un error en todos los mensajes enviados al plano posterior hasta que la secuencia deje de producir errores. La **longitud** de la cola de envío es el número de mensajes que se han expuesto pero que todavía no se han enviado.

- **Ampliación mensajes de bus de mensajes recibidos/seg.**
- **Total de secuencias ampliación**
- **Secuencias de ampliación abiertas**
- **Almacenamiento en búfer de secuencias ampliación**
- **Total de errores de ampliación**
- **Errores de ampliación por segundo**
- **Longitud de cola de envío de ampliación**

Para obtener más información sobre lo que están midiendo estos contadores, consulte [signalr ampliación con Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Usar otros contadores de rendimiento

Los siguientes contadores de rendimiento también pueden ser útiles para supervisar el rendimiento de la aplicación.

**Memoria**

- Memoria de .NET CLR\\# bytes en todos los montones (para w3wp)

**ASP.NET**

- ASP. Net\solicitudes actual
- ASP. NET\Queued
- ASP. NET\Rejected

**CPU**

- Tiempo de Information\Processor de procesador

**TCP/IP**

- TCPv6/conexiones establecidas
- TCPv4/conexiones establecidas

**Servicio Web**

- Conexiones web Ftp\conexiones
- Conexiones web Service\Maximum

**Subprocesamiento**

- Bloqueos y subprocesos de CLR de .NET\\n.º de subprocesos lógicos actuales
- Bloqueos y subprocesos de CLR de .NET\\n.º de subprocesos físicos actuales

<a id="otherresources"></a>

## <a name="other-resources"></a>Otros recursos

Para obtener más información sobre la supervisión y optimización del rendimiento de ASP.NET, vea los temas siguientes:

- [Información general acerca del rendimiento de ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Uso de subprocesos de ASP.NET en IIS 7,5, IIS 7,0 e IIS 6,0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [Elemento &lt;applicationPool&gt; (configuración Web)](https://msdn.microsoft.com/library/dd560842.aspx)
