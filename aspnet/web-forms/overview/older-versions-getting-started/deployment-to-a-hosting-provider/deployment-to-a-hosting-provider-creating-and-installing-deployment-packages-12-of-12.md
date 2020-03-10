---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12) | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: db8f58e3679e6dea865dadb6f64916032dd9f38c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78424039"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: solución de problemas (12 de 12)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo implementar en sitios web de Windows Azure, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

En esta página se describen algunos problemas comunes que pueden surgir al implementar una aplicación Web de ASP.NET con Visual Studio. Para cada uno de ellos, se proporcionan una o varias causas posibles y las soluciones correspondientes.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Error de servidor en la aplicación '/': la configuración de error personalizada actual impide que los detalles del error se vean de forma remota.

### <a name="scenario"></a>Escenario

Después de implementar un sitio en un host remoto, obtiene un mensaje de error que menciona el valor de customErrors en el archivo Web. config, pero no indica cuál fue la causa real del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

De forma predeterminada, ASP.NET solo muestra información de error detallada cuando la aplicación web se ejecuta en el equipo local. Por lo general, no desea mostrar información de error detallada cuando la aplicación web está disponible públicamente a través de Internet, ya que es posible que los hackers puedan usar esta información para encontrar vulnerabilidades en la aplicación. Sin embargo, cuando se implementa un sitio o actualizaciones en un sitio, a veces algo va mal y es necesario obtener el mensaje de error real.

Para permitir que la aplicación muestre mensajes de error detallados cuando se ejecuta en el host remoto, edite el archivo Web. config para establecer el modo de `customErrors` desactivado, vuelva a implementar la aplicación y vuelva a ejecutar la aplicación:

1. Si el archivo Web. config de la aplicación tiene un elemento `customErrors` en el elemento `system.web`, cambie el atributo `mode` a "OFF". De lo contrario, agregue un elemento `customErrors` en el elemento `system.web` con el atributo `mode` establecido en "OFF", como se muestra en el ejemplo siguiente:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Implemente la aplicación.
3. Ejecute la aplicación y repita cualquier cosa que haya hecho antes de que se produjera el error. Ahora puede ver cuál es el mensaje de error real.
4. Cuando haya resuelto el error, restaure el valor de `customErrors` original y vuelva a implementar la aplicación.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Acceso denegado en una página web que usa SQL Server Compact

### <a name="scenario"></a>Escenario

Al implementar un sitio que usa SQL Server Compact y ejecutar una página en el sitio implementado que tiene acceso a la base de datos, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La cuenta de servicio de red en el servidor debe poder leer archivos binarios nativos de SQL Service Compact que se encuentran en la carpeta *bin\amd64* o *bin\x86* , pero no tiene permisos de lectura para esas carpetas. Establezca el permiso de lectura para servicio de red en la carpeta *bin* y asegúrese de extender los permisos a las subcarpetas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>No se puede leer el archivo de configuración debido a permisos insuficientes

### <a name="scenario"></a>Escenario

Al hacer clic en el botón publicar de Visual Studio para implementar una aplicación en IIS en el equipo local, se produce un error en la publicación y la ventana de **salida** muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Para usar la publicación con un solo clic en IIS en el equipo local, debe ejecutar Visual Studio con permisos de administrador. Cierre Visual Studio y reinícielo con permisos de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>No se pudo conectar con el equipo de destino... Usar el proceso especificado

### <a name="scenario"></a>Escenario

Al hacer clic en el botón publicar de Visual Studio para implementar una aplicación, se produce un error en la publicación y la ventana de **salida** muestra un mensaje de error similar al siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Un servidor proxy está interrumpiendo la comunicación con el servidor de destino. En el panel de control de Windows o en Internet Explorer, seleccione **Opciones de Internet** y seleccione la pestaña **conexiones** . En el cuadro de diálogo **propiedades de Internet** , haga clic en **configuración de LAN**. En el cuadro de diálogo Configuración de la **red de área local (LAN)** , desactive la casilla **detectar la configuración automáticamente** . A continuación, vuelva a hacer clic en el botón publicar.

