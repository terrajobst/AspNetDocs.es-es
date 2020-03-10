---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Pertenencia | Microsoft Docs
author: microsoft
description: La pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios desde ASP.NET 1. x. La autenticación de formularios ASP.NET proporciona una manera cómoda de Incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521707"
---
# <a name="membership"></a>Pertenencia

por [Microsoft](https://github.com/microsoft)

> La pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios desde ASP.NET 1. x. La autenticación de formularios ASP.NET proporciona una manera cómoda de incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios en una base de datos u otro almacén de datos.

La pertenencia a ASP.NET se basa en el éxito del modelo de autenticación de formularios desde ASP.NET 1. x. La autenticación de formularios ASP.NET proporciona una manera cómoda de incorporar un formulario de inicio de sesión en la aplicación ASP.NET y validar a los usuarios en una base de datos u otro almacén de datos. Los miembros de la clase FormsAuthentication permiten controlar las cookies para la autenticación, buscar un inicio de sesión válido, cerrar la sesión de un usuario, etc. Sin embargo, la implementación de la autenticación de formularios en una aplicación ASP.NET 1. x puede requerir una cantidad considerable de código.

La pertenencia a ASP.NET 2,0 es un avance importante sobre el uso de la autenticación de formularios solo. (La pertenencia es más sólida cuando se acopla con la autenticación de formularios, pero el uso de la autenticación de formularios no es un requisito). Como verá pronto, puede usar la pertenencia a ASP.NET y los controles de inicio de sesión de ASP.NET 2,0 para implementar un sistema de pertenencia eficaz sin escribir mucho código.

## <a name="implementing-membership-in-aspnet-20"></a>Implementar la pertenencia a ASP.NET 2,0

La pertenencia se implementa siguiendo cuatro pasos. Tenga en cuenta que hay muchos subpasos que participan, así como una configuración opcional que también se puede implementar. Estos pasos están pensados para ilustrar la visión general de la configuración de la pertenencia.

1. Cree la base de datos de pertenencia (si se usa SQL Server como almacén de pertenencia).
2. Especifique las opciones de pertenencia en los archivos de configuración de la aplicación. (La pertenencia está habilitada de forma predeterminada).
3. Determine el tipo de almacén de pertenencia que desea usar. Las opciones son: 

    - Microsoft SQL Server (versión 7,0 o posterior)
    - Active Directory Store
    - Proveedor de pertenencia personalizado
4. Configure la aplicación para la autenticación de formularios ASP.NET. Una vez más, la pertenencia se ha diseñado para aprovechar la autenticación de formularios, pero el uso de la autenticación de formularios no es un requisito.
5. Defina las cuentas de usuario para la pertenencia y configure los roles si lo desea.

## <a name="creating-the-membership-database"></a>Crear la base de datos de pertenencia

Si usa SQL Server 7,0 o posterior como almacén de pertenencia, puede usar la utilidad ASPNET\_regsql (disponible más fácilmente desde el símbolo del sistema de Visual Studio .NET 2005) para configurar la base de datos. La utilidad ASPNET\_regsql se puede usar como una herramienta de símbolo del sistema o mediante un asistente de GUI. El método del asistente es la forma más sencilla de configurar la base de datos. Para obtener acceso al asistente, basta con ejecutar el siguiente comando:

`aspnet_regsql W`

Una vez que ejecute el comando, se le presentará el Asistente para la instalación de ASP.NET SQL Server como se muestra a continuación.

![](membership/_static/image1.jpg)

**Ilustración 1**

El Asistente para la instalación de ASP.NET SQL Server crea el sitio web en la instancia de que especifique en el asistente. Sin embargo, ASP.NET usará la cadena de conexión en el archivo Machine. config para conectarse a la base de datos. De forma predeterminada, esta cadena de conexión apuntará a una instancia SQL Server 2005, por lo que si usa una instancia de SQL Server 2000 o SQL Server 7,0, deberá modificar la cadena de conexión en el archivo Machine. config. Esa cadena de conexión se puede encontrar aquí:

[!code-xml[Main](membership/samples/sample1.xml)]

Desafortunadamente, si no modifica la cadena de conexión, ASP.NET no le proporcionará un error descriptivo. Simplemente se seguirá queja de que no ha creado la base de datos. En el caso anterior, he modificado la cadena de conexión para que apunte a mi instancia local de SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificación de la configuración y adición de usuarios y roles

El siguiente paso en la configuración de la pertenencia es agregar la información necesaria al archivo Web. config de la aplicación. En ASP.NET 1. x, la modificación del archivo Web. config a veces era difícil debido al uso de lowerCamelCase y a la falta de IntelliSense. Visual Studio .NET 2005 hace que la tarea sea mucho más fácil con IntelliSense para los archivos de configuración, pero ASP.NET 2,0 va un paso más allá proporcionando una interfaz web para editar archivos de configuración.

Puede iniciar la interfaz web haciendo clic en el botón configuración de ASP.NET en la barra de herramientas Explorador de soluciones como se muestra a continuación. También puede iniciar la interfaz web a través de elementos emergentes que se muestran cuando se insertan controles de inicio de sesión.

![](membership/_static/image2.jpg)

**Ilustración 2**

Esto inicia la herramienta de administración del sitio web de ASP.NET que se muestra a continuación. La administración del sitio web de ASP.NET es una interfaz de cuatro pestañas que facilita la administración de la configuración de la aplicación. Las siguientes pestañas están disponibles:

- **Página principal**
- **Seguridad** de Configurar usuarios, roles y acceso.
- **Aplicación** de Configurar las opciones de la aplicación.
- **Proveedor** de Configure y pruebe el proveedor de pertenencia de las aplicaciones.

La herramienta de administración de sitios web le permite crear fácilmente nuevos usuarios, crear nuevos roles y administrar usuarios y roles. Esta capacidad no está disponible en la interfaz de Windows. La interfaz de Windows permite definir fácilmente la configuración de autorización y agregar, eliminar y administrar proveedores, funcionalidades que no están en la herramienta de administración de sitios Web.

Para iniciar la interfaz de Windows, abra el complemento Internet Information Services, haga clic con el botón derecho en la aplicación y elija Propiedades. Haga clic en la pestaña ASP.NET y, a continuación, haga clic en el botón Editar configuración. (La aplicación debe ejecutarse en ASP.NET 2,0 para que el botón Editar configuración esté habilitado. También puede configurar la versión de ASP.NET en el cuadro de diálogo ASP.NET). Se muestra el cuadro de diálogo Opciones de configuración de ASP.NET, como se muestra a continuación.

![](membership/_static/image3.jpg)

**Ilustración 3**

En la pestaña general, se enumeran las cadenas de conexión y la configuración de la aplicación. Cualquier configuración en cursiva se define en un archivo de configuración principal (ya sea Machine. config o Web. config en un nivel superior) y la configuración no en cursiva procede del archivo de configuración de aplicaciones. Si se agrega, quita o edita una configuración en el nivel de aplicación, ASP.NET agregará, quitará o modificará la configuración en el archivo Web. config de los niveles de aplicación en lugar de quitar la configuración del archivo de configuración del que se hereda.

A continuación se muestra la pestaña autenticación. Aquí es donde se configurarán los valores de pertenencia. La configuración de autenticación de formularios, los proveedores de pertenencia y los proveedores de roles se pueden configurar aquí.

![](membership/_static/image4.jpg)

**Ilustración 4**

## <a name="implementing-membership-in-your-application"></a>Implementar la pertenencia a la aplicación

La manera más sencilla de implementar la pertenencia a ASP.NET 2,0 en la aplicación es usar los controles de inicio de sesión proporcionados. Este método permite implementar los aspectos básicos de la pertenencia a ASP.NET 2,0 sin escribir ningún código.

Los siguientes controles de inicio de sesión están disponibles en ASP.NET 2,0:

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona una interfaz para que alguien inicie sesión en el sistema de pertenencia. Proporciona un cuadro de texto de nombre de usuario y contraseña y un botón de inicio de sesión. Muchas otras características comunes, como un vínculo para registrarse para personas que aún no lo han hecho, una casilla que permite al usuario iniciar sesión automáticamente en visitas posteriores, un vínculo para un recordatorio de contraseña, etc. Todas las características del control de inicio de sesión se pueden personalizar a través de las propiedades del control.

En ASP.NET 1. x, los desarrolladores tenían que escribir una cantidad equitativa de código para realizar una búsqueda al utilizar la autenticación de formularios. Con la pertenencia a ASP.NET 2,0, puede validar los usuarios sin necesidad de escribir ningún código. ASP.NET realizará automáticamente la búsqueda del usuario. (Si usa el control de inicio de sesión sin usar la pertenencia a ASP.NET, puede usar el método **OnAuthenticate** para validar al usuario).

## <a name="loginview-control"></a>Control LoginView

El control LoginView es un control con plantilla que proporciona dos plantillas de forma predeterminada; AnonymousTemplate y LoggedInTemplate. La plantilla que se muestra viene determinada por si el usuario ha iniciado sesión en el sistema de pertenencia o no. Este control se utiliza normalmente para mostrar un control de inicio de sesión cuando un usuario aún no ha iniciado sesión y un control LoginStatus y/u otros controles de inicio de sesión cuando el usuario ha iniciado sesión. Si usa la administración de roles en la aplicación ASP.NET, el control LoginView puede mostrar una plantilla específica basada en el rol usuarios. (Más información sobre la administración de roles de ASP.NET se tratará más adelante).

## <a name="passwordrecovery-control"></a>Control PasswordRecovery

El control PasswordRecovery permite a los usuarios recibir un correo electrónico con su contraseña actual o restablecer su contraseña. El texto sin cifrar y las contraseñas cifradas se pueden recuperar y enviar por correo electrónico a los usuarios. Si se aplica un algoritmo hash a la contraseña, no se puede recuperar. En su lugar, se solicitará al usuario que realice un restablecimiento de contraseña.

## <a name="loginstatus-control"></a>LoginStatus (control)

El control LoginStatus se usa para mostrar un indicador de inicio de sesión a los usuarios que no han iniciado sesión y un indicador de cierre de sesión para los usuarios que han iniciado sesión actualmente. La propiedad request. IsAuthenticated se usa para determinar el indicador que se va a mostrar. El indicador mostrado por el control LoginStatus puede ser texto (implementado a través de las propiedades **LoginText** y **LogoutText** ) o imágenes (implementadas a través de las propiedades **LoginImageUrl** y **LogoutImageUrl** ).

Cuando un usuario cierra sesión a través del control LoginStatus, se le redirige a la dirección URL especificada por la propiedad **LogoutPageUrl** . Si no se establece esa propiedad, se actualiza la página actual. Dado que es probable que el sitio esté protegido por la autenticación de formularios, la actualización de la página actual redirigirá al usuario a la página de inicio de sesión del sitio.

## <a name="loginname-control"></a>LoginName (control)

El control LoginName muestra el nombre de usuario del usuario que ha iniciado sesión actualmente en el sitio.

## <a name="createuserwizard-control"></a>Control CreateUserWizard

El control CreateUserWizard proporciona a los usuarios una manera cómoda de registrarse para el sistema de pertenencia. Puede Agregar pasos (implementados como una colección de WizardSteps) a través de la interfaz que se muestra a continuación.

![](membership/_static/image5.jpg)

**Figura 5**

CreateUserWizard es un control con plantilla que se deriva de la clase Wizard y proporciona las siguientes plantillas:

- **HeaderTemplate** Esta plantilla controla la apariencia del encabezado del asistente.
- **SidebarTemplate** Esta plantilla controla la apariencia de la barra lateral del asistente.
- **StartNavigationTemplate** Esta plantilla controla la apariencia de la navegación del asistente en el paso Inicio.
- **StepNavigationTemplate** Esta plantilla controla la apariencia del área de navegación cuando no está en el paso iniciar o finalizar.
- **FinishNavigationTemplate** Esta plantilla controla la apariencia del área de navegación en el paso finalizar.

Además, para cada paso que agregue al asistente, ASP.NET creará una plantilla personalizada que contiene tanto ContentTemplate como CustomNavigationTemplate para ese paso. Para obtener detalles completos sobre cómo personalizar CreateUserWizard, consulte la documentación de VS.NET 2005:

## <a name="changepassword-control"></a>ChangePassword (control)

El control ChangePassword permite a los usuarios cambiar su contraseña. Si la propiedad DisplayUserName es true (de forma predeterminada, es false), el usuario puede cambiar su contraseña cuando no haya iniciado sesión. Si el usuario *ya ha* iniciado sesión y la propiedad DisplayUserName es true, el usuario podrá cambiar la contraseña de otro usuario que no haya iniciado sesión, lo que le indicará el identificador de usuario de dicho usuario.

Tenga en cuenta que si desea que los usuarios puedan cambiar las contraseñas sin tener que iniciar sesión, deberá asegurarse de que la página en la que se muestra el control ChangePassword permite el acceso anónimo. Obviamente, los usuarios tendrán que proporcionar la contraseña anterior para poder cambiar su contraseña.

## <a name="role-management"></a>Administración de roles

La administración de roles permite asignar usuarios a un rol determinado y, a continuación, restringir el acceso a determinados archivos o carpetas en función de ese rol. La administración de roles también proporciona una API para que pueda determinar mediante programación el rol de alguien o determinar todos los usuarios de un rol determinado y responder en consecuencia.

La administración de roles no es un requisito de la pertenencia a ASP.NET, ni se integra un requisito para poder usar la administración de roles. Sin embargo, los dos complementos se encargan perfectamente y es probable que los desarrolladores los utilicen conjuntamente entre sí.

Para habilitar la administración de roles en la aplicación, realice el siguiente cambio en el archivo Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Cuando el atributo **cacheRolesInCookie** está establecido en true, ASP.net almacena en caché la pertenencia a un rol de usuario en una cookie en el cliente. Esto permite que se produzcan búsquedas de roles sin llamadas a RoleProvider. Al utilizar este atributo, se recomienda a los desarrolladores que se aseguren de que el atributo **cookieProtection** está establecido en ALL. (Esta es la configuración predeterminada). Esto garantiza que los datos de la cookie están cifrados y ayuda a garantizar que el contenido de las cookies no se ha modificado. Los roles se pueden agregar mediante la herramienta de administración de sitios Web. Permite definir fácilmente roles, configurar el acceso a partes del sitio en función de esos roles y asignar usuarios a roles.

![](membership/_static/image6.jpg)

**Figura 6**

Como se mostró anteriormente, se pueden agregar nuevos roles simplemente escribiendo el nombre del rol y haciendo clic en agregar rol. Los roles existentes se pueden administrar o eliminar haciendo clic en el vínculo correspondiente en la lista de roles existentes.

Al administrar un rol, puede Agregar o quitar usuarios, como se muestra a continuación.

![](membership/_static/image7.jpg)

**Figura 7**

Al activar la casilla el usuario tiene el rol, puede agregar fácilmente un usuario a un rol específico. ASP.NET actualizará automáticamente la base de datos de pertenencia con las entradas adecuadas. También querrá configurar reglas de acceso para la aplicación. ASP.NET 1. x los desarrolladores están familiarizados con este procedimiento a través del elemento &lt;Authorization&gt; en el archivo Web. config, y esa opción sigue estando disponible en ASP.NET 2,0. Sin embargo, es más fácil administrar las reglas de acceso mediante la herramienta de administración de sitios web, como se muestra a continuación.

![](membership/_static/image8.jpg)

**Figura 8**

En este caso, la carpeta de administración se resalta (es difícil de ver porque la herramienta lo resalta en gris claro) y se ha concedido acceso al rol administradores. Se deniegan todos los demás usuarios. Puede hacer clic en el icono del encabezado para seleccionar una regla y, a continuación, usar los botones subir y bajar para organizar las reglas. Al igual que con el elemento de&gt; de autorización de ASP.NET &lt;, las reglas se procesan en el orden en que aparecen. En otras palabras, si se invierte el orden de las reglas en la captura anterior, nadie tendría acceso a la carpeta de administración porque la primera regla que ASP.NET encontraría sería la regla que deniega a todos los usuarios a la carpeta.

ASP.NET 2,0 agrega un archivo Web. config a la carpeta para la que está especificando una regla de acceso. Las reglas de acceso se pueden editar mediante el archivo de configuración o a través de la herramienta de administración de sitios Web. En otras palabras, la herramienta de administración de sitios web es simplemente una interfaz a través de la cual se puede editar el archivo de configuración en un entorno fácil de usar.

## <a name="using-roles-in-code"></a>Usar roles en el código

La API para la administración de roles no ha cambiado desde la versión 1. x. El método **IsInRole** se usa para determinar si un usuario está en un rol determinado.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET también crea una instancia de RolePrincipal como miembro del contexto actual. El objeto RolePrincipal se puede usar para obtener todos los roles a los que pertenece el usuario como se indica a continuación:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Usar RoleGroups con el control LoginView

Ahora que conoce la administración de roles y la pertenencia, analice brevemente cómo el control LoginView aprovecha esta funcionalidad en ASP.NET 2,0. Como se explicó anteriormente, el control LoginView es un control con plantilla que contiene dos plantillas de forma predeterminada. AnonymousTemplate y LoggedInTemplate. En el cuadro de diálogo tareas de LoginView hay un vínculo (que se muestra a continuación) que le permite editar RoleGroups.

![](membership/_static/image9.jpg)

**Ilustración 9**

Cada objeto RoleGroup contiene una matriz de cadenas que define los roles a los que se aplica el RoleGroup. Para agregar un nuevo RoleGroup al control LoginView, haga clic en el vínculo editar RoleGroups. En la imagen anterior, puede ver que he agregado un nuevo RoleGroup para administradores. Al seleccionar el valor de RoleGroup (RoleGroup [0]) en la lista desplegable vistas, puedo configurar una plantilla que solo se mostrará a los miembros del rol administradores. En la imagen siguiente, se ha agregado un nuevo RoleGroup que se aplica a los miembros del rol de ventas y el rol de distribución. Esto agrega un segundo RoleGroup al menú desplegable vistas en el cuadro de diálogo tareas de LoginView y todo lo que se agregue a esa plantilla será visible para cualquier usuario del rol de ventas o de distribución.

![](membership/_static/image10.jpg)

**Figura 10**

## <a name="overriding-the-existing-membership-provider"></a>Reemplazar el proveedor de pertenencia existente

Hay un par de maneras en que puede ampliar la funcionalidad de la pertenencia a ASP.NET. En primer lugar, puede cambiar obviamente la funcionalidad existente de la clase SqlMembershipProvider heredando de ella e invalidando sus métodos. Por ejemplo, si desea implementar su propia funcionalidad cuando se crean usuarios, puede crear su propia clase que herede de SqlMembershipProvider como se indica a continuación:

[!code-csharp[Main](membership/samples/sample5.cs)]

Por otro lado, si desea crear su propio proveedor (para almacenar la información de pertenencia en una base de datos de Access, por ejemplo), puede crear su propio proveedor.

## <a name="creating-your-own-membership-provider"></a>Crear su propio proveedor de pertenencia

Para crear su propio proveedor de pertenencia, primero deberá crear una clase que herede de la clase MembershipProvider. Si usa VB.NET, Visual Studio 2005 agregará el código auxiliar para todos los métodos que necesita invalidar. Si usa C#, puede Agregar el código auxiliar.

Tendrá que invalidar lo siguiente:

- Propiedad ApplicationName
- ChangePassword (función)
- ChangePasswordQuestionAndAnswer función)
- CreateUser (función)
- Función DeleteUser
- EnablePasswordReset (propiedad)
- Propiedad EnablePasswordRetrieval
- FindUsersByEmail función)
- FindUsersByName función)
- GetAllUsers función)
- GetNumberOfUsersOnline función)
- GetPassword (función)
- GetUser, función
- GetUserNameByEmail función)
- Propiedad MaxInvalidPasswordAttempts
- Propiedad MinRequiredNonAlphanumericCharacters
- MinRequiredPasswordLength (propiedad)
- Propiedad PasswordAttemptWindow
- Propiedad PasswordFormat
- Propiedad PasswordStrengthRegularExpression
- Propiedad RequiresQuestionAndAnswer
- Propiedad RequiresUniqueEmail
- ResetPassword función)
- Desbloqueo de la función de usuario
- UpdateUser función)
- ValidateUser función)

Que es una lista que se va a implementar C# como desarrollador. Puede que le resulte más fácil crear la clase en VB.NET sin ninguna implementación y, a continuación, usar .NET reflector u otra herramienta similar para C#convertir el código en.

La cadena de conexión y otras propiedades deben establecerse en sus valores predeterminados en el método Initialize. (El método Initialize se activa cuando el proveedor se carga en tiempo de ejecución). El segundo parámetro del método Initialize es de tipo System. Collections. Specialized. NameValueCollection y es una referencia al &lt;agregar&gt; elemento asociado a su proveedor personalizado en el archivo Web. config. Esa entrada tiene el siguiente aspecto:

[!code-xml[Main](membership/samples/sample6.xml)]

Este es un ejemplo del método Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar al usuario cuando envíe su formulario de inicio de sesión, debe usar el método ValidateUser. Este método se desencadena cuando el usuario hace clic en el botón de inicio de sesión del control de inicio de sesión. En este método, colocará el código que realiza la búsqueda del usuario.

Como puede ver, escribir su propio proveedor de pertenencia no es difícil y le permite ampliar esta eficaz funcionalidad de ASP.NET 2,0.
