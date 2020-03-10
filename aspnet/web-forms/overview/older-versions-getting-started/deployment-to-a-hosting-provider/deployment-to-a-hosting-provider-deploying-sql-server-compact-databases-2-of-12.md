---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación de bases de datos SQL Server Compacts: 2 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78458257"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementación de bases de datos SQL Server Compacts: 2 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo configurar dos bases de datos de SQL Server Compact y el motor de base de datos para la implementación.

Para el acceso a bases de datos, la aplicación contoso University requiere el software siguiente que se debe implementar con la aplicación porque no se incluye en el .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (el motor de base de datos).
- [Proveedores universales de ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (que permiten que el sistema de pertenencia de ASP.NET use SQL Server Compact)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First con migraciones).

También se deben implementar la estructura de la base de datos y algunos (no todos) de los datos en las dos bases de datos de la aplicación. Normalmente, cuando se desarrolla una aplicación, los datos de prueba se escriben en una base de datos que no se desea implementar en un sitio activo. Sin embargo, también puede especificar algunos datos de producción que desee implementar. En este tutorial configurará el proyecto contoso University para que el software necesario y los datos correctos se incluyan al realizar la implementación.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact frente a SQL Server Express

La aplicación de ejemplo utiliza SQL Server Compact 4,0. Este motor de base de datos es una opción relativamente nueva para sitios web; las versiones anteriores de SQL Server Compact no funcionan en un entorno de hospedaje Web. SQL Server Compact ofrece algunas ventajas en comparación con el escenario más común de desarrollo con SQL Server Express e implementación en SQL Server completo. En función del proveedor de hospedaje que elija, SQL Server Compact podría ser más barato de implementar, porque algunos proveedores cobran extra para admitir una base de datos de SQL Server completa. No hay ningún cargo adicional por SQL Server Compact porque puede implementar el propio motor de base de datos como parte de la aplicación Web.

Sin embargo, también debe tener en cuenta sus limitaciones. SQL Server Compact no admite procedimientos almacenados, desencadenadores, vistas o replicación. (Para obtener una lista completa de las características SQL Server que no son compatibles con SQL Server Compact, vea [diferencias entre SQL Server Compact y SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)). Además, algunas de las herramientas que puede usar para manipular esquemas y datos en SQL Server Express y en las bases de datos de SQL Server no funcionan con SQL Server Compact. Por ejemplo, no puede usar SQL Server Management Studio o SQL Server Data Tools en Visual Studio con bases de datos de SQL Server Compact. Tiene otras opciones para trabajar con bases de datos de SQL Server Compact:

