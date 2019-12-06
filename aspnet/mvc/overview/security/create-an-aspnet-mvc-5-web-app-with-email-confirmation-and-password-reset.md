---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correoC#electrónico y restablecimiento de contraseña () | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo compilar una aplicación Web ASP.NET MVC 5 con confirmación de correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia a ASP.NET Identity. CA...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899693"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, confirmación por correo electrónico y restablecimiento de contraseña (C#)

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

En este tutorial se muestra cómo compilar una aplicación Web ASP.NET MVC 5 con confirmación de correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia a ASP.NET Identity.

Para obtener una versión actualizada de este tutorial que usa .NET Core, consulte [confirmación de cuenta y recuperación de contraseña en ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creación de una aplicación de ASP.NET MVC

Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o posterior.

> [!NOTE]
> ADVERTENCIA: debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.

1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Los formularios Web Forms también admiten ASP.NET Identity, por lo que puede seguir pasos similares en una aplicación de formularios Web Forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla. Más adelante en el tutorial se implementará en Azure. Puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establezca el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Ejecute la aplicación, haga clic en el vínculo **registrar** y registre un usuario. En este momento, la única validación en el correo electrónico es con el atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
5. En Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic con el botón derecho y seleccione **abrir definición de tabla**.

    En la imagen siguiente se muestra el esquema de `AspNetUsers`:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Haga clic con el botón derecho en la tabla **AspNetUsers** y seleccione **Mostrar datos de tabla**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 En este momento no se ha confirmado el correo electrónico.
7. Haga clic en la fila y seleccione eliminar. Agregará este correo electrónico de nuevo en el paso siguiente y enviará un correo electrónico de confirmación.

## <a name="email-confirmation"></a>Confirmación por correo electrónico

Se recomienda confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no está suplantando a otra persona (es decir, que no se han registrado con el correo electrónico de otra persona). Supongamos que tiene un foro de discusión, desea evitar que `"bob@example.com"` se registre como `"joe@contoso.com"`. Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener un correo electrónico no deseado de la aplicación. Supongamos que Bob se registra accidentalmente como `"bib@example.com"` y no lo ha notado, no podría usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto. La confirmación por correo electrónico proporciona solo protección limitada de bots y no proporciona protección contra los remitentes de spam determinados, tienen muchos alias de correo electrónico que pueden usar para registrarse.

Por lo general, querrá evitar que los nuevos usuarios publiquen datos en el sitio Web antes de que se hayan confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo. <a id="build"></a>En las secciones siguientes, habilitaremos la confirmación por correo electrónico y modificaremos el código para evitar que los usuarios recién registrados inicien sesión hasta que se haya confirmado su correo electrónico.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Enlazar SendGrid

Las instrucciones de esta sección no son actuales. Consulte [configurar el proveedor de correo electrónico de SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) para obtener instrucciones actualizadas.

Aunque este tutorial solo muestra cómo agregar una notificación por correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).

