---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implementación web de ASP.NET con Visual Studio: preparación para la implementación de bases de datos | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618527"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implementación web de ASP.NET con Visual Studio: preparación para la implementación de bases de datos

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial se muestra cómo preparar el proyecto para la implementación de la base de datos. La estructura de la base de datos y algunos (no todos) de los datos en las dos bases de datos de la aplicación deben implementarse en entornos de prueba, de ensayo y de producción.

Normalmente, cuando se desarrolla una aplicación, los datos de prueba se escriben en una base de datos que no se desea implementar en un sitio activo. Sin embargo, es posible que también tenga algunos datos de producción que desee implementar. En este tutorial configurará el proyecto contoso University y preparará los scripts SQL para que se incluyan los datos correctos al realizar la implementación.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La aplicación de ejemplo utiliza SQL Server Express LocalDB. SQL Server Express es la edición gratuita de SQL Server. Normalmente se usa durante el desarrollo porque se basa en el mismo motor de base de datos que las versiones completas de SQL Server. Puede realizar pruebas con SQL Server Express y estar seguro de que la aplicación se comportará de la misma manera en producción, con algunas excepciones para las características que varían entre las ediciones de SQL Server.

LocalDB es un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como archivos *. MDF* . Normalmente, los archivos de base de datos LocalDB se guardan en la carpeta *App\_Data* de un proyecto Web. La característica de instancia de usuario de SQL Server Express también le permite trabajar con archivos *. MDF* , pero la característica de instancia de usuario está en desuso. por lo tanto, se recomienda LocalDB para trabajar con archivos *. MDF* .

Normalmente SQL Server Express no se utiliza para las aplicaciones Web de producción. En concreto, LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para funcionar con IIS.

En Visual Studio 2012, LocalDB se instala de forma predeterminada con Visual Studio. En Visual Studio 2010 y versiones anteriores, SQL Server Express (sin LocalDB) se instala de forma predeterminada con Visual Studio. esa es la razón por la que se instaló como uno de los requisitos previos del [primer tutorial de esta serie](introduction.md).

