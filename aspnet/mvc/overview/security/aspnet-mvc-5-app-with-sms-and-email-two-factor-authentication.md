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
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="eb449-104">Aplicación de ASP.NET MVC 5 con autenticación en dos fases por SMS y correo electrónico</span><span class="sxs-lookup"><span data-stu-id="eb449-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="eb449-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eb449-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="eb449-106">En este tutorial se muestra cómo crear una aplicación web MVC 5 ASP.NET con autenticación en dos fases.</span><span class="sxs-lookup"><span data-stu-id="eb449-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="eb449-107">Debe completar [la creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña antes de](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) continuar.</span><span class="sxs-lookup"><span data-stu-id="eb449-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="eb449-108">Puede descargar la aplicación completada [aquí](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="eb449-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="eb449-109">La descarga contiene aplicaciones auxiliares de depuración que permiten probar el correo electrónico de confirmación y SMS sin configurar un correo electrónico o un proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="eb449-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="eb449-110">Este tutorial fue escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="eb449-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="eb449-111">Creación de una aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb449-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="eb449-112">Configurar SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="eb449-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="eb449-113">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="eb449-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="eb449-114">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="eb449-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="eb449-115">Creación de una aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="eb449-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="eb449-116">Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="eb449-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="eb449-117">Instale [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o posterior.</span><span class="sxs-lookup"><span data-stu-id="eb449-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="eb449-118">ADVERTENCIA: debe completar [la creación de una aplicación Web de ASP.NET MVC 5 segura con inicio de sesión, confirmación de correo electrónico y restablecimiento de contraseña antes de](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) continuar.</span><span class="sxs-lookup"><span data-stu-id="eb449-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="eb449-119">Debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="eb449-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="eb449-120">Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC.</span><span class="sxs-lookup"><span data-stu-id="eb449-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="eb449-121">Los formularios Web Forms también admiten ASP.NET Identity, por lo que puede seguir pasos similares en una aplicación de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="eb449-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="eb449-122">Deje la autenticación predeterminada como **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="eb449-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="eb449-123">Si desea hospedar la aplicación en Azure, deje activada la casilla.</span><span class="sxs-lookup"><span data-stu-id="eb449-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="eb449-124">Más adelante en el tutorial se implementará en Azure.</span><span class="sxs-lookup"><span data-stu-id="eb449-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="eb449-125">Puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="eb449-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="eb449-126">Establezca el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="eb449-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="eb449-127">Configurar SMS para la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="eb449-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="eb449-128">En este tutorial se proporcionan instrucciones sobre el uso de Twilio o ASPSMS, pero puede usar cualquier otro proveedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="eb449-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="eb449-129">**Creación de una cuenta de usuario con un proveedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="eb449-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="eb449-130">Cree una cuenta de [Twilio](https://www.twilio.com/try-twilio) o de [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="eb449-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="eb449-131">**Instalación de paquetes adicionales o adición de referencias de servicio**</span><span class="sxs-lookup"><span data-stu-id="eb449-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="eb449-132">Twilio</span><span class="sxs-lookup"><span data-stu-id="eb449-132">Twilio:</span></span>  
   <span data-ttu-id="eb449-133">En la Consola del Administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="eb449-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="eb449-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="eb449-134">ASPSMS:</span></span>  
   <span data-ttu-id="eb449-135">Debe agregarse la siguiente referencia de servicio:</span><span class="sxs-lookup"><span data-stu-id="eb449-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="eb449-136">Dirección:</span><span class="sxs-lookup"><span data-stu-id="eb449-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="eb449-137">Espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="eb449-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="eb449-138">**Averiguación de las credenciales de usuario del proveedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="eb449-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="eb449-139">Twilio</span><span class="sxs-lookup"><span data-stu-id="eb449-139">Twilio:</span></span>  
   <span data-ttu-id="eb449-140">En la pestaña **Panel** de la cuenta de Twilio, copie el SID de la **cuenta** y el **token de autenticación**.</span><span class="sxs-lookup"><span data-stu-id="eb449-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="eb449-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="eb449-141">ASPSMS:</span></span>  
   <span data-ttu-id="eb449-142">En la configuración de la cuenta, vaya a **Userkey** y cópiela junto con la **contraseña**autodefinida.</span><span class="sxs-lookup"><span data-stu-id="eb449-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="eb449-143">Posteriormente, almacenaremos estos valores en el archivo *Web. config* dentro de las claves `"SMSAccountIdentification"` y `"SMSAccountPassword"`.</span><span class="sxs-lookup"><span data-stu-id="eb449-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="eb449-144">**Especificar SenderID/originador**</span><span class="sxs-lookup"><span data-stu-id="eb449-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="eb449-145">Twilio</span><span class="sxs-lookup"><span data-stu-id="eb449-145">Twilio:</span></span>  
   <span data-ttu-id="eb449-146">En la pestaña **números** , copie el número de teléfono de Twilio.</span><span class="sxs-lookup"><span data-stu-id="eb449-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="eb449-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="eb449-147">ASPSMS:</span></span>  
   <span data-ttu-id="eb449-148">En el menú **desbloquear orígenes** , desbloquee uno o más originadores o elija un originador alfanumérico (no admitido por todas las redes).</span><span class="sxs-lookup"><span data-stu-id="eb449-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="eb449-149">Más adelante se almacenará este valor en el archivo *Web. config* dentro del `"SMSAccountFrom"` de claves.</span><span class="sxs-lookup"><span data-stu-id="eb449-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="eb449-150">**Transferencia de credenciales de proveedor de SMS a la aplicación**</span><span class="sxs-lookup"><span data-stu-id="eb449-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="eb449-151">Haga que las credenciales y el número de teléfono del remitente estén disponibles para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb449-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="eb449-152">Para simplificar las cosas, almacenaremos estos valores en el archivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="eb449-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="eb449-153">Cuando se implementa en Azure, se pueden almacenar los valores de forma segura en la sección configuración de la **aplicación** de la pestaña configurar del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="eb449-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="eb449-154">Seguridad: nunca almacene datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="eb449-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="eb449-155">La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple.</span><span class="sxs-lookup"><span data-stu-id="eb449-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="eb449-156">Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="eb449-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="eb449-157">**Implementación de la transferencia de datos al proveedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="eb449-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="eb449-158">Configure la clase `SmsService` en el archivo *App\_Start\IdentityConfig.CS* .</span><span class="sxs-lookup"><span data-stu-id="eb449-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="eb449-159">En función del proveedor de SMS utilizado, active la sección **Twilio** o **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="eb449-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="eb449-160">Actualice la vista de Razor *Views\Manage\Index.cshtml* : (Nota: no quite solo los comentarios del código de salida, use el código que aparece a continuación).</span><span class="sxs-lookup"><span data-stu-id="eb449-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="eb449-161">Compruebe que los métodos de acción `EnableTwoFactorAuthentication` y `DisableTwoFactorAuthentication` en el `ManageController` tienen el atributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :</span><span class="sxs-lookup"><span data-stu-id="eb449-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="eb449-162">Ejecute la aplicación e inicie sesión con la cuenta que registró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="eb449-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="eb449-163">Haga clic en el identificador de usuario, que activa el método de acción `Index` en el controlador de `Manage`.</span><span class="sxs-lookup"><span data-stu-id="eb449-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="eb449-164">Haga clic en Agregar.</span><span class="sxs-lookup"><span data-stu-id="eb449-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="eb449-165">El método de acción `AddPhoneNumber` muestra un cuadro de diálogo para escribir un número de teléfono que pueda recibir mensajes SMS.</span><span class="sxs-lookup"><span data-stu-id="eb449-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="eb449-166">En unos segundos, recibirá un mensaje de texto con el código de verificación.</span><span class="sxs-lookup"><span data-stu-id="eb449-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="eb449-167">Escríbalo y presione **submit (enviar**).</span><span class="sxs-lookup"><span data-stu-id="eb449-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="eb449-168">La vista administrar muestra el número de teléfono que se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="eb449-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="eb449-169">Habilitar la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="eb449-169">Enable two-factor authentication</span></span>

<span data-ttu-id="eb449-170">En la aplicación generada por plantillas, debe usar la interfaz de usuario para habilitar la autenticación en dos fases (2FA).</span><span class="sxs-lookup"><span data-stu-id="eb449-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="eb449-171">Para habilitar 2FA, haga clic en su ID. de usuario (alias de correo electrónico) en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="eb449-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="eb449-172">Haga clic en habilitar 2FA.</span><span class="sxs-lookup"><span data-stu-id="eb449-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="eb449-173">Cierre la sesión y vuelva a iniciarla.</span><span class="sxs-lookup"><span data-stu-id="eb449-173">Logout, then log back in.</span></span> <span data-ttu-id="eb449-174">Si ha habilitado el correo electrónico (vea mi [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), puede seleccionar el SMS o el correo electrónico de 2FA.</span><span class="sxs-lookup"><span data-stu-id="eb449-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="eb449-175">Se muestra la página comprobar código donde puede escribir el código (desde SMS o correo electrónico).</span><span class="sxs-lookup"><span data-stu-id="eb449-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="eb449-176">Al hacer clic en la casilla **recordar este explorador** , no es necesario usar 2FA para iniciar sesión al usar el explorador y el dispositivo en el que activó la casilla.</span><span class="sxs-lookup"><span data-stu-id="eb449-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="eb449-177">Siempre que los usuarios malintencionados no puedan tener acceso a su dispositivo, habilitando 2FA y haciendo clic en el **recordatorio este explorador** le proporcionará un acceso de contraseña de un solo paso, a la vez que conservará la protección de 2FA segura para todo el acceso desde dispositivos que no son de confianza.</span><span class="sxs-lookup"><span data-stu-id="eb449-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="eb449-178">Puede hacerlo en cualquier dispositivo privada que se usan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="eb449-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="eb449-179">En este tutorial se proporciona una introducción rápida a la habilitación de 2FA en una nueva aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="eb449-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="eb449-180">Mi tutorial [autenticación en dos fases mediante SMS y correo electrónico con ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) profundiza en el código subyacente al ejemplo.</span><span class="sxs-lookup"><span data-stu-id="eb449-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="eb449-181">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="eb449-181">Additional Resources</span></span>

- <span data-ttu-id="eb449-182">[Autenticación en dos fases mediante SMS y correo electrónico con ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Entra en detalle en la autenticación en dos fases</span><span class="sxs-lookup"><span data-stu-id="eb449-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="eb449-183">Vínculos a ASP.NET Identity recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="eb449-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="eb449-184">[Confirmación de la cuenta y recuperación de la contraseña con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Profundiza en la recuperación de contraseñas y la confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="eb449-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="eb449-185">[Aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) En este tutorial se muestra cómo escribir una aplicación ASP.NET MVC 5 con la autorización de Facebook y Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="eb449-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="eb449-186">También se muestra cómo agregar datos adicionales a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="eb449-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="eb449-187">[Implemente una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Web de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="eb449-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="eb449-188">En este tutorial se agrega la implementación de Azure, cómo proteger la aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y otras características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="eb449-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="eb449-189">Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="eb449-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="eb449-190">Crear la aplicación en Facebook y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="eb449-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="eb449-191">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="eb449-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="eb449-192">Cómo configurar el C# entorno de desarrollo de MVC de y ASP.net</span><span class="sxs-lookup"><span data-stu-id="eb449-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
