---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Implementación web de ASP.NET con Visual Studio: Implementando en test | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: c45003325832258466a787bc589bf40e844248a2
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985856"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implementación web de ASP.NET con Visual Studio: Implementación de prueba

por [Tom Dykstra](https://github.com/tdykstra)

En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros con Visual Studio 2017. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

Para obtener una versión actual de la implementación en Azure, consulte [creación de una aplicación Web de ASP.net Core en Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Información general

En este tutorial, implementará una aplicación Web de ASP.NET en Internet Information Server (IIS) en el equipo local.

Generalmente, cuando se desarrolla una aplicación, se ejecuta y se prueba en Visual Studio. De forma predeterminada, los proyectos de aplicación Web de Visual Studio 2017 usan IIS Express como servidor Web de desarrollo. IIS Express se comporta más como IIS completo que el Servidor de desarrollo de Visual Studio (también conocido como Cassini), que Visual Studio 2017 usa de forma predeterminada. Sin embargo, ninguno de los servidores Web de desarrollo funciona exactamente igual que IIS. Por lo tanto, una aplicación podría ejecutarse y probarse correctamente en Visual Studio, pero producirá un error cuando se implemente en IIS.

Puede probar la aplicación de forma confiable de dos maneras:

1. Implemente la aplicación en IIS en el equipo de desarrollo mediante el mismo proceso que usará más adelante para implementarla en el entorno de producción.

   Puede configurar Visual Studio para que use IIS al ejecutar un proyecto Web, pero que no probará el proceso de implementación. Este método valida el proceso de implementación y la aplicación se ejecuta correctamente en IIS.

2. Implemente su aplicación en un entorno de prueba similar al del entorno de producción. 
 
   El entorno de producción para estos tutoriales se Web Apps en Azure App Service. El entorno de prueba ideal es una aplicación web adicional creada en el servicio de Azure. Aunque se configuraría de la misma manera que una aplicación Web de producción, solo se usaría para realizar pruebas.

La opción 2 es la manera más confiable de probar. Si usa la opción 2, no es necesario usar la opción 1. Sin embargo, si va a realizar la implementación en un proveedor de hospedaje de terceros, la opción 2 podría no ser factible o podría ser costosa, por lo que esta serie de tutoriales muestra ambos métodos. En el tutorial [implementación del entorno de producción](deploying-to-production.md) se proporciona orientación para la opción 2.

Para obtener más información sobre el uso de servidores Web en Visual Studio, vea [servidores Web en Visual Studio para proyectos Web de ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Avisos Si recibe un mensaje de error o algo no funciona a medida que avanza por el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Descargar el proyecto de inicio de Contoso University

Descargue e instale el proyecto y la solución de inicio de Visual Studio para contoso University. Esta solución contiene el tutorial completado. 

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Instalar IIS

Para implementar en IIS en el equipo de desarrollo, confirme que IIS y Web Deploy están instalados. De forma predeterminada, Visual Studio instala Web Deploy, pero IIS no se incluye en la configuración predeterminada de Windows 10, Windows 8 o Windows 7. Si ya ha instalado IIS y el grupo de aplicaciones predeterminado ya está establecido en .NET 4, vaya a [la sección siguiente](#sqlexpress).

1. Se recomienda usar el instalador de [plataforma web (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) para instalar IIS y Web deploy. WPI instala una configuración de IIS recomendada que incluye IIS y Web Deploy requisitos previos si es necesario.

   Si ya ha instalado IIS, Web Deploy o cualquiera de sus componentes necesarios, el WPI solo instala lo que falta.

   * Use el instalador de plataforma web para instalar IIS y Web Deploy:
    
     ![Instalar IIS mediante WPI](deploying-to-iis/_static/image30.png)

     ![Instalación de Web Deploy mediante WPI](deploying-to-iis/_static/image31.png)

     Verá mensajes que indican que se instalará IIS 7. El vínculo funciona para IIS 8 en Windows 8. pero para Windows 8 y versiones posteriores, siga los pasos siguientes para asegurarse de que ASP.NET 4,7 está instalado:

   * Abra **Panel** > de control**programas** > **programas y características** > **activar o desactivar las características de Windows**.

   * Expanda **Internet Information Services**, **World Wide Web Servicios**y **características de desarrollo de aplicaciones**.
   
   * Confirme que **ASP.NET 4,7** está seleccionado.

     ![Seleccione ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Confirme que se ha seleccionado **World Wide Web Services** y la **consola de administración de IIS** . Esto instala IIS y el administrador de IIS.
    
     ![Seleccionar servicios de World Wide Web](deploying-to-iis/_static/image24.png)    
  
   * Seleccione **Aceptar**. Aparecen mensajes de cuadro de diálogo que indican que se está produciendo una instalación.

Después de instalar IIS, ejecute el **Administrador de IIS** para asegurarse de que la .NET Framework versión 4 está asignada al grupo de aplicaciones predeterminado.

1. Presione WINDOWS + R para abrir el cuadro de diálogo **Ejecutar** .

   (En Windows 8 o posterior, escriba "ejecutar" en la página de **Inicio** . En Windows 7, seleccione **Ejecutar** en el menú **Inicio** . Si **Ejecutar** no está en el menú **Inicio** , haga clic con el botón secundario en la barra de tareas, seleccione **propiedades**, seleccione la pestaña **menú Inicio** , seleccione **personalizar**y seleccione **Ejecutar comando**.

2. Escriba "inetmgr" y seleccione **Aceptar**.

3. En el panel **conexiones** , expanda el nodo de servidor y seleccione **grupos de aplicaciones**. En el panel **grupos de aplicaciones** , si **DefaultAppPool** está asignado a la versión 4 de .NET Framework, como en la siguiente ilustración, vaya a la sección siguiente.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Si solo ve dos grupos de aplicaciones y ambos están establecidos en .NET Framework 2,0, instale ASP.NET 4 en IIS.

   Para Windows 8 o versiones posteriores, consulte las instrucciones de la sección anterior para asegurarse de que ASP.NET 4,7 está instalado o consulte [instalación de ASP.NET 4,5 en Windows 8 y Windows Server 2012](https://support.microsoft.com/kb/2736284). Para Windows 7, abra una ventana del símbolo del sistema; para ello, haga clic con el botón secundario en **símbolo del sistema** en el menú **Inicio** de Windows y seleccione **Ejecutar como administrador**. Ejecute [ASPNET\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar ASP.net 4 en IIS mediante los siguientes comandos. (En los sistemas de 32 bits, reemplace "Framework64" por "Framework").

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Este comando crea nuevos grupos de aplicaciones para el .NET Framework 4, pero el grupo de aplicaciones predeterminado permanecerá establecido en 2,0. Está implementando una aplicación destinada a .NET 4 a ese grupo de aplicaciones, por lo que debe cambiar el grupo de aplicaciones a .NET 4.

5. Si ha cerrado el **Administrador de IIS**, ejecútelo de nuevo, expanda el nodo de servidor y seleccione grupos de **aplicaciones**.

6. En el panel **grupos de aplicaciones** , seleccione **DefaultAppPool**. En el panel **acciones** , seleccione **configuración básica**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. En el cuadro de diálogo **modificar grupo de aplicaciones** , cambie la **versión de .net CLR** a **.net CLR v 4.0.30319**. Seleccione **Aceptar**.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Ahora está listo para publicar una aplicación web en IIS. Sin embargo, en primer lugar, cree bases de datos para pruebas.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instalar SQL Server Express

LocalDB no está diseñado para funcionar en IIS, por lo que el entorno de prueba debe tener SQL Server Express instalado. Si utiliza Visual Studio 2010 SQL Server Express, ya está instalado de forma predeterminada. Si utiliza Visual Studio 2012 o una versión posterior, instale SQL Server Express.

Para instalar SQL Server Express, descárguelo e instálelo desde [el centro de descarga: Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

En la primera página del centro de instalación de SQL Server, seleccione **nuevo SQL Server instalación independiente o agregue características a una instalación existente** , y siga las instrucciones que acepten las opciones predeterminadas. En el Asistente para la instalación, acepte la configuración predeterminada. Para obtener más información acerca de las opciones de instalación, consulte [instalar SQL Server desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Crear bases de datos de SQL Server Express para el entorno de prueba

La aplicación contoso University tiene dos bases de datos: 

1. Base de datos de pertenencia 
2. Base de datos de aplicación 

Puede implementar estas bases de datos en dos bases de datos independientes o en una sola. La combinación de ellos facilita la combinación de bases de datos entre ellos. 

Si va a realizar la implementación en un proveedor de hospedaje de terceros, es posible que el plan de hospedaje también le dé un motivo para combinarlos. Por ejemplo, el proveedor podría cobrar más por varias bases de datos o incluso no permitir más de una base de datos.

En este tutorial, implementará dos bases de datos en el entorno de prueba y en una base de datos en los entornos de ensayo y producción.

En el menú **Ver** de Visual Studio, seleccione **Explorador de servidores** (**Explorador de bases de datos** en Visual Web Developer). Haga clic con el botón secundario en **conexiones de datos** y seleccione **crear nueva SQL Server base**de datos.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

En el cuadro de diálogo **crear nueva SQL Server base de datos** , escriba ".\SQLExpress" en el cuadro **nombre del servidor** y "ASPNET-ContosoUniversity" en el cuadro Nombre de la **nueva base de datos** . Seleccione **Aceptar**.

![Crear ASPNET-ContosoUniversity](deploying-to-iis/_static/image9.png)

Siga el mismo procedimiento para crear una nueva base de datos de `ContosoUniversity`SQL Server Express School denominada.

**Explorador de servidores** muestra las dos bases de datos nuevas.

![Bases de datos nuevas en Explorador de servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Crear un script de concesión para las nuevas bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación usa las credenciales del grupo de aplicaciones predeterminado para tener acceso a la base de datos. Sin embargo, de forma predeterminada, el grupo de aplicaciones no tiene permiso para abrir las bases de datos. Esto significa que debe ejecutar un script para conceder ese permiso. En esta sección, creará ese script y lo ejecutará más adelante para asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

En un editor de texto, copie los siguientes comandos SQL en un archivo nuevo y guárdelo como *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

En Visual Studio, abra la solución contoso University. Haga clic con el botón secundario en la solución (no en uno de los proyectos) y seleccione **Agregar**. Seleccione **elemento existente**, vaya a *Grant. SQL*y ábralo.

> [!NOTE]
> Este script está diseñado para funcionar con SQL Server Express 2012 o posterior y con la configuración de IIS en Windows 10, Windows 8 o Windows 7, ya que se especifican en este tutorial. Si usa una versión diferente de SQL Server o Windows, o si configura IIS en el equipo de forma diferente, es posible que se necesiten cambios en este script. Para obtener más información sobre los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Nota de seguridad** Este script concede `db_owner` permisos al usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, es posible que desee especificar un usuario que tenga permisos de actualización de esquemas de base de datos completos solo para la implementación y especificar en tiempo de ejecución un usuario diferente que tenga permisos únicamente para leer y escribir datos. Para obtener más información, vea [revisar los cambios automáticos de Web. config para migraciones de Code First](#reviewingmigrations) más adelante en este tutorial.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Ejecutar el script Grant en la base de datos de la aplicación

Puede configurar el perfil de publicación para ejecutar el script Grant en la base de datos de pertenencia durante la implementación, ya que la implementación de base de datos usa el proveedor [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) . No puede ejecutar scripts durante la implementación de Migraciones de Code First, que es cómo va a implementar la base de datos de la aplicación. Esto significa que tiene que ejecutar manualmente el script antes de la implementación en la base de datos de la aplicación.

1. En Visual Studio, abra el archivo *Grant. SQL* que creó anteriormente.

2. Seleccione **Conectar**. 

    ![Botón conectar](deploying-to-iis/_static/image11.png)

3. En el cuadro de diálogo **conectar con el servidor** , escriba *.\SQLExpress* como el **nombre del servidor**. Seleccione **Conectar**.

4. En la lista desplegable base de datos, seleccione **ContosoUniversity**. Seleccione **Ejecutar**. 

   ![](deploying-to-iis/_static/image12.png)

La identidad del grupo de aplicaciones predeterminado ahora tiene los permisos suficientes en la base de datos de la aplicación para Migraciones de Code First crear las tablas de base de datos cuando se ejecuta la aplicación.

## <a name="publish-to-iis"></a>Publicación en IIS

Hay varias maneras de implementar en IIS con Visual Studio y Web Deploy:

* Usar publicación con un solo clic de Visual Studio.
* Publique desde la línea de comandos.
* Cree un paquete de implementación e instálelo mediante el administrador de IIS. El paquete tiene un archivo. zip con todos los archivos y metadatos necesarios para instalar un sitio en IIS.
* Cree un paquete de implementación e instálelo mediante la línea de comandos.

El proceso que realizó en los tutoriales anteriores para configurar Visual Studio para automatizar las tareas de implementación se aplica a todos estos métodos. En estos tutoriales, usará los dos primeros métodos. Para obtener información sobre el uso de paquetes de implementación, consulte [implementación de una aplicación Web mediante la creación e instalación de un paquete de implementación web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) en la asignación de contenido de implementación web para Visual Studio y ASP.net.

Antes de publicar, asegúrese de que está ejecutando Visual Studio en modo de administrador. Si no ve **(Administrador)** en la barra de título, cierre Visual Studio. En la página de **Inicio** de Windows 8 (o posterior) o en el menú **Inicio** de Windows 7, haga clic con el botón derecho en el icono de Visual Studio y seleccione **Ejecutar como administrador**. El modo de administrador solo es necesario para la publicación al publicar en IIS en el equipo local.

### <a name="create-the-publish-profile"></a>Crear el perfil de publicación

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto **ContosoUniversity** (no en el proyecto **ContosoUniversity. Dal** ). Seleccione **Publicar**. Aparece la página **publicar** .

2. Seleccione **nuevo perfil**. Aparece el cuadro de diálogo **seleccionar un destino de publicación** .

3. Seleccione **IIS, FTP, etc**. Seleccione **Crear perfil**. Aparece el Asistente para **publicación** .

   ![Pestaña perfil del Asistente para publicación Web](deploying-to-iis/_static/image26.png)

4. En el menú desplegable **método de publicación** , seleccione **Web deploy**.

5. En **servidor**, escriba *localhost*.

6. En **nombre del sitio**, escriba *sitio web predeterminado/ContosoUniversity*.

7. En **dirección URL**de destino *http://localhost/ContosoUniversity* , escriba.

   La configuración de la **dirección URL de destino** no es obligatoria. Cuando Visual Studio finaliza la implementación de la aplicación, abre automáticamente el explorador predeterminado en esta dirección URL. Si no desea que el explorador se abra automáticamente después de la implementación, deje este cuadro en blanco.

8. Seleccione **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

   Una marca de verificación verde comprueba que la conexión se ha realizado correctamente.

   ![Pestaña conexión del Asistente para publicación Web](deploying-to-iis/_static/image27.png)

9. Seleccione **siguiente** para avanzar a la pestaña **configuración** .

10. El cuadro desplegable **configuración** especifica la configuración de compilación que se va a implementar. Déjelo establecido en el valor predeterminado de **Release**. No va a implementar compilaciones de depuración en este tutorial.

11. Expanda **Opciones de publicación de archivo**. Seleccione **excluir archivos en la\_carpeta de datos de la aplicación**.

    En el entorno de prueba, la aplicación tiene acceso a las bases de datos creadas en la instancia de SQL Server Express local, no a los archivos. MDF de la carpeta de datos de la *aplicación\_* .

12. Deje desactivadas las casillas de verificación **precompilar durante la publicación** y **quitar archivos adicionales en el destino** .

    ![Opciones de publicación de archivos en la pestaña Configuración](deploying-to-iis/_static/image15a.png)

    La precompilación es una opción útil principalmente para sitios grandes. Puede reducir el tiempo de inicio la primera vez que se solicita una página después de publicar el sitio.

    No es necesario quitar archivos adicionales, ya que se trata de la primera implementación y aún no habrá ningún archivo en la carpeta de destino.

    > [!NOTE] 
    > Si selecciona **quitar archivos adicionales en el destino** para una implementación posterior en el mismo sitio, asegúrese de que usa la característica de vista previa para ver de antemano qué archivos se eliminarán antes de la implementación. El comportamiento esperado es que Web Deploy eliminará los archivos del servidor de destino que ha eliminado en el proyecto. Sin embargo, se compara toda la estructura de carpetas de las carpetas de origen y de destino; en algunos escenarios, Web Deploy podrían eliminar archivos que no desea eliminar.
    > 
    > Por ejemplo, si tiene una aplicación web en una subcarpeta del servidor al implementar un proyecto en la carpeta raíz, se eliminará la subcarpeta. Es posible que tenga un proyecto para el sitio principal en contoso.com y otro proyecto para un blog en contoso.com/blog. La aplicación de blog está en una subcarpeta. Si selecciona **quitar archivos adicionales en el destino** al implementar el sitio principal, se eliminará la aplicación de blog.
    > 
    > En otro ejemplo, la carpeta\_de datos de la aplicación podría eliminarse de forma inesperada. Algunas bases de datos, como SQL Server Compact almacenan archivos de base de\_datos en la carpeta de datos de la aplicación. Después de la implementación inicial, no desea seguir copiando los archivos de base de datos en las implementaciones posteriores, por lo que selecciona **excluir datos\_** de la aplicación en la pestaña empaquetar/publicar web. Una vez que haya **quitado los archivos adicionales en el destino** seleccionados, los archivos de base de datos\_y la carpeta de datos de la aplicación se eliminarán la próxima vez que publique.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar la implementación para la base de datos de pertenencia

Los pasos siguientes se aplican a la base de datos **DefaultConnection** en la sección **Databases** del cuadro de diálogo.

1. En el cuadro **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunte a la nueva base de datos de pertenencia a SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   El proceso de implementación coloca esta cadena de conexión en el archivo Web. config implementado porque está seleccionada **la opción usar esta cadena de conexión en tiempo de ejecución** .

    También puede obtener la cadena de conexión de **Explorador de servidores**. En **Explorador de servidores**, expanda **conexiones de datos** y seleccione la  **&lt;&gt;** base de datos MachineName \SQLEXPRESS.Aspnet-ContosoUniversity y, a continuación, en la ventana **propiedades** , copie la **cadena de conexión.** valor. Esa cadena de conexión tendrá una configuración adicional que se puede eliminar: `Pooling=False`.

2. Seleccione **Actualizar base de datos**.

   Esto hace que el esquema de la base de datos se cree en la base de datos de destino durante la implementación. En los pasos siguientes, se especifican los scripts adicionales que se deben ejecutar: uno para conceder el acceso a la base de datos al grupo de aplicaciones predeterminado y otro para implementar los datos.

3. Seleccione **configurar actualizaciones de base de datos**.
 
4. En el cuadro de diálogo **configurar actualizaciones de base de datos** , seleccione **Agregar script SQL**. Navegue hasta el script *Grant. SQL* que guardó anteriormente en la carpeta de la solución.

5. Repita el proceso para agregar el script *ASPNET-Data-dev. SQL* .

   ![Configurar actualizaciones de base de datos para la base de datos de suscripción](deploying-to-iis/_static/image16.png)

6. Seleccione **Cerrar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar la implementación para la base de datos de aplicación

Cuando Visual Studio detecta una clase Entity Framework `DbContext` , crea una entrada en la sección **bases** de datos que tiene una casilla **Ejecutar migraciones de Code First** en lugar de una **actualización de base de datos** . En este tutorial, usará esa casilla para especificar Migraciones de Code First implementación.

En algunos escenarios, es posible que esté usando `DbContext` una base de datos pero desee usar el proveedor dbDacFx en lugar de las migraciones para implementar la base de datos. En ese caso, vea [cómo implementar una base de datos de Code First sin migraciones](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) . en las p + f de ASP.net web Deployment en MSDN.

Los pasos siguientes se aplican a la base de datos **SchoolContext** en la sección **Databases** del cuadro de diálogo.

1. En el cuadro **cadena de conexión remota** , escriba la siguiente cadena de conexión que apunte a la nueva base de datos de la aplicación SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   El proceso de implementación coloca esta cadena de conexión en el archivo Web. config implementado porque está seleccionada **la opción usar esta cadena de conexión en tiempo de ejecución** .

   También puede obtener la cadena de conexión de la base de datos de la aplicación de **Explorador de servidores** de la misma manera que la cadena de conexión de la base de datos de pertenencia.

2. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** .

   Esta opción hace que el proceso de implementación configure el archivo Web. config implementado `MigrateDatabaseToLatestVersion` para especificar el inicializador. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación obtiene acceso a la base de datos por primera vez después de la implementación.

### <a name="configure-publish-profile-transforms"></a>Configurar transformaciones de Perfil de publicación

1. Seleccione **Cerrar**. Seleccione **sí** cuando se le pregunte si desea guardar los cambios.

2. En **Explorador de soluciones**, expanda **propiedades**y expanda **PublishProfiles**.

3. Haga clic con el botón secundario en *CustomProfile. pubxml* y cambie su nombre a *Test. pubxml*.

4. Haga clic con el botón secundario en *Test. pubxml*. Seleccione **Agregar transformación de configuración**.

   ![Menú Agregar transformación de configuración](deploying-to-iis/_static/image17.png)

   Visual Studio crea el archivo de transformación *Web. test. config* y lo abre.

5. En el archivo de transformación *Web. test. config* , inserte el siguiente código inmediatamente después de la etiqueta de configuración de apertura.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Cuando se usa el perfil de publicación de prueba, esta transformación establece el indicador de entorno en "prueba". En el sitio implementado, verá "(test)" después del encabezado H1 "Contoso University".

6. Guarde y cierre el archivo.

7. Haga clic con el botón secundario en el archivo *Web. test. config* y seleccione **vista previa de transformación** para asegurarse de que la transformación codificada genera los cambios esperados.

    En la ventana de **vista previa de Web. config** se muestra el resultado de aplicar las transformaciones *Web. Release. config* y las transformaciones *Web. test. config* .

### <a name="preview-the-deployment-updates"></a>Obtener una vista previa de las actualizaciones de implementación

1. Vuelva a abrir el Asistente para **publicación web** (haga clic con el botón derecho en el proyecto ContosoUniversity, seleccione **publicar**y, después, **vista previa**).

2. En el cuadro de diálogo **vista previa** , seleccione **iniciar vista previa** para ver una lista de los archivos que se copiarán. 

   ![Vista previa de publicación](deploying-to-iis/_static/image29.png)

   También puede seleccionar el vínculo de **base de datos de vista previa** para ver los scripts que se ejecutarán en la base de datos de pertenencia. (No se ejecutan scripts para la implementación de Migraciones de Code First, por lo que no hay nada que ver en la base de datos de la aplicación).

3. Seleccione **Publicar**.

   Si Visual Studio no está en modo de administrador, es posible que obtenga un mensaje de error de permisos. En ese caso, cierre Visual Studio, ábralo en modo de administrador e intente publicar de nuevo.

   Si Visual Studio está en modo de administrador, la ventana de **salida** informa de que la compilación se ha realizado correctamente y se ha publicado.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Si especificó la dirección URL en el cuadro **dirección URL de destino** de la pestaña publicar Perfil de **conexión** , el explorador se abrirá automáticamente en la Página principal de Contoso University que se ejecuta en IIS en el equipo.

## <a name="test-in-the-test-environment"></a>Prueba en el entorno de prueba

Observe que el indicador de entorno muestra "(test)" en lugar de "(dev)," que muestra que la transformación de *Web. config* para el indicador de entorno se realizó correctamente.

Ejecute la página **instructores** para comprobar que Code First ha inicializado la base de datos con datos del instructor. Al seleccionar esta página, la carga puede tardar unos minutos, ya que Code First crea la base de datos y, `Seed` a continuación, ejecuta el método. (No lo hacía cuando estaba en la Página principal porque la aplicación no ha intentado tener acceso a la base de datos todavía).

Seleccione la pestaña **estudiantes** para comprobar que la base de datos implementada no tiene estudiantes.

Seleccione **Agregar alumnos** en el menú **estudiantes** . Agregue un estudiante y, a continuación, vea el nuevo estudiante en la página **Students** . Esto comprueba que se puede escribir correctamente en la base de datos.

En el menú de **cursos** , seleccione **Actualizar créditos**. La página **Actualizar créditos** requiere permisos de administrador, por lo que se muestra la página **de inicio de sesión** . Escriba las credenciales de la cuenta de administrador que creó anteriormente ("admin" y "devpwd"). Se muestra la página **Actualizar créditos** . Esto comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Compruebe que existe una carpeta *ELMAH* en la carpeta *c:\inetpub\wwwroot\ContosoUniversity* que solo contiene el archivo de marcador de posición.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar los cambios automáticos de Web. config para Migraciones de Code First

Abra el archivo *Web. config* en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y puede ver dónde se configuró el proceso de implementación migraciones de Code First para actualizar automáticamente la base de datos a la versión más reciente.

![](deploying-to-iis/_static/image21.png)

El proceso de implementación también ha creado una nueva cadena de conexión para que Migraciones de Code First usar exclusivamente para actualizar el esquema de la base de datos:

![Cadena de conexión de Database_Publish](deploying-to-iis/_static/image22.png)

Esta cadena de conexión adicional le permite especificar una cuenta de usuario para las actualizaciones del esquema de la base de datos y una cuenta de usuario diferente para el acceso a los datos de la aplicación. Por ejemplo, puede asignar el rol **propietario\_** de la base de los migraciones de Code First y **dB\_DataReader** con los roles de autor de la **BD\_** a la aplicación. Se trata de un patrón común de defensa en profundidad que evita que un código potencialmente malintencionado en la aplicación cambie el esquema de la base de datos. (Por ejemplo, esto puede ocurrir en un ataque de inyección de SQL correcto). Estos tutoriales no usan este patrón. Para implementar este patrón en el escenario, siga estos pasos:

1. En el Asistente para **publicación web** , en la pestaña **configuración** , escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquemas de base de datos completa. Desactive la casilla **usar esta cadena de conexión en tiempo de ejecución** . En el archivo Web. config implementado, se convierte `DatabasePublish` en la cadena de conexión.

2. Cree una transformación de archivo Web. config para la cadena de conexión que desea que la aplicación use en tiempo de ejecución.

## <a name="summary"></a>Resumen

Ahora ha implementado la aplicación en IIS en el equipo de desarrollo y la ha probado allí.

![Página principal de prueba](deploying-to-iis/_static/image23.png)

Esto comprueba que el proceso de implementación copió el contenido de la aplicación en la ubicación correcta (excluyendo los archivos que no desea implementar) y que Web Deploy configurar IIS correctamente durante la implementación. En el siguiente tutorial, ejecutará una prueba más que encuentre una tarea de implementación que todavía no se ha realizado: establecimiento de permisos de carpeta en la carpeta *Elm ah* .

## <a name="more-information"></a>Más información

Para obtener información sobre cómo ejecutar IIS o IIS Express en Visual Studio, vea los siguientes recursos:

- [IIS Express información general](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sobre el sitio de IIS.net.
- [Presentación de IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Scott Guthrie.
- [Servidores Web en Visual Studio para proyectos Web de ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Diferencias principales entre IIS y el servidor de desarrollo de ASP.net](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio de ASP.net.

Para obtener información sobre los problemas que pueden surgir cuando la aplicación se ejecuta en un nivel de confianza medio, consulte [hospedaje de aplicaciones de ASP.net en confianza media](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los cuatro chicos del sitio de Rolla.

> [!div class="step-by-step"]
> [Anterior](project-properties.md)
> [Siguiente](setting-folder-permissions.md)
