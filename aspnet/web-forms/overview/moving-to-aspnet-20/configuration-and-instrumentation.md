---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuración e instrumentación | Microsoft Docs
author: microsoft
description: Hay cambios importantes en la configuración e instrumentación en ASP.NET 2,0. La nueva API de configuración de ASP.NET permite que los cambios de configuración se hagan PR...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78507883"
---
# <a name="configuration-and-instrumentation"></a>Configuración e instrumentación

por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración e instrumentación en ASP.NET 2,0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

Hay cambios importantes en la configuración e instrumentación en ASP.NET 2,0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

En este módulo, trataremos ASP.NET la API de configuración en lo que se refiere a la lectura y la escritura en los archivos de configuración de ASP.NET. también trataremos la instrumentación de ASP.NET. También trataremos las nuevas características disponibles en el seguimiento de ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuración de ASP.NET

La API de configuración de ASP.NET permite desarrollar, implementar y administrar datos de configuración de la aplicación mediante una interfaz de programación única. Puede usar la API de configuración para desarrollar y modificar las configuraciones de ASP.NET completas mediante programación sin editar directamente el XML en los archivos de configuración. Además, puede usar la API de configuración de en las aplicaciones de consola y los scripts que desarrolle, en las herramientas de administración basadas en Web y en los complementos de Microsoft Management Console (MMC).

Las dos herramientas de administración de configuración siguientes usan la API de configuración y se incluyen con la versión .NET Framework 2,0:

- El complemento MMC de ASP.NET, que usa la API de configuración de para simplificar las tareas administrativas, lo que proporciona una vista integrada de los datos de configuración local de todos los niveles de la jerarquía de configuración.
- La herramienta de administración de sitios web, que permite administrar los valores de configuración de las aplicaciones locales y remotas, incluidos los sitios hospedados.

La API de configuración de ASP.NET consta de un conjunto de objetos de administración de ASP.NET que puede usar para configurar los sitios web y las aplicaciones mediante programación. Los objetos de administración se implementan como una biblioteca de clases de .NET Framework. El modelo de programación de API de configuración ayuda a garantizar la coherencia del código y la confiabilidad mediante la aplicación de tipos de datos en tiempo de compilación. Para facilitar la administración de las configuraciones de la aplicación, la API de configuración permite ver los datos que se combinan desde todos los puntos de la jerarquía de configuración como una sola colección, en lugar de ver los datos como colecciones independientes de diferentes archivos de configuración. Además, la API de configuración permite manipular todas las configuraciones de aplicación sin modificar directamente el XML en los archivos de configuración. Por último, la API simplifica las tareas de configuración mediante la compatibilidad con herramientas administrativas, como la herramienta de administración de sitios Web. La API de configuración simplifica la implementación, ya que admite la creación de archivos de configuración en un equipo y la ejecución de scripts de configuración en varios equipos.

> [!NOTE]
> La API de configuración de no admite la creación de aplicaciones de IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Trabajar con opciones de configuración local y remota

Un objeto de configuración representa la vista combinada de los valores de configuración que se aplican a una entidad física concreta, como un equipo, o a una entidad lógica, como una aplicación o un sitio Web. La entidad lógica especificada puede existir en el equipo local o en un servidor remoto. Cuando no existe ningún archivo de configuración para una entidad especificada, el objeto de configuración representa los valores de configuración predeterminados tal y como se define en el archivo Machine. config.

Puede obtener un objeto de configuración mediante uno de los métodos de configuración abiertos de las clases siguientes:

1. La clase ConfigurationManager, si la entidad es una aplicación cliente.
2. La clase WebConfigurationManager, si la entidad es una aplicación Web.

Estos métodos devolverán un objeto de configuración que, a su vez, proporciona los métodos y las propiedades necesarios para controlar los archivos de configuración subyacentes. Puede tener acceso a estos archivos para lectura o escritura.

### <a name="reading"></a>Lectura

Use el método GetSection o GetSectionGroup para leer la información de configuración. El usuario o proceso que lee debe tener permisos de lectura en todos los archivos de configuración de la jerarquía.

> [!NOTE]
> Si usa un método GetSection estático que toma un parámetro Path, el parámetro Path debe hacer referencia a la aplicación en la que se está ejecutando el código. De lo contrario, el parámetro se omite y se devuelve la información de configuración de la aplicación que se está ejecutando actualmente.

### <a name="writing"></a>Escritura

Puede usar uno de los métodos Save para escribir información de configuración. El usuario o proceso que escribe debe tener permisos de escritura en el archivo de configuración y el directorio en el nivel de la jerarquía de configuración actual, así como permisos de lectura en todos los archivos de configuración de la jerarquía.

Para generar un archivo de configuración que represente los valores de configuración heredados para una entidad especificada, use uno de los siguientes métodos de guardado de la configuración:

1. El método Save para crear un nuevo archivo de configuración.
2. El método SaveAs para generar un nuevo archivo de configuración en otra ubicación.

## <a name="configuration-classes-and-namespaces"></a>Clases de configuración y espacios de nombres

Muchas clases y métodos de configuración son similares entre sí. En la tabla siguiente se describen las clases de configuración y los espacios de nombres que se usan con más frecuencia.

