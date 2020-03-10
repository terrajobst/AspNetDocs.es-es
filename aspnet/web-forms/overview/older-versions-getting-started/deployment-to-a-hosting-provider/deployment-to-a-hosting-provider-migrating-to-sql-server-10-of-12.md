---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migración a SQL Server-10 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462841"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: migración a SQL Server-10 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo migrar de SQL Server Compact a SQL Server. Uno de los motivos por los que podría querer hacer esto es aprovechar las ventajas de SQL Server características que SQL Server Compact no admite, como procedimientos almacenados, desencadenadores, vistas o replicación. Para obtener más información sobre las diferencias entre SQL Server Compact y SQL Server, vea el tutorial [implementación de SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express frente a SQL Server completos para el desarrollo

Una vez que haya decidido actualizar a SQL Server, es posible que desee usar SQL Server o SQL Server Express en los entornos de desarrollo y pruebas. Además de las diferencias en la compatibilidad de herramientas y en las características del motor de base de datos, existen diferencias en las implementaciones de proveedor entre SQL Server Compact y otras versiones de SQL Server. Estas diferencias pueden hacer que el mismo código genere resultados diferentes. Por lo tanto, si decide mantener SQL Server Compact como base de datos de desarrollo, debe probar exhaustivamente el sitio en SQL Server o SQL Server Express en un entorno de prueba antes de cada implementación en producción.

A diferencia de SQL Server Compact, SQL Server Express es esencialmente el mismo motor de base de datos y usa el mismo proveedor .NET que SQL Server completo. Al probar con SQL Server Express, puede estar seguro de obtener los mismos resultados que con SQL Server. Puede usar la mayoría de las herramientas de bases de datos con SQL Server Express que puede usar con SQL Server (una excepción importante es [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) y admite otras características de SQL Server como procedimientos almacenados, vistas, desencadenadores y replicación. Sin embargo, normalmente tiene que usar SQL Server completo en un sitio web de producción. SQL Server Express puede ejecutarse en un entorno de hospedaje compartido, pero no se diseñó para eso y muchos proveedores de hospedaje no lo admiten).

Si usa Visual Studio 2012, normalmente elige SQL Server Express LocalDB para el entorno de desarrollo, ya que es lo que se instala de forma predeterminada con Visual Studio. Sin embargo, LocalDB no funciona en IIS, por lo que para el entorno de prueba tiene que usar SQL Server o SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinar bases de datos frente a mantenerlas independientes

La aplicación contoso University tiene dos bases de datos SQL Server Compact: la base de datos de pertenencia (*Aspnet. sdf*) y la base de datos de la aplicación (*School. sdf*). Al migrar, puede migrar estas bases de datos a dos bases de datos independientes o a una base de datos única. Puede combinarlos para facilitar las combinaciones de bases de datos entre la base de datos de la aplicación y la base de datos de pertenencia. El plan de hospedaje también puede proporcionar un motivo para combinarlos. Por ejemplo, el proveedor de hospedaje puede cobrar más para varias bases de datos o incluso no permitir más de una base de datos. Este es el caso de la cuenta de hospedaje de Cytanium Lite que se usa en este tutorial, que permite una sola base de datos de SQL Server.

En este tutorial, migrará las dos bases de datos de esta manera:

- Migre a dos bases de datos LocalDB en el entorno de desarrollo.
- Migre a dos bases de datos de SQL Server Express en el entorno de prueba.
- Migre a una base de datos de SQL Server completa combinada en el entorno de producción.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalación de SQL Server Express

