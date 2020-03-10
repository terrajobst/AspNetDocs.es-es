---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Agregar ASP.NET Identity a un proyecto de formularios Web Forms vacío o existente-ASP.NET 4. x
author: raquelsa
description: En este tutorial se muestra cómo agregar ASP.NET Identity (el sistema de pertenencia a ASP.NET) a una aplicación de ASP.NET. Al crear un nuevo formulario Web Forms o MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471979"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Agregar ASP.NET Identity a un proyecto de formularios Web Forms vacío o existente

> En este tutorial se muestra cómo agregar [ASP.net Identity](introduction-to-aspnet-identity.md) (el nuevo sistema de pertenencia para ASP.net) a una aplicación de ASP.net.
> 
> Al crear un nuevo proyecto de formularios Web Forms o MVC en Visual Studio 2017 RTM con cuentas individuales, Visual Studio instalará todos los paquetes necesarios y agregará automáticamente todas las clases necesarias. En este tutorial se ilustran los pasos para agregar compatibilidad con ASP.NET Identity a su proyecto de formularios Web Forms existente o a un nuevo proyecto vacío. Describiré todos los paquetes de NuGet que necesita instalar y las clases que debe agregar. Usaremos los formularios Web Forms de ejemplo para registrar nuevos usuarios e iniciar sesión mientras resaltan todas las API principales de punto de entrada para la autenticación y administración de usuarios. Este ejemplo usará la implementación predeterminada de ASP.NET Identity para el almacenamiento de datos SQL que se basa en Entity Framework. En este tutorial, usaremos LocalDB para la base de datos SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Introducción a ASP.NET Identity