1. En la Consola del Administrador de paquetes, escriba el siguiente comando: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vaya a la página de registro de [Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y regístrese para obtener una cuenta gratuita de SendGrid. Configure SendGrid agregando código similar al siguiente en *App_Start/identityconfig.CS*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Deberá agregar lo siguiente:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para simplificar este ejemplo, almacenaremos la configuración de la aplicación en el archivo *Web. config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se almacenan en appSetting. En Azure, puede almacenar estos valores de forma segura en la pestaña **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** del Azure portal. Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar la confirmación por correo electrónico en el controlador de cuenta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Compruebe que el archivo *Views\Account\ConfirmEmail.cshtml* tiene la sintaxis de Razor correcta. (Es posible que falte el carácter @ en la primera línea. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Ejecute la aplicación y haga clic en el vínculo registrar. Una vez enviado el formulario de registro, ha iniciado sesión.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Requerir confirmación por correo electrónico antes de iniciar sesión

Actualmente, una vez que un usuario completa el formulario de registro, inicia sesión. Por lo general, querrá confirmar el correo electrónico antes de registrarlo en. En la sección siguiente, se modificará el código para requerir que los nuevos usuarios tengan un correo electrónico confirmado antes de que se inicie sesión (autenticado). Actualice el método de `HttpPost Register` con los siguientes cambios resaltados:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Al comentar el método `SignInAsync`, el usuario no iniciará sesión en el registro. La línea de `TempData["ViewBagLink"] = callbackUrl;` se puede usar para [depurar la aplicación y el](#dbg) registro de prueba sin enviar correo electrónico. `ViewBag.Message` se usa para mostrar las instrucciones de confirmación. El [ejemplo de descarga](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin configurar el correo electrónico y también se puede usar para depurar la aplicación.

Cree un archivo de `Views\Shared\Info.cshtml` y agregue el siguiente marcado de Razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Agregue el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) al método de acción `Contact` del controlador Home. Puede hacer clic en el vínculo **contacto** para comprobar que los usuarios anónimos no tienen acceso y que los usuarios autenticados tienen acceso.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

También debe actualizar el método de acción `HttpPost Login`:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Actualice la vista *Views\Shared\Error.cshtml* para mostrar el mensaje de error:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Elimine todas las cuentas de la tabla **AspNetUsers** que contengan el alias de correo electrónico con el que desea probar. Ejecute la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado su dirección de correo electrónico. Una vez confirmada la dirección de correo electrónico, haga clic en el vínculo **contacto** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Recuperación/restablecimiento de contraseñas

Quite los caracteres de comentario del método de acción `HttpPost ForgotPassword` del controlador de cuenta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Quite los caracteres de comentario del `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el archivo de vista de Razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

La página de inicio de sesión mostrará ahora un vínculo para restablecer la contraseña.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Vínculo reenviar correo electrónico de confirmación

Una vez que un usuario crea una nueva cuenta local, se le envía un vínculo de confirmación para que pueda iniciar sesión. Si el usuario elimina accidentalmente el correo electrónico de confirmación o el correo electrónico nunca llega, deberá volver a enviar el vínculo de confirmación. Los siguientes cambios de código muestran cómo habilitarlo.

Agregue el siguiente método auxiliar a la parte inferior del archivo *Controllers\AccountController.CS* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Actualice el método de registro para usar la nueva aplicación auxiliar:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Actualice el método de inicio de sesión para volver a enviar la contraseña si no se ha confirmado la cuenta de usuario:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinación de cuentas de inicio de sesión sociales y locales

Para combinar cuentas locales y sociales, haga clic en el vínculo de correo electrónico. En la siguiente secuencia **RickAndMSFT@gmail.com** se crea primero como un inicio de sesión local, pero puede crear la cuenta como un registro social en primer lugar y luego agregar un inicio de sesión local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Haga clic en el vínculo **administrar** . Anote los **inicios de sesión externos: 0** asociados a esta cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de la aplicación. Las dos cuentas se han combinado, podrá iniciar sesión con cualquier cuenta. Es posible que desee que los usuarios agreguen cuentas locales en caso de que su registro social en el servicio de autenticación esté inactivo, o más probabilidades de que hayan perdido el acceso a su cuenta social.

En la imagen siguiente, Tom es un inicio de sesión social (que puede ver en los **inicios de sesión externos: 1** que se muestra en la página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Al hacer clic en **seleccionar una contraseña** , puede Agregar un inicio de sesión local asociado a la misma cuenta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmación de correo electrónico con más profundidad

La confirmación de la cuenta de tutorial [y la recuperación de contraseñas con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entran en este tema con más detalles.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurar la aplicación

Si no recibe un correo electrónico que contenga el vínculo:

- Compruebe la carpeta de correo no deseado o correo no deseado.
- Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo actividad de correo electrónico](https://sendgrid.com/logs/index).

Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completado](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). En la página se mostrarán los códigos de confirmación y el vínculo de confirmación.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Vínculos a ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Profundiza en la recuperación de contraseñas y la confirmación de la cuenta.
- [Aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) En este tutorial se muestra cómo escribir una aplicación ASP.NET MVC 5 con la autorización de Facebook y Google OAuth 2. También se muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implemente una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). En este tutorial se agrega la implementación de Azure, cómo proteger la aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y otras características de seguridad.
- [Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Crear la aplicación en Facebook y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuración de SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
