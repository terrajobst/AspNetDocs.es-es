---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creación de una aplicación MVC 5 con el inicio de sesión OAuth2 de Facebook, Twitter,C#LinkedIn y Google () | Microsoft Docs
author: Rick-Anderson
description: En este tutorial se muestra cómo crear una aplicación web MVC 5 de ASP.NET que permite a los usuarios iniciar sesión con OAuth 2,0 con credenciales de un autenti externo...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: dd2e55d68ceb5a90134e394c00f3a3a231cb27d6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457692"
---
# <a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="f5b7f-103">Crear una aplicación de ASP.NET MVC 5 con el inicio de sesión OAuth2 de Facebook, Twitter, LinkedIn y Google (C#)</span><span class="sxs-lookup"><span data-stu-id="f5b7f-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>

<span data-ttu-id="f5b7f-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f5b7f-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="f5b7f-105">En este tutorial se muestra cómo crear una aplicación web MVC 5 de ASP.NET que permite a los usuarios iniciar sesión con [OAuth 2,0](http://oauth.net/2/) con las credenciales de un proveedor de autenticación externo, como Facebook, Twitter, LinkedIn, Microsoft o Google.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="f5b7f-106">Para simplificar, este tutorial se centra en el uso de credenciales de Facebook y Google.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="f5b7f-107">Habilitar estas credenciales en los sitios web proporciona una ventaja significativa porque millones de usuarios ya tienen cuentas con estos proveedores externos.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="f5b7f-108">Estos usuarios pueden estar más inclinados para registrarse en su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="f5b7f-109">Consulte también [aplicación ASP.NET MVC 5 con SMS y autenticación en dos fases de correo electrónico](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="f5b7f-110">En el tutorial también se muestra cómo agregar datos de perfil para el usuario y cómo usar la API de pertenencia para agregar roles.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="f5b7f-111">Este tutorial fue escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (síganos en Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="f5b7f-112">Introducción</span><span class="sxs-lookup"><span data-stu-id="f5b7f-112">Getting Started</span></span>