- Puede usar Explorador de servidores en Visual Studio, que ofrece una funcionalidad limitada de manipulación de bases de datos para SQL Server Compact.
- Puede usar la característica de manipulación de bases de datos de [WebMatrix](https://www.microsoft.com/web/webmatrix/), que tiene más características que explorador de servidores.
- Puede usar herramientas de código abierto o de terceros con una gran cantidad de características, como el [cuadro de herramientas de SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) y la [utilidad de script de esquema y datos de SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Puede escribir y ejecutar sus propios scripts DDL (lenguaje de definición de datos) para manipular el esquema de la base de datos.

Puede empezar por SQL Server Compact y, a continuación, actualizarse más tarde a medida que evolucionen sus necesidades. En los tutoriales posteriores de esta serie se muestra cómo migrar de SQL Server Compact a SQL Server Express y SQL Server. Sin embargo, si va a crear una nueva aplicación y espera necesitar SQL Server en un futuro próximo, probablemente sea mejor empezar con SQL Server o SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configuración del Motor de base de datos de SQL Server Compact para la implementación

El software necesario para el acceso a los datos en la aplicación contoso University se ha agregado mediante la instalación de los siguientes paquetes NuGet:

- [Sqlservercom](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (proveedores universales de ASP.net)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. Sqlservercom](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Los vínculos apuntan a las versiones actuales de estos paquetes, que pueden ser más recientes que las instaladas en el proyecto de inicio que descargó para este tutorial. Para la implementación en un proveedor de hospedaje, asegúrese de que usa Entity Framework 5,0 o posterior. Las versiones anteriores de Migraciones de Code First requieren plena confianza y, en muchos proveedores de hospedaje, la aplicación se ejecutará en un nivel de confianza medio. Para obtener más información sobre la confianza media, vea el tutorial [implementar en IIS como entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

En general, la instalación de paquetes NuGet se encarga de todo lo que necesita para implementar este software con la aplicación. En algunos casos, esto implica tareas como cambiar el archivo Web. config y agregar scripts de PowerShell que se ejecutan cada vez que se compila la solución. **Si desea agregar compatibilidad con cualquiera de estas características (como SQL Server Compact y Entity Framework) sin usar NuGet, asegúrese de que sabe lo que hace la instalación del paquete de NuGet para que pueda hacer el mismo trabajo manualmente.**

Existe una excepción en la que NuGet no se ocupa de todo lo que hay que hacer para garantizar una implementación correcta. El paquete NuGet Sqlservercom agrega un script posterior a la compilación al proyecto que copia los ensamblados nativos en las subcarpetas *x86* y *AMD64* en la carpeta *bin* del proyecto, pero el script no incluye esas carpetas en el proyecto. Como resultado, Web Deploy no los copiará en el sitio web de destino a menos que los incluya manualmente en el proyecto. (Este comportamiento es el resultado de la configuración de implementación predeterminada; otra opción, que no usará en estos tutoriales, es cambiar la configuración que controla este comportamiento. La configuración que se puede cambiar es **solo los archivos necesarios para ejecutar la aplicación** en **elementos que se van a implementar** en la pestaña **empaquetar/publicar web** de la ventana **propiedades del proyecto** . Normalmente no se recomienda cambiar esta configuración, ya que puede dar lugar a la implementación de muchos más archivos en el entorno de producción del que se necesitan allí. Para obtener más información sobre las alternativas, vea el tutorial [configuración de las propiedades del proyecto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) ).

Compile el proyecto y, a continuación, en **Explorador de soluciones** haga clic en **Mostrar todos los archivos** si aún no lo ha hecho. También puede que tenga que hacer clic en **Actualizar**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Expanda la carpeta **bin** para ver las carpetas **AMD64** y **x86** y, a continuación, seleccione esas carpetas, haga clic con el botón derecho y seleccione **incluir en el proyecto**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Los iconos de carpeta cambian para mostrar que la carpeta se ha incluido en el proyecto.

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configuración de Migraciones de Code First para la implementación de base de datos de aplicación

Cuando se implementa una base de datos de aplicación, normalmente no se implementa simplemente la base de datos de desarrollo con todos los datos que contiene en el entorno de producción, ya que la mayoría de los datos que contiene probablemente solo está presente para fines de prueba. Por ejemplo, los nombres de los estudiantes de una base de datos de prueba son ficticios. Por otro lado, a menudo no se puede implementar solo la estructura de la base de datos sin ningún dato en ella. Algunos de los datos de la base de datos de prueba podrían ser datos reales y deben estar allí cuando los usuarios empiecen a usar la aplicación. Por ejemplo, la base de datos podría tener una tabla que contenga valores de calificación válidos o nombres de Departamento reales.

Para simular este escenario común, configurará un método de inicialización Migraciones de Code First que inserta en la base de datos solo los datos que desea que estén en producción. Este método de inicialización no insertará datos de prueba porque se ejecutará en producción después de Code First crea la base de datos en producción.

En versiones anteriores de Code First antes de que se lanzaran las migraciones, era habitual que los métodos de inicialización insertaran los datos de prueba, ya que con cada cambio de modelo durante el desarrollo, la base de datos debía eliminarse completamente y volver a crearse desde el principio. Con Migraciones de Code First, los datos de prueba se conservan después de los cambios en la base de datos, por lo que no es necesario incluir los datos de prueba en el método de inicialización. El proyecto que descargó usa el método previo a la migración para incluir todos los datos en el método de inicialización de una clase de inicializador. En este tutorial deshabilitará la clase de inicializador y habilitará las migraciones. A continuación, actualizará el método de inicialización en la clase de configuración de migraciones para que inserte solo los datos que desea insertar en producción.

En el diagrama siguiente se muestra el esquema de la base de datos de la aplicación:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Para estos tutoriales, supondrá que las tablas `Student` y `Enrollment` deben estar vacías cuando se implemente el sitio por primera vez. Las demás tablas contienen datos que deben cargarse previamente cuando la aplicación se activa.

Puesto que va a usar Migraciones de Code First, ya no tiene que usar el inicializador de Code First **DropCreateDatabaseIfModelChanges** . El código de este inicializador se encuentra en el archivo SchoolInitializer.cs en el proyecto ContosoUniversity. DAL. Un valor en el elemento **appSettings** del archivo Web. config hace que este inicializador se ejecute cada vez que la aplicación intente obtener acceso a la base de datos por primera vez:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Abra el archivo Web. config de la aplicación y quite el elemento que especifica la Code First clase de inicializador del elemento appSettings. El elemento appSettings tiene ahora el siguiente aspecto:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Otra manera de especificar una clase de inicializador es hacerlo llamando a `Database.SetInitializer` en el método `Application_Start` en el archivo *global. asax* . Si va a habilitar las migraciones en un proyecto que usa ese método para especificar el inicializador, quite esa línea de código.

A continuación, habilite Migraciones de Code First.

El primer paso es asegurarse de que el proyecto ContosoUniversity está establecido como proyecto de inicio. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto ContosoUniversity y seleccione **establecer como proyecto de inicio**. Migraciones de Code First buscará la cadena de conexión de base de datos en el proyecto de inicio.

En el menú **Tools** (Herramientas), haga clic en **Administrador de paquetes NuGet** y luego en **Consola del Administrador de paquetes**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

En la parte superior de la ventana de la **consola del administrador de paquetes** , seleccione CONTOSOUNIVERSITY. Dal como el proyecto predeterminado y, a continuación, en el símbolo del sistema de `PM>`, escriba "Enable-Migrations".

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Este comando crea un archivo *Configuration.CS* en una nueva carpeta *Migrations* en el proyecto ContosoUniversity. Dal.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Ha seleccionado el proyecto DAL porque el comando "Enable-Migrations" debe ejecutarse en el proyecto que contiene la clase de contexto Code First. Cuando esa clase está en un proyecto de biblioteca de clases, Migraciones de Code First busca la cadena de conexión de base de datos en el proyecto de inicio de la solución. En la solución ContosoUniversity, el proyecto web se ha establecido como proyecto de inicio. (Si no quiere designar el proyecto que tiene la cadena de conexión como proyecto de inicio en Visual Studio, puede especificar el proyecto de inicio en el comando de PowerShell. Para ver la sintaxis de comando para el comando enable-Migrations, puede escribir el comando "Get-Help enable-Migrations").

Abra el archivo Configuration.cs y reemplace los comentarios en el método `Seed` por el código siguiente:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

Las referencias a `List` tienen líneas onduladas rojas en ellas porque todavía no tiene una instrucción `using` para su espacio de nombres. Haga clic con el botón secundario en una de las instancias de `List`, haga clic en **resolver**y, a continuación, haga clic en **Using System. Collections. Generic**.

![Resolver con la instrucción using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Esta selección de menú agrega el código siguiente a las instrucciones `using` que se encuentran cerca de la parte superior del archivo.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Agregar código al método `Seed` es una de las muchas maneras en que puede insertar datos fijos en la base de datos. Una alternativa consiste en agregar código a los métodos `Up` y `Down` de cada clase de migración. Los métodos `Up` y `Down` contienen código que implementa cambios en la base de datos. Verá ejemplos de ellos en el tutorial [implementación de una actualización de base de datos](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> También puede escribir código que ejecute instrucciones SQL mediante el método `Sql`. Por ejemplo, si estuviera agregando una columna presupuesto a la tabla Department y quisiera inicializar todos los presupuestos de departamento a $1.000,00 como parte de una migración, podría agregar la siguiente línea de código al método `Up` para esa migración:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> En este ejemplo se muestra en este tutorial se usa el método `AddOrUpdate` en el método `Seed` de la clase Migraciones de Code First `Configuration`. Migraciones de Code First llama al método `Seed` después de cada migración y este método actualiza las filas que ya se han insertado o las inserta si aún no existen. Es posible que el método `AddOrUpdate` no sea la mejor opción para su escenario. Para obtener más información, consulte el blog sobre cómo encargarse [con el método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julia Lerman.

Presione CTRL-MAYÚS-B para compilar el proyecto.

El siguiente paso consiste en crear una clase `DbMigration` para la migración inicial. Desea que esta migración cree una nueva base de datos, por lo que debe eliminar la base de datos que ya existe. SQL Server Compact bases de datos están contenidas en archivos *. sdf* en la carpeta *App\_Data* . En **Explorador de soluciones**, expanda *App\_Data* en el proyecto ContosoUniversity para ver las dos bases de datos SQL Server Compact, que se representan mediante archivos *. sdf* .

Haga clic con el botón derecho en el archivo *School. sdf* y haga clic en **eliminar**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

En la ventana de la **consola del administrador de paquetes** , escriba el comando "Add-Migration Initial" para crear la migración inicial y asígnele el nombre "Initial".

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migraciones de Code First crea otro archivo de clase en la carpeta *Migrations* y esta clase contiene código que crea el esquema de la base de datos.

En la **consola del administrador de paquetes**, escriba el comando "Update-Database" para crear la base de datos y ejecutar el método de **inicialización** .

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Si recibe un error que indica que una tabla ya existe y no se puede crear, es probable que haya ejecutado la aplicación después de eliminar la base de datos y antes de ejecutar `update-database`. En ese caso, vuelva a eliminar el archivo *School. sdf* y vuelva a intentar el comando `update-database`).

Ejecute la aplicación. Ahora la página Students está vacía pero la página Instructors contiene instructores. Esto es lo que obtendrá en producción después de implementar la aplicación.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

El proyecto ya está listo para implementar la base de datos *School* .

## <a name="creating-a-membership-database-for-deployment"></a>Crear una base de datos de pertenencia para la implementación

La aplicación contoso University usa el sistema de pertenencia de ASP.NET y la autenticación de formularios para autenticar y autorizar a los usuarios. Una de sus páginas solo es accesible para los administradores. Para ver esta página, ejecute la aplicación y seleccione **Actualizar créditos** en el menú desplegable en **cursos**. La aplicación muestra la página **de inicio de sesión** , ya que solo los administradores tienen autorización para usar la página **Actualizar créditos** .

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Inicie sesión como "admin" con la contraseña "Pas $ w0rd" (observe el número cero en lugar de la letra "o" en "w0rd"). Después de iniciar sesión, se muestra la página **Actualizar créditos** .

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Cuando se implementa un sitio por primera vez, es habitual excluir la mayoría de las cuentas de usuario que se crean para las pruebas. En este caso, implementará una cuenta de administrador y sin cuentas de usuario. En lugar de eliminar manualmente las cuentas de prueba, creará una nueva base de datos de pertenencia que solo tiene una cuenta de usuario de administrador que necesita en producción.

> [!NOTE]
> La base de datos de pertenencia almacena un hash de contraseñas de cuenta. Para implementar cuentas de un equipo en otro, debe asegurarse de que las rutinas hash no generen diferentes hash en el servidor de destino que en el equipo de origen. Generarán los mismos valores hash al usar el Proveedores universales de ASP.NET, siempre y cuando no cambie el algoritmo predeterminado. El algoritmo predeterminado es HMACSHA256 y se especifica en el atributo de **validación** del elemento **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** en el archivo Web. config.

Migraciones de Code First no mantiene la base de datos de pertenencia y no hay ningún inicializador automático que inicialice la base de datos con cuentas de prueba (como hay para la base de datos School). Por lo tanto, para mantener los datos de prueba disponibles, realizará una copia de la base de datos de prueba antes de crear una nueva.

En **Explorador de soluciones**, cambie el nombre del archivo *Aspnet. sdf* en la carpeta *App\_Data* a *ASPNET-dev. sdf*. (No haga una copia, simplemente cambie el nombre, ya que creará una nueva base de datos en un momento).

En **Explorador de soluciones**, asegúrese de que el proyecto web (ContosoUniversity, no CONTOSOUNIVERSITY. Dal) está seleccionado. A continuación, en el menú **proyecto** , seleccione **configuración de ASP.net** para ejecutar la herramienta de **Administración de sitios web**(Wat).

Seleccione la pestaña **Seguridad**.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Haga clic en **crear o administrar roles** y agregar un rol de **Administrador** .

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Vuelva a la pestaña **seguridad** , haga clic en **crear usuario**y agregue el usuario "admin" como administrador. Antes de hacer clic en el botón **crear usuario** de la página **crear usuario** , asegúrese de activar la casilla de verificación **Administrador** . La contraseña que se usa en este tutorial es "Pas $ w0rd" y puede escribir cualquier dirección de correo electrónico.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Cierre el explorador. En **Explorador de soluciones**, haga clic en el botón actualizar para ver el nuevo archivo *Aspnet. sdf* .

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Haga clic con el botón derecho en **Aspnet. sdf** y seleccione **incluir en el proyecto**.

## <a name="distinguishing-development-from-production-databases"></a>Distinguir el desarrollo de las bases de datos de producción

En esta sección, cambiará el nombre de las bases de datos para que las versiones de desarrollo sean School-Dev. sdf y aspnet-Dev. sdf y las versiones de producción sean School-Prod. sdf y aspnet-Prod. SDF. Esto no es necesario, pero hacerlo le ayudará a evitar que se confundan las versiones de prueba y producción de las bases de datos.

En **Explorador de soluciones**, haga clic en **Actualizar** y expanda la carpeta app\_Data para ver la base de datos School que creó anteriormente. Haga clic con el botón derecho en él y seleccione **incluir en el proyecto**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Cambie el nombre de *Aspnet. sdf* a *ASPNET-Prod. sdf*.

Cambie el nombre de *School. sdf* a *School-dev. sdf*.

Cuando ejecute la aplicación en Visual Studio, no querrá usar *las versiones de los archivos* de la base de datos, ya que quiere usar las versiones *de-dev* . Por lo tanto, debe cambiar las cadenas de conexión en el archivo Web. config para que señalen a las versiones de *desarrollo* de las bases de datos. (No ha creado un archivo School-Prod. SDF, pero es correcto porque Code First creará la base de datos en producción la primera vez que ejecute la aplicación).

Abra el archivo Web. config de la aplicación y busque las cadenas de conexión:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Cambie "Aspnet. sdf" a "aspnet-Dev. sdf" y cambie "School. sdf" por "School-Dev. sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

El motor de base de datos de SQL Server Compact y las dos bases de datos ya están listas para su implementación. En el tutorial siguiente se configuran las transformaciones automáticas del archivo *Web. config* para los valores que deben ser diferentes en los entornos de desarrollo, prueba y producción. (Entre las opciones que se deben cambiar se encuentran las cadenas de conexión, pero se configurarán los cambios más adelante al crear un perfil de publicación).

## <a name="more-information"></a>Más información

Para más información sobre NuGet, consulte [Administración de bibliotecas de proyectos con](https://msdn.microsoft.com/magazine/hh547106.aspx) la [documentación](http://docs.nuget.org/docs/start-here/overview)de Nuget y Nuget. Si no desea usar NuGet, deberá aprender a analizar un paquete de NuGet para determinar lo que hace cuando se instala. (Por ejemplo, puede configurar transformaciones de *Web. config* , configurar scripts de PowerShell para que se ejecuten en el momento de la compilación, etc.). Para obtener más información sobre cómo funciona NuGet, vea especialmente [crear y publicar un paquete y un](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) [archivo de configuración y transformaciones de código fuente](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
