---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Almacenamiento en caché | Microsoft Docs
author: microsoft
description: Es importante comprender el almacenamiento en caché para una aplicación de ASP.NET con un buen rendimiento. ASP.NET 1. x ofrecía tres opciones diferentes para el almacenamiento en caché; ,... de almacenamiento en caché de resultados
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464959"
---
# <a name="caching"></a>Almacenamiento en memoria caché

por [Microsoft](https://github.com/microsoft)

> Es importante comprender el almacenamiento en caché para una aplicación de ASP.NET con un buen rendimiento. ASP.NET 1. x ofrecía tres opciones diferentes para el almacenamiento en caché; almacenamiento en caché de resultados, almacenamiento en caché de fragmentos y API de caché.

Es importante comprender el almacenamiento en caché para una aplicación de ASP.NET con un buen rendimiento. ASP.NET 1. x ofrecía tres opciones diferentes para el almacenamiento en caché; almacenamiento en caché de resultados, almacenamiento en caché de fragmentos y API de caché. ASP.NET 2,0 ofrece los tres métodos, pero agrega algunas características adicionales importantes. Hay varias dependencias de caché nuevas y los desarrolladores tienen también la opción de crear dependencias de caché personalizadas. La configuración del almacenamiento en caché también se ha mejorado significativamente en ASP.NET 2,0.

## <a name="new-features"></a>Características nuevas

## <a name="cache-profiles"></a>Perfiles de caché

Los perfiles de caché permiten a los desarrolladores definir una configuración de caché específica que se puede aplicar a las páginas individuales. Por ejemplo, si tiene algunas páginas que deben expirar de la memoria caché después de 12 horas, puede crear fácilmente un perfil de memoria caché que se pueda aplicar a esas páginas. Para agregar un nuevo perfil de caché, use la sección &lt;outputCacheSettings&gt; en el archivo de configuración. Por ejemplo, a continuación se muestra la configuración de un perfil de caché denominado *twoday* que configura una duración de la caché de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar este perfil de caché a una página determinada, use el atributo CacheProfile de la directiva @ OutputCache, tal como se muestra a continuación:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependencias de caché personalizadas

ASP.NET 1. x desarrolladores Cried para las dependencias de caché personalizadas. En ASP.NET 1. x, la clase CacheDependency estaba sellada y evitaba que los desarrolladores derivaran sus propias clases. En ASP.NET 2,0, se elimina esa limitación y los desarrolladores pueden desarrollar sus propias dependencias de caché personalizadas. La clase CacheDependency permite la creación de una dependencia de caché personalizada basada en archivos, directorios o claves de caché.

Por ejemplo, el código siguiente crea una nueva dependencia de caché personalizada basada en un archivo denominado material. XML que se encuentra en la raíz de la aplicación web:

[!code-csharp[Main](caching/samples/sample3.cs)]

En este escenario, cuando el archivo cosas. XML cambia, se invalida el elemento almacenado en caché.

También es posible crear una dependencia de caché personalizada mediante claves de caché. Con este método, la eliminación de la clave de caché invalidará los datos almacenados en caché. Esto se ilustra en el siguiente ejemplo:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar el elemento que se insertó anteriormente, simplemente quite el elemento que se insertó en la memoria caché para que actúe como la clave de caché.

[!code-csharp[Main](caching/samples/sample5.cs)]

Tenga en cuenta que la clave del elemento que actúa como clave de caché debe ser la misma que el valor agregado a la matriz de claves de caché.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dependencias de caché de SQL basadas en sondeo (también denominadas dependencias basadas en tablas)

SQL Server 7 y 2000 usan el modelo basado en sondeo para las dependencias de caché de SQL. El modelo basado en sondeo utiliza un desencadenador en una tabla de base de datos que se desencadena cuando cambian los datos de la tabla. Dicho desencadenador actualiza un campo **changeId** en la tabla de notificación que ASP.net comprueba periódicamente. Si el campo **changeId** se ha actualizado, ASP.net sabe que los datos han cambiado y invalida los datos almacenados en caché.

> [!NOTE]
> SQL Server 2005 también puede usar el modelo basado en sondeo, pero dado que el modelo basado en sondeo no es el modelo más eficaz, es aconsejable usar un modelo basado en consultas (que se describe más adelante) con SQL Server 2005.

Para que una dependencia de caché de SQL que usa el modelo basado en sondeo funcione correctamente, las tablas deben tener habilitadas las notificaciones. Esto puede realizarse mediante programación con la clase SqlCacheDependencyAdmin o mediante la utilidad ASPNET\_regsql. exe.

La siguiente línea de comandos registra la tabla Products de la base de datos Northwind ubicada en una instancia de SQL Server denominada *dBase* para la dependencia de caché de SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

A continuación se muestra una explicación de los modificadores de la línea de comandos que se usan en el comando anterior:

| **Modificador de línea de comandos** | **Propósito** |
| --- | --- |
| -S *server* | Especifica el nombre del servidor. |
| -Ed | Especifica que la base de datos debe estar habilitada para la dependencia de caché de SQL. |
| -d *\_nombre* de la base de datos | Especifica el nombre de la base de datos que debe habilitarse para la dependencia de caché de SQL. |
| -E | Especifica que ASPNET\_regsql debe usar la autenticación de Windows al conectarse a la base de datos. |
| -et | Especifica que se va a habilitar una tabla de base de datos para la dependencia de caché de SQL. |
| -t *\_nombre* de la tabla | Especifica el nombre de la tabla de base de datos que se va a habilitar para la dependencia de caché de SQL. |

> [!NOTE]
> Hay otros modificadores disponibles para ASPNET\_regsql. exe. Para obtener una lista completa, ejecute ASPNET\_regsql. exe-? desde una línea de comandos.

Cuando este comando ejecuta los siguientes cambios se realizan en la base de datos SQL Server:

- Se agrega una tabla **SqlCacheTablesForChangeNotification de AspNet\_** . Esta tabla contiene una fila por cada tabla en la base de datos para la que se ha habilitado una dependencia de caché de SQL.
- Los procedimientos almacenados siguientes se crean en la base de datos:

| AspNet\_SqlCachePollingStoredProcedure | Consulta la tabla SqlCacheTablesForChangeNotification de AspNet\_y devuelve todas las tablas que están habilitadas para la dependencia de caché de SQL y el valor de changeId para cada tabla. Este procedimiento almacenado se usa para sondear y determinar si los datos han cambiado. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Devuelve todas las tablas habilitadas para la dependencia de caché de SQL consultando la tabla SqlCacheTablesForChangeNotification de AspNet\_y devuelve todas las tablas habilitadas para la dependencia de caché de SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabla para la dependencia de caché de SQL agregando la entrada necesaria en la tabla de notificación y agrega el desencadenador. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Anula el registro de una tabla para la dependencia de caché de SQL quitando la entrada de la tabla de notificaciones y quita el desencadenador. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Actualiza la tabla de notificaciones incrementando el changeId de la tabla modificada. ASP.NET usa este valor para determinar si los datos han cambiado. Tal y como se indica a continuación, el desencadenador que se crea cuando se habilita la tabla ejecuta este procedimiento almacenado. |

- Se crea un desencadenador de SQL Server denominado  **_TABLE\_Name_\_el desencadenador AspNet\_SqlCacheNotification\_** para la tabla. Este desencadenador ejecuta el AspNet\_SqlCacheUpdateChangeIdStoredProcedure cuando se realiza una inserción, una actualización o una eliminación en la tabla.
- Se agrega a la base de datos un rol de SQL Server denominado **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** .

El rol **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server tiene permisos exec en ASPNET\_SqlCachePollingStoredProcedure. Para que el modelo de sondeo funcione correctamente, debe agregar la cuenta de proceso al rol ASPNET\_ChangeNotification\_ReceiveNotificationsOnlyAccess. La herramienta ASPNET\_regsql. exe no lo hará por usted.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configuración de las dependencias de caché de SQL basadas en sondeo

Hay varios pasos que son necesarios para configurar las dependencias de caché de SQL basadas en sondeo. El primer paso es habilitar la base de datos y la tabla tal y como se indicó anteriormente. Una vez completado el paso, el resto de la configuración es el siguiente:

- Configurar el archivo de configuración ASP.NET.
- Configurar SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configuración del archivo de configuración ASP.NET

Además de agregar una cadena de conexión como se describe en un módulo anterior, también debe configurar un elemento de&gt; de caché &lt;con un elemento &lt;sqlCacheDependency&gt; como se muestra a continuación:

[!code-xml[Main](caching/samples/sample7.xml)]

Esta configuración habilita una dependencia de caché de SQL en la base de datos *pubs* . Tenga en cuenta que el atributo pollTime del elemento &lt;sqlCacheDependency&gt; tiene como valor predeterminado 60000 milisegundos o 1 minuto. (Este valor no puede ser inferior a 500 milisegundos). En este ejemplo, el &lt;agregar&gt; elemento agrega una nueva base de datos e invalida pollTime, estableciéndolo en 9 millones milisegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configurar SqlCacheDependency

El siguiente paso consiste en configurar SqlCacheDependency. La forma más fácil de hacerlo es especificar el valor para el atributo SqlDependency en la Directiva @ outcache de la siguiente manera:

[!code-aspx[Main](caching/samples/sample8.aspx)]

En la directiva @ OutputCache anterior, se configura una dependencia de caché de SQL para la tabla *authors* en la base de datos *pubs* . Se pueden configurar varias dependencias separándolas con un punto y coma como el siguiente:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Otro método para configurar SqlCacheDependency es hacerlo mediante programación. El código siguiente crea una nueva dependencia de caché de SQL en la tabla *authors* de la base de datos *pubs* .

[!code-csharp[Main](caching/samples/sample10.cs)]

Una de las ventajas de definir mediante programación la dependencia de caché de SQL es que puede controlar cualquier excepción que se pueda producir. Por ejemplo, si intenta definir una dependencia de caché de SQL para una base de datos que no se ha habilitado para la notificación, se producirá una excepción **DatabaseNotEnabledForNotificationException** . En ese caso, puede intentar habilitar la base de datos para las notificaciones llamando al método **SqlCacheDependencyAdmin. EnableNotifications** y pasándole el nombre de la base de datos.

Del mismo modo, si intenta definir una dependencia de caché de SQL para una tabla que no se ha habilitado para la notificación, se producirá una **TableNotEnabledForNotificationException** . Después, puede llamar al método **SqlCacheDependencyAdmin. EnableTableForNotifications** pasándole el nombre de la base de datos y el nombre de la tabla.

En el ejemplo de código siguiente se muestra cómo configurar correctamente el control de excepciones al configurar una dependencia de caché de SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Más información: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependencias de caché de SQL basadas en consultas (solo SQL Server 2005)

Cuando se usa SQL Server 2005 para la dependencia de caché de SQL, el modelo basado en sondeo no es necesario. Cuando se usa con SQL Server 2005, las dependencias de caché de SQL se comunican directamente a través de las conexiones SQL a la instancia de SQL Server (no es necesaria ninguna otra configuración) mediante notificaciones de consulta de SQL Server 2005.

La manera más sencilla de habilitar la notificación basada en consultas es hacerlo mediante declaración, estableciendo el atributo **SqlCacheDependency** del objeto de origen de datos en **CommandNotification** y estableciendo el atributo **EnableCaching** en **true**. Con este método, no se requiere ningún código. Si el resultado de un comando ejecutado en el origen de datos cambia, invalidará los datos de la memoria caché.

En el ejemplo siguiente se configura un control de origen de datos para la dependencia de caché de SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

En este caso, si la consulta especificada en **SelectCommand** devuelve un resultado diferente del original, se invalidan los resultados almacenados en caché.

También puede especificar que todos los orígenes de datos estén habilitados para las dependencias de caché de SQL estableciendo el atributo **SqlDependency** de la directiva **@ OutputCache** en **CommandNotification**. En el ejemplo siguiente se muestra esto.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obtener más información sobre las notificaciones de consulta en SQL Server 2005, vea el Libros en pantalla de SQL Server.

Otro método de configuración de una dependencia de caché de SQL basada en consultas es hacerlo mediante programación con la clase SqlCacheDependency. En el ejemplo de código siguiente se muestra cómo se logra esto.

[!code-csharp[Main](caching/samples/sample14.cs)]

Más información: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sustitución posterior a la caché

El almacenamiento en caché de una página puede aumentar drásticamente el rendimiento de una aplicación Web. Sin embargo, en algunos casos necesitará que la mayoría de las páginas estén almacenadas en caché y algunos fragmentos de la página sean dinámicos. Por ejemplo, si crea una página de noticias que es totalmente estática durante períodos de tiempo establecidos, puede establecer que la página completa se almacene en caché. Si desea incluir un banner de anuncio de rotación que cambió en cada solicitud de página, la parte de la página que contiene el anuncio debe ser dinámica. Para permitirle almacenar en caché una página pero sustituir el contenido de forma dinámica, puede usar la sustitución posterior a la caché de ASP.NET. Con la sustitución posterior a la caché, la página completa se almacena en caché con partes específicas marcadas como exentas del almacenamiento en caché. En el ejemplo de banners de anuncios, el control AdRotator permite aprovechar la sustitución posterior a la caché para que los anuncios se creen dinámicamente para cada usuario y para cada actualización de página.

Hay tres maneras de implementar la sustitución posterior a la caché:

- Mediante declaración, utilizando el control de sustitución.
- Mediante programación, con la API de control de sustitución.
- Implícitamente, mediante el control AdRotator.

### <a name="substitution-control"></a>Control de sustitución

El control de sustitución ASP.NET especifica una sección de una página almacenada en caché que se crea dinámicamente en lugar de almacenarse en caché. Coloque un control Substitution en la ubicación de la página en la que desea que aparezca el contenido dinámico. En tiempo de ejecución, el control de sustitución llama a un método que se especifica con la propiedad MethodName. El método debe devolver una cadena, que reemplaza el contenido del control de sustitución. El método debe ser un método estático en la página contenedora o en el control UserControl. El uso del control de sustitución hace que la caché del lado cliente se cambie a la caché del servidor, de modo que la página no se almacene en caché en el cliente. Esto garantiza que las solicitudes futuras a la página llamen de nuevo al método para generar contenido dinámico.

### <a name="substitution-api"></a>API de sustitución

Para crear contenido dinámico para una página en caché mediante programación, puede llamar al método [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) en el código de la página, pasándole el nombre de un método como parámetro. El método que controla la creación del contenido dinámico toma un único parámetro [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) y devuelve una cadena. La cadena devuelta es el contenido que se sustituirá en la ubicación especificada. Una ventaja de llamar al método WriteSubstitution en lugar de usar el control de sustitución mediante declaración es que se puede llamar a un método de cualquier objeto arbitrario en lugar de llamar a un método estático de la página o al objeto UserControl.

La llamada al método WriteSubstitution hace que la caché del lado cliente se cambie a la caché del servidor, de modo que la página no se almacene en caché en el cliente. Esto garantiza que las solicitudes futuras a la página llamen de nuevo al método para generar contenido dinámico.

### <a name="adrotator-control"></a>AdRotator (control)

El control de servidor AdRotator implementa la compatibilidad para la sustitución posterior a la caché. Si coloca un control AdRotator en la página, se representarán anuncios únicos en cada solicitud, independientemente de si la página primaria está almacenada en la memoria caché. Como resultado, una página que incluye un control AdRotator solo se almacena en caché en el lado del servidor.

## <a name="controlcachepolicy-class"></a>Clase ControlCachePolicy

La clase ControlCachePolicy permite el control mediante programación del almacenamiento en caché de fragmentos mediante controles de usuario. ASP.NET inserta los controles de usuario en una instancia de [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) . La clase BasePartialCachingControl representa un control de usuario que tiene habilitado el almacenamiento en caché de resultados.

Al acceder a la propiedad [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) de un control [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) , siempre recibirá un objeto ControlCachePolicy válido. Sin embargo, si tiene acceso a la propiedad [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) de un control [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) , recibirá un objeto ControlCachePolicy válido solo si el control de usuario ya está incluido en un control BasePartialCachingControl. Si no se ajusta, el objeto ControlCachePolicy devuelto por la propiedad producirá excepciones al intentar manipularlo porque no tiene un BasePartialCachingControl asociado. Para determinar si una instancia de UserControl admite el almacenamiento en caché sin generar excepciones, inspeccione la propiedad [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) .

El uso de la clase ControlCachePolicy es una de las diversas formas en que puede habilitar el almacenamiento en caché de resultados. En la lista siguiente se describen los métodos que puede usar para habilitar el almacenamiento en caché de resultados:

- Use la directiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) para habilitar el almacenamiento en caché de resultados en escenarios declarativos.
- Use el atributo [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) para habilitar el almacenamiento en caché de un control de usuario en un archivo de código subyacente.
- Use la clase ControlCachePolicy para especificar la configuración de la memoria caché en escenarios de programación en los que está trabajando con instancias de BasePartialCachingControl que se han habilitado para caché mediante uno de los métodos anteriores y cargados dinámicamente mediante el método [System. Web. UI. TemplateControl. LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) .