Si el problema persiste, póngase en contacto con el administrador del sistema para determinar lo que se puede hacer con la configuración de proxy o firewall. El problema se produce porque Web Deploy usa un puerto no estándar para la implementación del servicio de administración web (8172); en el caso de otras conexiones, Web Deploy usa el puerto 80. Cuando se implementa en un proveedor de hospedaje de terceros, normalmente se usa el servicio de administración web.

## <a name="default-net-40-application-pool-does-not-exist"></a>El grupo de aplicaciones .NET 4,0 predeterminado no existe.

### <a name="scenario"></a>Escenario

Cuando implemente una aplicación que requiera el .NET Framework 4, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

ASP.NET 4 no está instalado en IIS. Si el servidor en el que va a realizar la implementación es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo, pero es posible que no esté instalado en IIS. En el servidor en el que va a realizar la implementación, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Es posible que también tenga que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, vea el tutorial [implementar en IIS como entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>El formato de la cadena de inicialización no se ajusta a la especificación que empieza en el índice 0.

### <a name="scenario"></a>Escenario

Después de implementar una aplicación mediante la publicación con un solo clic, cuando se ejecuta una página que tiene acceso a la base de datos, aparece el siguiente mensaje de error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Abra el archivo *Web. config* en el sitio implementado y compruebe si los valores de la cadena de conexión comienzan con `$(ReplaceableToken_`, como en el ejemplo siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Si las cadenas de conexión son similares a las de este ejemplo, edite el archivo de proyecto y agregue la siguiente propiedad al elemento `PropertyGroup` que es para todas las configuraciones de compilación:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

A continuación, vuelva a implementar la aplicación.

## <a name="http-500-internal-server-error"></a>Error interno del servidor HTTP 500

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el siguiente mensaje de error sin información específica que indica la causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Hay muchas causas de errores de 500, pero una causa posible si está siguiendo estos tutoriales es que coloca un elemento XML en el lugar equivocado en uno de los archivos de transformación XML. Por ejemplo, obtendría este error si coloca la transformación que inserta un elemento `<location>` en `<system.web>` en lugar de hacerlo directamente en `<configuration>`. En ese caso, la solución es corregir el archivo de transformación XML y volver a implementarlo.

## <a name="http-50021-internal-server-error"></a>Error interno del servidor HTTP 500,21

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El sitio que ha implementado tiene como destino ASP.NET 4, pero ASP.NET 4 no está registrado en IIS en el servidor. En el servidor, abra un símbolo del sistema con privilegios elevados y registre ASP.NET 4 mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Es posible que también tenga que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, vea el tutorial [implementar en IIS como entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Error de inicio de sesión al abrir SQL Server Express base de datos en datos de\_de la aplicación

### <a name="scenario"></a>Escenario

Ha actualizado la cadena de conexión del archivo *Web. config* para que apunte a una base de datos de SQL Server Express como archivo *. MDF* en la carpeta de datos de la *aplicación\_* y, la primera vez que ejecute la aplicación, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El nombre del archivo *. MDF* no puede coincidir con el nombre de ninguna base de datos SQL Server Express que haya existido en el equipo, aunque se haya eliminado el archivo *. MDF* de la base de datos existente. Cambie el nombre del archivo *. MDF* por un nombre que nunca se haya usado como nombre de la base de datos y cambie el archivo *Web. config* para que use el nuevo nombre. Como alternativa, puede usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar las bases de datos de SQL Server Express existentes previamente.

## <a name="model-compatibility-cannot-be-checked"></a>No se puede comprobar la compatibilidad de modelos

### <a name="scenario"></a>Escenario

Ha actualizado la cadena de conexión del archivo *Web. config* para que apunte a una nueva base de datos de SQL Server Express y, la primera vez que ejecute la aplicación, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Si el nombre de la base de datos que coloca en el archivo Web. config se ha usado antes en el equipo, es posible que ya exista una base de datos con algunas tablas. Seleccione un nuevo nombre que no se haya usado en el equipo antes y cambie el archivo *Web. config* para que apunte a usar este nuevo nombre de base de datos. Como alternativa, puede usar [SQL Server Express utilidad](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar la base de datos existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Error de SQL cuando un script intenta crear usuarios o roles

### <a name="scenario"></a>Escenario

Está usando la implementación de base de datos configurada en la pestaña **empaquetar/publicar SQL** , los scripts SQL que se ejecutan durante la implementación incluyen los comandos crear usuario o crear rol y la ejecución del script produce un error cuando se ejecutan los comandos. Es posible que vea mensajes más detallados, como los siguientes:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Si este error se produce cuando ha configurado la implementación de base de datos en el Asistente para **publicación web** en lugar de la pestaña **empaquetar/publicar SQL** , cree un subproceso en el foro de [configuración e implementación](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) y la solución se agregará a esta página de solución de problemas.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La cuenta de usuario que está usando para realizar la implementación no tiene permiso para crear usuarios o roles. Por ejemplo, la empresa de hospedaje podría asignar los roles `db_datareader`, `db_datawriter`y `db_ddladmin` a la cuenta de usuario que configure. Son suficientes para crear la mayoría de los objetos de base de datos, pero no para crear usuarios o roles. Una manera de evitar el error es excluir los usuarios y los roles de la implementación de base de datos. Puede hacerlo modificando el elemento `PreSource` del script generado automáticamente de la base de datos para que incluya los siguientes atributos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Para obtener información sobre cómo editar el elemento de `PreSource` en el archivo de proyecto, vea [Cómo: editar la configuración de implementación en el archivo de proyecto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si los usuarios o los roles de la base de datos de desarrollo deben estar en la base de datos de destino, póngase en contacto con su proveedor de hospedaje para obtener ayuda.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Error de tiempo de espera de SQL Server al ejecutar scripts personalizados durante la implementación

### <a name="scenario"></a>Escenario

Ha especificado scripts SQL personalizados para ejecutarse durante la implementación y, cuando Web Deploy los ejecuta, se agota el tiempo de espera.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La ejecución de varios scripts que tienen distintos modos de transacción puede provocar errores de tiempo de espera. De forma predeterminada, los scripts generados automáticamente se ejecutan en una transacción, pero no los scripts personalizados. Si selecciona la opción **extraer datos y/o esquema de una base de datos existente** en la pestaña **empaquetar/publicar SQL** , y si agrega un script SQL personalizado, debe cambiar la configuración de la transacción en algunos scripts para que todos los scripts usen la misma configuración de transacciones. Para obtener más información, vea [Cómo: implementar una base de datos con un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343.aspx).

Si ha configurado las opciones de transacción para que todas sean las mismas pero sigue apareciendo este error, una posible solución consiste en ejecutar los scripts por separado. En la cuadrícula **scripts de base de datos** de la pestaña **empaquetar/publicar** SQL, desactive la casilla **incluir** del script que provoca el error de tiempo de espera y, a continuación, publique el proyecto. A continuación, vuelva a la cuadrícula **scripts de base de datos** , active la casilla **incluir** del script y desactive las casillas **incluir** para los demás scripts. A continuación, publique el proyecto de nuevo. Esta vez, al publicar, solo se ejecuta el script personalizado seleccionado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Los datos de la secuencia del manifiesto del sitio no están disponibles todavía

### <a name="scenario"></a>Escenario

Al instalar un paquete mediante el archivo *deploy. cmd* con la opción `t` (prueba), verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El mensaje de error significa que el comando no puede generar un informe de prueba. Sin embargo, el comando puede ejecutarse si se usa la opción `y` (instalación real). El mensaje solo indica que hay un problema con la ejecución del comando en modo de prueba.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Esta aplicación requiere ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Escenario

Cuando intente realizar la implementación, verá el mensaje de error siguiente:

 Error: los datos de la secuencia de ' sitemanifest/dbFullSql [@path= ' C:\TEMP\AdventureWorksGrant.sql ']/sqlScript ' todavía no están disponibles. El grupo de aplicaciones que está intentando usar tiene la propiedad "managedRuntimeVersion" establecida en "v 2.0". Esta aplicación requiere ' v 4.0 '. 

### <a name="possible-cause-and-solution"></a>Causa posible y solución

ASP.NET 4 no está instalado en IIS. Si el servidor en el que va a realizar la implementación es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo, pero es posible que no esté instalado en IIS. En el servidor en el que va a realizar la implementación, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>No se puede convertir Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Escenario

Al implementar un paquete, aparece el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Está intentando implementar desde el administrador de IIS con la interfaz de usuario de Web Deploy 1,1 en un servidor que tiene Web Deploy 2,0 instalado. Si está utilizando la herramienta de administración remota de IIS para implementar importando un paquete, compruebe el cuadro de diálogo **nuevas características disponibles** al establecer la conexión. (Este cuadro de diálogo solo se puede mostrar una vez cuando se establece la conexión por primera vez. Para borrar la conexión y volver a iniciarla, cierre el administrador de IIS y vuelva a iniciarla escribiendo `inetmgr /reset` en el símbolo del sistema). Si una de las características enumeradas es **Web deploy interfaz de usuario**y tiene un número de versión inferior a 8, es posible que el servidor en el que se está implementando tenga instaladas las versiones 1,1 y 2,0 de web deploy. Para realizar la implementación desde un cliente que tiene instalado 2,0, el servidor debe tener instalado solo Web Deploy 2,0. Tendrá que ponerse en contacto con el proveedor de hospedaje para resolver este problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>No se pueden cargar los componentes nativos de SQL Server Compact

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el mensaje de error siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El sitio implementado no tiene subcarpetas *AMD64* y *x86* con los ensamblados nativos en la carpeta *bin* de la aplicación. En un equipo que tenga SQL Server Compact instalado, los ensamblados nativos se encuentran en *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private*. La mejor manera de obtener los archivos correctos en las carpetas correctas en un proyecto de Visual Studio es instalar el paquete Sqlservercom de NuGet. La instalación de paquetes agrega un script posterior a la compilación para copiar los ensamblados nativos en *AMD64* y *x86*. Sin embargo, para que se implementen, debe incluirlas manualmente en el proyecto. Para obtener más información, consulte el tutorial [implementación de SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Error "la ruta de acceso no es válida" después de implementar una aplicación de Code First de Entity Framework

### <a name="scenario"></a>Escenario

Implementa una aplicación que usa Migraciones de Entity Framework Code First y un DBMS como SQL Server Compact que almacena su base de datos en un archivo de la carpeta app\_Data. Ha configurado Migraciones de Code First para crear la base de datos después de la primera implementación. Al ejecutar la aplicación, se obtiene un mensaje de error similar al siguiente ejemplo:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Code First está intentando crear la base de datos, pero no existe la carpeta de datos de la\_de la aplicación. O bien no tiene ningún archivo en la carpeta de datos de la *\_* de la aplicación cuando implementó, o bien seleccionó **excluir datos de\_de aplicación** en la pestaña **empaquetar/publicar web** de la ventana **propiedades del proyecto** . El proceso de implementación no creará una carpeta en el servidor si no hay ningún archivo en la carpeta que se va a copiar en el servidor. Si ya tiene la base de datos configurada en el sitio, el proceso de implementación eliminará los archivos y la *aplicación\_* carpeta de datos en sí misma si ha seleccionado **quitar archivos adicionales en el destino** en el perfil de publicación. Para solucionar el problema, coloque un archivo de marcador de posición como un archivo. txt en la carpeta de datos de la\_de la *aplicación* , asegúrese de que no tiene la opción **excluir datos de\_** de la aplicación seleccionada y vuelva a implementarla. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"El objeto COM que se ha separado de su RCW subyacente no se puede usar".

### <a name="scenario"></a>Escenario

Ha usado correctamente la publicación con un solo clic para implementar la aplicación y, a continuación, comienza a obtener este error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Cerrar y reiniciar Visual Studio suele ser todo lo que se necesita para resolver este error.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Se produce un error en la implementación porque las credenciales de usuario usadas para la publicación no tienen autoridad setACL

### <a name="scenario"></a>Escenario

La publicación genera un error que indica que no tiene autoridad para establecer permisos de carpeta (la cuenta de usuario que está usando no tiene autoridad setACL).

### <a name="possible-cause-and-solution"></a>Causa posible y solución

De forma predeterminada, Visual Studio establece permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la carpeta de datos de la\_de la aplicación. Si sabe que los permisos predeterminados en las carpetas de sitio son correctos y no es necesario establecerlos, deshabilite este comportamiento agregando **&lt;destino de IncludeSetACLProviderOn&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** al archivo de Perfil de publicación (para que afecte a un solo perfil) o al archivo WPP. targets (para que afecte a todos los perfiles). Para obtener información sobre cómo editar estos archivos, vea [Cómo: editar la configuración de implementación en archivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Errores de acceso denegado cuando la aplicación intenta escribir en una carpeta de aplicación

### <a name="scenario"></a>Escenario

Los errores de la aplicación cuando intenta crear o editar un archivo en una de las carpetas de la aplicación, ya que no tiene autoridad de escritura para esa carpeta.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

De forma predeterminada, Visual Studio establece permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la carpeta de datos de la\_de la aplicación. Si la aplicación necesita acceso de escritura a una subcarpeta, puede establecer permisos para esa carpeta, tal como se muestra en los tutoriales de [configuración de permisos de carpeta](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) e [implementación en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Si la aplicación necesita acceso de escritura a la carpeta raíz del sitio, tendrá que impedir que se configure el acceso de solo lectura en la carpeta raíz agregando **&lt;IncludeSetACLProviderOn destino&gt;falso&lt;/IncludeSetACLProviderOnDestination&gt;** al archivo de Perfil de publicación (para que afecte a un solo perfil) o al archivo WPP. targets (para que afecte a todos los perfiles). Para obtener información sobre cómo editar estos archivos, vea [Cómo: editar la configuración de implementación en archivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Error de configuración: el atributo targetFramework hace referencia a una versión posterior a la versión instalada de la .NET Framework

### <a name="scenario"></a>Escenario

Ha publicado correctamente un proyecto web destinado a ASP.NET 4,5, pero al ejecutar la aplicación (con el modo de `customErrors` establecido en "OFF" en el archivo Web. config) recibe el siguiente error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

En el cuadro error de origen de la página de error se resalta la siguiente línea de Web. config como causa del error:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El servidor no es compatible con ASP.NET 4,5. Póngase en contacto con el proveedor de hospedaje para determinar si se puede Agregar compatibilidad con ASP.NET 4,5. Si la actualización del servidor no es una opción, tiene que implementar un proyecto web destinado a ASP.NET 4 o una versión anterior en su lugar. Si implementa un proyecto Web de ASP.NET 4 o una versión anterior en el mismo destino, active la casilla **quitar archivos adicionales en el destino** en la pestaña **configuración** del Asistente para **publicación web** . Si no selecciona **quitar archivos adicionales en el destino**, seguirá recibiendo la página de error de configuración.

La ventana **propiedades** del proyecto incluye una lista desplegable plataforma de destino, pero no puede resolver este problema simplemente cambiando de **.NET Framework 4,5** a **.NET Framework 4**. Si cambia la plataforma de destino a una versión anterior de .NET Framework, el proyecto seguirá teniendo referencias a los ensamblados de la versión de .NET Framework posterior y no se ejecutará. Tendrá que cambiar manualmente esas referencias o crear un nuevo proyecto que tenga como destino .NET Framework 4 o una versión anterior. Para obtener más información, consulte [.NET Framework destino para sitios web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
