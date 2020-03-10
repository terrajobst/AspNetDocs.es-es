---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Usar métodos asincrónicos en ASP.NET 4,5 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET asincrónica con Visual Studio Express 2012 para Web, que es una versión gratuita...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 7abc3d7acc60d7d868958f2a313bc408f96c95a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507169"
---
# <a name="using-asynchronous-methods-in-aspnet-45"></a>Usar métodos asincrónicos en ASP.NET 4.5

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET asincrónica mediante [Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/11), que es una versión gratuita de Microsoft Visual Studio. También puede usar [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). En este tutorial se incluyen las secciones siguientes.
> 
> - [Cómo procesa el grupo de subprocesos las solicitudes](#HowRequestsProcessedByTP)
> - [Elegir métodos sincrónicos o asincrónicos](#ChoosingSyncVasync)
> - [La aplicación de ejemplo](#SampleApp)
> - [Página sincrónica de Gizmos](#GizmosSynch)
> - [Crear una página Gizmos asincrónica](#CreatingAsynchGizmos)
> - [Realizar varias operaciones en paralelo](#Parallel)
> - [Usar un token de cancelación](#CancelToken)
> - [Configuración del servidor para las llamadas de servicio Web de alta simultaneidad y latencia alta](#ServerConfig)
> 
> Se proporciona un ejemplo completo para este tutorial en  
> [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) en el sitio de [GitHub](https://github.com/) .

Las páginas web de ASP.NET 4,5 de la combinación [.net 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) permiten registrar métodos asincrónicos que devuelven un objeto de tipo [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). El .NET Framework 4 presentó un concepto de programación asincrónica denominado [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) y ASP.net 4,5 admite la [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Las tareas se representan mediante el tipo de **tarea** y los tipos relacionados en el espacio de nombres [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . El .NET Framework 4,5 se basa en esta compatibilidad asincrónica con las palabras clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) que hacen que el trabajo con objetos de [tarea](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) sea mucho menos complejo que los enfoques asincrónicos anteriores. La palabra clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) es una abreviatura sintáctica para indicar que un fragmento de código debe esperar de forma asincrónica en algún otro fragmento de código. La palabra clave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) representa una sugerencia que se puede usar para marcar métodos como métodos asincrónicos basados en tareas. La combinación de **Await**, **Async**y el objeto de **tarea** hace que sea mucho más fácil escribir código asincrónico en .net 4,5. El nuevo modelo de métodos asincrónicos se denomina *patrón asincrónico basado en tareas* (**TAP**). En este tutorial se supone que está familiarizado con los programas asincrónicos mediante las palabras clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y el espacio de nombres [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Para obtener más información sobre el uso de las palabras clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y el espacio de nombres [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , vea las referencias siguientes.

- [Notas del producto: asincronía en .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Preguntas más frecuentes sobre Async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programación asincrónica de Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Cómo procesa el grupo de subprocesos las solicitudes

En el servidor Web, el .NET Framework mantiene un grupo de subprocesos que se usan para atender las solicitudes de ASP.NET. Cuando se recibe una solicitud, se envía un subproceso del grupo para procesarla. Si la solicitud se procesa de forma sincrónica, el subproceso que procesa la solicitud está ocupado mientras se procesa la solicitud y ese subproceso no puede atender a otra solicitud.   
  
Esto podría no ser un problema, porque el grupo de subprocesos puede ser lo suficientemente grande como para alojar muchos subprocesos ocupados. Sin embargo, el número de subprocesos del grupo de subprocesos es limitado (el máximo predeterminado para .NET 4,5 es 5.000). En aplicaciones de gran tamaño con una gran simultaneidad de solicitudes de ejecución prolongada, todos los subprocesos disponibles podrían estar ocupados. Esta condición se conoce como colapso de subprocesos. Cuando se alcanza esta condición, el servidor web pone en cola las solicitudes. Si la cola de solicitudes se llena, el servidor Web rechazará las solicitudes con un Estado HTTP 503 (servidor demasiado ocupado). El grupo de subprocesos de CLR tiene limitaciones en las nuevas inyecciones de subprocesos. Si la simultaneidad es más grande (es decir, el sitio web puede obtener repentinamente un gran número de solicitudes) y todos los subprocesos de solicitud disponibles están ocupados debido a llamadas de back-end con latencia elevada, la velocidad de inserción de subprocesos limitada puede hacer que la aplicación responda muy mal. Además, cada nuevo subproceso agregado al grupo de subprocesos tiene una sobrecarga (por ejemplo, 1 MB de memoria de pila). Una aplicación web que usa métodos sincrónicos para atender las llamadas de latencia alta en las que el grupo de subprocesos crece hasta el máximo predeterminado de 5, 000 subprocesos consumen aproximadamente 5 GB de memoria más que una aplicación capaz de atender las mismas solicitudes con 4,5. métodos asincrónicos y solo 50 subprocesos. Cuando está realizando el trabajo asincrónico, no siempre se usa un subproceso. Por ejemplo, al realizar una solicitud de servicio Web asincrónica, ASP.NET no usará ningún subproceso entre la llamada al método **asincrónico** y el **Await**. El uso del grupo de subprocesos para atender las solicitudes con latencia alta puede conducir a una gran cantidad de memoria y un uso deficiente del hardware del servidor.

## <a name="processing-asynchronous-requests"></a>Procesar solicitudes asincrónicas

En las aplicaciones web que ven un gran número de solicitudes simultáneas en el inicio o que tienen una carga elevada (donde la simultaneidad aumenta repentinamente), realizar llamadas asincrónicas a servicios web aumentará la capacidad de respuesta de la aplicación. Una solicitud asincrónica tarda el mismo tiempo en procesarse que una sincrónica. Por ejemplo, si una solicitud realiza una llamada de servicio Web que requiere dos segundos para completarse, la solicitud tarda dos segundos si se realiza de forma sincrónica o asincrónica. Sin embargo, durante una llamada asincrónica, no se impide que un subproceso responda a otras solicitudes mientras espera a que se complete la primera solicitud. Por consiguiente, las solicitudes asincrónicas impiden la puesta en cola de solicitudes y el crecimiento del grupo de subprocesos cuando hay muchas solicitudes simultáneas que invocan operaciones de ejecución prolongada.

## <a id="ChoosingSyncVasync"></a>Elegir métodos sincrónicos o asincrónicos

En esta sección se enumeran las directrices sobre Cuándo usar métodos sincrónicos o asincrónicos. Estas son simplemente instrucciones; examine cada aplicación individualmente para determinar si los métodos asincrónicos ayudan a mejorar el rendimiento.

En general, use métodos sincrónicos para las siguientes condiciones:

- Las operaciones son simples o de ejecución breve.
- La simplicidad es más importante que la eficacia.
- Las operaciones son principalmente operaciones de la CPU y no operaciones que requieren una gran sobrecarga del disco o de la red. El uso de métodos asincrónicos en operaciones enlazadas a la CPU no proporciona ninguna ventaja y produce una mayor sobrecarga.

En general, use métodos asincrónicos para las siguientes condiciones:

- Está llamando a servicios que se pueden consumir mediante métodos asincrónicos y usa .NET 4,5 o superior.
- Las operaciones están relacionadas con la red o con E/S y no con la CPU.
- El paralelismo es más importante que la simplicidad de código.
- Se desea proporcionar un mecanismo que permita al usuario cancelar solicitudes de ejecución prolongada.
- Cuando la ventaja de cambiar subprocesos supera el costo del cambio de contexto. En general, debe hacer que un método sea asincrónico si el método sincrónico bloquea el subproceso de solicitud ASP.NET mientras no realiza ningún trabajo. Al hacer que la llamada sea asincrónica, el subproceso de solicitud ASP.NET no se bloquea sin ningún trabajo mientras espera a que se complete la solicitud de servicio Web.
- Las pruebas muestran que las operaciones de bloqueo son un cuello de botella en el rendimiento del sitio y que IIS puede atender más solicitudes mediante métodos asincrónicos para estas llamadas de bloqueo.

  En el ejemplo descargable se muestra cómo usar los métodos asincrónicos de forma eficaz. El ejemplo proporcionado se diseñó para proporcionar una demostración sencilla de la programación asincrónica en ASP.NET 4,5. El ejemplo no pretende ser una arquitectura de referencia para la programación asincrónica en ASP.NET. El programa de ejemplo llama a los métodos de [ASP.net web API](../../../web-api/index.md) que, a su vez, llaman a [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular llamadas a servicios Web de ejecución prolongada. La mayoría de las aplicaciones de producción no mostrarán tales ventajas obvias en el uso de métodos asincrónicos.   
  
Pocas aplicaciones requieren que todos los métodos sean asincrónicos. A menudo, convertir algunos métodos sincrónicos en métodos asincrónicos proporciona el mejor aumento de la eficacia para la cantidad de trabajo necesario.

## <a id="SampleApp"></a>La aplicación de ejemplo

Puede descargar la aplicación de ejemplo desde [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) en el sitio de [GitHub](https://github.com/) . El repositorio consta de tres proyectos:

- *WebAppAsync*: el proyecto de formularios web forms de ASP.net que usa el servicio **WEBAPIPWG** de la API Web. La mayor parte del código de este tutorial procede de este proyecto.
- *WebAPIpgw*: el proyecto de API Web de ASP.NET MVC 4 que implementa los controladores de `Products, Gizmos and Widgets`. Proporciona los datos para el proyecto *WebAppAsync* y el proyecto *Mvc4Async* .
- *Mvc4Async*: el proyecto ASP.NET MVC 4 que contiene el código usado en otro tutorial. Realiza llamadas API Web al servicio **WebAPIpwg** .

## <a id="GizmosSynch"></a>Página sincrónica de Gizmos

 En el código siguiente se muestra el método sincrónico `Page_Load` que se usa para mostrar una lista de Gizmos. (En este artículo, un Gizmo es un dispositivo mecánico ficticio). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

En el código siguiente se muestra el método `GetGizmos` del servicio Gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

El método `GizmoService GetGizmos` pasa un URI a un servicio ASP.NET Web API HTTP que devuelve una lista de datos de Gizmos. El proyecto *WebAPIpgw* contiene la implementación de la API Web `gizmos, widget` y los controladores de `product`.  
En la imagen siguiente se muestra la página Gizmos del proyecto de ejemplo.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Crear una página Gizmos asincrónica

En el ejemplo se usan las nuevas palabras clave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (disponibles en .net 4,5 y Visual Studio 2012) para permitir que el compilador sea responsable de mantener las complejas transformaciones necesarias para la programación asincrónica. El compilador permite escribir código mediante C#las construcciones de flujo de control sincrónicos y el compilador aplica automáticamente las transformaciones necesarias para utilizar las devoluciones de llamada con el fin de evitar el bloqueo de subprocesos.

Las páginas asincrónicas de ASP.NET deben incluir la directiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) con el atributo `Async` establecido en "true". En el código siguiente se muestra la directiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) con el atributo `Async` establecido en "true" para la página *GizmosAsync. aspx* .

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

En el código siguiente se muestra el método de `Page_Load` sincrónico `Gizmos` y la página `GizmosAsync` asincrónica. Si el explorador admite el [elemento HTML 5 &lt;mark&gt;](http://www.w3.org/wiki/HTML/Elements/mark), verá los cambios en `GizmosAsync` en amarillo resaltado.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

La versión asincrónica:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Se aplicaron los siguientes cambios para permitir que la página `GizmosAsync` sea asincrónica.

- La directiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) debe tener el atributo `Async` establecido en "true".
- El método `RegisterAsyncTask` se utiliza para registrar una tarea asincrónica que contiene el código que se ejecuta de forma asincrónica.
- El nuevo método de `GetGizmosSvcAsync` se marca con la palabra clave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , que indica al compilador que genere devoluciones de llamada para partes del cuerpo y que cree automáticamente una `Task` devuelta.
- &quot;Async&quot; se agregó al nombre del método asincrónico. Anexar "Async" no es necesario, pero es la Convención al escribir métodos asincrónicos.
- El tipo de valor devuelto del nuevo método `GetGizmosSvcAsync` es `Task`. El tipo de valor devuelto de `Task` representa el trabajo en curso y proporciona a los llamadores del método un identificador a través del cual se espera la finalización de la operación asincrónica.
- La palabra clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) se aplicó a la llamada al servicio Web.
- Se llamó a la API del servicio Web asincrónico (`GetGizmosAsync`).

Dentro del cuerpo del método `GetGizmosSvcAsync` otro método asincrónico, se llama a `GetGizmosAsync`. `GetGizmosAsync` devuelve inmediatamente un `Task<List<Gizmo>>` que se completará finalmente cuando los datos estén disponibles. Dado que no desea hacer nada más hasta que tenga los datos de Gizmo, el código espera la tarea (mediante la palabra clave **Await** ). Solo puede utilizar la palabra clave **Await** en métodos anotados con la palabra clave **Async** .

La palabra clave **Await** no bloquea el subproceso hasta que se completa la tarea. Registra el resto del método como una devolución de llamada en la tarea y devuelve inmediatamente. Cuando se complete la tarea esperada, se invocará esa devolución de llamada y, por tanto, se reanudará la ejecución del método justo donde se quedó. Para obtener más información sobre el uso de las palabras clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) y [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) y el espacio de nombres [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , vea [referencias asincrónicas](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

El siguiente código muestra los métodos `GetGizmos` y `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Los cambios asincrónicos son similares a los que se realizaron en el **GizmosAsync** anterior. 

- La firma del método se anotó con la palabra clave [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , el tipo de valor devuelto se cambió a `Task<List<Gizmo>>`y *Async* se anexó al nombre del método.
- La clase [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) asincrónica se usa en lugar de la clase [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) sincrónica.
- La palabra clave [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) se aplicó al método asincrónico[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) de [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx).

En la imagen siguiente se muestra la vista Gizmo asincrónica.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La presentación de los exploradores de los datos de Gizmos es idéntica a la vista creada por la llamada sincrónica. La única diferencia es que la versión asincrónica puede ser más eficaz en cargas pesadas.

## <a name="registerasynctask-notes"></a>Notas de RegisterAsyncTask

Los métodos enlazados con `RegisterAsyncTask` se ejecutarán inmediatamente después de la [representación](https://msdn.microsoft.com/library/ms178472.aspx)previa. También puede utilizar eventos de página de void void directamente, como se muestra en el código siguiente:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

El inconveniente de los eventos void Async es que los desarrolladores ya no tienen control total sobre cuándo se ejecutan los eventos. Por ejemplo, si es. aspx y. La definición maestra `Page_Load` eventos y uno o ambos son asíncronos, no se puede garantizar el orden de ejecución. Se aplica el mismo orden indeterminiate para los controladores que no son de eventos (como `async void Button_Click`). Para la mayoría de los desarrolladores, debería ser aceptable, pero aquellos que necesiten un control total sobre el orden de ejecución solo deben usar las API como `RegisterAsyncTask` que consumen los métodos que devuelven un objeto de tarea.

## <a id="Parallel"></a>Realizar varias operaciones en paralelo

Los métodos asincrónicos tienen una ventaja significativa con respecto a los métodos sincrónicos cuando una acción debe realizar varias operaciones independientes. En el ejemplo proporcionado, la página sincrónica *PWG. aspx*(para productos, widgets y Gizmos) muestra los resultados de tres llamadas al servicio web para obtener una lista de productos, widgets y Gizmos. El proyecto de [ASP.net web API](../../../web-api/index.md) que proporciona estos servicios usa [Task. Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) para simular latencia o llamadas de red lentas. Cuando el retraso se establece en 500 milisegundos, la página *PWGasync. aspx* asincrónica tarda un poco más de 500 milisegundos en completarse mientras la versión de `PWG` sincrónica tarda en 1.500 milisegundos. En el código siguiente se muestra la página sincrónicas de *PWG. aspx* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

A continuación se muestra el código de `PWGasync` asincrónico subyacente.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

En la imagen siguiente se muestra la vista devuelta desde la página asincrónica *PWGasync. aspx* .

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Usar un token de cancelación

Los métodos asincrónicos que devuelven `Task`son cancelables, es decir, toman un parámetro [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) cuando se proporciona uno con el atributo `AsyncTimeout` de la directiva [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) . En el código siguiente se muestra la página *GizmosCancelAsync. aspx* con un tiempo de espera de en el segundo.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

En el código siguiente se muestra el archivo *GizmosCancelAsync.aspx.CS* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

En la aplicación de ejemplo proporcionada, al seleccionar el vínculo *GizmosCancelAsync* se llama a la página *GizmosCancelAsync. aspx* y se muestra la cancelación (agotando el tiempo de espera) de la llamada asincrónica. Dado que el tiempo de retraso está dentro de un intervalo aleatorio, puede que tenga que actualizar la página un par de veces para obtener el mensaje de error de tiempo de espera.

## <a id="ServerConfig"></a>Configuración del servidor para las llamadas de servicio Web de alta simultaneidad y latencia alta

Para obtener las ventajas de una aplicación web asincrónica, es posible que tenga que realizar algunos cambios en la configuración predeterminada del servidor. Tenga en cuenta lo siguiente al configurar y realizar pruebas de esfuerzo de la aplicación web asincrónica.

- Windows 7, Windows Vista, Window 8 y todos los sistemas operativos de cliente de Windows tienen un máximo de 10 solicitudes simultáneas. Necesitará un sistema operativo Windows Server para ver las ventajas de los métodos asincrónicos con una carga elevada.
- Registre .NET 4,5 con IIS desde un símbolo del sistema con privilegios elevados mediante el comando siguiente:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  Consulte [ASP.net IIS registration Tool (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Es posible que tenga que aumentar el límite de la cola [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) del valor predeterminado de 1.000 a 5.000. Si el valor es demasiado bajo, es posible que vea las solicitudes de rechazo [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) con un estado http 503. Para cambiar el límite de la cola HTTP. sys:

    - Abra el administrador de IIS y navegue hasta el panel grupos de aplicaciones.
    - Haga clic con el botón derecho en el grupo de aplicaciones de destino y seleccione **Configuración avanzada**.  
        ](using-asynchronous-methods-in-aspnet-45/_static/image4.png) de ![avanzadas
    - En el cuadro de diálogo **Configuración avanzada** , cambie la longitud de la *cola* de 1.000 a 5.000.  
        ![longitud de cola](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Tenga en cuenta que en las imágenes anteriores, .NET Framework se muestra como v 4.0, aunque el grupo de aplicaciones use .NET 4,5. Para comprender esta discrepancia, consulte lo siguiente:

- [Control de versiones de .NET y compatibilidad con múltiples versiones: .NET 4,5 es una actualización local a .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Cómo establecer una aplicación de IIS o AppPool para usar ASP.NET 3,5 en lugar de 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [Versiones y dependencias de .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Si su aplicación usa servicios web o System.NET para comunicarse con un back-end a través de HTTP, puede que tenga que aumentar el elemento [connectionManagement/MaxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . En el caso de las aplicaciones ASP.NET, está limitado por la característica AutoConfig a 12 veces el número de CPU. Esto significa que en un proceso cuádruple, puede tener como máximo 12 \* 4 = 48 conexiones simultáneas a un extremo de IP. Dado que está vinculado a [AutoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), la manera más fácil de aumentar `maxconnection` en una aplicación ASP.net es establecer [System .net. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) mediante programación en el método from `Application_Start` en el archivo *global. asax* . Vea la descarga de ejemplo para obtener un ejemplo.
- En .NET 4,5, el valor predeterminado de 5000 para [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) debe ser correcto.

## <a name="contributors"></a>Colaboradores

- [Broderick de exacciones](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei GE](https://blogs.msdn.com/b/hongmeig/)
