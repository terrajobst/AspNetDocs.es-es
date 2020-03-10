---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: uso de la resistencia de conexión y la intercepción de comandos con EF en una aplicación ASP.NET MVC'
author: tdykstra
description: En este tutorial aprenderá a usar la resistencia de conexión y la interceptación de comandos. Son dos características importantes de Entity Framework 6.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 276266f8ae9df38529d44742ebe6ac0dc8e79727
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471403"
---
# <a name="tutorial-use-connection-resiliency-and-command-interception-with-entity-framework-in-an-aspnet-mvc-app"></a>Tutorial: uso de la resistencia de conexión y la intercepción de comandos con Entity Framework en una aplicación ASP.NET MVC

Hasta ahora, la aplicación se ejecuta localmente en IIS Express en el equipo de desarrollo. Para que una aplicación real esté disponible para otros usuarios a través de Internet, debe implementarla en un proveedor de hospedaje web y debe implementar la base de datos en un servidor de base de datos.

En este tutorial aprenderá a usar la resistencia de conexión y la interceptación de comandos. Son dos características importantes de Entity Framework 6 que son especialmente útiles cuando se está implementando en el entorno de la nube: resistencia de la conexión (reintentos automáticos de errores transitorios) e intercepción de comandos (detectar todas las consultas SQL enviadas a la base de datos). para registrarlos o cambiarlos).

Esta resistencia de conexión y el tutorial de interceptación de comandos son opcionales. Si omite este tutorial, se deben realizar algunos ajustes menores en los tutoriales posteriores.

En este tutorial va a:

> [!div class="checklist"]
> * Habilitar resistencia de conexión
> * Habilitar la interceptación de comandos
> * Prueba de la nueva configuración

## <a name="prerequisites"></a>Requisitos previos

