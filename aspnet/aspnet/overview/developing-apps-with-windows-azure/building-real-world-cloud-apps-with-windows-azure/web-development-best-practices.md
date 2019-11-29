---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Prácticas recomendadas de desarrollo web (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 0956aaaf1f6a1a0d2f5d93f98cb6959cec98dbaf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582702"
---
# <a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Prácticas recomendadas de desarrollo web (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Los tres primeros patrones fueron acerca de cómo configurar un proceso de desarrollo ágil; el resto está sobre la arquitectura y el código. Se trata de una colección de procedimientos recomendados de desarrollo web:

- [Servidores Web sin estado](#stateless) detrás de un equilibrador de carga inteligente.
- [Evite el estado](#sessionstate) de la sesión (o si no puede evitarlo, utilice la caché distribuida en lugar de una base de datos).
- [Usar una red CDN](#cdn) para almacenar en la caché perimetral los recursos de archivos estáticos (imágenes, scripts).
- [Use la compatibilidad asincrónica de .net 4.5](#async) para evitar llamadas de bloqueo.

Estas prácticas son válidas para todo el desarrollo web, no solo para las aplicaciones en la nube, pero son especialmente importantes para las aplicaciones en la nube. Funcionan de forma conjunta para ayudarle a hacer un uso óptimo del escalado altamente flexible que ofrece el entorno de nube. Si no sigue estos procedimientos, tendrá limitaciones cuando intente escalar la aplicación.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Nivel de Web sin estado detrás de un equilibrador de carga inteligente

El *nivel de Web sin estado* significa que no almacena ningún dato de aplicación en el sistema de archivos o la memoria del servidor Web. Mantener el nivel de Web sin estado le permite proporcionar una mejor experiencia de cliente y ahorrar dinero:

- Si el nivel Web no tiene estado y se encuentra detrás de un equilibrador de carga, puede responder rápidamente a los cambios en el tráfico de la aplicación mediante la adición o eliminación dinámica de servidores. En el entorno de nube en el que solo paga por los recursos del servidor mientras los usa realmente, la capacidad de responder a los cambios en la demanda puede traducirse en grandes ahorros.
- Un nivel de Web sin estado es, en la arquitectura, mucho más sencillo de escalar horizontalmente la aplicación. Esto le permite también responder a las necesidades de escalado más rápidamente y gastar menos dinero en el desarrollo y las pruebas en el proceso.
- Los servidores en la nube, como los servidores locales, deben revisarse y reiniciarse ocasionalmente; y si el nivel Web no tiene estado, el redireccionamiento del tráfico cuando un servidor deja de funcionar temporalmente no causará errores o un comportamiento inesperado.

La mayoría de las aplicaciones del mundo real necesitan almacenar el estado de una sesión Web. el punto principal aquí no es almacenarlo en el servidor Web. Puede almacenar el estado de otras maneras, por ejemplo, en el cliente en cookies o fuera del servidor de proceso en el estado de sesión de ASP.NET mediante un proveedor de caché. Puede almacenar archivos en el [almacenamiento de blobs de Windows Azure](unstructured-blob-storage.md) en lugar del sistema de archivos local.

Como ejemplo de lo fácil que es escalar una aplicación en sitios web de Windows Azure si el nivel Web no tiene estado, consulte la pestaña **escala** de un sitio web de Windows Azure en el portal de administración:

![Pestaña escala](web-development-best-practices/_static/image1.png)

Si desea agregar servidores Web, basta con que arrastre el control deslizante recuento de instancias hacia la derecha. Establézcalo en 5 y haga clic en **Guardar**y, en segundos, tiene 5 servidores Web en Windows Azure que controlan el tráfico del sitio Web.

![Cinco instancias](web-development-best-practices/_static/image2.png)

Puede establecer fácilmente el recuento de instancias en 3 o en 1. Al escalar de nuevo, se empieza a ahorrar dinero inmediatamente porque Windows Azure cobra por minuto, no por hora.

También puede indicar a Windows Azure que aumente o disminuya automáticamente el número de servidores Web en función del uso de la CPU. En el ejemplo siguiente, cuando el uso de CPU va por debajo del 60%, el número de servidores web disminuirá a un mínimo de 2 y, si el uso de la CPU supera el 80%, el número de servidores Web se incrementará hasta un máximo de 4.

![Escalado por uso de CPU](web-development-best-practices/_static/image3.png)

O bien, ¿qué ocurre si sabe que el sitio solo estará ocupado durante las horas de trabajo? Puede indicar a Windows Azure que ejecute varios servidores durante el día de la noche y disminuyó a un solo servidor, noches y fines de semana. En la siguiente serie de capturas de pantalla se muestra cómo configurar el sitio web para ejecutar un servidor en horas de inactividad y 4 servidores durante las horas de trabajo de 8 A.M. a 5 P.M.

![Escalado por programación](web-development-best-practices/_static/image4.png)

![Establecer horas de programación](web-development-best-practices/_static/image5.png)

![Programación diurna](web-development-best-practices/_static/image6.png)

![Programación de weeknight](web-development-best-practices/_static/image7.png)

![Programación del fin de semana](web-development-best-practices/_static/image8.png)

Y, por supuesto, todo esto se puede hacer en scripts, así como en el portal.

La capacidad de escalar horizontalmente la aplicación es casi ilimitada en Windows Azure, siempre que evite impedimentos para agregar o quitar de forma dinámica máquinas virtuales de servidor, manteniendo el nivel de Web sin estado.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Evitar el estado de sesión

A menudo no resulta práctico en una aplicación en la nube del mundo real para evitar almacenar algún tipo de estado para una sesión de usuario, pero algunos enfoques afectan al rendimiento y la escalabilidad más que otros. Si tiene que almacenar el estado, la mejor solución es mantener la cantidad de estado pequeña y almacenarla en cookies. Si eso no es factible, la siguiente mejor solución es usar el estado de sesión ASP.NET con un proveedor para la [caché distribuida en memoria](distributed-caching.md#sessionstate). La peor solución desde el punto de vista de rendimiento y escalabilidad es usar un proveedor de estado de sesión respaldado por la base de datos.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Usar una red CDN para almacenar en caché recursos de archivos estáticos

La red CDN es un acrónimo de Content Delivery Network. Los recursos de archivos estáticos, como imágenes y archivos de script, se proporcionan a un proveedor de CDN, y el proveedor almacena en caché estos archivos en todos los centros de datos de todo el mundo para que las personas tengan acceso a la aplicación, obtengan una respuesta relativamente rápida y una latencia baja para la memoria caché. circula. Esto acelera el tiempo de carga general del sitio y reduce la carga en los servidores Web. Las redes CDN son especialmente importantes si está llegando a una audiencia que está ampliamente distribuida geográficamente.

Windows Azure tiene una red CDN y puede usar otras redes CDN en una aplicación que se ejecuta en Windows Azure o en cualquier entorno de hospedaje Web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Usar la compatibilidad asincrónica de .NET 4.5 para evitar llamadas de bloqueo

.NET 4,5 mejoró C# los lenguajes de programación de y VB para que sea mucho más sencillo controlar las tareas de forma asincrónica. La ventaja de la programación asincrónica no es solo para situaciones de procesamiento paralelo, como cuando se desea iniciar varias llamadas al servicio Web simultáneamente. También permite que el servidor Web funcione de forma más eficaz y confiable en condiciones de carga elevada. Un servidor web solo tiene un número limitado de subprocesos disponibles y, en condiciones de carga alta cuando todos los subprocesos están en uso, las solicitudes entrantes deben esperar hasta que se liberen los subprocesos. Si el código de aplicación no controla tareas como consultas de base de datos y llamadas a servicios Web de forma asincrónica, muchos subprocesos están innecesariamente Unidos mientras el servidor está esperando una respuesta de e/s. Esto limita la cantidad de tráfico que el servidor puede controlar en condiciones de carga elevada. Con la programación asincrónica, los subprocesos que están a la espera de un servicio web o una base de datos para devolver los datos se liberan para atender las nuevas solicitudes hasta que se reciban los datos. En un servidor Web ocupado, se pueden procesar cientos o miles de solicitudes inmediatamente, lo que de otro modo sería esperar a que los subprocesos se liberen.

Como vimos anteriormente, es tan fácil reducir el número de servidores web que controlan el sitio web para aumentarlos. Por lo tanto, si un servidor puede lograr un mayor rendimiento, no necesitará tantas y podrá reducir los costos porque necesitará menos servidores para un volumen de tráfico determinado de lo que haría de otro modo.

La compatibilidad con el modelo de programación asincrónica de .NET 4,5 se incluye en ASP.NET 4,5 para Web Forms, MVC y Web API. en Entity Framework 6 y en la [API de Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Compatibilidad con Async en ASP.NET 4,5

En ASP.NET 4,5, se ha agregado compatibilidad con la programación asincrónica no solo al lenguaje, sino también a MVC, formularios Web Forms y marcos de API Web. Por ejemplo, un método de acción del controlador ASP.NET MVC recibe datos de una solicitud Web y los pasa a una vista que, a continuación, crea el código HTML que se va a enviar al explorador. Con frecuencia, el método de acción debe obtener datos de una base de datos o un servicio web para poder mostrarlos en una página web o para guardar los datos especificados en una página web. En esos escenarios es fácil hacer que el método de acción sea asincrónico: en lugar de devolver un objeto *ActionResult* , se devuelve *Task&lt;ActionResult&gt;* y se marca el método con la palabra clave *Async* . Dentro del método, cuando una línea de código inicia una operación que implica tiempo de espera, se marca con la palabra clave Await.

A continuación se muestra un método de acción simple que llama a un método de repositorio para una consulta de base de datos:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Y este es el mismo método que controla la llamada a la base de datos de forma asincrónica:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

En, el compilador genera el código asincrónico adecuado. Cuando la aplicación realiza la llamada a `FindTaskByIdAsync`, ASP.NET realiza el `FindTask` solicitud y, a continuación, desenreda el subproceso de trabajo y lo pone a disposición para procesar otra solicitud. Cuando se realiza la solicitud de `FindTask`, se reinicia un subproceso para continuar procesando el código que viene después de esa llamada. Durante el tiempo que transviene entre el momento en que se inicia la solicitud de `FindTask` y el momento en que se devuelven los datos, tiene un subproceso disponible para realizar un trabajo útil que, de otro modo, se enlazará a la espera de la respuesta.

Hay una sobrecarga para el código asincrónico, pero en condiciones de carga baja, esa sobrecarga es insignificante, mientras que en las condiciones de carga elevada se pueden procesar solicitudes que, de otro modo, se mantendrían en espera de los subprocesos disponibles.

Se ha podido realizar este tipo de programación asincrónica desde ASP.NET 1,1, pero era difícil de escribir, propensa a errores y difícil de depurar. Ahora que hemos simplificado la codificación en ASP.NET 4,5, ya no hay ninguna razón para hacerlo.

### <a name="async-support-in-entity-framework-6"></a>Compatibilidad con Async en Entity Framework 6

Como parte de la compatibilidad con Async en 4,5 se envió compatibilidad asincrónica con las llamadas de servicio Web, los sockets y la e/s del sistema de archivos, pero el patrón más común para las aplicaciones web es alcanzar una base de datos y nuestras bibliotecas de datos no admiten Async. Ahora Entity Framework 6 agrega compatibilidad asincrónica para el acceso a la base de datos.

En Entity Framework 6 todos los métodos que hacen que una consulta o un comando se envíen a la base de datos tienen versiones asincrónicas. En el ejemplo siguiente se muestra la versión asincrónica del método *Find* .

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Y esta compatibilidad asincrónica no solo funciona para inserciones, eliminaciones, actualizaciones y simples búsquedas, sino que también funciona con consultas LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Hay una versión `Async` del método `ToList` porque en este código es el método que hace que se envíe una consulta a la base de datos. Los métodos `Where` y `OrderByDescending` solo configuran la consulta, mientras que el método `ToListAsync` ejecuta la consulta y almacena la respuesta en la variable de `result`.

## <a name="summary"></a>Resumen

Puede implementar las prácticas recomendadas de desarrollo web que se describen aquí en cualquier marco de programación web y en cualquier entorno de nube, pero tenemos herramientas en ASP.NET y Windows Azure para facilitar la tarea. Si sigue estos patrones, puede escalar horizontalmente el nivel Web con facilidad y minimizará los gastos porque cada servidor podrá administrar más tráfico.

En el [siguiente capítulo](single-sign-on.md) se examina cómo la nube habilita los escenarios de inicio de sesión único.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos.

Servidores Web sin estado:

- [Microsoft Patterns and Practices: Guía de escalado automático](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deshabilitar la afinidad de la instancia de ARR en sitios web de Windows Azure](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Entrada de blog de Erez Benari, se explica la afinidad de la sesión en sitios web de Windows Azure.

COMPLEMENTA

- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de vídeos de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Consulte la explicación de la red CDN en el episodio 3 a partir de 1:34:00.
- [Patrón de Microsoft Patterns and Practices modelo de hospedaje de contenido estático](https://msdn.microsoft.com/library/dn589776.aspx)
- [Revisiones de la red CDN](http://www.cdnreviews.com/). Información general de muchas redes CDN.

Programación asincrónica:

- [Usar métodos asincrónicos en ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Tutorial de Rick Anderson.
- [Programación asincrónica con Async y Await (C# y Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). Notas del producto de MSDN que explican la lógica de la programación asincrónica, cómo funciona en ASP.NET 4,5 y cómo escribir código para implementarlo.
- [Entity Framework consulta asincrónica y guardar](https://msdn.microsoft.com/data/jj819165)
- [Cómo crear aplicaciones Web de ASP.net mediante Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Presentación en vídeo de Rowan Miller. Incluye una demostración gráfica de cómo la programación asincrónica puede facilitar aumentos drásticos en el rendimiento del servidor Web en condiciones de carga elevada.
- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de vídeos de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Para obtener información sobre el impacto de la programación asincrónica en la escalabilidad, consulte el episodio 4 y el episodio 8.
- [La magia de usar métodos asincrónicos en ASP.NET 4,5 más un problema importante](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Entrada de blog de Scott Hanselman, principalmente sobre el uso de Async en aplicaciones de formularios Web Forms de ASP.NET.

Para obtener más procedimientos recomendados de desarrollo web, consulte los siguientes recursos:

- [Procedimientos recomendados de la aplicación de ejemplo Fix it](the-fix-it-sample-application.md#bestpractices). En el apéndice de este libro electrónico se enumeran varios procedimientos recomendados que se implementaron en la aplicación Fix it.
- [Lista de comprobación para desarrolladores web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Anterior](continuous-integration-and-continuous-delivery.md)
> [Siguiente](single-sign-on.md)
