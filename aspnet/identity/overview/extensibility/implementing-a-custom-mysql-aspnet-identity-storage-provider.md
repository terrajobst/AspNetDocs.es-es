---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: 'Implementación de un proveedor de almacenamiento de ASP.NET Identity MySQL personalizado: ASP.NET 4. x'
author: raquelsa
description: ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin tener que volver a trabajar con las aplicaciones...
ms.author: riande
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 2f0b47d45bce82c71d1864536309f9e2ffed2d63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500071"
---
# <a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementar un proveedor de almacenamiento personalizado de ASP.NET Identity de MySQL

por [Raquel Soares de Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin tener que volver a trabajar con la aplicación. En este tema se describe cómo crear un proveedor de almacenamiento de MySQL para ASP.NET Identity. Para obtener información general sobre la creación de proveedores de almacenamiento personalizados, consulte [información general de proveedores de almacenamiento personalizados para ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Para completar este tutorial, debe tener Visual Studio 2013 con Update 2.
> 
> Este tutorial hará lo siguiente:
> 
> - Muestre cómo crear una instancia de base de datos MySQL en Azure.
> - Muestre cómo usar una herramienta de cliente de MySQL (MySQL Workbench) para crear tablas y administrar la base de datos remota en Azure.
> - Muestre cómo reemplazar la implementación de almacenamiento de ASP.NET Identity predeterminada con nuestra implementación personalizada en un proyecto de aplicación MVC.
> 
> Este tutorial fue escrito originalmente por Raquel Soares de Almeida y Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ). El proyecto de ejemplo se actualizó para la identidad 2,0 de Suhas Joshi. El tema se actualizó para la identidad 2,0 por Tom FitzMacken.

## <a name="download-completed-project"></a>Descargar proyecto completado

Al final de este tutorial, tendrá un proyecto de aplicación MVC con ASP.NET Identity trabajando con una base de datos MySQL hospedada en Azure.

Puede descargar el proveedor de almacenamiento de MySQL completado en [Aspnet. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL).

## <a name="the-steps-you-will-perform"></a>Los pasos que realizará

En este tutorial, aprenderá lo siguiente:

1. Crear una base de datos MySQL en Azure
2. Crear las tablas de ASP.NET Identity en MySQL
3. Crear una aplicación MVC y configurarla para que use el proveedor de MySQL
4. Ejecutar la aplicación

En este tema no se trata la arquitectura de ASP.NET Identity y las decisiones que debe tomar al implementar un proveedor de almacenamiento de cliente. Para obtener esa información, consulte [información general de los proveedores de almacenamiento personalizados para ASP.net Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Revisar las clases de proveedor de almacenamiento de MySQL

Antes de ir a los pasos para crear el proveedor de almacenamiento de MySQL, echemos un vistazo a las clases que componen el proveedor de almacenamiento. Necesitará clases que administren las operaciones de base de datos y las clases a las que se llama desde la aplicación para administrar usuarios y roles.

### <a name="storage-classes"></a>Clases de almacenamiento

- [IdentityUser](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs) : contiene propiedades para el usuario.
- [UserStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs) : contiene las operaciones para agregar, actualizar o recuperar usuarios.
- [IdentityRole](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs) : contiene las propiedades de los roles.
- [RoleStore](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) : contiene las operaciones para agregar, eliminar, actualizar y recuperar roles.

### <a name="data-access-layer-classes"></a>Clases de capa de acceso a datos

En este ejemplo, las clases de capa de acceso a datos contienen instrucciones SQL para trabajar con las tablas; sin embargo, en el código podría querer usar la asignación relacional de objetos (ORM) como Entity Framework o NHibernate. En concreto, la aplicación puede experimentar un rendimiento deficiente sin un ORM que incluye la carga diferida y el almacenamiento en caché de objetos. Para obtener más información, consulte [ASP.NET Identity 2,0 sin Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) : contiene la conexión de base de datos MySQL y los métodos para realizar operaciones de base de datos. Se crean instancias de UserStore y RoleStore con una instancia de esta clase.
- [RoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) : contiene las operaciones de base de datos para la tabla que almacena roles.
- [UserClaimsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) : contiene las operaciones de base de datos para la tabla que almacena notificaciones de usuario.
- [UserLoginsTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) : contiene las operaciones de base de datos para la tabla que almacena la información de inicio de sesión del usuario.
- [UserRoleTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) : contiene las operaciones de base de datos para la tabla que almacena los usuarios a los que se asignan los roles.
- [UserTable](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) : contiene las operaciones de base de datos para la tabla que almacena los usuarios.

## <a name="create-a-mysql-database-instance-on-azure"></a>Creación de una instancia de base de datos MySQL en Azure

1. Inicie sesión en [Azure Portal](https://manage.windowsazure.com/).
2. Haga clic en **+ nuevo** en la parte inferior de la página y, a continuación, seleccione **tienda**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. En el asistente **elegir y complemento** , seleccione base de **datos MySQL ClearDB** y haga clic en la flecha siguiente situada en la parte inferior derecha del cuadro de diálogo.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Mantenga el plan **gratuito** predeterminado y cambie el **nombre** a **IdentityMySQLDatabase**. Seleccione la región más cercana y, a continuación, haga clic en la flecha siguiente.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Haga clic en la marca de verificación para completar la creación de la base de datos.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Una vez que se ha creado la base de datos, puede administrarla desde la pestaña **COMPLEMENTOS** del Portal de administración.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Puede obtener la información de conexión de la base de datos haciendo clic en **información de conexión** en la parte inferior de la página.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Copie la cadena de conexión haciendo clic en el botón copiar y guárdela para poder usarlo más adelante en la aplicación MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Crear las tablas de ASP.NET Identity en una base de datos MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Instalación de la herramienta MySQL Workbench para conectarse y administrar la base de datos MySQL

1. Instalación de la herramienta **MySQL Workbench** desde la [Página de descargas de MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Inicie la aplicación y haga clic en el botón **MySQLConnections +** para agregar una nueva conexión. Use los datos de la cadena de conexión que ha copiado de la base de datos MySQL de Azure que creó anteriormente en este tutorial.
3. Después de establecer la conexión, abra una nueva pestaña de **consulta** ; pegue los comandos de [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) en la consulta y ejecútelo para crear las tablas de base de datos.
4. Ahora tiene todas las tablas necesarias de ASP.NET Identity creadas en una base de datos MySQL hospedada en Azure, como se muestra a continuación.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Crear un proyecto de aplicación de MVC a partir de una plantilla y configurarlo para usar el proveedor de MySQL

Si es necesario, instale [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) con Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-github"></a>Descargar el proyecto ASP. NET. Identity. MySQL de GitHub

1. Vaya a la dirección URL del repositorio en [Aspnet. Identity. MySQL (GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/AspNet.Identity.MySQL/).
2. Descargue el código fuente.
3. Extraiga el archivo. zip en una carpeta local.
4. Abra la solución AspNet. Identity. MySQL y genérelo.

### <a name="create-a-new-mvc-application-project-from-template"></a>Crear un nuevo proyecto de aplicación de MVC a partir de una plantilla

1. Haga clic con el botón derecho en la solución **Aspnet. Identity. MySQL** y **agregue**, **nuevo proyecto**
2. En el cuadro de diálogo **Agregar nuevo proyecto** , seleccione  **C# visual** a la izquierda, luego **Web** y, a continuación, seleccione **aplicación Web de ASP.net**. Asigne al proyecto el nombre **IdentityMySQLDemo**; y, a continuación, haga clic en Aceptar.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla MVC con las opciones predeterminadas (que incluye **las cuentas de usuario individuales** como método de autenticación) y haga clic en **Aceptar**.![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. En Explorador de soluciones, haga clic con el botón derecho en el proyecto IdentityMySQLDemo y seleccione **administrar paquetes NuGet**. En el cuadro de diálogo Buscar cuadro de texto, escriba **Identity. EntityFramework**. Seleccione este paquete en la lista de resultados y haga clic en **desinstalar**. Se le pedirá que desinstale el paquete de dependencia EntityFramework. Haga clic en sí, ya que ya no vamos a usar este paquete en esta aplicación.
5. Haga clic con el botón derecho en el proyecto IdentityMySQLDemo, seleccione **Agregar**, **referencia, solución, proyectos,** Seleccione el proyecto Aspnet. Identity. MySQL y haga clic en **Aceptar**.
6. En el proyecto IdentityMySQLDemo, reemplace todas las referencias a  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. En IdentityModels.cs, establezca **ApplicationDbContext** para que se derive de **MySqlDatabase** e incluya un constructor que tome un único parámetro con el nombre de la conexión.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Abra el archivo IdentityConfig.cs. En el método **ApplicationUserManager. Create** , reemplace la creación de instancias de UserManager con el código siguiente:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Abra el archivo Web. config y reemplace la cadena DefaultConnection por esta entrada reemplazando los valores resaltados por la cadena de conexión de la base de datos MySQL que creó en los pasos anteriores:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Ejecutar la aplicación y conectarse a la base de

1. Haga clic con el botón derecho en el proyecto **IdentityMySQLDemo** y seleccione **establecer como proyecto de inicio** .
2. Presione **Ctrl + F5** para compilar y ejecutar la aplicación.
3. Haga clic en la pestaña **registro** en la parte superior de la página.
4. Escriba un nombre de usuario y una contraseña nuevos y, a continuación, haga clic en **registrar**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. El nuevo usuario se ha registrado y ha iniciado sesión.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Vuelva a la herramienta MySQL Workbench e inspeccione el contenido de la tabla **IdentityMySQLDatabase** . Inspeccione las entradas de la tabla usuarios para registrar nuevos usuarios.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo habilitar otros métodos de autenticación en esta aplicación, consulte [creación de una aplicación de ASP.NET MVC 5 con el inicio de sesión de Facebook y Google OAuth2 y OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Para obtener información sobre cómo integrar la base de de con OAuth y configurar roles para limitar el acceso de los usuarios a la aplicación, consulte [implementación de una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