* [Ordenar, filtrar y paginar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-connection-resiliency"></a>Habilitar resistencia de conexión

Al implementar la aplicación en Windows Azure, implementará la base de datos en Windows Azure SQL Database, un servicio de base de datos en la nube. Los errores de conexión transitorios suelen ser más frecuentes cuando se conecta a un servicio de base de datos en la nube que cuando el servidor Web y el servidor de base de datos se conectan directamente en el mismo centro de datos. Incluso si un servidor Web en la nube y un servicio de base de datos en la nube se hospedan en el mismo centro de datos, hay más conexiones de red entre ellos que pueden tener problemas, como equilibradores de carga.

Además, un servicio en la nube suele ser compartido por otros usuarios, lo que significa que su capacidad de respuesta puede verse afectada. Y el acceso a la base de datos puede estar sujeto a limitación. La limitación significa que el servicio de base de datos produce excepciones cuando se intenta tener acceso a ella con más frecuencia de la que se permite en el Acuerdo de Nivel de Servicio (SLA).

Muchos o más problemas de conexión cuando se tiene acceso a un servicio en la nube son transitorios, es decir, se resuelven en un breve período de tiempo. Por lo tanto, cuando se intenta realizar una operación de base de datos y se obtiene un tipo de error que suele ser transitorio, puede volver a intentar la operación después de una breve espera y la operación puede ser correcta. Puede proporcionar una experiencia mucho mejor para los usuarios si controla los errores transitorios al intentarlo de nuevo automáticamente, lo que hace que la mayoría de ellos sean invisibles para el cliente. La característica de resistencia de conexión de Entity Framework 6 automatiza ese proceso de reintento de consultas SQL con errores.

La característica de resistencia de conexión debe estar configurada correctamente para un servicio de base de datos determinado:

- Tiene que saber qué excepciones es probable que sean transitorias. Desea volver a intentar los errores causados por una pérdida temporal de conectividad de red, no por errores causados por errores en el programa, por ejemplo.
- Tiene que esperar una cantidad de tiempo adecuada entre los reintentos de una operación con errores. Puede esperar más tiempo entre los reintentos de un proceso por lotes que en una página web en línea donde un usuario está esperando una respuesta.
- Tiene que Reintentar un número adecuado de veces antes de que se conceda. Es posible que desee volver a intentar más veces en un proceso por lotes que en una aplicación en línea.

Puede configurar estas opciones manualmente para cualquier entorno de base de datos compatible con un proveedor de Entity Framework, pero los valores predeterminados que normalmente funcionan bien para una aplicación en línea que usa Windows Azure SQL Database ya se han configurado para usted, y Estos son los valores que se van a implementar para la aplicación contoso University.

Todo lo que tiene que hacer para habilitar la resistencia de la conexión es crear una clase en el ensamblado que deriva de la clase [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) y, en esa clase, establecer la *estrategia de ejecución*SQL Database, que en EF es otro término para la *Directiva de reintentos*.

1. En la carpeta DAL, agregue un archivo de clase denominado *SchoolConfiguration.CS*.
2. Reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    El Entity Framework ejecuta automáticamente el código que encuentra en una clase que deriva de `DbConfiguration`. Puede usar la clase `DbConfiguration` para realizar tareas de configuración en código que, de lo contrario, haría en el archivo *Web. config* . Para obtener más información, vea [configuración basada en código EntityFramework](https://msdn.microsoft.com/data/jj680699).
3. En *StudentController.CS*, agregue una instrucción de `using` para `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Cambie todos los bloques de `catch` que detecten `DataException` excepciones para que detecten `RetryLimitExceededException` excepciones en su lugar. Por ejemplo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Usaba `DataException` para intentar identificar los errores que podrían ser transitorios con el fin de proporcionar un mensaje descriptivo "inténtelo de nuevo". Pero ahora que ha activado una directiva de reintentos, es posible que los únicos errores que puedan ser transitorios ya se hayan intentado y hayan generado un error varias veces y que la excepción real devuelta se ajuste en la `RetryLimitExceededException` excepción.

Para obtener más información, vea [Entity Framework la lógica de reintentos y la resistencia](https://msdn.microsoft.com/data/dn456835)de la conexión.

## <a name="enable-command-interception"></a>Habilitar la interceptación de comandos

Ahora que ha activado una directiva de reintentos, ¿cómo se prueba para comprobar que funciona según lo esperado? No es tan fácil forzar que se produzca un error transitorio, especialmente cuando se ejecuta localmente, y sería especialmente difícil integrar los errores transitorios reales en una prueba unitaria automatizada. Para probar la característica de resistencia de conexión, necesita una manera de interceptar las consultas que Entity Framework envía a SQL Server y reemplazar la respuesta de SQL Server por un tipo de excepción que suele ser transitorio.

También puede usar la intercepción de consultas para implementar un procedimiento recomendado para las aplicaciones en la nube: [registrar la latencia y el éxito o el fracaso de todas las llamadas a servicios externos](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) , como los servicios de base de datos. EF6 proporciona una [API de registro dedicada](https://msdn.microsoft.com/data/dn469464) que puede facilitar el registro, pero en esta sección del tutorial aprenderá a usar la [característica de intercepción](https://msdn.microsoft.com/data/dn469464) de Entity Framework directamente, tanto para el registro como para simular errores transitorios.

### <a name="create-a-logging-interface-and-class"></a>Crear una interfaz de registro y una clase

Un [procedimiento recomendado para el registro](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) es hacerlo mediante una interfaz en lugar de realizar llamadas de codificación rígida a System. Diagnostics. Trace o a una clase de registro. De este modo, es más fácil cambiar el mecanismo de registro más adelante si alguna vez tiene que hacerlo. Por lo tanto, en esta sección creará la interfaz de registro y una clase para implementarla./p >

1. Cree una carpeta en el proyecto y asígnele el nombre *Logging*.
2. En la carpeta de *registro* , cree un archivo de clase denominado *ILogger.CS*y reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    La interfaz proporciona tres niveles de seguimiento para indicar la importancia relativa de los registros y otro diseñado para proporcionar información de latencia para llamadas de servicio externas, como consultas de base de datos. Los métodos de registro tienen sobrecargas que permiten pasar una excepción. Esto es para que la clase que implementa la interfaz registre la información de la excepción, incluido el seguimiento de la pila y las excepciones internas, en lugar de confiar en que se realiza en cada llamada al método de registro en la aplicación.

    Los métodos TraceApi permiten realizar un seguimiento de la latencia de cada llamada a un servicio externo, como SQL Database.
3. En la carpeta de *registro* , cree un archivo de clase denominado *Logger.CS*y reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    La implementación de usa System. Diagnostics para realizar el seguimiento. Se trata de una característica integrada de .NET que facilita la generación y el uso de la información de seguimiento. Hay muchos "agentes de escucha" que puede usar con el seguimiento de System. Diagnostics, para escribir registros en archivos, por ejemplo, o para escribirlos en el almacenamiento de blobs en Azure. Consulte algunas de las opciones y vínculos a otros recursos para obtener más información, en [solución de problemas de sitios web de Azure en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). En este tutorial solo se examinan los registros de la ventana de **salida** de Visual Studio.

    En una aplicación de producción, es posible que quiera considerar la posibilidad de realizar un seguimiento de los paquetes que no sean System. Diagnostics, y la interfaz ILogger facilita la conmutación por un mecanismo de seguimiento diferente si decide hacerlo.

### <a name="create-interceptor-classes"></a>Crear clases de interceptor

A continuación, creará las clases a las que llamará el Entity Framework cada vez que vaya a enviar una consulta a la base de datos, una para simular errores transitorios y otra para el registro. Estas clases de interceptor deben derivar de la clase `DbCommandInterceptor`. En ellos se escriben invalidaciones de método a las que se llama automáticamente cuando se va a ejecutar la consulta. En estos métodos puede examinar o registrar la consulta que se va a enviar a la base de datos, y puede cambiar la consulta antes de que se envíe a la base de datos o de que se devuelva algo para Entity Framework sin pasar la consulta a la base de datos.

1. Para crear la clase Interceptor que registrará cada consulta SQL que se envía a la base de datos, cree un archivo de clase denominado *SchoolInterceptorLogging.CS* en la carpeta *Dal* y reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    En el caso de las consultas o los comandos correctos, este código escribe un registro de información con información de latencia. En el caso de las excepciones, crea un registro de errores.
2. Para crear la clase Interceptor que generará errores transitorios ficticios al escribir "Throw" en el cuadro de **búsqueda** , cree un archivo de clase denominado *SchoolInterceptorTransientErrors.CS* en la carpeta *Dal* y reemplace el código de plantilla por el código siguiente:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Este código solo invalida el método `ReaderExecuting`, al que se llama para las consultas que pueden devolver varias filas de datos. Si desea comprobar la resistencia de la conexión para otros tipos de consultas, también puede invalidar los métodos `NonQueryExecuting` y `ScalarExecuting`, como hace el interceptor de registro.

    Al ejecutar la página Student y escribir "Throw" como cadena de búsqueda, este código crea una excepción ficticia SQL Database para el número de error 20, un tipo que se sabe que suele ser transitorio. Otros números de error actualmente reconocidos como transitorios son 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 y 40613, pero están sujetos a cambios en nuevas versiones de SQL Database.

    El código devuelve la excepción que se va a Entity Framework en lugar de ejecutar la consulta y pasar los resultados de la consulta. La excepción transitoria se devuelve cuatro veces y, a continuación, el código revierte al procedimiento normal de pasar la consulta a la base de datos.

    Dado que se registra todo, podrá ver que Entity Framework intenta ejecutar la consulta cuatro veces antes de que finalice, y la única diferencia en la aplicación es que tarda más en representar una página con los resultados de la consulta.

    El número de veces que el Entity Framework volverá a intentarlo es configurable; el código especifica cuatro veces porque este es el valor predeterminado de la Directiva de ejecución SQL Database. Si cambia la Directiva de ejecución, también debe cambiar el código que especifica el número de veces que se generan errores transitorios. También puede cambiar el código para generar más excepciones, de modo que Entity Framework producirá la excepción `RetryLimitExceededException`.

    El valor que escriba en el cuadro de búsqueda estará en `command.Parameters[0]` y `command.Parameters[1]` (se usa uno para el primer nombre y otro para el apellido). Cuando se encuentra el valor "% Throw%", "Throw" se reemplaza en esos parámetros por "a" de modo que se encuentren y se devuelvan algunos alumnos.

    Se trata simplemente de una manera cómoda de probar la resistencia de la conexión basada en el cambio de alguna entrada a la interfaz de usuario de la aplicación. También puede escribir código que genere errores transitorios para todas las consultas o actualizaciones, como se explica más adelante en los comentarios sobre el método *DbInterception. Add* .
3. En *global. asax*, agregue las siguientes instrucciones de `using`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Agregue las líneas resaltadas al método `Application_Start`:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Estas líneas de código son lo que hace que se ejecute el código del interceptor cuando Entity Framework envía consultas a la base de datos. Tenga en cuenta que, dado que creó clases de interceptor independientes para la simulación y el registro de errores transitorios, puede habilitarlas y deshabilitarlas de forma independiente.

    Puede Agregar interceptores mediante el método `DbInterception.Add` en cualquier parte del código. no tiene que estar en el método `Application_Start`. Otra opción consiste en colocar este código en la clase DbConfiguration que creó anteriormente para configurar la Directiva de ejecución.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Siempre que coloque este código, tenga cuidado de no ejecutar `DbInterception.Add` para el mismo interceptor más de una vez, o bien obtendrá instancias adicionales del interceptor. Por ejemplo, si agrega dos veces el interceptor de registro, verá dos registros para cada consulta SQL.

    Los interceptores se ejecutan en el orden de registro (el orden en el que se llama al método `DbInterception.Add`). El orden puede ser importante en función de lo que esté haciendo en el interceptor. Por ejemplo, un interceptor podría cambiar el comando SQL que obtiene en la propiedad `CommandText`. Si cambia el comando SQL, el siguiente interceptor obtendrá el comando SQL cambiado, no el comando SQL original.

    Ha escrito el código de simulación de errores transitorios de forma que le permite producir errores transitorios escribiendo un valor diferente en la interfaz de usuario. Como alternativa, puede escribir el código del interceptor para generar siempre la secuencia de excepciones transitorias sin comprobar un valor de parámetro determinado. Después, puede Agregar el interceptor solo si desea generar errores transitorios. Sin embargo, si lo hace, no agregue el interceptor hasta que haya finalizado la inicialización de la base de datos. En otras palabras, realice al menos una operación de base de datos como, por ejemplo, una consulta en uno de los conjuntos de entidades antes de empezar a generar errores transitorios. El Entity Framework ejecuta varias consultas durante la inicialización de la base de datos y no se ejecutan en una transacción, por lo que los errores durante la inicialización pueden hacer que el contexto se encuentre en un estado incoherente.

## <a name="test-the-new-configuration"></a>Prueba de la nueva configuración

1. Presione **F5** para ejecutar la aplicación en modo de depuración y, a continuación, haga clic en la pestaña **estudiantes** .
2. Fíjese en la ventana **resultados** de Visual Studio para ver el resultado del seguimiento. Es posible que tenga que desplazarse más allá de algunos errores de JavaScript para llegar a los registros escritos por el registrador.

    Observe que puede ver las consultas SQL reales enviadas a la base de datos. Verá algunas consultas iniciales y comandos que Entity Framework se inician, comprobando la versión de la base de datos y la tabla de historial de migración (obtendrá información sobre las migraciones en el siguiente tutorial). Y verá una consulta para la paginación, para averiguar cuántos estudiantes hay y, por último, verá la consulta que obtiene los datos de los estudiantes.

    ![Registro para consulta normal](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. En la página **estudiantes** , escriba "Throw" como cadena de búsqueda y haga clic en **Buscar**.

    ![Iniciar cadena de búsqueda](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Observará que el explorador parece bloquear varios segundos mientras Entity Framework está reintentando la consulta varias veces. El primer reintento se produce con mucha rapidez y, después, la espera antes de que se incremente antes de cada reintento adicional. Este proceso de espera más tiempo antes de cada reintento se denomina *retroceso exponencial*.

    Cuando se muestre la página, donde se muestren los alumnos que tienen "a" en sus nombres, examine la ventana de salida y verá que se intentó la misma consulta cinco veces, las cuatro primeras veces que devolvían las excepciones transitorias. Para cada error transitorio, verá el registro que escribe al generar el error transitorio en la clase `SchoolInterceptorTransientErrors` ("devolviendo un error transitorio para el comando...") y verá que el registro se escribe cuando `SchoolInterceptorLogging` obtiene la excepción.

    ![Salida del registro que muestra reintentos](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Como escribió una cadena de búsqueda, la consulta que devuelve los datos de estudiante tiene parámetros:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    No está registrando el valor de los parámetros, pero podría hacerlo. Si desea ver los valores de los parámetros, puede escribir código de registro para obtener los valores de los parámetros de la propiedad `Parameters` del objeto `DbCommand` que obtiene en los métodos del interceptor.

    Tenga en cuenta que no puede repetir esta prueba a menos que detenga la aplicación y la reinicie. Si desea poder probar la resistencia de la conexión varias veces en una sola ejecución de la aplicación, puede escribir código para restablecer el contador de errores en `SchoolInterceptorTransientErrors`.
4. Para ver la diferencia que la estrategia de ejecución (Directiva de reintentos) realiza, comente la línea `SetExecutionStrategy` de *SchoolConfiguration.CS*, vuelva a ejecutar la página Students en modo de depuración y busque "Throw" de nuevo.

    Esta vez, el depurador se detiene en la primera excepción generada inmediatamente cuando intenta ejecutar la consulta la primera vez.

    ![Excepción ficticia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Quite la marca de comentario de la línea *SetExecutionStrategy* en *SchoolConfiguration.CS*.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en [el acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Resistencia de conexión habilitada
> * Interceptación de comandos habilitada
> * Prueba de la nueva configuración

Avance al siguiente artículo para obtener información sobre las migraciones de Code First y la implementación de Azure.
> [!div class="nextstepaction"]
> [Migraciones de Code First e implementación de Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
