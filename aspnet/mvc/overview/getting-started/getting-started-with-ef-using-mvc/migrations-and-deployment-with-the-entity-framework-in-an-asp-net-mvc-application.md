---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: uso de migraciones de EF en una aplicación ASP.NET MVC e implementación en Azure'
author: tdykstra
description: En este tutorial, habilitará las migraciones de Code First e implementará la aplicación en la nube en Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499369"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Tutorial: uso de migraciones de EF en una aplicación ASP.NET MVC e implementación en Azure

Hasta ahora, la aplicación Web de ejemplo contoso University se ha ejecutado localmente en IIS Express en el equipo de desarrollo. Para que una aplicación real esté disponible para que otras personas puedan usarla a través de Internet, debe implementarla en un proveedor de hospedaje Web. En este tutorial, habilitará las migraciones de Code First e implementará la aplicación en la nube en Azure:

- Habilitar Migraciones de Code First. La característica migraciones permite cambiar el modelo de datos e implementar los cambios en producción actualizando el esquema de la base de datos sin tener que quitar y volver a crear la base de datos.
- Implemente en Azure. Este paso es opcional; puede continuar con el resto de los tutoriales sin haber implementado el proyecto.

Se recomienda utilizar un proceso de integración continua con control de código fuente para la implementación, pero en este tutorial no se tratan los temas. Para más información, consulte los capítulos sobre el [control de código fuente](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) y la [integración continua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) de la [creación de aplicaciones en la nube reales con Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

En este tutorial va a:

> [!div class="checklist"]
> * Habilitación de migraciones de Code First
> * Implementación de la aplicación en Azure (opcional)

## <a name="prerequisites"></a>Requisitos previos

- [Resistencia de la conexión e intercepción de comandos](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Habilitación de migraciones de Code First

Al desarrollar una aplicación nueva, el modelo de datos cambia con frecuencia y, cada vez que lo hace, se deja de sincronizar con la base de datos. Ha configurado el Entity Framework para quitar y volver a crear la base de datos automáticamente cada vez que cambia el modelo de datos. Al agregar, quitar o cambiar las clases de entidad o cambiar la clase `DbContext`, la próxima vez que ejecute la aplicación, se eliminará automáticamente la base de datos existente, se creará una nueva que coincida con el modelo y se inicializará con los datos de prueba.

Este método para mantener la base de datos sincronizada con el modelo de datos funciona bien hasta que la aplicación se implemente en producción. Cuando la aplicación se ejecuta en producción, normalmente almacena los datos que desea conservar y no desea perder todo cada vez que realice un cambio, como agregar una nueva columna. La característica [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) resuelve este problema habilitando Code First para actualizar el esquema de la base de datos en lugar de quitar y volver a crear la base de datos. En este tutorial, va a implementar la aplicación y a prepararse para habilitar las migraciones.

1. Deshabilite el inicializador que configuró anteriormente; para ello, agregue un comentario o elimine el elemento `contexts` que ha agregado al archivo Web. config de la aplicación.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. También en el archivo *Web. config* de la aplicación, cambie el nombre de la base de datos en la cadena de conexión a ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Este cambio configura el proyecto para que la primera migración cree una nueva base de datos. Esto no es necesario, pero verá más adelante por qué es una buena idea.
3. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

1. En el símbolo del sistema de `PM>`, escriba los siguientes comandos:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    El comando `enable-migrations` crea una carpeta *Migrations* en el proyecto ContosoUniversity y coloca en esa carpeta un archivo *Configuration.CS* que se puede editar para configurar las migraciones.

    (Si se perdió el paso anterior que le indica cambiar el nombre de la base de datos, las migraciones buscarán la base de datos existente y realizarán automáticamente el comando `add-migration`. Esto es correcto, simplemente significa que no se ejecutará ninguna prueba del código de las migraciones antes de implementar la base de datos. Más adelante, cuando ejecute el comando `update-database` no ocurrirá nada porque la base de datos ya existe.

    Abra el archivo *ContosoUniversity\Migrations\Configuration.CS* . Al igual que la clase de inicializador que vio anteriormente, la clase `Configuration` incluye un método `Seed`.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    El propósito del método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) es permitir la inserción o actualización de los datos de prueba después de Code First crea o actualiza la base de datos. Se llama al método cuando se crea la base de datos y cada vez que se actualiza el esquema de la base de datos después de un cambio en el modelo de datos.

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

Cuando se quita y se vuelve a crear la base de datos para cada cambio de modelo de datos, se usa el método `Seed` de la clase de inicializador para insertar los datos de prueba, porque después de cada cambio de modelo se quita la base de datos y se pierden todos los datos de prueba. Con Migraciones de Code First, los datos de prueba se conservan después de los cambios en la base de datos, por lo que normalmente no es necesario incluir datos de prueba en el método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) . De hecho, no desea que el método `Seed` Inserte los datos de prueba si va a usar las migraciones para implementar la base de datos en producción, ya que el método `Seed` se ejecutará en producción. En ese caso, desea que el método `Seed` Inserte en la base de datos solo los datos que necesita en producción. Por ejemplo, puede que desee que la base de datos incluya nombres de Departamento reales en la tabla `Department` cuando la aplicación esté disponible en producción.

En este tutorial, va a usar las migraciones para la implementación, pero el método de `Seed` insertará los datos de prueba para que sea más fácil ver cómo funciona la funcionalidad de la aplicación sin tener que insertar manualmente una gran cantidad de datos.

1. Reemplace el contenido del archivo *Configuration.CS* por el código siguiente, que carga los datos de prueba en la nueva base de datos.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    El método de [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) toma el objeto de contexto de base de datos como parámetro de entrada y el código del método usa ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, las agrega a la propiedad [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) adecuada y, a continuación, guarda los cambios en la base de datos. No es necesario llamar al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) después de cada grupo de entidades, como se hace aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras el código está escribiendo en la base de datos.

    Algunas de las instrucciones que insertan datos usan el método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) para realizar una operación "Upsert". Dado que el método de `Seed` se ejecuta cada vez que se ejecuta el comando `update-database`, normalmente después de cada migración, no se pueden insertar datos, ya que las filas que está intentando agregar ya estarán allí después de la primera migración que crea la base de datos. La operación "Upsert" evita errores que podrían producirse si se intenta insertar una fila que ya existe, pero ***reemplaza*** cualquier cambio en los datos que haya realizado al probar la aplicación. En el caso de los datos de prueba de algunas tablas, es posible que no desee que suceda: en algunos casos, cuando cambie los datos mientras realiza las pruebas desea que los cambios permanezcan después de las actualizaciones de la base de datos. En ese caso, desea realizar una operación de inserción condicional: Inserte una fila solo si aún no existe. El método de inicialización utiliza ambos enfoques.

    El primer parámetro que se pasa al método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica la propiedad que se va a usar para comprobar si ya existe una fila. En el caso de los datos de estudiante de prueba que se proporcionan, se puede usar la propiedad `LastName` para este propósito, ya que cada apellido de la lista es único:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Este código supone que los apellidos son únicos. Si agrega manualmente un estudiante con un apellido duplicado, recibirá la siguiente excepción la próxima vez que realice una migración:

    **La secuencia contiene más de un elemento**

    Para obtener información acerca de cómo administrar datos redundantes, como dos estudiantes denominados "Alexander Carson", consulte las bases de datos de [propagación y Depuración Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) en el blog de Rick Anderson. Para obtener más información sobre el método `AddOrUpdate`, consulte el método de encargarse [con EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julia Lerman.

    El código que crea entidades `Enrollment` supone que tiene el valor `ID` en las entidades de la colección `students`, aunque no estableció esa propiedad en el código que crea la colección.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Aquí puede usar la propiedad `ID` porque se establece el valor de `ID` cuando se llama a `SaveChanges` para la colección de `students`. EF obtiene automáticamente el valor de la clave principal cuando inserta una entidad en la base de datos y actualiza la propiedad `ID` de la entidad en la memoria.

    El código que agrega cada entidad `Enrollment` al conjunto de entidades `Enrollments` no usa el método `AddOrUpdate`. Comprueba si ya existe una entidad e inserta la entidad si no existe. Este enfoque conservará los cambios que realice en una calificación de inscripción mediante la interfaz de usuario de la aplicación. El código recorre en bucle cada miembro de la [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) de `Enrollment`y, si no se encuentra la inscripción en la base de datos, agrega la inscripción a la base de datos. La primera vez que actualice la base de datos, la base de datos estará vacía, por lo que agregará cada inscripción.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Compile el proyecto.

### <a name="execute-the-first-migration"></a>Ejecutar la primera migración

Al ejecutar el comando `add-migration`, las migraciones generaron el código que crearía la base de datos desde cero. Este código también se encuentra en la carpeta *Migrations* , en el archivo denominado *&lt;timestamp&gt;\_InitialCreate.CS*. El método `Up` de la clase `InitialCreate` crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos y el método `Down` las elimina.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`.

Se trata de la migración inicial que se creó al escribir el comando `add-migration InitialCreate`. El parámetro (`InitialCreate` en el ejemplo) se usa para el nombre de archivo y puede ser el que desee. Normalmente, se elige una palabra o frase que resume lo que se hace en la migración. Por ejemplo, puede asignar un nombre a una migración posterior &quot;AddDepartmentTable&quot;.

Si creó la migración inicial cuando la base de datos ya existía, se genera el código de creación de la base de datos pero no es necesario ejecutarlo porque la base de datos ya coincide con el modelo de datos. Al implementar la aplicación en otro entorno donde la base de datos todavía no existe, se ejecutará este código para crear la base de datos, por lo que es recomendable probarlo primero. Esta es la razón por la que cambió el nombre de la base de datos en la cadena de conexión anterior&mdash;para que las migraciones puedan crear una nueva desde cero.

1. En la ventana **Consola del Administrador de paquetas** , escriba el siguiente comando:

    `update-database`

    El comando `update-database` ejecuta el método `Up` para crear la base de datos y, a continuación, ejecuta el método `Seed` para rellenar la base de datos. El mismo proceso se ejecutará automáticamente en producción después de implementar la aplicación, como verá en la sección siguiente.
2. Use **Explorador de servidores** para inspeccionar la base de datos como hizo en el primer tutorial y ejecute la aplicación para comprobar que todo funciona igual que antes.

## <a name="deploy-to-azure"></a>Implementar en Azure

Hasta ahora, la aplicación se ejecuta localmente en IIS Express en el equipo de desarrollo. Para que esté disponible para que otras personas puedan utilizarla a través de Internet, debe implementarla en un proveedor de hospedaje Web. En esta sección del tutorial, la implementará en Azure. Esta sección es opcional. puede omitir esta opción y continuar con el siguiente tutorial, o puede adaptar las instrucciones de esta sección para un proveedor de hospedaje diferente de su elección.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Usar migraciones de Code First para implementar la base de datos

Para implementar la base de datos, usará Migraciones de Code First. Al crear el perfil de publicación que se usa para configurar las opciones de implementación desde Visual Studio, se activará la casilla **Actualizar base de datos**. Esta configuración hace que el proceso de implementación configure automáticamente el archivo *Web. config* de la aplicación en el servidor de destino para que Code First utilice la clase de inicializador `MigrateDatabaseToLatestVersion`.

Visual Studio no hace nada con la base de datos durante el proceso de implementación mientras está copiando el proyecto en el servidor de destino. Cuando se ejecuta la aplicación implementada y se obtiene acceso a la base de datos por primera vez después de la implementación, Code First comprueba si la base de datos coincide con el modelo de datos. Si hay un error de coincidencia, Code First crea automáticamente la base de datos (si aún no existe) o actualiza el esquema de la base de datos a la versión más reciente (si existe una base de datos pero no coincide con el modelo). Si la aplicación implementa un método de `Seed` migraciones, el método se ejecuta después de que se cree la base de datos o se actualice el esquema.

El método Migrations `Seed` inserta datos de prueba. Si estuviera implementando en un entorno de producción, tendría que cambiar el método de `Seed` para que solo inserte los datos que desea insertar en la base de datos de producción. Por ejemplo, en el modelo de datos actual podría querer tener cursos reales pero estudiantes ficticios en la base de datos de desarrollo. Puede escribir un método `Seed` para cargarlo tanto en el entorno de desarrollo como para, a continuación, comentar los estudiantes ficticios antes de realizar la implementación en producción. O bien, puede escribir un método `Seed` para cargar solo los cursos, y especificar los estudiantes ficticios en la base de datos de prueba manualmente mediante la interfaz de usuario de la aplicación.

### <a name="get-an-azure-account"></a>Obtener una cuenta de Azure

Necesitará una cuenta de Azure. Si aún no tiene una, pero tiene una suscripción de Visual Studio, puede [activar las ventajas de la suscripción](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). De lo contrario, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Creación de un sitio web y una base de datos SQL en Azure

La aplicación Web de Azure se ejecutará en un entorno de hospedaje compartido, lo que significa que se ejecuta en máquinas virtuales (VM) que se comparten con otros clientes de Azure. Los entornos de hospedaje compartidos ofrecen un coste reducido a los nuevos usuarios de servicios en la nube. Más adelante, si el tráfico web aumenta, es posible escalar la aplicación para atender la demanda ejecutándola en máquinas virtuales dedicadas. Para obtener más información sobre las opciones de precios de Azure App Service, consulte [precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).

Implementará la base de datos en Azure SQL Database. SQL Database es un servicio de base de datos relacional basado en la nube que se basa en SQL Server Technologies. Las herramientas y aplicaciones que funcionan con SQL Server también funcionan con SQL Database.

1. En el [portal de administración de Azure](https://portal.azure.com), elija **crear un recurso** en la pestaña de la izquierda y, a continuación, elija **ver todo** en el panel **nuevo** (o en la *hoja*) para ver todos los recursos disponibles. Elija **aplicación web + SQL** en la sección **Web** de la hoja **todo** . Por último, elija **crear**.

    ![Creación de un recurso en Azure Portal](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   Se abre el formulario para crear un nuevo recurso de **aplicación web + SQL** .

2. Escriba una cadena en el cuadro Nombre de la **aplicación** para usarla como dirección URL única para la aplicación. La dirección URL completa consistirá en lo que escriba aquí más el dominio predeterminado de App de Azure Services (. azurewebsites.net). Si ya se ha tomado el nombre de la **aplicación** , el asistente le notificará con un mensaje de color rojo *el nombre de la aplicación no está disponible* . Si el **nombre** de la aplicación está disponible, verá una marca de verificación verde.

3. En el cuadro **suscripción** , elija la suscripción de Azure en la que desea que resida el **App Service** .

4. En el cuadro de texto **grupo de recursos** , elija un grupo de recursos o cree uno nuevo. Esta configuración especifica en qué centro de datos se ejecutará el sitio Web. Para obtener más información sobre los grupos de recursos, consulte [grupos de recursos](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Cree un **plan de App Service** nuevo haciendo clic en la *sección App Service*, **cree un nuevo**y rellene **App Service Plan** (puede tener el mismo nombre que App Service), la **Ubicación**y el plan de **tarifa** (hay una opción gratuita).

6. Haga clic en **SQL Database**y, a continuación, elija **crear una nueva base de datos** o seleccionar una base de datos existente.

7. En el cuadro **nombre** , escriba un nombre para la base de datos.
8. Haga clic en el cuadro **servidor de destino** y, a continuación, seleccione **crear un nuevo servidor**. Como alternativa, si creó anteriormente un servidor, puede seleccionar ese servidor en la lista de servidores disponibles.
9. Elija la sección **plan de tarifa** , seleccione *gratis*. Si se necesitan recursos adicionales, la base de datos se puede escalar verticalmente en cualquier momento. Para obtener más información sobre los precios de Azure SQL, consulte [precios de Azure SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modifique la [Intercalación](/sql/relational-databases/collations/collation-and-unicode-support) según sea necesario.
11. Escriba un **nombre de usuario** administrador de SQL y una contraseña de administrador de **SQL**.

    - Si seleccionó **nuevo servidor de SQL Database**, defina un nuevo nombre y contraseña que usará más adelante cuando tenga acceso a la base de datos.
    - Si ha seleccionado un servidor que ha creado anteriormente, escriba las credenciales para ese servidor.

12. La colección de telemetría se puede habilitar para App Service mediante Application Insights. Con poca configuración, Application Insights recopila información valiosa sobre eventos, excepciones, dependencias, solicitudes y seguimientos. Para obtener más información acerca de Application Insights, vea [Azure monitor](https://azure.microsoft.com/services/monitor/).
13. Haga clic en **crear** en la parte inferior para indicar que ha terminado.

    El Portal de administración vuelve a la página panel y el área **notificaciones** en la parte superior de la página muestra que se está creando el sitio. Después de un tiempo (normalmente menos de un minuto), hay una notificación de que la implementación se realizó correctamente. En la barra de navegación de la izquierda, el nuevo App Service aparece en la sección **App Services** y la nueva base de datos SQL aparece en la sección bases de datos **SQL** .

### <a name="deploy-the-app-to-azure"></a>Implementación de la aplicación en Azure

1. En Visual Studio, haga clic con el botón derecho en el proyecto, en el **Explorador de soluciones** y seleccione **Publicar** en el menú contextual.

2. En la página **seleccionar un destino de publicación** , elija **App Service** y, a continuación, **Seleccione existente**y, a continuación, elija **publicar**.

    ![Seleccionar una página de destino de publicación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Si no ha agregado previamente su suscripción de Azure en Visual Studio, siga los pasos de la pantalla. Estos pasos permiten que Visual Studio se conecte a su suscripción de Azure para que la lista de **App Services** incluya el sitio Web.

4. En la página **App Service** , seleccione la **suscripción** a la que ha agregado el App Service. En **Ver**, seleccione **grupo de recursos**. Expanda el grupo de recursos al que agregó el App Service y, a continuación, seleccione el App Service. Elija **Aceptar** para publicar la aplicación.

5. La ventana **Salida** muestra qué acciones de implementación se realizaron e informa de la correcta finalización de la implementación.

6. Tras una implementación correcta, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    La aplicación se ejecuta ahora en la nube.

En este momento, la base de datos *SchoolContext* se ha creado en la base de datos SQL de Azure porque seleccionó **Ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)** . El archivo *Web. config* del sitio Web implementado se ha cambiado para que el inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) se ejecute la primera vez que el código lea o escriba datos en la base de datos (que se produjo al seleccionar la pestaña **Students** ):

![Extracto del archivo Web. config](https://asp.net/media/4367421/mig.png)

El proceso de implementación también ha creado una nueva cadena de conexión *(SchoolContext\_DatabasePublish*) para que migraciones de Code First la pueda usar para actualizar el esquema de la base de datos y para inicializar la base de datos.

![Cadena de conexión en el archivo Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Puede encontrar la versión implementada del archivo Web. config en su propio equipo en *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Puede tener acceso al propio archivo *Web. config* implementado mediante FTP. Para obtener instrucciones, consulte [ASP.net web Deployment Using Visual Studio: Deploying a Code Update](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Siga las instrucciones que comienzan con "para usar una herramienta de FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña".

> [!NOTE]
> La aplicación web no implementa la seguridad, por lo que cualquiera que encuentre la dirección URL puede cambiar los datos. Para obtener instrucciones sobre cómo proteger el sitio web, vea [implementación de una aplicación ASP.NET MVC segura con suscripción, OAuth y SQL Database en Azure](/aspnet/core/security/authorization/secure-data). Puede impedir que otras personas usen el sitio al detener el servicio con Azure Portal de administración o **Explorador de servidores** en Visual Studio.

![Detener elemento de menú de App Service](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Escenarios de migraciones avanzadas

Si implementa una base de datos mediante la ejecución de migraciones automáticamente, tal y como se muestra en este tutorial, y está implementando en un sitio web que se ejecuta en varios servidores, puede obtener varios servidores que intenten ejecutar migraciones al mismo tiempo. Las migraciones son atómicas, por lo que si dos servidores intentan ejecutar la misma migración, una se realizará correctamente y la otra producirá un error (suponiendo que las operaciones no se pueden realizar dos veces). En ese escenario, si desea evitar estos problemas, puede llamar a las migraciones manualmente y configurar su propio código para que solo se produzca una vez. Para obtener más información, consulte [ejecución y scripting de migraciones desde código](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) en el blog de Rowan Miller y [Migrate. exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (para ejecutar migraciones desde la línea de comandos).

Para obtener información sobre otros escenarios de migración, consulte la [serie de presentaciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)en pantalla de migraciones.

## <a name="update-specific-migration"></a>Actualización de la migración específica

`update-database -target MigrationName`

El comando `update-database -target MigrationName` ejecuta la migración de destino.

## <a name="ignore-migration-changes-to-database"></a>Omitir cambios de migración en la base de datos

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges` crea una migración vacía con el modelo actual como una instantánea.

## <a name="code-first-initializers"></a>Inicializadores de Code First

En la sección implementación, vio el inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) que se usaba. Code First también proporciona otros inicializadores, como [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (el valor predeterminado), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que usó anteriormente) y [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). El inicializador de `DropCreateAlways` puede ser útil para configurar las condiciones de las pruebas unitarias. También puede escribir sus propios inicializadores, y puede llamar a un inicializador explícitamente si no desea esperar hasta que la aplicación lea o escriba en la base de datos.

Para obtener más información sobre los inicializadores, vea [Descripción de los inicializadores de base de datos en Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) y el capítulo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) by Julia Lerman y Rowan Miller.

## <a name="get-the-code"></a>Obtención del código

[Descargar el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en [el acceso a datos ASP.net: recursos recomendados](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Migraciones de Code First habilitadas
> * Implementación de la aplicación en Azure (opcional)

Avance al siguiente artículo para aprender a crear un modelo de datos más complejo para una aplicación ASP.NET MVC.
> [!div class="nextstepaction"]
> [Crear un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
