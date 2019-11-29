---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implementación web de ASP.NET con Visual Studio: implementación de una actualización de base de datos | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636829"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Implementación web de ASP.NET con Visual Studio: implementar una actualización de base de datos

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial, realizará un cambio en la base de datos y los cambios en el código relacionado, probará los cambios en Visual Studio y, a continuación, implementará la actualización en los entornos de prueba, ensayo y producción.

En primer lugar, el tutorial muestra cómo actualizar una base de datos administrada por Migraciones de Code First y, posteriormente, muestra cómo actualizar una base de datos mediante el proveedor dbDacFx.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Implementar una actualización de base de datos mediante Migraciones de Code First

En esta sección, agregará una columna de fecha de nacimiento a la clase base `Person` para las entidades `Student` y `Instructor`. A continuación, actualice la página que muestra los datos del instructor para que muestren la nueva columna. Por último, implemente los cambios de prueba, ensayo y producción.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Agregar una columna a una tabla en la base de datos de la aplicación

1. En el proyecto *ContosoUniversity. Dal* , Abra *Person.CS* y agregue la siguiente propiedad al final de la clase `Person` (debe haber dos llaves de cierre después):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    A continuación, actualice el método de `Seed` para que proporcione un valor para la nueva columna. Abra *Migrations\Configuration.CS* y reemplace el bloque de código que comienza `var instructors = new List<Instructor>` por el siguiente bloque de código que incluye la información de fecha de nacimiento:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compile la solución y, a continuación, abra la ventana **consola del administrador de paquetes** . Asegúrese de que ContosoUniversity. DAL sigue seleccionado como **proyecto predeterminado**.
3. En la ventana de la **consola del administrador de paquetes** , seleccione **ContosoUniversity. Dal** como **proyecto predeterminado**y, a continuación, escriba el siguiente comando:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva clase de `DbMigration` y, en el método `Up`, puede ver el código que crea la nueva columna. El método `Up` crea la columna cuando se está implementando el cambio y el método `Down` elimina la columna cuando se revierte el cambio.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compile la solución y, a continuación, escriba el siguiente comando en la ventana de la **consola del administrador de paquetes** (Asegúrese de que el proyecto CONTOSOUNIVERSITY. Dal todavía está seleccionado):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    El Entity Framework ejecuta el método `Up` y, a continuación, ejecuta el método `Seed`.

### <a name="display-the-new-column-in-the-instructors-page"></a>Mostrar la nueva columna en la página de instructores

1. En el proyecto ContosoUniversity, Abra *instructors. aspx* y agregue un nuevo campo plantilla para mostrar la fecha de nacimiento. Agréguelo entre los de la fecha de contratación y la asignación de la oficina:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Si la sangría de código no está sincronizada, puede presionar CTRL-K y, a continuación, CTRL + D para cambiar el formato del archivo automáticamente).
2. Ejecute la aplicación y haga clic en el vínculo **Instructors** .

    Cuando se cargue la página, verá que tiene el nuevo campo de fecha de nacimiento.

    ![Página de instructores con FechaNacimiento](deploying-a-database-update/_static/image2.png)
3. Cierre el explorador.

### <a name="deploy-the-database-update"></a>Implementar la actualización de la base de datos

1. En **Explorador de soluciones** Seleccione el proyecto ContosoUniversity.
2. En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web**. (Si la barra de herramientas está deshabilitada, seleccione el proyecto ContosoUniversity en **Explorador de soluciones**).

    Visual Studio implementa la aplicación actualizada y el explorador se abre en la Página principal.
3. Ejecute la página **instructores** para comprobar que la actualización se ha implementado correctamente.

    Cuando la aplicación intenta obtener acceso a la base de datos de esta página, Code First actualiza el esquema de la base de datos y ejecuta el método `Seed`. Cuando se muestre la página, verá la columna **fecha de nacimiento** esperada con fechas.
4. En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **ensayo** y, a continuación, haga clic en **publicar web**.
5. Ejecute la página de **instructores** en el almacenamiento provisional para comprobar que la actualización se ha implementado correctamente.
6. En la barra de herramientas de **publicación en Web** , haga clic en el perfil de publicación de **producción** y, a continuación, haga clic en **publicar web**.
7. Ejecute la página de **instructores** en producción para comprobar que la actualización se ha implementado correctamente.

    En el caso de una actualización de la aplicación de producción real que incluye un cambio en la base de datos, normalmente la aplicación también se desconecta durante la implementación mediante *app\_offline. htm*, como se vio en el tutorial anterior.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Implementar una actualización de base de datos mediante el proveedor dbDacFx

En esta sección, agregará una columna de *comentarios* a la tabla de *usuario* en la base de datos de pertenencia y creará una página que le permitirá mostrar y editar los comentarios de cada usuario. Después, implemente los cambios de prueba, ensayo y producción.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Agregar una columna a una tabla en la base de datos de pertenencia