Una instancia de ControlCachePolicy se puede manipular correctamente solo entre las fases init y PreRender del ciclo de vida del control. Si modifica un objeto ControlCachePolicy después de la fase de representación previa, ASP.NET produce una excepción porque cualquier cambio realizado después de que se represente el control no puede afectar realmente a la configuración de la memoria caché (un control se almacena en caché durante la fase de representación). Por último, una instancia de control de usuario (y por consiguiente su objeto ControlCachePolicy) solo está disponible para la manipulación mediante programación cuando se representa realmente.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Cambios en la configuración de almacenamiento en caché: el elemento de&gt; de almacenamiento en caché &lt;

Hay varios cambios en la configuración de almacenamiento en caché en ASP.NET 2,0. El elemento &lt;Caching&gt; es nuevo en ASP.NET 2,0 y permite realizar cambios en la configuración de almacenamiento en caché en el archivo de configuración. Están disponibles los siguientes atributos.

| **Element** | **Descripción** |
| --- | --- |
| **almacenar** | Elemento opcional. Define la configuración de la memoria caché global de la aplicación. |
| **outputCache** | Elemento opcional. Especifica la configuración de la caché de resultados de toda la aplicación. |
| **outputCacheSettings** | Elemento opcional. Especifica la configuración de la caché de resultados que se puede aplicar a las páginas de la aplicación. |
| **sqlCacheDependency** | Elemento opcional. Configura las dependencias de caché de SQL para una aplicación ASP.NET. |

