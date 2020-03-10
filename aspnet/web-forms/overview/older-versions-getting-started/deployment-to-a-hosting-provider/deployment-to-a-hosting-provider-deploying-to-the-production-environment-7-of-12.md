---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación en el entorno de producción (7 de 12) Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458173"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación en el entorno de producción (7 de 12)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

En este tutorial, configurará una cuenta con un proveedor de hospedaje e implementará la aplicación Web de ASP.NET en el entorno de producción mediante la característica de publicación con un solo clic de Visual Studio.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Seleccionar un proveedor de hospedaje

Para la aplicación contoso University y esta serie de tutoriales, necesita un proveedor que admita ASP.NET 4 y Web Deploy. Se eligió una empresa de hospedaje específica para que los tutoriales puedan ilustrar la experiencia completa de implementación en un sitio Web activo. Cada empresa de hospedaje proporciona diferentes características y la experiencia de implementación en sus servidores varía ligeramente. Sin embargo, el proceso descrito en este tutorial es típico para el proceso general. El proveedor de hospedaje que se usa para este tutorial, Cytanium.com, es uno de los muchos que están disponibles y su uso en este tutorial no constituye una aprobación o recomendación.

Cuando esté listo para seleccionar su propio proveedor de hospedaje, puede comparar las características y los precios de la [Galería de proveedores](https://www.microsoft.com/web/hosting) en el sitio de Microsoft.com/Web.

## <a name="creating-an-account"></a>Creación de una cuenta

Cree una cuenta en el proveedor seleccionado. Si se ha agregado compatibilidad con una base de datos de SQL Server completa, no es necesario seleccionarla para este tutorial, pero la necesitará para el tutorial [migrar a SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) más adelante en esta serie.

Para estos tutoriales, no tiene que registrar un nuevo nombre de dominio. Puede realizar pruebas para comprobar la implementación correcta mediante el uso de la dirección URL temporal asignada al sitio por el proveedor.

Una vez creada la cuenta, normalmente recibirá un correo electrónico de bienvenida que contiene toda la información que necesita para implementar y administrar el sitio. La información que el proveedor de hospedaje le envíe será similar a la que se muestra aquí. El correo electrónico de bienvenida de Cytanium que se envía a los propietarios de cuentas nuevos incluye la siguiente información:

- La dirección URL del sitio del panel de control del proveedor, donde puede administrar la configuración de su sitio. El identificador y la contraseña que especificó se incluyen en esta parte del correo electrónico de bienvenida para facilitar la referencia. (Ambos se han cambiado a un valor de demostración para esta ilustración).

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La versión de .NET Framework predeterminada y la información sobre cómo cambiarla. Muchos sitios de hospedaje tienen como valor predeterminado 2,0, que funciona con las aplicaciones de ASP.NET que tienen como destino los .NET Framework 2,0, 3,0 o 3,5. Sin embargo, contoso University es una aplicación .NET Framework 4, por lo que debe cambiar esta configuración. (En el caso de una aplicación ASP.NET 4,5, usaría la configuración de .NET 4,0).

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- La dirección URL temporal que puede usar para tener acceso a su sitio Web. Cuando se creó esta cuenta, se especificó "contosouniversity.com" como el nombre de dominio existente. Por lo tanto, la dirección URL temporal es `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Información sobre cómo configurar las bases de datos y las cadenas de conexión que necesita para tener acceso a ellas:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Información acerca de las herramientas y la configuración para implementar el sitio. (El correo electrónico de Cytanium también menciona WebMatrix, que se omite aquí).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Establecimiento de la versión de .NET Framework

El correo electrónico de bienvenida de Cytanium incluye un vínculo a instrucciones sobre cómo cambiar la versión de la .NET Framework. En estas instrucciones se explica que esto puede hacerse a través del panel de control de Cytanium. Otros proveedores tienen sitios del panel de control que tienen un aspecto diferente o pueden indicarle que lo haga de otra manera.

Vaya a la dirección URL del panel de control. Después de iniciar sesión con su nombre de usuario y contraseña, verá el panel de control.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

En el cuadro **espacios de hospedaje** , mantenga el puntero sobre el icono Web y seleccione **sitios web** en el menú.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

En el cuadro **sitios web** , haga clic en **contosouniversity.com** (el nombre del sitio que usó al crear la cuenta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

En el cuadro **propiedades del sitio web** , seleccione la pestaña **extensiones** .

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Cambie ASP.NET de la **canalización integrada 2,0** a **4,0 (canalización integrada)** y, a continuación, haga clic en **Actualizar**.

## <a name="publishing-to-the-hosting-provider"></a>Publicar en el proveedor de hospedaje

El correo electrónico de bienvenida del proveedor de hospedaje incluye toda la configuración que necesita para publicar el proyecto, y puede especificar esa información manualmente en un perfil de publicación. Pero usará un método más sencillo y menos propenso a errores para configurar la implementación en el proveedor: descargará un archivo *. publishsettings* y lo importará en un perfil de publicación.

En el explorador, vaya al panel de control de Cytanium y seleccione **Web** y, a continuación, seleccione **sitios Web.**

![Panel de control seleccionar sitios web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Seleccione el sitio Web **contosouniversity.com** .

![Panel de control seleccionar contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Seleccione la pestaña **publicación web** .

![Pestaña publicación web del panel de control](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Cree las credenciales que se usarán para la publicación en web escribiendo un nombre de usuario y una contraseña. Puede especificar las mismas credenciales que usa para iniciar sesión en el panel de control. A continuación, haga clic en **Enable** (Habilitar).

![Panel de control crear credenciales de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Haga clic en **Descargar Perfil de publicación para este sitio web**.

![Panel de control descargar Perfil de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Cuando se le pregunte si desea abrir o guardar el archivo, guárdelo.

![Guardar archivo de Perfil de publicación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

En **Explorador de soluciones** en Visual Studio, haga clic con el botón derecho en el proyecto ContosoUniversity y seleccione **publicar**. Se abrirá el cuadro de diálogo **publicación web** en la pestaña **vista previa** con el perfil de **prueba** seleccionado porque es el último perfil que usó.

Seleccione la pestaña **perfil** y, a continuación, haga clic en **importar**.

![Botón importar del Asistente para publicación Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

En el cuadro de diálogo **importar configuración de publicación** , seleccione el archivo *. publishsettings* que descargó y haga clic en **abrir**. El asistente avanza a la pestaña conexión con todos los campos rellenos.

![Pestaña conexión del Asistente para publicación Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

El archivo. publishsettings coloca la dirección URL permanente planeada para el sitio en el cuadro Dirección URL de destino, pero si aún no ha adquirido ese dominio, reemplace el valor por la dirección URL temporal. En este ejemplo, la dirección URL es  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* El único propósito de este cuadro es especificar la dirección URL que el explorador abrirá automáticamente después de la implementación. Si lo deja en blanco, la única consecuencia es que el explorador no se iniciará automáticamente después de la implementación.

Haga clic en **validar conexión** para comprobar que la configuración es correcta y puede conectarse al servidor. Como vimos anteriormente, una marca de verificación verde comprueba que la conexión se ha realizado correctamente.

Al hacer clic en validar conexión, es posible que aparezca un cuadro de diálogo de **error de certificado** . Si lo hace, compruebe que el nombre del servidor es el esperado. Si es así, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Aceptar**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de comprar un certificado SSL para la dirección URL en la que se está implementando. Si prefiere establecer una conexión segura con un certificado válido, póngase en contacto con el proveedor de hospedaje).

![Error de certificado](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Haga clic en **Siguiente**.

En la sección **bases de datos** de la pestaña **configuración** , escriba los mismos valores que especificó para el perfil de publicación de prueba. Encontrará las cadenas de conexión que necesita en las listas desplegables.

- En el cuadro cadena de conexión de **SchoolContext,** seleccione `Data Source=|DataDirectory|School-Prod.sdf`
- En **SchoolContext**, seleccione **aplicar migraciones de Code First**.
- En el cuadro cadena de conexión de **DefaultConnection**, seleccione `Data Source=|DataDirectory|aspnet-Prod.sdf`
- En **DefaultConnection**, deje la opción **Actualizar base de datos** desactivada.

![Pestaña Configuración del Asistente para publicación Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Haga clic en **Siguiente**.

En la pestaña **vista previa** , haga clic en **iniciar vista previa** para ver una lista de los archivos que se copiarán. Verá la misma lista que vio anteriormente cuando implementó en IIS en el equipo local.

Antes de publicar, cambie el nombre del perfil para que se aplique el archivo de transformación Web. Production. config. Seleccione la pestaña **perfil** y haga clic en **administrar perfiles**.

![Asistente para publicación web administración de perfiles](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

En el cuadro de diálogo **Editar perfiles de publicación web** , seleccione el perfil de producción, haga clic en cambiar **nombre**y cambie el nombre del perfil a producción. A continuación, haga clic en **Cerrar**.

![Cuadro de diálogo Editar perfiles de publicación Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Haga clic en **Publicar**.

La aplicación se publica en el proveedor de hospedaje. El resultado se muestra en la ventana de **salida** .

![Ventana de salida después de la implementación](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

El explorador se abre automáticamente en la dirección URL que especificó en el cuadro **dirección URL de destino** en la pestaña **conexión** del Asistente para **publicación web** . Verá la misma página principal que cuando ejecuta el sitio en Visual Studio, a excepción de que ahora no hay ningún indicador de entorno "(prueba)" o "(dev)" en la barra de título. Esto indica que la transformación *Web. config* del indicador de entorno funcionó correctamente.

> [!NOTE]
> Si sigue viendo "(test)" en el encabezado, elimine la carpeta *obj* del proyecto ContosoUniversity y vuelva a implementarla. En las versiones preliminares del software, el archivo de transformación aplicado previamente (Web. test. config) puede volver a aplicarse aunque esté usando el perfil de producción.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Antes de ejecutar una página que produce el acceso a la base de datos, asegúrese de que Elmah podrá registrar los errores que se produzcan.

## <a name="setting-folder-permissions-for-elmah"></a>Establecer permisos de carpeta para Elmah

Como Recuerde en el tutorial anterior de esta serie, debe asegurarse de que la aplicación tiene permisos de escritura para la carpeta en la aplicación en la que Elmah almacena los archivos de registro de errores. Cuando se implementa en IIS localmente en el equipo, los permisos se establecen de forma manual. En esta sección, verá cómo establecer permisos en Cytanium. (Es posible que algunos proveedores de hospedaje no le permitan hacer esto; pueden ofrecer una o varias carpetas predefinidas con permisos de escritura. En ese caso, tendría que modificar la aplicación para usar las carpetas especificadas).

Puede establecer permisos de carpeta en el panel de control de Cytanium. Vaya a la dirección URL del panel de control y seleccione **Administrador de archivos**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

En el cuadro **Administrador de archivos** , seleccione **contosouniversity.com** y, a continuación, **wwwroot** para ver la carpeta raíz de la aplicación. Haga clic en el icono de candado junto a **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

En la ventana permisos de **archivo**/**carpeta** , active las casillas de **lectura** y **escritura** de **contosouniversity.com** y haga clic en **establecer permisos**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Asegúrese de que Elmah tiene acceso de escritura a la carpeta *Elmah* generando un error y mostrando después el informe de errores Elmah. Solicite una dirección URL no válida como *Studentsxxx. aspx*. Como antes, verá la página *GenericErrorPage. aspx* . Haga clic en el vínculo **Cerrar sesión** y, a continuación, ejecute *Elmah. axd*. Primero se obtiene la página **de inicio de sesión** , que valida que la transformación *Web. config* haya agregado correctamente la autorización Elmah. Después de iniciar sesión, verá el informe que muestra el error que acaba de producir.

[![Elmah. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Pruebas en el entorno de producción

Ejecute la página **Students** . La aplicación intenta tener acceso a la base de datos School por primera vez, lo que desencadena Migraciones de Code First para crear la base de datos. Cuando se muestra la página después de un retraso de un momento, muestra que no hay ningún estudiante.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Ejecute la página **instructores** para comprobar que los datos de inicialización se han insertado correctamente en la base de datos.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Como hizo en el entorno de prueba, desea comprobar que las actualizaciones de la base de datos funcionan en el entorno de producción, pero normalmente no desea especificar los datos de prueba en la base de datos de producción. En este tutorial, usará el mismo método que en la prueba. Pero en una aplicación real podría querer encontrar un método que valide que las actualizaciones de la base de datos se han realizado correctamente sin introducir los datos de prueba en la base de datos de producción. En algunas aplicaciones, puede ser práctico agregar algo y, a continuación, eliminarlo.

Agregue un estudiante y, a continuación, vea los datos que escribió en la página **Students** para comprobar que puede actualizar los datos de la base de datos.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Compruebe que las reglas de autorización funcionan correctamente; para ello, seleccione **Actualizar créditos** en el menú de **cursos** . Se muestra la página **de inicio de sesión** . Escriba las credenciales de la cuenta de administrador, haga clic en **iniciar sesión**y se mostrará la página **Actualizar créditos** .

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Si el inicio de sesión se realiza correctamente, se muestra la página **Actualizar créditos** . Esto indica que la base de datos de pertenencia de ASP.NET (con la única cuenta de administrador) se implementó correctamente.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Ahora ha implementado y probado correctamente el sitio y está disponible públicamente a través de Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Crear un entorno de prueba más confiable

Tal como se explica en el tutorial [implementación del entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) , el entorno de prueba más confiable sería una segunda cuenta en el proveedor de hospedaje que sea igual que la cuenta de producción. Esto sería más caro que el uso de IIS local como entorno de prueba, ya que tendría que registrarse para obtener una segunda cuenta de hospedaje. Pero si se evitan errores o interrupciones en el sitio de producción, puede decidir que merece la pena el costo.

La mayor parte del proceso de creación e implementación en una cuenta de prueba es similar a lo que ya ha hecho para implementar en producción:

- Cree un archivo de transformación *Web. config* .
- Cree una cuenta en el proveedor de hospedaje.
- Cree un nuevo perfil de publicación e impleméntelo en la cuenta de prueba.

### <a name="preventing-public-access-to-the-test-site"></a>Impedir el acceso público al sitio de prueba

Una consideración importante para la cuenta de prueba es que estará activa en Internet, pero no desea que el público la use. Para mantener el sitio privado, puede usar uno o varios de los métodos siguientes:

- Póngase en contacto con el proveedor de hospedaje para establecer las reglas de firewall que permiten el acceso al sitio de pruebas únicamente desde las direcciones IP que se usan para las pruebas.
- Oculte la dirección URL para que no sea similar a la dirección URL del sitio público.
- Use un archivo *robots. txt* para asegurarse de que los motores de búsqueda no rastrearán el sitio de prueba y los vínculos de informe a él en los resultados de la búsqueda.

El primero de estos métodos es obviamente el más seguro, pero el procedimiento para eso es específico de cada proveedor de hospedaje y no se tratará en este tutorial. Si realiza la organización con su proveedor de hospedaje para permitir que solo la dirección IP vaya a la dirección URL de la cuenta de prueba, teóricamente no tiene que preocuparse por los motores de búsqueda que lo rastrean. Pero incluso en ese caso, la implementación de un archivo *robots. txt* es una buena idea como copia de seguridad en caso de que la regla de Firewall se desactive accidentalmente.

El archivo *robots. txt* se incluye en la carpeta del proyecto y debe incluir el texto siguiente:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

La línea de `User-agent` indica a los motores de búsqueda que las reglas del archivo se aplican a todos los rastreadores web (robots) del motor de búsqueda, y la línea `Disallow` especifica que no se debe rastrear ninguna página en el sitio.

Probablemente quiera que los motores de búsqueda cataloguen el sitio de producción, por lo que debe excluir este archivo de la implementación de producción. Para ello, consulte ¿ **se pueden excluir archivos o carpetas específicos de la implementación?** en las [preguntas más frecuentes sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Asegúrese de especificar la exclusión solo para el perfil de publicación de producción.

La creación de una segunda cuenta de hospedaje es un enfoque para trabajar con un entorno de prueba que no es necesario, pero que merece la pena el gasto adicional. En los tutoriales siguientes, seguirá usando IIS como entorno de prueba.

En el siguiente tutorial, actualizará el código de la aplicación e implementará el cambio en los entornos de prueba y producción.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