1. En Visual Studio, Abra **Explorador de objetos de SQL Server**.
2. Expanda **(LocalDB) \v11.0**, expanda **bases de datos**, expanda **ASPNET-ContosoUniversity** (no **ASPNET-ContosoUniversity-Prod**) y, a continuación, expanda **tablas**.

    Si no ve **(LocalDB) \v11.0** en el nodo **SQL Server** , haga clic con el botón secundario en el nodo **SQL Server** y haga clic en **Agregar SQL Server**. En el cuadro de diálogo **conectar al servidor** , escriba *(LocalDB) \v11.0* como el **nombre del servidor**y, a continuación, haga clic en **conectar**.

    Si no ve **ASPNET-ContosoUniversity**, ejecute el proyecto e inicie sesión con las credenciales de *Administrador* (la contraseña es *devpwd*) y, a continuación, actualice la ventana **Explorador de objetos de SQL Server** .
3. Haga clic con el botón secundario en la tabla **usuarios** y, a continuación, haga clic en **Diseñador de vistas**.

    ![Diseñador de vistas SSOX](deploying-a-database-update/_static/image3.png)
4. En el diseñador, agregue una columna *comentarios* y escríbala en *nvarchar (128)* y acepte valores NULL y, a continuación, haga clic en **Actualizar**.

    ![Agregar una columna de comentarios](deploying-a-database-update/_static/image4.png)
5. En el cuadro **vista previa de actualizaciones de base de datos** , haga clic en **Actualizar base de datos**.

    ![Vista previa de actualizaciones de base de datos](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Crear una página para mostrar y editar la nueva columna

1. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta **account** del proyecto ContosoUniversity, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.
2. Cree un nuevo **formulario Web Forms con la página maestra** y asígnele el nombre *UserInfo. aspx*. Acepte el archivo *site. Master* predeterminado como la página maestra.
3. Copie el marcado siguiente en el elemento `MainContent` `Content` (el último de los tres elementos `Content`):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Haga clic con el botón secundario en la página *UserInfo. aspx* y haga clic en **ver en el explorador**.
5. Inicie sesión con sus credenciales de usuario de *Administrador* (la contraseña es *devpwd*) y agregue algunos comentarios a un usuario para comprobar que la página funciona correctamente.

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. Cierre el explorador.

## <a name="deploy-the-database-update"></a>Implementar la actualización de la base de datos

Para realizar la implementación mediante el proveedor dbDacFx, solo tiene que seleccionar la opción **Actualizar base de datos** en el perfil de publicación. Sin embargo, para la implementación inicial, cuando se usa esta opción, también se configuraron algunos scripts SQL adicionales para ejecutarlos: todavía están en el perfil y tendrá que evitar que se ejecuten de nuevo.

1. Para abrir el Asistente para **publicación web** , haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Seleccione el perfil de **prueba** .
3. Haga clic en la pestaña **configuración** .
4. En **DefaultConnection**, seleccione **Actualizar base de datos**.
5. Deshabilite los scripts adicionales que configuró para ejecutarse para la implementación inicial:

    1. Haga clic en **configurar actualizaciones de base de datos**.
    2. En el cuadro de diálogo **configurar actualizaciones de base de datos** , desactive las casillas situadas junto a *Grant. SQL* y *ASPNET-Data-dev. SQL*.
    3. Haga clic en **Cerrar**.
6. Haga clic en la pestaña **vista previa** .
7. En **bases** de datos y a la derecha de **DefaultConnection**, haga clic en el vínculo **vista previa de base de datos** .

    ![Vista previa de base de datos](deploying-a-database-update/_static/image7.png)

    La ventana de vista previa muestra el script que se ejecutará en la base de datos de destino para que el esquema de la base de datos coincida con el esquema de la base de datos de origen. El script incluye un comando ALTER TABLE que agrega la nueva columna.
8. Cierre el cuadro de diálogo **vista previa de base de datos** y, a continuación, haga clic en **publicar**.

    Visual Studio implementa la aplicación actualizada y el explorador se abre en la Página principal.
9. Ejecute la página UserInfo (agregue *account/UserInfo. aspx* a la dirección URL de la Página principal) para comprobar que la actualización se ha implementado correctamente. Tendrá que iniciar sesión especificando *admin* y *devpwd*.

    Los datos de las tablas no se implementan de forma predeterminada y no se configuró un script de implementación de datos para ejecutarse, por lo que no encontrará el comentario que agregó en el desarrollo. Puede Agregar un nuevo comentario ahora en ensayo para comprobar que el cambio se implementó en la base de datos y que la página funciona correctamente.
10. Siga el mismo procedimiento para realizar la implementación en el entorno de ensayo y la producción.

    No olvide deshabilitar los scripts adicionales. La única diferencia en comparación con el perfil de prueba es que solo se deshabilitará un script en los perfiles de ensayo y de producción, ya que se configuraron para ejecutar solo *ASPNET-Prod-Data. SQL*.

    Las credenciales para el almacenamiento provisional y la producción son admin y prodpwd.

    En el caso de una actualización de la aplicación de producción real que incluye un cambio en la base de datos, normalmente también se desconecta la aplicación durante la implementación mediante la carga de *app\_offline. htm* antes de publicarla y eliminarla después, como se vio en [el tutorial anterior](deploying-a-code-update.md).

## <a name="summary"></a>Resumen

Ahora ha implementado una actualización de la aplicación que incluye un cambio en la base de datos mediante Migraciones de Code First y el proveedor dbDacFx.

![Página de instructores con FechaNacimiento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

En el siguiente tutorial se muestra cómo ejecutar implementaciones mediante la línea de comandos.

> [!div class="step-by-step"]
> [Anterior](deploying-a-code-update.md)
> [Siguiente](command-line-deployment.md)