Para obtener más información acerca de las ediciones de SQL Server, incluido LocalDB, consulte los siguientes recursos que [trabajan con bases de datos de SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework y Proveedores universales

Para el acceso a bases de datos, la aplicación contoso University requiere el software siguiente que se debe implementar con la aplicación porque no se incluye en el .NET Framework:

- [Proveedores universales de ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que el sistema de pertenencia de ASP.NET use Azure SQL Database)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Dado que este software se incluye en los paquetes NuGet, el proyecto ya está configurado para que los ensamblados necesarios se implementen con el proyecto. (Los vínculos apuntan a las versiones actuales de estos paquetes, que pueden ser más recientes que las instaladas en el proyecto de inicio que descargó para este tutorial).

Si va a implementar en un proveedor de hospedaje de terceros en lugar de en Azure, asegúrese de que usa Entity Framework 5,0 o posterior. Las versiones anteriores de Migraciones de Code First requieren plena confianza y la mayoría de los proveedores de hospedaje ejecutarán la aplicación en confianza media. Para obtener más información sobre la confianza media, vea el tutorial [implementación en IIS como entorno de prueba](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar Migraciones de Code First para la implementación de la base de datos de aplicación

La base de datos de la aplicación contoso University está administrada por Code First y la implementará mediante Migraciones de Code First. Para obtener información general sobre la implementación de bases de datos mediante Migraciones de Code First, vea [el primer tutorial de esta serie](introduction.md).

Cuando se implementa una base de datos de aplicación, normalmente no se implementa simplemente la base de datos de desarrollo con todos los datos que contiene en el entorno de producción, ya que la mayoría de los datos que contiene probablemente solo está presente para fines de prueba. Por ejemplo, los nombres de los estudiantes de una base de datos de prueba son ficticios. Por otro lado, a menudo no se puede implementar solo la estructura de la base de datos sin ningún dato en ella. Algunos de los datos de la base de datos de prueba podrían ser datos reales y deben estar allí cuando los usuarios empiecen a usar la aplicación. Por ejemplo, la base de datos podría tener una tabla que contenga valores de calificación válidos o nombres de Departamento reales.

Para simular este escenario común, configurará un método Migraciones de Code First `Seed` que inserta en la base de datos solo los datos que desea que estén en producción. Este método `Seed` no debe insertar datos de prueba porque se ejecutará en producción después de Code First crea la base de datos en producción.

En versiones anteriores de Code First antes de que se lanzaran las migraciones, era habitual que los métodos de `Seed` insertaran los datos de prueba también, porque con cada cambio de modelo durante el desarrollo, la base de datos debía eliminarse completamente y volver a crearse desde el principio. Con Migraciones de Code First, los datos de prueba se conservan después de los cambios en la base de datos, por lo que no es necesario incluir los datos de prueba en el método `Seed`. El proyecto que ha descargado utiliza el método de incluir todos los datos en el método `Seed` de una clase de inicializador. En este tutorial deshabilitará esa clase de inicializador y habilitará las migraciones. A continuación, actualizará el método `Seed` en la clase de configuración Migrations para que inserte solo los datos que desea insertar en producción.

En el diagrama siguiente se muestra el esquema de la base de datos de la aplicación:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Para estos tutoriales, supondrá que las tablas `Student` y `Enrollment` deben estar vacías cuando se implemente el sitio por primera vez. Las demás tablas contienen datos que deben cargarse previamente cuando la aplicación se activa.

### <a name="disable-the-initializer"></a>Deshabilitar el inicializador

Puesto que va a usar Migraciones de Code First, no tiene que usar el inicializador Code First `DropCreateDatabaseIfModelChanges`. El código de este inicializador se encuentra en el archivo *SchoolInitializer.CS* en el proyecto CONTOSOUNIVERSITY. Dal. Un valor en el elemento `appSettings` del archivo *Web. config* hace que este inicializador se ejecute cada vez que la aplicación intente obtener acceso a la base de datos por primera vez:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra el archivo *Web. config* de la aplicación y quite o comente el elemento `add` que especifica la Code First clase de inicializador. Ahora, el elemento `appSettings` tiene el siguiente aspecto:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Otra manera de especificar una clase de inicializador es hacerlo llamando a `Database.SetInitializer` en el método `Application_Start` en el archivo *global. asax* . Si va a habilitar las migraciones en un proyecto que usa ese método para especificar el inicializador, quite esa línea de código.

> [!NOTE]
> Si usa Visual Studio 2013, agregue los pasos siguientes entre los pasos 2 y 3: (a) en PMC escriba "Update-package entityframework-version 6.1.1" para obtener la versión actual de EF. Después (b) compile el proyecto para obtener una lista de errores de compilación y corríjalos. Elimine las instrucciones Using para los espacios de nombres que ya no existen, haga clic con el botón derecho y haga clic en resolver para agregar instrucciones Using donde sean necesarias y cambie las apariciones de System. Data. EntityState a System. Data. Entity. EntityState.

### <a name="enable-code-first-migrations"></a>Habilitar Migraciones de Code First

1. Asegúrese de que el proyecto ContosoUniversity (no ContosoUniversity. DAL) esté establecido como proyecto de inicio. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto ContosoUniversity y seleccione **establecer como proyecto de inicio**. Migraciones de Code First buscará la cadena de conexión de base de datos en el proyecto de inicio.
2. En el menú **herramientas** , elija **Administrador de paquetes NuGet** > **consola del administrador de paquetes**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. En la parte superior de la ventana de la **consola del administrador de paquetes** , seleccione CONTOSOUNIVERSITY. Dal como el proyecto predeterminado y, a continuación, en el símbolo del sistema de `PM>`, escriba "Enable-Migrations".

    ![comando enable-Migrations](preparing-databases/_static/image4.png)

    (Si recibe un error que indica que no se reconoce el comando *enable-Migrations* , escriba el comando *Update-package EntityFramework-REINSTALL* e inténtelo de nuevo).

    Este comando crea una carpeta *Migrations* en el proyecto CONTOSOUNIVERSITY. Dal y coloca en ella dos archivos: un archivo *Configuration.CS* que puede usar para configurar las migraciones y un archivo *InitialCreate.CS* para la primera migración que crea la base de datos.

    ![Carpeta migraciones](preparing-databases/_static/image5.png)

    Ha seleccionado el proyecto DAL en la lista desplegable **proyecto predeterminado** de la **consola del administrador de paquetes** porque el comando `enable-migrations` debe ejecutarse en el proyecto que contiene la clase de contexto Code First. Cuando esa clase está en un proyecto de biblioteca de clases, Migraciones de Code First busca la cadena de conexión de base de datos en el proyecto de inicio de la solución. En la solución ContosoUniversity, el proyecto web se ha establecido como proyecto de inicio. Si no quiere designar el proyecto que tiene la cadena de conexión como proyecto de inicio en Visual Studio, puede especificar el proyecto de inicio en el comando de PowerShell. Para ver la sintaxis del comando, escriba el comando `get-help enable-migrations`.

    El comando `enable-migrations` creó automáticamente la primera migración porque la base de datos ya existe. Una alternativa es hacer que las migraciones creen la base de datos. Para ello, use **Explorador de servidores** o **Explorador de objetos de SQL Server** para eliminar la base de datos ContosoUniversity antes de habilitar las migraciones. Después de habilitar las migraciones, cree la primera migración manualmente escribiendo el comando "Add-Migration InitialCreate". Después, puede crear la base de datos escribiendo el comando "Update-Database".

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

En este tutorial, agregará datos fijos agregando código al método `Seed` de la clase Migraciones de Code First `Configuration`. Migraciones de Code First llama al método `Seed` después de cada migración.

Dado que el método de `Seed` se ejecuta después de cada migración, ya hay datos en las tablas después de la primera migración. Para controlar esta situación, usará el método `AddOrUpdate` para actualizar las filas que ya se han insertado o insertarlas si aún no existen. Es posible que el método `AddOrUpdate` no sea la mejor opción para su escenario. Para obtener más información, consulte el blog sobre cómo encargarse [con el método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julia Lerman.

1. Abra el archivo *Configuration.CS* y reemplace los comentarios en el método `Seed` por el código siguiente:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. Las referencias a `List` tienen líneas onduladas rojas en ellas porque todavía no tiene una instrucción `using` para su espacio de nombres. Haga clic con el botón secundario en una de las instancias de `List`, haga clic en **resolver**y, a continuación, haga clic en **Using System. Collections. Generic**.

    ![Resolver con la instrucción using](preparing-databases/_static/image6.png)

    Esta selección de menú agrega el código siguiente a las instrucciones `using` que se encuentran cerca de la parte superior del archivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Presione CTRL-MAYÚS-B para compilar el proyecto.

El proyecto ya está listo para implementar la base de datos *ContosoUniversity* . Después de implementar la aplicación, la primera vez que la ejecuta y navega a una página que tiene acceso a la base de datos, Code First creará la base de datos y ejecutará este método `Seed`.

> [!NOTE]
> Agregar código al método `Seed` es una de las muchas maneras en que puede insertar datos fijos en la base de datos. Una alternativa consiste en agregar código a los métodos `Up` y `Down` de cada clase de migración. Los métodos `Up` y `Down` contienen código que implementa cambios en la base de datos. Verá ejemplos de ellos en el tutorial [implementación de una actualización de base de datos](deploying-a-database-update.md) .
> 
> También puede escribir código que ejecute instrucciones SQL mediante el método `Sql`. Por ejemplo, si estuviera agregando una columna presupuesto a la tabla Department y quisiera inicializar todos los presupuestos de departamento a $1.000,00 como parte de una migración, podría agregar la siguiente línea de código al método `Up` para esa migración:
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Crear scripts para la implementación de base de datos de pertenencia

La aplicación contoso University usa el sistema de pertenencia de ASP.NET y la autenticación de formularios para autenticar y autorizar a los usuarios. La página **Actualizar créditos** solo es accesible para los usuarios que están en el rol de administrador.

Ejecute la aplicación, haga clic en **cursos**y, a continuación, haga clic en **Actualizar créditos**.

![Haga clic en actualizar créditos](preparing-databases/_static/image7.png)

La página **iniciar sesión** aparece porque la página **Actualizar créditos** requiere privilegios administrativos.

Escriba *admin* como el nombre de usuario y *devpwd* como contraseña y haga clic en **iniciar sesión**.

![Página de inicio de sesión](preparing-databases/_static/image8.png)

Aparece la página **Actualizar créditos** .

![Página actualizar créditos](preparing-databases/_static/image9.png)

La información de usuario y de rol está en la base de datos *ASPNET-ContosoUniversity* especificada por la cadena de conexión **DefaultConnection** en el archivo *Web. config* .

Entity Framework Code First no administra esta base de datos, por lo que no puede usar las migraciones para implementarla. Usará el proveedor dbDacFx para implementar el esquema de la base de datos y configurará el perfil de publicación para ejecutar un script que insertará los datos iniciales en las tablas de base de datos.

> [!NOTE]
> Se presentó un nuevo sistema de pertenencia a ASP.NET (ahora denominado ASP.NET Identity) con Visual Studio 2013. El nuevo sistema le permite conservar las tablas de aplicación y de pertenencia en la misma base de datos, y puede usar Migraciones de Code First para implementar ambas. La aplicación de ejemplo usa el sistema de pertenencia de ASP.NET anterior, que no se puede implementar mediante Migraciones de Code First. Los procedimientos para implementar esta base de datos de pertenencia también se aplican a cualquier otro escenario en el que la aplicación necesita implementar una base de datos de SQL Server que no se haya creado con Entity Framework Code First.

En este caso, normalmente no desea los mismos datos en producción que en el entorno de desarrollo. Cuando se implementa un sitio por primera vez, es habitual excluir la mayoría de las cuentas de usuario que se crean para las pruebas. Por lo tanto, el proyecto descargado tiene dos bases de datos de pertenencia: *ASPNET-ContosoUniversity. MDF* con usuarios de desarrollo y *ASPNET-ContosoUniversity-Prod. MDF* con usuarios de producción. En este tutorial, los nombres de usuario son los mismos en ambas bases de datos: *admin* y *nonadmin*. Ambos usuarios tienen la contraseña *devpwd* en la base de datos de desarrollo y *prodpwd* en la base de datos de producción.

Implementará los usuarios de desarrollo en el entorno de prueba y los usuarios de producción en ensayo y producción. Para ello, creará dos scripts SQL en este tutorial, uno para el desarrollo y otro para producción y, en los tutoriales posteriores, configurará el proceso de publicación para ejecutarlos.

> [!NOTE]
> La base de datos de pertenencia almacena un hash de contraseñas de cuenta. Para implementar cuentas de un equipo en otro, debe asegurarse de que las rutinas hash no generen diferentes hash en el servidor de destino que en el equipo de origen. Generarán los mismos valores hash al usar el Proveedores universales de ASP.NET, siempre y cuando no cambie el algoritmo predeterminado. El algoritmo predeterminado es HMACSHA256 y se especifica en el atributo de **validación** del elemento **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** en el archivo Web. config.

Puede crear scripts de implementación de datos manualmente, mediante SQL Server Management Studio (SSMS) o mediante una herramienta de terceros. En el resto de este tutorial se muestra cómo hacerlo en SSMS, pero si no desea instalar y usar SSMS, puede obtener los scripts de la versión completada del proyecto y pasar a la sección donde se almacenan en la carpeta de la solución.

Para instalar SSMS, instálelo desde el [centro de descarga: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) ; para ello, haga clic en [ENU\x64\SQLManagementStudio\_x64\_ENU. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) o [ENU\x86\SQLManagementStudio\_x86\_ENU. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Si elige una incorrecta para su sistema, no podrá instalarse y podrá probar con la otra.

(Tenga en cuenta que se trata de una descarga de 600 megabytes. Puede tardar mucho tiempo en instalarse y requerirá que se reinicie el equipo).

En la primera página del centro de instalación de SQL Server, haga clic en **nuevo SQL Server instalación independiente o agregue características a una instalación existente**, y siga las instrucciones y acepte las opciones predeterminadas.

### <a name="create-the-development-database-script"></a>Crear el script de base de datos de desarrollo

1. Ejecute SSMS.
2. En el cuadro de diálogo **conectar al servidor** , escriba *(LocalDB) \v11.0* como el **nombre del servidor**, deje **autenticación** establecida en **autenticación de Windows**y, a continuación, haga clic en **conectar**.

    ![Conectar con el servidor de SSMS](preparing-databases/_static/image10.png)
3. En la ventana de **Explorador de objetos** , expanda **bases de datos**, haga clic con el botón secundario en **ASPNET-ContosoUniversity**, haga clic en **tareas**y, a continuación, haga clic en **generar scripts**.

    ![Generar scripts de SSMS](preparing-databases/_static/image11.png)
4. En el cuadro de diálogo **generar y publicar scripts** , haga clic en **establecer opciones de scripting**.

    Puede omitir el paso **elegir objetos** porque el valor predeterminado es incluir en el **script toda la base de datos y todos los objetos de base de datos** , y eso es lo que desea.
5. Haga clic en **Avanzado**.

    ![Opciones de scripting de SSMS](preparing-databases/_static/image12.png)
6. En el cuadro de diálogo **Opciones de scripting avanzadas** , desplácese hacia abajo hasta **tipos de datos que se van a incluir**en el script y haga clic en la opción **solo datos** de la lista desplegable.
7. Cambiar **script use la base de datos** en **false**. Las instrucciones de uso no son válidas para Azure SQL Database y no son necesarias para la implementación en SQL Server Express en el entorno de prueba.

    ![Solo datos de script de SSMS, sin instrucción USE](preparing-databases/_static/image13.png)
8. Haga clic en **Aceptar**.
9. En el cuadro de diálogo **generar y publicar scripts** , el cuadro **nombre de archivo** especifica dónde se creará el script. Cambie la ruta de acceso a la carpeta de la solución (la carpeta que tiene el archivo ContosoUniversity. sln) y el nombre de archivo a *ASPNET-Data-dev. SQL*.
10. Haga clic en **siguiente** para ir a la pestaña **Resumen** y, a continuación, vuelva a hacer clic en **siguiente** para crear el script.

    ![Script de SSMS creado](preparing-databases/_static/image14.png)
11. Haga clic en **Finalizar**.

### <a name="create-the-production-database-script"></a>Crear el script de base de datos de producción

Puesto que no ha ejecutado el proyecto con la base de datos de producción, aún no se ha adjuntado a la instancia de LocalDB. Por lo tanto, debe adjuntar primero la base de datos.

1. En el **Explorador de objetos**SSMS, haga clic con el botón derecho en **bases** de datos y haga clic en **asociar**.

    ![Asociación de SSMS](preparing-databases/_static/image15.png)
2. En el cuadro de diálogo **adjuntar bases** de datos, haga clic en **Agregar** y, a continuación, desplácese hasta el archivo *ASPNET-ContosoUniversity-Prod. mdf* en la carpeta *App\_Data* .

     ![Agregar archivo. MDF de SSMS para adjuntar](preparing-databases/_static/image16.png)
3. Haga clic en **Aceptar**.
4. Siga el mismo procedimiento que usó anteriormente para crear un script para el archivo de producción. Asigne al archivo de script el nombre *ASPNET-Data-Prod. SQL*.

## <a name="summary"></a>Resumen

Ambas bases de datos ya están listas para implementarse y tiene dos scripts de implementación de datos en la carpeta de la solución.

![Scripts de implementación de datos](preparing-databases/_static/image17.png)

En el tutorial siguiente se configuran las opciones del proyecto que afectan a la implementación, y se configuran las transformaciones automáticas del archivo *Web. config* para los valores que deben ser diferentes en la aplicación implementada.

## <a name="more-information"></a>Más información

Para más información sobre NuGet, consulte [Administración de bibliotecas de proyectos con](https://msdn.microsoft.com/magazine/hh547106.aspx) la [documentación](http://docs.nuget.org/docs/start-here/overview)de Nuget y Nuget. Si no desea usar NuGet, deberá aprender a analizar un paquete de NuGet para determinar lo que hace cuando se instala. (Por ejemplo, puede configurar transformaciones de *Web. config* , configurar scripts de PowerShell para que se ejecuten en el momento de la compilación, etc.). Para obtener más información sobre cómo funciona NuGet, vea [crear y publicar un paquete y un](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) [archivo de configuración y transformaciones de código fuente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Siguiente](web-config-transformations.md)