SQL Server Express se instala automáticamente de forma predeterminada con Visual Studio 2010, pero de forma predeterminada no se instala con Visual Studio 2012. Para instalar SQL Server Express 2012, haga clic en el siguiente vínculo:

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Elija *ENU/x64/SQLEXPR\_x64\_ENU. exe* o *ENU/x86/SQLEXPR\_x86\_ENU. exe*y, en el Asistente para la instalación, acepte la configuración predeterminada. Para obtener más información acerca de las opciones de instalación, consulte [instalación de SQL Server 2012 desde el Asistente para la instalación (programa de instalación)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Crear bases de datos de SQL Server Express para el entorno de prueba

El siguiente paso consiste en crear las bases de datos de pertenencia y escuela de ASP.NET.

En el **menú Ver** , seleccione **Explorador de servidores** (**Explorador de bases de datos** en Visual Web Developer) y, a continuación, haga clic con el botón secundario en conexiones de datos y seleccione **crear nueva SQL Server base**de **datos** .

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

En el cuadro de diálogo **crear nueva SQL Server base de datos** , escriba ".\SQLExpress" en el cuadro **nombre del servidor** y "ASPNET-test" en el cuadro Nombre de la **nueva base de datos** y, a continuación, haga clic en **Aceptar**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Siga el mismo procedimiento para crear una nueva base de datos de SQL Server Express School denominada "School-test".

(Va a anexar "Test" a estos nombres de base de datos, ya que más adelante creará una instancia adicional de cada base de datos para el entorno de desarrollo y tendrá que ser capaz de diferenciar los dos conjuntos de bases de datos).

Ahora **Explorador de servidores** muestra las dos nuevas bases de datos.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Crear un script de concesión para las nuevas bases de datos

Cuando la aplicación se ejecuta en IIS en el equipo de desarrollo, la aplicación obtiene acceso a la base de datos mediante las credenciales del grupo de aplicaciones predeterminadas. Sin embargo, de forma predeterminada, la identidad del grupo de aplicaciones no tiene permiso para abrir las bases de datos. Por lo tanto, tiene que ejecutar un script para conceder ese permiso. En esta sección, creará el script que ejecutará más adelante para asegurarse de que la aplicación puede abrir las bases de datos cuando se ejecuta en IIS.

En la carpeta *SolutionFiles* de la solución que ha creado en el tutorial [implementación del entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , cree un nuevo archivo SQL denominado *Grant. SQL*. Copie los siguientes comandos SQL en el archivo y, a continuación, guarde y cierre el archivo:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Este script está diseñado para funcionar con SQL Server 2008 y con la configuración de IIS en Windows 7, tal y como se especifica en este tutorial. Si usa una versión diferente de SQL Server o de Windows, o si configura IIS en el equipo de forma diferente, es posible que se necesiten cambios en este script. Para obtener más información sobre los scripts de SQL Server, vea [libros en pantalla de SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Nota de seguridad** Este script proporciona permisos de propietario de\_dB al usuario que tiene acceso a la base de datos en tiempo de ejecución, que es lo que tendrá en el entorno de producción. En algunos escenarios, es posible que desee especificar un usuario que tenga permisos de actualización de esquemas de base de datos completos solo para la implementación y especificar en tiempo de ejecución un usuario diferente que tenga permisos únicamente para leer y escribir datos. Para obtener más información, vea **revisar los cambios automáticos de Web. config para migraciones de Code First** en [implementar en IIS como entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Configurar la implementación de base de datos para el entorno de prueba

A continuación, configurará Visual Studio para que realice las tareas siguientes para cada base de datos:

- Genere un script SQL que cree la estructura de la base de datos de origen (tablas, columnas, restricciones, etc.) en la base de datos de destino.
- Genera un script SQL que inserta los datos de la base de datos de origen en las tablas de la base de datos de destino.
- Ejecute los scripts generados y el script de concesión que creó, en la base de datos de destino.

Abra la ventana **propiedades del proyecto** y seleccione la pestaña **empaquetar/publicar SQL** .

Asegúrese de que **Active (release)** o **Release** está seleccionado en la lista desplegable **configuración** .

Haga clic en **habilitar esta página**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Normalmente, la pestaña **empaquetar/publicar SQL** está deshabilitada porque especifica un método de implementación heredado. En la mayoría de los escenarios, debe configurar la implementación de base de datos en el Asistente para **publicación web** . La migración de SQL Server Compact a SQL Server o SQL Server Express es un caso especial para el que este método es una buena elección.

Haga clic en **Importar desde web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio busca cadenas de conexión en el archivo *Web. config* , encuentra una para la base de datos de pertenencia y otra para la base de datos School, y agrega una fila que corresponde a cada cadena de conexión de la tabla de entradas de la **base de datos** . Las cadenas de conexión que encuentra son para las bases de datos de SQL Server Compact existentes y el siguiente paso será configurar cómo y dónde implementar estas bases de datos.

Escriba la configuración de la implementación de la base de datos en la sección detalles de la entrada de la **base** **de datos** . La configuración que se muestra en la sección detalles de la entrada de la **base** de datos pertenece a la fila de la tabla entradas de la **base** de datos seleccionada, tal y como se muestra en la siguiente ilustración.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurar las opciones de implementación para la base de datos de pertenencia

Seleccione la fila **DefaultConnection-Deployment** en la tabla de entradas de la **base de datos** para configurar las opciones que se aplican a la base de datos de pertenencia.

En **cadena de conexión para la base de datos de destino**, escriba una cadena de conexión que apunte a la nueva base de datos de pertenencia a SQL Server Express. Puede obtener la cadena de conexión que necesita de **Explorador de servidores**. En **Explorador de servidores**, expanda **conexiones de datos** y seleccione la base de datos **aspnetTest** y, a continuación, en la ventana **propiedades** , copie el valor de la **cadena de conexión** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Aquí se reproduce la misma cadena de conexión:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copie y pegue esta cadena de conexión en la **cadena de conexión para la base de datos de destino** en la pestaña **empaquetar/publicar SQL** .

Asegúrese de que está seleccionada la opción **extraer datos o esquema de una base de datos existente** . Esto es lo que hace que los scripts SQL se generen y se ejecuten automáticamente en la base de datos de destino.

La **cadena de conexión para el valor de la base de datos de origen** se extrae del archivo *Web. config* y apunta a la base de datos de SQL Server Compact de desarrollo. Esta es la base de datos de origen que se utilizará para generar los scripts que se ejecutarán más adelante en la base de datos de destino. Como desea implementar la versión de producción de la base de datos, cambie "aspnet-Dev. sdf" por "aspnet-Prod. sdf".

Cambie **las opciones de scripting** de la base de datos del **esquema únicamente** a **esquemas y datos**, ya que desea copiar los datos (cuentas de usuario y roles), así como la estructura de la base de datos.

Para configurar la implementación para que ejecute los scripts de concesión que creó anteriormente, tiene que agregarlos a la sección **scripts de base de datos** . Haga clic en **Agregar script**y, en el cuadro de diálogo **agregar scripts SQL** , navegue hasta la carpeta donde almacenó el script Grant (esta es la carpeta que contiene el archivo de solución). Seleccione el archivo denominado *Grant. SQL*y haga clic en **abrir**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

La configuración de la fila **DefaultConnection-Deployment** en **las entradas** de la base de datos ahora es similar a la siguiente ilustración:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de las opciones de implementación de la base de datos School

A continuación, seleccione la fila **SchoolContext-Deployment** en la tabla de entradas de la **base de datos** para configurar las opciones de implementación de la base de datos School.

Puede usar el mismo método que usó anteriormente para obtener la cadena de conexión para la nueva base de datos de SQL Server Express. Copie esta cadena de conexión en la **cadena de conexión para la base de datos de destino** en la pestaña **empaquetar/publicar SQL** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Asegúrese de que está seleccionada la opción **extraer datos o esquema de una base de datos existente** .

La **cadena de conexión para el valor de la base de datos de origen** se extrae del archivo *Web. config* y apunta a la base de datos de SQL Server Compact de desarrollo. Cambie "School-Dev. sdf" por "School-Prod. sdf" para implementar la versión de producción de la base de datos. (Nunca ha creado un archivo School-Prod. sdf en la carpeta app\_Data, por lo que copiará ese archivo desde el entorno de prueba en la carpeta app\_data de la carpeta de proyecto ContosoUniversity más adelante).

Cambiar **las opciones de scripting** de la base de **datos a esquemas y datos**.

También desea ejecutar el script para conceder permiso de lectura y escritura para esta base de datos a la identidad del grupo de aplicaciones, por lo que debe agregar el archivo de script *Grant. SQL* como hizo para la base de datos de pertenencia.

Cuando haya terminado, la configuración de la fila **SchoolContext-Deployment** en **las entradas** de la base de datos será similar a la siguiente ilustración:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Guarde los cambios en la pestaña **empaquetar/publicar SQL** .

Copie el archivo *School-Prod. sdf* de la carpeta de *datos de c:\inetpub\wwwroot\ContosoUniversity\App\_* en la carpeta *App\_Data* del proyecto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Especificar el modo de transacción para el script Grant

El proceso de implementación genera scripts que implementan el esquema y los datos de la base de datos. De forma predeterminada, estos scripts se ejecutan en una transacción. Sin embargo, los scripts personalizados (como los scripts de concesión) no se ejecutan de forma predeterminada en una transacción. Si el proceso de implementación mezcla los modos de transacción, es posible que obtenga un error de tiempo de espera cuando se ejecuten los scripts durante la implementación. En esta sección, modificará el archivo de proyecto para configurar los scripts personalizados para que se ejecuten en una transacción.

En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **ContosoUniversity** y seleccione **descargar el proyecto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

A continuación, vuelva a hacer clic con el botón derecho en el proyecto y seleccione **Editar ContosoUniversity. csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

El editor de Visual Studio muestra el contenido XML del archivo de proyecto. Observe que hay varios elementos `PropertyGroup`. (En la imagen, se ha omitido el contenido de los elementos `PropertyGroup`).

![Ventana del editor de archivos de proyecto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

La primera, que no tiene ningún atributo `Condition`, es para los valores que se aplican independientemente de la configuración de compilación. Un `PropertyGroup` elemento solo se aplica a la configuración de compilación de depuración (tenga en cuenta el atributo `Condition`), uno solo se aplica a la configuración de compilación de versión y otro solo se aplica a la configuración de compilación de prueba. En el elemento `PropertyGroup` para la configuración de compilación de versión, verá un elemento `PublishDatabaseSettings` que contiene la configuración especificada en la pestaña **empaquetar/publicar SQL** . Hay un elemento `Object` que corresponde a cada uno de los scripts de concesión especificados (observe las dos instancias de "Grant. SQL"). De forma predeterminada, se `False`el atributo `Transacted` del elemento `Source` para cada script Grant.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Cambie el valor del atributo `Transacted` del elemento `Source` a `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Guarde y cierre el archivo del proyecto y, a continuación, haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y seleccione **volver a cargar el proyecto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurar transformaciones de Web. config para las cadenas de conexión

Las cadenas de conexión para las nuevas bases de datos de SQL Express que escribió en la pestaña **empaquetar/publicar SQL** se usan en Web deploy solo para actualizar la base de datos de destino durante la implementación. Todavía tiene que configurar transformaciones de *Web. config* para que las cadenas de conexión en el archivo *Web. config* implementado apunten a las nuevas bases de datos de SQL Server Express. (Cuando se usa la pestaña **empaquetar/publicar SQL** , no se pueden configurar cadenas de conexión en el perfil de publicación).

Abra *Web. test. config* y reemplace el elemento `connectionStrings` por el elemento `connectionStrings` en el ejemplo siguiente. (Asegúrese de que solo copia el elemento connectionStrings, no el código circundante que se muestra aquí para proporcionar contexto).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Este código hace que los atributos `connectionString` y `providerName` de cada elemento `add` se reemplacen en el archivo *Web. config* implementado. Estas cadenas de conexión no son idénticas a las que escribió en la pestaña **empaquetar/publicar SQL** . Se ha agregado el valor "MultipleActiveResultSets = True" porque es necesario para el Entity Framework y el Proveedores universales.

## <a name="installing-sql-server-compact"></a>Instalación de SQL Server Compact

El paquete NuGet de Sqlservercom proporciona los ensamblados del motor de base de datos de SQL Server Compact para la aplicación contoso University. Pero ahora no es la aplicación, sino que Web Deploy que deben poder leer las bases de datos de SQL Server Compact para crear scripts que se ejecuten en las bases de datos de SQL Server. Para habilitar Web Deploy para leer las bases de datos de SQL Server Compact, instale SQL Server Compact en el equipo de desarrollo mediante el siguiente vínculo: [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Implementar en el entorno de prueba

Para publicar en el entorno de prueba, debe crear un perfil de publicación configurado para usar la pestaña **empaquetar/publicar SQL** para la publicación de bases de datos en lugar de la configuración de la base de datos de Perfil de publicación.

En primer lugar, elimine el perfil de prueba existente.

En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione la pestaña **perfil** .

Haga clic en **Administrar perfiles**.

Seleccione **prueba**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cierre el Asistente para **publicación web** para guardar este cambio.

A continuación, cree un nuevo perfil de prueba y úselo para publicar el proyecto.

En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione la pestaña **perfil** .

Seleccione **&lt;nuevo...&gt;** en la lista desplegable y escriba "Test" como nombre del perfil.

En el cuadro **dirección URL del servicio** , escriba *localhost*.

En el cuadro **sitio o aplicación** , escriba *sitio web predeterminado/ContosoUniversity*.

En el cuadro **dirección URL de destino** , escriba `http://localhost/ContosoUniversity/`.

Haga clic en **Siguiente**.

La pestaña **configuración** le advierte de que se ha configurado la pestaña **empaquetar/publicar SQL** y le ofrece la oportunidad de invalidarlos haciendo clic en habilitar las nuevas mejoras de publicación de bases de datos. En esta implementación no desea invalidar la configuración de la pestaña **empaquetar/publicar SQL** , por lo que solo tiene que hacer clic en **siguiente**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Un mensaje en la pestaña **vista previa** indica que **no hay ninguna base de datos seleccionada para publicar**, pero esto solo significa que la publicación de la base de datos no está configurada en el perfil de publicación.

Haga clic en **Publicar**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio implementa la aplicación y abre el explorador en la Página principal del sitio en el entorno de prueba. Ejecute la página instructores para ver que muestra los mismos datos que vio anteriormente. Ejecute la página **Agregar estudiantes** , agregue un estudiante nuevo y, a continuación, vea el nuevo estudiante en **la página** Students. Comprueba que se puede actualizar la base de datos. Seleccione la página **Actualizar créditos** (debe iniciar sesión) para comprobar que la base de datos de pertenencia se ha implementado y que tiene acceso a ella.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Crear una base de datos SQL Server para el entorno de producción

Ahora que ha implementado en el entorno de prueba, está listo para configurar la implementación en producción. Comienza como hizo para el entorno de prueba mediante la creación de una base de datos en la que implementar. Al recordar de la información general, el plan de hospedaje de Cytanium Lite solo permite una única base de datos de SQL Server, por lo que solo configurará una base de datos, no dos. Todas las tablas y los datos de las bases de datos de pertenencia y escuela SQL Server Compact se implementarán en una SQL Server base de datos de producción.

Vaya al panel de control de Cytanium en [http://panel.cytanium.com](http://panel.cytanium.com). Mantenga el mouse sobre **las bases de datos** y, a continuación, haga clic en **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

En la página **SQL Server 2008** , haga clic en **crear base de datos**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Asigne el nombre "School" a la base de datos y haga clic en **Guardar**. (La página agrega automáticamente el prefijo "contosou", por lo que el nombre efectivo será "contosouSchool").

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

En la misma página, haga clic en **crear usuario**. En los servidores de Cytanium, en lugar de usar la seguridad integrada de Windows y permitir que la identidad del grupo de aplicaciones Abra la base de datos, creará un usuario que tenga autoridad para abrir la base de datos. Agregará las credenciales del usuario a las cadenas de conexión que van en el archivo *Web. config* de producción. En este paso, creará esas credenciales.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Rellene los campos obligatorios en la página **propiedades de usuario SQL** :

- Escriba "ContosoUniversityUser" como nombre.
- Escriba una contraseña.
- Seleccione **contosouSchool** como la base de datos predeterminada.
- Active la casilla **contosouSchool** .

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configuración de la implementación de base de datos para el entorno de producción

Ahora está listo para establecer la configuración de implementación de la base de datos en la pestaña **empaquetar/publicar SQL** , como hizo anteriormente para el entorno de prueba.

Abra la ventana **propiedades del proyecto** , seleccione la pestaña **empaquetar/publicar SQL** y asegúrese de que la opción **activo (versión)** o **versión** está seleccionada en la lista desplegable **configuración** .

Al configurar las opciones de implementación para cada base de datos, la diferencia clave entre lo que se hace en los entornos de prueba y producción es la forma de configurar cadenas de conexión. En el entorno de prueba, escribió diferentes cadenas de conexión a la base de datos de destino, pero para el entorno de producción la cadena de conexión de destino será la misma para ambas bases de datos. Esto se debe a que está implementando ambas bases de datos en una base de datos de producción.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Configurar las opciones de implementación para la base de datos de pertenencia

Para configurar las opciones que se aplican a la base de datos de pertenencia, seleccione la fila **DefaultConnection-Deployment** en la tabla de entradas de la **base de datos** .

En **cadena de conexión para la base de datos de destino**, escriba una cadena de conexión que apunte a la nueva base de datos de producción SQL Server que acaba de crear. Puede obtener la cadena de conexión desde el correo electrónico de bienvenida. La parte relevante del correo electrónico contiene la siguiente cadena de conexión de ejemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Después de reemplazar las tres variables, la cadena de conexión que necesita es similar a la de este ejemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copie y pegue esta cadena de conexión en la **cadena de conexión para la base de datos de destino** en la pestaña **empaquetar/publicar SQL** .

Asegúrese de que la opción **extraer datos o esquema de una base de datos existente** esté seleccionada y de que **las opciones de scripting** de la base de datos siguen siendo **esquema y datos**.

En el cuadro **scripts de base de datos** , desactive la casilla situada junto al script Grant. SQL.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Configuración de las opciones de implementación de la base de datos School

A continuación, seleccione la fila **SchoolContext-Deployment** en la tabla de entradas de la **base de datos** para configurar las opciones de la base de datos School.

Copie la misma cadena de conexión en la **cadena de conexión de la base de datos de destino** que copió en ese campo para la base de datos de pertenencia.

Asegúrese de que la opción **extraer datos o esquema de una base de datos existente** esté seleccionada y de que **las opciones de scripting** de la base de datos siguen siendo **esquema y datos**.

En el cuadro **scripts de base de datos** , desactive la casilla situada junto al script Grant. SQL.

Guarde los cambios en la pestaña **empaquetar/publicar SQL** .

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Configurar transformaciones de Web. config para las cadenas de conexión a bases de datos de producción

A continuación, configurará las transformaciones de *Web. config* para que las cadenas de conexión en el archivo *Web. config* implementado apunten a la nueva base de datos de producción. La cadena de conexión que especificó en la pestaña **empaquetar/publicar SQL** para web deploy que se va a usar es la misma que la que debe usar la aplicación, a excepción de la adición de la opción `MultipleResultSets`.

Abra *Web. Production. config* y reemplace el elemento `connectionStrings` por un elemento `connectionStrings` similar al siguiente ejemplo. (Copie solo el elemento `connectionStrings`, no las etiquetas circundantes que se proporcionan para mostrar el contexto).

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

A veces se ven consejos que le indican que siempre Cifre las cadenas de conexión en el archivo *Web. config* . Esto podría ser adecuado si estuviera implementando en los servidores de la red de su empresa. Sin embargo, cuando se implementa en un entorno de hospedaje compartido, está confiando en las prácticas de seguridad del proveedor de hospedaje y no es necesario ni práctico cifrar las cadenas de conexión.

## <a name="deploying-to-the-production-environment"></a>Implementación en el entorno de producción

Ahora está listo para realizar la implementación en producción. Web Deploy leerá las bases de datos de SQL Server Compact en la carpeta *\_de datos* de la aplicación del proyecto y volverá a crear todas las tablas y los datos en la base de datos de SQL Server de producción. Para publicar mediante la configuración de la pestaña **empaquetar/publicar web** , debe crear un nuevo perfil de publicación para producción.

En primer lugar, elimine el perfil de producción existente como hizo antes el perfil de prueba.

En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione la pestaña **perfil** .

Haga clic en **Administrar perfiles**.

Seleccione **producción**, haga clic en **quitar**y, a continuación, haga clic en **cerrar**.

Cierre el Asistente para **publicación web** para guardar este cambio.

A continuación, cree un nuevo perfil de producción y úselo para publicar el proyecto.

En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.

Seleccione la pestaña **perfil** .

Haga clic en **importar**y seleccione el archivo. publishsettings que descargó anteriormente.

En la pestaña **conexión** , cambie la **dirección URL de destino** a la dirección URL temporal correcta, que en este ejemplo es http://contosouniversity.com.vserver01.cytanium.com.

Cambie el nombre del perfil a producción. (Seleccione la pestaña **perfil** y haga clic en **administrar perfiles** para hacerlo).

Cierre el Asistente para **publicación web** para guardar los cambios.

En una aplicación real en la que se estaba actualizando la base de datos en producción, haría dos pasos adicionales ahora antes de publicar:

1. Cargue *app\_offline. htm*, tal como se muestra en el tutorial [implementación del entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .
2. Use la **característica administrador de archivos** del panel de control de Cytanium para copiar los archivos *ASPNET-Prod. sdf* y *School-Prod. sdf* del sitio de producción a la carpeta *App\_Data* del proyecto ContosoUniversity. Esto garantiza que los datos que se van a implementar en la nueva base de datos de SQL Server incluyen las actualizaciones más recientes realizadas por el sitio web de producción.

En la barra de herramientas de **publicación en Web** , asegúrese de que el perfil de **producción** está seleccionado y, a continuación, haga clic en **publicar**.

Si cargó <em>app\_offline. htm</em> antes de la publicación, tendrá que usar la utilidad <strong>File Manager</strong> en el panel de control de Cytanium para eliminar la <em>aplicación\_sin conexión.</em> htm antes de la prueba. También puede eliminar al mismo tiempo los archivos <em>. sdf</em> de la carpeta <em>App\_Data</em> .

Ahora puede abrir un explorador y ir a la dirección URL de su sitio público para probar la aplicación de la misma manera que lo hizo después de implementar en el entorno de prueba.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Cambiar a SQL Server Express LocalDB en desarrollo

Como se explicó en la información general, suele ser mejor usar el mismo motor de base de datos en el desarrollo que se usa en pruebas y producción. (Recuerde que la ventaja de usar SQL Server Express en el desarrollo es que la base de datos funcionará de la misma forma en los entornos de desarrollo, prueba y producción). En esta sección, configurará el proyecto ContosoUniversity para usar SQL Server Express LocalDB al ejecutar la aplicación desde Visual Studio.

La manera más sencilla de realizar esta migración es dejar que Code First y el sistema de pertenencia creen nuevas bases de datos de desarrollo. El uso de este método para migrar requiere tres pasos:

1. Cambie las cadenas de conexión para especificar las nuevas bases de datos de SQL Express LocalDB.
2. Ejecute la herramienta Administración de sitios web para crear un usuario administrador. Esto crea la base de datos de pertenencia.
3. Use el comando Migraciones de Code First Update-Database para crear e inicializar la base de datos de la aplicación.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Actualizar las cadenas de conexión en el archivo Web. config

Abra el archivo *Web. config* y reemplace el elemento `connectionStrings` por el código siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Crear la base de datos de pertenencia

En **Explorador de soluciones**, seleccione el proyecto ContosoUniversity y, a continuación, haga clic en **configuración de ASP.net** en el menú **proyecto** .

Seleccione la pestaña Seguridad.

Haga clic en **crear o administrar roles**y, a continuación, cree un rol de **Administrador** .

Vuelva a la pestaña seguridad.

Haga clic en **crear usuario**y, a continuación, active la casilla **Administrador** y cree un usuario denominado admin.

Cierre la **herramienta Administración de sitios web**.

### <a name="creating-the-school-database"></a>Crear la base de datos School

Abra la ventana consola del administrador de paquetes.

En la lista desplegable **proyecto predeterminado** , seleccione el proyecto CONTOSOUNIVERSITY. Dal.

Escriba el comando siguiente:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migraciones de Code First aplica la migración inicial que crea la base de datos y, a continuación, aplica la migración de AddBirthDate y, a continuación, ejecuta el método de inicialización.

Ejecute el sitio presionando Ctrl + F5. Como hizo con los entornos de prueba y producción, ejecute la página **Agregar estudiantes** , agregue un estudiante nuevo y, a continuación, vea el nuevo estudiante en la página **Students** . Esto comprueba que la base de datos School se ha creado e inicializado y que tiene acceso de lectura y escritura.

Seleccione la página **Actualizar créditos** e inicie sesión para comprobar que la base de datos de pertenencia se ha implementado y que tiene acceso a ella. Si no migró las cuentas de usuario, cree una cuenta de administrador y, después, seleccione la página **Actualizar créditos** para comprobar que funciona.

## <a name="cleaning-up-sql-server-compact-files"></a>Limpiar archivos de SQL Server Compact

Ya no necesita archivos ni paquetes NuGet que se incluyeron para admitir SQL Server Compact. Si lo desea (este paso no es necesario), puede limpiar las referencias y los archivos innecesarios.

En **Explorador de soluciones**, elimine los archivos *. sdf* de la carpeta *App\_Data* y las carpetas *AMD64* y *x86* de la carpeta *bin* .

En **Explorador de soluciones**, haga clic con el botón secundario en la solución (no en uno de los proyectos) y, a continuación, haga clic en **administrar paquetes NuGet para la solución**.

En el panel izquierdo del cuadro de diálogo **administrar paquetes NuGet** , seleccione **paquetes instalados**.

Seleccione el paquete **EntityFramework. sqlservercom** y haga clic en **administrar**.

En el cuadro de diálogo **seleccionar proyectos** , se seleccionan ambos proyectos. Para desinstalar el paquete en ambos proyectos, desactive ambas casillas y, a continuación, haga clic en **Aceptar**.

En el cuadro de diálogo que le pregunta si desea desinstalar también los paquetes dependientes, haga clic en no. Uno de ellos es el Entity Framework paquete que se debe conservar.

Siga el mismo procedimiento para desinstalar el paquete **sqlservercom** . (Los paquetes deben desinstalarse en este orden porque el paquete **EntityFramework. sqlservercom** depende del paquete **sqlservercom** ).

Ahora se ha migrado correctamente a SQL Server Express y SQL Server completos. En el siguiente tutorial, realizará otro cambio en la base de datos y verá cómo implementar los cambios de la base de datos cuando las bases de datos de prueba y de producción usan SQL Server Express y SQL Server completos.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
