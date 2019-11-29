---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
title: Supervisión y telemetría (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 07/09/2015
ms.assetid: 7e986ab5-6615-4638-add7-4614ce7b51db
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry
msc.type: authoredcontent
ms.openlocfilehash: 44941c9fd0dcd3223604fc4a4f2836f587578acb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585612"
---
# <a name="monitoring-and-telemetry-building-real-world-cloud-apps-with-azure"></a>Supervisión y telemetría (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Muchas personas confían en los clientes para que sepan cuándo su aplicación está inactiva. Eso no es realmente un procedimiento recomendado en cualquier lugar y, en especial, no en la nube. No hay ninguna garantía de una notificación rápida y, cuando se le notifica, a menudo obtiene datos mínimos o engañosos sobre lo que ha sucedido. Con buenos sistemas de registro y telemetría, puede tener en cuenta lo que ocurre con la aplicación y, cuando se produce algún problema, descubre de inmediato y tiene información útil para la solución de problemas con la que trabajar.

## <a name="buy-or-rent-a-telemetry-solution"></a>Comprar o alquilar una solución de telemetría

> [!NOTE]
> Este artículo se escribió antes de que se publicara [Application Insights](/azure/application-insights/app-insights-overview) . Application Insights es el método preferido para las soluciones de telemetría en Azure. Consulte [configuración de Application Insights para el sitio web de ASP.net](/azure/application-insights/app-insights-asp-net) para obtener más información.

Una de las cosas que es muy útil en el entorno de nube es que es realmente fácil comprar o alquilar el modo de Victoria. La telemetría es un ejemplo. Sin mucho esfuerzo, puede obtener un sistema de telemetría realmente bueno y en funcionamiento, de un modo muy rentable. Hay una serie de grandes asociados que se integran con Azure y algunos tienen niveles gratis, por lo que puede obtener datos de telemetría básicos para nada. Estas son solo algunas de las que están disponibles actualmente en Azure:

- [Nuevo Relic](http://newrelic.com/)
- [AppDynamics](http://www.appdynamics.com/)
- [Dynatrace](https://datamarket.azure.com/application/b4011de2-1212-4375-9211-e882766121ff)

[Microsoft System Center](http://www.petri.co.il/microsoft-system-center-introduction.htm#) también incluye características de supervisión.

Veremos rápidamente la configuración de nuevos Relic para mostrar lo fácil que es usar un sistema de telemetría.

En el portal de administración de Azure, Regístrese en el servicio. Haga clic en **nuevo**y, a continuación, en **tienda**. Aparece el cuadro de diálogo **elegir un complemento** . Desplácese hacia abajo y haga clic en **nuevo Relic**.

![Elegir un complemento](monitoring-and-telemetry/_static/image1.png)

Haga clic en la flecha derecha y elija el nivel de servicio que desee. En esta demostración, usaremos el nivel gratis.

![Personalizar complemento](monitoring-and-telemetry/_static/image2.png)

Haga clic en la flecha derecha, confirme la "compra" y ahora se muestra el nuevo Relic como un complemento en el portal.

![Revisar la compra](monitoring-and-telemetry/_static/image3.png)

![Nuevo complemento de Relic en el portal de administración](monitoring-and-telemetry/_static/image4.png)

Haga clic en **información de conexión**y copie la clave de licencia.

![Información de conexión](monitoring-and-telemetry/_static/image5.png)

Vaya a la pestaña **configurar** de la aplicación web en el portal, establezca **supervisión del rendimiento** en **complemento**y establezca la lista desplegable **elegir complemento** en **New Relic**. A continuación, haga clic en **Guardar**.

![Nueva Relic en la pestaña configurar](monitoring-and-telemetry/_static/image6.png)

En Visual Studio, instale el nuevo paquete NuGet de Relic en la aplicación.

![Developer Analytics en la pestaña configurar](monitoring-and-telemetry/_static/image7.png)

Implemente la aplicación en Azure y empiece a usarla. Cree algunas tareas de corrección de TI para proporcionar una actividad para la supervisión de nuevas Relic.

A continuación, vuelva a la página **New Relic** en la pestaña **Complementos** del portal y haga clic en **administrar**. El portal le envía al nuevo portal de administración de Relic, mediante el inicio de sesión único para la autenticación, por lo que no tiene que volver a escribir sus credenciales. La página información general presenta diversas estadísticas de rendimiento. (Haga clic en la imagen para ver la página de información general tamaño completo).

[![pestaña de supervisión de New Relic](monitoring-and-telemetry/_static/image9.png)](monitoring-and-telemetry/_static/image8.png)

Estas son solo algunas de las estadísticas que puede ver:

- Tiempo medio de respuesta en distintos momentos del día.

    ![Tiempo de respuesta](monitoring-and-telemetry/_static/image10.png)
- Tasas de rendimiento (en solicitudes por minuto) en distintos momentos del día.

    ![Rendimiento](monitoring-and-telemetry/_static/image11.png)
- Tiempo de CPU del servidor dedicado a administrar diferentes solicitudes HTTP.

    ![Tiempos de transacción Web](monitoring-and-telemetry/_static/image12.png)
- Tiempo de CPU invertido en distintas partes del código de la aplicación:

    ![Detalles del seguimiento](monitoring-and-telemetry/_static/image13.png)
- Estadísticas de rendimiento históricas.

    ![Rendimiento histórico](monitoring-and-telemetry/_static/image14.png)
- Llamadas a servicios externos como el Blob service y estadísticas sobre la confiabilidad y la capacidad de respuesta del servicio.

    ![Servicios externos](monitoring-and-telemetry/_static/image15.png)

    ![Servicios externos](monitoring-and-telemetry/_static/image16.png)

    ![Servicio externo](monitoring-and-telemetry/_static/image17.png)
- Información sobre el lugar del mundo o de dónde procede el tráfico de la aplicación Web de EE. UU.

    ![Geography](monitoring-and-telemetry/_static/image18.png)

También puede configurar informes y eventos. Por ejemplo, puede indicar siempre que empiece a ver los errores y enviar un correo electrónico al personal de soporte técnico de alertas al problema.

![Informes](monitoring-and-telemetry/_static/image19.png)

New Relic es solo un ejemplo de un sistema de telemetría; también puede obtener todo esto desde otros servicios. La belleza de la nube es que, sin tener que escribir ningún código y, por gastos mínimos o sin gastos, puede obtener mucho más información sobre cómo se usa la aplicación y qué realmente están experimentando los clientes.

<a id="log"></a>
## <a name="log-for-insight"></a>Registro para Insight

Un paquete de telemetría es un buen paso, pero todavía tiene que instrumentar su propio código. El servicio de telemetría le indica cuándo hay un problema y le indica lo que están experimentando los clientes, pero es posible que no le ofrezca mucha información sobre lo que está ocurriendo en el código.

No desea tener que tener acceso remoto a un servidor de producción para ver lo que está haciendo la aplicación. Esto puede resultar práctico cuando tiene un servidor, pero ¿qué ocurre si ha escalado a cientos de servidores y no sabe cuáles son los que necesita para Remote? El registro debe proporcionar información suficiente que nunca tenga que tener en los servidores de producción para analizar y depurar problemas. Debe registrar suficiente información para poder aislar los problemas únicamente a través de los registros.

### <a name="log-in-production"></a>Inicio de sesión en producción

Muchas personas activan el seguimiento en producción solo cuando hay un problema y quieren depurar. Esto puede suponer un retraso considerable entre el momento en que se conoce un problema y el momento en que se obtiene información útil para la solución de problemas. Y es posible que la información que obtenga no sea útil para errores intermitentes.

Lo que se recomienda en el entorno de nube donde el almacenamiento es barato es que siempre se deja el inicio de sesión en producción. De este modo, cuando se produzcan errores, habrán registrado y tendrá datos históricos que pueden ayudarle a analizar los problemas que se desarrollan a lo largo del tiempo o se producen con regularidad en momentos diferentes. Podría automatizar un proceso de purga para eliminar los registros antiguos, pero puede que le resulte más caro configurar este tipo de proceso que mantener los registros.

Los gastos agregados del registro son triviales en comparación con la cantidad de tiempo de solución de problemas y dinero que puede ahorrar si la información que necesita ya está disponible cuando se produce algún problema. Entonces, cuando alguien le indica que tenía un error aleatorio en unos 8:00 de la última noche, pero no recuerda el error, puede averiguar fácilmente cuál fue el problema.

Durante menos de $4 al mes, puede mantener a mano 50 gigabytes de registros y el impacto en el rendimiento del registro es trivial, siempre y cuando tenga en cuenta lo que se debe hacer para evitar cuellos de botella de rendimiento, asegúrese de que la biblioteca de registro es asincrónica.

### <a name="differentiate-logs-that-inform-from-logs-that-require-action"></a>Diferenciar los registros que informan de los registros que requieren una acción

Los registros están diseñados para informar (deseo saber algo) o actuar (deseo hacer algo). Tenga cuidado de escribir solo registros de ACT para los problemas que requieren de una persona o un proceso automatizado para realizar acciones. Demasiados registros de ACT crearán ruido, lo que requiere demasiado trabajo para examinarlo todo para encontrar problemas auténticos. Además, si los registros de ACT desencadenan automáticamente alguna acción, como enviar correo electrónico al personal de soporte técnico, evite que se activen miles de acciones con un solo problema.

En el seguimiento System. Diagnostics de .NET, se puede asignar el nivel de error, ADVERTENCIA, información y depuración/detalle a los registros. Puede diferenciar las acciones de NOTIFICAr los registros mediante la reserva del nivel de error para los registros de ACT y el uso de los niveles inferiores para los registros de informe.

![Niveles de registro](monitoring-and-telemetry/_static/image20.png)

### <a name="configure-logging-levels-at-run-time"></a>Configuración de los niveles de registro en tiempo de ejecución

Aunque merece la pena tener el registro siempre activado en producción, otro procedimiento recomendado es implementar un marco de registro que le permita ajustar en tiempo de ejecución el nivel de detalle que está registrando, sin tener que volver a implementar o reiniciar la aplicación. Por ejemplo, si usa la utilidad de seguimiento en `System.Diagnostics` puede crear registros de error, de advertencia, de información y depuración/detallado. Le recomendamos que registre siempre los registros de errores, advertencias y de información en producción, y querrá poder agregar dinámicamente el registro de depuración/detallado para la solución de problemas en cada caso.

Web Apps en Azure App Service tienen compatibilidad integrada para escribir `System.Diagnostics` registros en el sistema de archivos, el almacenamiento de tablas o el almacenamiento de blobs. Puede seleccionar diferentes niveles de registro para cada destino de almacenamiento, y puede cambiar el nivel de registro sobre la marcha sin necesidad de reiniciar la aplicación. La compatibilidad con almacenamiento de blobs facilita la ejecución de trabajos de análisis de [HDInsight](https://docs.microsoft.com/azure/hdinsight/) en los registros de la aplicación, ya que HDInsight sabe cómo trabajar con BLOB Storage directamente.

### <a name="log-exceptions"></a>para registrar excepciones

No solo debe poner una *excepción. ToString ()* en el código de registro. Que deja la información contextual. En el caso de los errores de SQL, deja el número de error de SQL. Para todas las excepciones, incluya información de contexto, la propia excepción y excepciones internas para asegurarse de que está proporcionando todo lo que se necesita para solucionar el problema. Por ejemplo, la información de contexto puede incluir el nombre del servidor, un identificador de transacción y un nombre de usuario (pero no la contraseña ni ningún secreto).

Si confía en cada desarrollador para hacer lo correcto con el registro de excepciones, algunos de ellos no podrán hacerlo. Para asegurarse de que se realiza de la forma correcta cada vez, cree el control de excepciones directamente en la interfaz del registrador: pase el objeto de excepción a la clase de registrador y registre los datos de la excepción correctamente en la clase Logger.

### <a name="log-calls-to-services"></a>Llamadas de registro a servicios

Se recomienda escribir un registro cada vez que la aplicación llame a un servicio, ya sea a una base de datos o a una API de REST o a un servicio externo. Incluya en los registros no solo una indicación de éxito o error, sino cuánto tiempo tardó cada solicitud. En el entorno de nube, a menudo verá problemas relacionados con las ralentizaciones en lugar de las interrupciones completas. Algo que normalmente tarda 10 milisegundos podría empezar a tardar un segundo. Cuando alguien le indica que la aplicación es lenta, quiere poder examinar los nuevos Relic o cualquier servicio de telemetría que tenga y validar su experiencia y, a continuación, quiere poder buscar sus propios registros para profundizar en los detalles de por qué son lentos.

### <a name="use-an-ilogger-interface"></a>Uso de una interfaz ILogger

Lo que se recomienda al crear una aplicación de producción es crear una interfaz de *ILogger* sencilla y pegar algunos métodos en ella. Esto facilita el cambio de la implementación del registro más adelante y no es necesario repasar todo el código para hacerlo. Podríamos usar la clase `System.Diagnostics.Trace` en la aplicación Fix it, pero, en su lugar, la usamos en una clase de registro que implementa *ILogger*y hacemos llamadas al método *ILogger* en la aplicación.

De este modo, si alguna vez desea que el registro sea más completo, puede reemplazar [`System.Diagnostics.Trace`](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio#apptracelogs) por el mecanismo de registro que desee. Por ejemplo, a medida que crece la aplicación, puede decidir que quiere usar un paquete de registro más completo, como [NLog](http://nlog-project.org/) o el [bloque de aplicaciones de registro de Enterprise Library](https://msdn.microsoft.com/library/dn440731(v=pandp.60).aspx). ([Log4net](http://logging.apache.org/log4net/) es otro marco de registro popular, pero no hace el registro asincrónico).

Una posible razón para usar un marco de trabajo como NLog es facilitar la división de la salida del registro en almacenes de datos de gran volumen y de alto valor independientes. Esto le ayuda a almacenar de forma eficaz grandes volúmenes de datos informativos de los que no necesita ejecutar consultas rápidas, a la vez que mantiene un acceso rápido a los datos de ACT.

### <a name="semantic-logging"></a>Registro semántico

Para obtener una manera relativamente nueva de registrar que puede generar información de diagnóstico más útil, consulte el [bloque de aplicación de registro semántico de Enterprise Library (losa)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). El losa usa el [seguimiento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) y la compatibilidad con [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) en .net 4,5 para que pueda crear registros más estructurados y consultados. Se define un método diferente para cada tipo de evento que se registra, lo que permite personalizar la información que se escribe. Por ejemplo, para registrar un error de SQL Database podría llamar a un método `LogSQLDatabaseError`. Para ese tipo de excepción, sabe que una parte clave de la información es el número de error, por lo que puede incluir un parámetro de número de error en la firma del método y registrar el número de error como un campo independiente en la entrada de registro que escribe. Dado que el número está en un campo independiente, puede obtener informes de forma más fácil y confiable en función de los números de error de SQL de los que podría si simplemente concatenara el número de error en una cadena de mensaje.

## <a name="logging-in-the-fix-it-app"></a>Inicio de sesión en la aplicación Fix it

### <a name="the-ilogger-interface"></a>La interfaz ILogger

Esta es la interfaz *ILogger* en la aplicación Fix it.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample1.cs)]

Estos métodos permiten escribir registros en los mismos cuatro niveles admitidos por *System. Diagnostics*. Los métodos de TraceApi son para registrar llamadas de servicio externas con información sobre la latencia. También puede Agregar un conjunto de métodos para el nivel de depuración o detallado.

### <a name="the-logger-implementation-of-the-ilogger-interface"></a>La implementación del registrador de la interfaz ILogger

La implementación de la interfaz es realmente sencilla. Básicamente solo llama a los métodos estándar *System. Diagnostics* . En el fragmento de código siguiente se muestran los tres métodos de información y uno de los demás.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample2.cs)]

### <a name="calling-the-ilogger-methods"></a>Llamar a los métodos ILogger

Cada vez que el código de la aplicación Fix it detecta una excepción, llama a un método *ILogger* para registrar los detalles de la excepción. Y cada vez que realiza una llamada a la base de datos, Blob service o una API de REST, inicia un cronómetro antes de la llamada, detiene el cronómetro cuando el servicio vuelve y registra el tiempo transcurrido junto con información sobre el éxito o el error.

Observe que el mensaje de registro incluye el nombre de clase y el nombre de método. Es recomendable asegurarse de que los mensajes de registro identifiquen qué parte del código de la aplicación los escribió.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample3.cs?highlight=6,14,20-21,25)]

Por lo tanto, ahora, para cada vez que la aplicación Fix it ha realizado una llamada a SQL Database, puede ver la llamada, el método que lo llamó y la cantidad de tiempo que tardó exactamente.

![SQL Database consulta en registros](monitoring-and-telemetry/_static/image21.png)

![](monitoring-and-telemetry/_static/image22.png)

Si va a examinar los registros, puede ver que las llamadas de base de datos de tiempo Take son variables. Esta información podría ser útil: dado que la aplicación registra todo esto, puede analizar las tendencias históricas en la forma en que el servicio de base de datos se realiza a lo largo del tiempo. Por ejemplo, un servicio podría ser la mayor parte del tiempo, pero las solicitudes podrían producir errores o las respuestas podrían ralentizarse en determinados momentos del día.

Puede hacer lo mismo para la Blob service: para cada vez que la aplicación cargue un archivo nuevo, hay un registro y puede ver exactamente cuánto tiempo se tardó en cargar cada archivo.

![Registro de carga de blobs](monitoring-and-telemetry/_static/image23.png)

Se trata de un par de líneas de código adicionales para escribir cada vez que se llama a un servicio y, ahora, cada vez que alguien indica que se ha producido un problema, sabe exactamente cuál fue el problema, si se trata de un error, o incluso si se ha ejecutado lentamente. Puede identificar el origen del problema sin tener que tener acceso remoto a un servidor o activar el registro después de que se produzca el error y esperar volver a crearlo.

## <a name="dependency-injection-in-the-fix-it-app"></a>Inserción de dependencias en la aplicación Fix it

Tal vez se pregunte cómo el constructor del repositorio en el ejemplo mostrado anteriormente obtiene la implementación de la interfaz del registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample4.cs?highlight=6)]

