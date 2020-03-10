---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apéndice: aplicación de ejemplo de corrección (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs'
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 896196bdb6a6b0d12a6c798ead510e37dd38a9fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471397"
---
# <a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apéndice: aplicación de ejemplo de corrección (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Este apéndice para crear aplicaciones en la nube de mundo real con el libro electrónico de Azure contiene las siguientes secciones que proporcionan información adicional sobre la aplicación de ejemplo de corrección de ti que puede descargar:

- [Problemas conocidos](#knownissues)
- [Procedimientos recomendados](#bestpractices)
- [Cómo ejecutar la aplicación desde Visual Studio en el equipo local](#run-in-vs)
- [Implementación de la aplicación base para Azure App Service Web Apps mediante scripts de Windows PowerShell](#deploybase)
- [Solución de problemas de los scripts de Windows PowerShell](#troubleshooting)
- [Implementación de la aplicación con el procesamiento de colas para Azure App Service Web Apps y un servicio en la nube de Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conocidos

La aplicación Fix it se desarrolló originalmente para ilustrar lo más sencillo posible algunos de los patrones presentados en este libro electrónico. Sin embargo, dado que el libro electrónico trata sobre la creación de aplicaciones reales, nos hemos sometido al código de corrección para un proceso de revisión y pruebas similar a lo que hacemos para el software publicado. Encontramos varios problemas y, al igual que con cualquier aplicación real, algunos de ellos se corrigieron y algunos de ellos se aplazón a una versión posterior.

En la lista siguiente se incluyen los problemas que deben abordarse en una aplicación de producción, pero por un motivo u otro que decidimos no abordar en la versión inicial de la aplicación de ejemplo Fix it.

### <a name="security"></a>Seguridad

- Asegúrese de que no se puede asignar una tarea a un propietario no existente.
- Asegúrese de que solo puede ver y modificar las tareas que ha creado o que tiene asignadas.
- Use HTTPS para las páginas de inicio de sesión y las cookies de autenticación.
- Especifique un límite de tiempo para las cookies de autenticación.

### <a name="input-validation"></a>Validación de entradas

En general, una aplicación de producción realizaría más validaciones de entrada que la aplicación de corrección de ti. Por ejemplo, el tamaño de imagen/tamaño de archivo de imagen permitido para la carga debe ser limitado.

### <a name="administrator-functionality"></a>Funcionalidad de administrador

Un administrador debe poder cambiar la propiedad de las tareas existentes. Por ejemplo, el creador de una tarea podría dejar la compañía, sin dejar ninguna con autoridad para mantener la tarea a menos que esté habilitado el acceso administrativo.

### <a name="queue-message-processing"></a>Procesamiento de mensajes en cola

El procesamiento de mensajes en cola en la aplicación Fix it se diseñó para ser sencillo con el fin de ilustrar el patrón de trabajo centrado en cola con una cantidad mínima de código. Este código simple no sería adecuado para una aplicación de producción real.

- El código no garantiza que cada mensaje de la cola se procese como máximo una vez. Cuando recibe un mensaje de la cola, hay un período de tiempo de espera, durante el cual el mensaje es invisible para otros agentes de escucha de la cola. Si el tiempo de espera expira antes de que se elimine el mensaje, el mensaje vuelve a estar visible. Por lo tanto, si una instancia de rol de trabajo dedica mucho tiempo a procesar un mensaje, teóricamente es posible que el mismo mensaje se procese dos veces, lo que da lugar a una tarea duplicada en la base de datos. Para obtener más información acerca de este problema, consulte [uso de colas de Azure Storage](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- La lógica de sondeo de la cola puede ser más rentable, mediante el procesamiento por lotes de la recuperación de mensajes. Cada vez que llame a [CloudQueue. GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), existe un costo de transacción. En su lugar, puede llamar a [CloudQueue. GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (tenga en cuenta el ') del plural, que recibe varios mensajes en una sola transacción. Los costos de las transacciones Azure Storage las colas son muy bajos, por lo que el impacto en los costos no es importante en la mayoría de los escenarios.
- El bucle estrecho en el código de procesamiento de mensajes en cola provoca la afinidad de la CPU, que no emplea máquinas virtuales de varios núcleos de forma eficaz. Un mejor diseño sería usar el paralelismo de tareas para ejecutar varias tareas asincrónicas en paralelo.
- Mensaje en cola: el procesamiento solo tiene un control de excepciones rudimentaria. Por ejemplo, el código no controla [los mensajes dudosos](https://msdn.microsoft.com/library/ms789028.aspx). (Cuando el procesamiento de mensajes produce una excepción, tiene que registrar el error y eliminar el mensaje, o bien el rol de trabajo intentará procesarlo de nuevo y el bucle continuará indefinidamente).

### <a name="sql-queries-are-unbounded"></a>Las consultas SQL están sin enlazar

El código de corrección actual no establece ningún límite en el número de filas que pueden devolver las consultas para las páginas de índice. Si se especifica un gran volumen de tareas en la base de datos, el tamaño de las listas resultantes recibidas podría causar problemas de rendimiento. La solución consiste en implementar la paginación. Para obtener un ejemplo, vea [ordenación, filtrado y paginación con el Entity Framework en una aplicación ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Ver modelos recomendados

La aplicación Fix it usa la clase de entidad FixItTask para pasar información entre el controlador y la vista. Un procedimiento recomendado es usar los modelos de vista. El modelo de dominio (por ejemplo, la clase de entidad FixItTask) está diseñado en torno a lo que se necesita para la persistencia de datos, mientras que un modelo de vista se puede diseñar para la presentación de datos. Para obtener más información, vea [12 procedimientos recomendados de MVC de ASP.net](https://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Se recomienda el BLOB de imagen segura

La aplicación Fix it almacena las imágenes cargadas como públicas, lo que significa que cualquiera que encuentre la dirección URL puede acceder a las imágenes. Las imágenes se pueden proteger en lugar de ser públicas.

### <a name="no-powershell-automation-scripts-for-queues"></a>No hay scripts de automatización de PowerShell para colas

Los scripts de Automation de PowerShell de ejemplo solo se escribieron para la versión base de Fix que se ejecuta completamente en Azure App Service Web Apps. No se han proporcionado scripts para configurar e implementar en la aplicación Web más el entorno de servicio en la nube necesario para el procesamiento de colas.

### <a name="special-handling-for-html-codes-in-user-input"></a>Control especial para códigos HTML en datos proporcionados por el usuario

ASP.NET evita automáticamente muchas maneras en las que los usuarios malintencionados podrían intentar ataques de scripts entre sitios escribiendo el script en los cuadros de texto de entrada del usuario. Y la aplicación auxiliar de `DisplayFor` MVC que se usa para mostrar los títulos de tarea y las notas codifica automáticamente los valores que envía al explorador. Pero en una aplicación de producción podría querer tomar medidas adicionales. Para obtener más información, consulte [solicitud de validación en ASP.net](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Procedimientos recomendados

A continuación se indican algunos problemas que se corrigieron después de detectarse en la revisión de código y las pruebas de la versión original de la aplicación de corrección de ti. Algunos se deben a que el codificador original no es consciente de un procedimiento recomendado determinado, algunos simplemente porque el código se ha escrito rápidamente y no se ha diseñado para software publicado. Aquí se enumeran los problemas en caso de que se haya aprendido esta revisión y las pruebas que podrían ser útiles para otras personas que también desarrollan aplicaciones Web.

### <a name="dispose-the-database-repository"></a>Desechar el repositorio de base de datos

La clase `FixItTaskRepository` debe eliminar la instancia de `DbContext` Entity Framework. Lo hicimos implementando `IDisposable` en la clase `FixItTaskRepository`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Tenga en cuenta que AutoFac eliminará automáticamente la instancia de `FixItTaskRepository`, por lo que no es necesario desecharla explícitamente.

Otra opción consiste en quitar el `DbContext` variable miembro de `FixItTaskRepository`y, en su lugar, crear una variable local `DbContext` dentro de cada método del repositorio, dentro de una instrucción `using`. Por ejemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singleton como tal con DI

Puesto que solo se necesita una instancia de la clase `PhotoService` y `Logger` clase, estas clases deben [registrarse como instancias únicas para la inserción de dependencias](https://code.google.com/p/autofac/wiki/InstanceScope) en *DependenciesConfig.CS*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Seguridad: no mostrar los detalles del error a los usuarios

La aplicación de corrección de ti original no tenía una página de error genérica y solo permite que todas las excepciones se propaguen hasta la interfaz de usuario, por lo que algunas excepciones, como los errores de conexión de base de datos, pueden dar lugar a que se muestre un seguimiento de la pila completo en el explorador. A veces, la información detallada del error puede facilitar los ataques de usuarios malintencionados. La solución consiste en registrar los detalles de la excepción y mostrar una página de error al usuario que no incluye los detalles del error. La aplicación Fix it ya estaba registrando y, para mostrar una página de error, agregamos `<customErrors mode=On>` en el archivo Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

De forma predeterminada, esto hace que *Views\Shared\Error.cshtml* se muestre en busca de errores. Puede personalizar *error. cshtml* o crear su propia vista de página de error y agregar un atributo `defaultRedirect`. También puede especificar diferentes páginas de error para errores concretos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Seguridad: solo permite que el creador edite una tarea

La página de índice del panel solo muestra las tareas creadas por el usuario que ha iniciado sesión, pero un usuario malintencionado podría crear una dirección URL con un identificador a la tarea de otro usuario. Hemos agregado código en *DashboardController.CS* para devolver 404 en ese caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>No admitir excepciones

La aplicación Fix it original devolvió NULL después de registrar una excepción que resultó de una consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

De este modo, se vería al usuario como si la consulta se realizara correctamente pero no devolvería ninguna fila. La solución consiste en volver a producir la excepción después de detectar y registrar:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Detección de todas las excepciones en los roles de trabajo

Cualquier excepción no controlada en un rol de trabajo hará que la máquina virtual se recicle, por lo que querrá incluir todo lo que haga en un bloque try-catch y controlar todas las excepciones.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar la longitud de las propiedades de cadena en las clases de entidad

Para Mostrar código simple, la versión original de la aplicación Fix it no especificó longitudes para los campos de la entidad FixItTask y, como resultado, se definieron como varchar (Max) en la base de datos. Como resultado, la interfaz de usuario aceptaría prácticamente cualquier cantidad de entrada. Especificar longitudes establece límites que se aplican a los datos proporcionados por el usuario en la página web y el tamaño de la columna en la base de datos:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar miembros privados como ReadOnly cuando no se espera que cambien

Por ejemplo, en la clase `DashboardController` se crea una instancia de `FixItTaskRepository` y no se espera que cambie, por lo que se define como [ReadOnly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Usar lista. Any () en lugar de List. Count () &gt; 0

Si todo lo que le interesa es que uno o varios elementos de una lista se ajusten a los criterios especificados, use el método [any](https://msdn.microsoft.com/library/bb534972.aspx) , ya que devuelve en cuanto se encuentra un elemento de ajuste de los criterios, mientras que el método `Count` siempre tiene que recorrer en iteración todos los elementos. El archivo *index. cshtml* del panel tenía originalmente este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Lo hemos cambiado por este:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generación de direcciones URL en las vistas de MVC con aplicaciones auxiliares de MVC

En el botón **crear un solucionador de ti** de la Página principal, la aplicación corregir it ha codificado de forma rígida un elemento delimitador:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

En el caso de vínculos de vista o acción como este, es mejor usar la aplicación auxiliar de HTML de la [dirección URL. Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) , por ejemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Usar Task. Delay en lugar de Thread. Sleep en el rol de trabajo

La plantilla New-Project coloca `Thread.Sleep` en el código de ejemplo de un rol de trabajo, pero hacer que el subproceso se suspenda puede provocar que el grupo de subprocesos genere subprocesos innecesarios adicionales. Puede evitarlo mediante [Task. Delay](https://msdn.microsoft.com/library/hh139096.aspx) en su lugar.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitar la anulación de Async

Si un método asincrónico no necesita devolver un valor, devuelve un tipo de `Task` en lugar de `void`.

Este ejemplo es de la clase `FixItQueueManager`:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Solo debe usar `async void` para los controladores de eventos de nivel superior. Si define un método como `async void`, el llamador no puede **esperar** al método o detectar las excepciones que el método produce. Para obtener más información, vea [prácticas recomendadas en programación asincrónica](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Usar un token de cancelación para interrumpir el bucle de rol de trabajo

Normalmente, el método **Run** de un rol de trabajo contiene un bucle infinito. Cuando se detiene el rol de trabajo, se llama al método [RoleEntryPoint. OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) . Debe utilizar este método para cancelar el trabajo que se realiza en el método **Run** y salir correctamente. De lo contrario, el proceso podría terminar en medio de una operación.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>No participar en el procedimiento automático de rastreo de MIME

En algunos casos, Internet Explorer informa de un tipo MIME diferente del tipo especificado por el servidor Web. Por ejemplo, si Internet Explorer encuentra contenido HTML en un archivo entregado con el encabezado de respuesta HTTP Content-Type: text/plain, Internet Explorer determina que el contenido se debe representar como HTML. Desafortunadamente, este "examen de MIME" también puede provocar problemas de seguridad en los servidores que hospedan contenido que no es de confianza. Para combatir este problema, Internet Explorer 8 ha realizado varios cambios en el código de determinación del tipo MIME y permite a los desarrolladores [de aplicaciones dejar de participar en el examen de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). El código siguiente se ha agregado al archivo *Web. config* .

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitación de la agrupación y minificación

Cuando Visual Studio crea un nuevo proyecto Web, la agrupación y minificación de archivos JavaScript no está habilitada de forma predeterminada. Hemos agregado una línea de código en BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Establecer un tiempo de espera de expiración para las cookies de autenticación

De forma predeterminada, las cookies de autenticación caducan en dos semanas. Un tiempo más corto es más seguro. Puede cambiar esta configuración en *StartupAuth.CS*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Cómo ejecutar la aplicación desde Visual Studio en el equipo local

Hay dos maneras de ejecutar la aplicación Fix it:

- Ejecute la aplicación base que escribe tareas nuevas directamente en la base de datos SQL.
- Ejecute la aplicación con una cola más un servicio back-end para crear tareas. El patrón de cola se describe en el capítulo [patrón de trabajo centrado en cola](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Ejecutar la aplicación base

1. Instalación de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Instale el [SDK de Azure para .net para Visual Studio](https://azure.microsoft.com/downloads/).
3. Descargue el archivo. zip de la [Galería de código de MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. En el explorador de archivos, haga clic con el botón derecho en el archivo. zip, haga clic en propiedades y, a continuación, en el ventana Propiedades haga clic en desbloquear.
5. Descomprima el archivo.
6. Haga doble clic en el archivo. sln para iniciar Visual Studio.
7. En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.
8. En la consola del administrador de paquetes (PMC), haga clic en restaurar.
9. Salga de Visual Studio.
10. Inicie el [emulador de almacenamiento de Azure](/azure/storage/common/storage-use-emulator).
11. Reinicie Visual Studio y abra el archivo de solución que cerró en el paso anterior.
12. Asegúrese de que el proyecto FixIt está establecido como proyecto de inicio y, a continuación, presione CTRL + F5 para ejecutar el proyecto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Ejecutar la aplicación con el procesamiento de colas

1. Siga las instrucciones para [ejecutar la aplicación base](#runbase)y, a continuación, cierre el explorador y cierre Visual Studio.
2. Inicie Visual Studio con privilegios de administrador. (Va a usar el emulador de proceso de Azure y que requiere privilegios de administrador).
3. En el archivo *Web. config* de la aplicación en el proyecto *MyFixIt* (el proyecto web), cambie el valor de `appSettings/UseQueues` a "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Si el [emulador de Azure Storage](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) aún no se está ejecutando, inícielo de nuevo.
5. Ejecute el proyecto web FixIt y el proyecto MyFixItCloudService simultáneamente.

    Con Visual Studio:

   1. Presione **F5** para ejecutar el proyecto FixIt.
   2. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto MyFixItCloudService y, a continuación, haga clic en **depurar** > **Iniciar nueva instancia**.

    Usar Visual Studio 2013 Express para Web:

   3. En Explorador de soluciones, haga clic con el botón secundario en la solución FixIt y seleccione **propiedades**.
   4. Seleccione **proyectos de inicio múltiples**.
   5. En la lista desplegable **acción** , en MyFixIt y MyFixItCloudService, seleccione **iniciar**.
   6. Haga clic en **Aceptar**.
   7. Presione **F5** para ejecutar ambos proyectos.

      Al ejecutar el proyecto MyFixItCloudService, Visual Studio inicia el emulador de proceso de Azure. En función de la configuración del firewall, es posible que deba permitir el emulador a través del firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Implementación de la aplicación base para Azure App Service Web Apps mediante scripts de Windows PowerShell

Para ilustrar el patrón [automatizar todo](automate-everything.md) , se proporciona la aplicación Fix it con scripts que configuran un entorno en Azure e implementan el proyecto en el nuevo entorno. En las instrucciones siguientes se explica cómo usar los scripts.

Si desea ejecutar en Azure sin usar colas y realizó los cambios para que se ejecuten localmente con colas, asegúrese de establecer el valor de UseQueues appSetting de nuevo en false antes de continuar con las instrucciones siguientes.

En estas instrucciones se supone que ya ha descargado y ejecutado la solución de corrección de ti localmente y que tiene una cuenta de Azure o tiene una suscripción de Azure que está autorizado a administrar.

1. Instale la consola de **Azure PowerShell** . Para obtener instrucciones, consulte [Instalación y configuración de Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esta consola personalizada está configurada para trabajar con su suscripción de Azure. El módulo de Azure se instala en el directorio *archivos de programa* y se importa automáticamente en cada uso de la consola de Azure PowerShell.

    Si prefiere trabajar en otro programa host, como Windows PowerShell ISE, asegúrese de usar el cmdlet [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) para importar el módulo de Azure o usar un comando en el módulo de Azure para desencadenar la importación automática del módulo.
2. Inicie Azure PowerShell con la opción **Ejecutar como administrador** .
3. Ejecute el cmdlet [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) para establecer la Directiva de ejecución Azure PowerShell en `RemoteSigned`. Escriba **Y** (para sí) para completar el cambio de directiva.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Esta configuración permite ejecutar scripts locales que no están firmados digitalmente. (También puede establecer la Directiva de ejecución en `Unrestricted`, lo que eliminará la necesidad del paso de desbloqueo más adelante, pero esto no se recomienda por motivos de seguridad).
4. Ejecute el cmdlet `Add-AzureAccount` para configurar PowerShell con las credenciales de su cuenta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Estas credenciales expiran después de un período de tiempo y tiene que volver a ejecutar el cmdlet `Add-AzureAccount`. A medida que se escribe este libro electrónico, el límite de tiempo antes de que expiren las credenciales es de 12 horas.
5. Si tiene varias suscripciones, use el cmdlet Select-AzureSubscription para especificar la suscripción en la que desea crear el entorno de prueba.
6. Importe un certificado de administración para la misma suscripción de Azure mediante los cmdlets `Get-AzurePublishSettingsFile` y `Import-AzurePublishSettingsFile`. El primero de estos cmdlets descarga un archivo de certificado y, en el segundo, se especifica la ubicación de ese archivo para poder importarlo. > [!IMPORTANT]
   > Mantenga el archivo descargado en una ubicación segura o elimínelo cuando haya terminado, ya que contiene un certificado que se puede usar para administrar los servicios de Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    El certificado se usa para una llamada de API de REST que detecta la dirección IP del equipo de desarrollo con el fin de establecer una regla de firewall en el servidor de SQL Database.
7. Ejecute el cmdlet [set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) (los alias se `cd`, `chdir`y `sl`) para desplazarse al directorio que contiene los scripts. (Se encuentran en la carpeta *Automation* de la carpeta Fix it Solution). Coloque la ruta de acceso entre comillas si alguno de los nombres de directorio contiene espacios. Por ejemplo, para ir al directorio `c:\Sample Apps\FixIt\Automation`, puede escribir el siguiente comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que Windows PowerShell ejecute estos scripts, use el cmdlet [unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) . (Los scripts están bloqueados porque se descargaron de Internet).

    > [!WARNING]
    > Seguridad: antes de ejecutar `Unblock-File` en cualquier script o archivo ejecutable, abra el archivo en el Bloc de notas, examine los comandos y compruebe que no contienen código malintencionado.

    Por ejemplo, el siguiente comando ejecuta el cmdlet `Unblock-File` en todos los scripts del directorio actual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para crear la aplicación web para la aplicación de corrección de base (sin procesamiento de colas), ejecute el script de creación del entorno.

    El parámetro `Name` obligatorio especifica el nombre de la base de datos y también se utiliza para la cuenta de almacenamiento que crea el script. El nombre debe ser único globalmente dentro del dominio azurewebsites.net. Si especifica un nombre que no es único, como FixIt o test (o incluso como en el ejemplo, fixitdemo), el cmdlet `New-AzureWebsite` produce un error interno que informa de un conflicto. El script convierte el nombre en minúsculas para cumplir los requisitos de nombre de las aplicaciones Web, las cuentas de almacenamiento y las bases de datos.

    El parámetro `SqlDatabasePassword` necesario especifica la contraseña de la cuenta de administrador que se creará para SQL Database. No incluya caracteres XML especiales en la contraseña (&amp; &lt; &gt;;). Se trata de una limitación de la forma en que se escribieron los scripts, no una limitación de Azure.

    Por ejemplo, si desea crear una aplicación web denominada "fixitdemo" y usar una contraseña de administrador de SQL Server de "Passw0rd1", puede escribir el siguiente comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    El nombre debe ser único en el dominio azurewebsites.net y la contraseña debe cumplir los requisitos de SQL Database para la complejidad de la contraseña. (El Passw0rd1 de ejemplo cumple los requisitos).

    Tenga en cuenta que el comando comienza por ".\". Para ayudar a evitar la ejecución malintencionada de scripts, Windows PowerShell requiere que proporcione la ruta de acceso completa al archivo de script al ejecutar un script. Puede usar un punto para indicar el directorio actual (".\") o proporcione la ruta de acceso completa, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obtener más información sobre el script, use el cmdlet `Get-Help`.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Puede usar los parámetros `Detailed`, `Full`, `Parameters`y `Examples` del cmdlet Get-Help para filtrar la ayuda que se devuelve.

    Si el script produce un error o genera errores, como "New-AzureWebsite: Call Set-AzureSubscription y Select-AzureSubscription First", es posible que no haya completado la configuración de Azure PowerShell.

    Una vez finalizado el script, puede usar Azure Portal de administración para ver los recursos que se crearon, tal como se muestra en el capítulo [automatizar todo](automate-everything.md) .
10. Para implementar el proyecto FixIt en el nuevo entorno de Azure, use el script *AzureWebsite. PS1* . Por ejemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Una vez realizada la implementación, el explorador se abre con corregirlo en ejecución en Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solución de problemas de los scripts de Windows PowerShell

Los errores más comunes que se producen al ejecutar estos scripts están relacionados con los permisos. Asegúrese de que `Add-AzureAccount` y `Import-AzurePublishSettingsFile` fueron correctos y que los usó para la misma suscripción de Azure. Incluso si `Add-AzureAccount` fue correcta, puede que tenga que ejecutarlo de nuevo. Los permisos agregados por `Add-AzureAccount` expiran en 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referencia a objeto no establecida como instancia de un objeto.

Si el script devuelve errores, como "referencia a objeto no establecida en una instancia de un objeto", lo que significa que Windows PowerShell no puede encontrar un objeto para procesar (se trata de una excepción de referencia nula), ejecute el cmdlet `Add-AzureAccount` y vuelva a intentar el script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: el servidor encontró un error interno.

El cmdlet `New-AzureWebsite` devuelve un error interno cuando el nombre no es único en el dominio azurewebsites.net. Para resolver el error, use un valor diferente para el nombre, que se encuentra en el parámetro name de *New-AzureWebsiteEnv. PS1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reinicio del script

Si necesita reiniciar el script *New-AzureWebsiteEnv. PS1* porque se produjo un error antes de imprimir el mensaje "el script se ha completado", es posible que quiera eliminar los recursos que creó el script antes de detenerse. Por ejemplo, si el script ya creó la aplicación web ContosoFixItDemo y vuelve a ejecutar el script con el mismo nombre, se producirá un error en el script porque el nombre está en uso.

Para determinar qué recursos creó el script antes de detenerse, use los cmdlets siguientes:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: para ejecutar este cmdlet, Canalice el nombre del servidor de base de datos a `Get-AzureSqlDatabase`: `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para eliminar estos recursos, use los comandos siguientes. Tenga en cuenta que si elimina el servidor de base de datos, eliminará automáticamente las bases de datos asociadas con el servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Implementación de la aplicación con el procesamiento de colas para Azure App Service Web Apps y un servicio en la nube de Azure

Para habilitar las colas, realice el siguiente cambio en el archivo MyFixIt\Web.config. En `appSettings`, cambie el valor de `UseQueues` a "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

A continuación, implemente la aplicación MVC en una aplicación web en Azure App Service, como se describió [anteriormente](#deploybase).

A continuación, cree un nuevo servicio en la nube de Azure. Los scripts que se incluyen con la aplicación Fix it no crean ni implementan el servicio en la nube, por lo que debe usar Azure Portal para ello. En el portal, haga clic en **nuevo** -- **Compute** : **servicio en la nube** -- **creación rápida**y, a continuación, escriba una dirección URL y una ubicación de centro de datos. Use el mismo centro de datos en el que implementó la aplicación Web.

![](the-fix-it-sample-application/_static/image1.png)

Para poder implementar el servicio en la nube, debe actualizar algunos de los archivos de configuración.

En MyFixIt. WorkerRole\app.config, en `connectionStrings`, reemplace el valor de la cadena de conexión `appdb` por la cadena de conexión real para el SQL Database. Puede obtener la cadena de conexión desde el portal. En el portal, haga clic en **bases de datos SQL** - **appdb** - **Ver SQL Database cadenas de conexión para ADO .net, ODBC, PHP y JDBC**. Copie la cadena de conexión ADO.NET y pegue el valor en el archivo app. config. Reemplace "{Your\_password\_here}" por la contraseña de la base de datos. (Suponiendo que usó los scripts para implementar la aplicación MVC, especificó la contraseña de la base de datos en el parámetro del script `SqlDatabasePassword`).

El resultado debería ser similar al siguiente:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

En el mismo archivo MyFixIt. WorkerRole\app.config, en `appSettings`, reemplace los dos valores de marcador de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Puede obtener la clave de acceso desde el portal. Consulte [Cómo administrar cuentas de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

En MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, reemplace los mismos dos valores de marcadores de posición para la cuenta de almacenamiento de Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Ahora está listo para implementar el servicio en la nube. En el explorador de soluciones, haga clic con el botón derecho en el proyecto MyFixItCloudService y seleccione **publicar**. Para obtener más información, consulte "[implementación de la aplicación en Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que se encuentra en la parte 2 de [este tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