1. Empiece por instalar y ejecutar [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Seleccione **nuevo proyecto** en la página de inicio, o bien, puede usar el menú, seleccionar **archivo**y, a continuación, **nuevo proyecto**.
3. En el panel izquierdo, expanda **Visual C#** , seleccione **Web**y, a continuación, **ASP.NET Web Application (.NET Framework)** . Asigne al proyecto el nombre "WebFormsIdentity" y seleccione **Aceptar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** .
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Observe que el botón **cambiar autenticación** está deshabilitado y no se proporciona compatibilidad con la autenticación en esta plantilla. Las plantillas de Web Forms, MVC y Web API le permiten seleccionar el enfoque de autenticación.

## <a name="add-identity-packages-to-your-app"></a>Agregar paquetes de identidades a la aplicación

En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **administrar paquetes NuGet**. Busque e instale el paquete **Microsoft. Aspnet. Identity. EntityFramework** . 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Tenga en cuenta que este paquete instalará los paquetes de dependencia: **EntityFramework** y **Microsoft ASP.net principal de identidad**.

## <a name="add-a-web-form-to-register-users"></a>Agregar un formulario web para registrar usuarios

1. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto, seleccione **Agregar**y, a continuación, **formulario web**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. En el cuadro de diálogo **especificar nombre para el elemento** , asigne un nombre al nuevo formulario Web Forms **y, después**, seleccione **Aceptar** .
3. Reemplace el marcado en el archivo *Register. aspx* generado con el código siguiente. Los cambios de código aparecen resaltados. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Se trata simplemente de una versión simplificada del archivo *Register. aspx* que se crea al crear un nuevo proyecto de formularios Web Forms de ASP.net. El marcado anterior agrega campos de formulario y un botón para registrar un nuevo usuario.
4. Abra el archivo *Register.aspx.CS* y reemplace el contenido del archivo por el código siguiente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. El código anterior es una versión simplificada del archivo *Register.aspx.CS* que se crea al crear un nuevo proyecto de formularios Web Forms de ASP.net.
    > 2. La clase *IdentityUser* es la implementación predeterminada de EntityFramework de la interfaz *IUser* . La interfaz *IUser* es la interfaz mínima para un usuario en ASP.net Identity Core.
    > 3. La clase *UserStore* es la implementación predeterminada de EntityFramework de un almacén de usuario. Esta clase implementa las interfaces mínimas de ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* y *IUserRoleStore*.
    > 4. La clase *UserManager* expone las API relacionadas con el usuario, que guardarán automáticamente los cambios en el *UserStore*.
    > 5. La clase *IdentityResult* representa el resultado de una operación de identidad.
5. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar**, **agregue la carpeta ASP.net** y, a continuación, **\_datos**de la aplicación.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra el archivo *Web. config* y agregue una entrada de cadena de conexión para la base de datos que se utilizará para almacenar información de usuario. La base de datos se creará en tiempo de ejecución por EntityFramework para las entidades de identidad. La cadena de conexión es similar a la que se crea cuando se crea un nuevo proyecto de formularios Web Forms. El código resaltado muestra el marcado que debe agregar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para Visual Studio 2015 o posterior, reemplace `(localdb)\v11.0` por `(localdb)\MSSQLLocalDB` en la cadena de conexión.
    
7. Haga clic con el botón derecho en archivo *Register. aspx* en el proyecto y seleccione **establecer como página de inicio**. Presione Ctrl + F5 para compilar y ejecutar la aplicación Web. Escriba un nombre de usuario y una contraseña nuevos y, a continuación, seleccione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity admite la validación y en este ejemplo puede comprobar el comportamiento predeterminado en los validadores de usuario y contraseña que proceden del paquete principal de identidades. El validador predeterminado para el usuario (`UserValidator`) tiene una propiedad `AllowOnlyAlphanumericUserNames` que tiene un valor predeterminado establecido en `true`. El validador predeterminado de contraseña (`MinimumLengthValidator`) garantiza que la contraseña tiene al menos 6 caracteres. Estos validadores son propiedades de `UserManager` que se pueden invalidar si desea tener validación personalizada,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Compruebe la base de datos de identidad de LocalDb y las tablas generadas por Entity Framework

1. En el menú **Ver** , seleccione **Explorador de servidores**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expanda **DefaultConnection (WebFormsIdentity)** , expanda **tablas**, haga clic con el botón secundario en **AspNetUsers** y, a continuación, seleccione **Mostrar datos de tabla**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configuración de la aplicación para la autenticación OWIN

En este momento, solo se ha agregado compatibilidad con la creación de usuarios. Ahora, vamos a mostrar cómo se puede Agregar la autenticación para iniciar sesión con un usuario. ASP.NET Identity usa el middleware de autenticación de Microsoft OWIN para la autenticación de formularios. La autenticación de cookies OWIN es una cookie y un mecanismo de autenticación basado en notificaciones que se puede usar en cualquier plataforma hospedada en [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) o IIS. Con este modelo, se pueden usar los mismos paquetes de autenticación en varios marcos, incluidos ASP.NET MVC y Web Forms. Para obtener más información sobre Project Katana y cómo ejecutar middleware en un host independiente, vea [Introducción con el proyecto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Instalación de paquetes de autenticación en la aplicación

1. En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **administrar paquetes NuGet**. Busque e instale el paquete ***Microsoft. Aspnet. Identity. Owin*** . 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Busque e instale el paquete ***Microsoft. Owin. host. SystemWeb*** .

    > [!NOTE]
    > El paquete **Microsoft. Aspnet. Identity. Owin** contiene un conjunto de clases de extensión Owin para administrar y configurar el middleware de autenticación Owin que van a consumir los paquetes de ASP.net Identity Core.
    > El paquete **Microsoft. Owin. host. SystemWeb** contiene un servidor Owin que permite que las aplicaciones basadas en Owin se ejecuten en IIS mediante la canalización de solicitudes de ASP.net. Para obtener más información, consulte [middleware de OWIN en la canalización integrada de IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Agregar clases de configuración de inicio y autenticación de OWIN

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar**y, a continuación, **Agregar nuevo elemento**. En el cuadro de diálogo Buscar cuadro de texto, escriba "*Owin*". Asigne a la clase el nombre "*Startup*" y seleccione **Agregar**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. En el archivo Startup.cs, agregue el código resaltado que se muestra a continuación para configurar la autenticación de cookies OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Esta clase contiene el atributo `OwinStartup` para especificar la clase de inicio OWIN. Cada aplicación OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de la aplicación. Consulte [detección de clases de inicio de OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obtener más información sobre este modelo.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Agregar formularios Web Forms para registrarse e iniciar sesión en los usuarios

1. Abra el archivo *Register.aspx.CS* y agregue el código siguiente que inicia sesión en el usuario cuando el registro se realiza correctamente.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Como ASP.NET Identity y la autenticación de cookies OWIN son sistema basado en notificaciones, el marco de trabajo requiere que el desarrollador de la aplicación genere un [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para el usuario. ClaimsIdentity contiene información sobre todas las notificaciones para el usuario, como los roles a los que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.
    > - Puede iniciar sesión en el usuario mediante AuthenticationManager desde OWIN y llamando a `SignIn` y pasando el ClaimsIdentity como se mostró anteriormente. Este código iniciará la sesión del usuario y generará también una cookie. Esta llamada es análoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) que usa el módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
2. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto, seleccione **Agregar**y, a continuación, **formulario web**. Asigne un nombre al **Inicio de sesión**de formulario Web.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Reemplace el contenido del archivo *login. aspx* con el código siguiente:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Reemplace el contenido del archivo *login.aspx.CS* por lo siguiente:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - En el `Page_Load` ahora se comprueba el estado del usuario actual y se realiza una acción en función de su estado de `Context.User.Identity.IsAuthenticated`.
    >   **Mostrar el nombre de usuario con sesión iniciada** : el marco de identidad de Microsoft ASP.net ha agregado métodos de extensión en [System. Security. principal. IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que le permite obtener los `UserName` y `UserId` del usuario que ha iniciado sesión. Estos métodos de extensión se definen en el ensamblado de `Microsoft.AspNet.Identity.Core`. Estos métodos de extensión son el reemplazo de [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método de inicio de sesión: `This` método reemplaza el método de `CreateUser_Click` anterior en este ejemplo y ahora inicia la sesión del usuario después de crear correctamente el usuario.   
    >   El marco OWIN de Microsoft ha agregado métodos de extensión en `System.Web.HttpContext` que le permite obtener una referencia a un `IOwinContext`. Estos métodos de extensión se definen en `Microsoft.Owin.Host.SystemWeb` ensamblado. La clase `OwinContext` expone una propiedad `IAuthenticationManager` que representa la funcionalidad de middleware de autenticación disponible en la solicitud actual. Puede iniciar sesión en el usuario mediante el `AuthenticationManager` desde OWIN y llamando a `SignIn` y pasando el `ClaimsIdentity` como se mostró anteriormente. Dado que ASP.NET Identity y la autenticación de cookies OWIN son un sistema basado en notificaciones, el marco de trabajo requiere que la aplicación genere un `ClaimsIdentity` para el usuario. El `ClaimsIdentity` contiene información sobre todas las notificaciones para el usuario, como los roles a los que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase, este código iniciará la sesión del usuario y generará también una cookie. Esta llamada es análoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) que usa el módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
    > - `SignOut` método: obtiene una referencia al `AuthenticationManager` desde OWIN y llama a `SignOut`. Es análogo al método [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) usado por el módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
5. Presione **Ctrl + F5** para compilar y ejecutar la aplicación Web. Escriba un nombre de usuario y una contraseña nuevos y, a continuación, seleccione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Nota: en este momento, el nuevo usuario se crea y se inicia sesión.
6. Seleccione el botón **Cerrar sesión** . Se le redirigirá al formulario de inicio de sesión.
7. Escriba un nombre de usuario o una contraseña no válidos y seleccione el botón **iniciar sesión** . 
   El método `UserManager.Find` devolverá null y se mostrará el mensaje de error: " *nombre de usuario o contraseña no válidos* ".
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
