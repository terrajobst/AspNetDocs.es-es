---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicación ASP.NET MVC 5 con autenticación en dos fases de SMS y correo electrónico | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo crear una aplicación web MVC 5 ASP.NET con autenticación en dos fases. Debe completar la creación de una aplicación Web de MVC 5 segura ASP.NET con...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432823"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplicación de ASP.NET MVC 5 con autenticación en dos fases por SMS y correo electrónico

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> En este tutorial se muestra cómo crear una aplicación web MVC 5 ASP.NET con autenticación en dos fases. Debe completar [la creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña antes de](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) continuar. Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). La descarga contiene aplicaciones auxiliares de depuración que permiten probar el correo electrónico de confirmación y SMS sin configurar un correo electrónico o un proveedor de SMS.
> 
> Este tutorial fue escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

- [Creación de una aplicación de ASP.NET MVC](#createMvc)
- [Configurar SMS para la autenticación en dos fases](#SMS)
- [Habilitar la autenticación en dos fases](#enable2)
- [Recursos adicionales](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Creación de una aplicación de ASP.NET MVC

Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o posterior.

> [!NOTE]
> ADVERTENCIA: debe completar [la creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña antes de](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) continuar. Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.

1. Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC. Los formularios Web Forms también admiten ASP.NET Identity, por lo que puede seguir pasos similares en una aplicación de formularios Web Forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Deje la autenticación predeterminada como **cuentas de usuario individuales**. Si desea hospedar la aplicación en Azure, deje activada la casilla. Más adelante en el tutorial se implementará en Azure. Puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Establezca el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Dirección:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Espacio de nombres:  
    `ASPSMSX2`
3. **Averiguación de las credenciales de usuario del proveedor de SMS**  
  
   Twilio  
   En la pestaña **Panel** de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.  
  
   ASPSMS:  
   En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**autodefinida.  
  
   Posteriormente, almacenaremos estos valores en el archivo *Web. config* dentro de las claves `"SMSAccountIdentification"` y `"SMSAccountPassword"`.
4. **Especificar SenderID/originador**  
  
   Twilio  
   En la pestaña **números** , copie el número de teléfono de Twilio.  
  
   ASPSMS:  
   En el menú **desbloquear orígenes** , desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).  
  
   Más adelante se almacenará este valor en el archivo *Web. config* dentro del `"SMSAccountFrom"` de claves.
5. **Transferencia de credenciales de proveedor de SMS a la aplicación**  
  
   Haga que las credenciales y el número de teléfono del remitente estén disponibles para la aplicación. Para simplificar las cosas, almacenaremos estos valores en el archivo *Web. config* . Cuando se implementa en Azure, se pueden almacenar los valores de forma segura en la sección configuración de la **aplicación** de la pestaña configurar del sitio Web. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Seguridad: nunca almacene datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple. Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementación de la transferencia de datos al proveedor de SMS**  
  
   Configure la clase `SmsService` en el archivo *App\_Start\IdentityConfig.CS* .  
  
   En función del proveedor de SMS utilizado, active la sección **Twilio** o **ASPSMS** : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Actualice la vista de Razor *Views\Manage\Index.cshtml* : (Nota: no quite solo los comentarios del código de salida, use el código que aparece a continuación).  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Compruebe que los métodos de acción `EnableTwoFactorAuthentication` y `DisableTwoFactorAuthentication` en el `ManageController` tienen el atributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.
10. Haga clic en el identificador de usuario, que activa el método de acción `Index` en el controlador de `Manage`.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Haga clic en Agregar.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. El método de acción `AddPhoneNumber` muestra un cuadro de diálogo para escribir un número de teléfono que pueda recibir mensajes SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. En unos segundos, recibirá un mensaje de texto con el código de verificación. Escríbalo y presione **submit (enviar**).  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. La vista administrar muestra el número de teléfono que se ha agregado.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Habilitar la autenticación en dos fases

En la aplicación generada por plantillas, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA). Para habilitar 2FA, haga clic en su ID. de usuario (alias de correo electrónico) en la barra de navegación.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Haga clic en habilitar 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Cierre la sesión y vuelva a iniciarla. Si ha habilitado el correo electrónico (vea mi [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o el correo electrónico de 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

Se muestra la página comprobar código donde puede escribir el código (desde SMS o correo electrónico).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Al hacer clic en la casilla **recordar este explorador** , no es necesario usar 2FA para iniciar sesión al usar el explorador y el dispositivo en el que activó la casilla. Siempre que los usuarios malintencionados no puedan tener acceso a su dispositivo, habilitando 2FA y haciendo clic en el **recordatorio este explorador** le proporcionará un acceso de contraseña de un solo paso, a la vez que conservará la protección de 2FA segura para todo el acceso desde dispositivos que no son de confianza. Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.

En este tutorial se proporciona una introducción rápida a la habilitación de 2FA en una nueva aplicación ASP.NET MVC. Mi tutorial [autenticación en dos fases mediante SMS y correo electrónico con ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) profundiza en el código subyacente al ejemplo.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Autenticación en dos fases mediante SMS y correo electrónico con ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Entra en detalle en la autenticación en dos fases
- [Vínculos a ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmación de la cuenta y recuperación de la contraseña con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Profundiza en la recuperación de contraseñas y la confirmación de la cuenta.
- [Aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) En este tutorial se muestra cómo escribir una aplicación ASP.NET MVC 5 con la autorización de Facebook y Google OAuth 2. También se muestra cómo agregar datos adicionales a la base de datos de identidad.
- [Implemente una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Web de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). En este tutorial se agrega la implementación de Azure, cómo proteger la aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y otras características de seguridad.
- [Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Crear la aplicación en Facebook y conectar la aplicación al proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuración de SSL en el proyecto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Cómo configurar el C# entorno de desarrollo de MVC de y ASP.net](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
