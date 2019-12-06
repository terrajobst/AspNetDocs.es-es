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
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="4918c-104">Crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, confirmación por correo electrónico y restablecimiento de contraseña (C#)</span><span class="sxs-lookup"><span data-stu-id="4918c-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>

<span data-ttu-id="4918c-105">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4918c-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="4918c-106">En este tutorial se muestra cómo compilar una aplicación Web ASP.NET MVC 5 con confirmación de correo electrónico y restablecimiento de contraseña mediante el sistema de pertenencia a ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="4918c-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span>

<span data-ttu-id="4918c-107">Para obtener una versión actualizada de este tutorial que usa .NET Core, consulte [confirmación de cuenta y recuperación de contraseña en ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).</span><span class="sxs-lookup"><span data-stu-id="4918c-107">For an updated version of this tutorial that uses .NET Core, see [Account confirmation and password recovery in ASP.NET Core[/aspnet/core/security/authentication/accconfirm).</span></span>

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="4918c-108">Creación de una aplicación de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4918c-108">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="4918c-109">Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="4918c-109">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="4918c-110">Instale [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o posterior.</span><span class="sxs-lookup"><span data-stu-id="4918c-110">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="4918c-111">ADVERTENCIA: debe instalar [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) o superior para completar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="4918c-111">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="4918c-112">Cree un nuevo proyecto Web ASP.NET y seleccione la plantilla MVC.</span><span class="sxs-lookup"><span data-stu-id="4918c-112">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="4918c-113">Los formularios Web Forms también admiten ASP.NET Identity, por lo que puede seguir pasos similares en una aplicación de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="4918c-113">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="4918c-114">Deje la autenticación predeterminada como **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="4918c-114">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="4918c-115">Si desea hospedar la aplicación en Azure, deje activada la casilla.</span><span class="sxs-lookup"><span data-stu-id="4918c-115">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="4918c-116">Más adelante en el tutorial se implementará en Azure.</span><span class="sxs-lookup"><span data-stu-id="4918c-116">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="4918c-117">Puede [abrir una cuenta de Azure de forma gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="4918c-117">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="4918c-118">Establezca el [proyecto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="4918c-118">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="4918c-119">Ejecute la aplicación, haga clic en el vínculo **registrar** y registre un usuario.</span><span class="sxs-lookup"><span data-stu-id="4918c-119">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="4918c-120">En este momento, la única validación en el correo electrónico es con el atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="4918c-120">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="4918c-121">En Explorador de servidores, vaya a **datos Connections\DefaultConnection\Tables\AspNetUsers**, haga clic con el botón derecho y seleccione **abrir definición de tabla**.</span><span class="sxs-lookup"><span data-stu-id="4918c-121">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="4918c-122">En la imagen siguiente se muestra el esquema de `AspNetUsers`:</span><span class="sxs-lookup"><span data-stu-id="4918c-122">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="4918c-123">Haga clic con el botón derecho en la tabla **AspNetUsers** y seleccione **Mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="4918c-123">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="4918c-124">En este momento no se ha confirmado el correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-124">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="4918c-125">Haga clic en la fila y seleccione eliminar.</span><span class="sxs-lookup"><span data-stu-id="4918c-125">Click on the row and select delete.</span></span> <span data-ttu-id="4918c-126">Agregará este correo electrónico de nuevo en el paso siguiente y enviará un correo electrónico de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4918c-126">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="4918c-127">Confirmación por correo electrónico</span><span class="sxs-lookup"><span data-stu-id="4918c-127">Email confirmation</span></span>

<span data-ttu-id="4918c-128">Se recomienda confirmar el correo electrónico de un nuevo registro de usuario para comprobar que no está suplantando a otra persona (es decir, que no se han registrado con el correo electrónico de otra persona).</span><span class="sxs-lookup"><span data-stu-id="4918c-128">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="4918c-129">Supongamos que tiene un foro de discusión, desea evitar que `"bob@example.com"` se registre como `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="4918c-129">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="4918c-130">Sin confirmación por correo electrónico, `"joe@contoso.com"` podría obtener un correo electrónico no deseado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4918c-130">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="4918c-131">Supongamos que Bob se registra accidentalmente como `"bib@example.com"` y no lo ha notado, no podría usar la recuperación de contraseña porque la aplicación no tiene su correo electrónico correcto.</span><span class="sxs-lookup"><span data-stu-id="4918c-131">Suppose Bob accidentally registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="4918c-132">La confirmación por correo electrónico proporciona solo protección limitada de bots y no proporciona protección contra los remitentes de spam determinados, tienen muchos alias de correo electrónico que pueden usar para registrarse.</span><span class="sxs-lookup"><span data-stu-id="4918c-132">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="4918c-133">Por lo general, querrá evitar que los nuevos usuarios publiquen datos en el sitio Web antes de que se hayan confirmado por correo electrónico, un mensaje de texto SMS u otro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="4918c-133">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="4918c-134">En las secciones siguientes, habilitaremos la confirmación por correo electrónico y modificaremos el código para evitar que los usuarios recién registrados inicien sesión hasta que se haya confirmado su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-134">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="4918c-135">Enlazar SendGrid</span><span class="sxs-lookup"><span data-stu-id="4918c-135">Hook up SendGrid</span></span>

<span data-ttu-id="4918c-136">Las instrucciones de esta sección no son actuales.</span><span class="sxs-lookup"><span data-stu-id="4918c-136">The instructions in this section are not current.</span></span> <span data-ttu-id="4918c-137">Consulte [configurar el proveedor de correo electrónico de SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) para obtener instrucciones actualizadas.</span><span class="sxs-lookup"><span data-stu-id="4918c-137">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="4918c-138">Aunque este tutorial solo muestra cómo agregar una notificación por correo electrónico a través de [SendGrid](http://sendgrid.com/), puede enviar correo electrónico mediante SMTP y otros mecanismos (consulte [recursos adicionales](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="4918c-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="4918c-139">En la Consola del Administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4918c-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="4918c-140">Vaya a la página de registro de [Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) y regístrese para obtener una cuenta gratuita de SendGrid.</span><span class="sxs-lookup"><span data-stu-id="4918c-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="4918c-141">Configure SendGrid agregando código similar al siguiente en *App_Start/identityconfig.CS*:</span><span class="sxs-lookup"><span data-stu-id="4918c-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="4918c-142">Deberá agregar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4918c-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="4918c-143">Para simplificar este ejemplo, almacenaremos la configuración de la aplicación en el archivo *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="4918c-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="4918c-144">Seguridad: nunca almacene datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="4918c-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="4918c-145">La cuenta y las credenciales se almacenan en appSetting.</span><span class="sxs-lookup"><span data-stu-id="4918c-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="4918c-146">En Azure, puede almacenar estos valores de forma segura en la pestaña **[configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** del Azure portal.</span><span class="sxs-lookup"><span data-stu-id="4918c-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="4918c-147">Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="4918c-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>

### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="4918c-148">Habilitar la confirmación por correo electrónico en el controlador de cuenta</span><span class="sxs-lookup"><span data-stu-id="4918c-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="4918c-149">Compruebe que el archivo *Views\Account\ConfirmEmail.cshtml* tiene la sintaxis de Razor correcta.</span><span class="sxs-lookup"><span data-stu-id="4918c-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="4918c-150">(Es posible que falte el carácter @ en la primera línea.</span><span class="sxs-lookup"><span data-stu-id="4918c-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="4918c-151">)</span><span class="sxs-lookup"><span data-stu-id="4918c-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="4918c-152">Ejecute la aplicación y haga clic en el vínculo registrar.</span><span class="sxs-lookup"><span data-stu-id="4918c-152">Run the app and click the Register link.</span></span> <span data-ttu-id="4918c-153">Una vez enviado el formulario de registro, ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="4918c-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="4918c-154">Compruebe su cuenta de correo electrónico y haga clic en el vínculo para confirmar su correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="4918c-155">Requerir confirmación por correo electrónico antes de iniciar sesión</span><span class="sxs-lookup"><span data-stu-id="4918c-155">Require email confirmation before log in</span></span>

<span data-ttu-id="4918c-156">Actualmente, una vez que un usuario completa el formulario de registro, inicia sesión.</span><span class="sxs-lookup"><span data-stu-id="4918c-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="4918c-157">Por lo general, querrá confirmar el correo electrónico antes de registrarlo en.</span><span class="sxs-lookup"><span data-stu-id="4918c-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="4918c-158">En la sección siguiente, se modificará el código para requerir que los nuevos usuarios tengan un correo electrónico confirmado antes de que se inicie sesión (autenticado).</span><span class="sxs-lookup"><span data-stu-id="4918c-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="4918c-159">Actualice el método de `HttpPost Register` con los siguientes cambios resaltados:</span><span class="sxs-lookup"><span data-stu-id="4918c-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="4918c-160">Al comentar el método `SignInAsync`, el usuario no iniciará sesión en el registro.</span><span class="sxs-lookup"><span data-stu-id="4918c-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="4918c-161">La línea de `TempData["ViewBagLink"] = callbackUrl;` se puede usar para [depurar la aplicación y el](#dbg) registro de prueba sin enviar correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="4918c-162">`ViewBag.Message` se usa para mostrar las instrucciones de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4918c-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="4918c-163">El [ejemplo de descarga](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contiene código para probar la confirmación por correo electrónico sin configurar el correo electrónico y también se puede usar para depurar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4918c-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="4918c-164">Cree un archivo de `Views\Shared\Info.cshtml` y agregue el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="4918c-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="4918c-165">Agregue el [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) al método de acción `Contact` del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="4918c-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="4918c-166">Puede hacer clic en el vínculo **contacto** para comprobar que los usuarios anónimos no tienen acceso y que los usuarios autenticados tienen acceso.</span><span class="sxs-lookup"><span data-stu-id="4918c-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="4918c-167">También debe actualizar el método de acción `HttpPost Login`:</span><span class="sxs-lookup"><span data-stu-id="4918c-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="4918c-168">Actualice la vista *Views\Shared\Error.cshtml* para mostrar el mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="4918c-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="4918c-169">Elimine todas las cuentas de la tabla **AspNetUsers** que contengan el alias de correo electrónico con el que desea probar.</span><span class="sxs-lookup"><span data-stu-id="4918c-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="4918c-170">Ejecute la aplicación y compruebe que no puede iniciar sesión hasta que haya confirmado su dirección de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="4918c-171">Una vez confirmada la dirección de correo electrónico, haga clic en el vínculo **contacto** .</span><span class="sxs-lookup"><span data-stu-id="4918c-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="4918c-172">Recuperación/restablecimiento de contraseñas</span><span class="sxs-lookup"><span data-stu-id="4918c-172">Password recovery/reset</span></span>

<span data-ttu-id="4918c-173">Quite los caracteres de comentario del método de acción `HttpPost ForgotPassword` del controlador de cuenta:</span><span class="sxs-lookup"><span data-stu-id="4918c-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="4918c-174">Quite los caracteres de comentario del `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) en el archivo de vista de Razor *Views\Account\Login.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="4918c-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="4918c-175">La página de inicio de sesión mostrará ahora un vínculo para restablecer la contraseña.</span><span class="sxs-lookup"><span data-stu-id="4918c-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="4918c-176">Vínculo reenviar correo electrónico de confirmación</span><span class="sxs-lookup"><span data-stu-id="4918c-176">Resend email confirmation link</span></span>

<span data-ttu-id="4918c-177">Una vez que un usuario crea una nueva cuenta local, se le envía un vínculo de confirmación para que pueda iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="4918c-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="4918c-178">Si el usuario elimina accidentalmente el correo electrónico de confirmación o el correo electrónico nunca llega, deberá volver a enviar el vínculo de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4918c-178">If the user accidentally deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="4918c-179">Los siguientes cambios de código muestran cómo habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="4918c-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="4918c-180">Agregue el siguiente método auxiliar a la parte inferior del archivo *Controllers\AccountController.CS* :</span><span class="sxs-lookup"><span data-stu-id="4918c-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="4918c-181">Actualice el método de registro para usar la nueva aplicación auxiliar:</span><span class="sxs-lookup"><span data-stu-id="4918c-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="4918c-182">Actualice el método de inicio de sesión para volver a enviar la contraseña si no se ha confirmado la cuenta de usuario:</span><span class="sxs-lookup"><span data-stu-id="4918c-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="4918c-183">Combinación de cuentas de inicio de sesión sociales y locales</span><span class="sxs-lookup"><span data-stu-id="4918c-183">Combine social and local login accounts</span></span>

<span data-ttu-id="4918c-184">Para combinar cuentas locales y sociales, haga clic en el vínculo de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="4918c-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="4918c-185">En la siguiente secuencia **RickAndMSFT@gmail.com** se crea primero como un inicio de sesión local, pero puede crear la cuenta como un registro social en primer lugar y luego agregar un inicio de sesión local.</span><span class="sxs-lookup"><span data-stu-id="4918c-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="4918c-186">Haga clic en el vínculo **administrar** .</span><span class="sxs-lookup"><span data-stu-id="4918c-186">Click on the **Manage** link.</span></span> <span data-ttu-id="4918c-187">Anote los **inicios de sesión externos: 0** asociados a esta cuenta.</span><span class="sxs-lookup"><span data-stu-id="4918c-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="4918c-188">Haga clic en el vínculo a otro servicio de inicio de sesión y acepte las solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4918c-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="4918c-189">Las dos cuentas se han combinado, podrá iniciar sesión con cualquier cuenta.</span><span class="sxs-lookup"><span data-stu-id="4918c-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="4918c-190">Es posible que desee que los usuarios agreguen cuentas locales en caso de que su registro social en el servicio de autenticación esté inactivo, o más probabilidades de que hayan perdido el acceso a su cuenta social.</span><span class="sxs-lookup"><span data-stu-id="4918c-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="4918c-191">En la imagen siguiente, Tom es un inicio de sesión social (que puede ver en los **inicios de sesión externos: 1** que se muestra en la página).</span><span class="sxs-lookup"><span data-stu-id="4918c-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="4918c-192">Al hacer clic en **seleccionar una contraseña** , puede Agregar un inicio de sesión local asociado a la misma cuenta.</span><span class="sxs-lookup"><span data-stu-id="4918c-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="4918c-193">Confirmación de correo electrónico con más profundidad</span><span class="sxs-lookup"><span data-stu-id="4918c-193">Email confirmation in more depth</span></span>

<span data-ttu-id="4918c-194">La confirmación de la cuenta de tutorial [y la recuperación de contraseñas con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entran en este tema con más detalles.</span><span class="sxs-lookup"><span data-stu-id="4918c-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="4918c-195">Depurar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4918c-195">Debugging the app</span></span>

<span data-ttu-id="4918c-196">Si no recibe un correo electrónico que contenga el vínculo:</span><span class="sxs-lookup"><span data-stu-id="4918c-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="4918c-197">Compruebe la carpeta de correo no deseado o correo no deseado.</span><span class="sxs-lookup"><span data-stu-id="4918c-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="4918c-198">Inicie sesión en su cuenta de SendGrid y haga clic en el [vínculo actividad de correo electrónico](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="4918c-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="4918c-199">Para probar el vínculo de comprobación sin correo electrónico, descargue el [ejemplo completado](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="4918c-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="4918c-200">En la página se mostrarán los códigos de confirmación y el vínculo de confirmación.</span><span class="sxs-lookup"><span data-stu-id="4918c-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="4918c-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4918c-201">Additional Resources</span></span>

- [<span data-ttu-id="4918c-202">Vínculos a ASP.NET Identity recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="4918c-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="4918c-203">[Confirmación de la cuenta y recuperación de la contraseña con ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Profundiza en la recuperación de contraseñas y la confirmación de la cuenta.</span><span class="sxs-lookup"><span data-stu-id="4918c-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="4918c-204">[Aplicación MVC 5 con el inicio de sesión de Facebook, Twitter, LinkedIn y Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) En este tutorial se muestra cómo escribir una aplicación ASP.NET MVC 5 con la autorización de Facebook y Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="4918c-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="4918c-205">También se muestra cómo agregar datos adicionales a la base de datos de identidad.</span><span class="sxs-lookup"><span data-stu-id="4918c-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="4918c-206">[Implemente una aplicación ASP.NET MVC segura con pertenencia, OAuth y SQL Database en Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="4918c-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="4918c-207">En este tutorial se agrega la implementación de Azure, cómo proteger la aplicación con roles, cómo usar la API de pertenencia para agregar usuarios y roles y otras características de seguridad.</span><span class="sxs-lookup"><span data-stu-id="4918c-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="4918c-208">Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="4918c-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="4918c-209">Crear la aplicación en Facebook y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="4918c-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="4918c-210">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="4918c-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
