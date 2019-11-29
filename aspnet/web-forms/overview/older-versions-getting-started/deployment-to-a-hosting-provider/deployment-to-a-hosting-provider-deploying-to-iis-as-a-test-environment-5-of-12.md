---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en IIS como entorno de prueba-5 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633480"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar en IIS como entorno de prueba (5 de 12)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general del

En este tutorial se muestra cómo implementar una aplicación Web de ASP.NET en IIS en el equipo local.

Al desarrollar una aplicación, normalmente se prueba mediante su ejecución en Visual Studio. De forma predeterminada, esto significa que está usando el Servidor de desarrollo de Visual Studio (también conocido como Cassini). El Servidor de desarrollo de Visual Studio facilita la prueba durante el desarrollo en Visual Studio, pero no funciona exactamente igual que IIS. Como resultado, es posible que una aplicación se ejecute correctamente al probarla en Visual Studio, pero que se produzca un error cuando se implemente en IIS en un entorno de hospedaje.

Puede probar la aplicación más confiablemente de estas maneras:

1. Use IIS Express o IIS completo en lugar del Servidor de desarrollo de Visual Studio al probar en Visual Studio durante el desarrollo. Por lo general, este método emula de manera más precisa cómo se ejecutará el sitio en IIS. Sin embargo, este método no prueba el proceso de implementación ni valida que el resultado del proceso de implementación se ejecute correctamente.
2. Implemente la aplicación en IIS en el equipo de desarrollo mediante el mismo proceso que usará más adelante para implementarla en el entorno de producción. Este método valida el proceso de implementación, además de validar que la aplicación se ejecutará correctamente en IIS.
3. Implemente la aplicación en un entorno de prueba lo más cercano posible a su entorno de producción. Puesto que el entorno de producción para estos tutoriales es un proveedor de hospedaje de terceros, el entorno de prueba ideal sería una segunda cuenta con el proveedor de hospedaje. Usaría esta segunda cuenta solo para realizar pruebas, pero se configuraría de la misma manera que la cuenta de producción.

En este tutorial se muestran los pasos para la opción 2. Al final del tutorial [implementación en el entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , se proporciona orientación para la opción 3 y, al final de este tutorial, hay vínculos a recursos para la opción 1.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configuración de la aplicación para que se ejecute en un nivel de confianza medio

Antes de instalar IIS e implementarlo en él, cambiará la configuración del archivo Web. config para que el sitio se ejecute más como lo hará en un entorno de hospedaje compartido típico.

Los proveedores de hospedaje normalmente ejecutan el sitio web con *confianza media*, lo que significa que hay algunas cosas que no se permiten. Por ejemplo, el código de aplicación no puede tener acceso al registro de Windows y no puede leer ni escribir archivos que estén fuera de la jerarquía de carpetas de la aplicación. De forma predeterminada, la aplicación se ejecuta en un *nivel de confianza alto* en el equipo local, lo que significa que es posible que la aplicación pueda hacer cosas en las que se produciría un error cuando se implemente en producción. Por lo tanto, para que el entorno de prueba refleje de forma más precisa el entorno de producción, configurará la aplicación para que se ejecute en un nivel de confianza medio.

En el archivo Web. config de la aplicación, agregue un elemento **Trust** en el elemento **System. Web** , tal como se muestra en este ejemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

La aplicación se ejecutará ahora en el nivel de confianza medio en IIS, incluso en el equipo local. Esta configuración permite detectar lo antes posible cualquier intento por parte del código de la aplicación para hacer algo que produciría un error en la producción.

> [!NOTE]
> Si usa Migraciones de Entity Framework Code First, asegúrese de que tiene instalada la versión 5,0 o posterior. En Entity Framework versión 4,3, las migraciones requieren plena confianza para poder actualizar el esquema de la base de datos.

## <a name="installing-iis-and-web-deploy"></a>Instalar IIS y Web Deploy

Para implementar en IIS en el equipo de desarrollo, debe tener instalado IIS y Web Deploy. No se incluyen en la configuración predeterminada de Windows 7. Si ya ha instalado IIS y Web Deploy, vaya a la sección siguiente.

Usar el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx) es la manera preferida de instalar iis y Web deploy, porque el instalador de plataforma web instala una configuración recomendada para IIS e instala automáticamente los requisitos previos para iis y Web deploy, si es necesario.

