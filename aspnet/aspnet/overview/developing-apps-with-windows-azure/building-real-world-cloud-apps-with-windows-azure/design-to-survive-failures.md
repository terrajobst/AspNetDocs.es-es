---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Diseño para sobrevivir a errores (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 9bf9acb8b4f8521d03c053c124c5fc4a07d6cb9a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585653"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Diseño para sobrevivir a errores (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Uno de los aspectos que hay que tener en cuenta al compilar cualquier tipo de aplicación, pero especialmente uno que se ejecutará en la nube donde muchos usuarios la usarán, es cómo diseñar la aplicación para que pueda controlar correctamente los errores y continuar ofreciendo el valor en la medida de lo posible. permita. Dado el tiempo suficiente, se producirá un error en cualquier entorno o sistema de software. La forma en que su aplicación controla esas situaciones determina el modo en que los clientes obtendrán y cuánto tiempo tiene que dedicar a analizar y solucionar problemas.

## <a name="types-of-failures"></a>Tipos de errores

Hay dos categorías básicas de errores que deseará controlar de forma diferente:

- Errores transitorios de recuperación automática, como problemas de conectividad de red intermitentes.
- Se producen errores que requieren intervención.

En el caso de los errores transitorios, puede implementar una directiva de reintentos para asegurarse de que la aplicación se recupere de forma rápida y automática. Es posible que los clientes perciban un tiempo de respuesta ligeramente mayor, pero de lo contrario no se verán afectados. Mostraremos algunas maneras de controlar estos errores en el [capítulo de control de errores transitorios](transient-fault-handling.md).

En el caso de los errores, puede implementar la funcionalidad de supervisión y registro que le notifica inmediatamente cuando surgen problemas y que facilita el análisis de la causa principal. Mostraremos algunas maneras de ayudarte a mantenerse al tanto de estos tipos de errores en el [capítulo de supervisión y telemetría](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Ámbito de error

También tiene que pensar en el ámbito del error: si se ve afectada una sola máquina, un servicio completo, como SQL Database o almacenamiento, o toda una región.

![Ámbito de error](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Errores de máquina

En Azure, un servidor con errores se reemplaza automáticamente por uno nuevo y una aplicación en la nube bien diseñada se recupera a partir de este tipo de error de forma automática y rápida. Anteriormente se sobrerayó las ventajas de escalabilidad de un nivel de Web sin estado y la facilidad de recuperación de un servidor con errores es otra ventaja de Estados. La facilidad de recuperación también es una de las ventajas de las características de plataforma como servicio (PaaS), como SQL Database y Azure App Service Web Apps. Los errores de hardware son poco frecuentes, pero cuando se producen, estos servicios los controlan automáticamente. incluso no tiene que escribir código para controlar los errores de la máquina cuando se usa uno de estos servicios.

### <a name="service-failures"></a>Errores de servicio

Las aplicaciones en la nube suelen usar varios servicios. Por ejemplo, la aplicación Fix it usa el servicio SQL Database, el servicio de almacenamiento y la aplicación web se implementa en Azure App Service. ¿Qué hará su aplicación si se produce un error en uno de los servicios de los que depende? En el caso de algunos errores de servicio, se recomienda el mensaje "lo sentimos, volver a intentarlo más tarde". Pero en muchos escenarios puede mejorar. Por ejemplo, cuando el almacén de datos de back-end está inactivo, puede aceptar la entrada del usuario, mostrar "se ha recibido la solicitud" y almacenar la entrada en otro lugar temporalmente; después, cuando el servicio que necesita está de nuevo en funcionamiento, puede recuperar la entrada y procesarla.

El capítulo [patrón de trabajo centrado en cola](queue-centric-work-pattern.md) muestra una manera de controlar este escenario. La aplicación Fix it almacena las tareas en SQL Database, pero no tiene que dejar de funcionar cuando SQL Database está inactivo. En este capítulo, veremos cómo almacenar la entrada del usuario para una tarea en una cola y cómo usar un proceso de trabajo para leer la cola y actualizar la tarea. Si SQL está inactivo, la capacidad de crear las tareas corregir ti no se ve afectada. el proceso de trabajo puede esperar y procesar las tareas nuevas cuando SQL Database está disponible.

### <a name="region-failures"></a>Errores de región

Se puede producir un error en todas las regiones. Un desastre natural puede destruir un centro de datos, es posible que se vea acoplado mediante un Meteor, la línea troncal en el centro de datos podría ser cortada por un agricultor que esté enterrando una vaca con un backhoe, etc. ¿Qué se puede hacer si la aplicación se hospeda en el centro de información de Stricken? Es posible configurar la aplicación en Azure para que se ejecute en varias regiones simultáneamente, de modo que, si se produce un desastre en una, continúe ejecutándose en otra región. Estos errores son repeticiones muy poco frecuentes y la mayoría de las aplicaciones no atraviesan los circulares necesarios para garantizar un servicio ininterrumpido a través de errores de este tipo. Consulte la sección recursos al final del capítulo para obtener información sobre cómo mantener la aplicación disponible incluso a través de un error en la región.

Un objetivo de Azure es hacer que el control de todos estos tipos de errores sea mucho más fácil y verá algunos ejemplos de cómo lo hacemos en los capítulos siguientes.

## <a name="slas"></a>Contrato

A menudo, los usuarios suelen conocer los acuerdos de nivel de servicio (SLA) en el entorno de nube. Básicamente, se trata de promesas que las compañías hacen sobre la confiabilidad del servicio. Un acuerdo de nivel de servicio del 99,9% significa que debe esperar que el servicio funcione correctamente el 99,9% del tiempo. Este es un valor bastante habitual para un acuerdo de nivel de servicio y suena como un número muy alto, pero es posible que no se tenga en cuanta cuánto tiempo de inactividad se importe realmente en el 0,1%. Esta es una tabla que muestra cuánto tiempo de inactividad varios porcentajes de acuerdo de nivel de servicio van a más de un año, un mes y una semana.

![Tabla de SLA](design-to-survive-failures/_static/image2.png)

Por lo tanto, un contrato de nivel de servicio del 99,9% significa que el servicio podría estar inactivo 8,76 horas al año o 43,2 minutos al mes. Este es más tiempo de inactividad del que la mayoría de los usuarios obtienen. Por lo tanto, como desarrollador, querrá tener en cuenta que es posible que haya una cantidad determinada de tiempo de inactividad y que la administre de forma correcta. En algún momento, alguien va a usar la aplicación y un servicio se va a desactivar y desea minimizar el impacto negativo de eso en el cliente.

Uno de los elementos que debe conocer sobre un acuerdo de nivel de servicio es el marco de tiempo al que hace referencia: ¿el reloj se restablece cada semana, cada mes o cada año? En Azure, restablecemos el reloj cada mes, que es mejor para usted que un acuerdo de nivel de servicio anual, dado que un contrato de nivel de servicio anual podría ocultar meses incorrectos al desviarlos con una serie de Buenos meses.

Por supuesto, siempre aspiramos a hacer mejor que el acuerdo de nivel de servicio; Normalmente, será mucho menor que eso. La promesa es que, si el tiempo de inactividad es mayor que el tiempo de inactividad máximo, puede solicitar dinero. Es posible que la cantidad de dinero que reciba probablemente no compense por completo el impacto de la empresa en el exceso de tiempo de inactividad, pero ese aspecto del contrato de nivel de servicio actúa como una directiva de cumplimiento y le permite saber que lo tomamos muy en serio.

### <a name="composite-slas"></a>SLAs compuestos

Una cuestión importante que hay que tener en cuenta cuando se examinan los acuerdos de nivel de servicio es el impacto del uso de varios servicios en una aplicación, cada uno de los cuales tiene un acuerdo de nivel de servicio independiente. Por ejemplo, la aplicación Fix it usa Azure App Service Web Apps, Azure Storage y SQL Database. Estos son los números de contrato de nivel de servicio a partir de la fecha en que se escribe este libro electrónico en diciembre, 2013:

![Sitio web del contrato de nivel de servicio, almacenamiento SQL Database](design-to-survive-failures/_static/image3.png)

¿Cuál es el tiempo de inactividad máximo que cabría esperar para la aplicación en función de estos acuerdos de nivel de servicio? Podría pensar que el tiempo de inactividad es igual al peor porcentaje del contrato de nivel de servicio o al 99,9% en este caso. Esto sería cierto si los tres servicios no siempre superaran al mismo tiempo, pero eso no es necesariamente lo que sucede realmente. Cada servicio puede producir un error de forma independiente en momentos diferentes, por lo que tiene que calcular el SLA compuesto multiplicando los números de contrato de nivel de servicio individuales.

![ACUERDO de nivel de servicio compuesto](design-to-survive-failures/_static/image4.png)

Por lo tanto, la aplicación podría estar inactiva no solo 43,2 minutos al mes, sino 3 veces esa cantidad, 108 minutos al mes, y sigue estando dentro de los límites del contrato de nivel de servicio de Azure.

Este problema no es exclusivo de Azure. En realidad, proporcionamos los mejores contratos de nivel de servicio en la nube de cualquier servicio en la nube disponible, y tendrá problemas similares para tratar si usa los servicios en la nube de los proveedores. Lo que esto destaca es la importancia de pensar en cómo se puede diseñar la aplicación para controlar los errores de servicio inevitables correctamente, ya que pueden producirse con la suficiente frecuencia para afectar a los clientes o usuarios.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Contratos de nivel de servicios en la nube en comparación con la experiencia empresarial

A veces, los usuarios dicen "en mi aplicación empresarial nunca tengo estos problemas". Si pregunta cuánto tiempo está inactivo durante un mes, suele decir que "bien, se produce ocasionalmente". Y, si se pregunta con qué frecuencia, se les admite "a veces es necesario hacer una copia de seguridad o instalar un nuevo servidor o actualizar el software". Por supuesto, esto cuenta como tiempo de inactividad. La mayoría de las aplicaciones empresariales, a menos que sean especialmente críticas, están inactivas durante un período de tiempo mayor que el permitido por nuestros contratos de nivel de servicio. Pero cuando se trata de su servidor y su infraestructura, y usted es responsable de ti y de controlarlo, tiende a sentir menos Angst acerca de los tiempos de inactividad. En un entorno de nube, depende de otra persona y no sabe lo que está ocurriendo, por lo que puede que le interese preocuparse por ella.

Cuando una empresa logra un porcentaje de tiempo de actividad superior al que se obtiene de un contrato de nivel de servicio en la nube, lo hacen dedicando mucho más dinero al hardware. Un servicio en la nube podría hacer esto, pero tendría que cobrar mucho más por sus servicios. En su lugar, se aprovecha un servicio rentable y se diseña el software para que los errores inevitados provoquen una interrupción mínima de los clientes. El trabajo como un diseñador de aplicaciones en la nube no es tan grande como para evitar un error en lo que se redujera la catástrofe y hacerlo centrándose en el software, no en el hardware. Mientras que las aplicaciones empresariales se esfuerzan por maximizar el tiempo medio entre errores, las aplicaciones en la nube se esfuerzan por minimizar el tiempo promedio de recuperación.

### <a name="not-all-cloud-services-have-slas"></a>No todos los servicios en la nube tienen acuerdos de nivel de servicio

Tenga en cuenta también que no todos los servicios en la nube tienen un acuerdo de nivel de servicio. Si su aplicación depende de un servicio sin ninguna garantía de tiempo de actividad, podría estar inactivo mucho más tiempo del que podría imaginar. Por ejemplo, si habilita el inicio de sesión en el sitio mediante un proveedor de redes sociales, como Facebook o Twitter, consulte con el proveedor de servicios para averiguar si hay un acuerdo de nivel de servicio y puede que descubra que no hay ninguno. Pero si el servicio de autenticación deja de funcionar o no es compatible con el volumen de solicitudes que se producen en él, los clientes están bloqueados fuera de la aplicación. Podría estar inactivo durante días o más. Los creadores de una nueva aplicación esperaban cientos de millones de descargas y tomaban una dependencia de la autenticación de Facebook, pero no se comunicaban con Facebook antes de activarse y detectarse demasiado tarde que no había ningún acuerdo de nivel de servicio para ese servicio.

### <a name="not-all-downtime-counts-toward-slas"></a>No todos los tiempos de inactividad se retienen

Algunos servicios en la nube pueden denegar deliberadamente el servicio si la aplicación los utiliza en exceso. Esto se denomina *limitación*. Si un servicio tiene un acuerdo de nivel de servicio, debe indicar las condiciones en las que se puede limitar, y el diseño de la aplicación debe evitar esas condiciones y reaccionar correctamente a la limitación si se produce. Por ejemplo, si se produce un error en las solicitudes a un servicio cuando supera un número determinado por segundo, querrá asegurarse de que los reintentos automáticos no se producen tan rápido que hacen que la limitación continúe. Tendremos más que decir sobre la limitación en el capítulo de [control de errores transitorios](transient-fault-handling.md).

## <a name="summary"></a>Resumen

En este capítulo se ha intentado comprender por qué una aplicación en la nube del mundo real tiene que estar diseñada para sobrevivir a errores correctamente. A partir del [siguiente capítulo](monitoring-and-telemetry.md), los patrones restantes de esta serie profundizan en más detalles sobre algunas estrategias que puede usar para ello:

- Tenga buena [supervisión y telemetría](monitoring-and-telemetry.md), de modo que pueda averiguar rápidamente los errores que requieran la intervención y tener suficiente información para resolverlos.
- [Controlar los errores transitorios](transient-fault-handling.md) mediante la implementación de una lógica de reintento inteligente, de modo que la aplicación se recupere automáticamente cuando pueda y recurra a la lógica del [disyuntor](transient-fault-handling.md#circuitbreakers) cuando no pueda hacerlo.
- Use el [almacenamiento en caché distribuido](distributed-caching.md) para minimizar el rendimiento, la latencia y los problemas de conexión con el acceso a la base de datos.
- Implemente el acoplamiento flexible mediante el [patrón de trabajo centrado en cola](queue-centric-work-pattern.md), de modo que el front-end de la aplicación pueda continuar funcionando cuando el back-end esté inactivo.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los capítulos posteriores de este libro electrónico y los siguientes recursos.

Documentación:

- [Failsafe: Guía para arquitecturas de nube resistentes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Notas del producto de Marc Mercuri, Ulrich Homann y Andrew Townhill. Versión de la Página Web de la serie de vídeos FailSafe.
- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). En las notas del producto, Mark SIMM y Michael Thomassy.
- [Guía técnica de la continuidad del negocio de Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Notas del producto de Patrick Wickline y Jason Roth.
- [Recuperación ante desastres y alta disponibilidad para las aplicaciones de Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Notas del producto de Michael McKeown, Hanu Kommalapati y Jason Roth.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Guía de implementación de varios centros de datos, patrón de disyuntor.
- [Soporte técnico de Azure: contratos de nivel de servicio](https://azure.microsoft.com/support/legal/sla/).
- [Continuidad empresarial en Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentación sobre SQL Database características de alta disponibilidad y recuperación ante desastres.
- [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure virtual machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vídeos:

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Los episodios 1 y 8 profundizan en los motivos para diseñar aplicaciones en la nube que sobreviven a los errores. Vea también el debate de seguimiento de la limitación en el episodio 2 a partir de 49:57, la explicación de los puntos de error y los modos de error en el episodio 2 a partir de 56:05 y la explicación de los disyuntores del episodio 3 a partir de 40:55.
- [Building Big: lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM habla sobre el diseño de errores y la instrumentación de todo. Similar a la serie failsafe, pero profundiza en más detalles.

> [!div class="step-by-step"]
> [Anterior](unstructured-blob-storage.md)
> [Siguiente](monitoring-and-telemetry.md)
