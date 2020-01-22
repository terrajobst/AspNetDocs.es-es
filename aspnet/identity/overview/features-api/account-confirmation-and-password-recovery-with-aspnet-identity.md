---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Confirmación de cuenta & recuperación de contraseña-C#ASP.net Identity ()-ASP.net 4. x
author: HaoK
description: Antes de realizar este tutorial, primero debe completar la creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña. Este tutorial...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4b2c88280df39aa81d60f9508910e8fe5d6db6b8
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519120"
---
# <a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmación de la cuenta y recuperación de laC#contraseña con ASP.net Identity ()

> Antes de realizar este tutorial, primero debe completar la [creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Este tutorial contiene más detalles y le mostrará cómo configurar el correo electrónico para la confirmación de la cuenta local y permitir que los usuarios restablezcan su contraseña olvidada en ASP.NET Identity.

Una cuenta de usuario local requiere que el usuario cree una contraseña para la cuenta y que esa contraseña se almacene (de forma segura) en la aplicación Web. ASP.NET Identity también admite cuentas de redes sociales, que no requieren que el usuario cree una contraseña para la aplicación. [Las cuentas de redes sociales](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) usan un tercero (por ejemplo, Google, Twitter, Facebook o Microsoft) para autenticar a los usuarios. En este tema se trata lo siguiente:

- [Cree una aplicación ASP.NET MVC](#createMvc) y explore ASP.net Identity características.
- [Compilación del ejemplo de identidad](#build)
- [Configuración de la confirmación por correo electrónico](#email)

Los nuevos usuarios registran su alias de correo electrónico, que crea una cuenta local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

Al seleccionar el botón registrar se envía un correo electrónico de confirmación que contiene un token de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

Al usuario se le envía un correo electrónico con un token de confirmación para su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Al seleccionar el vínculo, se confirma la cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseñas

Los usuarios locales que olvidan su contraseña pueden tener un token de seguridad enviado a su cuenta de correo electrónico, lo que les permite restablecer su contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
Pronto recibirá un correo electrónico con un vínculo que le permitirá restablecer su contraseña.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Al seleccionar el vínculo, se le llevará a la página restablecer.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
Si selecciona el botón **restablecer** , se confirmará que la contraseña se ha restablecido.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Crear una aplicación web ASP.NET

Empiece por instalar y ejecutar [Visual Studio 2017](https://visualstudio.microsoft.com/).

1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Los formularios Web Forms también admiten ASP.NET Identity, por lo que puede seguir pasos similares en una aplicación de formularios Web Forms.
2. Cambie la autenticación a **cuentas de usuario individuales**.
3. Ejecute la aplicación, seleccione el vínculo **registrar** y registre un usuario. En este momento, la única validación en el correo electrónico es con el atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
4. En Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic con el botón derecho y seleccione **abrir definición de tabla**.

    En la imagen siguiente se muestra el esquema de `AspNetUsers`:

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Haga clic con el botón derecho en la tabla **AspNetUsers** y seleccione **Mostrar datos de tabla**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   En este momento no se ha confirmado el correo electrónico.

El almacén de datos predeterminado para ASP.NET Identity es Entity Framework, pero puede configurarlo para usar otros almacenes de datos y agregar campos adicionales. Vea la sección [recursos adicionales](#addRes) al final de este tutorial.

Se llama a la [clase de inicio OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.CS* ) cuando la aplicación se inicia e invoca el método `ConfigureAuth` en *App\_Start\Startup.auth.CS*, que configura la canalización OWIN e inicializa ASP.net Identity. Examine el método `ConfigureAuth`. Cada llamada `CreatePerOwinContext` registra una devolución de llamada (guardada en el `OwinContext`) que se llamará una vez por solicitud para crear una instancia del tipo especificado. Puede establecer un punto de interrupción en el constructor y `Create` método de cada tipo (`ApplicationDbContext, ApplicationUserManager`) y comprobar que se llama en cada solicitud. Una instancia de `ApplicationDbContext` y `ApplicationUserManager` se almacena en el contexto OWIN, al que se puede tener acceso a través de la aplicación. ASP.NET Identity enlaza en la canalización OWIN a través del middleware de cookies. Para obtener más información, consulte [Administración de la duración de cada solicitud para la clase UserManager en ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Al cambiar el perfil de seguridad, se genera una nueva marca de seguridad y se almacena en el `SecurityStamp` campo de la tabla *AspNetUsers* . Tenga en cuenta que el campo `SecurityStamp` es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en la tabla `AspNetUsers` (ni en ninguna otra parte de la base de de identidad). El token de la cookie de seguridad se firma automáticamente mediante [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) y se crea con la información de tiempo de expiración y `UserId, SecurityStamp`.

El middleware de cookies comprueba la cookie en cada solicitud. El método `SecurityStampValidator` de la clase `Startup` alcanza la base de registros y comprueba la marca de seguridad periódicamente, según se especifique con el `validateInterval`. Esto solo sucede cada 30 minutos (en nuestro ejemplo), a menos que cambie el perfil de seguridad. Se eligió el intervalo de 30 minutos para minimizar los viajes a la base de datos. Consulte el [tutorial de autenticación en dos fases](index.md) para obtener más detalles.

Según los comentarios del código, el método `UseCookieAuthentication` admite la autenticación de cookies. El campo `SecurityStamp` y el código asociado proporcionan una capa de seguridad adicional a la aplicación, cuando se cambia la contraseña, se cerrará la sesión del explorador con el que ha iniciado sesión. El método `SecurityStampValidator.OnValidateIdentity` permite a la aplicación validar el token de seguridad cuando el usuario inicia sesión, que se usa cuando se cambia una contraseña o se usa el inicio de sesión externo. Esto es necesario para asegurarse de que se invalidan los tokens (cookies) generados con la contraseña antigua. En el proyecto de ejemplo, si cambia la contraseña de los usuarios, se genera un nuevo token para el usuario, se invalidan los tokens anteriores y se actualiza el campo de `SecurityStamp`.

El sistema de identidad le permite configurar la aplicación de modo que, cuando cambie el perfil de seguridad de los usuarios (por ejemplo, cuando el usuario cambie la contraseña o cambie el inicio de sesión asociado (por ejemplo, de Facebook, Google, cuenta de Microsoft, etc.), se cerrará la sesión del usuario. instancias del explorador. Por ejemplo, en la imagen siguiente se muestra la aplicación de [ejemplo Single SignOut](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) , que permite al usuario cerrar sesión en todas las instancias del explorador (en este caso, IE, Firefox y Chrome) seleccionando un botón. Como alternativa, el ejemplo solo permite cerrar la sesión de una instancia específica del explorador.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

La aplicación de [ejemplo Single SignOut](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SingleSignOutSample) muestra cómo ASP.net Identity permite volver a generar el token de seguridad. Esto es necesario para asegurarse de que se invalidan los tokens (cookies) generados con la contraseña antigua. Esta característica proporciona un nivel de seguridad adicional a la aplicación. al cambiar la contraseña, se cerrará la sesión en la que haya iniciado sesión en esta aplicación.

El archivo *App\_Start\IdentityConfig.CS* contiene las clases `ApplicationUserManager`, `EmailService` y `SmsService`. Cada una de las clases `EmailService` y `SmsService` implementa la interfaz `IIdentityMessageService`, por lo que tiene métodos comunes en cada clase para configurar el correo electrónico y SMS. Aunque este tutorial solo muestra cómo agregar una notificación por correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos.

La clase `Startup` también contiene la placa de la caldera para agregar inicios de sesión sociales (Facebook, Twitter, etc.). Consulte mi tutorial de la [aplicación MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 inicio de sesión](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) para obtener más información.

Examine la clase `ApplicationUserManager`, que contiene la información de identidad de los usuarios y configura las características siguientes:

- Requisitos de seguridad de contraseñas.
- Bloqueo de usuario (intentos y tiempo).
- Autenticación en dos fases (2FA). Trataremos 2FA y SMS en otro tutorial.
- Enlazar el correo electrónico y los servicios de SMS. (Hablaré de SMS en otro tutorial).

La clase `ApplicationUserManager` se deriva de la clase `UserManager<ApplicationUser>` genérica. `ApplicationUser` deriva de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` deriva de la clase de `IdentityUser` genérica:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Las propiedades anteriores coinciden con las propiedades de la tabla `AspNetUsers`, mostrada anteriormente.

Los argumentos genéricos en `IUser` permiten derivar una clase utilizando distintos tipos para la clave principal. Vea el ejemplo [ChangePK](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) , que muestra cómo cambiar la clave principal de String a int o GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) se define en *Models\IdentityModels.CS* como:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

El código resaltado anterior genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity y la autenticación de cookies OWIN están basadas en notificaciones, por lo tanto, el marco de trabajo requiere que la aplicación genere un `ClaimsIdentity` para el usuario. `ClaimsIdentity` contiene información sobre todas las notificaciones para el usuario, como el nombre del usuario, la edad y los roles a los que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.

El método OWIN `AuthenticationManager.SignIn` pasa el `ClaimsIdentity` e inicia sesión en el usuario:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

La [aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) muestra cómo puede Agregar propiedades adicionales a la clase `ApplicationUser`.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Es una buena idea confirmar el correo electrónico con el que se registra un nuevo usuario para comprobar que no está suplantando a otra persona (es decir, que no se han registrado con el correo electrónico de otra persona). Supongamos que tiene un foro de discusión, desea evitar que `"bob@example.com"` se registre como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener un correo electrónico no deseado de la aplicación. Supongamos que Bob se registra accidentalmente como `"bib@example.com"` y no lo ha notado, no podría usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. La confirmación por correo electrónico proporciona solo protección limitada de bots y no proporciona protección contra los remitentes de spam determinados, tienen muchos alias de correo electrónico que pueden usar para registrarse. En el ejemplo siguiente, el usuario no podrá cambiar su contraseña hasta que se haya confirmado su cuenta (para ello, seleccionando un vínculo de confirmación recibido en la cuenta de correo electrónico con la que se registraron). Puede aplicar este flujo de trabajo a otros escenarios, por ejemplo, enviar un vínculo para confirmar y restablecer la contraseña en nuevas cuentas creadas por el administrador, enviar al usuario un correo electrónico cuando haya cambiado su perfil y así sucesivamente. Por lo general, querrá evitar que los nuevos usuarios publiquen datos en el sitio Web antes de que se hayan confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo. <a id="build"></a>

## <a name="build-a-more-complete-sample"></a>Crear un ejemplo más completo

En esta sección, usará NuGet para descargar un ejemplo más completo con el que trabajaremos.

1. Cree un nuevo proyecto Web de ASP.NET ***vacío*** .
2. En la consola del administrador de paquetes, escriba los siguientes comandos: 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   En este tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar correo electrónico. El paquete `Identity.Samples` instala el código con el que se va a trabajar.
3. Establezca el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Para probar la creación de cuentas locales, ejecute la aplicación, seleccione el vínculo **registrar** y publique el formulario de registro.
5. Seleccione el vínculo de correo electrónico de demostración, que simula la confirmación de correo electrónico.
6. Quite el código de confirmación de vínculo de correo electrónico de demostración del ejemplo (el código `ViewBag.Link` del controlador de la cuenta. Vea los métodos de acción `DisplayEmail` y `ForgotPasswordConfirmation` y las vistas de Razor).

> [!WARNING]
> Si cambia cualquiera de las opciones de configuración de seguridad de este ejemplo, las aplicaciones de producción deberán someterse a una auditoría de seguridad que llame explícitamente a los cambios realizados.

## <a name="examine-the-code-in-app_startidentityconfigcs"></a>Examinar el código en App\_Start\IdentityConfig.cs

En el ejemplo se muestra cómo crear una cuenta y agregarla al rol de *Administrador* . Debe reemplazar el correo electrónico en el ejemplo por el correo electrónico que va a usar para la cuenta de administrador. La forma más sencilla de crear una cuenta de administrador es mediante programación en el método `Seed`. Esperamos tener una herramienta en el futuro que le permita crear y administrar usuarios y roles. El código de ejemplo le permite crear y administrar usuarios y roles, pero primero debe tener una cuenta de administradores para ejecutar los roles y las páginas de administración de usuarios. En este ejemplo, la cuenta de administrador se crea cuando se inicializa la base de BD.

Cambie la contraseña y cambie el nombre por una cuenta en la que pueda recibir notificaciones de correo electrónico.

> [!WARNING]
> Seguridad: nunca almacene datos confidenciales en el código fuente.

Como se mencionó anteriormente, la llamada a `app.CreatePerOwinContext` en la clase startup agrega las devoluciones de llamada al método `Create` de las clases de contenido de la base de aplicaciones, administrador de usuarios y administrador de roles. La canalización OWIN llama al método `Create` en estas clases para cada solicitud y almacena el contexto de cada clase. El controlador de cuentas expone el administrador de usuarios desde el contexto HTTP (que contiene el contexto OWIN):

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Cuando un usuario registra una cuenta local, se llama al método `HTTP Post Register`:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

El código anterior usa los datos del modelo para crear una nueva cuenta de usuario mediante el correo electrónico y la contraseña especificados. Si el alias de correo electrónico se encuentra en el almacén de datos, se produce un error en la creación de la cuenta y el formulario se muestra de nuevo. El método `GenerateEmailConfirmationTokenAsync` crea un token de confirmación seguro y lo almacena en el almacén de datos ASP.NET Identity. El método [URL. Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) crea un vínculo que contiene el `UserId` y el token de confirmación. Este vínculo se envía por correo electrónico al usuario, el usuario puede seleccionar en el vínculo de la aplicación de correo electrónico para confirmar su cuenta.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configuración de la confirmación por correo electrónico

Vaya a la página de registro de [Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) y regístrese para obtener una cuenta gratuita. Agregue código similar al siguiente para configurar SendGrid:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Con frecuencia, los clientes de correo electrónico solo aceptan mensajes de texto (no HTML). Debe proporcionar el mensaje en texto y HTML. En el ejemplo de SendGrid anterior, esto se hace con el código `myMessage.Text` y `myMessage.Html` mostrado anteriormente.

En el código siguiente se muestra cómo enviar correo electrónico mediante la clase [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) , donde `message.Body` devuelve solo el vínculo.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en appSetting. En Azure, puede almacenar estos valores de forma segura en la pestaña **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** del Azure portal. Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

Escriba sus credenciales de SendGrid, ejecute la aplicación y regístrese con un alias de correo electrónico puede seleccionar el vínculo confirmar en el correo electrónico. Para ver cómo hacer esto con su cuenta de correo electrónico de [Outlook.com](http://outlook.com) , consulte Configuración de SMTP de John ATTEN [ C# para host SMTP de Outlook.com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) y su[ASP.net Identity 2,0: configuración de la validación de cuentas y publicaciones de autorización en dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) .

Una vez que un usuario selecciona el botón **registrar** , se envía un correo electrónico de confirmación que contiene un token de validación a su dirección de correo electrónico.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

Al usuario se le envía un correo electrónico con un token de confirmación para su cuenta.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examen del código

En el código siguiente se muestra el método `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

El método produce un error en modo silencioso si no se ha confirmado el correo electrónico del usuario. Si se ha publicado un error para una dirección de correo electrónico no válida, los usuarios malintencionados podrían usar esa información para buscar un identificador de usuario (alias de correo electrónico) válido para atacar.

En el código siguiente se muestra el método `ConfirmEmail` del controlador de cuenta al que se llama cuando el usuario selecciona el vínculo de confirmación en el correo electrónico que se le ha enviado:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Una vez que se ha usado un token de contraseña olvidado, se invalida. El siguiente cambio de código en el método `Create` (en el archivo *App\_Start\IdentityConfig.CS* ) establece que los tokens expiren en 3 horas.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Con el código anterior, la contraseña olvidada y los tokens de confirmación de correo electrónico expirarán en 3 horas. El `TokenLifespan` predeterminado es un día.

En el código siguiente se muestra el método de confirmación por correo electrónico:

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Para que la aplicación sea más segura, ASP.NET Identity admite la autenticación en dos fases (2FA). Consulte [ASP.NET Identity 2,0: configurar la validación de la cuenta y la autorización en dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John ATTEN. Aunque puede establecer el bloqueo de cuentas en errores de intento de contraseña de inicio de sesión, este enfoque hace que el inicio de sesión sea susceptible a bloqueos de [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Se recomienda usar el bloqueo de cuentas solo con 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- La [aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [ASP.NET MVC e Identity 2,0: Descripción de los conceptos básicos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) de John ATTEN.
- [Introducción a ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Presentación de RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.