Para conectar la interfaz a la implementación, la aplicación usa la [inserción de dependencias](http://en.wikipedia.org/wiki/Dependency_injection)(di) con [AutoFac](http://autofac.org/). DI le permite usar un objeto basado en una interfaz en muchos lugares en todo el código y solo tiene que especificar en un lugar la implementación que se utiliza cuando se crea una instancia de la interfaz. Esto facilita el cambio de la implementación: por ejemplo, podría querer reemplazar el registrador System. Diagnostics por un registrador NLog. O para las pruebas automatizadas, puede que desee sustituir una versión ficticia del registrador.

La aplicación Fix it usa DI en todos los repositorios y todos los controladores. Los constructores de las clases de controlador obtienen una interfaz *ITaskRepository* de la misma manera que el repositorio obtiene una interfaz de registrador:

[!code-csharp[Main](monitoring-and-telemetry/samples/sample5.cs?highlight=5)]

La aplicación usa la biblioteca AutoFac DI para proporcionar automáticamente instancias de *TaskRepository* y *Logger* para estos constructores.

[!code-csharp[Main](monitoring-and-telemetry/samples/sample6.cs?highlight=8,10)]

Básicamente, este código indica que cualquier lugar en el que un constructor necesita una interfaz *ILogger* , pasar una instancia de la clase *Logger* y cada vez que necesita una interfaz *IFixItTaskRepository* , pasar una instancia de la clase *FixItTaskRepository* .

[AutoFac](http://autofac.org/) es uno de los muchos marcos de inserción de dependencias que puede usar. Otro popular es [Unity](https://blogs.msdn.com/b/agile/archive/2013/08/20/new-guide-dependency-injection-with-unity.aspx), que se recomienda y es compatible con los patrones y prácticas de Microsoft.

## <a name="built-in-logging-support-in-azure"></a>Compatibilidad con el registro integrado en Azure

Azure admite los siguientes tipos de [registro de Web Apps en Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio):

- Seguimiento de System. Diagnostics (puede activar y desactivar y establecer niveles sobre la marcha sin reiniciar el sitio).
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).

Azure admite los siguientes tipos de [registro en Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics):

- Seguimiento de System. Diagnostics.
- Contadores de rendimiento.
- Eventos de Windows.
- Registros de IIS (HTTP/FREB).
- Supervisión de directorios personalizada.

La aplicación Fix it usa el seguimiento de System. Diagnostics. Lo único que debe hacer para habilitar el registro de System. Diagnostics en una aplicación web es voltear un conmutador en el portal o llamar a la API de REST. En el portal, haga clic en la pestaña **configuración** del sitio y desplácese hacia abajo para ver la **diagnóstico de aplicaciones** sección. Puede activar o desactivar el registro y seleccionar el nivel de registro que desee. Puede hacer que Azure escriba los registros en el sistema de archivos o en una cuenta de almacenamiento.

![Diagnóstico de aplicaciones y diagnósticos de sitio en la pestaña configurar](monitoring-and-telemetry/_static/image24.png)

Después de habilitar el registro en Azure, puede ver los registros en la ventana de salida de Visual Studio a medida que se crean.

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-viewlogsmenu.png)