### <a name="the-ltcachegt-element"></a>El elemento&gt; de la memoria caché &lt;

Los siguientes atributos están disponibles en el elemento de&gt; de caché de &lt;:

| **Atributo** | **Descripción** |
| --- | --- |
| **disableMemoryCollection** | Atributo **Boolean** opcional. Obtiene o establece un valor que indica si la colección de memoria de caché que se produce cuando el equipo está bajo presión de memoria está deshabilitada. |
| **disableExpiration** | Atributo **Boolean** opcional. Obtiene o establece un valor que indica si la expiración de la memoria caché está deshabilitada. Cuando está deshabilitado, los elementos en caché no expiran y no se produce la eliminación en segundo plano de los elementos de caché expirados. |
| **privateBytesLimit** | Atributo **Int64** opcional. Obtiene o establece un valor que indica el tamaño máximo de los bytes privados de una aplicación antes de que la memoria caché comience a vaciar los elementos expirados e intentar reclamar la memoria. Este límite incluye la memoria usada por la memoria caché, así como la sobrecarga de memoria normal de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo se debe iniciar la recuperación de la memoria. |
| **percentagePhysicalMemoryUsedLimit** | Atributo **Int32** opcional. Obtiene o establece un valor que indica el porcentaje máximo de memoria física de una máquina que puede ser consumida por una aplicación antes de que la memoria caché comience a vaciar los elementos expirados e intentando recuperar memoria. este uso de memoria incluye también la memoria usada por la memoria caché. como el uso de memoria normal de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo se debe iniciar la recuperación de la memoria. |
| **privateBytesPollTime** | Atributo **TimeSpan** opcional. Obtiene o establece un valor que indica el intervalo de tiempo entre el sondeo del uso de memoria de bytes privados de la aplicación. |

