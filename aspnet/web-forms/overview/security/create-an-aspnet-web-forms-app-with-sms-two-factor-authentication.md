---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Creación de una aplicación de formularios Web Forms de ASP.NET con autenticaciónC#en dos fases de SMS () | Microsoft Docs
author: Erikre
description: En este tutorial se muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con autenticación en dos fases. Este tutorial se ha diseñado para complementar el tutorial titulado CR...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78455983"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Crear una aplicación de formularios Web Forms de ASP.NET con la autenticación en dos fases de SMS (C#)

por [Erik Reitan](https://github.com/Erikre)

[Descarga de la aplicación de formularios Web Forms de ASP.NET con correo electrónico y autenticación en dos fases de SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> En este tutorial se muestra cómo compilar una aplicación de formularios Web Forms ASP.NET con autenticación en dos fases. Este tutorial se ha diseñado para complementar el tutorial titulado [creación de una aplicación de formularios Web Forms de ASP.net segura con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Además, este tutorial se basó en el [tutorial de MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)de Rick Anderson.

## <a name="introduction"></a>Introducción

Este tutorial le guía por los pasos necesarios para crear una aplicación de formularios Web Forms ASP.NET que admita la autenticación en dos fases mediante Visual Studio. La autenticación en dos fases es un paso de autenticación de usuario adicional. Este paso adicional genera un número de identificación personal (PIN) único durante el inicio de sesión. Normalmente, el PIN se envía al usuario como un mensaje de correo electrónico o SMS. Después, el usuario de la aplicación escribe el PIN como medida de autenticación adicional al iniciar sesión.

### <a name="tutorial-tasks-and-information"></a>Tareas e información del tutorial:

- [Creación de una aplicación de formularios Web Forms ASP.NET](#createWebForms)
- [Configurar SMS y autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases para el usuario registrado](#use2FA)
- [Recursos adicionales](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Creación de una aplicación de formularios Web Forms ASP.NET

Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale también [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o una versión posterior. Además, deberá crear una cuenta de [Twilio](https://www.twilio.com/try-twilio) , como se explica a continuación.

> [!NOTE]
> Importante: debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.

1. Cree un nuevo proyecto (**archivo** -&gt; **nuevo proyecto**) y seleccione la plantilla **aplicación Web ASP.net** junto con la .NET Framework versión 4.5.2 en el cuadro de diálogo **nuevo proyecto** .
2. En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione la plantilla de **formularios Web Forms** . Deje la autenticación predeterminada como **cuentas de usuario individuales**. A continuación, haga clic en **Aceptar** para crear el nuevo proyecto.  
    ![Cuadro de diálogo New ASP.NET Project](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilite Capa de sockets seguros (SSL) para el proyecto. Siga los pasos disponibles en la sección **habilitación de SSL para el proyecto** de la [serie de tutoriales de introducción con formularios Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. En Visual Studio, abra la **consola del administrador de paquetes** (**herramientas** -&gt; **administrador de paquetes NuGet** -&gt; **consola del administrador de paquetes**) y escriba el siguiente comando:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configurar SMS y autenticación en dos fases

En este tutorial se usa Twilio, pero puede usar cualquier proveedor de SMS.

1. Cree una cuenta de [Twilio](https://www.twilio.com/try-twilio) .
2. En la pestaña **Panel** de la cuenta de Twilio, copie el **SID** de la cuenta y el **token de autenticación.** Lo agregará a la aplicación más adelante.
3. En la pestaña **números** , copie también el **número de teléfono** de Twilio.
4. Haga que el **SID**de la cuenta Twilio, el **token de autenticación** y el **número de teléfono** estén disponibles para la aplicación. Para simplificar las cosas, almacenará estos valores en el archivo *Web. config* . Al realizar la implementación en Azure, puede almacenar los valores de forma segura en la sección **appSettings** de la pestaña configurar del sitio Web. Además, al agregar el número de teléfono, use solo números.   
   Tenga en cuenta que también puede agregar credenciales de SendGrid. SendGrid es un servicio de notificación por correo electrónico. Para obtener más información sobre cómo habilitar SendGrid, consulte la sección "enlace de SendGrid" del tutorial titulado [creación de una aplicación de formularios Web Forms de ASP.net segura con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. En este ejemplo, la cuenta y las credenciales se almacenan en la sección **appSettings** del archivo *Web. config* . En Azure, puede almacenar estos valores de forma segura en la pestaña **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** del Azure portal. Para obtener información relacionada, vea el tema de Rick Anderson titulado [prácticas recomendadas para la implementación de contraseñas y otros datos confidenciales en ASP.net y Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
5. Configure la clase `SmsService` en el archivo *App\_Start\IdentityConfig.CS* realizando los siguientes cambios resaltados en amarillo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Agregue las siguientes instrucciones de `using` al principio del archivo *IdentityConfig.CS* : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Actualice el archivo *account/Manage. aspx* quitando las líneas resaltadas en amarillo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. En el controlador de `Page_Load` del código subyacente de *Manage.aspx.CS* , quite la marca de comentario de la línea de código resaltada en amarillo para que aparezca de la manera siguiente: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. En el código subyacente de la *cuenta*/*TwoFactorAuthenticationSignIn.aspx.CS*, actualice el controlador de `Page_Load` agregando el siguiente código resaltado en amarillo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Al realizar el cambio de código anterior, el DropDownList "Providers" que contiene las opciones de autenticación no se restablecerá al primer valor. Esto permitirá al usuario seleccionar correctamente todas las opciones que se usarán para la autenticación, no solo la primera.
10. En **Explorador de soluciones**, haga clic con el botón secundario en *default. aspx* y seleccione **establecer como página de inicio**.
11. Al probar la aplicación, primero compile la aplicación (**Ctrl**+**MAYÚS**+**B**) y, a continuación, ejecute la aplicación (**F5**) y seleccione **registrar** para crear una nueva cuenta de usuario o seleccione **iniciar sesión** si la cuenta de usuario ya se ha registrado.
12. Una vez que el usuario haya iniciado sesión, haga clic en el identificador de usuario (dirección de correo electrónico) en la barra de navegación para mostrar la página **Administrar cuenta** (Manage. aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Haga clic en **Agregar** junto a **número de teléfono** en la página **Administrar cuenta** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Agregue el número de teléfono en el que usted (como usuario) desea recibir mensajes SMS (mensajes de texto) y haga clic en el botón **Enviar** .   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    En este momento, la aplicación usará las credenciales de *Web. config* para ponerse en contacto con Twilio. Se enviará un mensaje SMS (mensaje de texto) al teléfono asociado a la cuenta de usuario. Para comprobar que se ha enviado el mensaje Twilio, consulte el panel de Twilio.
15. En unos segundos, el teléfono asociado a la cuenta de usuario obtendrá un mensaje de texto que contiene el código de verificación. Escriba el código de verificación y presione **submit (enviar**).  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar la autenticación en dos fases para un usuario registrado

En este punto, ha habilitado la autenticación en dos fases para la aplicación. Para que un usuario Use la autenticación en dos fases, simplemente puede cambiar su configuración mediante la interfaz de usuario. 

1. Como usuario de la aplicación, puede habilitar la autenticación en dos fases para su cuenta específica; para ello, haga clic en el identificador de usuario (alias de correo electrónico) en la barra de navegación para mostrar la página **Administrar cuenta** . A continuación, haga clic en el vínculo **Habilitar** para habilitar la autenticación en dos fases para la cuenta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Cierre la sesión y vuelva a iniciarla. Si ha habilitado el correo electrónico, puede seleccionar SMS o correo electrónico para la autenticación en dos fases. Si no ha habilitado el correo electrónico, vea el tutorial titulado [creación de una aplicación de formularios Web Forms de ASP.net segura con registro de usuarios, confirmación de correo electrónico y restablecimiento de contraseña](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. Se muestra la página de autenticación en dos fases, donde puede especificar el código (desde SMS o correo electrónico).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Al hacer clic en la casilla **recordar este explorador** , no es necesario usar la autenticación en dos fases para iniciar sesión al usar el explorador y el dispositivo en el que activó la casilla. Siempre y cuando los usuarios malintencionados no puedan tener acceso a su dispositivo, habilitar la autenticación en dos fases y hacer clic en el **recordatorio este explorador** le proporcionará un acceso de contraseña de un solo paso, a la vez que conserva la sólida protección de autenticación en dos fases para todo el acceso desde dispositivos que no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Vínculos a ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implementación de una aplicación de formularios Web Forms de ASP.NET segura con pertenencia, OAuth y SQL Database a un sitio web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Serie de tutoriales de formularios Web Forms de ASP.NET: agregar un proveedor OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Serie de tutoriales de ASP.NET Web Forms: habilitación de SSL para el proyecto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Crear la aplicación en Facebook y conectar la aplicación al proyecto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
