---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Patrón de trabajo centrado en colas (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c73b070f11366e781bcea70ffc84fd49a47d469a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582764"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Patrón de trabajo centrado en colas (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Anteriormente, vimos que el uso de varios servicios puede dar lugar a un acuerdo de nivel de servicio "compuesto", donde el acuerdo de nivel de servicio efectivo de la aplicación es el *producto* de los contratos de nivel de servicio individuales. Por ejemplo, la aplicación Fix it usa sitios web, almacenamiento y SQL Database. Si se produce un error en alguno de estos servicios, la aplicación devolverá un error al usuario.

El almacenamiento en caché es una buena manera de controlar los errores transitorios para el contenido de solo lectura. Pero, ¿qué ocurre si la aplicación necesita trabajar? Por ejemplo, cuando el usuario envía una nueva tarea corregir ti, la aplicación no puede simplemente colocar la tarea en la memoria caché. La aplicación debe escribir la tarea corregir ti en un almacén de datos persistente, por lo que se puede procesar.

Aquí es donde entra el patrón de trabajo centrado en la cola. Este patrón permite el acoplamiento flexible entre un nivel Web y un servicio back-end.

Este es el funcionamiento del patrón. Cuando la aplicación recibe una solicitud, coloca un elemento de trabajo en una cola y devuelve inmediatamente la respuesta. Después, un proceso de back-end independiente extrae los elementos de trabajo de la cola y realiza el trabajo.

El patrón de trabajo centrado en colas es útil para:

- Trabajo que tarda mucho tiempo (latencia alta).
- Trabajo que requiere un servicio externo que podría no estar siempre disponible.
- Trabajo que consume muchos recursos (CPU alta).
- Trabajo que se beneficiaría de la nivelación de tarifas (sujeto a ráfagas de carga repentinas).

## <a name="reduced-latency"></a>Latencia reducida

Las colas son útiles cada vez que se realiza un trabajo lento. Si una tarea tarda unos segundos o más, en lugar de bloquear el usuario final, coloque el elemento de trabajo en una cola. Indique al usuario "Estamos trabajando en él" y, a continuación, use un agente de escucha de la cola para procesar la tarea en segundo plano.

Por ejemplo, al comprar algo en un distribuidor en línea, el sitio web confirma el pedido inmediatamente. Pero eso no significa que sus cosas ya estén en un camión entregado. Colocan una tarea en una cola y, en segundo plano, realizan la comprobación de crédito, preparan los elementos para su envío, etc.

En el caso de escenarios con una latencia breve, el tiempo total de un extremo a otro podría ser más largo mediante una cola, en comparación con realizar la tarea de forma sincrónica. Pero incluso después, las otras ventajas pueden superar este inconveniente.

## <a name="increased-reliability"></a>Mayor confiabilidad

En la versión de corrección que hemos visto hasta ahora, el front-end web está estrechamente acoplado con el SQL Database back-end. Si el servicio SQL Database no está disponible, el usuario recibe un error. Si los reintentos no funcionan (es decir, el error es más que transitorio), lo único que puede hacer es mostrar un error y pedir al usuario que vuelva a intentarlo más tarde.

![Diagrama que muestra un error de front-end web cuando se produce un error de SQL Database back-end](queue-centric-work-pattern/_static/image1.png)

Mediante el uso de colas, cuando un usuario envía una tarea corregir ti, la aplicación escribe un mensaje en la cola. La carga del mensaje es una representación [JSON](http://json.org/) de la tarea. En cuanto se escribe el mensaje en la cola, la aplicación vuelve y muestra inmediatamente un mensaje de operación correcta para el usuario.

Si alguno de los servicios back-end, como la base de datos SQL o el agente de escucha de la cola, se desconectan, los usuarios pueden seguir enviando nuevas tareas de corrección de ti. Los mensajes solo se pondrán en cola hasta que los servicios back-end vuelvan a estar disponibles. En ese momento, los servicios back-end se detectarán en el trabajo pendiente.

![Diagrama que muestra que el front-end web sigue funcionando cuando hay un error de SQL Database](queue-centric-work-pattern/_static/image2.png)

Además, ahora puede agregar más lógica de back-end sin preocuparse por la resistencia del front-end. Por ejemplo, puede que desee enviar un mensaje de correo electrónico o SMS al propietario cada vez que se asigne una nueva corrección. Si el servicio de correo electrónico o SMS deja de estar disponible, puede procesar todo lo demás y, a continuación, colocar un mensaje en una cola independiente para enviar mensajes de correo electrónico o SMS.

Anteriormente, nuestro acuerdo de nivel de servicio efectivo era Web Apps &times; de almacenamiento &times; SQL Database = 99,7%. (Consulte [diseño para sobrevivir a errores](design-to-survive-failures.md)).

Cuando cambiamos la aplicación para usar una cola, el front-end web depende solo de Web Apps y almacenamiento, para un acuerdo de nivel de servicio compuesto del 99,8%. (Tenga en cuenta que las colas forman parte del servicio Azure Storage, por lo que se incluyen en el mismo SLA que el almacenamiento de blobs).

Si necesita incluso mejor que el 99,8%, puede crear dos colas en dos regiones diferentes. Designe uno como el principal y el otro como secundario. En la aplicación, conmute por error a la cola secundaria si la cola principal no está disponible. La posibilidad de que ambos no estén disponibles al mismo tiempo es muy pequeño.

## <a name="rate-leveling-and-independent-scaling"></a>Nivelación de tarifas y escalado independiente

Las colas también son útiles para algo denominado *nivelación de velocidad* o *nivelación de carga*.

Las aplicaciones web a menudo son susceptibles a ráfagas repentinas de tráfico. Aunque puede usar el escalado automático para agregar automáticamente servidores web para controlar el mayor tráfico web, el escalado automático podría no ser capaz de reaccionar lo suficientemente rápido para controlar picos bruscos en la carga. Si los servidores Web pueden descargar parte del trabajo que tienen que hacer escribiendo un mensaje en una cola, pueden administrar más tráfico. Después, un servicio back-end puede leer mensajes de la cola y procesarlos. La profundidad de la cola aumentará o disminuirá a medida que varíe la carga entrante.

Con gran parte del trabajo que tarda mucho tiempo en cargarse en un servicio back-end, el nivel Web puede responder más fácilmente a los picos repentinos del tráfico. Y ahorra dinero porque cualquier cantidad de tráfico determinada puede controlarse con menos servidores Web.

Puede escalar el nivel Web y el servicio back-end de forma independiente. Por ejemplo, puede que necesite tres servidores Web, pero solo un servidor de procesamiento de mensajes en cola. O bien, si está ejecutando una tarea de proceso intensivo en segundo plano, es posible que necesite más servidores back-end.

![](queue-centric-work-pattern/_static/image3.png)

El escalado automático funciona con los servicios back-end, así como con el nivel Web. Puede escalar o reducir verticalmente el número de máquinas virtuales que están procesando las tareas de la cola, en función del uso de CPU de las máquinas virtuales de back-end. O bien, se puede escalar automáticamente en función del número de elementos que hay en una cola. Por ejemplo, puede indicar a escalado automático que intente no mantener más de 10 elementos en la cola. Si la cola tiene más de 10 elementos, el escalado automático agregará máquinas virtuales. Cuando se pone al día, el escalado automático anulará las máquinas virtuales adicionales.

## <a name="adding-queues-to-the-fix-it-application"></a>Agregar colas a la aplicación Fix it

Para implementar el patrón de cola, es necesario realizar dos cambios en la aplicación Fix it.

- Cuando un usuario envía una nueva tarea corregir ti, coloca la tarea en la cola, en lugar de escribirla en la base de datos.
- Cree un servicio back-end que procese los mensajes de la cola.

Para la cola, usaremos el [servicio Azure Queue Storage](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Otra opción es usar [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Para decidir qué servicio de cola usar, tenga en cuenta cómo la aplicación debe enviar y recibir los mensajes en la cola:

- Si tiene productores cooperativos y consumidores de la competencia, considere la posibilidad de usar el servicio Azure Queue Storage. "Productores cooperativos" significa que varios procesos están agregando mensajes a una cola. "Consumidores de la competencia" significa que varios procesos extraen los mensajes de la cola para procesarlos, pero cualquier mensaje determinado solo puede ser procesado por un "consumidor". Si necesita más rendimiento de lo que puede obtener con una sola cola, use colas adicionales o cuentas de almacenamiento adicionales.
- Si necesita un [modelo de publicación/suscripción](http://en.wikipedia.org/wiki/Publish/subscribe), considere la posibilidad de usar colas de Azure Service Bus.

La aplicación Fix it se adapta al modelo de productores cooperativos y consumidores de la competencia.

Otra consideración es la disponibilidad de las aplicaciones. El servicio de Queue Storage forma parte del mismo servicio que estamos usando para el almacenamiento de blobs, por lo que su uso no tiene ningún efecto en nuestro contrato de nivel de servicio. Azure Service Bus es un servicio independiente con su propio SLA. Si usamos Service Bus colas, tendríamos que tener en cuenta un porcentaje adicional de SLA y nuestro contrato de nivel de servicio compuesto sería inferior. Cuando elija un servicio de cola, asegúrese de que comprende el impacto de su elección en la disponibilidad de la aplicación. Para obtener más información, vea la sección [recursos](#resources) .

## <a name="creating-queue-messages"></a>Crear mensajes de cola

Para colocar una tarea corregir ti en la cola, el front-end web realiza los siguientes pasos:

1. Cree una instancia de [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) . La instancia de `CloudQueueClient` se utiliza para ejecutar solicitudes en el servicio cola.
2. Cree la cola, si aún no existe.
3. Serialice la tarea corregir ti.
4. Llame a [CloudQueue. AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar el mensaje en la cola.

Haremos este trabajo en el constructor y `SendMessageAsync` método de una nueva clase de `FixItQueueManager`.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aquí vamos a usar la biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para serializar el FixIt en formato JSON. Puede usar cualquier enfoque de serialización que prefiera. JSON tiene la ventaja de ser legible y es menos detallado que XML.

El código de calidad de producción agregaría lógica de control de errores, pausaría si la base de datos deja de estar disponible, controlaría la recuperación de forma más limpia, crearía la cola al iniciarse la aplicación y administraría[mensajes "dudosos"](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Un mensaje dudoso es un mensaje que no se puede procesar por alguna razón. No quiere que los mensajes dudosos se colocan en la cola, en los que el rol de trabajo intentará procesarlos continuamente, producirá un error, lo intentará de nuevo, se producirá un error, etc.).

En la aplicación de MVC de front-end, es necesario actualizar el código que crea una nueva tarea. En lugar de colocar la tarea en el repositorio, llame al método `SendMessageAsync` mostrado anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Procesar mensajes de cola

Para procesar los mensajes en la cola, vamos a crear un servicio back-end. El servicio back-end ejecutará un bucle infinito que lleve a cabo los pasos siguientes:

1. Obtiene el siguiente mensaje de la cola.
2. Deserializa el mensaje en una tarea corregir ti.
3. Escriba la tarea corregir ti en la base de datos.

Para hospedar el servicio back-end, crearemos un servicio en la nube de Azure que contenga un *rol de trabajo*. Un rol de trabajo consta de una o más máquinas virtuales que pueden realizar el procesamiento de back-end. El código que se ejecuta en estas máquinas virtuales extraerá los mensajes de la cola a medida que estén disponibles. Para cada mensaje, deserializará la carga de JSON y escribirá una instancia de la entidad de tarea Fix it en la base de datos, usando el mismo repositorio que se usó anteriormente en el nivel Web.

En los pasos siguientes se muestra cómo agregar un proyecto de rol de trabajo a una solución que tiene un proyecto web estándar. Estos pasos ya se han realizado en el proyecto de corrección de ti que puede descargar.

En primer lugar, agregue un proyecto de servicio en la nube a la solución de Visual Studio. Haga clic con el botón derecho en la solución y seleccione **Agregar**y **nuevo proyecto**. En el panel izquierdo, expanda **Visual C#**  y seleccione **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

En el cuadro de diálogo **nuevo servicio** en la nube de Azure, expanda el nodo **Visual C#**  en el panel izquierdo. Seleccione **rol de trabajo** y haga clic en el icono de flecha derecha.

![](queue-centric-work-pattern/_static/image6.png)

(Tenga en cuenta que también puede Agregar un *rol Web*. Podríamos ejecutar el front-end de corrección en el mismo servicio en la nube en lugar de ejecutarlo en un sitio web de Azure. Esto tiene algunas ventajas en el hecho de que las conexiones entre el front-end y el back-end sean más fáciles de coordinar. Sin embargo, para simplificar esta demostración, se mantiene el front-end en una Azure App Service aplicación web y solo se ejecuta el back-end en un servicio en la nube).

Se asigna un nombre predeterminado al rol de trabajo. Para cambiar el nombre, mantenga el mouse sobre el rol de trabajo en el panel de la derecha y, a continuación, haga clic en el icono de lápiz.

![](queue-centric-work-pattern/_static/image7.png)

Haga clic en **Aceptar** para completar el cuadro de diálogo. Esto agrega dos proyectos a la solución de Visual Studio.

- un proyecto de Azure que define el servicio en la nube, incluida la información de configuración.
- Un proyecto de rol de trabajo que define el rol de trabajo.

![](queue-centric-work-pattern/_static/image8.png)

Para obtener más información, vea [creación de un proyecto de Azure con Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dentro del rol de trabajo, sondeamos los mensajes mediante una llamada al método `ProcessMessageAsync` de la clase `FixItQueueManager` que vimos anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

El método `ProcessMessagesAsync` comprueba si hay un mensaje en espera. Si hay alguno, deserializa el mensaje en una entidad `FixItTask` y guarda la entidad en la base de datos. Se repite hasta que la cola esté vacía.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

El sondeo de mensajes de la cola incurre en un gasto de transacciones pequeño, por lo que cuando no hay ningún mensaje en espera para su procesamiento, el método `RunAsync` del rol de trabajo espera un segundo antes de sondear de nuevo llamando a `Task.Delay(1000)`.

En un proyecto Web, la adición de código asincrónico puede mejorar automáticamente el rendimiento, ya que IIS administra un grupo de subprocesos limitado. Este no es el caso de un proyecto de rol de trabajo. Para mejorar la escalabilidad del rol de trabajo, puede escribir código multiproceso o usar código asincrónico para implementar la programación en [paralelo](https://msdn.microsoft.com/library/ff963553.aspx). En el ejemplo no se implementa la programación en paralelo, sino que se muestra cómo hacer que el código sea asincrónico para poder implementar la programación en paralelo.

## <a name="summary"></a>Resumen

En este capítulo, ha visto cómo mejorar la capacidad de respuesta de las aplicaciones, la confiabilidad y la escalabilidad implementando el patrón de trabajo centrado en cola.

Este es el último de los 13 patrones descritos en este libro electrónico, pero hay muchos otros patrones y prácticas que pueden ayudarle a crear aplicaciones en la nube que se ejecutan correctamente. En el [capítulo final](more-patterns-and-guidance.md) se proporcionan vínculos a recursos de temas que no se han tratado en estos 13 patrones.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información acerca de las colas, vea los siguientes recursos.

Documentación:

- [Microsoft Azure Storage colas parte 1: Introducción](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Artículo por Roman Schacherl.
- [Ejecutar tareas en segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), capítulo 5 de [mover aplicaciones a la nube, tercera edición](https://msdn.microsoft.com/library/ff728592.aspx) de patrones y prácticas de Microsoft. (En concreto, la sección ["uso de colas de Azure Storage"](https://msdn.microsoft.com/library/ff803365.aspx#sec7)).
- [Prácticas recomendadas para maximizar la escalabilidad y la rentabilidad de las soluciones de mensajería basadas en colas en Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Notas del producto de Valery Mizonov.
- [Comparación de las colas de Azure y las colas de Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). En el artículo de MSDN Magazine, se proporciona información adicional que puede ayudarle a elegir el servicio de cola que se va a usar. En el artículo se menciona que Service Bus depende de ACS para la autenticación, lo que significa que las colas de SB no estarán disponibles cuando ACS no esté disponible. Sin embargo, desde que se escribió el artículo, se modificó SB para que pueda usar [tokens de SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como alternativa a ACS.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte patrón de mensajería asincrónica, patrón de canalizaciones y filtros, patrón de transacción de compensación, patrón de consumidores en competencia, patrón CQRS.
- [Viaje CQRS](https://msdn.microsoft.com/library/jj554200). Libro electrónico sobre CQRS por patrones y prácticas de Microsoft.

Vídeo:

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de vídeos de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para ver una introducción al servicio Azure Storage y las colas, consulte el episodio 5 a partir de 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Siguiente](more-patterns-and-guidance.md)
