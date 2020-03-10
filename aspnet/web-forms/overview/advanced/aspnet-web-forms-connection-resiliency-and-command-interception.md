---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET la resistencia de conexión de formularios Web Forms y la interceptación de comandos | Microsoft Docs
author: Erikre
description: En este tutorial se describe cómo modificar una aplicación de ejemplo para admitir la resistencia de conexión y la interceptación de comandos.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 95f0b5635c12d5ef88622e5766c1278c6570dd4d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484237"
---
# <a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resistencia de la conexión e intercepción de comandos de Formularios Web Forms de ASP.NET

por [Erik Reitan](https://github.com/Erikre)

En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para admitir la resistencia de conexión y la interceptación de comandos. Al habilitar la resistencia de conexión, la aplicación de ejemplo Wingtip Toys volverá a intentar automáticamente las llamadas de datos cuando se produzcan errores transitorios típicos de un entorno de nube. Además, al implementar la intercepción de comandos, la aplicación de ejemplo Wingtip Toys detectará todas las consultas SQL enviadas a la base de datos para registrarlas o cambiarlas.

> [!NOTE] 
> 
> Este tutorial de formularios Web Forms se basó en el tutorial de MVC siguiente de Tom Dykstra:  
> [Resistencia de la conexión e intercepción de comandos con el Entity Framework en una aplicación ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo proporcionar resistencia de la conexión.
- Cómo implementar la interceptación de comandos.

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene instalado el siguiente software en el equipo:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web). El .NET Framework se instala automáticamente.
- El proyecto de ejemplo Wingtip Toys, para que pueda implementar la funcionalidad mencionada en este tutorial en el proyecto de Wingtip Toys. El siguiente vínculo proporciona detalles de la descarga:

    - [Introducción con ASP.net 4.5.1 Web Forms: Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Antes de completar este tutorial, considere la posibilidad de revisar la serie de tutoriales relacionados, [Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La serie de tutoriales le ayudará a familiarizarse con el proyecto y el código de **WingtipToys** .

## <a name="connection-resiliency"></a>Resistencia de conexión

Cuando considere la posibilidad de implementar una aplicación en Windows Azure, una opción a tener en cuenta es la implementación de la base de datos en **windows** **Azure SQL Database**, un servicio de base de datos en la nube. Los errores de conexión transitorios suelen ser más frecuentes cuando se conecta a un servicio de base de datos en la nube que cuando el servidor Web y el servidor de base de datos se conectan directamente en el mismo centro de datos. Incluso si un servidor Web en la nube y un servicio de base de datos en la nube se hospedan en el mismo centro de datos, hay más conexiones de red entre ellos que pueden tener problemas, como equilibradores de carga.

Además, un servicio en la nube suele ser compartido por otros usuarios, lo que significa que su capacidad de respuesta puede verse afectada. Y el acceso a la base de datos puede estar sujeto a limitación. La limitación significa que el servicio de base de datos produce excepciones cuando se intenta tener acceso a ella con más frecuencia de la que se permite en el *acuerdo de nivel de servicio* (SLA).

Muchos o la mayoría de los problemas de conexión que se producen cuando se tiene acceso a un servicio en la nube son transitorios, es decir, se resuelven en un breve período de tiempo. Por lo tanto, cuando se intenta realizar una operación de base de datos y se obtiene un tipo de error que suele ser transitorio, puede volver a intentar la operación después de una breve espera y la operación puede ser correcta. Puede proporcionar una experiencia mucho mejor para los usuarios si controla los errores transitorios al intentarlo de nuevo automáticamente, lo que hace que la mayoría de ellos sean invisibles para el cliente. La característica de resistencia de conexión de Entity Framework 6 automatiza ese proceso de reintento de consultas SQL con errores.

La característica de resistencia de conexión debe estar configurada correctamente para un servicio de base de datos determinado:

1. Tiene que saber qué excepciones es probable que sean transitorias. Desea volver a intentar los errores causados por una pérdida temporal de conectividad de red, no por errores causados por errores en el programa, por ejemplo.
2. Tiene que esperar una cantidad de tiempo adecuada entre los reintentos de una operación con errores. Puede esperar más tiempo entre los reintentos de un proceso por lotes que en una página web en línea donde un usuario está esperando una respuesta.
3. Tiene que Reintentar un número adecuado de veces antes de que se conceda. Es posible que desee volver a intentar más veces en un proceso por lotes que en una aplicación en línea.

Puede configurar estas opciones manualmente para cualquier entorno de base de datos compatible con un proveedor de Entity Framework.

Todo lo que tiene que hacer para habilitar la resistencia de la conexión es crear una clase en el ensamblado que deriva de la clase `DbConfiguration` y, en esa clase, establecer la estrategia de ejecución SQL Database, que en Entity Framework es otro término para la Directiva de reintentos.

### <a name="implementing-connection-resiliency"></a>Implementar resistencia de conexión

1. Descargue y abra la aplicación de formularios Web Forms de ejemplo [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) en Visual Studio.
2. En la carpeta *Logic* de la aplicación **WingtipToys** , agregue un archivo de clase denominado *WingtipToysConfiguration.CS*.
3. Reemplace el código existente con el siguiente código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

El Entity Framework ejecuta automáticamente el código que encuentra en una clase que deriva de `DbConfiguration`. Puede usar la clase `DbConfiguration` para realizar tareas de configuración en código que, de lo contrario, haría en el archivo *Web. config* . Para obtener más información, vea [configuración basada en código EntityFramework](https://msdn.microsoft.com/data/jj680699).

1. En la carpeta *lógica* , abra el archivo *AddProducts.CS* .
2. Agregue una instrucción de `using` para `System.Data.Entity.Infrastructure` como se muestra resaltada en amarillo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Agregue un bloque `catch` al método `AddProduct` para que el `RetryLimitExceededException` se registre como resaltado en amarillo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Al agregar la `RetryLimitExceededException` excepción, puede proporcionar un mejor registro o mostrar un mensaje de error al usuario en el que puede optar por volver a intentar el proceso. Al detectar la `RetryLimitExceededException` excepción, los únicos errores que probablemente sean transitorios ya se habrán intentado y se producirá un error varias veces. La excepción real devuelta se ajustará en la `RetryLimitExceededException` excepción. Además, también ha agregado un bloque catch general. Para obtener más información acerca de la excepción `RetryLimitExceededException`, consulte [Entity Framework la lógica de reintentos y resistencia de conexión](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interceptación de comandos

Ahora que ha activado una directiva de reintentos, ¿cómo se prueba para comprobar que funciona según lo esperado? No es tan fácil forzar que se produzca un error transitorio, especialmente cuando se ejecuta localmente, y sería especialmente difícil integrar los errores transitorios reales en una prueba unitaria automatizada. Para probar la característica de resistencia de conexión, necesita una manera de interceptar las consultas que Entity Framework envía a SQL Server y reemplazar la respuesta de SQL Server por un tipo de excepción que suele ser transitorio.

También puede usar la intercepción de consultas para implementar un procedimiento recomendado para las aplicaciones en la nube: registrar la latencia y el éxito o el fracaso de todas las llamadas a servicios externos, como los servicios de base de datos.

En esta sección del tutorial, usará la [*característica de intercepción*](https://msdn.microsoft.com/data/dn469464) de Entity Framework para el registro y la simulación de errores transitorios.

### <a name="create-a-logging-interface-and-class"></a>Crear una interfaz de registro y una clase

Un procedimiento recomendado para el registro es hacerlo mediante el uso de una [`interface`](https://msdn.microsoft.com/library/ms173156.aspx) en lugar de llamadas de codificación rígida a `System.Diagnostics.Trace` o una clase de registro. De este modo, es más fácil cambiar el mecanismo de registro más adelante si alguna vez tiene que hacerlo. Por lo tanto, en esta sección, creará la interfaz de registro y una clase para implementarla.

Según el procedimiento anterior, ha descargado y abierto la aplicación de ejemplo **WingtipToys** en Visual Studio.

1. Cree una carpeta en el proyecto **WingtipToys** y asígnele el nombre *Logging*.
2. En la carpeta de *registro* , cree un archivo de clase denominado *ILogger.CS* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   La interfaz proporciona tres niveles de seguimiento para indicar la importancia relativa de los registros y otro diseñado para proporcionar información de latencia para llamadas de servicio externas, como consultas de base de datos. Los métodos de registro tienen sobrecargas que permiten pasar una excepción. Esto es para que la clase que implementa la interfaz registre la información de la excepción, incluido el seguimiento de la pila y las excepciones internas, en lugar de confiar en que se realiza en cada llamada al método de registro en la aplicación.  
  
   Los métodos `TraceApi` permiten realizar un seguimiento de la latencia de cada llamada a un servicio externo, como SQL Database.
3. En la carpeta de *registro* , cree un archivo de clase denominado *Logger.CS* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

La implementación de usa `System.Diagnostics` para realizar el seguimiento. Se trata de una característica integrada de .NET que facilita la generación y el uso de la información de seguimiento. Hay muchos agentes de escucha de &quot;&quot; puede usar con el seguimiento de `System.Diagnostics`, para escribir registros en archivos, por ejemplo, o para escribirlos en el almacenamiento de blobs en Windows Azure. Vea algunas de las opciones y vínculos a otros recursos para obtener más información, en [solucionar problemas de sitios web de Windows Azure en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). En este tutorial, solo se examinan los registros de la ventana de **salida** de Visual Studio.

En una aplicación de producción, es posible que desee considerar la posibilidad de usar marcos de seguimiento distintos de `System.Diagnostics`, y la interfaz de `ILogger` hace que sea relativamente fácil cambiar a otro mecanismo de seguimiento si decide hacerlo.

### <a name="create-interceptor-classes"></a>Crear clases de interceptor

A continuación, creará las clases a las que llamará el Entity Framework cada vez que vaya a enviar una consulta a la base de datos, una para simular errores transitorios y otra para el registro. Estas clases de interceptor deben derivar de la clase `DbCommandInterceptor`. En ellos, se escriben invalidaciones de método a las que se llama automáticamente cuando la consulta está a punto de ejecutarse. En estos métodos puede examinar o registrar la consulta que se va a enviar a la base de datos, y puede cambiar la consulta antes de que se envíe a la base de datos o de que se devuelva algo para Entity Framework sin pasar la consulta a la base de datos.

1. Para crear la clase Interceptor que registrará cada consulta SQL antes de enviarla a la base de datos, cree un archivo de clase denominado *InterceptorLogging.CS* en la carpeta *lógica* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   En el caso de las consultas o los comandos correctos, este código escribe un registro de información con información de latencia. En el caso de las excepciones, crea un registro de errores.
2. Para crear la clase Interceptor que generará errores transitorios ficticios al escribir &quot;Throw&quot; en el cuadro de texto **nombre** de la página denominada *AdminPage. aspx*, cree un archivo de clase denominado *InterceptorTransientErrors.CS* en la carpeta *lógica* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Este código solo invalida el método `ReaderExecuting`, al que se llama para las consultas que pueden devolver varias filas de datos. Si desea comprobar la resistencia de la conexión para otros tipos de consultas, también puede invalidar los métodos `NonQueryExecuting` y `ScalarExecuting`, como hace el interceptor de registro.  
  
   Más adelante, iniciará sesión como el "administrador" y seleccionará el vínculo de **Administrador** en la barra de navegación superior. A continuación, en la página *AdminPage. aspx* agregará un producto denominado &quot;Throw&quot;. El código crea una excepción ficticia SQL Database para el número de error 20, un tipo conocido para ser normalmente transitorio. Otros números de error actualmente reconocidos como transitorios son 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 y 40613, pero están sujetos a cambios en nuevas versiones de SQL Database. El nombre del producto cambiará a "TransientErrorExample", que puede seguir en el código del archivo *InterceptorTransientErrors.CS* .  
  
   El código devuelve la excepción que se va a Entity Framework en lugar de ejecutar la consulta y pasar los resultados. La excepción transitoria se devuelve *cuatro* veces y, a continuación, el código revierte al procedimiento normal de pasar la consulta a la base de datos.

    Dado que se registra todo, podrá ver que Entity Framework intenta ejecutar la consulta cuatro veces antes de que finalice, y la única diferencia en la aplicación es que tarda más en representar una página con los resultados de la consulta.  
  
   El número de veces que el Entity Framework volverá a intentarlo es configurable; el código especifica cuatro veces porque este es el valor predeterminado de la Directiva de ejecución SQL Database. Si cambia la Directiva de ejecución, también debe cambiar el código que especifica el número de veces que se generan errores transitorios. También puede cambiar el código para generar más excepciones, de modo que Entity Framework producirá la excepción `RetryLimitExceededException`.
3. En *global. asax*, agregue las siguientes instrucciones Using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. A continuación, agregue las líneas resaltadas al método `Application_Start`:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Estas líneas de código son lo que hace que se ejecute el código del interceptor cuando Entity Framework envía consultas a la base de datos. Tenga en cuenta que, dado que creó clases de interceptor independientes para la simulación y el registro de errores transitorios, puede habilitarlas y deshabilitarlas de forma independiente.   
  
 Puede Agregar interceptores mediante el método `DbInterception.Add` en cualquier parte del código. no tiene que estar en el método `Application_Start`. Otra opción, si no ha agregado los interceptores en el método `Application_Start`, sería actualizar o agregar la clase denominada *WingtipToysConfiguration.CS* y colocar el código anterior al final del constructor de la clase `WingtipToysConfiguration`.

Siempre que coloque este código, tenga cuidado de no ejecutar `DbInterception.Add` para el mismo interceptor más de una vez, o bien obtendrá instancias adicionales del interceptor. Por ejemplo, si agrega dos veces el interceptor de registro, verá dos registros para cada consulta SQL.

Los interceptores se ejecutan en el orden de registro (el orden en el que se llama al método `DbInterception.Add`). El orden puede ser importante en función de lo que esté haciendo en el interceptor. Por ejemplo, un interceptor podría cambiar el comando SQL que obtiene en la propiedad `CommandText`. Si cambia el comando SQL, el siguiente interceptor obtendrá el comando SQL cambiado, no el comando SQL original.

Ha escrito el código de simulación de errores transitorios de forma que le permite producir errores transitorios escribiendo un valor diferente en la interfaz de usuario. Como alternativa, puede escribir el código del interceptor para generar siempre la secuencia de excepciones transitorias sin comprobar un valor de parámetro determinado. Después, puede Agregar el interceptor solo si desea generar errores transitorios. Sin embargo, si lo hace, no agregue el interceptor hasta que haya finalizado la inicialización de la base de datos. En otras palabras, realice al menos una operación de base de datos como, por ejemplo, una consulta en uno de los conjuntos de entidades antes de empezar a generar errores transitorios. El Entity Framework ejecuta varias consultas durante la inicialización de la base de datos y no se ejecutan en una transacción, por lo que los errores durante la inicialización pueden hacer que el contexto se encuentre en un estado incoherente.

## <a name="test-logging-and-connection-resiliency"></a>Prueba de registro y resistencia de conexión

1. En Visual Studio, presione **F5** para ejecutar la aplicación en modo de depuración y, a continuación, inicie sesión como "admin" con "Pa $ $Word" como contraseña.
2. Seleccione **Administrador** en la barra de navegación de la parte superior.
3. Escriba un nuevo producto denominado "Throw" con la descripción, el precio y el archivo de imagen adecuados.
4. Presione el botón **Agregar producto** .  
   Observará que el explorador parece bloquear varios segundos mientras Entity Framework está reintentando la consulta varias veces. El primer reintento se produce muy rápidamente y, después, la espera aumenta antes de cada reintento adicional. Este proceso de espera más tiempo antes de cada reintento se denomina *retroceso exponencial* .
5. Espere hasta que la página ya no intente cargarse.
6. Detenga el proyecto y mire la ventana **resultados** de Visual Studio para ver los resultados de la traza. Para encontrar la ventana de **salida** , seleccione **depurar** -&gt; **Windows** -&gt; **salida**. Es posible que tenga que desplazarse más allá de otros registros escritos por el registrador.  
  
   Observe que puede ver las consultas SQL reales enviadas a la base de datos. Verá algunas consultas iniciales y comandos que Entity Framework se inician, comprobando la versión de la base de datos y la tabla de historial de migración.   
    ![Resultados (Ventana)](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Tenga en cuenta que no puede repetir esta prueba a menos que detenga la aplicación y la reinicie. Si desea poder probar la resistencia de la conexión varias veces en una sola ejecución de la aplicación, puede escribir código para restablecer el contador de errores en `InterceptorTransientErrors`.
7. Para ver la diferencia que la estrategia de ejecución (Directiva de reintentos) realiza, comente la línea `SetExecutionStrategy` del archivo *WingtipToysConfiguration.CS* en la carpeta *lógica* , vuelva a ejecutar la página de **Administración** en modo de depuración y agregue el producto denominado &quot;Throw&quot; de nuevo.  
  
   Esta vez, el depurador se detiene en la primera excepción generada inmediatamente cuando intenta ejecutar la consulta la primera vez.  
    ![Depuración: ver detalles](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Quite la marca de comentario de la línea de `SetExecutionStrategy` en el archivo *WingtipToysConfiguration.CS* .

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo modificar una aplicación de ejemplo de formularios Web Forms para admitir la resistencia de conexión y la interceptación de comandos.

## <a name="next-steps"></a>Pasos siguientes

Después de revisar la resistencia de la conexión y la intercepción de comandos en formularios Web Forms de ASP.NET, revise los métodos asincrónicos del tema de formularios Web Forms [de ASP.net en ASP.NET 4,5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). El tema le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET asincrónica con Visual Studio.
