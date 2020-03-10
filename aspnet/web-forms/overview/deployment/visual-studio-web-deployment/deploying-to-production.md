---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'ASP.NET Web Deployment Using Visual Studio: Deploying to Production | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513673"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implementación web de ASP.NET con Visual Studio: implementación en producción

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general

En este tutorial, configurará una cuenta de Microsoft Azure, creará entornos de ensayo y producción e implementará la aplicación Web de ASP.NET en los entornos de ensayo y producción mediante la característica de publicación con un solo clic de Visual Studio.

Si lo prefiere, puede realizar la implementación en un proveedor de hospedaje de terceros. La mayoría de los procedimientos descritos en este tutorial son los mismos para un proveedor de hospedaje o para Azure, salvo que cada proveedor tiene su propia interfaz de usuario para la administración de cuentas y sitios Web. Puede encontrar un proveedor de hospedaje en la [Galería de proveedores](https://www.microsoft.com/web/hosting) en el sitio web de Microsoft.com.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la página de solución de problemas de esta serie de tutoriales.

## <a name="get-a-microsoft-azure-account"></a>Obtener una cuenta de Microsoft Azure

Si aún no tiene una cuenta de Azure, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Crear un entorno de ensayo

> [!NOTE]
> Dado que se ha escrito este tutorial, Azure App Service agregado una nueva característica para automatizar muchos de los procesos para crear entornos de ensayo y producción. Consulte [configuración de entornos de ensayo para aplicaciones web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Como se explica en el [tutorial implementar en el entorno de prueba](deploying-to-iis.md), el entorno de prueba más confiable es un sitio web en el proveedor de hospedaje que es como el sitio web de producción. En muchos proveedores de hospedaje tendría que sopesar las ventajas de esto con un costo adicional importante, pero en Azure puede crear una aplicación web gratuita adicional como aplicación de ensayo. También necesita una base de datos, y los gastos adicionales por el gasto de su base de datos de producción serán ninguno o mínimo. En Azure, se paga por la cantidad de almacenamiento de base de datos que se usa en lugar de para cada base de datos, y la cantidad de almacenamiento adicional que se va a usar en el almacenamiento provisional será mínima.

Como se explica en el [tutorial implementar en el entorno de prueba](deploying-to-iis.md), en ensayo y producción, va a implementar las dos bases de datos en una base de datos. Si desea mantenerlos separados, el proceso sería el mismo, salvo que crearía una base de datos adicional para cada entorno y seleccionaría la cadena de destino correcta para cada base de datos al crear el perfil de publicación.

En esta sección del tutorial, creará una aplicación web y una base de datos que se usarán para el entorno de ensayo y realizará la implementación en ensayo y pruebas antes de crear e implementar en el entorno de producción.

> [!NOTE]
> En los pasos siguientes se muestra cómo crear una aplicación web en Azure App Service mediante el portal de administración de Azure. En la versión más reciente del SDK de Azure, también puede hacerlo sin tener que cerrar Visual Studio, mediante el uso de Explorador de servidores. En Visual Studio 2013, también puede crear una aplicación web directamente desde el cuadro de diálogo publicar. Para obtener más información, consulte [creación de una aplicación Web de ASP.net en Azure App Service.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. En [Azure portal de administración](https://manage.windowsazure.com/), haga clic en **sitios web**y, a continuación, haga clic en **nuevo**.
2. Haga clic en **sitio web**y, a continuación, en **creación personalizada**.

    Se abre el Asistente para **crear nuevo sitio web** . El Asistente para **creación personalizada** le permite crear un sitio web y una base de datos al mismo tiempo.
3. En el paso **crear sitio web** del asistente, escriba una cadena en el cuadro **dirección URL** que se usará como dirección URL única para el entorno de ensayo de la aplicación. Por ejemplo, escriba ContosoUniversity-staging123 (incluidos los números aleatorios al final para que sea único en caso de que se realice ContosoUniversity-staging).

    La URL completa consistirá en el valor que escriba más el sufijo que aparece junto al cuadro de texto.
4. En la lista desplegable **región** , elija la región más cercana a usted.

    Esta configuración especifica en qué centro de datos se ejecutará la aplicación Web.
5. En la lista desplegable **base de datos** , elija **crear una nueva base de datos SQL**.
6. En el cuadro **nombre de cadena de conexión de base de BD** , deje el valor predeterminado, *DefaultConnection*.
7. Haga clic en la flecha que apunta a la derecha en la parte inferior del cuadro.

    En la ilustración siguiente se muestra el cuadro de diálogo **crear sitio web** con valores de ejemplo. La dirección URL y la región que ha escrito serán diferentes.

    ![Paso crear sitio web](deploying-to-production/_static/image1.png)

    El asistente avanza hasta el paso **Especificación de la configuración de la base de datos** .
8. En el cuadro **nombre** , escriba *ContosoUniversity* más un número aleatorio para que sea único, por ejemplo, *ContosoUniversity123*.
9. En el cuadro **servidor** , seleccione **nuevo SQL Database servidor**.
10. Escriba un nombre de administrador y una contraseña.

    Aquí no escribe un nombre y una contraseña existentes. Escribirá un nombre y una contraseña nuevos que definirá ahora para usarlos más adelante cuando acceda a la base de datos.
11. En el cuadro **región** , elija la misma región que eligió para la aplicación Web.

    Mantener el servidor Web y el servidor de base de datos en la misma región le ofrece el mejor rendimiento y minimiza los gastos.
12. Haga clic en la marca de verificación situada en la parte inferior del cuadro para indicar que ha terminado.

    En la ilustración siguiente se muestra el cuadro de diálogo **especificar configuración de base de datos** con valores de ejemplo. Los valores especificados pueden ser diferentes.

    ![Paso de configuración de base de datos nuevo sitio web: Asistente crear con base de datos](deploying-to-production/_static/image2.png)

    El Portal de administración vuelve a la página websites y la columna **status** muestra que se está creando la aplicación Web. Después de un tiempo (normalmente menos de un minuto), la columna **Estado** muestra que la aplicación web se ha creado correctamente. En la barra de navegación de la izquierda, el número de aplicaciones web que tiene en su cuenta aparece junto al icono de **websites** y el número de bases de datos aparece al lado del icono de **SQL Database** .

    ![Página sitios web de Portal de administración, sitio web creado](deploying-to-production/_static/image3.png)

    El nombre de la aplicación web será diferente de la aplicación de ejemplo en la ilustración.

## <a name="deploy-the-application-to-staging"></a>Implementar la aplicación en el almacenamiento provisional

Ahora que ha creado una aplicación web y una base de datos para el entorno de ensayo, puede implementar el proyecto en ella.

> [!NOTE]
> Estas instrucciones muestran cómo crear un perfil de publicación descargando un archivo *. publishsettings* , que funciona no solo en Azure, sino también para proveedores de hospedaje de terceros. El último SDK de Azure también le permite conectarse directamente a Azure desde Visual Studio y elegir en una lista de aplicaciones web que tenga en su cuenta de Azure. En Visual Studio 2013, puede iniciar sesión en Azure desde el cuadro de diálogo **publicación web** o desde la ventana **Explorador de servidores** . Para obtener más información, consulte [creación de una aplicación Web de ASP.net en Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Descargar el archivo. publishsettings

1. Haga clic en el nombre de la aplicación web que acaba de crear.

    ![Haga clic en el sitio para ir al panel](deploying-to-production/_static/image4.png)
2. En **vista rápida** en la pestaña **Panel** , haga clic en **Descargar Perfil de publicación**.

    ![Vínculo Descargar Perfil de publicación](deploying-to-production/_static/image5.png)

    En este paso se descarga un archivo que contiene toda la configuración que necesita para implementar una aplicación en la aplicación Web. Importará este archivo en Visual Studio para que no tenga que escribir esta información manualmente.
3. Guarde el archivo *. publishsettings* en una carpeta a la que pueda acceder desde Visual Studio.

    ![guardar el archivo. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Seguridad: el archivo *. publishsettings* contiene sus credenciales (sin codificar) que se usan para administrar sus servicios y suscripciones de Azure. El procedimiento recomendado para este archivo consiste en almacenarlo temporalmente fuera de los directorios de origen (por ejemplo en la carpeta Bibliotecas\Documentos) y, a continuación, eliminarlo cuando la importación se haya completado. Un usuario malintencionado que obtenga acceso al archivo *. publishsettings* puede editar, crear y eliminar sus servicios de Azure.

### <a name="create-a-publish-profile"></a>Crear un perfil de publicación

1. En Visual Studio, haga clic con el botón derecho en el proyecto ContosoUniversity en **Explorador de soluciones** y seleccione **publicar** en el menú contextual.

    Se abre el asistente para **publicación web** .
2. Haga clic en la pestaña **Perfil**.
3. Haga clic en **Import**.
4. Navegue hasta el archivo *. publishsettings* que descargó anteriormente y, a continuación, haga clic en **abrir**.

    ![Importar configuración de publicación (cuadro de diálogo)](deploying-to-production/_static/image7.png)
5. En la pestaña **conexión** , haga clic en **validar conexión** para asegurarse de que la configuración sea correcta.

    Cuando se ha validado la conexión, se muestra una marca de verificación verde junto al botón **validar conexión** .

    En el caso de algunos proveedores de hospedaje, al hacer clic en **validar conexión**, es posible que aparezca un cuadro de diálogo de **error de certificado** . Si lo hace, compruebe que el nombre del servidor es el esperado. Si el nombre del servidor es correcto, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Aceptar**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de comprar un certificado SSL para la dirección URL en la que se está implementando. Si prefiere establecer una conexión segura con un certificado válido, póngase en contacto con el proveedor de hospedaje).
6. Haga clic en **Siguiente**.

    ![icono conexión correcta y botón siguiente en la pestaña conexión](deploying-to-production/_static/image8.png)
7. En la pestaña **configuración** , expanda **Opciones de publicación de archivos**y, a continuación, seleccione **excluir archivos en la carpeta app\_Data**.

    Para obtener información acerca de las demás opciones de **Opciones de publicación de archivos**, consulte el tutorial [implementación de IIS](deploying-to-iis.md) . La captura de pantalla que muestra el resultado de este paso y los siguientes pasos de configuración de la base de datos se encuentra al final de los pasos de configuración de la base de datos.
8. En **DefaultConnection** , en la sección **bases** de datos, configure la implementación de bases de datos para la base de datos de pertenencia.
9. 1. Seleccione **Actualizar base de datos**.

        El cuadro **cadena de conexión remota** que se encuentra justo debajo de **DefaultConnection** se rellena con la cadena de conexión del archivo. publishsettings. La cadena de conexión incluye SQL Server credenciales, que se almacenan en texto sin formato en el archivo *. pubxml* . Si prefiere no almacenarlas de forma permanente, puede quitarlas del perfil de publicación después de implementar la base de datos y almacenarlas en Azure en su lugar. Para obtener más información, consulte [Cómo mantener la seguridad de las cadenas de conexión de la base de datos de ASP.net al realizar la implementación en Azure desde el](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) blog de Scott Hanselman.
      2. Haga clic en **configurar actualizaciones de base de datos**.
      3. En el cuadro de diálogo **configurar actualizaciones de base de datos** , haga clic en **Agregar script SQL**.
      4. En el cuadro **Agregar script SQL** , navegue hasta el script *ASPNET-Data-Prod. SQL* que guardó anteriormente en la carpeta de la solución y, a continuación, haga clic en **abrir**.
      5. Cierre el cuadro de diálogo **configurar actualizaciones de base de datos** .
10. En **SchoolContext** en la sección **bases de datos** , seleccione **Ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** .

    Visual Studio muestra **ejecutar migraciones de Code First** en lugar de **Actualizar base de datos** para las clases de `DbContext`. Si desea usar el proveedor de dbDacFx en lugar de las migraciones para implementar una base de datos a la que tiene acceso mediante una clase `DbContext`, consulte [cómo implementar una base de datos de Code First sin migraciones](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) . en las preguntas más frecuentes sobre la implementación web para Visual Studio y ASP.net en MSDN, vea.

    La pestaña **configuración** ahora es similar al ejemplo siguiente:

    ![Pestaña Configuración del almacenamiento provisional](deploying-to-production/_static/image9.png)
11. Realice los pasos siguientes para guardar el perfil y cambiarle el nombre a *almacenamiento provisional*:

    1. Haga clic en la pestaña **perfil** y, a continuación, haga clic en **administrar perfiles**.
    2. La importación creó dos nuevos perfiles, uno para FTP y otro para Web Deploy. Configuró el perfil de Web Deploy: cambie el nombre de este perfil a *ensayo*.

        ![Cambiar el nombre del perfil a ensayo](deploying-to-production/_static/image10.png)
    3. Cierre el cuadro de diálogo **Editar perfiles de publicación web** .
    4. Cierre el Asistente para **publicación web** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar una transformación de Perfil de publicación para el indicador de entorno

> [!NOTE]
> En esta sección se muestra cómo configurar una transformación de Web. config para el indicador de entorno. Dado que el indicador está en el elemento `<appSettings>`, tiene otra alternativa para especificar la transformación cuando está implementando en Azure App Service. Para obtener más información, vea [especificar la configuración de Web. config en Azure](web-config-transformations.md#watransforms).

1. En **Explorador de soluciones**, expanda **propiedades**y, a continuación, expanda **PublishProfiles**.
2. Haga clic con el botón secundario en *staging. pubxml*y, a continuación, haga clic en **Agregar transformación de configuración**.

    ![Agregar transformación de configuración para el almacenamiento provisional](deploying-to-production/_static/image11.png)

    Visual Studio crea el archivo de transformación *Web. staging. config* y lo abre.
3. En el archivo de transformación *Web. staging. config* , inserte el siguiente código inmediatamente después de la etiqueta de apertura `configuration`.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Cuando se usa el perfil de publicación de almacenamiento provisional, esta transformación establece el indicador de entorno en "Prod". En la aplicación web implementada, no verá ningún sufijo como "(dev)" o "(test)" después del encabezado H1 "Contoso University".
4. Haga clic con el botón secundario en el archivo *Web. staging. config* y haga clic en **vista previa de transformación** para asegurarse de que la transformación codificada genera los cambios esperados.

    En la ventana de **vista previa de Web. config** se muestra el resultado de aplicar las transformaciones *Web. Release. config* y las transformaciones *Web. staging. config* .

### <a name="prevent-public-use-of-the-test-app"></a>Impedir el uso público de la aplicación de prueba

Una consideración importante para la aplicación de ensayo es que estará activa en Internet, pero no desea que el público la use. Para minimizar la probabilidad de que la gente la encuentre y la use, puede usar uno o varios de los métodos siguientes:

- Establezca las reglas de firewall que permiten el acceso a la aplicación de ensayo solo desde las direcciones IP que se usan para probar el almacenamiento provisional.
- Use una dirección URL ofuscada que sería imposible de adivinar.
- Cree un archivo *robots. txt* para asegurarse de que los motores de búsqueda no rastrearán la aplicación de prueba y los vínculos de informe a él en los resultados de la búsqueda.

El primero de estos métodos es el más eficaz, pero no se trata en este tutorial porque requeriría la implementación en un servicio en la nube de Azure en lugar de Azure App Service. Para obtener más información acerca de las restricciones de Cloud Services y IP en Azure, consulte [Opciones de hospedaje de proceso proporcionadas por Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) y [bloquear el acceso de direcciones IP específicas a un rol Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Si va a realizar la implementación en un proveedor de hospedaje de terceros, póngase en contacto con el proveedor para averiguar cómo implementar las restricciones de IP.

En este tutorial, creará un archivo *robots. txt* .

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **Agregar nuevo elemento**.
2. Cree un nuevo **archivo de texto** llamado *robots. txt*y escriba el texto siguiente:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    La línea de `User-agent` indica a los motores de búsqueda que las reglas del archivo se aplican a todos los rastreadores web (robots) del motor de búsqueda, y la línea `Disallow` especifica que no se debe rastrear ninguna página en el sitio.

    Quiere que los motores de búsqueda cataloguen su aplicación de producción, por lo que debe excluir este archivo de la implementación de producción. Para ello, configurará un valor en el perfil de publicación de producción al crearlo.

### <a name="deploy-to-staging"></a>Implementación en entorno de ensayo

1. Abra el Asistente para **publicación web** haciendo clic con el botón secundario en el proyecto contoso University y haciendo clic en **publicar**.
2. Asegúrese de que está seleccionado el perfil de **almacenamiento provisional** .
3. Haga clic en **Publicar**.

    La ventana **Salida** muestra qué acciones de implementación se realizaron e informa de la correcta finalización de la implementación. El explorador predeterminado se abre automáticamente en la dirección URL de la aplicación web implementada.

## <a name="test-in-the-staging-environment"></a>Prueba en el entorno de ensayo

Observe que el indicador de entorno está ausente (no hay "(test)" o "(dev)" después del encabezado H1, lo que muestra que la transformación de *Web. config* para el indicador de entorno se ha realizado correctamente.

![Almacenamiento provisional de página principal](deploying-to-production/_static/image12.png)

Ejecute la página **Students** para comprobar que la base de datos implementada no tiene estudiantes.

Ejecute la página **instructores** para comprobar que Code First ha inicializado la base de datos con datos del instructor:

Seleccione **Agregar estudiantes** en el menú **estudiantes** , agregue un estudiante y, a continuación, vea el nuevo estudiante en **la página** Students para comprobar que puede escribir correctamente en la base de datos.

En la página **cursos** , haga clic en **Actualizar créditos**. La página **Actualizar créditos** requiere permisos de administrador, por lo que se muestra la página **de inicio de sesión** . Escriba las credenciales de la cuenta de administrador que creó anteriormente ("admin" y "prodpwd"). Se muestra la página **Actualizar créditos** , que comprueba que la cuenta de administrador que creó en el tutorial anterior se ha implementado correctamente en el entorno de prueba.

Solicite una dirección URL no válida para producir un error en el que se realizará ELMAH y, a continuación, solicite el informe de errores ELMAH. Si va a realizar la implementación en un proveedor de hospedaje de terceros, probablemente encontrará que el informe está vacío por el mismo motivo que estaba vacío en el tutorial anterior. Tendrá que usar las herramientas de administración de cuentas del proveedor de hospedaje para configurar los permisos de carpeta a fin de permitir que ELMAH escriba en la carpeta de registro.

La aplicación que ha creado se ejecuta ahora en la nube en una aplicación web que es similar a la que va a usar para producción. Dado que todo funciona correctamente, el siguiente paso es implementar en producción.

## <a name="deploy-to-production"></a>Implementar en producción

El proceso de creación de una aplicación Web de producción y la implementación en producción es el mismo que para el almacenamiento provisional, salvo que debe excluir el archivo *robots. txt* de la implementación. Para ello, edite el archivo de Perfil de publicación.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Crear el entorno de producción y el perfil de publicación de producción

1. Cree la aplicación web y la base de datos de producción en Azure siguiendo el mismo procedimiento que usó para el almacenamiento provisional.

    Al crear la base de datos, puede elegir colocarla en el mismo servidor que creó anteriormente, o crear un nuevo servidor.
2. Descargue el archivo *. publishsettings* .
3. Cree el perfil de publicación importando el archivo Production *. publishsettings* siguiendo el mismo procedimiento que usó para el almacenamiento provisional.

    No olvide configurar el script de implementación de datos en **DefaultConnection** en la sección **bases** de datos de la pestaña **configuración** .
4. Cambie el nombre del perfil de publicación a *producción*.
5. Configure una transformación de Perfil de publicación para el indicador de entorno, siguiendo el mismo procedimiento que usó para el almacenamiento provisional.

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite el archivo. pubxml para excluir robots. txt

Los archivos de Perfil de publicación se denominan &lt;ProfileName&gt; *. pubxml* y se encuentran en la carpeta *PublishProfiles* . La carpeta *PublishProfiles* se encuentra en la carpeta *propiedades* de C# un proyecto de aplicación Web, en la carpeta *mi proyecto* de un proyecto de aplicación web VB, o en la carpeta *App\_Data* de un proyecto de aplicación Web. Cada archivo *. pubxml* contiene los valores que se aplican a un perfil de publicación. Los valores que escriba en el Asistente para publicación web se almacenan en estos archivos, y puede modificarlos para crear o cambiar la configuración que no está disponible en la interfaz de usuario de Visual Studio.

De forma predeterminada, los archivos *. pubxml* se incluyen en el proyecto al crear un perfil de publicación, pero puede excluirlos del proyecto y Visual Studio los usará. Visual Studio busca en la carpeta *PublishProfiles* los archivos *. pubxml* , independientemente de si están incluidos en el proyecto.

Para cada archivo *. pubxml* hay un archivo *. pubxml. User* . El archivo *. pubxml. User* contiene la contraseña cifrada si seleccionó la opción **guardar contraseña** y, de forma predeterminada, se excluye del proyecto.

Un archivo *. pubxml* contiene la configuración que pertenece a un perfil de publicación específico. Si desea configurar las opciones que se aplican a todos los perfiles, puede crear un archivo *. WPP. targets* . El proceso de compilación importa estos archivos en el archivo de proyecto *. csproj* o *. vbproj* , por lo que la mayoría de las opciones que se pueden configurar en el archivo de proyecto se pueden configurar en estos archivos. Para obtener más información sobre los archivos *. pubxml* y los archivos. *WPP. targets* , vea [Cómo: editar la configuración de implementación en archivos de Perfil de publicación (. pubxml) y el archivo. WPP. targets en proyectos Web de Visual Studio](https://msdn.microsoft.com/library/ff398069.aspx).

1. En **Explorador de soluciones**, expanda **propiedades** y expanda **PublishProfiles**.
2. Haga clic con el botón secundario en *Production. pubxml* y haga clic en **abrir**.

    ![Abra el archivo. pubxml](deploying-to-production/_static/image13.png)
3. Haga clic con el botón secundario en *Production. pubxml* y haga clic en **abrir**.
4. Agregue las líneas siguientes inmediatamente antes del elemento de `PropertyGroup` de cierre:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    El archivo. pubxml ahora es similar al ejemplo siguiente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obtener más información sobre cómo excluir archivos y carpetas, vea ¿puedo [excluir archivos o carpetas específicos de la implementación?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) en las **preguntas más frecuentes sobre la implementación web para Visual Studio y ASP.net** en MSDN.

### <a name="deploy-to-production"></a>Implementar en producción

1. Abra el Asistente para **publicación web** Asegúrese de que el perfil de publicación de **producción** está seleccionado y, a continuación, haga clic en **iniciar vista previa** en la pestaña **vista previa** para comprobar que el archivo *robots. txt* no se copiará en la aplicación de producción.

    ![Vista previa de los archivos que se van a publicar en producción](deploying-to-production/_static/image14.png)

    Revise la lista de archivos que se copiarán. Verá que se omiten todos los archivos *. CS* , incluidos los archivos *. aspx.cs*, *. aspx.Designer.CS*, *Master.CS*y *Master.Designer.CS* . Todo este código se ha compilado en los archivos *ContosoUniversity. dll* y *ContosoUniversity. pdb* que encontrará en la carpeta *bin* . Dado que solo se necesita el *archivo. dll* para ejecutar la aplicación y ha especificado anteriormente que solo se deben implementar los archivos necesarios para ejecutar la aplicación, no se copiaron los archivos *. CS* en el entorno de destino. La carpeta *obj* y los archivos *ContosoUniversity. csproj* y *. csproj. User* se omiten por la misma razón.

    Haga clic en **publicar** para implementar en el entorno de producción.
2. Pruebe en producción siguiendo el mismo procedimiento que usó para el almacenamiento provisional.

    Todo es idéntico al almacenamiento provisional excepto la dirección URL y la ausencia del archivo *robots. txt* .

## <a name="summary"></a>Resumen

Ahora ha implementado y probado correctamente su aplicación web y está disponible públicamente a través de Internet.

![Página principal de producción](deploying-to-production/_static/image15.png)

En el siguiente tutorial, actualizará el código de la aplicación e implementará el cambio en los entornos de prueba, ensayo y producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción, debe implementar un plan de recuperación. Es decir, debe realizar copias de seguridad periódicas de las bases de datos de la aplicación de producción en una ubicación de almacenamiento segura y debe mantener varias generaciones de dichas copias de seguridad. Al actualizar la base de datos, debe realizar una copia de seguridad inmediatamente antes del cambio. Después, si comete un error y no lo detecta hasta después de implementarlo en producción, podrá recuperar la base de datos al estado en que se encontraba antes de que se dañara. Para obtener más información, consulte [Copia de seguridad y restauración de bases de datos de Azure SQL](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> En este tutorial, se Azure SQL Database la edición de SQL Server que está implementando. Aunque el proceso de implementación es similar a otras ediciones de SQL Server, una aplicación de producción real podría requerir código especial para Azure SQL Database en algunos escenarios. Para obtener más información, vea [trabajar con Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) y [elegir entre SQL Server y Azure SQL Database](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Siguiente](deploying-a-code-update.md)