![Menú de registros de streaming](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-nologsyet.png)

También puede hacer que los registros se escriban en la cuenta de almacenamiento y verlos con cualquier herramienta que pueda tener acceso al Table service de Azure Storage, como **Explorador de servidores** en Visual Studio o [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Registros en Explorador de servidores](http://wacomdpsstorage.blob.core.windows.net/articlesmedia/content-ppe.windowsazure.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/20140115062810/tws-storagelogs.png)

## <a name="summary"></a>Resumen

Es realmente sencillo implementar un sistema de telemetría preconfigurado, instrumentar el registro en su propio código y configurar el registro en Azure. Y cuando tenga problemas de producción, la combinación de un sistema de telemetría y los registros personalizados le ayudará a resolver problemas rápidamente antes de que se conviertan en problemas importantes para los clientes.

En el [siguiente capítulo](transient-fault-handling.md) veremos cómo controlar los errores transitorios para que no se conviertan en problemas de producción que deba investigar.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Documentación principalmente sobre la telemetría:

- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte orientación sobre instrumentación y telemetría, guía de medición de servicios, patrón de supervisión de puntos de conexión de mantenimiento y patrón de reconfiguración en tiempo de ejecución.
- Baja [del decimales en la nube: habilitación de la nueva supervisión de rendimiento de Relic en Azure websites](http://www.hanselman.com/blog/PennyPinchingInTheCloudEnablingNewRelicPerformanceMonitoringOnWindowsAzureWebsites.aspx).
- [Prácticas recomendadas para el diseño de servicios a gran escala en Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). En las notas del producto, Mark SIMM y Michael Thomassy. Consulte la sección telemetría y diagnósticos.
- [Desarrollo de próxima generación con Application Insights](https://msdn.microsoft.com/magazine/dn683794.aspx). Artículo de MSDN Magazine.

Documentación principalmente sobre el registro:

- [Bloque de aplicación de registro semántico (losa)](http://convective.wordpress.com/2013/08/12/semantic-logging-application-block-slab/). Neil Mackenzie presenta el caso del registro semántico con losa.
- [Crear registros estructurados y significativos con el registro semántico](https://channel9.msdn.com/Events/Build/2013/3-336). Cámara Juliano Dominguez presenta el caso del registro semántico con losa.
- [Registro de SQL de EF6: parte 1: registro simple](http://blog.oneunicorn.com/2013/05/08/ef6-sql-logging-part-1-simple-logging/). Arthur Vickers muestra cómo registrar las consultas ejecutadas por Entity Framework en EF 6.
- [Resistencia de la conexión e intercepción de comandos con el Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). En la cuarta parte de una serie de tutoriales de nueve partes, se muestra cómo usar la característica de intercepción de comandos de EF 6 para registrar comandos SQL enviados a la base de datos mediante Entity Framework.
- [Mejorar el registro C# con los atributos de información del llamador 5,0](http://www.dotnetcurry.com/showarticle.aspx?ID=972). Cómo registrar fácilmente el nombre del método de llamada sin codificarlo de forma rígida en literales ni usar la reflexión para obtenerlo manualmente.

Documentación principalmente acerca de la solución de problemas:

- [Blog de solución de problemas de Azure &amp; depuración](https://blogs.msdn.com/b/kwill/).
- [AzureTools: utilidad de diagnóstico usada por el equipo de Azure soporte técnico Developer](https://blogs.msdn.com/b/kwill/archive/2013/08/26/azuretools-the-diagnostic-utility-used-by-the-windows-azure-developer-support-team.aspx?Redirected=true). Presenta y proporciona un vínculo de descarga para una herramienta que se puede usar en una máquina virtual de Azure para descargar y ejecutar una amplia variedad de herramientas de diagnóstico y supervisión. Resulta útil cuando se necesita diagnosticar un problema en una máquina virtual determinada.
- [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Un tutorial paso a paso para empezar a trabajar con el seguimiento de System. Diagnostics y la depuración remota.

Vídeos:

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Los episodios 4 y 9 están relacionados con la supervisión y la telemetría. El episodio 9 incluye información general de los servicios de supervisión MetricsHub, AppDynamics, New Relic y PagerDuty.
- [Building Big: lecciones aprendidas de clientes de Azure, parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark SIMM habla sobre el diseño de errores y la instrumentación de todo. Similar a la serie failsafe, pero profundiza en más detalles.

Código de ejemplo:

- [Aspectos básicos del servicio en la nube en Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicación de ejemplo creada por el equipo de asesoramiento de clientes de Microsoft Azure. Muestra las prácticas de telemetría y registro, como se explica en los artículos siguientes. En el ejemplo se implementa el registro de la aplicación mediante [NLog](http://nlog-project.org/). Para obtener documentación relacionada, consulte la [serie de cuatro artículos de wiki de TechNet sobre la telemetría y el registro](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx#Telemetry_coming_soon).

> [!div class="step-by-step"]
> [Anterior](design-to-survive-failures.md)
> [Siguiente](transient-fault-handling.md)
