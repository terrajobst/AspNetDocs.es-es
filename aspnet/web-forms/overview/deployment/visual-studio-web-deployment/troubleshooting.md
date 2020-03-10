---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Implementación web de ASP.NET con Visual Studio: solución de problemas | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b42476fca18b04f4557a216ee205cfd9220023e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78465097"
---
# <a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Implementación web de ASP.NET con Visual Studio: solución de problemas

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

En esta página se describen algunos problemas comunes que pueden surgir al implementar una aplicación Web de ASP.NET con Visual Studio. Para cada uno de ellos, se proporcionan una o varias causas posibles y las soluciones correspondientes.

Los escenarios mostrados se aplican a los proveedores de hospedaje de Azure y de terceros. Para obtener más información sobre cómo solucionar problemas de aplicaciones web en Azure App Service, consulte los siguientes recursos:

- [Solución de problemas de una aplicación web en Azure App Service con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Supervisión de Web Apps en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Anuncio del lanzamiento del SDK de Windows Azure 2,0 para .net](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (blog de ScottGu, muestra cómo obtener registros de diagnóstico en Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Error de servidor en la aplicación '/': la configuración de error personalizada actual impide que los detalles del error se vean de forma remota.

### <a name="scenario"></a>Escenario

Después de implementar un sitio en un host remoto, obtiene un mensaje de error que menciona el valor de customErrors en el archivo Web. config, pero no indica cuál fue la causa real del error:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

De forma predeterminada, ASP.NET solo muestra información de error detallada cuando la aplicación web se ejecuta en el equipo local. Por lo general, no desea mostrar información de error detallada cuando la aplicación web está disponible públicamente a través de Internet, ya que es posible que los hackers puedan usar esta información para encontrar vulnerabilidades en la aplicación. Sin embargo, cuando se implementa un sitio o actualizaciones en un sitio, a veces algo va mal y es necesario obtener el mensaje de error real.

Para permitir que la aplicación muestre mensajes de error detallados cuando se ejecuta en el host remoto, edite el archivo Web. config para establecer el modo customErrors en OFF, vuelva a implementar la aplicación y vuelva a ejecutar la aplicación:

1. Si el archivo Web. config de la aplicación tiene un elemento customErrors en el elemento System. Web, cambie el atributo de modo a "OFF". De lo contrario, agregue un elemento customErrors en el elemento System. Web con el atributo Mode establecido en "OFF", como se muestra en el ejemplo siguiente: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Implemente la aplicación.
3. Ejecute la aplicación y repita cualquier cosa que haya hecho antes de que se produjera el error. Ahora puede ver cuál es el mensaje de error real.
4. Cuando haya resuelto el error, restaure el valor de customErrors original y vuelva a implementar la aplicación.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>No se puede crear ni instantánea ' ContosoUniversity ' cuando el archivo ya existe.

### <a name="scenario"></a>Escenario

Al intentar ejecutar un proyecto en Visual Studio, aparecerá una página de error con un mensaje similar al ejemplo siguiente:

Error del servidor en la aplicación '/'. No se puede crear ni instantánea ' ContosoUniversity ' cuando el archivo ya existe.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Espere un minuto y actualice el explorador, o vuelva a compilar el sitio e intente ejecutarlo de nuevo.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Acceso denegado en una página web que usa SQL Server Compact

### <a name="scenario"></a>Escenario

Al implementar un sitio que usa SQL Server Compact y ejecutar una página en el sitio implementado que tiene acceso a la base de datos, verá el mensaje de error siguiente:

Se denegó el acceso. (Excepción de HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La cuenta de servicio de red en el servidor debe poder leer archivos binarios nativos de SQL Service Compact que se encuentran en la carpeta *bin\amd64* o *bin\x86* , pero no tiene permisos de lectura para esas carpetas. Establezca el permiso de lectura para servicio de red en la carpeta *bin* y asegúrese de extender los permisos a las subcarpetas.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>No se puede leer el archivo de configuración debido a permisos insuficientes

### <a name="scenario"></a>Escenario

Al hacer clic en el botón publicar de Visual Studio para implementar una aplicación en IIS en el equipo local, se produce un error en la publicación y la ventana de **salida** muestra un mensaje de error similar al siguiente:

Se produjo un error al leer el archivo de configuración de IIS ' equipo/redireccionamiento '. La identidad que realiza esta operación fue... Error: no se puede leer el archivo de configuración debido a permisos insuficientes.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Para usar la publicación con un solo clic en IIS en el equipo local, debe ejecutar Visual Studio con permisos de administrador. Cierre Visual Studio y reinícielo con permisos de administrador.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>No se pudo conectar con el equipo de destino... Usar el proceso especificado

### <a name="scenario"></a>Escenario

Al hacer clic en el botón publicar de Visual Studio para implementar una aplicación, se produce un error en la publicación y la ventana de **salida** muestra un mensaje de error similar al siguiente:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Un servidor proxy está interrumpiendo la comunicación con el servidor de destino. En el panel de control de Windows o en Internet Explorer, seleccione **Opciones de Internet** y seleccione la pestaña **conexiones** . En el cuadro de diálogo **propiedades de Internet** , haga clic en **configuración de LAN**. En el cuadro de diálogo Configuración de la **red de área local (LAN)** , desactive la casilla **detectar la configuración automáticamente** . A continuación, vuelva a hacer clic en el botón publicar.

Si el problema persiste, póngase en contacto con el administrador del sistema para determinar lo que se puede hacer con la configuración de proxy o firewall. El problema se produce porque Web Deploy usa un puerto no estándar para la implementación del servicio de administración web (8172); en el caso de otras conexiones, Web Deploy usa el puerto 80. Cuando se implementa en un proveedor de hospedaje de terceros, normalmente se usa el servicio de administración web.

## <a name="default-net-40-application-pool-does-not-exist"></a>El grupo de aplicaciones .NET 4,0 predeterminado no existe.

### <a name="scenario"></a>Escenario

Cuando implemente una aplicación que requiera el .NET Framework 4, verá el mensaje de error siguiente:

El grupo de aplicaciones .NET 4,0 predeterminado no existe o no se pudo agregar la aplicación. Compruebe que ASP.NET 4,0 está instalado en este equipo.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

ASP.NET 4 no está instalado en IIS. Si el servidor en el que va a realizar la implementación es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo, pero es posible que no esté instalado en IIS. En el servidor en el que va a realizar la implementación, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Es posible que también tenga que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, vea el tutorial implementar en IIS como entorno de prueba de esta serie.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>El formato de la cadena de inicialización no se ajusta a la especificación que empieza en el índice 0.

### <a name="scenario"></a>Escenario

Después de implementar una aplicación mediante la publicación con un solo clic, cuando se ejecuta una página que tiene acceso a la base de datos, aparece el siguiente mensaje de error:

El formato de la cadena de inicialización no se ajusta a la especificación que empieza en el índice 0.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Abra el archivo *Web. config* en el sitio implementado y compruebe si los valores de la cadena de conexión comienzan con `$(ReplaceableToken_`, como en el ejemplo siguiente:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Si las cadenas de conexión son similares a las de este ejemplo, edite el archivo de proyecto y agregue la siguiente propiedad al elemento PropertyGroup que es para todas las configuraciones de compilación:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

A continuación, vuelva a implementar la aplicación.

## <a name="http-500-internal-server-error"></a>Error interno del servidor HTTP 500

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el siguiente mensaje de error sin información específica que indica la causa del error:

Error HTTP 500: error interno del servidor.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Hay muchas causas de errores de 500, pero una posible causa si está siguiendo estos tutoriales es que coloca un elemento XML en el lugar equivocado en uno de los archivos de transformación Web. config. Por ejemplo, obtendrá este error si coloca la transformación que inserta una &lt;ubicación&gt; elemento en &lt;&gt; System. Web en lugar de hacerlo directamente en &lt;&gt;de configuración. Puede usar la característica de vista previa de transformación de Web. config para comprobar que las transformaciones funcionan según lo previsto. La solución si encuentra una transformación que se ha codificado incorrectamente es corregir el archivo de transformación y volver a implementarlo. Si un error no es obvio, intente comentar las transformaciones y volver a implementar para ver cuál es la causa del error 500.

## <a name="http-50021-internal-server-error"></a>Error interno del servidor HTTP 500,21

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el mensaje de error siguiente:

Error HTTP 500,21: error interno del servidor. El controlador "PageHandlerFactory-Integrated" tiene un módulo incorrecto "ManagedPipelineHandler" en su lista de módulos.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El sitio que ha implementado tiene como destino ASP.NET 4, pero ASP.NET 4 no está registrado en IIS en el servidor. En el servidor, abra un símbolo del sistema con privilegios elevados y registre ASP.NET 4 mediante la ejecución de los siguientes comandos:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Es posible que también tenga que establecer manualmente la versión de .NET Framework del grupo de aplicaciones predeterminado. Para obtener más información, vea el tutorial implementar en IIS como entorno de prueba de esta serie.

## <a name="login-failed-opening-sql-server-express-database-in-app_data"></a>Error de inicio de sesión al abrir SQL Server Express base de datos en datos de\_de la aplicación

### <a name="scenario"></a>Escenario

Ha actualizado la cadena de conexión del archivo *Web. config* para que apunte a una base de datos de SQL Server Express como archivo *. MDF* en la carpeta de datos de la *aplicación\_* y, la primera vez que ejecute la aplicación, verá el mensaje de error siguiente:

System. Data. SqlClient. SqlException: no se puede abrir la base de datos "DatabaseName" solicitada por el inicio de sesión. Error de inicio de sesión.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El nombre del archivo *. MDF* no puede coincidir con el nombre de ninguna base de datos SQL Server Express que haya existido en el equipo, aunque se haya eliminado el archivo *. MDF* de la base de datos existente. Cambie el nombre del archivo *. MDF* por un nombre que nunca se haya usado como nombre de la base de datos y cambie el archivo *Web. config* para que use el nuevo nombre. Como alternativa, puede usar [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar las bases de datos de SQL Server Express existentes previamente.

## <a name="model-compatibility-cannot-be-checked"></a>No se puede comprobar la compatibilidad de modelos

### <a name="scenario"></a>Escenario

Ha actualizado la cadena de conexión del archivo *Web. config* para que apunte a una nueva base de datos de SQL Server Express y, la primera vez que ejecute la aplicación, verá el mensaje de error siguiente:

No se puede comprobar la compatibilidad de modelos porque la base de datos no contiene metadatos de modelo. Asegúrese de que se ha agregado IncludeMetadataConvention a las convenciones de DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Si el nombre de la base de datos que coloca en el archivo Web. config se ha usado antes en el equipo, es posible que ya exista una base de datos con algunas tablas. Seleccione un nuevo nombre que no se haya usado en el equipo antes y cambie el archivo *Web. config* para que apunte a usar este nuevo nombre de base de datos. Como alternativa, puede usar [SQL Server Express utilidad](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) o [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) para eliminar la base de datos existente.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Error de SQL cuando un script intenta crear usuarios o roles

### <a name="scenario"></a>Escenario

Está usando la implementación de base de datos configurada en la pestaña **empaquetar/publicar SQL** , los scripts SQL que se ejecutan durante la implementación incluyen los comandos crear usuario o crear rol y la ejecución del script produce un error cuando se ejecutan los comandos. Es posible que vea mensajes más detallados, como los siguientes:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Si este error se produce cuando ha configurado la implementación de base de datos en el Asistente para **publicación web** en lugar de la pestaña **empaquetar/publicar SQL** , cree un subproceso en el foro de [configuración e implementación](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) y la solución se agregará a esta página de solución de problemas.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La cuenta de usuario que está usando para realizar la implementación no tiene permiso para crear usuarios o roles. Por ejemplo, la empresa de hospedaje podría asignar los roles dB\_DataReader, dB\_inwriter y DB\_ddladmin a la cuenta de usuario que configure. Son suficientes para crear la mayoría de los objetos de base de datos, pero no para crear usuarios o roles. Una manera de evitar el error es excluir los usuarios y los roles de la implementación de base de datos. Para ello, edite el elemento PreSource del script generado automáticamente de la base de datos para que incluya los siguientes atributos:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Para obtener información sobre cómo editar el elemento de código fuente en el archivo de proyecto, vea [Cómo: editar la configuración de implementación en el archivo de proyecto](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si los usuarios o los roles de la base de datos de desarrollo deben estar en la base de datos de destino, póngase en contacto con su proveedor de hospedaje para obtener ayuda.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Error de tiempo de espera de SQL Server al ejecutar scripts personalizados durante la implementación

### <a name="scenario"></a>Escenario

Ha especificado scripts SQL personalizados para ejecutarse durante la implementación y, cuando Web Deploy los ejecuta, se agota el tiempo de espera.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

La ejecución de varios scripts que tienen distintos modos de transacción puede provocar errores de tiempo de espera. De forma predeterminada, los scripts generados automáticamente se ejecutan en una transacción, pero no los scripts personalizados. Si selecciona la opción **extraer datos y/o esquema de una base de datos existente** en la pestaña **empaquetar/publicar SQL** , y si agrega un script SQL personalizado, debe cambiar la configuración de la transacción en algunos scripts para que todos los scripts usen la misma configuración de transacciones. Para obtener más información, vea [Cómo: implementar una base de datos con un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343.aspx).

Si ha configurado las opciones de transacción para que todas sean las mismas pero sigue apareciendo este error, una posible solución consiste en ejecutar los scripts por separado. En la cuadrícula **scripts de base de datos** de la pestaña **empaquetar/publicar** SQL, desactive la casilla **incluir** del script que provoca el error de tiempo de espera y, a continuación, publique el proyecto. A continuación, vuelva a la cuadrícula **scripts de base de datos** , active la casilla **incluir** del script y desactive las casillas **incluir** para los demás scripts. A continuación, publique el proyecto de nuevo. Esta vez, al publicar, solo se ejecuta el script personalizado seleccionado.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Los datos de la secuencia del manifiesto del sitio no están disponibles todavía

### <a name="scenario"></a>Escenario

Al instalar un paquete mediante el archivo *deploy. cmd* con la opción t (test), verá el mensaje de error siguiente:

Error: los datos de la secuencia de ' sitemanifest/dbFullSql [@path= ' C:\TEMP\AdventureWorksGrant.sql ']/sqlScript ' todavía no están disponibles.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El mensaje de error significa que el comando no puede generar un informe de prueba. Sin embargo, el comando puede ejecutarse si se usa la opción y (instalación real). El mensaje solo indica que hay un problema con la ejecución del comando en modo de prueba.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Esta aplicación requiere ManagedRuntimeVersion v 4.0

### <a name="scenario"></a>Escenario

Cuando intente realizar la implementación, verá el mensaje de error siguiente:

El grupo de aplicaciones que está intentando usar tiene la propiedad "managedRuntimeVersion" establecida en "v 2.0". Esta aplicación requiere ' v 4.0 '.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

ASP.NET 4 no está instalado en IIS. Si el servidor en el que va a realizar la implementación es el equipo de desarrollo y tiene instalado Visual Studio 2010, ASP.NET 4 está instalado en el equipo, pero es posible que no esté instalado en IIS. En el servidor en el que va a realizar la implementación, abra un símbolo del sistema con privilegios elevados e instale ASP.NET 4 en IIS mediante la ejecución de los siguientes comandos:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>No se puede convertir Microsoft. Web. Deployment. DeploymentProviderOptions

### <a name="scenario"></a>Escenario

Al implementar un paquete, aparece el mensaje de error siguiente:

No se puede convertir el objeto de tipo ' Microsoft. Web. Deployment. DeploymentProviderOptions ' a ' Microsoft. Web. Deployment. DeploymentProviderOptions '.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Está intentando implementar desde el administrador de IIS con la interfaz de usuario de Web Deploy 1,1 en un servidor que tiene Web Deploy 2,0 instalado. Si está utilizando la herramienta de administración remota de IIS para implementar importando un paquete, compruebe el cuadro de diálogo **nuevas características disponibles** al establecer la conexión. (Este cuadro de diálogo solo se puede mostrar una vez cuando se establece la conexión por primera vez. Para borrar la conexión y volver a iniciarla, cierre el administrador de IIS y vuelva a iniciarla; para ello, escriba inetmgr/RESET en el símbolo del sistema). Si una de las características enumeradas es **Web deploy interfaz de usuario**y tiene un número de versión inferior a 8, es posible que el servidor en el que se está implementando tenga instaladas las versiones 1,1 y 2,0 de web deploy. Para realizar la implementación desde un cliente que tiene instalado 2,0, el servidor debe tener instalado solo Web Deploy 2,0. Tendrá que ponerse en contacto con el proveedor de hospedaje para resolver este problema.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>No se pueden cargar los componentes nativos de SQL Server Compact

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado, verá el mensaje de error siguiente:

No se pueden cargar los componentes nativos de SQL Server Compact correspondientes al proveedor ADO.NET de la versión 8482. Instale la versión correcta de SQL Server Compact. Consulte el artículo 974247 de Knowledge base para obtener más detalles.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El sitio implementado no tiene subcarpetas *AMD64* y *x86* con los ensamblados nativos en la carpeta *bin* de la aplicación. En un equipo que tenga SQL Server Compact instalado, los ensamblados nativos se encuentran en *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private*. La mejor manera de obtener los archivos correctos en las carpetas correctas en un proyecto de Visual Studio es instalar el paquete Sqlservercom de NuGet. La instalación de paquetes agrega un script posterior a la compilación para copiar los ensamblados nativos en *AMD64* y *x86*. Sin embargo, para que se implementen, debe incluirlas manualmente en el proyecto. Para obtener más información, consulte el tutorial [implementación de SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Error "la ruta de acceso no es válida" después de implementar una aplicación de Code First de Entity Framework

### <a name="scenario"></a>Escenario

Implementa una aplicación que usa Migraciones de Entity Framework Code First y un DBMS como SQL Server Compact que almacena su base de datos en un archivo de la carpeta app\_Data. Ha configurado Migraciones de Code First para crear la base de datos después de la primera implementación. Al ejecutar la aplicación, se obtiene un mensaje de error similar al siguiente ejemplo:

La ruta de acceso no es válida. Compruebe el directorio de la base de datos. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Code First está intentando crear la base de datos, pero no existe la carpeta de datos de la\_de la aplicación. O bien no tiene ningún archivo en la carpeta de datos de la\_de la *aplicación* cuando implementó, o bien seleccionó **excluir los datos de\_** de la aplicación en la pestaña **empaquetar/publicar web** del ventana Propiedades del proyecto. El proceso de implementación no creará una carpeta en el servidor si no hay ningún archivo en la carpeta que se va a copiar en el servidor. Si ya tiene la base de datos configurada en el sitio, el proceso de implementación eliminará los archivos y la *aplicación\_* carpeta de datos en sí misma si ha seleccionado **quitar archivos adicionales en el destino** en el perfil de publicación. Para solucionar el problema, coloque un archivo de marcador de posición como un archivo. txt en la carpeta de datos de la\_de la *aplicación* , asegúrese de que no tiene la opción **excluir datos de\_** de la aplicación seleccionada y vuelva a implementarla.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"El objeto COM que se ha separado de su RCW subyacente no se puede usar".

### <a name="scenario"></a>Escenario

Ha usado correctamente la publicación con un solo clic para implementar la aplicación y, a continuación, comienza a obtener este error:

Error en la tarea de implementación web. (No se pudo completar la solicitud a la dirección URL del agente remoto '<https://serverurl.com/msdeploy.axd?site=sitename>').  
 No se pudo completar la solicitud a la dirección URL del agente remoto '<https://url/msdeploy.axd?site=sitename>'.  
Se anuló la solicitud: se canceló la solicitud.  
No se puede usar el objeto COM que se ha separado de su RCW subyacente.

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

De forma predeterminada, Visual Studio establece permisos de lectura en la carpeta raíz del sitio y permisos de escritura en la carpeta de datos de la\_de la aplicación. Si la aplicación necesita acceso de escritura a una subcarpeta, puede establecer permisos para esa carpeta, tal como se muestra en los tutoriales de configuración de permisos de carpeta e implementación en el entorno de producción de esta serie. Si la aplicación necesita acceso de escritura a la carpeta raíz del sitio, tendrá que impedir que se configure el acceso de solo lectura en la carpeta raíz agregando **&lt;IncludeSetACLProviderOn destino&gt;falso&lt;/IncludeSetACLProviderOnDestination&gt;** al archivo de Perfil de publicación (para que afecte a un solo perfil) o al archivo WPP. targets (para que afecte a todos los perfiles). Para obtener información sobre cómo editar estos archivos, vea [Cómo: editar la configuración de implementación en archivos de perfil (. pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Error de configuración: el atributo targetFramework hace referencia a una versión posterior a la versión instalada de la .NET Framework

### <a name="scenario"></a>Escenario

Ha publicado correctamente un proyecto web destinado a ASP.NET 4,5, pero al ejecutar la aplicación (con el modo customErrors establecido en "OFF" en el archivo Web. config) recibe el siguiente error:

El atributo ' targetFramework ' del elemento &lt;Compilation&gt; del archivo Web. config solo se usa para la versión 4,0 y posterior de la .NET Framework (por ejemplo, '&lt;la compilación targetFramework = "4.0"&gt;'). El atributo ' targetFramework ' hace referencia actualmente a una versión posterior a la versión instalada de la .NET Framework. Especifique una versión de destino válida de la .NET Framework o instale la versión necesaria de la .NET Framework.

En el cuadro error de origen de la página de error se resalta la siguiente línea de Web. config como causa del error:

&lt;compilación targetFramework = "4.5"/&gt;

### <a name="possible-cause-and-solution"></a>Causa posible y solución

El servidor no es compatible con ASP.NET 4,5. Póngase en contacto con el proveedor de hospedaje para determinar si se puede Agregar compatibilidad con ASP.NET 4,5. Si la actualización del servidor no es una opción, tiene que implementar un proyecto web destinado a ASP.NET 4 o una versión anterior en su lugar.

Si implementa un proyecto Web de ASP.NET 4 o una versión anterior en el mismo destino, active la casilla **quitar archivos adicionales en el destino** en la pestaña **configuración** del Asistente para **publicación web** . Si no selecciona **quitar archivos adicionales en el destino**, seguirá recibiendo la página de error de configuración.

La ventana **propiedades** del proyecto incluye una lista desplegable plataforma de destino, pero no puede resolver este problema simplemente cambiando de **.NET Framework 4,5** a **.NET Framework 4**. Si cambia la plataforma de destino a una versión anterior de .NET Framework, el proyecto seguirá teniendo referencias a los ensamblados de la versión de .NET Framework posterior y no se ejecutará. Tendrá que cambiar manualmente esas referencias o crear un nuevo proyecto que tenga como destino .NET Framework 4 o una versión anterior. Para obtener más información, consulte [.NET Framework destino para sitios web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Errores de confianza media

### <a name="scenario"></a>Escenario

Al ejecutar la aplicación en producción, se obtiene un error relacionado con el nivel de confianza medio.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Muchos proveedores de hospedaje de terceros ejecutan su sitio web en confianza media, lo que significa que hay algunas cosas que no se permiten. Por ejemplo, el código de aplicación no puede tener acceso al registro de Windows y no puede leer ni escribir archivos que estén fuera de la jerarquía de carpetas de la aplicación. De forma predeterminada, la aplicación se ejecuta en *plena confianza* en el equipo local, lo que significa que es posible que la aplicación pueda hacer cosas en las que se produciría un error cuando se implemente en producción.

Puede configurar la aplicación para que se ejecute en un nivel de confianza medio en el entorno de IIS local con el fin de solucionar los problemas. Para ello, abra el archivo *Web. config* de la aplicación y agregue un elemento de **confianza** en el elemento **System. Web** , tal como se muestra en este ejemplo.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

La aplicación se ejecutará ahora en el nivel de confianza medio en IIS, incluso en el equipo local.

No lo haga si va a implementar en Azure App Service, porque Azure no requiere confianza media. En el momento en que se escribe este tutorial en febrero de 2012, el uso de este método para que la aplicación se ejecute con confianza media producirá un error en Azure.

Si usa Migraciones de Entity Framework Code First y está implementando en un proveedor de hospedaje que ejecuta su aplicación en confianza media, asegúrese de que tiene instalada la versión 5,0 o posterior. En Entity Framework versión 4,3, las migraciones requieren plena confianza para poder actualizar el esquema de la base de datos.

## <a name="http-40417-not-found-error"></a>Error HTTP 404,17 no encontrado

### <a name="scenario"></a>Escenario

Al ejecutar el sitio implementado en el equipo de desarrollo en IIS, verá el siguiente mensaje de error que indica que el servidor no puede procesar default. aspx:

Error HTTP 404,17-no encontrado

El contenido solicitado parece ser un script y el controlador de archivos estáticos no lo atenderá.

### <a name="possible-cause-and-solution"></a>Causa posible y solución

Es posible que ASP.NET 4,5 no esté instalado en el equipo. Vea los pasos del tutorial implementación de IIS como entorno de prueba de esta serie que explican cómo instalar ASP.NET 4,5.

> [!div class="step-by-step"]
> [Anterior](deploying-extra-files.md)
