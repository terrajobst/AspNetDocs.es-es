---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Control de errores transitorios (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: fc281e3d8f7c9edd4d98b029a67e58113132a8b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583654"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Control de errores transitorios (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Al diseñar una aplicación en la nube real, una de las cosas que tiene que pensar es cómo controlar las interrupciones temporales del servicio. Este problema es muy importante en las aplicaciones en la nube porque depende de las conexiones de red y los servicios externos. Con frecuencia, puede obtener pocos problemas que suelen ser de recuperación automática y, si no está preparado para controlarlos de forma inteligente, darán lugar a una mala experiencia para los clientes.

## <a name="causes-of-transient-failures"></a>Causas de errores transitorios

En el entorno de nube, observará que las conexiones de base de datos con errores y que se han quitado se producen periódicamente. Eso es en parte porque está pasando por más equilibradores de carga en comparación con el entorno local en el que el servidor Web y el servidor de base de datos tienen una conexión física directa. Además, en ocasiones, cuando dependa de un servicio multiinquilino, verá que las llamadas al servicio se ralentizarán o se agotará el tiempo de espera porque otra persona que use el servicio está llegando al alcance. En otros casos, podría ser el usuario que está llegando a un servicio con demasiada frecuencia y el servicio lo limita deliberadamente (deniega las conexiones) con el fin de evitar que afecte negativamente a otros inquilinos del servicio.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Usar la lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios

En lugar de producir una excepción y mostrar una página de error o no disponible para el cliente, puede reconocer los errores que suelen ser transitorios y reintentar automáticamente la operación que produjo el error, en espera de que se realice correctamente. La mayoría de las veces, la operación se realizará correctamente en el segundo intento y se recuperará del error sin que el cliente haya tenido en cuenta que se ha producido un problema.

Hay varias maneras de implementar lógica de reintento inteligente.

- El grupo Microsoft Patterns &amp; Practices tiene un [bloque de aplicaciones de control de errores transitorios](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) que hace todo lo necesario si utiliza ADO.net para el acceso de SQL Database (no a través de Entity Framework). Solo tiene que establecer una directiva para los reintentos: el número de veces que se vuelve a intentar una consulta o un comando, y cuánto tiempo se debe esperar entre los intentos, y ajustar el código SQL en un bloque *using* .

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH también es compatible con [Azure in-role cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) y [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Cuando se usa el Entity Framework normalmente no se trabaja directamente con conexiones SQL, por lo que no puede usar este paquete de patrones y prácticas, pero Entity Framework 6 crea este tipo de lógica de reintento en el marco de trabajo. De forma similar, se especifica la estrategia de reintento y, a continuación, EF usa esa estrategia cada vez que tiene acceso a la base de datos.

    Para usar esta característica en la aplicación Fix it, lo único que debemos hacer es agregar una clase que derive de *DbConfiguration* y activar la lógica de reintento.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    En el caso de SQL Database excepciones que el marco de trabajo identifica como errores transitorios, el código mostrado indica a EF que vuelva a intentar la operación hasta 3 veces, con un retraso de retroceso exponencial entre reintentos y un retraso máximo de 5 segundos. El retroceso exponencial significa que, después de cada reintento erróneo, esperará durante un período de tiempo más largo antes de volver a intentarlo. Si se produce un error en tres intentos en una fila, se producirá una excepción. En la sección siguiente sobre los disyuntores se explica por qué se desea una interrupción exponencial y un número limitado de reintentos.

    Puede tener problemas similares al usar el servicio de Azure Storage, ya que la aplicación de corrección de TI para blobs, y la API de cliente de almacenamiento de .NET ya implementa el mismo tipo de lógica. Solo tiene que especificar la Directiva de reintentos, o incluso no tiene que hacerlo si está satisfecho con la configuración predeterminada.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disyuntores

Hay varias razones por las que no desea volver a intentarlo demasiadas veces durante un período de tiempo:

- Demasiados usuarios que reintentan de forma persistente las solicitudes con error pueden degradar la experiencia de otros usuarios. Si millones de personas realizan solicitudes de reintento repetidas, podría estar enlazando colas de distribución de IIS y evitar que la aplicación atienda las solicitudes que, de otro modo, podría administrar correctamente.
- Si todo el mundo está intentando reintentarlo debido a un error del servicio, puede haber tantas solicitudes en cola que el servicio se sature cuando empieza a recuperarse.
- Si el error se debe a una limitación y hay un período de tiempo que el servicio usa para la limitación, los reintentos continuos podrían desproteger esa ventana y provocar que la limitación continúe.
- Es posible que un usuario espere a que se represente una página web. El hecho de que las personas esperen demasiado tiempo podría ser más molesto que les informa relativamente rápidamente de volver a intentarlo más tarde.

El retroceso exponencial soluciona algunos de estos problemas mediante la limitación de la frecuencia de los reintentos que un servicio puede obtener de la aplicación. Pero también debe tener *disyuntores*: Esto significa que, en un determinado umbral de reintentos, la aplicación deja de reintentar y realiza alguna otra acción, como una de las siguientes:

- Reserva personalizada. Si no puede obtener un precio bursátil de Reuters, quizás pueda obtenerlo de Bloomberg; o bien, si no puede obtener datos de la base de datos, quizás pueda obtenerlos de la memoria caché.
- Error de silencio. Si lo que necesita de un servicio no es todo o nada para su aplicación, simplemente devuelva NULL cuando no pueda obtener los datos. Si está mostrando una tarea corregir ti y el Blob service no responde, puede mostrar los detalles de la tarea sin la imagen.
- Error rápido. Se produce un error en el usuario para evitar inundar el servicio con solicitudes de reintento, lo que podría provocar la interrupción del servicio para otros usuarios o ampliar un período de limitación. Puede mostrar un mensaje "inténtelo de nuevo más tarde".

No hay ninguna directiva de reintentos de un solo ajuste. Puede volver a intentarlo más veces y esperar más tiempo en un proceso de trabajo en segundo plano asincrónico que en una aplicación web sincrónica en la que un usuario está esperando una respuesta. Puede esperar más tiempo entre los reintentos de un servicio de base de datos relacional que para un servicio de caché. Estas son algunas directivas de reintento recomendadas de ejemplo para que pueda hacer una idea de cómo pueden variar los números. ("Fast First" significa que no hay ningún retraso antes del primer reintento.

![Directivas de reintento de ejemplo](transient-fault-handling/_static/image1.png)

Para obtener SQL Database guía de la Directiva de reintentos, consulte [solución de problemas de errores transitorios y de conexión a SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumen

Una estrategia de reintento/interrupción puede ayudar a que los errores temporales sean invisibles para el cliente la mayor parte del tiempo, y Microsoft proporciona marcos que se pueden usar para minimizar el trabajo de implementación de una estrategia tanto si se usa ADO.NET, Entity Framework como Azure Storage servicio.

En el [siguiente capítulo](distributed-caching.md), veremos cómo mejorar el rendimiento y la confiabilidad mediante el uso de almacenamiento en caché distribuido.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

Documentation

- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). En las notas del producto, Mark SIMM y Michael Thomassy. Similar a la serie failsafe, pero profundiza en más detalles. Consulte la sección telemetría y diagnósticos.
- [Failsafe: Guía para arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto de Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la Página Web de la serie de vídeos FailSafe.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte patrón Retry, patrón Scheduler Agent supervisor.
- [Tolerancia a errores en Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Entrada de blog de Tony Petrossian.
- [Entity Framework: resistencia de la conexión o lógica de reintento](https://msdn.microsoft.com/data/dn456835). Cómo usar y personalizar la característica de control de errores transitorios de Entity Framework 6.
- [Resistencia de la conexión e intercepción de comandos con el Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). En la cuarta parte de una serie de tutoriales de nueve partes, se muestra cómo configurar la característica de resistencia de conexión EF 6 para SQL Database.

Vídeos

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Consulte la descripción de los disyuntores en el episodio 3 a partir de 40:55.
- [Building Big: lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM habla sobre el diseño de errores, el control de errores transitorios y la instrumentación de todo.

Ejemplo de código

- [Aspectos básicos del servicio en la nube en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo creada por el equipo de asesoramiento al cliente de Microsoft Azure que muestra cómo utilizar el [bloque de control de errores transitorios](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH) de Enterprise Library. Para obtener más información, consulte [nivel de acceso a datos de los aspectos básicos del servicio en la nube: control de errores transitorios](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). Se recomienda TFH para el acceso a la base de datos mediante ADO.NET directamente (sin usar Entity Framework).

> [!div class="step-by-step"]
> [Anterior](monitoring-and-telemetry.md)
> [Siguiente](distributed-caching.md)