### <a name="the-ltoutputcachegt-element"></a>El elemento &lt;outputCache&gt;

Los siguientes atributos están disponibles para el elemento &lt;outputCache&gt;.

|       <strong>Atributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descripción</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Atributo <strong>Boolean</strong> opcional. Habilita o deshabilita la caché de resultados de la página. Si se deshabilita, no se almacena en caché ninguna página independientemente de la configuración de programación o declarativa. El valor predeterminado es <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Atributo <strong>Boolean</strong> opcional. Habilita o deshabilita la caché de fragmentos de aplicación. Si se deshabilita, no se almacena en caché ninguna página independientemente de la directiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) o el perfil de almacenamiento en caché usados. Incluye un encabezado Cache-Control que indica que los servidores proxy que preceden en la cadena, así como los clientes del explorador no deben intentar almacenar en caché los resultados de la página. El valor predeterminado es <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Atributo <strong>Boolean</strong> opcional. Obtiene o establece un valor que indica si el módulo de caché de resultados envía el encabezado <strong>Cache-Control: Private</strong> de forma predeterminada. El valor predeterminado es <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Atributo <strong>Boolean</strong> opcional. Habilita o deshabilita el envío de un encabezado HTTP "<strong>Vary: \</strong ><em>" en la respuesta. Con el valor predeterminado de false,</em>se envía un encabezado "* Vary: \*<strong>" para las páginas almacenadas en caché de la salida. Cuando se envía el encabezado Vary, se pueden almacenar en caché diferentes versiones en función de lo que se especifique en el encabezado Vary. Por ejemplo, <em>varíe: los agentes de usuario</em> almacenarán versiones diferentes de una página en función del agente de usuario que emite la solicitud. El valor predeterminado es * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>El elemento &lt;outputCacheSettings&gt;