| **Clase de configuración o espacio de nombres** | **Descripción** |
| --- | --- |
| [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) (espacio de nombres) | Contiene las clases de configuración principales para todas las aplicaciones .NET Framework. Las clases de controlador de sección se usan para obtener datos de configuración para una sección de métodos, como GetSection y GetSectionGroup. Estos dos métodos no son estáticos. |
| System. Configuration. Configuration (clase) | Representa un conjunto de datos de configuración para un equipo, una aplicación, un directorio web u otro recurso. Esta clase contiene métodos útiles, como GetSection y GetSectionGroup, para actualizar los valores de configuración y obtener referencias a secciones y grupos de secciones. Esta clase se usa como tipo de valor devuelto para los métodos que obtienen datos de configuración en tiempo de diseño, como los métodos de las clases WebConfigurationManager y ConfigurationManager. |
| Espacio de nombres System. Web. Configuration | Contiene las clases de controlador de sección para las secciones de configuración de ASP.NET definidas en los [valores de configuración de ASP.net](https://msdn.microsoft.com/library/b5ysx397.aspx). Las clases de controlador de sección se usan para obtener datos de configuración para una sección de métodos, como GetSection y GetSectionGroup. |
| System. Web. Configuration. WebConfigurationManager (clase) | Proporciona métodos útiles para obtener referencias a los valores de configuración en tiempo de ejecución y en tiempo de diseño. Estos métodos usan la clase System. Configuration. Configuration como un tipo de valor devuelto. Puede usar el método estático GetSection de esta clase o el método GetSection no estático de la clase System. Configuration. ConfigurationManager indistintamente. En el caso de las configuraciones de aplicaciones Web, se recomienda la clase System. Web. Configuration. WebConfigurationManager en lugar de la clase System. Configuration. ConfigurationManager. |
| Espacio de nombres [System. Configuration. Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Proporciona una manera de personalizar y extender el proveedor de configuración. Esta es la clase base para todas las clases de proveedor en el sistema de configuración. |
| Espacio de nombres [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contiene clases e interfaces para administrar y supervisar el estado de las aplicaciones Web. En realidad, este espacio de nombres no se considera parte de la API de configuración. Por ejemplo, las clases de este espacio de nombres llevan a cabo el seguimiento y el desencadenamiento de eventos. |
| Espacio de nombres [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Proporciona las clases necesarias para la instrumentación de aplicaciones con el fin de exponer su información de administración y sus eventos a través de Instrumental de administración de Windows (WMI) a posibles consumidores. La supervisión de estado de ASP.NET usa WMI para proporcionar eventos. En realidad, este espacio de nombres no se considera parte de la API de configuración. |

## <a name="reading-from-aspnet-configuration-files"></a>Leer archivos de configuración de ASP.NET

La clase WebConfigurationManager es la clase principal para leer los archivos de configuración de ASP.NET. En realidad, hay tres pasos para leer los archivos de configuración de ASP.NET:

1. Obtiene un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Lea la información deseada del archivo de configuración.

El objeto de configuración que representa no representa un archivo de configuración determinado. En su lugar, representa una vista combinada de la configuración de un equipo, una aplicación o un sitio Web. En el ejemplo de código siguiente se crea una instancia de un objeto de configuración que representa la configuración de una aplicación web denominada *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Tenga en cuenta que si la ruta de acceso de/ProductInfo no existe, el código anterior devolverá la configuración predeterminada tal y como se especifica en el archivo Machine. config.

Una vez que tenga el objeto de configuración, puede usar el método GetSection o GetSectionGroup para profundizar en los valores de configuración. En el ejemplo siguiente se obtiene una referencia a la configuración de suplantación para la aplicación ProductInfo anterior:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Escribir en archivos de configuración de ASP.NET

Como en la lectura de archivos de configuración, la clase WebConfigurationManager es el núcleo para escribir en los archivos de configuración de Asp.NET. También hay tres pasos para escribir en los archivos de configuración de ASP.NET.

1. Obtiene un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Escriba la información deseada del archivo de configuración con el método Save o SaveAs.

El código siguiente cambia el atributo **Debug** del elemento &lt;compilation&gt; a false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Cuando se ejecuta este código, el atributo **Debug** del elemento &lt;compilation&gt; se establecerá en false para el archivo Web. config de la aplicación *webApp* .

## <a name="systemwebmanagement-namespace"></a>Espacio de nombres System. Web. Management

El espacio de nombres System. Web. Management proporciona las clases e interfaces para administrar y supervisar el estado de las aplicaciones de ASP.NET.

El registro se logra definiendo una regla que asocia eventos con un proveedor. La regla define el tipo de eventos que se envían al proveedor. Los siguientes eventos base están disponibles para su registro:

| **WebBaseEvent** | La clase de eventos base para todos los eventos. Contiene las propiedades necesarias para todos los eventos, como el código de evento, el código de detalle del evento, la fecha y la hora en que se generó el evento, el número de secuencia, el mensaje del evento y los detalles del evento. |
| --- | --- |
| **WebManagementEvent** | La clase de eventos base para los eventos de administración, como la duración de la aplicación, la solicitud, el error y los eventos de auditoría. |
| **WebHeartbeatEvent** | Evento generado por la aplicación en intervalos regulares para capturar información útil sobre el estado de tiempo de ejecución. |
| **WebAuditEvent** | La clase base para los eventos de auditoría de seguridad, que se usan para marcar condiciones como error de autorización, error de descifrado, *etc.* |
| **WebRequestEvent** | La clase base para todos los eventos de solicitud informativos. |
| **WebBaseErrorEvent** | La clase base para todos los eventos que indican las condiciones de error. |

Los tipos de proveedores disponibles permiten enviar resultados de eventos a Visor de eventos, SQL Server, Instrumental de administración de Windows (WMI) y el correo electrónico. Los proveedores preconfigurados y las asignaciones de eventos reducen la cantidad de trabajo necesario para obtener la salida de los eventos registrados.

ASP.NET 2,0 usa el proveedor de registro de eventos de forma integrada para registrar los eventos en función de los dominios de aplicación que se inician y se detienen, así como el registro de las excepciones no controladas. Esto ayuda a cubrir algunos de los escenarios básicos. Por ejemplo, supongamos que la aplicación produce una excepción, pero el usuario no guarda el error y no se puede reproducir. Con la regla de registro de eventos predeterminada, podrá recopilar la información de la pila y la excepción para obtener una idea más clara del tipo de error que se ha producido. Otro ejemplo se aplica si la aplicación pierde el estado de la sesión. En ese caso, puede buscar en el registro de eventos para determinar si el dominio de aplicación se está reciclando y por qué se detuvo el dominio de aplicación en primer lugar.

Además, el sistema de supervisión de estado es extensible. Por ejemplo, puede definir eventos Web personalizados, desencadenarlos en la aplicación y, a continuación, definir una regla para enviar la información de evento a un proveedor como, por ejemplo, el correo electrónico. Esto le permite asociar fácilmente su instrumental a los proveedores de supervisión de estado. Otro ejemplo sería desencadenar un evento cada vez que se procesa un pedido y configurar una regla que envía cada evento a la base de datos de SQL Server. También puede activar un evento cuando un usuario no puede iniciar sesión varias veces en una fila y configurar el evento para usar los proveedores basados en correo electrónico.

La configuración de los proveedores y eventos predeterminados se almacena en el archivo Web. config global. El archivo Web. config global almacena toda la configuración basada en Web que se almacenó en el archivo Machine. config en ASP.NET 1x. El archivo Web. config global se encuentra en el siguiente directorio:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

La sección &lt;healthMonitoring&gt; del archivo Web. config global proporciona los valores de configuración predeterminados. Puede invalidar esta configuración o configurar sus propias opciones implementando la sección &lt;healthMonitoring&gt; en el archivo Web. config de la aplicación.

La sección &lt;healthMonitoring&gt; del archivo Web. config global contiene los siguientes elementos:

| **providers** | Contiene proveedores configurados para el Visor de eventos, WMI y SQL Server. |
| --- | --- |
| **eventMappings** | Contiene las asignaciones para las diversas clases webbase. Puede extender esta lista si genera su propia clase de eventos. La generación de su propia clase de eventos proporciona una granularidad más fina sobre los proveedores a los que envía información. Por ejemplo, puede configurar las excepciones no controladas que se van a enviar a SQL Server, y enviar sus propios eventos personalizados al correo electrónico. |
| **Reglamento** | Vincula el eventMappings al proveedor. |
| **almacenamiento en búfer** | Se usa con SQL Server y proveedores de correo electrónico para determinar con qué frecuencia se deben vaciar los eventos en el proveedor. |

A continuación se muestra un ejemplo de código del archivo Web. config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Cómo almacenar eventos en Visor de eventos

Como se mencionó anteriormente, el proveedor para registrar eventos en la Visor de eventos se configura automáticamente en el archivo Web. config global. De forma predeterminada, se registran todos los eventos basados en **WebBaseErrorEvent** y **WebFailureAuditEvent** . Puede agregar reglas adicionales para registrar información adicional en el registro de eventos. Por ejemplo, si desea registrar todos los eventos (*es decir*, todos los eventos basados en **WebBaseEvent**), puede Agregar la siguiente regla al archivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Esta regla vincularía el mapa de eventos **todos los eventos** con el proveedor de registro de eventos. Tanto eventMapping como el proveedor se incluyen en el archivo Web. config global.

## <a name="how-to-store-events-to-sql-server"></a>Cómo almacenar eventos en SQL Server

Este método usa la base de datos **ASPNETDB** , generada por la herramienta ASPNET\_regsql. exe. El proveedor predeterminado usa la cadena de conexión LocalSqlServer, que usa una base de datos basada en archivos en la carpeta app\_Data o la instancia local SQLExpress de SQL Server. Tanto la cadena de conexión LocalSqlServer como SqlProvider se configuran en el archivo Web. config global.

La cadena de conexión LocalSqlServer en el archivo Web. config global tiene el siguiente aspecto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si desea usar otra instancia de SQL Server, debe usar la herramienta ASPNET\_regsql. exe, que se encuentra en%windir%\Microsoft.Net\Framework\v2.0.\* carpeta. Use la herramienta ASPNET\_regsql. exe para generar una base de datos **ASPNETDB** personalizada en la instancia de SQL Server, agregue la cadena de conexión al archivo de configuración de las aplicaciones y, a continuación, agregue un proveedor mediante la nueva cadena de conexión. Una vez creada la base de datos **ASPNETDB** , deberá establecer una regla para vincular un EventMapping a sqlProvider.

Tanto si usa el SqlProvider predeterminado como si configura su propio proveedor, deberá agregar una regla que vincule el proveedor con un mapa de eventos. La siguiente regla vincula el nuevo proveedor que creó anteriormente al mapa de eventos **todos los eventos** . Esta regla registrará todos los eventos basados en **WebBaseEvent** y los enviará a la MySqlWebEventProvider que usará la cadena de conexión de MYASPNETDB. El código siguiente agrega una regla para vincular el proveedor a un mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si desea enviar solo los errores a SQL Server, puede Agregar la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Cómo reenviar eventos a WMI

También puede reenviar los eventos a WMI. De forma predeterminada, el proveedor WMI se configura en el archivo Web. config global.

En el ejemplo de código siguiente se agrega una regla para reenviar los eventos a WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Tendrá que agregar una regla para asociar un eventMapping al proveedor y también una aplicación de escucha de WMI para escuchar los eventos. En el ejemplo de código siguiente se agrega una regla para vincular el proveedor WMI al mapa de eventos **All Events** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Cómo reenviar eventos al correo electrónico

También puede reenviar eventos al correo electrónico. Tenga cuidado con las reglas de eventos que asigne a su proveedor de correo electrónico, ya que puede enviarse involuntariamente una gran cantidad de información que puede ser más adecuada para SQL Server o el registro de eventos. Hay dos proveedores de correo electrónico: SimpleMailWebEventProvider y TemplatedMailWebEventProvider. Cada una de ellas tiene los mismos atributos de configuración, con la excepción de los atributos "template" y "detailedTemplateErrors", que solo están disponibles en TemplatedMailWebEventProvider.

> [!NOTE]
> Ninguno de estos proveedores de correo electrónico está configurado. Tendrá que agregarlos al archivo Web. config.

La diferencia principal entre estos dos proveedores de correo electrónico es que SimpleMailWebEventProvider envía mensajes de correo electrónico en una plantilla genérica que no se puede modificar. El archivo Web. config de ejemplo agrega este proveedor de correo electrónico a la lista de proveedores configurados mediante la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

También se agrega la siguiente regla para asociar el proveedor de correo electrónico al mapa de eventos de **todos los eventos** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Seguimiento de ASP.NET 2,0

Hay tres mejoras principales en el seguimiento en ASP.NET 2,0.

1. Funcionalidad de seguimiento integrada
2. Acceso mediante programación a los mensajes de seguimiento
3. Seguimiento de nivel de aplicación mejorado

## <a name="integrated-tracing-functionality"></a>Funcionalidad de seguimiento integrada

Ahora puede enrutar los mensajes emitidos por la clase System. Diagnostics. Trace a la salida de seguimiento de ASP.NET y enrutar los mensajes emitidos por el seguimiento de ASP.NET a System. Diagnostics. Trace. También puede reenviar eventos de instrumentación de ASP.NET a System. Diagnostics. Trace. Esta funcionalidad la proporciona el nuevo atributo **writeToDiagnosticsTrace** del elemento &lt;Trace&gt;. Cuando este valor booleano es true, los mensajes de seguimiento ASP.NET se reenvían a la infraestructura de seguimiento System. Diagnostics para su uso por parte de los agentes de escucha registrados para mostrar mensajes de seguimiento.

## <a name="programmatic-access-to-trace-messages"></a>Acceso mediante programación a los mensajes de seguimiento

ASP.NET 2,0 permite el acceso mediante programación a todos los mensajes de seguimiento a través de la clase **TraceContextRecord** y la colección **TraceRecords** . La forma más eficaz de obtener acceso a los mensajes de seguimiento es registrar un delegado **TraceContextEventHandler** (también nuevo en ASP.net 2,0) para controlar el nuevo evento **TraceFinished** . A continuación, puede recorrer los mensajes de seguimiento como desee.

En el ejemplo de código siguiente se muestra lo siguiente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

En el ejemplo anterior, se recorre la colección TraceRecords y, a continuación, se escribe cada mensaje en la secuencia de respuesta.

## <a name="improved-application-level-tracing"></a>Seguimiento de nivel de aplicación mejorado

El seguimiento en el nivel de la aplicación se mejora mediante la introducción del nuevo atributo **MostRecent** del elemento &lt;Trace&gt;. Este atributo especifica si se muestra el resultado de seguimiento de nivel de aplicación más reciente y se descartan los datos de seguimiento más antiguos que superen los límites indicados por el requestLimit. Si es false, los datos de seguimiento se muestran para las solicitudes hasta que se alcanza el atributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Herramientas de línea de comandos de ASP.NET

Hay varias herramientas de línea de comandos que ayudan a configurar ASP.NET. Los desarrolladores de ASP.NET deben estar familiarizados con la herramienta ASPNET\_regiis. exe. ASP.NET 2,0 proporciona otras tres herramientas de línea de comandos para ayudar en la configuración.

Están disponibles las siguientes herramientas de línea de comandos:

| **Herramienta** | **Uso** |
| --- | --- |
| **ASPNET\_regiis. exe** | Permite el registro de ASP.NET con IIS. Hay dos versiones de estas herramientas que se suministran con ASP.NET 2,0, una para los sistemas de 32 bits (en la carpeta del marco) y otra para los sistemas de 64 bits (en la carpeta Framework64). La versión de 64 bits no se instalará en un sistema operativo de 32 bits. |
| **ASPNET\_regsql. exe** | La herramienta de registro de SQL Server de ASP.NET se usa para crear una base de datos Microsoft SQL Server para que la usen los proveedores de SQL Server en ASP.NET, o para agregar o quitar opciones de una base de datos existente. El archivo Aspnet\_regsql. exe se encuentra en la carpeta [unidad:] \WINDOWS\Microsoft.NET\Framework\versionNumber del servidor Web. |
| **ASPNET\_regbrowsers. exe** | La herramienta de registro del explorador de ASP.NET analiza y compila todas las definiciones del explorador en todo el sistema en un ensamblado e instala el ensamblado en la caché global de ensamblados. La herramienta utiliza los archivos de definición del explorador (. Archivos del explorador) desde el subdirectorio de .NET Framework exploradores. La herramienta se puede encontrar en el directorio%SystemRoot%\Microsoft.NET\Framework\version\ |
| **ASPNET\_Compiler. exe** | La herramienta de compilación ASP.NET le permite compilar una aplicación Web de ASP.NET, ya sea en contexto o para su implementación en una ubicación de destino, como un servidor de producción. La compilación en contexto ayuda al rendimiento de las aplicaciones, ya que los usuarios finales no detectan un retraso en la primera solicitud a la aplicación mientras se compila la aplicación. |

Dado que la herramienta ASPNET\_regiis. exe no es nueva en ASP.NET 2,0, no se tratará aquí.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>Herramienta de registro de SQL Server de ASP.NET: ASPNET\_regsql. exe

Puede establecer varios tipos de opciones mediante la herramienta de registro de SQL Server de ASP.NET. Puede especificar una conexión SQL, especificar qué servicios de aplicación de ASP.NET usar SQL Server para administrar la información, indicar qué base de datos o tabla se usa para la dependencia de caché de SQL y agregar o quitar compatibilidad para usar SQL Server para almacenar procedimientos y estado de sesión.

Varios servicios de aplicación de ASP.NET dependen de un proveedor para administrar el almacenamiento y la recuperación de datos de un origen de datos. Cada proveedor es específico del origen de datos. ASP.NET incluye un proveedor de SQL Server para las siguientes características de ASP.NET:

- Membership (clase [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) ).
- Administración de roles (clase [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) ).
- Profile (clase [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) ).
- Personalización de elementos web (clase [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) ).
- Eventos Web (clase [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) ).

Al instalar ASP.NET, el archivo Machine. config del servidor incluye elementos de configuración que especifican SQL Server proveedores para cada una de las características de ASP.NET que dependen de un proveedor. De forma predeterminada, estos proveedores se configuran para conectarse a una instancia de usuario local de SQL Server Express 2005. Si cambia la cadena de conexión predeterminada que usan los proveedores, antes de poder usar cualquiera de las características de ASP.NET configuradas en la configuración del equipo, debe instalar la base de datos de SQL Server y los elementos de base de datos de la característica elegida mediante ASPNET\_regsql. exe. Si la base de datos que se especifica con la herramienta de registro de SQL no existe ya (Aspnetdb será la base de datos predeterminada si no se especifica ninguna en la línea de comandos), el usuario actual debe tener derechos para crear bases de datos en SQL Server así como para crear el esquema e lements en una base de datos.

### <a name="sql-cache-dependency"></a>Dependencia de memoria caché de SQL

Una característica avanzada del almacenamiento en caché de resultados de ASP.NET es la dependencia de caché de SQL. La dependencia de caché de SQL admite dos modos diferentes de operación: uno que utiliza una implementación de ASP.NET de sondeo de tabla y un segundo modo que usa las características de notificación de consulta de SQL Server 2005. La herramienta de registro de SQL se puede usar para configurar el modo de operación de sondeo de tabla.

### <a name="session-state"></a>Estado de la sesión

De forma predeterminada, los valores de estado de sesión y la información se almacenan en memoria dentro del proceso ASP.NET. Como alternativa, puede almacenar los datos de la sesión en una base de datos SQL Server, donde se pueden compartir entre varios servidores Web. Si la base de datos especificada para el estado de sesión con la herramienta de registro de SQL no existe, el usuario actual debe tener derechos para crear bases de datos en SQL Server así como para crear elementos de esquema en una base de datos. Si existe la base de datos, el usuario actual debe tener derechos para crear elementos de esquema en la base de datos existente.

Para instalar la base de datos de estado de sesión en SQL Server, ejecute la herramienta ASPNET\_regsql. exe y proporcione la siguiente información con el comando:

- Nombre de la instancia de SQL Server, con la opción **-S** .
- Las credenciales de inicio de sesión de una cuenta que tenga permiso para crear una base de datos en un equipo que ejecute SQL Server. Use la opción **-E** para usar el usuario que ha iniciado sesión actualmente o use la opción **-U** para especificar un identificador de usuario junto con la opción **-P** para especificar una contraseña.
- La opción de línea de comandos **-ssadd** para agregar la base de datos de estado de sesión.

De forma predeterminada, no se puede usar la herramienta ASPNET\_regsql. exe para instalar la base de datos de estado de sesión en un equipo que ejecuta SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>La herramienta de registro del explorador de ASP.NET: ASPNET\_regbrowsers. exe

En la versión 1,1 de ASP.NET, el archivo Machine. config contenía una sección denominada &lt;browserCaps&gt;. Esta sección contiene una serie de entradas XML que definen las configuraciones de varios exploradores en función de una expresión regular. Para la versión 2,0 de ASP.NET, un nuevo. El archivo del explorador define los parámetros de un explorador determinado mediante entradas XML. Agregue información en un nuevo explorador agregando un nuevo. Archivo del explorador en la carpeta que se encuentra en%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers en el sistema.

Dado que una aplicación no lee un archivo. config cada vez que requiere información del explorador, puede crear una nueva. Archivo del explorador y ejecute ASPNET\_regbrowsers. exe para agregar los cambios necesarios al ensamblado. Esto permite que el servidor tenga acceso a la información del nuevo explorador inmediatamente, por lo que no tiene que cerrar ninguna de las aplicaciones para recopilar la información. Una aplicación puede tener acceso a las funciones del explorador a través de la propiedad Browser del HttpRequest actual.

Las siguientes opciones están disponibles cuando se ejecuta ASPNET\_regbrowser. exe:

| **Opción** | **Descripción** |
| --- | --- |
| **-?** | Muestra el texto de ayuda ASPNET\_regbbrowsers. exe en la ventana de comandos. |
| **-i** | Crea el ensamblado de funciones del explorador en tiempo de ejecución y lo instala en la caché global de ensamblados. |
| **-u** | Desinstala el ensamblado de funciones del explorador en tiempo de ejecución de la caché global de ensamblados. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>La herramienta de compilación de ASP.NET: ASPNET\_Compiler. exe

La herramienta de compilación ASP.NET se puede usar de dos maneras generales: para la compilación en contexto y la compilación para la implementación, donde se especifica un directorio de salida de destino.

### <a name="compiling-an-application-in-place"></a>[Compilar una aplicación en contexto](https://msdn.microsoft.com/library/ms229863.aspx)

La herramienta de compilación ASP.NET puede compilar una aplicación en contexto, es decir, imita el comportamiento de realizar varias solicitudes a la aplicación, lo que provoca la compilación normal. Los usuarios de un sitio precompilado no experimentarán un retraso causado por la compilación de la página en la primera solicitud.

Al precompilar un sitio en su lugar, se aplican los siguientes elementos:

- El sitio conserva su estructura de archivos y directorios.
- Debe tener compiladores para todos los lenguajes de programación utilizados por el sitio en el servidor.
- Si se produce un error en la compilación de algún archivo, se produce un error de compilación en todo el sitio

También puede volver a compilar una aplicación en su lugar después de agregarle nuevos archivos de código fuente. La herramienta compila solo los archivos nuevos o modificados, a menos que incluya la opción **-c** .

> [!NOTE]
> La compilación de una aplicación que contiene una aplicación anidada no compila la aplicación anidada. La aplicación anidada se debe compilar por separado.

### <a name="compiling-an-application-for-deployment"></a>[Compilar una aplicación para la implementación](https://msdn.microsoft.com/library/ms229863.aspx)

Para compilar una aplicación para la implementación (compilación en una ubicación de destino), especifique el parámetro targetDir. El targetDir puede ser la ubicación final de la aplicación web o la aplicación compilada se puede implementar aún más. El uso de la opción **-u** compila la aplicación de forma que pueda realizar cambios en determinados archivos de la aplicación compilada sin volver a compilarlos. ASPNET\_compilador. exe hace una distinción entre los tipos de archivo estáticos y dinámicos, y los administra de forma diferente al crear la aplicación resultante.

- Los tipos de archivo estáticos son aquellos que no tienen un compilador asociado o un proveedor de compilación, como los archivos cuyo nombre tienen extensiones como. CSS,. gif,. htm,. html,. jpg,. js, etc. Estos archivos simplemente se copian en la ubicación de destino, y se conservan sus lugares relativos en la estructura de directorios.
- Los tipos de archivo dinámicos son aquellos que tienen un compilador asociado o un proveedor de compilación, incluidos los archivos con extensiones de nombre de archivo específicas de ASP.NET como. asax,. ascx,. ashx,. aspx,. Browser,. Master, etc. La herramienta de compilación ASP.NET genera ensamblados de estos archivos. Si se omite la opción **-u** , la herramienta también crea archivos con la extensión de nombre de archivo. COMPILADO que asigna los archivos de código fuente originales a su ensamblado. Para asegurarse de que se conserva la estructura de directorios del origen de la aplicación, la herramienta genera archivos de marcador de posición en las ubicaciones correspondientes de la aplicación de destino.

Debe usar la opción **-u** para indicar que se puede modificar el contenido de la aplicación compilada. De lo contrario, se omiten las modificaciones posteriores o se producen errores en tiempo de ejecución.

En la tabla siguiente se describe cómo la herramienta de compilación ASP.NET controla distintos tipos de archivo cuando se incluye la opción **-u** .

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .ascx, .aspx, .master | Estos archivos se dividen en código fuente y marcado, lo que incluye archivos de código subyacente y cualquier código incluido en &lt;script runat = "Server"&gt; elementos. El código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash, y los ensamblados se colocan en el directorio bin. Cualquier código alineado, es decir, el código incluido entre el **&lt;%** y%corchetes de **&gt;** , se incluye con el marcado y no se compila. Se crean nuevos archivos con el mismo nombre que los archivos de origen para contener el marcado y colocarlos en los directorios de salida correspondientes. |
| .ashx, .asmx | Estos archivos no se compilan y se mueven a los directorios de salida tal y como están y no se compilan. Si desea que se compile el código del controlador, coloque el código en los archivos de código fuente en el directorio de código de la aplicación\_. |
| . CS,. VB,. jsl,. cpp (sin incluir archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Estos archivos se compilan e incluyen como un recurso en los ensamblados que hacen referencia a ellos. Los archivos de origen no se copian en el directorio de salida. Si no se hace referencia a un archivo de código, no se compila. |
| Tipos de archivo personalizados | Estos archivos no se compilan. Estos archivos se copian en los directorios de salida correspondientes. |
| Archivos de código fuente en el subdirectorio de código de la aplicación\_ | Estos archivos se compilan en ensamblados y se colocan en el directorio bin. |
| archivos. resx y. Resource en el subdirectorio App\_GlobalResources | Estos archivos se compilan en ensamblados y se colocan en el directorio bin. No se crea ningún subdirectorio de App\_GlobalResources en el directorio de salida principal y no se copian los archivos. resx o. Resources ubicados en el directorio de origen en los directorios de salida. |
| archivos. resx y. Resource en el subdirectorio App\_LocalResources | Estos archivos no se compilan y se copian en los directorios de salida correspondientes. |
| archivos. Skin en el subdirectorio App\_themes | Los archivos. Skin y los archivos de tema estáticos no se compilan y se copian en los directorios de salida correspondientes. |
| los ensamblados de tipos de archivos estáticos Web. config del explorador ya están presentes en el directorio bin | Estos archivos se copian tal cual en los directorios de salida. |

En la tabla siguiente se describe cómo la herramienta de compilación ASP.NET controla distintos tipos de archivo cuando se omite la opción **-u** .

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Estos archivos se dividen en código fuente y marcado, lo que incluye archivos de código subyacente y cualquier código incluido en &lt;script runat = "Server"&gt; elementos. El código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash. Los ensamblados resultantes se colocan en el directorio bin. Cualquier código alineado, es decir, el código incluido entre el **&lt;%** y%corchetes de **&gt;** , se incluye con el marcado y no se compila. El compilador crea nuevos archivos para contener el marcado con el mismo nombre que los archivos de código fuente. Estos archivos resultantes se colocan en el directorio bin. El compilador también crea archivos con el mismo nombre que los archivos de código fuente, pero con la extensión. Compilada que contiene información de asignación. El. Los archivos compilados se colocan en los directorios de salida correspondientes a la ubicación original de los archivos de código fuente. |
| .ascx | Estos archivos se dividen en código fuente y marcado. El código fuente se compila en ensamblados y se coloca en el directorio bin, con nombres que se derivan de un algoritmo hash. No se generan archivos de marcado. |
| . CS,. VB,. jsl,. cpp (sin incluir archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Código fuente al que hacen referencia los ensamblados generados a partir de archivos. ascx,. ashx o. aspx se compilan en ensamblados y se colocan en el directorio bin. No se copia ningún archivo de código fuente. |
| Tipos de archivo personalizados | Estos archivos se compilan como archivos dinámicos. Dependiendo del tipo de archivo en el que se basan, el compilador puede colocar los archivos de asignación en los directorios de salida. |
| Archivos del subdirectorio de código del\_de la aplicación | Los archivos de código fuente de este subdirectorio se compilan en ensamblados y se colocan en el directorio bin. |
| Archivos en el subdirectorio GlobalResources de la aplicación\_ | Estos archivos se compilan en ensamblados y se colocan en el directorio bin. No se crea ninguna aplicación\_subdirectorio GlobalResources en el directorio de salida principal. Si el archivo de configuración especifica appliesTo = "All", los archivos. resx y. Resources se copian en los directorios de salida. No se copian si un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)hace referencia a ellos. |
| archivos. resx y. Resource en el subdirectorio App\_LocalResources | Estos archivos se compilan en ensamblados con nombres únicos y se colocan en el directorio bin. No se copia ningún archivo. resx o. Resource en los directorios de salida. |
| archivos. Skin en el subdirectorio App\_themes | Los temas se compilan en ensamblados y se colocan en el directorio bin. Los archivos de código auxiliar se crean para los archivos. Skin y se colocan en el directorio de salida correspondiente. Los archivos estáticos (como. CSS) se copian en los directorios de salida. |
| los ensamblados de tipos de archivos estáticos Web. config del explorador ya están presentes en el directorio bin | Estos archivos se copian tal cual en el directorio de salida. |

### <a name="fixed-assembly-names"></a>[Nombres de ensamblado fijos](https://msdn.microsoft.com/library/ms229863.aspx##)

Algunos escenarios, como la implementación de una aplicación Web mediante el Windows Installer MSI, requieren el uso de nombres de archivo y contenido coherentes, así como estructuras de directorios coherentes para identificar los ensamblados o los valores de configuración de las actualizaciones. En esos casos, puede usar la opción **-fixednames** para especificar que la herramienta de compilación ASP.net debe compilar un ensamblado para cada archivo de código fuente en lugar de usar, donde se compilan varias páginas en ensamblados. Esto puede conducir a un gran número de ensamblados, por lo que si le preocupa la escalabilidad, debe utilizar esta opción con precaución.

### <a name="strong-name-compilation"></a>[Compilación de nombre seguro](https://msdn.microsoft.com/library/ms229863.aspx##)

Se proporcionan las opciones **-APTCA**, **-delaysign**, **-keycontainer** y **-keyfile** para que pueda usar ASPNET\_Compiler. exe para crear ensamblados con nombre seguro sin usar la [herramienta de nombre seguro (SN. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) por separado. Estas opciones corresponden respectivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**y **AssemblyKeyFileAttribute**.

La explicación de estos atributos está fuera del ámbito de este curso.

## <a name="labs"></a>Laboratorios

Cada uno de los siguientes laboratorios se basa en los laboratorios anteriores. Tendrá que hacerlo en orden.

## <a name="lab-1-using-the-configuration-api"></a>Práctica 1: uso de la API de configuración

1. Cree un nuevo sitio web denominado *mod9lab*.
2. Agregue un nuevo archivo de configuración web al sitio.
3. Agregue lo siguiente al archivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Esto garantizará que tiene permiso para guardar los cambios en el archivo Web. config.

1. Agregue un nuevo control etiqueta a default. aspx y cambie el identificador a **lblDebugStatus**.
2. Agregue un nuevo control de botón a default. aspx.
3. Cambie el identificador del control de botón a **btnToggleDebug** y el texto para **alternar el estado de depuración**.
4. Abra la vista de código para el archivo de código subyacente de default. aspx y agregue una instrucción **using** para **System. Web. Configuration** como se indica a continuación:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Agregue dos variables privadas a la clase y una página\_método init como se muestra a continuación:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Agregue el siguiente código a la página\_carga:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Guarde y examine default. aspx. Observe que el control etiqueta muestra el estado de depuración actual.
2. Haga doble clic en el control Button en el diseñador y agregue el código siguiente al evento click del control Button:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Guarde y examine default. aspx y haga clic en el botón.
2. Abra el archivo Web. config después de cada botón haga clic y observe el atributo **Debug** en la sección &lt;compilation&gt;.

## <a name="lab-2-logging-application-restarts"></a>Práctica 2: registro de los reinicios de la aplicación

En este laboratorio, creará código que le permitirá alternar el registro de cierres de aplicaciones, inicios y recompilaciones en el Visor de eventos.

1. Agregue un DropDownList a default. aspx y cambie el identificador a ddlLogAppEvents.
2. Establezca la propiedad **AutoPostBack** para DropDownList en **true**.
3. Agregue tres elementos a la colección de elementos para DropDownList. Haga que el **texto** del primer elemento *Seleccione valor* y el valor-1. Haga que el **texto** y el **valor** del segundo elemento sean **true** y el **texto** y el **valor** del tercer elemento sean **false**.
4. Agregue una nueva etiqueta a default. aspx. Cambie el identificador a **lblLogAppEvents**.
5. Abra la vista de código subyacente para default. aspx y agregue una nueva declaración para una variable de tipo HealthMonitoringSection, como se muestra a continuación:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Agregue el código siguiente al código existente en la página\_init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Haga doble clic en el DropDownList y agregue el código siguiente al evento SelectedIndexChanged:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Examine default. aspx.
2. Establezca la lista desplegable en **false**.
3. Borre el registro de la aplicación en el Visor de eventos.
4. Haga clic en el botón para cambiar el atributo de depuración de la aplicación.
5. Actualice el registro de la aplicación en el Visor de eventos. 

    1. ¿Se han registrado eventos?
    2. ¿Por qué o por qué no?
6. Establezca la lista desplegable en **true.**
7. Haga clic en el botón para alternar el atributo de depuración de la aplicación.
8. Actualice el inicio de sesión de la aplicación en el Visor de eventos. 

    1. ¿Se han registrado eventos?
    2. ¿Cuál fue el motivo del cierre de la aplicación?
9. Experimente con la activación y desactivación del registro y examine los cambios realizados en el archivo Web. config.

## <a name="more-information"></a>Más información:

El modelo de proveedor de ASP.NET 2.0 le permite crear sus propios proveedores para no solo la instrumentación de la aplicación, sino también para muchos otros usos, como la pertenencia, los perfiles, etc. Para obtener información detallada sobre cómo escribir un proveedor personalizado para registrar eventos de aplicación en un archivo de texto, visite [este vínculo](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
