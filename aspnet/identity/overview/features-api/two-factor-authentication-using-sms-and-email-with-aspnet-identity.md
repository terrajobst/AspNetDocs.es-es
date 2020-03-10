---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity ASP.NET 4. x
author: HaoK
description: En este tutorial se muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS y el correo electrónico. Este artículo lo ha escrito Rick Anderson (@RickAndMSFT), PR...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 527b4392846e60dae0b216fdeabf21fd6618e4d7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78499873"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity

por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> En este tutorial se muestra cómo configurar la autenticación en dos fases (2FA) mediante SMS y el correo electrónico.
> 
> Este artículo lo ha escrito Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung y Suhas Joshi. El ejemplo de NuGet se escribió principalmente en Hao Kung.

En este tema se trata lo siguiente:

- [Compilación del ejemplo de identidad](#build)
- [Configurar SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Registro de un proveedor de autenticación en dos fases](#reg)
- [Combinación de cuentas de inicio de sesión sociales y locales](#combine)
- [Bloqueo de cuentas contra ataques por fuerza bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Compilación del ejemplo de identidad

En esta sección, usará NuGet para descargar un ejemplo con el que trabajaremos. Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) o una versión posterior.

> [!NOTE]
> ADVERTENCIA: debe instalar Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) para completar este tutorial.

1. Cree un nuevo proyecto Web de ASP.NET ***vacío*** .
2. En la consola del administrador de paquetes, escriba los siguientes comandos:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   En este tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar correo electrónico y [Twilio](https://www.twilio.com/) o [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) para el texto de SMS. El paquete `Identity.Samples` instala el código con el que se va a trabajar.
3. Establezca el [proyecto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga las instrucciones del [tutorial de confirmación de correo electrónico](account-confirmation-and-password-recovery-with-aspnet-identity.md) para enlazar SendGrid y, a continuación, ejecute la aplicación y registre una cuenta de correo electrónico.
5. *Opcional:* Quite el código de confirmación de vínculo de correo electrónico de demostración del ejemplo (el código `ViewBag.Link` del controlador de la cuenta. Vea los métodos de acción `DisplayEmail` y `ForgotPasswordConfirmation` y las vistas de Razor).
6. *Opcional:* Quite el código `ViewBag.Status` de los controladores de administración y de cuenta, y de las vistas de Razor *Views\Account\VerifyCode.cshtml* y *Views\Manage\VerifyPhoneNumber.cshtml* . Como alternativa, puede mantener el `ViewBag.Status` Mostrar para probar cómo funciona esta aplicación localmente sin tener que enlazar y enviar mensajes de correo electrónico y SMS.

> [!NOTE]
> ADVERTENCIA: Si cambia cualquiera de las opciones de configuración de seguridad de este ejemplo, las aplicaciones de producción deberán someterse a una auditoría de seguridad que llame explícitamente a los cambios realizados.

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar SMS para la autenticación en dos fases

En este tutorial se proporcionan instrucciones sobre el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor de SMS.

1. **Creación de una cuenta de usuario con un proveedor de SMS**  
  
   Cree una cuenta de [Twilio](https://www.twilio.com/try-twilio) o de [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Instalación de paquetes adicionales o adición de referencias de servicio**  
  
   Twilio  
   En la Consola del Administrador de paquetes, escriba el siguiente comando:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   Debe agregarse la siguiente referencia de servicio:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espacio de nombres:  
    `ASPSMSX2`
3. **Averiguación de las credenciales de usuario del proveedor de SMS**  
  
   Twilio  
   En la pestaña **Panel** de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.  
  
   ASPSMS:  
   En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**autodefinida.  
  
   Posteriormente, almacenaremos estos valores en las variables `SMSAccountIdentification` y `SMSAccountPassword`.
4. **Especificar SenderID/originador**  
  
   Twilio  
   En la pestaña **números** , copie el número de teléfono de Twilio.  
  
   ASPSMS:  
   En el menú **desbloquear orígenes** , desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).  
  
   Más adelante se almacenará este valor en la variable `SMSAccountFrom`.
5. **Transferencia de credenciales de proveedor de SMS a la aplicación**  
  
   Haga que las credenciales y el número de teléfono del remitente estén disponibles para la aplicación:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple. Consulte ASP.NET MVC de Jon ATTEN [: mantener la configuración privada fuera del control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementación de la transferencia de datos al proveedor de SMS**  
  
   Configure la clase `SmsService` en el archivo *App\_Start\IdentityConfig.CS* .  
  
   En función del proveedor de SMS utilizado, active la sección **Twilio** o **ASPSMS** : 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.
8. Haga clic en el identificador de usuario, que activa el método de acción `Index` en el controlador de `Manage`.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Haga clic en Agregar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. En unos segundos, recibirá un mensaje de texto con el código de verificación. Escríbalo y presione **submit (enviar**).  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. La vista administrar muestra el número de teléfono que se ha agregado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examen del código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

El método de acción `Index` del controlador de `Manage` establece el mensaje de estado en función de la acción anterior y proporciona vínculos para cambiar la contraseña local o agregar una cuenta local. El método `Index` también muestra el estado o el número de teléfono 2FA, inicios de sesión externos, 2FA habilitados y recordar el método 2FA para este explorador (se explica más adelante). Al hacer clic en su ID. de usuario (correo electrónico) en la barra de título no se pasa un mensaje. Al hacer clic en el **número de teléfono: Remove** link pasa `Message=RemovePhoneSuccess` como una cadena de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

El método de acción `AddPhoneNumber` muestra un cuadro de diálogo para escribir un número de teléfono que pueda recibir mensajes SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Al hacer clic en el botón **Enviar código de verificación** , se envía el número de teléfono al método de acción HTTP post `AddPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

El método `GenerateChangePhoneNumberTokenAsync` genera el token de seguridad que se establecerá en el mensaje SMS. Si se ha configurado el servicio de SMS, el token se envía como la cadena &quot;el código de seguridad está &lt;token&gt;&quot;. El método de `SmsService.SendAsync` a se llama de forma asincrónica y, a continuación, la aplicación se redirige al método de acción `VerifyPhoneNumber` (que muestra el siguiente cuadro de diálogo), donde puede especificar el código de verificación.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Una vez que escriba el código y haga clic en Submit (enviar), el código se expone en el método de acción HTTP POST `VerifyPhoneNumber`.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

El método `ChangePhoneNumberAsync` comprueba el código de seguridad expuesto. Si el código es correcto, el número de teléfono se agrega al campo `PhoneNumber` de la tabla `AspNetUsers`. Si la llamada se realiza correctamente, se llama al método `SignInAsync`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

El parámetro `isPersistent` establece si la sesión de autenticación se conserva en varias solicitudes.

Al cambiar el perfil de seguridad, se genera una nueva marca de seguridad y se almacena en el `SecurityStamp` campo de la tabla *AspNetUsers* . Tenga en cuenta que el campo `SecurityStamp` es diferente de la cookie de seguridad. La cookie de seguridad no se almacena en la tabla `AspNetUsers` (ni en ninguna otra parte de la base de de identidad). El token de la cookie de seguridad se firma automáticamente mediante [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) y se crea con la información de tiempo de expiración y `UserId, SecurityStamp`.

El middleware de cookies comprueba la cookie en cada solicitud. El método `SecurityStampValidator` de la clase `Startup` alcanza la base de registros y comprueba la marca de seguridad periódicamente, según se especifique con el `validateInterval`. Esto solo sucede cada 30 minutos (en nuestro ejemplo), a menos que cambie el perfil de seguridad. Se eligió el intervalo de 30 minutos para minimizar los viajes a la base de datos.

Se debe llamar al método `SignInAsync` cuando se realiza algún cambio en el perfil de seguridad. Cuando cambia el perfil de seguridad, la base de datos se actualiza en el campo `SecurityStamp` y, sin llamar al método `SignInAsync`, permanecerá conectado *solo* hasta la próxima vez que la canalización OWIN llegue a la base de datos (el `validateInterval`). Puede probar esto cambiando el método `SignInAsync` para que se devuelva inmediatamente y estableciendo la propiedad cookie `validateInterval` de 30 minutos a 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Con los cambios de código anteriores, puede cambiar el perfil de seguridad (por ejemplo, cambiando el estado de **dos factores habilitados**) y se cerrará la sesión en 5 segundos cuando se produzca un error en el método `SecurityStampValidator.OnValidateIdentity`. Quite la línea de retorno en el método `SignInAsync`, realice otro cambio en el perfil de seguridad y no se cerrará la sesión. El método `SignInAsync` genera una nueva cookie de seguridad.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación de ejemplo, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en su ID. de usuario (alias de correo electrónico) en la barra de navegación.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Haga clic en habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Cierre la sesión y vuelva a iniciarla. Si ha habilitado el correo electrónico (vea mi [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o el correo electrónico de 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) Se muestra la página comprobar código donde puede escribir el código (desde SMS o correo electrónico).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Al hacer clic en la casilla **recordar este explorador** , no es necesario usar 2FA para iniciar sesión con ese equipo y explorador. Al habilitar 2FA y hacer clic en el **recordatorio, este explorador** le proporcionará protección de 2FA segura a los usuarios malintencionados que intentan acceder a su cuenta, siempre y cuando no tengan acceso a su equipo. Puede hacerlo en cualquier equipo privado que use con regularidad. Al establecer la opción **recordar este explorador**, obtendrá la seguridad agregada de 2FA de los equipos que no usa con regularidad y que le ayuda a no tener que ir a través de 2FA en sus propios equipos. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Registro de un proveedor de autenticación en dos fases

Al crear un nuevo proyecto de MVC, el archivo *IdentityConfig.CS* contiene el código siguiente para registrar un proveedor de autenticación en dos fases:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Agregar un número de teléfono para 2FA

El método de acción `AddPhoneNumber` del controlador de `Manage` genera un token de seguridad y lo envía al número de teléfono que ha proporcionado.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Después de enviar el token, se redirige al método de acción `VerifyPhoneNumber`, donde puede especificar el código para registrar SMS para 2FA. SMS 2FA no se usa hasta que se ha comprobado el número de teléfono.

## <a name="enabling-2fa"></a>Habilitación de 2FA

El método de acción `EnableTFA` habilita 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Tenga en cuenta que se debe llamar al `SignInAsync` porque enable 2FA es un cambio en el perfil de seguridad. Cuando 2FA está habilitado, el usuario tendrá que usar 2FA para iniciar sesión, mediante los enfoques de 2FA que han registrado (SMS y correo electrónico en el ejemplo).

Puede agregar más proveedores de 2FA, como generadores de código QR, o puede escribir su propio (consulte [uso de Google Authenticator con ASP.net Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Los códigos 2FA se generan mediante el [algoritmo de contraseña de un solo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) uso y los códigos son válidos durante seis minutos. Si tarda más de seis minutos en escribir el código, obtendrá un mensaje de error de código no válido.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinación de cuentas de inicio de sesión sociales y locales

Para combinar cuentas locales y sociales, haga clic en el vínculo de correo electrónico. En la secuencia siguiente &quot;RickAndMSFT@gmail.com&quot; se crea primero como un inicio de sesión local, pero puede crear primero la cuenta como un registro de redes sociales y luego agregar un inicio de sesión local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Haga clic en el vínculo **administrar** . Observe los 0 externos (inicios de sesión sociales) asociados a esta cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de la aplicación. Las dos cuentas se han combinado, podrá iniciar sesión con cualquier cuenta. Es posible que desee que los usuarios agreguen cuentas locales en caso de que su registro social en el servicio de autenticación esté inactivo, o más probabilidades de que hayan perdido el acceso a su cuenta social.

En la imagen siguiente, Tom es un inicio de sesión social (que puede ver en los **inicios de sesión externos: 1** que se muestra en la página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Al hacer clic en **seleccionar una contraseña** , puede Agregar un inicio de sesión local asociado a la misma cuenta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueo de cuentas contra ataques por fuerza bruta

Puede proteger las cuentas de la aplicación de ataques de diccionario habilitando el bloqueo de usuario. El siguiente código del método `ApplicationUserManager Create` configura el bloqueo:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

El código anterior solo habilita el bloqueo para la autenticación en dos fases. Aunque puede habilitar el bloqueo para los inicios de sesión cambiando `shouldLockout` a true en el método de `Login` del controlador de cuenta, se recomienda no habilitar el bloqueo para los inicios de sesión, ya que hace que la cuenta sea susceptible a ataques de inicio de sesión de [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . En el código de ejemplo, el bloqueo está deshabilitado para la cuenta de administrador creada en el método `ApplicationDbInitializer Seed`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Requerir a un usuario que tenga una cuenta de correo electrónico validada

El código siguiente requiere que un usuario tenga una cuenta de correo electrónico validada para poder iniciar sesión:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Cómo comprueba SignInManager el requisito de 2FA

Tanto el inicio de sesión local como el inicio de sesión social en se comprueban para ver si 2FA está habilitado. Si 2FA está habilitado, el método de inicio de sesión `SignInManager` devuelve `SignInStatus.RequiresVerification`y el usuario se redirigirá al método de acción `SendCode`, donde tendrá que escribir el código para completar el registro en secuencia. Si el usuario tiene RememberMe establecido en la cookie local de los usuarios, el `SignInManager` devolverá `SignInStatus.Success` y no tendrá que ir a través de 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

En el código siguiente se muestra el método de acción `SendCode`. Se crea un [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) con todos los métodos de 2FA habilitados para el usuario. [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) se pasa a la aplicación auxiliar [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , que permite al usuario seleccionar el enfoque 2FA (normalmente, correo electrónico y SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Una vez que el usuario envía el enfoque 2FA, se llama al método de acción `HTTP POST SendCode`, el `SignInManager` envía el código 2FA y se redirige al usuario al método de acción `VerifyCode` en el que puede escribir el código para completar el inicio de sesión.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Bloqueo de 2FA

Aunque puede establecer el bloqueo de cuentas en errores de intento de contraseña de inicio de sesión, este enfoque hace que el inicio de sesión sea susceptible a bloqueos de [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . Se recomienda usar el bloqueo de cuentas solo con 2FA. Cuando se crea el `ApplicationUserManager`, el código de ejemplo establece el bloqueo de 2FA y `MaxFailedAccessAttemptsBeforeLockout` en cinco. Una vez que un usuario inicia sesión (a través de una cuenta local o de una cuenta de redes sociales), se almacenan todos los intentos fallidos de 2FA y, si se alcanza el número máximo de intentos, el usuario se bloquea durante cinco minutos (puede establecer el tiempo de bloqueo con `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.net Identity recursos recomendados](../getting-started/aspnet-identity-recommended-resources.md) Lista completa de blogs, vídeos, tutoriales y excelentes vínculos de identidad.
- La [aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) también muestra cómo agregar información de perfil a la tabla de usuarios.
- [ASP.NET MVC e Identity 2,0: Descripción de los conceptos básicos](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) de John ATTEN.
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introducción a ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Presentación de RTM de ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) por Pranav Rastogi.
- [ASP.NET Identity 2,0: configurar la validación de la cuenta y la autorización en dos fases](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John ATTEN.