El elemento &lt;outputCacheSettings&gt; permite la creación de perfiles de caché tal y como se ha descrito anteriormente. El único elemento secundario del elemento &lt;outputCacheSettings&gt; es el elemento &lt;objetos OutputCacheProfile&gt; para configurar los perfiles de caché.

### <a name="the-ltsqlcachedependencygt-element"></a>El elemento&gt; de &lt;sqlCacheDependency

Los siguientes atributos están disponibles para el elemento &lt;sqlCacheDependency&gt;.

| **Atributo** | **Descripción** |
| --- | --- |
| **enabled** | Atributo **booleano** obligatorio. Indica si se sondea o no los cambios. |
| **pollTime** | Atributo **Int32** opcional. Establece la frecuencia con la que SqlCacheDependency sondea la tabla de base de datos en busca de cambios. Este valor corresponde al número de milisegundos entre sondeos sucesivos. No se puede establecer en menos de 500 milisegundos. El valor predeterminado es 1 minuto. |

### <a name="more-information"></a>Más información

Hay información adicional que debe tener en cuenta con respecto a la configuración de la memoria caché.

- Si no se establece el límite de bytes privados del proceso de trabajo, la memoria caché utilizará uno de los siguientes límites: 

    - x86 2 GB: 800MB o 60% de la RAM física, lo que sea menor
    - 3GB x86:1800MB o 60% de la RAM física, lo que sea menor
    - x64:1 terabyte o 60% de la RAM física, lo que sea menor