Para ejecutar el instalador de plataforma web para instalar IIS y Web Deploy, use el siguiente vínculo. Si ya tiene instalado IIS, Web Deploy o cualquiera de los componentes necesarios, el instalador de plataforma web instala solo lo que falta.

- [Instalar IIS y Web Deploy mediante WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Establecer el grupo de aplicaciones predeterminado en .NET 4

Después de instalar IIS, ejecute el **Administrador de IIS** para asegurarse de que la .NET Framework versión 4 está asignada al grupo de aplicaciones predeterminado.

En el menú **Inicio** de Windows, seleccione **Ejecutar**, escriba "inetmgr" y, a continuación, haga clic en **Aceptar**. (Si el comando **Ejecutar** no está en el menú **Inicio** , puede presionar la tecla Windows y R para abrirlo. O haga clic con el botón secundario en la barra de tareas, haga clic en **propiedades**, seleccione la pestaña **menú Inicio** , haga clic en **personalizar**y seleccione **Ejecutar comando**.

En el panel **conexiones** , expanda el nodo de servidor y seleccione **grupos de aplicaciones**. En el panel **grupos de aplicaciones** , si **DefaultAppPool** se asigna a la versión 4 de .NET Framework, como en la siguiente ilustración, vaya a la sección siguiente.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Si solo ve dos grupos de aplicaciones y ambos están establecidos en el .NET Framework 2,0, tendrá que instalar ASP.NET 4 en IIS:

- Abra una ventana del símbolo del sistema; para ello, haga clic con el botón secundario en **símbolo del sistema** en el menú **Inicio** de Windows y seleccione **Ejecutar como administrador**. A continuación, ejecute [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar ASP.net 4 en IIS, con los siguientes comandos. (En los sistemas de 64 bits, reemplace "Framework" por "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando crea nuevos grupos de aplicaciones para el .NET Framework 4, pero el grupo de aplicaciones predeterminado se establecerá en 2,0. Va a implementar una aplicación que tiene como destino .NET 4 para ese grupo de aplicaciones, por lo que tiene que cambiar el grupo de aplicaciones a .NET 4.

Si ha cerrado el **Administrador de IIS**, ejecútelo de nuevo, expanda el nodo del servidor y haga clic en grupos de **aplicaciones** para volver a mostrar el panel grupos de **aplicaciones** .

En el panel **grupos de aplicaciones** , haga clic en **DefaultAppPool**y, a continuación, en el panel **acciones** , haga clic en **configuración básica**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

En el cuadro de diálogo **modificar grupo de aplicaciones** , cambie **.NET Framework versión** a **.NET Framework v 4.0.30319** y haga clic en **Aceptar**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Ahora está listo para publicar en IIS.

## <a name="publishing-to-iis"></a>Publicar en IIS

Hay varias maneras de implementar con Visual Studio 2010 y Web Deploy:

- Usar publicación con un solo clic de Visual Studio.
- Cree un *paquete de implementación* e instálelo mediante la interfaz de usuario del administrador de IIS. El paquete de implementación consta de un archivo *. zip* que contiene todos los archivos y metadatos necesarios para instalar un sitio en IIS.
- Cree un paquete de implementación e instálelo mediante la línea de comandos.

El proceso que realizó en los tutoriales anteriores para configurar Visual Studio para automatizar las tareas de implementación se aplica a todos estos tres métodos. En estos tutoriales, usará el primero de estos métodos. Para obtener información sobre el uso de paquetes de implementación, vea [mapa de contenido de implementación de ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

Antes de publicar, asegúrese de que está ejecutando Visual Studio en modo de administrador. (En el menú **Inicio** de Windows 7, haga clic con el botón derecho en el icono de la versión de Visual Studio que está usando y seleccione **Ejecutar como administrador**). El modo de administrador es necesario para la publicación únicamente cuando se publica en IIS en el equipo local.

En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto ContosoUniversity (no en el proyecto CONTOSOUNIVERSITY. Dal) y seleccione **publicar**.

Aparece el Asistente para **publicación web** .

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

En la lista desplegable, seleccione **&lt;nuevo...&gt;** .

En el cuadro de diálogo **nuevo perfil** , escriba "prueba" y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo Nuevo perfil](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Este nombre es el mismo que el nodo intermedio del archivo de transformación Web. test. config que creó anteriormente. Esta correspondencia es lo que hace que se apliquen las transformaciones Web. test. config al publicar con este perfil.

El asistente avanza automáticamente a la pestaña **conexión** .

En el cuadro **dirección URL del servicio** , escriba *localhost*.

En el cuadro **sitio o aplicación** , escriba *sitio web predeterminado/ContosoUniversity*.

En el cuadro **dirección URL de destino** , escriba `http://localhost/ContosoUniversity`.

La configuración de la **dirección URL de destino** no es obligatoria. Cuando Visual Studio finaliza la implementación de la aplicación, abre automáticamente el explorador predeterminado en esta dirección URL. Si no desea que el explorador se abra automáticamente después de la implementación, deje este cuadro en blanco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse a IIS en el equipo local.

Una marca de verificación verde comprueba que la conexión se ha realizado correctamente.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Haga clic en **siguiente** para avanzar a la pestaña **configuración** .

El cuadro desplegable **configuración** especifica la configuración de compilación que se va a implementar. El valor predeterminado es release, que es lo que desea.

Deje desactivada la casilla **quitar archivos adicionales en el destino** . Dado que se trata de la primera implementación, todavía no habrá ningún archivo en la carpeta de destino.

En la sección **bases de datos** , escriba el siguiente valor en el cuadro cadena de conexión de **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

El proceso de implementación colocará esta cadena de conexión en el archivo Web. config implementado porque está seleccionada **la opción usar esta cadena de conexión en tiempo de ejecución** .

También en **SchoolContext**, seleccione **aplicar migraciones de Code First**. Esta opción hace que el proceso de implementación configure el archivo Web. config implementado para especificar el inicializador de `MigrateDatabaseToLatestVersion`. Este inicializador actualiza automáticamente la base de datos a la versión más reciente cuando la aplicación obtiene acceso a la base de datos por primera vez después de la implementación.

En el cuadro cadena de conexión de **DefaultConnection**, escriba el siguiente valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deje la **base de datos de actualización** desactivada. La base de datos de pertenencia se implementará copiando el archivo. sdf en los datos de la\_de la aplicación y no quiere que el proceso de implementación haga nada más con esta base de datos.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Haga clic en **siguiente** para avanzar a la pestaña **vista previa** .

En la pestaña **vista previa** , haga clic en **iniciar vista previa** para ver una lista de los archivos que se copiarán.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Haga clic en **Publicar**.

Si Visual Studio no está en modo de administrador, es posible que reciba un mensaje de error que indica un error de permisos. En ese caso, cierre Visual Studio, ábralo en modo de administrador e intente publicar de nuevo.

Si Visual Studio está en modo de administrador, la ventana de **salida** informa de que la compilación se ha realizado correctamente y se ha publicado.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

El explorador se abre automáticamente en la Página principal de Contoso University que se ejecuta en IIS en el equipo local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Pruebas en el entorno de prueba

Observe que el indicador de entorno muestra "(test)" en lugar de "(dev)", que muestra que la transformación de *Web. config* para el indicador de entorno se realizó correctamente.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Ejecute la página **Students** para comprobar que la base de datos implementada no tiene estudiantes. Al seleccionar esta página, la carga puede tardar unos minutos, ya que Code First crea la base de datos y, a continuación, ejecuta el método `Seed`. (No lo hacía cuando estaba en la Página principal porque la aplicación no ha intentado tener acceso a la base de datos todavía).

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Ejecute la página **instructores** para comprobar que Code First ha inicializado la base de datos con datos del instructor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Seleccione **Agregar estudiantes** en el menú **estudiantes** , agregue un estudiante y, a continuación, vea el nuevo estudiante en **la página** Students para comprobar que puede escribir correctamente en la base de datos:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

En el menú de **cursos** , seleccione **Actualizar créditos**. La página **Actualizar créditos** requiere permisos de administrador, por lo que se muestra la página **de inicio de sesión** . Escriba las credenciales de la cuenta de administrador que creó anteriormente ("admin" y "Pas $ w0rd"). Se muestra la página **Actualizar créditos** , que comprueba que la cuenta de administrador que creó en el tutorial anterior se ha implementado correctamente en el entorno de prueba.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Compruebe que existe una carpeta *Elmah* con solo el archivo de marcador de posición.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisar los cambios automáticos de Web. config para Migraciones de Code First

Abra el archivo *Web. config* en la aplicación implementada en *C:\inetpub\wwwroot\ContosoUniversity* y puede ver dónde se configuró el proceso de implementación migraciones de Code First para actualizar automáticamente la base de datos a la versión más reciente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

El proceso de implementación también ha creado una nueva cadena de conexión para que Migraciones de Code First usar exclusivamente para actualizar el esquema de la base de datos:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Esta cadena de conexión adicional le permite especificar una cuenta de usuario para las actualizaciones del esquema de la base de datos y una cuenta de usuario diferente para el acceso a los datos de la aplicación. Por ejemplo, puede asignar el rol de propietario de la base de\_a Migraciones de Code First, y las funciones dB\_DataReader y DB\_DataReader a la aplicación. Se trata de un patrón común de defensa en profundidad que evita que un código potencialmente malintencionado en la aplicación cambie el esquema de la base de datos. (Por ejemplo, esto puede ocurrir en un ataque de inyección de SQL correcto). Este patrón no se usa en estos tutoriales. No se aplica a SQL Server Compact y no se aplica al migrar a SQL Server en un tutorial posterior de esta serie. El sitio de Cytanium ofrece una sola cuenta de usuario para tener acceso a la base de datos de SQL Server que se crea en Cytanium. Si es capaz de implementar este patrón en su escenario, puede realizar los pasos siguientes para hacerlo:

1. En la pestaña **configuración** del Asistente para **publicación web** , escriba la cadena de conexión que especifica un usuario con permisos de actualización de esquemas de base de datos completa y desactive la casilla **usar esta cadena de conexión en tiempo de ejecución** . En el archivo Web. config implementado, se convierte en el `DatabasePublish` cadena de conexión.
2. Cree una transformación de archivo Web. config para la cadena de conexión que desea que la aplicación use en tiempo de ejecución.

Ahora ha implementado la aplicación en IIS en el equipo de desarrollo y la ha probado allí. Esto comprueba que el proceso de implementación haya copiado el contenido de la aplicación en la ubicación correcta (excluyendo los archivos que no desea implementar) y que Web Deploy configurar IIS correctamente durante la implementación. En el siguiente tutorial, ejecutará una prueba más que encuentre una tarea de implementación que todavía no se ha realizado: establecimiento de permisos de carpeta en la carpeta *Elmah* .

## <a name="more-information"></a>Más información

Para obtener información sobre cómo ejecutar IIS o IIS Express en Visual Studio, vea los siguientes recursos:

- [IIS Express información general](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) sobre el sitio de IIS.net.
- [Presentación de IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) en el blog de Scott Guthrie.
- [Cómo: especificar el servidor web para proyectos web en Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Diferencias principales entre IIS y el servidor de desarrollo de ASP.net](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) en el sitio de ASP.net.
- [Pruebe su aplicación ASP.NET MVC o Web Forms en IIS 7 en 30 segundos en el](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) blog de Rick Anderson. Esta entrada proporciona ejemplos de por qué las pruebas con el Servidor de desarrollo de Visual Studio (Cassini) no son tan confiables como las pruebas en IIS Express y por qué las pruebas en IIS Express no son tan confiables como las pruebas en IIS.

Para obtener información sobre los problemas que pueden surgir cuando la aplicación se ejecuta en un nivel de confianza medio, consulte [hospedaje de aplicaciones de ASP.net en confianza media](http://www.4guysfromrolla.com/articles/100307-1.aspx) en los 4 chicos del sitio de Rolla.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
