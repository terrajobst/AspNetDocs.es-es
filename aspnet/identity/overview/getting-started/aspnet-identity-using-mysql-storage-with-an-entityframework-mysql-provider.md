---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: usar el almacenamiento de MySQL con un proveedor deC#EntityFramework MySQL ()-ASP.net 4. x'
author: maumar
description: En este tutorial se muestra cómo reemplazar el mecanismo de almacenamiento de datos predeterminado para ASP.NET Identity con EntityFramework (proveedor de cliente SQL) con un provid de MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: e89ed139657c5ce9ddcc56879946c62038919483
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471877"
---
# <a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: uso de almacenamiento de MySQL con un proveedor deC#MySQL para EntityFramework ()

por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares de Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> En este tutorial se muestra cómo reemplazar el mecanismo de almacenamiento de datos predeterminado para [**ASP.net Identity**](introduction-to-aspnet-identity.md) con EntityFramework (proveedor de cliente SQL) con un proveedor de MySQL.

En este tutorial se tratarán los siguientes temas:

- Creación de una base de datos MySQL en Azure
- Crear una aplicación MVC mediante Visual Studio 2013 plantilla MVC
- Configuración de EntityFramework para trabajar con un proveedor de base de datos MySQL
- Ejecutar la aplicación para comprobar los resultados

Al final de este tutorial, tendrá una aplicación MVC con el ASP.NET Identity almacén que usa una base de datos MySQL hospedada en Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Creación de una instancia de base de datos MySQL en Azure