- Si se establecen el límite de bytes privados del proceso de trabajo y &lt;privateBytesLimit/&gt; de la memoria caché, la memoria caché usará el mínimo de los dos.
- Al igual que en 1. x, se quitan las entradas de caché y se llama a GC. Recopile por dos motivos: 

    - Estamos muy cerca del límite de bytes privados
    - La memoria disponible es cercana o inferior al 10%
- Puede deshabilitar eficazmente el recorte y la memoria caché para las condiciones de memoria insuficiente si establece &lt;percentagePhysicalMemoryUseLimit de caché/&gt; en 100.
- A diferencia de 1. x, 2,0 suspenderá el recorte y recopilará llamadas si el último GC. Collect no reduciría los bytes privados o el tamaño de los montones administrados en más del 1% del límite de memoria (caché).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: dependencias de caché personalizadas

1. Crear un sitio web nuevo.
2. Agregue un nuevo archivo XML denominado cache. XML y guárdelo en la raíz de la aplicación Web.
3. Agregue el código siguiente al método de carga de la página\_en el código subyacente de default. aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Agregue lo siguiente a la parte superior de default. aspx en la vista de código fuente: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Examine default. aspx. ¿Qué significa la hora?
6. Actualice el explorador. ¿Qué significa la hora?
7. Abra cache. XML y agregue el código siguiente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Guarde cache. Xml.
9. Actualice el explorador. ¿Qué significa la hora?
10. Explique por qué se ha actualizado el tiempo en lugar de mostrar los valores previamente almacenados en caché:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Práctica 2: usar dependencias de caché basadas en sondeos

En este laboratorio se usa el proyecto que creó en el módulo anterior que permite editar datos en la base de datos Northwind a través de un control GridView y DetailsView.

1. Abra el proyecto en Visual Studio 2005.
2. Ejecute la utilidad ASPNET\_regsql en la base de datos Northwind para habilitar la base de datos y la tabla Products. Use el comando siguiente desde un símbolo del sistema de Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Agregue lo siguiente al archivo Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Agregue un nuevo WebForm denominado showData. aspx.
5. Agregue la siguiente directiva @ OutputCache a la página showData. aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Agregue el código siguiente a la página\_carga de showData. aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Agregue un nuevo control SqlDataSource a showData. aspx y configúrelo para utilizar la conexión a la base de datos Northwind. Haga clic en Siguiente.
8. Active las casillas ProductName y ProductID y haga clic en siguiente.
9. Haga clic en Finish.
10. Agregue un nuevo GridView a la página showData. aspx.
11. Elija SqlDataSource1 en la lista desplegable.
12. Guarde y examine showData. aspx. Anote la hora que se muestra.