<span data-ttu-id="f5b7f-113">Empiece por instalar y ejecutar [Visual Studio Express 2013 para web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="f5b7f-114">Instale Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o posterior.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="f5b7f-115">Para obtener ayuda con Dropbox, GitHub, LinkedIn, Instagram, buffer, Salesforce, vapor, Stack Exchange, TripIt, Twitch, Twitter, Yahoo!, etc., vea este [proyecto de ejemplo](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="f5b7f-116">Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior para usar Google OAuth 2 y para depurar localmente sin advertencias de SSL.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>

<span data-ttu-id="f5b7f-117">Haga clic en **nuevo proyecto** en la página de **Inicio** , o bien puede usar el menú, seleccionar **archivo**y, a continuación, **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="f5b7f-118">Crear la primera aplicación</span><span class="sxs-lookup"><span data-stu-id="f5b7f-118">Creating Your First Application</span></span>

<span data-ttu-id="f5b7f-119">Haga clic en **nuevo proyecto**, **Seleccione C# visual** a la izquierda y luego **Web** y, a continuación, seleccione **aplicación Web de ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="f5b7f-120">Asigne al proyecto el nombre "MvcAuth" y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="f5b7f-121">En el cuadro de diálogo **nuevo proyecto de ASP.net** , haga clic en **MVC**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="f5b7f-122">Si la autenticación no es una **cuenta de usuario individual**, haga clic en el botón **cambiar autenticación** y seleccione **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="f5b7f-123">Al comprobar **el host en la nube**, la aplicación será muy fácil de hospedar en Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="f5b7f-124">Si seleccionó **hospedar en la nube**, complete el cuadro de diálogo Configurar.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)

### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="f5b7f-125">Uso de NuGet para actualizar al middleware de OWIN más reciente</span><span class="sxs-lookup"><span data-stu-id="f5b7f-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="f5b7f-126">Use el administrador de paquetes NuGet para actualizar el [middleware OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="f5b7f-127">Seleccione **actualizaciones** en el menú de la izquierda.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="f5b7f-128">Puede hacer clic en el botón **Actualizar todo** o puede buscar solo paquetes OWIN (mostrados en la siguiente imagen):</span><span class="sxs-lookup"><span data-stu-id="f5b7f-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="f5b7f-129">En la imagen siguiente, solo se muestran los paquetes OWIN:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="f5b7f-130">En la consola del administrador de paquetes (PMC), puede escribir el comando `Update-Package`, que actualizará todos los paquetes.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="f5b7f-131">Presione **F5** o **Ctrl + F5** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="f5b7f-132">En la imagen siguiente, el número de puerto es 1234.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="f5b7f-133">Al ejecutar la aplicación, verá un número de puerto diferente.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="f5b7f-134">En función del tamaño de la ventana del explorador, es posible que tenga que hacer clic en el icono de navegación para ver los vínculos **Inicio**, **acerca**de, **contacto**, **registro** e inicio **de sesión** .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="f5b7f-135">Configuración de SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="f5b7f-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="f5b7f-136">Para conectarse a proveedores de autenticación como Google y Facebook, tendrá que configurar IIS-Express para usar SSL.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="f5b7f-137">Es importante seguir usando SSL después del inicio de sesión y no volver a HTTP, la cookie de inicio de sesión es tan secreta como el nombre de usuario y la contraseña, y sin usar SSL, se envía en texto sin cifrar a través de la conexión.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="f5b7f-138">Además, ya ha tomado el tiempo necesario para realizar el protocolo de enlace y proteger el canal (que es la mayor parte de lo que hace que HTTPS sea más lento que HTTP) antes de que se ejecute la canalización de MVC, por lo que si redirige de nuevo a HTTP después de haber iniciado sesión no se realizará la solicitud actual ni el futuro las solicitudes son mucho más rápidas.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="f5b7f-139">En **Explorador de soluciones**, haga clic en el proyecto **MvcAuth** .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="f5b7f-140">Presione la tecla F4 para mostrar las propiedades del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="f5b7f-141">Como alternativa, en el menú **Ver** puede seleccionar **ventana Propiedades**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="f5b7f-142">Cambie **SSL habilitado** a true.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="f5b7f-143">Copie la dirección URL de SSL (que se `https://localhost:44300/` a menos que haya creado otros proyectos SSL).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="f5b7f-144">En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **MvcAuth** y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="f5b7f-145">Seleccione la pestaña **Web** y, a continuación, pegue la dirección URL de SSL en el cuadro **dirección URL del proyecto** .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="f5b7f-146">Guarde el archivo (CTL + S).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="f5b7f-147">Necesitará esta dirección URL para configurar las aplicaciones de autenticación de Facebook y Google.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="f5b7f-148">Agregue el atributo [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) al controlador de `Home` para requerir que todas las solicitudes deban usar https.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="f5b7f-149">Un enfoque más seguro consiste en agregar el filtro [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="f5b7f-150">Vea la sección &quot;protección de la aplicación con SSL y el atributo Authorize&quot; en mi tutorial [creación de una aplicación de ASP.NET MVC con autenticación y base de SQL Database e implementación en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="f5b7f-151">A continuación se muestra una parte del controlador Home.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="f5b7f-152">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="f5b7f-153">Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y pasar a [crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto](#goog); de lo contrario, siga las instrucciones para confiar en el certificado autofirmado que ha generado IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="f5b7f-154">Lea el cuadro de diálogo **Advertencia de seguridad** y, a continuación, haga clic en **sí** si desea instalar el certificado que representa localhost.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="f5b7f-155">IE muestra la página *Inicio* , sin ninguna advertencia de SSL.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="f5b7f-156">Google Chrome también acepta el certificado y mostrará el contenido HTTPS sin ninguna advertencia.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="f5b7f-157">Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="f5b7f-158">Para nuestra aplicación, puede hacer clic en entiendo **los riesgos**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="f5b7f-159">Crear una aplicación de Google para OAuth 2 y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="f5b7f-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="f5b7f-160">Para obtener instrucciones actuales de Google OAuth, consulte [configuración de la autenticación de Google en ASP.net Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="f5b7f-161">Navegue a la [consola de desarrolladores de Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="f5b7f-162">Si no ha creado un proyecto antes, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="f5b7f-163">En la pestaña de la izquierda, haga clic en **credenciales**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="f5b7f-164">Haga clic en **crear credenciales** y luego en ID. de **cliente OAuth**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="f5b7f-165">En el cuadro de diálogo **crear ID** . de cliente, mantenga la **aplicación web** predeterminada para el tipo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="f5b7f-166">Establezca los orígenes de **JavaScript autorizados** en la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado otros proyectos SSL)</span><span class="sxs-lookup"><span data-stu-id="f5b7f-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="f5b7f-167">Establezca el **URI de redireccionamiento autorizado** en:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="f5b7f-168">Haga clic en el elemento de menú pantalla de consentimiento de OAuth y, a continuación, establezca la dirección de correo electrónico y el nombre del producto.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="f5b7f-169">Cuando haya completado el formulario, haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="f5b7f-170">Haga clic en el elemento de menú biblioteca, busque **Google + API**, haga clic en él y, a continuación, presione habilitar.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="f5b7f-171">En la imagen siguiente se muestran las API habilitadas.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="f5b7f-172">En el administrador de API de API de Google, visite la pestaña **credenciales** para obtener el **identificador de cliente**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="f5b7f-173">Descargar para guardar un archivo JSON con secretos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="f5b7f-174">Copie y pegue los **ClientID** y **ClientSecret** en el método `UseGoogleAuthentication` que se encuentra en el archivo *Startup.auth.CS* de la carpeta *App_Start* .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="f5b7f-175">Los valores de **ClientID** y **ClientSecret** que se muestran a continuación son ejemplos y no funcionan.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="f5b7f-176">Seguridad: nunca almacene datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="f5b7f-177">La cuenta y las credenciales se agregan al código anterior para mantener el ejemplo simple.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="f5b7f-178">Vea los [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.net y Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="f5b7f-179">Presione **CTRL+F5** para compilar y ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="f5b7f-180">Haga clic en el vínculo **Iniciar sesión** .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="f5b7f-181">En **usar otro servicio para iniciar sesión**, haga clic en **Google**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="f5b7f-182">Si se pierde alguno de los pasos anteriores, obtendrá un error HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="f5b7f-183">Volver a comprobar los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-183">Recheck your steps above.</span></span> <span data-ttu-id="f5b7f-184">Si se pierde una configuración necesaria (por ejemplo, el **nombre del producto**), agregue el elemento que falta y guárdelo. la autenticación puede tardar unos minutos en funcionar.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="f5b7f-185">Se le redirigirá al sitio de Google en el que escribirá sus credenciales.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="f5b7f-186">Una vez que haya proporcionado sus credenciales, el sistema le pedirá que le dé permisos a la aplicación web que acaba de crear:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="f5b7f-187">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-187">Click **Accept**.</span></span> <span data-ttu-id="f5b7f-188">Ahora se le redirigirá a la página de **registro** de la aplicación MvcAuth donde puede registrar su cuenta de Google.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="f5b7f-189">Tiene la opción de cambiar el nombre de registro de correo electrónico local que usa para su cuenta de Gmail, pero suele ser más práctico mantener el mismo alias de correo electrónico predeterminado (es decir, el que usó para la autenticación).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="f5b7f-190">Haga clic en **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="f5b7f-191">Crear la aplicación en Facebook y conectar la aplicación al proyecto</span><span class="sxs-lookup"><span data-stu-id="f5b7f-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="f5b7f-192">Para obtener instrucciones de autenticación de OAuth2 de Facebook actuales, consulte Configuración de la [autenticación de Facebook](/aspnet/core/security/authentication/social/facebook-logins) .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="f5b7f-193">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="f5b7f-193">Examine the Membership Data</span></span>

<span data-ttu-id="f5b7f-194">En el menú **Ver** , haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="f5b7f-195">Expanda **DefaultConnection (MvcAuth)** , expanda **tablas**, haga clic con el botón secundario en **AspNetUsers** y haga clic en **Mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="f5b7f-197">Agregar datos de perfil a la clase User</span><span class="sxs-lookup"><span data-stu-id="f5b7f-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="f5b7f-198">En esta sección, agregará la fecha de nacimiento y la ciudad de inicio a los datos de usuario durante el registro, como se muestra en la siguiente imagen.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG con ciudad de casa y Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="f5b7f-200">Abra el archivo *Models\IdentityModels.CS* y agregue las propiedades fecha de nacimiento y ciudad de residencia:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="f5b7f-201">Abra el archivo *Models\AccountViewModels.CS* y las propiedades establecer fecha de nacimiento y ciudad de casa en `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="f5b7f-202">Abra el archivo *Controllers\AccountController.CS* y agregue el código para la fecha de nacimiento y la ciudad de inicio en el método de acción `ExternalLoginConfirmation`, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="f5b7f-203">Agregue la fecha de nacimiento y la ciudad de inicio al archivo *Views\Account\ExternalLoginConfirmation.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="f5b7f-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="f5b7f-204">Elimine la base de datos de pertenencia para que vuelva a registrar su cuenta de Facebook en la aplicación y compruebe que puede Agregar la nueva fecha de nacimiento y la información del perfil de ciudad de casa.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="f5b7f-205">En **Explorador de soluciones**, haga clic en el icono **Mostrar todos los archivos** y, a continuación, haga clic con el botón secundario en *agregar\_Data\aspnet-MvcAuth-&lt;fecha&gt;. MDF* y haga clic en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="f5b7f-206">En el menú **herramientas** , haga clic en Administrador de **paquetes NuGet**y, a continuación, haga clic en **consola del administrador de paquetes** (PMC).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="f5b7f-207">Escriba los siguientes comandos en el PMC.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="f5b7f-208">Habilitar: migraciones</span><span class="sxs-lookup"><span data-stu-id="f5b7f-208">Enable-Migrations</span></span>
2. <span data-ttu-id="f5b7f-209">Agregar inicialización de migración</span><span class="sxs-lookup"><span data-stu-id="f5b7f-209">Add-Migration Init</span></span>
3. <span data-ttu-id="f5b7f-210">Actualizar base de datos</span><span class="sxs-lookup"><span data-stu-id="f5b7f-210">Update-Database</span></span>

<span data-ttu-id="f5b7f-211">Ejecute la aplicación y use FaceBook y Google para iniciar sesión y registrar a algunos usuarios.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="f5b7f-212">Examinar los datos de pertenencia</span><span class="sxs-lookup"><span data-stu-id="f5b7f-212">Examine the Membership Data</span></span>

<span data-ttu-id="f5b7f-213">En el menú **Ver** , haga clic en **Explorador de servidores**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="f5b7f-214">Haga clic con el botón secundario en **AspNetUsers** y haga clic en **Mostrar datos de tabla**.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="f5b7f-215">A continuación se muestran los campos `HomeTown` y `BirthDate`.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="f5b7f-216">Cierre la sesión de la aplicación e inicie sesión con otra cuenta</span><span class="sxs-lookup"><span data-stu-id="f5b7f-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="f5b7f-217">Si inicia sesión en su aplicación con Facebook, y, a continuación, cierre la sesión e intente iniciar sesión de nuevo con una cuenta de Facebook diferente (con el mismo explorador), iniciará sesión inmediatamente en la cuenta de Facebook anterior que usó.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="f5b7f-218">Para usar otra cuenta, debe navegar a Facebook y cerrar sesión en Facebook.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="f5b7f-219">La misma regla se aplica a cualquier otro proveedor de autenticación de terceros.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="f5b7f-220">Como alternativa, puede iniciar sesión con otra cuenta mediante un explorador diferente.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5b7f-221">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="f5b7f-221">Next Steps</span></span>

<span data-ttu-id="f5b7f-222">Consulte [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth para OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser para las instrucciones de Yahoo y LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="f5b7f-223">Consulte botones de inicio de sesión social Pretty de Jerrie para ASP.NET MVC 5 para obtener los botones Habilitar inicio de sesión social.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="f5b7f-224">Siga mi tutorial [creación de una aplicación de ASP.NET MVC con auth y SQL Database e impleméntela en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que continúa en este tutorial y muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f5b7f-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="f5b7f-225">Cómo implementar la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="f5b7f-226">Cómo proteger la aplicación con roles.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="f5b7f-227">Cómo proteger la aplicación con los filtros [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) .</span><span class="sxs-lookup"><span data-stu-id="f5b7f-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="f5b7f-228">Cómo usar la API de pertenencia para agregar usuarios y roles.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="f5b7f-229">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="f5b7f-230">También puede solicitar nuevos temas en [mostrarme cómo con el código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="f5b7f-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="f5b7f-231">Incluso puede solicitar y votar las nuevas características que se van a agregar a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="f5b7f-232">Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="f5b7f-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="f5b7f-233">Para obtener una buena explicación de cómo funcionan los servicios de autenticación externos de ASP.NET, consulte [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services)de Robert McMurray.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="f5b7f-234">El artículo de Robert también incluye detalles sobre cómo habilitar la autenticación de Microsoft y Twitter.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="f5b7f-235">En el excelente [tutorial de EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) de Tom Dykstra se muestra cómo trabajar con el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f5b7f-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