1. Inicie sesión en [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Haga clic en **nuevo** en la parte inferior de la página y, a continuación, seleccione **tienda**:

    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. En el asistente **elegir y complemento** , seleccione base de **datos MySQL ClearDB**y, a continuación, haga clic en la flecha **siguiente** situada en la parte inferior del marco:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Mantenga el plan **gratuito** predeterminado, cambie el **nombre** a **IdentityMySQLDatabase**, seleccione la región más cercana a usted y, a continuación, haga clic en la flecha **siguiente** situada en la parte inferior del marco:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Haga clic en la marca de verificación **comprar** para completar la creación de la base de datos.

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Una vez que se ha creado la base de datos, puede administrarla desde la pestaña **COMPLEMENTOS** del Portal de administración. Para recuperar la información de conexión de la base de datos, haga clic en **información de conexión** en la parte inferior de la página:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Copie la cadena de conexión haciendo clic en el botón copiar por el campo **CONNECTIONSTRING** y guárdelo. usará esta información más adelante en este tutorial para la aplicación MVC:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Creación de un proyecto de aplicación MVC

Para completar los pasos de esta sección del tutorial, primero deberá instalar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Una vez instalado Visual Studio, siga los pasos que se indican a continuación para crear un nuevo proyecto de aplicación MVC:

1. Abra Visual Studio 2103.
2. Haga clic en **nuevo proyecto** en la página de **Inicio** , o bien haga clic en el menú **archivo** y, a continuación, en **nuevo proyecto**:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Cuando se muestre el cuadro de diálogo **nuevo proyecto** , expanda **Visual C#**  en la lista de plantillas, haga clic en **Web**y seleccione **aplicación Web ASP.net**. Asigne al proyecto el nombre **IdentityMySQLDemo** y, a continuación, haga clic en **Aceptar**:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione **MVC** templatewith opciones predeterminadas. Esto configurará **las cuentas de usuario individuales** como método de autenticación. Haga clic en **Aceptar**:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Configuración de EntityFramework para trabajar con una base de datos MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Actualizar el ensamblado de Entity Framework del proyecto

La aplicación MVC que se creó a partir de la plantilla de Visual Studio 2013 contiene una referencia al paquete de [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) , pero se han realizado actualizaciones en ese ensamblado desde su versión que contiene mejoras significativas en el rendimiento. Para usar estas actualizaciones más recientes en su aplicación, siga estos pasos.

1. Abra el proyecto de MVC en Visual Studio.
2. Haga clic en **herramientas**, en **Administrador de paquetes NuGet**y, a continuación, en **consola del administrador de paquetes**:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. La **consola del administrador de paquetes** aparecerá en la sección inferior de Visual Studio. Escriba &quot;**Update-package EntityFramework**&quot; y presione ENTRAR:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalación del proveedor de MySQL para EntityFramework

Para que EntityFramework se conecte a la base de datos MySQL, debe instalar un proveedor de MySQL. Para ello, abra la **consola del administrador de paquetes** y escriba &quot;**Install-Package MySQL. Data. Entity-pre**&quot;y, a continuación, presione Entrar.

> [!NOTE]
> Se trata de una versión preliminar del ensamblado y, por lo tanto, puede contener errores. No debe utilizar una versión preliminar del proveedor en producción.

[Haga clic en la siguiente imagen para expandirla.]

[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Realizar cambios en la configuración del proyecto en el archivo Web. config de la aplicación

En esta sección, configurará el Entity Framework para usar el proveedor de MySQL que acaba de instalar, registrar el generador del proveedor de MySQL y agregar su cadena de conexión desde Azure.

> [!NOTE]
> Los ejemplos siguientes contienen una versión específica del ensamblado para MySql. Data. dll. Si la versión del ensamblado cambia, tendrá que modificar la configuración adecuada con la versión correcta.

1. Abra el archivo Web. config del proyecto en Visual Studio 2013.
2. Busque los siguientes valores de configuración, que definen el proveedor de base de datos y el generador predeterminados para el Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Reemplace esas opciones de configuración por lo siguiente, que configurará el Entity Framework para usar el proveedor de MySQL:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Busque la sección &lt;connectionStrings&gt; y reemplácela por el código siguiente, que definirá la cadena de conexión para la base de datos MySQL que se hospeda en Azure (tenga en cuenta que el valor providerName también se ha cambiado desde el original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Agregar contexto de MigrationHistory personalizado

Entity Framework Code First usa una tabla **MigrationHistory** para realizar un seguimiento de los cambios del modelo y garantizar la coherencia entre el esquema de la base de datos y el esquema conceptual. Sin embargo, esta tabla no funciona para MySQL de forma predeterminada porque la clave principal es demasiado grande. Para solucionar esta situación, debe reducir el tamaño de la clave de esa tabla. Para ello, siga estos pasos:

1. La información de esquema de esta tabla se captura en un **HistoryContext**, que se puede modificar como cualquier otro **DbContext**. Para ello, agregue un nuevo archivo de clase denominado **MySqlHistoryContext.CS** al proyecto y reemplace su contenido por el código siguiente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. A continuación, tendrá que configurar Entity Framework para usar el **HistoryContext**modificado, en lugar de uno predeterminado. Esto puede hacerse aprovechando las características de configuración basadas en código. Para ello, agregue un nuevo archivo de clase denominado **MySqlConfiguration.CS** al proyecto y reemplace su contenido por:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Creación de un inicializador EntityFramework personalizado para ApplicationDbContext

El proveedor de MySQL que se incluye en este tutorial no admite actualmente migraciones de Entity Framework, por lo que deberá usar inicializadores de modelo para conectarse a la base de datos. Dado que este tutorial usa una instancia de MySQL en Azure, tendrá que crear un inicializador de Entity Framework personalizado.

> [!NOTE]
> Este paso no es necesario si se está conectando a una instancia de SQL Server en Azure o si está utilizando una base de datos hospedada de forma local.

Para crear un inicializador de Entity Framework personalizado para MySQL, siga estos pasos:

1. Agregue un nuevo archivo de clase denominado **MySqlInitializer.CS** al proyecto y reemplace su contenido por el código siguiente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Abra el archivo **IdentityModels.CS** del proyecto, que se encuentra en el directorio **Models** , y reemplace su contenido por lo siguiente:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Ejecutar la aplicación y comprobar la base de datos

Una vez que haya completado los pasos de las secciones anteriores, debe probar la base de datos. Para ello, siga estos pasos:

1. Presione **Ctrl + F5** para compilar y ejecutar la aplicación Web.
2. Haga clic en la pestaña **registro** en la parte superior de la página:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Escriba un nombre de usuario y una contraseña nuevos y, a continuación, haga clic en **registrar**:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. En este punto, se crean las tablas de ASP.NET Identity en la base de datos MySQL y el usuario se registra y se registra en la aplicación:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalación de la herramienta MySQL Workbench para comprobar los datos

1. Instalación de la herramienta **MySQL Workbench** desde la [Página de descargas de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. En el Asistente para la instalación: pestaña **selección de características** , seleccione **MySQL Workbench** en la sección **aplicaciones** .
3. Inicie la aplicación y agregue una nueva conexión con los datos de la cadena de conexión de la base de datos MySQL de Azure que creó en el Begging de este tutorial.
4. Después de establecer la conexión, inspeccione el **ASP.net Identity** tablas creadas en el **IdentityMySQLDatabase.**
5. Verá que todas ASP.NET Identity tablas necesarias se crean como se muestra en la imagen siguiente:

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Inspeccione la tabla **aspnetusers** para comprobar si hay entradas que registren nuevos usuarios.

   [Haga clic en la siguiente imagen para expandirla. ] [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
